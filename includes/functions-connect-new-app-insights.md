---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b1bbc11d7772e4f56d7dc6ead580b0a0cbd3cd8d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68669609"
---
Met functies kunt u eenvoudig Application Insights integratie toevoegen aan een functie-app vanuit de [Azure Portal].

1. Selecteer in de [Portal][azure portal]-Portal **alle services > functie-apps**, selecteer de functie-app en selecteer vervolgens de banner **Application Insights** boven aan het venster

    ![Application Insights inschakelen vanuit de portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Maak een Application Insights resource met behulp van de instellingen die zijn opgegeven in de tabel onder de afbeelding.

   ![Een Application Insights-resource maken](media/functions-connect-new-app-insights/ai-general.png)

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Unieke app-naam | Het is het gemakkelijkst om dezelfde naam te gebruiken als uw functie-app, die uniek moet zijn in uw abonnement. | 
    | **Location** | Europa -west | Gebruik, indien mogelijk, dezelfde [regio](https://azure.microsoft.com/regions/) als de functie-app, of een van de regio's die zich dicht bij die regio bevinden. |

1. Selecteer **OK**. De Application Insights resource wordt gemaakt in dezelfde resource groep en hetzelfde abonnement als uw functie-app. Nadat de resource is gemaakt, sluit u het venster Application Insights.

1. Ga terug naar de functie-app, selecteer **Toepassings instellingen**en schuif vervolgens omlaag naar **Toepassings instellingen**. Als u een instelling ziet met `APPINSIGHTS_INSTRUMENTATIONKEY`de naam, wordt Application Insights integratie ingeschakeld voor uw functie-app die wordt uitgevoerd in Azure.

[Azure Portal]: https://portal.azure.com
