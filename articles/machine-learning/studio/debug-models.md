---
Titel: Fouten opsporen in uw model titleSuffix: Azure Machine Learning Studio description: Klik hier voor meer informatie over het opsporen van fouten die worden geproduceerd door de modules Train Model en Score Model in Azure Machine Learning Studio.
Services: machine learning ms.service: machine learning ms.component: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 03/14/2017
---
# <a name="debug-your-model-in-azure-machine-learning-studio"></a>Fouten opsporen in uw model in Azure Machine Learning Studio

Bij het uitvoeren van een model, mag u uitvoeren in de volgende fouten:

* de [Train Model] [ train-model] module, treedt een fout 
* de [Score Model] [ score-model] module onjuiste resultaten oplevert 

In dit artikel wordt uitgelegd mogelijke oorzaken voor deze fouten.


## <a name="train-model-module-produces-an-error"></a>Module Train-Model, treedt een fout

![image1](./media/debug-models/train_model-1.png)

De [Train Model] [ train-model] Module wordt verwacht dat de twee invoeren:

1. Het type van machine learning-model uit de verzameling van modellen die worden geleverd door Azure Machine Learning.
2. Het trainen van gegevens met een opgegeven labelkolom die Hiermee geeft u de variabele om te voorspellen (de andere kolommen wordt aangenomen dat functies worden).

Deze module kunt treedt er een fout in de volgende gevallen:

1. De kolom Label is onjuist opgegeven. Dit kan gebeuren als meer dan één kolom is geselecteerd als het Label of een onjuiste kolomindex is geselecteerd. Het tweede geval zou bijvoorbeeld van toepassing als de index van een kolom van 30 wordt gebruikt met een invoergegevensset met alleen 25 kolommen.

2. De gegevensset bevat geen functie-kolommen. Bijvoorbeeld, als de invoergegevensset slechts één kolom, dat is gemarkeerd als de kolom Label heeft, wordt er zijn geen functies waarmee het-model bouwt. In dit geval de [Train Model] [ train-model] module wordt een foutmelding weergegeven.

3. De invoergegevensset (functies of Label) bevat oneindig als een waarde.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model-Module oplevert onjuiste resultaten

![image2](./media/debug-models/train_test-2.png)

In een typische experiment training/testen voor leren met supervisie de [Split Data] [ split] module deelt de oorspronkelijke gegevensset uit twee delen: een deel wordt gebruikt om het model te trainen en één onderdeel wordt gebruikt om te beoordelen hoe ook het getrainde model wordt uitgevoerd. Het getrainde model wordt vervolgens gebruikt om de testgegevens, waarna de resultaten worden geëvalueerd om te bepalen van de nauwkeurigheid van het model te beoordelen.

De [Score Model] [ score-model] module vereist twee invoeren:

1. De uitvoer van een getraind model van de [Train Model] [ train-model] module.
2. Een scoring gegevensset die verschilt van de gegevensset die wordt gebruikt voor het model te trainen.

Het is mogelijk dat zelfs als het experiment is voltooid, de [Score Model] [ score-model] module onjuiste resultaten oplevert. Verschillende scenario's kunnen ertoe leiden dat dit probleem te gebeuren:

1. Als de opgegeven naam categorische is en een regressiemodel wordt getraind op gegevens, een onjuiste uitvoer zou worden geproduceerd door de [Score Model] [ score-model] module. Dit is omdat regressie een continue antwoord-variabele vereist. In dit geval is het beter geschikt om te gebruiken een model voor classificatie. 

2. Op dezelfde manier als een classificatie-model wordt getraind op een gegevensset met getallen met drijvende komma in de kolom Label, kan er ongewenste resultaten opleveren. Dit is omdat classificatie vereist een discrete antwoord-variabele die u alleen waarden die variëren ten opzichte van een eindige en klein, set van klassen kunt.

3. Als de scoring gegevensset niet alle functies die worden gebruikt bevat voor het model te trainen de [Score Model] [ score-model] , treedt een fout.

4. Als een rij in de scoring gegevensset een ontbrekende waarde of een oneindige waarde voor het gebruik van de functies ervan bevat, de [Score Model] [ score-model] produceert geen uitvoer overeenkomt met die rij.

5. De [Score Model] [ score-model] identieke uitvoer voor alle rijen in de scoring gegevensset kan opleveren. Dit kan bijvoorbeeld gebeuren wanneer wordt geprobeerd classificatie met behulp van de beslissing Forests als het minimale aantal steekproeven per leaf-knooppunt is gekozen moet meer zijn dan het aantal voorbeelden van training beschikbaar.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

