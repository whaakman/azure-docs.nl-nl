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
ms.openlocfilehash: 21232e5a678d6deed920e57cd0433a3b85ca4fdc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64987901"
---
# <a name="backup-and-restore"></a>Back-ups en herstellen

>[!IMPORTANT]
>In dit artikel is geen vervanging voor de documentatie bij het beheer van SAP HANA of SAP-opmerkingen. We verwachten dat u een goed begrip van en deskundigheid in SAP HANA-beheer en bewerkingen, met name voor back-up, herstel, hoge beschikbaarheid en herstel na noodgevallen hebt. In dit artikel worden schermafbeeldingen van SAP HANA Studio weergegeven. Inhoud, de structuur en de aard van de schermen van SAP-beheerprogramma's en de hulpprogramma's zelf kunnen worden gewijzigd van SAP HANA, release-versie.

Het is belangrijk dat u stappen en processen in uw omgeving en met uw HANA-versies en versies uitoefenen. Voor een beter begrip van de algemene vereenvoudigd en bepaalde processen die in dit artikel beschreven. Ze zijn niet bedoeld om te worden gebruikt als gedetailleerde stappen voor het uiteindelijke bewerking handboeken. Als u maken van de bewerking handboeken voor uw configuraties wilt, testen en uitoefening van uw processen en documenteren van de processen die betrekking hebben op uw specifieke configuraties. 

Een van de belangrijkste aspecten van operationele databases is Beveilig ze tegen catastrofale gebeurtenissen. De oorzaak van deze gebeurtenissen kan van alles van natuurrampen gebruiksvriendelijke fouten zijn.

Back-ups van een database met de mogelijkheid om het te herstellen naar een willekeurig punt in tijd, zoals voordat iemand verwijderde kritieke gegevens, kunnen worden hersteld naar een status die zo dicht mogelijk bij de manier waarop deze is voordat het wordt onderbroken.

Twee typen back-ups moeten worden uitgevoerd voor het bereiken van de mogelijkheid om te herstellen:

- Databaseback-ups: Volledige, incrementele of differentiële back-ups
- Transactielogboekback-ups

Naast de volledige-databaseback-ups uitgevoerd op het toepassingsniveau van een, kunt u back-ups met storage-momentopnamen uitvoeren. Storage-momentopnamen niet vervangen door transactielogboekback-ups. Transactielogboekback-ups blijven belangrijk de database herstellen naar een bepaald punt in tijd of de logboeken van al opgegeven transacties leeg. Storage-momentopnamen kunnen recovery versnellen omdat u snel een afbeelding doorvoeren van de database. 

SAP HANA op Azure (grote instanties) biedt twee opties voor back-up en herstel:

- **Hiervoor zelf (DIY).** Nadat u ervoor te zorgen dat er voldoende schijfruimte beschikbaar is, voert u volledige database en logboekback-ups met behulp van een van de volgende methoden van de schijf-back-up. U kunt back-up ofwel rechtstreeks op volumes die zijn gekoppeld aan de eenheden HANA grote instantie of NFS-shares die zijn ingesteld op een Azure-machine (VM). In dat laatste geval, klanten instellen van een Linux-VM in Azure, Azure Storage koppelen aan de virtuele machine en deel van de opslag via een geconfigureerde NFS-server in die virtuele machine. Als u de back-up op basis van volumes die rechtstreeks aan HANA grote instantie eenheden koppelen uitvoert, kopieert u de back-ups naar Azure storage-account. Dit doen na het instellen van een Azure-VM die met NFS-shares die zijn gebaseerd op Azure Storage worden geëxporteerd. U kunt ook een Azure Backup-kluis of een Azure koude opslag gebruiken. 

   Een andere optie is het gebruik van een derde partij hulpprogramma voor gegevensbeveiliging voor het opslaan van de back-ups nadat ze zijn gekopieerd naar een Azure storage-account. De back-upoptie voor zelf ook kan het nodig zijn voor gegevens die u nodig hebt om op te slaan voor langere tijd voor nalevings- en controledoeleinden. In alle gevallen moet worden de back-ups gekopieerd naar NFS-shares vertegenwoordigd door een virtuele machine en de Azure Storage.

- **Infrastructuur voor back-up en herstellen van functionaliteit.** U kunt ook gebruik van de back-up en herstellen van de functionaliteit die de onderliggende infrastructuur van SAP HANA op Azure (grote instanties) biedt. Deze optie wordt voldaan aan de noodzaak van back-ups en snelle herstelbewerkingen. De rest van deze sectie wordt de functionaliteit voor back-up en herstellen die wordt aangeboden met HANA grote instanties. In deze sectie worden ook de relatie die back-up en herstel de disaster recovery-functionaliteit die worden aangeboden door HANA grote instanties.

> [!NOTE]
>   SAP HANA-momentopnamen is afhankelijk van de momentopnametechnologie die wordt gebruikt door de onderliggende infrastructuur van HANA grote instanties. SAP HANA-momentopnamen werken op dit moment niet in combinatie met meerdere tenants van containers voor SAP HANA-multitenant-database. Als er slechts één tenant is geïmplementeerd, SAP HANA-momentopnamen werken en u kunt deze methode gebruiken.

## <a name="use-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Gebruik storage-momentopnamen van SAP HANA op Azure (grote instanties)

De opslaginfrastructuur onderliggende SAP HANA op Azure (grote instanties) biedt ondersteuning voor storage-momentopnamen van volumes. Zowel back-up en herstel van volumes wordt ondersteund, met de volgende overwegingen:

- In plaats van een volledige databaseback-ups, worden opslag volume momentopnamen regelmatig.
- Wanneer een momentopname wordt geactiveerd via /hana/data en /hana/shared, waaronder /usr/sap, volumes, de momentopnametechnologie een SAP HANA momentopname initieert voordat deze wordt uitgevoerd de storage-momentopname. Deze SAP HANA-momentopname is het setup-punt voor het uiteindelijke log herstelbewerkingen kunnen na het herstel van de momentopname van de opslag. Voor een HANA-momentopname om succesvol te zijn, moet u een actief exemplaar van HANA. In een scenario HSR, wordt niet een momentopname van de opslag ondersteund op een huidige secundaire knooppunt waar een HANA-momentopname kan niet worden uitgevoerd.
- Nadat de momentopname van de opslag wordt uitgevoerd, wordt de SAP HANA-momentopname verwijderd.
- Transactielogboekback-ups zijn vaak gemaakt en opgeslagen in het volume /hana/logbackups of in Azure. U kunt het /hana/logbackups volume met de transactielogboekback-ups als u wilt maken van een momentopname van een afzonderlijk activeren. In dat geval moet u niet een HANA-momentopname worden uitgevoerd.
- Als u een database naar een bepaald punt in tijd, voor een storing in de productie herstellen moet, aanvragen die Microsoft Azure-ondersteuning of SAP HANA op Azure herstellen naar een bepaalde opslag-momentopname. Een voorbeeld is een geplande herstel van een sandbox-systeem in de oorspronkelijke staat.
- De SAP HANA-momentopname die opgenomen in de storage-momentopname is een offset punt voor het toepassen van transactielogboekback-ups die is uitgevoerd en die zijn opgeslagen nadat de storage-momentopname is gemaakt.
- Deze transactielogboekback-ups gaat naar de database herstellen naar een bepaald punt in tijd.

U kunt storage-momentopnamen die zijn gericht op drie soorten volumes uitvoeren:

- Een gecombineerde momentopname/hana/gegevens en/hana/is gedeeld, waaronder /usr/sap. Deze momentopname is vereist voor het maken van een momentopname van een SAP HANA als voorbereiding op de momentopname van de opslag. De SAP HANA-momentopname zorgt ervoor dat de database in een consistente status van een punt opslag. Voor het herstelproces is die een punt om in te stellen op.
- Een momentopname van een afzonderlijke via/hana/logbackups.
- De partitie van een besturingssysteem.

Als u de meest recente momentopname scripts en documentatie, Zie [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Wanneer het downloaden van de momentopname-script-pakket van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), krijgt u drie bestanden. Een van de bestanden wordt in een PDF-bestand voor de functionaliteit die beschreven. Nadat u de set hulpprogramma's hebt gedownload, volgt u de instructies in ' de momentopname van hulpprogramma's downloaden."

## <a name="storage-snapshot-considerations"></a>Overwegingen met betrekking tot opslag-momentopname

>[!NOTE]
>Storage-momentopnamen gebruiken opslagruimte die wordt toegewezen aan de HANA grote instantie-eenheden. Houd rekening met de volgende aspecten van de planning voor storage-momentopnamen en het aantal opslagmomentopnamen te houden. 

De specifieke mechanics van opslagmomentopnamen voor SAP HANA op Azure (grote instanties) zijn onder andere:

- Een momentopname van een specifieke opslag op het punt in tijd wanneer wordt het opgevat verbruikt weinig opslagruimte.
- Als gegevens inhoud wordt gewijzigd en de inhoud van bestanden op het opslagvolume wijzigen SAP HANA-gegevens, moet de momentopname voor het opslaan van de oorspronkelijke inhoud blokkeren en de gegevens worden gewijzigd.
- Als gevolg hiervan de storage-momentopname wordt vergroot. Hoe langer de momentopname bestaat, hoe groter de storage-momentopname wordt.
- De meer wijzigingen die zijn aangebracht aan het volume van SAP HANA-database gedurende de levensduur van een momentopname opslag, hoe groter het verbruik van de ruimte van de momentopname van de opslag.

SAP HANA op Azure (grote instanties) wordt geleverd met grootten voor de SAP HANA-gegevens en logboekvolumes vast volume. Uitvoeren van momentopnamen van de volumes eats in uw ruimte op volume. U moet:

- Bepalen wanneer de storage-momentopnamen te plannen.
- Het gebruik van de ruimte van de opslagvolumes bewaken. 
- Het aantal momentopnamen die u opslaat beheren. 

Wanneer u grote hoeveelheden gegevens importeren of andere belangrijke wijzigingen aangebracht in de HANA-database uitvoert, kunt u de storage-momentopnamen uitschakelen. 


De volgende secties bevatten informatie voor het uitvoeren van deze momentopnamen en algemene aanbevelingen omvatten:

- Hoewel de hardware 255 momentopnamen per volume tolereren kan, die u wilt blijven goed onder dit getal. De aanbeveling is 250 of minder.
- Voordat u opslagmomentopnamen uitvoert, controleren en bijhouden van vrije ruimte.
- Verlaag het aantal opslagmomentopnamen op basis van de vrije ruimte. U kunt het aantal momentopnamen die u bijhoudt verlagen of kunt u de volumes uitbreiden. U kunt extra opslagruimte bestellen in eenheden van 1 terabyte.
- Tijdens de activiteiten, zoals het verplaatsen van gegevens in SAP HANA met hulpprogramma voor migratie van SAP-platform (R3load) of SAP HANA-databases herstellen vanuit back-ups, opslagmomentopnamen op het volume /hana/data uitschakelen 
- Vermijd indien mogelijk opslagmomentopnamen tijdens grotere reorganisaties van SAP HANA-tabellen.
- Storage-momentopnamen zijn vereist om te profiteren van de noodherstelprocessen mogelijkheden voor herstel van SAP HANA op Azure (grote instanties).

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Vereisten voor het gebruik van selfservice opslagmomentopnamen

Om ervoor te zorgen dat de momentopname-script met succes wordt uitgevoerd, zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties. Perl vooraf is geïnstalleerd op uw eenheid HANA grote instantie. Om te controleren of de versie van Perl, gebruikt u de volgende opdracht:

`perl -v`

![De openbare sleutel wordt door het uitvoeren van deze opdracht gekopieerd](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Instellen van opslagmomentopnamen

Volg deze stappen om in te stellen op storage-momentopnamen met HANA grote instanties.
1. Zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote instanties.
1. Wijzig de/etc/ssh/ssh\_config om toe te voegen van de regel _MACs hmac-sha1_.
1. Maak een back-SAP HANA-gebruikersaccount op het hoofdknooppunt voor elk SAP HANA-exemplaar dat u uitvoert, indien van toepassing.
1. De SAP HANA HDB-client installeren op alle servers in de SAP HANA grote instanties.
1. Maak een openbare sleutel voor toegang tot de onderliggende opslaginfrastructuur die het maken van momentopnamen bepaalt op de eerste server voor SAP HANA grote instanties van elke regio.
1. Kopieer de scripts en het configuratiebestand van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) naar de locatie van **hdbsql** in de SAP HANA-installatie.
1. Wijzig de *HANABackupDetails.txt* bestand zo nodig voor de betreffende klant-specificaties.

Lees het laatste momentopname scripts en documentatie van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Zie voor de eerder vermelde stappen [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

### <a name="consideration-for-mcod-scenarios"></a>Overweging voor MCOD scenario 's
Als u een [MCOD scenario](https://launchpad.support.sap.com/#/notes/1681092) met meerdere instanties van SAP HANA op één HANA grote instantie-eenheid, hebt u afzonderlijke opslagvolumes die zijn ingericht voor elk van de SAP HANA-instanties. Zie voor meer informatie over MDC en andere overwegingen "Dingen die belangrijk" in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Stap 1: De SAP HANA HDB-client installeren

Het Linux-besturingssysteem geïnstalleerd op SAP HANA op Azure (grote instanties) bevat de mappen en scripts die nodig zijn voor het uitvoeren van SAP HANA-opslagmomentopnamen voor back-up en herstel na noodgeval hersteldoeleinden. Raadpleeg voor meer recente versies in [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). De meest recente versie van de scripts is 4.0. Verschillende scripts mogelijk verschillende secundaire releases binnen dezelfde primaire versie.

Het is uw verantwoordelijkheid voor het installeren van de SAP HANA HDB-client op de eenheden HANA grote instantie tijdens de installatie van SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Stap 2: Wijzig de/etc/ssh/ssh\_config

Deze stap wordt beschreven in 'Communicatie met de opslag inschakelen' [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-3-create-a-public-key"></a>Stap 3: Maak een openbare sleutel

Voor toegang tot de interfaces van de momentopname van opslag van uw tenant HANA grote instantie, tot stand brengen een procedure aanmelden via een openbare sleutel. 

Maak op de eerste SAP HANA op Azure (grote instanties)-server in uw tenant, een openbare sleutel voor toegang tot de opslaginfrastructuur. Met een openbare sleutel is niet een wachtwoord vereist voor aanmelding bij de storage-momentopname-interfaces. Ook is het hoeft te onderhouden wachtwoordreferenties met een openbare sleutel. 

Voor het genereren van een openbare sleutel, raadpleegt u 'Inschakelen communicatie met de opslag' [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-4-create-an-sap-hana-user-account"></a>Stap 4: Een SAP HANA-gebruikersaccount maken

Om te beginnen met het maken van momentopnamen van SAP HANA, maakt u een gebruikersaccount in SAP HANA die de opslag momentopname-scripts kunnen gebruiken. Maak een SAP HANA-gebruikersaccount in SAP HANA Studio voor dit doel. De gebruiker moet worden gemaakt onder het SYSTEMDB en *niet* onder de SID-database voor MDC. De gebruiker wordt in de omgeving enkele container gemaakt in de tenant-database. Dit account moet hebben **back-up Admin** en **catalogus** bevoegdheden. 

Als u wilt instellen en gebruiken van een gebruikersaccount, raadpleegt u "Inschakelen communicatie met SAP HANA" in [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0).


### <a name="step-5-authorize-the-sap-hana-user-account"></a>Stap 5: Toestaan dat de SAP HANA-gebruikersaccount

In deze stap verleent u de SAP HANA-gebruikersaccount dat u hebt gemaakt, zodat de scripts niet hoeft in te dienen wachtwoorden tijdens runtime. De SAP HANA-opdracht `hdbuserstore` wordt het maken van de sleutel van een SAP HANA-gebruiker. De sleutel wordt opgeslagen op een of meer knooppunten van SAP HANA. De sleutel van de gebruiker krijgt de gebruiker toegang tot SAP HANA zonder het beheren van wachtwoorden op in het proces van scripts. Het uitvoeren van scripts proces wordt verderop in dit artikel besproken.

>[!IMPORTANT]
>Deze configuratieopdrachten uitvoeren met de dezelfde context van de gebruiker die de momentopnameopdrachten worden uitgevoerd in. Anders wordt goed de momentopnameopdrachten niet.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Stap 6: Ophalen van de momentopname-scripts, het configureren van de momentopnamen en het testen van de configuratie en connectiviteit

Download de meest recente versie van de scripts van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). De manier waarop die de scripts zijn geïnstalleerd met de versie 4.0 van de scripts worden gewijzigd. Zie voor meer informatie "Enable communicatie met SAP HANA" in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Zie 'Eenvoudige installatie van de momentopname-hulpprogramma's (standaard)' in voor de exacte volgorde van opdrachten, [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). We raden het gebruik van de standaardinstallatie. 

Upgrade uitvoeren van versie 3.x 4.0, Zie 'Een bestaande installatie upgraden' [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Zie voor het verwijderen van de hulpprogrammaset 4.0, 'Verwijderen van de momentopname-hulpprogramma's ' in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Vergeet niet om uit te voeren van de stappen in 'Volledige installatie van hulpprogramma's voor snapshot' [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Het doel van de andere scripts en -bestanden wordt omdat deze is geïnstalleerd beschreven in "Wat zijn hulpprogramma's voor deze momentopname?" in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Voordat u de momentopname-hulpprogramma's configureert, zorg ervoor dat u ook geconfigureerd HANA back-locaties en instellingen correct. Zie voor meer informatie 'SAP HANA-configuratie' in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

De configuratie van de momentopname-werkset wordt beschreven in 'file Config - HANABackupCustomerDetails.txt' [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-connectivity-with-sap-hana"></a>Test de verbinding met SAP HANA

Nadat u de configuratiegegevens in plaatst de *HANABackupCustomerDetails.txt* bestand, moet u controleren of de configuraties voor de HANA-Instantiegegevens correct zijn. Gebruik het script `testHANAConnection`, die onafhankelijk is van de configuratie van een SAP HANA omhoog- of scale-out.

Zie voor meer informatie 'Controleer de verbinding met SAP HANA - testHANAConnection' in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-storage-connectivity"></a>Opslagconnectiviteit testen

De volgende stap is om te controleren of de verbinding met de opslag op basis van de gegevens die u plaatst in het *HANABackupCustomerDetails.txt* configuratiebestand. Voer een momentopname van een test. Voordat u de `azure_hana_backup` opdracht, moet u deze test uitvoeren. Zie voor de reeks opdrachten voor deze test, "Controleer de verbinding met de opslag - testStorageSnapshotConnection" "in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Na een geslaagde aanmelding bij de interfaces van de virtuele machine opslag, het script wordt voortgezet met fase 2 en maakt u een momentopname van een test. De uitvoer wordt hier weergegeven voor een configuratie met drie knooppunten scale-out van SAP HANA.

Als de momentopname van de test met het script wordt uitgevoerd, kunt u de werkelijke opslagmomentopnamen plannen. Als dit niet geslaagd is, de problemen onderzoeken voordat u verdergaat. De momentopname van de test moet rond blijven totdat de eerste echte momentopnamen klaar bent.


### <a name="step-7-perform-snapshots"></a>Stap 7: Momentopnamen uitvoeren

Wanneer de voorbereidende stappen zijn voltooid, kunt u gaan configureren en plannen van de momentopnamen van de werkelijke opslag. Het script dat moet worden gepland werkt met configuraties voor het omhoog schalen en uitschalen van SAP HANA. Voor de uitvoering van periodieke en reguliere van het back-upscript, plannen dat het script met behulp van de cron-hulpprogramma. 

Zie 'Uitvoeren momentopnameback-up - azure_hana_backup' in voor de opdrachtsyntaxis van de exacte en de functionaliteit [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Wanneer het script `azure_hana_backup` wordt uitgevoerd, maakt de opslag momentopname in de volgende drie fasen:

1. Een momentopname van een SAP HANA wordt uitgevoerd.
1. Een momentopname van de opslag wordt uitgevoerd.
1. De SAP HANA-momentopname die is gemaakt voordat de storage-momentopname is uitgevoerd wordt verwijderd.

Het script wordt uitgevoerd, kunt u het aanroepen van de HDB uitvoerbare map waarnaar deze is gekopieerd. 

De bewaarperiode wordt beheerd met het aantal momentopnamen die zijn verzonden als een parameter als u het script uitvoert. De hoeveelheid tijd die wordt gedekt door de storage-momentopnamen is een functie van de termijn van de uitvoering van het aantal momentopnamen die zijn verzonden als een parameter wanneer het script wordt uitgevoerd. 

Als het aantal momentopnamen worden bewaard groter is dan het getal dat zijn met de naam als een parameter in de aanroep van het script, wordt de oudste momentopname van de opslag van hetzelfde label wordt verwijderd voordat een nieuwe momentopname wordt uitgevoerd. Het getal geeft u de laatste parameter van de aanroep is het aantal u kunt gebruiken voor het beheren van het aantal momentopnamen worden bewaard. Met dit nummer kunt ook u bepalen, indirect, de schijfruimte die wordt gebruikt voor momentopnamen. 


## <a name="snapshot-strategies"></a>Strategieën voor momentopname
De frequentie van momentopnamen voor de verschillende typen, is afhankelijk van of u de functie voor herstel na noodgevallen HANA grote instantie gebruiken. Deze functionaliteit is afhankelijk van opslagmomentopnamen, die mogelijk speciale aanbevelingen voor de perioden frequentie en de uitvoering van de storage-momentopnamen. 

In de overwegingen en aanbevelingen die volgen, wordt de veronderstelling dat u doet *niet* de disaster recovery functionaliteit gebruiken die HANA grote instanties biedt. In plaats daarvan kunt u de storage-momentopnamen zijn back-ups en kunnen voor point-in-time-herstel voor de afgelopen 30 dagen. Gezien de beperkingen van het aantal momentopnamen en ruimte, houd rekening met de volgende vereisten:

- De hersteltijd voor point-in-time-herstel.
- De ruimte die wordt gebruikt.
- Het herstelpunt en dat de beoogde hersteltijden voor mogelijke herstel na een noodgeval.
- De uiteindelijke uitvoering van HANA full-back-ups op basis van schijven. Wanneer een volledige-databaseback-up op basis van schijven of de **backint** interface wordt uitgevoerd, mislukt de uitvoering van de storage-momentopnamen. Als u van plan bent om uit te voeren van volledige-databaseback-ups op storage-momentopnamen, zorg ervoor dat de uitvoering van de storage-momentopnamen gedurende deze tijd is uitgeschakeld.
- Het aantal momentopnamen per volume, die beperkt tot 250 tekens is.

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

Als u het herstel na noodgevallen van HANA grote instanties niet gebruikt, wordt de momentopname-periode minder frequent optreden. In dergelijke gevallen de gecombineerde momentopnamen op /hana/data en /hana/shared, waaronder /usr/sap, in 12-uurs of 24-uurs perioden te uitvoeren. Houd de momentopnamen voor een maand. Hetzelfde geldt voor de momentopnamen van de back-up van het logboekvolume. De uitvoering van SAP HANA transactielogboekback-ups op basis van het logboekvolume voor back-up vindt plaats in de 5 minuten voor perioden van 15 minuten.

Geplande storage-momentopnamen worden het beste uitgevoerd met behulp van de cron. Gebruikt hetzelfde script, voor alle back-ups en herstel na noodgevallen moet. Het script wijzigt invoer zodat deze overeenkomen met de verschillende back-uptijden aangevraagd. Deze momentopnamen worden alle anders gepland in cron, afhankelijk van de uitvoeringstijd. Kan het zijn per uur, elke 12 uur, dagelijks of wekelijks. 

Het volgende voorbeeld ziet een cron-planning in /etc/crontab:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
In het vorige voorbeeld wordt een momentopname van een uurtarief gecombineerde bevat informatie over de volumes waarop de /hana/data en /hana/shared/SID, waaronder /usr/sap, locaties. Gebruik dit type momentopname voor een snellere point-in-time-herstel in de afgelopen twee dagen. Er is ook een dagelijkse momentopnamen op deze volumes. Daarom, moet u twee dagen lang dekking door momentopnamen van per uur plus vier weken dekking door dagelijkse momentopnamen. De transactie logboek back-upvolume is ook back-ups dagelijks. Deze back-ups worden bewaard gedurende vier weken. 

Zoals u in de derde regel van crontab ziet, worden de back-up van het transactielogboek HANA is gepland om uit te voeren om de 5 minuten. De uitvoertijd van de verschillende cron-taken met storage-momentopnamen worden gespreid. Op deze manier kunnen de momentopnamen niet worden uitgevoerd in één keer op een bepaald punt in tijd. 

In het volgende voorbeeld voert u een momentopname van een gecombineerde die betrekking heeft op de volumes waarop de /hana/data en /hana/shared/SID, waaronder /usr/sap locaties op uurbasis. U bewaard deze momentopnamen twee dagen. De momentopnamen van de back-up logboekvolumes van transactie worden uitgevoerd op basis van de 5 minuten en gedurende vier uur worden bewaard. Als is vóór de back-up van het logboekbestand van de HANA-transactie gepland voor uitvoering elke 5 minuten. 

De momentopname van de back-up logboekvolume van de transactie wordt uitgevoerd met een vertraging van 2 minuten nadat de back-up van de transactie is gestart. Onder normale omstandigheden, is de SAP HANA-transactie logboekback-up is voltooid binnen deze twee minuten. Als voordat, het volume met het bestand Boot.ini LUN eenmaal per dag wordt ondersteund door een momentopname van de opslag en vier weken wordt bewaard.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

De volgende afbeelding ziet u de reeksen in het vorige voorbeeld. Het bestand Boot.ini LUN is uitgesloten.

![Relatie tussen de back-ups en momentopnamen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA voert regelmatig schrijfbewerkingen ten opzichte van het volume /hana/log vastleggen van de doorgevoerde wijzigingen met de database. Op regelmatige basis schrijft SAP HANA een opslagpunt naar het volume /hana/data. Zoals opgegeven in crontab, een transactielogboek van SAP HANA elke 5 minuten wordt uitgevoerd. 

U ziet ook dat elk uur als gevolg van het activeren van een gecombineerde opslag via het volume /hana/data en /hana/shared/SID volume snapshot wordt uitgevoerd door een momentopname van een SAP HANA. Nadat de HANA-momentopname is voltooid, een momentopname van de gecombineerde opslag wordt uitgevoerd. Volgens de instructies in crontab, wordt de momentopname van de opslag op het volume /hana/logbackup uitgevoerd elke 5 minuten, ongeveer twee minuten na het transactielogboek van HANA.

> 

>[!IMPORTANT]
> Het gebruik van storage-momentopnamen voor back-ups van SAP HANA is nuttig wanneer de momentopnamen worden uitgevoerd in combinatie met SAP HANA transactielogboekback-ups. Deze transactielogboekback-ups moeten betrekking hebben op de perioden tussen de storage-momentopnamen. 

Als u een betrokkenheid bij gebruikers van een point-in-time-herstel van 30 dagen hebt ingesteld, moet u naar:

- Toegang tot een momentopname van een gecombineerde opslag via /hana/data en /hana/shared/SID die 30 dagen oud is, in uitzonderlijke gevallen. 
- Aaneengesloten transactielogboekback-ups die betrekking hebben op de tijd tussen een van de gecombineerde opslagmomentopnamen hebben. De oudste momentopname van het back-up logboekvolume van transactie moet dus worden 30 dagen oud. Dit is niet het geval als u de transactielogboekback-ups kopiëren naar een andere NFS-share die zich in Azure Storage. In dat geval kunt u pull-oude transactielogboekback-ups van deze NFS-share.

Als u wilt profiteren van een storage-momentopnamen en de uiteindelijke storage-replicatie van transactielogboekback-ups, wijzigt u de locatie op waarnaar de SAP HANA de transactielogboekback-ups schrijft. In HANA Studio kunt u deze wijziging. 

Hoewel SAP HANA maakt automatisch een reservekopie van volledige logboek segmenten, opgeven voor een back-log-interval om te worden deterministisch. Dit is vooral van toepassing wanneer u de optie voor disaster recovery, omdat u meestal wilt uitvoeren van logboekback-ups met een deterministische punt. In het volgende geval is 15 minuten ingesteld als de back-upinterval logboek.

![Back-logboeken van de SAP HANA in SAP HANA Studio plannen](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ook kunt u back-ups die vaker dan om de 15 minuten. Een vaker instelling wordt vaak gebruikt in combinatie met herstel na noodgevallen van HANA grote instanties. Sommige klanten voeren transactielogboekback-ups om de 5 minuten.

Als de database heeft nooit back-ups, wordt de laatste stap is om uit te voeren van een bestand op basis van databaseback-up voor het maken van een eenmalige back-vermelding die moet zijn opgeslagen in de back-catalogus. SAP HANA kan anders uw opgegeven logboekback-ups niet starten.

![Een op basis van bestanden back-up maken van een post voor één back-up maken](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nadat de eerste geslaagde opslagmomentopnamen hebt uitgevoerd, verwijdert u de test-momentopname die u in stap 6 hebt uitgevoerd. Zie voor meer informatie 'Test momentopnamen verwijderen - removeTestStorageSnapshot' in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 


### <a name="monitor-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Het aantal en de grootte van momentopnamen op het schijfvolume bewaken

U kunt het aantal momentopnamen en het opslagverbruik van deze momentopnamen kunt controleren op een specifieke opslagvolume. De `ls` opdracht niet wordt weergegeven de momentopname-map of bestanden. De opdracht Linux-besturingssysteem `du` bevat gedetailleerde informatie over de storage-momentopnamen omdat ze zijn opgeslagen op de dezelfde volumes. Gebruik de opdracht met de volgende opties:

- `du –sh .snapshot`: Deze optie biedt een totaal van alle momentopnamen in de map momentopname.
- `du –sh --max-depth=1`: Deze optie geeft een lijst van alle momentopnamen die zijn opgeslagen in de **.snapshot** map en de grootte van elke momentopname.
- `du –hc`: Deze optie geeft de totale grootte die worden gebruikt door alle momentopnamen.

Deze opdrachten gebruiken om ervoor te zorgen dat de momentopnamen die zijn uitgevoerd en die zijn opgeslagen op de opslag op de volumes niet gebruiken.

>[!NOTE]
>De momentopnamen van de opstartinstallatiekopie LUN zijn niet zichtbaar zijn met de voorgaande opdrachten.

### <a name="get-details-of-snapshots"></a>Details van momentopnamen ophalen
Voor meer informatie over momentopnamen, gebruikt u het script `azure_hana_snapshot_details`. U kunt dit script uitvoeren op een locatie als er een actieve server in de locatie voor noodherstel. Het script bevat de volgende uitvoer, onderverdeeld op basis van elk volume dat momentopnamen bevat: 
   * De grootte van het totale aantal momentopnamen in een volume
   * De volgende gegevens in elke momentopname in volume: 
      - De naam van de momentopname 
      - Aanmaaktijd 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan deze momentopname, indien van toepassing

Zie 'Lijst met momentopnamen - azure_hana_snapshot_details' in voor de syntaxis van de opdracht en de uitvoer, [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reduce-the-number-of-snapshots-on-a-server"></a>Verminder het aantal momentopnamen op een server

Als eerder uiteengezet verkleint u het nummer van bepaalde labels van de momentopnamen die u opslaat. De laatste twee parameters van de opdracht voor het initiëren van een momentopname zijn het label en het aantal momentopnamen die u wilt bewaren.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

In het vorige voorbeeld, een label voor de momentopname is **dailyhana**. Het aantal momentopnamen met dit label moet worden gehouden is **28**. Als u op schijfruimteverbruik reageren, is het raadzaam om het aantal opgeslagen momentopnamen te verminderen. Een eenvoudige manier om te beperken van het aantal momentopnamen naar 15, bijvoorbeeld, is het script uitvoeren met de laatste parameter zijn ingesteld op **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Als u het script met deze instelling uitvoert, is het aantal momentopnamen, waaronder de nieuwe opslag-momentopname, 15. De meest recente 15-momentopnamen worden bewaard, en de 15 oudere momentopnamen worden verwijderd.

 >[!NOTE]
 > Met dit script vermindert het aantal momentopnamen alleen als er meer dan een uur oud momentopnamen. Het script niet momentopnamen die minder dan een uur oud zijn verwijderd. Deze beperkingen zijn gerelateerd aan de optionele herstel na noodgevallen aangeboden.

Als u niet meer wilt onderhouden van een set met momentopnamen met het voorvoegsel van de back-up **dailyhana** uitvoeren in de voorbeelden is de syntaxis voor het script **0** als het getal dat bewaren. Alle momentopnamen die overeenkomen met dit label wordt vervolgens verwijderd. Alle momentopnamen verwijderen kan van invloed zijn op de mogelijkheden van HANA grote instanties herstel na noodgevallen.

Een tweede optie specifieke momentopnamen verwijderen is het gebruik van het script `azure_hana_snapshot_delete`. Met dit script is ontworpen voor het verwijderen van een momentopname of een set van momentopnamen, hetzij met behulp van de back-HANA-ID zoals gevonden in HANA Studio of via de naam van de momentopname. Op dit moment alleen de back-up-ID is gekoppeld aan de momentopnamen die zijn gemaakt voor de **hana** type momentopname. Momentopname maken van back-ups van het type **logboeken** en **boot** niet uitvoeren van een SAP HANA momentopname, zodat er geen back-up-ID voor deze momentopnamen worden gevonden. Als de naam van de momentopname wordt opgegeven, zoekt alle momentopnamen op de verschillende volumes die overeenkomen met de naam van de opgegeven momentopname. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Zie voor meer informatie over het script 'Verwijderen van een momentopname - azure_hana_snapshot_delete' in [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Voer het script als gebruiker **hoofdmap**.

>[!IMPORTANT]
>Als er gegevens alleen op de momentopname die u wilt verwijderen, nadat de momentopname is verwijderd, dat gegevens verloren is permanent.


## <a name="file-level-restore-from-a-storage-snapshot"></a>Terugzetten vanuit een momentopname van de opslag op bestandsniveau

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
Voor de momentopname-typen **hana** en **logboeken**, u hebt toegang tot de momentopnamen rechtstreeks op de volumes in de **.snapshot** directory. Er is een submap voor elk van de momentopnamen. Elk bestand in de staat waarin deze op het moment van de momentopname van de submap in de werkelijke mapstructuur kopiëren. 

In de huidige versie van het script, wordt er *geen* script dat is opgegeven voor het terugzetten van momentopnamen als self-service herstellen. Terugzetten van momentopnamen kan worden uitgevoerd als onderdeel van de scripts voor het herstel na noodgevallen selfservice op de site voor noodherstel tijdens de failover. Als u een momentopname van een gewenste herstellen van de bestaande beschikbaar momentopnamen, moet u door het openen van een serviceaanvraag contact opnemen met het operationele team van Microsoft.

>[!NOTE]
>Eén bestand werkt terugzetten niet voor momentopnamen van de opstartinstallatiekopie LUN onafhankelijk van het type van de eenheden HANA grote instantie. De **.snapshot** directory wordt niet weergegeven in het bestand Boot.ini LUN. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>Herstellen naar de meest recente HANA-momentopname

In een scenario voor productie-omlaag, kan het proces van het herstellen van een momentopname van de opslag worden gestart als een klant-incident met ondersteuning voor Microsoft Azure. Het is een paar met hoge urgentie als gegevens in een productiesysteem is verwijderd en is de enige manier om op te halen om de productiedatabase te herstellen.

In een andere situatie een point-in-time-herstel mogelijk met lage urgentie en geplande dagen van tevoren. U kunt dit herstel met SAP HANA op Azure plannen in plaats van de markering van een hoge prioriteit verhogen. Bijvoorbeeld, als u wilt upgraden van de SAP-software door het toepassen van een nieuwe uitbreiding-pakket. Vervolgens moet u terugkeren naar een momentopname die de status voor de uitbreiding pakketupgrade vertegenwoordigt.

Voordat u de aanvraag verzendt, moet u om voor te bereiden. De SAP HANA op Azure-team kan vervolgens de aanvragen worden verwerkt en de herstelde volumes bevatten. Daarna herstelt u de HANA-database op basis van de momentopnamen.

Voor de mogelijkheden voor het ophalen van een momentopname hersteld met de nieuwe set hulpprogramma's, Zie 'Hoe om te herstellen van een momentopname van een' in [handmatig herstel uphandleiding voor SAP HANA op Azure vanuit een momentopname van de opslag](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Volg deze stappen om voor te bereiden voor de aanvraag.

1. Bepaal welke momentopname om terug te zetten. Alleen de gegevens van hana/volume is teruggezet, tenzij u anders opdracht geven. 

1. Sluit de HANA-instantie.

   ![De HANA-exemplaar afsluiten](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database. Als de gegevensvolumes zijn nog steeds gekoppeld aan het besturingssysteem, de herstelbewerking van de momentopname is mislukt.

   ![Ontkoppel de gegevensvolumes op elk knooppunt van HANA-database](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Een Azure-ondersteuningsaanvraag openen en bevat instructies over het terugzetten van een momentopname van een specifieke:

   - Tijdens het terugzetten: SAP HANA op Azure-Service mogelijk gevraagd om bij te wonen een telefonische vergadering te coördineren, te controleren en te bevestigen dat de juiste opslag-momentopname is teruggezet. 

   - Na het herstel: SAP HANA op Azure-Service waarschuwt u als de opslag-momentopname is teruggezet.

1. Nadat het herstelproces voltooid is, koppelen de gegevensvolumes.

   ![Alle gegevensvolumes koppelen](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Een andere mogelijkheid voor het ophalen, bijvoorbeeld SAP HANA-gegevensbestanden is hersteld van een momentopname van de opslag en wordt beschreven in stap 7 in [handmatig herstel uphandleiding voor SAP HANA op Azure vanuit een momentopname van de opslag](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Als u wilt herstellen vanuit een momentopname van back-up, Zie [handmatig herstel uphandleiding voor SAP HANA op Azure vanuit een momentopname van de opslag](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 

>[!Note]
>Als de momentopname is teruggezet met Microsoft operations, moet u niet stap 7.


### <a name="recover-to-another-point-in-time"></a>Herstellen naar een ander punt in tijd
Als u wilt herstellen naar een bepaald punt in tijd, Zie 'Herstel de database naar het volgende punt in tijd' in [handmatig herstel uphandleiding voor SAP HANA op Azure vanuit een momentopname van de opslag](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 


## <a name="next-steps"></a>Volgende stappen
- Zie [Disaster recovery principes en voorbereiding](hana-concept-preparation.md).
