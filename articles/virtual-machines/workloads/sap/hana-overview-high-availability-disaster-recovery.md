---
title: Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Tot stand brengen voor hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61f536ee5eb27982bd63daf0b278e6c7a836fe08
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390736"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA grote instanties hoge beschikbaarheid en herstel na noodgeval op Azure 

>[!IMPORTANT]
>Deze documentatie is geen vervanging van de documentatie bij het beheer van SAP HANA of SAP-opmerkingen. Er wordt verwacht dat de lezer een goed begrip van en deskundigheid in SAP HANA-beheer en bewerkingen, vooral bij de onderwerpen van back-up, herstel, hoge beschikbaarheid en herstel na noodgevallen is.

Het is belangrijk dat u stappen en processen in uw omgeving en met uw HANA-versies en versies uitoefenen. Bepaalde processen die worden beschreven in deze documentatie voor een beter begrip van de algemene zijn vereenvoudigd en zijn niet bedoeld als gedetailleerde stappen voor uiteindelijke bewerking handboeken worden gebruikt. Als u maken van de bewerking handboeken voor uw configuraties wilt, moet u om te testen en uitoefening van uw processen en documenteren van de processen die betrekking hebben op uw specifieke configuraties. 


Hoge beschikbaarheid en herstel na noodgeval (DR) zijn belangrijke aspecten van het uitvoeren van uw essentiële SAP HANA op de server van Azure (grote instanties). Het is belangrijk om te werken met SAP, uw systeemintegrator of Microsoft om correct te ontwerpen en implementeren van de juiste hoge beschikbaarheid en strategieën voor noodherstel. Het is ook belangrijk dat u rekening houden met het beoogde herstelpunt (RPO) en de beoogde hersteltijd, die specifiek voor uw omgeving zijn.

Microsoft biedt ondersteuning voor bepaalde functies van SAP HANA hoge beschikbaarheid met HANA grote instanties. Deze mogelijkheden omvatten:

- **Storage-replicatie**: het opslagsysteem kunnen alle gegevens repliceren naar een andere HANA grote instantie stempel in een andere Azure-regio. SAP HANA werkt onafhankelijk van deze methode. Deze functionaliteit is de disaster recovery standaardmechanisme voor HANA grote instanties aangeboden.
- **HANA-systeemreplicatie**: de [replicatie van alle gegevens in SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) op een afzonderlijk SAP HANA-systeem. De beoogde hersteltijd wordt geminimaliseerd tot en met replicatie van gegevens met regelmatige intervallen. Asynchrone, synchrone in het geheugen en synchrone modus biedt ondersteuning voor SAP HANA. Synchrone modus wordt alleen gebruikt voor SAP HANA-systemen die zich binnen hetzelfde datacenter of minder dan 100 km van elkaar. Met het huidige ontwerp van HANA grote instantie stempels, kan de HANA-systeemreplicatie voor hoge beschikbaarheid binnen één regio alleen worden gebruikt. HANA-systeemreplicatie vereist een omgekeerde proxy van derden of routing onderdeel voor configuratie van herstel na noodgevallen in een andere Azure-regio. 
- **Automatische failover hosten**: een oplossing voor lokale foutherstel voor SAP HANA die is een alternatief voor HANA-systeemreplicatie. Als het hoofdknooppunt niet beschikbaar is, configureert u een of meer stand-by-SAP HANA-knooppunten in scale-out-modus, en SAP HANA automatisch failover optreedt naar een stand-by-knooppunt.

SAP HANA op Azure (grote instanties) wordt aangeboden in twee Azure-regio's in vier geopolitieke gebieden (VS, Australië, Europa en Japan). Twee regio's binnen een geopolitieke gebied die als host HANA grote instantie stempels fungeren zijn verbonden met afzonderlijke speciaal netwerk circuits. Deze worden gebruikt voor het repliceren van opslagmomentopnamen voor disaster recovery-methoden. De replicatie is standaard niet ingesteld, maar is ingesteld voor klanten die de volgorde van herstel na noodgevallen. Storage-replicatie is afhankelijk van het gebruik van storage-momentopnamen voor HANA grote instanties. Het is niet mogelijk om te kiezen van een Azure-regio als een DR-regio die zich in een ander geopolitieke gebied. 

De volgende tabel ziet u de momenteel ondersteunde hoge beschikbaarheid en noodherstel herstelmethoden en combinaties:

| Scenario worden ondersteund in HANA grote instanties | Optie voor hoge beschikbaarheid | Optie voor herstel na noodgevallen | Opmerkingen |
| --- | --- | --- | --- |
| Eén knooppunt | Niet beschikbaar. | Toegewezen DR-instellingen.<br /> De installatie van de Multipurpose DR. | |
| Automatische failover hosten: Scale-out (met of zonder stand-by)<br /> met inbegrip van 1 + 1 | Mogelijk is met de stand-by houdend met de actieve rol.<br /> HANA Hiermee bepaalt u de rol-switch. | Toegewezen DR-instellingen.<br /> De installatie van de Multipurpose DR.<br /> DR-synchronisatie met behulp van storage-replicatie. | HANA volumesets zijn gekoppeld aan alle knooppunten.<br /> DR-site moet hetzelfde aantal knooppunten hebben. |
| HANA-systeemreplicatie | Mogelijk is met de primaire of secundaire setup.<br /> Secundaire verplaatst naar de primaire rol in het geval van een failover.<br /> HANA-systeemreplicatie en OS beheren failover. | Toegewezen DR-instellingen.<br /> De installatie van de Multipurpose DR.<br /> DR-synchronisatie met behulp van storage-replicatie.<br /> Herstel na Noodgevallen via HANA system replication is nog niet mogelijk zonder onderdelen van derden. | Afzonderlijke set van volumes op schijven zijn gekoppeld aan elk knooppunt.<br /> Alleen volumes op schijven van de secundaire replica in de productiesite worden gerepliceerd naar de locatie voor herstel na Noodgevallen.<br /> Een verzameling van volumes is vereist op de DR-site. | 

Een toegewezen DR-configuratie is waar de eenheid HANA grote instantie in de DR-site niet wordt gebruikt voor het uitvoeren van een andere werkbelasting of niet-productieomgeving. De eenheid is een passieve en is geïmplementeerd alleen als er een ramp-failover wordt uitgevoerd. Deze instelling is echter niet een uitstekende keuze is voor veel klanten.

Raadpleeg [HLI scenario's ondersteund](hana-supported-scenario.md) voor meer informatie over storage lay-out en Ethernet-details voor uw architectuur.

> [!NOTE]
> [SAP HANA MCOD implementaties](https://launchpad.support.sap.com/#/notes/1681092) (meerdere HANA instanties in één eenheid) teken met scenario's werken met de HA en DR methoden die worden beschreven in de tabel. Een uitzondering is het gebruik van HANA-Systeemreplicatie met een automatische failover-cluster op basis van Pacemaker. Een dergelijk geval ondersteunt slechts één HANA-exemplaar per eenheid. Voor [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) implementaties, alleen niet-opslag-methoden op basis van HA en DR werken als meer dan één tenant wordt geïmplementeerd. Alle methoden die worden vermeld, met één tenant geïmplementeerd, zijn geldig.  

Een configuratie voor meerdere doeleinden DR is waarop een niet-productie-werkbelasting in de eenheid HANA grote instantie op de DR-site wordt uitgevoerd. Koppel het storage-replicatie (Extra) volume wordt in geval van nood, de niet-productie-systeem afsluiten en start de productie-HANA-instantie. De meeste klanten die gebruikmaken van de functie voor herstel na noodgevallen HANA grote instantie deze configuratie gebruiken. 


U vindt meer informatie over SAP HANA met hoge beschikbaarheid in de volgende artikelen voor SAP: 

- [Technisch document hoge beschikbaarheid van SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Gebruikershandleiding voor SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA Academy Video op SAP HANA-Systeemreplicatie](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP-ondersteuning Opmerking #1999880: veelgestelde vragen over SAP HANA-Systeemreplicatie](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [SAP-ondersteuning Opmerking #2165547: SAP HANA Back up en herstel binnen de omgeving van SAP HANA System Replication](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP-ondersteuning Opmerking #1984882 – met behulp van SAP HANA System Replication voor Hardware-uitwisseling met minimale/nul Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Aandachtspunten voor netwerken voor herstel na noodgeval met HANA grote instanties

Om te profiteren van het herstel na noodgevallen van HANA grote instanties, moet u de netwerkverbinding met de twee Azure-regio's te ontwerpen. Moet u een Azure ExpressRoute-circuit-verbinding van on-premises in uw Azure-regio en een andere circuitverbinding van on-premises naar de Dr-regio. Deze meting bevat informatie over een situatie waarin er een probleem in een Azure-regio is, met inbegrip van de locatie van een Microsoft Enterprise Edge Router (MSEE).

Als een tweede maat, kunt u alle Azure-netwerken die verbinding met SAP HANA op Azure (grote instanties maken) in één regio aan een ExpressRoute-circuit dat is verbonden HANA grote instanties in de andere regio. Met deze *cross-verbinding maken met*, die worden uitgevoerd op een Azure-services virtueel netwerk in de regio 1 verbinding kan maken met HANA grote instantie eenheden in de regio 2 en andersom. Deze meting wordt een aanvraag waarin slechts één van de MSEE-locaties die verbinding met uw on-premises locatie met Azure maakt offline gaat.

De volgende afbeelding ziet u de configuratie van een robuuste voor disaster recovery gevallen:

![Optimale configuratie voor herstel na noodgevallen](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>Andere vereisten met HANA grote instanties storage-replicatie voor herstel na noodgevallen

Naast de voorgaande vereisten voor een installatie van noodhersteladapter met HANA grote instanties moet u:

- Volgorde van SAP HANA op Azure (grote instanties)-SKU's van de even groot is als uw productie-SKU's en implementeer deze in de Dr-regio. In de huidige implementaties van klanten, zijn deze instanties worden gebruikt om uit te voeren niet-productie HANA-instanties. Deze configuraties worden aangeduid als *multipurpose DR-instellingen*.   
- Volgorde van extra opslagruimte op de DR-site voor elk van uw SAP HANA op Azure (grote instanties)-SKU's die u wilt herstellen in de site voor noodherstel. Extra opslag kopen, kunt u de opslagvolumes toewijzen. U kunt de volumes die het doel van de storage-replicatie van de productie van Azure-regio in de Azure-regio voor noodherstel kunt toewijzen.
- In het geval is, waarbij u HSR setup op de primaire hebt en u op basis van opslagreplicatie naar de DR-site instellen, moet u extra opslagruimte in de DR-site zodat zowel de primaire aanschaffen en gegevens van secundaire knooppunten worden gerepliceerd naar de DR-site.

 **Volgende stappen**
- Raadpleeg [back-up en herstel](hana-backup-restore.md).













