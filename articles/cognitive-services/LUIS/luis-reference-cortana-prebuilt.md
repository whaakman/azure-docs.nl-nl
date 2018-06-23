---
title: Vooraf gedefinieerde app Cortana verwijzen naar | Microsoft Docs
description: Verwijzing voor de Cortana persoonlijke assistent een vooraf gedefinieerde toepassing van Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: cd808c30a6781fc0252992c13ba247486e35ad44
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344588"
---
# <a name="cortana-prebuilt-app-reference"></a>Vooraf gedefinieerde app Cortana verwijst naar
Deze naslaginformatie bevat de intents en de entiteiten die de vooraf gedefinieerde app Cortana herkent.

## <a name="cortana-prebuilt-intents"></a>Cortana vooraf gedefinieerde intents
De toepassing vooraf gedefinieerde persoonlijke assistent kan de volgende intents identificeren:

| Doel | Voorbeeld utterances |
|--------| ------------------|
| Builtin.Intent.alarm.alarm_other|bijwerken van mijn 7:30 alarm moet acht uur <br/>Mijn alarm wijzigen van 8 uur in 9: 00 uur |
| Builtin.Intent.alarm.delete_alarm|verwijderen van een waarschuwing wordt gegeven <br/>verwijderen van mijn alarm 'ontwaken'|
| Builtin.Intent.alarm.find_alarm|welk tijdstip is mijn alarm wake-up instellen voor? <br/> Mijn alarm wake-up is ingeschakeld? |
| Builtin.Intent.alarm.set_alarm|Mijn alarm wake-up inschakelen<br/>kunt u een waarschuwing instellen voor 12 aangeroepen antibiotica hebben?|
| Builtin.Intent.alarm.Snooze|bewerkingen worden uitgesteld alarm voor 5 minuten<br/>Waarschuwing uitstellen|
| Builtin.Intent.alarm.time_remaining| hoe lang moet ik hebben tot 'wake-up'? <br/> na hoeveel tijd totdat mijn volgende alarm?|
| Builtin.Intent.alarm.turn_off_alarm | Mijn alarm 7 uur uitschakelen <br/> Mijn alarm wake-up uitschakelen |
| Builtin.Intent.Calendar.change_calendar_entry| een afspraak wijzigen<br/>Mijn vergadering vanaf vandaag aan morgen verplaatsen|
|Builtin.Intent.Calendar.check_availability|tim bezet is op vrijdag?<br/>vrije ruimte op zaterdag brian is|
|Builtin.Intent.Calendar.connect_to_meeting|verbinding maken met een vergadering<br/>deelnemen aan de vergadering online|
|Builtin.Intent.Calendar.create_calendar_entry|Ik wil een afspraak met tony voor vrijdag<br/>een afspraak met ties om 2 uur op zondag maken|
|Builtin.Intent.Calendar.delete_calendar_entry|Mijn afspraak verwijderen<br/>de vergadering 3 middags morgen verwijderen uit Mijn agenda|
|Builtin.Intent.Calendar.find_calendar_entry|Mijn agenda weergeven<br/>mijn wekelijkse agenda weergeven|
|Builtin.Intent.Calendar.find_calendar_when|Wanneer is mijn volgende vergadering met Jan?<br/>welk tijdstip is mijn afspraak met larry op maandag?|
|Builtin.Intent.Calendar.find_calendar_where|de locatie van mijn vergadering 6 uur weergeven<br/>waar is die vergadering met Jan?|
|Builtin.Intent.Calendar.find_calendar_who|wie is deze vergadering met?<br/>die anders is uitgenodigd?|
|Builtin.Intent.Calendar.find_calendar_why|Wat zijn de details van mijn vergadering 11 uur?<br/>Wat is die vergadering met Jan over?|
|Builtin.Intent.Calendar.find_duration|hoe lang is mijn volgende vergadering<br/>het aantal minuten lang is mijn vergadering deze 's middags|
|Builtin.Intent.Calendar.time_remaining|na hoeveel tijd totdat mijn volgende afspraak?<br/>hoeveel meer tijd heb ik vóór de vergadering?|
|Builtin.Intent.Communication.add_contact|Sla dit nummer en de naam op als jose plaatsen<br/>jason plaatsen in de lijst met contactpersonen|
|Builtin.Intent.Communication.answer_phone|antwoord<br/>de oproep wordt beantwoord|
|Builtin.Intent.Communication.assign_nickname|bijnaam voor nickolas bewerken<br/>toevoegen van een bijnaam voor Jan de Vries|
|Builtin.Intent.Communication.call_voice_mail|voicemail<br/>voicemail oproepen|
|Builtin.Intent.Communication.find_contact|Mijn contactpersonen weergeven<br/>contactpersonen zoeken|
|Builtin.Intent.Communication.forwarding_off|stopt met het doorsturen van mijn aanroepen<br/>aanroep doorsturen uitschakelen|
|Builtin.Intent.Communication.forwarding_on|aanroep doorsturen naar het verzenden van aanroepen naar telefoon thuis instellen<br/>doorsturen aanroepen naar telefoon thuis|
|Builtin.Intent.Communication.ignore_incoming|deze aanroep niet beantwoord<br/>Ik ben niet nu bezet|
|Builtin.Intent.Communication.ignore_with_message|geen antwoord die aanroep, maar in plaats daarvan een bericht verzenden<br/>negeren en terug in een tekst verzenden|
|Builtin.Intent.Communication.make_call|bob en john aanroepen<br/>Roep mom en Pa|
|Builtin.Intent.Communication.press_key|ster kiezen<br/>Druk op de 1-2-3|
|Builtin.Intent.Communication.read_aloud|tekst lezen<br/>wat ze zeggen in het bericht|
|Builtin.Intent.Communication.redial|de laatste aanroep opnieuw<br/>opnieuw kiezen|
|Builtin.Intent.Communication.send_email|e-mailbericht wilt voorzien van een wateren Mark of diner afgelopen week schitterende is<br/>een e-mail sturen naar bob|
|Builtin.Intent.Communication.send_text|tekst naar bob en john verzenden<br/>message|
|Builtin.Intent.Communication.speakerphone_off|Breng me uitschakelen spreker<br/>Handsfree uitschakelen|
|Builtin.Intent.Communication.speakerphone_on|Handsfree-modus<br/>in de Handsfree plaatsen|
|Builtin.Intent.MYSTUFF.find_attachment|Zoek dat de john document per e-mail verzonden mij gisteren <br/>het document in john vinden|
|Builtin.Intent.MYSTUFF.find_my_stuff|Ik wil mijn winkelwagen lijst van gisteren bewerken<br/>de notities van mijn chemistry vanaf September zoeken
|Builtin.Intent.MYSTUFF.search_messages|Open bericht <br/>berichten|
|Builtin.Intent.MYSTUFF.transform_my_stuff|delen van mijn winkelwagen lijst met Mijn man<br/>Mijn winkelwagen lijst verwijderen|
|Builtin.Intent.ondevice.open_setting|cortana instellingen openen <br/>Ga naar meldingen|
|Builtin.Intent.ondevice.pause|music uitschakelen<br/>Music uitschakelen|
|Builtin.Intent.ondevice.play_music|Ik wil de eigenaar van een gemakkelijk te beïnvloeden hart horen<br/>Sommige gospel muziek afspelen|
|Builtin.Intent.ondevice.recognize_song|Laat me weten wat dit nummer is<br/>analyseren en ophalen van de titel van nummer|
|Builtin.Intent.ondevice.Repeat|Herhaal dit nummer<br/>Dit nummer opnieuw afspelen|
|Builtin.Intent.ondevice.Resume|music starten<br/>music opnieuw starten|
|Builtin.Intent.ondevice.skip_back|back-up van een bijhouden<br/>vorige nummer|
|Builtin.Intent.ondevice.skip_forward|volgende nummer<br/>een nummer overslaan|
|Builtin.Intent.ondevice.turn_off_setting|Wi-Fi uitschakelen<br/>Vliegtuigmodus uitschakelen|
|Builtin.Intent.ondevice.turn_on_setting|Wi-Fi op<br/>bluetooth inschakelen|
|Builtin.Intent.Places.add_favorite_place|Dit adres toevoegen aan Favorieten<br/>deze locatie in Mijn favorieten opslaan|
|Builtin.Intent.Places.book_public_transportation|een ticket train kopen<br/>een pass tram boek|
|Builtin.Intent.Places.book_taxi|u vindt mij een rijpositie op deze uur?<br/>een taxi vinden|
|Builtin.Intent.Places.check_area_traffic|Wat is het verkeer zoals op 520<br/>hoe wordt het verkeer op i-5|
|Builtin.Intent.Places.check_into_place|Controleer in joya<br/>Schakel dit selectievakje|
|Builtin.Intent.Places.check_route_traffic|het verkeer op de manier om kirkland weergeven<br/>hoe wordt het verkeer naar seattle?|
|Builtin.Intent.Places.find_place|waar ik woont <br/>Geef me de top-3 Japanse restaurant|
|Builtin.Intent.Places.get_address|weergeven het adres van guu op robson straat<br/>Wat is het adres van de dichtstbijzijnde starbucks?|
|Builtin.Intent.Places.get_coupon|aanbiedingen op televisies in mijn regio vinden<br/>kortingen in mountain weergeven|
|Builtin.Intent.Places.get_distance|hoe ver inn vakantie is?<br/>Wat is de afstand tot tahoe|
|Builtin.Intent.Places.get_hours|Wat zijn de balk van del corso uur op maandag?<br/>Wanneer is de bibliotheek openen?|
|Builtin.Intent.Places.get_menu|applebee van menu weergeven<br/>Wat is er in het menu sizzler|
|Builtin.Intent.Places.get_phone_number|Geeft het aantal voor thuis depot<br/>Wat is het telefoonnummer van de dichtstbijzijnde starbucks?|
|Builtin.Intent.Places.get_price_range|Wat kost diner rode kreeft kosten<br/>hoe dure is paarse pub|
|Builtin.Intent.Places.get_reviews|beoordelingen voor lokale hardware winkels weergeven<br/>Ik wil restaurant beoordeelt weergeven|
|Builtin.Intent.Places.get_route|Geef me routebeschrijving naar|het is mogelijk om pizza hut voet|
|Builtin.Intent.Places.get_star_rating|hoeveel sterren heeft de Franse wasgoed?<br/>het aquarium in monterrey goed is?|
|Builtin.Intent.Places.get_transportation_schedule|welk tijdstip de san francisco veerboot verlaten?<br/>Wanneer is de meest recente trein voor seattle?|
|Builtin.Intent.Places.get_travel_time|Wat is de aangedreven tijd om denver van SF<br/>hoe lang het duurt om te krijgen tot san francisco hiervandaan|
|Builtin.Intent.Places.make_call|aanroep Dr. smith in bellevue<br/>aanroep van de opslagplaats thuis in 1e straat|
|Builtin.Intent.Places.rate_place|Geef een classificatie voor deze restaurant<br/>frequentie waarmee il fornaio 5 sterren op yelp|
|Builtin.Intent.Places.show_map|Wat is mijn huidige locatie <br/>Wat is mijn locatie|
|Builtin.Intent.Places.takes_reservations|is het mogelijk is gereserveerd op de productieve tuin<br/>accepteert de galerie illustraties reserveringen|
|Builtin.Intent.Reminder.change_reminder|een herinnering wijzigen<br/>Mijn afbeelding dag herinnering omhoog met 30 minuten|
|Builtin.Intent.Reminder.create_single_reminder|herinnering voor ontwaken op 7 uur<br/>herinnering voor truc of behandelen met luca op 4:40 pm|
|Builtin.Intent.Reminder.delete_reminder|Deze herinnering verwijderen<br/>Mijn herinnering afbeelding verwijderen|
|Builtin.Intent.Reminder.find_reminder|heb ik herinneringen is ingesteld voor vandaag<br/>heb ik een herinnering is ingesteld voor zijn party|
|Builtin.Intent.Reminder.read_aloud|herinnering hardop lezen<br/>die herinnering lezen|
|Builtin.Intent.Reminder.Snooze|die herinnering uitstellen tot morgen<br/>Deze melding uitstellen|
|Builtin.Intent.Reminder.turn_off_reminder|herinnering uitschakelen<br/>luchthaven ophalen herinnering negeren|
|Builtin.Intent.Weather.change_temperature_unit|wijzigen van fahrenheit in kelvin<br/>wijzigen van fahrenheit in c|
|Builtin.Intent.Weather.check_weather|de prognose voor mijn toekomstige reis voor seattle weergeven<br/>hoe worden de weer weekend|
|Builtin.Intent.Weather.check_weather_facts|Wat is het weer zoals in Hawaï in December?<br/>Wat is de temperatuur ditmaal vorig jaar?|
|Builtin.Intent.Weather.compare_weather|Geef me een vergelijking tussen de temperatuur hoge en lage waarden van dallas en houston, tx<br/>hoe wordt het weer vergelijking slc en nyc|
|Builtin.Intent.Weather.get_frequent_locations|Geef me mijn meest voorkomende locatie<br/>meest stopt weergeven|
|Builtin.Intent.Weather.get_weather_advisory|waarschuwingen weer<br/>weer advisory voor sacramento weergeven|
|Builtin.Intent.Weather.get_weather_maps|een kaart weer weergeven<br/>maps weer Afrika weergeven|
|Builtin.Intent.Weather.question_weather|kan het mistige morgen ochtend zijn?<br/>moet ik shovel sneeuw weekend?|
|Builtin.Intent.Weather.show_weather_progression|lokale weer radargrafiek weergeven<br/>beginnen met radargrafiek|
|Builtin.Intent.None|Hoe oud ben je<br/>camera openen|

## <a name="prebuilt-entities"></a>Vooraf gedefinieerde entiteiten 

Hier volgen enkele voorbeelden van de toepassing vooraf gedefinieerde persoonlijke assistent kunt identificeren entiteiten:

|Entiteit |Voorbeeld van een entiteit in utterance |
|-------|------------------|
|Builtin.alarm.alarm_state | Schakel `off` mijn alarm wake-up    <br/> Mijn alarm wake-up is `on`  | 
|Builtin.alarm.duration |voor bewerkingen worden uitgesteld `10 minutes`    <br/> Waarschuwing voor uitstellen `5 minutes`  |
|Builtin.alarm.start_date | instellen van een alarm voor `monday` op 7 uur   <br/> instellen van een alarm voor `tomorrow` op twaalf uur 's middags   |
|Builtin.alarm.start_time | maken van een waarschuwing voor `30 minutes`    <br/> Stel het alarm om uit te gaan `in 20 minutes`   |
|Builtin.alarm.title | is mijn `wake up` waarschuwing aan <br/> u kunt een waarschuwing instellen voor het kwartaal tot 12 maandag tot vrijdag aangeroepen `take antibiotics` |
|Builtin.Calendar.absolute_location | Maak een afspraak voor morgen op `123 main street`   <br/> de vergadering wordt gehouden `cincinnati` op de 5e van juni    |
|Builtin.Calendar.Contact_Name|een marketing vergadering in Mijn agenda plaatsen en zorg ervoor dat `joe` is er <br/>Ik wil een lunch op il fornaio instellen en uit te nodigen `paul` |   
|Builtin.Calendar.destination_calendar|Voeg deze aan mijn `work` planning   <br/>Plaats deze in mijn `personal` kalender |
|Builtin.Calendar.duration| instellen van een afspraak voor `an hour` op vanavond 6 <br/>  adresboek een `2 hour` vergadering met Jan |  
|Builtin.Calendar.end_date | een vakantie aangeroepen vanuit morgen tot agenda-item maken `next monday` <br/>    mijn tijd als bezet tot blokkeren `monday, october 5th` | 
|Builtin.Calendar.end_time | de vergadering eindigt op `5:30 PM` <br/> plannen van 11 tot en met `noon`  |   
|Builtin.Calendar.implicit_location | de afspraak van de dmv annuleren <br/> Wijzig de locatie van de mijl verjaardag `poppy restaurant` |    
|Builtin.Calendar.move_earlier_time | push de vergadering doorsturen `an hour` <br/> de tandarts afspraak omhoog verplaatsen `30 minutes`       |
|Builtin.Calendar.move_later_time | Mijn tandarts verplaatsen `30 minutes`    <br/> de vergadering push uit `an hour` |  
|Builtin.Calendar.original_start_date | Mijn afspraak op de barber van 'Dinsdag' tot woensdag opnieuw plannen <br/> verplaatsen van mijn vergadering met ken van `monday` naar dinsdag |
|Builtin.Calendar.original_start_time | Mijn vergadering plannen `2:00` 3  <br/> wijzigen van mijn tandarts van `3:30` tot en met 4 |  
|Builtin.Calendar.start_date | welk tijdstip begint mijn partij op `flag day` <br/> plannen van lunch voor de `friday after next` op twaalf uur 's middags 
|Builtin.Calendar.start_time | Ik wil plannen voor `this morning` <br/> Ik wil plannen in de `morning` |  
|Builtin.Calendar.title | `vet appointment`  <br/> `dentist` Tuesday |
|Builtin.Communication.audio_device_type | het gebruik van de aanroep `bluetooth`  <br/> Bel met mijn `headset` | 
|Builtin.Communication.Contact_Name | tekst `bob jones`  <br/> | aanroep `sarah`|
|Builtin.Communication.destination_platform | dave-aanroep in `london` <br/> aanroepen zijn `work line` |    
|Builtin.Communication.from_relationship_name | weergeven van aanroepen vanuit Mijn `daughter` <br/> lezen van het e-mailbericht `mom`   |   
|Builtin.Communication.Key | inbelverbinding `star` <br/>  Druk op de `hash` sleutel    |
|Builtin.Communication.Message | e-mailadres in te spreken carly `i'm running late` <br/> tekst angus smith `good luck on your exam` |    
|Builtin.Communication.message_category | nieuw e-mailadres is gemarkeerd voor `follow up`  <br/> nieuw e-mailadres gemarkeerd `high priority` |    
|Builtin.Communication.message_type | verzenden van een `email`   <br/> Lees mijn `text` hardop berichten |
|Builtin.Communication.phone_number | Ik wil kiezen `1-800-328-9459` <br/>     aanroep `555-555-5555` |   
|Builtin.Communication.relationship_name | tekst mijn `husband` <br/>  E-mail `family`| 
|Builtin.Communication.slot_attribute | Wijzig de `recipient` <br/>    Wijzig de `text` | 
|Builtin.comminication.source_platform | belt hem uit `skype` <br/> belt hem uit Mijn `personal line` |
|Builtin.MYSTUFF.Attachment| met behulp van documenten `attached` <br/> Zoek de e-mailbericht `attachment` bob verzonden |
|Builtin.MYSTUFF.Contact_Name| het werkblad dat ties om te verzonden zoeken `me` <br/> Wanneer het document i verzonden naar `susan` laatste nacht |
|Builtin.MYSTUFF.data_source | `c:\dev\` <br/> Mijn `desktop` <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|Builtin.MYSTUFF.data_type | Zoek de `document` i laatste nacht gewerkt <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> online zetten van de Mark `visio diagram`  |
|Builtin.MYSTUFF.end_date| de documenten die ik gewerkt weergeven `between yesterday and today`   <br/> welke doc i werkte vinden `before thursday the 31st` |
|Builtin.MYSTUFF.end_time| zoeken naar bestanden i opgeslagen `before noon` <br/> welke doc i werkte vinden `before 4pm`|      
|Builtin.MYSTUFF.file_action| Open het werkblad ik `saved` gisteren <br/> het werkblad vinden kevin `created`| 
|Builtin.MYSTUFF.from_contact_name | Zoek het voorstel `jason` mij verzonden <br/> Open `isaac` van laatste e-mailadres |
|Builtin.MYSTUFF.Keyword | Geef de `french conjugation` bestanden <br/> Zoek de `marketing plan` i opgesteld gisteren |
|Builtin.MYSTUFF.Location | het document i bewerkt `work` <br/> foto's i vond in `paris` |
|Builtin.MYSTUFF.message_category | Zoek naar mijn `new` e-mailberichten <br/> zoeken naar mijn `high priority` e |
|Builtin.MYSTUFF.message_type | Controleer mijn `email` <br/>  demonstratie mijn `text` berichten  |
|Builtin.MYSTUFF.source_platform | Zoek mijn `hotmail` e-mailadres voor e-mailadres van john    <br/> het document i verzonden van vinden `work` |
|Builtin.MYSTUFF.start_date | notities van zoeken `january` <br/> het e-mailbericht i verzenden rob zoeken `after january 1st` |
|Builtin.MYSTUFF.start_time | dat e-mailadres vinden i verzonden rob enige tijd voordat 14 uur maar `after noon`? <br/> het werkblad dat Kristin verzonden aan mij die ik bewerkt vinden `last night` | 
|Builtin.MYSTUFF.title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|Builtin.MYSTUFF.transform_action | `download` de john bestand verzonden mij <br/> `open` Mijn aantekening richtlijnen document |
|Builtin.Note.note_text | Maak een kruidenierswaren lijst inclusief `pork chops, applesauce and milk` <br/> Maak een notitie naar `buy milk` |
|Builtin.Note.title | Noteer aangeroepen `grocery list` <br/> Noteer aangeroepen `people to call` |
|Builtin.ondevice.music_artist_name | Alles wat door afspelen `rufus wainwright` <br/> afspelen `garth brooks` muziek |   
|Builtin.ondevice.music_genre | weergeven `classic rock` nummers <br/> afspelen mijn `classical` muziek van de Barok periode |
|Builtin.ondevice.music_playlist | alle britney spears van willekeurige `workout` afspeellijst <br/> afspelen `breakup` afspeellijst
|Builtin.ondevice.music_song_name | Play `summertime` <br/> Play `me and bobby mcgee` | 
|Builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|Builtin.Places.absolute_location | Breng me naar het snijpunt van de `5th and pike` <br/> Nee, ik wil dat routebeschrijving naar `1 microsoft way redmond wa 98052` |
|Builtin.Places.atmosphere | Zoek naar `interesting` dat wordt verzonden    <br/> waar vind ik een `casual` restaurant |  
|Builtin.Places.audio_device_type |aanroepen van de office post voor `hands free` <br/> papa john met aanroepen `speakerphone` |    
|Builtin.Places.avoid_route | Vermijd de `toll road` <br/> hoe kom ik te vermijden san francisco de `construction on 101` |  
|Builtin.Places.Cuisine | `halal` Tijdscheid bijna mountain weergeven   <br/> `kosher` op de schiereiland eten fijn |
|Builtin.Places.date | maken van een reservering voor `next friday the 12th` <br/>  is mashiko geopend op `mondays` ? |
|Builtin.Places.discount_type | zoeken naar een `coupon` voor macy van <br/> vinden mij een `coupon` |
|Builtin.Places.Distance | is er een goede diner `within 5 miles` van hier <br/> zoeken naar groepen. `within 15 miles` |   
|Builtin.Places.from_absolute_location | aanwijzingen van `45 elm street` naar de introductiepagina <br/> hoe kom ik aanwijzingen van `san francisco` naar palo alto  |
|Builtin.Places.from_place_name | verkeer van de `post office` 56 center straat <br/> hoe kom ik aanwijzingen van `home depot` naar lowes |
|Builtin.Places.from_place_type | aanwijzingen voor het centrum van `work` <br/>  hoe kom ik richtlijnen van de `drug store` naar de introductiepagina |
|Builtin.Places.meal_type | in de buurt plaatsen voor `dinner` <br/> een goede plaats vinden voor een bedrijf `lunch` | 
|Builtin.Places.nearby | Sommige cool internetcafés weergeven `near` mij  <br/> zijn er goede Libanees restaurant `around` hier? |
|Builtin.Places.open_status | Wanneer is het winkelcentrum `closed` <br/> hoe kom ik de `opening` uur van de store | 
|Builtin.Places.place_name | Breng me naar `central park` <br/> opzoeken van de `eiffel tower` |   
|Builtin.Places.place_type | `atms` <br/> `post office` |
|Builtin.Places.prefer_route | weergeven van de richtingen door de `shortest` route <br/> nemen de `fastest` route | 
|Builtin.Places.price_range| Geef me plaatsen die zijn `moderately affordable` <br/>  Ik wil een `expensive` een   |
|Builtin.Places.product | waar vind ik `fresh fish` rond hier  <br/> waar om hier verkocht `bare minerals` |
|Builtin.Places.public_transportation_route | busschema voor de `m2` bus <br/> Bus route `3x` |  
|Builtin.Places.rating | weergeven `3 star` restaurant <br/> resultaten weergeven `3 stars or higher`  |
|Builtin.Places.reservation_number | een tabel voor het adresboek `seven` personen <br/>  maken van een reservering voor `two` op il-fornaio |
|Builtin.Places.results_number | Geef de `10` in internetcafés die het dichtst bij hier <br/> weergeven eerste `3` aquariums  
|Builtin.Places.service_provided | waar kan ik ga naar `whale watch` door bus? <br/> Ik moet een mechanic naar `fix my brakes` |    
|Builtin.Places.time | Ik wil plaatsen die zijn geopend op zaterdag om `8 am`| is mashiko openen `now` |
|Builtin.Places.transportation_company | schema's voor trainen `new jersey transit` <br/> Ik kan terecht op `bart` | 
|Builtin.Places.transportation_type | waar wordt een muziekwinkel ik toegang krijgen tot `on foot`? <br/>  Ik wil `biking` routebeschrijving naar mashiko|
|Builtin.Places.travel_time | Ik wil station `less than 15 minutes` <br/>   Ik wil ergens die ik toegang krijgen tot in `under 15 minutes` |   
|Builtin.Reminder.absolute_location | herinnering voor mijn dad aanroepen wanneer ik terechtkomen in `chicago` <br/> Wanneer ik weer aan de slag `seattle` herinneren gas ophalen |
|Builtin.Reminder.Contact_Name | Wanneer `bob` -aanroepen, herinnering voor de grapje vertellen <br/> maken van een herinnering om de school bus wanneer ik neem op met contact `arthur` |
|Builtin.Reminder.leaving_absolute_location | herinnering craig opgepikt bij het verlaten `1200 main` |
|Builtin.Reminder.leaving_implicit_location | herinnering voor het ophalen van gas wanneer ik laat `work`  |
|Builtin.Reminder.original_start_date | Wijzig de herinnering over de graskantensnijders van `saturday` naar zondag <br/> verplaatsen van Mijn herinnering over school van `monday` naar dinsdag   |
|Builtin.Reminder.relationship_name | Als mijn `husband` -aanroepen, herinnering voor te vertellen over de vergadering pta <br/> Help me herinneren opnieuw wanneer `mom` aanroepen|
|Builtin.Reminder.reminder_text | kunt u herinnering voor `bring up my small spot of patchy skin` wanneer dr smith'calls  <br/> herinnering voor `pick up dry cleaning` op 4:40   |
|Builtin.Reminder.start_date | Help me herinneren de `thursday after next` om 8 uur  <br/> Help me herinneren `next thursday the 18th` om 8 uur    |
|Builtin.Reminder.start_time | een herinnering maken `in 30 minutes` <br/> een herinnering water van de fabrieken maken `this evening at 7` |  
|Builtin.Weather.absolute_location | het wordt regen `boston` <br/> Wat is de prognose voor `seattle`?  |
|Builtin.Weather.date_range | weer in nyc `this weekend` <br/>   opzoeken van de `five day` prognose in hollywood florida |
|Builtin.Weather.suitable_for | kan ik ga `hiking` in korte weekend?   <br/> wordt het niet handig om te `walk` aan het spel vandaag? | 
|Builtin.Weather.temperature_unit | Wat is de temperatuur vandaag in `kelvin`   <br/> de temps in weergeven `celsius` |  
|Builtin.Weather.time_range | biedt deze eruitzien zoals deze wordt sneeuwruimen `tonight`? <br/> is het recht winderig `now`?  |
|Builtin.Weather.weather_condition | weergeven `precipitation` <br/> hoe thick is de `snow` op lake tahoe nu?  |

