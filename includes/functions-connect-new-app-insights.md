---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b6cafcfe6c892cd43f056458fe3586da834c2fd1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131440"
---
Functies kunt u eenvoudig integratie van Application Insights toevoegen aan een functie-app uit de [Azure Portal].

1. In de [portal][Azure Portal], selecteer **alle services > functie-Apps**, selecteer uw functie-app en kies vervolgens de **Application Insights** standaardvaandel aan de bovenkant van het venster

    ![Application Insights inschakelen vanuit de portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Maak een Application Insights-resource met behulp van de instellingen die zijn opgegeven in de tabel onder de afbeelding:

   ![Een Application Insights-resource maken](media/functions-connect-new-app-insights/ai-general.png)

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam** | Unieke app-naam | Het is eenvoudigst te gebruiken dezelfde naam als uw functie-app moet uniek zijn in uw abonnement. | 
    | **Locatie** | Europa -west | Gebruik zo mogelijk dezelfde [regio](https://azure.microsoft.com/regions/) als uw functie-app, of bij het. |

1. Kies **OK**. De Application Insights-resource wordt gemaakt in dezelfde resourcegroep en abonnement als uw functie-app. Nadat het maken is voltooid, sluit u het venster Application Insights.

1. Selecteer terug in uw functie-app **toepassingsinstellingen**, en schuif omlaag naar **toepassingsinstellingen**. Als u een instelling met de naam ziet `APPINSIGHTS_INSTRUMENTATIONKEY`, betekent dit dat de Application Insights-integratie is ingeschakeld voor uw functie-app in Azure.

[Azure Portal]: https://portal.azure.com
