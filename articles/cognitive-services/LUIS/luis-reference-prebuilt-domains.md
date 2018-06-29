---
title: Overzicht van vooraf gedefinieerde domain - Azure | Microsoft Docs
titleSuffix: Azure
description: Verwijzing voor de vooraf gedefinieerde domeinen vooraf gedefinieerde verzamelingen van intents en entiteiten van Language Understanding Intelligent Services (LUIS zijn).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 14c53bd25913922a0cd7cc438ad0fbe8b4663dd1
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061961"
---
# <a name="prebuilt-domain-reference"></a>Vooraf gemaakte domeinverwijzing
Deze verwijzing bevat informatie over de [vooraf gedefinieerde domeinen](luis-how-to-use-prebuilt-domains.md), die zijn vooraf gedefinieerde verzamelingen van intents en entiteiten die LUIS biedt.

[Aangepaste domeinen](create-new-app.md), daarentegen starten zonder intents en modellen. U kunt alle vooraf gedefinieerde domein intents en entiteiten toevoegen aan een aangepaste model.

## <a name="list-of-prebuilt-domains"></a>Lijst met vooraf gedefinieerde domeinen
LUIS biedt 20 vooraf gedefinieerde domeinen. 

| Vooraf gedefinieerde domein | Beschrijving | Ondersteunde talen |
| ---------------- |-----------------------|:------:|
| Agenda | Het domein van de kalender biedt bedoeling en entiteiten voor toe te voegen, te verwijderen, of een afspraak te bewerken, deelnemers beschikbaarheid controleren en om informatie over gebeurtenissen te zoeken.| nl-NL<br/> zh-CN |
| Camera | Het domein van de Camera biedt intents en entiteiten voor het maken van foto's, video's opnemen en broadcasting video voor een toepassing.| nl-NL |
| Communicatie | Verzenden van berichten en telefoongesprekken.| nl-NL <br/> zh-CN |
| Entertainment  | Query's die zijn gerelateerd aan muziek, films en tv-verwerking.| nl-NL |
| Gebeurtenissen | Reservering van tickets voor concerten, festivals, sport-games en comedy bevat.| nl-NL |
| Geschiktheid | Afhandeling van aanvragen in verband met het bijhouden van geschiktheid activiteiten.| nl-NL |
| Gaming | Afhandeling van aanvragen in verband met een game partij in een spel.| nl-NL |
| HomeAutomation | Smart thuis-apparaten zoals lichten en apparaten beheert.| nl-NL<br/> zh-CN |
| MovieTickets | Reservering kaartjes voor films op een locatie film.| nl-NL |
| Music | Het afspelen van muziek op een muziekspeler.| nl-NL<br/> zh-CN |
| Opmerking | Het domein Opmerking biedt intents en entiteiten die zijn gerelateerd aan het maken, bewerken en notities zoeken.| nl-NL<br/> zh-CN |
| OnDevice | Het domein OnDevice biedt intents en entiteiten die betrekking hebben op het apparaat te beheren.| nl-NL<br/> zh-CN |
| Plaatsen  | Verwerking van query's die betrekking hebben op locaties zoals bedrijven, instellingen, restaurant, ruimten voor algemeen gebruik en -adressen.| nl-NL<br/> zh-CN |
| Herinnering | Afhandeling van aanvragen in verband met het maken, bewerken en zoeken herinneringen.| nl-NL<br/> zh-CN |
| RestaurantReservation | Afhandeling van aanvragen voor het beheren van restaurant reserveringen.| nl-NL<br/> zh-CN |
| Taxi | Reserveringen voor een taxi verwerking.| nl-NL<br/> zh-CN |
| Vertalen | Tekst in een taal die doel vertalen.| nl-NL<br/> zh-CN |
| TV | Beheert televisies.| nl-NL |
| Nutsbedrijven  | Afhandeling van aanvragen die worden gebruikt in veel domeinen, zoals 'help', 'Herhaal', "beginnen."| nl-NL |
| Weer | De rapporten weer en prognoses ophalen.| nl-NL<br/> zh-CN |
| Web | Navigeren naar een website.| nl-NL<br/> zh-CN |

Zie de volgende secties voor meer informatie over elk domein.

## <a name="calendar"></a>Agenda 

Het domein van de kalender biedt intents en entiteiten die zijn gerelateerd aan een agenda-items. De kalender intents bevatten toevoegen, verwijderen of bewerken van een afspraak beschikbaarheid controleren en om informatie over een agenda-item of een afspraak te zoeken.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Toevoegen | Een nieuw eenmalige item toevoegen aan de kalender.| Een afspraak met ties om 2 uur op zondag maken <br/><br/>Ik wil een vergadering plannen<br/><br/>Ik wil een vergadering instellen|
| CheckAvailability | Beschikbaarheid voor een afspraak of vergadering kalender van de gebruiker of een andere persoon kalender niet vinden.| Wanneer is beschikbaar om te voldoen aan Jim? <br/><br/>Weergeven wanneer de Carola morgen beschikbaar is<br/><br/>Chris vrij is op zaterdag?|
| Verwijderen | Verzoek voor het verwijderen van een agenda-item.| Mijn afspraak met Carola annuleren. <br/><br/>Verwijderen van mijn vergadering 9: 00 uur<br/>|
| Bewerken | Verzoek voor het wijzigen van een bestaande vergadering of een agenda-item.| Verplaats mijn vergadering 9: 00 uur tot 10 uur.<br/><br/>Ik wil mijn schema bijwerken.<br/><br/>Reschdule Mijn vergadering met Ryan.|
| Find | Mijn wekelijkse agenda weergeven.| Zoek de tandarts afspraak bekijken. <br/><br/>Mijn agenda weergeven<br/>|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Locatie | Locatie van agenda-item, vergadering of afspraak. Adressen, plaatsen en regio's zijn goede voorbeelden van locaties.| 209 Nashville maakt <br/><br/>897 pancake house<br/><br/>Garage|
| Onderwerp | De titel van een vergadering of afspraak.| De tandarts <br/><br/>Lunch met Julia<br/><br/>Arts afspraak|

## <a name="camera"></a>Camera 
Het domein van de Camera biedt intents en over het gebruik van een camera-entiteiten. De intents betrekking op het vastleggen van een foto, selfie, schermafbeelding of video en video's naar een toepassing wordt uitgezonden.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| CapturePhoto| Een foto vastleggen.| Een foto neemt<br/><br/>vastleggen|
| CaptureScreenshot | Een schermopname vastleggen.| Schermopname in beslag nemen.<br/><br/>schermopname.|
| CaptureSelfie | Een selfie vastleggen.| Een selfie nemen <br/><br/>Maak een foto van mij |
| CaptureVideo | Opname video te starten.| Opname starten <br/><br/>Beginnen met opnemen|
| StartBroadcasting| Broadcasting video te starten.| Uitzenden naar Facebook starten|
| StopBroadcasting| Broadcasting video stoppen.| Broadcasting stoppen|
| StopVideoRecording| Stop de opname van een video.| Dat is voldoende<br/><br/>opname stoppen|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | De naam van een toepassing voor het uitzenden van video.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>Communicatie 
Het domein communicatie biedt intents en entiteiten die betrekking hebben op e-mail, berichten en telefoongesprekken.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AddContact| Een nieuwe contactpersoon toevoegen aan lijst met contactpersonen van de gebruiker.|Nieuwe contactpersoon toevoegen <br/><br/>Sla dit nummer en de naam op als Carola plaatsen|
| AddMore| Meer op een e-mailadres of de tekst, als onderdeel van een stapsgewijs samenstelling van e-mailadres of tekst toevoegen.|Meer naar tekst toevoegen <br/><br/>Voeg meer voor tekst van e-mail|
| Antwoord| Een inkomend telefoongesprek te beantwoorden.|de oproep wordt beantwoord <br/><br/>Deze kunnen worden opgepikt|
| AssignContactNickname| Een bijnaam een contactpersoon.|Pa Isaac wijzigen <br/>De Jim bijnaam bewerken<br/>Bijnaam aan Patti Owens toevoegen|
| CallVoiceMail| Verbinding maken met de voicemail van de gebruiker.|Verbinding maken met mijn voicemail-vak <br/>voicemail<br/>voicemail oproepen|
| CheckIMStatus| Controleer de status van een contactpersoon in Skype.|Is de onlinestatus van Jim ingesteld op Opslaan? <br/>Carola chatten met beschikbaar is?|
| Bevestigen| Bevestig de actie.|Ja<br/>OK<br/>OK<br/>Bevestig ik dat ik wil deze e-mail verzenden.<br/>|
| inbelverbinding| Een telefoongesprek.|Aanroep Jim<br/>Neem 311 kiezen<br/>|
| FindContact| Contactgegevens zoeken op naam.|Carola getal zoeken<br/>Carola nummer weergeven<br/>|
| FindSpeedDial| Zoek de speedial nummer een telefoonnummer in dat is ingesteld op en vice versa.|Wat is mijn telefoonnummer 5?<br/>Heb ik snelheid set kiezen?<br/>Wat is het telefoonnummer van 941-5555-333?|
| GetForwardingsStatus| De huidige status van de aanroep doorsturen ophalen.|Is het doorsturen van mijn aanroep ingeschakeld?<br/>Meer informatie als de status van mijn aanroep in- of uitschakelen wordt<br/>|
| GoBack| Ga terug naar de vorige stap.|Ga terug naar twitter<br/>Een stap terug<br/>Terug|
| Negeren| Een inkomend gesprek negeren.|Geen antwoord<br/>Aanroep negeren|
| IgnoreWithMessage| Een inkomend gesprek negeren en in plaats daarvan reageren met tekst.|Geen antwoord die aanroep, maar in plaats daarvan een bericht te verzenden.<br/>Negeren en terug in een tekst verzenden.|
| PressKey| Druk op een knop of het nummer op het toetsenbord.|Externe ster.<br/>Druk op 1-2-3.|
| ReadAloud| Een bericht of e-mail lezen voor de gebruiker.|Lees de tekst.<br/>Wat ze zeggen in het bericht?|
| TurnForwardingOff| Een telefoongesprek.|<br/><br/>|
| opnieuw kiezen| Kiezen of een nummer bellen opnieuw.|Kies opnieuw.<br/>Kies de laatste aanroep opnieuw.|
| Afwijzen| Een binnenkomende oproep afwijzen.|Gesprek weigeren<br/>Kan nu niet beantwoorden<br/>Niet beschikbaar op het moment dat en zullen terugbellen later.|
| EmailVerzenden| Een e-mail sturen. Dit doel is van toepassing op e-mailadres, maar geen SMS-berichten.|E-mail Mike Waters: Mike, dat diner afgelopen week schitterende is.<br/>Een e-mailbericht verzenden naar Bob<br/>|
| SendMessage| Een SMS-bericht of een chatbericht verzenden.|Tekst verzenden naar Chris en Carola|
| SetSpeedDial| Een snelkoppeling snelheid-op-verzoek voor een Contacttelefoonnummer instellen.|Snelkiezen een voor Carola instellen.<br/>Snelkiezen voor mom instellen.|
| ShowNext| Zie het volgende item, bijvoorbeeld in een lijst met SMS-berichten of e-mailberichten.|De volgende gateway weergeven.<br/>Ga naar de volgende pagina.|
| ShowPrevious| Zie het vorige item, bijvoorbeeld in een lijst met SMS-berichten of e-mailberichten.|De vorige taak weergeven.<br/>Vorige<br/>Ga naar vorige.|
| Opnieuw| Het systeem opnieuw starten of een nieuwe sessie starten.|Opnieuw beginnen<br/>Nieuwe sessie<br/>opnieuw opstarten|
| TurnForwardingOff| Aanroep doorsturen uitschakelen.|stopt met het doorsturen van mijn aanroepen<br/>aanroep doorsturen uitschakelen|
| TurnForwardingOn| De telefoon spreker uitschakelen.|Mijn aanroepen naar 3333 doorsturen<br/>Doorsturen van de aanroep naar 3333 inschakelen|
| TurnSpeakerOff| De telefoon spreker uitschakelen.|Breng me uitschakelen spreker.<br/>Handsfree uitschakelen.<br/>|
| TurnSpeakerOn| Schakel op de spreker telefoon.|Handsfree-modus.<br/>Plaats Handsfree op.<br/>|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AudioDeviceType | Type audioapparaat (spreker, hoofdtelefoon, microfoon, enzovoort).| Spreker<br/>Handsfree<br/>Bluetooth|
| Category | De categorie van een bericht of e-mail.| Belangrijk<br/>Hoge prioriteit|
| ContactAttribute | Een kenmerk van het contact op met de gebruiker query's over.| Verjaardagen<br/>Adres<br/>Telefoonnummer|
| Contactpersoon | De naam van een ontvanger of neem contact op met het bericht.| Carola<br/>Jim<br/>Chris|
| EmailSubject | De tekst die wordt gebruikt als de onderwerpregel voor een e-mailbericht.| RE: interessante verhaal|
| Regel | De regel van de gebruiker wil gebruiken om te bellen of een e-mail tekst/uit.| Werk regel<br/>Brits cel<br/>Skype|
| Bericht | Het bericht te verzenden als een e-mailadres of de tekst.| Goede vandaag ziens was. Gauw!|
| MessageType | De naam van een ontvanger of neem contact op met het bericht.| Tekst<br/>Email|
| OrderReference | De rangtelwoord of relatieve positie in een lijst, het identificeren van een item ophalen. Bijvoorbeeld 'laatste' of 'recente' in 'Wat is het laatste bericht dat ik verzonden?'| Laatste<br/>Recent|
| SenderName | De naam van de afzender.| Patti Owens|

## <a name="entertainment"></a>Entertainment  
Het domein biedt intents en entiteiten die zijn gerelateerd aan het zoeken naar films, muziek, games en TV Entertainment bevat.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Search| Zoeken naar films, muziek, apps, games en TV bevat.|Zoek in de store voor Halo.<br/>Zoeken naar Avatar.|

### <a name="entities"></a>Entiteiten
| De naam van de entiteiten | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| ContentRating | Media inhoudsrestricties zoals G of R voor films.|Video van kinderen.<br/>PG geclassificeerd.|
| Genre | Het genre van een film, de game, de app of het nummer.|Comedies<br/>Dramas<br/>Grappige|
| Sleutelwoord| Een algemene trefwoord opgeven van een kenmerk de bestaat niet in de specifiekere media sleuven.|Soundtracks<br/>Verplichte maan<br/>Amelia Earhart|
| Taal | Media inhoudsrestricties zoals G of R voor films.|Frans<br/>Nederlands<br/>Koreaans|
| MediaFormat | De aanvullende speciale technische type waarin het medium is geformatteerd.|HD films<br/>3D-films<br/>Downloadbare|
| MediaSource | De store of marketplace voor het ophalen van de media.|NetFlix<br/>Prime|
| MediaSubTypes| Kleiner dan films en games mediatypen.|Demo 's<br/>De DLC<br/>Aanhangwagens|
| Nationaliteit| Het land waar een film, weergeven of nummer is gemaakt.|Frans<br/>Duits<br/>Koreaans|
| Person| De acteur, directeur, producent, muzikant of artiest die zijn gekoppeld aan een film, app, game of tv-programma.|Madonna<br/>Stanley Kubrick|
| Rol| Rol van een persoon bij het maken van media.|Sings<br/>Omgeleid door<br/>Door|
| Titel| De naam van een film app, game, tv-programma of nummer.|Vrienden<br/>Minecraft|
| Type| Het type of media-indeling van een film app, game, tv-programma of nummer.|Music<br/>MovieTV <br/>toont|
| UserRating| Gebruiker ster of duim classificatie.|5 sterren<br/>3 sterren<br/>4 sterren|

## <a name="events"></a>Gebeurtenissen 
Het domein van gebeurtenissen biedt intents en entiteiten die zijn gerelateerd aan reservering tickets voor gebeurtenissen, zoals concerten, festivals, sport-games en comedy bevat.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Rapport| Koop tickets op een gebeurtenis.|Ik wil een ticket voor de symphony weekend kopen.|


### <a name="entities"></a>Entiteiten
| De naam van de entiteiten | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres | Locatie van de gebeurtenis of het adres. |Palo Alto<br/>300 112th Ave SE <br/> Seattle |
| Naam | De naam van een gebeurtenis.|Shakespeare in de Park|
| PlaceName| De naam van de gebeurtenis-locatie.|Louvre<br/>Opera huis<br/>Gewijzigd in Broadway|
| PlaceType | Het type van de locatie van de gebeurtenis worden ondergebracht in.|Cafe<br/>OK<br/>Bibliotheek|
| Type | Het type van een gebeurtenis.|Energieverbruikoptimalisatie tegelijk<br/>Sport game|

## <a name="fitness"></a>Geschiktheid 
Het domein geschiktheid biedt intents en entiteiten voor het bijhouden van geschiktheid activiteiten. De intents bevatten opmerkingen, de resterende tijd of op afstand of het opslaan van Activiteitsresultaten opslaan.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AddNote| Voegt aanvullende opmerkingen toe aan een activiteit die wordt bijgehouden.|De moeite van deze sessie is 6/10<br/>Het terrein dat ik ben op die worden uitgevoerd op asfalt is<br/>Ik gebruik een fiets 3 snelheid|
|GetRemaining| Hiermee haalt de resterende tijd of afstand voor een activiteit.|Na hoeveel tijd tot de volgende lap?<br/>Hoeveel mijl blijven in mijn uitvoeren? Na hoeveel tijd voor de splitsing?|
| LogActivity| Opslaan of meld u resultaten van de voltooide activiteit.|Opslaan van mijn laatste uitvoering<br/>Meld u Mijn zaterdag ochtend walk<br/>Mijn vorige zwem opslaan|
| LogWeight| Opslaan of meld u huidige gewicht van de gebruiker.|Mijn huidige gewicht opslaan<br/>Mijn gewicht nu niet aanmelden<br/>Mijn huidige instantie gewicht opslaan|

### <a name="entities"></a>Entiteiten
| De naam van de entiteiten | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| ActivityType | Het type activiteit om bij te houden. |Voer<br/>Walk<br/>Swim<br/>Cyclus |
| Voedsel | Een type voeding om bij te houden in een app geschiktheid. |Banana<br/>Zalm<br/>Schud eiwit|
| MealType| Het type maaltijd om bij te houden in een status- of geschiktheid app.|Ontbijt<br/>Diner<br/>Lunch<br/>Supper|
| Meting| Een type afmetingen voor de tijd, afstand of het gewicht voor gebruik in een app geschiktheid of status.|Kilometers<br/>Miles<br/>Minuten<br/>Kilogram|
| Aantal | Een numerieke hoeveelheid voor gebruik in een app geschiktheid of status.|19<br/>drie<br/>200<br/>één|
| StatType | Een type statistiek op geaggregeerde gegevens, voor gebruik in een app geschiktheid of status.|Sum<br/>Gemiddeld<br/>Maximum<br/>Minimum|

## <a name="gaming"></a>Gaming 
Het domein games biedt intents en entiteiten die zijn gerelateerd aan een game partij in een spel te beheren.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| InviteParty| Contactpersoon voor een gaming-party uitnodigen.|Deze player aan mijn partij uit te nodigen<br/>Ga naar Mijn partij<br/>Deelnemen aan mijn clan|
|LeaveParty| Hiermee haalt de resterende tijd of afstand voor een activiteit.|Ik ben uit<br/>Deze belanghebbende voor een andere laat<br/>Ik ben afsluiten|
| StartParty| Start een gaming-party in een spel.|Laten we man starten van een partij<br/>starten van een partij<br/>Er moet een clan vanavond beginnen|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Contact| De naam van een contactpersoon in een spel gebruiken.|Carola<br/>Jim|


## <a name="homeautomation"></a>HomeAutomation 
Het domein HomeAutomation biedt intents en entiteiten die betrekking hebben op de smartcard thuis apparaten zoals lichten en toestellen besturen.

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Uitschakelen| Uitschakelen, sluiten of een apparaat te ontgrendelen.|De lichten uitschakelen<br/>De maker koffie stoppen<br/>Sluiten Garagedeur|
|Inschakelen| Schakel op een apparaat of het apparaat is ingesteld op een bepaalde instelling of modus.|de maker van mijn koffie inschakelen<br/>kunt u de maker van mijn koffie inschakelen?<br/>Stel de thermostaat op 72 graden.|


### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Apparaat | Een type apparaat dat kan worden ingeschakeld in- of uitschakelen.|koffie maker<br/>Thermostaat<br/>lichten|
| Bewerking | De status van het apparaat.|vergrendelen<br/>openen<br/>op<br/>uit|
| Ruimte | De locatie of het apparaat zich in de ruimte.|Woonkamer<br/>kamer uw<br/>keuken|

## <a name="movietickets"></a>MovieTickets 
Het domein MovieTickets biedt intents en reservering kaartjes voor films op een locatie film-entiteiten.

### <a name="examples"></a>Voorbeelden
```
Book me two tickets for Captain Omar and the two Musketeers
Cancel tickets
When is Captain Omar showing?
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Rapport | Film tickets aanschaffen.|Het adresboek mij twee tickets voor kapitein Omar en de twee musketeers<br/>Ik wil een ticket voor morgen film kopen<br/>Ik wil dat een serviceticket voor Captian Omar deel 2 volgende woensdag|
|GetShowTime| De showtime van een film ophalen.|Wanneer wordt kapitein Omar weergegeven?|


### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres | Het adres van de locatie van een film.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| MovieTitle | De titel van een film.|Levensduur van Pi<br/>Hunger Games<br/>Begin|
| PlaceName | De naam van een film locatie of bioscoop.|Bioscoop Amir<br/>Alexandrië OK<br/>Locatie van de New York|
| PlaceType | Het type van een film wordt weergegeven op locatie.|bioscoop<br/>locatie<br/>IMAX bioscoop|

## <a name="music"></a>Music 
Het domein muziek biedt intents en entiteiten die zijn gerelateerd aan het afspelen van muziek op een muziekspeler.

### <a name="examples"></a>Voorbeelden
```
play Beethoven
Increase track volume
Skip to the next song
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| DecreaseVolume | De apparaat-volume te verlagen.|track volume te verlagen<br/>volume omlaag|
| IncreaseVolume | Vergroot het volume van het apparaat.|track volume verhogen<br/>volume omhoog|
| Dempen |Dempen afgespeeld muziek.|Dempen nummer<br/>Het nummer op dempen plaatsen<br/>Dempen muziek |
| Onderbreken | De afgespeeld muziek onderbreken.|Onderbreken<br/>Music onderbreken<br/>Onderbreken bijhouden|
| PlayMusic | Muziek afspelen op een apparaat.|Kevin Durant afspelen<br/>Paradise door Coldplay afspelen<br/>Hallo door Adele afspelen|
| Herhalen |Herhaal de afgespeeld muziek.|Herhaal nummer<br/>Het voordeel bijhouden afspelen<br/>Herhaal muziek|
| Hervatten | Hervat de afgespeeld muziek.|Nummer hervatten<br/>music opnieuw starten<br/>Hervatten|
| SkipBack | Overslaan achterwaartse bijgehouden.|Ga naar het volgende nummer<br/>Het volgende nummer afspelen|
| SkipForward |Overslaan doorsturen bijgehouden.|Het vorige nummer afspelen<br/>Ga terug naar het vorige nummer |
| Stoppen | Een actie met betrekking tot muziek afspelen stoppen. |Dit album afspelen stoppen.|
| Uitschakelen | Een muziek afspelen apparaat uitschakelen.| Uitschakelen.|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| ArtistName | De acteur, directeur, producent, schrijver, muzikant of artiest media af te spelen op een apparaat gekoppeld.|ELVIS Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Genre | Het genre van de aangevraagde muziek.|Land muziek<br/>Gewijzigd in Broadway kerstmuziek<br/>Mijn klassieke muziek van de Barok periode|

## <a name="note"></a>Opmerking 
Het domein Opmerking biedt intents en entiteiten die zijn gerelateerd aan het maken, bewerken en notities zoeken.

### <a name="examples"></a>Voorbeelden
```
Add to my groceries note lettuce tomato bread coffee
Check off bananas from my grocery list
Remove all items from my vacation list
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AddToNote | Gegevens aan een opmerking toevoegen.|Toevoegen aan mijn boodschappen Opmerking Sla tomaten brood koffie<br/>Toevoegen aan mijn takenlijst<br/>Cakejes gebruiken toevoegen aan mijn Wunderlist|
| CheckOffItem | Controleer de items van een bestaande notitie af.|Schakel bananen uit mijn boodschappenlijst<br/>Kaas taart op Mijn vakantie winkelen lijst als voltooid markeren|
| Wissen | Alle items uit een bestaande opmerking verwijderen.|Alle items verwijderen uit de lijst van Mijn vakantie<br/>Alles wissen uit de lijst van mijn lezen|
| Bevestigen | Een actie met betrekking tot een notitie bevestigen.|Het is geen probleem door mij<br/>ja<br/>Ik ben bevestigen zorgt ervoor dat alle items op lijsten|
| Maken | Een nieuwe notitie maken. | Een lijst maken<br/>Om te herinneren dat Jason is in de stad eerste week van mei|
| Verwijderen | Een volledige Opmerking verwijderen. |Mijn vakantie lijst verwijderen <br/>Mijn boodschappen Opmerking verwijderen|
| DeleteNoteItem | Items verwijderen uit een bestaande opmerking.| Chips uit mijn boodschappenlijst verwijderen<br/>Pennen uit mijn school winkelen lijst verwijderen|
| ReadAloud | Een lijst hardop lezen.|Lees dit het eerste beheerpunt<br/>De details lezen|
| ShowNext | Zie het volgende item in een lijst van notities.|De volgende gateway weergeven<br/>Volgende pagina<br/>Volgende|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | De toepassingsnaam notities.|Wunderlist<br/>OneNote|
| Contactpersoon | De naam van een contactpersoon in een opmerking.|Carola<br/>Jim<br/>Chris|
| Gegevensbron | Locatie van opmerkingen.|OneDrive<br/>Google-docs<br/>Mijn computer|
| Gegevenstype | Het type van het bestand of het document, meestal samenhangen met bepaalde softwareprogramma's.|Dia<br/>Spreadsheet<br/>Werkblad|
| Tekst | De tekst van een notitie of herinnering.|stretch voordat roulatie van<br/>morgen lange termijn|
| Titel | De titel van een opmerking.|boodschappen<br/>mensen aan te roepen<br/>taak|

## <a name="ondevice"></a>OnDevice 
Het domein OnDevice biedt intents en entiteiten die betrekking hebben op het apparaat te beheren.

### <a name="examples"></a>Voorbeelden
```
Close video player
Cancel playback
Can you make the screen brighter?
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AreYouListening | Vraag als het apparaat luistert.|is dit op?<br/>wordt geluisterd?|
|CloseApplication|Sluit de apparaattoepassing.|Sluit-speler|
|FileBug|Het bestand een bug op het apparaat.|Geef een bug bestand<br/>Kunt u een bug indienen voor mij?<br/>Ik wil deze fout rapporteren|
|GoBack|Vraag om één stap teruggaan of Ga terug naar de vorige stap.|Ga terug<br/>Ga naar het vorige scherm<br/>Ga terug stoppen luisteren|
|Help| Hulp vragen.|Neem Help<br/>Hallo!<br/>Wat kunt u doen?<br/>Ik heb nodig hulp| 
|LocateDevice|Zoek het apparaat.|U kunt vinden mijn telefoon<br/>Vinden van de aangepaste iphone<br/>Mijn telefoon zoeken|
|Aanmelding|Aanmelden bij een service met het apparaat.|Aanmelding Neem<br/>Facebook aanmelden<br/>Meld u aan bij LinkedIn|
|Afmelden|Afmelding van een service met het apparaat.|Mijn telefoon afmelden<br/>Meld u aan bij twitter<br/>Afmelden|
|MainMenu|Het hoofdmenu van een apparaat weergeven.|Menu Beeld.|
|OpenApplication|Hiermee opent u een toepassing op het apparaat.|Geef het alarm openen<br/>Camera inschakelen<br/>Starten van de kalender|
|OpenSetting|Open een instelling op het apparaat.|Open de netwerkinstellingen.|
|PairDevice|Koppel het apparaat.|U kunt u mij Bluetooth-signaal moet telefoon koppelen<br/>De bluetooth inschakelen en koppel het aan een laptop<br/>Bluetooth-signaal op mijn laptop koppelen|
|Uitgeschakeld | Het apparaat uitschakelen.|U kunt de computer afsluiten<br/>Afsluiten<br/>Uitschakelen van mijn mobiele telefoon|
|QueryBattery|Informatie ophalen over de batterij.|Batterij weergeven.<br/>Wat is de accustatus van mijn<br/>Hoeveel accu links nu?<br/>Accu weergeven|
|QueryWifi|Informatie ophalen over Wi-Fi.|Wi-Fi-gegevens opvragen.|
|Opnieuw starten|Het apparaat opnieuw te starten.|Start opnieuw.|
|RingDevice| Vraag het apparaat ring, om te kunnen terugvinden als deze is verbroken.|Mijn telefoon ring.| 
|SetBrightness|De helderheid apparaat instellen.|De helderheid instellen op Gemiddeld<br/>Helderheid op Hoog instellen<br/>Set helderheid op laag|
|SetupDevice|Start de configuratie van het apparaat.|Ik wil installatie van het Besturingssyteem installeren<br/>Neem instellen<br/>Heb ik setup|
|ShowAppBar|Een app-balk weergeven.|De toepassingsbalk weergeven<br/>Toepassing balk Neem<br/>Ik kan de toepassing weergegeven|
|ShowContextMenu|Een contextmenu weergeven.|Ik wil het contextmenu Zie<br/>Contextmenu Neem<br/>Ik kan zien het contextmenu|
|Slaapstand|Het apparaat naar de slaapstand plaatsen.|Ga slapen<br/>Slaapstand<br/>Mijn computer slaapstand|
|SwitchApplication|Schakel over naar de toepassing te gebruiken op het apparaat.|Overschakelen naar Mijn mediaspeler.|
|TurnDownBrightness|Schakel omlaag apparaat helderheid.|Het scherm dimmen.|
|TurnOffSetting|De Apparaatinstelling van een uitschakelen.|Bluetooth deactiveren<br/>Uitschakelen van gegevens<br/>Bluetooth verbreken|
|TurnOnSetting|Schakel de Apparaatinstelling van een.|Aan <br/> Uit|
|TurnUpBrightness|Schakel de helderheid apparaat.|Kunt u het scherm lichter?|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AppName | Naam van een toepassing op het apparaat.|SoundCloud<br/>YouTube|
| BrightnessLevel | Het helderheidsniveau van de ingesteld op het apparaat.|100 procent<br/>Vijftig<br/>40%|
| Contactpersoon | De naam van een contactpersoon op het apparaat.|Paul<br/>Marlen Max|
| DeviceType | Het type apparaat. |Telefoon<br/>Kindle<br/>Laptop|
| MediaType | Het mediatype is verwerkt door het apparaat.|Music<br/>Film<br/>Tv-programma 's|
| Settingtype hebben | Een type instelling of het paneel met toepassingsinstellingen die de gebruiker wil bewerken.|Wi-Fi<br/>Draadloos netwerk<br/>Kleurenschema<br/>Meldingencentrum|

## <a name="places"></a>Plaatsen  
Het domein plaatsen biedt intents voor het verwerken van query's die betrekking hebben op locaties zoals bedrijven, instelling, restaurant, ruimten voor algemeen gebruik en -adressen.

### <a name="examples"></a>Voorbeelden
```
Save this location to my favorites
How far away is Holiday Inn?
At what time does Safeway close?
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Toevoegen van een locatie aan de lijst met Favorieten van de gebruiker.|deze locatie in Mijn favorieten opslaan<br/>Dit adres toevoegen aan Favorieten|
|CheckAccident|Vraag of er een ongeluk beschikbaar is op een opgegeven weg.|Is er een ongeluk op 880?<br/>Ongeluk informatie weergeven|
|CheckAreaTraffic|Controleer het verkeer voor een algemene gedeelte of snelweg niet op een opgegeven route.|Verkeer in Haarlem<br/>Wat is het verkeer zoals in Haarlem?|
|CheckIntoPlace|Controleer een plaats met behulp van sociale media.|Me inchecken op Foursquare<br/>Schakel dit selectievakje|
|CheckRouteTraffic| Controleer het verkeer van een specifieke route die is opgegeven door de gebruiker.|Hoe wordt het verkeer naar Mashiko?<br/>De traffice naar Kirkland weergeven<br/>Hoe wordt het verkeer naar Seattle?| 
|Bevestigen|Controleer een actie met betrekking tot een plaats.|Bevestig de reservering van mijn restaurant.|
|Afsluiten|Actie om af te sluiten van een taak met betrekking tot een plaats.|Neem afsluiten<br/>Sluit geeft mij richtingen|
|FindPlace|Zoeken naar een locatie (business, instelling, restaurant, openbare ruimte, adres).|Waar bevindt zich de dichtstbijzijnde bibliotheek?<br/>Mij een goede Italiaanse restaurant vinden in Mountain weergeven|
|GetAddress| Vragen voor het adres van een plaats.|Het adres van Guu in Robson straat weergeven<br/>Wat is het adres van de dichtstbijzijnde Starbucks?| 
|GetDistance|Vraag afstand tot een bepaalde locatie.|Hoe ver Inn vakantie is?<br/>hoe ver is het aan Bellevue vierkante hiervandaan<br/>Wat is de afstand tot Tahoe|
|GetHours|Vragen over de operationele uur voor een plaats.|Welk tijdstip Safeway sluiten?<br/>Wat zijn de uren voor Start Depot?<br/>Starbucks nog is geopend?|
|GetMenu|Vragen om de menu-items voor een restaurant.|Zucca biedt fungeren iets Veganistische?<br/>Wat is er in het menu Sizzler<br/>Applebee van menu weergeven|
|GetPhoneNumber| Vragen om het telefoonnummer van een plaats.|Wat is het telefoonnummer van de dichtstbijzijnde Starbucks?<br/>Geeft het aantal voor Start Depot| 
|GetPriceRange| Vraagt om het prijsbereik van een plaats.|Zucca goedkope is?<br/>Is de Cineplex half prijs op woensdagen?<br/>Wat kost een hele kreeft diner op Sizzler?|
|GetReviews|Vraag voor revisies van een plaats.|Beoordelingen voor Cheesecase Factory weergeven<br/>Beoordelingen Cineplex in Yelp lezen|
|GetRoute|Vragen naar een locatie.|Hoe helpt naar Bellevue vierkante<br/>De kortste manier om 8ste en 59th hier weergeven<br/>Hoe kom ik routebeschrijving naar uitzicht CA|
|GetStarRating|Vragen voor de beoordeling van een plaats.|Hoe wordt de Zucca volgens Yelp geclassificeerd?<br/>Hoeveel sterren heeft de wasgoed Frans?<br/>Het aquarium in Monterrey goed is?|
|GetTransportationSchedule|De planning bus ophalen voor een plaats.|Welk tijdstip is de volgende bus naar centrum?<br/>De bussen in koning regio weergeven|
|GetTravelTime|Vragen om de reistijd naar een opgegeven bestemming.|Hoe lang het duurt om te krijgen tot San Francisco hiervandaan<br/>Wat is de aangedreven tijd om Denver van SF|
|MakeCall|Een telefoongesprek naar een locatie op te nemen.|Roep mom<br/>Ik wil een Skype gesprek Anna<br/>Aanroep Jim|
|MakeReservation|Aanvraag een reservering voor een restaurant of andere bedrijven.|Reserveer op Zucca voor twee tonight<br/>Een tabel voor morgen adresboek<br/>Tabel 3 in Palo Alto op 8|
|MapQuestions|Informatie over de richtingen of of een opgegeven weg overschakelt naar een bestemming aanvragen.|Geeft 13 in het Centrum?<br/>Kan ik 880 tot Oakland duren?|
|Waardering|De beschrijving van de beoordeling van een restaurant of plaats ophalen.|Hoeveel sterren heeft de Contoso-Inn?|
|ReadAloud|Een lijst van plaatsen hardop lezen.|Lees dit het eerste beheerpunt<br/>De details lezen|
|SelectItem|Kies een item uit een lijst met opties met betrekking tot een plaats of plaatsen.|Kies in het tweede voorbeeld<br/>Selecteer de eerste|
|ShowMap|Een toewijzing van een gebied weergeven.|Een toewijzing voor de tweede weergeven<br/>Kaart weergeven<br/>San Francisco vinden op de kaart|
|ShowNext|Het volgende item in een serie weergeven.|De volgende gateway weergeven<br/>Ga naar de volgende pagina|
|ShowPrevious|Het vorige item in een serie weergeven.|Vorige weergeven<br/>vorige<br/>Ga naar vorige|
|Opnieuw|Start de app opnieuw of start een nieuwe sessie.|Opnieuw beginnen<br/>Nieuwe sessie<br/>
opnieuw opstarten|
|TakesReservations|Vraag of een plaats reserveringen accepteert.|accepteert de galerie illustraties reserveringen<br/>Is het mogelijk om een reservering op de Olive Garden

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| AbsoluteLocation | De locatie of het adres van een plaats.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Faciliteiten | De servicedoelstelling kenmerken/voordelen van een plaats.|kinderen eat gratis<br/>kust<br/>Gratis parkeren|
| Lucht | De omgeving van een plaats.|KID-vriendelijk<br/>verborgen restaurant<br/>sportief|
| Cuisine | De cuisine een plaats. |Middellandse<br/>Italiaans<br/>Indische Oceaan|
| DestinationAddress| Een nieuwe locatie of het adres.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName| De naam van een doel is van een bedrijf, een restaurant, openbare voordelen of instelling.|centrale park<br/>safeway<br/>walmart|
| DestinationPlaceType | Het type van een doel dat is een lokale business, restaurant, openbare voordelen of instelling. |Restaurant<br/>Opera<br/>bioscoop|
| Afstand | De afstand tot een plaats.|15 mijl<br/>5 mijl<br/>10 mijl verwijderd|
| MealType | Type maaltijd zoals ontbijt of lunch. |Ontbijt<br/>Diner<br/>Lunch<br/>Supper|
| OpenStatus | Hiermee wordt aangegeven of een plaats open of closed is.|Open<br/>gesloten<br/>openen|
| PlaceName | De naam van een plaats.|Cheesecake Factory|
| PlaceType | Het type van een plaats.|Cafe<br/>OK<br/>Bibliotheek|
| PreferredRoute | De route die de voorkeur is opgegeven door de gebruiker. | 101 <br/>202 <br/>Route 401|
| Product | Het product die worden aangeboden door een plaats. | Kleding<br/>ASR digitale camera 's<br/>Nieuwe vis | 
| PublicTransportationRoute | De naam van de openbare vervoer route die naar de gebruiker zoekt. | De trein noordoosten bandbreedte<br/>Bus route 3 X |
| Waardering | De classificatie van een plaats. | 5 sterren<br/>3 sterren<br/>4 sterren|
| RouteAvoidanceCriteria | Criteria voor het vermijden van specifieke routes zoals ongevallen, constructies of tolgelden voorkomen | Tolgelden <br/>Constructies<br/>Route 11|
| ServiceProvided | Dit is de service van een bedrijf of een plaats zoals kapper, sneeuw plowing, waarde van onze. | kapper<br/>Mechanic<br/>loodgieter|
| TransportationCompany | De naam van een transportprovider.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Het type transport.|Bus<br/>Trainen<br/>Auto|

## <a name="reminder"></a>Herinnering 
Het domein herinnering biedt intents en entiteiten voor het maken, bewerken en zoeken herinneringen.

### <a name="examples"></a>Voorbeelden
```
Change my interview to 9 am tomorrow
Remind me to buy milk on my way back home
Can you check if I have a reminder about Christine's birthday?
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Wijzigen| Een herinnering wijzigen.|Mijn gesprek 9: 00 uur morgen wijzigen<br/>Mijn herinnering toewijzing aan morgen verplaatsen|
| Maken| Maak een nieuwe herinnering.|een herinnering maken<br/>Herinnering voor melk kopen<br/>Ik wil onthouden om aan te roepen Rebecca wanneer ik thuis ben|
| Verwijderen | Een herinnering verwijderen.|Mijn herinnering afbeelding verwijderen<br/>Deze herinnering verwijderen|
| Find | Een herinnering vinden.|Heb ik een herinnering over mijn verjaardag?<br/>Kunt u controleren als ik een herinnering over van Christine geboren?|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Tekst | De tekstbeschrijving van een herinnering.|Stomerij ophalen<br/>mijn auto verwijderen uitschakelen in het midden van de service|

## <a name="restaurantreservation"></a>RestaurantReservation 
Het domein RestaurantReservation biedt intents en entiteiten die zijn gerelateerd aan een restaurant reserveringen beheren.

### <a name="examples"></a>Voorbeelden
```
Reserve at Zucca for two for tonight
Book a table at BJ's for tomorrow
Table for 3 in Palo Alto at 7
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Reserve | Aanvraag een reservering voor een restaurant. |Reserveer op Zucca voor twee tonight<br/>Een tabel voor morgen adresboek<br/>Tabel 3 in Palo Alto op 7|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres| Een gebeurtenis locatie of het adres voor een reservering.|Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| Faciliteiten | Een kenmerk met een beschrijving van de voorzieningen van een plaats.|weergave Oceaan<br/>niet soorten|
| AppName | De naam van een toepassing voor het reserveren van plaatsen.|TabelOpenen<br/>Yelp<br/>TripAdvisor|
| Lucht | Een beschrijving van de omgeving van een restaurant of een andere locatie.|Romantisch<br/>verborgen<br/>goede voor groepen|
| Cuisine | Een soort voedingsmiddel, cuisine of cuisine nationaliteit. |Chinees<br/>Italiaans<br/>Mexico|
| MealType | Een maaltijd type die zijn gekoppeld aan een reservering.|Ontbijt<br/>Diner<br/>Lunch<br/>Supper|
| PlaceName | De naam van een lokale business, restaurant, openbare voordelen of instelling.|IHOP<br/>Cheesecake Factory<br/>Louvre|
| PlaceType | Het type van een lokale business, restaurant, openbare voordelen of instelling.|restaurant<br/>Opera<br/>bioscoop|
| Waardering | De classificatie van een plaats of een restaurant.|5 sterren<br/>3 sterren<br/>4 sterren|

## <a name="taxi"></a>Taxi 
 
Het domein Taxi biedt intents en entiteiten voor het maken en beheren van taxi reserveringen.

### <a name="examples"></a>Voorbeelden
```
Get me a cab at 3 pm
How much longer do I have to wait for my taxi?
Cancel my Uber
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Rapport | Een taxi aanroepen. |Hoe kom ik een CAB-bestand<br/>een taxi vinden<br/>Mij een uber boek x|
| Annuleren | Een actie met betrekking tot een taxi reservering annuleren.|Mijn taxi annuleren<br/>Mijn Uber annuleren|
| Bijhouden | Een route taxi traceren.|Hoe lang heb ik mijn taxi wachten?<br/>Waar kan ik mijn Uber?|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Adres| Het adres die zijn gekoppeld aan een taxi reservering. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationAddress| Een nieuwe locatie of het adres. |Palo Alto<br/>300 112th Ave SE<br/>Seattle|
| DestinationPlaceName | De naam van een doel dat is een lokale business, restaurant, openbare voordelen of instelling. |Centrale Park<br/>Safeway<br/>Walmart|
| DestinationPlaceType | Het type van een doel dat is een lokale business, restaurant, openbare voordelen of instelling. |Restaurant<br/>Opera<br/>bioscoop|
| PlaceName | Naam van de lokale business, restaurant, openbare voordelen of instelling. |Centrale Park<br/>Safeway<br/>Walmart|
| PlaceType| Het type plaats in een aanvraag bij een taxi het adresboek.|Restaurant<br/>Opera<br/>bioscoop|
| TransportationCompany | De naam van een transportprovider.|Amtrak<br/>Acela<br/>Greyhound|
| TransportationType | Het type transport.|Bus<br/>Trainen<br/>Auto|

## <a name="translate"></a>Vertalen 
Het domein vertalen biedt intents en entiteiten die zijn gerelateerd aan het vertalen van de tekst naar een target-taal.

### <a name="examples"></a>Voorbeelden
```
Translate to French
Translate hello to German
Translate this sentence to English
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Vertalen| Tekst in een andere taal te vertalen.|Kan vertalen naar Frans<br/>Hallo Duits vertalen|


### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| TargetLanguage | De taal van het doel van een vertaling.|Frans<br/>Duits<br/>Koreaans|
| Tekst | Te vertalen tekst.|Hallo wereld<br/>Goedemorgen<br/>Goedenavond|

## <a name="tv"></a>TV 
 
Het domein TV biedt intents en entiteiten voor het beheren van televisies.

### <a name="examples"></a>Voorbeelden
```
Switch channel to BBC
Show TV guide
Watch National Geographic
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| ChangeChannel| Een kanaal op een TV wijzigen.|Wijziging kanaal CNN<br/>Switchkanaal BBC<br/>Ga naar het kanaal 4|
| ShowGuide| De gids TV weergeven.|Tv-gids weergeven<br/>Wat is nu op kanaal film?<br/>Mijn lijst met programma's weergeven|
| WatchTV| Vraag een tv-kanaal bekijkt.|Ik wil het kanaal Disney bekijken<br/>Ga naar TV Neem<br/>Bekijk National geografische|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| ChannelName | De naam van een tv-kanaal.|CNN<br/>BBC<br/>Film kanaal|

## <a name="utilities"></a>Nutsbedrijven  
Het domein van de hulpprogramma's biedt intents voor taken die gemeenschappelijk voor veel taken, zoals begroetingen, annulering, bevestiging, help, herhaling, navigatie zijn, starten en stoppen.

### <a name="examples"></a>Voorbeelden
```
Go back to Twitter
Please help
Repeat last question please
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Annuleren | Een actie annuleren.|Annuleer het bericht<br/>Ik wil niet meer het e-mailbericht verzenden|
| Bevestigen | Bevestig de actie.|Ja bevestigen geselecteerd ik<br/>Ik ben goed bevestigen<br/>Ik ben bevestigen OK|
| FinishTask | Een taak gestart voor de gebruiker voltooien.|Ik ben klaar<br/>Ik ben klaar met<br/>Het is voltooid|
| GoBack | Ga terug naar stap of Ga terug naar de vorige stap.|Ga terug naar Twitter<br/>Een stap terug<br/>Terug|
| Help | Aanvraag voor hulp.|Help<br/>help openen<br/>Help|
| Herhalen | Een actie herhalen.|Herhaal de laatste vraag Neem<br/>Herhaal de laatste nummer|
| ShowNext | Het volgende item in een serie weergeven. |De volgende gateway weergeven<br/>Ga naar de volgende pagina|
| ShowPrevious | Het vorige item in een serie weergeven.|Vorige weergeven|
| Opnieuw | Start de app opnieuw of start een nieuwe sessie.|Opnieuw beginnen<br/>Nieuwe sessie<br/>opnieuw opstarten|
| Stoppen | Een actie stoppen.| Deze melding stoppen<br/>Afsluiten van<br/>Neem stoppen|

## <a name="weather"></a>Weer 
Het domein weer biedt intents en entiteiten voor het ophalen van de rapporten weer en prognoses.

### <a name="examples"></a>Voorbeelden
```
weather in London in september
What?s the 10 day forecast?
What's the average temperature in India in september?
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| GetCondition | Historische gegevens gerelateerd aan weer opgehaald. |weer in Londen september<br/>Wat is de gemiddelde temperatuur India september?|
| GetForecast | Ophalen van het huidige weer en het voorspellen van de volgende paar dagen. |Hoe wordt het weer vandaag?<br/>Wat is de 10 dagen forecast?<br/>Hoe worden de weer weekend?|

### <a name="entities"></a>Entiteiten
| De naam van de entiteit | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Locatie| De absolute locatie voor een aanvraag weer.|Seattle<br/>Parijs<br/>Palo Alto|

## <a name="web"></a>Web 
Het domein Web biedt een doel voor het navigeren naar een website.

### <a name="examples"></a>Voorbeelden
```
Navigate to facebook.com
Go to www.twitter.com
Navigate to www.bing.com
```

### <a name="intents"></a>Intents
| De naam van de opzet | Beschrijving | Voorbeelden |
| ---------------- |-----------------------|----|
| Navigeren | Een aanvraag om naar een opgegeven website te gaan. |Navigeer naar facebook.com<br/>Ga naar www.twitter.com|

