---
title: Zelfstudie met patronen voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Azure
description: In deze zelfstudie gebruikt u patroon voor intents om betere LUIS bedoeling en entiteit voorspellingen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: d2d680a1e2749c6e0a5b17876915c5c0d86eb420
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345963"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Zelfstudie: Patronen gebruiken voor het verbeteren van voorspellingen

In deze zelfstudie patronen te verhogen bedoeling en entiteit voorspelling te gebruiken.  

> [!div class="checklist"]
* Identificeren dat een patroon uw app helpen kan
* Het maken van een patroon 
* Het gebruik van vooraf gedefinieerde en aangepaste entiteiten in een patroon 
* Het patroon voorspelling verbeteringen controleren
* Een rol toevoegen aan een entiteit contextueel gebaseerde entiteiten vinden
* Het toevoegen van een Pattern.any om te zoeken vrije vorm entiteiten

Voor dit artikel, moet u een gratis [LUIS] [ LUIS] account om uw toepassing LUIS ontwerpen.

## <a name="import-humanresources-app"></a>Importeren humanresources heet app
Deze zelfstudie importeert een app humanresources heet. De app heeft drie intents: None, GetEmployeeOrgChart, GetEmployeeBenefits. De app heeft twee entiteiten: vooraf gedefinieerde aantal en de werknemer. De werknemer entiteit is een enkele entiteit uitpakken van de naam van een werknemer. 

1. Maak een nieuw LUIS app-bestand en noem deze `HumanResources.json`. 

2. Kopieer de volgende app-definitie in het bestand:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. Op de LUIS **Apps** pagina **importeren nieuwe app**. 

4. In de **importeren nieuwe app** dialoogvenster, selecteer de `HumanResources.json` bestand dat u in stap 1 hebt gemaakt.

5. Selecteer de **GetEmployeeOrgChart** bedoeling en vervolgens de wijzigingen van de **entiteiten weergave** naar **Tokens weergave**. Verschillende voorbeeld utterances worden weergegeven. Elke utterance bevat een naam die een entiteit van de werknemer is. U ziet dat de naam van elke anders is en dat de indeling van de vermelding voor elke utterance verschilt. Deze verscheidenheid helpt LUIS meer informatie over een breed scala aan utterances.

    ![Schermafbeelding van de doel-pagina met entiteiten weergave uitgeschakeld](media/luis-tutorial-pattern/utterances-token-view.png)

6. Selecteer **trainen** in de bovenste navigatiebalk voor het trainen van de app. Wacht totdat de balk groen geslaagd.

7. Selecteer **Test** in het bovenste deelvenster. Voer `Who does Patti Owens report to?` voert u selecteert. Selecteer **inspecteren** onder de utterance voor meer informatie over de test.
    
    De naam van de werknemer, Patti Owens, is niet in een voorbeeld utterance nog gebruikt. Dit is een test om te zien hoe goed LUIS geleerd deze utterance is voor de `GetEmployeeOrgChart` doel en de werknemer-entiteit moeten worden `Patti Owens`. Het resultaat moet minder zijn dan 50% (. 50) voor de `GetEmployeeOrgChart` bedoeld. Terwijl de bedoeling juist is, is de score laag. De entiteit van de werknemer ook correct wordt geïdentificeerd als `Patti Owens`. Patronen vergroot deze initiële voorspelling score. 

    ![Schermafbeelding van de Test-Configuratiescherm](media/luis-tutorial-pattern/original-test.png)

8. Het paneel test sluiten door het selecteren van de **testen** knop in de bovenste navigatiebalk. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Patronen leren LUIS algemene utterances met minder voorbeelden
Vanwege de aard van het domein Personeelszaken zijn er enkele algemene manieren van vragen over werknemer relaties in organisaties. Bijvoorbeeld:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Deze utterances zijn te dicht bij de contextuele Uniekheid van elke zonder op te geven voorbeelden van veel utterance bepalen. Als u een patroon voor de opzet, leert LUIS algemene utterance patronen voor een doel zonder op te geven voorbeelden van veel utterance. 

Voorbeeld van de sjabloon utterances hiervoor opzet zijn:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Het patroon is een combinatie van reguliere expressie die overeenkomt met en machine learning. Vervolgens bieden sommige sjabloon utterance voorbeelden voor LUIS voor meer informatie over het patroon. Deze voorbeelden, samen met de opzet utterances geven LUIS beter inzicht aanpassen aan de bedoeling van welke utterances en waar binnen de utterance de entiteit bestaat. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>De sjabloon utterances toevoegen

1. In het linkernavigatievenster onder **app sneller**, selecteer **patronen** van de linkernavigatiebalk.

2. Selecteer de **GetEmployeeOrgChart** bedoelingen, voert u de volgende sjabloon utterances, één voor één, selecteren invoeren na elke utterance sjabloon:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    De `{Employee}` syntaxis markeert de locatie van de entiteit in de sjabloon utterance alsmede welke entiteit is. 

    ![Schermopname van het sjabloon utterances voor bedoeling invoeren](./media/luis-tutorial-pattern/enter-pattern.png)

3. Selecteer **Train** in de bovenste navigatiebalk. Wacht totdat de balk groen geslaagd.

4. Selecteer **Test** in het bovenste deelvenster. Voer `Who does Patti Owens report to?` in het tekstvak. Selecteer invoeren. Dit is de dezelfde utterance getest in de vorige sectie. Het resultaat moet hoger zijn voor de `GetEmployeeOrgChart` bedoeld. 

    De score is nu veel beter. Het patroon die relevant zijn voor de bedoeling LUIS geleerd zonder te veel voorbeelden geven.

    ![Schermopname van Test deelvenster met de hoogste score leiden](./media/luis-tutorial-pattern/high-score.png)

    De entiteit is eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waar de entiteit wordt niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon). 

5. Het paneel test sluiten door het selecteren van de **testen** knop in de bovenste navigatiebalk.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Een entiteit met een rol in een patroon gebruiken
De app LUIS wordt gebruikt om te verplaatsen van werknemers vanaf één locatie naar een andere. Een voorbeeld utterance is `Move Bob Jones from Seattle to Los Colinas`. Elke locatie in de utterance heeft een andere betekenis. Seattle is de oorspronkelijke locatie en Los Colinas is de bestemmingslocatie voor de verplaatsing. Als u wilt onderscheiden van die locaties in het patroon, in de volgende secties u een enkele entiteit voor de locatie van kunt maken met twee rollen: oorsprong en de bestemming. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Maak een nieuwe bedoeld voor het verplaatsen van mensen en activa
Maak een nieuwe bedoeld voor een utterances die over zwevend personen of activa zijn.

1. Selecteer **Intents** van linkernavigatiegedeelte
2. Selecteer **nieuwe doel maken**
3. De naam van de nieuwe bedoeling `MoveAssetsOrPeople`
4. Voorbeeld utterances toevoegen:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Schermopname van voorbeeld utterance voor MoveAssetsOrPeople doel](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    Het doel van de voorbeeld-utterances is om voldoende voorbeelden te geven. Als u later in de test, de locatie-entiteit wordt niet gedetecteerd en als gevolg daarvan het patroon is niet gevonden, keert u terug naar deze stap en meer voorbeelden toevoegen. Vervolgens trainen en test u het opnieuw. 

5. Markeer de entiteiten in de utterances voorbeeld met de werknemer entiteit door de voornaam vervolgens de achternaam in een utterance selecteren en vervolgens de werknemer entiteit selecteren in de lijst.

    ![Schermopname van utterances in MoveAssetsOrPeople gemarkeerd met werknemer entiteit](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Selecteer de tekst `portland` in de utterance `move travis hinton from portland to orlando`. Geef de nieuwe entiteitsnaam in het pop-updialoogvenster `Location`, en selecteer **nieuwe entiteit maken**. Kies de **eenvoudige** entiteitstype en selecteer **gedaan**.

    ![Schermafdruk van het maken van de nieuwe locatie entiteit](./media/luis-tutorial-pattern/create-new-location-entity.png)

    De rest van de locatienamen in de utterances markeren. 

    ![Schermopname van alle entiteiten die zijn gemarkeerd](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    Het patroon van word keuze en -volgorde is duidelijk in de vorige afbeelding. Als u **niet** met patronen, en de utterances van het doel een duidelijk patroon, dat een goede indicatie moet u patronen hebben. 

    Als u verwacht een groot aantal utterances in plaats van een patroon dat, zijn dit de verkeerde voorbeeld utterances. In dat geval kunt u uiteenlopende utterances in term woord keuze utterance lengte en de plaatsing van de entiteit. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Functie toevoegen aan de entiteit locatie 
Rollen kunnen alleen worden gebruikt voor patronen. De rollen van de oorsprong en de bestemming toevoegen aan de locatie-entiteit. 

1. Selecteer **entiteiten** in de navigatiebalk links, klikt u vervolgens **locatie** uit de lijst van entiteiten.

2. Voeg `Origin` en `Destination` rollen aan de entiteit.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/location-entity.png)

    De rollen zijn niet gemarkeerd op de opzet pagina MoveAssetsOrPeople omdat de rollen op opzet utterances bestaan niet. Ze bestaan alleen op patroon sjabloon utterances. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Sjabloon utterances die gebruikmaakt van locatie- en doelserver rollen toevoegen
Sjabloon utterances die gebruikmaken van de nieuwe entiteit toevoegen.

1. Selecteer **patronen** van de linkernavigatiebalk.

2. Selecteer de **MoveAssetsOrPeople** bedoeld.

3. Voer een nieuwe sjabloon utterance met behulp van de nieuwe entiteit `Move {Employee} from {Location:Origin} to {Location:Destination}`. De syntaxis voor een entiteit en de rol binnen een sjabloon utterance is `{entity:role}`.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Training van de app voor de nieuwe doel, de entiteit en het patroon.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Het nieuwe patroon voor het ophalen van de rol gegevens testen
Het nieuwe patroon overeen met een test.

1. Selecteer **Test** in het bovenste deelvenster. 
2. Voer de utterance `Move Tammi Carlson from Bellingham to Winthrop`.
3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor entiteit en hetzelfde doel.

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/test-with-roles.png)

    De entiteiten worden eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waarbij de entiteiten worden niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon). 

4. Het paneel test sluiten door het selecteren van de **testen** knop in de bovenste navigatiebalk.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Gebruik van een entiteit Pattern.any om te zoeken vrije vorm entiteiten in een patroon
Deze app humanresources heet helpt ook bij werknemers bedrijf formulieren zoeken. Veel van de formulieren hebben softwaretitels die zijn verschillende lengte. De verschillende lengten omvat die verwarrend LUIS over waar de naam van het formulier eindigt. Met behulp van een **Pattern.any** entiteit in een patroon kunt u het begin en einde van de naam van het formulier opgeven zodat LUIS correct naam van het formulier extraheert. 

### <a name="create-a-new-intent-for-the-form"></a>Een nieuw doel voor het formulier maken
Maak een nieuwe bedoeld voor utterances die formulieren zoekt.

1. Selecteer **Intents** van de navigatiebalk aan de linkerkant.

2. Selecteer **maken van nieuwe bedoeling**.

3. Naam van de nieuwe bedoeling `FindForm`.

4. Een voorbeeld utterance toevoegen.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Schermafbeelding van de nieuwe entiteit met rollen](./media/luis-tutorial-pattern/intent-findform.png)

    De formuliertitel is `What to do when a fire breaks out in the Lab`. De utterance vraagt om de locatie van het formulier en wordt ook gevraagd wie moet het valideren van de werknemer lezen ondertekenen. Zonder een entiteit Pattern.any zou het lastig zijn om te begrijpen waar de titel van het formulier wordt beëindigd en de titel van het formulier als een entiteit van de utterance extraheren.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Maak een entiteit Pattern.any voor de titel van het formulier
De entiteit Pattern.any kunt voor entiteiten met verschillende lengten. Deze functie werkt alleen in een patroon omdat het patroon het begin en einde van de entiteit markeert. Als u die het patroon bevat een Pattern.any, uitgepakt entiteiten foutief, gebruiken een [expliciete lijst](luis-concept-patterns.md#explicit-lists) om dit probleem te verhelpen. 

1. Selecteer **entiteiten** in de linkernavigatiebalk.

2. Selecteer **nieuwe entiteit maken**. 

3. Naam van de entiteit `FormName` met type **Pattern.any**. Voor deze specifieke zelfstudie hoeft u geen rollen toevoegen aan de entiteit.

    ![Afbeelding van dialoogvenster voor de naam van de entiteit en entiteitstype](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Een patroon dat gebruikmaakt van de Pattern.any toevoegen

1. Selecteer **patronen** van de linkernavigatiebalk.

2. Selecteer de **FindForm** bedoeld.

3. Voer een sjabloon utterance met behulp van de nieuwe entiteit `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Schermafbeelding van de sjabloon utterance met pattern.any entiteit](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Training van de app voor de nieuwe doel, de entiteit en het patroon.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Het nieuwe patroon voor het ophalen van gegevens van de vrije vorm testen
1. Selecteer **testen** van de bovenste balk te openen van het paneel test. 

2. Voer de utterance `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor entiteit en hetzelfde doel.

    ![Schermafbeelding van de sjabloon utterance met pattern.any entiteit](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    De entiteit is eerst gevonden en vervolgens het patroon wordt gevonden, die wijzen op het doel. Als u een testresultaat waarbij de entiteiten worden niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon).

4. Het paneel test sluiten door het selecteren van de **testen** knop in de bovenste navigatiebalk.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, verwijdert u de app LUIS. Om dit te doen, selecteert u het menu drie punt (...) rechts van de naam van de app in de lijst met Apps, selecteer **verwijderen**. In het pop-updialoogvenster **app verwijderen?**, selecteer **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Lijst van woordgroep gebruiken voor het verbeteren van de voorspelling](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: luis-reference-regions.md