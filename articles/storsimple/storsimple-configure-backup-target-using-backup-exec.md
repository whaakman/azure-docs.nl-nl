---
title: StorSimple 8000-serie als back-updoel met Backup Exec | Microsoft Docs
description: Beschrijft de configuratie van de back-updoel StorSimple met Veritas Backup Exec.
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
ms.date: 12/05/2016
ms.author: hkanna
ms.openlocfilehash: a28b46e10bbdd5331cc665fad3f80523b3aa8a58
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>StorSimple als een back-updoel met Backup Exec

## <a name="overview"></a>Overzicht

Azure StorSimple is een hybride cloud-opslag-oplossing van Microsoft. De complexiteit van de exponentiële Gegevensgroei adressen StorSimple met behulp van een Azure storage-account als een uitbreiding van de on-premises-oplossing en lagen automatisch gegevens over opslag voor lokale en cloud-opslag.

In dit artikel wordt besproken StorSimple-integratie met Veritas Backup Exec en aanbevolen procedures voor het integreren van beide oplossingen. We ook aanbevelingen voor het instellen van back-up Exec beste integreren met StorSimple. We stellen Veritas aanbevolen procedures voor back-architecten en administrators voor de beste manier om back-up Exec instellen om te voldoen aan de vereisten voor afzonderlijke back-up- en service level agreements (Sla's).

Hoewel we configuratiestappen en belangrijkste concepten illustreren, is in dit artikel geen stapsgewijze handleiding voor een configuratie of de installatie. Er wordt ervan uitgegaan dat de basisonderdelen van en de infrastructuur zijn en gereed voor de ondersteuning van de concepten die worden beschreven.

### <a name="who-should-read-this"></a>Wie is dit?

De informatie in dit artikel is zeer geschikt om back-beheerders, opslagbeheerders en storage-architecten die kennis van opslag, Windows Server 2012 R2, Ethernet, cloudservices en Backup Exec hebben.

## <a name="supported-versions"></a>Ondersteunde versies

-   [Back-up uitvoeren / 16 en latere versies](http://backupexec.com/compatibility)
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
![lagen StorSimple-diagram](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

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

![StorSimple als een primaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de back-upagent brengt gegevens over naar de back-upserver.
2.  De back-upserver schrijft gegevens naar de StorSimple gelaagde volumes.
3.  De back-upserver werkt de catalogusdatabase en vervolgens de back-uptaak voltooid.
4.  Een script momentopname activeert StorSimple cloud snapshot manager (start of verwijderen).
5.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.


### <a name="primary-target-restore-logical-steps"></a>Primaire doel terugzetten logische stappen

1.  De back-upserver wordt gestart bij het herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver voltooit de hersteltaak.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als secundaire back-doel

In dit scenario worden StorSimple-volumes voornamelijk gebruikt voor lange bewaartermijn en archiveren.

De volgende afbeelding toont een architectuur in welke eerste back-ups en een hoge prestaties doelvolume hersteld. Deze back-ups zijn gekopieerd en wordt gearchiveerd naar een StorSimple gelaagd volume op een vast schema.

Het is belangrijk grootte van het volume met hoge prestaties, zodat deze kan omgaan met uw vereisten bewaren beleid capaciteit en prestaties.

![StorSimple als een secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Back-up logische stappen secundaire doel

1.  De back-upserver neemt contact op met de back-upagent doel en de back-upagent brengt gegevens over naar de back-upserver.
2.  De back-upserver schrijft gegevens naar krachtige opslag.
3.  De back-upserver werkt de catalogusdatabase en vervolgens de back-uptaak voltooid.
4.  De back-upserver back-ups naar StorSimple op basis van een bewaarbeleid gekopieerd.
5.  Een script momentopname activeert StorSimple cloud snapshot manager (start of verwijderen).
6.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="secondary-target-restore-logical-steps"></a>Secundaire doel terugzetten logische stappen

1.  De back-upserver wordt gestart bij het herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver voltooit de hersteltaak.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Implementatie van de oplossing moet drie stappen:
1. Bereid de netwerkinfrastructuur.
2. Uw StorSimple-apparaat implementeren als een back-doel.
3. Back-up Exec implementeren.

Elke stap wordt uitgebreid beschreven in de volgende secties.

### <a name="set-up-the-network"></a>Instellen van het netwerk

Omdat StorSimple een oplossing die geïntegreerd met de Azure-cloud is, vereist StorSimple een actieve verbinding met de Azure-cloud. Deze verbinding wordt gebruikt voor bewerkingen, zoals cloudmomentopnamen, beheer en overdracht van de metagegevens van en naar de laag die ouder zijn, kleiner gebruikte gegevens naar Azure-cloudopslag.

Voor de oplossing optimaal te raden we aan dat u deze netwerken aanbevolen procedures volgt:

-   De koppeling die verbinding maakt van uw StorSimple tiering naar Azure moet voldoen aan de bandbreedtevereisten van uw. Om dit te bereiken, het vereiste niveau voor Quality of Service (QoS) van toepassing op uw infrastructuur switches moeten overeenkomen met uw RPO en herstel tijd objective (RTO) Sla's.
-   Maximale toegangslatentie van Azure Blob-opslag moet ongeveer 80 ms.

### <a name="deploy-storsimple"></a>StorSimple implementeren

Zie voor een stapsgewijze implementatiehulp StorSimple [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-backup-exec"></a>Back-up Exec implementeren

Zie voor aanbevolen procedures voor de back-up Exec-installatie, [aanbevolen procedures voor de installatie van de back-up Exec](https://www.veritas.com/support/en_US/article.000068207).

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

- Gebruik geen spanned volumes (gemaakt door Windows Schijfbeheer). Spanned schijven worden niet ondersteund.
- De volumes formatteren met NTFS met een toewijzingsgrootte van 64 KB.
- De StorSimple-volumes rechtstreeks verwijzen naar de back-up Exec server.
    - ISCSI-gebruiken voor fysieke servers.
    - Passthrough-schijven voor virtuele servers gebruiken.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>Aanbevolen procedures voor StorSimple en back-up Exec

Instellen van uw oplossing volgens de richtlijnen in de volgende secties.

### <a name="operating-system-best-practices"></a>Aanbevolen procedures van besturingssysteem

-   Windows Server-versleuteling en Ontdubbeling voor het NTFS-bestandssysteem uitgeschakeld.
-   Schakel defragmentatie van Windows Server op de StorSimple-volumes.
-   Schakel Windows Server op de StorSimple-volumes te indexeren.
-   Een antivirusprogramma scan uitvoeren op de bronhost (niet op de StorSimple-volumes).
-   De standaardwaarde uitschakelen [onderhoud van Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) in Taakbeheer. Dit op een van de volgende manieren doen:
   - De configurator onderhoud in Windows Taakplanner uitschakelen.
   - Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) van Windows Sysinternals. Nadat u PsExec hebt gedownload, voert u Azure PowerShell als beheerder en typ:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>Aanbevolen procedures voor StorSimple

  -   Zorg dat het StorSimple-apparaat is bijgewerkt naar [Update 3 of hoger](storsimple-install-update-3.md).
  -   Isoleren iSCSI- en cloud-verkeer. Speciale iSCSI-verbindingen gebruiken voor verkeer tussen StorSimple en de back-upserver.
  -   Zorg ervoor dat uw StorSimple-apparaat een toegewezen back-doel is. Gemengde werkbelastingen worden niet ondersteund omdat ze invloed heeft op uw RTO en RPO.

### <a name="backup-exec-best-practices"></a>Aanbevolen procedures voor back-up Exec

-   Exec back-up moet worden geïnstalleerd op een lokale schijf van de server en niet op een StorSimple-volume.
-   De back-up Exec opslag instellen **gelijktijdige schrijfbewerkingen** het maximum.
    -   De back-up Exec opslag instellen **blokkeren en in buffer grootte** tot 512 KB.
    -   Back-up Exec opslag inschakelen **lezen en schrijven in de buffer opgeslagen**.
-   StorSimple Backup Exec volledige en incrementele back-ups ondersteunt. U wordt aangeraden geen synthetische en differentiële back-ups te gebruiken.
-   Back-upgegevensbestanden moeten alleen gegevens voor een specifieke taak bevatten. Bijvoorbeeld geen media worden toegevoegd via verschillende taken zijn toegestaan.
-   Controle van de taak niet uitschakelen. Indien nodig, moet verificatie worden gepland na de meest recente back-uptaak. Het is belangrijk te weten dat deze taak is van invloed op uw back-upvenster.
-   Selecteer **opslag** > **uw schijf** > **Details** > **eigenschappen**. Schakel **vooraf schijfruimte**.

Zie voor de meest recente back-up Exec instellingen en aanbevolen procedures voor het implementeren van deze vereisten, [de website Veritas](https://www.veritas.com).

## <a name="retention-policies"></a>Bewaarbeleid

Een van de meest voorkomende typen bewaren van back-up is een beleid opa vader en zoon (algemene). Een incrementele back-up dagelijks wordt uitgevoerd in een algemene-beleid en volledige back-ups wekelijkse en maandelijkse klaar bent. Dit beleid resulteert in zes StorSimple gelaagde volumes. Een volume bevat de wekelijkse, maandelijkse en jaarlijkse volledige back-ups. De vijf volumes opslaan dagelijkse incrementele back-ups.

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

## <a name="set-up-backup-exec-storage"></a>Back-up Exec opslag instellen

### <a name="to-set-up-backup-exec-storage"></a>Back-up Exec opslag instellen

1.  Selecteer in de beheerconsole voor back-up Exec **opslag** > **opslag configureren** > **schijven gebaseerde opslag** > **volgende**.

    ![Back-up van de beheerconsole Exec, opslagpagina configureren](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  Selecteer **schijfopslag**, en selecteer vervolgens **volgende**.

    ![Back-up Exec-beheerconsole, selecteer opslagpagina](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Voer een representatieve naam, bijvoorbeeld: **zaterdag volledige**, en een beschrijving. Selecteer **volgende**.

    ![Pagina back-up Exec management console, naam en beschrijving](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Selecteer de schijf waarop u wilt maken van het schijfopslagapparaat en selecteer vervolgens **volgende**.

    ![Back-up Exec beheerconsole opslagpagina schijf selecteren](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Verhoog het aantal schrijfbewerkingen naar **16**, en selecteer vervolgens **volgende**.

    ![Back-up Exec beheerconsole gelijktijdige bewerkingen instellingenpagina schrijven](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Controleer de instellingen en selecteer vervolgens **voltooien**.

    ![Back-up Exec beheerconsole opslag configuratie overzichtspagina](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Aan het einde van elke toewijzing volume, wijzig de instellingen voor het apparaat van opslag zodat deze overeenkomen met de aanbevolen op [aanbevolen procedures voor StorSimple en back-up Exec](#best-practices-for-storsimple-and-backup-exec).

    ![Back-up Exec beheerconsole opslagpagina voor apparaatinstellingen](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  Herhaal stap 1-7 totdat u klaar voor het toewijzen van uw StorSimple-volumes aan back-up Exec bent.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple instellen als primaire back-doel

> [!NOTE]
> Gegevens terugzetten vanuit een back-up die naar de cloud is gelaagde vindt plaats met een snelheid van de cloud.

De volgende afbeelding ziet de toewijzing van een typische volume aan een back-uptaak. In dit geval worden alle wekelijkse back-ups worden toegewezen aan de volledige schijf zaterdag en de incrementele back-ups worden toegewezen aan incrementele schijven van maandag tot vrijdag. Alle back-ups en herstelbewerkingen van een StorSimple zijn gelaagd volume.

![Primaire back-updoel configuratie logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple als primaire back-updoel algemene voorbeeld plannen

Hier volgt een voorbeeld van een algemene rotatieschema vier weken, maandelijkse en jaarlijkse:

| Type frequentie/back-up | Volledig | Incrementele (dagen 1-5)  |   
|---|---|---|
| Wekelijks (weken 1-4) | Zaterdag | Maandag tot vrijdag |
| Maandelijks  | Zaterdag  |   |
| Jaarlijks | Zaterdag  |   |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Backup Exec

De volgende procedure wordt ervan uitgegaan dat back-up Exec en de doelhost zijn geconfigureerd in overeenstemming met de back-up Exec agent richtlijnen.

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Backup Exec

1.  Selecteer in de beheerconsole voor back-up Exec **Host** > **back-up** > **back-up naar schijf**.

    ![Back-up Exec beheerconsole, selecteer host back-up- en back-up op schijf](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  In de **back-up definitie eigenschappen** dialoogvenster onder **back-up**, selecteer **bewerken**.

    ![Back-up Exec beheerconsole in het dialoogvenster Eigenschappen van back-up-definitie](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  Uw volledige en incrementele back-ups instellen, zodat ze voldoen aan uw vereisten RPO en RTO en aan aanbevolen procedures Veritas voldoen.

4.  In de **opties voor back-up** dialoogvenster, **opslag**.

    ![Back-up Exec beheerconsole voor het dialoogvenster Opties voor opslag van back-up](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  Bijbehorende StorSimple-volumes toewijzen aan uw back-upschema.

    > [!NOTE]
    > **Compressie** en **versleutelingstype** zijn ingesteld op **geen**.

6.  Onder **controleren**, selecteer de **gegevens voor deze taak niet controleren** selectievakje. Met deze optie kan beïnvloeden StorSimple lagen.

    > [!NOTE]
    > Defragmentatie indexeren en achtergrond verificatie beïnvloeden negatief de StorSimple-lagen.

    ![Controleer de instellingen van back-up Exec beheerconsole, opties voor back-up](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Wanneer u de rest van de back-upopties te voldoen aan uw vereisten hebt ingesteld, selecteert u **OK** te voltooien.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple instellen als een secundaire back-doel

> [!NOTE]
>Gegevens terugzetten vanuit een back-up die naar de cloud is gelaagde optreden met een snelheid van de cloud.

In dit model, moet u een opslagmedium (met uitzondering van StorSimple) hebben om te fungeren als een tijdelijke cache. Bijvoorbeeld, kunt u een redundante matrix van onafhankelijke schijven (RAID) volume aan schijfruimte, input/output (I/O) en de bandbreedte. U wordt aangeraden met behulp van RAID-5, 50 en 10.

De volgende afbeelding toont typisch kortetermijnback bewaren (naar de server) lokale volumes en lange bewaartermijn archiveert volumes. In dit scenario worden alle back-ups uitvoeren op de lokale (naar de server) RAID-volume. Deze back-ups worden regelmatig gedupliceerd en gearchiveerd op een volume archieven. Het is belangrijk om de grootte van uw lokale (naar de server) RAID-volume, zodat deze kan omgaan met uw op korte termijn capaciteit en prestaties vereisten voor de bewaarperiode.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple als een voorbeeld van een secundaire back-updoel algemene

![StorSimple als secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

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


### <a name="assign-storsimple-volumes-to-a-backup-exec-archive-and-deduplication-job"></a>StorSimple-volumes toewijzen aan een archief Exec back-up en Ontdubbeling van taak

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-archive-and-duplication-job"></a>StorSimple-volumes toewijzen aan een back-up Exec archief en duplicatie taak

1.  Met de rechtermuisknop op de taak die u wilt archiveren naar een StorSimple-volume en selecteert u vervolgens in de beheerconsole Backup Exec **back-up definitie eigenschappen** > **bewerken**.

    ![Back-up Exec-beheerconsole, tabblad Eigenschappen van de back-up-definitie](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Selecteer **fase toevoegen** > **dupliceren naar schijf** > **bewerken**.

    ![Back-up van de beheerconsole Exec, fase toevoegen](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  In de **opties dubbele** dialoogvenster Selecteer de waarden die u gebruiken wilt voor **bron** en **planning**.

    ![Back-up Exec beheerconsole, definities van de back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  In de **opslag** vervolgkeuzelijst, selecteert u het StorSimple-volume waar u de archief-taak voor het opslaan van de gegevens.

    ![Back-up Exec beheerconsole, definities van de back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Selecteer **controleren**, en selecteer vervolgens de **gegevens voor deze taak niet controleren** selectievakje.

    ![Back-up Exec beheerconsole, definities van de back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  Selecteer **OK**.

    ![Back-up Exec beheerconsole, definities van de back-eigenschappen en opties voor dubbele](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  In de **back-up** kolom, een nieuwe fase toevoegen. Gebruik voor de bron **incrementele**. Kies het StorSimple-volume waar de incrementele back-uptaak wordt gearchiveerd voor het doel. Herhaal stap 1-6.

## <a name="storsimple-cloud-snapshots"></a>De cloudmomentopnamen StorSimple

De cloudmomentopnamen StorSimple Beveilig de gegevens die zich in uw StorSimple-apparaat bevindt. Maken van een cloudmomentopname is gelijk aan het lokale back-uptapes naar een faciliteit off-site back-upfunctie. Als u Azure geografisch redundante opslag gebruikt, is het maken van een cloudmomentopname gelijk aan het back-upfunctie back-uptapes op meerdere sites zijn. Als u herstellen van een apparaat na een noodgeval wilt, kunt u mogelijk een andere StorSimple-apparaat online brengen en voer een failover. U zou toegang kunnen krijgen tot de gegevens (snelheden cloud) van de meest recente cloudmomentopname zijn na de failover.

De volgende sectie wordt beschreven hoe een korte script maken om te beginnen en cloudmomentopnamen StorSimple tijdens de back-up naverwerking verwijderen.

> [!NOTE]
> Momentopnamen die handmatig of programmatisch gemaakt niet op het verloopbeleid van StorSimple snapshot. Deze momentopnamen moeten handmatig of via een programma verwijderen.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Start en cloudmomentopnamen verwijderen met behulp van een script

> [!NOTE]
> Evalueer zorgvuldig de naleving en gegevens bewaren weerslag voordat u een momentopname van een StorSimple verwijderen. Zie voor meer informatie over het uitvoeren van een script dat na het [Backup Exec documentatie](https://www.veritas.com/support/en_US/15047.html).

### <a name="backup-lifecycle"></a>De levenscyclus van de back-up

![Back-Lifecycle-diagram](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

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
4.  Het script toevoegen aan de back-uptaak in Backup Exec door het bewerken van uw back-up Exec taak options vooraf verwerken en na verwerking van opdrachten.

    ![Back-up Exec console, back-upopties, tabblad opdrachten vóór en na verwerking](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Het is raadzaam dat u uw back-upbeleid voor StorSimple cloud momentopname als een script dat na verwerking aan het einde van de dagelijkse back-uptaak uitgevoerd. Voor meer informatie over het back-up en herstellen van uw back-uptoepassing omgeving om te voldoen aan de RPO en RTO Neem contact op met uw back-architect.

## <a name="storsimple-as-a-restore-source"></a>StorSimple als een bron terugzetten

Herstelt vanuit een StorSimple-apparaat werk zoals herstelt vanaf elk apparaat met blokopslag. Herstellen van gegevens die naar de cloud is gelaagd vindt plaats met een snelheid van de cloud. Voor lokale gegevens voorkomen herstelacties op de snelheid van de lokale schijf van het apparaat. Raadpleeg de documentatie van de Backup Exec voor informatie over het uitvoeren van een terugzetbewerking. Het is raadzaam dat u aan de aanbevolen procedures voor back-up Exec terugzetten voldoet.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover en herstel na noodgevallen

> [!NOTE]
> Voor scenario's met back-updoel wordt StorSimple Cloud toestel niet ondersteund als een doel voor herstel.

Een noodgeval kan zijn veroorzaakt door een verscheidenheid aan factoren. De volgende tabel bevat algemene herstel na noodgevallen.

| Scenario | Impact | Herstellen | Opmerkingen |
|---|---|---|---|
| Fout met StorSimple | Back-up en herstelbewerkingen zijn onderbroken. | Vervang het mislukte apparaat en uit te voeren [StorSimple failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md). | Als u een herstelbewerking na het herstel van apparaat moet, worden volledige gegevens wordt opgehaald uit de cloud naar het nieuwe apparaat. Alle bewerkingen zijn met een snelheid van de cloud. Het proces voor indexering en catalogiseren scannen kan ertoe leiden dat alle back-upsets worden gescand en opgehaald uit de cloudlaag naar het lokale apparaat-laag kan een tijdrovend proces zijn. |
| Back-Exec-serverfout | Back-up en herstelbewerkingen zijn onderbroken. | De back-upserver bouwen en uitvoeren van het terugzetten van de database zoals beschreven in [hoe u een handmatige back-up en herstellen van back-up Exec (BEDB) database](http://www.veritas.com/docs/000041083). | U moet opnieuw samenstellen of herstellen van de back-up Exec-server op de site van de herstel na noodgevallen. De database herstellen naar de meest recente. Als de herstelde database voor back-up Exec niet synchroon met de meest recente back-uptaken is, is indexeren en catalogiseren vereist. Deze index en de catalogus opnieuw scannen proces mogelijk alle back-upsets worden gescand en opgehaald uit de cloudlaag aan de laag van het lokale apparaat. Dit maakt het verdere tijdrovende. |
| Fout bij de site die in het verlies van de back-upserver en de StorSimple resulteert | Back-up en herstelbewerkingen zijn onderbroken. | StorSimple eerst herstellen en herstel vervolgens Backup Exec. | StorSimple eerst herstellen en herstel vervolgens Backup Exec. Als u een herstelbewerking na het herstel van apparaat moet, worden de volledige gegevenssets werken vanuit de cloud naar het nieuwe apparaat opgehaald. Alle bewerkingen zijn met een snelheid van de cloud. |

## <a name="references"></a>Verwijzingen

De volgende documenten zijn waarnaar wordt verwezen voor dit artikel:

- [StorSimple multipath i/o-installatie](storsimple-configure-mpio-windows-server.md)
- [Scenario's voor opslag: Thin provisioning](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Met behulp van GPT-schijven](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Schaduwkopieën van gedeelde mappen instellen](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [terugzetten vanuit een back-upset](storsimple-restore-from-backup-set-u2.md).
- Meer informatie over het uitvoeren van [apparaat failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md).
