---
title: Multi-Geo-Help-documentatie | Microsoft Docs
description: Meer informatie over het maken van een werkruimte en publiceren van een webservice in een Azure-regio verschillende van de Zuid-centraal VS (SCUS) Azure-regio.
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: e4941ccf8c6d7a0c77527e9c1d722bc3a770114a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="multi-geo-help-documentation"></a>Help-documentatie voor meerdere geografische gebieden
Dit artikel wordt beschreven hoe u een werkruimte maken en publiceren van een webservice in verschillende Azure-regio's.  De [Azure producten op de pagina regio](https://azure.microsoft.com/en-us/regions/services/) bevat gebieden waarin Azure Machine Learning beschikbaar is.

## <a name="create-a-workspace"></a>Een werkruimte maken
1. Aanmelden bij de klassieke Azure Portal.
2. Klik op **+ nieuw** > **GEGEVENSSERVICES** > **MACHINE LEARNING** > **met snelle invoer**.  Onder **locatie** selecteren, zoals een andere regio **Zuidoost-Azië**.
   ![Multi-Geo-Help afbeelding 1][1]
3. Selecteer de werkruimte en klik vervolgens op **aanmelden bij ML Studio**.
   ![Multi-Geo-Help afbeelding 2][2]
4. U hebt nu een werkruimte in een andere regio die u net als elke andere werkruimte mag gebruiken. U kunt schakelen tussen uw werkruimten, zoek in de rechterbovenhoek van het scherm. Klik op de vervolgkeuzelijst, selecteer de regio en selecteer vervolgens de werkruimte. Alles is lokaal op de werkruimte regio.  Bijvoorbeeld, worden alle webservices gemaakt op basis van een werkruimte in de werkruimte bevindt zich in dezelfde regio.
   ![Multi-Geo-Help afbeelding 3][3]

## <a name="open-an-experiment-from-gallery"></a>Een experiment openen vanuit galerie
Als u een experiment vanuit galerie openen, kunt u ook welke regio die u wilt kopiëren van het experiment te selecteren.

![Multi-Geo-Help afbeelding 4][4a]

## <a name="web-service-management"></a>Webservice beheren
Om webservices, zoals programmatisch te beheren voor retraining, het regiospecifieke-adres te gebruiken:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Let op
1. U kunt alleen experimenten tussen werkruimten die deel uitmaken van dezelfde regio op deze manier kopiëren. Als u wilt kopiëren experiment tussen werkruimten in verschillende regio's, kunt u de [PowerShell](http://aka.ms/amlps) commandlet [ *kopie AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) Agent installeert. Een andere oplossing is voor het publiceren van het experiment in de galerie in de modus voor niet-vermelde, opent u het in de werkruimte van de andere regio.
2. De regio-selector wordt alleen weergegeven voor werkruimten voor één regio tegelijk.  
3. Een gratis werkruimte of gast (anonieme verificatie) werkruimte wordt gemaakt en gehost in Zuid-centraal VS  
4. Web-services op basis van een werkruimte in Zuidoost-Azië geïmplementeerd zal ook worden gehost in Zuidoost-Azië.  

## <a name="more-information"></a>Meer informatie
Stel een vraag op de [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/multi-geo/multi-geo_1.png
[2]: ./media/multi-geo/multi-geo_2.png
[3]: ./media/multi-geo/multi-geo_3.png
[4a]: ./media/multi-geo/multi-geo_4a.png
