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
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 554991c7c0f11a095a11ae24dbb693a1a3ba50fd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430117"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA grote instanties hoge beschikbaarheid en herstel na noodgeval op Azure 

>[!IMPORTANT]
>Deze documentatie is geen vervanging van de documentatie bij het beheer van SAP HANA of SAP-opmerkingen. Er wordt verwacht dat de lezer een goed begrip van en deskundigheid in SAP HANA-beheer en bewerkingen, vooral bij de onderwerpen van back-up, herstel, hoge beschikbaarheid en herstel na noodgevallen is. Schermafbeeldingen van SAP HANA Studio worden in deze documentatie wordt weergegeven. Inhoud, de structuur en de aard van de schermen van SAP-beheerprogramma's en de hulpprogramma's zelf kunnen worden gewijzigd van SAP HANA, release-versie.

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

 

## <a name="backup-and-restore"></a>Back-ups en herstellen

Een van de belangrijkste aspecten aan operationele databases is Beveilig ze tegen catastrofale gebeurtenissen. De oorzaak van deze gebeurtenissen kan van alles van natuurrampen gebruiksvriendelijke fouten zijn.

Back-ups van een database met de mogelijkheid om het te herstellen naar een willekeurig punt in tijd (zoals voordat iemand kritieke gegevens verwijderd), kunnen worden hersteld naar een status die zich zo dicht mogelijk in de manier waarop het was voordat het wordt onderbroken.

Twee typen back-ups moeten worden uitgevoerd voor de beste resultaten:

- Databaseback-ups: volledige, incrementele of differentiële back-ups
- Transactielogboekback-ups

Naast de volledige-databaseback-ups uitgevoerd op het toepassingsniveau van een, kunt u back-ups met storage-momentopnamen uitvoeren. Storage-momentopnamen vervangen transactielogboekback-ups niet. Transactielogboekback-ups blijven belangrijk de database herstellen naar een bepaald punt in tijd of de logboeken van al opgegeven transacties leeg. Storage-momentopnamen kunnen echter recovery versnellen door snel een afbeelding doorvoeren van de database. 

SAP HANA op Azure (grote instanties) biedt twee opties voor back-up en herstel:

- Doe (DIY). Nadat u berekenen om ervoor te zorgen dat er voldoende schijfruimte beschikbaar is, voert u volledige database en logboekback-ups met behulp van een van de volgende methoden van de schijf-back-up. U kunt back-up ofwel rechtstreeks op volumes die zijn gekoppeld aan de HANA grote instantie-eenheden, of voor Network File Shares (NFS) die zijn ingesteld op een Azure-machine (VM). In dat laatste geval, klanten instellen van een Linux-VM in Azure, Azure Storage koppelen aan de virtuele machine en deel van de opslag via een geconfigureerde NFS-server in die virtuele machine. Als u de back-up op basis van volumes die rechtstreeks aan HANA grote instantie eenheden koppelen uitvoert, moet u de back-ups naar Azure storage-account kopiëren (na het instellen van een Azure-VM die Hiermee exporteert u NFS-shares die zijn gebaseerd op Azure Storage). U kunt ook een Azure Backup-kluis of een Azure koude opslag gebruiken. 

   Een andere optie is het gebruik van een derde partij hulpprogramma voor gegevensbeveiliging voor het opslaan van de back-ups nadat ze zijn gekopieerd naar een Azure storage-account. De back-upoptie voor zelf kan ook zijn die nodig zijn voor gegevens die u nodig hebt om op te slaan voor langere tijd voor nalevings- en controledoeleinden. In alle gevallen moet worden de back-ups gekopieerd naar NFS-shares vertegenwoordigd door een virtuele machine en de Azure Storage.

- Infrastructuur voor back-up en herstellen van functionaliteit. U kunt ook gebruik van de back-up en herstellen van de functionaliteit die de onderliggende infrastructuur van SAP HANA op Azure (grote instanties) biedt. Deze optie wordt voldaan aan de noodzaak van back-ups en snelle herstelbewerkingen. De rest van deze sectie wordt de functionaliteit voor back-up en herstellen die wordt aangeboden met HANA grote instanties. In deze sectie bevat ook informatie over de relatie back-up en herstel heeft tot de disaster recovery-functionaliteit die worden aangeboden door HANA grote instanties.

>   [!NOTE]
>   SAP HANA-momentopnamen is afhankelijk van de momentopnametechnologie die wordt gebruikt door de onderliggende infrastructuur van HANA grote instanties. SAP HANA-momentopnamen werken op dit moment niet in combinatie met meerdere tenants van containers voor SAP HANA-multitenant-database. Als er slechts één tenant is geïmplementeerd, SAP HANA-momentopnamen werken en deze methode kan worden gebruikt.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Met behulp van storage-momentopnamen van SAP HANA op Azure (grote instanties)

De opslaginfrastructuur onderliggende SAP HANA op Azure (grote instanties) biedt ondersteuning voor storage-momentopnamen van volumes. Zowel back-up en herstel van volumes wordt ondersteund, met de volgende overwegingen:

- In plaats van een volledige databaseback-ups, worden opslag volume momentopnamen regelmatig.
- Bij het activeren van een momentopname over /hana/data en /hana/shared (inclusief /usr/sap) initieert volumes, de momentopnametechnologie voor van een SAP HANA momentopname voordat deze wordt uitgevoerd met de storage-momentopname. Deze SAP HANA-momentopname is het setup-punt voor het uiteindelijke log herstelbewerkingen kunnen na het herstel van de momentopname van de opslag. Voor de HANA-momentopname om succesvol te zijn, moet u een actief exemplaar van HANA.  Storage-momentopname wordt in HSR scenario wordt niet ondersteund op huidige secundaire knooppunt waar de HANA-momentopname kan niet worden uitgevoerd.
- Nadat de momentopname van de opslag is uitgevoerd, wordt de SAP HANA-momentopname verwijderd.
- Transactielogboekback-ups regelmatig worden genomen en worden opgeslagen in het volume /hana/logbackups of in Azure. U kunt het /hana/logbackups volume met de transactielogboekback-ups als u wilt maken van een momentopname van een afzonderlijk activeren. In dat geval hoeft u niet voor het uitvoeren van een momentopname van een HANA.
- Als u een database naar een bepaald punt in tijd herstellen moet, aanvragen die Microsoft Azure-ondersteuning (voor een storing in de productie) of SAP HANA op Azure Service Management terugzetten van een momentopname van een bepaalde opslag. Een voorbeeld is een geplande herstel van een sandbox-systeem in de oorspronkelijke staat.
- De SAP HANA-momentopname die opgenomen in de storage-momentopname is een offset punt voor het toepassen van transactielogboekback-ups die zijn uitgevoerd en opgeslagen nadat de storage-momentopname is gemaakt.
- Deze transactielogboekback-ups gaat naar de database herstellen naar een bepaald punt in tijd.

U kunt storage-momentopnamen die zijn gericht op drie soorten volumes uitvoeren:

- Een momentopname van een gecombineerde/hana/gegevens en /hana/shared (inclusief/usr/sap). Deze momentopname is vereist voor het maken van een momentopname van een SAP HANA als voorbereiding op de momentopname van de opslag. De SAP HANA-momentopname zorgt ervoor dat de database zich in een consistente status van een punt opslag. En dat voor de terugzetbewerking verwerken dat wil zeggen een punt om in te stellen u op.
- Een momentopname van een afzonderlijke via/hana/logbackups.
- De partitie van een besturingssysteem.

Lees het laatste momentopname scripts en documentatie van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="storage-snapshot-considerations"></a>Overwegingen met betrekking tot opslag-momentopname

>[!NOTE]
>Storage-momentopnamen gebruiken opslagruimte die is toegewezen aan de eenheden HANA grote instantie. U moet rekening houden met de volgende aspecten van de planning voor storage-momentopnamen en het aantal opslagmomentopnamen te houden. 

De specifieke mechanics van opslagmomentopnamen voor SAP HANA op Azure (grote instanties) zijn onder andere:

- Een momentopname van een specifieke opslag (op het punt in tijd waarop deze is gemaakt) verbruikt weinig opslagruimte.
- Als gegevens inhoud wordt gewijzigd en de inhoud van bestanden op het opslagvolume wijzigen SAP HANA-gegevens, moet de momentopname voor het opslaan van de oorspronkelijke inhoud blokkeren, evenals de gegevens worden gewijzigd.
- Als gevolg hiervan de storage-momentopname wordt vergroot. Hoe langer de momentopname bestaat, hoe groter de storage-momentopname wordt.
- De meer wijzigingen die zijn aangebracht aan het volume van SAP HANA-database gedurende de levensduur van een momentopname opslag, hoe groter het verbruik van de ruimte van de momentopname van de opslag.

SAP HANA op Azure (grote instanties) wordt geleverd met grootten voor de SAP HANA-gegevens en logboekvolumes vast volume. Uitvoeren van momentopnamen van de volumes eats in uw ruimte op volume. U moet bepalen bij het plannen van de storage-momentopnamen. U moet ook het gebruik van de ruimte van de opslagvolumes bewaken, evenals het aantal momentopnamen die u opslaat beheren. Wanneer u grote hoeveelheden gegevens importeren of andere belangrijke wijzigingen aangebracht in de HANA-database uitvoert, kunt u de storage-momentopnamen uitschakelen. 


De volgende secties bevatten informatie voor het uitvoeren van deze momentopnamen, met inbegrip van algemene aanbevelingen:

- Hoewel de hardware 255 momentopnamen per volume tolereren kan, die u wilt blijven goed onder dit getal. De aanbeveling is 250 of minder.
- Voordat u opslagmomentopnamen uitvoert, controleren en bijhouden van vrije ruimte.
- Verlaag het aantal opslagmomentopnamen op basis van de vrije ruimte. U kunt het aantal momentopnamen die u bijhoudt verlagen of kunt u de volumes uitbreiden. U kunt extra opslagruimte bestellen in eenheden van 1 terabyte.
- Tijdens de activiteiten, zoals het verplaatsen van gegevens in SAP HANA met hulpprogramma voor migratie van SAP-platform (R3load) of SAP HANA-databases herstellen vanuit back-ups, opslagmomentopnamen op het volume /hana/data uitschakelen 
- Tijdens de grotere reorganisaties van SAP HANA-tabellen, moeten opslagmomentopnamen worden vermeden, indien mogelijk.
- Storage-momentopnamen zijn vereist om te profiteren van de noodherstelprocessen mogelijkheden voor herstel van SAP HANA op Azure (grote instanties).

### <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Vereisten voor het gebruik van selfservice opslagmomentopnamen

Om ervoor te zorgen dat de momentopname-script met succes wordt uitgevoerd, zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties. Perl is vooraf geïnstalleerd op uw eenheid HANA grote instantie. Om te controleren of de versie van Perl, gebruikt u de volgende opdracht:

`perl -v`

![De openbare sleutel wordt door het uitvoeren van deze opdracht gekopieerd](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


### <a name="set-up-storage-snapshots"></a>Instellen van opslagmomentopnamen

Als u de storage-momentopnamen met HANA grote instanties instelt, de volgende stappen uit:
1. Zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties.
1. Wijzig de/etc/ssh/ssh\_config om toe te voegen van de regel _MACs hmac-sha1_.
1. Maak een back-SAP HANA-gebruikersaccount op het hoofdknooppunt voor elk SAP HANA-exemplaar dat u uitvoert, indien van toepassing.
1. De SAP HANA HDB-client installeren op alle servers in de SAP HANA grote instanties.
1. Maak een openbare sleutel voor toegang tot de onderliggende opslaginfrastructuur die het maken van momentopnamen bepaalt op de eerste server voor SAP HANA grote instanties van elke regio.
1. Kopieer de scripts en het configuratiebestand van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) naar de locatie van **hdbsql** in de SAP HANA-installatie.
1. Wijzig de *HANABackupDetails.txt* bestand zo nodig voor de betreffende klant-specificaties.

Lees het laatste momentopname scripts en documentatie van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="consideration-for-mcod-scenarios"></a>Overweging voor MCOD scenario 's
Als u werkt met een [MCOD scenario](https://launchpad.support.sap.com/#/notes/1681092) met meerdere instanties van SAP HANA op één HANA grote instantie-eenheid, hebt u afzonderlijke opslagvolumes die zijn ingericht voor elk van de SAP HANA-instanties. U kunt geen afzonderlijke momentopnamen op elk systeem dat HANA-exemplaar-ID (SID) te starten in de huidige versie van de momentopname van selfservice-automatisering. De functionaliteit voorziet in controles voor de geregistreerde SAP HANA-instanties van de server in het configuratiebestand (Zie verderop in dit artikel), en voert een gelijktijdige momentopname van de volumes van de exemplaren die zijn geregistreerd op de eenheid.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Stap 1: De SAP HANA HDB-client installeren

Het Linux-besturingssysteem geïnstalleerd op SAP HANA op Azure (grote instanties) bevat de mappen en scripts die nodig zijn voor het uitvoeren van SAP HANA-opslagmomentopnamen voor hersteldoeleinden voor back-up en herstel na noodgeval. Raadpleeg voor meer recente versies in [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). De meest recente versie van de scripts is 3.x. Verschillende scripts mogelijk verschillende secundaire releases binnen dezelfde primaire versie.

>[!IMPORTANT]
>Bij het verplaatsen van versie 2.1 naar versie 3.x van de scripts, houd er rekening mee dat de structuur van het configuratiebestand en sommige syntaxissen is gewijzigd. Zie de bijschriften in de specifieke secties. 

Het is uw verantwoordelijkheid voor het installeren van de SAP HANA HDB-client op de eenheden HANA grote instantie tijdens de installatie van SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Stap 2: Wijzigen/etc/ssh/ssh\_config

Wijziging `/etc/ssh/ssh_config` door toe te voegen de _MACs hmac-sha1_ regel zoals hier wordt weergegeven:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Stap 3: Maak een openbare sleutel

Voor toegang tot de interfaces van de momentopname van opslag van uw tenant HANA grote instantie, moet u een procedure aanmelden via een openbare sleutel vast. Maak op de eerste SAP HANA op Azure (grote instanties)-server in uw tenant, een openbare sleutel moet worden gebruikt voor toegang tot de opslaginfrastructuur. De openbare sleutel zorgt ervoor dat een wachtwoord niet vereist is voor aanmelding bij de storage-momentopname-interfaces. Het maken van een openbare sleutel betekent ook dat u niet wilt wachtwoordreferenties onderhouden. Voer de volgende opdracht uit om de openbare sleutel te genereren in Linux op door de SAP HANA grote instanties-server:
```
  ssh-keygen –t dsa –b 1024
```
De nieuwe locatie is **_/root/.ssh/id\_dsa.pub**. Voer een wachtwoord niet, anders moet u het wachtwoord invoeren telkens wanneer die u zich aanmeldt. Selecteer in plaats daarvan **Enter** twee keer te verwijderen van de vereiste "wachtwoord" voor het aanmelden.

Zorg ervoor dat de openbare sleutel is gecorrigeerd zoals verwacht door het wijzigen van mappen die moeten worden **/root/.ssh/** en vervolgens uitvoeren van de `ls` opdracht. Als de sleutel aanwezig is, kunt u deze kunt kopiëren met de volgende opdracht:

![De openbare sleutel wordt door het uitvoeren van deze opdracht gekopieerd](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Op dit moment, neem contact op met SAP HANA op Azure Service Management en hen voorzien van de openbare sleutel. De medewerker van de service maakt gebruik van de openbare sleutel registreren in de onderliggende opslaginfrastructuur die voor uw tenant HANA grote instantie is gehaald.

### <a name="step-4-create-an-sap-hana-user-account"></a>Stap 4: Een SAP HANA-gebruikersaccount maken

Voor het starten van het maken van momentopnamen van SAP HANA, moet u een gebruikersaccount maken in SAP HANA die de opslag momentopname-scripts kunnen gebruiken. Maak een SAP HANA-gebruikersaccount in SAP HANA Studio voor dit doel. De gebruiker moet worden gemaakt onder het SYSTEMDB en niet onder de SID-database voor MDC. Gebruiker is in de omgeving enkele container instellingen onder de tenant-database. Dit account moet de volgende bevoegdheden hebben: **back-up Admin** en **catalogus**. In dit voorbeeld wordt de gebruikersnaam is **SCADMIN**. Naam van het gebruikersaccount hebt gemaakt in HANA Studio is hoofdlettergevoelig. Zorg ervoor dat u selecteert **geen** voor het vereisen van de gebruiker het wachtwoord voor de volgende aanmelding wijzigen.

![Het maken van een gebruiker in HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Als u MCOD implementaties met meerdere SAP HANA-instanties in één eenheid gebruikt, moet u deze stap herhalen voor elke instantie van SAP HANA.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Stap 5: Machtigen de SAP HANA-gebruikersaccount

In deze stap verleent u de SAP HANA-gebruikersaccount dat u hebt gemaakt, zodat de scripts niet hoeft in te dienen wachtwoorden tijdens runtime. De SAP HANA-opdracht `hdbuserstore` wordt het maken van een sleutel van de gebruiker SAP HANA, die is opgeslagen op een of meer knooppunten van SAP HANA. De sleutel van de gebruiker krijgt de gebruiker toegang tot SAP HANA zonder het beheren van wachtwoorden op in het proces van scripts. Het uitvoeren van scripts proces wordt verderop in dit artikel besproken.

>[!IMPORTANT]
>Voer de volgende opdracht onder de gebruiker dat de scripts zijn gepland om te worden uitgevoerd. Anders wordt het script niet goed werken.

Voer de `hdbuserstore` opdracht als volgt:

**Voor de installatie van niet - MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Voor de installatie van MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

In het volgende voorbeeld wordt de gebruiker is **SCADMIN01**, de hostnaam is **lhanad01**, en het exemplaarnummer **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Als u een HANA MCOD-implementatie met meerdere SAP HANA-instanties in één eenheid gebruikt, moet u de stap herhalen voor elke instantie van SAP HANA en de bijbehorende back-gebruiker op de eenheid.

Als u de configuratie van een SAP HANA-scale-out hebt, moet u voor het beheren van alle scripts van één server. In dit voorbeeld wordt de SAP HANA-sleutel **SCADMIN01** voor elke host op een manier die laat zien welke host is gerelateerd aan de sleutel moet worden gewijzigd. De back-SAP HANA-account met het exemplaarnummer van de HANA-database te wijzigen. De sleutel moet beheerdersbevoegdheden hebben op de host waarop deze toegewezen en de back-gebruiker voor configuraties met scale-out moet toegangsrechten hebben voor alle exemplaren van de SAP HANA. Ervan uitgaande dat de drie knooppunten van de scale-out hebben namen **lhanad01**, **lhanad02**, en **lhanad03**, de volgorde van de opdrachten er als volgt uitzien:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Stap 6: Ophalen van de momentopname-scripts, de momentopnamen configureren en testen van de configuratie en connectiviteit

Download de meest recente versie van de scripts van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). De gedownloade scripts en het bestand kopiëren naar de werkmap voor **hdbsql**. Voor huidige HANA-installaties, deze map is in de volgende indeling: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Tijdens het afhandelen van Perl-scripts: 

- De scripts niet wijzigen tenzij Microsoft Operations.
- Wanneer u wordt gevraagd om het script of een parameterbestand te wijzigen, en gebruik altijd de Linux-editor, zoals 'vi' en niet een Windows-editor, zoals Kladblok. Met behulp van een Windows-editor, kan de bestandsindeling beschadigd raken.
- Gebruik altijd de meest recente scripts. U kunt de meest recente versie downloaden vanuit GitHub.
- Gebruik dezelfde versie van scripts in de liggende modus.
- Test de scripts en maak kennis met de vereiste parameters en de uitvoer van het script voordat u deze rechtstreeks in het productiesysteem.
- Naam van het koppelpunt van de server die is ingericht met Microsoft Operations niet te wijzigen. Deze scripts zijn afhankelijk van deze standaard koppelpunten moet beschikbaar zijn voor een geslaagde uitvoering.


Het doel van de andere scripts en -bestanden is als volgt:

- **Azure\_hana\_backup.pl**: met dit script is gepland met het hulpprogramma Linux Cron-planning voor het uitvoeren van opslagmomentopnamen op de HANA-gegevens en gedeelde volumes, het volume/hana/logbackups of het besturingssysteem.
- **Azure\_hana\_replicatie\_status.pl**: in dit script geeft de algemene gegevens over de replicatiestatus van de van de productiesite naar de site voor noodherstel. De script-monitors om ervoor te zorgen dat de replicatie plaatsvindt en de grootte van de items die toont worden gerepliceerd. Het bevat ook richtlijnen als een-replicatie is te lang duurt of als de koppeling niet actief is.
- **Azure\_hana\_momentopname\_details.pl**: met dit script geeft een lijst van algemene informatie over alle momentopnamen, per volume, die aanwezig zijn in uw omgeving. Met dit script kan worden uitgevoerd op de primaire server of op een server-eenheid in de locatie voor noodherstel. Het script bevat de volgende informatie, onderverdeeld op basis van elk volume dat momentopnamen bevat:
   * De grootte van het totale aantal momentopnamen in een volume
   * De volgende gegevens in elke momentopname in volume: 
      - De naam van de momentopname 
      - Aanmaaktijd 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan deze momentopname, indien van toepassing
- **Azure\_hana\_momentopname\_delete.pl**: met dit script Hiermee verwijdert u een momentopname van een opslag- of een set met momentopnamen. U kunt de back-ID van de SAP HANA zoals gevonden in HANA Studio, of de naam van de momentopname gebruiken. De back-up-ID is op dit moment alleen gekoppeld aan de momentopnamen die zijn gemaakt voor de HANA gegevens/log/gedeelde clustervolumes. Anders, als de momentopname-ID wordt opgegeven, het zoekt alle momentopnamen die overeenkomen met de opgegeven momentopname-ID.  
- **testHANAConnection.pl**: met dit script test de verbinding met de SAP HANA-instantie en is vereist voor het instellen van de storage-momentopnamen.
- **testStorageSnapshotConnection.pl**: met dit script heeft twee doeleinden. Eerst, zorgt deze ervoor dat de eenheid HANA grote instantie die wordt uitgevoerd de scripts toegang tot de toegewezen opslag virtuele machine, en de momentopname-interface van de opslag van uw HANA grote instanties heeft. Het tweede doel is het maken van een momentopname van een tijdelijke voor de HANA-instantie die u wilt testen. Met dit script moet worden uitgevoerd voor elk HANA-exemplaar op een server om ervoor te zorgen dat de back-scripts werken zoals verwacht.
- **removeTestStorageSnapshot.pl**: met dit script wordt verwijderd van de test-momentopname gemaakt met het script **testStorageSnapshotConnection.pl**.
- **Azure\_hana\_dr\_failover.pl**: met dit script initieert een DR-failover in een andere regio. Het script moet worden uitgevoerd op de eenheid HANA grote instantie in de DR-regio, of op de eenheid die u wilt een failover uitvoeren naar. Met dit script storage-replicatie vanaf de primaire naar de secundaire zijde stopt, worden de meest recente momentopname op de DR-volumes hersteld en biedt de quorumbron: voor het herstel na Noodgevallen volumes.
- **Azure\_hana\_testen\_dr\_failover.pl**: met dit script wordt een testfailover uitgevoerd in de DR-site. In tegenstelling tot het script azure_hana_dr_failover.pl onderbroken deze tot uitvoering van de storage-replicatie van primaire naar secundaire niet. In plaats daarvan klonen van de volumes gerepliceerde opslag worden gemaakt op de DR-zijde en de Stel de volgende parameter van de gekloonde volumes worden geleverd. 
- **HANABackupCustomerDetails.txt**: dit bestand is een aanpasbare configuratiebestand dat u wijzigen wilt om aan te passen aan uw SAP HANA-configuratie. De *HANABackupCustomerDetails.txt* bestand is het besturingselement en de configuratie-bestand voor het script dat wordt uitgevoerd de storage-momentopnamen. Het bestand voor uw toepassing en de instellingen aanpassen. U ontvangt de **back-up Opslagnaam** en de **opslag IP-adres** van SAP HANA op Azure Service Management bij het implementeren van uw instanties. U kunt de volgorde hebben, niet wijzigen, bestellen of afstand van een van de variabelen in dit bestand. Als u dit doet, worden de scripts niet correct uitgevoerd. Bovendien ontvangt u het IP-adres van het knooppunt omhoog schalen of het hoofdknooppunt (als scale-out) van SAP HANA op Azure Service Management. U weet ook het nummer van HANA-instantie die u tijdens de installatie van SAP HANA. Nu moet u een back-naam toevoegen aan het configuratiebestand.

Voor een scale-up- of scale-out-implementatie, zou het configuratiebestand eruit het volgende voorbeeld nadat u de naam van de server van de eenheid HANA grote instantie en het IP-adres van de server hebt ingevuld. Vul alle vereiste velden voor elke SAP HANA-SID die u wilt back-up of herstellen.

U kunt ook opmerkingen bij de rijen van exemplaren die u niet wilt dat back-up voor een bepaalde periode door toe te voegen een "#" voor een verplicht veld. U hoeft ook te alle SAP HANA-instanties die zijn opgenomen op een server als de hoeft geen back-up of herstellen van dat specifieke exemplaar invoeren. De indeling moet worden bewaard voor alle velden of alle scripts genereren een foutbericht weergegeven en het script wordt beëindigd. Extra vereiste rijen van een SID informatie gegevens u niet na de laatste SAP HANA-instantie gebruikt, kunt u verwijderen. Alle rijen moeten ofwel ingevuld, als commentaar of is verwijderd.

>[!IMPORTANT]
>De structuur van het bestand gewijzigd met de overgang van versie 2.1 naar versie 3.x. Als u de versie 3.x scripts gebruiken wilt, moet u de structuur van het configuratie-bestand aan te passen. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

Voor elke instantie die u op de eenheid HANA grote instantie configureert, of de configuratie van de scale-out, moet u de gegevens als volgt gedefinieerd:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
Voor scale-out en HANA-Systeemreplicatie configuraties, herhaalt u deze configuratie op elk van de knooppunten. Deze meting zorgt ervoor dat in gevallen mislukt de back-ups en de uiteindelijke storage-replicatie blijven werken.   

Nadat u de configuratiegegevens in plaatst de *HANABackupCustomerDetails.txt* bestand, moet u controleren of de configuraties voor de HANA-Instantiegegevens correct zijn. Gebruik het script `testHANAConnection.pl`, die onafhankelijk is van de configuratie van een SAP HANA omhoog- of scale-out.

```
testHANAConnection.pl
```

Hebt u de configuratie van een SAP HANA-scale-out, zorg ervoor dat de master HANA-instantie toegang tot de vereiste HANA-servers en -exemplaren heeft. Er zijn geen parameters aan het testscript, maar moet u uw gegevens in toevoegen de *HANABackupCustomerDetails.txt* configuratiebestand voor het script correct kunnen worden uitgevoerd. Alleen de foutcodes voor shell-opdracht worden geretourneerd, dus het is niet mogelijk voor fout met het script voor elk exemplaar. Het script biedt, zodat enkele handige opmerkingen voor u om te controleren.

Voer het script wordt uitgevoerd, de volgende opdracht:
```
 ./testHANAConnection.pl
```
Als het script heeft de status van de HANA-instantie is verkregen, wordt er een bericht weergegeven dat de HANA-verbinding geslaagd is.


De volgende stap is om te controleren of de verbinding met de opslag op basis van de gegevens die u plaatst in het *HANABackupCustomerDetails.txt* configuratie van het bestand en vervolgens een momentopname van een test wordt uitgevoerd. Voordat u uitvoeren de `azure_hana_backup.pl` script, moet u deze test uitvoeren. Als een volume geen momentopnamen bevat, is het niet mogelijk om te bepalen of het volume is leeg of als er een SSH-fout voor de details van de momentopname. Daarom is het script wordt uitgevoerd twee stappen:

- Er wordt gecontroleerd dat de opslag virtuele machine en de interfaces van de tenant toegankelijk is voor de scripts voor het uitvoeren van momentopnamen zijn.
- Een momentopname van een test- of dummy, voor elk volume wordt gemaakt door HANA-exemplaar.

De HANA-instantie is om deze reden, opgenomen als een argument. Als de uitvoering is mislukt, is het niet mogelijk voor fout bij het controleren van de opslagverbinding. Zelfs als er geen fout bij het controleren, biedt het script nuttige tips.

1. De volgorde van de opdrachten voor het uitvoeren van deze test uitvoeren:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   Zowel de naam van de gebruiker en het IP-adres voor opslag zijn aan u zijn verleend aan de overdracht van de eenheid HANA grote instantie.

1. Voer het testscript:
   ```
    ./testStorageSnapshotConnection.pl
   ```

Het script probeert te melden bij de opslag met behulp van de openbare sleutel die is opgegeven in de vorige instellingsstappen en met de gegevens die zijn geconfigureerd in de *HANABackupCustomerDetails.txt* bestand. Als de aanmelding is geslaagd, ziet u de volgende inhoud:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Als er problemen optreden in de verbinding te maken met de opslag-console, ziet de uitvoer er als volgt uit:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Na een geslaagde aanmelding bij de interfaces van de virtuele machine opslag, het script wordt voortgezet met fase 2 en maakt u een momentopname van een test. De uitvoer wordt hier weergegeven voor een configuratie met drie knooppunten scale-out van SAP HANA:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Als de momentopname van de test is met het script is uitgevoerd, kunt u doorgaan met het configureren van de werkelijke opslag-momentopnamen. Als dit niet geslaagd is, moet u de problemen onderzoeken voordat u verder gaat. De momentopname van de test moet rond blijven totdat de eerste echte momentopnamen klaar bent.


### <a name="step-7-perform-snapshots"></a>Stap 7: Voer momentopnamen

Wanneer de voorbereidende stappen zijn voltooid, kunt u gaan configureren van de configuratie van de werkelijke opslag voor momentopnamen. Het script dat moet worden gepland werkt met configuraties voor het omhoog schalen en uitschalen van SAP HANA. Voor de uitvoering van periodieke en reguliere van het back-upscript, plannen dat het script met behulp van de cron-hulpprogramma. 

U kunt drie soorten momentopnameback-ups maken:
- **HANA**: een momentopname van de gecombineerde back-up waarin de volumes die/hana/gegevens bevatten en hana/gedeelde (die ook /usr/sap bevat) wordt gedekt door de gecoördineerde momentopname. Een enkel bestand terugzetten is vanuit deze momentopname mogelijk.
- **Logboeken**: een momentopname van back-up van het volume/hana/logbackups. Er is geen HANA-momentopname wordt voor het uitvoeren van deze momentopname opslag geactiveerd. Deze opslagvolume is bedoeld om de SAP HANA transactielogboekback-ups bevatten. Dit zijn vaker uitgevoerd om te beperken van de groei van de logboekbestanden en mogelijk gegevensverlies te voorkomen dat. Een enkel bestand terugzetten is vanuit deze momentopname mogelijk. Geen Verlaag de frequentie in op minder dan 3 minuten.
- **Opstarten**: een momentopname van het volume dat het opstarten logische eenheidnummer (LUN) van de HANA grote instantie bevat. Deze momentopname back-up is mogelijk alleen met het Type I-SKU's van HANA grote instanties. U kunt niet uitvoeren enkel bestand herstelt vanuit de momentopname van het volume met het bestand Boot.ini LUN.


>[!NOTE]
> De syntaxis van de aanroep voor deze drie typen van momentopnamen veranderd bij de overgang naar de versie 3.x scripts, die ondersteuning bieden voor MCOD implementaties. Er is niet nodig om op te geven de HANA-SID van een exemplaar niet meer. U moet ervoor zorgen dat de SAP HANA-instanties van een eenheid zijn geconfigureerd in het configuratiebestand *HANABackupCustomerDetails.txt*.

>[!NOTE]
> Als u het script voor het eerst uitvoert, kan het enkele onverwachte fouten weergeven op het milieu multi-SID. Opnieuw uit te voeren in het script wordt het probleem opgelost.



De syntaxis van de nieuwe aanroep voor het uitvoeren van opslagmomentopnamen met het script *azure_hana_backup.pl* ziet er als volgt:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

De details van de parameters zijn als volgt: 

- De eerste parameter geeft het type van de momentopname van back-up. De toegestane waarden zijn **hana**, **logboeken**, en **boot**. 
- De parameter **<HANA Large Instance Type>** nodig is voor opstarten volume back-ups alleen. Er zijn twee geldige waarden met "TypeI" of "TypeII" afhankelijk van de eenheid van HANA grote instantie. Om erachter te komen wat typt u de eenheid is, raadpleegt u [SAP HANA (grote instanties) overzicht en architectuur op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- De parameter **< snapshot_prefix >** is een momentopname of een back-label voor het type van de momentopname. Het heeft twee doeleinden: Eén meter is voor u het een naam geven zodat u wat deze momentopnamen zijn weet over. Het tweede doel is van het script *azure\_hana\_backup.pl* om te bepalen het aantal opslagmomentopnamen van die onder dat specifieke label worden bewaard. Als u twee opslagmomentopnamen van hetzelfde type plannen (zoals **hana**), met twee verschillende labels, en definieer dat 30 momentopnamen moeten worden opgeslagen voor elk, krijgt u uiteindelijk met 60 opslagmomentopnamen van de volumes die worden beïnvloed. Alleen alfanumerieke ("A-Z, a-z, 0-9'), onderstrepingsteken ('_') en dash ('-') tekens zijn toegestaan. 
- De parameter **< snapshot_frequency >** is gereserveerd voor toekomstige ontwikkelingen en heeft geen invloed. Stel deze in op "3 minuten" bij het uitvoeren van back-ups van het type **log**, en "15 min" bij het uitvoeren van de back-uptypen.
- De parameter **<number of snapshots retained>** definieert de retentie van de momentopnamen indirect met het definiëren van het aantal momentopnamen met hetzelfde momentopname voorvoegsel (label). Deze parameter is belangrijk voor geplande uitvoeringen via cron. Als het aantal momentopnamen met de dezelfde snapshot_prefix het aantal door deze parameter is opgegeven overschrijdt, wordt de oudste momentopname verwijderd alvorens een momentopname van een nieuwe opslag.

In het geval van een scale-out wordt het script extra controles om ervoor te zorgen dat u toegang hebt tot alle HANA-servers. Het script wordt ook gecontroleerd dat alle HANA-exemplaren de juiste status van de exemplaren geretourneerd voordat een SAP HANA-momentopname wordt gemaakt. De SAP HANA-momentopname wordt gevolgd door een momentopname van de opslag.

De uitvoering van het script `azure_hana_backup.pl` maakt de opslag momentopname in de volgende drie fasen:

1. Een momentopname van een SAP HANA wordt uitgevoerd
1. Een momentopname van de opslag wordt uitgevoerd
1. Hiermee verwijdert u de SAP HANA-momentopname die is gemaakt voordat de uitvoering van de storage-momentopname

Voor het uitvoeren van het script, kunt u het aanroepen van de HDB uitvoerbare map waarnaar deze is gekopieerd. 

De bewaarperiode wordt beheerd met het aantal momentopnamen die zijn verzonden als een parameter als u het script uitvoeren. De hoeveelheid tijd die wordt gedekt door de storage-momentopnamen is een functie van de termijn van de uitvoering van het aantal momentopnamen die zijn verzonden als een parameter bij het uitvoeren van het script. Als het aantal momentopnamen worden bewaard groter is dan het getal dat zijn met de naam als een parameter in de aanroep van het script, wordt de oudste momentopname van de opslag van hetzelfde label wordt verwijderd voordat een nieuwe momentopname wordt uitgevoerd. Het getal geeft u de laatste parameter van de aanroep is het aantal u kunt gebruiken voor het beheren van het aantal momentopnamen worden bewaard. Met dit nummer, kunt u ook bepalen, indirect, de schijfruimte die wordt gebruikt voor momentopnamen. 

> [!NOTE]
>Als u het label wijzigt, wordt de telling opnieuw gestart. U moet strikte in labels, zodat de momentopnamen niet per ongeluk worden verwijderd.

### <a name="snapshot-strategies"></a>Strategieën voor momentopname
De frequentie van momentopnamen voor de verschillende typen, is afhankelijk van of u de functie voor herstel na noodgevallen HANA grote instantie gebruiken. Deze functionaliteit is afhankelijk van opslagmomentopnamen, die mogelijk speciale aanbevelingen voor de perioden frequentie en de uitvoering van de storage-momentopnamen. 

In de overwegingen en aanbevelingen die volgen, wordt de veronderstelling dat u doet *niet* de disaster recovery functionaliteit gebruiken die HANA grote instanties biedt. In plaats daarvan kunt u de storage-momentopnamen zijn back-ups en kunnen voor point-in-time-herstel voor de afgelopen 30 dagen. Gezien de beperkingen van het aantal momentopnamen en ruimte, klanten zijn aangemerkt als de volgende vereisten:

- De hersteltijd voor point-in-time-herstel.
- De ruimte die wordt gebruikt.
- Het herstelpunt en dat de beoogde hersteltijden voor mogelijke herstel na een noodgeval.
- De uiteindelijke uitvoering van HANA full-back-ups op basis van schijven. Wanneer een volledige-databaseback-up op basis van schijven of de **backint** interface wordt uitgevoerd, mislukt de uitvoering van de storage-momentopnamen. Als u van plan bent voor het uitvoeren van volledige-databaseback-ups op storage-momentopnamen, zorg ervoor dat de uitvoering van de storage-momentopnamen gedurende deze tijd is uitgeschakeld.
- Het aantal momentopnamen per volume (beperkt tot 250).


De momentopname-periode is voor klanten die geen gebruik van het herstel na noodgevallen van HANA grote instanties, minder frequent optreden. In dergelijke gevallen klanten voeren de gecombineerde momentopnamen op /hana/data en /hana/shared (inclusief /usr/sap) in de periode van een 12-uurs of 24-uurs en ze houden de momentopnamen voor een maand. Hetzelfde geldt voor de momentopnamen van de back-up van het logboekvolume. Echter, het uitvoeren van SAP HANA transactielogboekback-ups op basis van de back-up van het logboekvolume treedt op 5 minuten voor perioden van 15 minuten.

Geplande storage-momentopnamen worden het beste uitgevoerd met behulp van de cron. Gebruikt hetzelfde script, voor alle back-ups en behoeften voor herstel na noodgevallen, en dat u de invoer script zodat deze overeenkomt met de verschillende back-uptijden aangevraagd. Deze momentopnamen worden alle geplande anders in cron, afhankelijk van de uitvoeringstijd: per uur, 12 uur, dagelijks of wekelijks. 

Hier volgt een voorbeeld van een cron-planning in/etc/crontab:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
In het vorige voorbeeld is een momentopname van een uurtarief gecombineerde die betrekking heeft op de volumes die de gegevens/hana/bevatten en /hana/shared (inclusief/usr/sap) locaties. Gebruik dit type momentopname voor een snellere point-in-time-herstel in de afgelopen twee dagen. Er is bovendien een momentopname van een dagelijks op deze volumes. Dus, hebt u twee dagen dekking door momentopnamen van per uur, plus vier weken dekking door dagelijkse momentopnamen. De transactie logboek back-upvolume is bovendien back-ups dagelijks. Deze back-ups worden gehouden en vier weken. Zoals u in de derde regel van crontab ziet, worden de back-up van het transactielogboek HANA is gepland om uit te voeren om de 5 minuten. De uitvoertijd van de verschillende cron-taken die uitvoeren van de storage-momentopnamen worden gespreid, zodat deze momentopnamen worden niet uitgevoerd in één keer op een bepaald punt in tijd. 

In het volgende voorbeeld voert u een momentopname van een gecombineerde die betrekking heeft op de volumes waarop de hana-/ data/hana/gedeelde (inclusief/usr/sap) locaties en op uurbasis. U bewaard deze momentopnamen twee dagen. De momentopnamen van de back-up logboekvolumes van transactie worden uitgevoerd op basis van 5 minuten en gedurende 4 uur worden bewaard. Als is vóór de back-up van het logboekbestand van de HANA-transactie gepland om uit te voeren om de 5 minuten. De momentopname van de back-up logboekvolume van de transactie wordt uitgevoerd met een vertraging van 2 minuten nadat de back-up van de transactie is gestart. Binnen deze twee minuten duurt de SAP HANA-transactielogboek onder normale omstandigheden. Als voordat, het volume met het bestand Boot.ini LUN eenmaal per dag wordt ondersteund door een momentopname van de opslag en vier weken wordt bewaard.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

De volgende afbeelding ziet u de volgorde van het vorige voorbeeld, met uitzondering van het bestand Boot.ini LUN:

![Relatie tussen de back-ups en momentopnamen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA voert regelmatig schrijfbewerkingen ten opzichte van het volume /hana/log vastleggen van de doorgevoerde wijzigingen met de database. Op regelmatige basis schrijft SAP HANA een opslagpunt naar het volume /hana/data. Zoals opgegeven in crontab, is een SAP HANA transactielogboekback-up om de 5 minuten uitgevoerd. U ziet ook dat elk uur als gevolg van een momentopname van een gecombineerde storage activeren via de /hana/data en /hana/shared volumes wordt uitgevoerd door een momentopname van een SAP HANA. Nadat de HANA-momentopname is voltooid, wordt de momentopname van het gecombineerde opslag wordt uitgevoerd. Volgens de instructies in crontab, wordt de momentopname van de opslag op het volume /hana/logbackup elke 5 minuten, ongeveer twee minuten na de HANA-transactie logboekback-up uitgevoerd.

> 

>[!IMPORTANT]
> Het gebruik van storage-momentopnamen voor back-ups van SAP HANA is nuttig wanneer de momentopnamen worden uitgevoerd in combinatie met SAP HANA transactielogboekback-ups. Deze transactielogboekback-ups moeten betrekking hebben op de perioden tussen de storage-momentopnamen. 

Als u een betrokkenheid bij gebruikers van een point-in-time-herstel van 30 dagen hebt ingesteld, moet u naar:

- In uitzonderlijke gevallen, toegang hebben tot een gecombineerde opslag momentopname over /hana/data en /hana/shared die 30 dagen oud is.
- Aaneengesloten transactielogboekback-ups die betrekking hebben op de tijd tussen een van de gecombineerde opslagmomentopnamen hebben. De oudste momentopname van het back-up logboekvolume van transactie moet dus worden 30 dagen oud. Dit is niet het geval als u de transactielogboekback-ups naar een andere NFS-share die op Azure-opslag bevindt zich kopiëren. In dat geval kunt u pull-oude transactielogboekback-ups van deze NFS-share.

Als u wilt profiteren van een storage-momentopnamen en de uiteindelijke storage-replicatie van transactielogboekback-ups, moet u de locatie op waarnaar de SAP HANA de transactielogboekback-ups schrijft te wijzigen. In HANA Studio kunt u deze wijziging. Hoewel SAP HANA maakt automatisch een reservekopie van volledige logboek segmenten, moet u een back-log-interval om te worden deterministisch. Dit is vooral van toepassing wanneer u de optie voor disaster recovery, omdat u meestal wilt logboekback-ups met een deterministische punt uitvoeren. In het volgende geval zijn 15 minuten ingesteld als de back-upinterval logboek.

![Back-logboeken van de SAP HANA in SAP HANA Studio plannen](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

U kunt ook back-ups die vaker dan om de 15 minuten. Een vaker instelling wordt vaak gebruikt in combinatie met herstel na noodgevallen van HANA grote instanties. Sommige klanten voeren transactielogboekback-ups om de 5 minuten.  

Als de database heeft nooit back-ups, wordt de laatste stap is om uit te voeren van een bestand op basis van databaseback-up voor het maken van een eenmalige back-vermelding die moet zijn opgeslagen in de back-catalogus. SAP HANA kan anders uw opgegeven logboekback-ups niet starten.

![Een op basis van bestanden back-up maken van een post voor één back-up maken](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nadat de eerste geslaagde storage-momentopnamen zijn uitgevoerd, kunt u de test-momentopname die is uitgevoerd in stap 6 verwijderen. Voer het script uit om dit te doen, `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl
```

Hier volgt een voorbeeld van de uitvoer van het script:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Het aantal en de grootte van momentopnamen op het schijfvolume controleren

U kunt het aantal momentopnamen en het opslagverbruik van deze momentopnamen kunt controleren op een specifieke opslagvolume. De `ls` opdracht niet wordt weergegeven de momentopname-map of bestanden. Echter, de opdracht Linux-besturingssysteem `du` bevat gedetailleerde informatie over de storage-momentopnamen, omdat ze zijn opgeslagen op de dezelfde volumes. De opdracht kan worden gebruikt met de volgende opties:

- `du –sh .snapshot`: Deze optie biedt een totaal van alle momentopnamen in de map momentopname.
- `du –sh --max-depth=1`: Deze optie geeft een lijst van alle momentopnamen die zijn opgeslagen in de **.snapshot** map en de grootte van elke momentopname.
- `du –hc`: Met deze optie kunt u de totale grootte die worden gebruikt door alle momentopnamen.

Deze opdrachten gebruiken om ervoor te zorgen dat de opslag op de volumes niet door de momentopnamen die zijn uitgevoerd en die zijn opgeslagen worden verbruikt.

>[!NOTE]
>De momentopnamen van de opstartinstallatiekopie LUN zijn niet zichtbaar zijn met de voorgaande opdrachten.

### <a name="getting-details-of-snapshots"></a>Details van momentopnamen ophalen
Voor meer informatie over momentopnamen, kunt u ook het script gebruiken `azure_hana_snapshot_details.pl`. Met dit script kan worden uitgevoerd in één van beide locaties als er een actieve server in de locatie voor noodherstel. Het script bevat de volgende uitvoer, onderverdeeld op basis van elk volume dat momentopnamen bevat: 
   * De grootte van het totale aantal momentopnamen in een volume
   * De volgende gegevens in elke momentopname in volume: 
      - De naam van de momentopname 
      - Aanmaaktijd 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan deze momentopname, indien van toepassing

Hier volgt een voorbeeld van de syntaxis van de uitvoering van script:

```
./azure_hana_snapshot_details.pl 
```

Omdat het script probeert op te halen van de back-HANA-ID, moet verbinding maken met de SAP HANA-exemplaar. Het configuratiebestand is vereist voor deze verbinding *HANABackupCustomerDetails.txt* correct zijn ingesteld. Een uitvoer van de twee momentopnamen op een volume ziet er als volgt uit:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Terugzetten vanuit een momentopname van de opslag op bestandsniveau
Voor de momentopname-typen **hana** en **logboeken**, u hebt toegang tot de momentopnamen rechtstreeks op de volumes in de **.snapshot** directory. Er is een submap voor elk van de momentopnamen. U kunt elk bestand kopiëren in de staat waarin deze op het moment van de momentopname van de submap in de werkelijke mapstructuur. In de huidige versie van het script, is er **Nee** script dat is opgegeven voor het terugzetten van momentopnamen als een selfservice (Hoewel terugzetten van momentopnamen kan worden uitgevoerd als onderdeel van de selfservice-DR-scripts op de DR-site tijdens de failover) herstellen. Neem contact op met het operationele team van Microsoft door het openen van een serviceaanvraag voor het herstellen van een momentopname van een gewenste van de bestaande momentopnamen beschikbaar.

>[!NOTE]
>Enkel bestand werkt terugzetten niet voor momentopnamen van de opstartinstallatiekopie LUN onafhankelijk van het type van de eenheden HANA grote instantie. De **.snapshot** directory is niet beschikbaar gemaakt in het bestand Boot.ini LUN. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Het aantal momentopnamen op een server te verminderen

Zoals eerder beschreven, kunt u het nummer van bepaalde labels van de momentopnamen die u opslaat verminderen. De laatste twee parameters van de opdracht voor het initiëren van een momentopname zijn het label en het aantal momentopnamen die u wilt bewaren.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

In het vorige voorbeeld, een label voor de momentopname is **dailyhana** en is het aantal momentopnamen met dit label moet worden bewaard **28**. Als u op schijfruimteverbruik reageren, is het raadzaam om het aantal opgeslagen momentopnamen te verminderen. De eenvoudige manier om het aantal momentopnamen beperken tot en met 15, bijvoorbeeld, is het script uitvoeren met de laatste parameter zijn ingesteld op **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Als u het script met deze instelling uitvoert, is het aantal momentopnamen, met inbegrip van de momentopname van de nieuwe opslag, 15. De meest recente 15-momentopnamen worden bewaard, en de 15 oudere momentopnamen worden verwijderd.

 >[!NOTE]
 > Met dit script vermindert het aantal momentopnamen alleen als er meer dan 1 uur oud momentopnamen. Het script worden de momentopnamen die minder dan 1 uur oud zijn niet verwijderd. Deze beperkingen zijn gerelateerd aan de optionele herstel na noodgevallen aangeboden.

Als u niet meer wilt onderhouden van een set met momentopnamen met de back-label **hanadaily** in de syntaxisvoorbeelden kunt u het script met uitvoeren **0** als het getal dat bewaren. Alle momentopnamen die overeenkomt met dat label worden vervolgens verwijderd. Verwijderen van alle momentopnamen kan echter de mogelijkheden van HANA grote instanties herstel na noodgevallen beïnvloeden.

Een tweede optie specifieke momentopnamen verwijderen is het gebruik van het script `azure_hana_snapshot_delete.pl`. Met dit script is ontworpen voor het verwijderen van een momentopname of een set van momentopnamen, hetzij met behulp van de back-HANA-ID zoals gevonden in HANA Studio, of via de naam van de momentopname. Op dit moment alleen de back-up-ID is gekoppeld aan de momentopnamen die zijn gemaakt voor de **hana** type momentopname. Momentopname maken van back-ups van het type **logboeken** en **boot** geen momentopname van een SAP HANA wordt uitgevoerd en er bevindt zich dus er is geen back-ID voor deze momentopnamen worden gevonden. Als de naam van de momentopname wordt opgegeven, zoekt alle momentopnamen op de verschillende volumes die overeenkomen met de naam van de opgegeven momentopname. 

Het script u de SID van de HANA-exemplaar opgeven moet met behulp van de syntaxis van de aanroep van het script aanroepen:

```
./azure_hana_snapshot_delete.pl <SID>

```

Voer het script als gebruiker **hoofdmap**.

Als u een momentopname van een selecteert, kunt u elke momentopname afzonderlijk verwijderen. U het volume met de momentopname voor het eerst opgeven en geef vervolgens de naam van de momentopname. Als de momentopname in het volume bestaat en meer dan 1 uur oud is, wordt deze verwijderd. U kunt de volumenamen van en momentopname vinden door te voeren de `azure_hana_snapshot_details` script. 

>[!IMPORTANT]
>Als er gegevens die alleen op de momentopname bestaat die u verwijdert, nadat de momentopname is verwijderd, dat gegevens verloren is permanent.

   

### <a name="recover-to-the-most-recent-hana-snapshot"></a>Herstellen naar de meest recente HANA-momentopname

In een scenario voor productie-omlaag, kan het proces van het herstellen van een momentopname van de opslag worden gestart als een klant-incident met ondersteuning voor Microsoft Azure. Het is een paar met hoge urgentie als gegevens in een productiesysteem is verwijderd, en de enige manier om op te halen om de productiedatabase te herstellen.

In een andere situatie een point-in-time-herstel mogelijk met lage urgentie en geplande dagen van tevoren. U kunt dit herstel met SAP HANA op Azure Service Management plannen in plaats van de markering van een hoge prioriteit verhogen. U kan bijvoorbeeld worden plan te upgraden van de SAP-software door het toepassen van een nieuwe uitbreiding-pakket. Vervolgens moet u terugkeren naar een momentopname die de status voor de uitbreiding pakketupgrade vertegenwoordigt.

Voordat u de aanvraag verzendt, moet u om voor te bereiden. De SAP HANA op Azure Service Management-team kan vervolgens de aanvragen worden verwerkt en de herstelde volumes bevatten. Daarna herstelt u de HANA-database op basis van de momentopnamen. 

Het volgende laat zien hoe u om voor te bereiden voor de aanvraag:

>[!NOTE]
>Uw gebruikersinterface kan afwijken van de volgende schermafbeeldingen, afhankelijk van de SAP HANA-versie die u gebruikt.

1. Bepaal welke momentopname om terug te zetten. Alleen de gegevens van hana/volume is teruggezet, tenzij u anders opdracht geven. 

1. Sluit de HANA-instantie.

 ![De HANA-exemplaar afsluiten](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database. Als de gegevensvolumes zijn nog steeds gekoppeld aan het besturingssysteem, de herstelbewerking van de momentopname is mislukt.
 ![Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Een Azure-ondersteuningsaanvraag openen en instructies over het terugzetten van een momentopname van een specifieke bevatten.

 - Tijdens de herstelbewerking: SAP HANA op Azure Service Management kan u vragen om Neem deel aan een telefonische vergadering om ervoor te zorgen coördinatie, verificatie en bevestiging dat de juiste opslag-momentopname is teruggezet. 

 - Na het herstel: SAP HANA op Azure Service Management wordt u gewaarschuwd wanneer de storage-momentopname is teruggezet.

1. Nadat het herstelproces voltooid is, koppelen de gegevensvolumes.

 ![Alle gegevensvolumes koppelen](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

1. Selecteer de opties voor herstel van SAP HANA Studio, als ze niet automatisch afkomstig zijn van wanneer u opnieuw verbinding met HANA DB via SAP HANA Studio maken. Het volgende voorbeeld ziet een herstelbewerking op de laatste HANA-momentopname. Een momentopname van een storage integreert één HANA-momentopname. Als u naar de meest recente momentopname van de opslag, moet de meest recente HANA-momentopname. (Als u naar een momentopname van een oudere opslag herstellen, moet u zoekt de HANA-momentopname op basis van de tijd die de storage-momentopname is gemaakt.)

 ![Selecteer opties voor herstellen in SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

1. Selecteer **herstel de database naar een momentopname van een specifieke gegevens back-up of opslag**.

 ![Het venster hersteltype opgeven](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

1. Selecteer **Geef back-up zonder catalogus**.

 ![Het venster van de locatie van back-up opgeven](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

1. In de **doeltype** in de lijst met **momentopname**.

 ![Geef op de back-up te herstellen venster](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

1. Selecteer **voltooien** om het herstelproces te starten.

 ![Selecteer 'Voltooid' om het herstelproces te starten](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

1. De HANA-database is hersteld en hersteld naar de HANA-momentopname die opgenomen in de storage-momentopname.

 ![HANA-database is hersteld en hersteld naar de HANA-momentopname](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>Herstellen naar de meest recente status

Het volgende proces worden hersteld door de HANA-momentopname die is opgenomen in de storage-momentopname. Vervolgens worden hersteld de transactielogboekback-ups naar de meest recente status van de database voordat u de storage-momentopname is teruggezet.

>[!IMPORTANT]
>Voordat u doorgaat, zorg ervoor dat u een voltooid en een aaneengesloten reeks transactielogboekback-ups. Zonder deze back-ups, kunt u de huidige status van de database niet herstellen.

1. Voltooi de stappen 1-6 in [herstellen naar de meest recente HANA-momentopname](#recovering-to-the-most-recent-hana-snapshot).

1. Selecteer **herstel de database naar de meest recente status**.

 ![Selecteer 'Herstel de database naar de meest recente status'](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

1. Geef de locatie van de meest recente HANA logboekback-ups. De locatie moet bevatten van alle de HANA transactielogboekback-ups van de HANA-momentopname naar de meest recente status.

 ![Geef de locatie van de meest recente HANA logboekback-ups](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

1. Selecteer een back-up als een basis van waaruit de database herstellen. In dit voorbeeld is de HANA-momentopname in de schermafbeelding de HANA-momentopname die is opgenomen in de storage-momentopname. 

 ![Selecteer een back-up als een basis van waaruit de database herstellen](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

1. Schakel de **Delta-back-ups gebruik** selectievakje in als delta's nog niet bestaan tussen de tijd van de HANA-momentopname en de meest recente status.

 ![Schakel het selectievakje 'Gebruik Delta-back-ups' uit als er geen delta's bestaan](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

1. Selecteer op het scherm Samenvatting **voltooien** om te beginnen met de procedure voor herstel.

 ![Klik op 'Voltooid' op het scherm Samenvatting](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Herstellen naar een ander punt in tijd
Als u wilt herstellen naar een punt in tijd tussen de HANA-momentopname (opgenomen in de storage-momentopname) en één die later is dan het herstel van HANA momentopname point-in-time, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat u de transactielogboeken van de HANA-momentopname voor de tijd die u herstellen wilt naar een hebt.
1. Beginnen met de procedure onder [herstellen naar de meest recente status](#recovering-to-the-most-recent-state).
1. In stap 2 van de procedure in de **hersteltype opgeven** venster **herstel de database naar het volgende punt in tijd**, en geeft u op het punt in tijd. 
1. Voltooi stap 3-6.

### <a name="monitor-the-execution-of-snapshots"></a>Monitor voor de uitvoering van momentopnamen

Als u opslagmomentopnamen van HANA grote instanties gebruikt, moet u ook voor het bewaken van de uitvoering van deze momentopnamen. Het script dat wordt uitgevoerd een momentopname van een storage-uitvoer wordt geschreven naar een bestand en slaat deze op dezelfde locatie als de Perl-scripts. Een afzonderlijk bestand is geschreven voor elke momentopname opslag. De uitvoer van elk bestand bevat de verschillende fasen dat de momentopname-script wordt uitgevoerd:

1. Hiermee zoekt u de volumes die u nodig hebt om een momentopname te maken.
1. Zoeken naar de momentopnamen die zijn overgenomen uit deze volumes.
1. Hiermee verwijdert u eventuele bestaande momentopnamen zodat deze overeenkomt met het aantal momentopnamen die u hebt opgegeven.
1. Hiermee maakt u een momentopname van een SAP HANA.
1. Hiermee maakt de storage-momentopname voor de volumes.
1. Hiermee verwijdert u de SAP HANA-momentopname.
1. Wijzigt de naam van de meest recente momentopname **.0**.

Het belangrijkste onderdeel van het script CAB-bestand geïdentificeerd maakt deze deel uit:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
U kunt in dit voorbeeld zien hoe het script registreert voor het maken van de HANA-momentopname. Dit proces wordt in het geval van scale-out wordt gestart op het hoofdknooppunt. Het hoofdknooppunt initieert de synchrone maken van de SAP HANA-momentopnamen op elk van de worker-knooppunten. De momentopname van de opslag wordt vervolgens gemaakt. Na de voltooiing van uitvoering van de storage-momentopnamen, is de HANA-momentopname verwijderd. Het verwijderen van de HANA-momentopname wordt gestart vanuit het hoofdknooppunt.


## <a name="disaster-recovery-principles"></a>Principes van herstel na noodgevallen
HANA grote instanties bieden een herstel na noodgevallen tussen HANA grote instantie stempels in verschillende Azure-regio's. Bijvoorbeeld, als u de eenheden HANA grote instantie in de regio VS West van Azure implementeert, kunt u de eenheden HANA grote instantie in de regio VS-Oost als disaster recovery-eenheden. Zoals eerder vermeld, herstel na noodgevallen niet automatisch geconfigureerd, omdat het vereist dat u betaalt voor een andere HANA grote instantie eenheid in de DR-regio. De installatie van noodhersteladapter werkt voor zowel opschalen als scale-out-instellingen. 

In de scenario's tot nu toe is geïmplementeerd, wordt met klanten de eenheid in de DR-regio gebruiken om uit te voeren niet-productiesystemen die gebruikmaken van een geïnstalleerde HANA-exemplaar. De eenheid HANA grote instantie moet van dezelfde SKU hebben als de SKU die wordt gebruikt voor productiedoeleinden. De volgende afbeelding ziet u wat de schijfconfiguratie tussen de server-eenheid in de-Azure-productieregio en de Dr-regio ziet eruit als:

![Configuratie van de DR-installatie uit oogpunt van schijf](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Zoals u in deze afbeelding overzicht, moet u vervolgens een tweede set schijfvolumes rangschikken. De doel-schijfvolumes zijn even groot is als de productievolumes voor de productie-instantie in de disaster recovery-eenheden. Deze volumes op schijven zijn gekoppeld aan de eenheid van de server HANA grote instantie in de site voor noodherstel. De volgende volumes worden gerepliceerd van de productieregio naar de DR-site:

- / hana/gegevens
- / hana/logbackups 
- /Hana/Shared (inclusief/usr/sap)

Het volume /hana/log wordt niet gerepliceerd omdat het transactielogboek van SAP HANA is niet nodig op de manier waarop het terugzetten van die volumes wordt uitgevoerd. 

De basis van het herstel na noodgevallen aangeboden functionaliteit voor de opslag van replicatie wordt aangeboden door de infrastructuur HANA grote instantie. De functionaliteit die wordt gebruikt op de opslag is niet een constante stream met wijzigingen die worden gerepliceerd op asynchrone wijze als wijzigingen in het opslagvolume optreden. In plaats daarvan is het een mechanisme dat is afhankelijk van het feit dat momentopnamen van deze volumes regelmatig worden gemaakt. De verschillen tussen een momentopname van een al gerepliceerde en een nieuwe momentopname die nog niet is gerepliceerd wordt vervolgens overgedragen naar de site voor noodherstel in target-schijfvolumes.  Deze momentopnamen worden opgeslagen op de volumes en, in het geval van een failover van het herstel na noodgevallen moeten worden teruggezet op deze volumes.  

De eerste overdracht van de volledige gegevens van het volume moet zijn voordat de hoeveelheid gegevens kleiner is dan de verschillen tussen momentopnamen wordt. Als gevolg hiervan, bevatten de volumes in de DR-site om een van de volumemomentopnamen uitgevoerd in de productiesite. Uiteindelijk, kunt u dat DR-systeem om te gaan naar een eerdere status verloren gaan om gegevens te herstellen, zonder het terugdraaien van het productiesysteem.

In het geval van MCOD implementaties met meerdere onafhankelijke SAP HANA-instanties in één eenheid van HANA grote instantie, is het waarschijnlijk dat alle SAP HANA-instanties zijn worden gebruikt voor het ophalen van opslag gerepliceerd naar de DR-zijde.

In gevallen waar u HANA System Replication in uw productiesite gebruiken in plaats van de functionaliteit voor hoge beschikbaarheid en opslag op basis van replicatie voor de DR-site gebruikt, zijn de volumes van zowel de knooppunten van de primaire site naar de DR-instantie worden gerepliceerd. U moet extra opslag (dezelfde grootte vanaf primaire knooppunt) op de DR-site voor replicatie van primaire en secundaire naar de DR kopen. 



>[!NOTE]
>De replicatie van HANA grote instantie opslagfunctionaliteit is spiegelen en storage-momentopnamen te repliceren. Als u geen opslagmomentopnamen uitvoeren, zoals geïntroduceerd in de [back-up en herstel](#backup-and-restore) sectie van dit artikel kan niet worden elk replicatie naar de site voor noodherstel. Uitvoering van de momentopname Storage is een vereiste voor storage-replicatie naar de site voor noodherstel.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>De voorbereiding van de noodherstelscenario
In dit scenario hebt u een productiesysteem op HANA grote instanties in de productie van Azure-regio. Voor de stappen die volgen, gaan we ervan uit dat de SID van dat HANA-systeem "PRD" is en dat u hebt een niet-productie-systeem dat wordt uitgevoerd op HANA grote instanties in de DR-Azure-regio. Voor de laatste, gaan we wordt ervan uitgegaan dat de SID "TST." Deze configuratie wordt weergegeven in de volgende afbeelding:

![Begin van herstel na Noodgeval instellen](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Als de server-exemplaar is niet al besteld met de extra opslagruimte volume is ingesteld, SAP HANA op Azure Service Management wordt de aanvullende set volumes als een doel voor de productie-replica op de HANA grote instantie-eenheid waarvoor u de TST uitvoert HANA-instantie. Voor dit doel moet u de SID van uw HANA-productie-exemplaar opgeven. Nadat de SAP HANA op Azure Service Management wordt bevestigd dat de bijlage van deze volumes, moet u deze volumes aan de HANA grote instantie eenheid koppelen.

![Volgende stap voor herstel na Noodgeval instellen](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

De volgende stap is voor installatie van de tweede instantie van SAP HANA op de eenheid HANA grote instantie in de DR-Azure-regio, waar u de TST HANA-exemplaar uitvoeren. De nieuw geïnstalleerde SAP HANA-instantie moet de dezelfde beveiligings-id hebben. De gebruikers die zijn gemaakt, moet dezelfde gebruikers-id en groep-ID die de productie-instantie is. Als de installatie is voltooid, moet u naar:

- Voer stap 2 van de momentopname voorbereidingen voor de opslagruimte die eerder in dit artikel worden besproken.
- Maak een openbare sleutel voor de DR-eenheid van HANA grote instantie als u dat nog niet nog gedaan. Zie stap 3 van de momentopname voorbereidingen voor de opslagruimte die eerder in dit artikel worden besproken.
- Onderhouden de *HANABackupCustomerDetails.txt* met het nieuwe exemplaar van HANA en testen of verbinding naar de opslag goed werkt.  
- Stop de nieuw geïnstalleerde SAP HANA-exemplaar op de eenheid HANA grote instantie in de DR-Azure-regio.
- Ontkoppel deze volumes PRD en neem contact op met SAP HANA op Azure Service Management. De volumes niet kunnen blijven gekoppeld aan de eenheid omdat ze niet toegankelijk tijdens functioneert als opslag ingesteld als replicatiedoel.  

![Herstel na Noodgevallen instellingsstap voordat het tot stand brengen van replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Het operationele team stelt de replicatierelatie tussen de PRD volumes in de productie-Azure-regio en de volumes prd. in de DR-Azure-regio.

>[!IMPORTANT]
>Het volume /hana/log wordt niet gerepliceerd omdat het is niet nodig de gerepliceerde SAP HANA-database herstellen naar een consistente status in de site voor noodherstel.

Vervolgens instellen of aanpassen van de back-upschema van opslag momentopname te krijgen tot uw RTO en RPO in het geval van noodherstel. Om te beperken het beoogde herstelpunt, stelt u de volgende replicatie-intervallen in de service HANA grote instantie:
- Voor de volumes wordt gedekt door de momentopname van het gecombineerde (type momentopname **hana**), ingesteld om te repliceren om de 15 minuten aan de doelen van het volume gelijk opslag in de site voor noodherstel.
- Voor de back-up logboekvolume van de transactie (type momentopname **logboeken**), ingesteld om te repliceren naar de doelen van het volume gelijk opslag in de site voor noodherstel om de 3 minuten.

Om te beperken het beoogde herstelpunt, instellen van het volgende:
- Uitvoeren een **hana** type storage-momentopname (Zie ' stap 7: momentopnamen uitvoeren ') om de 30 minuten naar één uur.
- Voer SAP HANA transactielogboekback-ups om de 5 minuten.
- Voer een **logboeken** typt u een momentopname van elke 5 tot 15 minuten opslag. Met deze Intervalperiode, moet u een RPO van ongeveer 15-25 minuten bereiken.

Met deze installatie, de volgorde van de transactielogboekback-ups, opslagmomentopnamen en de replicatie van de transactie HANA Meld u back-upgegevens voor volume en hana-/ en /hana/shared (inclusief/usr/sap) kan eruitzien als de gegevens op deze afbeelding:

 ![Relatie tussen een transactie logboek back-upmomentopname en een mirror module op een tijdas](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Voor het bereiken van een nog betere RPO in het geval van de herstel na noodgevallen, kunt u de HANA transactielogboekback-ups van SAP HANA op Azure (grote instanties) kopiëren naar de andere Azure-regio. Als u wilt deze verdere vermindering van het RPO bereiken, moet u de volgende stappen uitvoeren:

1. Meld u net zo vaak mogelijk /hana/logbackups maakt u een back-up van de HANA-transactie.
1. Rsync gebruiken om te kopiëren van de transactielogboekback-ups naar de NFS-share gehoste virtuele machines van Azure. De virtuele machines zijn in virtuele netwerken van Azure in de-Azure-productieregio en in de DR-regio's. U moet beide virtuele netwerken van Azure verbinden met het circuit verbinding maken met de productie-HANA grote instanties van Azure. Zie de afbeeldingen in de [netwerk overwegingen voor herstel na noodgeval met HANA grote instanties](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) sectie. 
1. Houd de transactielogboekback-ups in de regio in de virtuele machine die is gekoppeld aan de NFS geëxporteerd opslag.
1. In geval failover na noodgevallen vormen een aanvulling op de transactielogboekback-ups die u vinden op het volume /hana/logbackups met meer recent gebruikte transactielogboekback-ups op de NFS delen in de site voor noodherstel. 
1. Start een transactielogboekback-up te herstellen naar de meest recente back-up die via kan worden opgeslagen in de DR-regio.

Wanneer HANA grote instantie operations Bevestig de instellingen van de relatie replicatie en u de uitvoering van opslagmomentopnamen start, begint de replicatie van gegevens.

![Herstel na Noodgevallen instellingsstap voordat het tot stand brengen van replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Als de replicatie vordert, de momentopnamen op de volumes prd. in de DR-Azure-regio's niet teruggezet worden. Ze worden alleen opgeslagen. Als de volumes zijn gekoppeld in een status heeft, geven ze de status waarin u de volumes ontkoppeld nadat het PRD SAP HANA-exemplaar is geïnstalleerd in de eenheid van de server in de DR-Azure-regio. Ze geven ook de opslag back-ups die nog niet zijn hersteld.

In het geval van een failover, kunt ook u om een momentopname van een oudere opslag in plaats van de meest recente momentopname van de opslag te herstellen.

## <a name="disaster-recovery-failover-procedure"></a>Failover van de noodherstelprocedure
Er zijn twee mogelijke situaties moet overwegen bij het uitvoeren van een failover naar de DR-site:

- U moet de SAP HANA-database naar de meest recente status van gegevens. In dit geval is er een selfservice-script waarmee u de failover zonder de noodzaak contactgegevens van Microsoft kunt uitvoeren. Voor de failback moet u echter werken met Microsoft.
- U wilt herstellen naar een momentopname van een opslag die niet de meest recente gerepliceerde momentopname. In dit geval moet u werken met Microsoft. 

>[!NOTE]
>De volgende stappen moeten worden uitgevoerd op de eenheid HANA grote instantie, die staat voor de DR-eenheid. 
 
Als u wilt herstellen naar de meest recente momentopnamen met gerepliceerde opslag, moet u de volgende stappen uitvoeren: 

1. Sluit de niet-productie-exemplaar van HANA op de eenheid van het herstel na noodgevallen van HANA grote instanties die u uitvoert. Dit is omdat een niet-actieve productie-exemplaar voor HANA is vooraf geïnstalleerd is.
1. Zorg ervoor dat er geen SAP HANA-processen worden uitgevoerd. Gebruik de volgende opdracht voor deze controle: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. U moet worden weergegeven door de uitvoer de **hdbdaemon** verwerken in een status ' gestopt ' en er zijn geen andere HANA-processen in een status die wordt uitgevoerd of gestart.
1. Voer het script uit op de DR-site HANA grote instantie-eenheid *azure_hana_dr_failover.pl*. Het script vraagt om een SID SAP HANA kunnen worden hersteld. Wanneer aangevraagd, typt u in een of de enige SAP HANA SID die is gerepliceerd en wordt bijgehouden in de *HANABackupCustomerDetails.txt* bestand op de eenheid HANA grote instantie in de DR-site. 

      Als u wilt dat meerdere SAP HANA-instanties failover is uitgevoerd, moet u het script meerdere keren uitgevoerd. Wanneer aangevraagd, typt u in de SAP HANA-SID die u wilt een failover en herstel. Het script bevat is voltooid, een lijst met koppelpunten van de volumes die zijn toegevoegd aan de eenheid HANA grote instantie. Deze lijst bevat de herstelde DR-volumes.

1. De herstelde disaster recovery-volumes koppelen met behulp van Linux-besturingssysteem-opdrachten voor de eenheid HANA grote instantie in de site voor noodherstel. 
1. Start de niet-actieve productie-instantie van SAP HANA.
1. Als u wilt het logboek back-up-transactielogboeken kopiëren om de RPO-tijd te beperken, moet u deze transactielogboekback-ups in de directory van de zojuist gekoppelde DR/hana/logbackups samenvoegen. Bestaande back-ups niet worden overschreven. Kopieer de nieuwere back-ups die niet zijn gerepliceerd met de meest recente replicatie van een momentopname van de opslag.
1. U kunt ook enkele bestanden uit de momentopnamen die zijn gerepliceerd naar het volume /hana/shared/PRD in de DR-Azure-regio herstellen. 

U kunt ook de DR-failover testen zonder enige impact op de werkelijke replicatierelatie. Een testfailover uitvoeren, volgt u de voorgaande stappen 1 en 2 en ga vervolgens door met de volgende stap 3.

>[!IMPORTANT]
>Voer *niet* productietransacties worden uitgevoerd op het exemplaar dat u hebt gemaakt in de DR-site door het proces van **testen van een failover** met het script dat is geïntroduceerd in stap 3. Deze opdracht maakt u een set van volumes die geen relatie met de primaire site hebben. Als gevolg hiervan synchronisatie naar de primaire site is *niet* mogelijk. 

Stap 3 voor de failovertest:

Voer het script uit op de DR-site HANA grote instantie-eenheid **azure_hana_test_dr_failover.pl**. Dit script is *niet* stoppen van de replicatierelatie tussen de primaire site en de DR-site. Met dit script wordt in plaats daarvan de DR-opslagvolumes klonen. Nadat het proces voor het klonen is voltooid, worden de gekloonde volumes hersteld naar de status van de meest recente momentopname en vervolgens gekoppeld aan de DR-eenheid. Het script vraagt om een SID SAP HANA kunnen worden hersteld. Type in één of de enige SAP HANA SID die is gerepliceerd en wordt bijgehouden in de *HANABackupCustomerDetails.txt* bestand op de eenheid HANA grote instantie in de DR-site. 

Als u wilt dat meerdere SAP HANA-instanties om te testen, moet u het script meerdere keren uitgevoerd. Wanneer aangevraagd, typt u in de SAP HANA-SID van het exemplaar dat u wilt testen voor failover. Na voltooiing ziet u het script een lijst met koppelpunten van de volumes die zijn toegevoegd aan de eenheid HANA grote instantie. Deze lijst bevat de gekloonde DR-volumes.

Ga verder met stap 4.

   >[!NOTE]
   >Als u een failover wilt uitvoeren naar de DR-site te redden sommige gegevens met uur geleden is verwijderd en moeten de volumes voor herstel na Noodgevallen moet worden ingesteld op een eerdere momentopname, wordt deze procedure geldt. 

1. Sluit de niet-productie-exemplaar van HANA op de eenheid van het herstel na noodgevallen van HANA grote instanties die u uitvoert. Dit is omdat een niet-actieve productie-exemplaar voor HANA is vooraf geïnstalleerd is.
1. Zorg ervoor dat er geen SAP HANA-processen worden uitgevoerd. Gebruik de volgende opdracht voor deze controle: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. U moet worden weergegeven door de uitvoer de **hdbdaemon** verwerken in een status ' gestopt ' en er zijn geen andere HANA-processen in een status die wordt uitgevoerd of gestart.
1. Bepaal waarvan de naam van de momentopname of SAP HANA-back-ID die u wilt dat de site voor noodherstel hersteld. Deze momentopname is in de echte disaster recovery gevallen, meestal de meest recente momentopname. Als u herstellen, verlies van gegevens wilt, kiest u een eerdere momentopname.
1. Contact opnemen met ondersteuning voor Azure via een ondersteuningsaanvraag met hoge prioriteit. Vraag voor het herstellen van die momentopname (met de naam en datum van de momentopname) of de back-HANA-ID op de DR-site. De standaardwaarde is dat de operations-zijde/hana/gegevensvolume alleen herstelt. Als u de hana-/ logbackups-volumes wilt, moet u die specifiek status. *Moet het volume /hana/shared niet herstellen.* In plaats daarvan moet u specifieke bestanden, zoals global.ini van de **.snapshot** map en submappen nadat u / hana koppelen/gedeeld clustervolume voor PRD. 

   Aan de bewerkingen voor de volgende stappen plaats:

   a. De replicatie van de volumes van de herstel na noodgevallen van momentopnamen van de productievolume is gestopt. Deze onderbreking mogelijk al is dit als een storing op de productiesite is de reden moet u de noodherstelprocedure uitvoeren.
   
   b. De opslag een momentopname van de naam of momentopname met de back-ID die u hebt gekozen op de disaster recovery volumes is hersteld.
   
   c. Na het herstel zijn de disaster recovery-volumes beschikbaar moet worden gekoppeld aan de eenheden HANA grote instantie in de Dr-regio.
      
1. Koppel de disaster recovery-volumes in de eenheid HANA grote instantie in de site voor noodherstel. 
1. Start de niet-actieve productie-instantie van SAP HANA.
1. Als u wilt het logboek back-up-transactielogboeken kopiëren om de RPO-tijd te beperken, moet u deze transactielogboekback-ups in de directory van de zojuist gekoppelde DR/hana/logbackups samenvoegen. Bestaande back-ups niet worden overschreven. Kopieer de nieuwere back-ups die niet zijn gerepliceerd met de meest recente replicatie van een momentopname van de opslag.
1. U kunt ook enkele bestanden uit de momentopnamen die zijn gerepliceerd naar het volume /hana/shared/PRD in de DR-Azure-regio herstellen.

De volgende reeks stappen omvat het herstellen van de productie-instantie van SAP HANA op basis van de momentopname van het herstelde opslag en de transactielogboekback-ups die beschikbaar zijn:

1. Wijzigen van de back-uplocatie naar **/hana/logbackups** met behulp van SAP HANA Studio.
   ![Wijzigen van de back-uplocatie voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA via de back-upbestand locaties wordt gescand en stelt de meest recente transactielogboek om naar te herstellen. De scan kan duurt enkele minuten tot een scherm zoals hieronder wordt weergegeven: ![lijst met transactielogboekback-ups voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Enkele van de standaardinstellingen aan te passen:

      - Schakel **gebruik back-ups verschillen**.
      - Selecteer **initialiseren Log gebied**.

   ![Het logboek initialiseren gebied instellen](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Selecteer **Voltooien**.

   ![Het terugzetten van de DR voltooien](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Een venster uitgevoerd, zoals hieronder, moet worden weergegeven. Houd er rekening mee dat het voorbeeld van een herstel na noodgevallen herstel van een configuratie met drie knooppunten scale-out SAP HANA.

![Voortgang van terugzetten](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Als het herstel lijkt te reageren op de **voltooien** scherm en niet weergeven van het scherm wordt uitgevoerd, bevestigen dat de SAP HANA-exemplaren op de worker-knooppunten worden uitgevoerd. Indien nodig, start u de SAP HANA-instanties handmatig in.


### <a name="failback-from-a-dr-to-a-production-site"></a>Failback van een herstel na Noodgevallen naar een productiesite
U kunt een failback van een herstel na Noodgevallen met een productiesite. We bekijken een scenario waarin de failover naar de site voor noodherstel is veroorzaakt door problemen in de productie van Azure-regio, en niet door de noodzaak om verloren gegevens te herstellen. U uitvoert uw SAP-productie-werkbelasting een tijdje in de site voor noodherstel. Als de problemen in de productiesite zijn opgelost, die u wilt een failback uitvoeren naar de productiesite. Omdat u kunt geen gegevens verloren gaan, wordt de stap terug naar de productiesite omvat meerdere stappen en nauwe samenwerking met de SAP HANA op Azure-bewerkingen team. Het is aan u voor het activeren van het operationele team om te beginnen met het synchroniseren terug naar de productiesite nadat de problemen zijn opgelost.

Dit is de volgorde van stappen moeten uitvoeren:

1. De SAP HANA op Azure-bewerkingen team Hiermee haalt u de trigger om te synchroniseren van de productie-opslagvolumes uit de opslagvolumes disaster recovery, die nu de productiestatus van de vertegenwoordigen. De eenheid HANA grote instantie in de productiesite wordt in deze status wordt afgesloten.
1. De SAP HANA op Azure-bewerkingen team controleert de replicatie en zorgt ervoor dat deze wordt bijgewerkt voordat aan u gemeld.
1. U afsluiten de toepassingen die gebruikmaken van de productie HANA-exemplaar in de site voor noodherstel. Vervolgens voert u een transactielogboek van HANA. Vervolgens maakt u de HANA-instantie die wordt uitgevoerd op de eenheden HANA grote instantie in de site voor noodherstel te stoppen.
1. Nadat de HANA-instantie die wordt uitgevoerd in de eenheid HANA grote instantie in de site voor noodherstel wordt afgesloten, synchroniseert het operationele team handmatig de schijfvolumes opnieuw.
1. De SAP HANA op Azure operationele team de eenheid HANA grote instantie in de productiesite opnieuw gestart en geeft die boven aan u. U ervoor zorgen dat de SAP HANA-instantie in een afsluitingsstaat bij het opstarten van de eenheid HANA grote instantie.
1. U kunt stappen voor het terugzetten van dezelfde database uitvoeren zoals u deed toen eerder Failover-overschakeling uitvoeren naar de site voor noodherstel.

### <a name="monitor-disaster-recovery-replication"></a>Disaster recovery-replicatie controleren

U kunt de status van de voortgang van uw storage-replicatie controleren door het uitvoeren van script `azure_hana_replication_status.pl`. Met dit script moet worden uitgevoerd vanaf een eenheid die wordt uitgevoerd in de locatie voor noodherstel werken zoals verwacht. Werking van het script, ongeacht of de replicatie actief is. Het script kan worden uitgevoerd voor elke eenheid HANA grote instantie van uw tenant in de locatie voor noodherstel. Het kan niet worden gebruikt voor het verkrijgen van informatie over het opstartvolume.

Aanroepen van het script met de volgende opdracht:
```
./replication_status.pl <HANA SID>
```

De uitvoer wordt opgedeeld, per volume, in de volgende secties:  

- Status van koppeling
- Huidige replicatieactiviteit
- Meest recente momentopname gerepliceerd 
- Grootte van de meest recente momentopname
- Huidige vertragingstijd tussen momentopnamen (tussen de laatste voltooide snapshot-replicatie en nu)

Status van de koppeling wordt weergegeven als **Active** , tenzij de koppeling tussen locaties niet actief is of er een failover uitgevoerd is. De replicatieactiviteit-adressen of alle gegevens op dit moment wordt gerepliceerd of niet-actief is of dat andere activiteiten zijn nu plaatsvindt op de koppeling. De laatste momentopname gerepliceerd moet alleen worden weergegeven als `snapmirror…`. De grootte van de laatste momentopname wordt vervolgens weergegeven. Ten slotte wordt de vertraging weergegeven. De vertragingstijd geeft de tijd van de geplande replicatie wanneer de replicatie is voltooid. Een vertraging is groter is dan een uur voor replicatie van gegevens, met name in de eerste replicatie, zelfs als replicatie is gestart. De vertragingstijd blijft toenemen totdat de voortdurende replicatie is voltooid.

Hier volgt een voorbeeld van de uitvoer:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












