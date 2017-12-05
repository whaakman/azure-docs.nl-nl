---
title: Azure API Management-services in meerdere Azure-regio's implementeren | Microsoft Docs
description: Informatie over het implementeren van een Azure API Management-service-exemplaar op meerdere Azure-regio's.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: e126e34bc9fce21243b0ef79f5ab661aec3a2de6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Een Azure API Management-service-exemplaar implementeren op meerdere Azure-regio 's
API Management biedt ondersteuning voor meerdere landen/regio-implementatie waarmee de API-uitgevers voor een enkele API management-service verdelen over een willekeurig aantal gewenste Azure-regio's. Dit vermindert de latentie waargenomen door geografisch verspreid API consumenten en verbetert tevens de servicebeschikbaarheid als één regio offline gaat. 

Wanneer u een API Management-service in eerste instantie is gemaakt, bevat deze slechts één [eenheid] [ unit] en bevindt zich op één Azure-regio, die is opgegeven als de primaire regio. Extra gebieden kunnen eenvoudig worden toegevoegd via de Azure-Portal. Een API Management-gatewayserver wordt geïmplementeerd voor elke regio en aanroep verkeer wordt doorgestuurd naar de dichtstbijzijnde gateway. Als een regio offline gaat, wordt het verkeer automatisch opnieuw gerichte naar de volgende dichtstbijzijnde gateway. 

> [!IMPORTANT]
> Implementatie van meerdere landen/regio is alleen beschikbaar in de  **[Premium] [ Premium]**  laag.
> 
> 

## <a name="add-region"></a>Exemplaar van API Management-service implementeren in een nieuw gebied
> [!NOTE]
> Als u nog geen exemplaar van API Management-service hebt gemaakt, raadpleegt u [API Management service-exemplaar maken][Create an API Management service instance].
> 
> 

Navigeer in de Azure Portal naar de **schaal en prijzen** pagina voor uw API Management-service-exemplaar. 

![Tabblad schaal][api-management-scale-service]

Als u wilt implementeren op een nieuwe regio, klikt u op **+ toevoegen regio** van de werkbalk.

![Regio toevoegen][api-management-add-region]

Selecteer de locatie in de vervolgkeuzelijst en het aantal eenheden voor met de schuifregelaar ingesteld.

![Geef de eenheden][api-management-select-location-units]

Klik op **toevoegen** uw selectie in de tabel locaties plaatsen. 

Herhaal dit proces totdat u alle geconfigureerde locaties en klik op **opslaan** van de werkbalk om het implementatieproces start.

## <a name="remove-region"></a>Verwijderen van exemplaar van API Management-service van een locatie
Navigeer in de Azure Portal naar de **schaal en prijzen** pagina voor uw API Management-service-exemplaar. 

![Tabblad schaal][api-management-scale-service]

Voor de locatie die u wilt verwijderen open het context-menu met de **...**  knop aan de rechterkant van de tabel. Selecteer de **verwijderen** optie.

![Regio verwijderen][api-management-remove-region]

De verwijdering te bevestigen en klikt u op **opslaan** de wijzigingen toe te passen.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

