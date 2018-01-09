---
title: Schrijven van aangepaste R-Modules in Azure Machine Learning | Microsoft Docs
description: Snel starten voor het schrijven van aangepaste R-modules in Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/29/2017
ms.author: bradsev;ankarlof;garye
ms.openlocfilehash: 16442a30f130e7cc9b60d2d9ae9c86d7282471ff
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Aangepaste R-modules maken in Azure Machine Learning
In dit onderwerp wordt beschreven hoe ontwerpen en implementeren van een aangepaste R-module in Azure Machine Learning. Hierin wordt uitgelegd wat aangepaste R-modules zijn en welke bestanden worden gebruikt om deze te bepalen. Het maken van de bestanden die een module te definiëren en het registreren van de module voor implementatie in een Machine Learning-werkruimte worden geïllustreerd. De elementen en kenmerken die worden gebruikt in de definitie van de aangepaste module worden vervolgens in meer detail beschreven. Het gebruik van aanvullende functionaliteit, bestanden en meerdere uitgangen wordt ook beschreven. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a>Wat is een aangepaste R-module?
Een **aangepaste module** is een door de gebruiker gedefinieerde module die kan worden geüpload naar uw werkruimte en kan worden uitgevoerd als onderdeel van een Azure Machine Learning-experiment. Een **aangepaste R-module** is een aangepaste module die wordt uitgevoerd een R door de gebruiker gedefinieerde functie. **R** is een programmeertaal voor statistische computing en afbeeldingen die veel door statistici en gegevens verzameld gebruikt wordt voor het implementeren van de algoritmen. R is momenteel de enige ondersteunde in aangepaste modules, maar ondersteuning voor extra talen is gepland voor toekomstige releases taal.

Aangepaste modules hebben **eersteklas status** in Azure Machine Learning in de zin dat ze net als elke andere module kunnen worden gebruikt. Ze kunnen worden uitgevoerd met andere modules, opgenomen in gepubliceerde experimenten of visualisaties. Hebt u controle over de algoritme die wordt geïmplementeerd door de module, de invoer en uitvoerpoorten die worden gebruikt, de parameters modelleren en andere verschillende runtime-bewerkingen. Een experiment met aangepaste modules kan ook worden gepubliceerd in de galerie van Azure AI voor eenvoudig delen.

## <a name="files-in-a-custom-r-module"></a>Bestanden in een aangepaste R-module
Een aangepaste R-module is gedefinieerd door een ZIP-bestand met ten minste twee bestanden:

* Een **bronbestand** die de R-functie die worden weergegeven door de module wordt geïmplementeerd
* Een **definitie XML-bestand** die beschrijft de aangepaste module-interface

Aanvullende hulpbestanden kunnen ook worden opgenomen in het ZIP-bestand dat wordt functionaliteit geboden die toegankelijk is vanaf de aangepaste module. Deze optie wordt beschreven in de **argumenten** deel uit van de sectie verwijzingen **elementen in de definitie van het XML-bestand** na het Quick Start-voorbeeld.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Quick Start-voorbeeld: definiëren, verpakken en registreren van een aangepaste R-module
In dit voorbeeld ziet u hoe de bestanden die vereist zijn door een aangepaste R-module maken, verpakken in een zip-bestand en registreert u de module in Machine Learning-werkruimte. De voorbeeld zip-pakket en voorbeeld-bestanden kunnen worden gedownload vanuit [CustomAddRows.zip downloaden bestand](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="the-source-file"></a>Het bronbestand
Bekijk het voorbeeld van een **rijen toevoegen van aangepaste** module die u de standaardimplementatie van wijzigt de **rijen toevoegen** module gebruikt voor het samenvoegen van rijen (opmerkingen) van twee gegevenssets (gegevensframes). De standaard **rijen toevoegen** module de rijen van de tweede invoergegevensset toegevoegd aan het einde van de eerste invoergegevensset met behulp van de `rbind` algoritme. De aangepaste `CustomAddRows` functie op dezelfde manier accepteert twee gegevenssets, maar ook een parameter Booleaanse wisselen als extra invoer. Als de parameter wisselen is ingesteld op **FALSE**, wordt de gegevensset als de standaardimplementatie. Maar als de parameter wisselen **TRUE**, de functie rijen van de eerste invoergegevensset toegevoegd aan het einde van de tweede gegevensset in plaats daarvan. De CustomAddRows.R-bestand met de implementatie van het R `CustomAddRows` functie beschikbaar is gemaakt door de **rijen toevoegen van aangepaste** module heeft de volgende R-code.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="the-xml-definition-file"></a>De definitie van het XML-bestand
Dit blootstellen `CustomAddRows` functie als een Azure Machine Learning-module, een XML-definitie-bestand moet worden gemaakt om op te geven hoe de **rijen toevoegen van aangepaste** module dient uiterlijk en gedrag. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify the base language, script file and R function to use for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>The combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


Het is uiterst belangrijk te weten dat de waarde van de **id** kenmerken van de **invoer** en **Arg** elementen in het XML-bestand moeten de functie parameternamen van het R-code in het bestand CustomAddRows.R exact overeenkomen: (*dataset1*, *dataset2*, en *wisselen* in het voorbeeld). Op dezelfde manier de waarde van de **entryPoint** kenmerk van de **taal** element moet overeenkomen met de naam van de functie in het R-script PRECIES: (*CustomAddRows* in het voorbeeld). 

Daarentegen de **id** kenmerk voor de **uitvoer** element komt niet overeen met alle variabelen in het R-script. Als meer dan één uitvoer vereist is, gewoon retourneren een lijst van de functie R met resultaten geplaatst *in dezelfde volgorde* als **uitvoer** elementen zijn gedefinieerd in het XML-bestand.

### <a name="package-and-register-the-module"></a>Het pakket en de module registreren
Deze twee bestanden opslaan als *CustomAddRows.R* en *CustomAddRows.xml* en zip de twee bestanden samen in een *CustomAddRows.zip* bestand.

Als u wilt registreren in Machine Learning-werkruimte, gaat u naar de werkruimte van de Machine Learning Studio, klikt u op de **+ nieuw** knop aan de onderkant en kies **MODULE -> van ZIP-pakket** voor het uploaden van de nieuwe **rijen toevoegen van aangepaste** module.

![Zip uploaden](./media/custom-r-modules/upload-from-zip-package.png)

De **rijen toevoegen van aangepaste** module is nu gereed om te worden geopend door uw Machine Learning-experimenten.

## <a name="elements-in-the-xml-definition-file"></a>Elementen in de definitie van het XML-bestand
### <a name="module-elements"></a>Module-elementen
De **Module** element wordt gebruikt voor het definiëren van een aangepaste module in het XML-bestand. Meerdere modules kunnen worden gedefinieerd in een XML-bestand meerdere **module** elementen. Elke module in uw werkruimte moet een unieke naam hebben. Een aangepaste module hebt geregistreerd met dezelfde naam als een bestaande aangepaste module en vervangt de bestaande module met de nieuwe knop. Aangepaste modules kunnen echter worden geregistreerd met dezelfde naam als een bestaande Azure Machine Learning-module. Als u dus ze worden weergegeven in de **aangepaste** categorie van het modulepalet.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


Binnen de **Module** element, kunt u twee extra optionele elementen:

* een **eigenaar** element dat is ingesloten in de module  
* een **beschrijving** element met de tekst die wordt weergegeven in de snelle help voor de module en wanneer u de muisaanwijzer op de module in de gebruikersinterface van de Machine Learning.

Regels voor beperkingen voor tekens in de Module-elementen:

* De waarde van de **naam** kenmerk in de **Module** element mag niet groter zijn dan 64 tekens lang zijn. 
* De inhoud van de **beschrijving** element mag niet groter zijn dan 128 tekens.
* De inhoud van de **eigenaar** element mag niet groter zijn dan 32 tekens lang zijn.

De resultaten van de module kunnen deterministische of nondeterministic.* * standaard, alle modules worden beschouwd als deterministisch. Dat wil zeggen, gezien een onveranderlijke reeks invoerparameters en gegevens, de module moet worden geretourneerd de dezelfde resultaten eacRAND of een functionh terwijl die deze wordt uitgevoerd. Dit gedrag, Azure Machine Learning Studio wordt alleen opnieuw uitgevoerd modules die zijn gemarkeerd als deterministisch als een parameter of de ingevoerde gegevens is gewijzigd. De Gecachede resultaten retourneren biedt ook veel experimenten sneller wordt uitgevoerd.

Er zijn functies die niet-deterministisch, zoals RNG of een functie die de huidige datum of tijd retourneert. Als uw module heeft een niet-deterministische functie gebruikt, kunt u opgeven of de module niet-deterministisch is door de optionele instelling **isDeterministic** kenmerk **FALSE**. Hierdoor weet u zeker dat de module wordt opnieuw uitgevoerd wanneer het experiment wordt uitgevoerd, zelfs als de module-invoer en parameters zijn niet gewijzigd. 

### <a name="language-definition"></a>Definitie van de taal
De **taal** -element in het XML-definitie-bestand wordt gebruikt om op te geven van de aangepaste module-taal. R is momenteel de enige ondersteunde taal. De waarde van de **bronbestand** kenmerk moet de naam van het R-bestand met de functie aan te roepen wanneer de module wordt uitgevoerd. Dit bestand moet deel uitmaken van het zip-pakket. De waarde van de **entryPoint** kenmerk is de naam van de functie wordt aangeroepen en moet overeenkomen met een geldige functie gedefinieerd met behulp van het bronbestand.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Poorten
De invoer en uitvoer poorten voor een aangepaste module zijn opgegeven in de onderliggende elementen van de **poorten** sectie van de definitie van het XML-bestand. De volgorde van deze elementen bepaalt de lay-out ervaren (UX) door gebruikers. Het eerste onderliggende **invoer** of **uitvoer** die worden vermeld in de **poorten** element van het XML-bestand wordt de meest linkse invoerpoort in de Machine Learning-UX
Elk invoer en uitvoerpoort heeft misschien een optionele **beschrijving** onderliggend element waarmee de tekst die wordt weergegeven wanneer u de muisaanwijzer de muisaanwijzer op de poort in de gebruikersinterface van de Machine Learning.

**Regels poorten**:

* Maximum aantal **invoer en uitvoer poorten** is 8 voor elk.

### <a name="input-elements"></a>Invoer-elementen
Ingangspoorten kunnen u gegevens aan uw R-functie en de werkruimte doorgeven. De **gegevenstypen** die worden ondersteund voor ingangspoorten als volgt zijn: 

**DataTable:** dit type wordt doorgegeven aan uw R-functie als een data.frame. In feite geen typen (bijvoorbeeld CSV-bestanden of bestanden ARFF) die worden ondersteund door Machine Learning en die compatibel zijn met **DataTable** worden geconverteerd naar een data.frame automatisch. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

De **id** die zijn gekoppeld aan elk kenmerk **DataTable** invoerpoort moet een unieke waarde hebben en deze waarde moet overeenkomen met de bijbehorende parameter in uw R-functie met de naam.
Optionele **DataTable** poorten die niet worden doorgegeven als invoer in een experiment hebben de waarde **NULL** doorgegeven aan de R-functie en de optionele zip poorten worden genegeerd als de invoer niet is verbonden. De **isOptional** kenmerk is optioneel voor zowel de **DataTable** en **Zip** en afkomstig is *false* standaard.

**Postcode:** aangepaste modules kunnen een zip-bestand als invoer worden geaccepteerd. Deze invoer is uitgepakt in de werkmap R van de functie

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

Voor aangepaste R-modules heeft de id voor een Zip-poort niet overeenkomen met de parameters van de R-functie. Dit is omdat het zip-bestand automatisch wordt uitgepakt in de werkmap R.

**Invoer regels:**

* De waarde van de **id** kenmerk van de **invoer** element moet een geldige R variabelenaam in.
* De waarde van de **id** kenmerk van de **invoer** element mag niet langer zijn dan 64 tekens zijn.
* De waarde van de **naam** kenmerk van de **invoer** element mag niet langer zijn dan 64 tekens zijn.
* De inhoud van de **beschrijving** element mag niet langer zijn dan 128 tekens zijn
* De waarde van de **type** kenmerk van de **invoer** element moet *Zip* of *DataTable*.
* De waarde van de **isOptional** kenmerk van de **invoer** element is niet vereist (en *false* standaard als niet is opgegeven); maar als deze is opgegeven, moet dit *true* of *false*.

### <a name="output-elements"></a>Elementen van de uitvoer
**Standaard uitvoer poorten:** Output-poorten zijn toegewezen aan de retourwaarden van uw R-functie, die vervolgens kan worden gebruikt door de volgende modules. *DataTable* is het alleen standaarduitvoer poorttype op dit moment ondersteund. (Ondersteuning voor *overbrengen* en *transformeert* ontbreekt.) Een *DataTable* uitvoer is gedefinieerd als:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Voor de uitvoer in aangepaste R-modules, de waarde van de **id** kenmerk heeft geen overeen te komen met alles in het R-script, maar deze moet uniek zijn. Voor de uitvoer van een module enkele is de geretourneerde waarde van de functie R moet een *data.frame*. Als u wilt meer dan een object van een ondersteund gegevenstype uitvoert, de juiste uitvoerpoorten moeten worden opgegeven in de definitie van het XML-bestand en de objecten moeten worden geretourneerd als een lijst. De uitvoer-objecten zijn toegewezen aan de uitvoer van de poorten van links naar rechts en de volgorde waarin objecten worden geplaatst in de geretourneerde lijst opgetreden bij het weergeven.

Bijvoorbeeld, als u wilt wijzigen de **rijen toevoegen van aangepaste** module voor uitvoer van de oorspronkelijke twee gegevenssets *dataset1* en *dataset2*, naast de nieuwe gekoppelde gegevensset *gegevensset*, (in volgorde, van links naar rechts, als: *gegevensset*, *dataset1*, *dataset2*), vervolgens definieert u de uitvoerpoorten in het bestand CustomAddRows.xml als volgt:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


En de lijst met objecten retourneren in een lijst in de juiste volgorde in 'CustomAddRows.R':

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Visualisatie uitvoer:** kunt u ook een uitvoerpoort van het type opgeven *visualisatie*, wordt de uitvoer van de R-grafische apparaat en de console-uitvoer. Deze poort maakt geen deel uit van de uitvoer van de functie R en niet van invloed op de volgorde van de andere uitvoer poorttypen. Als u wilt een visualisatie poort toevoegen aan de aangepaste modules, een **uitvoer** element met een waarde van *visualisatie* voor de **type** kenmerk:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

**Regels voor uitvoer:**

* De waarde van de **id** kenmerk van de **uitvoer** element moet een geldige R variabelenaam in.
* De waarde van de **id** kenmerk van de **uitvoer** element mag niet langer zijn dan 32 tekens lang zijn.
* De waarde van de **naam** kenmerk van de **uitvoer** element mag niet langer zijn dan 64 tekens zijn.
* De waarde van de **type** kenmerk van de **uitvoer** element moet *visualisatie*.

### <a name="arguments"></a>Argumenten
Aanvullende gegevens kunnen worden doorgegeven aan de functie R via de moduleparameters die zijn gedefinieerd in de **argumenten** element. Deze parameters worden weergegeven in het deelvenster van de meest rechtse eigenschappen van de Machine Learning-gebruikersinterface wanneer de module is geselecteerd. Argumenten kunnen bestaan uit een van de ondersteunde typen of kunt u een aangepaste opsomming wanneer deze nodig is. Net als bij de **poorten** elementen, **argumenten** elementen kunnen hebben een optioneel **beschrijving** element dat de tekst die wordt weergegeven geeft wanneer u de muisaanwijzer op de parameternaam van de.
Optionele eigenschappen voor een module, zoals defaultValue minValue en maxValue kunnen worden toegevoegd aan een van de argumenten als kenmerken aan een **eigenschappen** element. Geldige eigenschappen voor de **eigenschappen** element afhankelijk van het argumenttype en met de ondersteunde argumenttypen in de volgende sectie worden beschreven. Argumenten in met de **isOptional** eigenschap ingesteld op **'true'** hoeven niet de gebruiker een waarde in te voeren. Als een waarde voor het argument niet is opgegeven, klikt u vervolgens het argument niet doorgegeven aan de functie voor het item. Argumenten van de functie voor het vermeldingspunt die optioneel zijn moeten expliciet worden verwerkt door de functie, bijvoorbeeld een standaardwaarde van NULL in de functiedefinitie vermelding punt toegewezen. Een optioneel argument wordt alleen de andere argument beperkingen, dat wil zeggen min of max afdwingen als een waarde is opgegeven door de gebruiker.
Net als bij de invoer en uitvoer, is het cruciaal dat elk van de parameters die zijn gekoppeld aan deze unieke id-waarden hebben. In ons voorbeeld snel starten de bijbehorende id/parameter was *wisselen*.

### <a name="arg-element"></a>Arg element
Een module-parameter is gedefinieerd met behulp van de **Arg** onderliggend element van de **argumenten** sectie van de definitie van het XML-bestand. Net als bij de onderliggende elementen in de **poorten** sectie, de volgorde van parameters in de **argumenten** sectie definieert de lay-out aangetroffen in de UX De parameters weergegeven van boven naar beneden in de gebruikersinterface in de volgorde waarin ze zijn gedefinieerd in het XML-bestand. De typen die worden ondersteund door Machine Learning voor parameters worden hier weergegeven. 

**int** – een typeparameter van geheel getal (32-bits).

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Optionele eigenschappen*: **min**, **max**, **standaard** en **isOptional**

**dubbele** – een double-type-parameter.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Optionele eigenschappen*: **min**, **max**, **standaard** en **isOptional**

**BOOL** – een Boole-parameter die wordt vertegenwoordigd door een selectievakje in UX

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Optionele eigenschappen*: **standaard** -false als dat niet is ingesteld

**tekenreeks**: een standaardtekenreeks

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *Optionele eigenschappen*: **standaard** en **isOptional**

**ColumnPicker**: een parameter van de selectie kolom. Dit type wordt gerenderd in de UX als het kiezen van een kolom. De **eigenschap** element wordt hier gebruikt om op te geven van de id van de poort van waaruit de kolommen worden geselecteerd, waarin het doeltype van de poort moet *DataTable*. Het resultaat van de kolomselectie is doorgegeven aan de functie R als een lijst met tekenreeksen die de namen van de geselecteerde kolom bevat. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Vereiste eigenschappen*: **portId** -komt overeen met de id van een invoer-element met het type *DataTable*.
* *Optionele eigenschappen*:
  
  * **allowedTypes** -Filters de kolom typen uit die u kunt kiezen. Geldige waarden zijn: 
    
    * Numeriek
    * Boole-waarde
    * Categorische gegevens
    * Tekenreeks
    * Label
    * Functie
    * Score
    * Alle
  * **standaard** -geldige standaardselecties voor de kolomkiezer omvatten: 
    
    * None
    * NumericFeature
    * NumericLabel
    * NumericScore
    * NumericAll
    * BooleanFeature
    * BooleanLabel
    * BooleanScore
    * BooleanAll
    * CategoricalFeature
    * CategoricalLabel
    * CategoricalScore
    * CategoricalAll
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Alle

**Vervolgkeuzelijst**: een gebruiker opgegeven opgesomde (vervolgkeuzelijst) wordt weergegeven. De dropdown-items zijn opgegeven in de **eigenschappen** met behulp van element een **Item** element. De **id** voor elk **Item** moeten uniek zijn en een geldige R-variabele. De waarde van de **naam** van een **Item** fungeert als zowel de tekst die wordt weergegeven als de waarde die wordt doorgegeven aan de R-functie.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *Optionele eigenschappen*:
  * **standaard** -de waarde voor de standaardeigenschap moet overeenkomen met een id-waarde van een van de **Item** elementen.

### <a name="auxiliary-files"></a>Hulpbestanden
Elk bestand dat wordt geplaatst in uw aangepaste module-ZIP-bestand is het verstandig om beschikbaar voor gebruik tijdens uitvoeringstijd. Eventuele mapstructuren aanwezig blijven behouden. Dit betekent dat bestand bron werkt hetzelfde lokaal en in Azure Machine Learning worden uitgevoerd. 

> [!NOTE]
> U ziet dat alle bestanden zijn uitgepakt naar de map 'src' zodat alle paden moeten hebben ' src /' voorvoegsel.
> 
> 

Stel dat u wilt verwijderen van rijen met NAs uit de gegevensset en ook eventuele dubbele rijen verwijderen voordat deze worden uitgevoerd in CustomAddRows en u hebt al een R functie die in een bestand RemoveDupNARows.R geschreven:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
U kunt het Aux-bestand RemoveDupNARows.R in de functie CustomAddRows bron:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Vervolgens uploaden een zip-bestand met 'CustomAddRows.R', 'CustomAddRows.xml' en 'RemoveDupNARows.R' als een aangepaste R-module.

## <a name="execution-environment"></a>Uitvoeringsomgeving
De uitvoeringsomgeving van het R-script maakt gebruik van dezelfde versie van R als de **R-Script uitvoeren** module en de dezelfde standaard pakketten kunnen gebruiken. U kunt ook extra R-pakketten toevoegen aan uw aangepaste module door ze in de aangepaste module-zip-pakket. Zojuist geladen in het R-script zoals u zou in uw eigen omgeving voor R doen. 

**Beperkingen van de uitvoeringsomgeving** omvatten:

* Niet-permanente bestandssysteem: bestanden die zijn geschreven wanneer de aangepaste module wordt uitgevoerd, blijven niet bestaan voor meerdere dezelfde module uitvoert.
* Er is geen toegang tot het netwerk

