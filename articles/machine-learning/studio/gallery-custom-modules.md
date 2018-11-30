---
title: Azure AI Gallery aangepaste modules - Azure Machine Learning Studio | Microsoft Docs
description: Aangepaste machine learning-modules in Azure AI Gallery detecteren.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.openlocfilehash: a504bcfbbcd70c1d6fc8402d4c232e7e8a5476b0
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313738"
---
# <a name="machine-learning-studio-discover-custom-machine-learning-modules-in-azure-ai-gallery"></a>Machine Learning Studio: Aangepaste machine learning-modules in Azure AI Gallery detecteren

[Azure AI Gallery](http://gallery.cortanaintelligence.com) bevat verschillende [aangepaste modules](https://gallery.cortanaintelligence.com/customModules) die de mogelijkheden van Azure Machine Learning Studio uitbreiden. U kunt de te gebruiken in uw experimenten, zodat u kunt ook meer geavanceerde predictive analytics-oplossingen ontwikkelen modules importeren.

Op dit moment de galerie modules biedt op *time series-analytics*, *koppelingsregels voor*, *clustering algoritmen* (anders dan k-means), en  *Visualisaties*, en andere hulpprogramma werkpaard modules.


## <a name="discover"></a>Ontdekken
Aangepaste modules Bladeren [in de galerie](http://gallery.cortanaintelligence.com)onder **meer**, selecteer **aangepaste Modules**.

![Aangepaste Modules op de startpagina van de galerie selecteren](./media/gallery-custom-modules/select-custom-modules-in-gallery.png)

De **[aangepaste Modules](https://gallery.cortanaintelligence.com/customModules)** pagina geeft een lijst met onlangs toegevoegde en populaire modules. Als u wilt weergeven van alle aangepaste modules, selecteer de **alle** knop. Als u wilt zoeken naar een specifieke aangepaste module, selecteer **alle**, en selecteer vervolgens filtercriteria. Ook kunt u zoektermen op in de **zoeken** vak aan de bovenkant van de pagina van de galerie.

![Selecteer 'Zie alle' om te bladeren alle aangepaste modules](./media/gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Begrijpen

Om te begrijpen hoe een gepubliceerde aangepaste module werkt, selecteert u de aangepaste module openen van de pagina met details van de module. De pagina met details biedt een consistente en informatieve leerervaring. Bijvoorbeeld, de detailpagina van het doel van de module worden gemarkeerd en een lijst met verwachte invoer, uitvoer en parameters. De pagina met details heeft ook een koppeling naar de onderliggende broncode, die u kunt bekijken en aanpassen.

### <a name="comment-and-share"></a>Opmerking en delen
Op een aangepaste module details pagina in de **opmerkingen** sectie, u kunt opmerkingen, feedback geven of vragen stellen over de module. U kunt de module ook delen met vrienden en collega's op Twitter of LinkedIn. U kunt ook een koppeling naar de pagina van de module details aan andere gebruikers uitnodigen voor het weergeven van de pagina e.

![Dit item delen met vrienden](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Uw eigen opmerkingen toevoegen](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>Importeren
U kunt een aangepaste module uit de galerie door uw eigen experimenten wilt importeren.

Azure AI Gallery biedt twee methoden voor het importeren van een kopie van de module:

* **In de galerie**. Als u een aangepaste module vanuit de galerie importeren, krijgt u ook een voorbeeldexperiment waarmee u een voorbeeld van hoe u het gebruik van de module.
* **Vanuit Machine Learning Studio**. U kunt een aangepaste module importeren terwijl u in Machine Learning Studio werkt (in dit geval niet u het voorbeeldexperiment ophalen).

### <a name="from-the-gallery"></a>In de galerie

1. Open de pagina met details van de module in de galerie. 
2. Selecteer **Open in Studio**.
   
    ![Open aangepaste module op basis van de galerie](./media/gallery-custom-modules/open-custom-module-from-gallery.png)
   
Elke aangepaste module bevat een voorbeeldexperiment die laat hoe u zien het gebruik van de module. Wanneer u selecteert **Open in Studio**, het voorbeeldexperiment wordt geopend in uw Machine Learning Studio-werkruimte. (Als u nog niet bent aangemeld bij Studio, u wordt gevraagd naar de eerst aanmeldt met uw Microsoft-account.)

Naast het voorbeeldexperiment, wordt de aangepaste module gekopieerd naar uw werkruimte. Ook wordt deze geplaatst in het modulepalet, met alle uw ingebouwde of aangepaste Machine Learning Studio-modules. U kunt deze nu gebruiken in uw eigen experimenten, net als elke andere module in uw werkruimte.

### <a name="from-within-machine-learning-studio"></a>Vanuit Machine Learning Studio

1. Selecteer in Machine Learning Studio, **nieuw**.
2. Selecteer **Module**. U kunt kiezen uit een lijst met Gallery-modules, of een specifieke module vinden met behulp van de **zoeken** vak.
3. Wijs met de muis op een module en selecteer vervolgens **Import-Module**. (Informatie over de module Selecteer **weergave in de galerie**. Hiermee gaat u naar de detailpagina van de module in de galerie.)
   
    ![Aangepaste module importeren in Machine Learning Studio](./media/gallery-custom-modules/add-custom-module-in-studio.png)

De aangepaste module is gekopieerd naar uw werkruimte en geplaatst in het modulepalet met de ingebouwde of aangepaste Machine Learning Studio-modules. U kunt deze nu gebruiken in uw eigen experimenten, net als elke andere module in uw werkruimte.

## <a name="use"></a>Gebruiken

Ongeacht welke methode u kiest voor het importeren van een aangepaste module bij het importeren van de module, de module wordt geplaatst in het modulepalet in Machine Learning Studio. In het modulepalet kunt u de aangepaste module in een experiment in uw werkruimte, net als elke andere module.

Het gebruik van een geïmporteerde module:

1. Een experiment maken of openen van een bestaand experiment.
2. Selecteer om uit te breiden de lijst met aangepaste modules in uw werkruimte, in het modulepalet **aangepaste**. Het modulepalet is aan de linkerkant van het experimentcanvas.
   
    ![Lijst met aangepaste module in Studio palet](./media/gallery-custom-modules/custom-module-in-studio-palette.png)
3. Selecteer de module die u hebt geïmporteerd en sleep deze naar uw experiment.


**[Ga naar de galerie](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

