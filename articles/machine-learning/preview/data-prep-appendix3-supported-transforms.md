---
title: Gegevens transformaties gebruiken voor het voorbereiden van gegevens in Azure Machine Learning | Microsoft Docs
description: Dit artikel bevat een volledige lijst van transformaties beschikbaar voor het voorbereiden van gegevens van Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: 062143ae34450484cd66cd7364fb8839d697e483
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Gegevens transformaties gebruiken voor het voorbereiden van gegevens in Azure Machine Learning

Een *transformeren* in Azure Machine Learning gebruikt gegevens in een bepaalde indeling, voert u een bewerking op de gegevens (zoals het wijzigen van het gegevenstype) en vervolgens produceert gegevens in de nieuwe indeling. Elke transformatie heeft een eigen interface en gedrag. Door verschillende transformaties samen chaining via de stappen in de gegevensstroom, kunt u complexe en herhaalbare transformaties uitvoeren op uw gegevens. Dit is de kern van voorbereiding-functionaliteit.

Hier volgen de transformaties die beschikbaar zijn in Azure Machine Learning. 

## <a name="column-selection"></a>Kolom selecteren 
Veel van de transformaties in deze lijst werken op één kolom of veel. Schakel meerdere kolommen, gebruiken de Ctrl-toets. Als u een reeks kolommen, gebruikt u de Shift-toets.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Transformaties vanuit het hoofdmenu of de koptekst van het raster 
Hiermee transformeert u toegang via de optie transformaties in het hoofdmenu. U kunt ook transformaties selecteren met de rechtermuisknop op de naam van de kolom in het gegevensraster. Als meerdere kolommen zijn geselecteerd, biedt met de rechtermuisknop op een van deze een menu transformaties.

Het snelmenu toont alleen geldige transformaties voor het gegevenstype dat is geselecteerd. Het hoofdmenu biedt alle transformaties maar uitgeschakeld die geen verband houden met de geselecteerde kolommen.

Een kleine subset van contextuele transformaties is beschikbaar met de rechtermuisknop op een cel. Deze transformaties worden gekopieerd, vervangen en filteren. Dit zijn gegevens type-bewust is, zodat de opties voor een numerieke kolom anders dan voor een kolom met tekenreeksen zijn.

## <a name="derive-column-by-example"></a>Afgeleid kolom door voorbeeld
Gebruik deze transformatie naar een nieuwe kolom maken als een afgeleide van een of meer bestaande kolommen. De transformatie wordt gekeken naar de invoerkolommen (geselecteerd) en het voorbeeld en vervolgens bepaalt de gewenste uitvoer in de nieuwe kolom. 

Selecteer een of meer kolommen voor het gebruik van deze transformatie. Een nieuwe (lege) afgeleide kolom toevoegen door voorbeeld. Typ een voorbeeld van wat u zien in de afgeleide kolom wilt (ervan uitgaande dat deze afgeleid van andere kolommen) en de "voorbeeld" technologie probeert te vullen in de andere cellen in de kolom. 

Voor voorbeelden van complexe moet u mogelijk vindt u meer dan één voorbeeld. U doet dit door een andere cel selecteert en typ een ander voorbeeld.

De technologie "Voorbeeld" gebruikt geselecteerde kolommen om te bepalen van de betekenis van een voorbeeld. Als er zijn geen kolommen worden geselecteerd wanneer deze transformatie wordt aangeroepen, worden alle cellen voor de huidige rij gebruikt. Als u alleen de vereiste kolommen leidt tot een meer nauwkeurige resultaten.

U kunt de kolommen selecteren voordat de transformatie wordt aangeroepen. Wanneer de transformatie-editor is geopend, gaat selectievakjes aan het begin van elke kolom geven aan welke kolommen worden geselecteerd als invoer. U kunt toevoegen of verwijderen van kolommen uit de selectie met behulp van de selectievakjes in de kolomkoppen.

Voor een meer gedetailleerde uitleg van de **afgeleide kolom door voorbeeld** transformatie, samen met meer voorbeelden, Zie [kolom afgeleid door voorbeeld verwijzing](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Kolom splitsen door voorbeeld
Deze transformatie een bestaande kolom heeft en met behulp van de "Voorbeeld"-engine probeert het scheiden van die kolom in  *n*  andere kolommen. U kunt de splitsing van het automatisch uitvoeren op de volgende gegenereerde kolommen.

Voor een meer gedetailleerde uitleg van de **kolom splitsen door voorbeeld** transformatie, samen met meer voorbeelden, Zie [kolom splitsen op basis van het voorbeeld](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>JSON uitbreiden

Deze transformatie kunt u meerdere kolommen toevoegen door het uitbreiden van een kolom met een geldig JSON-tekst.

Voor een meer gedetailleerde uitleg van de **Vouw JSON** transformatie, samen met meer voorbeelden, Zie [naslaginformatie voor JSON Vouw](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Kolommen door voorbeeld combineren

Deze transformatie wordt een nieuwe kolom toegevoegd door een combinatie van waarden uit meerdere kolommen. 

Voor een meer gedetailleerde uitleg van de **kolommen combineren door voorbeeld** transformatie, samen met meer voorbeelden, Zie [kolommen combineren door voorbeeld verwijzing](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Dubbele kolom
Deze transformatie is een exacte kopie van een of meer geselecteerde kolommen en geeft elke een nieuwe naam die is afgeleid van de oorspronkelijke kolomnaam.

## <a name="text-clustering"></a>Tekst Clustering 
Deze transformatie is bedoeld om inconsistente waarden dat moeten overeenkomen met de en ze te groeperen.  

Met deze transformatie worden de waarden in één kolom op gelijkenis geanalyseerd en gegroepeerd in clusters. Voor elk cluster is een canonieke waarde, dit is de waarde die wordt vervangen door alle exemplaren in het cluster en alle exemplaarwaarden. Volledige clusters kunnen worden verwijderd en de canonieke waarde bewerkt. Exemplaren kunnen worden verwijderd uit een opgegeven cluster. Het filter van gelijkenis score drempelwaarde die is gebruikt voor het groep-exemplaren in een cluster kan worden gewijzigd.

Standaard vervangt deze transformatie alle waarden van de cluster-exemplaar met de canonieke waarde voor dat cluster, het maken van een nieuwe kolom die de nieuwe waarden bevatten. U kunt ook de score gelijkenis voor elk exemplaar naar een nieuwe kolom (die kan worden genoemd) toevoegen voor gebruik verderop in de gegevensstroom.

## <a name="replace-values"></a>Vervangt waarden
Gebruik deze transformatie in één tekenreeks vervangen door een andere. De brontekenreeks kan niet een gedeeltelijke tekenreeks of een volledige cel en de vervanging van toepassing op één kolom of veel. De zoekreeks ondersteunt zoekt en speciale tekens als voor gewone tekens. 

## <a name="replace-na-values"></a>Vervang de waarden NA
Gebruik deze transform ter vervanging van de verschillende vormen van NA (N.V.T., NA, null, NaN, enz.), of lege tekenreeksen vervangen door een enkele waarde zodat ze overeenkomen. Deze transformatie ondersteunt één of meerdere kolommen en het wordt alleen weergegeven wanneer een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-missing-values"></a>Ontbrekende waarden vervangen
Deze transformatie worden ontbrekende gegevens vervangen door een enkele waarde en ondersteunt één of meerdere kolommen. Deze transformatie wordt alleen weergegeven wanneer een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-error-values"></a>Foutwaarden vervangen
Deze transformatie fouten vervangt door een enkele waarde en ondersteunt één of meerdere kolommen. Deze transformatie wordt alleen weergegeven wanneer een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="trim-string"></a>Trim tekenreeks
Deze transformatie verwijdert voorloopspaties en afsluitende 'witruimte'-tekens (inclusief spaties, tabs, enz.) uit een of meer kolommen.

## <a name="adjust-precision"></a>Precisie aanpassen
Deze transformatie Hiermee stelt u het aantal decimalen voor een numerieke kolom.

## <a name="rename-column"></a>Wijzig de naam van kolom
Deze transformatie wijzigt de naam van de geselecteerde kolom. U kunt deze ook inline in de kolomkop aanroepen door te klikken op de naam van de kolom.

## <a name="remove-column"></a>Kolom verwijderen
Deze transformatie verwijdert de geselecteerde kolommen en werkt op één kolom of veel. 

## <a name="keep-column"></a>Houd kolom
Deze transformatie blijft alleen de geselecteerde kolommen en werkt op één kolom of veel.

## <a name="handle-path-column"></a>De kolom pad naar ingang
Een kolom van het pad wordt tijdens het importeren van een bestand automatisch toegevoegd aan de gegevensset die door de **gegevensbron toevoegen** functie. Het bevat de volledig gekwalificeerde bestandsnaam die dient als het pad naar de gegevensset. Deze transformatie toegevoegd of verwijderd die extra kolom in de gegevensset.

## <a name="convert-field-type-to-numeric"></a>Veldtype converteren naar numerieke
Deze transformatie verandert het kolomtype in numerieke. U kunt het scheidingsteken voor niet-geheel getal opgeven. Standaard deze transformatie niet wordt gevraagd voor het scheidingselement, waardoor de **bewerken** menu-item aan te roepen de editor. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-date"></a>Veldtype converteren naar Date
Deze transformatie wijzigt het kolomtype naar datum. Een standaard datum/tijd-indeling wordt gebruikt, maar deze kan worden genegeerd met behulp van `strftime` richtlijnen. U kunt ook tijdwaarden met een vaste datum toevoegen.

Standaard deze transformatie voor de indeling niet gevraagd, gebruikt u dus de **bewerken** menu-item in de resulterende stap bij het aanroepen van de editor. Deze transformatie werkt op één kolom of veel.

Alleen datums tussen 9-22-1677 en 4-11-. 2262 kunnen worden geconverteerd naar het datumtype.

## <a name="convert-field-type-to-boolean"></a>Veldtype converteren naar Booleaanse waarde
Deze transformatie verandert het kolomtype in waar/onwaar. Deze ondersteuning biedt voor meerdere waarden toewijzen aan waar of ONWAAR en u kunt deze toewijzingen bewerken. Het ondersteunt ook een constante waaraan u waarden die niet bestaan in de toewijzingstabellen waar/onwaar kunt toewijzen. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-string"></a>Veldtype niet converteren naar tekenreeks
Deze transformatie wijzigt het kolomtype in tekenreeks en werkt op één kolom of veel.

## <a name="convert-unix-timestamp-to-datetime"></a>Unix tijdstempel converteren naar DateTime
Deze transformatie begrijpt de tijdstempelnotatie Unix, zelfs als deze wordt weergegeven als een tekenreeks in de gegevens. Het converteert tijdstempel correct naar een datum in het voorbereiden van gegevens.

## <a name="filter"></a>Filteren
Deze transformatie biedt ondersteuning voor filteren op basis van de waarden in een of meer kolommen rijen. De voorwaarden van het filter hangen af van het gegevenstype van elke kolom, zodat u kunt filteren tekenreekskolommen 'bevat' of "bevat geen." Numerieke kolommen kunnen worden gefilterd door 'die groter is dan' of 'kleiner dan' voorwaarden.

Wanneer de **Filter** transformatie wordt aangeroepen vanuit het hoofdmenu of uit met de rechtermuisknop op de koptekst van een kolom, de optie voor de mislukte rijen in een andere gegevensstroom vertakken beschikbaar is. De belangrijkste gegevensstroom verder met de gefilterde **in** rijen en een nieuwe gegevensstroom is gemaakt met de rijen die zijn gefilterd **uit**.

## <a name="use-first-row-as-headers"></a>Eerste rij als veldnamen gebruiken
Deze transformatie bijdraagt aan de eerste rij van de gegevensset moet de kolomnamen. Als sommige kolommen geen gegevens in de eerste rij, wordt er een naam is automatisch gegenereerd. Met deze transformatie betekent dat het importeren van gegevens in rij 2 zo snel mogelijk begint. Afhankelijk van de **overslaan rijen** instelt, kunt het importeren nog verder omlaag in de gegevensset starten. U kunt deze ook gebruiken om te verwijderen van de promotie en namen alleen automatisch gegenereerde gebruiken.

## <a name="join"></a>Koppelen
Deze transformatie wordt gebruikt voor het samenvoegen van twee gegevensstromen. U kunt selecteren welke uitvoer van de join moet het resultaat: geslaagd rijen uit de join, het 'links' mislukt rijen uit de join of 'rechts' mislukt rijen uit de join.

De **Join** transformatie wordt gestart vanaf een enkele gegevensstroom en die gegevensstroom selecteert als een-zijde van de join. Vervolgens wordt u gevraagd om te selecteren van een andere gegevensstroom voor de andere kant van de join. Nadat u de twee stromen hebt geselecteerd, geeft de transformatie is vereist voor elke zijde van de join worden geselecteerd om toe te voegen op één kolom. Als de join meer dan een kolom moet, maakt u een afgeleide kolom voordat u begint de transformatie voor het maken van een nieuwe, samengevoegde kolom moet alleen worden gebruikt voor de join. De transformatie probeert te raden join-sleutels en, indien mogelijk automatisch genereren van de afgeleide kolom.

Nadat de join is voltooid, wordt een diagramweergave Sankey het koppelen van de weergegeven. De breedte van de lijnen ten opzichte van elkaar vertegenwoordigt het aantal rijen dat deel van de join-stroom doorlopen. Met behulp van het paneel aan de rechterkant, kunt u de geslaagde rijen, links mislukt rijen of het recht rijen uitsluitend mislukken. U kunt ook een filiaal.

## <a name="append-rows"></a>Rijen toevoegen
Deze transformatie worden gegevens uit een andere gegevensstroom naar de huidige toegevoegd. Dit wijst de kolommen die door de positie van de nieuwe rijen toevoegen aan het einde.

## <a name="append-columns"></a>Kolommen toevoegen
Deze transformatie voegt nieuwe kolommen uit een andere gegevensstroom naar de huidige. De nieuwe kolommen wordt toegevoegd aan de rechterkant. Er wordt niet geprobeerd om gegevens in rijen uit te lijnen.

## <a name="summarize"></a>Samenvatten
Deze transformatie berekent aggregaties voor de combinatie van unieke waarden in een of meer geselecteerde kolommen. 

De statistische functies ondersteund zijn: aantal, SUM, MIN, MAX, gemiddelde, VARIANTIE en standaarddeviatie. De lijst met samenvoegingen voor een opgegeven kolom is gefilterd op de statistische functies die betrekking hebben op het gegevenstype. Statistische functies die niet van toepassing zijn uitgeschakeld. Bijvoorbeeld, het is niet mogelijk is het gemiddelde van een kolom met tekenreeksen berekenen, maar het is mogelijk om te berekenen van de MIN en MAX.

Wanneer de editor beschikbaar is, sleept u van de kolomkop omhoog in het deelvenster linksboven, waarbij de kolommen moeten worden samengevoegd worden weergegeven. Dit deelvenster is hiërarchisch, zodat u geneste statistische functies kunt doen. Het deelvenster editor in de rechterbovenhoek wordt gebruikt om te selecteren welke combinatie van toepassing op een kolom. Één kolom kan worden geaggregeerd als een of meer keer. Nadat ten minste één statistische functie is gekozen, wordt het raster aan de onderkant rechts een voorbeeld van de gegevens in de geaggregeerde vorm. 

Deze transformatie is vergelijkbaar met een `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Dubbele waarden verwijderen
Deze transformatie wordt de hele rij verwijderd wanneer er dubbele waarden in een of meer geselecteerde kolommen zijn. Als er zijn geen kolommen worden gekozen, zijn alleen de rijen verwijderd zijn waarin de kolomwaarden hetzelfde zijn.

## <a name="sort"></a>Sorteren
Deze transformatie sorteert deze gegevens. De sortering kan worden gedaan door één kolom of veel en elke kolom kan worden gesorteerd (de standaardinstelling) oplopende of aflopende (die kan worden gewijzigd in de editor).

Deze transformatie is vergelijkbaar met een `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Uitvoer transformaties
De volgende transformaties uitvoergegevens. U kunt meerdere schrijven blokken in één stroom schrijven van de gegevens op verschillende tijdstippen hebben.

### <a name="write-to-csv"></a>Schrijven naar CSV
Deze transformatie schrijft de gegevens in CSV-formulier van het huidige punt in de gegevensstroom. Hiermee kunt u verschillende instellingen om het bestand en de locatie (lokaal of extern).

### <a name="write-to-parquet"></a>Schrijven naar parketvloeren
Deze transformatie schrijft de gegevens in parketvloeren formulier vanaf het huidige punt in de gegevensstroom. Hiermee kunt u verschillende instellingen om het bestand en de locatie (lokaal of extern).

## <a name="script-based-transforms"></a>Transformaties op basis van scripts
De volgende transformaties script (Python) gebruiken om uit te voeren van de functie die ontbreekt in het basisproduct. 

>[!NOTE]
>Lees voordat u een van deze transformaties gebruiken, [uitbreidbaarheid met behulp van Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Kolom (script) toevoegen
Deze transformatie voegt een kolom met de gegevens via een Python-expressie.
Zie voor meer informatie [voorbeeld Python-code voor het afleiden van nieuwe kolommen](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Geavanceerde Filter (script)
Gebruik deze transformatie schrijven van een niveau rijfilter van Python.
Zie voor meer informatie [voorbeeld filterexpressies](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transformeren gegevensstroom (script)
Python geldt deze transformatie voor de volledige gegevensset.
Zie voor meer informatie [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

Deze transformatie kunt Python ook toepassen op een gegevenspartitie volledige.
Zie voor meer informatie [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Gegevensstroom (script) te schrijven
Deze transformatie maakt gebruik van Python schrijven van een volledige gegevensset.
Zie voor meer informatie [Python voorbeeld voor het afleiden van nieuwe kolommen](data-prep-appendix9-sample-destination-connections-python.md).



