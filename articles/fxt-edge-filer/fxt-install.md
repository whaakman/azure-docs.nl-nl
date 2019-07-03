---
title: Zelfstudie over het installeren van een fysiek apparaat Azure FXT Edge Filer | Microsoft Docs
description: Uitpakken, rek en het fysieke apparaatonderdeel van de Microsoft Azure FXT Edge Filer hybride opslagcache-kabel
services: ''
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: ed9eca88e5ccc386b25acb95fa729a3cfb95cbd0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543470"
---
# <a name="tutorial-install-azure-fxt-edge-filer"></a>Zelfstudie: Azure FXT Edge Filer installeren 

In deze zelfstudie wordt beschreven hoe u een hardware-knooppunt voor de cache van Azure FXT Edge Filer hybride opslag installeren. U moet ten minste drie hardwareknooppunten voor het maken van een cluster Azure FXT Edge Filer installeren.

De installatieprocedure omvat het uitpakken en rack koppelen van het apparaat en het koppelen van de kabel beheertak (CMA) en het voorpaneel. Een afzonderlijke zelfstudie wordt uitgelegd aan koppelen en verbindende power. 

Het duurt ongeveer één uur voor het installeren van een Azure FXT Edge Filer-knooppunt. 

Deze zelfstudie bevat de instellingsstappen van deze: 

> [!div class="checklist"]
> * Het apparaat uitpakken
> * Het apparaat in een rack koppelen
> * Installeer het voorpaneel (optioneel)

## <a name="installation-prerequisites"></a>Vereisten voor de installatie 

Voordat u begint, zorg ervoor dat het datacenter en rack die u wilt gebruiken, heeft deze functies:

* Een beschikbare 1U sleuf op het rek te plaatsen waar u van plan bent om te koppelen van het apparaat.
* AC-voeding en koeling systemen die voldoen aan de behoeften van de Azure FXT Edge Filer. (Lezen [Power en warmte specificaties](fxt-specs.md#power-and-thermal-specifications) voor meer informatie over planning en het formaat van de installatie.)  

  > [!NOTE] 
  > Als u wilt profiteren van de twee redundante voeding aanbod eenheden (PSUs), gebruikt u power distribution units op twee verschillende vertakking circuits bij het toevoegen van Netstroom zijn aangesloten. Lezen [power kabels verbinden](fxt-network-power.md#connect-power-cables) voor meer informatie.  

## <a name="unpack-the-hardware-node"></a>Uitpakken van de hardware-knooppunt 

Elk knooppunt Azure FXT Edge Filer wordt verzonden in een vak. Deze stappen uit te pakken een apparaat.

1. Leg de doos op een vlak, horizontaal oppervlak.

2. Controleer de doos en het verpakkingsmateriaal op deuken, scheuren, waterschade of andere duidelijk zichtbare schade. Als de doos of het verpakkingsmateriaal ernstig beschadigd is, moet u de doos niet openen. Neem contact op met Microsoft Support om in overleg te bepalen of het apparaat nog in goede staat zal zijn.

3. Pak de doos uit. Zorg ervoor dat dit de volgende items bevat:
   * Één enkele behuizing FXT apparaat
   * Twee netsnoeren
   * Een voorpaneel en de sleutel
   * Een spoor kit assembly
   * Een kabel beheertak (CMA)
   * CMA installatie-instructies brochure
   * Rack-installatie-instructies brochure
   * Veiligheid, milieu en wettelijke informatie brochure

Als u hebt ontvangen alle items die worden vermeld, neem dan contact op met de leverancier van het apparaat voor ondersteuning. 

Zorg ervoor dat het apparaat heeft voldoende tijd is voor het bereiken van de dezelfde temperatuur als de ruimte bevindt voordat de installatie of het ingeschakeld. Als u condensvorming op een deel van het apparaat merkt, wacht u minstens 24 uur voordat u installeert.

De volgende stap is het in het rek monteren van uw apparaat.

## <a name="rack-the-device"></a>Het apparaat in het rek monteren

Het apparaat Azure FXT Edge Filer moet in een standard 19-inch rack zijn geïnstalleerd. 

De cache van Azure FXT Edge Filer hybride opslag bestaat uit drie of meer Azure FXT Edge Filer-apparaten. Herhaal de stappen van de installatie rack voor elk apparaat dat deel uitmaakt van uw systeem. 

### <a name="rack-install-prerequisites"></a>Rack vereiste onderdelen installeren

* Voordat u begint, lees de veiligheidsinstructies de veiligheid, milieu en wettelijke informatie bevat die is geleverd met uw apparaat zijn.

  > [!NOTE]
  > Gebruik altijd twee personen wanneer het knooppunt, met inbegrip van wanneer u deze in een rack installeren of uit het rack verwijderen op te heffen. 

* Identificeer het type spoor installatie gebruikt in combinatie met uw rack apparatuur. 
  * Volg de instructies van de gereedschap spoor voor vierkant of ronde hole-module rekken.
  * Volg de instructies van de tooled spoor voor threads hole rekken. 
  
    Voor de tooled spoor configuratie koppelen, moet u acht schroeven, type 10-32, 12 tot 24, M5 of M6 opgeven. De kop diameter van de schroeven moet minder dan 10 mm (0,4-").

### <a name="identify-the-rail-kit-contents"></a>De inhoud van de kit spoor te identificeren

Zoek de onderdelen voor het installeren van de trein kit assembly:

1. Twee A7 Dell ReadyRails II schuiven spoor assembly's (1)
1. Twee van een toepassing aansluiten en banden (2) in een lus

![Genummerde tekening van de trein kit inhoud](media/fxt-install/identify-rail-kit-contents-400.png)

### <a name="rail-assembly---tool-less-rails-square-hole-or-round-hole-racks"></a>Spoor assembly - gereedschap rails (vierkante hole of ronde openingen rekken)

Volg deze procedure om samen te stellen en installeert de rails voor rekken met-module vierkant of ronde gaten. 

1. Plaats de linker- en spoor end-onderdelen met het label **FRONT** naar binnen gericht. Plaats elke stuk end zodat deze in de gaten op de front-zijde van de verticale rackflenzen seats. (1)

2. Lijn van elk onderdeel end in de gaten onder en top van rack in de ruimte die u wilt koppelen. (2)

3. De back-end van de trein betrekken voor de seats op de verticale rack moer totdat het volledig en de vergrendeling op klikt in plaats. Herhaal deze stappen om te plaatsen en het stuk front-end in de verticale rack moer seat. (3)

> [!TIP]
> Als u wilt verwijderen van de rails, halen de vergrendeling release-knop op het end stuk middelpunt (4) en elke spoor unseat.

![Diagram van installeren en verwijderen van gereedschap rails, met de genummerde stappen](media/fxt-install/installing-removing-tool-less-rails-400.png)

### <a name="rail-assembly---tooled-rails-threaded-hole-racks"></a>Spoor assembly - tooled rails (rekken hiërarchische hole)

Volg deze procedure om samen te stellen en installeert de rails voor rekken met threads gaten.

1. De pincodes uit de voor- en achterkant vierkante haken koppelen met een plat draai verwijderen. (1)
1. Ophalen van en het draaien van de trein vergrendeling subverzamelingen als u wilt verwijderen uit de haakjes koppelen. (2)
1. Links en rechts koppelen rails op de front verticale rackflenzen met behulp van twee sets schroeven koppelen. (3)
1. Schuif de linker- en achterkant vierkante haken doorsturen op basis van de achterste verticale rackflenzen en koppelt u ze met behulp van twee sets schroeven. (4)

![Diagram van installeren en verwijderen van tooled rails, met de genummerde stappen](media/fxt-install/installing-removing-tooled-rails-400.png)

### <a name="install-the-system-in-the-rack"></a>Installeren van het systeem in het rek

Volg deze stappen voor het koppelen van het apparaat Azure FXT Edge Filer in het rek te plaatsen.

1. Haal de rails binnenste dia buiten het rack totdat ze vergrendeld in plaats. (1)
1. Zoek de achterste spoor standoff voor elke zijde van het apparaat en deze aan op de achterkant J-sleuven voor de assembly dia's verlagen. (2) 
1. Het apparaat omlaag draaien totdat alle standoffs van de trein worden geplaatst in de "j"-sleuven. (3)
1. Pushen naar binnen het apparaat in totdat de remhendels vergrendeling op naar de juiste plaats.
1. Druk op de knoppen dia-release-vergrendeling op beide rails (4) en schuift u het apparaat in het rek te plaatsen.

![Systeem installeren in een rack-diagram met de genummerde stappen](media/fxt-install/installing-system-rack-400.png)

### <a name="remove-the-system-from-the-rack"></a>Het systeem verwijderen uit het rek

Als het apparaat uit het rack verwijderen, volgt u deze procedure. 

1. Zoek de vergrendeling remhendels aan de zijde van de binnenste rails (1).
2. Maak gebruik van elke hendel door roteren tot de release-positie (2).
3. Het aantal zijden van het systeem goed rapportelementen en naar voren halen totdat het spoor standoffs aan het begin van de "j"-sleuven zijn. Het systeem optillen up en er vandaan lopen van het rack en plaats deze op een niveau oppervlak (3).

![Afbeelding van het verwijderen van een systeem van het rack, met de genummerde stappen](media/fxt-install/removing-system-rack-400.png)

### <a name="engage-the-slam-latch"></a>De vergrendeling slam betrekken

1. De front-end gerichte, de vergrendeling slam (1) Zoek aan beide zijden van het systeem.
2. De vergrendelingen betrekken automatisch als het systeem in het rack is gepusht. 

Als u wilt vrijgeven de vergrendelingen bij het verwijderen van het systeem, er even ze (2).

Optionele harde koppeling schroeven zijn opgegeven voor het beveiligen van het systeem op het rek voor verzending of in andere omgevingen instabiel. De installatie onder elke vergrendeling vinden en ze met een #2 webserverbeheerders kruiskopschroevendraaier (3).

![Afbeelding van aantrekkelijke en het vrijgeven van de vergrendeling slam genummerde](media/fxt-install/engaging-releasing-slam-latch-400.png)

### <a name="install-the-cable-management-arm"></a>De kabel beheertak installeren 

Een beheertak optionele kabel (CMA) is opgegeven met de FXT Edge Filer. Afgedrukte instructies voor het installeren van deze zijn opgegeven in het pakket. 

1. Uitpakken en de onderdelen van de kabel management arm kit identificeren:
   * CMA lade (1)
   * CMA (2)
   * Nylon kabel tie verpakt (3)
   * CMA bijlage vierkante haken (4)
   * Status indicator kabel (5) 

   > [!TIP] 
   > Als u wilt beveiligen de CMA voor verzending in het rek te plaatsen, de tie om zowel opslagplaatsen en lade omloopt lus en ze goed cinch. Beveiligen van de CMA op deze manier wordt bovendien voor beveiliging van uw systeem in omgevingen met instabiel.

   ![Afbeelding van CMA delen](media/fxt-install/cma-kit-400.png)

2. Installeer de lade CMA.

   De lade CMA biedt ondersteuning en fungeert als een totaalbedrag van de CMA. 

   1. Uitlijnen en elke zijde van de lade met de haakjes ontvanger op de binnenste randen van de rails te betrekken. 
   1. Push de lade doorsturen totdat deze naar de juiste plaats op klikt. (1)
   1. Als u wilt verwijderen in de lade, de knoppen vergrendeling-release verondersteld naar het midden- en pull-de lade buiten de ontvanger vierkante haken (2).

   ![Afbeelding van CMA lade installatie](media/fxt-install/cma-tray-install-400.png)

3. Installeer de CMA bijlage vierkante haken. 

   > [!NOTE]
   >
   > * U kunt de CMA koppelen naar rechts of links spoor, afhankelijk van hoe u route kabels uit het systeem wilt koppelen. 
   > * Voor het gemak de CMA te koppelen aan de zijde tegenover de voedingen (side-A). Als deze is gekoppeld aan B, moet de CMA wilt verwijderen van de buitenste voeding worden losgekoppeld. 
   > * Altijd de lade verwijderen voordat het verwijderen van de kracht levert. 

   ![Afbeelding van CMA haakje-installatie](media/fxt-install/cma-bracket-l-r-install-400.png)

   1. Selecteer de juiste CMA bijlage haakje voor de clientzijde waar u wilt de CMA (kant B of kant A) koppelen.
   1. De bijlage CMA haakje met de bijbehorende kant A of kant B markering aan het einde van het spoor dia installeren.
   1. Lijn de gaten op het haakje sluiten met de pincodes op de dia-spoor. De haak beneden wordt geduwd totdat deze Hiermee vergrendelt u naar de juiste plaats. 

4. Installeer de CMA.

   1. Aan het einde van het systeem, past u de vergrendeling op de front-end van de CMA op het binnenste haakje sluiten van de assembly dia totdat de vergrendeling praat met (1). 
   1. Aanpassen aan de andere vergrendeling op het einde van de buitenste haakje totdat de vergrendeling praat met (2). 
   1. Als u wilt verwijderen van de CMA, ontworpen beide vergrendelingen door op de knoppen CMA vrijgeven aan de bovenkant van de binnenste en buitenste vergrendeling ombouw (3) te drukken.

   ![Afbeelding van de belangrijkste CMA-installatie](media/fxt-install/cma-install-400.png)

   De CMA kan worden gedraaid weg van het systeem voor toegang en -service. Aan het einde scharnierend, breng de CMA weg van het systeemvak om unseat (1). Nadat het is niet goed aangesloten vanuit de lade, swing de CMA weg van het systeem (2).

   ![Afbeelding van CMA gedraaid openen voor service](media/fxt-install/cma-swing-over-tray-400.png)

## <a name="install-the-front-bezel-optional"></a>Installeer het voorpaneel (optioneel)

In deze sectie wordt uitgelegd hoe u installeren en verwijderen van het voorpaneel (faceplate) voor de Azure FXT Edge Filer-hardware. 

Het voorpaneel installeren: 

1. Zoek en verwijder de sleutel omlijsting, die is opgegeven in het pakket omlijsting. 
1. De omlijsting met het begin van het chassis uitlijnen en de pincodes aan de rechterkant van de omlijsting invoegen in de gaten op van het knooppunt rechterkant rack mount flens. 
1. Aanpassen aan de linkerkant van de omlijsting naar het chassis. Druk op de omlijsting totdat de knop aan de linkerkant op in plaats klikt.
1. De omlijsting vergrendelen met de sleutel.

Het voorpaneel verwijderen: 
1. Maak gebruik van de omlijsting met behulp van de sleutel omlijsting.
1. Druk op de knop vrijgeven aan de linkerkant en ophalen van de linkerkant van de omlijsting weg van het chassis.
1. Verbreken van de rechterkant en verwijder de omlijsting.
   
   ![Afbeelding van de release-knop aan de linkerkant van de omlijsting en hoe u deze door binnen naar buiten aan de linkerkant te verwijderen](media/fxt-install/remove-bezel-edited-600.png)

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt uitgepakt en het apparaat hebt, gaan met setup door te koppelen aan en netstroom verbinding te maken met de Azure FXT Edge Filer.

> [!div class="nextstepaction"]
> [De netwerkpoorten bekabelen en power opgeven](fxt-network-power.md)