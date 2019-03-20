---
title: Overzicht van Microsoft Azure StorSimple Virtual Array | Microsoft Docs
description: Beschrijving van de StorSimple Virtual Array, een geïntegreerde opslagaccountoplossing waarmee tussen een on-premises virtuele matrix en Microsoft Azure-cloudopslag opslagtaken.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: e5713af737a6d9d190814b4155a8e772deea06bc
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013910"
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>Inleiding tot StorSimple Virtual Array

## <a name="overview"></a>Overzicht

De Microsoft Azure StorSimple Virtual Array is een geïntegreerde opslagaccountoplossing waarmee opslagtaken tussen een on-premises virtuele matrix die worden uitgevoerd in een hypervisor en Microsoft Azure-cloudopslag. De virtuele matrix is een efficiënte, goedkope en eenvoudig te beheren bestandsserver of iSCSI-server-oplossing waarmee veel van de problemen en de kosten die zijn gekoppeld aan de enterprise- en gegevensbescherming worden opgelost. De virtuele matrix is met name geschikt voor de opslag van weinig gebruikte archivering van gegevens.

In dit artikel biedt een overzicht van de virtuele matrix: Hier volgen enkele andere resources:

* Zie voor aanbevolen procedures [best practices voor StorSimple Virtual Array](storsimple-ova-best-practices.md).
* Voor een overzicht van de apparaten van de StorSimple 8000-serie, gaat u naar [StorSimple 8000-serie: een hybride cloudoplossing](storsimple-overview.md).
* Voor informatie over apparaten uit de StorSimple 5000/7000-serie, gaat u naar [Online-Help voor StorSimple](http://onlinehelp.storsimple.com/).

De virtuele matrix biedt ondersteuning voor het iSCSI- of Server Message Block (SMB)-protocol. Het wordt uitgevoerd op uw bestaande infrastructuur van de hypervisor en biedt meerdere lagen naar de cloud, cloudback-up, snel herstel, herstel op itemniveau en herstelfuncties.

De volgende tabel geeft een overzicht van de belangrijke functies van de StorSimple Virtual Array.

| Functie | StorSimple Virtual Array |
| --- | --- |
| Vereisten voor installatie |Maakt gebruik van virtualisatie-infrastructuur (Hyper-V of VMware) |
| Beschikbaarheid |Eén knooppunt |
| Totale capaciteit (met inbegrip van de cloud) |Maximaal 64 TB aan bruikbare capaciteit per virtuele matrix |
| Lokale capaciteit |390 GB tot 6.4 TB aan bruikbare capaciteit per virtuele matrix (hoeft in te richten van 500 GB tot 8 TB aan schijfruimte) |
| Systeemeigen protocollen |iSCSI- of SMB |
| Beoogde hersteltijd (RTO) |iSCSI: minder dan twee minuten, ongeacht de grootte |
| Recovery Point Objective (RPO) |Dagelijkse back-ups en back-ups op aanvraag |
| Opslaglagen |Maakt gebruik van warmte toewijzing om te bepalen welke gegevens moeten worden lagen in- of uitschalen |
| Ondersteuning |Virtualisatie-infrastructuur wordt ondersteund door de leverancier |
| Prestaties |Is afhankelijk van de onderliggende infrastructuur |
| Gegevensmobiliteit |Op hetzelfde apparaat kunt terugzetten of herstel op itemniveau (bestandsserver) |
| Opslaglagen |Lokale hypervisor opslag- en cloud |
| Grootte van de bestandsshare |Lagen: tot 20 TB; lokaal vastgemaakt: tot 2 TB |
| Volumegrootte |Lagen: 500 GB tot 5 TB; lokaal vastgemaakt: 50 GB tot 200 GB <br> Maximale lokale ruimte gereserveerd voor gelaagde volumes is 200 GB. |
| Momentopnamen |Crash-consistent |
| Herstel op itemniveau |Ja. gebruikers kunnen herstellen van shares |

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

StorSimple maakt gebruikers en -servers verbinding met Azure-opslag in enkele minuten, er zijn geen wijzigingen in de toepassing.

De volgende tabel worden enkele van de belangrijkste voordelen van de oplossing StorSimple Virtual Array biedt beschreven.

| Functie | Voordeel |
| --- | --- |
| Transparante-integratie |De virtuele matrix biedt ondersteuning voor de iSCSI- of het SMB-protocol. De verplaatsing van gegevens tussen de lokale laag en de cloudlaag is een naadloze en transparant voor de gebruiker. |
| Lagere opslagkosten |Met StorSimple richt u voldoende lokale opslag om te voldoen aan de huidige vraag voor de meest gebruikte gegevens. Opslag behoeften groeien, cloudopslag StorSimple lagen koude gegevens in een goedkope. De gegevens worden ontdubbeld en worden gecomprimeerd voordat ze worden verzonden naar de cloud om verder te beperken opslagvereisten en onkosten. |
| Vereenvoudigd opslagbeheer |StorSimple biedt gecentraliseerd beheer in de cloud met StorSimple Device Manager voor het beheren van meerdere apparaten. |
| Verbeterde noodherstel en naleving |StorSimple vereenvoudigt het uitvoeren van noodherstel sneller door de metagegevens onmiddellijk terugzetten en herstellen van de gegevens zo nodig. Dit betekent dat normale bewerkingen kunnen doorgaan met minimale onderbreking. |
| Gegevensmobiliteit |Gelaagde gegevens in de cloud kunnen worden benaderd vanaf andere sites voor herstel- en migratiedoeleinden. Houd er rekening mee dat u gegevens alleen naar de oorspronkelijke virtuele matrix herstellen kunt. U kunt echter herstelfuncties gebruiken om te herstellen van de volledige virtuele matrix naar een andere virtuele matrix. |

## <a name="storsimple-workload-summary"></a>StorSimple workloadoverzicht

Een overzicht van ondersteunde werkbelastingen van StorSimple is Zie tabel hieronder.

|Scenario     |Workload     |Ondersteund      |Beperkingen               | Versies van toepassing|
|-------------|-------------|---------------|---------------------------|--------------------|
|Externe kantoren/filialen Office (ROBO)  |Bestandsdeling     |Ja      |Zie [maximumlimieten voor bestandsserver](storsimple-ova-limits.md).<br></br>Zie [systeemvereisten voor ondersteunde versies van SMB-](storsimple-ova-system-requirements.md).| Alle versies     |
|In de cloud archiveren  |Archivering het delen van bestanden     |Ja      |Zie [maximumlimieten voor bestandsserver](storsimple-ova-limits.md).<br></br>Zie [systeemvereisten voor ondersteunde versies van SMB-](storsimple-ova-system-requirements.md).| Alle versies     |

De StorSimple Virtual Array is het meest geschikt voor weinig gebruikte gegevens. Terwijl de virtuele matrix een lokale cache is om prestaties te verbeteren, gebruikers ervan uitgaan dat het apparaat services-bestanden van het laagste niveau van de opslag (cloud). Elke virtuele matrix kunt schrijven en lezen naar Azure-opslag met ongeveer 100 Mbps. Deze koppeling wordt gedeeld door alle aanvragen die afkomstig zijn bij het apparaat en kan een knelpunt zoals wordt weergegeven in het onderstaande diagram.

![In de cloud archiveren](./media/storsimple-ova-overview/cloud-archiving.png)

Als meerdere gelijktijdige gebruikers toegang hebben tot de virtuele matrix, delen ze allemaal de verbinding met Azure zou leiden tot een lagere prestaties. Er is geen gegarandeerde prestaties per gebruiker en het apparaat afzonderlijke aanvragen verwerkt wanneer ze binnenkomen.

StorSimple Virtual Array is niet geschikt voor workloads waarvoor hoge beschikbaarheid. De virtuele matrix is een enkel knooppunt-apparaat dat uitvaltijd optreedt wanneer software-updates zijn geïnstalleerd. Beheerders moeten plannen voor een periode van 30 minuten 3 - 4 keer per jaar.

## <a name="workflows"></a>Workflows

De StorSimple Virtual Array is met name geschikt voor de volgende werkstromen:

* [Cloud-gebaseerd opslagbeheer](#cloud-based-storage-management)
* [Locatie-onafhankelijke back-up](#location-independent-backup)
* [Gegevens bescherming en herstel na noodgevallen](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Cloud-gebaseerd opslagbeheer
U kunt de StorSimple Device Manager-service die wordt uitgevoerd in Azure portal gebruiken voor het beheren van gegevens die zijn opgeslagen op meerdere apparaten en op meerdere locaties. Dit is vooral handig in scenario's voor gedistribueerde vertakking. Houd er rekening mee dat u afzonderlijke exemplaren van de service StorSimple Device Manager voor het beheer van virtuele matrices en fysieke StorSimple-apparaten moet maken. Houd er ook rekening mee dat de virtuele matrix nu de nieuwe Azure portal in plaats van de klassieke Azure portal gebruikt.

![Cloud-gebaseerd opslagbeheer](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Locatie-onafhankelijke back-up
De virtuele matrix biedt cloudmomentopnamen een locatie-onafhankelijke, punt-in-time-kopie van een volume of share. Cloudmomentopnamen zijn standaard ingeschakeld en kunnen niet worden uitgeschakeld. Alle volumes en shares zijn back-up op hetzelfde moment via een enkele dagelijkse back-upbeleid en kunt u aanvullende ad-hoc back-ups van elk gewenst moment uitvoeren.

### <a name="data-protection-and-disaster-recovery"></a>Gegevens bescherming en herstel na noodgevallen
De virtuele matrix ondersteunt de volgende gegevensbescherming en herstel na noodgevallen:

* **Volume of share herstellen** – het herstellen als de nieuwe werkstroom gebruiken om te herstellen van een volume of share. Deze aanpak gebruiken om te herstellen van het hele volume of share.
* **Herstel op itemniveau** – Shares toestaan vereenvoudigde toegang tot een recente back-ups. U kunt eenvoudig een afzonderlijk bestand herstellen vanaf een speciale *.backup* map die beschikbaar zijn in de cloud. Deze mogelijkheid terugzetten is door gebruikers geïnitieerde en geen interventie van een beheerder is vereist.
* **Herstel na noodgevallen** – de failover-functionaliteit gebruiken om te herstellen van alle volumes of shares op een nieuwe virtuele matrix. U de nieuwe virtuele matrix maken en registreren bij de StorSimple Device Manager-service en vervolgens de oorspronkelijke virtuele matrix een failover. De nieuwe virtuele matrix wordt vervolgens wordt ervan uitgegaan dat de ingerichte resources.

## <a name="storsimple-virtual-array-components"></a>StorSimple Virtual Array onderdelen

De virtuele matrix omvat de volgende onderdelen:

* [Virtuele matrix](#virtual-array) – een opslagapparaat van hybride cloud op basis van een virtuele machine ingericht in uw gevirtualiseerde omgeving of de hypervisor.
* [StorSimple Device Manager-service](#storsimple-device-manager-service) ; een uitbreiding van de Azure-portal waarmee u een of meer StorSimple-apparaten beheren via één interface die toegankelijk is vanuit verschillende geografische locaties. De service StorSimple Device Manager kunt u maken en beheren van services, weergeven en beheren van apparaten en waarschuwingen en beheren van volumes, shares en momentopnamen van bestaande.
* [Lokale webinterface](#local-web-user-interface) : een web gebaseerde gebruikersinterface die wordt gebruikt voor het configureren van het apparaat, zodat u kunt verbinding met het lokale netwerk maken en vervolgens het apparaat bij de StorSimple Device Manager-service registreren. 
* [Opdrachtregelinterface](#command-line-interface) – A Windows PowerShell-interface waarmee u kunt een ondersteuningssessie voor de voor de virtuele matrix starten.
  De volgende secties beschrijven elk van deze onderdelen in meer detail en wordt uitgelegd hoe de oplossing zorgt ervoor dat gegevens, opslag kan worden toegewezen en vereenvoudigt het uitvoeren van beheer van opslag- en gegevensbeveiliging.

### <a name="virtual-array"></a>Virtuele matrix

De virtuele matrix is een opslagoplossing van één knooppunt primaire opslag, waarmee communicatie met opslag in de cloud beheert, en helpt om te controleren of de beveiliging en geheimhouding van alle gegevens die zijn opgeslagen op het apparaat.

De virtuele matrix is beschikbaar in één model dat is gedownload. De virtuele matrix heeft een maximale capaciteit van 6.4 TB op het apparaat (met een onderliggende opslagvereiste van 8 TB) en de met inbegrip van 64 TB opslagruimte in de cloud.

De virtuele matrix heeft de volgende functies:

* Dit is kosteneffectief. Het maakt gebruik van uw bestaande virtualisatie-infrastructuur en kunnen worden geïmplementeerd op uw bestaande Hyper-V of VMware-hypervisor.
* Deze bevindt zich in het datacenter en kan worden geconfigureerd als een iSCSI-server of een bestandsserver.
* Dit is geïntegreerd met de cloud.
* Back-ups worden opgeslagen in de cloud, die kunt herstel na noodgevallen vergemakkelijken en herstel op itemniveau (ILR) vereenvoudigen.
* U kunt updates toepassen op de virtuele matrix, net zoals u ze op een fysiek apparaat toepassen wilt.

> [!NOTE]
> Een virtuele matrix kan niet worden uitgebreid. Daarom is het belangrijk dat u voldoende opslagruimte inrichten bij het maken van de virtuele matrix.

### <a name="storsimple-device-manager-service"></a>StorSimple-apparaatbeheerservice

Microsoft Azure StorSimple biedt een web gebaseerde gebruikersinterface, de StorSimple Device Manager-service, waarmee u centraal StorSimple om opslag te beheren. De service StorSimple Device Manager kunt u de volgende taken uitvoeren:

* Meerdere virtuele StorSimple-matrices beheren vanaf één service.
* Configureren en beheren van instellingen voor virtuele StorSimple-matrices. (Versleuteling in de cloud is afhankelijk van de Microsoft Azure-API's.)
* Referenties voor het opslagaccount en eigenschappen configureren.
* Configureren en beheren van volumes of shares.
* Back-up en herstel van gegevens op volumes of shares.
* Prestaties controleren.
* Controleer de instellingen en mogelijke problemen te identificeren.

U de StorSimple Device Manager-service worden uitgevoerd dagelijkse beheer van uw virtuele array gebruiken.

Ga voor meer informatie naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Lokale webinterface

De virtuele matrix bevat een webgebaseerde gebruikersinterface die wordt gebruikt voor eenmalige configuratie en registratie van het apparaat met de StorSimple Device Manager-service. U kunt het afsluiten en opnieuw opstarten van de virtuele matrix, Voer diagnostische tests uit, software bijwerken, wijzigen van het beheerderswachtwoord, het systeemlogboek in logboeken bekijken en neem contact op met Microsoft Support om een serviceaanvraag.

Ga voor informatie over het gebruik van het web gebaseerde gebruikersinterface naar [het web gebaseerde gebruikersinterface gebruiken voor het beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Opdrachtregelinterface

De opgenomen Windows PowerShell-interface kunt u het starten van een ondersteuningssessie met Microsoft Support zodat deze u problemen op te lossen die op uw virtuele array optreden mogelijk kunnen ondersteunen.

## <a name="storage-management-technologies"></a>Storage management-technologieën

Naast de virtuele matrix en andere onderdelen, de StorSimple-oplossing gebruikmaakt van de volgende softwaretechnologieën bieden snelle toegang tot belangrijke gegevens, opslag kan worden verkleind en gegevens die zijn opgeslagen op uw virtuele matrix:

* [Automatische opslaglagen](#automatic-storage-tiering) 
* [Lokaal vastgemaakt shares en volumes](#locally-pinned-shares-and-volumes)
* Compressie van gegevens voor Ontdubbeling en lagen of een back-up naar de cloud 
* [Geplande en on-demand back-ups](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatische opslaglagen
De virtuele matrix maakt gebruik van een nieuw cloudlagen mechanisme voor het beheren van opgeslagen gegevens op de virtuele matrix en in de cloud. Er zijn slechts twee lagen: de lokale virtuele matrix en Azure-cloudopslag. De StorSimple Virtual Array automatisch zorgt ervoor dat gegevens in de lagen op basis van een heatmap, houdt u huidige gebruik, leeftijd en relaties met andere gegevens. Gegevens die het meest actief (meest belaste) lokaal is opgeslagen, terwijl minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. (Alle back-ups worden opgeslagen in de cloud.) StorSimple wordt aangepast en gegevens opnieuw worden ingedeeld en toewijzingen van opslag als de gebruikspatronen wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actieve na verloop van tijd. Omdat het progressief minder actieve, is het gelaagd uit naar de cloud. Als deze gegevens opnieuw geactiveerd wordt, is het gelaagd op de opslagmatrix.

Gegevens voor een bepaalde gelaagde bestandsshare of een volume kan een eigen lokale laag-ruimte (ongeveer 10% van de totale ingerichte ruimte voor deze share of het volume) worden gegarandeerd. Hoewel er hierdoor worden de beschikbare opslag voor de virtuele matrix voor deze share of het volume, zorgt u ervoor dat meerdere lagen voor één share of het volume wordt niet beïnvloed door de cloudlagen behoeften van de andere bestandsshares of volumes. Dus een erg druk werkbelasting op een share of het volume kan niet afdwingen dat alle andere werkbelastingen naar de cloud.

Gelaagde volumes die zijn gemaakt voor iSCSI-zijn een maximale lokale reservering van 200 GB, ongeacht de grootte van het volume.

![Automatische opslaglagen](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> U kunt een als lokaal vastgemaakt volume opgeven, in welk geval de gegevens op de virtuele matrix blijven en nooit wordt gelaagde in de cloud. Ga voor meer informatie naar [lokaal vastgemaakt shares en volumes](#locally-pinned-shares-and-volumes).


### <a name="locally-pinned-shares-and-volumes"></a>Lokaal vastgemaakt shares en volumes

U kunt de juiste shares en volumes als lokaal vastgemaakt maken. Deze mogelijkheid zorgt ervoor dat gegevens die zijn vereist voor essentiële toepassingen in de virtuele matrix blijft en nooit naar de cloud is gelaagd. Lokaal vastgemaakt shares en volumes hebben de volgende kenmerken:

* Ze vallen niet onder cloud latenties of problemen met de netwerkverbinding.
* Ze blijven profiteren van de StorSimple cloud back-ups en disaster recovery functies.

U kunt een lokaal vastgemaakt share of volume als gelaagde of een gelaagde share herstellen of volume als lokaal vastgemaakt. 

Voor meer informatie over lokaal vastgemaakte volumes, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van volumes](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Compressie van gegevens voor Ontdubbeling en lagen of een back-up naar de cloud

StorSimple maakt gebruik van Ontdubbeling en gegevens-compressie om verder te beperken opslagvereisten in de cloud. Ontdubbeling vermindert de totale hoeveelheid gegevens die zijn opgeslagen door het elimineren van redundantie in de opgeslagen gegevens-set. Als gegevens veranderen, StorSimple de ongewijzigde gegevens worden genegeerd en wordt alleen de wijzigingen vastgelegd. StorSimple wordt bovendien de hoeveelheid opgeslagen gegevens door te identificeren en verwijderen van dubbele gegevens.

> [!NOTE]
> Gegevens die zijn opgeslagen op de virtuele matrix is niet ontdubbeld of gecomprimeerd. Alle gegevensontdubbeling en compressie vindt plaats voordat de gegevens worden verzonden naar de cloud.

### <a name="scheduled-and-on-demand-backups"></a>Geplande en on-demand back-ups

Functies voor gegevensbescherming StorSimple kunnen u back-ups op aanvraag maken. Een standaard back-upschema Bovendien zorgt ervoor dat de back-up dagelijks. Back-ups worden genomen in de vorm van incrementele momentopnamen die zijn opgeslagen in de cloud. Momentopnamen, die alleen de wijzigingen vastgelegd sinds de laatste back-up, worden gemaakt en hersteld snel. Deze momentopnamen kunnen worden in noodgevallen herstelscenario's cruciaal omdat ze vervangen door secundaire opslag-systemen (zoals tapeback-up), en kunnen u gegevens naar uw datacenter of naar andere sites indien nodig herstellen.

## <a name="managing-personal-information"></a>Persoonlijke gegevens beheren

De StorSimple Device Manager voor virtuele serie verzamelt persoonlijke gegevens in twee belangrijke exemplaren:
 - Ontvang een waarschuwing gebruikersinstellingen waar e-mailadressen van gebruikers zijn geconfigureerd. Deze informatie kan worden verwijderd door de beheerder. 
 - Gebruikers die toegang krijgen tot de gegevens die zich bevinden op de shares. Een lijst met gebruikers die toegang de sharegegevens tot krijgen wordt weergegeven en kan worden geëxporteerd. Deze lijst wordt ook verwijderd wanneer de shares wordt verwijderd.

Raadpleeg voor meer informatie de [Microsoft Privacy-policy op Vertrouwenscentrum](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [voorbereiden van de portal virtuele matrix](storsimple-virtual-array-deploy1-portal-prep.md).
