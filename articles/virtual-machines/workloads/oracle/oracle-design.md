---
title: Ontwerpen en implementeren van een Oracle-database in Azure | Microsoft Docs
description: Ontwerpen en implementeren van een Oracle-database in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: 8241dc0303b7e60f9ce1e04e56d152c9a0b3906c
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327507"
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Een Oracle-database ontwerpen en in Azure implementeren

## <a name="assumptions"></a>Veronderstellingen

- U van plan bent een Oracle-database migreren van on-premises naar Azure.
- U hebt een goed begrip van de verschillende metrische gegevens in Oracle AWR rapporten.
- Hebt u een basislijn begrip van de prestaties van toepassingen en het gebruik van platform.

## <a name="goals"></a>Doelstellingen

- Inzicht in het optimaliseren van uw Oracle-implementatie in Azure.
- Verken opties voor een Oracle-database in een Azure-omgeving afstemmen van de prestaties.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>De verschillen tussen een on-premises en Azure-implementatie 

Hier volgen enkele belangrijke zaken waarmee u rekening moet houden wanneer u bij het migreren van on-premises toepassingen naar Azure. 

Een belangrijk verschil is dat de resources zoals virtuele machines, schijven en virtuele netwerken in een Azure-implementatie worden gedeeld door andere clients. Resources kunnen bovendien worden beperkt op basis van de vereisten. In plaats van voor het voorkomen van (MBTF) mislukt, wordt Azure meer gericht op functionerende de fouten (MTTR).

De volgende tabel bevat enkele van de verschillen tussen een on-premises implementatie en een Azure-implementatie van een Oracle-database.

> 
> |  | **On-premises implementatie** | **Azure-implementatie** |
> | --- | --- | --- |
> | **Netwerken** |LAN/WAN  |SDN (software gedefinieerde netwerken)|
> | **Beveiligingsgroep** |IP/poort beperking hulpprogramma 's |[Netwerkbeveiligingsgroep (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Flexibiliteit** |MBTF (gemiddelde tijd tussen fouten) |MTTR (gemiddelde tijd tot herstel)|
> | **Gepland onderhoud** |Patching/upgrades|[Beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patches of upgrades worden uitgevoerd door Azure beheerd) |
> | **Resource** |Toegewezen  |Gedeeld met andere clients|
> | **Regio's** |Datacenters |[Regioparen](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Storage** |SAN-/ fysieke schijven |[Beheerde Azure opslag](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Schalen** |Verticaal schalen |Horizontaal schalen|


### <a name="requirements"></a>Vereisten

- Bepaalt de snelheid van grootte en de groei van de database.
- Bepaalt de IOPS-vereisten die u kunt een schatting maken op basis van Oracle AWR rapporten of andere hulpmiddelen voor netwerkbeheer.

## <a name="configuration-options"></a>Configuratie-opties

Er zijn vier potentiële probleemgebieden die u stemmen kunt voor betere prestaties in een Azure-omgeving:

- Grootte virtuele machine
- Netwerkdoorvoer
- Schijftypen en configuraties
- Cache-instellingen voor schijf

### <a name="generate-an-awr-report"></a>Een rapport AWR genereren

Als u een bestaande een Oracle-database, en u van plan bent te migreren naar Azure, hebt u verschillende mogelijkheden. U kunt het rapport Oracle AWR om op te halen van de metrische gegevens (IOPS, Mbps, GiBs, enzovoort) uitvoeren. Kies vervolgens de virtuele machine op basis van de metrische gegevens die u hebt verzameld. Of u kunt contact opnemen met uw team voor infrastructuur voor vergelijkbare informatie.

Kunt u overwegen uw rapport AWR uitvoeren tijdens de normale en piekuren workloads, zodat u kunt vergelijken. Op basis van deze rapporten, u kunt het formaat van de virtuele machines op basis van de gemiddelde workload of de maximale werkbelasting.

Hieronder volgt een voorbeeld van hoe u een rapport AWR genereren:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Belangrijke metrische gegevens

Hieronder vindt u de metrische gegevens die u uit het rapport AWR verkrijgen kunt:

- Totale aantal kernen
- Kloksnelheid van CPU
- Totaal geheugen in GB
- CPU-gebruik
- Gegevensoverdrachtssnelheid piek
- Aantal i/o-wijzigingen (lezen/schrijven)
- Opnieuw uitvoeren van logboek tarief (MBPs)
- Netwerkdoorvoer
- Netwerk latentie tarief (laag/hoog)
- Grootte van de database in GB
- Bytes ontvangen via SQL * Net van/naar client

### <a name="virtual-machine-size"></a>Grootte virtuele machine

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. Schatting VM-grootte op basis van CPU, geheugen en i/o-gebruik van het rapport AWR

U kijken naar kunt wat is de top vijf getimede voorgrond gebeurtenissen die aangeven waar de knelpunten van het systeem zijn.

In het volgende diagram wordt is de synchronisatie van log-bestand bijvoorbeeld aan de bovenkant. Deze geeft het aantal wachten op die moet voldaan worden voordat de LGWR de logboekbuffer naar het logboekbestand opnieuw schrijft. Deze resultaten blijkt dat betere presterende opslag of schijven vereist zijn. Het diagram toont bovendien ook het aantal CPU (kernen) en de hoeveelheid geheugen.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/cpu_memory_info.png)

Het volgende diagram toont de totale i/o van lezen en schrijven. Er zijn 59 lezen en 247.3 GB geschreven tijdens het ophalen van het rapport.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Kies een virtuele machine

Op basis van de informatie die u hebt verzameld uit het rapport AWR, is de volgende stap om te kiezen van een virtuele machine met een vergelijkbare grootte die voldoet aan uw vereisten. U kunt een lijst met beschikbare virtuele machines vinden in het artikel [geoptimaliseerd voor geheugen](../../linux/sizes-memory.md).

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. De grootte van de virtuele machine met een vergelijkbare VM-reeks op basis van de ACU aanpassen

Nadat u de virtuele machine hebt gekozen, Let op de ACU voor de virtuele machine. U kunt een andere virtuele machine op basis van de ACU-waarde die beter aansluit bij uw vereisten. Zie voor meer informatie, [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Schermafbeelding van de ACU eenheden-pagina](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Netwerkdoorvoer

Het volgende diagram toont de relatie tussen de doorvoer en IOPS:

![Schermafbeelding van doorvoer](./media/oracle-design/throughput.png)

De totale netwerkdoorvoer wordt geschat op basis van de volgende informatie:
- SQL * Net verkeer
- MBps maal het aantal servers (uitgaande stroom, zoals Oracle Data Guard)
- Andere factoren, zoals het repliceren van toepassingen

![Schermafbeelding van de SQL * Net doorvoer](./media/oracle-design/sqlnet_info.png)

Op basis van uw netwerkvereisten voor bandbreedte, zijn er diverse soorten gateways voor u om de verkeersbelasting. Het gaat hierbij om basic, VpnGw en Azure ExpressRoute. Zie voor meer informatie de [pagina met prijzen voor VPN-gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/?v=17.23h).

**Aanbevelingen**

- De netwerklatentie hoger vergeleken met een on-premises implementatie. Netwerk round trips kan aanzienlijk verminderen, de prestaties verbeteren.
- Als u wilt verkleinen retouren, consolideren toepassingen met hoge transacties of 'intensieve' apps op dezelfde virtuele machine.

### <a name="disk-types-and-configurations"></a>Schijftypen en configuraties

- *Standaard besturingssysteemschijven*: Deze schijftypen bieden uw permanente gegevens en caching. Ze zijn geoptimaliseerd voor het besturingssysteem toegang bij het opstarten en zijn niet bedoeld voor transactionele of datawarehouse-workloads (analytische).

- *Niet-beheerde schijven*: Met deze schijftypen, moet u de storage-accounts die opslaan van de virtuele harde schijf (VHD)-bestanden die met de VM-schijven overeenkomen beheren. VHD-bestanden worden opgeslagen als pagina-blobs in Azure storage-accounts.

- *Beheerde schijven*: Azure beheert de storage-accounts die u voor de VM-schijven gebruikt. Geeft u het schijftype (premium of standard) en de grootte van de schijf die u nodig hebt. Azure maakt en beheert de schijf voor u.

- *Premium-opslagschijven*: Deze schijftypen zijn zeer geschikt voor werkbelastingen voor productie. Premium-opslag biedt ondersteuning voor VM-schijven die kunnen worden gekoppeld aan specifieke grootte-series-VM's, zoals Active Directory-, DSv2-, GS- en F-serie VM's. De premium-schijf wordt geleverd met verschillende formaten en kunt u kiezen tussen schijven, variërend van 32 GB tot 4096 GB. De grootte van elke schijf heeft een eigen prestatiespecificaties. Afhankelijk van uw toepassingsvereisten kunt u een of meer schijven koppelen aan uw virtuele machine.

Wanneer u een nieuwe beheerde schijf via de portal maakt, kunt u de **accounttype** voor het type van de schijf die u wilt gebruiken. Houd er rekening mee dat niet alle beschikbare schijven worden weergegeven in de vervolgkeuzelijst. Nadat u een specifieke VM-grootte kiest, ziet u het menu alleen de premiumopslag van de beschikbare SKU's die zijn gebaseerd op deze VM-grootte.

![Schermafbeelding van de beheerde schijf-pagina](./media/oracle-design/premium_disk01.png)

Nadat u uw opslag op een virtuele machine hebt geconfigureerd, kunt u laden van de schijven te testen voordat u een database te maken. De i/o-snelheid in termen van zowel de latentie en doorvoer te weten, kunt u bepalen of de virtuele machines de verwachte doorvoer met latentie doelen ondersteunen.

Er zijn een aantal hulpprogramma's voor belastingstesten van toepassingen, zoals Oracle Orion Sysbench en Fio.

Voer de belastingtest opnieuw uit nadat u een Oracle-database hebt geïmplementeerd. Start uw normale en piekuren workloads en de resultaten ziet u de basislijn van uw omgeving.

Het is mogelijk om de grootte van de opslag op basis van het tarief voor IOPS in plaats van de grootte van de belangrijkste. Bijvoorbeeld, als de vereiste IOPS 5.000 is, maar u slechts 200 GB hoeft, kunt u nog steeds krijgen de P30 klasse premium-schijf zelfs als deze wordt geleverd met meer dan 200 GB aan opslagruimte.

De snelheid IOP's kan worden verkregen uit het rapport AWR. Dit wordt bepaald door het logboek opnieuw, de fysieke leesbewerkingen en de snelheid van schrijfbewerkingen.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/awr_report.png)

Bijvoorbeeld, is de grootte van de opnieuw 12,200,000 bytes per seconde, die gelijk aan 11.63 MBPs is.
De IOPS is 12.200.000 / 2,358 = 5,174.

Nadat u een helder beeld van de i/o-vereisten hebt, kunt u een combinatie van stations die bij uitstek geschikt zijn om te voldoen aan deze vereisten.

**Aanbevelingen**

- Voor gegevens verdeeld tabelruimte, de i/o-werkbelasting over een aantal schijven met behulp van beheerde opslag of Oracle Asm installeren.
- Als de grootte van het i/o-blok voor lees-intensieve en write-intensieve bewerkingen toeneemt, Voeg meer gegevensschijven.
- Verhoog de grootte van het blok voor grote opeenvolgende processen.
- Compressie van gegevens gebruiken om te beperken van i/o (voor zowel gegevens als indexen).
- Afzonderlijke opnieuw Logboeken, systeem- en temps en ongedaan maken van TS op afzonderlijke gegevensschijven.
- Plaats geen toepassingsbestanden op de standaard OS-schijven (/ dev/sda). Deze schijven niet zijn geoptimaliseerd voor snelle VM boot keer, en ze bieden mogelijk niet goede prestaties voor uw toepassing.

### <a name="disk-cache-settings"></a>Cache-instellingen voor schijf

Er zijn drie opties voor opslaan in cache:

- *Alleen-lezen*: Alle aanvragen voor toekomstige leesbewerkingen in cache zijn opgeslagen. Alle schrijfbewerkingen zijn rechtstreeks naar Azure Blob-opslag opgeslagen.

- *Lezen / schrijven*: Dit is een "read-ahead" algoritme. De lees- en schrijfbewerkingen in cache zijn opgeslagen voor toekomstige leesbewerkingen. Niet-write-through schrijfbewerkingen worden persistent gemaakt met de lokale cache eerst. Schrijfbewerkingen worden doorgevoerd naar Azure Storage voor SQL Server, omdat het gebruikmaakt van write-through. Het biedt ook de laagste latentie van de schijf voor lichte workloads.

- *Geen* (uitgeschakeld): Deze optie gebruikt, kunt u de cache overslaan. Alle de gegevens worden overgebracht naar de schijf en persistent gemaakt met Azure Storage. Deze methode biedt de hoogste i/o-snelheid voor i/o-intensieve workloads. U moet ook "transactiekosten" rekening mee te houden.

**Aanbevelingen**

Als u wilt de doorvoer te maximaliseren, raden we aan dat u met Start **geen** voor opslaan in cache. Voor Premium-opslag, houd er rekening mee moet u de 'barrières"uitschakelen wanneer u het bestandssysteem met koppelt de **ReadOnly** of **geen** opties. Het bestand/etc/fstab bijwerken met de UUID naar de schijven.

![Schermafbeelding van de beheerde schijf-pagina](./media/oracle-design/premium_disk02.png)

- Bij een besturingssysteemschijf gebruiken standaard **lezen/schrijven** opslaan in cache.
- Gebruik voor het systeem, TEMP en ongedaan maken **geen** voor opslaan in cache.
- Voor gegevens, gebruikt u **geen** voor opslaan in cache. Maar als uw database alleen-lezen of lees-intensieve is, gebruikt u **alleen-lezen** opslaan in cache.

Nadat de instelling van de gegevens-schijf zijn opgeslagen, kunt u de host de cache-instelling niet wijzigen, tenzij u het station op het niveau van het besturingssysteem ontkoppelen en koppelen nadat u de wijziging hebt aangebracht.


## <a name="security"></a>Beveiliging

Nadat u hebt ingesteld en geconfigureerd van uw Azure-omgeving, wordt de volgende stap is om uw netwerk te beveiligen. Hier volgen enkele aanbevelingen:

- *Beleid voor NSG*: NSG kan worden gedefinieerd door een subnet of NIC. Het is eenvoudiger om toegang te beheren op het subnetniveau zowel voor beveiliging en werking routering voor zaken zoals firewalls van toepassingen.

- *Jumpbox*: Voor veiliger toegang beheerders moeten niet rechtstreeks verbinding maken met de toepassingsservice of de database. Een jumpbox wordt gebruikt als een medium tussen de administrator-machine en de Azure-resources.
![Schermafbeelding van de Jumpbox-topologie-pagina](./media/oracle-design/jumpbox.png)

    De beheerder-machine moet IP beperkte toegang tot de jumpbox alleen aanbieden. De jumpbox hebt toegang tot de toepassing en de database.

- *Particulier netwerk* (subnetten): Het is raadzaam dat u hebt de toepassingsservice en de database op afzonderlijke subnetten, zodat betere controle kan worden ingesteld door NSG-beleid.


## <a name="additional-reading"></a>Meer lezen

- [Oracle ASM configureren](configure-oracle-asm.md)
- [Oracle Data Guard configureren](configure-oracle-dataguard.md)
- [Oracle Golden Gate configureren](configure-oracle-golden-gate.md)
- [Oracle back-up en herstel](oracle-backup-recovery.md)

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)
- [Azure CLI-voorbeelden voor VM-implementatie verkennen](../../linux/cli-samples.md)
