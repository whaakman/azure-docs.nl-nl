---
title: Zelfstudie over het gebruiken van patronen voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Azure
description: In deze zelfstudie, gebruikt u patroon voor intents voor het verbeteren van LUIS intentie en entiteit voorspellingen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 12105829f62b988760d3bbf18000466fd27b9aff
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888327"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Zelfstudie: Gebruiken om patronen te verbeteren van voorspellingen

In deze zelfstudie gebruiken u patronen voor het verhogen van de intentie en entiteit voorspelling.  

> [!div class="checklist"]
* Identificeren dat een patroon waarmee uw app
* Over het maken van een patroon 
* Over het gebruik van vooraf gedefinieerde en aangepaste entiteiten in een patroon 
* Patroon voorspelling verbeteringen controleren
* Een rol toevoegen aan een entiteit contextueel op basis van entiteiten zoeken
* Een Pattern.any om te zoeken vrije entiteiten toevoegen

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md)-account nodig om uw LUIS-toepassing te creëren.

## <a name="import-humanresources-app"></a>Personeelszaken-app importeren
In deze zelfstudie worden geïmporteerd van een app personeelszaken. De app heeft drie intents: None, GetEmployeeOrgChart, GetEmployeeBenefits. De app heeft twee entiteiten: vooraf gedefinieerde aantal en de werknemer. De werknemer entiteit is een eenvoudige entiteit om op te halen van de naam van een werknemer. 

1. Maak een nieuw bestand van LUIS-app en geef deze de naam `HumanResources.json`. 

2. Kopieer de volgende app-definitie in het bestand:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. Op de LUIS **Apps** weergeeft, schakelt **importeren nieuwe app**. 

4. In de **importeren nieuwe app** dialoogvenster, selecteer de `HumanResources.json` bestand dat u in stap 1 hebt gemaakt.

5. Selecteer de **GetEmployeeOrgChart** intentie en vervolgens wijziging ten opzichte van **entiteiten weergeven** naar **Tokens weergave**. Verschillende voorbeeld-uitingen worden weergegeven. Elke utterance bevat een naam, een werknemer-entiteit. U ziet dat elke andere naam heeft en dat de indeling van de bewoording voor elk utterance verschilt. Deze verscheidenheid kunt u informatie over een breed scala aan uitingen LUIS.

    ![Schermafbeelding van de doel-pagina met entiteiten weergave omschakelt](media/luis-tutorial-pattern/utterances-token-view.png)

6. Selecteer **trainen** in de bovenste navigatiebalk om terug te trainen van de app. Wachten op de balk groen geslaagd.

7. Selecteer **Test** in het bovenste deelvenster. Voer `Who does Patti Owens report to?` en selecteer enter. Selecteer **inspecteren** onder de utterance voor meer informatie over de test.
    
    De naam van de werknemer, Patti Owens, is niet in een voorbeeld-utterance nog gebruikt. Dit is een test om te zien hoe goed LUIS geleerd, deze utterance is voor de `GetEmployeeOrgChart` intentie en de entiteit werknemer moet `Patti Owens`. Het resultaat moet minder zijn dan 50% (. 50) voor de `GetEmployeeOrgChart` intentie. Terwijl de bedoeling juist is, wordt de score is laag. De entiteit werknemer ook correct wordt geïdentificeerd als `Patti Owens`. Patronen Verhoog deze score initiële voorspelling. 

    ![Schermafbeelding van de Test-Configuratiescherm](media/luis-tutorial-pattern/original-test.png)

8. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Patronen leren LUIS algemene uitingen met minder voorbeelden
Vanwege de aard van het human resources-domein, moet u er een paar algemene manieren om vragen over werknemer relaties in organisaties zijn. Bijvoorbeeld:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Deze uitingen zijn te dicht bij de contextuele Uniekheid van elk zonder op te geven veel utterance voorbeelden bepalen. Door toe te voegen een patroon voor een doel, LUIS algemene utterance-patronen leert herkennen voor een doel zonder op te geven veel utterance voorbeelden. 

Voorbeeld van de sjabloon uitingen hiervoor intentie zijn:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Het patroon is een combinatie van reguliere expressie die overeenkomen met en machine learning. Geef enkele sjabloon vervolgens utterance voorbeelden voor LUIS voor meer informatie over het patroon. Deze voorbeelden, samen met de intentie uitingen geven LUIS een beter beeld van welke uitingen past de bedoeling is en waar, binnen de utterance, de entiteit bestaat. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>De sjabloon utterances toevoegen

1. In het linker navigatiedeelvenster, onder **verbeterde app-prestaties**, selecteer **patronen** in de linkernavigatiebalk.

2. Selecteer de **GetEmployeeOrgChart** doel, voert u de volgende sjabloon uitingen, één voor één, selecteren enter na elke utterance sjabloon:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    De `{Employee}` syntaxis van de locatie van de entiteit in de sjabloon utterance als het goed als welke entiteit is gemarkeerd. 

    ![Schermafbeelding van het invoeren van de sjabloon-uitingen voor doel](./media/luis-tutorial-pattern/enter-pattern.png)

3. Selecteer **Train** in de bovenste navigatiebalk. Wachten op de balk groen geslaagd.

4. Selecteer **Test** in het bovenste deelvenster. Voer `Who does Patti Owens report to?` in het tekstvak in. Selecteer invoeren. Dit is de dezelfde utterance getest in de vorige sectie. Het resultaat moet hoger zijn voor de `GetEmployeeOrgChart` intentie. 

    De score is nu veel beter. LUIS hebt geleerd van het patroon die relevant zijn voor het doel zonder op te geven veel voorbeelden.

    ![Schermafbeelding van de Test-deelvenster met hoge score leiden tot](./media/luis-tutorial-pattern/high-score.png)

    De entiteit is eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waar de entiteit wordt niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon). 

5. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Een entiteit met een rol in een patroon gebruiken
De LUIS-app wordt gebruikt om u te helpen bij het verplaatsen van werknemers van de ene locatie naar een andere. Een voorbeeld-utterance is `Move Bob Jones from Seattle to Los Colinas`. Elke locatie in de utterance heeft een andere betekenis. Seattle is de oorspronkelijke locatie en Los Colinas is de doellocatie voor de verplaatsing. Om te onderscheiden van die locaties in het patroon maakt in de volgende secties u een eenvoudige entiteit voor de locatie van kunt maken met twee rollen: bron en doel. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Maak een nieuw doel voor het verplaatsen van mensen en activa
Maak een nieuw doel voor alle uitingen die over het verplaatsen personen of activa zijn.

1. Selecteer **Intents** in linkernavigatiebalk
2. Selecteer **nieuwe doel maken**
3. De naam van het nieuwe doel `MoveAssetsOrPeople`
4. Voorbeeld utterances toevoegen:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Schermopname van voorbeeld utterance voor MoveAssetsOrPeople doel](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    Het doel van de voorbeeld-uitingen is om voldoende voorbeelden te geven. Als u later in de test, de locatie-entiteit wordt niet gedetecteerd en als gevolg daarvan het patroon is niet gevonden, keert u terug naar deze stap en meer voorbeelden toevoegen. Vervolgens trainen en opnieuw testen. 

5. Markeer de entiteiten in de voorbeeld-uitingen aan de werknemer entiteit door de naam van de eerste vervolgens de achternaam in een utterance selecteren en vervolgens de werknemer entiteit selecteren in de lijst.

    ![Schermafbeelding van uitingen in MoveAssetsOrPeople gemarkeerd met werknemer entiteit](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Selecteer de tekst `portland` in de utterance `move travis hinton from portland to orlando`. Voer de naam van de nieuwe entiteit in het pop-updialoogvenster `Location`, en selecteer **nieuwe entiteit maken**. Kies de **eenvoudige** entiteitstype en selecteer **gedaan**.

    ![Schermopname van het maken van nieuwe entiteit op locatie](./media/luis-tutorial-pattern/create-new-location-entity.png)

    De rest van de locatienamen van de in de uitingen markeren. 

    ![Schermafbeelding van alle entiteiten die zijn gemarkeerd](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    Het patroon van word keuze en volgorde is duidelijk in de vorige afbeelding. Als u **niet** met patronen, en de uitingen op het doel hebben een duidelijk patroon, dat is een goede indicatie u patronen moet worden gebruikt. 

    Dit zijn de verkeerde voorbeeld-uitingen als u verwacht een groot aantal uitingen, in plaats van een patroon dat. In dat geval wilt u uiteenlopende uitingen term of word keuze, utterance lang zijn, en de plaatsing van de entiteit. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Functie toevoegen aan de entiteit locatie 
Rollen kunnen alleen worden gebruikt voor patronen. De rollen van de bron en doel toevoegen aan de locatie-entiteit. 

1. Selecteer **entiteiten** in het linkernavigatievenster, klikt u vervolgens **locatie** uit de lijst met entiteiten.

2. Voeg `Origin` en `Destination` rollen aan de entiteit.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/location-entity.png)

    De rollen zijn niet gemarkeerd op de intentie pagina MoveAssetsOrPeople omdat rollen niet bestaan op de intentie uitingen. Ze bestaan alleen op uitingen van patroon-sjabloon. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Sjabloon-uitingen die gebruikmaakt van locatie-en-doel toevoegen
Sjabloon-uitingen die gebruikmaken van de nieuwe entiteit toevoegen.

1. Selecteer **patronen** in de linkernavigatiebalk.

2. Selecteer de **MoveAssetsOrPeople** intentie.

3. Voer een nieuwe sjabloon utterance met behulp van de nieuwe entiteit `Move {Employee} from {Location:Origin} to {Location:Destination}`. De syntaxis voor een entiteit en een rol in een sjabloon utterance is `{entity:role}`.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. De app voor de nieuwe doel, de entiteit en het patroon trainen.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Het nieuwe patroon voor rol gegevensextractie testen
Het nieuwe patroon overeen met een test.

1. Selecteer **Test** in het bovenste deelvenster. 
2. Voer de utterance `Move Tammi Carlson from Bellingham to Winthrop`.
3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor de entiteit en voor welk doel.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/test-with-roles.png)

    De entiteiten worden eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waar de entiteiten zijn niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon). 

4. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Een entiteit Pattern.any vrije entiteiten zoeken in een patroon gebruiken
Deze app Personeelszaken helpt ook bij werknemers bedrijf formulieren zoeken. Veel van de formulieren hebben softwaretitels die worden verschillende lengte. De verschillende lengten bevat zinnen die verwarrend kunnen zijn LUIS over waar de naam van het formulier eindigt. Met behulp van een **Pattern.any** entiteit in een patroon kunt u het begin en einde van de naam van het formulier opgeven, zodat de naam van het formulier in LUIS correct worden geëxtraheerd. 

### <a name="create-a-new-intent-for-the-form"></a>Een nieuw doel voor het formulier maken
Maak een nieuw doel voor uitingen die formulieren zoekt.

1. Selecteer **Intents** in linkernavigatiebalk.

2. Selecteer **Create new intent**.

3. Naam van het nieuwe doel `FindForm`.

4. Voeg een voorbeeld-utterance.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/intent-findform.png)

    De formuliertitel van het is `What to do when a fire breaks out in the Lab`. De utterance vraagt om de locatie van het formulier en wordt ook gevraagd die behoefte hebben aan te melden dat het valideren van de werknemer lezen. Zonder een entiteit Pattern.any, zou het lastig zijn om te begrijpen waar de titel van het formulier wordt beëindigd en uitpakken van de titel van het formulier als een entiteit van de utterance.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Een entiteit Pattern.any voor de formuliertitel van het maken
De entiteit Pattern.any kunt voor entiteiten met verschillende lengten. Dit werkt alleen in een patroon omdat het patroon het begin en einde van de entiteit markeert. Als u dat het patroon zien wanneer deze een Pattern.any bevat, extraheert entiteiten onjuist, gebruikt u een [expliciete lijst](luis-concept-patterns.md#explicit-lists) om dit probleem te verhelpen. 

1. Selecteer **entiteiten** in het linkernavigatievenster.

2. Selecteer **Create new intent**. 

3. Naam van de entiteit `FormName` met het type **Pattern.any**. Voor deze specifieke zelfstudie hoeft u niet alle functies toevoegen aan de entiteit.

    ![Afbeelding van dialoogvenster voor de naam van de entiteit en entiteitstype](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Een patroon dat gebruikmaakt van de Pattern.any toevoegen

1. Selecteer **patronen** in de linkernavigatiebalk.

2. Selecteer de **FindForm** intentie.

3. Voer de utterance van een sjabloon met behulp van de nieuwe entiteit `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Schermafbeelding van de sjabloon utterance pattern.any entiteit gebruiken](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. De app voor de nieuwe doel, de entiteit en het patroon trainen.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Het nieuwe patroon voor vrije gegevensextractie testen
1. Selecteer **testen** in de bovenste balk met om de test-deelvenster te openen. 

2. Voer de utterance `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor de entiteit en voor welk doel.

    ![Schermafbeelding van de sjabloon utterance pattern.any entiteit gebruiken](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    De entiteit is eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waar de entiteiten zijn niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon).

4. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Om dit te doen, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de naam van de app in de lijst met Apps, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over aanbevolen procedures voor LUIS-apps](luis-concept-best-practices.md)