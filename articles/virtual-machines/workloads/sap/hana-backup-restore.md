---
title: HANA back-up en herstellen op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: HANA back-up en herstellen op SAP HANA op Azure (grote instanties)
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
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c03a7e5763f580bf1e17232a5850064710c8227
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707259"
---
# <a name="backup-and-restore"></a>Back-ups en herstellen

>[!IMPORTANT]
>Deze documentatie is geen vervanging van de documentatie bij het beheer van SAP HANA of SAP-opmerkingen. Er wordt verwacht dat de lezer een goed begrip van en deskundigheid in SAP HANA-beheer en bewerkingen, vooral bij de onderwerpen van back-up, herstel, hoge beschikbaarheid en herstel na noodgevallen is. Schermafbeeldingen van SAP HANA Studio worden in deze documentatie wordt weergegeven. Inhoud, de structuur en de aard van de schermen van SAP-beheerprogramma's en de hulpprogramma's zelf kunnen worden gewijzigd van SAP HANA, release-versie.

Het is belangrijk dat u stappen en processen in uw omgeving en met uw HANA-versies en versies uitoefenen. Bepaalde processen die worden beschreven in deze documentatie voor een beter begrip van de algemene zijn vereenvoudigd en zijn niet bedoeld als gedetailleerde stappen voor uiteindelijke bewerking handboeken worden gebruikt. Als u maken van de bewerking handboeken voor uw configuraties wilt, moet u om te testen en uitoefening van uw processen en documenteren van de processen die betrekking hebben op uw specifieke configuraties. 

Een van de belangrijkste aspecten aan operationele databases is Beveilig ze tegen catastrofale gebeurtenissen. De oorzaak van deze gebeurtenissen kan van alles van natuurrampen gebruiksvriendelijke fouten zijn.

Back-ups van een database met de mogelijkheid om het te herstellen naar een willekeurig punt in tijd (zoals voordat iemand kritieke gegevens verwijderd), kunnen worden hersteld naar een status die zich zo dicht mogelijk in de manier waarop het was voordat het wordt onderbroken.

Twee typen back-ups moeten worden uitgevoerd voor het bereiken van dergelijke een mogelijkheid om terug te zetten:

- Databaseback-ups: Volledige, incrementele of differentiële back-ups
- Transactielogboekback-ups

Naast de volledige-databaseback-ups uitgevoerd op het toepassingsniveau van een, kunt u back-ups met storage-momentopnamen uitvoeren. Storage-momentopnamen vervangen transactielogboekback-ups niet. Transactielogboekback-ups blijven belangrijk de database herstellen naar een bepaald punt in tijd of de logboeken van al opgegeven transacties leeg. Storage-momentopnamen kunnen echter recovery versnellen door snel een afbeelding doorvoeren van de database. 

SAP HANA op Azure (grote instanties) biedt twee opties voor back-up en herstel:

- Doe (DIY). Nadat u ervoor zorgen dat er voldoende schijfruimte beschikbaar is, kunt u de volledige database en logboekbestanden back-ups uitvoeren met behulp van een van de volgende methoden van de schijf-back-up. U kunt back-up ofwel rechtstreeks op volumes die zijn gekoppeld aan de HANA grote instantie-eenheden, of voor Network File Shares (NFS) die zijn ingesteld op een Azure-machine (VM). In dat laatste geval, klanten instellen van een Linux-VM in Azure, Azure Storage koppelen aan de virtuele machine en deel van de opslag via een geconfigureerde NFS-server in die virtuele machine. Als u de back-up op basis van volumes die rechtstreeks aan HANA grote instantie eenheden koppelen uitvoert, moet u de back-ups naar Azure storage-account kopiëren (na het instellen van een Azure-VM die Hiermee exporteert u NFS-shares die zijn gebaseerd op Azure Storage). U kunt ook een Azure Backup-kluis of een Azure koude opslag gebruiken. 

   Een andere optie is het gebruik van een derde partij hulpprogramma voor gegevensbeveiliging voor het opslaan van de back-ups nadat ze zijn gekopieerd naar een Azure storage-account. De back-upoptie voor zelf kan ook zijn die nodig zijn voor gegevens die u nodig hebt om op te slaan voor langere tijd voor nalevings- en controledoeleinden. In alle gevallen moet worden de back-ups gekopieerd naar NFS-shares vertegenwoordigd door een virtuele machine en de Azure Storage.

- Infrastructuur voor back-up en herstellen van functionaliteit. U kunt ook gebruik van de back-up en herstellen van de functionaliteit die de onderliggende infrastructuur van SAP HANA op Azure (grote instanties) biedt. Deze optie wordt voldaan aan de noodzaak van back-ups en snelle herstelbewerkingen. De rest van deze sectie wordt de functionaliteit voor back-up en herstellen die wordt aangeboden met HANA grote instanties. In deze sectie bevat ook informatie over de relatie back-up en herstel heeft tot de disaster recovery-functionaliteit die worden aangeboden door HANA grote instanties.

> [!NOTE]
>   SAP HANA-momentopnamen is afhankelijk van de momentopnametechnologie die wordt gebruikt door de onderliggende infrastructuur van HANA grote instanties. SAP HANA-momentopnamen werken op dit moment niet in combinatie met meerdere tenants van containers voor SAP HANA-multitenant-database. Als er slechts één tenant is geïmplementeerd, SAP HANA-momentopnamen werken en deze methode kan worden gebruikt.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Met behulp van storage-momentopnamen van SAP HANA op Azure (grote instanties)

De opslaginfrastructuur onderliggende SAP HANA op Azure (grote instanties) biedt ondersteuning voor storage-momentopnamen van volumes. Zowel back-up en herstel van volumes wordt ondersteund, met de volgende overwegingen:

- In plaats van een volledige databaseback-ups, worden opslag volume momentopnamen regelmatig.
- Bij het activeren van een momentopname over /hana/data en /hana/shared (inclusief /usr/sap) initieert volumes, de momentopnametechnologie voor van een SAP HANA momentopname voordat deze wordt uitgevoerd met de storage-momentopname. Deze SAP HANA-momentopname is het setup-punt voor het uiteindelijke log herstelbewerkingen kunnen na het herstel van de momentopname van de opslag. Voor HANA-momentopname om succesvol te zijn, moet u een actief exemplaar van HANA.  Storage-momentopname wordt in HSR scenario wordt niet ondersteund op huidige secundaire knooppunt waar de HANA-momentopname kan niet worden uitgevoerd.
- Nadat de momentopname van de opslag is uitgevoerd, wordt de SAP HANA-momentopname verwijderd.
- Transactielogboekback-ups regelmatig worden genomen en worden opgeslagen in het volume /hana/logbackups of in Azure. U kunt het /hana/logbackups volume met de transactielogboekback-ups als u wilt maken van een momentopname van een afzonderlijk activeren. In dat geval hoeft u niet voor het uitvoeren van een momentopname van een HANA.
- Als u een database naar een bepaald punt in tijd herstellen moet, aanvragen die Microsoft Azure-ondersteuning (voor een storing in de productie) of SAP HANA op Azure herstellen naar een bepaalde opslag-momentopname. Een voorbeeld is een geplande herstel van een sandbox-systeem in de oorspronkelijke staat.
- De SAP HANA-momentopname die opgenomen in de storage-momentopname is een offset punt voor het toepassen van transactielogboekback-ups die zijn uitgevoerd en opgeslagen nadat de storage-momentopname is gemaakt.
- Deze transactielogboekback-ups gaat naar de database herstellen naar een bepaald punt in tijd.

U kunt storage-momentopnamen die zijn gericht op drie soorten volumes uitvoeren:

- Een momentopname van een gecombineerde/hana/gegevens en /hana/shared (inclusief/usr/sap). Deze momentopname is vereist voor het maken van een momentopname van een SAP HANA als voorbereiding op de momentopname van de opslag. De SAP HANA-momentopname zorgt ervoor dat de database zich in een consistente status van een punt opslag. En dat voor de terugzetbewerking verwerken dat wil zeggen een punt om in te stellen u op.
- Een momentopname van een afzonderlijke via/hana/logbackups.
- De partitie van een besturingssysteem.

Lees het laatste momentopname scripts en documentatie van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Wanneer het downloaden van de momentopname-script-pakket van de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), krijgt u drie bestanden waarvan een een PDF-documentatie voor de functionaliteit die is opgegeven is. Zorg ervoor dat u gaan langs de instructies in het hoofdstuk ophalen van de momentopname-hulpprogramma's als de set hulpprogramma's downloaden.

## <a name="storage-snapshot-considerations"></a>Overwegingen met betrekking tot opslag-momentopname

>[!NOTE]
>Storage-momentopnamen gebruiken opslagruimte die is toegewezen aan de eenheden HANA grote instantie. U moet rekening houden met de volgende aspecten van de planning voor storage-momentopnamen en het aantal opslagmomentopnamen te houden. 

De specifieke mechanics van opslagmomentopnamen voor SAP HANA op Azure (grote instanties) zijn onder andere:

- Een momentopname van een specifieke opslag (op het punt in tijd waarop deze is gemaakt) verbruikt weinig opslagruimte.
- Als gegevens inhoud wordt gewijzigd en de inhoud van bestanden op het opslagvolume wijzigen SAP HANA-gegevens, moet de momentopname voor het opslaan van de oorspronkelijke inhoud blokkeren, evenals de gegevens worden gewijzigd.
- Als gevolg hiervan de storage-momentopname wordt vergroot. Hoe langer de momentopname bestaat, hoe groter de storage-momentopname wordt.
- De meer wijzigingen die zijn aangebracht aan het volume van SAP HANA-database gedurende de levensduur van een momentopname opslag, hoe groter het verbruik van de ruimte van de momentopname van de opslag.

SAP HANA op Azure (grote instanties) wordt geleverd met grootten voor de SAP HANA-gegevens en logboekvolumes vast volume. Uitvoeren van momentopnamen van de volumes eats in uw ruimte op volume. U moet bepalen, bij het plannen van de storage-momentopnamen. U moet ook het gebruik van de ruimte van de opslagvolumes bewaken, evenals het aantal momentopnamen die u opslaat beheren. Wanneer u grote hoeveelheden gegevens importeren of andere belangrijke wijzigingen aangebracht in de HANA-database uitvoert, kunt u de storage-momentopnamen uitschakelen. 


De volgende secties bevatten informatie voor het uitvoeren van deze momentopnamen, met inbegrip van algemene aanbevelingen:

- Hoewel de hardware 255 momentopnamen per volume tolereren kan, die u wilt blijven goed onder dit getal. De aanbeveling is 250 of minder.
- Voordat u opslagmomentopnamen uitvoert, controleren en bijhouden van vrije ruimte.
- Verlaag het aantal opslagmomentopnamen op basis van de vrije ruimte. U kunt het aantal momentopnamen die u bijhoudt verlagen of kunt u de volumes uitbreiden. U kunt extra opslagruimte bestellen in eenheden van 1 terabyte.
- Tijdens de activiteiten, zoals het verplaatsen van gegevens in SAP HANA met hulpprogramma voor migratie van SAP-platform (R3load) of SAP HANA-databases herstellen vanuit back-ups, opslagmomentopnamen op het volume /hana/data uitschakelen 
- Tijdens de grotere reorganisaties van SAP HANA-tabellen, moeten opslagmomentopnamen worden vermeden, indien mogelijk.
- Storage-momentopnamen zijn vereist om te profiteren van de noodherstelprocessen mogelijkheden voor herstel van SAP HANA op Azure (grote instanties).

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Vereisten voor het gebruik van selfservice opslagmomentopnamen

Om ervoor te zorgen dat de momentopname-script met succes wordt uitgevoerd, zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties. Perl is vooraf geïnstalleerd op uw eenheid HANA grote instantie. Om te controleren of de versie van Perl, gebruikt u de volgende opdracht:

`perl -v`

![De openbare sleutel wordt door het uitvoeren van deze opdracht gekopieerd](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Instellen van opslagmomentopnamen

Als u de storage-momentopnamen met HANA grote instanties instelt, de volgende stappen uit:
1. Zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties.
1. Wijzig de/etc/ssh/ssh\_config om toe te voegen van de regel _MACs hmac-sha1_.
1. Maak een back-SAP HANA-gebruikersaccount op het hoofdknooppunt voor elk SAP HANA-exemplaar dat u uitvoert, indien van toepassing.
1. De SAP HANA HDB-client installeren op alle servers in de SAP HANA grote instanties.
1. Maak een openbare sleutel voor toegang tot de onderliggende opslaginfrastructuur die het maken van momentopnamen bepaalt op de eerste server voor SAP HANA grote instanties van elke regio.
1. Kopieer de scripts en het configuratiebestand van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) naar de locatie van **hdbsql** in de SAP HANA-installatie.
1. Wijzig de *HANABackupDetails.txt* bestand zo nodig voor de betreffende klant-specificaties.

Lees het laatste momentopname scripts en documentatie van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Raadpleeg voor gedetailleerde stappen die hierboven worden vermeld, [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

### <a name="consideration-for-mcod-scenarios"></a>Overweging voor MCOD scenario 's
Als u werkt met een [MCOD scenario](https://launchpad.support.sap.com/#/notes/1681092) met meerdere instanties van SAP HANA op één HANA grote instantie-eenheid, hebt u afzonderlijke opslagvolumes die zijn ingericht voor elk van de SAP HANA-instanties. Voor meer informatie over MDC en andere overwegingen [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) hoofdstuk **'Dingen die belangrijk'**.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Stap 1: De SAP HANA HDB-client installeren

Het Linux-besturingssysteem geïnstalleerd op SAP HANA op Azure (grote instanties) bevat de mappen en scripts die nodig zijn voor het uitvoeren van SAP HANA-opslagmomentopnamen voor hersteldoeleinden voor back-up en herstel na noodgeval. Raadpleeg voor meer recente versies in [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). De meest recente versie van de scripts is 4.0. Verschillende scripts mogelijk verschillende secundaire releases binnen dezelfde primaire versie.

Het is uw verantwoordelijkheid voor het installeren van de SAP HANA HDB-client op de eenheden HANA grote instantie tijdens de installatie van SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Stap 2: Wijzig de/etc/ssh/ssh\_config

Deze stap wordt beschreven in het selectievakje voor meer recente versies in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) in hoofdstuk **'Communicatie met de opslag inschakelen'**


### <a name="step-3-create-a-public-key"></a>Stap 3: Maak een openbare sleutel

Voor toegang tot de interfaces van de momentopname van opslag van uw tenant HANA grote instantie, moet u een procedure aanmelden via een openbare sleutel vast. Maak op de eerste SAP HANA op Azure (grote instanties)-server in uw tenant, een openbare sleutel moet worden gebruikt voor toegang tot de opslaginfrastructuur. De openbare sleutel zorgt ervoor dat een wachtwoord niet vereist is voor aanmelding bij de storage-momentopname-interfaces. Het maken van een openbare sleutel betekent ook dat u niet wilt wachtwoordreferenties onderhouden. De exacte stappen over het genereren van de openbare sleutel wordt beschreven in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) in hoofdstuk **'Communicatie met de opslag inschakelen'**


### <a name="step-4-create-an-sap-hana-user-account"></a>Stap 4: Een SAP HANA-gebruikersaccount maken

Voor het starten van het maken van momentopnamen van SAP HANA, moet u een gebruikersaccount maken in SAP HANA die de opslag momentopname-scripts kunnen gebruiken. Maak een SAP HANA-gebruikersaccount in SAP HANA Studio voor dit doel. De gebruiker moet worden gemaakt onder het SYSTEMDB en niet onder de SID-database voor MDC. De gebruiker wordt in de omgeving enkele container gemaakt in de tenant-database. Dit account moet de volgende bevoegdheden hebben: **Back-up van beheerder** en **catalogus lezen**. Lees voor de exacte stappen voor het instellen van de gebruiker en het gebruik van de gebruiker, [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) in hoofdstuk **'Communicatie met de SAP HANA inschakelen'**


### <a name="step-5-authorize-the-sap-hana-user-account"></a>Stap 5: Toestaan dat de SAP HANA-gebruikersaccount

In deze stap verleent u de SAP HANA-gebruikersaccount dat u hebt gemaakt, zodat de scripts niet hoeft in te dienen wachtwoorden tijdens runtime. De SAP HANA-opdracht `hdbuserstore` wordt het maken van een sleutel van de gebruiker SAP HANA, die is opgeslagen op een of meer knooppunten van SAP HANA. De sleutel van de gebruiker krijgt de gebruiker toegang tot SAP HANA zonder het beheren van wachtwoorden op in het proces van scripts. Het uitvoeren van scripts proces wordt verderop in dit artikel besproken.

>[!IMPORTANT]
>Deze configuratieopdrachten uitvoeren met dezelfde context voor de gebruiker die de momentopnameopdrachten worden uitgevoerd in. Anders wordt kunnen niet de momentopnameopdrachten correct werken.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Stap 6: Ophalen van de momentopname-scripts, het configureren van de momentopnamen en het testen van de configuratie en connectiviteit

Download de meest recente versie van de scripts van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). De manier waarop die de scripts gaat worden geïnstalleerd is met versie 4.0 van de scripts van majorly gewijzigd. Lees voor de exacte details [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) in hoofdstuk **'Communicatie met de SAP HANA inschakelen'**

Lees voor de exacte volgorde van opdrachten, hoofdstuk **'Eenvoudige installatie van de momentopname-hulpprogramma's (standaard)'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). U wordt aangeraden het gebruik van de standaardinstallatie. Als u wilt een upgrade van versie 3.x 4.0, controleert u de sectie **upgraden van een bestaande installatie** van [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Volg de instructies in voor het verwijderen van de hulpprogrammaset 4.0 **'Verwijderen van de momentopname-hulpprogramma's '** in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Vergeet niet om uit te voeren van de stappen in **'Volledige installatie van hulpprogramma's voor snapshot'** van [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Het doel van de andere scripts en -bestanden omdat deze is geïnstalleerd wordt weergegeven en is uiteengezet in **'Wat zijn hulpprogramma's voor deze momentopname?'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Voordat u de momentopname-hulpprogramma's configureert, zorg ervoor dat u ook back-HANA-locaties en instellingen correct geconfigureerd zoals is beschreven in **'SAP HANA Configuration'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

De configuratie van de momentopname-toolset is beschreven in **configuratiebestand - HANABackupCustomerDetails.txt** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="testing-connectivity-with-sap-hana"></a>Testen van de connectiviteit met SAP HANA

Nadat u de configuratiegegevens in plaatst de *HANABackupCustomerDetails.txt* bestand, moet u controleren of de configuraties voor de HANA-Instantiegegevens correct zijn. Gebruik het script `testHANAConnection`, die onafhankelijk is van de configuratie van een SAP HANA omhoog- of scale-out.

Zie voor meer informatie **'Controleer de verbinding met SAP HANA - testHANAConnection'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

#### <a name="testing-storage-connectivity"></a>Opslagconnectiviteit testen

De volgende stap is om te controleren of de verbinding met de opslag op basis van de gegevens die u plaatst in het *HANABackupCustomerDetails.txt* configuratie van het bestand en vervolgens een momentopname van een test wordt uitgevoerd. Voordat u uitvoeren de `azure_hana_backup` opdracht, moet u deze test uitvoeren. De volgorde van de opdrachten voor deze test wordt vermeld in **'Controleer de verbinding met de opslag - testStorageSnapshotConnection'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Na een geslaagde aanmelding bij de interfaces van de virtuele machine opslag, het script wordt voortgezet met fase 2 en maakt u een momentopname van een test. De uitvoer wordt hier weergegeven voor een configuratie met drie knooppunten scale-out van SAP HANA:

Als de momentopname van de test is met het script is uitgevoerd, kunt u doorgaan met het plannen van de momentopnamen van de werkelijke opslag. Als dit niet geslaagd is, moet u de problemen onderzoeken voordat u verder gaat. De momentopname van de test moet rond blijven totdat de eerste echte momentopnamen klaar bent.


### <a name="step-7-perform-snapshots"></a>Stap 7: Momentopnamen uitvoeren

Wanneer de voorbereidende stappen zijn voltooid, kunt u gaan configureren en plannen van de momentopnamen van de werkelijke opslag. Het script dat moet worden gepland werkt met configuraties voor het omhoog schalen en uitschalen van SAP HANA. Voor de uitvoering van periodieke en reguliere van het back-upscript, plannen dat het script met behulp van de cron-hulpprogramma. 

Lees voor de opdrachtsyntaxis van de exacte en de functionaliteit **'Uitvoeren momentopnameback-up - azure_hana_backup'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).  

De uitvoering van het script `azure_hana_backup` maakt de opslag momentopname in de volgende drie fasen:

1. Een momentopname van een SAP HANA wordt uitgevoerd
1. Een momentopname van de opslag wordt uitgevoerd
1. Hiermee verwijdert u de SAP HANA-momentopname die is gemaakt voordat de uitvoering van de storage-momentopname

Voor het uitvoeren van het script, kunt u het aanroepen van de HDB uitvoerbare map waarnaar deze is gekopieerd. 

De bewaarperiode wordt beheerd met het aantal momentopnamen die zijn verzonden als een parameter als u het script uitvoeren. De hoeveelheid tijd die wordt gedekt door de storage-momentopnamen is een functie van de termijn van de uitvoering van het aantal momentopnamen die zijn verzonden als een parameter bij het uitvoeren van het script. Als het aantal momentopnamen worden bewaard groter is dan het getal dat zijn met de naam als een parameter in de aanroep van het script, wordt de oudste momentopname van de opslag van hetzelfde label wordt verwijderd voordat een nieuwe momentopname wordt uitgevoerd. Het getal geeft u de laatste parameter van de aanroep is het aantal u kunt gebruiken voor het beheren van het aantal momentopnamen worden bewaard. Met dit nummer, kunt u ook bepalen, indirect, de schijfruimte die wordt gebruikt voor momentopnamen. 


## <a name="snapshot-strategies"></a>Strategieën voor momentopname
De frequentie van momentopnamen voor de verschillende typen, is afhankelijk van of u de functie voor herstel na noodgevallen HANA grote instantie gebruiken. Deze functionaliteit is afhankelijk van opslagmomentopnamen, die mogelijk speciale aanbevelingen voor de perioden frequentie en de uitvoering van de storage-momentopnamen. 

In de overwegingen en aanbevelingen die volgen, wordt de veronderstelling dat u doet *niet* de disaster recovery functionaliteit gebruiken die HANA grote instanties biedt. In plaats daarvan kunt u de storage-momentopnamen zijn back-ups en kunnen voor point-in-time-herstel voor de afgelopen 30 dagen. Gezien de beperkingen van het aantal momentopnamen en ruimte, klanten zijn aangemerkt als de volgende vereisten:

- De hersteltijd voor point-in-time-herstel.
- De ruimte die wordt gebruikt.
- Het herstelpunt en dat de beoogde hersteltijden voor mogelijke herstel na een noodgeval.
- De uiteindelijke uitvoering van HANA full-back-ups op basis van schijven. Wanneer een volledige-databaseback-up op basis van schijven of de **backint** interface wordt uitgevoerd, mislukt de uitvoering van de storage-momentopnamen. Als u van plan bent voor het uitvoeren van volledige-databaseback-ups op storage-momentopnamen, zorg ervoor dat de uitvoering van de storage-momentopnamen gedurende deze tijd is uitgeschakeld.
- Het aantal momentopnamen per volume (beperkt tot 250).

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

De momentopname-periode is voor klanten die geen gebruik van het herstel na noodgevallen van HANA grote instanties, minder frequent optreden. In dergelijke gevallen klanten voeren de gecombineerde momentopnamen op /hana/data en /hana/shared (inclusief /usr/sap) in de periode van een 12-uurs of 24-uurs en ze houden de momentopnamen voor een maand. Hetzelfde geldt voor de momentopnamen van de back-up van het logboekvolume. Echter de uitvoering van SAP HANA transactielogboekback-ups op basis van het logboekvolume voor back-up vindt plaats in perioden van 5 tot 15 minuten.

Geplande storage-momentopnamen worden het beste uitgevoerd met behulp van de cron. Gebruikt hetzelfde script, voor alle back-ups en behoeften voor herstel na noodgevallen, en dat u de invoer script zodat deze overeenkomt met de verschillende back-uptijden aangevraagd. Deze momentopnamen worden alle geplande anders in cron, afhankelijk van de uitvoeringstijd: per uur, 12 uur, dagelijks of wekelijks. 

Hier volgt een voorbeeld van een cron-planning in/etc/crontab:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
In het vorige voorbeeld is een momentopname van een uurtarief gecombineerde die betrekking heeft op de volumes die de gegevens/hana/bevatten en /hana/shared/SID (inclusief/usr/sap) locaties. Gebruik dit type momentopname voor een snellere point-in-time-herstel in de afgelopen twee dagen. Er is bovendien een momentopname van een dagelijks op deze volumes. Dus, hebt u twee dagen dekking door momentopnamen van per uur, plus vier weken dekking door dagelijkse momentopnamen. De transactie logboek back-upvolume is bovendien back-ups dagelijks. Deze back-ups worden gehouden en vier weken. Zoals u in de derde regel van crontab ziet, worden de back-up van het transactielogboek HANA is gepland om uit te voeren om de 5 minuten. De uitvoertijd van de verschillende cron-taken die uitvoeren van de storage-momentopnamen worden gespreid, zodat deze momentopnamen worden niet uitgevoerd in één keer op een bepaald punt in tijd. 

In het volgende voorbeeld, voert u een momentopname van een gecombineerde die betrekking heeft op de volumes waarop de hana-/ gegevens en /hana/shared/SID (inclusief/usr/sap) locaties op uurbasis. U bewaard deze momentopnamen twee dagen. De momentopnamen van de back-up logboekvolumes van transactie worden uitgevoerd op basis van 5 minuten en gedurende 4 uur worden bewaard. Als is vóór de back-up van het logboekbestand van de HANA-transactie gepland om uit te voeren om de 5 minuten. De momentopname van de back-up logboekvolume van de transactie wordt uitgevoerd met een vertraging van 2 minuten nadat de back-up van de transactie is gestart. Binnen deze twee minuten duurt de SAP HANA-transactielogboek onder normale omstandigheden. Als voordat, het volume met het bestand Boot.ini LUN eenmaal per dag wordt ondersteund door een momentopname van de opslag en vier weken wordt bewaard.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

De volgende afbeelding ziet u de volgorde van het vorige voorbeeld, met uitzondering van het bestand Boot.ini LUN:

![Relatie tussen de back-ups en momentopnamen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA voert regelmatig schrijfbewerkingen ten opzichte van het volume /hana/log vastleggen van de doorgevoerde wijzigingen met de database. Op regelmatige basis schrijft SAP HANA een opslagpunt naar het volume /hana/data. Zoals opgegeven in crontab, is een SAP HANA transactielogboekback-up om de 5 minuten uitgevoerd. U ziet ook dat elk uur als gevolg van een momentopname van een gecombineerde storage activeren via de /hana/data en /hana/shared/SID volumes wordt uitgevoerd door een momentopname van een SAP HANA. Nadat de HANA-momentopname is voltooid, wordt de momentopname van het gecombineerde opslag wordt uitgevoerd. Volgens de instructies in crontab, wordt de momentopname van de opslag op het volume /hana/logbackup elke 5 minuten, ongeveer twee minuten na de HANA-transactie logboekback-up uitgevoerd.

> 

>[!IMPORTANT]
> Het gebruik van storage-momentopnamen voor back-ups van SAP HANA is nuttig wanneer de momentopnamen worden uitgevoerd in combinatie met SAP HANA transactielogboekback-ups. Deze transactielogboekback-ups moeten betrekking hebben op de perioden tussen de storage-momentopnamen. 

Als u een betrokkenheid bij gebruikers van een point-in-time-herstel van 30 dagen hebt ingesteld, moet u naar:

- In uitzonderlijke gevallen, toegang hebben tot een gecombineerde opslag momentopname over /hana/data en /hana/shared/SID die 30 dagen oud is.
- Aaneengesloten transactielogboekback-ups die betrekking hebben op de tijd tussen een van de gecombineerde opslagmomentopnamen hebben. De oudste momentopname van het back-up logboekvolume van transactie moet dus worden 30 dagen oud. Dit is niet het geval als u de transactielogboekback-ups naar een andere NFS-share die op Azure-opslag bevindt zich kopiëren. In dat geval kunt u pull-oude transactielogboekback-ups van deze NFS-share.

Als u wilt profiteren van een storage-momentopnamen en de uiteindelijke storage-replicatie van transactielogboekback-ups, moet u de locatie op waarnaar de SAP HANA de transactielogboekback-ups schrijft te wijzigen. In HANA Studio kunt u deze wijziging. Hoewel SAP HANA maakt automatisch een reservekopie van volledige logboek segmenten, moet u een back-log-interval om te worden deterministisch. Dit is vooral van toepassing wanneer u de optie voor disaster recovery, omdat u meestal wilt logboekback-ups met een deterministische punt uitvoeren. In het volgende geval zijn 15 minuten ingesteld als de back-upinterval logboek.

![Back-logboeken van de SAP HANA in SAP HANA Studio plannen](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

U kunt ook back-ups die vaker dan om de 15 minuten. Een vaker instelling wordt vaak gebruikt in combinatie met herstel na noodgevallen van HANA grote instanties. Sommige klanten voeren transactielogboekback-ups om de 5 minuten.  

Als de database heeft nooit back-ups, wordt de laatste stap is om uit te voeren van een bestand op basis van databaseback-up voor het maken van een eenmalige back-vermelding die moet zijn opgeslagen in de back-catalogus. SAP HANA kan anders uw opgegeven logboekback-ups niet starten.

![Een op basis van bestanden back-up maken van een post voor één back-up maken](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nadat de eerste geslaagde storage-momentopnamen zijn uitgevoerd, moet u het verwijderen van de test-momentopname die is uitgevoerd in stap 6. Lezen **'Test momentopnamen verwijderen - removeTestStorageSnapshot'** in het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) voor meer informatie. 


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Het aantal en de grootte van momentopnamen op het schijfvolume controleren

U kunt het aantal momentopnamen en het opslagverbruik van deze momentopnamen kunt controleren op een specifieke opslagvolume. De `ls` opdracht niet wordt weergegeven de momentopname-map of bestanden. Echter, de opdracht Linux-besturingssysteem `du` bevat gedetailleerde informatie over de storage-momentopnamen, omdat ze zijn opgeslagen op de dezelfde volumes. De opdracht kan worden gebruikt met de volgende opties:

- `du –sh .snapshot`: Deze optie biedt een totaal van alle momentopnamen in de map momentopname.
- `du –sh --max-depth=1`: Deze optie geeft een lijst van alle momentopnamen die zijn opgeslagen in de **.snapshot** map en de grootte van elke momentopname.
- `du –hc`: Deze optie geeft de totale grootte die worden gebruikt door alle momentopnamen.

Deze opdrachten gebruiken om ervoor te zorgen dat de opslag op de volumes niet door de momentopnamen die zijn uitgevoerd en die zijn opgeslagen worden verbruikt.

>[!NOTE]
>De momentopnamen van de opstartinstallatiekopie LUN zijn niet zichtbaar zijn met de voorgaande opdrachten.

### <a name="getting-details-of-snapshots"></a>Details van momentopnamen ophalen
Voor meer informatie over momentopnamen, kunt u ook het script gebruiken `azure_hana_snapshot_details`. Met dit script kan worden uitgevoerd in één van beide locaties als er een actieve server in de locatie voor noodherstel. Het script bevat de volgende uitvoer, onderverdeeld op basis van elk volume dat momentopnamen bevat: 
   * De grootte van het totale aantal momentopnamen in een volume
   * De volgende gegevens in elke momentopname in volume: 
      - De naam van de momentopname 
      - Aanmaaktijd 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan deze momentopname, indien van toepassing

Voor de syntaxis van de opdracht- en uitvoer controle **'Lijst met momentopnamen - azure_hana_snapshot_details'** in het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Het aantal momentopnamen op een server te verminderen

Zoals eerder beschreven, kunt u het nummer van bepaalde labels van de momentopnamen die u opslaat verminderen. De laatste twee parameters van de opdracht voor het initiëren van een momentopname zijn het label en het aantal momentopnamen die u wilt bewaren.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

In het vorige voorbeeld, een label voor de momentopname is **dailyhana** en is het aantal momentopnamen met dit label moet worden bewaard **28**. Als u op schijfruimteverbruik reageren, is het raadzaam om het aantal opgeslagen momentopnamen te verminderen. De eenvoudige manier om het aantal momentopnamen beperken tot en met 15, bijvoorbeeld, is het script uitvoeren met de laatste parameter zijn ingesteld op **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Als u het script met deze instelling uitvoert, is het aantal momentopnamen, met inbegrip van de momentopname van de nieuwe opslag, 15. De meest recente 15-momentopnamen worden bewaard, en de 15 oudere momentopnamen worden verwijderd.

 >[!NOTE]
 > Met dit script vermindert het aantal momentopnamen alleen als er meer dan 1 uur oud momentopnamen. Het script worden de momentopnamen die minder dan 1 uur oud zijn niet verwijderd. Deze beperkingen zijn gerelateerd aan de optionele herstel na noodgevallen aangeboden.

Als u niet meer wilt onderhouden van een set met momentopnamen met het voorvoegsel van de back-up **dailyhana** in de syntaxisvoorbeelden kunt u het script met uitvoeren **0** als het getal dat bewaren. Alle momentopnamen die overeenkomt met dat label worden vervolgens verwijderd. Verwijderen van alle momentopnamen kan echter de mogelijkheden van HANA grote instanties herstel na noodgevallen beïnvloeden.

Een tweede optie specifieke momentopnamen verwijderen is het gebruik van het script `azure_hana_snapshot_delete`. Met dit script is ontworpen voor het verwijderen van een momentopname of een set van momentopnamen, hetzij met behulp van de back-HANA-ID zoals gevonden in HANA Studio, of via de naam van de momentopname. Op dit moment alleen de back-up-ID is gekoppeld aan de momentopnamen die zijn gemaakt voor de **hana** type momentopname. Momentopname maken van back-ups van het type **logboeken** en **boot** geen momentopname van een SAP HANA wordt uitgevoerd en er bevindt zich dus er is geen back-ID voor deze momentopnamen worden gevonden. Als de naam van de momentopname wordt opgegeven, zoekt alle momentopnamen op de verschillende volumes die overeenkomen met de naam van de opgegeven momentopname. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Raadpleeg voor informatie over het script **'Verwijderen van een momentopname - azure_hana_snapshot_delete'** in het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Voer het script als gebruiker **hoofdmap**.

>[!IMPORTANT]
>Als er gegevens die alleen op de momentopname bestaat die u verwijdert, nadat de momentopname is verwijderd, dat gegevens verloren is permanent.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Terugzetten vanuit een momentopname van de opslag op bestandsniveau

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
Voor de momentopname-typen **hana** en **logboeken**, u hebt toegang tot de momentopnamen rechtstreeks op de volumes in de **.snapshot** directory. Er is een submap voor elk van de momentopnamen. U kunt elk bestand kopiëren in de staat waarin deze op het moment van de momentopname van de submap in de werkelijke mapstructuur. In de huidige versie van het script, is er **Nee** script dat is opgegeven voor het terugzetten van momentopnamen als een selfservice (Hoewel terugzetten van momentopnamen kan worden uitgevoerd als onderdeel van de selfservice-DR-scripts op de DR-site tijdens de failover) herstellen. Neem contact op met het operationele team van Microsoft door het openen van een serviceaanvraag voor het herstellen van een momentopname van een gewenste van de bestaande momentopnamen beschikbaar.

>[!NOTE]
>Enkel bestand werkt terugzetten niet voor momentopnamen van de opstartinstallatiekopie LUN onafhankelijk van het type van de eenheden HANA grote instantie. De **.snapshot** directory is niet beschikbaar gemaakt in het bestand Boot.ini LUN. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Herstellen naar de meest recente HANA-momentopname

In een scenario voor productie-omlaag, kan het proces van het herstellen van een momentopname van de opslag worden gestart als een klant-incident met ondersteuning voor Microsoft Azure. Het is een paar met hoge urgentie als gegevens in een productiesysteem is verwijderd, en de enige manier om op te halen om de productiedatabase te herstellen.

In een andere situatie een point-in-time-herstel mogelijk met lage urgentie en geplande dagen van tevoren. U kunt dit herstel met SAP HANA op Azure plannen in plaats van de markering van een hoge prioriteit verhogen. U kan bijvoorbeeld worden plan te upgraden van de SAP-software door het toepassen van een nieuwe uitbreiding-pakket. Vervolgens moet u terugkeren naar een momentopname die de status voor de uitbreiding pakketupgrade vertegenwoordigt.

Voordat u de aanvraag verzendt, moet u om voor te bereiden. De SAP HANA op Azure-team kan vervolgens de aanvragen worden verwerkt en de herstelde volumes bevatten. Daarna herstelt u de HANA-database op basis van de momentopnamen.

De mogelijkheden om op te halen van een momentopname is teruggezet met de nieuwe set hulpprogramma's worden beschreven in de sectie **'Het herstellen van een momentopname van een'** van het document [handmatig herstel handleiding voor SAP HANA op Azure Storage-momentopname](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Het volgende laat zien hoe u om voor te bereiden voor de aanvraag:

1. Bepaal welke momentopname om terug te zetten. Alleen de gegevens van hana/volume is teruggezet, tenzij u anders opdracht geven. 

1. Sluit de HANA-instantie.

   ![De HANA-exemplaar afsluiten](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database. Als de gegevensvolumes zijn nog steeds gekoppeld aan het besturingssysteem, de herstelbewerking van de momentopname is mislukt.
   ![Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Een Azure-ondersteuningsaanvraag openen en instructies over het terugzetten van een momentopname van een specifieke bevatten.

   - Tijdens het terugzetten: SAP HANA op Azure mogelijk gevraagd om bij te wonen een telefonische vergadering om ervoor te zorgen coördinatie, verificatie en bevestiging dat de juiste opslag-momentopname is teruggezet. 

   - Na het herstel: SAP HANA op Azure-Service waarschuwt u als de opslag-momentopname is teruggezet.

1. Nadat het herstelproces voltooid is, koppelen de gegevensvolumes.

   ![Alle gegevensvolumes koppelen](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Een andere mogelijkheid voor het ophalen, bijvoorbeeld SAP HANA-gegevensbestanden is hersteld van een momentopname van de opslag wordt beschreven in stap 7 van het document [handmatig herstel handleiding voor SAP HANA op Azure Storage-momentopname](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Het document [handmatig herstel handleiding voor SAP HANA op Azure Storage-momentopname](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) ziet u de restore-sequentie van momentopnameback-up. Gebruik deze documentatie voor het uitvoeren van een herstelpunt. 

>[!Note]
>Stap 7 is niet nodig zijn om uit te voeren als u uw hersteld door Microsoft operations momentopname.


### <a name="recover-to-another-point-in-time"></a>Herstellen naar een ander punt in tijd
Het document [handmatig herstel handleiding voor SAP HANA op Azure Storage-momentopname](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) ziet u de restore-sequentie naar een bepaald punt in tijd in de sectie **' De database herstellen naar het volgende punt in tijd'**. Gebruik deze documentatie voor het uitvoeren van een herstelpunt op een bepaald punt in tijd. 


## <a name="next-steps"></a>Volgende stappen
- Zie [principes voor herstel na noodgevallen en voorbereiding](hana-concept-preparation.md).
