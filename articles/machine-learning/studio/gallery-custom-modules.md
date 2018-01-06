---
title: Azure-galerie AI aangepaste modules | Microsoft Docs
description: Aangepaste machine learning-modules in AI-galerie van Azure detecteren.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: roopalik;garye
ms.openlocfilehash: 278a24c44b76e6df097355b91d94a146be4aa9a9
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="discover-custom-machine-learning-modules-in-azure-ai-gallery"></a>Aangepaste machine learning-modules in AI-galerie van Azure detecteren
[!INCLUDE [machine-learning-gallery-item-selector](../../../includes/machine-learning-gallery-item-selector.md)]

## <a name="custom-modules-for-machine-learning-studio"></a>Aangepaste modules voor Machine Learning Studio
Azure AI-galerie biedt diverse [aangepaste modules](https://gallery.cortanaintelligence.com/customModules) die de mogelijkheden van Azure Machine Learning Studio uitbreiden. U kunt de modules gebruiken in uw experimenten, zodat u nog meer geavanceerde predictive analytics-oplossingen kunt ontwikkelen importeren.

Op dit moment wordt de galerie modules biedt op *time series analytics*, *koppeling regels*, *algoritmen clustering* (buiten k-means) en *visualisaties*, en andere hulpprogramma werkpaard modules.


## <a name="discover"></a>Ontdekken
Om te bladeren aangepaste modules [in de galerie](http://gallery.cortanaintelligence.com)onder **meer**, selecteer **aangepaste Modules**.

![Aangepaste Modules op de startpagina van de galerie selecteren](./media/gallery-custom-modules/select-custom-modules-in-gallery.png)

De  **[aangepaste Modules](https://gallery.cortanaintelligence.com/customModules)**  pagina geeft een lijst met modules populaire en onlangs toegevoegd. Als u wilt weergeven van alle aangepaste modules, selecteer de **alle** knop. Als u wilt zoeken naar een specifieke aangepaste module, selecteer **alle**, en selecteer vervolgens filtercriteria. Ook kunt u zoektermen op in de **Search** vak aan de bovenkant van de pagina Galerie.

![Selecteer 'Zie alle' om te bladeren alle aangepaste modules](./media/gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Begrijpen

Om te begrijpen hoe een aangepaste module voor gepubliceerde werkt, selecteert u de aangepaste module om de module detailpagina te openen. De detailpagina biedt een consistente en informatieve learning-ervaring. Bijvoorbeeld, de detailpagina markeert het doel van de module en geeft het verwachte invoer, uitvoer en parameters. De detailpagina heeft ook een koppeling naar de onderliggende gegevensbron code, die u kunt bekijken en aanpassen.

### <a name="comment-and-share"></a>Opmerking en -share
Op een aangepaste module pagina details van de **opmerkingen** sectie, u kunt opmerkingen, feedback geven of vragen hebt over de module. U kunt zelfs de module delen met vrienden of collega's op Twitter of LinkedIn. U kunt ook een koppeling naar de pagina van de module details om uit te nodigen van andere gebruikers naar de pagina e.

![Dit item delen met vrienden](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Uw eigen opmerkingen toevoegen](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>Importeren
U kunt een aangepaste module importeren uit de galerie naar uw eigen experimenten.

Azure AI-galerie biedt twee manieren een kopie van de module importeren:

* **In de galerie**. Als u een aangepaste module uit de galerie importeren, krijgt u ook een voorbeeldexperiment waarmee u een voorbeeld van het gebruik van de module.
* **Vanuit Machine Learning Studio**. U kunt een aangepaste module importeren terwijl u in Machine Learning Studio werkt (in dit geval wordt er geen het voorbeeldexperiment).

### <a name="from-the-gallery"></a>In de galerie

1. Open de pagina met details van de module in de galerie. 
2. Selecteer **openen in Studio**.
   
    ![Open aangepaste module uit de galerie](./media/gallery-custom-modules/open-custom-module-from-gallery.png)
   
Elke aangepaste module bevat een voorbeeldexperiment die laat zien hoe u de module. Wanneer u selecteert **openen in Studio**, het voorbeeldexperiment in uw werkruimte van Machine Learning Studio wordt geopend. (Als u nog niet bent aangemeld bij Studio, u gevraagd om de eerste te ondertekenen in met behulp van uw Microsoft-account.)

Naast het voorbeeldexperiment wordt de aangepaste module gekopieerd naar de werkruimte. Ook wordt deze geplaatst in het modulepalet met alle uw ingebouwde of aangepaste Machine Learning Studio-modules. U kunt het nu gebruiken in uw eigen experimenten, net als elke andere module in uw werkruimte.

### <a name="from-within-machine-learning-studio"></a>Vanuit Machine Learning Studio

1. Selecteer in Machine Learning Studio **nieuw**.
2. Selecteer **Module**. U kunt kiezen uit een lijst met modules galerie of een specifieke module zoeken met behulp van de **Search** vak.
3. Wijs met de muis op een module en selecteer vervolgens **Import Module**. (Als u informatie over de module, selecteer **weergave in de galerie**. Hiermee gaat u naar de pagina van de module details in de galerie.)
   
    ![Aangepaste module importeren in Machine Learning Studio](./media/gallery-custom-modules/add-custom-module-in-studio.png)

De aangepaste module is gekopieerd naar de werkruimte en geplaatst in het modulepalet met de ingebouwde of aangepaste Machine Learning Studio-modules. U kunt het nu gebruiken in uw eigen experimenten, net als elke andere module in uw werkruimte.

## <a name="use"></a>Gebruiken

Ongeacht welke methode u kiest voor het importeren van een aangepaste module, wanneer u de module importeren, de module wordt geplaatst in het modulepalet in Machine Learning Studio. In het modulepalet kunt u de aangepaste module in een experiment in uw werkruimte, net als elke andere module.

Een geïmporteerde module gebruiken:

1. Een experiment maken of open een bestaande experiment.
2. Selecteer om uit te breiden, de lijst met aangepaste modules in uw werkruimte, in het modulepalet **aangepaste**. Er is het modulepalet links van het experimentcanvas.
   
    ![Aangepaste Modulelijst in Studio palet](./media/gallery-custom-modules/custom-module-in-studio-palette.png)
3. Selecteer de module die u hebt geïmporteerd en sleep deze naar uw experiment.


**[Ga naar de galerie](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

