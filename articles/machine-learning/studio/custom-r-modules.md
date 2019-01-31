---
Titel: Definieer aangepaste R-modules titleSuffix: Azure Machine Learning Studio description: In dit onderwerp wordt beschreven hoe u voor het ontwerpen en implementeren van een aangepaste R-module in Azure Machine Learning. Hierin wordt uitgelegd wat aangepaste R-modules zijn en welke bestanden worden gebruikt om deze te bepalen. Services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 11/29/2017
---
# <a name="define-custom-r-modules-for-azure-machine-learning-studio"></a>Aangepaste R-modules voor Azure Machine Learning Studio definieert

In dit onderwerp wordt beschreven hoe u voor het ontwerpen en implementeren van een aangepaste R-module in Azure Machine Learning Studio. Hierin wordt uitgelegd wat aangepaste R-modules zijn en welke bestanden worden gebruikt om deze te bepalen. Deze ziet u hoe u een van de bestanden die een module definiëren en registreren van de module voor implementatie in een Machine Learning-werkruimte. De elementen en kenmerken die worden gebruikt in de definitie van de aangepaste module worden vervolgens in meer detail beschreven. Het gebruik van aanvullende functionaliteit, bestanden en meerdere uitvoer wordt ook beschreven. 



## <a name="what-is-a-custom-r-module"></a>Wat is een aangepaste R-module?
Een **aangepaste module** is een door de gebruiker gedefinieerde module die kan worden geüpload naar uw werkruimte en kan worden uitgevoerd als onderdeel van een Azure Machine Learning-experiment. Een **aangepaste R-module** is een aangepaste module die een door de gebruiker gedefinieerde R-functie wordt uitgevoerd. **R** is een programmeertaal voor statistische computing en afbeeldingen die veel door statistici en data scientists wordt gebruikt voor het implementeren van algoritmen. R is momenteel de enige ondersteunde in aangepaste modules, maar ondersteuning voor extra talen is gepland voor toekomstige versies taal.

Aangepaste modules zijn **eersteklas status** in Azure Machine Learning in de zin dat ze net als elke andere module kunnen worden gebruikt. Ze kunnen worden uitgevoerd met andere modules, die zijn opgenomen in de gepubliceerde experimenten of in visualisaties. Hebt u controle over de algoritme die is geïmplementeerd door de module, de invoer en uitvoerpoorten die worden gebruikt, de parameters modelleren en andere verschillende gedrag voor runtime. Een experiment met aangepaste modules kan ook worden gepubliceerd in de Azure AI Gallery delen.

## <a name="files-in-a-custom-r-module"></a>Bestanden in een aangepaste R-module
Een aangepaste R-module is gedefinieerd door een ZIP-bestand dat ten minste twee bestanden bevat:

* Een **bronbestand** die de R-functie die worden weergegeven door de module implementeert
* Een **definitie XML-bestand** met de beschrijving van de aangepaste module-interface

Aanvullende hulpbestanden kunnen ook worden opgenomen in het ZIP-bestand die kan worden benaderd vanaf de aangepaste module functionaliteit biedt. Deze optie wordt beschreven in de **argumenten** deel uitmaakt van de sectie documentatie **elementen in het definitie van het XML-bestand** de quickstart-voorbeeld te volgen.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Voorbeeld van de Quick Start: definiëren, verpakken en het registreren van een aangepaste R-module
In dit voorbeeld ziet u hoe u de bestanden die vereist zijn door een aangepaste R-module, verpakken in een zip-bestand, en vervolgens de module hebt geregistreerd in uw Machine Learning-werkruimte. De voorbeeld-zip-bestanden-pakket en voorbeeld kunnen worden gedownload vanaf [CustomAddRows.zip downloaden bestand](https://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="the-source-file"></a>Het bronbestand
Bekijk het voorbeeld van een **rijen toevoegen van aangepaste** -module die Hiermee wijzigt u de standard-implementatie van de **rijen toevoegen** module gebruikt voor het samenvoegen van rijen (opmerkingen) van twee gegevenssets (gegevensframes). De standaard **rijen toevoegen** module worden de rijen van de tweede invoergegevensset toegevoegd aan het einde van de eerste invoergegevensset met behulp van de `rbind` algoritme. De aangepaste `CustomAddRows` functie op dezelfde manier accepteert twee gegevenssets, maar ook een wisselen van Booleaanse parameter als een aanvullende invoer. Als de parameter wisselen is ingesteld op **FALSE**, wordt de gegevensset als de standard-implementatie. Maar als de parameter wisselen **waar**, de functie rijen van de eerste invoergegevensset toegevoegd aan het einde van de tweede gegevensset in plaats daarvan. Het CustomAddRows.R-bestand met de implementatie van de R `CustomAddRows` functie beschikbaar is gemaakt door de **rijen toevoegen van aangepaste** -module bevat de volgende R-code.

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
Om beschikbaar te stellen deze `CustomAddRows` functie als een module voor Azure Machine Learning, definitie van een XML-bestand moet worden gemaakt om op te geven hoe de **rijen toevoegen van aangepaste** module moet hetzelfde uiterlijk en gedrag. 

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


Het is essentieel om te weten dat de waarde van de **id** kenmerken van de **invoer** en **func** elementen in het XML-bestand moeten overeenkomen met de parameter functienamen van de R-code in de CustomAddRows.R bestand exact: (*dataset1*, *dataset2*, en *wisselen* in het voorbeeld). Op dezelfde manier de waarde van de **entryPoint** kenmerk van de **taal** element moet overeenkomen met de naam van de functie in het R-script PRECIES: (*CustomAddRows* in het voorbeeld). 

Daarentegen de **id** kenmerk voor de **uitvoer** element komt niet overeen met eventuele variabelen die in de R-script. Als meer dan één uitvoer vereist is, retourneert een lijst van de R-functie met resultaten geplaatst *in dezelfde volgorde* als **uitvoer** elementen zijn gedefinieerd in het XML-bestand.

### <a name="package-and-register-the-module"></a>Het pakket en de module registreren
Deze twee bestanden opslaan als *CustomAddRows.R* en *CustomAddRows.xml* en zip de twee bestanden samen in een *CustomAddRows.zip* bestand.

Als u wilt registreren in uw Machine Learning-werkruimte, gaat u naar uw werkruimte in de Machine Learning Studio, klikt u op de **+ nieuw** knop aan de onderkant en kies **MODULE -> uit ZIP-pakket** het uploaden van de nieuwe **Rijen toevoegen van aangepaste** module.

![Zip uploaden](./media/custom-r-modules/upload-from-zip-package.png)

De **rijen toevoegen van aangepaste** -module is nu klaar om te worden geopend door uw Machine Learning-experimenten.

## <a name="elements-in-the-xml-definition-file"></a>Elementen in het definitie van het XML-bestand
### <a name="module-elements"></a>Module-elementen
De **Module** element wordt gebruikt voor het definiëren van een aangepaste module in het XML-bestand. Meerdere modules kunnen worden gedefinieerd in een XML-bestand meerdere **module** elementen. Elke module in uw werkruimte moet een unieke naam hebben. Een aangepaste module hebt geregistreerd met dezelfde naam als een bestaande aangepaste module en de bestaande module vervangt door de nieuwe. Aangepaste modules kunnen echter worden geregistreerd met dezelfde naam als een bestaande Azure Machine Learning-module. Als u dus ze worden weergegeven in de **aangepaste** categorie van het modulepalet.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


Binnen de **Module** -element, kunt u twee aanvullende optionele elementen:

* een **eigenaar** element dat is ingesloten in de module  
* een **beschrijving** element met de tekst die wordt weergegeven in de snelle help voor de module en wanneer u de muisaanwijzer over de module in de gebruikersinterface van de Machine Learning.

Regels voor de limieten van de tekens in de Module-elementen:

* De waarde van de **naam** kenmerk in de **Module** element mag niet groter zijn dan 64 tekens lang. 
* De inhoud van de **beschrijving** element mag niet groter zijn dan 128 tekens.
* De inhoud van de **eigenaar** element mag niet groter zijn dan 32 tekens lang.

Resultaten van een module kunnen deterministische of nondeterministic.* * standaard, alle modules worden beschouwd als deterministisch. Dat wil zeggen, moet een onveranderlijke set invoerparameters en gegevens opgegeven, de module retourneren de dezelfde resultaten eacRAND of een functionh terwijl die deze wordt uitgevoerd. Dit gedrag, Azure Machine Learning Studio wordt alleen opnieuw uitgevoerd modules die zijn gemarkeerd als deterministisch als een parameter of de ingevoerde gegevens is gewijzigd. Retourneren van de resultaten in de cache levert ook veel sneller uitvoeren van experimenten.

Er zijn functies die niet-deterministisch, zoals RAND of een functie die de huidige datum of tijd retourneert. Als de module een niet-deterministische functie gebruikt, kunt u opgeven of de module niet-deterministisch is door de optionele instelling **isDeterministic** kenmerk **FALSE**. Hierdoor weet u zeker dat de module wordt opnieuw uitgevoerd wanneer het experiment wordt uitgevoerd, zelfs als de invoer-module en de parameters zijn niet gewijzigd. 

### <a name="language-definition"></a>Definitie van de taal
De **taal** -element in het definitie XML-bestand wordt gebruikt om op te geven van de aangepaste module-taal. Momenteel is R de enige ondersteunde taal. De waarde van de **bronbestand** kenmerk moet de naam van het R-bestand met de functie om aan te roepen wanneer de module wordt uitgevoerd. Dit bestand moet deel uitmaken van het zip-pakket. De waarde van de **entryPoint** kenmerk is de naam van de functie die wordt aangeroepen en moet overeenkomen met een geldige functie gedefinieerd met behulp van het bronbestand.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Poorten
De invoer- en poorten voor een aangepaste module zijn opgegeven in de onderliggende elementen van de **poorten** sectie van de definitie van het XML-bestand. De volgorde van deze elementen bepaalt de lay-out ervaren (UX) door gebruikers. Het eerste onderliggende **invoer** of **uitvoer** die worden vermeld in de **poorten** element van het XML-bestand wordt de meest linkse invoerpoort in de Machine Learning-UX.
Elk invoerkaart en uitvoerpoort kan hebben een optioneel **beschrijving** onderliggend element waarmee de tekst die wordt weergegeven wanneer u de muisaanwijzer de muisaanwijzer over de poort in de gebruikersinterface van de Machine Learning.

**Regels poorten**:

* Maximum aantal **invoer- en poorten** is 8 voor elk.

### <a name="input-elements"></a>Invoerelementen
Ingangspoorten kunnen u gegevens doorgeven aan uw R-functie en de werkruimte. De **gegevenstypen** die worden ondersteund voor invoer poorten als volgt zijn: 

**DataTable:** Dit type wordt doorgegeven aan uw R-functie als een data.frame. In feite alle typen (bijvoorbeeld CSV-bestanden of bestanden ARFF) die worden ondersteund door Machine Learning en die compatibel zijn met **DataTable** worden geconverteerd naar een data.frame automatisch. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

De **id** die zijn gekoppeld aan elk kenmerk **DataTable** invoerpoort moet een unieke waarde hebben en deze waarde moet overeenkomen met de bijbehorende parameter in de R-functie met de naam.
Optionele **DataTable** poorten die niet worden doorgegeven als invoer in een experiment hebben de waarde **NULL** doorgegeven aan de R-functie en de optionele zip poorten worden genegeerd als de invoer niet is verbonden. De **isOptional** kenmerk is optioneel voor zowel de **DataTable** en **Zip** van het type en is *false* standaard.

**Zip:** Aangepaste modules kunnen u een zip-bestand accepteren als invoer. Deze invoer is uitgepakt in de R-werkmap van uw functie

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

Voor aangepaste R-modules heeft de id voor een Zip-poort niet overeenkomt met de parameters van de R-functie. Dit is omdat het zip-bestand automatisch naar de R-werkmap wordt opgehaald.

**Invoer regels:**

* De waarde van de **id** kenmerk van de **invoer** element moet een geldige naam van de R-variabele.
* De waarde van de **id** kenmerk van de **invoer** element mag niet langer dan 64 tekens.
* De waarde van de **naam** kenmerk van de **invoer** element mag niet langer dan 64 tekens.
* De inhoud van de **beschrijving** element mag niet langer dan 128 tekens
* De waarde van de **type** kenmerk van de **invoer** element moet *Zip* of *DataTable*.
* De waarde van de **isOptional** kenmerk van de **invoer** element is niet vereist (en *false* standaard als niet is opgegeven); maar als deze is opgegeven, moeten deze *waar* of *false*.

### <a name="output-elements"></a>Uitvoer-elementen
**Standaarduitvoer poorten:** Uitvoerpoorten worden toegewezen aan de retourwaarden van uw R-functie, die vervolgens kan worden gebruikt door de volgende modules. *DataTable* is het alleen standaarduitvoer-poorttype momenteel ondersteund. (Ondersteuning voor *cursisten* en *transformeert* ontbreekt.) Een *DataTable* uitvoer wordt gedefinieerd als:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Voor uitvoer in aangepaste R-modules, de waarde van de **id** kenmerk heeft geen overeen te komen met iets in de R-script, maar deze moet uniek zijn. Voor een uitvoer één module de geretourneerde waarde van de R-functie moet een *data.frame*. Als u wilt meer dan een object van een ondersteunde gegevenstype uitvoer, moeten de juiste uitvoerpoorten worden opgegeven in de definitie van het XML-bestand en de objecten moeten worden geretourneerd als een lijst. De uitvoer-objecten worden toegewezen aan de uitvoer van de poorten van links naar rechts, te zetten op basis van de volgorde waarin de objecten in de lijst met resultaten worden geplaatst.

Bijvoorbeeld, als u wilt wijzigen de **rijen toevoegen van aangepaste** module voor de uitvoer van de oorspronkelijke twee gegevenssets, *dataset1* en *dataset2*, naast de nieuwe gekoppelde gegevensset *gegevensset*, (in een volgorde van links naar rechts, als: *gegevensset*, *dataset1*, *dataset2*), definieert u de uitvoerpoorten in de CustomAddRows.xml bestand als volgt:

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


En de lijst van objecten in een lijst in de juiste volgorde in 'CustomAddRows.R' retourneren:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Visualisatie-uitvoer:** U kunt ook opgeven een uitvoerpoort van het type *visualisatie*, die de uitvoer van de R-afbeeldingen apparaat en de console-uitvoer wordt weergegeven. Deze poort maakt geen deel uit van de uitvoer van de R-functie en de volgorde van de andere poort uitvoertypen niet verstoort. Als u wilt de poort van een visualisatie toevoegen aan de aangepaste modules, een **uitvoer** -element met een waarde van *visualisatie* voor de **type** kenmerk:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

**Regels voor uitvoer:**

* De waarde van de **id** kenmerk van de **uitvoer** element moet een geldige naam van de R-variabele.
* De waarde van de **id** kenmerk van de **uitvoer** element mag niet langer dan 32 tekens.
* De waarde van de **naam** kenmerk van de **uitvoer** element mag niet langer dan 64 tekens.
* De waarde van de **type** kenmerk van de **uitvoer** element moet *visualisatie*.

### <a name="arguments"></a>Argumenten
Aanvullende gegevens kunnen worden doorgegeven aan de R-functie via de moduleparameters die zijn gedefinieerd in de **argumenten** element. Deze parameters worden weergegeven in het deelvenster met de meest rechtse eigenschappen van de Machine Learning-gebruikersinterface wanneer de module is geselecteerd. Argumenten kunnen bestaan uit een van de ondersteunde typen of kunt u een aangepaste opsomming wanneer dat nodig is. Vergelijkbaar met de **poorten** elementen, **argumenten** elementen kunnen hebben een optioneel **beschrijving** element dat Hiermee geeft u de tekst die wordt weergegeven wanneer u de muisaanwijzer de parameternaam.
Optionele eigenschappen voor een module, zoals defaultValue, minValue en maxValue kunnen worden toegevoegd aan een van de argumenten als kenmerken aan een **eigenschappen** element. Geldige eigenschappen voor de **eigenschappen** element afhankelijk zijn van het argumenttype en met de ondersteunde argumenttypen in de volgende sectie worden beschreven. Argumenten met de **isOptional** eigenschap ingesteld op **"true"** niet vereisen dat de gebruiker een waarde invoeren. Als een waarde niet aan het argument is opgegeven, klikt u vervolgens het argument niet doorgegeven aan de functie voor het item. Argumenten van de functie voor de vermelding die optioneel zijn moeten expliciet worden verwerkt door de functie, bijvoorbeeld een standaardwaarde van NULL in de definitie van de vermelding punt functie toegewezen. Een optioneel argument wordt alleen de andere argument beperkingen, dat wil zeggen min of max, afdwingen als een waarde is opgegeven door de gebruiker.
Net als bij de invoer en uitvoer, is het essentieel dat elk van de parameters die zijn gekoppeld aan deze unieke id-waarden hebben. In ons voorbeeld snel aan de slag met de bijbehorende id/parameter was *wisselen*.

### <a name="arg-element"></a>Func-element
Een module-parameter wordt gedefinieerd met behulp van de **func** onderliggend element van de **argumenten** sectie van de definitie van het XML-bestand. Net als bij de onderliggende elementen in de **poorten** sectie, de volgorde van de parameters in de **argumenten** gedeelte definieert de lay-out aangetroffen in de UX De parameters weergegeven van boven naar beneden in de gebruikersinterface in dezelfde volgorde als waarin ze zijn gedefinieerd in het XML-bestand. De typen die worden ondersteund door Machine Learning voor parameters worden hier weergegeven. 

**int** – een geheel getal (32-bits) typeparameter.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Optionele eigenschappen*: **min**, **max**, **standaard** en **isOptional**

**dubbele** – een dubbele typeparameter.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Optionele eigenschappen*: **min**, **max**, **standaard** en **isOptional**

**BOOL** – een Boole-parameter die wordt vertegenwoordigd door een selectievakje in UX.

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Optionele eigenschappen*: **standaard** -false als niet is ingesteld

**tekenreeks**: een standaardtekenreeks

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *Optionele eigenschappen*: **standaard** en **isOptional**

**ColumnPicker**: de parameter van een kolom selecteren. Dit type wordt weergegeven in de UX als een kolomkiezer. De **eigenschap** element wordt hier gebruikt om op te geven van de id van de poort van waaruit kolommen zijn geselecteerd, waarbij het doeltype van de poort moet zijn *DataTable*. Het resultaat van de kolomselectie wordt doorgegeven aan de R-functie als een lijst met tekenreeksen die de naam van de geselecteerde kolom bevat. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Vereiste eigenschappen*: **portId** -komt overeen met de id van een invoer-element met het type *DataTable*.
* *Optionele eigenschappen*:
  
  * **allowedTypes** -Filters de kolom van het type van die u kunt kiezen. Geldige waarden zijn: 
    
    * Numeriek
    * Booleaans
    * Categorische gegevens
    * Reeks
    * Label
    * Functie
    * Score
    * Alle
  * **standaard** -geldige standaardselecties voor de kolomkiezer opnemen: 
    
    * Geen
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

**Vervolgkeuzelijst**: een lijst van de gebruiker opgegeven geïnventariseerd (vervolgkeuzelijst). De vervolgkeuzelijst items zijn opgegeven in de **eigenschappen** met behulp van element een **Item** element. De **id** voor elk **Item** moet uniek zijn en een geldige R-variabele. De waarde van de **naam** van een **Item** fungeert als zowel de tekst die u ziet als de waarde die is doorgegeven aan de R-functie.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *Optionele eigenschappen*:
  * **standaard** -de waarde voor de eigenschap default moet overeenkomen met een id-waarde van een van de **Item** elementen.

### <a name="auxiliary-files"></a>Aanvullende bestanden
Elk bestand dat wordt geplaatst in uw aangepaste module-ZIP-bestand is het verstandig om beschikbaar voor gebruik tijdens de uitvoeringstijd. Een directory-structuren aanwezig blijven behouden. Dit betekent dat bestand sourcing werkt hetzelfde lokaal en in Azure Machine Learning kan worden uitgevoerd. 

> [!NOTE]
> U ziet dat alle bestanden zijn uitgepakt naar de map 'src', zodat alle paden moet ' src /' voorvoegsel.
> 
> 

Stel dat u wilt dat alle rijen met NAs verwijderen uit de gegevensset en ook eventuele dubbele rijen verwijderen voordat u deze uitvoeren in CustomAddRows en u al een R-functie die door die worden ondersteund in een bestand RemoveDupNARows.R hebt geschreven:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
U kunt het aanvullende bestand RemoveDupNARows.R in de functie CustomAddRows verzendt via bron:

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

Upload vervolgens een zip-bestand met 'CustomAddRows.R', 'CustomAddRows.xml' en 'RemoveDupNARows.R' als een aangepaste R-module.

## <a name="execution-environment"></a>Uitvoeringsomgeving
De uitvoeringsomgeving van het R-script maakt gebruik van dezelfde versie van R als de **R-Script uitvoeren** module en de dezelfde standaard-pakketten kunt gebruiken. U kunt ook extra R-pakketten toevoegen aan uw aangepaste module door ze in de aangepaste module-zip-pakket. Alleen ze laden in uw R-script als in uw eigen R-omgeving. 

**Beperkingen van de uitvoeringsomgeving** opnemen:

* Niet-permanente bestandssysteem: Bestanden die zijn geschreven wanneer de aangepaste module wordt uitgevoerd, zijn niet permanent voor verschillende runs van dezelfde module.
* Er is geen toegang tot het netwerk

