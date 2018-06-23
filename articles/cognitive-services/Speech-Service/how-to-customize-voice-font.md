---
title: Wat is aangepast stem? -Azure-cognitieve Services | Microsoft Docs
description: Dit artikel overzichten Microsoft Text to Speech stem aanpassing, waarmee u een stem te herkennen, een van een soort merk maken.
services: cognitive-services
author: noellelacharite
manager: nolach
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: ad5af799fd46dc51b85432999f986de8cdb056ec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345645"
---
# <a name="creating-custom-voice-fonts"></a>Maken van aangepaste gesproken lettertypen

Microsoft Text to Speech (TTS) stem aanpassing kunt u een stem te herkennen, een van een type voor uw huisstijl maken: een *voice-lettertype.* 

Voor het maken van uw stem lettertype een opname studio maken en de bijbehorende scripts als de trainingsgegevens uploaden. De service maakt vervolgens een unieke stem model dat is afgestemd op uw opname. U kunt dit lettertype stem vervolgens gebruiken om na te bootsen spraak. 

U kunt aan de slag met een kleine hoeveelheid gegevens voor het testen van het concept. Maar hoe meer gegevens u opgeeft, hoe meer natuurlijke en professional uw stem klinkt.

Voice-aanpassing is beschikbaar voor Amerikaans Engels (en-US) en via Chinees (zh-CN).

## <a name="prerequisites"></a>Vereisten

De functie Text to Speech stem aanpassing wordt momenteel private preview. [Vul het formulier toepassing](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) worden overwogen om toegang te krijgen.

U hebt ook het volgende nodig:

* Een Azure-account ([gratis aanmelden](https://azure.microsoft.com/free/ai/) als u er nog geen hebt).

* Een abonnement op de service spraak. [Maak een](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) als u dat nog niet gedaan hebt.

    ![Deelvenster maken](media/custom-voice/create-panel.png)

Nadat uw abonnement is gemaakt, vindt u twee abonnement sleutels in het deelvenster Quick Start of het overzicht van het nieuwe abonnement. U kunt de sleutel.

Ten slotte verbinding maken met uw abonnement bij de portal voor aangepaste gesproken als volgt.

1. Meld u aan bij de [aangepaste gesproken portal](https://customvoice.ai) met hetzelfde Microsoft-account dat u gebruikt om te passen om toegang te krijgen.

2. Ga naar 'Abonnementen' onder de accountnaam van uw op de rechterbovenhoek.

    ![Abonnementen](media/custom-voice/subscriptions.png)

3. Kies op de pagina 'Abonnementen', 'Verbinding maken met bestaande subscription'.

     ![Verbinding maken met bestaande abonnement](media/custom-voice/connect-existing-sub.png)

4. Plak de abonnementssleutel van uw in de tabel, zoals hieronder wordt weergegeven.

     ![Abonnement toevoegen](media/custom-voice/add-subscription.png)

U bent nu klaar voor!

## <a name="prepare-recordings-and-transcripts"></a>Opnamen en transcripties voorbereiden

Een stem training gegevensset bestaat uit een reeks audio-bestanden, samen met een tekstbestand met de transcripties van al deze audio-bestanden.

U kunt deze bestanden in beide richtingen voorbereiden: ofwel een script schrijven en laat het gelezen door stem talent, of gebruik openbaar beschikbaar audio en ze transcriberen met gebruikmaking naar tekst. Bewerk in het laatste geval disfluencies uit de audio-bestanden, zoals 'um' en andere opvulteken geluiden, haperen, mumbled woorden of mispronunciations.

Een goede voice-lettertype produceren, is het belangrijk dat de opnamen in een stille lokaal worden uitgevoerd met een microfoon van hoge kwaliteit. Consistente volume spreken frequentie, spreken presentatie en expressieve gebaren van spraak zijn essentieel voor het bouwen van een geweldige digitale stem. U wordt aangeraden dat u een professionele opname studio en stem talent gebruiken voor het maken van een stem voor gebruik in productieomgevingen.

### <a name="audio-files"></a>Audio-bestanden

Elke audiobestand moet een enkel utterance (bijvoorbeeld één zin of een enkel inschakelen van een systeem dialoogvenster) bevatten. Alle bestanden moeten zich in dezelfde taal (meertalige aangepaste stemmen worden niet ondersteund). De audio-bestanden ook elke moet een unieke numerieke bestandsnaam die zijn aangebracht met de bestandsnaamextensie `.wav`.

Audio-bestanden moeten als volgt worden voorbereid. Andere indelingen worden niet ondersteund en wordt geweigerd.

| **Eigenschap** | **Waarde** |
| ------------ | --------- |
| Bestandsindeling  | RIFF (WAV)|
| Samplefrequentie| 16.000 Hz |
| Kanalen     | 1 (mono)  |
| Voorbeeld-indeling| PCM, 16-bits |
| Bestandsnaam    | Numerieke met `.wav` extensie |
| Archiefindeling| Postcode      |
| De archiefgrootte van de maximale|200 MB|

Plaats de reeks audio-bestanden in een enkele map zonder submappen en de gehele set als een ZIP-archief voor één bestand van het pakket.

> [!NOTE]
> De portal op dit moment importeert ZIP archiveert maximaal 200 MB. Meerdere archieven kunnen echter worden geüpload. Het maximum aantal toegestane gegevenssets is dat 10 ZIP bestanden gratis abonnement gebruikers en 50 voor standaardabonnement gebruikers.

### <a name="transcripts"></a>Transcripties

Het bestand schrijffouten is een gewone Unicode-tekstbestand (weinig UTF-16-endian). Elke regel van het bestand schrijffouten moet de naam van een audiobestand, gevolgd door een tab-teken (codepunt 9) en tot slot de tekst van de hebben. Geen lege regels zijn toegestaan.

Bijvoorbeeld:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

Het systeem voor aangepaste gesproken normaliseert transcripties door de tekst converteren naar kleine letters bestaan en verwijderen van overbodige leestekens. Het is belangrijk dat de transcripties 100% nauwkeurig tot op het bijbehorende geluidsopnamen zijn.

> [!TIP]
> Wanneer bouwen productie spraak stemmen, selecteer utterances (of scripts schrijven) overweegt zowel fonetische dekking en efficiëntie.

## <a name="upload-your-datasets"></a>Gegevenssets uploaden

Na het voorbereiden van uw audio bestandsarchief en transcripties uploaden via de [serviceportal voor aangepaste gesproken](https://customvoice.ai).

> [!NOTE]
> Gegevenssets kan niet worden bewerkt nadat ze zijn geüpload. Als u vergeten transcripties van enkele van de audio-bestanden bevatten, bijvoorbeeld of per ongeluk het verkeerde geslacht kiest, moet u de volledige gegevensset opnieuw uploaden. Controleer uw gegevensset en instellingen zorgvuldig door voordat u begint met het uploaden.

1. Meld u aan bij de portal.

2. Kies **gegevens** onder aangepaste spraak op de hoofdpagina. 

    ![Mijn projecten](media/custom-voice/my-projects.png)

    De tabel mijn stem wordt weergegeven. Het is leeg als u alle gegevenssets stem nog niet hebt geüpload.

3. Klik op **gegevens importeren** om de pagina voor het uploaden van een nieuwe gegevensset te openen.

    ![Voice-gegevens importeren](media/custom-voice/import-voice-data.png)

4. Voer een naam en beschrijving in de opgegeven velden. 

5. Selecteer de landinstelling voor uw stem lettertypen. Zorg ervoor dat de landinstelling-informatie komt overeen met de taal van de opname-gegevens en de scripts. 

6. Selecteer het geslacht van de spreker waarvan stem die u gebruikt.

7. Kies de script- en audio-bestanden te uploaden. 

8. Klik op **importeren** om uw gegevens te uploaden. Voor grotere gegevenssets kan importeren enkele minuten duren.

> [!NOTE]
> Gratis abonnement gebruikers kunnen twee gegevenssets tegelijk uploaden. Standard-abonnement gebruikers kunnen tegelijkertijd vijf gegevenssets uploaden. Als u de limiet bereikt, wacht u tot ten minste één van uw gegevenssets eindigt importeren, probeer het opnieuw.

Wanneer het uploaden voltooid is, wordt de mijn stem gegevenstabel opnieuw weergegeven. U ziet een vermelding die overeenkomt met uw DataSet zojuist hebt geüpload. 

Gegevenssets worden automatisch gevalideerd nadat het uploaden. De gegevensvalidatie bevat een reeks controles op de audio-bestanden om te controleren of de bestandsindeling, de grootte en de samplingfrequentie. Controles op de bestanden schrijffouten controleert de bestandsindeling en voert u enkele normalisatie tekst. De utterances worden omgezet met behulp van spraakherkenning en de resulterende tekst wordt vergeleken met de tekst die u hebt opgegeven.

![Mijn Voice-gegevens](media/custom-voice/my-voice-data.png)

De volgende tabel geeft de verwerking voor geïmporteerde gegevenssets. 

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw gegevensset is ontvangen en in de wachtrij staat voor verwerking
| `Running` | Uw gegevensset wordt gevalideerd
| `Succeeded` | Uw gegevensset is gevalideerd en kan nu worden gebruikt voor het bouwen van een voice-lettertype

Nadat de validatie is voltooid, ziet u het totale aantal overeenkomende utterances voor elk van de gegevenssets in de kolom Utterance.

U kunt een rapport om te controleren van de uitspraak scores en het niveau van de ruis voor elk van de opnamen downloaden. De uitspraak score kan variëren van 0 tot 100; een score hieronder 70 doorgaans duidt dit op spraak fout of het script niet overeenkomen. Een zware accent vermindert uw score uitspraak en invloed van de gegenereerde digitale stem.

Een hogere signaal ruis ratio (signaal) geeft aan dat lagere ruis in de audio. U kunt een signaal 50 + doorgaans bereiken door opname via professional studios. Audio met een signaal hieronder 20 kan resulteren in voor de hand liggende ruis in uw gegenereerde stem.

U kunt nieuwe opname eventuele utterances met een lage uitspraak scores of slechte signaal ruis ratio's. Als nieuwe opname niet mogelijk is, kunt u deze utterances uitsluiten van uw gegevensset.

## <a name="build-your-voice-font"></a>Het lettertype stem bouwen

Als uw gegevensset is gevalideerd, kunt u deze kunt gebruiken voor het bouwen van uw aangepaste gesproken lettertype. 

1. Kies **modellen** in de vervolgkeuzelijst 'Aangepaste stem'. 
 
    De tabel mijn stem lettertypen verschijnt, eventuele aangepaste gesproken lettertypen die u al hebt gemaakt.

1. Klik op **stemmen maken** onder de titel van de tabel. 

    De pagina voor het maken van een lettertype stem wordt weergegeven. De huidige landinstelling wordt weergegeven in de eerste rij van de tabel. De landinstelling voor het maken van een stem in een andere taal wijzigen. De landinstelling moet hetzelfde zijn als de gegevenssets die wordt gebruikt voor het maken van de stem.

1. Net als bij uw gegevensset die u hebt geüpload, voer een naam en beschrijving voor hulp bij het identificeren van dit model. 

    De naam die u hier opgeeft, worden de naam die u gebruikt voor het opgeven van de stem in uw aanvraag voor spraak-synthese als onderdeel van de SSML-invoer en dus zorgvuldig te kiezen. Alleen letters, cijfers en enkele leestekens zoals '-', '_' '(',')' zijn toegestaan.

    Het beschrijvingsveld worden vaak gebruikt is om vast te leggen van de namen van de gegevenssets die werden gebruikt voor het maken van het model.

1. Kies het geslacht van uw voice-lettertype. Deze moet overeenkomen met het geslacht van de gegevensset.

1. Selecteer de gegevensset (s) die u gebruiken wilt voor het trainen van het lettertype stem. Alle gegevenssets die worden gebruikt, moet tussen de dezelfde spreker.

1. Klik op **maken** om te beginnen met het maken van uw voice-lettertype.

    ![Model maken](media/custom-voice/create-model.png)

Het nieuwe model wordt weergegeven in de tabel mijn stem lettertypen. 

![Mijn stem lettertypen](media/custom-voice/my-voice-fonts.png)

De weergegeven status weerspiegelt het proces van het converteren van uw gegevensset naar een lettertype spraak, zoals hier wordt weergegeven.

| Status | Betekenis
| ----- | -------
| `NotStarted` | Uw aanvraag voor het maken van de stem lettertype in de wachtrij voor verwerking
| `Running` | Het lettertype stem wordt gemaakt
| `Succeeded` | Het lettertype stem is gemaakt en kan worden geïmplementeerd

Training tijd varieert, afhankelijk van het volume van audiogegevens verwerkt. Typische keren tussen over honderden utterances 30 minuten en 40 uur voor 20.000 utterances.

> [!NOTE]
> Gratis abonnement gebruikers kunnen u twee stem lettertypen trainen tegelijk. Standaardabonnement gebruikers kunnen tegelijkertijd drie stemmen trainen. Als u de limiet bereikt, wacht totdat ten minste één van uw stem lettertypen training is voltooid en probeer het opnieuw.

## <a name="test-your-voice-font"></a>Het lettertype stem testen

Als uw stem lettertype met succes is gebouwd, kunt u het testen voordat u deze implementeert voor gebruik. Klik op **Test** in de Operations-kolom. De testpagina wordt weergegeven voor het lettertype van de geselecteerde stem. De tabel is leeg als u verzoeken van test voor de voice nog niet hebt ingediend.

![Mijn stem lettertypen, deel 2](media/custom-voice/my-voice-fonts2.png)

Klik op **Test met tekst** knop onder de titel van de tabel om een pop-upmenu voor het indienen van aanvragen van de tekst weer te geven. U kunt uw testaanvraag in tekst zonder opmaak of SSML verzenden. De maximale grootte van de invoer is 1024 tekens, inclusief alle codes voor SSML-aanvraag. De taal van de tekst moet hetzelfde zijn als de taal van het lettertype stem.

![Stem lettertype testen](media/custom-voice/voice-font-testing.png)

Nadat de invullen in het tekstvak en de invoermodus te bevestigen, klikt u op **Ja** dient u de testaanvraag en terugkeren naar de testpagina. De tabel bevat nu een vermelding die overeenkomt met uw nieuwe aanvraag en de kolom nu bekende status. Het kan even duren om na te bootsen spraak. Wanneer de statuskolom is voltooid gelezen, kunt u downloaden met de tekstinvoer (een `.txt` bestand) en audio-uitvoer (een `.wav` bestand) en de laatste voor kwaliteit beluisteren.

![Voice lettertype testen, deel 2](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Maken en gebruiken van een aangepaste eindpunt

Nadat u hebt gemaakt en het model van uw stem getest, kunt u deze implementeren in een aangepaste Text to Speech-eindpunt. Vervolgens gebruikt u dit eindpunt in plaats van het gebruikelijke eindpunt bij Text to Speech-aanvragen via de REST-API. Uw aangepaste eindpunt kan alleen worden aangeroepen door het abonnement dat u gebruikt voor het implementeren van het lettertype.

Kies voor het maken van een nieuwe aangepaste eindpunt **eindpunten** in het menu aangepaste gesproken boven aan de pagina. De pagina implementatie weergegeven met de tabel van de huidige aangepaste gesproken eindpunten, indien van toepassing.

Klik op de **implementeren stemmen** knop om een nieuwe eindpunt te maken. Pagina uit het eindpunt maken', de huidige landinstelling wordt weergegeven in de eerste rij van de tabel. Wijzig de landinstelling weergegeven voor het maken van een implementatie voor een andere taal. (Moet overeenkomen met de stem die u implementeert.) Voer de naam en beschrijving van uw aangepaste eindpunt.

Kies in het menu abonnement, het abonnement dat u wilt gebruiken. Gratis abonnement gebruikers hebben slechts één model tegelijk worden geïmplementeerd. Standaardabonnement-gebruikers kunnen maximaal 20 eindpunten, elk met een eigen aangepaste gesproken maken.

![Eindpunt maken](media/custom-voice/create-endpoint.png)

Klik na het selecteren van het model om te worden geïmplementeerd op **maken**. De pagina implementatie opnieuw wordt weergegeven, nu met een vermelding voor uw nieuwe eindpunt. Het duurt enkele minuten instantiëren van een nieuw eindpunt. Wanneer de status van de implementatie is geslaagd, wordt het eindpunt is klaar voor gebruik.

![Mijn geïmplementeerde stemmen](media/custom-voice/my-deployed-voices.png)

Wanneer de status van de implementatie is voltooid, het eindpunt van het lettertype geïmplementeerde stem wordt weergegeven in de tabel geïmplementeerde stemmen. U kunt deze URI gebruiken rechtstreeks in een HTTP-aanvraag.

Online testen van het eindpunt is ook beschikbaar via de portal voor aangepaste gesproken. Als u wilt testen van uw eindpunt, kies **eindpunten testen** uit de vervolgkeuzelijst van aangepaste stem. Het eindpunt pagina testen wordt weergegeven. Kies een stem die u hebt geïmplementeerd en voer de tekst die moet worden gesproken (in tekst zonder opmaak of SSML-indeling) in het tekstvak.

> [!NOTE] 
> Wanneer u SSML, de `<voice>` tag geeft de naam die u uw aangepaste stem is opgegeven toen u het hebt gemaakt.

Klik op **afspelen** de tekst die in uw aangepaste gesproken lettertype horen.

![Eindpunt testen](media/custom-voice/endpoint-testing.png)

Het aangepaste eindpunt is identiek aan het standaardeindpunt gebruikt voor het aanvragen van spraak. Zie [REST-API](rest-apis.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Herkent spraak in C#](quickstart-csharp-windows.md)
