---
title: Een classificatie - Custom Vision Service maken
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de website van Custom Vision om een installatiekopie van classificatie-model te maken.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: f2cd8f5074f815e84caaedb01335406657f29088
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58088006"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Over het bouwen van een classificatie met Custom Vision

Voor het gebruik van de Custom Vision Service voor classificatie van afbeeldingen, moet u eerst een classificatie-model bouwen. In deze handleiding leert u hoe u een classificatie via de website van Custom Vision maken.

## <a name="prerequisites"></a>Vereisten

- Een geldig Azure-abonnement. [Maak een account](https://azure.microsoft.com/free/) gratis.
- Een set waarmee u uw classificatie trainen. Hieronder vindt u tips over het kiezen van afbeeldingen.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Custom Vision resources maken in Azure Portal
Voor het gebruik van Custom Vision Service, moet u maken van aangepaste Vision trainen en voorspellen resources in de in de [Azure-portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). Hiermee maakt u een Training- en Voorspellingsgegevens resource. 

## <a name="create-a-new-project"></a>Een nieuw project maken

Navigeer in uw webbrowser naar de [Custom Vision webpagina](https://customvision.ai) en selecteer __aanmelden__. Meld u aan met hetzelfde account waarmee u zich bij de Azure-Portal.

![Afbeelding van de aanmeldingspagina opgeven](./media/browser-home.png)


1. Voor het maken van uw eerste project, selecteer **nieuw Project**. De **nieuw project maken** in het dialoogvenster wordt weergegeven.

    ![Het dialoogvenster Nieuw project bevat de velden voor de naam, beschrijving en domeinen.](./media/getting-started-build-a-classifier/new-project.png)

1. Voer een naam en een beschrijving op voor het project. Selecteer vervolgens een resourcegroep. Als uw aangemelde account gekoppeld aan een Azure-account is, worden alle van uw Azure-resourcegroepen die een Resource Custom Vision Service bevatten door de resourcegroep vervolgkeuzelijst weergegeven. 

> [!NOTE]
> Als er is geen resourcegroep beschikbaar is, Controleer of dat u hebt aangemeld bij [customvision.ai](https://customvision.ai) met hetzelfde account als u gebruikt voor aanmelding bij de [Azure Portal](https://portal.azure.com/). Ook het geval is, Controleer of dat u hebt dezelfde "map' in de portal voor Custom Vision geselecteerd als de map in de Azure-portal waar uw aangepaste Vision resources zich bevinden. Op beide locaties, kunt u uw directory selecteren in de vervolgkeuzelijst account in de rechterbovenhoek van het scherm. 

1. Selecteer __classificatie__ onder __projecttypen__. Klik vervolgens onder __Classificatietypen__, kiest u **Multilabel** of **Multiklasse**, afhankelijk van uw situatie. Multilabel classificatie geldt een willekeurig aantal uw tags op een installatiekopie (nul of meer), terwijl multiklassen classificatie afbeeldingen in enkele categorieën sorteert (elke afbeelding die u indient worden gesorteerd in de meest waarschijnlijke tag). U kunt zich later het type classificatie wijzigen als u wenst.

1. Selecteer vervolgens een van de beschikbare domeinen. Elk domein optimaliseert de classificatie voor specifieke typen van installatiekopieën, zoals beschreven in de volgende tabel. U kunt zich het domein desgewenst later wijzigen.

    |Domain|Doel|
    |---|---|
    |__Algemene__| Geoptimaliseerd voor een grote verscheidenheid aan taken voor bestandsclassificatie installatiekopie. Als geen van de andere domeinen geschikt zijn, of u niet zeker weet welk domein om te kiezen, selecteert u het algemene domein. |
    |__Voedsel__|Geoptimaliseerd voor foto's van maaltijden zoals u ze in een restaurant zou zien. Als u wilt voor het classificeren van foto's van individuele vruchten of fruit, gebruikt u het domein Food.|
    |__Oriëntatiepunten__|Geoptimaliseerd voor het herkennen van oriëntatiepunten, natuurlijke en kunstmatige. Dit domein werkt het beste als de oriëntatiepunt duidelijk zichtbaar zijn in de foto is. Dit domein werkt, zelfs als de oriëntatiepunt iets door mensen in het zicht van deze vorm.|
    |__Detailhandel__|Geoptimaliseerd voor afbeeldingen die in een winkelwagen catalogus of webwinkels zijn gevonden. Als u classificeren van hoge precisie tussen japonnen, broeken en shirt wilt, gebruikt u dit domein.|
    |__Volwassene__|Geoptimaliseerd voor de inhoud voor volwassenen en niet-volwassene inhoud beter definiëren. Bijvoorbeeld, als u blokkeren van installatiekopieën van mensen in bad kleuren wilt, kunt dit domein u een aangepaste classificatie om dit te doen maken.|
    |__Compact domeinen__| Geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. De modellen die worden gegenereerd door compact domeinen kunnen worden geëxporteerd als lokaal wilt uitvoeren.|
    
1. Selecteer ten slotte __project maken__.

## <a name="choose-training-images"></a>Trainingsafbeeldingen kiezen

Als een minimum, raden wij aan u ten minste 30 afbeeldingen per tag in de eerste training-set. Moet u een paar extra afbeeldingen om uw model testen nadat deze is getraind verzamelen.

Als u wilt effectief te werk om uw model te trainen, afbeeldingen met verschillende visual te gebruiken. Selecteren van installatiekopieën die variëren per:
* camerahoek
* licht
* Achtergrond
* stijl van de Visual
* afzonderlijke/gegroepeerd subject(s)
* grootte
* type

Controleer ook of alle van de trainingsafbeeldingen voldoen aan de volgende criteria:
* JPG, PNG of BMP-indeling
* niet langer zijn dan 6MB in grootte (4MB voor voorspelling installatiekopieën)
* niet kleiner zijn dan 256 pixels aan de rand van het kortste; alle installatiekopieën die korter zijn dan deze worden automatisch geschaald van door de Custom Vision Service

## <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

In deze sectie wordt u uploaden en afbeeldingen te trainen classificatie handmatig labelen. 

1. Als u wilt toevoegen van afbeeldingen, klikt u op de __afbeeldingen toevoegen__ knop en selecteer vervolgens __door lokale bestanden bladeren__. Selecteer __Open__ naar tagging. Uw selectie van het label wordt toegepast op de hele groep van installatiekopieën die u hebt geselecteerd om te uploaden, zodat het eenvoudiger om afbeeldingen in afzonderlijke groepen op basis van hun gewenste labels te uploaden. U kunt ook de tags voor afzonderlijke afbeeldingen wijzigen nadat ze zijn geüpload.

    ![Het besturingselement van de installatiekopieën toevoegen wordt weergegeven in de linkerbovenhoek en als een knop op onderaan in het midden.](./media/getting-started-build-a-classifier/add-images01.png)


1. Voer tekst in voor het maken van een tag, de __mijn Tags__ veld en drukt u op Enter. Als de code al bestaat, wordt deze weergegeven in een vervolgkeuzelijst. In een multilabel-project, kunt u meer dan één tag toevoegen aan uw installatiekopieën, maar in een multiklassen project u slechts één kunt toevoegen. Voor het voltooien van de afbeeldingen te uploaden, gebruikt u de __[aantal] bestanden uploaden__ knop. 

    ![Afbeelding van de pagina tag en uploaden](./media/getting-started-build-a-classifier/add-images03.png)

1. Selecteer __gedaan__ zodra de afbeeldingen zijn geüpload.

    ![De voortgangsbalk wordt aangegeven dat alle taken zijn voltooid.](./media/getting-started-build-a-classifier/add-images04.png)

Voor het uploaden van een andere set van installatiekopieën, Ga terug naar de bovenkant van deze sectie en Herhaal de stappen. Op een bepaald moment in uw project, moet u mogelijk toevoegen _voorbeelden negatieve_ om uw classificatie nauwkeurigere ervoor. Negatieve voorbeelden zijn die niet overeenkomen met een van de andere labels. Wanneer u deze installatiekopieën uploaden, van toepassing op de speciale **negatieve** naar hen te labelen.

> [!NOTE]
> De Custom Vision Service biedt ondersteuning voor de verwerking van sommige automatische negatieve afbeeldingen. Bijvoorbeeld, als u een gedeeltelijk versus bananen classificatie bouwen en verzenden van een installatiekopie van een schoen voor voorspelling, moet de classificatie die installatiekopie als dicht bij 0% score voor gedeeltelijk en bananen.
> 
> In gevallen waar de negatieve afbeeldingen slechts een variant van de afbeeldingen in training gebruikt zijn, is het aan de andere kant waarschijnlijk dat het model wordt het classificeren van de installatiekopieën van het negatieve als een gelabelde klasse vanwege de grote overeenkomsten. Bijvoorbeeld, als u een oranje versus pompelmoezen en pomelo's classificatie hebt en u in een afbeelding van een clementine feed, kan het beoordelen de clementine als een oranje omdat veel functies van de clementine lijken op die van de appels. Als uw negatieve installatiekopieën van deze aard zijn, het beste maken van een of meer extra labels (zoals **andere**) en de negatieve afbeeldingen met dit label te labelen tijdens de training om toe te staan van het model beter onderscheid maken tussen deze klassen .

## <a name="train-the-classifier"></a>De classificatie trainen

De classificatie trainen, selecteer de **trainen** knop. De classificatie maakt gebruik van alle huidige installatiekopieën om een model waarin de visuele eigenschappen van elke tag te maken.

![De knop van de trein in de rechterbovenhoek van de webpagina header-werkbalk](./media/getting-started-build-a-classifier/train01.png)

Het trainingsproces duurt slechts een paar minuten. Informatie over het trainingsproces wordt gedurende deze periode worden weergegeven in de **prestaties** tabblad.

![Het browservenster een dialoogvenster training in het hoofdgedeelte](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>De classificatie evalueren

Nadat de training is voltooid, wordt de prestaties van het model geschat en weergegeven. De Custom Vision Service maakt gebruik van de installatiekopieën die u hebt ingediend voor training voor het berekenen van precisie- en zoals eerder vermeld, met behulp van een proces met de naam [k-Vouw cross validatie](https://en.wikipedia.org/wiki/Cross-validation_(statistics)). Zijn twee verschillende metingen van de effectiviteit van een classificatie van precisie- en intrekken:

- **Precisie** geeft aan dat de fractie van geïdentificeerde classificaties die correct zijn. Bijvoorbeeld, als het model geïdentificeerd 100 afbeeldingen als honden en 99 hiervan daadwerkelijk van honden zijn, zou klikt u vervolgens de nauwkeurigheid zijn 99%.
- **Intrekken** geeft aan dat het gedeelte van de werkelijke classificaties die zijn geïdentificeerd. Bijvoorbeeld, als er daadwerkelijk 100 afbeeldingen van appels en het model 80 als appels geïdentificeerd, zou het intrekken van het 80%.

![De resultaten van de training tonen de algehele precisie en intrekken en de nauwkeurigheid en intrekken voor elk label in de classificatie.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>Drempelwaarde voor waarschijnlijkheid

Houd er rekening mee de **drempelwaarde voor waarschijnlijkheid** schuifregelaar in het linkerdeelvenster van de **prestaties** tabblad. Dit is de drempel voor een voorspelde kans juist worden overwogen bij het berekenen van precisie- en intrekken.

Voorspelling aanroepen met de drempelwaarde voor een hoge waarschijnlijkheid interpreteren doorgaans retourneren van resultaten met hoge precisie ten koste van de intrekken (de gevonden classificaties juist zijn, maar veel zijn niet gevonden); een kans op lage drempelwaarde komt het tegenovergestelde (de meeste van de werkelijke classificaties zijn gevonden, maar er zijn fout-positieven binnen deze is ingesteld). Met dat gegeven in gedachte, moet u de drempelwaarde voor waarschijnlijkheid op basis van de specifieke behoeften van uw project instellen. Later, op de client, moet u de dezelfde drempelwaarde voor waarschijnlijkheid als een filter bij het ontvangen van voorspellingsresultaten uit het model.

## <a name="manage-training-iterations"></a>Training iteraties beheren

Elke keer dat u uw classificatie trainen, maakt u een nieuw _iteratie_ met een eigen bijgewerkte prestatiemetrieken. U kunt alle uw iteraties weergeven in het linkerdeelvenster van de **prestaties** tabblad. Wanneer u een selecteert, hebt u de optie zodat de _standaard iteratie_ door te klikken op de **standaard** bovenaan op de knop. De _standaard iteratie_ is het model die standaard worden gebruikt wanneer u een query via de API voor voorspellingen uitvoeren (vanuit een app, bijvoorbeeld). Als u niet akkoord gaat om bij te werken de _iteratie standaard_, kunt u doorgaan met het trainen van uw model zonder gevolgen voor de huidige gedrag van uw app; vervolgens, wanneer u tevreden met het verbeterde model bent, kunt u de standaard bijwerken.

In het linkerdeelvenster vindt u ook de **verwijderen** knop, die u een iteratie verwijderen kunt als deze verouderd is. Als u een iteratie verwijdert, verwijdert u alle installatiekopieën die gekoppeld aan deze zijn.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd over het maken en een afbeelding classificatie-model met behulp van de website van Custom Vision te trainen. Meer informatie over de iteratief proces van het verbeteren van uw model vervolgens ophalen

[Een model testen en opnieuw trainen](test-your-model.md)

