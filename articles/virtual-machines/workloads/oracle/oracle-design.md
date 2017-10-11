---
title: Ontwerpen en implementeren van een Oracle-database in Azure | Microsoft Docs
description: Ontwerpen en implementeren van een Oracle-database in uw Azure-omgeving.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 1af7e1d40a0eb129875dd6a30ac899f2025bee13
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Ontwerpen en implementeren van een Oracle-database in Azure

## <a name="assumptions"></a>Veronderstellingen

- U bent van plan te migreren van een Oracle-database van on-premises naar Azure.
- U hebt een goed begrip van de verschillende metrische gegevens in rapporten voor Oracle AWR.
- U hebt een basislijn begrip van de prestaties van toepassingen en platforms te gebruiken.

## <a name="goals"></a>Doelstellingen

- Begrijpen hoe uw implementatie Oracle in Azure te optimaliseren.
- Gebruik de opties voor een Oracle-database in een Azure-omgeving afstemmen van de prestaties.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>De verschillen tussen een on-premises en Azure-implementatie 

Hier volgen een aantal belangrijke zaken rekening moet houden wanneer u bij het migreren van on-premises toepassingen naar Azure. 

Een belangrijk verschil is dat de resources, zoals virtuele machines, schijven en virtuele netwerken in een Azure-implementatie moet worden gedeeld door andere clients. Resources kunnen bovendien worden beperkt op basis van de vereisten. In plaats van te focussen op het voorkomen (MBTF) mislukt, wordt Azure meer gericht op functionerende van de fout (MTTR).

De volgende tabel bevat enkele van de verschillen tussen een lokale implementatie en een Azure-implementatie van een Oracle-database.

> 
> |  | **Lokale implementatie** | **Azure-implementatie** |
> | --- | --- | --- |
> | **Netwerken** |LAN/WAN  |SDN (software gedefinieerde netwerken)|
> | **Beveiligingsgroep** |Hulpprogramma's voor beperking van de IP/poort |[Netwerkbeveiligingsgroep (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Herstelmogelijkheden** |MBTF (gemiddelde tijd tussen storingen) |MTTR (gemiddelde tijd Recovery)|
> | **Gepland onderhoud** |Patchen of upgrades worden uitgevoerd|[Beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patchen/upgrades worden beheerd door Azure) |
> | **Resource** |Toegewezen  |Gedeeld met andere clients|
> | **Regio 's** |Datacenters |[Regio paren](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Storage** |SAN of fysieke schijven |[Beheerde Azure-opslag](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Schalen** |Verticale schaal |Horizontaal schalen|


### <a name="requirements"></a>Vereisten

- Bepaalt de snelheid van grootte en de groei van de database.
- Bepaal de vereisten IOP's, die u kunt een schatting maken op basis van Oracle AWR rapporten of andere hulpprogramma's voor Netwerkcontrole.

## <a name="configuration-options"></a>Configuratie-opties

Er zijn vier mogelijke gebieden die u afstemmen kunt om de prestaties verbeteren in een Azure-omgeving:

- Grootte van virtuele machine
- Netwerkdoorvoer
- Schijftypen en configuraties
- Schijf-cache-instellingen

### <a name="generate-an-awr-report"></a>Een AWR genereren

Als u een bestaande een Oracle-database en u van plan bent om te migreren naar Azure, hebt u verschillende mogelijkheden. U kunt het rapport Oracle AWR ophalen van de metrische gegevens (IOP's, Mbps, GiBs, enzovoort) uitvoeren. Kies vervolgens de virtuele machine op basis van de metrische gegevens die u hebt verzameld. Of u kunt contact opnemen met uw team voor infrastructuur voor vergelijkbare informatie.

U kunt uw rapport AWR uitvoeren tijdens normale en piekuren werkbelastingen, zodat u kunt vergelijken. Op basis van deze rapporten, kunt u de virtuele machines op basis van de gemiddelde werkbelasting of de maximale werkbelasting groot.

Hieronder volgt een voorbeeld van hoe u een rapport AWR genereren:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>De belangrijkste metrische gegevens

Hieronder volgen de metrische gegevens die u kunt het rapport AWR ophalen:

- Totaal aantal kernen
- CPU-kloksnelheid
- Totale geheugen in GB
- CPU-gebruik
- Overdrachtssnelheid piek
- Het aantal i/o-wijzigingen (lezen/schrijven)
- Opnieuw logboek snelheid (MBPs)
- Netwerkdoorvoer
- Netwerk latentie snelheid (laag/hoog)
- Databasegrootte in GB
- Bytes ontvangen via SQL * Net van/naar client

### <a name="virtual-machine-size"></a>Grootte van virtuele machine

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. VM-grootte van een schatting op basis van CPU, geheugen en i/o-gebruik van het rapport AWR

U kunt bekijken wat is de bovenste vijf is een time-out opgetreden voor gebeurtenissen die aangeven waar de knelpunten in het systeem zijn.

In het volgende diagram wordt is de synchronisatie log-bestand bijvoorbeeld aan de bovenkant. Dit geeft het aantal wacht die vereist zijn voordat de LGWR de logboekbuffer naar het logboekbestand opnieuw schrijft. Deze resultaten blijkt dat beter presterende opslag of schijven vereist zijn. Het diagram toont bovendien ook het aantal CPU (kernen) en de hoeveelheid geheugen.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/cpu_memory_info.png)

Het volgende diagram toont het totale aantal i/o lezen en schrijven. Er zijn 59 lezen en 247.3 GB geschreven tijdens de periode van het rapport.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Kies een virtuele machine

Op basis van de informatie die u uit het rapport AWR hebt verzameld, is de volgende stap om te kiezen van een virtuele machine met een vergelijkbare grootte die voldoet aan uw vereisten. U vindt een lijst met beschikbare virtuele machines in het artikel [geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. De grootte van de virtuele machine met een vergelijkbare VM-reeks op basis van de ACU stemmen

Nadat u de virtuele machine hebt gekozen, moet u aandacht schenken aan de ACU voor de virtuele machine. U kunt een andere virtuele machine op basis van de waarde ACU dat beter past bij uw vereisten. Zie voor meer informatie [Azure compute eenheid](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Schermafbeelding van de pagina ACU-eenheden](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Netwerkdoorvoer

Het volgende diagram toont de relatie tussen de doorvoer en IOP's:

![Schermopname van doorvoer](./media/oracle-design/throughput.png)

De totale netwerkdoorvoer geschat op basis van de volgende informatie:
- SQL * Net verkeer
- MBps x het aantal servers (uitgaande stream zoals Oracle Data Guard)
- Andere factoren, zoals toepassing replicatie

![Schermafbeelding van de SQL * Net doorvoer](./media/oracle-design/sqlnet_info.png)

Op basis van de vereisten van uw netwerkbandbreedte, zijn er verschillende gatewaytypen waarmee u kunt kiezen uit. Het gaat hierbij om basic, VpnGw en Azure ExpressRoute. Zie voor meer informatie de [VPN-gateway pagina met prijzen](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Aanbevelingen**

- Netwerklatentie hoger vergeleken met een on-premises implementatie. Netwerk ronde reizen kan aanzienlijk verminderen, de prestaties verbeteren.
- Als u retourbewerkingen, worden geconsolideerd toepassingen met hoge transacties of 'chatty' apps op dezelfde virtuele machine.

### <a name="disk-types-and-configurations"></a>Schijftypen en configuraties

- *Standaard OS schijven*: deze schijftypen bieden permanente gegevens en opslaan in cache. Ze zijn geoptimaliseerd voor OS toegang bij het opstarten en zijn niet bedoeld voor transactionele of datawarehouse-werkbelastingen (analytische).

- *Schijven zonder begeleiding*: met deze schijftypen die u beheert de opslagaccounts die voor het opslaan van de virtuele harde schijf (VHD)-bestanden die met uw VM-schijven overeenkomen. VHD-bestanden worden opgeslagen als pagina-blobs in Azure storage-accounts.

- *Schijven die worden beheerd*: Azure beheert de storage-accounts die u voor uw VM-schijven gebruikt. U geeft het schijftype (premium of standaard) en de grootte van de schijf die u nodig hebt. Azure maakt en beheert de schijf voor u.

- *Premium-opslag-schijven*: deze schijftypen geschikt zijn voor productieworkloads. Premium-opslag biedt ondersteuning voor VM-schijven die kunnen worden bijgevoegd aan een specifieke grootte-serie-VM's, zoals Active Directory, DSv2 GS en F-serie virtuele machines. De premium-schijf is voorzien van verschillende grootte en kunt u kiezen tussen schijven variërend van 32 GB tot 4096 GB. De grootte van elke schijf heeft zijn eigen prestatiespecificaties. Afhankelijk van uw toepassing, kunt u een of meer schijven aan uw virtuele machine te koppelen.

Wanneer u een nieuwe beheerde schijf van de portal maakt, kunt u de **accounttype** voor het type schijf dat u wilt gebruiken. Houd er rekening mee dat niet alle beschikbare schijven worden weergegeven in de vervolgkeuzelijst. Nadat u een bepaalde VM-grootte hebt gekozen, ziet u het menu alleen de beschikbare premium-opslag SKU's die zijn gebaseerd op deze VM-grootte.

![Schermafbeelding van de beheerde schijfpagina](./media/oracle-design/premium_disk01.png)

Zie voor meer informatie [beheerde schijven voor virtuele machines en hoge prestaties Premium-opslag](https://docs.microsoft.com/azure/storage/storage-premium-storage).

Nadat u uw opslag op een virtuele machine hebt geconfigureerd, kunt u laden van de schijven te testen voordat u een database te maken. De i/o-snelheid in termen van de latentie en de doorvoer weten, kunt u bepalen of de VM's de verwachte doorvoer latentie doelen ondersteunen.

Er zijn een aantal hulpprogramma's voor de toepassing werklast testen, zoals Oracle Orion Sysbench en Fio.

Voer de test load opnieuw uit nadat u een Oracle-database hebt geïmplementeerd. Start de werkbelasting van uw normale en piekuren en de resultaten staat dat u de basislijn van uw omgeving.

Kan het zijn belangrijker grootte van de opslag op basis van de snelheid van de IOPS in plaats van de grootte van de opslagruimte. Bijvoorbeeld, als het aantal IOPS dat vereist 5.000 is, maar u alleen 200 GB hoeft, u mogelijk nog steeds de P30 klasse premium schijf ook al beschikt u over meer dan 200 GB aan opslagruimte.

De snelheid IOP's kan worden verkregen van het rapport AWR. Dit wordt bepaald door het logboek opnieuw, fysieke leesbewerkingen en snelheid van schrijfbewerkingen.

![Schermafbeelding van de rapportpagina AWR](./media/oracle-design/awr_report.png)

Bijvoorbeeld, is de grootte van de opnieuw 12,200,000 bytes per seconde die gelijk aan 11.63 MBPs is.
Het aantal IOPS dat is 12.200.000 / 2,358 = 5,174.

Nadat u een duidelijk beeld van de i/o-vereisten hebt, kunt u een combinatie van stations die geschikt zijn om te voldoen aan deze vereisten zijn.

**Aanbevelingen**

- Voor gegevens verdeeld tabelruimte, de i/o-werkbelasting over een aantal schijven met behulp van beheerde opslaggroep of Oracle ASM.
- Als de blokgrootte van i/o-voor lees-intensieve en write-intensieve bewerkingen toeneemt, Voeg meer gegevensschijven.
- Vergroot de blokgrootte voor grote opeenvolgende processen.
- Gegevenscompressie gebruiken om te beperken van i/o (voor gegevens en indexen).
- Afzonderlijke opnieuw Logboeken, systeem en temps en ongedaan maken van TS op afzonderlijke gegevensschijven.
- Niet alle toepassingsbestanden op standaard OS-schijven (/ dev/sda) plaatsen. Deze schijven worden niet geoptimaliseerd voor snelle VM opstarttijden, en ze bieden geen goede prestaties voor uw toepassing.

### <a name="disk-cache-settings"></a>Schijf-cache-instellingen

Er zijn drie opties voor hostcaching:

- *Alleen-lezen*: alle aanvragen in cache zijn opgeslagen voor toekomstige leesbewerkingen. Alle schrijfbewerkingen worden vastgehouden rechtstreeks naar Azure Blob-opslag.

- *Lezen / schrijven*: dit is een 'read-ahead' algoritme. Lees- en schrijfbewerkingen in cache zijn opgeslagen voor toekomstige leesbewerkingen. Niet-write-through schrijfbewerkingen worden eerst naar de lokale cache persistent. Voor SQL Server, schrijfbewerkingen naar Azure Storage vastgehouden omdat deze write-through gebruikt. Het bevat ook de laagste latentie voor de schijf voor lichte werkbelasting.

- *Geen* (uitgeschakeld): deze optie gebruikt, kunt u de cache overslaan. Alle gegevens is overgebracht naar de schijf en opgeslagen in Azure Storage. Deze methode biedt u de hoogste i/o-snelheid voor i/o-intensieve werkbelastingen. U moet ook rekening te houden 'transactiekosten'.

**Aanbevelingen**

Als u wilt de doorvoer maximaliseren, wordt aangeraden dat u met begint **geen** voor hostcaching. Voor Premium-opslag, houd er rekening mee moet u de 'barrières"uitschakelen wanneer u het bestandssysteem met koppelt de **ReadOnly** of **geen** opties. Het bestand /etc/fstab bijwerken met de UUID aan de schijven.

Zie voor meer informatie [Premium-opslag voor virtuele Linux-machines](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Schermafbeelding van de beheerde schijfpagina](./media/oracle-design/premium_disk02.png)

- Voor OS-schijven, gebruikt u de standaard **lezen/schrijven** opslaan in cache.
- Gebruik voor het systeem, TEMP en ongedaan maken **geen** voor opslaan in cache.
- Gebruik voor gegevens, **geen** voor opslaan in cache. Maar als de database alleen-lezen of lees-intensieve is, gebruikt u **alleen-lezen** opslaan in cache.

Nadat de instelling van de schijf gegevens zijn opgeslagen, kunt u de host-cache-instelling niet wijzigen, tenzij u ontkoppelen van het station op het niveau van het besturingssysteem en koppel deze het vervolgens opnieuw nadat u de wijziging hebt aangebracht.


## <a name="security"></a>Beveiliging

Nadat u hebt ingesteld en geconfigureerd van uw Azure-omgeving, wordt de volgende stap is om uw netwerk te beveiligen. Hier volgen enkele aanbevelingen:

- *NSG-beleid*: NSG worden gedefinieerd met een subnet of NIC. Het is eenvoudiger om toegang te controleren op subnetniveau zowel voor beveiliging en werking routering voor dingen zoals firewalls, toepassing.

- *Jumpbox*: voor een beter beveiligde toegang beheerders moeten niet rechtstreeks toegang tot de toepassingsservice of de database. Een jumpbox wordt gebruikt als een medium tussen de administrator-machine en de Azure-resources.
![Schermafbeelding van de topologie Jumpbox pagina](./media/oracle-design/jumpbox.png)

    De beheerder machine moet IP beperkte toegang tot de jumpbox alleen aanbieden. De jumpbox moet toegang hebben tot de toepassing en de database.

- *Particulier netwerk* (subnetten): We raden u aan de toepassingsservice en de database op afzonderlijke subnetten, zodat betere controle kan worden ingesteld door het NSG-beleid.


## <a name="additional-reading"></a>Aanvullende bronnen

- [Oracle ASM configureren](configure-oracle-asm.md)
- [Oracle Data Guard configureren](configure-oracle-dataguard.md)
- [Oracle Golden Gate configureren](configure-oracle-golden-gate.md)
- [Oracle back-up en herstel](oracle-backup-recovery.md)

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Een maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)
- [VM-implementatie Azure CLI voorbeelden verkennen](../../linux/cli-samples.md)
