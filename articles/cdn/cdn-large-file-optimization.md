---
title: Optimalisatie van grote bestanden downloaden via het Azure Content Delivery Network
description: Optimalisatie van downloads van grote bestanden beschreven in de diepte
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 7a5d5d1d0de24ebb0a5115ede1e572f38454bd78
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="large-file-download-optimization-via-the-azure-content-delivery-network"></a>Optimalisatie van grote bestanden downloaden via het Azure Content Delivery Network

Grootte van inhoud via het Internet worden geleverd blijven groeien vanwege de uitgebreide functionaliteit, verbeterde afbeeldingen en uitgebreide media-inhoud. Deze groei wordt aangedreven door vele factoren: breedband binnendringen, grotere goedkope opslagapparaten, wijdverbreid toename van hoogwaardige video- en Internet verbonden apparaten (IoT). Een snelle en efficiënte bezorgingsmechanisme voor grote bestanden is essentieel voor een consumer smooth en uitmuntende ervaring.

De bezorging van grote bestanden heeft enkele uitdagingen. Eerst kan de gemiddelde tijd voor het downloaden van een groot bestand worden belangrijke omdat toepassingen mogelijk alle gegevens niet opeenvolgend downloaden. In sommige gevallen kunnen toepassingen die het laatste deel van een bestand voordat u het eerste deel downloaden. Wanneer een kleine hoeveelheid een bestand wordt aangevraagd of een gebruiker een download wordt onderbroken, kan het downloaden van het mislukken. Het downloaden van het kan ook worden uitgesteld tot na het content delivery network (CDN) het hele bestand op de bronserver haalt. 

Ten tweede bepaalt de latentie tussen de computer van een gebruiker en het bestand de snelheid waarmee ze inhoud kunnen weergeven. Bovendien problemen met het netwerk opstoppingen en capaciteit ook van invloed op de doorvoer. Aanvullende mogelijkheden voor pakketverlies optreden, waardoor kwaliteit maken groter afstanden tussen servers en gebruikers De vermindering van de kwaliteit veroorzaakt door een beperkte doorvoer en verbeterde pakketverlies verhogen de wachttijd voor het downloaden van een bestand om te voltooien. 

Ten slotte veel grote bestanden niet in hun geheel worden geleverd. Gebruikers kunnen downloaden halverwege annuleren of alleen de eerste paar minuten van een lange MP4-video bekijken. Software en media levering-bedrijven willen daarom alleen het gedeelte van een bestand dat aangevraagd leveren. Efficiënte distributie van de aangevraagde delen minder het uitgaande verkeer van de bronserver. Efficiënte distributie ook minder geheugen en i/o-belasting op de bronserver. 

Het Azure Content Delivery Network van Akamai biedt nu een functie die voorziet in grote bestanden efficiënt aan gebruikers overal ter wereld op grote schaal. De functie vermindert latenties omdat dit de belasting van de oorsprong-servers verlaagt. Deze functie is beschikbaar met de standaard Akamai prijscategorie.

## <a name="configure-a-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Een CDN-eindpunt voor het optimaliseren van de bezorging van grote bestanden configureren

U kunt uw CDN-eindpunt voor het optimaliseren van leveringsmethode voor grote bestanden via de Azure-portal configureren. U kunt ook onze REST-API's of een van de client-SDK's gebruiken om dit te doen. De volgende stappen ziet het proces via de Azure-portal.

1. Een nieuw eindpunt toevoegen aan de **CDN-profiel** pagina **eindpunt**.

    ![Nieuwe endpoint](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. In de **geoptimaliseerd voor** vervolgkeuzelijst, selecteer **downloaden van grote bestanden**.

    ![Optimalisatie van grote bestanden geselecteerd](./media/cdn-large-file-optimization/02_Creating.png)


Nadat u het CDN-eindpunt hebt gemaakt, wordt het toegepast groot bestand optimalisaties voor alle bestanden die aan bepaalde criteria voldoen. De volgende sectie wordt beschreven in dit proces.

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-akamai"></a>Voor de levering van grote bestanden met de Azure Content Delivery Network van Akamai optimaliseren

De functie van grote bestanden optimization type Hiermee schakelt u netwerkoptimalisaties en configuraties voor het leveren van grote bestanden sneller en meer responsively. Algemene webtoepassingen levering met Akamai bestanden alleen hieronder 1,8 GB cache en tunnel (geen cache) bestanden kan maximaal 150 GB. Grote bestanden optimalisatie caches bestanden maximaal 150 GB.

Optimalisatie van grote bestanden is van kracht wanneer bepaalde voorwaarden wordt voldaan. Voorwaarden omvatten de werking van de bronserver en de grootte en de typen van de bestanden die worden aangevraagd. Voordat we naar meer informatie over deze onderwerpen sturen, moet u begrijpen hoe de optimalisatie werkt. 

### <a name="object-chunking"></a>Object-verdeling in segmenten 

Het Azure Content Delivery Network van Akamai maakt gebruik van een techniek die genoemd object verdeling in segmenten. Wanneer een groot bestand wordt aangevraagd, haalt de CDN kleinere delen van het bestand vanuit de oorsprong. Nadat de CDN edge/POP-server een volledige of bereik aan bytes bestand-aanvraag ontvangt, controleert deze of het bestandstype voor deze optimalisatie wordt ondersteund. Controleert ook of het bestandstype dat voldoet aan de vereisten van de grootte van bestand. Als de bestandsgrootte groter dan 10 MB is, vraagt de CDN-randserver het bestand vanuit de oorsprong in segmenten van 2 MB. 

Nadat het segment dat aan de rand van het CDN is binnengekomen, heeft deze in de cache opgeslagen en onmiddellijk worden aangeboden aan de gebruiker. De CDN dat vervolgens de volgende chunk parallel. Deze prefetch zorgt ervoor dat de inhoud blijft één segment voor de gebruiker, waardoor latentie. Dit proces gaat door totdat het gehele bestand wordt gedownload (indien nodig), zijn alle bytereeksen beschikbaar (indien nodig), of als de client de verbinding verbreekt. 

Zie voor meer informatie over de aanvraag van het bereik aan bytes [RFC 7233](https://tools.ietf.org/html/rfc7233).

De CDN plaatst alle segmenten als ze worden ontvangen. Het hele bestand hoeft niet te worden in de cache op de CDN-cache. De volgende aanvragen voor het bestand of de byte-bereiken worden uit de cache CDN behandeld. Als niet alle chunks in de cache op de CDN, prefetch wordt gebruikt om te vragen van de segmenten van de oorsprong. Deze optimalisatie wordt gebruikgemaakt van de mogelijkheid van de oorspronkelijke server te ondersteunen byte-bereikaanvragen. _Als de oorspronkelijke server biedt geen ondersteuning voor byte-bereikaanvragen, deze optimalisatie niet effectief._ 

### <a name="caching"></a>Caching
Optimalisatie van grote bestanden maakt gebruik van verschillende standaardtijden opslaan in cache verloopt uit algemene webtoepassingen levering. Het onderscheidt positieve caching en negatieve cache-opslag op basis van HTTP-antwoordcodes. Als de oorspronkelijke server een verlooptijd via een cache-control bevat of expires-header in het antwoord, respecteert de CDN die waarde. Wanneer de oorsprong niet opgeven en het bestand overeenkomt met de voorwaarden van het type en de grootte voor dit type optimalisatie, gebruikt de CDN de standaardwaarden voor optimalisatie van grote bestanden. Anders dan de CDN standaardwaarden gebruikt voor de levering van algemene webtoepassingen.


|    | Algemene webtoepassingen | Optimalisatie van grote bestanden 
--- | --- | --- 
Opslaan in cache: positief <br> HTTP 200, 203, 300, <br> 301, 302 en 410 | 7 dagen |1 dag  
Opslaan in cache: negatieve <br> HTTP 204 305 404, <br> en 405 | Geen | 1 seconde 

### <a name="deal-with-origin-failure"></a>Omgaan met fouten in de oorsprong

De lengte van de oorsprong lezen-timeout verhoogt van twee seconden voor de levering van algemene webtoepassingen tot twee minuten voor het bestandstype voor grote optimalisatie. Deze toename is verantwoordelijk voor de grotere bestanden om te voorkomen dat een voortijdige time-out voor verbinding.

Wanneer een verbinding een optreedt time-out, probeert de CDN opnieuw een aantal keren voordat het een fout '504 - Gateway timeout gegenereerd' naar de client verzendt. 

### <a name="conditions-for-large-file-optimization"></a>Voorwaarden voor de optimalisatie van grote bestanden

De volgende tabel bevat de set criteria om te worden voldaan voor optimalisatie van grote bestanden:

Voorwaarde | Waarden 
--- | --- 
Ondersteunde bestandstypen | 3g, 2, 3gp, AVP, avi, bz2, dmg, exe, f4v, flv <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, mts, pkg, qt, DB, swf, tar, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minimale bestandsgrootte | 10 MB 
Maximale bestandsgrootte | 150 GB 
De kenmerken van een oorsprong | Byte-bereikaanvragen moet ondersteunen 

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-verizon"></a>Voor de levering van grote bestanden met de Azure Content Delivery Network van Verizon optimaliseren

Het Azure Content Delivery Network van Verizon biedt grote bestanden zonder een limiet op de bestandsgrootte. Aanvullende functies zijn ingeschakeld door standaard op de bezorging van grote bestanden sneller maken.

### <a name="complete-cache-fill"></a>Volledige cache opvulling

De standaard volledige opvulling cachefunctie kan de CDN voor het ophalen van een bestand in de cache als een eerste aanvraag is afgebroken of verbroken. 

Volledige cache opvulling is vooral handig voor grote activa. Normaal gesproken downloaden gebruikers niet van begin tot einde. Ze gebruiken progressief downloaden. Het standaardgedrag zorgt ervoor dat de edge-server voor het initiëren van een op de achtergrond ophalen van de activa op de bronserver. De asset is daarna in lokale cache van de edge-server. Nadat het volledig object zich in de cache, aan de edge-server voldoet byte-bereikaanvragen naar de CDN voor het object in de cache.

Het standaardgedrag kan worden uitgeschakeld via de Engine regels in de laag Premium van Verizon.

### <a name="peer-cache-fill-hot-filing"></a>Peer-cache hot indiening vullen

De standaard peer-cache opvulling hot indiening functie gebruikt een geavanceerde eigen algoritme. Aanvullende edge cache servers op basis van bandbreedte wordt gebruikt en aggregatie vraagt metrische gegevens om te voldoen aan aanvragen van clients voor grote, maximaal populaire objecten. Deze functie wordt voorkomen dat een situatie waarin grote aantallen extra aanvragen worden verzonden naar de bronserver van de gebruiker. 

### <a name="conditions-for-large-file-optimization"></a>Voorwaarden voor de optimalisatie van grote bestanden

De van optimalisatiefuncties voor Verizon zijn standaard ingeschakeld. Er zijn geen limieten op de maximale grootte. 

## <a name="additional-considerations"></a>Aanvullende overwegingen

Houd rekening met de volgende aanvullende aspecten voor dit type optimalisatie.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Azure Content Delivery Network van Akamai

- Het proces chunking genereert aanvullende aanvragen naar de oorspronkelijke server. De totale hoeveelheid gegevens die worden geleverd vanuit de oorsprong is echter veel kleiner. Chunking resulteert in betere cache in kenmerken op het CDN.

- Omdat kleinere delen van het bestand worden afgeleverd worden bij de oorsprong geheugen en i/o-belasting gereduceerd.

- Voor de segmenten in cache bij de CDN, zijn er geen aanvullende aanvragen naar de oorsprong totdat de inhoud verloopt of deze wordt verwijderd uit de cache.

- Kunnen gebruikers range-aanvragen naar de CDN aanbrengen en ze worden behandeld als een normale bestand. Optimalisatie geldt alleen als het is een ongeldig bestandstype en het bereik aan bytes tussen 10 MB en 150 GB. Als de gemiddelde bestandsgrootte aangevraagd kleiner dan 10 MB is, wilt u mogelijk gebruik in plaats hiervan levering van algemene webtoepassingen.

### <a name="azure-content-delivery-network-from-verizon"></a>Azure Content Delivery Network van Verizon

Het type Algemeen web levering optimalisatie kan grote bestanden bieden.
