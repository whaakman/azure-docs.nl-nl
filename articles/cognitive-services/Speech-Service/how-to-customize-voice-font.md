---
title: Wat is aangepaste spraak? -Azure Cognitive Services
description: In dit artikel overzichten Microsoft tekst naar spraak stem aanpassing, waarmee u kunt het maken van een stem te herkennen, één van een soort merk.
services: cognitive-services
author: noellelacharite
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: 042216d03749273c590ce5ba812f7a6d609f8e83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987900"
---
# <a name="creating-custom-voice-fonts"></a>Het maken van aangepaste spraakstijlen

Microsoft Text to Speech (Text-to-Speech) gesproken aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken: een *spraakstijl.* 

Voor het maken van uw spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt deze spraakstijl vervolgens gebruiken om na te bootsen spraak. 

U kunt aan de slag met een kleine hoeveelheid gegevens voor een concepttest. Maar hoe meer gegevens u hebt opgegeven, hoe meer natuurlijke en professional geluiden in uw stem.


## <a name="prerequisites"></a>Vereisten

De **tekst naar spraak** stem aanpassing functie is momenteel in private preview. [Vul het aanvraagformulier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) worden overwogen om toegang te krijgen.

U moet ook een Azure-account en een abonnement op de Speech-service. [Maak een](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) als u dat nog niet gedaan hebt. Uw abonnement als volgt verbinding te maken met de aangepaste spraak-portal.

1. Meld u aan bij de [aangepaste spraak portal](https://customvoice.ai) met hetzelfde Microsoft-account dat u gebruikt om toe te passen om toegang te krijgen.

2. Ga naar alle abonnementen onder de accountnaam in de rechterbovenhoek.

    ![Abonnementen](media/custom-voice/subscriptions.png)

3. Kies op de pagina 'Abonnementen', 'Verbinding maken met bestaande abonnement'.

4. Plak uw abonnementssleutel in de tabel, zoals hieronder wordt weergegeven. Elk abonnement heeft twee sleutels en u kunt een van beide.

     ![Abonnement toevoegen](media/custom-voice/add-subscription.png)

U bent klaar om te beginnen.

> [!IMPORTANT]
> In de beperkte preview-fase moet abonnementen in de whitelist opgenomen om de aangepaste spraak-functie te gebruiken. Volg de stappen op de pagina om uw abonnement in de whitelist opgenomen.

## <a name="prepare-recordings-and-transcripts"></a>Voorbereiden van de opnamen en transcripties

Een stem training-gegevensset bestaat uit een verzameling van audio-bestanden, samen met een tekstbestand met de Transcripten van alle audio-bestanden.

U kunt deze bestanden in beide richtingen voorbereiden: hetzij een script schrijven en deze gelezen door spraak talent, of gebruik openbaar beschikbare audio en deze moet transcriberen naar tekst. Bewerken in het laatste geval disfluencies uit de audio-bestanden, zoals 'um' en andere geluiden opvullende, haperen, mumbled woorden of mispronunciations.

Als u wilt een goede spraakstijl produceren, is het belangrijk dat de opnamen in een stille ruimte klaar bent met een hoge kwaliteit microfoon. Consistente volume, prijs, spreekstijl inspiratie en expressieve gebaren van spraak spreken zijn essentieel voor het bouwen van een geweldige digitale stem. Voor het maken van een stem voor gebruik in productieomgevingen, is het raadzaam dat een professionele opname studio en toon talent te gebruiken. Zie voor meer informatie, [vastleggen voice-voorbeelden voor een aangepaste spraak](record-custom-voice-samples.md).

### <a name="audio-files"></a>Audio-bestanden

Elke audiobestand moet een enkel utterance (bijvoorbeeld één zin of een enkel inschakelen van een systeem dialoogvenster) bevatten. Alle bestanden moeten zich in dezelfde taal (meertalige aangepaste stemmen worden niet ondersteund). De audio-bestanden moeten ook elk een unieke numerieke bestandsnaam hebben die zijn aangebracht met de bestandsextensie `.wav`.

Audio-bestanden moeten als volgt worden voorbereid. Andere indelingen worden niet ondersteund en worden geweigerd.

| **Eigenschap** | **Waarde** |
| ------------ | --------- |
| Bestandsindeling  | RIFF (WAV)|
| Samplefrequentie| ten minste 16.000 Hz |
| Sample-indeling| PCM, 16-bits |
| Bestandsnaam    | Numerieke met `.wav` extensie |
| Archiefindeling| Zip      |
| Maximale grootte archief|200 MB|

> [!NOTE]
> Bestanden met een lager is dan 16.000 Hz samplefrequentie Wave worden geweigerd. In het geval waarbij een zip-bestand blokken met verschillende tarieven bevat, worden alleen die gelijk is aan of hoger is dan 16.000 Hz geïmporteerd.
> De portal importeert momenteel ZIP-archieven 200 MB. Meerdere archieven kunnen echter worden geüpload. Het maximum aantal toegestane gegevenssets is dat 10 ZIP bestanden gratis gebruikers en 50 voor abonnementsgebruikers van de standard.

### <a name="transcripts"></a>Transcripten

Het bestand transcriptie is een bestand met tekst zonder opmaak (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE). Elke regel van het bestand transcriptie moet de naam van een geluidsbestand, gevolgd door een tab (codepunt 9) en ten slotte het transcript hebben. Er is geen lege regels zijn toegestaan.

Bijvoorbeeld:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Het systeem aangepaste gesproken normaliseert Transcripten door de tekst converteren naar kleine letters en te verwijderen van overbodige leestekens. Het is belangrijk dat de Transcripten 100% nauwkeurig tot op het bijbehorende audio-opnamen zijn.

> [!TIP]
> Wanneer het maken van productie tekst naar spraak stemmen, selecteer uitingen (of scripts schrijven) overweegt zowel fonetische dekking en efficiëntie. Problemen met het ophalen van de resultaten wilt u dat? [Neem contact op met het team voor aangepaste spraak](mailto:tts@microsoft.com) out meer over ons een Raadpleeg vinden.

## <a name="upload-your-datasets"></a>Uw gegevenssets uploaden

Na het voorbereiden van uw audio-bestand archiveren en transcripties, upload deze via de [aangepaste spraak-serviceportal](https://customvoice.ai).

> [!NOTE]
> Gegevenssets kan niet worden bewerkt nadat ze zijn geüpload. Als u bent vergeten Transcripten van enkele van de audio-bestanden bevatten, bijvoorbeeld of per ongeluk de verkeerde geslacht kiest, moet u de volledige gegevensset opnieuw uploaden. Controleer uw gegevensset en instellingen zorgvuldig voordat u begint met het uploaden.

1. Meld u aan bij de portal.

2. Kies **gegevens** onder aangepaste spraak op de hoofdpagina. 

    De tabel mijn Voice-gegevens wordt weergegeven. Het is leeg als u hebt geen gegevenssets stem nog niet geüpload.

3. Klik op **gegevens importeren** om de pagina voor het uploaden van een nieuwe gegevensset te openen.

    ![Voice-gegevens importeren](media/custom-voice/import-voice-data.png)

4. Voer een naam en beschrijving in de opgegeven velden. 

5. Selecteer de landinstelling voor uw spraakstijlen. Zorg ervoor dat de informatie over landinstellingen overeenkomt met de taal van de gegevens opnemen en de scripts. 

6. Selecteer het geslacht van de spreker waarvan u gebruikmaakt van spraak.

7. Kies de script- en audio-bestanden te uploaden. 

8. Klik op **importeren** om uw gegevens te uploaden. Voor grotere gegevenssets kan importeren enkele minuten duren.

> [!NOTE]
> Gratis gebruikers kunnen twee gegevenssets tegelijk uploaden. Abonnementsgebruikers van de Standard-kunnen vijf gegevenssets gelijktijdig uploaden. Als u de limiet is bereikt, wacht tot ten minste één van uw gegevenssets is geïmporteerd, probeer het vervolgens opnieuw.

Wanneer het uploaden voltooid is, wordt de tabel mijn Voice-gegevens opnieuw weergegeven. U ziet een vermelding die overeenkomt met uw gegevensset zojuist hebt geüpload.

Gegevenssets worden automatisch gevalideerd nadat het uploaden. De gegevensvalidatie bevat een reeks controles op de audio-bestanden om te controleren of de bestandsindeling, de grootte en de samplingfrequentie. Controles op de bestanden transcriptie controleren of de bestandsindeling en uitvoeren van sommige normalisering tekst. De uitingen worden omgezet met behulp van spraakherkenning en de resulterende tekst wordt vergeleken met het transcript die u hebt opgegeven.

![Mijn Voice-gegevens](media/custom-voice/my-voice-data.png)

De volgende tabel ziet u de verwerking van statussen voor geïmporteerde gegevenssets. 

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw gegevensset is ontvangen en in de wachtrij is geplaatst voor verwerking
| `Running` | Uw gegevensset wordt gevalideerd
| `Succeeded` | Uw gegevensset is gevalideerd en kan nu worden gebruikt voor het bouwen van een spraakstijl

Nadat de validatie is voltooid, ziet u het totale aantal overeenkomende uitingen voor elk van uw gegevenssets in de kolom Utterance.

U kunt een rapport om te controleren of de uitspraak van scores en het niveau van de ruis voor elk van de opnamen downloaden. Uitspraak van score varieert van 0 tot 100; een score lager 70 geeft doorgaans aan dat de spraak-fout of script niet overeenkomen. Een zware accent vermindert uw score uitspraak en van invloed zijn op de gegenereerde digitale stem.

Een hogere signaal ruis snelheid (signaal) geeft aan dat lagere ruis in uw audio. Doorgaans kunt u een signaal meer dan 50 bereiken door registratie via professionele studios. Audio met een signaal hieronder 20 kan resulteren in voor de hand liggende ruis in uw gegenereerde stem.

Houd rekening met alle uitingen met lage uitspraak van scores of slechte signaal ruis ratio's opnieuw op te nemen. Als niet is het mogelijk om opnieuw op te nemen, kunt u die uitingen uitsluiten van uw gegevensset.

## <a name="build-your-voice-font"></a>Bouw uw spraakstijl

Als uw gegevensset is gevalideerd, kunt u deze kunt gebruiken voor het bouwen van uw aangepaste spraakstijl. 

1. Kies **modellen** in de vervolgkeuzelijst 'Aangepaste spraak'. 
 
    De tabel mijn Spraakstijlen wordt weergegeven, met een aangepaste spraakstijlen die u al hebt gemaakt.

1. Klik op **stemmen maken** onder de tabeltitel. 

    De pagina voor het maken van een spraakstijl wordt weergegeven. De huidige landinstelling wordt weergegeven in de eerste rij van de tabel. Wijzig de landinstellingen voor het maken van een stem in een andere taal. De landinstelling moet gelijk zijn aan de gegevenssets die worden gebruikt voor het maken van de stem.

1. Zoals u deed toen u uw gegevensset hebt geüpload, voer een naam en beschrijving om te identificeren van dit model. 

    De naam die u hier opgeeft, worden de naam die u kunt de toon in uw aanvraag voor spraaksynthese opgeven als onderdeel van de invoer SSML, kiest u dus zorgvuldig. Alleen letters, cijfers en enkele leestekens zoals '-', '_' '(',')' zijn toegestaan.

    Een algemene gebruik van het veld Beschrijving is om vast te leggen van de namen van de gegevenssets die zijn gebruikt voor het maken van het model.

1. Kies het geslacht van uw spraakstijl. Dit moet overeenkomen met het geslacht van de gegevensset.

1. Selecteer de gegevensset (s) die u gebruiken wilt voor het trainen van het lettertype van spraak. Alle gegevenssets die worden gebruikt, moeten op de dezelfde spreker.

1. Klik op **maken** om te beginnen met het maken van uw spraakstijl.

    ![Model maken](media/custom-voice/create-model.png)

Het nieuwe model wordt weergegeven in de tabel mijn Spraakstijlen. 

![Mijn Spraakstijlen](media/custom-voice/my-voice-fonts.png)

De weergegeven status geeft het proces van het converteren van uw gegevensset naar een spraakstijl, zoals hier wordt weergegeven.

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw aanvraag voor het maken van spraak lettertype is in de wachtrij geplaatst voor verwerking
| `Running` | Uw spraakstijl wordt gemaakt
| `Succeeded` | Uw spraakstijl is gemaakt en kan worden geïmplementeerd

Tijd training, is afhankelijk van het volume van audiogegevens verwerkt. Typische tijden variëren van informatie over-30 minuten voor honderden uitingen tot 20.000 uitingen 40 uur.

> [!NOTE]
> Gratis gebruikers kunnen één spraakstijl trainen op een tijdstip. Abonnementsgebruikers van de Standard-kunnen drie stemmen tegelijkertijd trainen. Als u de limiet is bereikt, wacht totdat ten minste één van uw spraakstijlen training is voltooid en probeer het opnieuw.

## <a name="test-your-voice-font"></a>Test uw spraakstijl

Als uw spraakstijl met succes is gemaakt, kunt u deze testen voordat u deze implementeert voor gebruik. Klik op **Test** in de Operations-kolom van de tabel mijn Spraakstijlen. De testpagina wordt weergegeven voor de geselecteerde spraakstijl. De tabel is leeg als u nog een test-aanvragen voor de stem nog niet hebt verzonden.

Klik op **Test met tekst** knop onder de tabeltitel om een pop-upmenu voor het indienen van aanvragen van de tekst weer te geven. U kunt uw testaanvraag in tekst zonder opmaak of SSML indienen. De maximale grootte van de invoer is 1024 tekens, inclusief alle tags voor SSML aanvraag. De taal van de tekst moet hetzelfde zijn als de taal van uw spraakstijl.

Na het invullen van het tekstvak en waaruit blijkt dat de invoer-modus, klikt u op **Ja** uw testaanvraag indienen en gaat u terug naar de testpagina. De tabel bevat nu een vermelding die overeenkomt met uw nieuwe aanvraag en de kolom nu bekende status. Het kan enkele minuten om na te bootsen spraak duren. Wanneer de statuskolom is voltooid gelezen, kunt u de tekstinvoer downloaden (een `.txt` bestand) en audio-uitvoer (een `.wav` bestand) en de laatste voor kwaliteit beluisteren.

## <a name="create-and-use-a-custom-endpoint"></a>Maken en gebruiken van een aangepast eindpunt

Nadat u hebt gemaakt en het model van uw stem getest, kunt u deze implementeren in een aangepaste tekst-naar-spraak-eindpunt. Vervolgens gebruikt u dit eindpunt in plaats van het eindpunt van de gebruikelijke bij het maken van tekst naar spraak aanvragen via de REST-API. Uw aangepaste eindpunt kan worden aangeroepen door het abonnement dat u gebruikt voor het implementeren van het lettertype.

Kies voor het maken van een nieuw aangepast eindpunt **eindpunten** in het menu voor aangepaste gesproken aan de bovenkant van de pagina. De pagina Mijn geïmplementeerd stemmen wordt weergegeven, met de tabel van de huidige eindpunten voor aangepaste spraak, indien van toepassing. De huidige landinstelling is doorgevoerd in de eerste rij van de tabel. Wijzig de landinstelling van het weergegeven voor het maken van een implementatie voor een andere taal. (Deze moet overeenkomen met de stem die u implementeert.)

Klik op de **implementeren stemmen** knop om een nieuwe eindpunt te maken. Voer de naam en beschrijving van uw aangepast eindpunt.

Kies het abonnement dat u wilt gebruiken in het menu abonnement. Gratis abonnementsgebruikers kunnen slechts één model geïmplementeerd op een moment hebben. Abonnementsgebruikers van de Standard-kunnen maximaal 20 eindpunten, elk met een eigen aangepaste gesproken maken.

![Eindpunt maken](media/custom-voice/create-endpoint.png)

Selecteer het model om te worden geïmplementeerd en klik op **maken**. De pagina Mijn geïmplementeerd stemmen opnieuw wordt weergegeven, nu met een vermelding voor het nieuwe eindpunt voor. Het duurt een paar minuten voor het starten van een nieuw eindpunt. Wanneer de status van de implementatie is geslaagd, wordt het eindpunt is klaar voor gebruik.

![Mijn geïmplementeerde stemmen](media/custom-voice/my-deployed-voices.png)

Wanneer de status van de implementatie is geslaagd, wordt het eindpunt van uw geïmplementeerde spraakstijl weergegeven in de tabel mijn stemmen geïmplementeerd. U kunt deze URI gebruiken rechtstreeks in een HTTP-aanvraag.

Online testen van het eindpunt is ook beschikbaar via de portal voor aangepaste gesproken. Als u wilt testen op uw eindpunt, kies **eindpunten testen** uit de vervolgkeuzelijst van aangepaste spraak. Het eindpunt testen van de pagina wordt weergegeven. Kies een geïmplementeerde aangepaste gesproken en voer de tekst die moet worden uitgesproken (in tekst zonder opmaak of SSML-indeling) in het tekstvak in.

> [!NOTE] 
> Bij het gebruik van SSML, de `<voice>` tag geeft de naam die u uw aangepaste gesproken hebt opgegeven toen u het maakte. Als u tekst zonder opmaak hebt ingediend, wordt de aangepaste gesproken altijd gebruikt.

Klik op **afspelen** graag de tekst die in uw aangepaste spraakstijl gesproken.

![Eindpunt testen](media/custom-voice/endpoint-testing.png)

Het eindpunt van de aangepaste is identiek aan de standaard-eindpunt gebruikt voor het aanvragen van tekst naar spraak. Zie [REST-API](rest-apis.md) voor meer informatie.

## <a name="language-support"></a>Taalondersteuning

Stem aanpassing is beschikbaar voor Amerikaans Engels (en-US), vasteland Chinees (zh-CN) en Italiaans (it-IT).

> [!NOTE]
> Trainingen voor de Italiaanse toon begint met een gegevensset van meer dan 2000 uitingen. Chinees-Engels tweetalige modellen worden ook ondersteund met een gegevensset van meer dan 2000 uitingen.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Water in C#](quickstart-csharp-dotnet-windows.md)
