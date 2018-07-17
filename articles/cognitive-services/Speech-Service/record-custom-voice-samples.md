---
title: Het opnemen van stem-voorbeelden voor het maken van een aangepaste gesproken | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Maak een stem productiekwaliteit hulpmiddelen naar door een robuuste script voorbereiden, verhuur goede stem talent en professioneel op te nemen.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/5/2018
ms.author: v-jerkin
ms.openlocfilehash: 11d96ce0c92916e1975e0cb403aabf057ab8b825
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036826"
---
# <a name="how-to-record-voice-samples-for-a-custom-voice"></a>Het opnemen van stem-voorbeelden voor een aangepaste spraak

Het maken van een aangepaste gesproken van hoge kwaliteit productie helemaal is niet een eenvoudige onderneming. Het centrale onderdeel van een aangepaste gesproken is een grote verzameling van audio-voorbeelden van menselijke spraak. Het is essentieel dat deze audio-opnamen van hoge kwaliteit zijn. Kies stem talent die heeft ervaring bij het maken van dit soort opnamen en hebben ze geregistreerd door een bevoegde opname engineer met behulp van professionele apparatuur.

Voordat u kunt deze opnamen, dient u echter een script: de woorden die wordt gesproken worden door uw stem talent te maken van de audio-voorbeelden. Het script moet voor de beste resultaten hebben goede fonetische dekking en voldoende verscheidenheid aan het aangepaste spraak-model te trainen.

Veel kleine, maar belangrijke details gaat u naar het maken van een professionele spraakopname. Deze handleiding is een schema voor een proces waarmee u goed, consistente resultaten krijgt.

> [!TIP]
> Voor de kwaliteitsresultaten van de hoogste, kunt u Microsoft helpt bij het ontwikkelen van uw aangepaste gesproken benaderen. Microsoft heeft uitgebreide ervaring produceren van hoogwaardige stemmen voor een eigen-producten, met inbegrip van Cortana en Office.

## <a name="voice-recording-roles"></a>Spraakopname rollen

Er zijn vier eenvoudige rollen in een aangepaste gesproken opname-project.

Rol|Doel
-|-
Stem talent        |De persoon waarvan stem de basis van de aangepaste stem vormt.
Opname-engineer  |De persoon die verantwoordelijk is voor de technische aspecten van de opname en werkt de opname-apparatuur.
Directeur            |Het script wordt voorbereid en prestaties van de stem-talent coaches.
Editor              |Hiermee wordt de audio-bestanden en bereidt u deze voor het uploaden naar de portal voor aangepaste spraak.

Een individu kan meer dan één rol vult. Deze handleiding wordt ervan uitgegaan dat u worden voornamelijk de rol van de directeur vullen zal en inhuren stem talent en een technicus opnemen. Er is informatie over de rol van de technicus opname in het geval u wilt maken van de opnamen zelf.

## <a name="choosing-voice-talent"></a>Stem talent kiezen

Goede aangepaste gesproken talent maken actors met ervaring in voiceover of stem teken voor werk U kunt ook vaak geschikt talent tussen-commentaar en nieuwslezers vinden.

Kies stem talent waarvan natuurlijke stem u graag. Het is mogelijk te maken van de unieke 'character' stemmen, maar is veel moeilijker is voor de meeste talent uit te voeren ze consistent en de moeite stem stam kan veroorzaken.

> [!TIP]
> Over het algemeen te voorkomen dat herkenbare stemmen gebruiken voor het maken van een aangepaste spraak, tenzij het doel natuurlijk is voor het produceren van een stem beroemdheden. Minder bekende stemmen zijn meestal minder aandacht aan gebruikers.

De enkele van de belangrijkste factor voor het kiezen van spraak talent is consistentie. In het ideale geval moeten alle uw opnamen geluids-, zoals ze zijn gemaakt op dezelfde dag in één ruimte. U kunt deze ideaal via goed opname procedures en engineering benaderen. 

Uw stem talent is de andere helft van de vergelijking. Hij of zij moet mogelijk met consistente snelheid, volumeniveau, inspiratie en toon te spreken. Schakel diction is een moet. Er moet ook uw talent kunnen strikt zijn of haar inspiratie variatie emotionele invloed en spraak gebaren.

Aangepaste spraak-voorbeelden op te nemen kan meer vermoeiend dan andere soorten stem werk zijn. De meeste stem talent kunt vastleggen voor twee of drie uur per dag. Aantal sessies beperken tot drie of vier per week, met een dag uit tussen indien mogelijk.

Opnamen, voor een model spraak mentale neutrale moet worden gemaakt. Dat wil zeggen, worden een sad utterance niet gelezen in een sad manier. Stemming kan later worden toegevoegd aan de kunstmatige spraak. Werken met uw stem talent voor het ontwikkelen van een 'persoon' waarin de totale geluid en emotionele toon van de aangepaste stem. In het proces, zult u welke "neutrale" benadering voor die persona lokaliseren.

Een persoon kan bijvoorbeeld een natuurlijk upbeat persoonlijkheid hebben. De stem kan dus een notitie van optimisme uitvoeren, zelfs wanneer geval spreken. Dergelijke persoonlijkheid kenmerk hebben moet echter subtiele en consistent zijn. Luister naar enkele bestaande stemmen voor een beter beeld van wat u die gericht zijn bent.

### <a name="legalities"></a>Legalities

Normaal gesproken moet u eigenaar van de opnamen stem die u maakt. Uw stem talent moet aanmerking voor een contract werk voor medewerkers voor het project.

## <a name="creating-a-script"></a>Het maken van een script

Het beginpunt van een aangepaste spraakopname sessie is het script, waarin de uitingen door uw stem talent worden uitgesproken. (De term 'uitingen' omvat zowel volledige zinnen en kortere zinnen.)

De uitingen in het script kunnen afkomstig zijn vanaf elke locatie: fiction, niet-fiction en transcripties van toespraken, nieuws, rapporten en anders beschikbaar in de vorm afgedrukt. Als u wilt om te controleren of dat uw stem op specifieke typen van woorden (zoals medische terminologie of programming jargon) heeft, kunt u om op te nemen van zinnen vanaf wetenschappelijk documenten of technische documenten. (Zie [Legalities](#legalities) hieronder.) U kunt ook uw eigen tekst schrijven.

Uw uitingen niet afkomstig moet zijn van dezelfde bron of hetzelfde type gegevensbron. Als u wilt maken van de opname zelf, in plaats van te gaan naar een opname-studio, hier is een korte inleiding tot. Dankzij de opkomst van home opnemen en podcasts is het eenvoudiger dan ooit om te zoeken naar goede opname advies en bronnen online. Hierdoor krijgt een grotere kans van het uitspreken van deze zinnen en uw aangepaste spraak. Gordijnen aan de muur kunnen worden gebruikt te verminderen echo en neutraliseren of "deaden" het geluid van de ruimte bevindt.

Gebruik een hoge kwaliteit studio koeler microfoon ('mic') die bestemd zijn voor het opnemen van spraak. Sennheiser AKG en zelfs nieuwere zoomen microfoons kunt goede resultaten opleveren. U kunt uw mic kopen of huurt van een lokale audiovisuele verhuur vast.* 

Zoek naar een met een USB-interface. Dit type mic gemakkelijk de microfoon element, preamp en analoog naar digitaal converter gecombineerd in één pakket, verbinden te vereenvoudigen.

U kunt ook een analoge microfoon. Veel verhuur huizen bieden "Superieur" microfoons die gewend bent voor hun stem-teken zijn. Houd er rekening mee dat professionele analoog versnelling met gelijke taakverdeling XLR-connectors gebruikt, in plaats van de 1/4" plug-in consumentenapparatuur gebruikt. Wanneer u analoog gaat, moet u ook een preamp en een computer audio-interface met deze connectors.

De microfoon installeren op een zelfstandige of tijdperk en gebruiken van een pop-filter in frot van de microfoon te elimineren ruis van "plosive" klinkt "p" en "b". Sommige microfoons worden geleverd met een onderbreking koppeling waarmee ze worden geïsoleerd van trillingen in de zelfstandige, wat nuttig is. De spraak-talent moet blijven op een consistente afstand van de microfoon.

### <a name="script-format"></a>Tape markeren waar ze moeten staan op de verdieping gebruiken.

Als de talent liever bevinden zich, erop letten mic afstand bewaken en te voorkomen dat stoel ruis. Gebruik een zelfstandige voor het opslaan van het script. Vermijd de zelfstandige sportvisserij zodat geluid richting van de microfoon kan worden weergegeven.

De persoon die daadwerkelijk de opname, de engineer, moeten zich in een afzonderlijk lokaal van de talent, met een manier om te communiceren met de talent in de opname tolloket (een "talkback-circuit').

* De opname moet bevatten als weinig ruis mogelijk, met als doel van een 80 db signaal ruis verhouding of hoger. Luister goed naar een opname van stilte in uw "tolloket,' afbeelding waar elke ruis afkomstig is van en de oorzaak te elimineren. Meest voorkomende oorzaken van de ruis zijn ventilatoren, TL lichte ballasten, verkeer op in de buurt wegen en apparatuur fans (zelfs laptops hebben ventilatoren).
* Een lege kolom waar u gaat schrijven in het getal of wanneer de code van elke utterance kunt u vinden in de registratie is voltooid.
* In sommige gevallen is het mogelijk een equalizer of een invoegtoepassing ruis vermindering software gebruiken om u te helpen ruis verwijderen uit uw opnamen, maar het is altijd verstandig om te voorkomen dat deze bij de bron indien mogelijk.

![Voorbeeldscript](media/custom-voice/script.png)

> [!NOTE]
> De meeste studios vastleggen in een korte segmenten, aangeduid als 'neemt', elke doorgaans met tien tot 24 uitingen nemen. Alleen waarbij het getal is voldoende zijn voor het vinden van een bepaalde utterance later opnieuw. Sommige studios wellicht beter langer opnamen maken. In dit geval moet u de time-offset opmerking in het bestand (de tijdcode). De studio heeft een prominente tijd weer te geven.

Niveaus moeten worden ingesteld, zodat die het beste van de beschikbare dynamisch bereik van digitale registratie zonder overdriving in vervorming wordt gebruikt. Dit betekent dat mogelijk, maar niet dus mogelijk dat de audio geen juist beeld geeft. Hieronder volgt een voorbeeld van de stroom van een goede opname.

goede opname golf U kunt zien dat de meeste van het bereik (hoogte) wordt gebruikt, maar de hoogste pieken van het signaal boven of onder aan de weergave niet bereiken.

### <a name="legalities"></a>Legalities

U ziet ook dat de stilte in de registratie een thin horizontale lijn benadert die wijzen op een verdieping lage ruis. Deze registratie heeft acceptabele dynamisch bereik en signaal ruis verhouding. Neem rechtstreeks op de computer met een goede audio-interface of een USB-poort, afhankelijk van het soort mic u gebruikt. Houd de audio keten eenvoudig: mic, preamp, audio-interface, computer.

Gelukkig is het mogelijk om volledig te voorkomen dat deze problemen. Als uw budget zeer krachtige is, probeert u het gratis Audacity.

|Tekstbron|Beschrijving|
|-|-|
|[Vastleggen met een 44,1 KHz 16-bits mono (kwaliteit CD) of hoger.](http://festvox.org/cmu_arctic/)|Huidige status-of-the-art is 48 KHz 24-bits, als uw apparatuur wordt ondersteund. U wordt verkleinen tot 16 KHz 16-bits de audio voordat u deze bij de portal voor aangepaste spraak indient.|
|Het loont nog steeds, hebben een hoge kwaliteit oorspronkelijke op te nemen in het geval van wijzigingen nodig zijn.<br>In het ideale geval hebben verschillende mensen in de rollen van director, engineering en talent vervullen.|Probeer niet om dit te doen alle zelf! In een knijpen, kunnen de directeur en engineer één persoon zijn. Voordat de sessie|
|Om te voorkomen dat ze studio tijd heeft verspild, doorloopt u het script met uw stem talent voordat de opnamesessie.|Als de stem-talent bekend zijn met de tekst verandert, kan hij of zij de uitspraak van woorden die niet bekend verduidelijken.|
|De meeste opname studios bieden elektronische weergave van scripts in de stand opnemen.|In dit geval, typ de opmerkingen bij de uitzending rechtstreeks in document van het script. (Het niet mogelijk om af te wijken copyright volledig in sommige rechtsgebieden.)|
|U nog steeds wilt papier aantekeningen op tijdens de sessie, maar.|Werkt onder een licentie is gedistribueerd, zoals Creative Commons of de documentatie van GNU gratis licentie. In deze fase, kunt u bewerken van kleine ongewenste geluiden die u tijdens de opname hebt gemist, zolang deze daadwerkelijke spraak niet overlappen. Als u een bestand niet kan oplossen, verwijderen van uw gegevensset volledig, waardoor een Houd er rekening mee dat u hebt gedaan.|

## <a name="recording-your-script"></a>Verkleinen elk bestand naar 16 KHz en 16 bits voordat u opslaat en, als u hebt genoteerd in stereo, verwijdert u het tweede kanaal.

Bij voorkeur, moet u uw script aan een professionele opname-studio die gespecialiseerd is in voiceover werk vastleggen. Archiveren van de oorspronkelijke opname in een veilige plaats als u wilt terugkeren naar het later opnieuw. Uw script en notities, te behouden.

U kunt nu uw opnamen uploaden en maken van uw aangepaste gesproken! De opname hebt weinig of geen dynamisch bereik compressie (maximaal 4:1). Het is essentieel dat de audio consistente volume en de verhouding van een hoge signaal ruis, terwijl deze gratis ongewenste geluiden hebben.

### <a name="doing-it-yourself"></a>Alles zelf te doen

Als u wilt maken van de opname zelf, in plaats van te gaan naar een opname-studio, hier is een korte inleiding tot. Dankzij de opkomst van home opnemen en podcasts is het eenvoudiger dan ooit om te zoeken naar goede opname advies en bronnen online.

Uw tolloket"registratie" moet een kleine zaal plaatsen zonder merkbare echo of 'Toon ruimte' en als stille en soundproof mogelijk. Gordijnen aan de muur kunnen worden gebruikt te verminderen echo en neutraliseren of "deaden" het geluid van de ruimte bevindt.

Gebruik een hoge kwaliteit studio koeler microfoon ('mic') die bestemd zijn voor het opnemen van spraak. Sennheiser AKG en zelfs nieuwere zoomen microfoons kunt goede resultaten opleveren. U kunt uw mic kopen of huurt van een lokale audiovisuele verhuur vast. Zoek naar een met een USB-interface. Dit type mic gemakkelijk de microfoon element, preamp en analoog naar digitaal converter gecombineerd in één pakket, verbinden te vereenvoudigen.

U kunt ook een analoge microfoon. Veel verhuur huizen bieden "Superieur" microfoons gerenommeerde voor hun stem-teken. Houd er rekening mee dat professionele analoog versnelling met gelijke taakverdeling XLR-connectors gebruikt, in plaats van de 1/4" plug-in consumentenapparatuur gebruikt. Wanneer u analoog gaat, moet u ook een preamp en een computer audio-interface met deze connectors.

De microfoon op een zelfstandige of tijdperk van installeren en gebruiken van een pop-filter voor de microfoon te elimineren ruis van "plosive" klinkt "p" en "b". Sommige microfoons worden geleverd met een onderbreking koppeling waarmee ze worden geïsoleerd van trillingen in de zelfstandige, wat nuttig is.

De spraak-talent moet blijven op een consistente afstand van de microfoon. Tape markeren waar ze moeten staan op de verdieping gebruiken. Als de talent liever bevinden zich, erop letten mic afstand bewaken en te voorkomen dat stoel ruis.

Gebruik een zelfstandige voor het opslaan van het script. Vermijd de zelfstandige sportvisserij zodat geluid richting van de microfoon kan worden weergegeven.

De persoon die daadwerkelijk de opname, de engineer, moeten zich in een afzonderlijk lokaal van de talent, met een manier om te communiceren met de talent in de opname tolloket (een "talkback-circuit').

De opname moet bevatten als weinig ruis mogelijk, met als doel van een 80 db signaal ruis verhouding of hoger.

Luister goed naar een opname van stilte in uw "tolloket,' afbeelding waar elke ruis afkomstig is van en de oorzaak te elimineren. Meest voorkomende oorzaken van de ruis zijn ventilatoren, TL lichte ballasten, verkeer op in de buurt wegen en apparatuur fans (zelfs laptops hebben ventilatoren). Microfoons en kabels pikken elektrische ruis uit in de buurt AC-bekabeling, meestal een verwijdert of buzz.

> [!TIP]
> In sommige gevallen is het mogelijk een equalizer of een invoegtoepassing ruis vermindering software gebruiken om u te helpen ruis verwijderen uit uw opnamen, maar het is altijd verstandig om te voorkomen dat deze bij de bron indien mogelijk.

Niveaus moeten worden ingesteld, zodat die het beste van de beschikbare dynamisch bereik van digitale registratie zonder overdriving wordt gebruikt. Dit betekent dat mogelijk, maar niet dus mogelijk dat de audio geen juist beeld geeft. Hieronder volgt een voorbeeld van de stroom van een goede opname.

![goede opname golf](media/custom-voice/good-recording.png)

U kunt zien dat de meeste van het bereik (hoogte) wordt gebruikt, maar de hoogste pieken van het signaal boven of onder aan de weergave niet bereiken. U ziet ook dat de stilte in de registratie een thin horizontale lijn benadert die wijzen op een verdieping lage ruis. Deze registratie heeft acceptabele dynamisch bereik en signaal ruis verhouding.

Neem rechtstreeks op de computer met een goede audio-interface of een USB-poort, afhankelijk van het soort mic u gebruikt. Houd de audio keten eenvoudig: mic, preamp, audio-interface, computer. Beide [Avid Pro Tools](http://www.avid.com/en/pro-tools) en [Adobe Audition](https://www.adobe.com/products/audition.html) maandelijks redelijke kosten kunnen worden gelicentieerd. Als uw budget zeer krachtige is, probeert u het gratis [Audacity](https://www.audacityteam.org/).

Vastleggen met een 44,1 KHz 16-bits mono (kwaliteit CD) of hoger. Huidige status-of-the-art is 48 KHz 24-bits, als uw apparatuur wordt ondersteund. U wordt verkleinen tot 16 KHz 16-bits de audio voordat u deze bij de portal voor aangepaste spraak indient. Het loont nog steeds, hebben een hoge kwaliteit oorspronkelijke op te nemen in het geval van wijzigingen nodig zijn.

In het ideale geval hebben verschillende mensen in de rollen van director, engineering en talent vervullen. Probeer niet om dit te doen alle zelf! In een knijpen, kunnen de directeur en engineer één persoon zijn.

### <a name="before-the-session"></a>Voordat de sessie

Om te voorkomen dat ze studio tijd heeft verspild, doorloopt u het script met uw stem talent voordat de opnamesessie. Als de stem-talent bekend zijn met de tekst verandert, kan hij of zij de uitspraak van woorden die niet bekend verduidelijken.

> [!NOTE]
> De meeste opname studios bieden elektronische weergave van scripts in de stand opnemen. In dit geval, typ de opmerkingen bij de uitzending rechtstreeks in document van het script. U nog steeds wilt papier aantekeningen op tijdens de sessie, maar. De meeste engineers wilt afdrukken, te. En zult u nog steeds dat het derde exemplaar als een back-up voor de talent afgedrukt als de computer niet actief is.

Uw stem talent kan vragen die u wilt dat word FDA in een utterance. Actoren aanroepen dit vaak de 'kracht woord'. Ze vertellen dat u wilt dat een natuurlijke lezen met geen nadruk. Nadruk kan worden toegevoegd wanneer spraak is gemaakt; het mag geen onderdeel van de oorspronkelijke opname.

De talent moet worden uitgesproken woorden wachtwoorddelen afzonderlijk sturen. Elk woord in het script moet worden verergerd geschreven. Geluiden moeten niet worden weggelaten of samen komt veel voor bij alledaagse spraak, slurred *, tenzij ze op die manier zijn geschreven in het script.*

|Geschreven tekst|Uitspraak van ongewenste informeel|
|-|-|
|nooit gaan op te geven u|nooit gaan op te geven u|
|Er zijn vier verlichting|Er zijn vier verlichting|
|hoe wordt het weer vandaag|Wat is het e-weerbericht van vandaag|
|Maak kennis met mijn weinig vriend|zegt Hallo naar Mijn lil' vriend|

Talent moet *niet* afzonderlijke pauzes tussen woorden toevoegen. De zin moet nog steeds op een natuurlijke manier, zelfs terwijl een beetje formele hoorbaar stromen. Dit goed onderscheid kan duren voordat sommige praktijk meteen.

### <a name="the-recording-session"></a>De opnamesessie

Maak een verwijzing op te nemen, of *overeenkomst bestand* van een typische utterance aan het begin van de sessie. Vraag de talent deze regel herhalen elke pagina of pagina en een halve. Telkens wanneer de nieuwe opname Reference vergelijken. Deze techniek helpt de talent in volume, tempo, inspiratie en intonation consistent blijven. De engineer kunt het bestand overeenkomst in de tussentijd zorgen, gebruiken als uitgangspunt voor niveaus en algehele consistentie van geluid.

Het bestand vergelijken is vooral belangrijk bij het vastleggen van gegevens hervatten na een onderbreking of op een andere dag. Moet u een paar keer voor de talent spelen en laten herhalen elke keer totdat ze zijn ook overeenkomen.

Uw talent een grondige adem en wacht een ogenblik voordat elke utterance coachen. Een paar seconden stilte tussen uitingen opnemen. Woorden die moeten worden verergerd dezelfde manier telkens wanneer ze worden weergegeven, rekening houdend met context: 'record' als een bewerking anders van 'record' als een zelfstandig naamwoord uitgesproken is.

Registreren van een goede vijf seconden stilte voordat de eerste registratie voor het vastleggen van de 'ruimte toon." Dit helpt de compenseren voor eventuele resterende ruis in de opnamen aangepaste spraak-portal.

> [!TIP]
> Alles wat u echt nodig om vast te leggen de talent stem is dus het is prima om een mono (één kanaal)-opname van alleen de regels maken. Als u in stereo registreert, kunt u het tweede kanaal gebruiken om vast te leggen van de chatter in de controlekamer. Vaak is het handig om te verwijzen naar dit later opnieuw. Dit nummer van de versie die is geüpload naar de portal voor aangepaste spraak verwijderen.

Luister goed, hoofdtelefoon, om de prestaties van de stem-talent te gebruiken. U zoekt naar goede maar natuurlijke diction, juiste uitspraak en een gebrek aan ongewenste geluiden. Kunt u vragen uw talent om vast te leggen opnieuw een utterance die niet voldoet aan deze standaarden. 

> [!TIP] 
> Bij het vastleggen van een groot aantal uitingen, verlies van een enkele utterance kan weinig invloed op de resulterende aangepaste spraak. Zo kan het zijn meer wenselijk is om te weten gewoon alle uitingen die problemen ondervindt bij en Zie hoe uw aangepaste gesproken blijkt uitsluiten uw gegevensset. U kunt altijd gaat u terug naar de studio en de gemiste voorbeelden later opnemen.

Noteer het getal of de code op uw script tijd voor elke utterance. Vraag de engineer als ze elke utterance in de opname van metagegevens of hint blad ook kunnen markeren.

Reguliere onderbrekingen voor uw stem talent zijn of haar spraak in goede staat houden, kunnen nemen. Geeft het talent iets drank om te voorkomen dat hun keel droge aan.

### <a name="after-the-session"></a>Na de sessie

Moderne opname studios uitgevoerd op computers. Aan het einde van de sessie ontvangt, u een of meer audio bestanden, niet een tape. Deze bestanden wordt waarschijnlijk WAV of AIFF-indeling in het CD-kwaliteit (44,1 KHz 16-bits) of hoger. 48 kHz 24-bits is gemeenschappelijk en wenselijk is. Hogere sampling-frequenties, zoals 96 KHz zijn doorgaans niet nodig.

De studio levert een of meer audio bestanden met meerdere uitingen. Als u wilt de opnamen uploaden naar de portal voor aangepaste spraak, moet elke utterance in een eigen bestand. De opname-technicus die mogelijk een markering in het bestand geplaatst (of een lijst met afzonderlijke hints opgegeven) om aan te geven waarbij elke utterance wordt gestart.

U moet de audio-opnamen doorlopen en een WAV-bestand voor elke utterance. Uw notities gebruiken om te vinden van de exacte uitingen die u wilt en gebruik een hulpprogramma zoals bewerken geluid [Avid Pro Tools](http://www.avid.com/en/pro-tools), [Adobe Audition](https://www.adobe.com/products/audition.html), of de gratis [Audacity](https://www.audacityteam.org/) kopiëren naar een nieuw bestand.

Laat u slechts circa 0,2 seconden stilte aan het begin en einde van elke clip, met uitzondering van de eerste. Dat bestand moet beginnen met een volledige vijf seconden stilte. Gebruik niet de audio-editor 'nul out' op de achtergrond delen van het bestand. Met inbegrip van de 'Toon ruimte', kunnen de aangepaste spraak algoritmen voor eventuele resterende achtergrondgeluiden compenseren.

Luister naar elk bestand zorgvuldig. In deze fase, kunt u bewerken van kleine ongewenste geluiden die u tijdens het opnemen, zoals een lichte lip smack vóór een regel hebt gemist, zolang deze spraak niet overlappen. Als u een bestand niet kan oplossen, kunt u deze in uw gegevensset, waardoor een Houd er rekening mee dat u hebt gedaan, verwijderen.

Verkleinen elk bestand naar 16 KHz en 16 bits voordat u opslaat en, als u hebt genoteerd in stereo, verwijdert u het tweede kanaal. Elk bestand opslaan in WAV-indeling.

Archiveren van de oorspronkelijke opname in een veilige plaats als u wilt terugkeren naar het later opnieuw. Uw script en notities, te behouden.

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw opnamen uploaden en maken van uw aangepaste gesproken!

> [!div class="nextstepaction"]
> [Het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md)
