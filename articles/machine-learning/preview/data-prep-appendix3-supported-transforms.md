---
title: Gegevens transformaties gebruiken voor het voorbereiden van gegevens in Azure Machine Learning | Microsoft Docs
description: In dit artikel biedt een volledig overzicht van transformaties beschikbaar voor Azure Machine Learning gegevens prep
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 10/09/2017
ms.openlocfilehash: 4331bb4a16d56f027dd63a97868822fa6ecc92d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Gegevens transformaties gebruiken voor het voorbereiden van gegevens in Azure Machine Learning

Een *transformeren* in Azure Machine Learning gebruikt gegevens in een bepaalde indeling, voert u een bewerking op de gegevens (zoals het wijzigen van het gegevenstype) en vervolgens produceert gegevens in de nieuwe indeling. Elke transformatie heeft een eigen interface en gedrag. U kunt verschillende transformaties samen via de stappen in de gegevensstroom koppelen zodat u kunt complexe en herhaalbare transformaties uitvoeren op uw gegevens. Dit is de kern van voorbereiding-functionaliteit.

Hier volgt een lijst van de transformaties die beschikbaar zijn in Azure Machine Learning. 

## <a name="column-selection"></a>Kolom selecteren 
Veel van de volgende vermelde transformaties werken met één kolom of veel. Gebruik om te selecteren in meerdere kolommen, de **Ctrl** sleutel; of u selecteert een bereik met kolommen met de **Shift** sleutel.

## <a name="transforms-from-main-menu-andor-grid-header"></a>Transformeert van hoofd-header menu en/of rasterlijnen 
Transformaties zijn toegankelijk via de optie transformaties in het hoofdmenu. Transformaties kunnen ook worden geselecteerd met de rechtermuisknop op de naam van de kolom in het gegevensraster. Als meerdere kolommen zijn geselecteerd, biedt Klik met de rechtermuisknop op een van deze een menu transformaties.

Geldige transformaties voor het gegevenstype dat is geselecteerd zijn alleen beschikbaar in het snelmenu. Het hoofdmenu biedt alle transformaties maar schakelt de transformaties die niet relevant zijn voor de geselecteerde kolommen.

Een kleine subset van contextuele transformaties is beschikbaar met de rechtermuisknop op een cel. Deze transformaties worden gekopieerd, vervangen en filteren. Deze transformaties zijn gegevenstype op de hoogte zodat de opties voor een numerieke kolom anders dan voor een kolom met tekenreeksen zijn.

## <a name="derive-column-by-example"></a>Afgeleid kolom door voorbeeld
Deze transformatie kan het maken van een nieuwe kolom als een afgeleide van een of meer bestaande kolommen. De transformaties de invoerkolommen (geselecteerd) en het voorbeeld wordt gekeken en wordt vervolgens bepaald de wens uitvoer in de nieuwe kolom. 

Selecteer een of meer kolommen voor het gebruik van deze transformatie. Een nieuwe (lege) afgeleide kolom toevoegen door voorbeeld. Typ een voorbeeld van wat u zien in de afgeleide kolom wilt (ervan uitgaande dat deze afgeleid van andere kolommen) en de "voorbeeld" technologie probeert te vullen in de andere cellen in de kolom. 

Voor voorbeelden van complexe het u mogelijk vindt u meer dan één voorbeeld. U doet dit door een andere cel selecteert en typ een ander voorbeeld.

De technologie "Voorbeeld" gebruikt geselecteerde kolommen om te bepalen van de betekenis van een voorbeeld. Als er zijn geen kolommen worden geselecteerd wanneer deze transformatie wordt aangeroepen, worden alle cellen voor de huidige rij gebruikt. Als u alleen de vereiste kolommen leidt tot een meer nauwkeurige resultaten.

U kunt de kolommen selecteren voordat de transformatie wordt aangeroepen. Zodra de editor voor transformatie is gestart, gaat selectievakjes aan het begin van elke kolom geven aan welke kolommen worden geselecteerd als invoer. U kunt toevoegen of verwijderen van kolommen uit de selectie met behulp van de selectievakjes in de kolomkoppen.

Voor een meer gedetailleerde uitleg van **kolom afgeleid door voorbeeld** transformatie, samen met meer voorbeelden, Zie: [kolom afgeleid door voorbeeld verwijzing](data-prep-derive-column-by-example.md)  

## <a name="split-column-by-example"></a>Kolom splitsen door voorbeeld
Deze transformatie neemt een bestaande kolom en het scheiden van die kolom in de "Voorbeeld"-engine probeert  *n*  andere kolommen. Het is mogelijk de splitsing van het automatisch uitvoeren op de volgende gegenereerde kolommen.

Voor een meer gedetailleerde uitleg van **kolom door voorbeeld Split** transformatie, samen met meer voorbeelden, Zie: [kolom splitsen op basis van het voorbeeld](data-prep-split-column-by-example.md)

## <a name="expand-json"></a>JSON uitbreiden

Deze transformatie kunt u meerdere kolommen toevoegen door het uitbreiden van een kolom met een geldig JSON-tekst.

Voor een meer gedetailleerde uitleg van **Vouw JSON** transformatie, samen met meer voorbeelden, Zie: [naslaginformatie voor JSON uitvouwen](data-prep-expand-json.md)


## <a name="combine-columns-by-example"></a>Kolommen door voorbeeld combineren

Deze transformatie kunt u een nieuwe kolom toevoegen door een combinatie van waarden uit meerdere kolommen. 

Voor een meer gedetailleerde uitleg van **kolommen combineren door voorbeeld** transformatie, samen met meer voorbeelden, Zie: [kolommen combineren door voorbeeld verwijzing](data-prep-combine-columns-by-example.md)


## <a name="duplicate-column"></a>Dubbele kolom
Deze transformatie is een exacte kopie van een of meer geselecteerde kolommen en geeft elke een nieuwe naam die is afgeleid van de oorspronkelijke kolomnaam.

## <a name="text-clustering"></a>Tekst Clustering 
Deze transformatie is bedoeld om inconsistente waarden dat moeten overeenkomen met de en ze te groeperen.  

Wanneer u deze transformatie gebruikt, worden de waarden in één kolom op gelijkenis geanalyseerd en gegroepeerd in clusters. Er is een canonieke waarde, dit is de waarde die wordt vervangen door alle exemplaren in het cluster en de exemplaarwaarden voor elk cluster. Volledige clusters kunnen worden verwijderd en de canonieke waarde kan worden bewerkt. Exemplaren kunnen worden verwijderd uit een opgegeven cluster. Bovendien kan het filter van gelijkenis score drempelwaarde die is gebruikt voor het groep-exemplaren in een cluster kan worden gewijzigd.

Standaard vervangt deze transformatie alle waarden van de cluster-exemplaar met de canonieke waarde voor dat cluster, het maken van een nieuwe kolom die de nieuwe waarden bevatten. Het is ook mogelijk de score gelijkenis voor elk exemplaar toevoegen aan een nieuwe kolom (die kan worden genoemd) om het gebruik ervan in de gegevensstroom later op toestaan.

## <a name="replace-values"></a>Vervangt waarden
Deze transformatie kan één tekenreeks worden vervangen door een andere. De brontekenreeks is een gedeeltelijke tekenreeks of een volledige cel. de vervanging van toepassing op één kolom of veel. De zoekreeks ondersteunt zoeken voor speciale tekens, evenals reguliere tekens. 

## <a name="replace-na-values"></a>Vervang de waarden NA
Deze transformatie kunt u de verschillende andere vormen van N.V.T. (N.V.T., NA, null, NaN, enz.) of lege tekenreeksen worden vervangen door een enkele waarde zodat ze overeenkomen. Deze transformatie ondersteunt één of meerdere kolommen. Deze transformatie wordt alleen weergegeven wanneer u een kolom is geselecteerd en is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-missing-values"></a>Ontbrekende waarden vervangen
Deze transformatie vervangen ontbrekende gegevens door een enkele waarde. Deze transformatie ondersteunt één of meerdere kolommen. Deze transformatie wordt alleen weergegeven wanneer u een kolom is geselecteerd en is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="replace-error-values"></a>Foutwaarden vervangen
Deze transformatie vervangen fouten door een enkele waarde. Deze transformatie ondersteunt één of meerdere kolommen. Deze transformatie wordt alleen weergegeven wanneer u een kolom is geselecteerd en is niet aanwezig in het menu van de belangrijkste transformatie wanneer er geen kolommen zijn geselecteerd.

## <a name="trim-string"></a>Trim tekenreeks
Deze transformatie-verwijdert voorloopspaties en afsluitende 'witruimte'-tekens (inclusief spaties, tabs, *enz.*), van een of meer kolommen.

## <a name="adjust-precision"></a>Precisie aanpassen
Deze transformatie kunt u het aantal decimalen voor een numerieke kolom instellen.

## <a name="rename-column"></a>Wijzig de naam van kolom
Deze transformatie wijzigt de naam van de geselecteerde kolom. Deze transformatie kan ook aangeroepen inline in de kolomkop worden door te klikken op de naam van de kolom.

## <a name="remove-column"></a>Kolom verwijderen
Deze transformatie Hiermee verwijdert u de geselecteerde kolommen. Deze transformatie werkt op één kolom of veel. 

## <a name="keep-column"></a>Houd kolom
Deze transformatie houdt alleen de geselecteerde kolommen. Deze transformatie werkt op één kolom of veel.

## <a name="handle-path-column"></a>De kolom pad naar ingang
Tijdens het importeren van een bestand met wordt een kolom van het pad automatisch toegevoegd aan de gegevensset die door de wizard Gegevensbron toevoegen. Het bevat de volledig gekwalificeerde bestandsnaam die dient als het pad naar de gegevensset. Deze transformatie toegevoegd of verwijderd die extra kolom uit de gegevensset.

## <a name="convert-field-type-to-numeric"></a>Veldtype converteren naar numerieke
Deze transformatie verandert het kolomtype in numerieke. Als er geen integer-gegevens, kunt u het scheidingsteken opgeven. Standaard deze transformatie niet wordt gevraagd voor het scheidingselement, gebruikt u de **bewerken** menu-item aan te roepen de editor. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-date"></a>Veldtype converteren naar Date
Deze transformatie wijzigt het kolomtype naar datum. Een standaard datum/tijd-indeling wordt gebruikt, maar kan worden genegeerd met `strftime` richtlijnen. Het is ook mogelijk tijdwaarden met een vaste datum toevoegen aan het begin.

Standaard deze transformatie voor de indeling niet wordt gevraagd, gebruikt de **bewerken** menu-item in de resulterende stap bij het aanroepen van de editor. Deze transformatie werkt op één kolom of veel.

Alleen datums tussen 9-22-1677 en 4-11-. 2262 kunnen worden geconverteerd naar het datumtype.

## <a name="convert-field-type-to-boolean"></a>Veldtype converteren naar Booleaanse waarde
Deze transformatie verandert het kolomtype in waar/onwaar. Deze transformatie ondersteunt meerdere waarden toewijzen aan waar of ONWAAR en is het mogelijk deze toewijzingen te bewerken. Het ondersteunt ook een constante waaraan u waarden die niet bestaan in de toewijzingstabellen waar/onwaar kunt toewijzen. Deze transformatie werkt op één kolom of veel.

## <a name="convert-field-type-to-string"></a>Veldtype niet converteren naar tekenreeks
Deze transformatie verandert het kolomtype in de tekenreeks. Deze transformatie werkt op één kolom of veel.

## <a name="convert-unix-timestamp-to-datetime"></a>Unix tijdstempel converteren naar DateTime
Deze transformatie begrijpt de tijdstempelnotatie Unix, zelfs als deze wordt weergegeven als een tekenreeks in de gegevens en correct naar een datum in gegevens prep converteert.

## <a name="filter"></a>Filteren
Deze transformatie biedt ondersteuning voor filteren op basis van de waarden in een of meer kolommen rijen. De voorwaarden van het filter zijn afhankelijk van het gegevenstype van elke kolom, waardoor het mogelijk voor tekenreekskolommen filter door 'bevat' of 'bevat geen'. Numerieke kolommen kunnen worden gefilterd op 'die groter is dan' ' kleiner dan ' voorwaarden.

Wanneer de transformatie filter wordt gestart vanuit het hoofdmenu of vanuit het met de rechtermuisknop op de koptekst van een kolom, is de optie voor de mislukte rijen in een andere gegevensstroom vertakken beschikbaar. Gaat de belangrijkste gegevensstroom verder met de gefilterde **in** rijen en een nieuwe gegevensstroom is gemaakt met de rijen die zijn gefilterd **uit**.

## <a name="use-first-row-as-headers"></a>Eerste rij als veldnamen gebruiken
Deze transformatie bijdraagt aan de eerste rij van de gegevensset moet de kolomnamen. Als sommige kolommen geen gegevens in de eerste rij, is de naam van een automatisch gegenereerd. Met deze transformatie betekent dat het importeren van gegevens in rij 2 zo snel mogelijk begint. Afhankelijk van de instelling overslaan rijen mogelijk de import nog verder omlaag gaan in de gegevensset. Het kan ook worden gebruikt om te verwijderen van de promotie en automatisch gegenereerde naam alleen te hebben.

## <a name="join"></a>Koppelen
Deze transformatie wordt gebruikt voor het samenvoegen van twee gegevensstromen. U kunt selecteren welke uitvoer van de join moet het resultaat: geslaagd rijen uit de join, het 'links' mislukt rijen uit de join of 'rechts' mislukt rijen uit de join.

De wizard lid worden van een enkele gegevensstroom is gestart en die gegevensstroom selecteert als een-zijde van de join. Vervolgens vraagt u om een andere gegevensstroom voor de andere kant van de join. Zodra de twee stromen hebt geselecteerd, wordt in de wizard voor elke zijde van de gemengde worden geselecteerd om toe te voegen op één kolom vereist. Als de join meer dan een kolom moet, maakt u een afgeleide kolom vóór het starten van de wizard voor het maken van een nieuwe (samengevoegde) kolom moet worden gebruikt voor de join. De wizard probeert join sleutels voorstellen en indien mogelijk automatisch genereren van de afgeleide kolom.

Zodra de join is voltooid, wordt een diagramweergave van Sankey het koppelen van de weergegeven. De breedte van de lijnen ten opzichte van elkaar vertegenwoordigt het aantal rijen dat deel van de join-stroom doorlopen. Het paneel aan de rechterkant kunt u de geslaagde rijen, mislukken links of rechts uitsluitend mislukt selecteren. Het is ook mogelijk slechts één vertakking kiezen.

## <a name="append-rows"></a>Rijen toevoegen
Deze transformatie worden gegevens uit een andere gegevensstroom naar de huidige toegevoegd. Dit wijst de kolommen die door de positie van de nieuwe rijen toevoegen aan het einde.

## <a name="append-columns"></a>Kolommen toevoegen
Deze transformatie voegt nieuwe kolommen uit een andere gegevensstroom naar de huidige. De nieuwe kolommen worden toegevoegd aan het recht; Er wordt niet geprobeerd om gegevens in rijen uit te lijnen.

## <a name="summarize"></a>Samenvatten
Deze transformatie berekent aggregaties voor de combinatie van unieke waarden in een of meer geselecteerde kolommen. 

De statistische functies ondersteund zijn: aantal, SUM, MIN, MAX, gemiddelde, VARIANTIE en standaarddeviatie. De lijst met samenvoegingen voor een opgegeven kolom is gefilterd op alleen de waarden die betrekking hebben op het gegevenstype. Statistische functies die niet van toepassing zijn uitgeschakeld. Bijvoorbeeld, het is niet mogelijk is het gemiddelde van een kolom met tekenreeksen berekenen, maar het is mogelijk om te berekenen van de min en max.

Zodra de editor beschikbaar is, sleept van de kolomkop om in het deelvenster linksboven op. waar de kolommen moeten worden samengevoegd worden weergegeven. Dit deelvenster is hiërarchische, dus is het mogelijk geneste statistische functies. Het deelvenster editor op de rechterbovenhoek wordt gebruikt om te selecteren welke combinatie van toepassing op een kolom. Één kolom kan worden geaggregeerd als een of meer keer. Nadat ten minste één statistische functie is gekozen, wordt het raster aan de onderkant rechts een voorbeeld van de gegevens in de geaggregeerde vorm. 

Deze transformatie is vergelijkbaar met een `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Dubbele waarden verwijderen
Deze transformatie Hiermee verwijdert u de hele rij waar er zijn dubbele waarden in een of meer geselecteerde kolommen. Als er zijn geen kolommen worden gekozen, klikt u vervolgens zijn alleen rijen die zijn verwijderd zijn waarin de kolomwaarden hetzelfde zijn.

## <a name="sort"></a>Sorteren
Deze transformatie sorteert deze gegevens. De sortering kan worden gedaan door één kolom of veel en elke kolom kan worden gesorteerd (de standaardinstelling) oplopende of aflopende (kan worden gewijzigd in de editor).

Deze transformatie is vergelijkbaar met een `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Uitvoer transformaties
De volgende transformaties uitvoergegevens. Het is mogelijk meerdere schrijven blokken in één stroom schrijven van de gegevens op verschillende momenten kunnen hebben.

### <a name="write-to-csv"></a>Schrijven naar CSV
Deze transformatie schrijft de gegevens in CSV-formulier van het huidige punt in de gegevensstroom. Kunt u verschillende instellingen voor beheer van de locatie (lokaal of extern) en om het bestand.

### <a name="write-to-parquet"></a>Schrijven naar parketvloeren
Deze transformatie schrijft de gegevens in parketvloeren formulier vanaf het huidige punt in de gegevensstroom. Kunt u verschillende instellingen voor beheer van de locatie (lokaal of extern) en om het bestand.

## <a name="script-based-transforms"></a>Transformaties op basis van scripts
De volgende transformaties script (Python) gebruiken om uit te voeren van de functie die ontbreekt in het basisproduct. Lees voordat u een van deze transformaties, [uitbreidbaarheid met behulp van Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Kolom (Script) toevoegen
Deze transformatie kan een kolom die moet worden toegevoegd aan de gegevens met behulp van een Python-expressie.
Zie voor meer informatie [voorbeeld Python-code voor het afleiden van nieuwe kolommen](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Geavanceerde Filter (Script)
Deze transformatie kan een niveau rijfilter Python worden geschreven.
Zie voor meer informatie [voorbeeld filterexpressies](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transformeren gegevensstroom (Script)
Deze transformatie kunt Python moet worden toegepast op de volledige gegevensset.
Zie voor meer informatie [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="transform-dataflow-script"></a>Transformeren gegevensstroom (Script)
Deze transformatie kunt Python moet worden toegepast op een gegevenspartitie volledige.
Zie voor meer informatie [voorbeeld transformatie gegevensstroom transformaties](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Gegevensstroom (Script) te schrijven
Deze transformatie kunt Python moet worden toegepast op het uitschrijven van en de volledige gegevensset.
Zie voor meer informatie [Python voorbeeld voor het afleiden van nieuwe kolommen](data-prep-appendix9-sample-destination-connections-python.md).



