---
title: Maak een aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Wanneer u klaar om uw gegevens te uploaden bent, gaat u naar de portal voor aangepaste spraak. Maak of Selecteer een aangepaste spraak-project. Het project moet delen geslacht eigenschappen als de gegevens en de juiste taal/landinstelling doel te gebruiken voor uw stemtraining.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 6189ea2866d1c16f994179df0179e29353e6c47d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65410720"
---
# <a name="create-a-custom-voice"></a>Maken van een aangepaste spraak

In [gegevens voorbereiden voor aangepaste spraak](how-to-custom-voice-prepare-data.md), we de verschillende gegevenstypen die u gebruiken kunt voor het trainen van een aangepaste spraak- en de verschillende vereisten die worden beschreven. Als u uw gegevens hebt voorbereid, kunt u beginnen met uploaden ze naar de [aangepaste spraak portal](https://aka.ms/custom-voice-portal), of via de API voor aangepaste spraak training. Hier beschrijven we de stappen voor het trainen van een aangepaste gesproken via de portal.

> [!NOTE]
> Deze pagina wordt ervan uitgegaan dat u hebt gelezen [aan de slag met aangepaste spraak](how-to-custom-voice.md) en [gegevens voorbereiden voor aangepaste spraak](how-to-custom-voice-prepare-data.md), en een aangepaste spraak-project hebt gemaakt.

Controleer de talen die worden ondersteund voor aangepaste spraak: [taal voor aanpassing](language-support.md#customization).

## <a name="upload-your-datasets"></a>Uw gegevenssets uploaden

Als u klaar om uw gegevens te uploaden bent, gaat u naar de [aangepaste spraak portal](https://aka.ms/custom-voice-portal). Maak of Selecteer een aangepaste spraak-project. Het project moet delen geslacht eigenschappen als de gegevens en de juiste taal/landinstelling doel te gebruiken voor uw stemtraining. Selecteer bijvoorbeeld `en-GB` als de audio-opnamen hebt gedaan met een groot-Brittannië accent in het Engels.

Ga naar de **gegevens** tabblad en klik op **gegevens uploaden**. Selecteer het juiste gegevenstype die overeenkomt met wat u hebt voorbereid in de wizard.

Elke gegevensset die u uploadt, moet voldoen aan de vereisten voor het gegevenstype dat u kiest. Het is belangrijk om correct uw gegevens voordat deze wordt geüpload. Dit zorgt ervoor dat de gegevens nauwkeurig wordt verwerkt door de aangepaste spraak-service. Ga naar [gegevens voorbereiden voor aangepaste spraak](how-to-custom-voice-prepare-data.md) en zorg ervoor dat uw gegevens terecht is geformatteerd.

> [!NOTE]
> Gratis abonnement (F0) gebruikers kunnen tegelijkertijd twee gegevenssets uploaden. Standard-(S0) abonnementsgebruikers kunnen tegelijkertijd vijf gegevenssets uploaden. Als u de limiet is bereikt, wacht u totdat ten minste één van uw gegevenssets geïmporteerd is. Probeer het opnieuw.

> [!NOTE]
> Het maximum aantal gegevenssets kunnen worden geïmporteerd per abonnement is 10 .zip bestanden gratis abonnementsgebruikers (F0) en 500 voor abonnementsgebruikers van de standard-(S0).

Gegevenssets worden automatisch gecontroleerd zodra u de knop voor uploaden bereikt. Gegevensvalidatie bevat een reeks controles op de audio-bestanden om te controleren of de bestandsindeling, de grootte en de samplingfrequentie. Corrigeer de fouten indien van toepassing en verzend opnieuw. Wanneer de aanvraag voor het importeren van gegevens is gestart, ziet u een vermelding in de tabel die overeenkomt met de gegevensset die u zojuist hebt geüpload.

De volgende tabel ziet u de verwerking van statussen voor geïmporteerde gegevenssets:

| Status | Betekenis |
| ----- | ------- |
| Verwerken | Uw gegevensset is ontvangen en wordt verwerkt. |
| Geslaagd | Uw gegevensset is gevalideerd en kan nu worden gebruikt om een stem-model bouwen. |
| Mislukt | Uw gegevensset is tijdens de verwerking van vanwege verschillende redenen, bijvoorbeeld bestandsfouten, problemen met gegevens of netwerkproblemen is mislukt. |

Nadat de validatie is voltooid, ziet u het totale aantal overeenkomende uitingen voor elk van uw gegevenssets in de **uitingen** kolom. Als het gegevenstype dat u hebt geselecteerd lange-audio segmentering vereist, geeft deze kolom alleen de uitingen die we hebben gesegmenteerd voor u die een op basis van uw Transcripten of via de service transcriptie van spraak. Verder kunt u downloaden van de gegevensset die is gevalideerd om de resultaten van de details van de uitingen die zijn geïmporteerd en hun toewijzing Transcripten weer te geven. Hint: lange-audio segmentatie kan meer dan een uur gegevensverwerking duren.

Voor gegevenssets en-US en zh-CN, kunt u een rapport om te controleren of de uitspraak van scores en het niveau van de ruis voor elk van de opnamen verder te downloaden. De uitspraak van score varieert van 0 tot 100. Een score lager 70 geeft doorgaans aan dat de spraak-fout of script niet overeenkomen. Een zware accent vermindert uw score uitspraak en van invloed zijn op de gegenereerde digitale stem.

Een hogere signaal ruis snelheid (signaal) geeft aan dat lagere ruis in uw audio. Doorgaans kunt u een signaal meer dan 50 bereiken door de opname op professionele studios. Audio met een signaal hieronder 20 kan resulteren in voor de hand liggende ruis in uw gegenereerde stem.

Houd rekening met alle uitingen met lage uitspraak van scores of slechte signaal ruis ratio's opnieuw op te nemen. Als u kan niet opnieuw registreert, kunt u die uitingen uitsluiten van uw gegevensset.

## <a name="build-your-custom-voice-model"></a>Uw aangepaste spraak-model bouwt.

Nadat uw gegevensset is gevalideerd, kunt u deze kunt gebruiken om uw aangepaste spraak-model te bouwen.

1.  Navigeer naar **tekst naar spraak > aangepaste gesproken > Training**.

2.  Klik op **Train model**.

3.  Voer vervolgens een **naam** en **beschrijving** zodat u kunt dit model herkennen.

    Kies een naam zorgvuldig. De naam die u hier opgeeft, worden de naam die u gebruikt om op te geven van de toon in uw aanvraag voor spraaksynthese als onderdeel van de SSML invoer. Alleen letters, cijfers en enkele leestekens zoals - \_, en (',') zijn toegestaan. Gebruik verschillende namen voor andere stem-modellen.

    Een algemene gebruik van de **beschrijving** veld, is het vastleggen van de namen van de gegevenssets die zijn gebruikt voor het maken van het model.

4.  Uit de **Selecteer trainingsgegevens** pagina, kies een of meer gegevenssets die u wilt gebruiken voor training. Controleer het aantal uitingen voordat u ze verzendt. U kunt beginnen met een willekeurig aantal uitingen voor en-US- en zh-CN voice-modellen. Voor andere landinstellingen, moet u meer dan 2000 uitingen kunnen met het trainen van een stem selecteren.

    > [!NOTE]
    > Dubbele namen van de audio wordt verwijderd uit de training. Zorg ervoor dat de gegevenssets die u selecteert hebben niet de dezelfde namen van de audio in meerdere ZIP-bestanden.

    > [!TIP]
    > Met behulp van de gegevenssets van de dezelfde spreker is vereist voor kwaliteitsresultaten. Wanneer de gegevenssets die u hebt verzonden voor training een totaalaantal van minder dan 6000 afzonderlijke uitingen bevat, wordt u het model van uw stem via de techniek statistische parametrische synthese trainen. In het geval waarin uw trainingsgegevens groter is dan een totaal aantal 6000 afzonderlijke uitingen, wordt u een trainingsproces met de techniek samenvoeging synthese gestart. Normaal gesproken kan de samenvoeging technologie resulteren in meer natuurlijke en hogere kwaliteit van zowel spraak resultaten. [Neem contact op met het team voor aangepaste spraak](mailto:speechsupport@microsoft.com) als u wilt een model trainen met de meest recente Neurale TTS-technologie die een digitale stem gelijk is aan de openbaar beschikbare kan opleveren [neurale stemmen](language-support.md#neural-voices).

5.  Klik op **Train** om te beginnen met het maken van uw stem-model.

Een nieuwe vermelding die overeenkomt met bevat de Training-tabel naar de zojuist gemaakte model. De tabel wordt de status ook weergegeven: Verwerking is voltooid, is mislukt.

De status die wordt weergegeven, geeft het proces van het converteren van uw gegevensset naar een model spraak, zoals hier wordt weergegeven.

| Status | Betekenis |
| ----- | ------- |
| Verwerken | Uw stem-model wordt gemaakt. |
| Geslaagd | Het model van uw stem is gemaakt en kan worden geïmplementeerd. |
| Mislukt | Het model van uw stem is uitgevoerd in training vanwege verschillende redenen, bijvoorbeeld niet waren herkend problemen of netwerkproblemen. |

Tijd training, is afhankelijk van het volume van audiogegevens verwerkt. Typische tijden variëren van informatie over-30 minuten voor honderden uitingen tot 20.000 uitingen 40 uur. Als uw modeltraining is voltooid, kunt u starten om deze te testen.

> [!NOTE]
> Gratis abonnement (F0) gebruikers kunnen tegelijkertijd een spraakstijl trainen. Standard-(S0) abonnementsgebruikers kunnen tegelijkertijd drie stemmen trainen. Als u de limiet is bereikt, wacht totdat ten minste één van uw spraakstijlen training is voltooid en probeer het vervolgens opnieuw.

> [!NOTE]
> Het maximum aantal voice-modellen kunnen worden getraind per abonnement is 10 modellen voor gebruikers van gratis abonnement (F0) en 100 voor abonnementsgebruikers van de standard-(S0).

## <a name="test-your-voice-model"></a>Uw stem-model testen

Nadat uw spraakstijl is is gebouwd, kunt u deze testen voordat u deze implementeert voor gebruik.

1.  Navigeer naar **tekst naar spraak > aangepaste gesproken > testen**.

2.  Klik op **toevoegen test**.

3.  Selecteer een of meerdere modellen die u wilt testen.

4.  Geef de tekst die u de voice(s) wilt te spreken. Als u voor het testen van meerdere modellen in één keer hebt geselecteerd, wordt dezelfde tekst worden gebruikt voor het testen van verschillende modellen.

    > [!NOTE]
    > De taal van de tekst moet hetzelfde zijn als de taal van uw spraakstijl. Alleen met succes getrainde modellen kunnen worden getest. Alleen tekst zonder opmaak wordt ondersteund in deze stap.

5.  Klik op **Create**.

Nadat u uw testaanvraag hebt ingediend, gaat u terug naar de testpagina. De tabel bevat nu een vermelding die overeenkomt met uw nieuwe aanvraag en de statuskolom. Het kan enkele minuten om na te bootsen spraak duren. Als de statuskolom zegt **geslaagd**, u kunt het geluid afspelen of de tekstinvoer (een txt-bestand) downloaden en audio-uitvoer (een bestand met de indeling .wav), en verder beluisteren voor kwaliteit.

U kunt ook de resultaten van de vinden op de detailpagina met van elke modellen die u hebt geselecteerd voor het testen. Ga naar de **Training** tabblad en klik op de naam van het model in te voeren, de detailpagina van het model.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Maken en gebruiken van een aangepaste spraak-eindpunt

Nadat u hebt met succes gemaakt en het model van uw stem getest, kunt u deze implementeren in een aangepast Text to Speech-eindpunt. Vervolgens gebruikt u dit eindpunt in plaats van het eindpunt van de gebruikelijke bij het maken van tekst naar spraak aanvragen via de REST-API. Uw aangepaste eindpunt kan worden aangeroepen door het abonnement dat u hebt gebruikt voor het implementeren van het lettertype.

Voor het maken van een nieuwe aangepaste spraak-eindpunt, gaat u naar **tekst naar spraak > aangepaste gesproken > implementatie**. Selecteer **eindpunt toevoegen** en voer een **naam** en **beschrijving** voor uw aangepast eindpunt. Selecteer vervolgens het aangepaste spraak-model dat u wilt koppelen aan dit eindpunt.

Nadat u hebt geklikt op de **toevoegen** knop in de tabel-eindpunt, ziet u een vermelding voor het nieuwe eindpunt voor. Het duurt een paar minuten voor het starten van een nieuw eindpunt. Wanneer de status van de implementatie is **geslaagd**, het eindpunt is gereed voor gebruik.

> [!NOTE]
> Gratis abonnement (F0) gebruikers kunnen slechts één model geïmplementeerd hebben. Standard-(S0) abonnementsgebruikers kunnen maximaal 50 eindpunten, elk met een eigen aangepaste gesproken maken.

> [!NOTE]
> Voor het gebruik van uw aangepaste spraak, moet u opgeven de naam van het stem-model, gebruikt u de aangepaste URI rechtstreeks in een HTTP-aanvraag en hetzelfde abonnement worden gebruikt om door te geven via de verificatie van TTS-service.

Nadat het eindpunt is geïmplementeerd, wordt de naam van het eindpunt als een koppeling weergegeven. Klik op de koppeling om specifieke informatie aan uw eindpunt, zoals de eindpuntsleutel, de eindpunt-URL en de voorbeeldcode weer te geven.

Online testen van het eindpunt is ook beschikbaar via de portal voor aangepaste gesproken. Als u wilt testen op uw eindpunt, kies **controleren op endpoint** uit de **Endpoint details** pagina. Het eindpunt testen van de pagina wordt weergegeven. Voer de tekst die moet worden uitgesproken (hetzij als tekst zonder opmaak of [SSML indeling](speech-synthesis-markup.md) in het tekstvak in. Als u wilt de tekst die in uw aangepaste spraakstijl gesproken horen, selecteer **afspelen**. Deze functie testen wordt in rekening gebracht op basis van uw aangepaste spraak synthese gebruik.

Het eindpunt van de aangepaste is identiek aan de standard-eindpunt dat wordt gebruikt voor de Text to Speech-aanvragen. Zie [REST-API](rest-text-to-speech.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Handleiding voor het: Registreren van uw stem-voorbeelden](record-custom-voice-samples.md)
* [Text to Speech-API-verwijzing](rest-text-to-speech.md)
