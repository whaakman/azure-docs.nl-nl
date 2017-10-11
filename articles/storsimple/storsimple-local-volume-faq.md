---
title: StorSimple lokaal vastgemaakte volumes Veelgestelde vragen | Microsoft Docs
description: Vindt u antwoorden op veelgestelde vragen over StorSimple lokaal vastgemaakte volumes.
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 7a2f4b1a-4ac9-4ce1-9359-bd40a9cbbb5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/11/2017
ms.author: manuaery
ms.openlocfilehash: 22eb95bf0e3a20893acbb860ad10cfe6a3bcf088
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple lokaal vastgemaakte volumes: veelgestelde vragen (FAQ)
## <a name="overview"></a>Overzicht
Hieronder vindt u vragen en antwoorden die u hebben wellicht wanneer u een StorSimple lokaal vastgemaakt volume maakt, converteren van een gelaagd volume naar een lokaal vastgemaakt volume (en omgekeerd), of back-up en herstellen van een lokaal vastgemaakt volume.

Vragen en antwoorden worden gerangschikt in de volgende categorieën

* Een lokaal vastgemaakt volume maken
* Back-ups van een lokaal vastgemaakt
* Een gelaagd volume converteren naar een lokaal vastgemaakt volume
* Herstellen van een lokaal vastgemaakt volume
* Mislukte via een lokaal vastgemaakt volume

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Vragen over het maken van een lokaal vastgemaakt volume
**V:** Wat is de maximale grootte van een lokaal vastgemaakt volume dat ik op de apparaten uit de 8000 serie maken kunt?

**Een** op apparaten waarop StorSimple 8000 Series Update 3.0 wordt uitgevoerd, kunt u lokaal vastgemaakte volumes inrichten tot 8.5 TB en gelaagde volumes tot 200 TB op het 8100-apparaat. Op een groter 8600-apparaat kunt u lokaal vastgemaakte volumes inrichten tot 22,5 TB en gelaagde volumes tot maximaal 500 TB.    
Op apparaten waarop StorSimple 8000 serie Update 2.x, kunt u lokaal vastgemaakte volumes tot inrichten 8 TB en gelaagde volumes tot 200 TB op het 8100-apparaat. Op een groter 8600-apparaat kunt u lokaal vastgemaakte volumes inrichten tot 20 TB en gelaagde volumes tot maximaal 500 TB.   

**V:** Ik mijn 8100-apparaat onlangs hebt bijgewerkt naar Update 2.0 en wanneer ik wil een lokaal vastgemaakt volume maakt, de maximale grootte van de beschikbare alleen 6 TB en niet 8 TB. Waarom niet kan ik een volume van 8 TB maken?

**Een** als het apparaat kan update 2.0 wordt uitgevoerd, kunt u inrichten lokaal vastgemaakte volumes tot maximaal 8 TB en gelaagde volumes tot 200 TB op het 8100-apparaat. Als het apparaat reeds volumes, gelaagde wordt de beschikbare schijfruimte voor het maken van een lokaal vastgemaakt volume proportioneel lager is dan deze limiet. Bijvoorbeeld, als 100 TB van gelaagde volumes al op uw 8100-apparaat (dit is de helft van de capaciteit voor gelaagde) zijn ingericht, wordt klikt u vervolgens de maximale grootte van een lokaal volume dat kan worden gemaakt op het 8100-apparaat navenant verkleind tot 4 TB (ongeveer helft van de lokaal vastgemaakt volumecapaciteit).

Aangezien lokale ruimte op het apparaat wordt gebruikt voor het hosten van de werkset van gelaagde volumes, is de beschikbare ruimte voor het maken van een lokaal vastgemaakt volume als het apparaat volumes gelaagde verminderd. Als u daarentegen vermindert de beschikbare ruimte voor gelaagde volumes maken van een lokaal vastgemaakt volume proportioneel. De volgende tabellen geeft een overzicht van de beschikbare gelaagde capaciteit op het 8100 en 8600-apparaten wanneer lokaal vastgemaakte volumes worden gemaakt.

####<a name="update-30"></a>3.0 bijwerken 
| Lokaal vastgemaakte volumes ingerichte capaciteit | Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8100 | Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8600 |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176,5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |N.v.t. |277.8 TB |
| 15 TB |N.v.t. |166.7 TB |
| 22.5 TB |N.v.t. |0 TB |

####<a name="update-2x"></a>2.x bijwerken  
 | Lokaal vastgemaakte volumes ingerichte capaciteit | Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8100 | Beschikbare capaciteit worden ingericht voor gelaagde volumes - 8600 |  
 | --- | --- | --- |  
 | 0 |200 TB |500 TB |  
 | 1 TB |25 TB |475 TB |  
 | 4 TB |100 TB |400 TB |  
 | 8 TB |0 TB |300 TB |  
 | 10 TB |N.v.t. |250 TB |  
 | 15 TB |N.v.t. |125 TB |  
 | 20 TB |N.v.t. |0 TB |   

**V:** Waarom is een langdurige bewerking lokaal vastgemaakt volume maken? 

**A:** Lokaal vastgemaakte volumes compact ingericht. Om ruimte te maken op de lokale lagen van het apparaat, kunnen sommige gegevens van bestaande gelaagde volumes worden gepusht naar de cloud tijdens het inrichtingsproces. En omdat dit is afhankelijk van de grootte van het volume wordt ingericht, de bestaande gegevens op uw apparaat en de beschikbare bandbreedte voor de cloud, duurt het maken van een lokaal volume kan enkele uren.

**V:** Hoe lang duurt het maken van een lokaal vastgemaakt volume?

**A:** Omdat lokaal vastgemaakte volumes compact ingericht zijn, kunnen sommige bestaande gegevens van gelaagde volumes worden verplaatst naar de cloud tijdens het inrichtingsproces. De tijd voor het maken van een lokaal vastgemaakt volume is daarom afhankelijk van meerdere factoren, onder andere de grootte van het volume, de gegevens op uw apparaat en de beschikbare bandbreedte. Op een geheel nieuw geïnstalleerde apparaat dat geen volumes heeft, is de tijd voor het maken van een lokaal vastgemaakt volume ongeveer tien minuten per terabyte van gegevens. Maken van lokale volumes duurt echter enkele uren op basis van de factoren die hierboven worden uitgelegd op een apparaat dat wordt gebruikt.

**V:** Ik wil een lokaal vastgemaakt volume maakt. Zijn er aanbevolen procedures die ik moet rekening houden met?

**A:** Lokaal vastgemaakte volumes zijn geschikt voor werkbelastingen waarvoor lokale garanties van gegevens te allen tijde vereisen en in de cloud latentie gevoelig zijn. Tijdens het gebruik van lokale volumes voor een van de werkbelasting van uw overweegt, zorg rekening met het volgende:

* Lokaal vastgemaakte volumes zijn compact ingericht en maken van lokale volumes heeft impact op de beschikbare ruimte voor gelaagde volumes. We raden daarom u beginnen met kleiner volumes en omhoog schalen als uw opslag vereiste toeneemt.
* Het inrichten van lokale volumes is een langdurige bewerking waarbij bestaande gegevens van gelaagde volumes pushen naar de cloud mogelijk. Als gevolg hiervan kunnen optreden verminderde prestaties op deze volumes.
* Het inrichten van lokale volumes is een tijdrovende bewerking. De werkelijke tijd die zijn betrokken, is afhankelijk van meerdere factoren: de grootte van het volume wordt ingericht, de gegevens op het apparaat en de beschikbare bandbreedte. Als uw bestaande volumes niet naar de cloud ondersteund, zijn maken van volumes is langzamer. We raden dat u momentopnamen cloud van uw bestaande volumes voordat u een lokaal volume inricht.
* U kunt bestaande gelaagde volumes converteren naar lokaal vastgemaakte volumes en deze conversie omvat het inrichten van ruimte op het apparaat voor het resulterende lokaal vastgemaakt volume (naast het meebrengen omlaag gelaagde gegevens [eventuele] vanuit de cloud). Dit is ook een langdurige bewerking die is afhankelijk van factoren besproken hierboven hebt. Het is raadzaam om de back-up van uw bestaande volumes voordat conversie omdat het proces nog langer duren als er bestaande volumes zijn niet een back-up. Uw apparaat tegenkomen verminderde prestaties tijdens dit proces ook.

Meer informatie over het [een lokaal vastgemaakt volume maken](storsimple-manage-volumes-u2.md#add-a-volume)

**V:** Kan ik meerdere lokaal vastgemaakte volumes tegelijkertijd maken?

**A:** Ja, maar geen lokaal vastgemaakt volume maken en uitbreiding van de taken worden opeenvolgend verwerkt.

Lokaal vastgemaakte volumes zijn compact ingericht en hiervoor maken van lokale ruimte op het apparaat (dit kan leiden tot bestaande gegevens van gelaagde volumes moet tijdens het inrichtingsproces worden gepusht naar de cloud). Dus als een taak uitgevoerd wordt, andere taken voor het maken van lokaal volume wordt in de wachtrij gezet tot deze taak is voltooid.

Op dezelfde manier als een bestaand lokaal volume wordt uitgebreid of een gelaagd volume wordt omgezet naar een lokaal vastgemaakt volume, klikt u vervolgens het maken van een nieuw lokaal vastgemaakt volume is in de wachtrij totdat de vorige taak is voltooid. De grootte van een lokaal vastgemaakt volume uitbreiden, moet u de uitbreiding van de bestaande lokale ruimte die voor dat volume. Conversie van een gelaagde naar een lokaal vastgemaakt volume ook betrekking heeft op het maken van lokale ruimte voor de resulterende lokaal volume vastgemaakt. In beide deze bewerkingen, maken of uitbreiding van de lokale ruimte wordt een long uitgevoerd taak.

U vindt deze taken in de **taken** pagina van de Azure StorSimple Manager-service. De taak die actief wordt verwerkt, wordt steeds bijgewerkt om de voortgang van de inrichting ruimte weer te geven. De resterende taken van lokaal vastgemaakt volume zijn gemarkeerd als actief, maar hun progressie tot stilstand is gekomen en ze worden opgenomen in de volgorde waarin dat ze zijn in de wachtrij.

**V:** Ik heb verwijderd een lokaal vastgemaakt volume. Waarom zie ik niet de geregenereerde ruimte in de beschikbare ruimte wordt weergegeven wanneer ik probeer om een nieuw volume te maken? 

**A:** Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor de nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-Service-updates de lokale schijfruimte ongeveer om het uur. We raden dat u wachten tot een uur voordat u probeert te maken van het nieuwe volume.

**V:** Ondersteunt het toestel cloud lokaal vastgemaakte volumes?

**A:** Lokaal vastgemaakte volumes worden niet ondersteund op het cloud-toestel (8010 en 8020 apparaten voorheen bekend als het virtuele StorSimple-apparaat).

**V:** Kan ik de Azure PowerShell-cmdlets gebruiken om te maken en beheren van lokaal vastgemaakte volumes? 

**A:** Nee, u lokaal vastgemaakte volumes via Azure PowerShell-cmdlets (alle volumes die u via Azure PowerShell maakt is gelaagd) niet maken. We raden ook gebruiken de Azure PowerShell-cmdlets niet alle eigenschappen van een lokaal vastgemaakt volume wijzigen omdat het het ongewenst effect van het wijzigen van het volumetype zal hebben tot gelaagde.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Vragen over het back-ups van een lokaal vastgemaakt volume
**V:** Zijn lokale momentopnamen van lokaal vastgemaakte volumes ondersteund?

**A:** Ja, u kunt momentopnamen lokale van uw lokaal vastgemaakte volumes. Echter het beste regelmatig back-up van uw lokaal vastgemaakte volumes met cloudmomentopnamen om ervoor te zorgen dat uw gegevens in de deze van een noodgeval zijn beveiligd.

**V:** Zijn er richtlijnen voor het beheren van lokale momentopnamen voor lokaal vastgemaakte volumes?

**A:** Veelvuldig lokale momentopnamen samen met een hoge frequentie van gegevensverloop in de lokaal vastgemaakt volume mogelijk lokale ruimte op het apparaat te snel worden verbruikt, en er gegevens van gelaagde volumes worden gepusht naar de cloud. We raden daarom minimaliseert u het nummer van lokale momentopnamen.

**V:** Ik krijg een waarschuwing met de mededeling dat mijn lokale momentopnamen van lokaal vastgemaakte volumes mogelijk ongeldig worden gemaakt. Wanneer kan dit gebeuren

**A:** Veelvuldig lokale momentopnamen samen met een hoge frequentie van gegevensverloop in de lokaal vastgemaakt volume mogelijk lokale ruimte op het apparaat te snel worden verbruikt. Als de lokale lagen van het apparaat intensief gebruikt wordt, een uitgebreide cloud storing kan leiden tot het apparaat vol raken en binnenkomende schrijven naar het volume kunnen leiden tot ongeldig te maken van de momentopnamen (zoals geen ruimte beschikbaar is voor het bijwerken van de momentopnamen voor het verwijzen naar de oudere blokken met gegevens die zijn overschreven). In een dergelijke situatie blijven het schrijven naar het volume worden geleverd, maar de lokale momentopnamen is mogelijk ongeldig. Er zijn geen gevolgen voor uw bestaande cloudmomentopnamen.

De waarschuwing waarschuwing is om u te waarschuwen dat een dergelijke situatie kan ontstaan en zorg ervoor dat u hetzelfde adres tijdig dat door uw lokale momentopnamen planningen om minder frequente lokale momentopnamen te bekijken of verwijderen van oudere lokale momentopnamen die niet langer vereist zijn.

Als de lokale momentopnamen ongeldig worden gemaakt, ontvangt u een informatiemelding om u te informeren dat de lokale momentopnamen voor de specifieke back-upbeleid zijn ongeldig gemaakt naast de lijst met tijdstempels van de lokale momentopnamen die ongeldig zijn gemaakt. Deze momentopnamen worden automatisch verwijderd en u niet langer kunnen worden weer te geven in de **back-up catalogussen** pagina in de klassieke Azure portal.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Vragen over het converteren van een gelaagd volume naar een lokaal vastgemaakt volume
**V:** Ik ben sommige traagheid op het apparaat tijdens het converteren van een gelaagd volume naar een lokaal vastgemaakt volume observeren. Waarom is dit gebeurt? 

**A:** Het conversieproces bestaat uit twee stappen: 

1. Het inrichten van ruimte op het apparaat voor het snel-naar-worden-geconverteerd lokaal vastgemaakt volume.
2. Downloaden van een gelaagde gegevens vanuit de cloud om ervoor te zorgen lokale gegarandeerd.

Beide stappen zijn lang met bewerkingen die afhankelijk van de grootte van het volume dat wordt geconverteerd zijn, gegevens op het apparaat en de beschikbare bandbreedte. Als sommige gegevens van bestaande gelaagde volumes mogelijk naar de cloud worden gelekt als onderdeel van het inrichtingsproces, kan uw apparaat verminderde prestaties tijdens deze periode optreden. Bovendien het conversieproces duurt mogelijk langer als:

* Bestaande volumes zijn geen back-ups naar de cloud; zodat het is raadzaam om de dat back-up van uw volumes voordat u een conversie te starten.
* Bandbreedte, snelheidsbeperking beleidsregels zijn toegepast, die de beschikbare bandbreedte naar de cloud; kan beperken Daarom raden we aan dat u hebt een speciale 40 Mbps of meer verbinding naar de cloud.
* Het conversieproces kan enkele uren vanwege het meerdere factoren uiteengezet; duren Daarom is het raadzaam dat u deze bewerking niet pieken op tijdstippen of op een weekend om te voorkomen dat de gevolgen voor consumenten end uitvoert.

Meer informatie over het [een gelaagd volume converteren naar een lokaal vastgemaakt volume](storsimple-manage-volumes-u2.md#change-the-volume-type)

**V:** Kan ik de conversiebewerking volume annuleren?

**A:** Nee, u kunt niet de annuleren de conversiebewerking eenmaal is gestart. Zoals beschreven in de vorige vragen, u zich van de mogelijke prestatieproblemen die u mogelijk bij het proces stuit en volg de aanbevolen procedures die hierboven worden genoemd als u van plan uw conversie bent.

**V:** Wat gebeurt er met mijn volume als de conversiebewerking mislukt?

**A:** Volumeconversie kan mislukken door problemen met de netwerkverbinding van de cloud. Het apparaat niet meer het conversieproces uiteindelijk na een bepaald aantal mislukte pogingen om de gelaagde gegevens vanuit de cloud. In een dergelijk scenario het volumetype blijft het brontype volume vóór-conversie en:

* Er verschijnt een kritieke waarschuwing voor zullen u informeren over de fout bij het converteren van het volume. Meer informatie over [waarschuwingen die zijn gerelateerd aan lokaal vastgemaakte volumes](storsimple-manage-alerts.md#locally-pinned-volume-alerts)
* Als u een gelaagde naar een lokaal vastgemaakt volume converteren wilt, blijft het volume eigenschappen van een gelaagd volume vertonen, zoals gegevens nog steeds zich in de cloud bevinden mogelijk. We raden u los de problemen met de netwerkverbinding en probeer het opnieuw conversie.
* Op dezelfde manier als de conversie van een lokaal vastgemaakt naar een gelaagd volume uitvalt, hoewel het volume wordt gemarkeerd als een lokaal vastgemaakt volume, werkt als een gelaagd volume (omdat de gegevens kan terechtgekomen naar de cloud). Het blijft echter ruimte op de lokale lagen van het apparaat in beslag nemen. Deze ruimte worden niet beschikbaar voor andere lokaal vastgemaakte volumes. Het is raadzaam dat u het opnieuw om ervoor te zorgen dat de volumeconversie voltooid is en de lokale ruimte op het apparaat kan worden vrijgemaakt.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Vragen over het herstellen van een lokaal vastgemaakt volume
**V:** Lokaal vastgemaakte volumes hersteld onmiddellijk?

**A:** Ja, lokaal vastgemaakte volumes zijn direct hersteld. Zodra de informatie over de metagegevens voor het volume is opgehaald uit de cloud als onderdeel van de herstelbewerking, wordt het volume online is gebracht en toegankelijk zijn voor de host. Lokale garanties met betrekking tot het volume gegevens niet meer aanwezig totdat alle gegevens die is gedownload vanuit de cloud en treden verminderd echter op de prestaties van deze volumes voor de duur van het terugzetten.

**V:** Hoe lang duurt het herstellen van een lokaal vastgemaakt volume?

**A:** Lokaal vastgemaakte volumes zijn onmiddellijk teruggezet en online wordt gebracht zodra de volumegegevens voor metagegevens worden opgehaald uit de cloud terwijl de volumegegevens wordt voortgezet op de achtergrond worden gedownload. Dit laatste deel van de herstelbewerking--om terug te zetten van de lokale garanties voor de volumegegevens--het is een langdurige bewerking en duurt enkele uren voor alle gegevens voor lokale opnieuw worden gemaakt. De tijd voltooien hetzelfde is afhankelijk van meerdere factoren, zoals de grootte van het volume wordt hersteld en de beschikbare bandbreedte. Als het oorspronkelijke volume dat wordt hersteld, is verwijderd, worden extra tijd gaat maken van de lokale ruimte op het apparaat als onderdeel van de herstelbewerking opnieuw.

**V:** Ik wil mijn bestaande lokaal vastgemaakt volume herstellen naar een momentopname van een oudere (uitgevoerd wanneer het volume is gelaagd). Het volume worden hersteld als lagen in dit geval?

**A:** Nee, wordt het volume worden hersteld als een lokaal vastgemaakt volume. Hoewel de datums van de momentopname aan de tijd wanneer het volume is gelaagd, tijdens het herstellen van bestaande volumes StorSimple altijd maakt gebruik van het type volume op de schijf omdat deze momenteel bestaat.

**V:** Ik mijn lokaal vastgemaakt volume onlangs hebt uitgebreid, maar ik wil nu de gegevens herstellen naar een tijd waarop het volume kleiner is. Wordt terugzetten van de grootte van het huidige volume en moet ik de grootte van het volume uitbreiden nadat het herstel is voltooid?

**A:** Ja, de herstelbewerking wordt de grootte van het volume en moet u de grootte van het volume uitbreiden nadat het herstel is voltooid.

**V:** Kan ik tijdens het terugzetten van het type van een volume wijzigen?

**A.**Nee, u het volumetype niet wijzigen tijdens het terugzetten.

* Volumes die zijn verwijderd zijn als het type dat is opgeslagen in de momentopname hersteld.
* Bestaande volumes zijn hersteld op basis van hun huidige type, ongeacht het type dat is opgeslagen in de momentopname (Zie de vorige twee vragen).

**V:** Ik wil mijn lokaal vastgemaakt volume te herstellen, maar ik heb een onjuist punt in tijd momentopname. Kan ik de huidige herstelbewerking annuleren?

**A:** Ja, kunt u een herstelbewerking continu annuleren. De status van het volume wordt teruggedraaid naar de status aan het begin van het terugzetten. Er geen schrijfbewerkingen die zijn aangebracht aan het volume, terwijl de herstelbewerking werd uitgevoerd is verloren gegaan.

**V:** Ik een restore-bewerking op een van mijn lokaal vastgemaakte volumes gestart en wordt nu een momentopname wordt weergegeven in de catalogus van mijn achterstand die ik niet nu maken. Wat wordt dit gebruikt?

**A:** Dit is de tijdelijke momentopname die vóór de herstelbewerking is gemaakt en wordt gebruikt voor ongedaan maken als het herstel is geannuleerd of mislukt. Deze momentopname; niet verwijderen Deze worden automatisch verwijderd wanneer het herstel voltooid is. Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als.

**V:** Kan ik een lokaal vastgemaakt volume kloon?

**A:** Ja, kunt u. Lokaal vastgemaakt volume wordt echter als een gelaagd volume standaard worden gekloond. Meer informatie over het [een lokaal vastgemaakt volume klonen](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Vragen over mislukte via een lokaal vastgemaakt volume
**V:** Ik wil mijn apparaat aan een ander fysiek apparaat failover. Mijn lokaal vastgemaakte volumes mislukken dan lokaal vastgemaakt of gelaagde?

**A:** Afhankelijk van de softwareversie van het doelapparaat, lokaal vastgemaakte volumes failover plaats als:

* Lokaal vastgemaakt als het doelapparaat wordt uitgevoerd op StorSimple 8000 update series 2
* Gelaagde als het doelapparaat wordt uitgevoerd op StorSimple bijwerken 8000 series 1.x
* Als het doelapparaat het toestel cloud is lagen (software-update voor versie 2 of 1.x bijwerken)

Meer informatie over [failover en DR van lokaal vastgemaakte volumes op versies van](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**V:** Lokaal vastgemaakte volumes direct herstellen tijdens herstel na noodgeval (DR)?

**A:** Ja, lokaal vastgemaakte volumes zijn hersteld onmiddellijk tijdens failover. Zodra de informatie over de metagegevens voor het volume is opgehaald uit de cloud als onderdeel van de failoverbewerking, wordt het volume online brengen op het doelapparaat en toegankelijk zijn voor de host. Ondertussen blijft de volumegegevens downloaden op de achtergrond en verminderde prestaties op deze volumes voor de duur van de failover kunnen optreden.

**V:** Zie ik de failover-taak is voltooid, hoe kan ik de voortgang van een lokaal vastgemaakt volume dat wordt hersteld bijhouden op het doelapparaat?

**A:** Tijdens een failoverbewerking worden de failover-taak is gemarkeerd als voltooid eenmaal alle volumes in de set met failover zijn onmiddellijk teruggezet en online brengen op het doelapparaat. Dit omvat alle lokaal vastgemaakte volumes die mogelijk zijn via; is mislukt echter, lokale garanties van de gegevens worden pas beschikbaar wanneer de gegevens voor het volume is gedownload. U kunt deze voortgang voor elk lokaal vastgemaakt volume dat is mislukt door de bewaking van de bijbehorende hersteltaken die zijn gemaakt als onderdeel van de failover volgen. Deze het herstellen van afzonderlijke taken wordt alleen gemaakt voor lokaal vastgemaakte volumes.

**V:** Kan ik het type van een volume wijzigen tijdens de failover?

**A:** Nee, kunt u het volumetype niet wijzigen tijdens een failover. Als u zich niet via een ander fysiek apparaat met StorSimple 8000 series update 2, de volumes wordt failover uitgevoerd op basis van het volumetype dat is opgeslagen in de momentopname. Wanneer failover wordt uitgevoerd naar een andere apparaatversie, raadpleegt u de vraag boven op het volumetype na een failover.

**V:** Kan ik een volumecontainer met lokaal vastgemaakte volumes op het toestel cloud failover?

**A:** Ja, kunt u. Lokaal vastgemaakte volumes wordt als gelaagde volumes failover worden uitgevoerd. Meer informatie over [failover en DR van lokaal vastgemaakte volumes op versies van](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)

