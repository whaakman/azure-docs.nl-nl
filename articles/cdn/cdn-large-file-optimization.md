---
title: Optimalisatie van grote bestanden downloaden met Azure CDN
description: In dit artikel wordt uitgelegd hoe groot bestand downloads kunnen worden geoptimaliseerd.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: magattus
ms.openlocfilehash: 9793348b47763e6de10992b9a8a4606fc532cc4d
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094017"
---
# <a name="large-file-download-optimization-with-azure-cdn"></a>Optimalisatie van grote bestanden downloaden met Azure CDN

Grootte van de inhoud die wordt geleverd via het internet blijven groeien vanwege de uitgebreide functionaliteit, verbeterde afbeeldingen en uitgebreide media-inhoud. Deze groei wordt aangestuurd door vele factoren: breedband indringingstests, grotere goedkope opslagapparaten, wijdverbreid toename van HD-video- en internet verbonden apparaten (IoT). Een leveringsmechanisme voor grote bestanden voor snelle en efficiënte is van essentieel belang om te controleren of de ervaring van een soepele en leuker consumenten.

Levering van grote bestanden heeft verschillende uitdagingen. Ten eerste kan is de gemiddelde tijd voor het downloaden van een groot bestand aanzienlijk zijn omdat toepassingen mogelijk alle gegevens niet opeenvolgend downloaden. In sommige gevallen mogelijk toepassingen die het laatste deel van een bestand voordat u het eerste deel downloaden. Als slechts een kleine hoeveelheid van een bestand wordt aangevraagd of een gebruiker een download wordt onderbroken, kan het downloaden van het mislukken. Het downloaden van het kan ook worden uitgesteld tot na het content delivery network (CDN) het hele bestand van de oorspronkelijke server haalt. 

Ten tweede bepaalt de latentie tussen de computer van een gebruiker en het bestand de snelheid waarmee ze inhoud kunnen bekijken. Problemen met het netwerk opstoppingen in het en -capaciteit wordt bovendien ook een doorvoer beïnvloeden. Groter zijn afstanden tussen servers en gebruikers maken als u meer mogelijkheden voor pakketverlies optreden, waardoor de kwaliteit. De vermindering van de kwaliteit veroorzaakt door een beperkte doorvoer en verbeterde pakketverlies verhogen de wachttijd voor het downloaden van een bestand om te voltooien. 

Derde veel grote bestanden worden niet in hun geheel geleverd. Gebruikers kunnen halverwege downloaden annuleren of alleen de eerste paar minuten een lange MP4-video bekijken. Software en media delivery-bedrijven willen daarom bieden alleen het gedeelte van een bestand dat wordt aangevraagd. Efficiënte distributie van de aangevraagde delen vermindert het uitgaande verkeer van de oorspronkelijke server. Efficiënte distributie verlaagt u ook het geheugen en i/o-druk op de oorspronkelijke server. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-microsoft"></a>Optimaliseren voor de levering van grote bestanden met Azure CDN van Microsoft

**Azure CDN Standard van Microsoft** eindpunten leveren grote bestanden zonder een bovengrens voor bestandsgrootte. Extra functies worden standaard om levering van grote bestanden sneller ingeschakeld.

### <a name="object-chunking"></a>Object logische groepen te verdelen 

**Azure CDN Standard van Microsoft** maakt gebruik van de techniek object logische groepen te verdelen. Wanneer een groot bestand wordt aangevraagd, wordt in het CDN kleinere delen van het bestand opgehaald uit de oorsprong. Nadat de CDN POP-server een volledige of bereik in bytes bestand-aanvraag ontvangt, vraagt de CDN edge-server het bestand van de oorsprong in chunks van 8 MB. 

Nadat het segment bij de rand van CDN aankomt, is het in de cache opgeslagen en onmiddellijk worden geleverd aan de gebruiker. Het volgende segment parallel wordt vervolgens dat door het CDN. Deze prefetch zorgt ervoor dat de inhoud blijft één segment voor de gebruiker, waardoor latentie. Dit proces gaat door totdat het volledige bestand is gedownload (indien nodig), zijn alle bereiken in bytes beschikbaar zijn (indien nodig), of als de client de verbinding verbreekt. 

Zie voor meer informatie over de aanvraag bereik in bytes, [RFC 7233](https://tools.ietf.org/html/rfc7233).

De CDN slaat alle segmenten als ze worden ontvangen. Het hele bestand hoeft niet te worden in de cache op de CDN-cache. Volgende aanvragen voor het bestand of de byte-adresbereiken worden aangeleverd vanuit de CDN-cache. Als niet alle chunks in cache zijn opgeslagen op het CDN, prefetch wordt gebruikt om aan te vragen van segmenten van de oorsprong. Deze optimalisatie is afhankelijk van de mogelijkheid van de oorspronkelijke server voor de ondersteuning van de byte-bereikaanvragen; Als de oorspronkelijke server biedt geen ondersteuning voor byte-bereikaanvragen, is deze optimalisatie niet effectief. 

### <a name="conditions-for-large-file-optimization"></a>Voorwaarden voor de optimalisatie van grote bestanden
Optimalisatie van grote bestanden voor functies **Azure CDN Standard van Microsoft** zijn standaard ingeschakeld wanneer u de algemene web delivery optimization-type gebruikt. Er zijn geen beperkingen op de maximale bestandsgrootte.


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-verizon"></a>Optimaliseren voor de levering van grote bestanden met Azure CDN van Verizon

**Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** eindpunten leveren grote bestanden zonder een bovengrens voor bestandsgrootte. Extra functies worden standaard om levering van grote bestanden sneller ingeschakeld.

### <a name="complete-cache-fill"></a>Volledige cache opvulling

De standaard volledige cache opvulling functie kunt het CDN voor het ophalen van een bestand in de cache als een eerste aanvraag is afgebroken of verbroken. 

Opvulling van de volledige cache is vooral nuttig zijn voor grote activa. Normaal gesproken downloaden gebruikers niet van begin tot eind. Ze gebruiken progressief downloaden. Het standaardgedrag zorgt ervoor dat de edge-server om te starten van een op de achtergrond ophalen van de activa van de oorspronkelijke server. Daarna wordt is het actief in lokale cache van de edge-server. Nadat de volledige-object in de cache is, is de edge-server voldoet aan vereisten byte-bereikaanvragen op het CDN voor het object in de cache.

Het standaardgedrag kan worden uitgeschakeld via de regelengine in **Azure CDN Premium van Verizon**.

### <a name="peer-cache-fill-hot-filing"></a>Peer-cache vult hot archiveren

De standaard peer-cache opvulling hot indiening functie maakt gebruik van een geavanceerde eigen algoritme. Hierbij meer edge cache-servers op basis van bandbreedte en statistische functie aanvragen metrische gegevens om te voldoen aan aanvragen van clients voor grote, veelgebruikte objecten. Deze functie wordt voorkomen dat een situatie waarin grote aantallen extra aanvragen worden verzonden naar de bronserver van de gebruiker. 

### <a name="conditions-for-large-file-optimization"></a>Voorwaarden voor de optimalisatie van grote bestanden

Optimalisatie van grote bestanden voor functies **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** zijn standaard ingeschakeld wanneer u de algemene web delivery optimization-type gebruikt. Er zijn geen beperkingen op de maximale bestandsgrootte. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-standard-from-akamai"></a>Optimaliseren voor de levering van grote bestanden met Azure CDN Standard van Akamai

**Azure CDN Standard van Akamai** profiel-eindpunten bieden een functie waarmee leveren van grote bestanden efficiënt aan gebruikers over de hele wereld op schaal. De functie vermindert latenties omdat de belasting van de bronservers vermindert.

De functie voor grote bestanden optimalisatie typen Hiermee schakelt u netwerkoptimalisaties en configuraties die u kunt grote bestanden sneller en meer responsively leveren. Algemene webweergave met **Azure CDN Standard van Akamai** eindpunten in de cache opgeslagen bestanden alleen hieronder 1.8 GB en tunnel (geen cache) bestanden kan maximaal 150 GB. Grote bestanden optimalisatie caches bestanden tot 150 GB.

Optimalisatie van grote bestanden is effectief wanneer bepaalde voorwaarden is voldaan. Voorwaarden omvatten de werking van de oorspronkelijke server en de grootte en de typen van de bestanden die worden aangevraagd. 

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Een Akamai CDN-eindpunt voor het optimaliseren van de levering van grote bestanden configureren

U kunt uw **Azure CDN Standard van Akamai** eindpunt om te optimaliseren voor grote bestanden via Azure portal. U kunt ook de REST API's of een van de client-SDK's gebruiken om dit te doen. De volgende stappen ziet u het proces via de Azure-portal voor een **Azure CDN Standard van Akamai** profiel:

1. Een nieuw eindpunt toevoegen aan een Akamai **CDN-profiel** weergeeft, schakelt **eindpunt**.

    ![Nieuw eindpunt](./media/cdn-large-file-optimization/cdn-new-akamai-endpoint.png)    
 
2. In de **geoptimaliseerd voor** vervolgkeuzelijst, selecteer **grote bestanden downloaden**.

    ![Optimalisatie van grote bestanden geselecteerd](./media/cdn-large-file-optimization/cdn-large-file-select.png)


Nadat u het CDN-eindpunt hebt gemaakt, wordt de optimalisatie van grote bestanden voor alle bestanden die voldoen aan bepaalde criteria is van toepassing. De volgende sectie wordt beschreven in dit proces.

### <a name="object-chunking"></a>Object logische groepen te verdelen 

Optimalisatie van grote bestanden met **Azure CDN Standard van Akamai** maakt gebruik van de techniek object logische groepen te verdelen. Wanneer een groot bestand wordt aangevraagd, wordt in het CDN kleinere delen van het bestand opgehaald uit de oorsprong. Nadat de CDN POP-server een volledige of bereik in bytes bestand-aanvraag ontvangt, wordt gecontroleerd of het bestandstype wordt ondersteund voor deze optimalisatie. Er wordt ook gecontroleerd of het bestandstype dat voldoet aan de vereisten van de grootte van bestand. Als de bestandsgrootte groter dan 10 MB is, vraagt de CDN edge-server het bestand van de oorsprong in chunks van 2 MB. 

Nadat het segment bij de rand van CDN aankomt, is het in de cache opgeslagen en onmiddellijk worden geleverd aan de gebruiker. Het volgende segment parallel wordt vervolgens dat door het CDN. Deze prefetch zorgt ervoor dat de inhoud blijft één segment voor de gebruiker, waardoor latentie. Dit proces gaat door totdat het volledige bestand is gedownload (indien nodig), zijn alle bereiken in bytes beschikbaar zijn (indien nodig), of als de client de verbinding verbreekt. 

Zie voor meer informatie over de aanvraag bereik in bytes, [RFC 7233](https://tools.ietf.org/html/rfc7233).

De CDN slaat alle segmenten als ze worden ontvangen. Het hele bestand hoeft niet te worden in de cache op de CDN-cache. Volgende aanvragen voor het bestand of de byte-adresbereiken worden aangeleverd vanuit de CDN-cache. Als niet alle chunks in cache zijn opgeslagen op het CDN, prefetch wordt gebruikt om aan te vragen van segmenten van de oorsprong. Deze optimalisatie is afhankelijk van de mogelijkheid van de oorspronkelijke server voor de ondersteuning van de byte-bereikaanvragen; Als de oorspronkelijke server biedt geen ondersteuning voor byte-bereikaanvragen, is deze optimalisatie niet effectief.

### <a name="caching"></a>Caching
Optimalisatie van grote bestanden maakt gebruik van verschillende standaard opslaan in cache verloopt tijden van algemene webweergave. Het onderscheid gemaakt tussen de caching van positieve en negatieve cache-opslag op basis van HTTP-responscodes. Als de oorspronkelijke server Hiermee geeft u een verlooptijd via een cache-control of expires-header in het antwoord, houdt het CDN die waarde. Wanneer de oorsprong niet wordt opgegeven en het bestand overeenkomt met de voorwaarden van het type en de grootte voor dit optimalisatietype, gebruikt het CDN de standaardwaarden voor de optimalisatie van grote bestanden. Anders worden de standaardwaarden van het CDN gebruikt voor algemene webweergave.


|    | Algemene web | Optimalisatie van grote bestanden 
--- | --- | --- 
Opslaan in cache: positief <br> HTTP 200, 203, 300, <br> 301, 302 en 410 | 7 dagen |1 dag  
Opslaan in cache: negatief zijn <br> HTTP 204, 305, 404, <br> en 405 | Geen | 1 seconde 

### <a name="deal-with-origin-failure"></a>Fout bij de oorsprong zijn getroffen

De lengte van de oorsprong-lezen-timeout neemt van twee seconden voor algemene webweergave tot twee minuten voor het type van de optimalisatie van grote bestanden. Deze toename van de accounts voor de grotere bestanden om te voorkomen dat een verbinding voortijdig time-out.

Wanneer een verbinding een optreedt time-out, probeert een aantal keer voordat er een fout '504 - time-out voor de Gateway' naar de client verzendt opnieuw door het CDN. 

### <a name="conditions-for-large-file-optimization"></a>Voorwaarden voor de optimalisatie van grote bestanden

De volgende tabel bevat de set criteria worden voldaan voordat voor optimalisatie van grote bestanden:

Voorwaarde | Waarden 
--- | --- 
Ondersteunde bestandstypen | 3g, 2, 3gp, AVP, avi, bz2, dmg, exe, f4v, flv <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, mts, pak, qt, DB, swf, tar, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minimale bestandsgrootte | 10 MB 
Maximale bestandsgrootte | 150 GB 
De kenmerken van een oorsprong | Byte-bereikaanvragen moet ondersteunen 

## <a name="additional-considerations"></a>Aanvullende overwegingen

Houd rekening met de volgende aanvullende aspecten voor dit optimalisatietype:

- De chunking proces genereert meer aanvragen naar de oorspronkelijke server. De totale hoeveelheid gegevens die worden geleverd bij de oorsprong is echter veel kleiner. Chunking resulteert in betere opslaan in cache kenmerken op het CDN.

- Omdat kleinere delen van het bestand worden geleverd worden bij de oorsprong geheugen en i/o-druk gereduceerd.

- Voor de segmenten in de cache opgeslagen op het CDN, zijn er geen aanvullende aanvragen naar de oorsprong totdat de inhoud verloopt of deze wordt verwijderd uit de cache.

- Gebruikers kunnen bereikaanvragen versturen naar de CDN, die net als normale bestanden worden behandeld. Optimalisatie geldt alleen als het is een ongeldig bestandstype en het bereik in bytes tussen 10 MB en 150 GB. Als de gemiddelde grootte aangevraagd kleiner dan 10 MB is, gebruikt u de algemene webweergave.

