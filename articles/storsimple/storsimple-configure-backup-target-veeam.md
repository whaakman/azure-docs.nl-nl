---
title: StorSimple 8000-serie als back-updoel met Veeam | Microsoft Docs
description: Beschrijving van de configuratie van de back-updoel StorSimple met Veeam.
services: storsimple
documentationcenter: ''
author: harshakirank
manager: matd
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2016
ms.author: hkanna
ms.openlocfilehash: f06b74493bad546997f82ed6eef0a89cffb7c75b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261975"
---
# <a name="storsimple-as-a-backup-target-with-veeam"></a>StorSimple als back-updoel met Veeam

## <a name="overview"></a>Overzicht

Azure StorSimple is een oplossing voor hybride cloudopslag van Microsoft. De complexiteit van de oplossing die exponentiële gegevenstoename adressen StorSimple met behulp van een Azure Storage-account als een uitbreiding van de on-premises oplossing en de gegevens die automatisch warmtemeting voor on-premises opslag en opslag in de cloud.

In dit artikel wordt besproken hoe de integratie van StorSimple met Veeam en aanbevolen procedures voor het integreren van beide oplossingen. We doen ook aanbevelingen voor het instellen van Veeam beste integreren met StorSimple. We stellen Veeam aanbevolen procedures, back-architecten en administrators voor de beste manier om Veeam instellen om te voldoen aan de vereisten voor afzonderlijke back-up- en service level agreements (Sla's).

Hoewel we laten configuratiestappen en belangrijkste concepten zien, is in dit artikel geen stapsgewijze handleiding-configuratie of de installatie. Gaan we ervan uit dat de basisonderdelen van en de infrastructuur zijn en klaar voor de ondersteuning van de concepten die worden beschreven.

### <a name="who-should-read-this"></a>Wie is dit?

De informatie in dit artikel is vooral handig voor back-upbeheerders, opslagbeheerders en opslag-architecten die kennis van opslag, Windows Server 2012 R2, Ethernet, cloudservices en Veeam hebben.

### <a name="supported-versions"></a>Ondersteunde versies

-   Veeam 9 en latere versies
-   [StorSimple Update 3 en latere versies](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Waarom StorSimple als back-updoel?

StorSimple is een goede keuze voor een back-updoel omdat:

-   Het biedt standaard, lokale opslag voor back-uptoepassingen om te gebruiken als een snelle back-upbestemming, zonder deze te wijzigen. U kunt ook StorSimple gebruiken voor een snel herstel van recente back-ups.
-   De cloud tiering is volledig geïntegreerd met een Azure-cloud storage-account voordelige Azure Storage gebruiken.
-   Het biedt automatisch offsiteopslag voor herstel na noodgevallen.


## <a name="key-concepts"></a>Belangrijkste concepten

Net als bij een storage-oplossing, een zorgvuldige evaluatie van de opslagprestaties van de oplossing, sla's, is de snelheid van de wijzigings- en groei van de capaciteitsbehoeften essentieel voor succes. Het belangrijkste idee is dat door de introductie van een cloudlaag, uw toegangstijden en doorvoercapaciteit naar de cloud play een belangrijke rol in de mogelijkheid van StorSimple om zijn werk te doen.

StorSimple is ontworpen om opslag te bieden tot toepassingen die worden uitgevoerd op een goed gedefinieerde werkset van gegevens (gegevens). In dit model, de werkset van gegevens is opgeslagen op de lokale lagen en de resterende vrije/koude/gearchiveerd set gegevens naar de cloud is gelaagd. Dit model wordt weergegeven in de volgende afbeelding. Bijna vaste groene lijn staat voor de gegevens die zijn opgeslagen op de lokale lagen van de StorSimple-apparaat. De rode lijn staat voor de totale hoeveelheid gegevens die zijn opgeslagen op de StorSimple-oplossing voor alle lagen. De ruimte tussen de vaste groene lijn en de exponentiële rode kromme vertegenwoordigt de totale hoeveelheid gegevens die zijn opgeslagen in de cloud.

**StorSimple opslaglagen**
![cloudlagen StorSimple-diagram](./media/storsimple-configure-backup-target-using-veeam/image1.jpg)

Met deze architectuur in gedachten vindt u dat StorSimple is ideaal als een back-updoel wilt gebruiken. U kunt StorSimple te gebruiken:

-   Uw meest frequente herstelbewerkingen uitvoeren vanaf de lokale werkset van gegevens.
-   De cloud gebruiken voor offsite-noodherstel en oudere gegevens waar herstelbewerkingen zijn minder frequent optreden.

## <a name="storsimple-benefits"></a>Voordelen van StorSimple

StorSimple biedt een on-premises-oplossing die naadloos is geïntegreerd met Microsoft Azure, door te profiteren van naadloze toegang tot on-premises en cloud-opslag.

StorSimple maakt gebruik van automatische lagen tussen de on-premises-apparaat met SSD-apparaat (SSD) en serial attached SCSI (SAS) storage en Azure Storage. Automatische lagen, houdt veelgebruikte gegevens lokaal op de SSD- en SAS-lagen. Wordt weinig gebruikte gegevens verplaatst naar Azure Storage.

StorSimple biedt deze voordelen:

-   De unieke deduplicatie en compressie-algoritmen die gebruikmaken van de cloud te bereiken ongekende Ontdubbeling niveaus
-   Hoge beschikbaarheid
-   Geo-replicatie met behulp van Azure geo-replicatie
-   Integratie van Azure
-   Versleuteling van gegevens in de cloud
-   Verbeterde noodherstel en naleving

Hoewel StorSimple geeft fundamenteel twee belangrijkste implementatiescenario's (back-updoel primaire en secundaire back-updoel), is het een gewone, blokopslagapparaat. StorSimple biedt alle compressie en Ontdubbeling. Naadloos wordt verzonden en opgehaald van gegevens tussen de cloud en de toepassing en het bestandssysteem.

Zie voor meer informatie over StorSimple [StorSimple 8000-serie: hybride cloudopslagoplossing](storsimple-overview.md). U kunt ook controleren de [technische specificaties van de StorSimple 8000-serie](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Met behulp van een storsimple-oplossing wordt apparaat als een back-updoel alleen ondersteund voor StorSimple 8000-Update 3 en latere versies.

## <a name="architecture-overview"></a>Overzicht van de architectuur

De volgende tabellen tonen de initiële richtlijnen voor apparaat-model-naar-architectuur.

**StorSimple-capaciteit voor lokale en opslag in de cloud**

| Opslagcapaciteit | 8100 | 8600 |
|---|---|---|
| Lokale opslagcapaciteit | &lt; 10 TiB\*  | &lt; 20 TiB\*  |
| Cloudopslagcapaciteit | &gt; 200 TiB\* | &gt; 500 TiB\* |

\* Maximale grootte wordt ervan uitgegaan dat er geen gegevensontdubbeling of compressie.

**StorSimple-capaciteiten voor primaire en secundaire back-ups**

| Back-scenario  | Lokale opslagcapaciteit  | Cloudopslagcapaciteit  |
|---|---|---|
| Primaire back-up  | Recente back-ups opgeslagen op lokale opslag voor snel herstel om te voldoen aan de beoogde herstelpunt (RPO) | Back-upgeschiedenis (RPO) past in de cloud-capaciteit |
| Secundaire back-up | Secundaire kopie van de back-upgegevens kan worden opgeslagen in de cloud-capaciteit  | N/A  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple als primaire back-updoel

In dit scenario worden de StorSimple-volumes weergegeven voor de back-uptoepassing als de enige opslagruimte voor back-ups. De volgende afbeelding toont de oplossingsarchitectuur van een waarin alle back-ups gebruik StorSimple volumes voor back-ups en herstelbewerkingen gelaagde.

![StorSimple als een logisch diagram van de primaire back-updoel](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de backup-agent verzendt gegevens naar de back-upserver.
2.  De back-upserver schrijft gegevens naar de StorSimple gelaagde volumes.
3.  De back-upserver van de catalogusdatabase bijgewerkt en klikt u vervolgens klaar is met de back-uptaak.
4.  Een momentopname-script wordt geactiveerd StorSimple cloud snapshot manager (start of verwijderen).
5.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="primary-target-restore-logical-steps"></a>Primaire doel terugzetten logische stappen

1.  De back-upserver Start herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver klaar is met de hersteltaak.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple als secundaire back-updoel

In dit scenario, worden StorSimple-volumes voornamelijk gebruikt voor langdurige bewaarperioden en archiveren.

De volgende afbeelding ziet u een architectuur die eerste back-ups en herstellen van een hoogwaardige doelvolume. Deze back-ups worden gekopieerd en opgeslagen in een storsimple-oplossing gelaagd volume op een vast schema.

Het is belangrijk om de grootte van het volume met hoge prestaties, zodat uw retentie capaciteit en prestaties vereisten kunnen worden verwerkt.

![StorSimple als een secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Secundaire doel van de back-up logische stappen

1.  De back-upserver neemt contact op met de back-upagent doel en de backup-agent verzendt gegevens naar de back-upserver.
2.  De back-upserver schrijft gegevens naar opslag met hoge prestaties.
3.  De back-upserver van de catalogusdatabase bijgewerkt en klikt u vervolgens klaar is met de back-uptaak.
4.  De back-upserver worden back-ups voor StorSimple op basis van een bewaarbeleid gekopieerd.
5.  Een momentopname-script wordt geactiveerd StorSimple cloud snapshot manager (start of verwijderen).
6.  De back-upserver worden verlopen back-ups op basis van een bewaarbeleid verwijderd.

### <a name="secondary-target-restore-logical-steps"></a>Secundaire doel terugzetten logische stappen

1.  De back-upserver Start herstellen van de juiste gegevens uit de opslagplaats voor opslag.
2.  De back-upagent ontvangt de gegevens van de back-upserver.
3.  De back-upserver klaar is met de hersteltaak.

## <a name="deploy-the-solution"></a>De oplossing implementeren

Implementatie van de oplossing zijn drie stappen vereist:

1. Bereid de netwerkinfrastructuur.
2. Uw StorSimple-apparaat implementeren als een back-updoel.
3. Veeam implementeren.

Elke stap wordt besproken in de volgende secties.

### <a name="set-up-the-network"></a>Het netwerk instellen

Omdat StorSimple een oplossing die is geïntegreerd met de Azure-cloud is, vereist StorSimple een actieve verbinding met de Azure-cloud. Deze verbinding wordt gebruikt voor bewerkingen zoals cloudmomentopnamen, beheer van gegevens en metagegevens overbrengen en naar laag oudere, minder gebruikte gegevens naar Azure-cloudopslag.

Voor de oplossing om optimaal te presteren, raden we aan dat u deze netwerken aanbevolen procedures volgt:

-   De koppeling die verbinding maakt StorSimple meerdere lagen naar Azure moet voldoen aan uw eisen aan de bandbreedte. Dit doen door het toepassen van het vereiste niveau van de Quality of Service (QoS) op uw infrastructuur switches zodat deze overeenkomen met uw RPO en recovery time objective (RTO) Sla's.
-   Maximale toegangslatentie van Azure Blob-opslag moet ongeveer 80 ms.

### <a name="deploy-storsimple"></a>StorSimple implementeren

Zie voor stapsgewijze instructies voor StorSimple-implementatie, [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-veeam"></a>Veeam implementeren

Zie voor de aanbevolen installatieprocedures Veeam, [Veeam back-up en aanbevolen procedures voor replicatie](https://bp.veeam.expert/), en lees de handleiding bij [Veeam Help en ondersteuning (technische documentatie)](https://www.veeam.com/documentation-guides-datasheets.html).

## <a name="set-up-the-solution"></a>Instellen van de oplossing

In deze sectie ziet enkele configuratievoorbeelden. De volgende voorbeelden en aanbevelingen ziet de meest elementaire en fundamentele uitvoering. Deze implementatie mogelijk niet van toepassing zijn rechtstreeks aan uw specifieke behoeften van de back-up.

### <a name="set-up-storsimple"></a>Instellen van StorSimple

| StorSimple-implementatietaken  | Aanvullende opmerkingen |
|---|---|
| Uw on-premises StorSimple-apparaat implementeren. | Ondersteunde versies: Update 3 en latere versies. |
| Schakel in het back-updoel. | Gebruik deze opdrachten inschakelen of uitschakelen van back-updoel modus en status ophalen. Zie voor meer informatie, [verbinding maken met een StorSimple-apparaat op afstand](storsimple-remote-connect.md).</br> Om in te schakelen op back-upmodus: `Set-HCSBackupApplianceMode -enable`. </br> Back-modus uit te schakelen: `Set-HCSBackupApplianceMode -disable`. </br> Om op te halen van de huidige status van de instellingen voor back-upmodus: `Get-HCSBackupApplianceMode`. |
| Maak een algemene volumecontainer voor het volume waarin de back-upgegevens. Alle gegevens in een volumecontainer is ontdubbeld. | StorSimple-volumecontainers definiëren domeinen met Ontdubbeling.  |
| StorSimple-volumes maken. | Maak volumes met grootten als dicht bij het verwachte gebruik mogelijk, omdat de grootte van volume is van invloed op de duur van de tijd van cloudmomentopname. Voor informatie over hoe u het formaat van een volume, leest u over [bewaarbeleid](#retention-policies).</br> </br> Gebruik StorSimple gelaagde volumes en selecteert u de **dit volume gebruiken voor minder frequent gebruikte gearchiveerde gegevens** selectievakje. </br> Gebruik alleen lokaal vastgemaakt volumes wordt niet ondersteund. |
| Een unieke StorSimple-back-upbeleid voor alle back-updoel volumes maken. | Een StorSimple-back-upbeleid definieert de volumegroep consistentie. |
| De planning uitschakelen als de momentopnamen zijn verlopen. | Momentopnamen worden geactiveerd als een bewerking na verwerking. |

### <a name="set-up-the-host-backup-server-storage"></a>Instellen van de opslag van de back-upserver van host

Instellen van de opslag van de back-upserver host op basis van deze richtlijnen:  

- Gebruik geen spanned volumes (gemaakt door Windows Schijfbeheer). Spanned volumes worden niet ondersteund.
- De volumes formatteren met NTFS met clustergrootte van 64 KB.
- De StorSimple-volumes rechtstreeks naar de server Veeam worden toegewezen.
    - Gebruik iSCSI voor fysieke servers.


## <a name="best-practices-for-storsimple-and-veeam"></a>Aanbevolen procedures voor StorSimple en Veeam

Instellen van uw oplossing op basis van de richtlijnen in de volgende gedeelten.

### <a name="operating-system-best-practices"></a>Aanbevolen procedures voor besturingssysteem

-   Windows Server-versleuteling en Ontdubbeling voor het NTFS-bestandssysteem uitgeschakeld.
-   Uitschakelen van Windows Server-defragmentatie op de StorSimple-volumes.
-   Indexeren van Windows Server op het StorSimple-volumes uitschakelen.
-   Voer een antivirusprogramma uit op de bronhost (niet op basis van de StorSimple-volumes).
-   De standaardwaarde uitschakelen [onderhoud van Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) in Taakbeheer. Dit op een van de volgende manieren doen:
    - De configurator onderhoud in Windows Taakplanner uitschakelen.
    - Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) van Windows Sysinternals. Nadat u PsExec hebt gedownload, voert u de Windows PowerShell als beheerder, en typ:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>Aanbevolen procedures voor StorSimple

-   Zorg dat het StorSimple-apparaat is bijgewerkt naar [Update 3 of hoger](storsimple-install-update-3.md).
-   Isoleren iSCSI en cloudverkeer. ISCSI-specifieke verbindingen gebruiken voor verkeer tussen StorSimple en de back-upserver.
-   Zorg ervoor dat uw StorSimple-apparaat een toegewezen back-updoel is. Gemengde werkbelastingen worden niet ondersteund omdat ze invloed heeft op uw RTO en RPO.

### <a name="veeam-best-practices"></a>Aanbevolen procedures voor Veeam

-   De database Veeam moet lokaal op de server en bevindt zich niet op een StorSimple-volume.
-   Voor herstel na noodgevallen, back-up van de database Veeam op een StorSimple-volume.
-   Veeam volledige en incrementele back-ups die u voor deze oplossing wordt ondersteund. U wordt aangeraden dat u geen synthetische en differentiële back-ups gebruikt.
-   Back-upgegevens bestanden mag alleen de gegevens voor een specifieke taak. Bijvoorbeeld geen media worden toegevoegd voor verschillende taken zijn toegestaan.
-   Verificatie van de taak uitschakelen. Indien nodig, kan verificatie moet worden gepland na de meest recente back-uptaak. Het is belangrijk om te begrijpen dat deze taak is van invloed op uw back-upvenster.
-   Schakel vooraf mediatoewijzing.
-   Zorg ervoor dat parallelle verwerking is ingeschakeld.
-   Compressie uitschakelt.
-   Ontdubbeling op de back-uptaak uitschakelen.
-   Optimalisatie ingesteld op **LAN doel**.
-   Schakel **maken active volledige back-up** (elke 2 weken).
-   Op de back-opslagplaats instellen **gebruikt per VM-back-upbestanden**.
-   Stel **gebruik van meerdere uploaden stromen per taak** naar **8** (maximaal 16 is toegestaan). Wijzig dit aantal omhoog of omlaag op basis van CPU-gebruik op het StorSimple-apparaat.

## <a name="retention-policies"></a>Bewaarbeleid

Een van de meest voorkomende typen van de back-upretentie-beleid is een zogenaamde grootvader vader en zoon (algemene)-beleid. Een incrementele back-up dagelijks wordt uitgevoerd in een beleid algemene en volledige back-ups wekelijkse en maandelijkse klaar bent. Deze resultaten van Groepsbeleid in zes StorSimple gelaagde volumes: één volume de wekelijkse, maandelijkse en jaarlijkse volledige back-ups bevat; de vijf volumes opslaan dagelijkse incrementele back-ups.

In het volgende voorbeeld gebruiken we een algemene draaiing. Het voorbeeld wordt ervan uitgegaan:

-   Niet-ontdubbelde of gecomprimeerde gegevens worden gebruikt.
-   Volledige back-ups zijn 1 TiB.
-   Dagelijkse incrementele back-ups zijn 500 GiB.
-   Vier wekelijkse back-ups worden bewaard gedurende een maand.
-   Twaalf maandelijkse back-ups worden voor een jaar bewaard.
-   Een jaarlijkse back-up wordt bewaard gedurende tien jaar.

Op basis van de voorgaande veronderstellingen, maak een 26-TiB StorSimple gelaagd volume voor de maandelijkse en jaarlijkse volledige back-ups. Maken van een 5-TiB StorSimple gelaagd volume voor elk van de incrementele dagelijkse back-ups.

| Type back-up bewaren | Grootte (TiB) | Algemene vermenigvuldiger\* | Totale capaciteit (TiB)  |
|---|---|---|---|
| Wekelijkse volledige | 1 | 4  | 4 |
| Dagelijkse incrementele | 0.5 | 20 (cycli gelijk aantal weken per maand) | 12 (2 voor extra quota) |
| Maandelijks volledige | 1 | 12 | 12 |
| Jaarlijks volledige | 1  | 10 | 10 |
| Algemene vereisten |   | 38 |   |
| Uitbreiding van uw quotum  | 4  |   | 42 totale algemene vereiste  |
\* De algemene vermenigvuldiger is het aantal exemplaren dat u wilt beveiligen en te behouden om te voldoen aan de vereisten van uw back-upbeleid.

## <a name="set-up-veeam-storage"></a>Veeam opslag instellen

### <a name="to-set-up-veeam-storage"></a>Veeam opslag instellen

1.  In de console Veeam back-up en replicatie in **opslagplaats extra**, gaat u naar **back-upinfrastructuur**. Met de rechtermuisknop op **back-up opslagplaatsen**, en selecteer vervolgens **back-up-opslagplaats toevoegen**.

    ![Veeam-beheerconsole, back-opslagplaats pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage1.png)

2.  In de **nieuwe back-up-opslagplaats** dialoogvenster vak, voer een naam en beschrijving voor de opslagplaats. Selecteer **Volgende**.

    ![Veeam management console, naam en beschrijving van pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage2.png)

3.  Selecteer het type **Microsoft Windows server**. Selecteer de Veeam-server. Selecteer **Volgende**.

    ![Veeam-beheerconsole, Selecteer type back-opslagplaats](./media/storsimple-configure-backup-target-using-veeam/veeamimage3.png)

4.  Om op te geven **locatie**, bladeren en selecteert u het volume. Selecteer de **maximum aantal gelijktijdige taken te beperken:** selectievakje en stel de waarde voor **4**. Dit zorgt ervoor dat slechts vier virtuele schijven worden verwerkt gelijktijdig terwijl elke virtuele machine (VM) wordt verwerkt. Selecteer de **Geavanceerd** knop.

    ![Veeam-beheerconsole, selecteer volume](./media/storsimple-configure-backup-target-using-veeam/veeamimage4.png)


5.  In de **compatibiliteit Opslaginstellingen** in het dialoogvenster, selecteer de **gebruikt per VM-back-upbestanden** selectievakje.

    ![Veeam-beheerconsole, compatibiliteitsinstellingen voor opslag](./media/storsimple-configure-backup-target-using-veeam/veeamimage5.png)

6.  In de **nieuwe back-up-opslagplaats** in het dialoogvenster, selecteer de **vPower NFS-service op de mount-server (aanbevolen) inschakelen** selectievakje. Selecteer **Volgende**.

    ![Veeam-beheerconsole, back-opslagplaats pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage6.png)

7.  Controleer de instellingen en selecteer vervolgens **volgende**.

    ![Veeam-beheerconsole, back-opslagplaats pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage7.png)

    Een opslagplaats is toegevoegd aan de Veeam-server.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple instellen als primaire back-updoel

> [!IMPORTANT]
> Het herstellen van gegevens vanuit een back-up die is gelaagde naar de cloud vindt plaats met een snelheid van de cloud.

De volgende afbeelding ziet u de toewijzing van een typische volume naar een back-uptaak. In dit geval alle wekelijkse back-ups worden toegewezen aan de volledige schijf zaterdag en de incrementele back-ups worden toegewezen aan incrementele schijven van maandag tot en met vrijdag. Alle back-ups en herstelbewerkingen van een storsimple-oplossing zijn een gelaagd volume.

![Primaire back-updoel configuratie logisch diagram](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple als primaire back-updoel algemene voorbeeld plannen

Hier volgt een voorbeeld van een algemene rotatieschema vier weken, maandelijkse en jaarlijkse:

| Frequentie/back-up-type | Volledig | Incrementele (dagen 1-5)  |   
|---|---|---|
| Wekelijks (weken 1-4) | zaterdag | Maandag tot vrijdag |
| Maandelijks  | zaterdag  |   |
| Per jaar | zaterdag  |   |   |


### <a name="assign-storsimple-volumes-to-a-veeam-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Veeam

Primaire back-updoel scenario voor een dagelijkse taak te maken met uw primaire Veeam StorSimple-volume. Voor een scenario secundaire back-updoel een dagelijkse taak te maken met behulp van Direct gekoppelde opslag (DAS), Network Attached Storage (NAS) of hoeft dan alleen maar een aantal van de schijven (JBOD)-opslag.

#### <a name="to-assign-storsimple-volumes-to-a-veeam-backup-job"></a>StorSimple-volumes toewijzen aan een back-uptaak Veeam

1.  Selecteer in de beheerconsole Veeam back-up en replicatie **back-up en replicatie**. Met de rechtermuisknop op **back-up**, en selecteer vervolgens **VMware** of **Hyper-V**, afhankelijk van uw omgeving.

    ![Veeam-beheerconsole, nieuwe back-uptaak](./media/storsimple-configure-backup-target-using-veeam/veeamimage8.png)

2.  In de **nieuwe back-uptaak** dialoogvenster vak, voer een naam en beschrijving voor de dagelijkse back-uptaak.

    ![Veeam-beheerconsole, nieuwe back-uptaak pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage9.png)

3.  Selecteer een virtuele machine naar de back-ups van maken.

    ![Veeam-beheerconsole, nieuwe back-uptaak pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage10.png)

4.  Selecteer de waarden die u wilt gebruiken voor **back-up proxy** en **back-up opslagplaats**. Selecteer een waarde voor **herstelpunten op schijf** volgens de definities van de RPO en RTO voor uw omgeving op lokaal gekoppelde opslag. Selecteer **Geavanceerd**.

    ![Veeam-beheerconsole, nieuwe back-uptaak pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage11.png)

5. In de **geavanceerde instellingen** dialoogvenster op de **back-up** tabblad **incrementele**. Controleer of de **periodiek synthetische volledige back-ups maken** selectievakje is uitgeschakeld. Selecteer de **periodiek active volledige back-ups maken** selectievakje. Onder **Active volledige back-up**, selecteer de **wekelijks op geselecteerde dagen** selectievakjes in voor zaterdag.

    ![Veeam-beheerconsole, nieuwe back-uptaak geavanceerde instellingenpagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage12.png)

6. Op de **opslag** tabblad, zorg ervoor dat de **inline-gegevensontdubbeling inschakelen** selectievakje is uitgeschakeld. Selecteer de **uitsluiten wisselen bestandsblokken** selectievakje en selecteert u de **uitsluiten verwijderd bestandsblokken** selectievakje. Stel **compressieniveau** naar **geen**. Voor een balans tussen prestaties en Ontdubbeling instellen **opslagoptimalisatie** naar **LAN doel**. Selecteer **OK**.

    ![Veeam-beheerconsole, nieuwe back-uptaak geavanceerde instellingenpagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage13.png)

    Zie voor meer informatie over Veeam deduplicatie en compressie-instellingen [compressie van gegevens en Ontdubbeling](https://helpcenter.veeam.com/backup/vsphere/compression_deduplication.html).

7.  In de **bewerken back-uptaak** in het dialoogvenster kunt u de **toepassingsbewust verwerking inschakelen** selectievakje (optioneel).

    ![Veeam-beheerconsole, nieuwe back-uptaak Gast verwerken van pagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage14.png)

8.  Instellen van de planning om uit te voeren eenmaal per dag, op een tijdstip dat u kunt opgeven.

    ![Veeam-beheerconsole, nieuwe pagina van de back-uptaak plannen](./media/storsimple-configure-backup-target-using-veeam/veeamimage15.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple instellen als een secundaire back-updoel

> [!NOTE]
> Gegevens herstellen vanaf een back-up die naar de cloud is gelaagde optreden met een snelheid van de cloud.

In dit model, moet u een opslagmedium (met uitzondering van StorSimple) hebben om te fungeren als een tijdelijke cache. U kunt bijvoorbeeld een redundante matrix van onafhankelijke schijven (RAID) volume voor ruimte, input/output (I/O), en bandbreedte. Het is raadzaam om met behulp van RAID-5, 50 en 10.

De volgende afbeelding toont een typische op korte termijn retentie lokale (op de server) volumes en op lange termijn bewaarvolumes archief. In dit scenario worden alle back-ups uitgevoerd op het lokale (op de server) RAID-volume. Deze back-ups worden periodiek gedupliceerd en gearchiveerd op een volume dat archief. Het is belangrijk om de grootte van uw lokale (op de server) RAID-volume, zodat uw op korte termijn capaciteit en prestaties van vereisten voor de bewaarperiode kan worden verwerkt.

![StorSimple als secundaire back-updoel logisch diagram](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetdiagram.png)

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple als een voorbeeld van de algemene secundaire back-updoel

De volgende tabel laat zien hoe het instellen van back-ups uit te voeren op de lokale en de StorSimple-schijven. Capaciteitsvereisten voor afzonderlijke en de totale bevat.

| Type back-up en retentie | Geconfigureerde opslag | Grootte (TiB) | Algemene vermenigvuldiger | Totale capaciteit\* (TiB) |
|---|---|---|---|---|
| Week 1 (volledige en incrementele) |Lokale schijf (op korte termijn)| 1 | 1 | 1 |
| StorSimple weken 2-4 |StorSimple-schijf (langlopende) | 1 | 4 | 4 |
| Maandelijks volledige |StorSimple-schijf (langlopende) | 1 | 12 | 12 |
| Jaarlijks volledige |StorSimple-schijf (langlopende) | 1 | 1 | 1 |
|Vereiste grootte van algemene volumes |  |  |  | 18*|
\* Totale capaciteit omvat 17 TiB van StorSimple-schijven en 1 TiB aan lokale RAID-volume.


### <a name="gfs-example-schedule"></a>Algemene voorbeeld plannen

Algemene rotatie wekelijkse, maandelijkse en jaarlijkse planning

| Wekelijks | Volledig | Incrementele dag 1 | Incrementele dag 2 | Incrementele dag 3 | Incrementele dag 4 | Incrementele dag 5 |
|---|---|---|---|---|---|---|
| 1 week | Lokale RAID-volume  | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume | Lokale RAID-volume |
| Week van 2 | StorSimple weken 2-4 |   |   |   |   |   |
| Week van 3 | StorSimple weken 2-4 |   |   |   |   |   |
| Week 4 | StorSimple weken 2-4 |   |   |   |   |   |
| Maandelijks | Maandelijks StorSimple |   |   |   |   |   |
| Per jaar | StorSimple jaarlijks  |   |   |   |   |   |   |

### <a name="assign-storsimple-volumes-to-a-veeam-copy-job"></a>StorSimple-volumes toewijzen aan een taak voor het kopiëren van Veeam

#### <a name="to-assign-storsimple-volumes-to-a-veeam-copy-job"></a>StorSimple-volumes toewijzen aan een taak voor het kopiëren van Veeam

1.  Selecteer in de beheerconsole Veeam back-up en replicatie **back-up en replicatie**. Met de rechtermuisknop op **back-up**, en selecteer vervolgens **VMware** of **Hyper-V**, afhankelijk van uw omgeving.

    ![Veeam management console, de pagina back-up maken van nieuwe taak](./media/storsimple-configure-backup-target-using-veeam/veeamimage16.png)

2.  In de **nieuwe taak voor het kopiëren van back-up** dialoogvenster vak, voer een naam en beschrijving voor de taak.

    ![Veeam management console, de pagina back-up maken van nieuwe taak](./media/storsimple-configure-backup-target-using-veeam/veeamimage17.png)

3.  Selecteer de virtuele machines die u wilt verwerken. Selecteer in de back-ups en selecteer vervolgens de dagelijkse back-up die u eerder hebt gemaakt.

    ![Veeam management console, de pagina back-up maken van nieuwe taak](./media/storsimple-configure-backup-target-using-veeam/veeamimage18.png)

4.  Objecten uitsluiten van de taak back-up, indien nodig.

5.  Selecteer uw back-opslagplaats en een waarde instellen voor **herstelpunten te houden**. Zorg ervoor dat u selecteert de **houden van de volgende herstelpunten voor archiveringsdoeleinden** selectievakje. De back-upfrequentie definiëren, en selecteer vervolgens **Geavanceerd**.

    ![Veeam management console, de pagina back-up maken van nieuwe taak](./media/storsimple-configure-backup-target-using-veeam/veeamimage19.png)

6.  De volgende geavanceerde instellingen opgeven:

    * Op de **onderhoud** tabblad, opslag niveau beschadiging guard uitschakelen.

    ![Veeam-beheerconsole, nieuwe back-up-taak geavanceerde instellingenpagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage20.png)

    * Op de **opslag** tabblad, zorg ervoor dat deduplicatie en compressie zijn uitgeschakeld.

    ![Veeam-beheerconsole, nieuwe back-up-taak geavanceerde instellingenpagina](./media/storsimple-configure-backup-target-using-veeam/veeamimage21.png)

7.  Geef op of de overdracht van gegevens direct.

8.  Definieer de planning van het venster back-up op basis van uw behoeften en vervolgens de wizard hebt voltooid.

Zie voor meer informatie, [maken van back-up taken](https://helpcenter.veeam.com/backup/hyperv/backup_copy_create.html).

## <a name="storsimple-cloud-snapshots"></a>StorSimple cloud-momentopnamen

StorSimple cloud-momentopnamen bescherming van de gegevens die zich in uw StorSimple-apparaat bevinden. Het maken van een cloud-momentopname is gelijk aan het verzenden van lokale back-uptapes op een gebouw van een externe locatie. Als u geografisch redundante opslag met Azure, is het maken van een cloud-momentopname gelijk aan het verzenden van back-uptapes op meerdere sites zijn. Als u herstellen van een apparaat na een noodgeval wilt, kunt u mogelijk een andere StorSimple-apparaat online brengen en een failover. Na de failover, zou u kunnen krijgen tot de gegevens (snelheden cloud) van de meest recente cloudmomentopname zijn.

De volgende sectie wordt beschreven hoe u een korte script om te starten en verwijderen van StorSimple cloud-momentopnamen tijdens back-up na de verwerking van maken.

> [!NOTE]
> Momentopnamen die zijn handmatig of via een programma gemaakt Volg het vervalbeleid voor StorSimple snapshot niet. Deze momentopnamen moeten handmatig of via een programma verwijderen.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Start en cloudmomentopnamen verwijderen met behulp van een script

> [!NOTE]
> Evalueer zorgvuldig de naleving en de gegevens bewaren gevolgen voordat u een StorSimple-momentopname verwijderen. Zie de Veeam-documentatie voor meer informatie over hoe u een na de back-script uit te voeren.


### <a name="backup-lifecycle"></a>Back-levenscyclus

![Diagram van de levenscyclus van de back-up](./media/storsimple-configure-backup-target-using-veeam/backuplifecycle.png)

### <a name="requirements"></a>Vereisten

-   De server waarop het script wordt uitgevoerd moet toegang hebben tot Azure-cloudresources.
-   Het gebruikersaccount moet de vereiste machtigingen hebben.
-   Een back-upbeleid van StorSimple met de bijbehorende StorSimple-volumes moet worden ingesteld maar niet is ingeschakeld.
-   U moet de naam van de StorSimple-resource, de registratiesleutel, apparaat-id op de naam en een back-beleid wordt gebruikt.

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Om te starten of een cloudmomentopname verwijderen

1. [Installeer Azure PowerShell](/powershell/azure/overview).
2. Downloaden en installeren [beheren CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell-script.
3. Voer op de server waarop het script wordt uitgevoerd, PowerShell als beheerder. Zorg ervoor dat u het script uitgevoerd `-WhatIf $true` om te zien wat het script verandert maakt. Nadat de validatie voltooid is, doorgeven `-WhatIf $false`. Voer de onderstaande opdracht:
```powershell
.\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
```
4. Geavanceerde opties voor het script toevoegen aan uw back-uptaak, uw taak Veeam bewerken.

    ![Tabblad Veeam geavanceerde instellingen voor back-up-scripts](./media/storsimple-configure-backup-target-using-veeam/veeamimage22.png)

Het is raadzaam dat u uw back-upbeleid van StorSimple cloud-momentopname als een na verwerking script aan het einde van uw dagelijkse back-uptaak uitvoeren. Voor meer informatie over hoe u het back-up en herstellen van uw back-uptoepassing omgeving om te voldoen aan de RPO en de RTO, neem contact op met uw back-architect.

## <a name="storsimple-as-a-restore-source"></a>StorSimple als een bron herstellen

Herstelt vanuit een StorSimple-apparaat werk, zoals het terugzetten van een apparaat met blokopslag. Herstellen van gegevens die naar de cloud is gelaagd vindt plaats met een snelheid van de cloud. Voor lokale gegevens plaatsvinden herstelt op de snelheid van de lokale schijf van het apparaat.

Met Veeam krijgt u snel gedetailleerde, op bestandsniveau herstel via StorSimple via de ingebouwde explorer-weergaven in de console Veeam. Gebruik de Explorers Veeam afzonderlijke items, zoals e-mailberichten, Active Directory-objecten en SharePoint-items herstellen vanuit back-ups. Het herstel kan worden uitgevoerd zonder onderbreking van de on-premises VM. U kunt ook punt in tijd herstel voor Azure SQL Database en Oracle-databases doen. Veeam en StorSimple kunt het proces van herstel op itemniveau van Azure snel en eenvoudig. Zie de documentatie Veeam voor informatie over het uitvoeren van een herstelpunt:

- Voor [Exchange Server](https://www.veeam.com/microsoft-exchange-recovery.html)
- Voor [Active Directory](https://www.veeam.com/microsoft-active-directory-explorer.html)
- Voor [SQL Server](https://www.veeam.com/microsoft-sql-server-explorer.html)
- Voor [SharePoint](https://www.veeam.com/microsoft-sharepoint-recovery-explorer.html)
- Voor [Oracle](https://www.veeam.com/oracle-backup-recovery-explorer.html)


## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover en herstel na noodgevallen

> [!NOTE]
> Voor back-updoel scenario's, wordt StorSimple-Cloudapparaat niet ondersteund als het doel van een herstel.

Na een noodgeval kan worden veroorzaakt door een verscheidenheid aan factoren. De volgende tabel bevat algemene herstel na noodgevallen.

| Scenario | Impact | Herstellen | Opmerkingen |
|---|---|---|---|
| Fout voor StorSimple-apparaat | Back-up en herstelbewerkingen zijn onderbroken. | Vervang het mislukte apparaat en uit te voeren [StorSimple failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md). | Als u uitvoeren van een herstelbewerking na het herstel van apparaat wilt, worden de volledige gegevens wordt vanuit de cloud naar het nieuwe apparaat opgehaald. Alle bewerkingen zijn met een snelheid van de cloud. De index en de catalogus opnieuw scannen proces kunnen leiden tot alle back-upsets worden gescand en opgehaald uit de cloudlaag naar de lokale apparaat-laag, dit kan een tijdrovend proces. |
| Veeam-serverfout | Back-up en herstelbewerkingen zijn onderbroken. | Opnieuw opbouwen van de back-upserver en database terugzetten, zoals beschreven in [Veeam Help en ondersteuning (technische documentatie)](https://www.veeam.com/documentation-guides-datasheets.html).  | U moet opnieuw samenstellen of herstellen van de server Veeam op de site voor noodherstel. De database herstellen naar de meest recente. Als de herstelde Veeam-database niet gesynchroniseerd met uw meest recente back-uptaken is, is indexeren en catalogiseren vereist. Deze index en de catalogus opnieuw scannen proces kunnen leiden tot alle back-upsets worden gescand en opgehaald uit de cloudlaag naar de prijscategorie van het lokale apparaat. Hierdoor kunt u meer tijd-intensieve. |
| Sitefout die in het verlies van gegevens van de Backup-server en de StorSimple resulteert | Back-up en herstelbewerkingen zijn onderbroken. | StorSimple eerst terugzetten en herstel vervolgens Veeam. | StorSimple eerst terugzetten en herstel vervolgens Veeam. Als u uitvoeren van een herstelbewerking na het herstel van apparaat wilt, worden de volledige gegevens wordt opgehaald vanuit de cloud naar het nieuwe apparaat. Alle bewerkingen zijn met een snelheid van de cloud. |


## <a name="references"></a>Verwijzingen

De volgende documenten werd voor dit artikel verwezen:

- [Multipath i/o-instelling van StorSimple](storsimple-configure-mpio-windows-server.md)
- [Scenario's voor opslag: Thin provisioning](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Met behulp van de GPT-schijven](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Instellen van schaduwkopieën voor gedeelde mappen](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [terugzetten vanuit een back-upset](storsimple-restore-from-backup-set-u2.md).
- Meer informatie over het uitvoeren van [apparaat failover en herstel na noodgevallen](storsimple-device-failover-disaster-recovery.md).
