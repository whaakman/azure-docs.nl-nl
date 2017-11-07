---
title: Overzicht van Microsoft Azure StorSimple virtuele matrix | Microsoft Docs
description: "Beschrijft de StorSimple virtuele matrix, een geïntegreerde opslagoplossing die opslagtaken tussen een lokale virtuele-matrix en Microsoft Azure cloud-opslag beheert."
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/16/2017
ms.author: alkohli
ms.openlocfilehash: b9a5797751fa970c569c93e5efe300d4d74319ce
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>Inleiding tot het virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

De Microsoft Azure StorSimple virtuele matrix is een geïntegreerde opslagoplossing die beheert opslagtaken tussen een lokale virtuele-matrix uitgevoerd in een hypervisor en Microsoft Azure cloud-opslag. De virtuele matrix is een efficiënte, rendabele en eenvoudig beheerde bestandsserver of iSCSI-serveroplossing die veel van de problemen en de kosten in verband met enterprise-opslag en gegevensbeveiliging. De virtuele matrix is met name geschikt voor de opslag van minder vaak gebruikte gegevensarchivering.

In dit artikel biedt een overzicht van de virtuele matrix - er zijn andere bronnen:

* Zie voor aanbevolen procedures [aanbevolen procedures voor virtuele StorSimple-matrix](storsimple-ova-best-practices.md).
* Voor een overzicht van de apparaten uit de StorSimple 8000 serie, gaat u naar [StorSimple 8000-serie: een oplossing voor hybride cloud](storsimple-overview.md).
* Voor informatie over de apparaten van de StorSimple 5000/7000-serie, gaat u naar [Online-Help voor StorSimple](http://onlinehelp.storsimple.com/).

De virtuele matrix ondersteunt het iSCSI- of Server Message Block (SMB)-protocol. Het wordt uitgevoerd op uw bestaande infrastructuur van de hypervisor en biedt tiering naar de cloud, cloud back-up, snel herstel, herstel op itemniveau en herstelfuncties na noodgevallen.

De volgende tabel geeft een overzicht van de belangrijke functies van de virtuele StorSimple-matrix.

| Functie | StorSimple Virtual Array |
| --- | --- |
| Vereisten voor installatie |Maakt gebruik van virtualisatie-infrastructuur (Hyper-V- of VMware) |
| Beschikbaarheid |Eén knooppunt |
| Totale capaciteit (met inbegrip van de cloud) |Maximaal 64 TB bruikbare capaciteit per virtuele matrix |
| Lokale capaciteit |390 GB aan bruikbare capaciteit 6.4 TB per virtuele matrix (nodig voor het inrichten van 500 GB 8 TB aan schijfruimte) |
| Systeemeigen protocollen |iSCSI- of SMB |
| Beoogde hersteltijd (RTO) |iSCSI: ongeacht de grootte van minder dan twee minuten |
| Recovery Point Objective (RPO) |Dagelijkse back-ups en back-ups op aanvraag |
| Opslaglagen |Maakt gebruik van Breng toewijzing om te bepalen welke gegevens moeten tiers worden verdeeld, in- of |
| Ondersteuning |Virtualisatie-infrastructuur wordt ondersteund door de leverancier |
| Prestaties |Varieert afhankelijk van de onderliggende infrastructuur |
| Gegevensmobiliteit |Op hetzelfde apparaat kunt herstellen of itemniveau herstel (bestandsserver) |
| Opslaglagen |Opslag van de lokale hypervisor en cloud |
| Grootte van de bestandsshare |Lagen: tot 20 TB; lokaal vastgemaakt: tot 2 TB |
| De grootte van volume |Gelaagde: 500 GB tot 5 TB; lokaal vastgemaakt: 50 GB tot 200 GB <br> Maximum aantal lokale reservering voor gelaagde volumes is 200 GB. |
| Momentopnamen |Crashconsistent |
| Herstel op itemniveau |Ja; gebruikers kunnen herstellen van shares |

## <a name="why-use-storsimple"></a>Waarom StorSimple gebruiken?

StorSimple verbindt gebruikers en -servers naar Azure-opslag in minuten, er zijn geen wijzigingen in de toepassing.

De volgende tabel worden enkele van de belangrijkste voordelen van het virtuele StorSimple-matrix-oplossing beschreven.

| Functie | Voordeel |
| --- | --- |
| Transparante-integratie |De virtuele matrix ondersteunt iSCSI- of SMB-protocol. De verplaatsing van gegevens tussen de lokale laag en de cloudlaag is naadloze en transparant voor de gebruiker. |
| Lagere opslagkosten |Met StorSimple, moet u voldoende lokale opslag om te voldoen aan de huidige aanvragen voor de meest gebruikte gegevens hot inrichten. Wanneer opslag groeien behoeften, cloud StorSimple lagen koude gegevens in rendabele opslag. De gegevens worden ontdubbeld en gecomprimeerd voordat verzonden naar de cloud naar opslagvereisten en onkosten verder te beperken. |
| Vereenvoudigd opslagbeheer |StorSimple biedt gecentraliseerd beheer in de cloud met behulp van Apparaatbeheer voor StorSimple om meerdere apparaten te beheren. |
| Verbeterde noodherstel en naleving |Noodherstel sneller vergemakkelijkt StorSimple door de metagegevens onmiddellijk terugzetten en herstellen van de gegevens zo nodig. Dit betekent dat met het normale bewerkingen kunnen doorgaan met minimale onderbrekingen. |
| Gegevensmobiliteit |Gegevens naar de cloud gelaagde toegankelijk is vanaf andere sites voor herstel- en migratiedoeleinden. Houd er rekening mee dat u gegevens alleen naar de oorspronkelijke virtuele matrix terugzetten kunt. U kunt echter disaster recovery functies gebruiken om terug te zetten van de volledige virtuele array naar een andere virtuele matrix. |

## <a name="storsimple-workload-summary"></a>StorSimple workloadoverzicht

Hieronder wordt een overzicht van ondersteunde StorSimple werkbelastingen in een tabel.

|Scenario     |Workload     |Ondersteund      |Beperkingen               |
|-------------|-------------|---------------|---------------------------|
|ROBO  |Delen van bestanden     |Ja      |Zie [maximaal limieten voor bestandsserver](storsimple-ova-limits.md).<br></br>Zie [systeemvereisten voor ondersteunde versies van SMB-](storsimple-ova-system-requirements.md).| Alle versies     |
|Cloud archiveren  |Archivering bestanden delen     |Ja      |Zie [maximaal limieten voor bestandsserver](storsimple-ova-limits.md).<br></br>Zie [systeemvereisten voor ondersteunde versies van SMB-](storsimple-ova-system-requirements.md).| Alle versies     |

De virtuele StorSimple-matrix is het meest geschikt voor minder vaak gebruikte gegevens. Terwijl de virtuele matrix een lokale cache heeft voor de prestaties verbeteren, gebruikers ervan uitgaan dat het apparaat services-bestanden op de laagste categorie van opslag (de cloud). Elke virtuele matrix kunt geschreven en gelezen naar Azure-opslag met ongeveer 100 Mbps. Deze koppeling wordt gedeeld voor alle aanvragen die afkomstig zijn in het apparaat en kan een knelpunt geworden zoals weergegeven in het onderstaande diagram.

![Cloud archiveren](./media/storsimple-ova-overview/cloud-archiving.png)

Wanneer meerdere gelijktijdige gebruikers toegang krijgen de virtuele-matrix tot, delen ze allemaal de verbinding met Azure ertoe leidt dat een lagere prestaties. Er is geen gegarandeerde prestaties per gebruiker en het apparaat verwerkt afzonderlijke aanvragen naarmate ze binnenkomen.

Virtuele StorSimple-matrix is niet geschikt voor werkbelastingen waarvoor hoge beschikbaarheid. De virtuele matrix is een enkel knooppunt-apparaat dat downtime optreedt wanneer software-updates zijn geïnstalleerd. Beheerders moeten plannen voor een periode van 30 minuten 3 - 4 keer per jaar.

## <a name="workflows"></a>Werkstromen

De virtuele StorSimple-matrix is met name geschikt is voor de volgende werkstromen:

* [Cloud-gebaseerd opslagbeheer](#cloud-based-storage-management)
* [Locatie-onafhankelijke back-up](#location-independent-backup)
* [Data protection en herstel na noodgevallen](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>cloud-gebaseerd opslagbeheer
U kunt de Apparaatbeheer StorSimple-service in de Azure portal gebruiken voor het beheren van gegevens die zijn opgeslagen op verschillende apparaten en op meerdere locaties. Dit is vooral nuttig in scenario's voor gedistribueerde vertakking. Houd er rekening mee dat u afzonderlijke exemplaren van de service Manager voor StorSimple-apparaat om matrices virtuele en fysieke StorSimple-apparaten te beheren moet maken. Houd er ook rekening mee dat de virtuele matrix nu de nieuwe Azure portal in plaats van de klassieke Azure portal gebruikt.

![cloud-gebaseerd opslagbeheer](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Locatie-onafhankelijke back-up
Met de virtuele matrix voorzien cloudmomentopnamen in een locatie-onafhankelijke, punt in tijd kopie van een volume of share. Cloudmomentopnamen zijn standaard ingeschakeld en kunnen niet worden uitgeschakeld. Alle volumes en shares zijn back-up op hetzelfde moment via een enkel dagelijkse back-upbeleid en u extra ad-hoc back-ups zo nodig kunt nemen.

### <a name="data-protection-and-disaster-recovery"></a>Data protection en herstel na noodgevallen
De virtuele matrix ondersteunt de volgende gegevensbeveiliging en herstel na noodgevallen:

* **Volume of share terugzetten** – het herstellen als de nieuwe werkstroom gebruiken om te herstellen van een volume of share. Gebruik deze benadering voor het herstellen van de hele volume of share.
* **Item-level recovery** – Shares vereenvoudigde toegang tot recente back-ups toestaan. U kunt gemakkelijk een afzonderlijk bestand herstellen vanaf een speciale *.backup* map die beschikbaar zijn in de cloud. Deze mogelijkheid terugzetten is door gebruikers geïnitieerde en hoeft u geen beheerdersrechten ondernemen.
* **Herstel na noodgevallen** – de failoverfunctionaliteit gebruiken om te herstellen van alle volumes of shares naar een nieuwe virtuele matrix. U de nieuwe virtuele matrix maken en bij de service Manager voor StorSimple-apparaat registreren en vervolgens de oorspronkelijke virtuele matrix failover. De nieuwe virtuele matrix wordt vervolgens wordt ervan uitgegaan dat de ingerichte resources.

## <a name="storsimple-virtual-array-components"></a>Virtuele StorSimple-matrix-onderdelen

De virtuele matrix omvat de volgende onderdelen:

* [Virtuele matrix](#virtual-array) – een hybride cloud-opslagapparaat op basis van een virtuele machine ingericht in uw gevirtualiseerde omgeving of de hypervisor.
* [StorSimple-apparaat Manager-service](#storsimple-device-manager-service) – een uitbreiding van de Azure portal waarmee u een of meer StorSimple-apparaten beheren via een enkel webinterface die u vanuit verschillende geografische locaties openen kunt. U kunt de service Manager voor StorSimple-apparaat maken en beheren van services, weergeven en beheren van apparaten en waarschuwingen en beheren van volumes, shares en bestaande momentopnamen.
* [Lokale online gebruikersinterface](#local-web-user-interface) – een webgebaseerde gebruikersinterface die wordt gebruikt voor het apparaat configureren zodat het verbinding met het lokale netwerk maken en Registreer het apparaat met de service Manager voor StorSimple-apparaat. 
* [Opdrachtregelinterface](#command-line-interface) – A Windows PowerShell-interface waarmee u kunt een ondersteuningssessie te starten op de virtuele-matrix.
  De volgende secties beschrijven elk van deze onderdelen in meer detail en wordt uitgelegd hoe de oplossing rangschikt gegevens, wijst opslag en vereenvoudigt het beheer van opslag en gegevensbeveiliging.

### <a name="virtual-array"></a>Virtuele matrix

De virtuele matrix is een enkel knooppunt opslagoplossing die primaire opslag biedt, communicatie met de cloudopslag beheert en kunt u de beveiliging en de vertrouwelijkheid van alle gegevens die zijn opgeslagen op het apparaat.

De virtuele matrix is beschikbaar in één model dat is gedownload. De virtuele matrix heeft een maximale capaciteit van 6.4 TB op het apparaat (met een onderliggende eisen aan opslag van 8 TB) en de met inbegrip van 64 TB cloudopslag.

De virtuele matrix heeft de volgende functies:

* Betaalbaar is. Deze maken gebruik van uw bestaande virtualisatie-infrastructuur en kunnen worden geïmplementeerd op uw bestaande Hyper-V- of VMware-hypervisor.
* Deze bevindt zich in het datacenter en kan worden geconfigureerd als een server met iSCSI- of een bestandsserver.
* Het is geïntegreerd met de cloud.
* Back-ups worden opgeslagen in de cloud, die u kunt herstel na noodgevallen vergemakkelijken en herstel op itemniveau (ILR) vereenvoudigen.
* U kunt updates toepassen op de virtuele-matrix, net zoals u deze op een fysiek apparaat toepassen zou.

> [!NOTE]
> Een virtuele-matrix kan niet worden uitgebreid. Daarom is het belangrijk dat u voldoende opslagruimte inrichten bij het maken van de virtuele-matrix.

### <a name="storsimple-device-manager-service"></a>StorSimple-apparaat Manager-service

Microsoft Azure StorSimple biedt een webgebaseerde gebruikersinterface de Apparaatbeheer StorSimple-service, waarmee u centraal StorSimple om opslag te beheren. De service Manager voor StorSimple-apparaat kunt u de volgende taken uitvoeren:

* Meerdere virtuele StorSimple-matrices van een service beheren.
* Configureren en beheren van de beveiligingsinstellingen voor het virtuele StorSimple-matrices. (Versleuteling in de cloud is afhankelijk van de Microsoft Azure-API's.)
* Storage-accountreferenties en -eigenschappen configureren.
* Configureren en beheren van volumes of shares.
* Back-up en herstel van gegevens op volumes of shares.
* Prestaties controleren.
* Systeeminstellingen bekijken en mogelijke problemen te identificeren.

De service Manager voor StorSimple-apparaat kunt u dagelijks beheer van uw virtuele matrix uitvoeren.

Ga voor meer informatie naar [de Apparaatbeheer StorSimple-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Lokale online gebruikersinterface

De virtuele matrix bevat een webgebaseerde gebruikersinterface die wordt gebruikt voor eenmalige configuratie en de registratie van het apparaat met de service Manager voor StorSimple-apparaat. U kunt deze afsluiten en opnieuw opstarten van de virtuele-matrix, diagnostische tests uitvoeren software bijwerken, wijzigen van het beheerderswachtwoord van het apparaat, het systeemlogboek in logboeken bekijken en neem contact op met Microsoft Support om een serviceaanvraag.

Ga naar voor informatie over het gebruik van de gebruikersinterface voor het web gebaseerde [de webgebaseerde gebruikersinterface gebruiken voor het beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Opdrachtregelinterface

De opgenomen Windows PowerShell-interface kunt u een ondersteuningssessie met Microsoft Support starten zodat u ze kunt u problemen op te lossen die op uw virtuele matrix optreden kunnen.

## <a name="storage-management-technologies"></a>Beheer van opslagtechnologieën

Naast de virtuele matrix en andere onderdelen, het StorSimple-oplossing gebruikmaakt van de volgende softwaretechnologieën voor snel toegang bieden tot belangrijke gegevens, het beperken van opslagverbruik en het beveiligen van gegevens die zijn opgeslagen op uw virtuele matrix:

* [Automatische opslaglagen](#automatic-storage-tiering) 
* [Lokaal vastgemaakt shares en -volumes](#locally-pinned-shares-and-volumes)
* [Compressie van gegevens voor Ontdubbeling en lagen of een back-up naar de cloud](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [Geplande en on-demand back-ups](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatische opslaglagen
De virtuele matrix wordt een nieuwe lagen gebruikt voor het beheren van opgeslagen gegevens op de virtuele matrix en de cloud. Er zijn slechts twee lagen: het lokale virtuele matrix en de Azure-cloudopslag. De virtuele StorSimple-matrix worden gegevens automatisch in de lagen die op basis van een heatmap die huidige gebruik, leeftijd en relaties met andere gegevens bijhoudt geplaatst. Gegevens die het meest actief (gegevensbeheer) lokaal is opgeslagen, terwijl minder actieve en inactieve gegevens worden automatisch gemigreerd naar de cloud. (Alle back-ups worden opgeslagen in de cloud.) StorSimple aanpast en gegevens opnieuw worden ingedeeld en toewijzingen van de opslag als gebruikspatronen wijzigen. Sommige gegevens kan bijvoorbeeld worden minder actief gedurende een bepaalde periode. Als deze actief geleidelijk minder, wordt het gelaagde uit naar de cloud. Als dat dezelfde gegevens opnieuw geactiveerd wordt, is het lagen de opslagmatrix.

Gegevens voor een bepaalde gelaagde bestandsshare of het volume is gegarandeerd een eigen lokale laag ruimte (ongeveer 10% van de totale ingerichte ruimte voor deze share of het volume). Terwijl deze wordt beperkt door de beschikbare ruimte op de virtuele matrix voor die share of het volume, zorgt u ervoor dat lagen voor één share of het volume niet worden beïnvloed door de lagen behoeften van andere shares of volumes. Een bezet werkbelasting op een share of het volume kan niet dus afdwingen dat andere werkbelastingen naar de cloud.

Gelaagde volumes die zijn gemaakt voor iSCSI-hebben een maximale lokale reservering van 200 GB ongeacht de grootte van het volume.

![Automatische opslaglagen](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> U kunt een als lokaal vastgemaakt volume opgeven, in welk geval de gegevens op de virtuele matrix blijft en nooit is gelaagd naar de cloud. Ga voor meer informatie naar [lokaal vastgemaakt-shares en -volumes](#locally-pinned-shares-and-volumes).


### <a name="locally-pinned-shares-and-volumes"></a>Lokaal vastgemaakt shares en -volumes

U kunt de juiste shares en -volumes als lokaal vastgemaakt maken. Deze mogelijkheid zorgt ervoor dat gegevens die nodig zijn voor kritieke toepassingen in de virtuele matrix blijft en nooit naar de cloud is gelaagd. Lokaal vastgemaakt bestandsshares en volumes hebben de volgende functies:

* Ze vallen niet onder cloud latenties of problemen met de netwerkverbinding.
* Ze blijven profiteren van de StorSimple-cloud back-up en noodherstel herstelfuncties.

U kunt een lokaal vastgemaakt share of volume als lagen of een gelaagde share herstellen of volume als lokaal vastgemaakt. 

Voor meer informatie over lokaal vastgemaakte volumes, gaat u naar [de service Manager voor StorSimple-apparaat gebruiken voor het beheren van volumes](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Compressie van gegevens voor Ontdubbeling en lagen of een back-up naar de cloud

Compressie van gegevens voor Ontdubbeling en het StorSimple gebruikt voor het verder verminderen de opslagvereisten in de cloud. Ontdubbeling reduceert de totale hoeveelheid gegevens die zijn opgeslagen doordat redundantie in de opgeslagen gegevensset. Als gegevens worden gewijzigd, StorSimple ongewijzigde gegevens worden genegeerd en wordt alleen de wijzigingen vastgelegd. StorSimple vermindert bovendien de hoeveelheid opgeslagen gegevens door het identificeren en verwijderen van dubbele gegevens.

> [!NOTE]
> Gegevens die zijn opgeslagen op de virtuele matrix is geen ontdubbeld of gecomprimeerd. Alle Ontdubbeling en compressie vindt plaats voordat de gegevens worden verzonden naar de cloud.

### <a name="scheduled-and-on-demand-backups"></a>Geplande en on-demand back-ups

Functies voor gegevensbeveiliging StorSimple kunnen u back-ups op aanvraag maken. Een back-upschema standaard Bovendien zorgt ervoor dat de back-up dagelijks. Back-ups zijn gemaakt in de vorm van incrementele momentopnamen die zijn opgeslagen in de cloud. Momentopnamen die alleen de wijzigingen sinds de laatste back-up vastlegt, worden gemaakt en snel worden hersteld. Deze momentopnamen is zeer belangrijk in herstel na noodgevallen, omdat ze secundaire opslagsystemen (zoals tapeback-up vervangen) en kunnen u gegevens op uw datacenter of op alternatieve locaties herstellen indien nodig.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [voorbereiden van de portal virtuele matrix](storsimple-virtual-array-deploy1-portal-prep.md).
