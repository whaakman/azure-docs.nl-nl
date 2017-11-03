---
title: StorSimple 8000-serie als back-updoel met NetBackup | Microsoft Docs
description: Beschrijft de configuratie van de back-updoel StorSimple met Veritas NetBackup.
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: hkanna
ms.openlocfilehash: b1878c181a77ac6d54654fc55228907743243c45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a>StorSimple als een back-updoel met NetBackup

## <a name="overview"></a>Overzicht

Azure StorSimple is een hybride cloud-opslag-oplossing van Microsoft. De complexiteit van de exponentiële Gegevensgroei adressen StorSimple met behulp van een Azure storage-account als een uitbreiding van de on-premises-oplossing en lagen automatisch gegevens over opslag voor lokale en cloud-opslag.

In dit artikel wordt besproken StorSimple-integratie met Veritas NetBackup en aanbevolen procedures voor het integreren van beide oplossingen. We ook aanbevelingen voor het instellen van Veritas NetBackup beste integreren met StorSimple. We stellen Veritas aanbevolen procedures voor back-architecten en administrators voor de beste manier Veritas NetBackup instellen om te voldoen aan de vereisten voor afzonderlijke back-up- en service level agreements (Sla's).

Hoewel we configuratiestappen en belangrijkste concepten illustreren, is in dit artikel geen stapsgewijze handleiding voor een configuratie of de installatie. Er wordt ervan uitgegaan dat de basisonderdelen van en de infrastructuur zijn en gereed voor de ondersteuning van de concepten die worden beschreven.

### <a name="who-should-read-this"></a>Wie is dit?

De informatie in dit artikel is vooral handig voor back-beheerders, opslagbeheerders en storage-architecten die kennis van opslag, Windows Server 2012 R2, Ethernet, cloudservices en Veritas NetBackup hebben.

### <a name="supported-versions"></a>Ondersteunde versies

-   NetBackup 7.7.x en latere versies
-   [StorSimple Update 3 en hoger](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Waarom StorSimple als een back-updoel?

StorSimple is een goede keuze voor een back-updoel omdat:

-   Het biedt standaard, lokale opslag voor back-uptoepassingen om te gebruiken als een snelle back-up bestemming, zonder deze te wijzigen. U kunt ook StorSimple gebruiken voor een snel herstel van recente back-ups.
-   De cloud tiering is volledig geïntegreerd met een Azure-cloud-opslagaccount rendabele Azure Storage gebruiken.
-   Het biedt automatisch offsite-opslag voor herstel na noodgevallen.

## <a name="key-concepts"></a>Belangrijkste concepten

Net als bij een opslagoplossing, een zorgvuldige evaluatie van de oplossing opslagprestaties, sla's, is de frequentie van wijzigings- en groei capaciteitsbehoeften essentieel is voor succes. Het belangrijkste idee is die door de introductie van een cloudlaag, uw toegangstijden en doorvoercapaciteit naar de cloud play een belangrijke rol in de mogelijkheid van StorSimple de taak uit te voeren.

StorSimple is ontworpen om opslag te bieden tot toepassingen die werken op een goed gedefinieerde werkset van gegevens (hot gegevens). In dit model, de werkset van gegevens wordt opgeslagen op de lokale lagen en de resterende vrije, koude/gearchiveerd set gegevens naar de cloud is gelaagd. Dit model wordt vertegenwoordigd in de volgende afbeelding. Bijna platte groene lijn vertegenwoordigt de gegevens die zijn opgeslagen op de lokale lagen van het StorSimple-apparaat. De rode lijn vertegenwoordigt de totale hoeveelheid gegevens die zijn opgeslagen op de StorSimple-oplossing in alle lagen. De ruimte tussen de platte groene lijn en de exponentiële rode curve vertegenwoordigt de totale hoeveelheid gegevens die zijn opgeslagen in de cloud.

**StorSimple tiering**
![lagen StorSimple-diagram](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)

Met deze architectuur rekening zult u merken dat StorSimple is ideaal als een back-updoel wilt gebruiken. U kunt StorSimple om te gebruiken:
-   De meest voorkomende herstelacties uitvoeren vanaf de lokale werkset van gegevens.
-   Gebruik de cloud voor noodherstel offsite en oudere gegevens waar herstelacties minder frequente zijn.

## <a name="storsimple-benefits"></a>Voordelen van StorSimple

StorSimple biedt een on-premises-oplossing is volledig geïntegreerd met Microsoft Azure, door gebruik te maken van naadloze toegang tot on-premises en in de cloud-opslag.

StorSimple maakt gebruik van automatische lagen tussen de on-premises-apparaat met SSD-apparaat (SSD) en serial attached SCSI (SAS)-opslag en Azure Storage. Automatische lagen, houdt vaak gebruikte gegevens lokaal is, op de SSD- en SAS-lagen. Deze verplaatst minder vaak gebruikte gegevens naar Azure Storage.

StorSimple biedt deze voordelen:

-   Unieke gegevensontdubbeling en compressie-algoritmen die ongekende Ontdubbeling niveaus kunt gebruiken voor de cloud
-   Hoge beschikbaarheid
-   Geo-replicatie met behulp van Azure geo-replicatie
-   Integratie van Azure
-   Versleuteling van gegevens in de cloud
-   Verbeterde noodherstel en naleving

Hoewel StorSimple fundamenteel geeft de twee belangrijkste implementatiescenario's (back-updoel primaire en secundaire back-updoel), is het een gewone, apparaat met blokopslag. StorSimple biedt de compressie en Ontdubbeling. Naadloos verzendt en haalt gegevens tussen de cloud en de toepassing en het bestandssysteem.

Zie voor meer informatie over StorSimple [StorSimple 8000-serie: hybride cloud opslagoplossing](storsimple-overview.md). U kunt ook controleren de [technische specificaties van StorSimple 8000 serie](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Met behulp van een StorSimple wordt apparaat als een back-updoel alleen ondersteund voor StorSimple 8000 Update 3 en hoger.

## <a name="architecture-overview"></a>Overzicht van de architectuur

De volgende tabellen geven de initiële richtlijnen voor apparaat-model voor architectuur.

**StorSimple-capaciteit voor lokale en cloud-opslag**

| Opslagcapaciteit       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Capaciteit van de lokale opslag | &lt;10 TiB\*  | &lt;20 TiB\*  |
| Cloudopslagcapaciteit | &gt;200 TiB\* | &gt;500 TiB\* |
\*Grootte van de opslagruimte wordt ervan uitgegaan dat er geen gegevensontdubbeling of compressie.

**StorSimple capaciteitswaarden voor de primaire en secundaire back-ups**

| Back-scenario  | Capaciteit van de lokale opslag  | Cloudopslagcapaciteit  |
|---|---|---|
| Primaire back-up  | Recente back-ups opgeslagen op lokale opslag op snel herstel om te voldoen aan de beoogde herstelpunt (RPO) | Back-upgeschiedenis (RPO) past in cloudcapaciteit |
| Secundaire back-up | Secundaire kopie van de back-upgegevens kan worden opgeslagen in de cloudcapaciteit  | N.v.t.  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple als primaire back-doel

In dit scenario wordt worden aan de back-uptoepassing als de enige opslagplaats voor back-ups StorSimple-volumes gepresenteerd. De volgende afbeelding toont de oplossingsarchitectuur van een waarin alle back-ups gebruik StorSimple volumes voor back-ups en herstelbewerkingen gelaagde.

![StorSimple als een primaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de back-upagent brengt gegevens over naar de back-upserver.
2.  De back-upserver schrijft gegevens naar de StorSimple gelaagde volumes.
3.  De back-upserver werkt de catalogusdatabase en vervolgens de back-uptaak voltooid.
4.  Een script momentopname activeert StorSimple snapshot manager (start of verwijderen).
5.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="primary-target-restore-logical-steps"></a>Primaire doel terugzetten logische stappen

1.  De back-upserver wordt gestart bij het herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver voltooit de hersteltaak.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als secundaire back-doel

In dit scenario worden StorSimple-volumes voornamelijk gebruikt voor lange bewaartermijn en archiveren.

De volgende afbeelding toont een architectuur in welke eerste back-ups en een hoge prestaties doelvolume hersteld. Deze back-ups zijn gekopieerd en wordt gearchiveerd naar een StorSimple gelaagd volume op een vast schema.

Het is belangrijk grootte van het volume met hoge prestaties, zodat deze kan omgaan met uw vereisten bewaren beleid capaciteit en prestaties.

![StorSimple als een secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Back-up logische stappen secundaire doel

1.  De back-upserver neemt contact op met de back-upagent doel en de back-upagent brengt gegevens over naar de back-upserver.
2.  De back-upserver schrijft gegevens naar krachtige opslag.
3.  De back-upserver werkt de catalogusdatabase en vervolgens de back-uptaak voltooid.
4.  De back-upserver back-ups naar StorSimple op basis van een bewaarbeleid gekopieerd.
5.  Een script momentopname activeert StorSimple snapshot manager (start of verwijderen).
6.  De back-upserver worden de verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="secondary-target-restore-logical-steps"></a>Secundaire doel terugzetten logische stappen

1.  De back-upserver wordt gestart bij het herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver voltooit de hersteltaak.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Implementatie van deze oplossing vereist drie stappen:
1. Bereid de netwerkinfrastructuur.
2. Uw StorSimple-apparaat implementeren als een back-doel.
3. Veritas NetBackup implementeren.

Elke stap wordt uitgebreid beschreven in de volgende secties.

### <a name="set-up-the-network"></a>Instellen van het netwerk

Omdat StorSimple een oplossing die geïntegreerd met de Azure-cloud is, vereist StorSimple een actieve verbinding met de Azure-cloud. Deze verbinding wordt gebruikt voor bewerkingen, zoals cloudmomentopnamen, het beheer van gegevens en metagegevens overbrengen en naar de laag die ouder zijn, kleiner gebruikte gegevens naar Azure-cloudopslag.

Voor de oplossing optimaal te raden we aan dat u deze netwerken aanbevolen procedures volgt:

-   De koppeling die verbinding maakt de StorSimple tiering naar Azure moet voldoen aan de bandbreedtevereisten van uw. Om dit te bereiken, het juiste beveiligingsniveau voor Quality of Service (QoS) van toepassing op uw infrastructuur switches moeten overeenkomen met uw RPO en herstel tijd objective (RTO) Sla's.

-   Maximale toegangslatentie van Azure Blob-opslag moet ongeveer 80 ms.

### <a name="deploy-storsimple"></a>StorSimple implementeren

Zie voor stapsgewijze instructies voor StorSimple-implementatie, [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-netbackup"></a>NetBackup implementeren

Zie voor stapsgewijze instructies NetBackup 7.7.x voor implementatie, de [NetBackup 7.7.x documentatie](http://www.veritas.com/docs/000094423).

## <a name="set-up-the-solution"></a>Instellen van de oplossing

In deze sectie ziet u configuratievoorbeelden. De volgende voorbeelden en aanbevelingen illustratie van de meest eenvoudige en fundamentele-implementatie. Deze implementatie mogelijk niet rechtstreeks toepassen op uw specifieke vereisten van de back-up.

### <a name="set-up-storsimple"></a>StorSimple instellen

| Implementatietaken StorSimple  | Aanvullende opmerkingen |
|---|---|
| Uw on-premises StorSimple-apparaat implementeren. | Ondersteunde versies: Update 3 en latere versies. |
| Schakel in het back-updoel. | Deze opdrachten gebruiken in-of back-updoel modus uit te schakelen en status ophalen. Zie voor meer informatie [op afstand verbinding maken met een StorSimple-apparaat](storsimple-remote-connect.md).</br> Back-upmodus inschakelen: `Set-HCSBackupApplianceMode -enable`. </br> Back-modus uit te schakelen: `Set-HCSBackupApplianceMode -disable`. </br> Ophalen van de huidige status van de instellingen van de back-upmodus: `Get-HCSBackupApplianceMode`. |
| Maak een algemene volumecontainer voor het volume waarop de back-gegevens worden opgeslagen. Alle gegevens in een volumecontainer is ontdubbeld. | StorSimple volumecontainers definiëren Ontdubbeling domeinen.  |
| StorSimple-volumes maken. | Volumes met grootten als dicht bij het verwachte gebruik maken mogelijk, omdat de grootte van volume is van invloed op een momentopname van de opgegeven tijdsduur cloud. Lees voor informatie over het formaat van een volume over [bewaarbeleidsregels](#retention-policies).</br> </br> Gebruik StorSimple gelaagde volumes en selecteert u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje. </br> Gebruik alleen lokaal vastgemaakte volumes wordt niet ondersteund. |
| Een unieke StorSimple-back-upbeleid voor alle back-updoel volumes maken. | Een StorSimple-back-upbeleid definieert de groep van de consistentie volume. |
| De planning uitschakelen wanneer de momentopnamen verlopen. | Momentopnamen worden geactiveerd als een na verwerking bewerking. |

### <a name="set-up-the-host-backup-server-storage"></a>De opslag van de back-upserver host instellen

Stelt u de opslag van de back-upserver host volgens deze richtlijnen:  

- Gebruik geen spanned volumes (gemaakt door Windows Schijfbeheer;) spanned volumes worden niet ondersteund.
- De volumes formatteren met NTFS met een toewijzingsgrootte van 64 KB.
- De StorSimple-volumes rechtstreeks naar de server NetBackup toewijzen.
    - ISCSI-gebruiken voor fysieke servers.
    - Passthrough-schijven voor virtuele servers gebruiken.


## <a name="best-practices-for-storsimple-and-netbackup"></a>Aanbevolen procedures voor StorSimple en NetBackup

Instellen van uw oplossing volgens de richtlijnen in de volgende secties.

### <a name="operating-system-best-practices"></a>Aanbevolen procedures van besturingssysteem

-   Windows Server-versleuteling en Ontdubbeling voor het NTFS-bestandssysteem uitgeschakeld.
-   Schakel defragmentatie van Windows Server op de StorSimple-volumes.
-   Schakel Windows Server op de StorSimple-volumes te indexeren.
-   Een antivirusprogramma scan uitvoeren op de bronhost (niet op de StorSimple-volumes).
-   De standaardwaarde uitschakelen [onderhoud van Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) in Taakbeheer. Dit op een van de volgende manieren doen:
    - De configurator onderhoud in Windows Taakplanner uitschakelen.
    - Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) van Windows Sysinternals. Na het downloaden van PsExec Voer Windows PowerShell als beheerder en typ:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>Aanbevolen procedures voor StorSimple

-   Zorg dat het StorSimple-apparaat is bijgewerkt naar [Update 3 of hoger](storsimple-install-update-3.md).
-   Isoleren iSCSI- en cloud-verkeer. Speciale iSCSI-verbindingen gebruiken voor verkeer tussen StorSimple en de back-upserver.
-   Zorg ervoor dat uw StorSimple-apparaat een toegewezen back-doel is. Gemengde werkbelastingen worden niet ondersteund omdat ze invloed heeft op uw RTO en RPO.

### <a name="netbackup-best-practices"></a>Aanbevolen procedures NetBackup

-   De database NetBackup moet lokaal op de server en bevindt zich niet op een StorSimple-volume.
-   Voor herstel na noodgevallen, back-up de NetBackup-database op een StorSimple-volume.
-   We NetBackup volledige en incrementele back-ups ondersteunen (ook wel differentiële incrementele back-ups in NetBackup genoemd) voor deze oplossing. Het is raadzaam dat u geen synthetische cumulatieve incrementele back-ups en gebruikt.
-   Back-upgegevensbestanden moeten alleen de gegevens voor een specifieke taak bevatten. Bijvoorbeeld geen media worden toegevoegd via verschillende taken zijn toegestaan.

Voor de meest recente NetBackup-instellingen en aanbevolen procedures voor het implementeren van deze vereisten, Zie de documentatie bij NetBackup [www.veritas.com](https://www.veritas.com).


## <a name="retention-policies"></a>Bewaarbeleid

Een van de meest voorkomende typen bewaren van back-up is een beleid opa vader en zoon (algemene). Een incrementele back-up dagelijks wordt uitgevoerd in een algemene-beleid en volledige back-ups wekelijkse en maandelijkse klaar bent. Dit beleid resulteert in zes StorSimple gelaagde volumes: één volume bevat de wekelijkse, maandelijkse en jaarlijkse volledige back-ups; de vijf volumes opslaan dagelijkse incrementele back-ups.

In het volgende voorbeeld gebruiken we een algemene worden gedraaid. In het voorbeeld wordt ervan uitgegaan:

-   Niet-ontdubbelde of gecomprimeerde gegevens worden gebruikt.
-   Volledige back-ups zijn 1 TiB.
-   Dagelijkse incrementele back-ups zijn 500 GiB.
-   Vier wekelijkse back-ups worden gedurende een maand bewaard.
-   Twaalf maandelijkse back-ups worden van een jaar bewaard.
-   Een jaarlijkse back-up is 10 jaar bewaard.

Op basis van de voorgaande veronderstellingen kunt maken van een 26-TiB StorSimple gelaagd volume voor de maandelijkse en jaarlijkse volledige back-ups. Maken van een 5-TiB StorSimple gelaagd volume voor elk van de incrementele dagelijkse back-ups.

| Back-uptype bewaren | Grootte (TiB) | Algemene vermenigvuldiger\* | Totale capaciteit (TiB)  |
|---|---|---|---|
| Wekelijkse volledige | 1 | 4  | 4 |
| Dagelijkse incrementele | 0.5 | 20 (cycli gelijk aantal weken per maand) | 12 (2 voor extra quotum) |
| Maandelijks volledige | 1 | 12 | 12 |
| Jaarlijks volledige | 1  | 10 | 10 |
| Algemene vereisten |   | 38 |   |
| Extra quota  | 4  |   | 42 totale algemene vereisten  |
\*De vermenigvuldigingsfactor algemene is het aantal exemplaren dat u wilt beveiligen en om te voldoen aan de vereisten van uw back-upbeleid behouden.

## <a name="set-up-netbackup-storage"></a>NetBackup opslag instellen

### <a name="to-set-up-netbackup-storage"></a>NetBackup opslag instellen

1.  Selecteer in de beheerconsole NetBackup **Media- en Apparaatbeheer** > **apparaten** > **schijfgroepen**. Selecteer het opslagtype voor de server in de configuratiewizard van schijf groep **AdvancedDisk**, en selecteer vervolgens **volgende**.

    ![NetBackup-beheerconsole schijf Pool-configuratiewizard](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  Selecteer uw server en selecteer vervolgens **volgende**.

    ![NetBackup-beheerconsole, selecteer de server](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  Selecteer uw StorSimple-volume.

    ![NetBackup-beheerconsole, selecteer de schijf StorSimple-volume](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  Voer een naam voor het back-updoel en selecteer vervolgens **volgende** > **volgende** om de wizard te voltooien.

5.  Controleer de instellingen en selecteer vervolgens **voltooien**.

6.  Aan het einde van elke toewijzing volume, wijzig de instellingen voor het apparaat van opslag zodat deze overeenkomen met de aanbevolen [aanbevolen procedures voor StorSimple en NetBackup](#best-practices-for-storsimple-and-netbackup).

7. Herhaal stap 1-6 totdat u bent klaar voor het toewijzen van uw StorSimple-volumes.

    ![NetBackup-beheerconsole schijfconfiguratie](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple instellen als primaire back-doel

> [!NOTE]
> Gegevens terugzetten vanuit een back-up die naar de cloud is gelaagde optreden met een snelheid van de cloud.

De volgende afbeelding ziet de toewijzing van een typische volume aan een back-uptaak. In dit geval worden alle wekelijkse back-ups worden toegewezen aan de volledige schijf zaterdag en de incrementele back-ups worden toegewezen aan incrementele schijven van maandag tot vrijdag. Alle back-ups en herstelbewerkingen van een StorSimple zijn gelaagd volume.

![Primaire back-updoel configuratie logisch diagram ](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple als primaire back-updoel algemene voorbeeld plannen

Hier volgt een voorbeeld van een algemene rotatieschema vier weken, maandelijkse en jaarlijkse:

| Type frequentie/back-up | Volledig | Incrementele (dagen 1-5)  |   
|---|---|---|
| Wekelijks (weken 1-4) | Zaterdag | Maandag tot vrijdag |
| Maandelijks  | Zaterdag  |   |
| Jaarlijks | Zaterdag  |   |   |

## <a name="assigning-storsimple-volumes-to-a-netbackup-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak NetBackup

De volgende procedure wordt ervan uitgegaan dat NetBackup en de doelhost zijn geconfigureerd in overeenstemming met de agent NetBackup richtlijnen.

### <a name="to-assign-storsimple-volumes-to-a-netbackup-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak NetBackup

1.  Selecteer in de beheerconsole NetBackup **NetBackup Management**, met de rechtermuisknop op **beleid**, en selecteer vervolgens **nieuw beleid**.

    ![NetBackup Administration Console, een nieuw beleid maken](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2.  In de **toevoegen van een nieuw beleid** in het dialoogvenster een naam voor het beleid en selecteer vervolgens de **gebruik beleid configuratiewizard** selectievakje. Selecteer **OK**.

    ![NetBackup-beheerconsole een dialoogvenster voor nieuw beleid toevoegen](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3.  In de Wizard Back-beleid configureren voor het back-uptype dat u wilt en selecteer vervolgens kiezen **volgende**.

    ![NetBackup-beheerconsole, Selecteer type back-up](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4.  Selecteer het beleidstype stelt **standaard**, en selecteer vervolgens **volgende**.

    ![NetBackup-beheerconsole, selecteer beleidstype](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5.  Selecteer de host, selecteert u de **client-besturingssysteem detecteren** selectievakje en selecteer vervolgens **toevoegen**. Selecteer **volgende**.

    ![NetBackup-beheerconsole, lijst met clients in een nieuw beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6.  Selecteer de stations die u back wilt-up.

    ![NetBackup-beheerconsole, back-up van de selecties voor een nieuw beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7.  Selecteer de frequentie en het bewaren van waarden die voldoen aan de vereisten van uw back-up worden gedraaid.

    ![NetBackup-beheerconsole, back-upfrequentie en de rotatie voor een nieuw beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8.  Selecteer **volgende** > **volgende** > **voltooien**.  U kunt het schema wijzigen nadat het beleid is gemaakt.

9.  Selecteer voor het uitbreiden van het beleid dat u zojuist hebt gemaakt en selecteer vervolgens **planningen**.

    ![NetBackup-beheerconsole, schema's voor een nieuw beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10.  Met de rechtermuisknop op **differentiële Inc**, selecteer **kopiëren naar nieuwe**, en selecteer vervolgens **OK**.

    ![NetBackup-beheerconsole, schema voor het kopiëren naar een nieuw beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11.  Met de rechtermuisknop op de zojuist gemaakte planning en selecteer vervolgens **wijziging**.

12.  Op de **kenmerken** tabblad de **beleid Opslagselectie overschrijven** uit en selecteer vervolgens het volume waar maandag incrementele back-ups gebleven.

    ![NetBackup-beheerconsole, schema wijzigen](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13.  Op de **venster Start** tabblad, selecteert u het tijdvenster voor uw back-ups.

    ![NetBackup-beheerconsole start wijzigingsvenster](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14.  Selecteer **OK**.

15.  Herhaal stap 10-14 voor elke incrementele back-up. Selecteer de relevante volume en de planning voor elke back-up die u maakt.

16.  Met de rechtermuisknop op de **differentiële Inc** plannen en vervolgens te verwijderen.

17.  Wijzig de volledige schema om te voldoen aan de behoeften van uw back-up.

    ![NetBackup-beheerconsole, volledige schema wijzigen](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18.  Het startvenster wijzigen.

    ![NetBackup-beheerconsole, het startvenster wijzigen](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19.  De laatste planning ziet er als volgt:

    ![NetBackup-beheerconsole, laatste planning](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple instellen als een secundaire back-doel

> [!NOTE]
>Gegevens terugzetten vanuit een back-up die naar de cloud is gelaagde optreden met een snelheid van de cloud.

In dit model, moet u een opslagmedium (met uitzondering van StorSimple) hebben om te fungeren als een tijdelijke cache. Bijvoorbeeld, kunt u een redundante matrix van onafhankelijke schijven (RAID) volume aan schijfruimte, input/output (I/O) en de bandbreedte. U wordt aangeraden met behulp van RAID-5, 50 en 10.

De volgende afbeelding toont typisch kortetermijnback bewaren (naar de server) lokale volumes en lange bewaartermijn archiveert volumes. In dit scenario worden alle back-ups uitvoeren op de lokale (naar de server) RAID-volume. Deze back-ups worden regelmatig gedupliceerd en gearchiveerd op een volume archieven. Het is belangrijk om de grootte van uw lokale (naar de server) RAID-volume, zodat deze kan omgaan met uw op korte termijn capaciteit en prestaties vereisten voor de bewaarperiode.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple als een voorbeeld van een secundaire back-updoel algemene

![StorSimple als een secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

De volgende tabel ziet het instellen van back-ups worden uitgevoerd op de lokale en het StorSimple-schijven. Bevat capaciteitsvereisten voor afzonderlijke en de totale.

### <a name="backup-configuration-and-capacity-requirements"></a>Back-upconfiguratie en capaciteitsvereisten van opslaggroepen

| Type back-up en retentie | Geconfigureerde opslag | Grootte (TiB) | Algemene vermenigvuldiger | Totale capaciteit\* (TiB) |
|---|---|---|---|---|
| Week 1 (volledige en incrementele) |Lokale schijf (korte)| 1 | 1 | 1 |
| StorSimple weken 2-4 |StorSimple schijf (op lange termijn) | 1 | 4 | 4 |
| Maandelijks volledige |StorSimple schijf (op lange termijn) | 1 | 12 | 12 |
| Jaarlijks volledige |StorSimple schijf (op lange termijn) | 1 | 1 | 1 |
|Vereiste grootte van algemene volumes |  |  |  | 18*|
\*Totale capaciteit omvat 17 TiB van StorSimple-schijven en 1 TiB van lokale RAID-volume.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>Algemene voorbeeld planning: algemene rotatie wekelijkse, maandelijkse en jaarlijkse planning

| Week | Volledig | Incrementele dag 1 | Incrementele dag 2 | Incrementele dag 3 | Incrementele dag 4 | Incrementele dag 5 |
|---|---|---|---|---|---|---|
| 1 week | Lokale RAID-volume  | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume |
| Week 2 | StorSimple weken 2-4 |   |   |   |   |   |
| Week 3 | StorSimple weken 2-4 |   |   |   |   |   |
| Week 4 | StorSimple weken 2-4 |   |   |   |   |   |
| Maandelijks | Maandelijks StorSimple |   |   |   |   |   |
| Jaarlijks | Jaarlijks StorSimple  |   |   |   |   |   |   |


## <a name="assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple-volumes toewijzen aan een NetBackup archief en duplicatie taak

Omdat NetBackup tal van opties voor opslag en media management biedt, wordt u aangeraden dat u contact opneemt met Veritas of uw systeemarchitect NetBackup om uw opslagvereisten lifecycle-beleid (SLP) goed vast te stellen.

Nadat de eerste schijf van toepassingen die u hebt gedefinieerd, moet u drie extra opslagruimte lifecycle-beleid, voor een totaal van vier beleidsregels definiëren:
* LocalRAIDVolume
* StorSimpleWeek2 4
* StorSimpleMonthlyFulls
* StorSimpleYearlyFulls

### <a name="to-assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple-volumes toewijzen aan een NetBackup archief en duplicatie taak

1.  Selecteer in de beheerconsole NetBackup **opslag** > **Lifecycle-beleid voor opslag** > **Lifecycle-beleid voor nieuwe opslag**.

    ![NetBackup-beheerconsole, nieuwe opslag lifecycle-beleid](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2.  Voer een naam voor de momentopname en selecteer vervolgens **toevoegen**.

3.  In de **nieuwe bewerking** in het dialoogvenster op de **eigenschappen** tabblad voor **bewerking**, selecteer **back-up**. Selecteer de gewenste waarden voor **doelopslagaccount**, **bewaren type**, en **bewaarperiode**. Selecteer **OK**.

    ![NetBackup Administration Console, in het dialoogvenster voor nieuwe bewerking](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

    Hiermee definieert u de eerste back-upbewerking en de opslagplaats.

4.  Selecteer op de vorige bewerking markeren en selecteer vervolgens **toevoegen**. In de **wijziging opslagbewerking** dialoogvenster Selecteer de gewenste waarden voor **doelopslagaccount**, **bewaren type**, en **bewaarperiode**.

    ![NetBackup Administration Console, in het dialoogvenster van de opslagbewerking wijzigen](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5.  Selecteer op de vorige bewerking markeren en selecteer vervolgens **toevoegen**. In de **Lifecycle-beleid voor nieuwe opslag** dialoogvenster Voeg maandelijkse back-ups van een jaar.

    ![NetBackup Administration Console, in het dialoogvenster voor nieuw beleid voor opslag levenscyclus](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6.  Herhaal stap 4 en 5 totdat u de uitgebreide SLP bewaarbeleid die u nodig hebt gemaakt.

    ![NetBackup-beheerconsole, beleid toevoegen in het dialoogvenster Nieuw Lifecycle-beleid voor opslag](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7.  Als u hebt onder uw bewaarbeleid SLP definiëren **beleid**, een back-upbeleid definiëren de stappen uiteengezet in [toewijzen StorSimple-volumes naar een back-uptaak NetBackup](#assigning-storsimple-volumes-to-a-netbackup-backup-job).

8.  Onder **planningen**, in de **schema wijzigen** in het dialoogvenster met de rechtermuisknop op **volledige**, en selecteer vervolgens **wijziging**.

    ![NetBackup-beheerconsole, het dialoogvenster schema wijzigen](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9.  Selecteer de **beleid Opslagselectie overschrijven** uit en selecteer vervolgens het bewaarbeleid SLP die u hebt gemaakt in stap 1-6.

    ![NetBackup-beheerconsole overschrijven beleid Opslagselectie](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10.  Selecteer **OK**, en Herhaal voor incrementele back-upschema.

    ![NetBackup Administration Console, in het dialoogvenster schema wijzigen voor incrementele back-ups](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| Back-uptype bewaren | Grootte (TiB) | Algemene vermenigvuldiger\* | Totale capaciteit (TiB)  |
|---|---|---|---|
| Wekelijkse volledige |  1  |  4 | 4  |
| Dagelijkse incrementele  | 0.5  | 20 (cycli gelijk zijn aan het aantal weken per maand) | 12 (2 voor extra quotum) |
| Maandelijks volledige  | 1 | 12 | 12 |
| Jaarlijks volledige | 1  | 10 | 10 |
| Algemene vereisten  |     |     | 38 |
| Extra quota  | 4  |    | 42 totale algemene vereisten |
\*De vermenigvuldigingsfactor algemene is het aantal exemplaren dat u wilt beveiligen en om te voldoen aan de vereisten van uw back-upbeleid behouden.

## <a name="storsimple-cloud-snapshots"></a>De cloudmomentopnamen StorSimple

De cloudmomentopnamen StorSimple Beveilig de gegevens die zich in uw StorSimple-apparaat bevindt. Maken van een cloudmomentopname is gelijk aan het lokale back-uptapes naar een faciliteit off-site back-upfunctie. Als u Azure geografisch redundante opslag gebruikt, is het maken van een cloudmomentopname gelijk aan het back-upfunctie back-uptapes op meerdere sites zijn. Als u herstellen van een apparaat na een noodgeval wilt, kunt u mogelijk een andere StorSimple-apparaat online brengen en voer een failover. U zou toegang kunnen krijgen tot de gegevens (snelheden cloud) van de meest recente cloudmomentopname zijn na de failover.

De volgende sectie wordt beschreven hoe een korte script maken om te beginnen en cloudmomentopnamen StorSimple tijdens de back-up naverwerking verwijderen.

> [!NOTE]
> Momentopnamen die handmatig of programmatisch gemaakt niet op het verloopbeleid van StorSimple snapshot. Deze momentopnamen moeten handmatig of via een programma verwijderen.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Start en cloudmomentopnamen verwijderen met behulp van een script

> [!NOTE]
> Evalueer zorgvuldig de naleving en gegevens bewaren weerslag voordat u een momentopname van een StorSimple verwijderen. Zie voor meer informatie over het uitvoeren van een script dat na het [NetBackup documentatie](http://www.veritas.com/docs/000094423).

### <a name="backup-lifecycle"></a>De levenscyclus van de back-up

![Back-Lifecycle-diagram](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

### <a name="requirements"></a>Vereisten

-   De server waarop het script wordt uitgevoerd moet toegang hebben tot Azure-cloud-bronnen.
-   Het gebruikersaccount moet de juiste machtigingen hebben.
-   Een StorSimple-back-upbeleid met de bijbehorende StorSimple-volumes moet instellen maar niet is ingeschakeld.
-   U moet de naam van de StorSimple-bron, registratiesleutel, apparaat-id op naam en een back-beleid.

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Om te starten of een cloudmomentopname verwijderen

1.  [Installeer Azure PowerShell](/powershell/azure/overview).
2. Downloaden en installeren [beheren CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell-script.
3. Voer op de server waarop het script wordt uitgevoerd, PowerShell als beheerder. Zorg ervoor dat u het script uitvoert `-WhatIf $true` om te zien welke wijzigingen van het script maakt. Nadat de validatie voltooid is, geeft `-WhatIf $false`. Voer de onderstaande opdracht:
```powershell
.\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
```
4.  Het script toevoegen aan de back-uptaak in NetBackup. U doet dit door uw NetBackup taak options vooraf verwerken en bewerken na verwerking van opdrachten.

> [!NOTE]
> Het is raadzaam dat u uw back-upbeleid voor StorSimple cloud momentopname als een script dat na verwerking aan het einde van de dagelijkse back-uptaak uitgevoerd. Voor meer informatie over het back-up en herstellen van uw back-uptoepassing omgeving om te voldoen aan de RPO en RTO Neem contact op met uw back-architect.

## <a name="storsimple-as-a-restore-source"></a>StorSimple als een bron terugzetten

Herstelt vanuit een StorSimple-apparaat werk zoals herstelt vanaf elk apparaat met blokopslag. Herstellen van gegevens die naar de cloud is gelaagd vindt plaats met een snelheid van de cloud. Voor lokale gegevens voorkomen herstelacties op de snelheid van de lokale schijf van het apparaat. Zie voor meer informatie over het uitvoeren van een terugzetbewerking de [NetBackup documentatie](http://www.veritas.com/docs/000094423). Het is raadzaam dat u aan de aanbevolen procedures voor NetBackup terugzetten voldoet.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover en herstel na noodgevallen

> [!NOTE]
> Voor scenario's met back-updoel wordt StorSimple Cloud toestel niet ondersteund als een doel voor herstel.

Een noodgeval kan zijn veroorzaakt door een verscheidenheid aan factoren. De volgende tabel bevat algemene herstel na noodgevallen.

| Scenario | Impact | Herstellen | Opmerkingen |
|---|---|---|---|
| Fout met StorSimple | Back-up en herstelbewerkingen zijn onderbroken. | Vervang het mislukte apparaat en uit te voeren [StorSimple failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md). | Als u een herstelbewerking na het herstel van apparaat moet, worden volledige gegevens wordt opgehaald uit de cloud naar het nieuwe apparaat. Alle bewerkingen zijn met een snelheid van de cloud. De index en de catalogus opnieuw scannen proces mogelijk te worden gecontroleerd en opgehaald uit de cloudlaag naar het lokale apparaat-laag kan een tijdrovend proces zijn alle back-upsets. |
| Serverfout NetBackup | Back-up en herstelbewerkingen zijn onderbroken. | De back-upserver bouwen en uitvoeren database terugzetten. | U moet opnieuw samenstellen of herstellen van de server NetBackup op de site van de herstel na noodgevallen. De database herstellen naar de meest recente. Als de herstelde NetBackup-database niet gesynchroniseerd met de meest recente back-uptaken is, is indexeren en catalogiseren vereist. Deze index en de catalogus opnieuw scannen proces mogelijk alle back-upsets worden gescand en opgehaald uit de cloudlaag aan de laag van het lokale apparaat. Dit maakt het verdere tijdrovende. |
| Fout bij de site die in het verlies van de back-upserver en de StorSimple resulteert | Back-up en herstelbewerkingen zijn onderbroken. | StorSimple eerst herstellen en herstel vervolgens NetBackup. | StorSimple eerst herstellen en herstel vervolgens NetBackup. Als u een herstelbewerking na het herstel van apparaat moet, worden de volledige gegevenssets werken vanuit de cloud naar het nieuwe apparaat opgehaald. Alle bewerkingen zijn met een snelheid van de cloud. |

## <a name="references"></a>Verwijzingen

De volgende documenten zijn waarnaar wordt verwezen voor dit artikel:

- [StorSimple multipath i/o-installatie](storsimple-configure-mpio-windows-server.md)
- [Scenario's voor opslag: Thin provisioning](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Met behulp van GPT-schijven](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Schaduwkopieën van gedeelde mappen instellen](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [terugzetten vanuit een back-upset](storsimple-restore-from-backup-set-u2.md).
- Meer informatie over het uitvoeren van [apparaat failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md).
