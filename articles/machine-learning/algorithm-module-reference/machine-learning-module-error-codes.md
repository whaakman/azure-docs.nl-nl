---
title: Module-fouten oplossen
titleSuffix: Azure Machine Learning service
description: Module-uitzonderingen in Azure Machine Learning Studio en gebruik foutcodes oplossen
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 5ce3d86d37940f66065debce30b348371b7aa6af
ms.sourcegitcommit: 3107874d7559ea975e4d55ae33cdf45f4b5485e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67568250"
---
# <a name="exceptions-and-error-codes-for-algorithm--module-reference"></a>Uitzonderingen en foutcodes voor algoritme en Module-verwijzing

Meer informatie over de foutberichten en uitzondering codes die optreden met behulp van modules in Azure Machine Learning Studio. 

Los het probleem, zoek naar de fout in dit artikel voor meer informatie over algemene oorzaken. Er zijn twee manieren om de volledige tekst van een foutbericht weergegeven in de Studio:  
 
- Klik op de koppeling **logboek van de uitvoer weergeven**, in het rechter deelvenster en Ga naar de onderkant. Het gedetailleerde foutbericht wordt weergegeven in de laatste twee regels van het venster.  
  
- Selecteer de module die de fout is en klik op de rode X. Alleen de tekst van de relevante fout wordt weergegeven.  
  
Als de tekst van foutberichten niet handig is, stuur ons informatie over de context en de gewenste toevoegingen of wijzigingen. U kunt feedback indienen over de fout onderwerp of Ga naar de [forum van Azure Machine Learning STUDIO](https://aka.ms/aml-forum-studio) en een vraag stellen.  


## <a name="error-0001"></a>Fout 0001  
 Er treedt op als een of meer opgegeven kolommen van de gegevensset kunnen niet worden gevonden.  
  
 U ontvangt deze fout als een kolomselectie is gemaakt voor een module, maar de geselecteerde kolommen bestaan niet in de invoer-gegevensset. Deze fout kan optreden als u handmatig hebt getypt in een kolomnaam of als de kolomkiezer een voorgestelde kolom die zich niet in uw gegevensset bij het uitvoeren van het experiment heeft geleverd.  
  
**Oplossing:** Terug naar de module die deze uitzondering veroorzaakt en valideert u of de kolomnaam of namen juist zijn en dat alle kolommen waarnaar wordt verwezen bestaan.  
  
|Uitzondering berichten|  
|------------------------|  
|Een of meer opgegeven kolommen zijn niet gevonden|  
|Kolom met de naam of index "{0}' niet gevonden|  
|Kolom met de naam of index "{0}'bestaat niet in'{1}"|  
 

## <a name="error-0002"></a>Fout 0002  
 Er treedt op als een of meer parameters niet kunnen worden geparseerd of geconverteerde van opgegeven type in vereist voor methode doeltype.  
  
 Deze fout treedt op in Azure Machine Learning wanneer u een parameter als invoer opgeven en het waardetype verschilt van het type dat wordt verwacht en impliciete conversie kan niet worden uitgevoerd.  
  
**Oplossing:** De module-vereisten controleren en te bepalen welk waardetype is vereist (string, integer, double, enz.)  
  
|Uitzondering berichten|  
|------------------------|  
|Kan niet worden geparseerd parameter|  
|Kan niet worden geparseerd "{0}" parameter|  
|Kan niet worden geparseerd (omzetten) "{0}"parameter"{1}"|  
|Kan niet worden geconverteerd "{0}'parameter van'{1}'naar'{2}"|  
|Kan niet worden geconverteerd "{0}'waarde voor parameter'{1}'van'{2}'naar'{3}"|  
|Kan niet converteren van waarde "{0}'in de kolom'{1}'van'{2}'naar'{3}"bij het gebruik van de indeling'{4}' opgegeven|  
  

## <a name="error-0003"></a>Fout 0003  
 Er treedt op als een of meer van de invoer zijn null of leeg zijn.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als invoer of parameters aan een module null of leeg zijn zijn.  Deze fout kan bijvoorbeeld optreden wanneer u niet in een willekeurige waarde voor een parameter hebt getypt. Het kan ook gebeuren als u een gegevensset met ontbrekende waarden of een lege gegevensset hebt gekozen.  
  
**Oplossing:**
 
+ Open de module die de uitzondering heeft geproduceerd en controleer of alle invoer hebt opgegeven. Zorg ervoor dat alle vereiste invoer zijn opgegeven. 
+ Zorg ervoor dat de gegevens die worden geladen uit Azure storage toegankelijk is en dat de accountnaam of de sleutel niet is gewijzigd.  
+ Controleer de ingevoerde gegevens voor ontbrekende waarden of null-waarden.
+ Als u een query op een gegevensbron gebruikt, controleert u of dat gegevens in de indeling die u verwacht dat wordt geretourneerd. 
+ Controleer of typfouten of andere wijzigingen in de specificatie van de gegevens.
  
|Uitzondering berichten|  
|------------------------|  
|Een of meer van de invoer zijn null of leeg zijn|  
|Invoer "{0}" is null of leeg zijn|  
  

## <a name="error-0004"></a>Fout 0004  
 Er treedt op als parameter kleiner dan of gelijk zijn aan specifieke waarde is.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de parameter in het bericht lager dan de grenswaarde van een vereist voor de module is voor het verwerken van de gegevens.  
  
**Oplossing:** Terug naar de module die de uitzondering veroorzaakt en wijzig de parameter niet groter zijn dan de opgegeven waarde.  
  
|Uitzondering berichten|  
|------------------------|  
|Parameter moet groter zijn dan grenswaarde.|  
|Parameter '{0}' waarde moet groter zijn dan {1}.|  
|Parameter '{0}'heeft de waarde'{1}' die moet groter zijn dan {2}|  
  


## <a name="error-0005"></a>Fout 0005  
 Er treedt op als parameter kleiner dan een specifieke waarde is.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de parameter in het bericht kleiner dan is of gelijk is aan een grenswaarde die is vereist voor de module voor het verwerken van de gegevens.  
  
**Oplossing:** Terug naar de module die de uitzondering veroorzaakt en wijzig de parameter groter is dan of gelijk zijn aan de opgegeven waarde.  
  
|Uitzondering berichten|  
|------------------------|  
|Parameter moet groter zijn dan of gelijk zijn aan de grenswaarde.|  
|Parameter '{0}' waarde moet groter zijn dan of gelijk zijn aan {1}.|  
|Parameter '{0}'heeft de waarde'{1}"die moet groter zijn dan of gelijk zijn aan {2}.|  
  

## <a name="error-0006"></a>Fout 0006  
 Er treedt op als parameter groter dan of gelijk zijn aan de opgegeven waarde is.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de parameter in het bericht is groter dan of gelijk zijn aan een grenswaarde die is vereist voor de module voor het verwerken van de gegevens.  
  
**Oplossing:** Terug naar de module die de uitzondering veroorzaakt en wijzig de parameter kleiner zijn dan de opgegeven waarde.  
  
|Uitzondering berichten|  
|------------------------|  
|Parameters komt niet overeen. Een van de parameters moet kleiner zijn dan het andere.|  
|Parameter '{0}'waarde moet kleiner zijn dan de parameter '{1}' waarde.|  
|Parameter '{0}'heeft de waarde'{1}"die moet minder dan {2}.|  
  

## <a name="error-0007"></a>Fout 0007  
 Er treedt op als parameter groter is dan een specifieke waarde.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als in de eigenschappen voor de module die u hebt opgegeven een waarde die groter is dan is toegestaan. Bijvoorbeeld, kunt u een gegevens die zich buiten het bereik van ondersteunde datums opgeven of u kunt aangeven dat vijf kolommen worden gebruikt wanneer er slechts drie kolommen beschikbaar zijn. 
 
 U ziet deze fout kan ook als u twee gegevenssets die overeen moeten komen met op een bepaalde manier opgeeft. Bijvoorbeeld, als u kolommen naam en geef de kolommen met de index, het aantal namen die u opgeeft moet overeenkomen met het aantal kolomindexen. Een ander voorbeeld is mogelijk een rekenkundige bewerking die gebruikmaakt van twee kolommen, waarbij de kolommen hetzelfde aantal rijen moeten hebben. 
  
**Oplossing:**
 
 + Open de module in kwestie en controleert u alle instellingen van de numerieke eigenschap op.
 + Zorg ervoor dat elke parameterwaarden binnen het ondersteunde bereik van waarden voor die eigenschap vallen.
 + Als de module meerdere invoergegevens, zorg ervoor dat invoer van dezelfde grootte.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Controleer of de gegevensset of de gegevensbron is gewijzigd. Een waarde die met een eerdere versie van de gegevens hebt gewerkt, soms mislukt nadat het aantal kolommen, de gegevenstypen van de kolom of de grootte van de gegevens is gewijzigd.  
  
|Uitzondering berichten|  
|------------------------|  
|Parameters komt niet overeen. Een van de parameters moet kleiner dan of gelijk zijn aan een andere.|  
|Parameter '{0}'waarde moet kleiner dan of gelijk zijn aan de parameter'{1}' waarde.|  
|Parameter '{0}'heeft de waarde'{1}"die moet kleiner zijn dan of gelijk zijn aan {2}.|  
  

## <a name="error-0008"></a>Fout 0008  
 Er treedt op als parameter niet binnen het bereik is.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de parameter in het bericht buiten de grenzen die vereist zijn voor de module is voor het verwerken van de gegevens.  
  
 Bijvoorbeeld: deze fout wordt weergegeven als u probeert te gebruiken [rijen toevoegen](add-rows.md) twee gegevenssets waarvoor een verschillend aantal kolommen te combineren.  
  
**Oplossing:** Terug naar de module die de uitzondering veroorzaakt en wijzig de parameter worden binnen het opgegeven bereik.  
  
|Uitzondering berichten|  
|------------------------|  
|Parameterwaarde is niet in het opgegeven bereik.|  
|Parameter '{0}' waarde is niet binnen het bereik.|  
|Parameter '{0}' waarde moet binnen het bereik van [{1}, {2}].|  
  

## <a name="error-0009"></a>Fout 0009  
 Er treedt op wanneer het Azure storage-accountnaam of de containernaam is onjuist opgegeven.  
  
Deze fout treedt op in Azure Machine Learning Studio wanneer u de parameters voor een Azure storage-account opgeven, maar de naam of het wachtwoord kan niet worden omgezet. Fouten op wachtwoorden of accountnamen kunnen gebeuren om verschillende redenen:
 
 + Het account heeft een onjuist type. Sommige nieuwe accounttypen worden niet ondersteund voor gebruik met Machine Learning Studio. Zie [importgegevens](import-data.md) voor meer informatie.
 + U hebt ingevoerd met de naam van het onjuiste
 + Het account bestaat niet meer
 + Het wachtwoord voor het opslagaccount is onjuist of is gewijzigd
 + U hebt opgegeven dat de containernaam van de of de container bestaat niet
 + U niet hebt het pad (pad naar de blob) volledig opgeven
   
**Oplossing:**

Dergelijke problemen optreden vaak wanneer u de naam, het wachtwoord of container het pad van het handmatig invoeren. Het is raadzaam dat u de nieuwe wizard voor de [importgegevens](import-data.md) -module, waarmee u kunt opzoeken en namen controleren.

Controleer ook of de account, container of blob is verwijderd. Gebruik een ander Azure-opslag-hulpprogramma om te controleren of de accountnaam en het wachtwoord juist hebt ingevoerd en of de container bestaat. 

Sommige nieuwere accounttypen worden niet ondersteund door Azure Machine Learning. Bijvoorbeeld, de nieuwe 'warm' of 'koude' opslagtypen kunnen niet worden gebruikt voor machine learning. Klassieke opslagaccounts en opslagaccounts die zijn gemaakt als 'Algemeen' goed werkt.

Als het volledige pad naar een blob is opgegeven, controleert u of dat het pad is opgegeven als **container/blobname**, en dat zowel de container en de blob bestaat in het account.  
  
 Het pad mag geen een toonaangevende slash bevatten. Bijvoorbeeld **blob/container/** is onjuist en moet worden opgegeven als **container/blob**.  

  
|Uitzondering berichten|  
|------------------------|  
|De Azure storage-accountnaam of de containernaam is onjuist.|  
|De Azure storage-accountnaam "{0}'of de containernaam van de '{1}' is onjuist; een containernaam zijn van de indeling van de container/blob werd verwacht.|  
  

## <a name="error-0010"></a>Fout 0010  
 Er treedt op als invoergegevenssets kolomnamen die moeten overeenkomen met maar niet hebt.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de kolomindex in het bericht verschillende kolomnamen in de twee gegevenssets voor invoer heeft.  
  
**Oplossing:** Gebruik [metagegevens bewerken](edit-metadata.md) of wijzigen van de oorspronkelijke gegevensset met de naam van de dezelfde kolom voor de index van de opgegeven kolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolommen met de bijbehorende index in invoergegevenssets hebben verschillende namen.|  
|Kolomnamen zijn niet hetzelfde zijn voor de kolom {0} (op nul gebaseerd) invoergegevenssets ({1} en {2} respectievelijk).|  
  

## <a name="error-0011"></a>Fout 0011  
 Uitzondering optreedt als doorgegeven kolom set argument is niet van toepassing op elk van de kolommen van de gegevensset.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de selectie van de opgegeven kolom komt niet overeen met een van de kolommen in de opgegeven gegevensset.  
  
 U kunt ook deze foutmelding krijgen als u een kolom nog niet hebt geselecteerd en ten minste één kolom is vereist voor de module om te werken.  
  
**Oplossing:** Wijzig de kolomselectie in de module zodat deze van toepassing op de kolommen in de gegevensset.  
  
 Als de module is vereist dat u een specifieke kolom selecteert, zoals een labelkolom, controleert u of dat de juiste kolom is geselecteerd.  
  
 Als ongepast kolommen zijn geselecteerd, wordt deze verwijderen en het experiment opnieuw uitvoeren.  
  
|Uitzondering berichten|  
|------------------------|  
|Opgegeven kolomset geldt niet voor alle kolommen van de gegevensset.|  
|Opgegeven kolomset "{0}" geldt niet voor alle kolommen van de gegevensset.|  
  

## <a name="error-0012"></a>Fout 0012  
 Er treedt op als de instantie van klasse kan niet worden gemaakt met doorgegeven set argumenten.  
  
**Oplossing:** Deze fout is geen actie worden uitgevoerd door de gebruiker en wordt afgeschaft in een toekomstige release.  
  
|Uitzondering berichten|  
|------------------------|  
|Ongetrainde model, eerst model te trainen.|  
|Ongetrainde model ({0}), gebruikt u getrainde model.|  
  

## <a name="error-0013"></a>Fout 0013  
 Er treedt op als de cursist doorgegeven aan de module een ongeldig type is.  
  
 Deze fout treedt op wanneer een getraind model niet compatibel met de verbonden scoremodule is. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the experiment is run.  -->
  
**Oplossing:**

Bepaalt het type van de cursist die wordt geproduceerd door de trainingsmodule en de scoring-module die geschikt is voor de cursist bepalen. 

Als het model is getraind met een van de speciale training-modules, verbinding maken met het getrainde model alleen voor de bijbehorende gespecialiseerde scoremodule. 


|Modeltype|Trainingsmodule| Scoring-module|
|----|----|----|
|een classificatie|[Train Model](train-model.md) |[Score-Model](score-model.md)|
|een regressiemodel|[Train Model](train-model.md) |[Score-Model](score-model.md)|

<!--| clustering models| [Train Clustering Model](train-clustering-model.md) or [Sweep Clustering](sweep-clustering.md)| [Assign Data to Clusters](assign-data-to-clusters.md)|
| anomaly detection - One-Class SVM | [Train Anomaly Detection Model](train-anomaly-detection-model.md) |[Score Model](score-model.md)|
| anomaly detection - PCA |[Train Model](train-model.md) |[Score Model](score-model.md) </br> Some additional steps are required to evaluate the model. |
| anomaly detection - time series|  [Time Series Anomaly Detection](time-series-anomaly-detection.md) |Model trains from data and generates scores. The module does not create a trained learner and no additional scoring is required. |
| recommendation model| [Train Matchbox Recommender](train-matchbox-recommender.md) | [Score Matchbox Recommender](score-matchbox-recommender.md) |
| image classification | [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) | [Score Model](score-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) | [Score Vowpal Wabbit Version 7-4 Model](score-vowpal-wabbit-version-7-4-model.md) |   
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-10 Model](train-vowpal-wabbit-version-7-10-model.md) | [Score Vowpal Wabbit Version 7-10 Model](score-vowpal-wabbit-version-7-10-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) | [Score Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) |-->
  
|Uitzondering berichten|  
|------------------------|  
|Learner van het ongeldige type wordt doorgegeven.|  
|Learner "{0}" heeft een ongeldig type.|  


## <a name="error-0014"></a>Fout 0014  
 Er treedt op als het aantal unieke waarden van kolom groter is dan toegestaan.  
  
 Deze fout treedt op wanneer een kolom te veel unieke waarden bevat.  Bijvoorbeeld, u kunt deze fout mogelijk ziet als u opgeeft dat een kolom als categorische gegevens worden verwerkt, maar er zijn te veel unieke waarden in de kolom om toe te staan van de bewerking is voltooid. U ziet deze fout kan ook als er een discrepantie tussen het aantal unieke waarden in de twee invoeren.   
  
**Oplossing:**

Open de module die de fout is gegenereerd en identificeren van de kolommen die worden gebruikt als invoer. Voor sommige modules, u kunt met de rechtermuisknop op de gegevensset invoer- en selecteer **Visualize** om op te halen van statistieken op afzonderlijke kolommen, met inbegrip van het aantal unieke waarden en de verdeling ervan.

Voor een kolom die u wilt gebruiken voor de groepering of categorisatie, nemen stappen voor het verminderen van het aantal unieke waarden in de kolommen. U kunt op verschillende manieren, afhankelijk van het gegevenstype van de kolom verminderen. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> Kan niet een oplossing te vinden die overeenkomt met uw scenario? U kunt feedback geven over dit onderwerp met de naam van de module die de fout en de gegevens van het type gegenereerd en de kardinaliteit van de kolom. We gebruiken de informatie voor meer gerichte stappen voor algemene scenario's voor probleemoplossing.   
  
|Uitzondering berichten|  
|------------------------|  
|Aantal unieke waarden van kolom is groter dan toegestaan.|  
|Aantal unieke waarden in kolom: '{0}"tuple aantal overschrijdt {1}.|  
  

## <a name="error-0015"></a>Fout 0015  
 Er treedt op als de verbinding met database is mislukt.  
  
 U ontvangt deze foutmelding als u een onjuiste naam van de SQL-account, wachtwoord, database-server of de naam van database, of als een verbinding met de database kan niet worden gemaakt vanwege problemen met de database of de server.  
  
**Oplossing:** Controleer of de accountnaam, het wachtwoord, de database-server en de database correct hebt ingevoerd en of het opgegeven account heeft het juiste machtigingsniveau. Controleer of de database is momenteel toegankelijk.  
  
|Uitzondering berichten|  
|------------------------|  
|Fout bij het maken van een databaseverbinding.|  
|Fout bij het maken van verbinding met database: {0}.|  
  


## <a name="error-0016"></a>Fout 0016  
 Er treedt op als invoergegevenssets doorgegeven aan de module moet compatibel kolomtypen hebt, maar niet.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de typen van de kolommen die wordt doorgegeven in twee of meer gegevenssets niet compatibel met elkaar zijn.  
  
**Oplossing:** Gebruik [metagegevens bewerken](edit-metadata.md) of wijzigen van de oorspronkelijke gegevensset voor invoer<!--, or use [Convert to Dataset](convert-to-dataset.md)--> om ervoor te zorgen dat de typen van de kolommen compatibel zijn.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolommen met de bijbehorende index in invoergegevenssets hebben incompatibele typen.|  
|Kolommen {0} en {1} zijn niet compatibel.|  
|Kolom elementtypen zijn niet compatibel voor kolom {0} (op nul gebaseerd) invoergegevenssets ({1} en {2} respectievelijk).|  
  

## <a name="error-0017"></a>Fout 0017  
 Er treedt op als een gegevenstype dat niet wordt ondersteund door de huidige module maakt gebruik van een geselecteerde kolom.  
  
 Bijvoorbeeld, kunt u dit foutbericht in Azure Machine Learning als uw kolomselectie bevat een kolom met een gegevenstype dat kan niet worden verwerkt door de module, zoals een kolom met tekenreeksen voor een rekenkundige bewerking of een kolom score waar een categorische functie-kolom is Vereist.  
  
**Oplossing:**
 1. De kolom die het probleem identificeren.
 2. Bekijk de vereisten van de module.
 3. De kolom zodat deze voldoen aan vereisten wijzigen. U moet mogelijk te wijzigen, afhankelijk van de kolom en de conversie die u probeert een combinatie van verschillende van de volgende modules:
    + Gebruik [metagegevens bewerken](edit-metadata.md) naar het gegevenstype van kolommen wijzigen of te wijzigen van het kolomgebruik van de functie in numeriek, categorische naar niet-categorische, enzovoort.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Als een laatste toevlucht moet u mogelijk de oorspronkelijke invoergegevensset wijzigen.

> [!TIP]
> Kan niet een oplossing te vinden die overeenkomt met uw scenario? U kunt feedback geven over dit onderwerp met de naam van de module die de fout en de gegevens van het type gegenereerd en de kardinaliteit van de kolom. We gebruiken de informatie voor meer gerichte stappen voor algemene scenario's voor probleemoplossing. 
  
|Uitzondering berichten|  
|------------------------|  
|Kolom van het huidige type kan niet worden verwerkt. Het type wordt niet ondersteund door de module.|  
|Kolom van het type kan niet worden verwerkt {0}. Het type wordt niet ondersteund door de module.|  
|Kan de kolom niet verwerken. "{1}' van het type {0}. Het type wordt niet ondersteund door de module.|  
|Kan de kolom niet verwerken. "{1}' van het type {0}. Het type wordt niet ondersteund door de module. Parameternaam: {2}|  
  

## <a name="error-0018"></a>Fout 0018  
 Er treedt op als invoergegevensset niet geldig is.  
  
**Oplossing:** Deze fout in Azure Machine Learning kan worden weergegeven in de context van veel dus er is niet een enkele oplossing. De fout geeft in het algemeen aan dat de gegevens die zijn opgegeven als invoer voor een module heeft het verkeerde aantal kolommen of het gegevenstype komt niet overeen met de vereisten van de module. Bijvoorbeeld:  
  
-   De module is een labelkolom vereist, maar er is geen kolom is gemarkeerd als een label of u een labelkolom nog niet hebt geselecteerd.  
  
-   De module vereist dat gegevens categorische worden, maar uw gegevens numeriek is.  
  
<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->
  
-   De gegevens heeft de verkeerde indeling.  
  
-   Geïmporteerde gegevens bevat ongeldige tekens, onjuiste waarden, of buiten het bereikwaarden.  
-   De kolom is leeg of bevat te veel waarden ontbreken.  
  
 Om te bepalen van de vereisten en hoe uw gegevens mogelijk, Controleer het help-onderwerp voor de module die de gegevensset als invoer gaan gebruiken.  
  
 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  
  
|Uitzondering berichten|  
|------------------------|  
|Gegevensset is niet geldig.|  
|{0} Ongeldige gegevens bevat.|  
|{0} en {1} consistente kolom verstandig moet zijn.|  
  

## <a name="error-0019"></a>Fout 0019  
 Er treedt op als kolom wordt verwacht dat gesorteerde waarden bevatten, maar die niet bestaat.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als de waarden van de opgegeven kolom niet de juiste volgorde zijn.  
  
**Oplossing:** Handmatig wijzigen van de invoergegevensset van de kolomwaarden sorteren en de module opnieuw.  
  
|Uitzondering berichten|  
|------------------------|  
|In de kolom worden niet gesorteerd.|  
|In de kolom '{0}' niet worden gesorteerd.|  
|In de kolom '{0}'van de gegevensset'{1}' niet worden gesorteerd.|  
  

## <a name="error-0020"></a>Fout 0020  
 Er treedt op als het aantal kolommen in enkele van de gegevenssets die zijn doorgegeven aan de module is te klein.  
  
 U ontvangt deze fout in Azure Machine Learning als dat niet genoeg kolommen zijn geselecteerd voor een module.  
  
**Oplossing:** Terug naar de module en zorg ervoor dat kolomkiezer heeft juiste aantal geselecteerde kolommen.  
  
|Uitzondering berichten|  
|------------------------|  
|Aantal kolommen in de invoergegevensset is kleiner dan het toegestane minimum.|  
|Aantal kolommen in de invoergegevensset is lager dan het toegestane minimum van {0} kolom(men).|  
|Aantal kolommen in de invoergegevensset "{0}' is lager dan het toegestane minimum van {1} kolom(men).|

## <a name="error-0021"></a>Fout 0021  
 Er treedt op als het aantal rijen in enkele van de gegevenssets die zijn doorgegeven aan de module is te klein.  
  
 Deze fout in gezien in Azure Machine Learning wanneer er niet genoeg rijen in de gegevensset in de opgegeven bewerking niet uitvoeren. Bijvoorbeeld, ziet u deze fout als de invoergegevensset is leeg of als u probeert een bewerking waarvoor sommige minimumaantal rijen om geldig te zijn uit te voeren. Deze bewerkingen kunnen bevatten (maar niet beperkt tot) groepering of classificatie op basis van statistische methoden, bepaalde typen binning en leren met tellingen.  
  
**Oplossing:**
 
 + Open de module die de fout is geretourneerd, en controleer de eigenschappen voor de gegevensset en module invoer. 
 + Controleer of de invoergegevensset is niet leeg zijn en of er voldoende rijen met gegevens om te voldoen aan de vereisten die worden beschreven in de help van de module.  
 + Als uw gegevens zijn geladen uit een externe bron, zorg ervoor dat de gegevensbron beschikbaar is en er is geen fout of wijzigen in het gegevensdefinitie van de die ertoe kan leiden dat het importproces minder rijen ophalen.
 + Als u een bewerking op de gegevens upstream van de module die mogelijk van invloed zijn op het type gegevens of het aantal waarden, zoals het opruimen, splitsen, of join-bewerkingen uitvoert, controleert u de uitvoer van deze bewerkingen om te bepalen het aantal rijen dat wordt geretourneerd.  



## <a name="error-0022"></a>Fout 0022  
 Er treedt op als het aantal geselecteerde kolommen in de invoergegevensset is niet gelijk aan het verwachte aantal.  
  
 Deze fout in Azure Machine Learning kan optreden als de downstream-module of de bewerking een specifiek aantal kolommen of invoer vereist, en u hebt opgegeven te weinig of te veel kolommen of invoer. Bijvoorbeeld:  
  
-   U geeft een enkelvoudig labelkolom of een sleutelkolom en meerdere kolommen per ongeluk hebt geselecteerd.  
  
-   U bent hernoemen van kolommen, maar meer of minder namen opgegeven dan er kolommen zijn.  
  
-   Het aantal kolommen in de bron- of is gewijzigd of komt niet overeen met het aantal kolommen die door de module wordt gebruikt.  
  
-   U hebt een door komma's gescheiden lijst met waarden opgegeven voor de invoer, maar komt niet overeen met het aantal waarden of meerdere invoergegevens worden niet ondersteund.  
  
**Oplossing:** Terug naar de module en controleer de kolomselectie van de om ervoor te zorgen dat het juiste aantal tabelkolommen is geselecteerd. Controleer of de uitvoer van de upstream-modules en de vereisten van de downstream-bewerkingen.  
  
 Als u een van de opties voor de selectie van kolommen die meerdere kolommen (kolomindex, alle functies, alle numerieke, enz.) kunnen selecteren gebruikt, controleert u het exacte aantal kolommen geretourneerd door de selectie.  
  
 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->
  
 Controleer of het getal of een upstream kolommen niet is gewijzigd.  
  
 Als u een aanbeveling-gegevensset naar een model te trainen, houd er rekening mee dat de recommender wordt verwacht een beperkt aantal kolommen dat, overeenkomt met het item voor gebruikersgegevens paren of gebruiker-item-classificaties. Extra kolommen voordat u het model te trainen of aanbeveling gegevenssets splitsen verwijderen. Zie voor meer informatie, [Split Data](split-data.md).  
  
|Uitzondering berichten|  
|------------------------|  
|Aantal geselecteerde kolommen in de invoergegevensset is niet gelijk aan het verwachte aantal.|  
|Aantal geselecteerde kolommen in de invoergegevensset is niet gelijk aan {0}.|  
|Kolom selectie patroon '{0}' geeft het aantal geselecteerde kolommen in de invoergegevensset die niet gelijk zijn aan {1}.|  
|Kolom selectie patroon '{0}' wordt verwacht voor {1} kolommen geselecteerd in de invoergegevensset, maar {2} kolom(men) is/zijn opgegeven.|  



## <a name="error-0023"></a>Fout 0023  
 Er treedt op als de doelkolom van invoergegevensset is niet geldig voor de huidige trainer-module.  
  
 Deze fout in Azure Machine Learning treedt op als de doelkolom (zoals geselecteerd in de parameters van de module) niet van het geldig gegevenstype is, die alle ontbrekende waarden of geen categorische is zoals verwacht.  
  
**Oplossing:** Terug naar de module invoer om te controleren van de inhoud van het label/doelkolom. Zorg ervoor dat er geen alle ontbrekende waarden. Als de module is doelkolom categorische worden verwacht, zorg ervoor dat er meer dan één afzonderlijke waarden in de doelkolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Invoergegevensset bevat niet-ondersteunde doelkolom.|  
|Invoergegevensset bevat niet-ondersteunde doelkolom '{0}'.|  
|Invoergegevensset bevat niet-ondersteunde doelkolom "{0}' voor learner van het type {1}.|  
 

## <a name="error-0024"></a>Fout 0024  
Er treedt op als de gegevensset bevat de kolom van een label.  

 Deze fout in Azure Machine Learning treedt op wanneer de module is vereist voor een labelkolom en de gegevensset heeft geen een labelkolom. Bijvoorbeeld, moet evaluatie van een gegevensset beoordeelde meestal een labelkolom aanwezig zijn voor het berekenen van de nauwkeurigheid van metrische gegevens.  
 
Het kan ook gebeuren dat een labelkolom aanwezig in de gegevensset is, maar niet correct gedetecteerd door Azure Machine Learning.
  
**Oplossing:**

+ Open de module die de fout is gegenereerd en bepalen of een labelkolom aanwezig is. De naam of het gegevenstype van de kolom niet van belang, zolang de kolom bevat één resultaat (of afhankelijke variabele) die u probeert te voorspellen. Als u niet zeker weet welke kolom heeft het label, zoek naar een algemene naam zoals *klasse* of *doel*. 
+  Als de gegevensset niet onder de kolom van een label, is het mogelijk dat de labelkolom is expliciet of per ongeluk verwijderd upstream. Dit kan ook worden de gegevensset is niet de uitvoer van een upstream scoremodule.
+ Als u wilt markeren expliciet de kolom als de labelkolom, voeg de [metagegevens bewerken](edit-metadata.md) module en maak verbinding van de gegevensset. Selecteer alleen de kolom van het label en selecteer **Label** uit de **velden** vervolgkeuzelijst. 
+ Als de verkeerde kolom als het label is gekozen, kunt u selecteren **duidelijke label** uit de **velden** om op te lossen van de metagegevens voor de kolom. 
  
|Uitzondering berichten|  
|------------------------|  
|Er is geen labelkolom in de gegevensset.|  
|Er is geen labelkolom in '{0}'.|  
  

## <a name="error-0025"></a>Fout 0025  
 Er treedt op als gegevensset geen een score-kolom bevat.  
  
 Deze fout in Azure Machine Learning treedt op als invoer voor de evaluate-model geen geldige score bevat kolommen. De gebruiker wil bijvoorbeeld een gegevensset te evalueren voordat deze is beoordeeld met een juiste getrainde model of de kolom score expliciet upstream is verwijderd. Deze uitzondering vindt ook plaats wanneer de score kolommen voor de twee gegevenssets niet compatibel zijn. Bijvoorbeeld, u mogelijk wilt vergelijken van de nauwkeurigheid van een lineaire regressor zijn met die van een binaire classificatie.  
  
**Oplossing:** Terug naar de invoer voor het model evalueren en te controleren of deze een of meer score-kolommen bevat. Als dat niet het geval is, wordt de gegevensset is niet beoordeeld of de score-kolommen zijn verwijderd in een upstream-module.  
  
|Uitzondering berichten|  
|------------------------|  
|Er is geen kolom score in de gegevensset.|  
|Er is geen kolom score in '{0}'.|  
|Er is geen kolom score in '{0}' die wordt geproduceerd door een '{1}'. De gegevensset met behulp van het juiste type cursist te beoordelen.|  
  

## <a name="error-0026"></a>Fout 0026  
 Er treedt op als kolommen met dezelfde naam zijn niet toegestaan.  
  
 Deze fout in Azure Machine Learning treedt op als u meerdere kolommen dezelfde naam hebben. Een manier die u kunt deze fout kan optreden is als de gegevensset heeft geen een rij met koppen en de namen van kolommen worden automatisch toegewezen: Col0, Col1, enzovoort.  
  
**Oplossing:** Als u kolommen dezelfde naam hebben, plaatst u een [metagegevens bewerken](edit-metadata.md) module tussen de invoergegevensset en de module. Gebruik de kolomkiezer in [metagegevens bewerken](edit-metadata.md) om kolommen te wijzigen, te selecteren te typen van de nieuwe namen in de **nieuwe kolomnamen** tekstvak.  
  
|Uitzondering berichten|  
|------------------------|  
|Gelijk kolomnamen zijn opgegeven in de argumenten. Gelijk kolomnamen zijn niet toegestaan door de module.|  
|Gelijk aan de namen van kolommen in de argumenten '{0}'en'{1}' zijn niet toegestaan. Geef verschillende namen.|  
  

## <a name="error-0027"></a>Fout 0027  
 Er treedt een in het geval wanneer twee objecten moeten van dezelfde grootte hebben, maar niet op.  
  
 Dit is een algemene fout in Azure Machine Learning en kan worden veroorzaakt door veel voorwaarden.  
  
**Oplossing:** Er is geen specifieke oplossing. Echter, kunt u voorwaarden, zoals het volgende controleren:  
  
-   Als u kolommen, zorg ervoor dat elke lijst (de invoerkolommen en de lijst met nieuwe namen van) hetzelfde aantal items heeft.  
  
-   Als u wilt toevoegen of samenvoegen van twee gegevenssets, zorg er dan voor dat ze hetzelfde schema hebben.  
  
-   Als u twee gegevenssets waarvoor meerdere kolommen, zorg ervoor dat de sleutelkolommen dezelfde gegevens typt, en selecteer de optie **dubbele vermeldingen toestaat en sla deze op volgorde van kolommen in de selectie**.  
  
|Uitzondering berichten|  
|------------------------|  
|De grootte van de doorgegeven objecten is niet consistent.|  
|De grootte van '{0}'is niet consistent met de grootte van'{1}'.|  
  

## <a name="error-0028"></a>Fout 0028  
 Er treedt op in het geval wanneer kolomset dubbele kolomnamen bevat en het is niet toegestaan.  
  
 Deze fout in Azure Machine Learning treedt op wanneer de namen van kolommen worden gedupliceerd; dat wil zeggen, niet uniek.  
  
**Oplossing:** Als alle kolommen hebben dezelfde naam, het toevoegen van een exemplaar van [metagegevens bewerken](edit-metadata.md) tussen de invoergegevensset en de module die de fout. Gebruik de kolomkiezer in [metagegevens bewerken](edit-metadata.md) naar kolommen te wijzigen, en typ de naam van de nieuwe kolommen in de **nieuwe kolomnamen** tekstvak. Als u meerdere kolommen, zorg ervoor dat de waarden die u typt in het **nieuwe kolomnamen** uniek zijn.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolomset bevat dubbele kolom namen.|  
|De naam '{0}' wordt gedupliceerd.|  
|De naam '{0}'is gedupliceerd in'{1}'.|  
  

## <a name="error-0029"></a>Fout 0029  
 Er treedt op in het geval wanneer ongeldige URI wordt doorgegeven.  
  
 Deze fout in Azure Machine Learning vindt plaats in het geval wanneer ongeldige URI wordt doorgegeven.  U ontvangt deze foutmelding als een van de volgende voorwaarden voldaan wordt:, of.  
  
-   De openbare of SAS-URI opgegeven voor Azure Blob-opslag om te lezen of schrijven bevat een fout.  
  
-   Het tijdvenster voor de SAS is verlopen.  
  
-   De URL van de webservice via HTTP-bron vertegenwoordigt een bestand of een loopback-URI.  
  
-   De URL van de webservice via HTTP, bevat een onjuist opgemaakte URL.  
  
-   De URL kan niet worden omgezet door de externe bron.  
  
**Oplossing:** Terug naar de module en controleer of de indeling van de URI. Als de gegevensbron een Web-URL via HTTP is, controleert u of de beoogde bron is niet een bestand of een loopback-URI (localhost).  
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldige Uri wordt doorgegeven.|  
  

## <a name="error-0030"></a>Fout 0030  
 Er treedt op in het geval wanneer het is niet mogelijk om een bestand te downloaden.  
  
 Deze uitzondering in Azure Machine Learning treedt op wanneer het is niet mogelijk om een bestand te downloaden. U ontvangt deze uitzondering bij een poging tot lezen van een HTTP-bron is mislukt na drie (3) probeer het opnieuw probeert.  
  
**Oplossing:** Controleer of dat de URI naar de HTTP-bron juist is en dat de site momenteel toegankelijk via Internet is.  
  
|Uitzondering berichten|  
|------------------------|  
|Kan een bestand te downloaden.|  
|Fout tijdens het downloaden van het bestand: {0}.|  
  

## <a name="error-0031"></a>Fout 0031  
 Er treedt op als het aantal kolommen in de kolomset is kleiner dan nodig is.  
  
 Deze fout in Azure Machine Learning treedt op als het aantal geselecteerde kolommen kleiner is dan nodig is.  U ontvangt deze foutmelding als de minimaal vereiste aantal kolommen niet zijn geselecteerd.  
  
**Oplossing:** Extra kolommen toevoegen aan de kolom selecteren met behulp van de **kolomkiezer**.  
  
|Uitzondering berichten|  
|------------------------|  
|Aantal kolommen in de kolomset is kleiner dan is vereist.|  
|{0} kolommen moeten worden opgegeven. Het werkelijke aantal opgegeven kolommen is {1}.|  

## <a name="error-0032"></a>Fout 0032  
 Er treedt op als argument geen getal is.  
  
 U ontvangt deze foutmelding in Azure Machine Learning als het argument een Double-waarde of NaN is.  
  
**Oplossing:** Het opgegeven argument voor het gebruik van een geldige waarde wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Argument is geen getal.|  
|"{0}' is geen getal.|  
  

## <a name="error-0033"></a>Fout 0033  
 Er treedt op als argument oneindig is.  
  
 Deze fout in Azure Machine Learning treedt op als het argument oneindig is. U ontvangt deze fout als het argument `double.NegativeInfinity` of `double.PositiveInfinity`.  
  
**Oplossing:** Het opgegeven argument om te worden van een geldige waarde wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Argument moet beperkt.|  
|"{0}' is niet beperkt.|  
  

## <a name="error-0034"></a>Fout 0034  
 Er treedt op als er meer dan één classificatie bestaat voor de combinatie van een bepaalde gebruiker-item.  
  
 Deze fout in Azure Machine Learning optreedt in de aanbeveling als de combinatie van een item voor gebruikersgegevens van meer dan één classificatiewaarde heeft.  
  
**Oplossing:** Zorg ervoor dat het item voor gebruikersgegevens-paar slechts één classificatiewaarde heeft.  
  
|Uitzondering berichten|  
|------------------------|  
|Meer dan één classificatie bestaat voor de waarden in de gegevensset.|  
|Meer dan één classificatie voor gebruiker {0} en item {1} in classificatie voorspelling gegevenstabel.|  
  

## <a name="error-0035"></a>Fout 0035  
 Er treedt op als er geen functies zijn opgegeven voor een bepaalde gebruiker of het item.  
  
 Deze fout in Azure Machine Learning treedt, maar u probeert te gebruiken een model aanbeveling voor het scoren van een functie-vector kan niet worden gevonden.  
  
**Oplossing:**

De recommender Matchbox heeft bepaalde vereisten waaraan moeten worden voldaan bij het gebruik van item functies of onderdelen van de gebruiker.  Deze fout geeft aan dat een functie-vector ontbreekt voor een gebruiker of het item dat u hebt opgegeven als invoer.  U moet ervoor zorgen dat een vector van functies beschikbaar in de gegevens voor elke gebruiker of het item is.  
  
 Bijvoorbeeld, als u getraind een aanbeveling model gemaakt met behulp van functies zoals de leeftijd, locatie of inkomsten van de gebruiker, maar nu wil maken van scores voor nieuwe gebruikers die niet zijn gezien tijdens de training, moet u enkele set gelijkwaardige van functies (dat wil zeggen leeftijd, locatie, en inkomsten) voor de nieuwe gebruikers om juiste voorspellingen voor hen. 
 
 Als u geen functies voor deze gebruikers hebt, kunt u overwegen feature-engineering voor het genereren van de desbetreffende functies.  Als u geen afzonderlijke gebruiker leeftijd of inkomsten waarden hebt, kunt u bijvoorbeeld geschatte waarden moet worden gebruikt voor een groep gebruikers genereren. 
 
<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).
 
For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->
  
 > [!TIP]
 > De oplossing is niet van toepassing op uw situatie? U bent Welkom bij het verzenden van feedback van dit artikel en bevatten informatie over het scenario, met inbegrip van de module en het aantal rijen in de kolom. We gebruiken deze informatie voor meer gedetailleerde stappen voor probleemoplossing in de toekomst.
   
|Uitzondering berichten|  
|------------------------|  
|Er zijn geen functies zijn opgegeven voor een gebruiker of het item.|  
|Functies voor {0} vereist maar is niet opgegeven.|  
  

## <a name="error-0036"></a>Fout 0036  
 Er treedt op als meerdere functie vectoren zijn opgegeven voor een bepaalde gebruiker of het item.  
  
 Deze fout in Azure Machine Learning treedt op als een functie-vector is meer dan één keer gedefinieerd.  
  
**Oplossing:** Zorg ervoor dat de functie-vector niet meer dan één keer gedefinieerd.  
  
|Uitzondering berichten|  
|------------------------|  
|Dubbele definitie van de functie voor een gebruiker of het item.|  
|Definitie van de functie voor dubbele {0}.|  
  

## <a name="error-0037"></a>Fout 0037  
 Er treedt op als meerdere kolommen van het label worden opgegeven en slechts één is toegestaan.  
  
 Deze fout in Azure Machine Learning treedt op als meer dan één kolom is geselecteerd als de nieuwe kolom van het label. Meest onder supervisie learning-algoritmen moeten één kolom moet worden gemarkeerd als het doel of het label.  
  
**Oplossing:** Zorg ervoor dat u één kolom selecteert als het nieuwe labelkolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Er zijn meerdere label kolommen opgegeven.|  
  

## <a name="error-0038"></a>Error 0038  
 Uitzondering doet zich voor als het aantal verwachte elementen een exacte waarde moet, maar is niet.  
  
 Deze fout in Azure Machine Learning doet zich voor als het aantal verwachte elementen een exacte waarde moet, maar is niet.  U ontvangt deze foutmelding als het aantal elementen niet gelijk aan de geldige verwachte waarde is.  
  
**Oplossing:** De invoer als u het juiste aantal elementen wilt wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Aantal elementen is niet geldig.|  
|Aantal elementen in '{0}' is niet geldig.|  
|Aantal elementen in '{0}' is niet gelijk aan een geldig aantal {1} element(en).|  
  

## <a name="error-0039"></a>Fout 0039  
 Er treedt op als een bewerking is mislukt.  
  
 Deze fout in Azure Machine Learning treedt op wanneer een interne bewerking kan niet worden voltooid.  
  
**Oplossing:** Deze fout wordt veroorzaakt door veel voorwaarden en er is geen specifieke verhaal.  
 De volgende tabel bevat algemene berichten voor deze fout, die worden gevolgd door een specifieke beschrijving van de voorwaarde. 
 
 Als er geen details beschikbaar is zijn, [feedback verzenden](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) en geef informatie op over de modules die de fout en de bijbehorende voorwaarden die worden gegenereerd.
  
|Uitzondering berichten|  
|------------------------|  
|Bewerking is mislukt.|  
|Fout bij het voltooien van bewerking: {0}.|  
  

## <a name="error-0040"></a>Fout 0040  
 Er treedt op bij het aanroepen van een module afgeschaft.  
  
 Deze fout in Azure Machine Learning wordt gemaakt bij het aanroepen van een module afgeschaft.  
  
**Oplossing:** De afgeschafte module vervangen door een ondersteunde versie. Zie het logboek van de module uitvoer voor de informatie over welke module gebruiken in plaats daarvan.  
  
|Uitzondering berichten|  
|------------------------|  
|Toegang tot afgeschaft module.|  
|Module "{0}" is afgeschaft. Gebruik de module '{1}' in plaats daarvan.|  
 

## <a name="error-0041"></a>Fout 0041  
 Er treedt op bij het aanroepen van een module afgeschaft.  
  
 Deze fout in Azure Machine Learning wordt gemaakt bij het aanroepen van een module afgeschaft.  
  
**Oplossing:** De afgeschafte module vervangen door een set ondersteunde apparaten. Deze informatie moet worden weergegeven in het logboek van de uitvoer module zijn.  
  
|Uitzondering berichten|  
|------------------------|  
|Toegang tot afgeschaft module.|  
|Module "{0}" is afgeschaft. De modules gebruiken '{1}' voor de aangevraagde functionaliteit.|  
 

## <a name="error-0042"></a>Fout 0042  
 Er treedt op wanneer het is niet mogelijk kolom converteren naar een ander type.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk kolom converteren naar het opgegeven type.  U ontvangt deze fout als een module nodig heeft voor een bepaald type, zoals datum/tijd, tekst, een getal met drijvende komma of een geheel getal, maar het is niet mogelijk een bestaande kolom converteren naar het vereiste type.  
 
U kunt bijvoorbeeld een kolom selecteren en probeert te converteren naar een numeriek gegevenstype voor gebruik in een rekenkundige bewerking en deze foutmelding krijgen als de kolom ongeldige gegevens bevat. 

Een andere reden die u deze fout krijgen kunt als u probeert te gebruiken van een kolom met getallen met drijvende komma of veel unieke waarden als een categorische kolom. 
  
**Oplossing:**

+ Open de Helppagina voor de module die de fout is gegenereerd, en controleer of de vereisten voor gegevenstypen.
+ Controleer de gegevenstypen van de kolommen in de invoergegevensset.
+ Inspecteer de gegevens die afkomstig zijn van zogeheten gegevensbronnen zonder schema.
+ Controleer de gegevensset voor ontbrekende waarden of speciale tekens op waaruit de conversie naar het gewenste gegevenstype blokkeren mogelijk. 
    + Numerieke gegevenstypen moet consistent zijn: bijvoorbeeld controleren op getallen met drijvende komma in een kolom met gehele getallen.
    + Zoeken naar tekenreeksen of N.V.T.-waarden in een kolom. 
    + Boole-waarden kunnen worden geconverteerd naar een juiste weergave, afhankelijk van het vereiste type.
    + Tekstkolommen voor niet-Unicode-tekens, tabblad tekens of stuurcodes onderzoeken
    + Datum/tijd-gegevens moet consistent zijn om te voorkomen dat fouten modelleren, maar opschoning kan lastig zijn vanwege de veel-indelingen. Overweeg het gebruik van <!--the [Execute R Script](execute-r-script.md) or -->[Python-Script uitvoeren](execute-python-script.md) modules om uit te voeren opruimen.  
+ Wijzig indien nodig de waarden in de invoergegevensset zodat de kolom met succes kan worden geconverteerd. Wijziging advies inwinnen binning, moet worden afgekapt of afronding operations, eliminatie van de uitschieters en toerekening van ontbrekende waarden. Zie de volgende artikelen voor een aantal veelvoorkomende van de transformatie van gegevens in machine learning:
    + [De ontbrekende gegevens opschonen](clean-missing-data.md)
    + [Gegevens normaliseren](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->
 
> [!TIP]
> Resolutie onduidelijk of niet van toepassing op uw situatie? U bent Welkom bij het verzenden van feedback van dit artikel en bevatten informatie over het scenario, met inbegrip van de module en het gegevenstype van de kolom. We gebruiken deze informatie voor meer gedetailleerde stappen voor probleemoplossing in de toekomst.  
  
|Uitzondering berichten|  
|------------------------|  
|Conversie toegestaan niet.|  
|Kolom van het type kan niet worden geconverteerd {0} naar kolom van het type {1}.|  
|Kan niet converteren voor kolom '{2}' van het type {0} naar kolom van het type {1}.|  
|Kan niet converteren voor kolom '{2}' van het type {0} naar kolom "{3}' van het type {1}.|  
  

## <a name="error-0043"></a>Fout 0043  
 Uitzondering treedt op wanneer het elementtype is gelijk aan niet expliciet implementeert.  
  
 Deze fout in Azure Machine Learning wordt gebruikt en wordt afgeschaft.  
  
**Oplossing:** Geen.  
  
|Uitzondering berichten|  
|------------------------|  
|Er is geen toegankelijke expliciete methode gelijk aan de gevonden.|  
|Waarden voor de kolom kan niet vergelijken \\"{0}\\' van het type {1}. Er is geen toegankelijke expliciete methode gelijk aan de gevonden.|  


## <a name="error-0044"></a>Fout 0044  
 Er treedt op wanneer het is niet mogelijk aan het elementtype van de kolom zijn afgeleid van de bestaande waarden.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk voor het afleiden van het type van een kolom of kolommen in een gegevensset. Dit gebeurt doorgaans wanneer twee of meer gegevenssets samenvoegen met andere elementtypen. Als Azure Machine Learning niet mogelijk te bepalen van een gemeenschappelijk type die in staat om weer te geven van alle waarden in een kolom of kolommen zonder verlies van gegevens is is, wordt deze fout gegenereerd.  
  
**Oplossing:** Zorg ervoor dat alle waarden in een bepaalde kolom in beide gegevenssets die worden gecombineerd van hetzelfde zijn type (numeriek, Boolean, categorische, string, datum, enzovoort) of hetzelfde type kan worden afgedwongen.  
  
|Uitzondering berichten|  
|------------------------|  
|Elementtype van de kolom kan niet worden afgeleid.|  
|Kan niet worden afgeleid elementtype voor kolom '{0}"--alle elementen zijn null-verwijzingen.|  
|Kan niet worden afgeleid elementtype voor kolom '{0}'van de gegevensset'{1}"--alle elementen zijn null-verwijzingen.|  
  

## <a name="error-0045"></a>Fout 0045  
 Er treedt op wanneer dit niet mogelijk te maken van een kolom vanwege gemengde elementtypen in de bron.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer het soort element van de twee gegevenssets worden gecombineerd, verschillen.  
  
**Oplossing:** Zorg ervoor dat alle waarden in een bepaalde kolom in beide gegevenssets die worden gecombineerd van hetzelfde type (numeriek, Boolean, categorische, string, datum, enzovoort).  
  
|Uitzondering berichten|  
|------------------------|  
|Kan de kolom met gemengde elementtypen niet maken.|  
|Kan kolom maken met de ID '{0}"van element gemengde typen: \n\tType van gegevens [{1}, {0}] is {2}\n\tType van gegevens [{3}, {0}] is {4}.|  
  

## <a name="error-0046"></a>Fout 0046  
 Er treedt op wanneer het is niet mogelijk voor het maken van de map op het opgegeven pad.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk voor het maken van een map op het opgegeven pad. U ontvangt deze fout als een deel van het pad naar de uitvoermap voor een Hive-Query onjuist of niet toegankelijk is.  
  
**Oplossing:** Terug naar de module en controleer of het mappad is ingedeeld en dat deze toegankelijk is met de huidige referenties.  
  
|Uitzondering berichten|  
|------------------------|  
|Geef een geldige uitvoerdirectory.|  
|Map: {0} kan niet worden gemaakt. Ongeldig pad opgeven.|  
  

## <a name="error-0047"></a>Fout 0047  
 Er treedt op als het aantal kolommen van de functie in enkele van de gegevenssets die zijn doorgegeven aan de module is te klein.  
  
 Deze fout in Azure Machine Learning treedt op als invoergegevensset voor een opleiding niet het minimum aantal kolommen dat is vereist door de algoritme bevat. Meestal een van beide de gegevensset is leeg of bevat alleen training kolommen.  
  
**Oplossing:** Terug naar de invoergegevensset Zorg ervoor dat er een of meer extra kolommen naast de labelkolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Aantal kolommen van de functie in de invoergegevensset is kleiner dan het toegestane minimum.|  
|Aantal kolommen van de functie in de invoergegevensset is lager dan het toegestane minimum van {0} kolom(men).|  
|Aantal kolommen in de invoergegevensset functie "{0}' is lager dan het toegestane minimum van {1} kolom(men).|  
  

## <a name="error-0048"></a>Fout 0048  
 Er treedt op in het geval wanneer het is niet mogelijk om een bestand te openen.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk een bestand openen voor lezen of schrijven. U kunt dit foutbericht om volgende redenen:  
  
-   De container of het bestand (blob) bestaat niet  
  
-   Het toegangsniveau van het bestand of de container staat niet toe dat u toegang tot het bestand  
  
-   Het bestand is te groot om te lezen of een onjuiste indeling  
  
**Oplossing:** Terug naar de module en het bestand dat u probeert te lezen.  
  
 Controleer of dat de namen van de container en de bestandsnaam juist zijn.  
  
 Gebruik de klassieke Azure portal of een Azure storage-hulpprogramma om te controleren of u gemachtigd bent om het bestand te openen.  
  
  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->
  
|Uitzondering berichten|  
|------------------------|  
|Kan een bestand te openen.|  
|Fout bij het openen van het bestand: {0}.|  


## <a name="error-0049"></a>Fout 0049  
 Er treedt op in het geval wanneer het is niet mogelijk een bestand parseren.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk een bestand parseren. U ontvangt deze foutmelding als de bestandsindeling die is geselecteerd in de [importgegevens](import-data.md) module komt niet overeen met de actuele indeling van het bestand, of als het bestand een onherkenbare teken bevat.  
  
**Oplossing:** Terug naar de module en corrigeren van de selectie van de indeling bestand, indien deze komt niet overeen met de indeling van het bestand. Indien mogelijk, inspecteren van het bestand om te bevestigen dat deze geen geen ongeldige tekens bevat.  
  
|Uitzondering berichten|  
|------------------------|  
|Een kan bestand niet parseren.|  
|Fout tijdens het parseren van het bestand: {0}.|  
  

## <a name="error-0050"></a>Fout 0050  
 Er treedt op in het geval als invoer en uitvoerbestanden zijn hetzelfde.  
  
**Oplossing:** Deze fout in Azure Machine Learning wordt gebruikt en wordt afgeschaft.  
  
|Uitzondering berichten|  
|------------------------|  
|Opgegeven bestanden voor invoer en uitvoer kunnen niet hetzelfde zijn.|


## <a name="error-0051"></a>Fout 0051  
 Er treedt op in het geval wanneer verschillende uitvoerbestanden hetzelfde zijn.  
  
**Oplossing:** Deze fout in Azure Machine Learning wordt gebruikt en wordt afgeschaft.  
  
|Uitzondering berichten|  
|------------------------|  
|Opgegeven bestanden voor de uitvoer kunnen niet hetzelfde zijn.|


## <a name="error-0052"></a>Fout 0052  
 Er treedt op als sleutel voor Azure storage-account is onjuist opgegeven.  
  
 Deze fout in Azure Machine Learning treedt op als de sleutel die wordt gebruikt voor toegang tot de Azure storage-account onjuist is. Bijvoorbeeld, ziet u deze fout als de Azure-opslagsleutel is afgekapt wanneer gekopieerd en geplakt, of als de verkeerde sleutel is gebruikt.  
  
 Zie voor meer informatie over het ophalen van de sleutel voor een Azure storage-account [weergeven, kopiëren en opnieuw genereren opslag toegangssleutels](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/).  
  
**Oplossing:** Terug naar de module en controleer of de Azure-opslagsleutel juist is voor het account Kopieer de sleutel opnieuw van de klassieke Azure portal indien nodig.  
  
|Uitzondering berichten|  
|------------------------|  
|De sleutel van de Azure storage-account is onjuist.|  
  

## <a name="error-0053"></a>Fout 0053  
 Er treedt op in het geval wanneer er geen functies voor gebruikers of -items voor matchbox aanbevelingen zijn.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer een functie-vector kan niet worden gevonden.  
  
**Oplossing:** Zorg ervoor dat een functie-vector aanwezig zijn in de invoergegevensset.  
  
|Uitzondering berichten|  
|------------------------|  
|Functies voor gebruikers of / en items zijn vereist, maar niet opgegeven.|  

## <a name="error-0054"></a>Fout 0054  
 Er treedt op als er te weinig verschillende waarden in de kolom die moet worden voltooid.  
  
**Oplossing:** Deze fout in Azure Machine Learning wordt gebruikt en wordt afgeschaft.  
  
|Uitzondering berichten|  
|------------------------|  
|Gegevens heeft te weinig verschillende waarden in de opgegeven kolom bewerking te voltooien.|  
|Gegevens heeft te weinig verschillende waarden in de opgegeven kolom bewerking te voltooien. Het vereiste minimum is {0} elementen.|  
|Gegevens heeft te weinig verschillende waarden in de kolom '{1}"bewerking te voltooien. Het vereiste minimum is {0} elementen.|  
  

## <a name="error-0055"></a>Fout 0055  
 Er treedt op bij het aanroepen van een module afgeschaft.  Deze fout in Azure Machine Learning wordt weergegeven als u probeert aan te roepen een module die is afgeschaft.
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Toegang tot afgeschaft module.|  
|Module "{0}" is afgeschaft.|  

## <a name="error-0056"></a>Fout 0056  
 Er treedt op als de kolommen die u hebt geselecteerd voor een bewerking is in strijd met vereisten.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u kolommen voor een bewerking die vereist zijn om de kolom van een bepaald type kiest. 
 
 Deze fout kan ook gebeuren als de kolom het juiste gegevenstype is, maar de module die u gebruikt is vereist dat de kolom ook worden gemarkeerd als een functie, een label of een categorische kolom.  
  
  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->
  
**Oplossing:**
  
1.  Controleer het gegevenstype van de kolommen die momenteel zijn geselecteerd. 

2. Nagaan of de geselecteerde kolommen categorische zijn, label of functie kolommen.  
  
3.  Bekijk het help-onderwerp voor de module waarin u de kolomselectie, om te bepalen of er specifieke vereisten voor het type of de kolom gegevensgebruik hebt gemaakt.  
  
3.  Gebruik [metagegevens bewerken](edit-metadata.md) te wijzigen van het kolomtype voor de duur van deze bewerking. Zorg ervoor dat u het wijzigen van het kolomtype terug naar de oorspronkelijke waarde, met behulp van een ander exemplaar van [metagegevens bewerken](edit-metadata.md), als u nodig voor downstream-bewerkingen hebt.  
  
|Uitzondering berichten|  
|------------------------|  
|Een of meer geselecteerde kolommen zijn niet in een toegestane categorie.|  
|Kolom met de naam '{0}' is niet in een toegestane categorie.|  
  

## <a name="error-0057"></a>Fout 0057  
 Er treedt op bij het maken van een bestand of de blob die al bestaat.  
  
 Deze uitzondering treedt op wanneer u de [gegevens exporteren](export-data.md) module of andere opslaan van resultaten van een experiment in Azure Machine Learning in Azure blob-opslag, maar u probeert te maken van een bestand of de blob die al bestaat.   
  
**Oplossing:**
 
 U ontvangt deze fout alleen als u eerder de eigenschap stelt **Azure-blobopslag schrijven modus** naar **fout**. Het ontwerp van deze module wordt een fout gegenereerd, als u probeert het schrijven van een gegevensset naar een blob die al bestaat.
 
 - Open de module-eigenschappen en wijzig de eigenschap **Azure-blobopslag schrijven modus** naar **overschrijven**.
 - U kunt ook het geval is, typ de naam van een andere bestemmings-blob of een bestand en zorg ervoor dat u het opgeven van een blob die nog niet bestaat.  
  
|Uitzondering berichten|  
|------------------------|  
|Bestand of de Blob bestaat al.|  
|Bestand of de Blob "{0}" bestaat al.|  
  

## <a name="error-0058"></a>Fout 0058  
 Deze fout in Azure Machine Learning treedt op als de gegevensset niet de verwachte labelkolom bevat.  
  
 Deze uitzondering kan ook optreden als de labelkolom opgegeven komt niet overeen met de gegevens of het gegevenstype van de cursist verwacht of de juiste waarden. Deze uitzondering wordt bijvoorbeeld bij het gebruik van een labelkolom van de werkelijke waarde bij het trainen van een binaire classificatie geproduceerd.  
  
**Oplossing:** De resolutie, is afhankelijk van de cursist of trainer die u gebruikt en de gegevenstypen van de kolommen in uw gegevensset. Controleer eerst of de vereisten van de algoritme voor machine learning of trainingsmodule.  
  
 Terug naar de invoergegevensset. Controleer of dat de kolom die u worden behandeld moet als het label heeft het juiste gegevenstype voor het model dat u maakt.  
  
 Controleer de invoer voor de ontbrekende waarden te elimineren en vervangt u deze indien nodig.  
  
 Indien nodig, voeg de [metagegevens bewerken](edit-metadata.md) module en zorg ervoor dat de labelkolom is gemarkeerd als een label.  
  
|Uitzondering berichten|  
|------------------------|  
|De labelkolom is niet zoals verwacht.|  
|De labelkolom is niet werkt zoals verwacht in '{0}'.|  
|De kolom van het label "{0}'wordt niet verwacht in'{1}'.|  
  

## <a name="error-0059"></a>Fout 0059  
 Er treedt op als een index voor kolom opgegeven in het kiezen van een kolom kan niet worden geparseerd.  
  
 Deze fout in Azure Machine Learning treedt op als een kolomindex die is opgegeven bij het gebruik van de kolomkiezer kan niet worden geparseerd.  U ontvangt deze foutmelding wanneer de kolomindex is een ongeldige indeling die kan niet worden geparseerd.  
  
**Oplossing:** De kolomindex voor het gebruik van de waarde van een geldige index wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Een of meer opgegeven kolom indexen of bereiken van de index kunnen niet worden geparseerd.|  
|De kolomindex of het bereik '{0}' kan niet worden geparseerd.|  
  

## <a name="error-0060"></a>Fout 0060  
 Er treedt op wanneer een out-of bereik kolombereik is opgegeven in een kolom kiezen.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het kolombereik van een van buiten het bereik is opgegeven in de kolomkiezer. U ontvangt deze foutmelding als het kolombereik in de kolomkiezer komt niet overeen met de kolommen in de gegevensset.  
  
**Oplossing:** Het kolombereik in de kolomkiezer overeen te komen met de kolommen in de gegevensset wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldig of valt buiten het bereik kolom index bereik is opgegeven.|  
|Kolombereik '{0}' is ongeldig of valt buiten het bereik.|  
  

## <a name="error-0061"></a>Fout 0061  
 Er treedt op wanneer u probeert een rij toevoegt aan een DataTable waarvoor een verschillend aantal kolommen dan in de tabel.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een rij toevoegt aan een gegevensset met een verschillend aantal kolommen dan de gegevensset.  U ontvangt deze foutmelding als de rij die wordt toegevoegd aan de gegevensset een verschillend aantal kolommen van de invoergegevensset heeft.  De rij kan niet worden toegevoegd aan de gegevensset als het aantal kolommen afwijkt.  
  
**Oplossing:** Wijzigen van de invoergegevensset met hetzelfde aantal kolommen als de rij die is toegevoegd, of wijzig de rij die is toegevoegd aan hetzelfde aantal kolommen als de gegevensset hebben.  
  
|Uitzondering berichten|  
|------------------------|  
|Alle tabellen moeten hetzelfde aantal kolommen hebben.|  
  

## <a name="error-0062"></a>Fout 0062  
 Er treedt op wanneer u probeert om twee modellen met verschillende learner typen te vergelijken.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer de metrische evaluatiegegevens voor twee verschillende beoordeelde gegevenssets kunnen niet worden vergeleken. In dit geval is het niet mogelijk om te vergelijken van de effectiviteit van de modellen die worden gebruikt voor het produceren van de twee beoordeelde gegevenssets.  
  
**Oplossing:** Controleer of dat de beoordeelde resultaten worden geproduceerd door de dezelfde soort machine learning-model (binaire classificatie, regressie, ROC-classificatie, aanbeveling, clustering, detectie van afwijkingen, enz.) Alle modellen die u wilt vergelijken, moeten hetzelfde learner type hebben.  
  
|Uitzondering berichten|  
|------------------------|  
|Alle modellen moeten hetzelfde learner type hebben.|  
  

 <!--## Error 0063  
 This exception is raised when R script evaluation fails with an error.  
  
 This error occurs when you have provided an R script in one of the [R language modules](r-language-modules.md) in Azure Machine Learning, and the R code contains internal syntax errors. The exception can also occur if you provide the wrong inputs to the R script. 
 
 The error can also occur if the script is too large to execute in the workspace. The maximum script size for the **Execute R Script** module is 1,000 lines or 32 KB of work space, whichever is lesser.
  
**Resolution:**

1. In Azure Machine Learning Studio, right-click the module that has the error, and select **View Log**.
2. Examine the standard error log of the module, which contains the stack trace.
    + Lines beginning with [ModuleOutput] indicate output from R.
    + Messages from R marked as **warnings** typically do not cause the experiment to fail.
3. Resolve script issues.  
    + Check for R syntax errors. Check for variables that are defined but never populated.
    + Review the input data and the script to determine if either the data or variables in the script use characters not supported by Azure Machine Learning.
    + Check whether all package dependencies are installed.
    + Check whether your code loads required libraries that are not loaded by default.
    + Check whether the required packages are the correct version.
    + Make sure that any dataset that you want to output is converted to a data frame.  
4.  Resubmit the experiment.

 <!--
> [!NOTE]
> These topics contains examples of R code that you can use, as well as links to experiments in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com) that use R script.
> + [Execute R Script](execute-r-script.md)
> + [Create R Model](create-r-model.md)
-->  
|Uitzondering berichten|  
|------------------------|  
|Fout tijdens de evaluatie van R-script.|  
|De volgende fout is opgetreden tijdens de evaluatie van R-script:---begin van het foutbericht van R--- {0} ---einde van het foutbericht van R---|  
|Tijdens het evalueren van R-script '{1}' de volgende fout is opgetreden:---begin van het foutbericht van R--- {0} ---einde van het foutbericht van R---|  
  


## <a name="error-0064"></a>Fout 0064  
 Er treedt op als Azure storage-accountnaam of opslagsleutel is onjuist opgegeven.  
  
 Deze fout in Azure Machine Learning treedt op als de Azure storage-accountnaam of de opslagsleutel is onjuist opgegeven. U ontvangt deze foutmelding als u een onjuist account of wachtwoord voor het opslagaccount invoeren. Dit kan gebeuren als u de accountnaam of het wachtwoord voor het handmatig invoeren. Het kan ook optreden als het account is verwijderd.  
  
**Oplossing:** Controleer of de accountnaam en het wachtwoord juist hebt ingevoerd en of het account bestaat.  
  
|Uitzondering berichten|  
|------------------------|  
|De Azure storage-accountnaam of de opslagsleutel is onjuist.|  
|De Azure storage-accountnaam '{0}' of de opslagsleutel voor de accountnaam is onjuist.|  
  

## <a name="error-0065"></a>Fout 0065  
 Er treedt op als Azure blob-naam is onjuist opgegeven.  
  
 Deze fout in Azure Machine Learning treedt op als de naam van de Azure-blob is onjuist opgegeven.  U ontvangt de fout als:  
  
-   De blob kan niet worden gevonden in de opgegeven container.  
  
 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->
  
-   Alleen de container is opgegeven als de bron in een [importgegevens](import-data.md) aanvragen wanneer de indeling Excel- of CSV met codering is; samenvoeging van de inhoud van alle blobs in een container is niet toegestaan voor deze indelingen.  
  
-   Een SAS-URI bevat niet de naam van een geldige blob.  
  
**Oplossing:** Terug naar de module die de uitzondering veroorzaakt. Controleer of de opgegeven blob bestaat in de container in het storage-account en machtigingen kunnen u de blob. Controleer of de invoer van het formulier **containername/filename** als u Excel- of CSV met indelingen te coderen. Controleer of een SAS-URI bevat de naam van een geldige blob.  
  
|Uitzondering berichten|  
|------------------------|  
|De Azure storage-blob is onjuist.|  
|De naam van de Azure storage-blob "{0}' is onjuist|  
  

## <a name="error-0066"></a>Fout 0066  
 Er treedt op als een resource kan niet worden geüpload naar een Azure-Blob.  
  
 Deze fout in Azure Machine Learning treedt op als een resource kan niet worden geüpload naar een Azure-Blob.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Beide worden opgeslagen in hetzelfde Azure storage-account als het account met het invoerbestand.  
  
**Oplossing:** Terug naar de module. Controleer of de naam van de Azure-account, sleutel van een opslagaccount en container juist zijn en dat het account de machtiging schrijven naar de container heeft.  
  
|Uitzondering berichten|  
|------------------------|  
|De resource kan niet worden geüpload naar Azure storage.|  
|Het bestand '{0}' kan niet worden geüpload naar Azure storage als {1}.|  
  

## <a name="error-0067"></a>Fout 0067  
 Er treedt op als een gegevensset een verschillend aantal kolommen heeft dan verwacht.  
  
 Deze fout in Azure Machine Learning treedt op als een gegevensset een verschillend aantal kolommen heeft dan verwacht.  U ontvangt deze foutmelding wanneer het aantal kolommen in de gegevensset verschillen van het aantal kolommen dat de module wordt verwacht tijdens de uitvoering dat.  
  
**Oplossing:** De invoergegevensset of de parameters wijzigen.  
  
|Uitzondering berichten|  
|------------------------|  
|Onverwacht aantal kolommen in de datatable.|  
|Verwacht "{0}'kolommen gevonden maar'{1}' kolommen in plaats daarvan.|  
  

## <a name="error-0068"></a>Fout 0068  
 Er treedt op als het opgegeven Hive-script niet juist is.  
  
 Deze fout in Azure Machine Learning treedt op als er syntaxisfouten zijn in een Hive SQL-script, of als de Hive-interpreter er een fout optreedt tijdens het uitvoeren van de query of het script.  
  
**Oplossing:**

Normaal gesproken wordt het foutbericht van Hive terug in het foutenlogboek gerapporteerd zodat u actie op basis van de specifieke fout ondernemen kunt. 

+ Open de module en de query voor fouten controleren.  
+ Controleer of de query werkt goed buiten Azure Machine Learning door te melden bij de Hive-console van uw Hadoop-cluster en de query uit te voeren.  
+ Probeer het plaatsen van opmerkingen in uw Hive-script in een afzonderlijke regel in plaats van met een combinatie van uitvoerbare instructies en opmerkingen in een enkele regel.  

### <a name="resources"></a>Resources

Zie de volgende artikelen voor meer informatie over met Hive-query's voor machine learning:

+ [Hive-tabellen maken en gegevens laden uit Azure Blob Storage](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Gegevens in tabellen met Hive-query's verkennen](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Functies maken voor gegevens in een Hadoop-cluster met behulp van Hive-query 's](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Hive voor overzichtskaart van SQL-gebruikers (PDF-bestand)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Uitzondering berichten|  
|------------------------|  
|Hive-script is onjuist.|  
|Hive-script {0} is niet juist.|  
  

## <a name="error-0069"></a>Fout 0069  
 Er treedt op als het opgegeven SQL-script niet juist is.  
  
 Deze fout in Azure Machine Learning treedt op als het opgegeven SQL-script problemen met syntaxis heeft of als de kolommen of de tabel zijn opgegeven in het script niet geldig is. 
 
 U ontvangt deze foutmelding als de SQL-engine een fout wordt aangetroffen tijdens het uitvoeren van de query of het script. Normaal gesproken wordt de SQL-foutbericht terug in het foutenlogboek gerapporteerd zodat u actie op basis van de specifieke fout ondernemen kunt.  
  
**Oplossing:** Terug naar de module en de SQL-query voor fouten controleren.  
  
 Controleer of de query werkt goed buiten Azure ML door rechtstreeks aanmelden bij de database-server en de query uit te voeren.  
  
 Als er een bericht gegenereerd SQL is gerapporteerd door de module-uitzondering, maatregelen nemen op basis van de gemelde fout. Zo bevatten de foutberichten soms specifieke hulp over de fout waarschijnlijk:
+ *De kolom of ontbrekende database bestaat niet*, waarmee wordt aangegeven dat u een verkeerde kolomnaam mogelijk ingevoerd. Als u ervoor dat de de kolomnaam juist is, probeert u plaatst u de kolom-id met behulp van haakjes of aanhalingstekens.
+ *SQL logische fout in de buurt \<SQL-sleutelwoord\>* , waarmee wordt aangegeven dat u een syntaxisfout voor de opgegeven trefwoord wellicht

  
|Uitzondering berichten|  
|------------------------|  
|SQL-script is onjuist.|  
|SQL-query '{0}' is niet juist.|  
|SQL-query '{0}' is niet correct: {1}|  
  

## <a name="error-0070"></a>Fout 0070  
 Er treedt op wanneer u probeert te krijgen tot niet-bestaande Azure-tabel.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u probeert te krijgen tot een niet-bestaande Azure-tabel. U ontvangt deze foutmelding als u een tabel in Azure storage opgeven, niet bestaat bij het lezen van of schrijven naar Azure Table Storage. Dit kan gebeuren als u de naam van de gewenste tabel verkeerd of u een naam van het doel en het opslagtype komen niet overeen hebt. Bijvoorbeeld, u bedoeld om te lezen uit een tabel, maar in plaats daarvan de naam van een blob ingevoerd.  
  
**Oplossing:** Terug naar de module om te controleren of de naam van de tabel juist is.  
  
|Uitzondering berichten|  
|------------------------|  
|Azure-tabel bestaat niet.|  
|Azure-tabel "{0}' bestaat niet.|  
  
## <a name="error-0071"></a>Fout 0071  
 Er treedt op als de opgegeven referenties onjuist zijn.  
  
 Deze fout in Azure Machine Learning treedt op als de opgegeven referenties onjuist zijn.  
  
 U ontvangt deze foutmelding kan ook als de module kan geen verbinding met een HDInsight-cluster maken.  
  
**Oplossing:** Controleer de invoer voor de module en controleer of de accountnaam en het wachtwoord.  
  
 Controleer de volgende problemen die leiden een fout opgetreden tot kunnen:  
  
-   Het schema van de gegevensset komt niet overeen met het schema van de doel-datatable.  
  
-   Kolomnamen worden ontbrekende of verkeerd gespelde  
  
-   U schrijft naar een tabel met kolomnamen met ongeldige tekens. Normaal gesproken kunt u dergelijke kolomnamen zet tussen vierkante haken, maar als dit niet werkt, bewerkt u kolomnamen gebruiken alleen letters en onderstrepingstekens (_)  
  
-   Bevatten tussen enkele aanhalingstekens plaatsen van tekenreeksen die u probeert te schrijven  
  
 Als u probeert verbinding maken met een HDInsight-cluster, Controleer of het doelcluster toegankelijk zijn met de opgegeven referenties.  
  
|Uitzondering berichten|  
|------------------------|  
|Onjuiste referenties worden doorgegeven.|  
|Onjuiste gebruikersnaam "{0}" of het wachtwoord wordt doorgegeven.|  
  

## <a name="error-0072"></a>Fout 0072  
 Er treedt op in het geval van time-out voor verbindingen.  
  
 Deze fout in Azure Machine Learning treedt op wanneer een verbinding een optreedt time-out. U ontvangt deze foutmelding als er zijn momenteel problemen met de netwerkverbinding met de gegevensbron of de bestemming, zoals trage verbinding met internet, of als de gegevensset is groot en/of de SQL-query in de gegevens worden gelezen complexe verwerking wordt uitgevoerd.  
  
**Oplossing:** Bepalen of er momenteel problemen met langzame verbindingen met Azure storage of internet zijn.  
  
|Uitzondering berichten|  
|------------------------|  
|Er is een time-out opgetreden.|  
  

## <a name="error-0073"></a>Fout 0073  
 Er treedt op als er een fout optreedt tijdens het converteren van een kolom in een ander type.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het is niet mogelijk kolom converteren naar een ander type.  U ontvangt deze fout als een module vereist dat een bepaald type en het is niet mogelijk de kolom converteren naar het nieuwe type.  
  
**Oplossing:** Wijzig de invoergegevensset, zodat de kolom kan worden omgezet op basis van de binnenste uitzondering.  
  
|Uitzondering berichten|  
|------------------------|  
|Kan niet converteren van de kolom.|  
|Kan niet worden geconverteerd van kolom {0}.|  
  

## <a name="error-0074"></a>Fout 0074  
 Er treedt op wanneer de [metagegevens bewerken](edit-metadata.md) probeert een sparse kolom converteren naar categorische.  
  
 Deze fout in Azure Machine Learning treedt op wanneer de [metagegevens bewerken](edit-metadata.md) probeert een sparse kolom converteren naar categorische.  U ontvangt deze fout bij het converteren van sparse-kolommen naar categorische met de **maken categorische** optie.  Azure machine Learning biedt geen ondersteuning sparse categorische matrices, zodat de module mislukken.  
  
 <!--**Resolution:**
 Make the column dense by using [Convert to Dataset](convert-to-dataset.md) first or do not convert the column to categorical.  -->
  
|Uitzondering berichten|  
|------------------------|  
|Sparse-kolommen kunnen niet worden geconverteerd naar Categorisch.|  
  

## <a name="error-0075"></a>Fout 0075  
Er treedt op wanneer een ongeldig binning-functie wordt gebruikt wanneer een gegevensset quantizing.  
  
Deze fout in Azure Machine Learning treedt op wanneer u probeert naar de opslaglocatie van gegevens met behulp van een niet-ondersteunde methode, of wanneer de combinaties van parameters zijn ongeldig.  
  
**Oplossing:**

Foutafhandeling voor deze gebeurtenis is geïntroduceerd in een eerdere versie van Azure Machine Learning die meer aanpassingsmogelijkheden van methoden binning toegestaan. Op dit moment alle binning methoden zijn op basis van een selectie uit een vervolgkeuzelijst zodat technisch dat moet worden niet meer mogelijk dat deze fout.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldige binning functie die wordt gebruikt.|  
  

## <a name="error-0077"></a>Fout 0077  
 Er treedt op wanneer onbekende blobbestand modus doorgegeven schrijft.  
  
 Deze fout in Azure Machine Learning treedt op als een ongeldig argument wordt doorgegeven in de specificaties voor een gegevensbron of het doel van de blob-bestand.  
  
**Oplossing:** In bijna alle modules die gegevens importeren of exporteren naar en van Azure blob-opslag, zijn parameterwaarden beheren de schrijfmodus die toegewezen met behulp van een vervolgkeuzelijst; Daarom is het niet mogelijk om door te geven van een ongeldige waarde, en deze fout wordt niet weergegeven. Deze fout wordt afgeschaft in een latere versie.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteunde blob schrijft modus.|  
|Niet-ondersteunde blob schrijft modus: {0}.|  
  

## <a name="error-0078"></a>Fout 0078  
 Er treedt op wanneer de optie voor het HTTP [gegevens importeren](import-data.md) ontvangt een 3xx statuscode omleiding waarmee wordt aangegeven.  
  
 Deze fout in Azure Machine Learning treedt op wanneer de optie voor het HTTP [gegevens importeren](import-data.md) ontvangt een 3xx (301, 302, 304, enz.)-statuscode omleiding waarmee wordt aangegeven. U ontvangt deze foutmelding als u probeert verbinding maken met een HTTP-bron die de browser wordt omgeleid naar een andere pagina. Voor beveiliging zijn redenen, websites omleiden niet toegestaan als gegevensbronnen voor Azure Machine Learning.  
  
**Oplossing:** Als de website een vertrouwde website is, voert u de URL van de omgeleide rechtstreeks in.  
  
|Uitzondering berichten|  
|------------------------|  
|HTTP-omleiding is niet toegestaan|  
  

## <a name="error-0079"></a>Fout 0079  
 Er treedt op als de naam van de Azure storage-container is onjuist opgegeven.  
  
 Deze fout in Azure Machine Learning treedt op als de naam van de Azure storage-container is onjuist opgegeven. U ontvangt deze fout als u niet hebt opgegeven, de container en het gebruik van blob (bestand) de naam van **het pad naar de blob die begint met container** optie bij het schrijven naar Azure Blob Storage.  
  
**Oplossing:** Terug naar de [gegevens exporteren](export-data.md) module en controleer of het opgegeven pad naar de blob bevat zowel de container en de bestandsnaam in de indeling **container/filename**.  
  
|Uitzondering berichten|  
|------------------------|  
|De naam van de Azure storage-container is onjuist.|  
|De naam van de Azure storage-container "{0}' is onjuist; een containernaam zijn van de indeling van de container/blob werd verwacht.|  
  

## <a name="error-0080"></a>Fout 0080  
 Er treedt op wanneer de kolom met alle waarden die ontbreekt is niet toegestaan door de module.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer een of meer van de kolommen die worden gebruikt door de module alle ontbrekende waarden bevat. Bijvoorbeeld, als een module is statistische gegevens voor elke kolom computing, deze kan niet worden uitgevoerd op een kolom die geen gegevens bevat. In dergelijke gevallen wordt uitvoering van module met deze uitzondering onderbroken.  
  
**Oplossing:** Terug naar de invoergegevensset en verwijder alle kolommen die alle ontbrekende waarden bevatten.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolommen met alle ontbrekende-waarden zijn niet toegestaan.|  
|Kolom {0} heeft alle waarden ontbreekt.|  
  

## <a name="error-0081"></a>Fout 0081  
 Er treedt een PCA-module op als het aantal dimensies om te beperken tot gelijk zijn aan het aantal kolommen van de functie in de invoergegevensset, met ten minste één functie voor sparse-kolom.  
  
 Deze fout in Azure Machine Learning wordt gemaakt als de volgende voorwaarden wordt voldaan: (a) de invoergegevensset heeft ten minste één sparse kolom en (b) het laatste aantal dimensies aangevraagd is hetzelfde als het aantal invoer dimensies.  
  
**Oplossing:** Verklein het aantal dimensies in de uitvoer moet minder dan het aantal dimensies in de invoer. Dit is gebruikelijk in PCA-toepassingen.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->
  
|Uitzondering berichten|  
|------------------------|  
|Voor de gegevensset die functie voor sparse kolommen bevat moet het aantal dimensies te verminderen kleiner dan het aantal kolommen van de functie.|  
 

## <a name="error-0082"></a>Fout 0082  
 Er treedt op wanneer een model kan niet is gedeserialiseerd worden.  
  
 Deze fout in Azure Machine Learning vindt plaats wanneer een opgeslagen virtuele machine learning-model of transformeren met een nieuwere versie van de Azure Machine Learning-runtime als gevolg van een belangrijke wijziging kan niet worden geladen.  
  
**Oplossing:** Het trainingsexperiment dat het model of de transformatie geproduceerd moet opnieuw uitvoeren en het model of opnieuw is transformatie moet worden opgeslagen.  
  
|Uitzondering berichten|  
|------------------------|  
|Model kan niet worden gedeserialiseerd, omdat deze waarschijnlijk met een oudere serialisatie-indeling is geserialiseerd. Opnieuw trainen en het model opnieuw op te slaan.|  
  

## <a name="error-0083"></a>Fout 0083  
 Er treedt op als de gegevensset die wordt gebruikt voor trainingen voor concreet type zijn van de cursist kan niet worden gebruikt.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer de gegevensset is niet compatibel met de cursist wordt getraind. Bijvoorbeeld de gegevensset ten minste één waarde ontbreekt in elke rij kan bevatten, en als gevolg hiervan de volledige gegevensset zou worden overgeslagen tijdens de training. In andere gevallen enkele machine learning-algoritmen, zoals de detectie van afwijkingen verwachten niet dat labels aanwezig zijn en kunnen deze uitzondering genereert als labels aanwezig zijn in de gegevensset.  
  
**Oplossing:** Raadpleeg de documentatie van de cursist wordt gebruikt om te controleren van vereisten voor de invoergegevensset. Bekijk de kolommen om te zien van alle vereiste kolommen aanwezig zijn.  
  
|Uitzondering berichten|  
|------------------------|  
|Gegevensset die wordt gebruikt voor de training is ongeldig.|  
|{0} bevat ongeldige gegevens voor de training.|  
|{0} bevat ongeldige gegevens voor de training. Type van de cursist: {1}.|  
  

## <a name="error-0084"></a>Fout 0084  
 Er treedt op wanneer scores afkomstig van een R-Script worden geëvalueerd. Dit is momenteel niet ondersteund.  
  
 Deze fout in Azure Machine Learning treedt op als u probeert te gebruiken een van de modules voor het evalueren van een model met de uitvoer van een R-script met scores.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Evaluatie van scores die worden geproduceerd door R is momenteel niet ondersteund.|  
  

## <a name="error-0085"></a>Fout 0085  
 Er treedt op wanneer de evaluatie van script mislukt met een fout.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u aangepaste scripts die syntaxisfouten bevat worden uitgevoerd.  
  
**Oplossing:** Uw code in een externe editor en Controleer op fouten bekijken.  
  
|Uitzondering berichten|  
|------------------------|  
|Fout tijdens de evaluatie van het script.|  
|De volgende fout is opgetreden tijdens de evaluatie van script, de uitvoerlogboek voor meer informatie bekijken:---begin van het foutbericht van de {0} interpreter--- {1} ---einde van het foutbericht van de {0} interpreter--- ------|  
  

## <a name="error-0086"></a>Fout 0086  
 Er treedt op wanneer een tellen transformatie ongeldig is.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een transformatie op basis van een aantal tabel selecteren, maar de geselecteerde transformatie niet compatibel met de huidige gegevens of met het nieuwe aantal tabel is.  
  
**Oplossing:** De module ondersteuning biedt voor het opslaan van het aantal en de regels die gezamenlijk de transformatie in twee verschillende indelingen. Als u het aantal tabellen worden samengevoegd, controleert u of dat de dezelfde indeling voor het gebruik van beide tabellen die u van plan bent om samen te voegen.  
  
In het algemeen kan een transformatie op basis van aantal alleen worden toegepast op gegevenssets waarvoor hetzelfde schema als de gegevensset waarop de transformatie oorspronkelijk is gemaakt.  
  
 <!-- For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Uitzondering berichten|  
|------------------------|  
|Ongeldige tellen transformatie opgegeven.|  
|De telling transformatie op invoerpoort '{0}' is ongeldig.|  
|De telling transformatie op invoerpoort '{0}'kan niet worden samengevoegd met de transformatie tellen aan de invoerpoort'{1}'. Controleer om de metagegevens gebruikt voor het tellen van overeenkomsten.|  
  

## <a name="error-0087"></a>Fout 0087  
 Er treedt op wanneer een ongeldig aantal tabeltype is opgegeven voor leren met tellingen modules.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u probeert te importeren van een bestaande count-tabel, maar de tabel niet compatibel met de huidige gegevens of met het nieuwe aantal tabel is.  
  
**Oplossing:** Er zijn verschillende indelingen voor het opslaan van het aantal en de regels die gezamenlijk de transformatie. Als u het aantal tabellen worden samengevoegd, controleert u of dat beide dezelfde indeling gebruiken.  
  
 Over het algemeen kan een transformatie op basis van aantal alleen worden toegepast op gegevenssets waarvoor hetzelfde schema als de gegevensset waarop de transformatie oorspronkelijk is gemaakt.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). -->
  

## <a name="error-0088"></a>Fout 0088  
 Er treedt op wanneer een ongeldig tellen type is opgegeven voor leren met tellingen modules.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u probeert te gebruiken een andere tellen methode dan wordt ondersteund voor parametrisatie op basis van aantal.  
  
**Oplossing:** In het algemeen worden tellen methoden in een vervolgkeuzelijst gekozen, zodat u deze fout niet ziet.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Uitzondering berichten|  
|------------------------|  
|Ongeldig type telling is opgegeven.|  
|Het opgegeven type tellen '{0}' is geen geldig type tellen.|  
  

## <a name="error-0089"></a>Fout 0089  
 Er treedt op wanneer het opgegeven aantal klassen kleiner dan het werkelijke aantal klassen in een gegevensset die wordt gebruikt is voor het tellen van.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u het maken van een tabel van het aantal en de labelkolom een ander aantal klassen dan u hebt opgegeven in de moduleparameters bevat.  
  
**Oplossing:** Controleer uw gegevensset en ontdek precies hoe veel afzonderlijke waarden (mogelijk klassen) er in de labelkolom zijn. Wanneer u het aantal tabel maakt, moet u ten minste dit aantal klassen.  
  
 De tabel aantal kan niet automatisch te bepalen het aantal beschikbare klassen.  
  
 Wanneer u het aantal tabel maakt, kunt u 0 niet opgeven of een nummer dat kleiner is dan het werkelijke aantal klassen in de labelkolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Het aantal klassen is onjuist. Zorg ervoor dat het aantal klassen die u in het deelvenster parameter opgeeft groter dan of gelijk zijn aan het aantal klassen in de labelkolom is.|  
|Het aantal opgegeven klassen is '{0}', dat is niet groter zijn dan een waarde voor het label'{1}' in de gegevensset die is gebruikt voor het tellen. Zorg ervoor dat het aantal klassen die u in het deelvenster parameter opgeeft groter dan of gelijk zijn aan het aantal klassen in de labelkolom is.|  
  

## <a name="error-0090"></a>Fout 0090  
 Er treedt op wanneer de Hive-tabel maken is mislukt.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u gebruikmaakt van [gegevens exporteren](export-data.md) of een andere optie voor het opslaan van gegevens in een HDInsight-cluster en de opgegeven Hive-tabel kan niet worden gemaakt.  
  
**Oplossing:** Controleer de naam van de Azure storage-account dat is gekoppeld aan het cluster en controleer of dat u van hetzelfde account in de eigenschappen van de module gebruikmaakt.  
  
|Uitzondering berichten|  
|------------------------|  
|De Hive-tabel kan niet worden gemaakt. Controleer of de naam van het Azure-opslag die is gekoppeld aan het cluster is hetzelfde als wat wordt doorgegeven via de module-parameter voor een HDInsight-cluster.|  
|De Hive-tabel "{0}' kan niet worden gemaakt. Controleer of de naam van het Azure-opslag die is gekoppeld aan het cluster is hetzelfde als wat wordt doorgegeven via de module-parameter voor een HDInsight-cluster.|  
|De Hive-tabel "{0}' kan niet worden gemaakt. Controleer of de Azure storage-accountnaam die zijn gekoppeld aan het cluster is voor een HDInsight-cluster, '{1}'.|  
 

## <a name="error-0100"></a>Fout 0100  
 Er treedt op wanneer een niet-ondersteunde taal is opgegeven voor een aangepaste module.  
  
 Deze fout in Azure Machine Learning treedt op bij het bouwen van een aangepaste module en de eigenschap name van de **taal** -element in een XML-definitie-bestand van aangepaste module heeft een ongeldige waarde. Op dit moment de enige geldige waarde voor deze eigenschap is `R`. Bijvoorbeeld:  
  
 `<Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Oplossing:** Controleer de eigenschap name van de **taal** -element in het definitie aangepaste module xml-bestand is ingesteld op `R`. Sla het bestand, de aangepaste module-zip-pakket bijwerken en probeer het opnieuw toe te voegen de aangepaste module.  
  
|Uitzondering berichten|  
|------------------------|  
|Aangepaste module voor niet-ondersteunde taal die is opgegeven|  
  

## <a name="error-0101"></a>Fout 0101  
 Alle poort en de parameter-ID moeten uniek zijn.  
  
 Deze fout in Azure Machine Learning treedt op wanneer een of meer poorten of parameters zijn toegewezen de dezelfde id-waarde in een aangepaste module definitie XML-bestand.  
  
**Oplossing:** Controleer of de id-waarden over alle poorten en parameters uniek zijn. Sla het xml-bestand, de aangepaste module-zip-pakket bijwerken en probeert toe te voegen van de aangepaste module opnieuw.  
  
|Uitzondering berichten|  
|------------------------|  
|Alle poort en de parameter ID's voor een module moeten uniek zijn|  
|Module '{0}' dubbele poort/argument id's heeft. Alle poort/argument-id's moeten uniek zijn voor een module.|  
  

## <a name="error-0102"></a>Fout 0102  
 Gegenereerd wanneer een ZIP-bestand kan niet worden geëxtraheerd.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een ZIP-pakket met de extensie .zip wilt importeren, maar het pakket niet is een zip-bestand of het bestand een ondersteunde zip-bestand niet gebruiken.  
  
**Oplossing:** Zorg ervoor dat het geselecteerde bestand is een geldige ZIP-bestand en het is gecomprimeerd met behulp van een van de ondersteunde compressie-algoritmen.  
  
 Als u deze foutmelding krijgt bij het importeren van gegevenssets in gecomprimeerde vorm, controleert u of alle ingesloten bestanden gebruik een van de ondersteunde bestandsindelingen, en in Unicode-indeling.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->
  
 Probeer het opnieuw toe te voegen de gewenste bestanden naar een nieuwe gecomprimeerde ZIP-map en probeer het opnieuw toe te voegen de aangepaste module.  
  
|Uitzondering berichten|  
|------------------------|  
|De opgegeven POSTCODE is bestand niet de juiste indeling|  


## <a name="error-0103"></a>Fout 0103  
 Gegenereerd wanneer een ZIP-bestand bevat geen eventuele XML-bestanden  
  
 Deze fout in Azure Machine Learning treedt op wanneer de aangepaste module-zip-pakket module-definitiebestanden (.xml bevat). Deze bestanden moeten zich bevinden in de hoofdmap van het zip-pakket (bijvoorbeeld niet in een submap.)  
  
**Oplossing:** Controleer of dat een of meer XML-module definitiebestanden zich in de hoofdmap van het zip-pakket door het uitpakken naar een tijdelijke map op de schijf. XML-bestanden moeten worden rechtstreeks in de map die u het zip-pakket hebt uitgepakt. Zorg ervoor dat bij het maken van het zip-pakket dat u een map met XML-bestanden als Hiermee u een submap in het zip-pakket met dezelfde naam als de map die u hebt geselecteerd maakt voor het zip-zip niet selecteert.  
  
|Uitzondering berichten|  
|------------------------|  
|De opgegeven POSTCODE bevat bestand geen module definitiebestanden (XML-bestanden)|  


## <a name="error-0104"></a>Fout 0104  
 Gegenereerd wanneer een module-definitiebestand verwijst naar een script dat kan niet gevonden worden  
  
 Deze fout in Azure Machine Learning wordt gegenereerd wanneer een definitie aangepaste module xml-bestand verwijst naar een scriptbestand in de **taal** element dat niet in het zip-pakket bestaat. Pad naar het script is gedefinieerd in de **bronbestand** eigenschap van de **taal** element. Het pad naar het bronbestand is ten opzichte van de hoofdmap van het zip-pakket (dezelfde locatie als de module XML-definitie-bestanden). Als het scriptbestand in een submap is, kan het relatieve pad naar het scriptbestand moet worden opgegeven. Bijvoorbeeld, als alle scripts zijn opgeslagen in een **myScripts** map in het zip-pakket, de **taal** element zelf zou moeten dit pad toevoegen de **bronbestand** eigenschap hieronder. Bijvoorbeeld:  
  
 `<Language name="R" sourceFile="myScripts/CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Oplossing:** Zorg ervoor dat de waarde van de **bronbestand** eigenschap in de **taal** element van de xml-definitie van aangepaste module juist is en dat het bronbestand bestaat in de juiste relatieve pad in het zip-pakket.  
  
|Uitzondering berichten|  
|------------------------|  
|Waarnaar wordt verwezen, R-scriptbestand bestaat niet.|  
|Waarnaar wordt verwezen, R-scriptbestand '{0}' kan niet worden gevonden. Zorg ervoor dat het relatieve pad naar het bestand juist vanaf de locatie van de definities is.|  


## <a name="error-0105"></a>Fout 0105  
 Deze fout wordt weergegeven als een module-definitiebestand een niet-ondersteund parametertype bevat  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer u een aangepaste module xml-definitie maken en het type van een parameter of in het definitie komt niet overeen met een ondersteund type.  
  
**Oplossing:** Zorg ervoor dat de eigenschap type van een **func** -element in het definitie aangepaste module xml-bestand is een ondersteund type.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteund parametertype.|  
|Niet-ondersteund parametertype '{0}is opgegeven.|  


## <a name="error-0106"></a>Fout 0106  
 Gegenereerd wanneer een module-definitiebestand een niet-ondersteund type gebruikersinvoer definieert  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer het type van een invoerpoort in een aangepaste module XML-definitie komt niet overeen met een ondersteund type.  
  
**Oplossing:** Zorg ervoor dat de eigenschap type van een invoer-element in het definitiebestand van de aangepaste module-XML een ondersteund type is.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteund type gebruikersinvoer.|  
|Niet-ondersteund type gebruikersinvoer '{0}is opgegeven.|  


## <a name="error-0107"></a>Fout 0107  
 Gegenereerd wanneer een module-definitiebestand u een niet-ondersteunde uitvoertype definieert  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer het type van een uitvoerpoort in een xml-definitie van aangepaste module komt niet overeen met een ondersteund type.  
  
**Oplossing:** Zorg ervoor dat de eigenschap type van een uitvoer-element in het definitiebestand van de aangepaste module-xml een ondersteund type is.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteunde uitvoertype.|  
|Niet-ondersteund uitvoertype '{0}is opgegeven.|  


## <a name="error-0108"></a>Fout 0108  
 Gegenereerd wanneer een module-definitiebestand meer invoer- of -poorten definieert dan wordt ondersteund  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer er te veel invoer- of -poorten zijn gedefinieerd in een xml-definitie van aangepaste module.  
  
**Oplossing:** Zorgt ervoor dat het maximum aantal invoer- en poorten die zijn gedefinieerd in de aangepaste module xml-definitie niet groter zijn dan het maximum aantal ondersteunde poorten.  
  
|Uitzondering berichten|  
|------------------------|  
|Ondersteunde aantal invoer- of -poorten is overschreden.|  
|Overschreden aantal ondersteund '{0}' poorten. Maximaal toegestane aantal van '{0}'poorten is'{1}'.| 

## <a name="error-0109"></a>Fout 0109  
 Gegenereerd wanneer een module-definitiebestand een kolomkiezer onjuist definieert  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer de syntaxis voor een argument van de datumkiezer kolom een fout in de xml-definitie van een aangepaste module bevat.  
  
**Oplossing:** Deze fout wordt gemaakt wanneer de syntaxis voor een argument van de datumkiezer kolom een fout in de xml-definitie van een aangepaste module bevat.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteunde syntaxis voor de kolomkiezer.|  
  

## <a name="error-0110"></a>Fout 0110  
 Gegenereerd wanneer een module-definitiebestand definieert een kolom kiezen die verwijst naar een niet-bestaande invoerpoort-ID  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer de *portId* eigenschap in het eigenschapselement van een argument van het type ColumnPicker komt niet overeen met de id-waarde van een invoerpoort.  
  
**Oplossing:** Zorg ervoor dat de eigenschap portId overeenkomt met de id-waarde van een invoerpoort gedefinieerd in de xml-definitie van aangepaste module.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolomkiezer verwijst naar een niet-bestaande invoerpoort-ID.|  
|Kolomkiezer verwijst naar een niet-bestaande invoerpoort-ID '{0}'.|  
  

## <a name="error-0111"></a>Fout 0111  
 Gegenereerd wanneer een module-definitiebestand een eigenschap is ongeldig definieert  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer een ongeldige eigenschap is toegewezen aan een element in de aangepaste module XML-definitie.  
  
**Oplossing:** Zorg ervoor dat de eigenschap wordt ondersteund door de aangepaste module-element.  
  
|Uitzondering berichten|  
|------------------------|  
|De eigenschapsdefinitie is ongeldig.|  
|De eigenschapsdefinitie van de '{0}' is ongeldig.|  
  

## <a name="error-0112"></a>Fout 0112  
 Gegenereerd wanneer een definitiebestand van de module kan niet worden geparseerd.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer er een fout optreedt in de xml-indeling waarmee wordt voorkomen de aangepaste module XML-definitie dat van worden geparseerd als een geldig XML-bestand.  
  
**Oplossing:** Zorg ervoor dat elk element wordt geopend en correct worden afgesloten. Zorg ervoor dat er geen fouten zijn opgetreden in de XML-opmaak.  
  
|Uitzondering berichten|  
|------------------------|  
|Module-definitie kan bestand niet parseren.|  
|Kan module definitie-bestand niet parseren '{0}'.|  
  

## <a name="error-0113"></a>Fout 0113  
 Gegenereerd wanneer een definitiebestand van de module fouten bevat.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer de aangepaste module definitie XML-bestand kan worden geparseerd, maar fouten, zoals de definitie van de elementen niet wordt ondersteund door aangepaste modules bevat.  
  
**Oplossing:** Zorg ervoor dat het definitiebestand van de aangepaste module-elementen en eigenschappen die worden ondersteund door aangepaste modules worden gedefinieerd.  
  
|Uitzondering berichten|  
|------------------------|  
|Module-definitiebestand bevat fouten.|  
|Module-definitiebestand '{0}' bevat fouten.|  
|Module-definitiebestand '{0}' bevat fouten. {1}|  
  

## <a name="error-0114"></a>Fout 0114  
 Een opgetreden bij het bouwen van een aangepaste module mislukt.  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer een build aangepaste module is mislukt. Dit gebeurt wanneer een of meer aangepaste module-gerelateerde fouten zijn opgetreden tijdens het toevoegen van de aangepaste module. De aanvullende fouten worden gerapporteerd binnen dit foutbericht wordt weergegeven.  
  
**Oplossing:** Los de fouten die is gerapporteerd binnen dit bericht van uitzondering.  
  
|Uitzondering berichten|  
|------------------------|  
|Maken van aangepaste module is mislukt.|  
|Aangepaste module builds is mislukt met de fout(en): {0}|  
  

## <a name="error-0115"></a>Fout 0115  
 Gegenereerd wanneer een Standaardscript aangepaste module een niet-ondersteunde extensie heeft.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een script voor een aangepaste module die gebruikmaakt van een onbekende extensie opgeeft.  
  
**Oplossing:** Controleer of de bestandsextensie voor indeling en de bestandsnaam van de scriptbestanden dat is opgenomen in de aangepaste module.  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteunde extensie voor Standaardscript.|  
|Niet-ondersteund bestandstype extensie {0} voor Standaardscript.|  
  

## <a name="error-0121"></a>Fout 0121  
 Wanneer SQL mislukt schrijft omdat de tabel niet beschrijfbaar is opgetreden  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer u gebruikmaakt van de [gegevens exporteren](export-data.md) module resultaten opslaan in een tabel in een SQL-database en de tabel kan niet worden geschreven. Normaal gesproken ziet u deze fout als de [gegevens exporteren](export-data.md) module is een verbinding maken met de SQL Server-exemplaar, maar kan niet schrijven van de inhoud van de Azure ML-gegevensset naar de tabel is.  
  
**Oplossing:**
 - Open het deelvenster met eigenschappen van de [gegevens exporteren](export-data.md) module en controleer of dat de namen van de database en tabel correct worden ingevoerd. 
 - Controleer het schema van de gegevensset die u wilt exporteren en zorg ervoor dat de gegevens compatibel met de doeltabel is.
 - Controleer of de SQL aanmelden die zijn gekoppeld aan de gebruikersnaam en wachtwoord is gemachtigd te schrijven naar de tabel. 
 - Als de uitzondering Aanvullende foutinformatie van SQL Server bevat, gebruikt u die informatie te corrigeren.  
  
|Uitzondering berichten|  
|------------------------|  
|Verbonden met server, kan niet schrijven naar de tabel.|  
|Kan niet schrijven naar de Sql-tabel: {0}|  


## <a name="error-0122"></a>Fout 0122  
 Er treedt op als meerdere gewicht kolommen worden opgegeven en slechts één is toegestaan.  
  
 Deze fout in Azure Machine Learning treedt op wanneer er te veel kolommen zijn geselecteerd als gewicht kolommen.  
  
**Oplossing:** Bekijk de invoergegevensset en bijbehorende metagegevens. Zorg ervoor dat slechts één kolom bevat het gewicht.  
  
|Uitzondering berichten|  
|------------------------|  
|Er zijn meerdere gewicht kolommen opgegeven.|  


## <a name="error-0123"></a>Fout 0123  
 Er treedt op als kolom van vectoren met Label-kolom is opgegeven.  
  
 Deze fout in Azure Machine Learning treedt op als u een vector als de labelkolom.  
  
**Oplossing:** De gegevensindeling van de kolom indien nodig wijzigen, of kies een andere kolom.  
  
|Uitzondering berichten|  
|------------------------|  
|Kolom van vectoren is opgegeven als kolom Label.|  


## <a name="error-0124"></a>Fout 0124  
 Er treedt op als niet-numerieke kolommen zijn opgegeven om te worden van de kolom gewicht.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Niet-numerieke kolom is opgegeven als de kolom gewicht.|  
  


## <a name="error-0125"></a>Fout 0125  
 Gegenereerd wanneer het schema voor meerdere gegevenssets komt niet overeen.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Gegevensset schema komt niet overeen.|  


## <a name="error-0126"></a>Fout 0126  
 Er treedt op als de gebruiker Hiermee geeft u een SQL-domein dat wordt niet ondersteund in Azure ML.  
  
 Deze fout wordt gemaakt wanneer de gebruiker Hiermee geeft u een SQL-domein dat wordt niet ondersteund in Azure Machine Learning. U ontvangt deze foutmelding als u probeert verbinding maken met een database-server in een domein dat is niet opgenomen in de whitelist. De toegestane SQL-domeinen zijn momenteel: ". database.windows.net ','. cloudapp.net ', of '. database.secure.windows.net '. Dat wil zeggen, moet de server een Azure SQL-server of een server in een virtuele machine op Azure.  
  
**Oplossing:** Terug naar de module. Controleer of dat de SQL database-server deel uitmaakt van een van de geaccepteerde domeinen:  
  
-   .database.windows.net  
  
-   .cloudapp.net  
  
-   .database.secure.windows.net  
  
|Uitzondering berichten|  
|------------------------|  
|Niet-ondersteunde SQL-domein.|  
|De SQL-domein {0} wordt momenteel niet ondersteund in Azure ML|  
  

## <a name="error-0127"></a>Fout 0127  
 Afbeeldingsgrootte pixel overschrijdt de toegestane limiet  
  
 Deze fout treedt op als u bij het lezen van afbeeldingen uit de gegevensset van een installatiekopie voor classificatie en de afbeeldingen groter is zijn dan het model kan worden verwerkt.  
  
 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  
  
-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->
  
|Uitzondering berichten|  
|------------------------|  
|Afbeeldingsgrootte pixel overschrijdt de toegestane limiet.|  
|Afbeeldingsgrootte van pixel in het bestand '{0}' overschrijdt de toegestane limiet: '{1}'|  


## <a name="error-0128"></a>Fout 0128  
 Aantal voorwaardelijke kansen voor categorische kolommen overschrijdt.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Aantal voorwaardelijke kansen voor categorische kolommen overschrijdt.|  
|Aantal voorwaardelijke kansen voor categorische kolommen overschrijdt. Kolommen{0}'en'{1}' het problematische paar zijn.|  


## <a name="error-0129"></a>Fout 0129  
 Aantal kolommen in de gegevensset overschrijdt de toegestane limiet.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Aantal kolommen in de gegevensset overschrijdt de toegestane limiet.|  
|Aantal kolommen in de gegevensset in '{0}'is groter dan toegestaan.'|  
|Aantal kolommen in de gegevensset in '{0}'overschrijdt de toegestane limiet van'{1}'.'|  
|Aantal kolommen in de gegevensset in '{0}'overschrijdt de toegestane'{1}'beperken van'{2}'.'|  
## <a name="error-0130"></a>Fout 0130  
 Er treedt op wanneer alle rijen in de gegevensset training ontbrekende waarden bevatten.  
  
 Dit gebeurt wanneer een van de kolommen in de gegevensset training leeg is.  
  
**Oplossing:** Gebruik de [Clean Missing Data](clean-missing-data.md) module kolommen met alle ontbrekende waarden moeten worden verwijderd.  
  
|Uitzondering berichten|  
|------------------------|  
|Alle rijen in de gegevensset training bevatten ontbrekende waarden.  Overweeg het gebruik van de module Clean Missing Data om ontbrekende waarden te verwijderen.|  
 

## <a name="error-0131"></a>Fout 0131  
 Er treedt op als een of meer gegevenssets in een zip-bestand niet kan worden uitgepakt en juist geregistreerd  
  
 Deze fout wordt gemaakt wanneer een of meer gegevenssets in een zip-bestand niet kan worden uitgepakt en correct worden gelezen. U ontvangt deze foutmelding als het uitpakken is mislukt omdat het zip-bestand zelf of een van de bestanden erin beschadigd is of er een systeemfout opgetreden is tijdens het uitpakken en uitbreiden van een bestand.  
  
**Oplossing:** Gebruik de informatie in het foutbericht wordt weergegeven om te bepalen hoe om door te gaan.  
  
|Uitzondering berichten|  
|------------------------|  
|ZIP gegevenssets kan niet uploaden|  
|Gegevensset ingepakte {0} is mislukt met het volgende bericht: {1}|  
|Gegevensset ingepakte {0} is mislukt met een {1} uitzondering met bericht: {2}|  
  

## <a name="error-0132"></a>Fout 0132  
 Er geen bestandsnaam is opgegeven voor het uitpakken van; meerdere bestanden zijn gevonden in het zip-bestand.  
  
 Deze fout wordt gemaakt wanneer er geen bestandsnaam is opgegeven voor het uitpakken van; meerdere bestanden zijn gevonden in het zip-bestand. U ontvangt deze foutmelding als het ZIP-bestand meer dan een gecomprimeerd bestand bevat, maar u geen voor uitpakken in een bestand opgegeven hebt de **gegevensset uitpakken** tekst in het vak de **eigenschap** deelvenster van de module. Telkens wanneer die de module wordt uitgevoerd kan op dit moment aan slechts één bestand worden geëxtraheerd.  
  
**Oplossing:** Het foutbericht bevat een lijst van de bestanden die in het ZIP-bestand vinden. De naam van het gewenste bestand kopiëren en plak deze in de **gegevensset uitpakken** in het tekstvak.  
  
|Uitzondering berichten|  
|------------------------|  
|ZIP-bestand bevat meerdere bestanden. u moet het bestand om uit te vouwen.|  
|Het bestand bevat meer dan één bestand. Geef het bestand om uit te vouwen. De volgende bestanden zijn aangetroffen: {0}|  
  

## <a name="error-0133"></a>Fout 0133  
 Het opgegeven bestand is niet gevonden in het zip-bestand  
  
 Deze fout wordt gemaakt wanneer de bestandsnaam is opgegeven in de **gegevensset uitpakken** veld van de **eigenschap** deelvenster komt niet overeen met de naam van elk bestand gevonden in het ZIP-bestand. De meest voorkomende oorzaken van deze fout wordt een fout te typen of te zoeken naar de verkeerde archiefbestand voor het bestand om uit te vouwen.  
  
**Oplossing:** Terug naar de module. Als de naam van het bestand dat u bedoeld om u te decomprimeren wordt weergegeven in de lijst met bestanden die zijn gevonden, kopieert u de bestandsnaam en plak deze in de **gegevensset uitpakken** vak van de eigenschap. Als u de gewenste bestandsnaam in de lijst niet ziet, controleert u of u hebt de juiste ZIP-bestand en de juiste naam voor het gewenste bestand.  
  
|Uitzondering berichten|  
|------------------------|  
|Het opgegeven bestand is niet gevonden int het zip-bestand.|  
|Het opgegeven bestand is niet gevonden. De volgende bestanden te vinden: {0}|  
  

## <a name="error-0134"></a>Fout 0134
Er treedt op als labelkolom ontbreekt of onvoldoende aantal gelabelde rijen heeft.  
  
Deze fout treedt op wanneer de module is een labelkolom vereist, maar u niet in de kolom selecteren opgenomen hebt of te veel waarden in de labelkolom ontbreekt.

Deze fout kan ook optreden als een eerdere bewerking met de gegevensset wordt gewijzigd zodat onvoldoende rijen beschikbaar voor een downstream-bewerking zijn. Stel bijvoorbeeld dat u een expressie in de **partitie en steekproef** module aan een gegevensset delen door waarden. Als er geen overeenkomsten worden gevonden voor uw expressie, zou een van de gegevenssets die voortvloeien uit de partitie niet leeg zijn.

Oplossing: 

 Als u een labelkolom in de kolom selecteren opnemen, maar deze niet wordt herkend, gebruikt u de [metagegevens bewerken](edit-metadata.md) module om deze te markeren als een labelkolom.
  
  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->Vervolgens kunt u de [Clean Missing Data](clean-missing-data.md) module voor het verwijderen van rijen met ontbrekende waarden in de labelkolom. 

 Controleer uw invoer gegevenssets om ervoor te zorgen dat ze geldige gegevens- en genoeg rijen om te voldoen aan de vereisten van de bewerking bevatten. Veel algoritmen genereert een foutbericht weergegeven als ze nodig enkele minimum aantal rijen met gegevens hebben, maar de gegevens slechts een paar rijen of alleen een koptekst bevat.
  
|Uitzondering berichten|
|------------------------|
|Er treedt op als labelkolom ontbreekt of onvoldoende aantal gelabelde rijen heeft.|  
|Er treedt op wanneer het labelkolom ontbreekt of heeft minder dan {0} rijen met labels|  
  

## <a name="error-0135"></a>Fout 0135  
 Alleen massamiddelpunt gebaseerd cluster wordt ondersteund.  
  
**Oplossing:** U kunt dit foutbericht tegenkomen als u hebt geprobeerd om te evalueren clustering-model dat is gebaseerd op een aangepaste clustering-algoritme dat centroids niet gebruikt voor het initialiseren van het cluster.  
  
  <!--You can use [Evaluate Model](evaluate-model.md) to evaluate clustering models that are based on the  [K-Means Clustering](k-means-clustering.md) module. For custom algorithms, use the [Execute R Script](execute-r-script.md) module to create a custom evaluation script.  -->
  
|Uitzondering berichten|  
|------------------------|  
|Alleen massamiddelpunt gebaseerd cluster wordt ondersteund.|  
  

## <a name="error-0136"></a>Fout 0136  
 Er is geen bestandsnaam is geretourneerd. kan niet als gevolg hiervan verwerken van het bestand.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Er is geen bestandsnaam is geretourneerd. kan niet als gevolg hiervan verwerken van het bestand.|  
  

## <a name="error-0137"></a>Fout 0137  
 Azure Storage-SDK is een fout opgetreden bij het converteren van tussen de eigenschappen van tabellen en kolommen van de gegevensset tijdens het lezen of schrijven.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Conversiefout tussen de Azure table storage eigenschap en gegevensset kolom.|  
|Conversiefout tussen de Azure table storage eigenschap en gegevensset kolom. Aanvullende informatie: {0}|  

## <a name="error-0138"></a>Fout 0138  
 Geheugen is bereikt, kan de volledige uitvoering van de module niet. Downsampling de gegevensset mogelijk kunt u het probleem te verlichten.  
  
 Deze fout treedt op wanneer de module met meer geheugen dan beschikbaar in de Azure-container is vereist. Dit kan gebeuren als u met een grote gegevensset werkt en de huidige bewerking in het geheugen te groot.  
  
**Oplossing:** Als u probeert te lezen van een grote gegevensset en de bewerking kan niet worden voltooid, downsampling de gegevensset kan helpen.  
  
  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  
  
 Sometimes transient loads can lead to this error. Machine support also changes over time. 
  
 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->
  
|Uitzondering berichten|  
|------------------------|  
|Geheugen is bereikt, kan de volledige uitvoering van de module niet.|  
  

## <a name="error-0139"></a>Fout 0139  
 Er treedt op wanneer het is niet mogelijk een kolom converteren naar een ander type.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u probeert te converteren van een kolom in een ander gegevenstype, maar dat type wordt niet ondersteund door de huidige bewerking of door de module.  
  
 De fout mogelijk ook weergegeven als een module probeert de gegevens om te voldoen aan de vereisten van de huidige module impliciet converteren, maar de conversie niet mogelijk is.  
  
**Oplossing:**

1. Controleer uw invoergegevens en bepaal de exacte gegevenstype van de kolom die u wilt gebruiken en het gegevenstype van de kolom die de fout is produceren. Soms denkt het gegevenstype correct is, maar vinden dat een upstream bewerking de gegevens van het type of het gebruik van een kolom is gewijzigd. Gebruik de [metagegevens bewerken](edit-metadata.md) module kolommetagegevens terugzetten naar de oorspronkelijke staat. 
2. Bekijk de module help-pagina om te controleren of de vereisten voor de opgegeven bewerking. Bepaal welke typen worden ondersteund door de huidige module en welke reeks waarden wordt ondersteund. 
 <!--3. If values need to be truncated, rounded, or outliers removed, use the [Apply Math Operation](apply-math-operation.md) or [Clip Values](clip-values.md) modules to make corrections.-->
4. Overweeg of het is mogelijk om te zetten of de kolom in een ander gegevenstype cast-conversie uitvoeren. De volgende alle modules bieden aanzienlijke flexibiliteit en power voor het wijzigen van gegevens: 
 <!--
   + [Apply SQL Transformation](apply-sql-transformation.md)
   + [Execute R Script](execute-r-script.md)
-->   
   + [Python-Script uitvoeren](execute-python-script.md).  

> [!NOTE]
> Nog steeds niet werkt? Houd rekening met aanvullende feedback geven over het probleem dat, om ons beter oplossen van problemen met een leidraad te helpen. Alleen feedback indienen over deze pagina en geef de naam van de module die de fout en de conversie van gegevens die niet zijn gegenereerd.
  
|Uitzondering berichten|  
|------------------------|  
|Conversie toegestaan niet.|  
|Kan niet worden geconverteerd: {0}.|  
|Kan niet worden geconverteerd: {0}, op rij {1}.|  
|Kolom van het type kan niet worden geconverteerd {0} naar kolom van het type {1} op rij {2}.|  
|Kan niet converteren voor kolom '{2}' van het type {0} naar kolom van het type {1} op rij {3}.|  
|Kolom kan niet worden geconverteerd "{2}' van het type {0} naar kolom"{3}' van het type {1} op rij {4}.| 

## <a name="error-0140"></a>Fout 0140  
 Er treedt op als doorgegeven argument voor set kolom bevat geen andere kolommen behalve de labelkolom.  
  
 Deze fout treedt op als u een gegevensset hebt verbonden met een module waarvoor meerdere kolommen, inclusief functies, maar u alleen de labelkolom hebt opgegeven.  
  
**Oplossing:** Kies ten minste één functie kolom om op te nemen in de gegevensset.  
  
|Uitzondering berichten|  
|------------------------|  
|Instellen van de opgegeven kolom bevat geen andere kolommen behalve de labelkolom.|  
  

## <a name="error-0141"></a>Fout 0141  
 Er treedt op als het nummer van de geselecteerde numerieke kolommen en de unieke waarden in de categorische en de tekenreekskolommen te klein is.  
  
 Deze fout in Azure Machine Learning treedt op wanneer er niet voldoende unieke waarden in de geselecteerde kolom de bewerking uit te voeren.  
  
**Oplossing:** Bepaalde bewerkingen uitvoeren van statistische bewerkingen op de functie en categorische kolommen, en als er niet voldoende waarden, de bewerking kan mislukken of een ongeldig resultaat geretourneerd. Controleer uw gegevensset om te zien hoeveel waarden er in de functie en het label kolommen zijn en bepalen of de bewerking die u probeert uit te voeren statistisch geldig is.  
  
 Als de brongegevensset geldig is, kunt u ook controleren of een gegevensbron stroomopwaarts bewerken of de metagegevens van de bewerking heeft de gegevens gewijzigd en sommige waarden verwijderd.  
  
 Als de upstream bewerkingen zijn onder meer splitsen, steekproeven te nemen of pixels, controleert u of dat de uitvoer het verwachte aantal rijen en waarden bevatten.  
  
|Uitzondering berichten|  
|------------------------|  
|Het nummer van de geselecteerde numerieke kolommen en de unieke waarden in de categorische en de tekenreekskolommen is te klein.|  
|Het totale aantal van de geselecteerde numerieke kolommen en de unieke waarden in de categorische en de tekenreekskolommen (momenteel {0}) moet ten minste {1}|  
  

## <a name="error-0142"></a>Fout 0142  
 Er treedt op wanneer het systeem kan niet worden geladen certificaat om te verifiëren.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Het certificaat kan niet worden geladen.|  
|Het certificaat {0} kan niet worden geladen. De vingerafdruk is {1}.|  
  

## <a name="error-0143"></a>Fout 0143  
 Kan de gebruiker opgegeven URL die moet worden vanuit GitHub niet parseren.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een ongeldige URL opgeeft en de module een geldige URL in GitHub moet.  
  
**Oplossing:** Controleer of dat de URL naar een geldige GitHub-opslagplaats verwijst. Andere typen sites worden niet ondersteund.  
  
|Uitzondering berichten|  
|------------------------|  
|Er is geen URL van github.com.|  
|URL is niet afkomstig van github.com: {0}|  

## <a name="error-0144"></a>Fout 0144  
 Gebruiker opgegeven GitHub-url, de verwachte onderdeel ontbreekt.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u een GitHub-bestandsbron met een ongeldige URL-indeling opgeeft.  
  
**Oplossing:** Controleer of de URL van de GitHub-opslagplaats geldig is en met \blob\ of \tree eindigt\\.  
  
|Uitzondering berichten|  
|------------------------|  
|Kan de GitHub-URL niet parseren.|  
|Kan de GitHub-URL niet parseren (verwacht ' \blob\\' of ' \tree\\' achter de naam van de opslagplaats): {0}|  

## <a name="error-0145"></a>Fout 0145  
 Kan de replicatie-map niet maken voor een of andere reden.  
  
 Deze fout in Azure Machine Learning treedt op wanneer de module niet kan de opgegeven map niet maken.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Kan de replicatie-map niet maken.|  
  

## <a name="error-0146"></a>Fout 0146  
 Wanneer de gebruikersbestanden uitgepakt naar de lokale map zijn, is het mogelijk dat het gecombineerde pad te lang.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u bij het uitpakken van bestanden, maar sommige bestandsnamen te lang is zijn als uitgepakt.  
  
**Oplossing:** De bestandsnamen bewerken zodat gecombineerd pad en bestandsnaam is niet langer zijn dan 248 tekens.  
  
|Uitzondering berichten|  
|------------------------|  
|Replicatiepad is langer dan 248 tekens, verkort de scriptnaam of het pad.|  

## <a name="error-0147"></a>Fout 0147  
 Kan geen materiaal downloaden vanuit GitHub om een bepaalde reden  
  
 Deze fout in Azure Machine Learning treedt op wanneer u deze niet kan lezen of de opgegeven bestanden vanuit GitHub downloaden.  
  
**Oplossing:** Het probleem mogelijk tijdelijk. u kunt proberen toegang tot de bestanden op een later tijdstip. Of Controleer of dat u de benodigde machtigingen hebt en dat de bron ongeldig is.  
  
|Uitzondering berichten|  
|------------------------|  
|Bij de toegang van GitHub.|  
|Bij de toegang van GitHub. {0}|  
  

## <a name="error-0148"></a>Fout 0148  
 Toegangsproblemen met niet-geautoriseerde tijdens het ophalen van gegevens of het maken van map.  
  
 Deze fout in Azure Machine Learning treedt op wanneer u probeert te maken van een map of het lezen van gegevens uit de opslag, maar niet de juiste machtigingen hebben.  
  
**Oplossing:**
  
|Uitzondering berichten|  
|------------------------|  
|Uitzondering tijdens het extraheren van gegevens voor onbevoegde toegang.|  
  

## <a name="error-0149"></a>Fout 0149  
 Het bestand bestaat niet in de GitHub-bundel.  
  
 Deze fout in Azure Machine Learning treedt op wanneer het opgegeven bestand kan niet worden gevonden.  
  
Oplossing: 
  
|Uitzondering berichten|  
|------------------------|  
|GitHub-bestand is niet gevonden.|  
|GitHub-bestand is niet gevonden.: {0}|  
  

## <a name="error-0150"></a>Fout 0150  
 De scripts die afkomstig van het pakket van de gebruiker zijn kunnen niet worden uitgepakt, waarschijnlijk vanwege een conflict met GitHub-bestanden.  
  
 Deze fout in Azure Machine Learning treedt op wanneer een script kan niet worden uitgepakt, gewoonlijk wanneer er een bestaand bestand met dezelfde naam.  
  
Oplossing:
  
|Uitzondering berichten|  
|------------------------|  
|Kan niet voor het uitpakken van de bundel; mogelijk is de naam conflicteert met GitHub-bestanden.|  
  

## <a name="error-0151"></a>Fout 0151  
 Er is een fout opgetreden bij het schrijven naar de cloudopslag. Controleer of de URL.  
  
 Deze fout in Azure Machine Learning treedt op wanneer de module probeert de gegevens te schrijven naar opslag in de cloud, maar de URL niet beschikbaar of is ongeldig is.  
  
Oplossing: Controleer of de URL en controleer of dat deze schrijfbaar is.  
  
|Uitzondering berichten|  
|------------------------|  
|Fout bij het schrijven naar de cloudopslag (mogelijk een onjuiste url).|  
|Fout bij het schrijven naar de cloud storage: {0}. Controleer of de url.|  
  
## <a name="error-0152"></a>Fout 0152  
 Het type Azure-cloud is onjuist opgegeven in de context van de module.  
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldige Azure-Cloud-Type|  
|Ongeldige Azure-Cloud-Type: {0}|  
  
## <a name="error-0153"></a>Fout 0153  
 De opgegeven opslag-eindpunt is ongeldig.  
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldige Azure-Cloud-Type|  
|Ongeldige Storage-eindpunt: {0}|  

## <a name="error-0154"></a>Fout 0154  
 Naam van de opgegeven server kan niet worden omgezet.  
  
|Uitzondering berichten|  
|------------------------|  
|Naam van de opgegeven server kan niet worden omgezet.|  
|De opgegeven server {0}. documents.azure.com kan niet worden omgezet|

## <a name="error-0155"></a>Fout 0155  
 De DocDb-Client heeft een uitzondering gegenereerd.  
  
|Uitzondering berichten|  
|------------------------|  
|De DocDb-Client heeft een uitzondering gegenereerd.|  
|DocDb-Client: {0}|

## <a name="error-0156"></a>Fout 0156  
 Ongeldig antwoord voor HCatalog-Server.  
  
|Uitzondering berichten|  
|------------------------|  
|Ongeldig antwoord voor HCatalog-Server. Controleer of alle services worden uitgevoerd.|  
|Ongeldig antwoord voor HCatalog-Server. Controleer of alle services worden uitgevoerd. Foutdetails: {0}|

## <a name="error-0157"></a>Fout 0157  
 Er is een fout opgetreden bij het lezen van het Azure Cosmos DB vanwege inconsistente of een ander document schema's. Lezer vereist dat alle documenten hetzelfde schema hebben.  
  
|Uitzondering berichten|  
|------------------------|  
|Gedetecteerde documenten met verschillende schema's. Zorg ervoor dat alle documenten hebben hetzelfde schema|

## <a name="error-1000"></a>Fout 1000  
Interne bibliotheek van uitzondering.  
  
Deze fout wordt geboden om vast te leggen, anders niet-verwerkte interne-engine van fouten. Daarom kan de oorzaak van deze fout afwijken afhankelijk van de module die de fout is gegenereerd.  
  
Als u hulp nodig hebt, is het raadzaam dat u het gedetailleerde bericht die wordt meegestuurd met de fout naar de Azure Machine Learning-forum, samen met een beschrijving van het scenario, met inbegrip van de gegevens die worden gebruikt als invoer plaatsen. Deze feedback helpt ons prioriteit fouten en identificeren van de belangrijkste problemen voor verdere werk.  
  
|Uitzondering berichten|  
|------------------------|  
|Uitzondering van de bibliotheek.|  
|Bibliotheek uitzondering: {0}|  
|{0} Bibliotheek uitzondering: {1}|  
