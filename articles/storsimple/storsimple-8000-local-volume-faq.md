---
title: StorSimple lokaal vastgemaakt volumes Veelgestelde vragen over | Microsoft Docs
description: Vindt u antwoorden op veelgestelde vragen over StorSimple lokaal vastgemaakt volumes.
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: aa69d8b07d31b5cf0386e34c113475cbf4191891
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58013798"
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokaal vastgemaakt volumes: veelgestelde vragen (FAQ)
## <a name="overview"></a>Overzicht
Hier volgen vragen en antwoorden die u hebben wellicht wanneer u een StorSimple lokaal vastgemaakt volume maakt, converteert van een gelaagd volume naar een lokaal vastgemaakt volume (en omgekeerd), of back-up en herstellen van een lokaal vastgemaakt volume.

Vragen en antwoorden zijn gerangschikt in de volgende categorieën

* Het maken van een lokaal vastgemaakt volume
* Back-ups van een lokaal vastgemaakt
* Een gelaagd volume converteren naar een lokaal vastgemaakt volume
* Een lokaal vastgemaakt volume herstellen
* Failover wordt uitgevoerd een lokaal vastgemaakt volume

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Vragen over het maken van een lokaal vastgemaakt volume
**V:** Wat is de maximale grootte van een lokaal vastgemaakt volume dat ik op de 8000-serie-apparaten kunt maken?

**Een** op apparaten waarop StorSimple 8000 serie Update 3.0 wordt uitgevoerd, kunt u lokaal vastgemaakte volumes inrichten tot maximaal 8,5 TB en gelaagde volumes op het 8100-apparaat tot 200 TB. Op een groter 8600-apparaat kunt u lokaal vastgemaakte volumes inrichten tot 22,5 TB en gelaagde volumes tot maximaal 500 TB.

**V:** Kan ik mijn 8100-apparaat naar Update 3.0 onlangs hebt bijgewerkt en wanneer ik wil een lokaal vastgemaakt volume maakt, de maximale grootte beschikbaar is alleen 6 TB opslagruimte en niet 8,5 TB. Waarom niet kan ik een volume van 8,5 TB maken?

**Een** als update 3.0 op uw apparaat wordt uitgevoerd, kunt u inrichten lokaal vastgemaakte volumes tot maximaal 8,5 TB en gelaagde volumes op het 8100-apparaat tot 200 TB. Als uw apparaat reeds volumes, gelaagde wordt de beschikbare schijfruimte voor het maken van een lokaal vastgemaakt volume proportioneel lager is dan de maximale limiet. Bijvoorbeeld, als van gelaagde volumes van circa 106 TB al op uw 8100-apparaat (dit is de helft van de gelaagde capaciteit) zijn ingericht, vervolgens de maximale grootte van een lokaal volume dat u op het 8100-apparaat maken kunt echter navenant beperkt tot 4 TB (ongeveer de helft van de maximale lokaal vastgemaakt volumecapaciteit).

Aangezien lokale ruimte op het apparaat wordt gebruikt voor het hosten van de werkset van gelaagde volumes, wordt de beschikbare ruimte voor het maken van een lokaal vastgemaakt volume als het apparaat volumes gelaagde verminderd. Daarentegen evenredig met het maken van een lokaal vastgemaakt volume, wordt de beschikbare ruimte voor gelaagde volumes verminderd. De volgende tabel geeft een overzicht van de beschikbare gelaagde capaciteit op het 8100- en 8600-apparaten wanneer lokaal vastgemaakte volumes worden gemaakt.

#### <a name="update-30"></a>Update 3.0 

| Ingerichte capaciteit van lokaal vastgemaakte volumes | Beschikbare capaciteit in te richten voor gelaagde volumes - 8100 | Beschikbare capaciteit in te richten voor gelaagde volumes - 8600 |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176,5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8,5 TB |0 TB |311.1 TB |
| 10 TB |N.v.t. |277.8 TB |
| 15 TB |N.v.t. |166.7 TB |
| 22.5 TB |N.v.t. |0 TB |

**V:** Waarom is een langdurige bewerking lokaal vastgemaakt volume maken?

**A:** Lokaal vastgemaakte volumes worden compact ingericht. Om ruimte te maken op de lokale lagen van het apparaat, mogelijk bepaalde gegevens van bestaande gelaagde volumes naar de cloud worden doorgegeven tijdens het inrichtingsproces. En omdat dit is de grootte van het volume wordt ingericht, de bestaande gegevens op uw apparaat en de beschikbare bandbreedte naar de cloud, afhankelijk van de gebruikte tijd voor het maken van een lokaal volume mogelijk enkele uren.

**V:** Hoe lang duurt het een lokaal vastgemaakt volume maakt?

**A:** Omdat de lokaal vastgemaakte volumes worden compact ingericht, kunnen sommige bestaande gegevens van gelaagde volumes naar de cloud worden gepusht tijdens het inrichtingsproces. De gebruikte tijd voor het maken van een lokaal vastgemaakt volume is daarom afhankelijk van meerdere factoren, onder andere de grootte van het volume, worden de gegevens op uw apparaat en de beschikbare bandbreedte. Op het apparaat van een opnieuw worden geïnstalleerd dat geen volumes heeft, is de tijd voor het maken van een lokaal vastgemaakt volume ongeveer tien minuten per terabyte aan gegevens. Het maken van lokale volumes duurt maar enkele uren op basis van de factoren die hierboven is uitgelegd op een apparaat dat wordt gebruikt.

**V:** Ik wil een lokaal vastgemaakt volume maakt. Zijn er best practices moet ik rekening houden met?

**A:** Lokaal vastgemaakte volumes zijn geschikt voor workloads waarvoor lokale garanties van gegevens te allen tijde is vereist en zijn gevoelig voor latentie in de cloud. Tijdens het gebruik van de lokale volumes voor elk van uw workloads overweegt, houd rekening met het volgende:

* Lokaal vastgemaakte volumes worden compact ingericht en het maken van lokale volumes heeft gevolgen voor de beschikbare ruimte voor gelaagde volumes. Daarom stellen we voor u begint met volumes kleiner en omhoog schalen wanneer uw opslag vereiste toeneemt.
* Inrichting van de lokale volumes is een langdurige bewerking die kan betrekking hebben op de bestaande gegevens pushen van gelaagde volumes naar de cloud. Als gevolg hiervan kan optreden verminderde prestaties op deze volumes.
* Inrichting van de lokale volumes is een tijdrovende bewerking. De werkelijke tijd voor het afhankelijk is van meerdere factoren: de grootte van het volume wordt ingericht, de gegevens op uw apparaat en de beschikbare bandbreedte. Als u hebt geen back-ups uw bestaande volumes op de cloud, zijn het maken van volume is trager. We raden dat u cloudmomentopnamen van uw bestaande volumes uitvoeren voordat u een lokaal volume inricht.
* U kunt bestaande gelaagde volumes converteren naar lokaal vastgemaakte volumes en deze conversie een hoeveelheid omvat het inrichten van ruimte op het apparaat voor de resulterende lokaal vastgemaakt volume (naast het gedeactiveerd gelaagde gegevens, indien van toepassing, vanuit de cloud). Dit is ook een langdurige bewerking die is afhankelijk van factoren die hebben we eerder hebben besproken. Het is raadzaam om de back-up van uw bestaande volumes voordat u conversie als het proces wordt ook langer duren als er bestaande volumes worden niet ondersteund. Verminderde prestaties tijdens dit proces kan ook optreden door uw apparaat.

Meer informatie over het [een lokaal vastgemaakt volume maken](storsimple-8000-manage-volumes-u2.md#add-a-volume)

**V:** Kan ik meerdere lokaal vastgemaakte volumes op hetzelfde moment maken?

**A:** Ja, maar een lokaal vastgemaakt volume maken en uitbreiding taken sequentieel worden verwerkt.

Lokaal vastgemaakte volumes worden compact ingericht en hiervoor wordt het maken van lokale ruimte op het apparaat (dit kan leiden tot bestaande gegevens van gelaagde volumes naar de cloud worden gepusht tijdens het inrichtingsproces). Dus als een taak uitgevoerd wordt, andere taken voor het maken van het lokale volume wordt in de wachtrij geplaatst totdat deze taak is voltooid.

Op dezelfde manier als een bestaande lokale volume wordt uitgebreid of als een gelaagd volume wordt geconverteerd naar een lokaal vastgemaakt volume, klikt u vervolgens het maken van een nieuw lokaal vastgemaakt volume is in de wachtrij geplaatst totdat de vorige taak is voltooid. De grootte van een lokaal vastgemaakt volume uitbreiden, moet u de uitbreiding van de bestaande lokale ruimte die voor dat volume. De conversie van een gelaagde naar een lokaal vastgemaakt volume omvat ook het maken van lokale ruimte voor de resulterende lokaal volume vastgemaakt. In beide van deze bewerkingen, maken of uitbreiding van de lokale ruimte is een lange uitgevoerd taak.

U vindt deze taken in de **taken** blade van de StorSimple Device Manager-service. De taak die actief wordt verwerkt, wordt voortdurend bijgewerkt om de voortgang van het inrichten van ruimte weer te geven. De resterende taken in de lokaal vastgemaakte volume zijn gemarkeerd als die wordt uitgevoerd, maar de voortgang is vastgelopen en ze zijn opgenomen in de volgorde waarin dat ze zijn in de wachtrij geplaatst.

**V:** Ik heb een lokaal vastgemaakt volume verwijderd. Waarom zie ik niet de geregenereerde ruimte in de beschikbare ruimte wordt weergegeven wanneer ik wil een nieuw volume maken?

**A:** Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Device Manager-Service-updates de lokale ruimte die beschikbaar ongeveer elk uur. We raden dat u wachten tot een uur voordat u probeert te maken van het nieuwe volume.

**V:** Worden lokaal vastgemaakte volumes op het cloudapparaat ondersteund?

**A:** Lokaal vastgemaakte volumes worden niet ondersteund op het cloudapparaat (8010 en 8020-apparaten voorheen aangeduid als het virtuele StorSimple-apparaat).

**V:** Kan ik de Azure PowerShell-cmdlets gebruiken om te maken en beheren van lokaal vastgemaakte volumes?

**A:** Nee, u lokaal vastgemaakte volumes via Azure PowerShell-cmdlets (alle volumes die u via Azure PowerShell maakt is gelaagd) niet maken. Ook het is raadzaam gebruik de Azure PowerShell-cmdlets niet te wijzigen van de eigenschappen van een lokaal vastgemaakt volume, omdat het de ongewenst effect van het wijzigen van het volumetype zal hebben tot gelaagde.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Vragen over back-ups van een lokaal vastgemaakt volume
**V:** Zijn lokale momentopnamen van lokaal vastgemaakte volumes die worden ondersteund?

**A:** Ja, u kunt uitvoeren op lokale momentopnamen van uw lokaal vastgemaakte volumes. Echter ten zeerste het is raadzaam dat u regelmatig back-up van uw lokaal vastgemaakte volumes met behulp van cloudmomentopnamen om ervoor te zorgen dat uw gegevens in de deze na een noodgeval worden beschermd.

Houd er rekening mee dat lokale momentopnamen van lokaal vastgemaakte volumes ook onderverdelen in uit naar de cloud lagen kunnen en zijn niet noodzakelijkerwijs blijven in de lokale laag van het apparaat.

**V:** Zijn er richtlijnen voor het beheren van lokale momentopnamen voor lokaal vastgemaakte volumes?

**A:** Regelmatig lokale momentopnamen samen met een hoge frequentie van het gegevensverloop op de lokaal vastgemaakte volume kunnen leiden tot lokale ruimte op het apparaat snel wordt verbruikt en leiden tot gegevens van gelaagde volumes naar de cloud worden gepusht. Daarom stellen we voor dat u het nummer van de lokale momentopnamen minimaliseren.

**V:** Ik heb ontvangen een bericht waarin staat dat mijn lokale momentopnamen van lokaal vastgemaakte volumes mogelijk ongeldig worden gemaakt. Wanneer kan dit gebeuren?

**A:** Regelmatig lokale momentopnamen samen met een hoge frequentie van het gegevensverloop op de lokaal vastgemaakte volume kunnen leiden tot lokale ruimte op het apparaat snel wordt verbruikt. Als de lokale lagen van het apparaat intensief worden gebruikt, een uitgebreide cloud storing kan leiden tot het apparaat is vol en binnenkomende schrijven naar het volume kunnen leiden tot die van de momentopnamen (als er geen ruimte beschikbaar is voor het bijwerken van de momentopnamen om te verwijzen naar de oudere blokken van gegevens die zijn overschreven). In dit geval blijven de schrijfbewerkingen naar het volume worden geleverd, maar de lokale momentopnamen is mogelijk ongeldig. Er zijn geen gevolgen voor uw bestaande cloudmomentopnamen.

De waarschuwing waarschuwing is om u te waarschuwen dat een dergelijke situatie kan zich voordoen en zorg ervoor dat u hetzelfde adres tijdig dat door uw lokale momentopnamen schema's om minder frequente lokale momentopnamen te controleren of het verwijderen van oudere lokale momentopnamen die niet langer vereist zijn.

Als de lokale momentopnamen ongeldig gemaakt zijn, ontvangt u een informatiemelding om te informeren dat de lokale momentopnamen voor de specifieke back-upbeleid zijn ongeldig verklaard naast de lijst met tijdstempels van de lokale momentopnamen die ongeldig zijn gemaakt. Deze momentopnamen worden automatisch verwijderd en u niet langer kunnen deze bekijken in de **back-up catalogi** blade in Azure portal.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Vragen over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume
**V:** Ik ben sommige traagheid op het apparaat uitgevoerd tijdens het converteren van een gelaagd volume naar een lokaal vastgemaakt volume. Waarom is het probleem aanhoudt?

**A:** Het conversieproces bestaat uit twee stappen:

1. Inrichting van ruimte op het apparaat voor het snel-naar--geconverteerd lokaal vastgemaakt volume.
2. Alle gelaagde gegevens downloaden vanuit de cloud om ervoor te zorgen lokale garanties.

Beide van deze stappen zijn lang bewerkingen die afhankelijk van de grootte van het volume wordt geconverteerd zijn, gegevens op het apparaat en de beschikbare bandbreedte. Als u bepaalde gegevens van bestaande gelaagde volumes kunnen worden gelekt naar de cloud als onderdeel van het inrichtingsproces, kan uw apparaat verminderde prestaties tijdens deze periode optreden. Bovendien worden tijdens de conversie kan langzamer als:

* Bestaande volumes zijn niet back-in de cloud. dus het is raadzaam om de dat back-up van uw volumes voordat u een conversie wordt gestart.
* Beleidsregels voor beperking van bandbreedte zijn toegepast, kan dit de beschikbare bandbreedte naar de cloud; beperken Daarom raden wij aan dat u hebt een speciale 40 Mbps of meer verbindingen naar de cloud.
* Tijdens de conversie kan enkele uren vanwege het meerdere factoren uiteengezet; duren Daarom is het raadzaam dat u deze bewerking niet pieken op tijdstippen of op een weekend om te voorkomen dat de impact op end consumenten uitvoeren.

Meer informatie over het [een gelaagd volume converteren naar een lokaal vastgemaakt volume](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)

**V:** Kan ik de conversiebewerking volume annuleren?

**A:** Nee, u kunt niet de annuleren de conversiebewerking eenmaal is gestart. Zoals beschreven in de vorige vraag, houd er rekening houden met de mogelijke prestatieproblemen die u mogelijk bij het proces stuit en volg de aanbevolen procedures die hierboven wordt weergegeven wanneer u van plan uw conversie bent.

**V:** Wat gebeurt er met mijn volume als de conversiebewerking mislukt?

**A:** Volumeconversie kan mislukken vanwege verbindingsproblemen met een cloud. Het apparaat niet meer het conversieproces uiteindelijk na een bepaald aantal mislukte pogingen om de gelaagde gegevens vanuit de cloud. In zo'n geval moet het volumetype blijft het volume brontype vóór de conversie, en:

* Er verschijnt een kritieke waarschuwing voor een melding van de fout bij het converteren van het volume. Meer informatie over [waarschuwingen met betrekking tot de lokaal vastgemaakte volumes](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts)
* Als u een gelaagde naar een lokaal vastgemaakt volume converteren wilt, blijft het volume eigenschappen van een gelaagd volume vertonen gegevens mogelijk nog steeds bevindt zich op de cloud. Het is raadzaam dat u kunt oplossen door de problemen met de netwerkverbinding en voer vervolgens de conversiebewerking opnieuw uit.
* Op dezelfde manier als de conversie van een lokaal vastgemaakt naar een gelaagd volume is mislukt, hoewel het volume worden gemarkeerd als een lokaal vastgemaakt volume, werkt als een gelaagd volume (omdat de gegevens kan terechtgekomen naar de cloud). Deze blijft echter ruimte op de lokale lagen van het apparaat in beslag nemen. Deze ruimte is niet beschikbaar voor andere lokaal vastgemaakte volumes. Het is raadzaam om deze bewerking om ervoor te zorgen dat de volumeconversie voltooid is en de lokale ruimte op het apparaat kan worden vrijgemaakt opnieuw te proberen.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Vragen over het herstellen van een lokaal vastgemaakt volume
**V:** Lokaal vastgemaakte volumes hersteld direct?

**A:** Ja, lokaal vastgemaakte volumes worden meteen teruggezet. Als de informatie over de metagegevens voor het volume wordt opgehaald uit de cloud als onderdeel van de herstelbewerking, wordt het volume online is geplaatst en kan worden geopend door de host. Echter lager lokale garanties met betrekking tot het volume gegevens niet meer aanwezig totdat alle gegevens vanuit de cloud is gedownload en mogelijk is uw prestaties uitvallen op deze volumes voor de duur van het herstel.

**V:** Hoe lang duurt het een lokaal vastgemaakt volume herstellen?

**A:** Lokaal vastgemaakte volumes worden meteen teruggezet en online wordt gebracht zodra de volumegegevens voor metagegevens wordt opgehaald uit de cloud, terwijl de gegevens op het volume wordt voortgezet op de achtergrond worden gedownload. Dit laatste deel van de herstelbewerking--ophalen terug van de lokale garanties voor gegevens op het volume, is een langdurige bewerking en duurt enkele uren voor de gegevens naar het lokale opnieuw worden gemaakt. De tijd om uit te voeren hetzelfde is afhankelijk van meerdere factoren, zoals de grootte van het volume wordt hersteld en de beschikbare bandbreedte. Als het oorspronkelijke volume dat wordt hersteld, is verwijderd, worden extra tijd om te maken van de lokale ruimte op het apparaat als onderdeel van de herstelbewerking opnieuw worden uitgevoerd.

**V:** Ik wil mijn bestaande lokaal vastgemaakt volume herstellen naar een momentopname van een oudere (uitgevoerd wanneer het volume is gelaagd). Het volume worden hersteld als in dit geval lagen?

**A:** Nee, wordt het volume worden hersteld als een lokaal vastgemaakt volume. Hoewel de datums momentopname van de tijd waarop het volume is gelaagd, bij het terugzetten van bestaande volumes, StorSimple altijd maakt gebruik van het type van het volume op de schijf zoals dit zich op dit moment.

**V:** Kan ik mijn lokaal vastgemaakt volume onlangs hebt uitgebreid, maar ik wil nu de gegevens te herstellen naar een tijdstip waarop het volume kleiner dan de grootte is. Wordt herstel de grootte van het huidige volume en wordt die ik wil de grootte van het volume uitbreiden als het herstellen is voltooid?

**A:** Ja, het herstellen wordt de grootte van het volume, en moet u de grootte van het volume uitbreiden nadat het herstel is voltooid.

**V:** Kan ik het type van een volume wijzigen tijdens het terugzetten?

**A:** U kunt het volumetype Nee, niet wijzigen tijdens het terugzetten.

* Volumes die zijn verwijderd zijn als het type die zijn opgeslagen in de momentopname hersteld.
* Bestaande volumes worden teruggezet op basis van hun huidige type, ongeacht het type die zijn opgeslagen in de momentopname (Zie de vorige twee vragen).

**V:** Ik wil mijn lokaal vastgemaakt volume herstellen, maar ik een onjuiste punt in tijd momentopname opgehaald. Kan ik de huidige herstelbewerking annuleren?

**A:** Ja, kunt u een continue restore-bewerking annuleren. De status van het volume wordt teruggedraaid naar de status aan het begin van het herstel. Alle schrijfbewerkingen die zijn aangebracht aan het volume, terwijl het terugzetten werd uitgevoerd, gaan echter verloren.

**V:** Kan ik een herstelbewerking uit op een van mijn lokaal vastgemaakte volumes aan de slag en nu zie ik een momentopname in mijn achterstand catalogus die ik niet nu maken. Wat is dit gebruikt voor?

**A:** Dit is de tijdelijke momentopname die is gemaakt voordat de herstelbewerking en wordt gebruikt voor ongedaan maken als de terugzetbewerking is geannuleerd of mislukt. Deze momentopname; niet verwijderen Deze worden automatisch verwijderd wanneer het herstel voltooid is. Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd.

**V:** Kan ik een lokaal vastgemaakt volume klonen?

**A:** Ja, dat is mogelijk. Het lokaal vastgemaakte volume wordt echter als een gelaagd volume standaard worden gekloond. Meer informatie over het [een lokaal vastgemaakt volume klonen](storsimple-8000-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Vragen over de failover wordt uitgevoerd een lokaal vastgemaakt volume
**V:** Ik wil mijn apparaat naar een ander fysiek apparaat een failover. Wordt mijn lokaal vastgemaakte volumes worden kan niet meer dan lokaal vastgemaakt of lagen?

**A:** De lokaal vastgemaakte volumes failover is uitgevoerd als lokaal vastgemaakt als het doelapparaat StorSimple 8000 serie update 3 of hoger wordt uitgevoerd.

Meer informatie over [failover en herstel na Noodgevallen van lokaal vastgemaakt volumes tussen versies](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**V:** Worden lokaal vastgemaakte volumes meteen teruggezet tijdens herstel na noodgeval (DR)?

**A:** Ja, lokaal vastgemaakte volumes worden teruggezet direct tijdens de failover. Als de informatie over de metagegevens voor het volume wordt opgehaald uit de cloud als onderdeel van de failoverbewerking, wordt het volume online is geplaatst op het doelapparaat en kan worden geopend door de host. Ondertussen blijft de volumegegevens downloaden op de achtergrond en verminderde prestaties op deze volumes voor de duur van de failover kan optreden.

**V:** Ik zie de failovertaak is voltooid, hoe kan ik de voortgang van lokaal vastgemaakte volume dat wordt hersteld volgen op het doelapparaat?

**A:** Tijdens een failoverbewerking, worden de failover-taak is gemarkeerd als voltooid nadat alle volumes in de failoverset met zijn direct teruggezet en online worden gebracht op het doelapparaat. Dit omvat alle lokaal vastgemaakte volumes die mogelijk een failover zijn; lokale garanties van de gegevens wordt alleen zijn echter beschikbaar wanneer de gegevens voor het volume is gedownload. Deze wordt uitgevoerd voor elk lokaal vastgemaakt volume dat door de bewaking van de bijbehorende hersteltaken die zijn gemaakt als onderdeel van de failover is mislukt, kunt u bijhouden. Deze taken aparte herstelbewerking wordt alleen voor lokaal vastgemaakte volumes worden gemaakt.

**V:** Kan ik het type van een volume wijzigen tijdens de failover?

**A:** U kunt het volumetype Nee, niet wijzigen tijdens een failover. Als u mislukken over naar een ander fysiek apparaat met StorSimple 8000 serie update 3, de volumes failover is uitgevoerd op basis van het volumetype dat is opgeslagen in de momentopname.

**V:** Kan ik een volumecontainer met lokaal vastgemaakte volumes met het cloudapparaat failover?

**A:** Ja, dat is mogelijk. De lokaal vastgemaakte volumes failover wordt als gelaagde volumes. Meer informatie over [failover en herstel na Noodgevallen van lokaal vastgemaakt volumes tussen versies](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover)

