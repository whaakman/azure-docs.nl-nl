---
title: Over het maken van een aangepaste spraakstijl
titlesuffix: Azure Cognitive Services
description: In dit artikel wordt een overzicht van tekst naar spraak stem aanpassing, waarmee u een stem te herkennen, één van een soort merk maken.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: panosper
ms.openlocfilehash: e2c176e35cbc75747230e429d0ddae9d420db8b5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867577"
---
# <a name="creating-custom-voice-fonts"></a>Het maken van aangepaste spraakstijlen

Spraak gesproken aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken: een *spraakstijl.* 

Voor het maken van uw spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt deze spraakstijl gebruiken om na te bootsen spraak. 

U kunt aan de slag met een kleine hoeveelheid gegevens voor een concepttest. Maar hoe meer gegevens u hebt opgegeven, hoe meer natuurlijke en professional geluiden in uw stem.

## <a name="prerequisites"></a>Vereisten

U moet een Azure-account en een abonnement met de spraak-service. [Maak een](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) als u dat nog niet gedaan hebt. Verbinding maken met uw abonnement op de portal voor aangepaste spraak zoals hier wordt weergegeven.

1. Aanmelden bij de [aangepaste spraak portal](https://customvoice.ai) met behulp van het Microsoft-account die u hebt gebruikt om toe te passen om toegang te krijgen.

2. Onder de naam van uw account in de rechterbovenhoek, gaat u naar **abonnementen**.

    ![Abonnementen](media/custom-voice/subscriptions.png)

3. Kies op de pagina abonnementen **verbinding maken met bestaande abonnement**. Houd er rekening mee dat Speech Services biedt ondersteuning voor verschillende regio's. De regio waar de abonnementssleutel van uw is gemaakt en zorg ervoor dat u uw sleutel verbinding met de juiste onderliggende portal maken controleren.  

4. Plak uw abonnementssleutel in de tabel, zoals wordt weergegeven in het volgende voorbeeld. Elk abonnement heeft twee sleutels, en u kunt een van beide.

     ![Abonnement toevoegen](media/custom-voice/add-subscription.png)

U bent klaar om te beginnen.

## <a name="prepare-recordings-and-transcripts"></a>Voorbereiden van de opnamen en transcripties

Een stem training-gegevensset bestaat uit een verzameling van audio-bestanden, samen met een tekstbestand met de Transcripten van de audio-bestanden.

U kunt deze bestanden op twee manieren voorbereiden. Ofwel een script schrijven en deze gelezen door spraak talent of openbaar beschikbare audio gebruiken en deze moet transcriberen naar tekst. Als u de laatste, bewerk disfluencies uit de audio-bestanden, zoals 'um' en andere geluiden opvullende, haperen, mumbled woorden of mispronunciations.

Zorg voor een goede spraakstijl produceren, de opnamen in een stille ruimte met een hoge kwaliteit microfoon. Consistente volume, prijs, spreekstijl inspiratie en expressieve gebaren van spraak spreken zijn essentieel voor het bouwen van een geweldige digitale stem. 

Voor het maken van een stem voor gebruik in productieomgevingen, is het raadzaam dat een professionele opname studio en toon talent te gebruiken. Zie voor meer informatie, [vastleggen voice-voorbeelden voor een aangepaste spraak](record-custom-voice-samples.md).

### <a name="audio-files"></a>Audio-bestanden

Elke audiobestand moet een enkel utterance (bijvoorbeeld één zin of een enkel inschakelen van een systeem dialoogvenster) bevatten. Alle bestanden moeten zich in dezelfde taal. (Meertalige aangepaste stemmen worden niet ondersteund.) De audio-bestanden ook elk moet een unieke numerieke bestandsnaam met de bestandsextensie `.wav`.

Audio-bestanden moeten als volgt worden voorbereid. Andere indelingen worden niet ondersteund en worden geweigerd.

| **Eigenschap** | **Waarde** |
| ------------ | --------- |
| Bestandsindeling  | RIFF (.wav)|
| Samplefrequentie| ten minste 16.000 Hz |
| Voorbeeldindeling| PCM, 16-bits |
| Bestandsnaam    | Numerieke met `.wav` extensie |
| Indeling archiveren| .zip      |
| Grootte van maximaal archiveren|200 MB|

> [!NOTE]
> wav-bestanden met een lager is dan 16.000 Hz samplefrequentie worden geweigerd. Als een ZIP-bestand blokken met verschillende tarieven bevat, worden alleen die gelijk is aan of hoger is dan 16.000 Hz wordt geïmporteerd.
> De portal momenteel invoer ZIP-archieven 200 MB. Meerdere archieven kunnen echter worden geüpload. Het maximum aantal toegestane gegevenssets is 10 .zip bestanden gratis gebruikers en 50 voor abonnementsgebruikers van de standard.

### <a name="transcripts"></a>Transcripten

Het bestand transcriptie is een tekst zonder opmaak (ANSI, UTF-8, UTF-8-BOM, UTF-16-LE of UTF-16-BE). Elke regel van het bestand transcriptie moet de naam van een geluidsbestand, gevolgd door een tab (codepunt 9) en ten slotte het transcript hebben. Er is geen lege regels zijn toegestaan.

Bijvoorbeeld:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Het systeem aangepaste gesproken normaliseert Transcripten door de tekst converteren naar kleine letters en verwijder overbodige leestekens. Het is belangrijk dat de Transcripten nauwkeurige transcripties 100% van de bijbehorende audio-opnamen zijn.

> [!TIP]
> Wanneer rekening nemen in het bouwen van productie tekst naar spraak stemmen, selecteer uitingen (of scripts schrijven) die zowel fonetische dekking en efficiëntie. Problemen met het ophalen van de resultaten wilt u dat? [Neem contact op met het team voor aangepaste spraak](mailto:speechsupport@microsoft.com) out meer over ons een Raadpleeg vinden.

## <a name="upload-your-datasets"></a>Uw gegevenssets uploaden

Nadat u uw audio-bestand archiveren en transcripties hebt voorbereid, uploaden via de [aangepaste spraak-serviceportal](https://customvoice.ai).

> [!NOTE]
> Gegevenssets kan niet worden bewerkt nadat ze zijn geüpload. Als u vergeet Transcripten van enkele van de audio-bestanden bevatten, bijvoorbeeld of per ongeluk de verkeerde geslacht kiest, moet u de volledige gegevensset opnieuw uploaden. Controleer uw gegevensset en instellingen zorgvuldig voordat u begint met het uploaden.

1. Meld u aan bij de portal.

2. Op de hoofdpagina onder **aangepaste spraak**, selecteer **gegevens**.   

    De **mijn stem** tabel wordt weergegeven. Het is leeg als u dit nog niet hebt nog geen voice-gegevenssets geüpload.

3. Open de pagina voor het uploaden van een nieuwe gegevensset, selecteert u **gegevens importeren**. 

    ![Voice-gegevens importeren](media/custom-voice/import-voice-data.png)

4. Voer een naam en beschrijving in de velden die worden geleverd. 

5. Selecteer de landinstelling voor uw spraakstijlen. Zorg ervoor dat de informatie over landinstellingen overeenkomt met de taal van de gegevens opnemen en de scripts. 

6. Selecteer het geslacht van de spreker waarvan u stem.

7. Selecteer de script- en audio-bestanden te uploaden. 

8. Selecteer **importeren** om uw gegevens te uploaden. Voor grotere gegevenssets kan importeren enkele minuten duren.

> [!NOTE]
> Gratis gebruikers kunnen twee gegevenssets tegelijk uploaden. Abonnementsgebruikers van de Standard-kunnen vijf gegevenssets gelijktijdig uploaden. Als u de limiet is bereikt, wacht u totdat ten minste één van uw gegevenssets geïmporteerd is. Probeer het opnieuw.

Wanneer het uploaden voltooid is, de **mijn spraakgegevens** tabel wordt opnieuw weergegeven. Hier ziet u een vermelding die overeenkomt met de gegevensset die u net hebt geüpload. 

Gegevenssets worden automatisch gevalideerd nadat het uploaden. De gegevensvalidatie bevat een reeks controles op de audio-bestanden om te controleren of de bestandsindeling, de grootte en de samplingfrequentie. Controles op de bestanden transcriptie controleren of de bestandsindeling en sommige normalisering tekst doen. De uitingen worden omgezet met behulp van spraakherkenning. Vervolgens wordt de resulterende tekst vergeleken met het transcript die u hebt opgegeven.

![Mijn Voice-gegevens](media/custom-voice/my-voice-data.png)

De volgende tabel ziet u de verwerking van statussen voor geïmporteerde gegevenssets: 

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw gegevensset is ontvangen en in de wachtrij is geplaatst voor verwerking.
| `Running` | Uw gegevensset wordt gevalideerd.
| `Succeeded` | Uw gegevensset is gevalideerd en kan nu worden gebruikt om een spraakstijl bouwen.

Nadat de validatie is voltooid, ziet u het totale aantal overeenkomende uitingen voor elk van uw gegevenssets in de **Utterance** kolom.

U kunt een rapport om te controleren of de uitspraak van scores en het niveau van de ruis voor elk van de opnamen downloaden. De uitspraak van score varieert van 0 tot 100. Een score lager 70 geeft doorgaans aan dat de spraak-fout of script niet overeenkomen. Een zware accent vermindert uw score uitspraak en van invloed zijn op de gegenereerde digitale stem.

Een hogere signaal ruis snelheid (signaal) geeft aan dat lagere ruis in uw audio. Doorgaans kunt u een signaal meer dan 50 bereiken door de opname op professionele studios. Audio met een signaal hieronder 20 kan resulteren in voor de hand liggende ruis in uw gegenereerde stem.

Houd rekening met alle uitingen met lage uitspraak van scores of slechte signaal ruis ratio's opnieuw op te nemen. Als u kan niet opnieuw registreert, kunt u die uitingen uitsluiten van uw gegevensset.

## <a name="build-your-voice-font"></a>Bouw uw spraakstijl

Nadat uw gegevensset is gevalideerd, kunt u deze kunt gebruiken voor het bouwen van uw aangepaste spraakstijl. 

1.  In de **aangepaste spraak** vervolgkeuzelijst, kiest u **modellen**.
 
    De **mijn Spraakstijlen** tabel wordt weergegeven, een aangepaste spraakstijlen aanbieding die u al hebt gemaakt.

1. Selecteer onder de tabeltitel, **stemmen maken**. 

    De pagina voor het maken van een spraakstijl wordt weergegeven. De huidige landinstelling wordt weergegeven in de eerste rij van de tabel. Wijzig de landinstellingen voor het maken van een stem in een andere taal. De landinstelling moet hetzelfde zijn als voor de gegevenssets die worden gebruikt voor het maken van de stem.

1. Zoals u deed toen u uw gegevensset hebt geüpload, voer een naam en beschrijving om te identificeren van dit model. 

    Kies een naam zorgvuldig. De naam die u hier opgeeft, worden de naam die u gebruikt om op te geven van de toon in uw aanvraag voor spraaksynthese als onderdeel van de SSML invoer. Alleen letters, cijfers en enkele leestekens zoals '-', '_' en '(',')' zijn toegestaan.

    Een algemene gebruik van de **beschrijving** veld, is het vastleggen van de namen van de gegevenssets die zijn gebruikt voor het maken van het model.

1. Kies het geslacht van uw spraakstijl. Dit moet overeenkomen met het geslacht van de gegevensset.

1. Selecteer de gegevensset (s) die u gebruiken wilt voor het trainen van het lettertype van spraak. Alle gegevenssets die u moet de dezelfde spreker.

1. Klik op **maken** om te beginnen met het maken van uw spraakstijl.

    ![Model maken](media/custom-voice/create-model.png)

Het nieuwe model wordt weergegeven in de **mijn Spraakstijlen** tabel. 

![Mijn Spraakstijlen](media/custom-voice/my-voice-fonts.png)

De status die wordt weergegeven, geeft het proces van het converteren van uw gegevensset naar een spraakstijl, zoals hier wordt weergegeven.

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw aanvraag voor het maken van spraak lettertype is in de wachtrij voor verwerking.
| `Running` | Uw spraakstijl wordt gemaakt.
| `Succeeded` | Uw spraakstijl is gemaakt en kan worden geïmplementeerd.

Tijd training, is afhankelijk van het volume van audiogegevens verwerkt. Typische tijden variëren van informatie over-30 minuten voor honderden uitingen tot 20.000 uitingen 40 uur.

> [!NOTE]
> Gratis gebruikers kunnen één spraakstijl trainen op een tijdstip. Abonnementsgebruikers van de Standard-kunnen drie stemmen tegelijkertijd trainen. Als u de limiet is bereikt, wacht totdat ten minste één van uw spraakstijlen training is voltooid en probeer het vervolgens opnieuw.

## <a name="test-your-voice-font"></a>Test uw spraakstijl

Nadat uw spraakstijl is is gebouwd, kunt u deze testen voordat u deze implementeert voor gebruik. In de **Operations** kolom, selecteer **Test**. De testpagina wordt weergegeven voor de geselecteerde spraakstijl. De tabel is leeg als u nog een test-aanvragen voor de stem nog niet hebt verzonden.

Als een pop-upmenu voor het indienen van aanvragen voor tekst weergeven, selecteert u de **Test met tekst** knop onder de tabeltitel. U kunt uw testaanvraag in tekst zonder opmaak of SSML indienen. De maximale grootte van de invoer is 1024 tekens, inclusief alle tags voor de aanvraag SSML. De taal van de tekst moet hetzelfde zijn als de taal van uw spraakstijl.

Na het invullen van het tekstvak en waaruit blijkt dat de invoer-modus, selecteer **Ja** uw testaanvraag indienen en gaat u terug naar de testpagina. De tabel bevat nu een vermelding die overeenkomt met uw nieuwe aanvraag en de statuskolom. Het kan enkele minuten om na te bootsen spraak duren. Als de statuskolom zegt **geslaagd**, kunt u de tekstinvoer downloaden (een `.txt` bestand) en audio-uitvoer (een `.wav` bestand), en de laatste voor kwaliteit beluisteren.

## <a name="create-and-use-a-custom-endpoint"></a>Maken en gebruiken van een aangepast eindpunt

Nadat u hebt met succes gemaakt en het model van uw stem getest, kunt u deze implementeren in een aangepast Text to Speech-eindpunt. Vervolgens gebruikt u dit eindpunt in plaats van het eindpunt van de gebruikelijke bij het maken van tekst naar spraak aanvragen via de REST-API. Uw aangepaste eindpunt kan worden aangeroepen door het abonnement dat u gebruikt voor het implementeren van het lettertype.

Kies voor het maken van een nieuw aangepast eindpunt **eindpunten** uit de **aangepaste gesproken** menu aan de bovenkant van de pagina. De **mijn geïmplementeerd stemmen** pagina wordt weergegeven, met de tabel van de huidige eindpunten voor aangepaste spraak, indien van toepassing. De huidige landinstelling is doorgevoerd in de eerste rij van de tabel. Wijzig de landinstelling van het weergegeven voor het maken van een implementatie voor een andere taal. (Deze moet overeenkomen met de stem die u implementeert.)

Voor het maken van een nieuw eindpunt selecteert de **implementeren stemmen** knop. Voer de naam en beschrijving van uw aangepast eindpunt.

Uit de **abonnement** menu, kies het abonnement dat u wilt gebruiken. Gratis abonnementsgebruikers kunnen slechts één model geïmplementeerd op een moment hebben. Abonnementsgebruikers van de Standard-kunnen maximaal 20 eindpunten, elk met een eigen aangepaste gesproken maken.

![Eindpunt maken](media/custom-voice/create-endpoint.png)

Na het selecteren van het model om te worden geïmplementeerd, selecteert u **maken**. De **mijn geïmplementeerd stemmen** pagina nu opnieuw weergegeven met een vermelding voor het nieuwe eindpunt voor. Het duurt een paar minuten voor het starten van een nieuw eindpunt. Wanneer de status van de implementatie is **geslaagd**, het eindpunt is gereed voor gebruik.

![Mijn geïmplementeerde stemmen](media/custom-voice/my-deployed-voices.png)

Wanneer de status van de implementatie is **geslaagd**, het eindpunt van uw geïmplementeerde spraakstijl wordt weergegeven in de **mijn geïmplementeerd stemmen** tabel. U kunt deze URI gebruiken rechtstreeks in een HTTP-aanvraag.

Online testen van het eindpunt is ook beschikbaar via de portal voor aangepaste gesproken. Als u wilt testen op uw eindpunt, kies **eindpunten testen** uit de **aangepaste spraak** vervolgkeuzelijst. Het eindpunt testen van de pagina wordt weergegeven. Kies een geïmplementeerde aangepaste gesproken en voer de tekst die moet worden uitgesproken (in tekst zonder opmaak of SSML-indeling) in het tekstvak in.

> [!NOTE] 
> Bij het gebruik van SSML, de `<voice>` tag geeft de naam die u uw aangepaste gesproken hebt gegeven toen u het maakte. Als u tekst zonder opmaak hebt ingediend, wordt de aangepaste gesproken altijd gebruikt.

Als u wilt de tekst die in uw aangepaste spraakstijl gesproken horen, selecteer **afspelen**.

![Eindpunt testen](media/custom-voice/endpoint-testing.png)

Het eindpunt van de aangepaste is identiek aan de standard-eindpunt dat wordt gebruikt voor de Text to Speech-aanvragen. Zie [REST-API](rest-apis.md) voor meer informatie.

## <a name="language-support"></a>Taalondersteuning

Stem aanpassing is beschikbaar voor Amerikaans Engels (en-US), vasteland Chinees (zh-CN) en Italiaans (it-IT).

> [!NOTE]
> Trainingen voor de Italiaanse toon begint met een gegevensset van meer dan 2000 uitingen. Chinees-Engels tweetalige modellen worden ook ondersteund met een gegevensset van meer dan 2000 uitingen.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Registreren van uw stem-voorbeelden](record-custom-voice-samples.md)
