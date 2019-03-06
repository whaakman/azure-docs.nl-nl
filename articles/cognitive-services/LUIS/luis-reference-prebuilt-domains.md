---
title: Vooraf gemaakte domeinverwijzing
titleSuffix: Azure
description: Referentie voor de vooraf gemaakte domeinen vooraf gemaakte verzamelingen van intenties en entiteiten van Language Understanding Intelligent Services (LUIS zijn).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: e1e579233a5ad1af1ef8ee84019cd995959d3b2b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433770"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Naslaginformatie voor vooraf gedefinieerde voor uw LUIS-app
Deze referentie bevat informatie over de [vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md), die zijn vooraf gemaakte verzamelingen van intenties en entiteiten die LUIS biedt.

[Aangepaste domeinen](luis-how-to-start-new-app.md), daarentegen, beginnen met geen intents en modellen. U kunt een vooraf gedefinieerde domein intenties en entiteiten toevoegen aan een aangepast model.

## <a name="list-of-prebuilt-domains"></a>Lijst met vooraf gemaakte domeinen
LUIS biedt 20 vooraf gemaakte domeinen. 

| Vooraf gedefinieerde domein | Description | Ondersteunde talen |
| ---------------- |-----------------------|:------:|
| Agenda | Het domein van de agenda biedt intentie en entiteiten voor toe te voegen, te verwijderen, of bewerken van een afspraak, deelnemers aan de beschikbaarheid gecontroleerd en zoeken naar informatie over een agenda-gebeurtenis.| en-US<br/> zh-CN |
| Camera | Het domein van de Camera biedt intenties en entiteiten voor het maken van afbeeldingen, video's opnemen en uitzenden video aan een toepassing.| en-US |
| Communicatie | Verzenden van berichten en telefoongesprekken.| en-US <br/> zh-CN |
| entertainment  | Verwerking van query's met betrekking tot muziek, films en tv-programma's.| en-US |
| Gebeurtenissen | Reservering van tickets voor concerten, festivals, sport-games en comedy bevat.| en-US |
| Geschiktheid | Afhandeling van aanvragen met betrekking tot het bijhouden van geschiktheid activiteiten.| en-US |
| Gaming | Afhandeling van aanvragen die betrekking hebben op een game partij in een spel.| en-US |
| HomeAutomation | Slimme home apparaten zoals verlichting en apparaten beheert.| en-US<br/> zh-CN |
| MovieTickets | Tickets naar films op een film theater reservering.| en-US |
| Muziek | Het afspelen van muziek op een muziekspeler.| en-US<br/> zh-CN |
| Opmerking | Het domein Opmerking biedt intenties en entiteiten met betrekking tot het maken, bewerken en het zoeken van notities.| en-US<br/> zh-CN |
| OnDevice | Het domein OnDevice biedt intenties en entiteiten die betrekking hebben op het apparaat te beheren.| en-US<br/> zh-CN |
| Plaatsen  | Verwerking van query's die betrekking hebben op locaties zoals bedrijven, instellingen, restaurants, ruimten voor algemeen gebruik en -adressen.| en-US<br/> zh-CN |
| Herinnering | Afhandeling van aanvragen in verband met het maken, bewerken en zoeken van herinneringen.| en-US<br/> zh-CN |
| RestaurantReservation | Afhandeling van aanvragen voor het beheren van restaurant reserveringen.| en-US<br/> zh-CN |
| Over taxi 's | Reserveringen voor een taxi verwerken.| en-US<br/> zh-CN |
| Vertalen | Vertalen van tekst naar een doel-taal.| en-US<br/> zh-CN |
| TV-PROGRAMMA 'S | Tv's beheren.| en-US |
| Nutsbedrijven  | Afhandeling van aanvragen die gemeenschappelijk zijn in veel domeinen, zoals "help", "Herhaal", "opnieuw beginnen."| en-US |
| Weer | Ophalen van de rapporten weer en geeft een prognose.| en-US<br/> zh-CN |
| Web | Navigeren naar een website.| en-US<br/> zh-CN |

Zie de volgende secties voor meer informatie over elk domein.

## <a name="calendar"></a>Agenda 

Het domein van de agenda biedt intenties en entiteiten met betrekking tot de agenda-items. De intenties agenda bevatten toe te voegen, verwijderen of bewerken van een afspraak, de beschikbaarheid gecontroleerd en informatie over een agenda-item of een afspraak zoeken.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Toevoegen | Een nieuwe eenmalige item toevoegen aan de agenda.| Een afspraak met Lisa om 2 uur op zondag <br/><br/>Ik wil een vergadering plannen<br/><br/>Ik wil een vergadering instellen|
| CheckAvailability | Beschikbaarheid voor een afspraak of vergadering agenda van de gebruiker of een andere persoon kalender niet vinden.| Wanneer is Jim beschikbaar om te voldoen? <br/><br/>Weergeven als Carola morgen beschikbaar is<br/><br/>Chris vrij is op zaterdag?|
| Verwijderen | Aanvraag voor het verwijderen van een agenda-item.| Mijn afspraak met Carola annuleren. <br/><br/>Mijn 9: 00-vergadering verwijderen<br/>|
| Bewerken | De aanvraag voor het wijzigen van een bestaande vergadering of een agenda-item.| Mijn 9: 00-vergadering verplaatsen naar 10 uur.<br/><br/>Ik wil mijn planning bijwerken.<br/><br/>Mijn vergadering met Ryan plannen.|
| Find | Mijn wekelijkse agenda weergeven.| Zoek de tandarts afspraak bekijken. <br/><br/>Mijn agenda weergeven<br/>|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Locatie | Locatie van agenda-item, afspraak of vergadering. Adressen, plaatsen en regio's zijn goede voorbeelden van locaties.| 209 Nashville sportschool <br/><br/>897 pancake house<br/><br/>Garage|
| Onderwerp | De titel van een afspraak of vergadering.| De tandarts <br/><br/>Lunch met Julia<br/><br/>De afspraak van Doctor's|

## <a name="camera"></a>Camera 
Het domein van de Camera biedt intenties en entiteiten met betrekking tot het gebruik van een camera. De intenties van toepassing op een foto, selfie, schermopname of video vastleggen en uitzenden video aan een toepassing.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| CapturePhoto| Een foto vastleggen.| Maak een foto<br/><br/>vastleggen|
| CaptureScreenshot | Een schermopname vastleggen.| Schermopname duren.<br/><br/>de schermopname.|
| CaptureSelfie | Een selfie vastleggen.| Een selfie maken <br/><br/>een afbeelding van mij |
| CaptureVideo | Video van opname te starten.| Opname starten <br/><br/>Beginnen met opnemen|
| StartBroadcasting| Broadcasting video te starten.| Uitzenden naar Facebook starten|
| StopBroadcasting| Broadcasting video stoppen.| Broadcasting stoppen|
| StopVideoRecording| Een video-opname stoppen| Dat is voldoende<br/><br/>opname stoppen|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | De naam van een toepassing voor het uitzenden van video.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>Communicatie 
Het domein communicatie biedt intenties en entiteiten die betrekking hebben op e-mail, berichten en telefoongesprekken.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AddContact| Een nieuwe contactpersoon toevoegen aan lijst met contactpersonen van de gebruiker.|Nieuwe contactpersoon toevoegen <br/><br/>Sla dit nummer en de naam op als Carola plaatsen|
| AddMore| Voeg meer toe aan een e-mailadres of de tekst, als onderdeel van een stap voor stap e-mail of SMS-samenstelling.|Voeg meer toe aan de tekst <br/><br/>Meer e-instantie toevoegen|
| Antwoord| Een inkomend telefoongesprek te beantwoorden.|De oproep beantwoorden <br/><br/>Om te kiezen|
| AssignContactNickname| Een bijnaam toewijzen aan een contactpersoon.|Isaac Pa wijzigen <br/>De bijnaam van Jim bewerken<br/>Bijnaam aan Patti Owens toevoegen|
| CallVoiceMail| Verbinding maken met de voicemail van de gebruiker.|Verbinding maken met mijn vak voicemail <br/>Voicemail<br/>Voicemail oproepen|
| CheckIMStatus| Controleer de status van een contactpersoon in Skype.|Is de onlinestatus van Jim ingesteld op opgeslagen? <br/>Carola beschikbaar om te chatten met is?|
| Bevestigen| Controleer of een actie.|Ja<br/>OK<br/>OK<br/>Ik bevestig dat ik wil dit e-mailbericht verzenden.<br/>|
| Inbelverbinding| Een telefoongesprek.|Jim aanroepen<br/>Neem 311 kiezen<br/>|
| FindContact| De contactgegevens op naam vinden.|Carola van nummer zoeken<br/>Carola van getal weergeven<br/>|
| FindSpeedDial| Zoek de snelheid kiezen nummer een telefoonnummer in dat is ingesteld op en vice versa.|Wat is mijn telefoonnummer 5?<br/>Heb ik snelheid set kiezen?<br/>Wat is het telefoonnummer van 941-5555-333?|
| GetForwardingsStatus| De huidige status van de oproep doorschakelen ophalen.|Wordt mijn doorschakelen ingeschakeld?<br/>Meer informatie als de status van de aanroep in- of uitschakelen is<br/>|
| GoBack| Ga terug naar de vorige stap.|Ga terug naar twitter<br/>Een stap terug<br/>Terug|
| Negeren| Een binnenkomende oproep negeren.|Geen antwoord<br/>Aanroep negeren|
| IgnoreWithMessage| Een binnenkomende oproep negeren en reageren met de tekst in plaats daarvan.|Geen deze oproep wordt beantwoord, maar in plaats daarvan een bericht verzenden.<br/>Negeren en terug tekstbericht te verzenden.|
| PressKey| Druk op een knop of een getal op het toetsenbord.|Ster met kiezen.<br/>Druk op 1-2-3.|
| ReadAloud| Lees een bericht of een e-mailbericht naar de gebruiker.|Lees de tekst.<br/>Wat ze zeggen in het bericht?|
| TurnForwardingOff| Een telefoongesprek.|<br/><br/>|
| Kiezen| Kiezen of het opnieuw aanroepen van een getal.|Kies opnieuw.<br/>Mijn laatste aanroep opnieuw.|
| Afwijzen| Een binnenkomende oproep afwijzen.|Oproep afwijzen<br/>Nu kan geen antwoord<br/>Niet beschikbaar is op dit moment en wordt voor de terugbelfunctie later opnieuw.|
| EmailVerzenden| Een e-mail verzenden. Dit doel is van toepassing op e-mailadres, maar geen SMS-berichten.|E-mailadres aan het water Mike: Mike of diner afgelopen week schitterende is.<br/>Een e-mailbericht verzenden naar Bob<br/>|
| SendMessage| Een SMS-bericht of een expresbericht verzenden.|Tekst naar Chris en Carola verzenden|
| SetSpeedDial| Stel een snelkoppeling naar de snelheid kiezen voor het telefoonnummer van een contactpersoon.|Stel snelkiezen een voor Carola.<br/>Snelheid kiezen voor mom instellen.|
| ShowNext| Zie het volgende item, bijvoorbeeld in een lijst van de SMS-berichten of e-mailberichten.|Het volgende object weergeven.<br/>Ga naar de volgende pagina.|
| ShowPrevious| Zie het vorige item, bijvoorbeeld in een lijst van de SMS-berichten of e-mailberichten.|Het vorige voorbeeld weergegeven.<br/>Vorige<br/>Ga naar vorige.|
| Opnieuw| Het systeem opnieuw starten of een nieuwe sessie starten.|Opnieuw beginnen<br/>Nieuwe sessie<br/>restart|
| TurnForwardingOff| Oproep doorschakelen uitschakelen.|Niet langer doorgestuurd van mijn oproepen<br/>Oproep doorschakelen uitschakelen|
| TurnForwardingOn| De telefoon spreker uitschakelen.|Mijn oproepen naar 3333 doorsturen<br/>Oproep doorschakelen aan 3333 inschakelen|
| TurnSpeakerOff| De telefoon spreker uitschakelen.|Breng me uit spreker.<br/>Handsfree uitschakelen.<br/>|
| TurnSpeakerOn| Schakel de sprekerherkenning-telefoon.|Handsfree-modus.<br/>Plaats Handsfree op.<br/>|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AudioDeviceType | Het type van audio-apparaat (spreker, hoofdtelefoon, microfoon, enzovoort).| Spreker<br/>Hands-free<br/>Bluetooth|
| Categorie | De categorie van een bericht of e-mail.| Belangrijk<br/>Hoge prioriteit|
| ContactAttribute | Een kenmerk van de Neem contact op met die de gebruiker query's over.| Verjaardagen<br/>Adres<br/>Telefoonnummer|
| Naam contactpersoon | De naam van de ontvanger van een contactpersoon of een bericht.| Carola<br/>Jim<br/>Chris|
| EmailSubject | De tekst die wordt gebruikt als de onderwerpregel voor een e-mailbericht.| RE: interessant verhaal|
| Regel | De regel van de gebruiker wil gebruiken om te bellen of een tekst/e-mailbericht van verzenden.| Werk regel<br/>Britse cel<br/>Skype|
| Bericht | Het bericht te verzenden als een e-mailadres of tekst.| Het was geweldige u vandaag nog aan. Bekijk u later opnieuw.|
| MessageType | De naam van de ontvanger van een contactpersoon of een bericht.| Tekst<br/>Email|
| OrderReference | De rangtelwoord of relatieve positie in een lijst, het identificeren van een item om op te halen. Bijvoorbeeld, 'last' of 'recente"in"Wat was de laatste ik verzonden bericht?"| Laatste<br/>Recent|
| SenderName | De naam van de afzender.| Patti Owens|

## <a name="entertainment"></a>entertainment  
Het domein biedt intenties en entiteiten die betrekking hebben op te zoeken naar films, muziek, games en muziek Entertainment bevat.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Search| Zoeken naar films, muziek, apps, games en tv-programma's bevat.|Zoek in de store naar Halo.<br/>Zoeken naar Avatar.|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| ContentRating | Media inhoudsrestricties, zoals G of R voor films.|Video van kinderen.<br/>PG beoordeeld.|
| Genre | Het genre van een film, games, app of nummer.|Comedies<br/>Dramas<br/>Grappige|
| Trefwoord| Een algemene trefwoord op te geven van een kenmerk de bestaat niet in de specifiekere media sleuven.|Soundtracks<br/>Maan-rivier<br/>Amelia Earhart|
| Taal | De taal die wordt gebruikt in de media, zoals gesproken taal van de film of een nummer.|Frans<br/>Nederlands<br/>Koreaans|
| MediaFormat | De aanvullende speciale technische type waarin het medium is geformatteerd.|HD-films<br/>3D-films<br/>Downloadbare|
| MediaSource | De store of de marketplace voor het verkrijgen van de media.|NetFlix<br/>Prime|
| MediaSubTypes| Media die kleiner is dan films en games.|Demo 's<br/>De DLC<br/>Aanhangwagen|
| Nationaliteit| Het land waar een film, weergeven of nummer is gemaakt.|Frans<br/>Duits<br/>Koreaans|
| Person| De actor, director, producent, muzikant of artiest die zijn gekoppeld aan een film-, app-, spel- of tv-programma.|Madonna<br/>Stanley Kubrick|
| Rol| Rol van een persoon bij het maken van media.|Sings<br/>Omgeleid door<br/>Door|
| Titel| De naam van een film, app, games, tv-programma of nummer.|Vrienden<br/>Minecraft|
| Type| Het type of media-indeling van een film, app, games, tv-programma of nummer.|Muziek<br/>MovieTV <br/>bevat|
| UserRating| Gebruiker ster of thumbs waardering.|5 sterren<br/>3 sterren<br/>4 sterren|

## <a name="events"></a>Gebeurtenissen 
Het domein van de gebeurtenissen biedt intenties en entiteiten die betrekking hebben op reservering tickets voor gebeurtenissen, zoals concerten, festivals, sport-games en comedy wordt weergegeven.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Book| Tickets op een gebeurtenis kopen.|Ik wil graag een ticket voor de symphony dit weekend kopen.|


### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres | Locatie van de gebeurtenis of het adres. |Palo Alto<br/>300 112th Ave SE <br/> Seattle |
| Name | De naam van een gebeurtenis.|Shakespeare in het strand|
| PlaceName| De naam van de gebeurtenis-locatie.|Louvre<br/>Opera House<br/>Gewijzigd in Broadway|
| PlaceType | Het type van de locatie van de gebeurtenis worden ondergebracht in.|Café<br/>Theater<br/>Bibliotheek|
| Type | Het type van een gebeurtenis.|Concert<br/>Sport-game|

## <a name="fitness"></a>Geschiktheid 
Het domein geschiktheid biedt intenties en entiteiten met betrekking tot het bijhouden van geschiktheid activiteiten. De intenties bevatten opslaan, notities, resterende tijd of afstand of het opslaan van Activiteitsresultaten.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AddNote| Aanvullende opmerkingen toevoegen aan een activiteit die wordt bijgehouden.|De moeite van het uitvoeren is 6/10<br/>Het terrein dat ik ben over het uitvoeren van op asfalt is<br/>Ik gebruik een fiets 3 snelheid|
|GetRemaining| Hiermee haalt de resterende tijd of afstand voor een activiteit.|Hoeveel tijd tot de volgende lap?<br/>Hoeveel mijl blijven in mijn uitvoeren? Hoeveel tijd voor de splitsing?|
| LogActivity| Opslaan of resultaten van de voltooide activiteit vastleggen.|Opslaan van de laatste uitvoering<br/>Meld u Mijn zaterdag ochtend-overzicht<br/>Mijn vorige zwem opslaan|
| LogWeight| Opslaan of de huidige gewicht van de gebruiker zich.|Mijn huidige gewicht opslaan<br/>Mijn gewicht nu aanmelden<br/>Mijn huidige instantie gewicht opslaan|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| ActivityType | Het type van de activiteit om bij te houden. |Voer<br/>Lopen<br/>Zwemmen<br/>Cyclus |
| Voedsel | Een type food om bij te houden in een app geschiktheid. |Bananen<br/>Zalmkleurig<br/>Door te schudden eiwitten|
| MealType| Het type maaltijd om bij te houden in een status- of geschiktheid app.|Ontbijt<br/>Diner<br/>Lunchpauze<br/>Supper|
| Meting| Een type van de metingen voor tijd, afstand of het gewicht voor gebruik in een app geschiktheid of status.|Kilometer zijn verwijderd<br/>Mijl<br/>Minuten<br/>Kg|
| Aantal | Een numerieke hoeveelheid voor gebruik in een app geschiktheid of status.|19<br/>drie<br/>200<br/>één|
| StatType | Een type statistiek op geaggregeerde gegevens, voor gebruik in een app geschiktheid of status.|Sum<br/>Gemiddeld<br/>Maximum<br/>Minimum|

## <a name="gaming"></a>Gaming 
Het domein Gaming biedt intenties en entiteiten met betrekking tot het beheren van een game partij in een spel.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| InviteParty| Een contactpersoon voor deelname aan een gaming-party uitnodigen.|De speler aan mijn partij uitnodigen<br/>Worden geleverd aan mijn partij<br/>Lid worden van mijn clan|
|LeaveParty| Hiermee haalt de resterende tijd of afstand voor een activiteit.|Ik ben uit<br/>Ik heb deze partij voor een andere verlaten<br/>Ik ben afsluiten|
| StartParty| Start een partij games in een spel.|Laten we man starten van een partij<br/>starten van een partij<br/>we moeten een clan vanavond beginnen|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Contactpersoon| De naam van een contactpersoon in een spel gebruiken.|Carola<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
Het domein HomeAutomation biedt intenties en entiteiten die betrekking hebben op de slimme home apparaten zoals verlichting en apparaten te beheren.

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Uitschakelen| Uit te schakelen, sluiten of een apparaat kan worden ontgrendeld.|De verlichting uitschakelen<br/>De maker van de koffie stoppen<br/>Garagedeur die sluiten|
|Inschakelen| Schakel op een apparaat of het apparaat ingesteld op een bepaalde instelling of modus.|de maker van mijn koffie inschakelen<br/>kunt u de maker van mijn koffie inschakelen?<br/>Stel de thermostaat op 72 graden.|


### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Apparaat | Een type apparaat dat kan worden ingeschakeld in- of uitschakelen.|koffie maker<br/>Thermostaat<br/>licht|
| Bewerking | De status van het apparaat.|vergrendelen<br/>open<br/>op<br/>uit|
| Ruimte | De locatie of het apparaat zich in de ruimte.|Woonkamer<br/>slaapkamers<br/>keuken|

## <a name="movietickets"></a>MovieTickets 
Het domein MovieTickets biedt intenties en entiteiten met betrekking tot reservering tickets naar films op een theater film.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Boek me twee tickets voor kapitein Omar en de twee Musketeers|
|Tickets annuleren|
|Wanneer wordt kapitein Omar weergegeven?|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Book | Film tickets kopen.|Boek me twee tickets voor kapitein Omar en de twee musketeers<br/>Ik wil een ticket voor morgen film kopen<br/>Ik wilt een ticket kapitein Omar deel 2 volgende woensdag|
|GetShowTime| Haal de showtime van een film.|Wanneer wordt kapitein Omar weergegeven?|


### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres | Het adres van een theater film.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| MovieTitle | De titel van een film.|Levensduur van Pi<br/>Hunger Games<br/>Begin|
| PlaceName | De naam van een film theater of bioscoopvertoningen.|Bioscoopvertoningen Amir<br/>Alexandrië Theater<br/>New York Theater|
| PlaceType | Het type van een film wordt weergegeven op locatie.|bioscoopvertoningen<br/>Theater<br/>IMAX bioscoopvertoningen|

## <a name="music"></a>Muziek 
Het domein muziek biedt intenties en entiteiten met betrekking tot het afspelen van muziek op een muziekspeler.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Beethoven afspelen|
|spoor volume verhogen|
|Gaat u naar het volgende nummer|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| DecreaseVolume | De apparaat-volume te verlagen.|spoor volume te verlagen<br/>volume omlaag|
| IncreaseVolume | Verhoog het volume van het apparaat.|spoor volume verhogen<br/>volume van|
| Dempen |Dempen van de afgespeelde muziek.|Dempen nummer<br/>De track op dempen plaatsen<br/>Dempen muziek |
| Onderbreken | Onderbreken van de afgespeelde muziek.|Onderbreken<br/>Onderbreken muziek<br/>Onderbreken bijhouden|
| PlayMusic | Het afspelen van muziek op een apparaat.|Kevin Durant afspelen<br/>Paradise door Coldplay afspelen<br/>Hallo door songs afspelen|
| Herhaal deze procedure |Herhaal de afgespeelde muziek.|Herhaal de nummer<br/>De track winst afspelen<br/>Herhaal de muziek|
| Hervatten | Hervatten van de afgespeelde muziek.|Nummer hervatten<br/>Muziek opnieuw starten<br/>Hervatten|
| SkipBack | Overslaan achterwaartse een nummer.|Gaat u naar het volgende nummer<br/>Het volgende nummer afspelen|
| SkipForward |Een nummer vooruit springen.|De vorige nummer afspelen<br/>Ga terug naar de vorige bijhouden |
| Stoppen | Een actie met betrekking tot het afspelen van muziek stoppen. |Dit album afspelen stoppen.|
| Dempen opheffen | Een afspelen van muziekapparaat uitschakelen.| Dempen opheffen.|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| ArtistName | De actor, director, producent, schrijver, muzikant of artiest media om af te spelen op een apparaat dat is gekoppeld.|ELVIS Presley<br/>Taylor Swift<br/>Songs<br/>Mozart|
| Genre | Het genre muziek wordt aangevraagd.|Land/regio muziek<br/>Gewijzigd in Broadway klassiekers<br/>Mijn klassieke muziek van de Barok periode|

## <a name="note"></a>Opmerking 
Het domein Opmerking biedt intenties en entiteiten met betrekking tot het maken, bewerken en het zoeken van notities.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Toevoegen aan mijn boodschappen Opmerking Sla tomaten brood koffie|
|Schakel bananen in mijn lijst kruidenierswaren|
|Alle items uit de lijst van Mijn vakantie verwijderen|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AddToNote | Gegevens toevoegen aan een opmerking.|Toevoegen aan mijn boodschappen Opmerking Sla tomaten brood koffie<br/>Toevoegen aan mijn takenlijst<br/>cupcakes toevoegen aan mijn Wunderlist|
| CheckOffItem | Items uit een bestaande notitie afvinken.|Schakel bananen in mijn lijst kruidenierswaren<br/>Kaas taart op Mijn vakantie winkelen lijst als voltooid markeren|
| Wissen | Alle items uit een bestaande notitie wissen.|Alle items uit de lijst van Mijn vakantie verwijderen<br/>Alles wissen uit de lijst van mijn lezen|
| Bevestigen | Controleer of een actie met betrekking tot een opmerking.|Dit klopt door mij<br/>ja<br/>Ik ben bevestigen zorgt ervoor dat alle items op een lijst met|
| Maken | Maak een nieuwe notitie. | Een lijst maken<br/>Houd er rekening mee om u te herinneren dat Jason is in een stad eerste week van mei|
| Verwijderen | Een volledige notitie verwijderen. |Mijn lijst vakantie verwijderen <br/>Mijn boodschappen Opmerking verwijderen|
| DeleteNoteItem | Items verwijderen uit een bestaande opmerking.| Chips verwijderen uit de lijst van mijn kruidenierswaren<br/>Pennen van mijn school winkelen lijst verwijderen|
| ReadAloud | Een lijst hardop lezen.|Lees het eerste item<br/>Lees de details|
| ShowNext | Zie het volgende item in een lijst van notities.|Het volgende object weergeven<br/>Volgende pagina<br/>Volgende|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | De naam van de notities.|Wunderlist<br/>OneNote|
| Naam contactpersoon | De naam van een contactpersoon in een opmerking.|Carola<br/>Jim<br/>Chris|
| Gegevensbron | Locatie van notities.|OneDrive<br/>Google docs<br/>Mijn computer|
| Gegevenstype | Het type van het bestand of document, meestal samenhangen met bepaalde softwareprogramma's.|Dia 's<br/>Spreadsheet<br/>Werkblad|
| Tekst | De tekst van een opmerking of een herinnering.|voordat u walking uitbreiden<br/>lange termijn morgen|
| Titel | De titel van een opmerking.|boodschappen<br/>mensen om aan te roepen<br/>taak|

## <a name="ondevice"></a>OnDevice 
Het domein OnDevice biedt intenties en entiteiten die betrekking hebben op het apparaat te beheren.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Sluit de videospeler|
|Afspelen annuleren|
|Kunt u het scherm lichtere?|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AreYouListening | Vraag als het apparaat luistert.|is dit op?<br/>u luisteren?|
|CloseApplication|Sluit de apparaattoepassing.|Sluit de videospeler|
|FileBug|Een bug melden op het apparaat.|een bug.<br/>Kunt u een bug voor mij?<br/>Ik wil deze fout melden|
|GoBack|Vraag om één stap terug of Ga terug naar de vorige stap.|Ga terug.<br/>Ga naar het vorige scherm<br/>Ga terug stoppen luisteren|
|Help| Hulp vragen.|Help.<br/>Hallo!<br/>Wat kunt u doen?<br/>Ik heb nodig hulp| 
|LocateDevice|Ga naar het apparaat.|U kunt mijn telefoon vinden<br/>Zoek mijn iphone tom's<br/>Mijn telefoon zoeken|
|Aanmelding|Meld u aan bij een service met behulp van het apparaat.|Aanmelding.<br/>Facebook-aanmelding.<br/>Meld u aan bij LinkedIn|
|Afmelden|Meld u af bij van een service met behulp van het apparaat.|Mijn telefoon afmelden<br/>Meld u aan bij twitter<br/>Afmelden|
|MainMenu|Het hoofdmenu van een apparaat weergeven.|Menu Beeld.|
|OpenApplication|Hiermee opent u een toepassing op het apparaat.|Open de waarschuwing.<br/>Camera inschakelen<br/>Agenda openen|
|OpenSetting|Open een instelling op het apparaat.|Open de netwerkinstellingen.|
|PairDevice|Koppel het apparaat.|U kunt mij in het koppelen van Bluetooth-signaal naar telefoon helpen<br/>De bluetooth inschakelen en koppel het aan een laptop<br/>Bluetooth-signaal op mijn laptop worden gekoppeld|
|Zijn uitgeschakeld | Het apparaat uitschakelen.|U kunt de computer afsluiten<br/>Afsluiten<br/>Mijn mobiele apparaat uitschakelen|
|QueryBattery|Informatie ophalen over acculevensduur hebben.|Levensduur van de batterij weergeven.<br/>Wat is de accustatus van mijn<br/>Hoeveel accu links nu?<br/>Toon accu|
|QueryWifi|Informatie ophalen over Wi-Fi.|Wi-Fi-gegevens opvragen.|
|Opnieuw starten|Het apparaat opnieuw te starten.|Start opnieuw.|
|RingDevice| Het apparaat ring, vragen om te kunnen terugvinden wanneer deze is verbroken.|Mijn telefoon ring.| 
|SetBrightness|Stel de helderheid van het apparaat.|De helderheid instellen op Gemiddeld<br/>Helderheid op Hoog instellen<br/>Helderheid op laag instellen|
|SetupDevice|Start de installatie van apparaat.|Ik wil installeren OS setup<br/>Setup.<br/>Voer setup voor mij|
|ShowAppBar|Een app-balk weergeven.|De balk weergeven<br/>Toepassing balk Neem<br/>Ik zie de balk|
|ShowContextMenu|Weergeven in het contextmenu.|Ik zie het contextmenu<br/>In het contextmenu.<br/>Ik kan zien het contextmenu|
|Slaapstand|Plaats het apparaat naar de slaapstand.|Ga slapen<br/>Slaapstand<br/>Mijn slaapstand|
|SwitchApplication|Schakel over naar de toepassing te gebruiken op het apparaat.|Schakel over naar Mijn MediaPlayer.|
|TurnDownBrightness|Schakel omlaag helderheid van het apparaat.|Het scherm dimmen.|
|TurnOffSetting|De Apparaatinstelling van een uitschakelen.|Bluetooth deactiveren<br/>Gegevens uitschakelen<br/>Bluetooth verbreken|
|TurnOnSetting|Schakel de Apparaatinstelling van een.|Aan <br/> Uit|
|TurnUpBrightness|Inschakelen van de helderheid van het apparaat.|Kunt u het scherm lichtere?|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | De naam van een toepassing op het apparaat.|SoundCloud<br/>YouTube|
| BrightnessLevel | Het helderheidsniveau van de ingesteld op het apparaat.|Honderd procent<br/>Vijftig<br/>40%|
| Naam contactpersoon | De naam van een contactpersoon op het apparaat.|Paul<br/>Maximum aantal Marlen|
| Apparaattype | Het type apparaat. |Telefoon<br/>Kindle<br/>Laptop|
| MediaType | Het mediatype dat is verwerkt door het apparaat.|Muziek<br/>Film<br/>Tv-programma 's|
| Settingtype hebben | Een type instelling of het instellingenpaneel die de gebruiker wil bewerken.|Wi-Fi<br/>Draadloos netwerk<br/>Kleurenschema<br/>Meldingencentrum|

## <a name="places"></a>Plaatsen  
Het domein locaties biedt intents voor het verwerken van query's die betrekking hebben op locaties zoals bedrijven, instelling, restaurants, ruimten voor algemeen gebruik en -adressen.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Deze locatie opslaan in mijn Favorieten|
|Hoe ver weg Inn vakantie is?|
|Op welk tijdstip wordt Safeway gesloten?|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Voeg een locatie toe aan de lijst Favorieten van de gebruiker.|Deze locatie opslaan in mijn Favorieten<br/>Dit adres toevoegen aan mijn Favorieten|
|CheckAccident|Vraag of er een ongeluk beschikbaar is op een opgegeven weg.|Is er een ongeluk op 880?<br/>Ongeluk informatie weergeven|
|CheckAreaTraffic|Controleer het verkeer voor een algemene gebied of weg, niet op een opgegeven route.|Verkeer in Seattle<br/>Wat is het verkeer, zoals in Seattle?|
|CheckIntoPlace|Controleer in op een plaats met behulp van sociale media.|Ik wil inchecken op Foursquare<br/>Kom hier|
|CheckRouteTraffic| Controleer het verkeer van een specifieke route die is opgegeven door de gebruiker.|Hoe wordt het verkeer naar Mashiko?<br/>Het verkeer naar Kirkland weergeven<br/>Hoe wordt het verkeer naar Seattle?| 
|Bevestigen|Controleer of een actie die betrekking hebben op een plaats.|Controleer of de reservering van mijn restaurant.|
|Afsluiten|Actie om af te sluiten van een taak met betrekking tot een centrale locatie.|Sluit het.<br/>Sluit mij aanwijzingen geven|
|FindPlace|Zoeken naar een locatie (business, instelling, restaurant, openbare ruimte, adres).|Waar is de dichtstbijzijnde bibliotheek?<br/>Me een goede Italiaanse restaurant vinden in de weergave Mountain|
|GetAddress| Vraag voor het adres van een centrale locatie.|Het adres van Guu in Robson straat weergeven<br/>Wat is het adres van de dichtstbijzijnde ergens?| 
|GetDistance|Vragen over afstand tot een bepaalde locatie.|Hoe ver weg Inn vakantie is?<br/>hoe ver is het vierkante Bellevue hier<br/>Wat is de afstand tot Tahoe|
|GetHours|Vragen over de operationele uur voor een centrale locatie.|Op welk tijdstip wordt Safeway gesloten?<br/>Wat zijn de uren voor Start Depot?<br/>Is er ergens nog steeds openen?|
|GetMenu|Vragen om het menu-items voor een restaurant.|Zucca biedt fungeren iets Veganistische?<br/>Wat is er in het menu Sizzler<br/>Toon Applebee van menu|
|GetPhoneNumber| Vragen om het telefoonnummer van een centrale locatie.|Wat is het telefoonnummer van de dichtstbijzijnde ergens?<br/>Geeft het aantal voor Start Depot| 
|GetPriceRange| Wordt u gevraagd voor het prijsbereik van een centrale locatie.|Zucca goedkoop is?<br/>Is de halve prijs aangeboden Cineplex op woensdag?<br/>Wat kost een hele lobster diner op Sizzler?|
|GetReviews|Beoordelingen van een plaats om vragen.|Beoordelingen voor Cheesecake Factory weergeven<br/>Cineplex beoordelingen in Yelp lezen|
|GetRoute|Vragen in een locatie.|Hoe worden uitgelegd op Bellevue vierkant<br/>De kortste manier om de 8 en 59th hier weergeven<br/>Hoe kom ik routebeschrijving uitzicht op CA|
|GetStarRating|Vragen om de beoordeling van een centrale locatie.|Hoe wordt Zucca beoordeeld op basis van Yelp?<br/>Hoeveel sterren is er in het Frans wasgoed?<br/>Het aquarium in Monterrey goed is?|
|GetTransportationSchedule|De planning bus ophalen voor een plaats.|Welk moment is het volgende bus centrum?<br/>De bussen in King County weergeven|
|GetTravelTime|Vraag om de reistijd voor een opgegeven bestemming.|Hoe lang duurt het voor de San Francisco komen vanaf deze locatie<br/>Wat is de rijtijd naar Denver van SF|
|MakeCall|Bellen naar een locatie.|Roep mom<br/>Ik wil graag wilt Anna Skype bellen<br/>Jim aanroepen|
|MakeReservation|Een reservering voor een restaurant of andere zakelijke vragen.|Op Zucca reserveren voor twee voor tonight<br/>Boek een tabel voor morgen<br/>Tabel 3 in Palo Alto op 8|
|MapQuestions|Aanvragen van informatie over instructies of of een opgegeven weg overschakelt naar een bestemming.|Voldoet 13 aan in het centrum van?<br/>Kan ik duurt het voordat 880 Oakland?|
|Classificatie|De beschrijving van de beoordeling van een restaurant of een plaats ophalen.|Hoeveel sterren hebben de Contoso-Inn?|
|ReadAloud|Een lijst met locaties hardop lezen.|Lees het eerste item<br/>Lees de details|
|SelectItem|Kies een item uit een lijst van de opties die betrekking hebben op een plaats of plaatsen.|Kies de tweede waarde<br/>Selecteer de eerste|
|ShowMap|Een overzicht van een gebied weergeven.|Een kaart voor de tweede waarde weergeven<br/>Kaart weergeven<br/>San Francisco vinden op de kaart|
|ShowNext|Het volgende item in een reeks weergeven.|Het volgende object weergeven<br/>Ga naar de volgende pagina|
|ShowPrevious|Het vorige item weergeven in een serie.|Vorige weergeven<br/>vorige<br/>Ga naar vorige|
|Opnieuw|De app opnieuw starten of een nieuwe sessie starten.|Opnieuw beginnen<br/>Nieuwe sessie<br/>
restart|
|TakesReservations|Vragen of een plaats reserveringen accepteert.|Accepteert de Galerie WordArt reserveringen<br/>Is het mogelijk om een reservering op de Olive Garden maken

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| AbsoluteLocation | De locatie of het adres van een centrale locatie.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Recreatie | De servicedoelstelling kenmerken/voordelen van een centrale locatie.|kinderen eten gratis<br/>afgebakend<br/>gratis vervangende domeinpagina|
| Lucht | De omgeving van een centrale locatie.|kind-vriendelijk<br/>illegaal restaurant<br/>sportief|
| Cuisine | De cuisine van een centrale locatie. |Middellandse<br/>Italiaans<br/>Indiase|
| DestinationAddress| Een nieuwe locatie of het adres.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName| De naam van een doel dat is een bedrijf, restaurant, openbare voordelen of instelling.|centrale park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Het type van een doel dat is een lokale bedrijven, restaurant, openbare voordelen of instelling. |Restaurant<br/>Opera<br/>bioscoopvertoningen|
| afstand | De afstand op een plaats.|15 mijl<br/>5 mijl<br/>10 mijl opslaan|
| MealType | Het type maaltijd zoals ontbijt of lunch. |Ontbijt<br/>Diner<br/>Lunchpauze<br/>Supper|
| OpenStatus | Geeft aan of een plaats open of gesloten is.|Open<br/>Gesloten<br/>openen|
| PlaceName | De naam van een centrale locatie.|Cheesecake Factory|
| PlaceType | Het type van een centrale locatie.|Café<br/>Theater<br/>Bibliotheek|
| PreferredRoute | De voorkeursroute is opgegeven door de gebruiker. | 101 <br/>202 <br/>Route 401|
| Product | Het product die worden aangeboden door een centrale locatie. | Kleding<br/>ASR digitale camera 's<br/>Nieuwe aquarium | 
| PublicTransportationRoute | De naam van de route voor openbaar vervoer die naar de gebruiker zoekt. | Noordoosten bandbreedte train<br/>Bus route 3 X |
| Classificatie | De classificatie van een centrale locatie. | 5 sterren<br/>3 sterren<br/>4 sterren|
| RouteAvoidanceCriteria | Criteria voor het voorkomen van specifieke routes zoals ongevallen, constructies of tolwegen voorkomen | Tolwegen <br/>Constructies<br/>Route 11|
| ServiceProvided | Dit is de service van een bedrijf of een locatie zoals kapper, willen plowing, waarde van onze. | kapper<br/>Mechanic<br/>loodgieter|
| TransportationCompany | De naam van een transportprovider.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Het soort vervoer.|Bus<br/>Trainen<br/>Auto|

## <a name="reminder"></a>Herinnering 
Het domein herinnering biedt intenties en entiteiten voor het maken, bewerken en zoeken naar herinneringen.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Mijn interview 9 uur morgen wijzigen|
|Herinner me aan te schaffen melk op mijn manier opnieuw start|
|Kunt u als ik een herinnering over de verjaardag van Christine controleren?|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Wijzigen| Een herinnering wijzigen.|Mijn interview 9 uur morgen wijzigen<br/>Mijn herinnering toewijzing naar morgen verplaatsen|
| Maken| Maak een nieuwe herinnering.|Een herinnering maken<br/>Herinner me aan te schaffen melk<br/>Ik wil Vergeet niet om aan te roepen Rebecca wanneer ik bij u thuis ben|
| Verwijderen | Een herinnering verwijderen.|De herinnering van mijn afbeelding verwijderen<br/>Deze herinnering verwijderen|
| Find | Zoek een herinnering.|Heb ik een herinnering over mijn verjaardag?<br/>Kunt u als ik een herinnering over de verjaardag van Christine controleren?|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Tekst | De beschrijving van een herinnering.|pikken chemisch reinigen<br/>mijn auto verwijderen uitschakelen in het midden van de service|

## <a name="restaurantreservation"></a>RestaurantReservation 
Het domein RestaurantReservation biedt intenties en entiteiten met betrekking tot het beheren van restaurant reserveringen.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Op Zucca reserveren voor twee voor tonight|
|Boek een tabel met de BJ voor morgen|
|Tabel 3 in Palo Alto op 7|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Reserve | Vraag een reservering voor een restaurant. |Op Zucca reserveren voor twee voor tonight<br/>Boek een tabel voor morgen<br/>Tabel 3 in Palo Alto op 7|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres| Een locatie van de gebeurtenis of het adres voor een reservering.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Recreatie | Een kenmerk met een beschrijving van de voorzieningen van een centrale locatie.|in de Indische Oceaan weergeven<br/>niet soorten|
| AppName | De naam van een toepassing voor het maken van reserveringen.|TabelOpenen<br/>Yelp<br/>TripAdvisor|
| Lucht | Een beschrijving van de omgeving van een restaurant of een andere locatie.|Romantisch<br/>illegaal<br/>goed voor groepen|
| Cuisine | Een type eten, cuisine of cuisine nationaliteit. |Chinees<br/>Italiaans<br/>Mexicaanse|
| MealType | Een maaltijd-type dat is gekoppeld aan een reservering.|Ontbijt<br/>Diner<br/>Lunchpauze<br/>Supper|
| PlaceName | De naam van een lokale bedrijven, restaurant, openbare voordelen of instelling.|IHOP<br/>Cheesecake Factory<br/>Louvre|
| PlaceType | Het type van een lokale bedrijven, restaurant, openbare voordelen of instelling.|Restaurant<br/>Opera<br/>bioscoopvertoningen|
| Classificatie | De classificatie van een plaats of een restaurant.|5 sterren<br/>3 sterren<br/>4 sterren|

## <a name="taxi"></a>Over taxi 's 
 
Het domein Taxi biedt intenties en entiteiten voor het maken en beheren van taxi-reserveringen.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Hoe kom ik een CAB-bestand om 3 uur|
|Hoe lang heb ik mijn taxi wachten?|
|Mijn Uber annuleren|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Book | Een taxi aanroepen. |Hoe kom ik een CAB-bestand<br/>Een taxi zoeken<br/>Boek me een uber x|
| Annuleren | Een actie met betrekking tot een taxi reservering annuleren.|Mijn taxi annuleren<br/>Mijn Uber annuleren|
| Bijhouden | Een taxi route traceren.|Hoe lang heb ik mijn taxi wachten?<br/>Waar bevindt mijn Uber?|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres| Het adres dat is gekoppeld aan een taxi reservering. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationAddress| Een nieuwe locatie of het adres. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName | De naam van een doel dat is een lokale bedrijven, restaurant, openbare voordelen of instelling. |Centrale Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Het type van een doel dat is een lokale bedrijven, restaurant, openbare voordelen of instelling. |Restaurant<br/>Opera<br/>bioscoopvertoningen|
| PlaceName | De naam van lokale bedrijven, restaurant, openbare voordelen of instelling. |Centrale Park<br/>Safeway<br/>Walmart|
| PlaceType| Het type van plaats in een aanvraag voor een taxi het adresboek.|Restaurant<br/>Opera<br/>bioscoopvertoningen|
| TransportationCompany | De naam van een transportprovider.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Het soort vervoer.|Bus<br/>Trainen<br/>Auto|

## <a name="translate"></a>Vertalen 
Het domein vertalen biedt intenties en entiteiten met betrekking tot het vertalen van tekst naar een doel-taal.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Vertalen naar Frans|
|Hallo Duits vertalen|
|Deze zin in het Engels vertalen|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Vertalen| Vertaal tekst naar een andere taal.|Vertalen naar Frans<br/>Hallo Duits vertalen|


### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| TargetLanguage | De doeltaal van een vertaling.|Frans<br/>Duits<br/>Koreaans|
| Tekst | Te vertalen tekst.|Hallo wereld<br/>Goedemorgen<br/>Goedenavond|

## <a name="tv"></a>TV-PROGRAMMA 'S 
 
Het domein tv-programma biedt intenties en entiteiten voor het beheren van tv's.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Switchkanaal BBC|
|Tv-handleiding weergeven|
|Nationale geografische bekijken|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| ChangeChannel| Een kanaal op een tv-programma's wijzigen.|Wijziging CNN-kanaal<br/>Switchkanaal BBC<br/>Ga naar kanaal 4|
| ShowGuide| De handleiding tv-programma's weergeven.|Tv-handleiding weergeven<br/>Wat is nu in film-kanaal ontvangen?<br/>Mijn lijst met programma's weergeven|
| WatchTV| Vragen om te bekijken van een tv-kanaal.|Ik wil het kanaal Disney bekijken<br/>Ga naar tv-programma's.<br/>Nationale geografische bekijken|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| ChannelName | De naam van een tv-kanaal.|CNN<br/>BBC<br/>Film-kanaal|

## <a name="utilities"></a>Nutsbedrijven  
Het domein van de hulpprogramma's biedt intents voor taken die gemeenschappelijk voor veel taken, zoals begroetingen, annuleren, bevestiging, help, herhaling, navigatie zijn, starten en stoppen.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Ga terug naar Twitter|
|Help|
|Herhaal de laatste vraag.|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Annuleren | Een actie annuleren.|Annuleren van het bericht<br/>Ik wil niet meer het e-mailbericht verzenden|
| Bevestigen | Controleer of een actie.|Ja bevestigen oh ik<br/>Ik ben goed bevestigen<br/>Oké, die ik ben bevestigen|
| FinishTask | Een taak die de gebruiker begon voltooien.|Ik ben klaar<br/>Ik ben klaar<br/>Dit is voltooid|
| GoBack | Ga naar de vorige stap, of Ga terug naar een vorige stap.|Ga terug naar Twitter<br/>Een stap terug<br/>Terug|
| Help | Aanvraag voor hulp.|Help<br/>help openen<br/>Help|
| Herhaal deze procedure | Een actie herhalen.|Herhaal de laatste vraag.<br/>Herhaal de laatste nummer|
| ShowNext | Het volgende item in een reeks weergeven. |Het volgende object weergeven<br/>Ga naar de volgende pagina|
| ShowPrevious | Het vorige item weergeven in een serie.|Vorige weergeven|
| Opnieuw | De app opnieuw starten of een nieuwe sessie starten.|Opnieuw beginnen<br/>Nieuwe sessie<br/>restart|
| Stoppen | Een actie stoppen.| Dit zeggen stoppen<br/>Afsluiten van<br/>Stoppen.|

## <a name="weather"></a>Weer 
Het domein weer biedt intenties en entiteiten voor het ophalen van de rapporten weer en geeft een prognose.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|weer in Londen in september|
|Wat? s de tien dagen prognose?|
|Wat is de gemiddelde temperatuur in India in september?|


### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| GetCondition | Historische gegevens met betrekking tot de weersomstandigheden ophalen. |weer in Londen in September<br/>Wat is de gemiddelde temperatuur in India in September?|
| GetForecast | De huidige weersvoorspelling opvragen en prognose voor de komende dagen. |Hoe wordt het weer vandaag?<br/>Wat is de tien dagen prognose?<br/>Hoe worden de weersomstandigheden dit weekend?|

### <a name="entities"></a>Entiteiten
| Naam van de entiteit | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Locatie| De absolute locatie voor een aanvraag weer.|Seattle<br/>Parijs<br/>Palo Alto|

## <a name="web"></a>Web 
Het domein Web biedt een doel voor het navigeren naar een website.

### <a name="examples"></a>Voorbeelden

|Voorbeelden|
|--|
|Navigeer naar facebook.com|
|Ga naar www.twitter.com|
|Navigeer naar www.bing.com|

### <a name="intents"></a>Intents
| De naam van de intentie | Description | Voorbeelden |
| ---------------- |-----------------------|----|
| Navigeren | Een aanvraag om naar een opgegeven website te gaan. |Navigeer naar facebook.com<br/>Ga naar www.twitter.com|

