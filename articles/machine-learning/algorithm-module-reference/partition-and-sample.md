---
title: 'Partitie en steekproef: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Leer hoe u de module partitie en steekproef in Azure Machine Learning-service gebruikt om uit te voeren van steekproeven op een gegevensset of partities uit uw gegevensset te maken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 72f9e11e3582d804eecc7479ea079276564bd12f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029278"
---
# <a name="partition-and-sample-module"></a>Module-partitie en steekproef

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om uit te voeren van steekproeven op een gegevensset of partities uit uw gegevensset te maken.

Steekproeven is een belangrijk hulpprogramma in machine learning omdat u Hiermee kunt u de grootte van een gegevensset verkleinen terwijl de dezelfde verhouding van waarden. Deze module biedt ondersteuning voor verschillende taken met betrekking tot die belangrijk zijn in machine learning zijn: 

- Uw gegevens verdelen in meerdere subsecties van dezelfde grootte. 

    U kunt de partities voor kruisvalidatie of gevallen aan willekeurige groepen toewijzen.

- Het scheiden van gegevens in groepen en vervolgens werken met gegevens uit een specifieke groep. 

    Na het willekeurig gevallen toewijzen aan verschillende groepen, moet u de functies die gekoppeld aan slechts één groep zijn wijzigen.

- Een steekproef. 

    U kunt een percentage van de gegevens te extraheren, steekproeven van toepassing of kies een kolom te gebruiken voor taakverdeling van de gegevensset en toepassing stratificatie steekproeven uitvoeren op de waarden ervan.

- Het maken van een kleinere gegevensset voor het testen. 

    Als u een grote hoeveelheid gegevens hebt, wilt u mogelijk gebruiken alleen de eerste *n* rijen tijdens het instellen van het experiment en vervolgens overschakelen naar de volledige gegevensset gebruiken wanneer u uw model bouwt. U kunt ook steekproeven s kleinere om gegevensset te maken voor gebruik in ontwikkeling.

## <a name="configure-partition-and-sample"></a>Partitie en steekproef configureren

Deze module biedt ondersteuning voor meerdere methoden voor het verdelen van uw gegevens in partities of steekproeven. Eerst de methode te kiezen en vervolgens de aanvullende opties instellen door de methode vereist.

- Kop
- Steekproeven
- Toewijzen aan vouwen
- Vouw kiezen

### <a name="get-top-n-rows-from-a-dataset"></a>BOVENSTE N rijen uit een gegevensset ophalen

Gebruik deze modus om alleen de eerste *n* rijen. Deze optie is handig als u wilt testen van een experiment op een klein aantal rijen, en hoeft de gegevens worden met gelijke taakverdeling of een steekproef genomen op geen enkele manier.

1. Voeg de **partitie en steekproef** module aan uw experiment in de interface, en verbinding maken met de gegevensset.  

2. **Partitie of een voorbeeld van modus**: Deze optie instelt op **Head**.

3. **Aantal rijen te selecteren**: Typ het aantal rijen te retourneren.

    Het aantal rijen dat u opgeeft moet een niet-negatief geheel getal zijn. Als het aantal geselecteerde rijen groter dan het aantal rijen in de gegevensset is, wordt de volledige gegevensset geretourneerd.

4. Voer het experiment uit.

De module voert één gegevensset met alleen het opgegeven aantal rijen. De rijen zijn altijd lezen vanaf de bovenkant van de gegevensset.

### <a name="create-a-sample-of-data"></a>Een voorbeeld van gegevens maken

Deze optie biedt ondersteuning voor eenvoudige steekproeven of toepassing stratificatie steekproeven. Dit is handig als u maken van een kleinere representatieve steekproef gegevensset wilt voor het testen.

1. Toevoegen de **partitie en steekproef** module naar het experimentcanvas en koppel de gegevensset.

2. **Partitie of een voorbeeld van modus**: Stel dit in op **steekproeven**.

3. **Aantal steekproeven**: Typ een waarde tussen 0 en 1. Deze waarde geeft het percentage van rijen uit de brongegevensset die moeten worden opgenomen in de uitvoergegevensset.

    Bijvoorbeeld, als u slechts de helft van de oorspronkelijke gegevensset wilt, typt `0.5` om aan te geven dat de samplingfrequentie 50% moet zijn.

    De rijen van de invoergegevensset zijn zij en selectief plaatsen in de uitvoergegevensset op basis van de opgegeven verhouding.

4. **Willekeurige basis voor lijnen**: Typ desgewenst een geheel getal te gebruiken als een seed-waarde.

    Deze optie is belangrijk als u wilt dat de rijen op dezelfde manier telkens worden verdeeld. De standaardwaarde is 0, zin dat een eerste seeding is gegenereerd op basis van de systeemklok. Dit kan enigszins resultaten leiden telkens wanneer die u het experiment uitvoert.

5. **Toepassing stratificatie splitsen voor lijnen**: Selecteer deze optie als het is belangrijk dat de rijen in de gegevensset gelijkmatig moeten worden gedeeld door een sleutelkolom voordat steekproeven.

    Voor **indeling sleutelkolom voor lijnen**, selecteert u een enkel *strata kolom* te gebruiken bij het delen van de gegevensset. De rijen in de gegevensset vervolgens als volgt verdeeld:

    1. Alle invoer rijen worden (stratified) door de waarden in de kolom opgegeven strata gegroepeerd.

    2. Rijen worden zij in elke groep.

    3. Elke groep wordt selectief toegevoegd aan de uitvoergegevensset om te voldoen aan de opgegeven verhouding.


6. Voer het experiment uit.

    Met deze optie voert de module een enkele gegevensset die een representatieve steekproef van de gegevens bevat. De resterende, unsampled gedeelte van de gegevensset wordt niet uitgevoerd. 

## <a name="split-data-into-partitions"></a>Gegevens splitsen in partities

Gebruik deze optie als u wilt de gegevensset onderverdelen in subsets van de gegevens. Deze optie is ook handig als u wilt maken van een aangepaste aantal vouwen voor kruisvalidatie, of om te splitsen rijen in verschillende groepen.

1. Toevoegen de **partitie en steekproef** module naar het experimentcanvas en koppel de gegevensset.

2. Voor **partitie of voorbeeld modus**, selecteer **toewijzen aan vouwen**.

3. **Gebruik vervanging in de partitioneren**: Selecteer deze optie als u wilt dat de sample de rij die moet worden teruggeplaatst in de groep van de rijen voor mogelijk hergebruik. Als gevolg hiervan mogelijk dezelfde rij worden toegewezen aan verschillende vouwen.

    Als u geen vervanging (de standaardoptie), werkt de sample rij is niet teruggeplaatst in de groep van de rijen voor mogelijk hergebruik. Elke rij kan als gevolg hiervan worden toegewezen aan slechts één vouwen.

4. **Willekeurige split**:  Selecteer deze optie als u wilt dat rijen willekeurig worden toegewezen aan vouwen.

    Als u deze optie niet selecteert, worden rijen zijn toegewezen aan vouwen met behulp van de methode round robin.

5. **Random seed**: Typ desgewenst een geheel getal te gebruiken als de seed-waarde. Deze optie is belangrijk als u wilt dat de rijen op dezelfde manier telkens worden verdeeld. Anders de standaardwaarde van 0 betekent dat een random seed starten wordt gebruikt.

6. **Geef de methode partitioner**: Geeft aan hoe u gegevens naar elke partitie worden verdeeld met behulp van deze opties:

    - **Partitie-gelijkmatig**: Gebruik deze optie om een gelijk aantal rijen in elke partitie. Als het aantal partities van de uitvoer opgeven, typt u een geheel getal in de **geeft het aantal vouwen gelijkmatig gesplitst** in het tekstvak.

    - **Partitie met aangepaste verhoudingen**: Gebruik deze optie om op te geven van de grootte van elke partitie als een door komma's gescheiden lijst.

        Bijvoorbeeld, als u wilt maken van drie partities, met de eerste partitie met 50% van de gegevens en de resterende twee partities elke met 25% van de gegevens, klikt u op de **door met door komma's gescheiden lijst verhoudingen** in het tekstvak en Deze nummers hebt getypt: `.5, .25, .25`

        De som van de grootte van alle partities moet toevoegen tot exact 1 zijn.

        - Als u getallen oplopen tot **minder dan 1**, een extra partitie is gemaakt voor het opslaan van de overige rijen. Bijvoorbeeld, als u de waarden .2 of hoger en.3, een derde partitie is gemaakt die de resterende 50 procent van alle rijen bevat.

        - Als u getallen oplopen tot **meer dan 1**, een foutmelding wordt gegenereerd wanneer u het experiment uitvoert.

7. **Toepassing stratificatie split**: Selecteer deze optie als u wilt dat de rijen in de toepassing worden stratificatie wanneer splitsen en kies vervolgens de _strata kolom_.

8. Voer het experiment uit.

    Met deze optie voert de module meerdere gegevenssets, gepartitioneerd met behulp van de regels die u hebt opgegeven.

### <a name="use-data-from-a-predefined-partition"></a>Gegevens uit een vooraf gedefinieerde partitie gebruiken  

Deze optie wordt gebruikt wanneer u een gegevensset zijn onderverdeeld in meerdere partities en nu wilt laden van elke partitie op zijn beurt voor verdere analyse of verwerking.

1. Voeg de **partitie en steekproef** module naar het experimentcanvas.

2. Verbinden met de uitvoer van een vorige instantie van **partitie en steekproef**. Dat exemplaar moet hebben gebruikt de **toewijzen aan vouwen** optie voor het genereren van een aantal partities.

3. **Partitie of een voorbeeld van modus**: Selecteer **kiezen vouwen**.

4. **Opgeven welke fold om te worden opgehaald uit**: Selecteer een partitie te gebruiken door de index te typen. Partitie-indexen zijn gebaseerd op 1. Bijvoorbeeld, als u de gegevensset onderverdeeld in drie delen, moet de partities indexen 1, 2 en 3.

    Als u een ongeldige indexwaarde typt, wordt er een fout bij het ontwerp van de code gegenereerd: "Fout 0018: DataSet bevat ongeldige gegevens."

    Naast de gegevensset door vouwen groeperen, scheidt u de gegevensset in twee groepen: een vouw doel, en alle andere opties. Dit doet, typt u de index van een enkele Vouw en selecteert u vervolgens de optie **kiezen aanvulling van de geselecteerde Vouw**, alles behalve de gegevens in de opgegeven Vouw ophalen.

5. Als u met meerdere partities werkt, moet u extra exemplaren van toevoegen de **partitie en steekproef** module voor het afhandelen van elke partitie.

    Stel dat bijvoorbeeld eerder gepartitioneerde patiënten in vier vouwen met leeftijd. Als u wilt werken met elke afzonderlijke delen, moet u vier kopieën van de **partitie en steekproef** -module, en selecteer in elk u een andere vouwen, zoals hieronder wordt weergegeven. Het is niet juist gebruik van de **toewijzen aan vouwen** uitvoer rechtstreeks.  

    [![Partitie en steekproef](./media/partition-and-sample/partition-and-sample.png)](./media/partition-and-sample/partition-and-sample-lg.png#lightbox)

5. Voer het experiment uit.

    Met deze optie voert de module één gegevensset met alleen de rijen die zijn toegewezen aan die vouwen.

> [!NOTE]
>  U kunt de aanduidingen vouwen rechtstreeks; niet weergeven ze zijn alleen toegestaan in de metagegevens.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 