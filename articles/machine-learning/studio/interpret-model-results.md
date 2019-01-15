---
Titel: Modelresultaten in Machine Learning Studio titleSuffix interpreteren: Azure Machine Learning Studio description: Klik hier voor meer informatie over het kiezen van de optimale parameter is ingesteld voor een algoritme gebruikt en score model uitvoer visualiseren.
Services: machine learning ms.service: machine learning ms.component: studio ms.topic: artikel

author: ericlicoding ms.author: amlstudiodocs ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 11/29/2017
---
# <a name="interpret-model-results-in-azure-machine-learning-studio"></a>Modelresultaten in Azure Machine Learning Studio interpreteren
In dit onderwerp wordt uitgelegd hoe te visualiseren en te interpreteren voorspellingsresultaten in Azure Machine Learning Studio. Nadat u hebt een model wordt getraind en voorspellingen daarboven ("het model beoordeeld') wordt uitgevoerd, moet u om te begrijpen en interpreteren van het resultaat voorspelling.



Er zijn vier belangrijke soorten machine learning-modellen in Azure Machine Learning:

* Classificatie
* Clustering
* Regressie
* Recommender systemen

De modules die worden gebruikt voor voorspelling boven op deze modellen zijn:

* [Score Model] [ score-model] -module voor de classificatie- en regressiemodellen
* [Toewijzen aan Clusters] [ assign-to-clusters] -module voor clustering
* [Score Matchbox Recommender] [ score-matchbox-recommender] voor aanbevelingssystemen

Dit document wordt uitgelegd hoe u moet interpreteren voorspellingsresultaten voor elk van deze modules. Zie voor een overzicht van deze modules [hoe u parameters kiezen voor het optimaliseren van uw algoritmen in Azure Machine Learning](algorithm-parameters-optimize.md).

Dit onderwerp bevat voorspelling interpretatie, maar niet de evaluatie van het model. Zie voor meer informatie over het evalueren van uw model [hoe modelprestaties evalueren in Azure Machine Learning](evaluate-model-performance.md).

Als u niet bekend bent met Azure Machine Learning en hulp krijgen bij het maken van een eenvoudig experiment aan de slag, raadpleeg dan [een eenvoudig experiment maken in Azure Machine Learning Studio](create-experiment.md) in Azure Machine Learning Studio.

## <a name="classification"></a>Classificatie
Er zijn twee subcategorieën van classificatie problemen:

* Problemen met slechts twee klassen (twee klassen of binaire classificatie)
* Problemen met meer dan twee klassen (ROC-indeling)

Azure Machine Learning heeft andere modules te maken met elk van deze typen classificatie, maar de methoden voor het interpreteren van hun voorspellingsresultaten zijn vergelijkbaar.

### <a name="two-class-classification"></a>Classificatie van twee klassen
**Voorbeeldexperiment**

Een voorbeeld van een probleem met twee klassen classificatie is de indeling van de iris bloemen. De taak is het classificeren van iris bloemen op basis van hun functies. De Iris-gegevensset vindt u in Azure Machine Learning is een subset van de populaire [Iris-gegevensset](http://en.wikipedia.org/wiki/Iris_flower_data_set) exemplaren van die alleen de twee bloem soorten (klassen 0 en 1). Er zijn vier functies voor elke bloem (petal length, petal width petal length en petal width).

![Schermafbeelding van de iris-experiment](./media/interpret-model-results/1.png)

Afbeelding 1. IRIS twee klassen classificatie probleem experiment

Een experiment is uitgevoerd om op te lossen dit probleem, zoals wordt weergegeven in afbeelding 1. Een tweeklasse boosted decision tree model is getraind en beoordeeld. Nu u de voorspellingsresultaten van visualiseren kunt de [Score Model] [ score-model] module door te klikken op de uitvoerpoort van de [Score Model] [ score-model] module en vervolgens te klikken op **Visualize**.

![Score model-module](./media/interpret-model-results/1_1.png)

Hiermee wordt de scoring resultaten zoals wordt weergegeven in afbeelding 2.

![Resultaten van de iris twee klassen classificatie-experiment](./media/interpret-model-results/2.png)

Afbeelding 2. Een resultaat score-model in twee klassen classificatie visualiseren

**Resultaat interpretatie**

Er zijn zes kolommen in de tabel met resultaten. Vier kolommen aan de linkerkant vindt u de vier functies. De juiste twee kolommen, Labels beoordeeld en gewaardeerd kansen, worden de voorspellingsresultaten. De kolom beoordeeld kansen toont de kans dat een bloem behoort tot het positieve class (klasse 1). Bijvoorbeeld, is het eerste getal in de kolom (0.028571) betekent dat er 0.028571 kans dat de eerste bloem deel uitmaakt van klasse 1. De kolom Scored Labels geeft de voorspelde klasse voor elke bloem. Dit is gebaseerd op de kolom kansen beoordeeld. Als de beoordeelde waarschijnlijkheid van een bloem groter zijn dan 0,5 is, wordt voorspeld als klasse 1. Anders wordt voorspeld als klasse 0.

**Publicatie van de Web service**

Nadat de voorspellingsresultaten zijn begrepen en beoordeeld, gezonde, kan het experiment worden gepubliceerd als een webservice zodat u kunt deze in verschillende toepassingen implementeren en deze om te verkrijgen, voorspellingen van de klasse op alle nieuwe iris bloem aanroepen. Zie voor informatie over het wijzigen van een trainingsexperiment in een scoring-experiment en deze publiceren als een webservice, [publiceren van de Azure Machine Learning-webservice](walkthrough-5-publish-web-service.md). Deze procedure biedt u een experiment scoring zoals wordt weergegeven in afbeelding 3.

![Schermopname van het scoring-experiment](./media/interpret-model-results/3.png)

Afbeelding 3. Het experiment iris twee klassen classificatie probleem scoren

Nu moet u om in te stellen van de invoer en uitvoer voor de webservice. De invoer is de rechterinvoerpoort van [Score Model][score-model], die de bloem Iris is uitgerust met invoer. De keuze van de uitvoer is afhankelijk van of u geïnteresseerd in de voorspelde klasse (beoordeelde label), de kans op beoordeelde of beide bent. In dit voorbeeld wordt ervan uitgegaan dat u geïnteresseerd in beide bent. U selecteert de gewenste uitvoerkolommen met een [kolommen in gegevensset selecteren] [ select-columns] module. Klik op [kolommen in gegevensset selecteren][select-columns], klikt u op **kolomkiezer lancering**, en selecteer **Scored Labels** en **Scored Kansen**. Na het instellen van de uitvoerpoort van [kolommen in gegevensset selecteren] [ select-columns] en het nogmaals uit te voeren, moet u het scoring experiment als een webservice publiceren door te klikken op **WEBSERVICE publiceren** . Het laatste experiment ziet er uit zoals in afbeelding 4.

![Het iris twee klassen classificatie-experiment](./media/interpret-model-results/4.png)

Afbeelding 4. Laatste scoring experiment van een probleem met de classificatie van twee klassen iris

Nadat u de webservice uitvoeren en enkele waarden van de functie van een test-instantie invoeren, retourneert het resultaat twee getallen. Het eerste getal is het beoordeelde label en de tweede is de kans op beoordeelde. Deze bloem wordt voorspeld als klasse 1 met 0.9655 kans.

![Test interpreteren score-model](./media/interpret-model-results/4_1.png)

![De resultaten van score](./media/interpret-model-results/5.png)

Afbeelding 5. Web service resultaat van de iris twee klassen classificatie

### <a name="multi-class-classification"></a>Meerdere klasse-classificatie
**Voorbeeldexperiment**

In dit experiment voert u een taak letter erkenning als een voorbeeld van multiklassen classificatie. De classificatie probeert te voorspellen van een bepaalde letter (klasse) op basis van bepaalde incheck-kenmerkwaarden opgehaald uit de incheck-installatiekopieën.

![Voorbeeld van de spraakherkenning letter](./media/interpret-model-results/5_1.png)

Er zijn 16 functies die zijn geëxtraheerd uit een letter incheck-installatiekopieën in de trainingsgegevens. De 26 letters vormen onze 26-klassen. Afbeelding 6 ziet u een experiment die een multiklassen classificeringsmodel voor opname van de letter trainen en voorspellen op dezelfde functieset van een test-gegevensset.

![Letter erkenning multiklassen classificatie-experiment](./media/interpret-model-results/6.png)

Afbeelding 6. Letter erkenning multiklassen classificatie probleem experiment

Visualiseren de resultaten van de [Score Model] [ score-model] module door te klikken op de uitvoerpoort van [Score Model] [ score-model] -module en vervolgens te klikken op **Visualize**, ziet u inhoud zoals wordt weergegeven in afbeelding 7.

![Score modelresultaten](./media/interpret-model-results/7.png)

Afbeelding 7. Score modelresultaten in een ROC-classificatie te visualiseren

**Resultaat interpretatie**

Kolommen aan de linkerkant 16 vertegenwoordigen de waarden van de functie van de test is ingesteld. De kolommen met namen beoordeeld kansen voor klasse "XX" zijn net als de kolom kansen gescoord in het geval van twee klassen. Ze worden weergegeven de kans dat de bijbehorende vermelding in een bepaalde klasse valt. Voor het eerste item is er bijvoorbeeld 0.003571 kans dat het is een 'A,' 0.000451 kans dat het is een "B", enzovoort. De laatste kolom (Scored Labels) is hetzelfde als Scored Labels in het geval van twee klassen. Wordt de klasse met de grootste kans op beoordeelde geselecteerd als de voorspelde klasse van de bijbehorende vermelding. Bijvoorbeeld, voor het eerste item het beoordeelde label is 'F' omdat dit de grootste kans om te worden van een 'F' (0.916995) heeft.

**Publicatie van de Web service**

U kunt ook de beoordeelde label ophalen voor elk item en de kans op het beoordeelde label. De basislogica is te vinden van de grootste kans tussen de beoordeelde kansen. Om dit te doen, moet u gebruiken de [R-Script uitvoeren] [ execute-r-script] module. De R-code wordt weergegeven in afbeelding 8, en het resultaat van het experiment wordt weergegeven in afbeelding 9.

![Voorbeeld van R-code](./media/interpret-model-results/8.png)

Afbeelding 8. R-code voor het extraheren van Scored Labels en de bijbehorende waarschijnlijkheid van de labels

![Resultaat van experiment](./media/interpret-model-results/9.png)

Afbeelding 9. Laatste scoring experiment van de letter erkenning regressieprobleem

Nadat u publiceren en de webservice uitvoeren en voer de waarden van bepaalde invoer functie, de geretourneerd resultaat eruit afbeelding 10. Deze letter incheck-met de uitgepakte 16-functies, wordt moet een "T" met 0.9715 kans voorspeld.

![Module voor de tests interpreteren score](./media/interpret-model-results/9_1.png)

![Testresultaat](./media/interpret-model-results/10.png)

Afbeelding 10. Resultaat van de Web service van multiklassen classificatie

## <a name="regression"></a>Regressie
Regressie problemen treden verschillende problemen met de classificatie. In een classificatie probleem probeert u afzonderlijke klassen, zoals welke klasse een iris-bloem te behoort voorspellen. Maar zoals u in het volgende voorbeeld van een regressieprobleem ziet, probeert u een continue variabele, zoals de prijs van een auto's voorspellen.

**Voorbeeldexperiment**

Auto's voorspellen gebruiken als uw voorbeeld voor regressie. U probeert te voorspellen de prijs van een auto op basis van de functies, waaronder maken, brandstoftype, hoofdtekst en station wheel. Het experiment wordt weergegeven in afbeelding 11.

![Auto's prijs regressie-experiment](./media/interpret-model-results/11.png)

Afbeelding 11. Auto's prijs regressie probleem experiment

Visualiseren van de [Score Model] [ score-model] -module, het resultaat ziet eruit als afbeelding 12.

![Scoring-resultaten voor auto's prijs voorspelling probleem](./media/interpret-model-results/12.png)

Afbeelding 12. Scoring resultaat voor het probleem van ' Automobile ' prijs voorspellen

**Resultaat interpretatie**

Scored Labels is de kolom met resultaten in dit scoring resultaat. De getallen zijn de voorspelde prijs voor elke auto.

**Publicatie van de Web service**

U kunt de regressie-experiment naar een webservice publiceren en deze op dezelfde manier als in de classificatie van twee klassen use-case voor auto's voorspellen aanroepen.

![Scoring-experiment voor auto's prijs regressieprobleem](./media/interpret-model-results/13.png)

Afbeelding 13. Scoring-experiment van een auto's prijs regressieprobleem

Met de webservice, het geretourneerde resultaat ziet eruit als afbeelding 14. De voorspelde prijs voor deze auto is $15,085.52.

![Interpreteren scoremodule testen](./media/interpret-model-results/13_1.png)

![Resultaten van de module score](./media/interpret-model-results/14.png)

Afbeelding 14. Web service resultaat van een auto's prijs regressieprobleem

## <a name="clustering"></a>Clustering
**Voorbeeldexperiment**

Laten we de Iris-gegevensset opnieuw gebruiken voor het bouwen van een experiment clustering. Hier kunt u filteren van de klasse labels in de gegevensset zodat deze alleen functies en kan worden gebruikt voor clustering. In deze iris use-case, geef het aantal clusters worden twee tijdens de training, wat betekent dat u zou de bloemen cluster in de twee klassen. Het experiment wordt weergegeven in afbeelding 15.

![IRIS clustering probleem experimenteren](./media/interpret-model-results/15.png)

Afbeelding 15. IRIS clustering probleem experimenteren

Clustering wijkt af van de classificatie in die de set trainingsgegevens geen grond waarheid labels op zichzelf. Clustering groepen de exemplaren van de gegevensset training in verschillende clusters. Tijdens de training label het model de vermeldingen door te leren van de verschillen tussen de functies. Daarna kan het getrainde model worden gebruikt voor het classificeren van verdere toekomstige vermeldingen. Er zijn twee onderdelen van het resultaat we geïnteresseerd in binnen een clustering probleem zijn. Het eerste deel is de set trainingsgegevens labels en de tweede is een nieuwe gegevensset met het getrainde model classificeren.

Het eerste deel van het resultaat kan worden gevisualiseerd door te klikken op de uitvoerpoort links van [Clustering Train-Model] [ train-clustering-model] en vervolgens te klikken op **Visualize**. De visualisatie wordt weergegeven in afbeelding 16.

![Clustering resultaat](./media/interpret-model-results/16.png)

Afbeelding 16. Resultaat van de set trainingsgegevens clustering visualiseren

Het resultaat van het tweede deel, nieuwe vermeldingen met het getrainde clustering-model, clustering wordt weergegeven in afbeelding 17.

![Clustering resultaat visualiseren](./media/interpret-model-results/17.png)

Afbeelding 17. Resultaat van een nieuwe gegevensset clustering visualiseren

**Resultaat interpretatie**

Hoewel de resultaten van de twee onderdelen uit verschillende experiment fasen voortvloeien, ze zien er hetzelfde en op dezelfde manier worden geïnterpreteerd. De eerste vier kolommen zijn functies. De laatste kolom, toewijzingen, is het resultaat voorspelling. De vermeldingen hetzelfde nummer toegewezen wordt voorspeld dat zich in hetzelfde cluster, dat wil zeggen, dat er overeenkomsten op een bepaalde manier (dit experiment wordt gebruikgemaakt van de standaard Euclidean afstand metrische gegevens) worden gedeeld. Omdat u het aantal clusters 2 hebt opgegeven, wordt de vermeldingen in toewijzingen zijn gelabeld 0 of 1.

**Publicatie van de Web service**

U kunt de clustering experiment naar een webservice publiceren en deze aanroepen voor voorspellingen van de dezelfde manier als in de classificatie van twee klassen use-case-clustering.

![Scoring-experiment voor clustering iris-probleem](./media/interpret-model-results/18.png)

Afbeelding 18. Scoring-experiment van een iris clustering probleem

Na het uitvoeren van de webservice, is de geretourneerde resultaten ziet eruit als afbeelding 19. Deze bloem wordt voorspeld moeten in een cluster 0.

![Test scoremodule interpreteren](./media/interpret-model-results/18_1.png)

![Resultaat van de module score](./media/interpret-model-results/19.png)

Afbeelding 19. Web service resultaat van de iris twee klassen classificatie

## <a name="recommender-system"></a>Recommender system
**Voorbeeldexperiment**

Voor recommender systemen, kunt u het probleem van de aanbeveling restaurant gebruiken als een voorbeeld: u kunt restaurants aanbeveelt voor klanten op basis van de geschiedenis van hun classificatie. De ingevoerde gegevens bestaat uit drie onderdelen:

* Restaurant beoordelingen van klanten
* Functie-klantgegevens
* Gegevens van de functie restaurant

Er zijn verschillende dingen die we met doen kunnen de [Train Matchbox Recommender] [ train-matchbox-recommender] module in Azure Machine Learning:

* Classificaties voor een bepaalde gebruiker en een item te voorspellen
* Doe aanbevelingen voor artikelen aan een bepaalde gebruiker
* Gebruikers met betrekking tot een bepaalde gebruiker zoeken
* Items die betrekking hebben op een bepaald item zoeken

U kunt kiezen wat u wilt doen door te selecteren in de vier opties in de **Recommender voorspelling soort** menu. Hier kunt u alle vier scenario's behandelen.

![Matchbox recommender](./media/interpret-model-results/19_1.png)

Een typische Azure Machine Learning-experiment voor een systeem recommender eruit als afbeelding 20. Zie voor meer informatie over het gebruik van deze system aanbevelingsmodules [Train matchbox recommender] [ train-matchbox-recommender] en [Score matchbox recommender] [ score-matchbox-recommender].

![Recommender system experiment](./media/interpret-model-results/20.png)

Afbeelding 20. Recommender system experiment

**Resultaat interpretatie**

**Classificaties voor een bepaalde gebruiker en een item te voorspellen**

Door het selecteren van **classificatie voorspelling** onder **Recommender voorspelling soort**, vraagt u het systeem recommender om te voorspellen van de classificatie voor een bepaalde gebruiker en een item. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 21.

![Resultaat van het systeem recommender--voorspelling beoordeling beoordelen](./media/interpret-model-results/21.png)

Afbeelding 21. Het resultaat van de score van het systeem recommender--waardering voorspelling visualiseren

De eerste twee kolommen zijn het item voor gebruikersgegevens-paren die is geleverd door de ingevoerde gegevens. De derde kolom is de voorspelde score van een gebruiker voor een bepaald onderdeel. In de eerste rij de klant U1048 bijvoorbeeld naar tarief restaurant 135026 voorspelde door 2.

**Doe aanbevelingen voor artikelen aan een bepaalde gebruiker**

Door het selecteren van **Item aanbeveling** onder **Recommender voorspelling soort**, vraagt u het systeem recommender items gaat aanraden aan een bepaalde gebruiker. De laatste parameter om te kiezen in dit scenario is *aanbevolen itemselectie*. De optie **uit het prioriteitsniveau van Items (voor evaluatie van model)** is voornamelijk bedoeld voor model evaluatie tijdens de training. Voor deze fase voorspelling kiezen wij **van alle Items**. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 22.

![Resultaat van de score van recommender men item aanbeveling](./media/interpret-model-results/22.png)

Afbeelding 22. Resultaat van het systeem recommender--item aanbeveling score visualiseren

De eerste dag van de zes kolommen staat voor de opgegeven gebruikers-id's om aan te bevelen items, zoals opgegeven door de ingevoerde gegevens. De vijf kolommen vertegenwoordigen de items die zijn aanbevolen voor de gebruiker in aflopende volgorde van relevantie. In de eerste rij is het meest aanbevolen restaurant voor klant U1048 bijvoorbeeld 134986, gevolgd door 135018, 134975 135021 en 132862.

**Gebruikers met betrekking tot een bepaalde gebruiker zoeken**

Door het selecteren van **gerelateerde gebruikers** onder **Recommender voorspelling soort**, vraagt u het systeem recommender om gerelateerde gebruikers aan een bepaalde gebruiker te vinden. Gerelateerde gebruikers zijn de gebruikers die vergelijkbare voorkeuren hebben. De laatste parameter om te kiezen in dit scenario is *gebruikersselectie gerelateerde*. De optie **van gebruikers die beoordeeld Items (voor evaluatie van model)** is voornamelijk bedoeld voor model evaluatie tijdens de training. Kies **van alle gebruikers** voor de fase van deze voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 23.

![Resultaat van de score van recommender systeem--Verwante gebruikers](./media/interpret-model-results/23.png)

Afbeelding 23. Scoreresultaten van het systeem recommender--Verwante gebruikers visualiseren

De eerste dag van de zes kolommen bevat de opgegeven gebruiker id's die nodig zijn om gerelateerde gebruikers te vinden zoals bepaald door de invoergegevens. De vijf kolommen opslaan van de voorspelde gerelateerde gebruikers van de gebruiker in aflopende volgorde van relevantie. In de eerste rij is de meest relevante klant voor klant U1048 bijvoorbeeld U1051, gevolgd door U1066, U1044 U1017 en U1072.

**Items die betrekking hebben op een bepaald item zoeken**

Door het selecteren van **verwante Items** onder **Recommender voorspelling soort**, vraagt u het systeem recommender om gerelateerde items aan een bepaald artikel te vinden. Verwante items zijn de items die waarschijnlijk worden leuk gevonden door dezelfde gebruiker. De laatste parameter om te kiezen in dit scenario is *die betrekking hebben de selectie van items*. De optie **uit het prioriteitsniveau van Items (voor evaluatie van model)** is voornamelijk bedoeld voor model evaluatie tijdens de training. We kiezen **van alle Items** voor de fase van deze voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als afbeelding 24 uur per dag.

![Resultaat van de score van recommender systeem--verwante items](./media/interpret-model-results/24.png)

Afbeelding 24 uur per dag. Scoreresultaten van het systeem recommender--verwante items visualiseren

De eerste dag van de zes kolommen vertegenwoordigt het opgegeven item-id's die nodig zijn om verwante items te vinden, zoals bepaald door de ingevoerde gegevens. De vijf kolommen opslaan van de voorspelde verwante items van het item in aflopende volgorde in termen van relevantie. In de eerste rij is het meest relevant item voor item 135026 bijvoorbeeld 135074, gevolgd door 135035, 132875 135055 en 134992.

**Publicatie van de Web service**

Het proces van de publicatie van deze experimenten als webservices om op te halen van voorspellingen is vergelijkbaar voor elk van de vier scenario's. We nemen hier het tweede scenario (aanbevolen items aan een bepaalde gebruiker) als voorbeeld. U kunt dezelfde procedure met de andere drie volgen.

Het systeem getrainde recommender opslaan als een getraind model en filteren de invoergegevens naar een enkele gebruiker-ID-kolom, zoals aangevraagd, kunt u het experiment zoals in afbeelding 25 koppelt en publiceren als een webservice.

![Scoring-experiment van het probleem van de aanbeveling restaurant](./media/interpret-model-results/25.png)

Afbeelding 25. Scoring-experiment van het probleem van de aanbeveling restaurant

Met de webservice, het geretourneerde resultaat ziet eruit als afbeelding 26. De vijf aanbevolen restaurants voor gebruiker U1048 zijn 134986, 135018, 134975, 135021 en 132862.

![Voorbeeld van recommender systeemservice](./media/interpret-model-results/25_1.png)

![Voorbeeldresultaten van experiment](./media/interpret-model-results/26.png)

Afbeelding 26. Resultaat van de Web service restaurant aanbeveling probleem

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
