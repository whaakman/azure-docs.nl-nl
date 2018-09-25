---
title: Met behulp van gegevens transformaties voor gegevensvoorbereiding in Azure Machine Learning | Microsoft Docs
description: Dit artikel bevat een volledige lijst van transformaties voor gegevensvoorbereiding van Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 26674648217e01b66fbe722cd013d51d867e9ba9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984372"
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Gebruik gegevens transformaties voor gegevensvoorbereiding in Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Een *transformeren* verbruikt in Azure Machine Learning-gegevens in een opgegeven indeling, voert een bewerking op de gegevens (zoals het wijzigen van het gegevenstype) en produceert vervolgens gegevens in de nieuwe indeling. Elke transformatie heeft zijn eigen interface en gedrag. Door verschillende transformaties samen door stappen te volgen in de gegevensstroom koppelen, kunt u complexe en herhaalbare transformaties uitvoeren op uw gegevens. Dit is de kern van de functionaliteit voor het voorbereiden van gegevens.

Hieronder vindt u de transformaties die beschikbaar zijn in Azure Machine Learning. 

## <a name="column-selection"></a>Kolom selecteren 
Veel van de transformaties in deze lijst worden gebruikt voor één kolom of veel. U selecteert meerdere kolommen met de Ctrl-toets. Als u wilt een bereik van de kolommen selecteert, gebruikt u de Shift-toets.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Transformaties uit in het hoofdmenu of de header raster 
Toegang tot transformaties uit de transformaties-optie in het hoofdmenu. U kunt ook transformaties selecteren met de rechtermuisknop op de naam van de kolom in het gegevensraster. Als er meerdere kolommen zijn geselecteerd, biedt met de rechtermuisknop op een van deze een menu transformaties.

Het snelmenu alleen weergegeven geldige transformaties voor het geselecteerde type. In het hoofdmenu biedt alle transformaties maar schakelt die niet relevant zijn voor de geselecteerde kolommen.

Een kleine subset van contextuele transformaties is beschikbaar met de rechtermuisknop op een cel. Deze transformaties zijn vervangen, kopiëren en filteren. Dit zijn gegevens type-aware, zodat de opties voor een kolom met anders dan voor een kolom met tekenreeksen zijn.

## <a name="derive-column-by-example"></a>Kolom afleiden per voorbeeld
Gebruik deze transformatie naar een nieuwe kolom maken als een afleiding van een of meer bestaande kolommen. De transformatie kijkt naar de invoer (geselecteerde) kolommen en het voorbeeld, en vervolgens bepaalt de gewenste uitvoer in de nieuwe kolom. 

Selecteer een of meer kolommen voor het gebruik van deze transformatie. Een nieuwe (lege) afgeleide kolom per voorbeeld toevoegen. Typ een voorbeeld van wat u zien in de afgeleide kolom wilt (ervan uitgaande dat deze afgeleid van andere kolommen) en de "door voorbeeld" technologie probeert in te vullen in alle andere cellen in de kolom. 

Voor voorbeelden van complexe moet u mogelijk meer dan één voorbeeld geven. Om dit te doen, selecteert u een andere cel en typt u een ander voorbeeld.

De "Voorbeeld"-technologie gebruikt geselecteerde kolommen om te bepalen van de betekenis van een voorbeeld. Als er zijn geen kolommen geselecteerd wanneer deze transformatie wordt aangeroepen, worden alle cellen voor de huidige rij gebruikt. Alleen de vereiste kolommen selecteren leidt tot een meer nauwkeurige resultaten.

U kunt de kolommen selecteren voordat Functieselectie wordt aangeroepen de transformatie. Wanneer de transformatie-editor is geopend, selectievakjes in aan de bovenkant van elke kolom geven aan welke kolommen zijn geselecteerd als invoer. U kunt kolommen toevoegen of verwijderen uit de selectie met behulp van de selectievakjes in de kolomkoppen.

Voor een meer gedetailleerde uitleg van de **afgeleide kolom per voorbeeld** transformatie, samen met meer voorbeelden Zie [kolom afleiden per voorbeeld verwijzing](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Kolom splitsen per voorbeeld
Deze transformatie heeft een bestaande kolom, en met behulp van de engine "Voorbeeld", probeert u het splitsen van die kolom in *n* andere kolommen. U kunt de splitsing van het automatisch uitvoeren op de volgende gegenereerde kolommen.

Voor een meer gedetailleerde uitleg van de **kolom splitsen per voorbeeld** transformatie, samen met meer voorbeelden Zie [kolom splitsen per voorbeeld verwijzing](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>JSON uitbreiden

Deze transformatie kunt u meerdere kolommen toevoegen door het uitbreiden van een kolom met een geldige JSON-tekst.

Voor een meer gedetailleerde uitleg van de **Vouw JSON** transformatie, samen met meer voorbeelden Zie [naslaginformatie voor JSON Vouw](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Combineer kolommen per voorbeeld

Deze transformatie wordt een nieuwe kolom toegevoegd door een combinatie van waarden uit meerdere kolommen. 

Voor een meer gedetailleerde uitleg van de **kolommen combineren per voorbeeld** transformatie, samen met meer voorbeelden Zie [kolommen combineren op basis van het voorbeeld](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Dubbele kolom
Deze transformatie is een exacte kopie van een of meer geselecteerde kolommen en biedt elke een nieuwe naam die is afgeleid van de naam van de oorspronkelijke kolom.

## <a name="text-clustering"></a>Tekst Clustering 
Deze transformatie is bedoeld om inconsistente waarden die moeten overeenkomen met het en ze te groeperen.  

Met deze transformatie, worden de waarden in één kolom geanalyseerd voor overeenkomsten en gegroepeerd in clusters. Voor elk cluster is een canonieke waarde, is de waarde die wordt vervangen door alle exemplaren in het cluster en alle exemplaarwaarden. Volledige clusters kunnen worden verwijderd en de canonieke waarde bewerkt. Exemplaren kunnen worden verwijderd uit een bepaald cluster. Het filter van gelijkenis score drempelwaarde die is gebruikt voor exemplaren van de groep in een cluster kan worden gewijzigd.

Standaard vervangt deze transformatie alle waarden van de cluster-exemplaar met de canonieke waarde voor dat cluster, het maken van een nieuwe kolom voor de nieuwe waarden bevatten. U kunt ook de score overeenkomsten voor elk exemplaar toevoegen aan een nieuwe kolom (die kan worden met de naam) voor gebruik verderop in de gegevensstroom.

## <a name="replace-values"></a>Waarden vervangen
Gebruik deze transformatie naar een tekenreeks vervangen door een andere. De brontekenreeks mag een gedeeltelijke tekenreeks of een volledige cel en de vervanging kunt toepassen op één kolom of veel. De zoekreeks ondersteunt ook speciale tekens als voor gewone tekens zoeken. 

## <a name="replace-na-values"></a>Vervang de waarden N.V.T.
Deze transformatie gebruiken om te vervangen van de verschillende soorten NA (N.V.T., NA, null, NaN, enzovoort), of lege tekenreeksen vervangen door een enkele waarde zodat deze consistent zijn. Deze transformatie ondersteunt een of meer kolommen, en deze wordt alleen weergegeven als een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-missing-values"></a>Ontbrekende waarden vervangen
Deze transformatie worden ontbrekende gegevens vervangen door een enkele waarde en ondersteunt een of meer kolommen. Deze transformatie wordt alleen weergegeven als een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-error-values"></a>Vervangen door foutwaarden
Deze transformatie fouten met één waarde vervangt en ondersteunt een of meer kolommen. Deze transformatie wordt alleen weergegeven als een kolom is geselecteerd. Het is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="trim-string"></a>Trim tekenreeks
Deze transformatie verwijdert voorloopspaties en afsluitende "witruimte"-tekens (inclusief spaties, tabs, enzovoort) uit een of meer kolommen.

## <a name="adjust-precision"></a>Precisie aanpassen
Deze transformatie Hiermee stelt u het aantal decimalen aan voor een numerieke kolom.

## <a name="rename-column"></a>Wijzig de naam van kolom
Deze transformatie verandert de naam van de geselecteerde kolom. U kunt deze ook inline in de kolomkop aanroepen door te klikken op de naam van de kolom.

## <a name="remove-column"></a>Kolom verwijderen
Deze transformatie Hiermee verwijdert u de geselecteerde kolommen en het werkt op één kolom of veel. 

## <a name="keep-column"></a>Kolom behouden
Deze transformatie blijft alleen de geselecteerde kolommen en het op één kolom of veel werkt.

## <a name="handle-path-column"></a>Path Column verwerken
Een kolom pad wordt tijdens het importeren van een bestand, automatisch toegevoegd aan de gegevensset door de **gegevensbron toevoegen** functie. Het bevat de volledig gekwalificeerde bestandsnaam die u het pad naar de gegevensset vormt. Deze transformatie wordt toegevoegd of verwijderd die extra kolom in de gegevensset.

## <a name="convert-field-type-to-numeric"></a>Veldtype converteren naar een numerieke waarde
Deze transformatie verandert het kolomtype naar een numerieke waarde. U kunt het scheidingsteken voor gegevens in niet-gehele getallen opgeven. Standaard deze transformatie niet gevraagd om het scheidingsteken, Gebruik daarom de **bewerken** menu-item voor het aanroepen van de editor. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-date"></a>Veldtype converteren naar Date
Deze transformatie verandert het kolomtype in datum. Een standaard datum/tijd-indeling wordt gebruikt, maar deze kan worden genegeerd met behulp van `strftime` richtlijnen. U kunt ook vooraf tijdwaarden met een vaste datum.

Standaard deze transformatie niet gevraagd om de indeling, Gebruik daarom de **bewerken** menu-item in de resulterende stap bij het aanroepen van de editor. Deze transformatie werkt op één kolom of veel.

Alleen tussen 9-22-1677 datums en 4-11-. 2262 kunnen worden geconverteerd naar het datumtype.

## <a name="convert-field-type-to-boolean"></a>Veldtype converteren naar een Booleaanse waarde
Deze transformatie verandert de kolom van het type waar/onwaar. Het ondersteunt meerdere waarden toe te wijzen aan true of false, en kunt u deze toewijzingen. Het ondersteunt ook een constante waarmee u waarden die niet in de toewijzingstabellen waar/onwaar-bestaat kunt toewijzen. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-string"></a>Veldtype converteren naar een tekenreeks
Deze transformatie verandert het kolomtype in tekenreeks en het werkt op één kolom of veel.

## <a name="convert-unix-timestamp-to-datetime"></a>Unix-Timestamp converteren naar DateTime
Deze transformatie begrijpt de tijdstempelnotatie Unix, zelfs als deze wordt vertegenwoordigd als een tekenreeks in de gegevens. Het converteert tijdstempel van de correct naar een gegevenstype in de gegevens voor te bereiden.

## <a name="filter"></a>Filteren
Deze transformatie biedt ondersteuning voor het filteren van rijen op basis van de waarden in een of meer kolommen. De voorwaarden van het filter afhankelijk zijn van het gegevenstype van elke kolom, zodat u tekenreekskolommen kunt filteren op 'contains' of "bevat." Numerieke kolommen kunnen worden gefilterd op 'groter is dan' of 'kleiner dan' voorwaarden.

Wanneer de **Filter** transformatie wordt aangeroepen in het hoofdmenu of uit met de rechtermuisknop op de kop van een kolom, de optie een fork van de mislukte rijen in een andere gegevensstroom beschikbaar is. De belangrijkste gegevensstroom wordt voortgezet met gefilterd **in** rijen en een nieuwe gegevensstroom gemaakt met de rijen die zijn gefilterd **uit**.

## <a name="use-first-row-as-headers"></a>De eerste rij als veldnamen gebruiken
Deze transformatie de energiestatus van de eerste rij van de gegevensset moet de kolomnamen. Als sommige kolommen geen gegevens in de eerste rij, wordt de naam van een automatisch gegenereerd. Met deze transformatie betekent dat het importeren van gegevens binnen tien dagen wordt gestart op rij 2. Afhankelijk van de **overslaan rijen** instelt, kunt het importeren nog verder omlaag in de gegevensset starten. U kunt deze ook gebruiken om te verwijderen van de promotie en automatisch gegenereerde namen alleen te gebruiken.

## <a name="join"></a>Koppelen
Deze transformatie wordt gebruikt om twee gegevensstromen samen lid. U kunt selecteren welke uitvoer van de join moet het resultaat: geslaagd rijen uit de join, het "links" werkt het uitvoeren van rijen uit de join of het 'recht' rijen uit de join mislukken.

De **Join** transformatie wordt gestart vanuit een stroom één en selecteert u deze gegevensstroom als een-zijde van de join. Vervolgens wordt u gevraagd om te selecteren van een andere gegevensstroom voor de andere kant van de join. Nadat u de twee stromen hebt geselecteerd, geeft de transformatie is vereist voor elke zijde van de join worden geselecteerd om toe te voegen in één kolom. Als de join meer dan één kolom moet, maakt u een afgeleide kolom voordat u begint met de transformatie voor het maken van een nieuwe, samengevoegde kolom moet worden gebruikt alleen voor de join. De transformatie wil lid worden van sleutels voorstellen en, indien mogelijk automatisch genereren van de afgeleide kolom.

Nadat de join is voltooid, wordt een Sankey-diagramweergave van de join wordt weergegeven. De breedte van de regels ten opzichte van elkaar geeft het aantal rijen dat deel van de join-stroom doorlopen. U kunt de geslaagde rijen, werkt het uitvoeren van rijen, of het recht rijen uitsluitend mislukt links in het deelvenster aan de rechterkant, selecteren. U kunt ook een filiaal.

## <a name="append-rows"></a>Rijen toevoegen
Deze transformatie voegt gegevens van een andere gegevensstroom naar het huidige abonnement. Hiermee wijst u de kolommen op positie naar de nieuwe rijen toevoegen aan het einde.

## <a name="append-columns"></a>Kolommen toevoegen
Deze transformatie voegt nieuwe kolommen uit een andere gegevensstroom naar de huidige. De nieuwe kolommen worden toegevoegd aan de rechterkant. Probeert niet om gegevens in rijen.

## <a name="summarize"></a>samenvatten
Deze transformatie berekent statistische functies voor de combinatie van unieke waarden in een of meer geselecteerde kolommen. 

De statistische functies die ondersteund zijn: aantal, som, MIN, MAX, gemiddelde, afwijking en de standaarddeviatie. De lijst met statistische functies voor een opgegeven kolom is gefilterd op alleen de statistische functies die betrekking hebben op het gegevenstype. Statistische functies die niet van toepassing zijn uitgeschakeld. Bijvoorbeeld, het is niet mogelijk voor het berekenen van het gemiddelde van een kolom met tekenreeksen, maar het is mogelijk voor het berekenen van de MIN en MAX.

Wanneer de editor beschikbaar is, sleept u uit de kolomkop van in het deelvenster bovenaan links, waarbij de kolommen die moeten worden samengevoegd worden weergegeven. Dit deelvenster is hiërarchisch, zodat u geneste statistische functies doen kunt. Het deelvenster editor in de rechterbovenhoek wordt te selecteren welke statistische functie toe te passen op een kolom gebruikt. Één kolom kan worden samengevoegd als een of meer keer. Nadat ten minste één statistische functie is gekozen, wordt een voorbeeld van het raster aan de onderkant rechts de gegevens in geaggregeerde vorm. 

Deze transformatie is vergelijkbaar met een `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Dubbele waarden verwijderen
Deze transformatie wordt de hele rij verwijderd wanneer er dubbele waarden in een of meer geselecteerde kolommen zijn. Als u geen kolommen worden gekozen, zijn alleen de rijen verwijderd services die waarin alle waarden in de kolom hetzelfde zijn.

## <a name="sort"></a>Sorteren
Deze transformatie worden de gegevens gesorteerd. De sortering kan worden gedaan met één kolom of veel en elke kolom kan worden gesorteerd (de standaardinstelling) oplopende of aflopende volgorde (die kunnen worden gewijzigd in de editor).

Deze transformatie is vergelijkbaar met een `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Uitvoer transformaties
De volgende transformaties productuitvoergegevens. U kunt meerdere schrijven blokken in één stroom om te schrijven om de gegevens op verschillende momenten hebben.

### <a name="write-to-csv"></a>Schrijven naar CSV
Deze transformatie schrijft de gegevens in CSV-indeling van het huidige punt in de gegevensstroom. Hiermee kunt u verschillende instellingen om het bestand en de locatie (lokaal of extern).

### <a name="write-to-parquet"></a>Schrijven naar Parquet
Deze transformatie schrijft de gegevens in Parquet-indeling van het huidige punt in de gegevensstroom. Hiermee kunt u verschillende instellingen om het bestand en de locatie (lokaal of extern).

## <a name="script-based-transforms"></a>Op basis van een script transformaties
De volgende transformaties script (Python) gebruiken om uit te voeren van de functionaliteit die ontbreekt in het basisproduct. 

>[!NOTE]
>Lees voordat u een van deze transformaties gebruiken, [uitbreidbaarheid met behulp van Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Kolom (script) toevoegen
Deze transformatie voegt een kolom toe aan de gegevens met behulp van een Python-expressie.
Zie voor meer informatie, [voorbeeld van Python-code voor nieuwe kolommen die is afgeleid](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Geavanceerd Filter (script)
Deze transformatie gebruiken om te schrijven van een filter voor het niveau van Python-rij.
Zie voor meer informatie, [voorbeeld filterexpressies](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transformeren (script)-gegevensstroom
Python geldt deze transformatie voor de volledige gegevensset.
Zie voor meer informatie, [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

Deze transformatie kunt Python ook toepassen op een hele partitie.
Zie voor meer informatie, [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Write Dataflow (script)
Deze transformatie Python gebruikt om te schrijven om een volledige gegevensset.
Zie voor meer informatie, [voorbeeld van Python voor nieuwe kolommen die is afgeleid](data-prep-appendix9-sample-destination-connections-python.md).



