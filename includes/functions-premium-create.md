---
title: bestand opnemen
description: bestand opnemen
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/01/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 0f3303e7bc87ca0bd29f367405372568ed6da7a7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131565"
---
1. Ga naar de [Azure Portal](https://portal.azure.com).

1. Selecteer **+ een resource maken** Kies aan de linkerzijde **functie-app**.

1. Voor **Hosting plan**, kiest u **App Service-plan**en selecteer vervolgens **App Service-plan /-locatie**.

    ![Een functie-app maken](./media/functions-premium-create/create-function-app-resource.png)

1. Selecteer **nieuw**, type een **App Service-plan** -naam, kiest u een **locatie** in een [regio](https://azure.microsoft.com/regions/) buurt of in de buurt van andere services uw functies toegang tot en selecteer vervolgens **prijscategorie**.

    ![Een App Service-plan maken](./media/functions-premium-create/new-app-service-plan.png)

1. Kies de **EP1** (elastische Premium) plannen en selecteer vervolgens **toepassen**.

    ![Premium-abonnement selecteren](./media/functions-premium-create/hosting-plan.png) 

1. Selecteer **OK** te maken van het plan en gebruik vervolgens de resterende functie app-instellingen als de opgegeven in de tabel onder de afbeelding. 

    ![Voltooide app service-plan](./media/functions-premium-create/create-function-app.png)  

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. U kunt ook de aanbevolen waarde gebruiken. |
    | **Besturingssysteem** | Windows | Linux is momenteel niet ondersteund in het Premium-abonnement. |
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. Alleen talen die worden ondersteund op de door u gekozen **OS** worden weergegeven. |
    | **[Opslag](../articles/storage/common/storage-quickstart-create-account.md)** |  Wereldwijd unieke naam |  Maak een opslagaccount die wordt gebruikt door uw functie-app. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Standaard | Hiermee maakt u een Application Insights-resource met dezelfde *appnaam* in de dichtstbijzijnde ondersteunde regio bevindt. Door het uitbreiden van deze instelling kunt u de **nieuwe resourcenaam** of kies een andere **locatie** in een [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) waar u uw gegevens kunt opslaan. |

1. Nadat u uw instellingen worden gevalideerd, schakelt u **maken**.

1. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-premium-create/function-app-create-notification.png)

1. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken. U kunt ook selecteren **vastmaken aan dashboard**. Vast te maken, maakt het eenvoudiger om terug te keren naar de resource voor deze functie-app in uw dashboard.