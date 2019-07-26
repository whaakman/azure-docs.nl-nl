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
ms.openlocfilehash: 9bac92bc1cc94b88dc694b468b795049db4ac9df
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443983"
---
1. Ga naar de [Azure Portal](https://portal.azure.com).

1. Selecteer **+ een resource maken** aan de linkerkant en kies vervolgens functie- **app**.

1. Voor het **hosting plan**kiest u **app service plan**en selecteert u vervolgens **app service plan/locatie**.

    ![Een functie-app maken](./media/functions-premium-create/create-function-app-resource.png)

1. Selecteer **Nieuw maken**, typ een naam voor het **app service plan** , kies een **locatie** in een [regio](https://azure.microsoft.com/regions/) bij u in de buurt of in de buurt van andere services die uw functies gebruiken en selecteer vervolgens **prijs categorie**.

    ![Een App Service-plan maken](./media/functions-premium-create/new-app-service-plan.png)

1. Kies het **EP1** -abonnement (elastisch Premium) en selecteer vervolgens **Toep assen**.

    ![Premium-abonnement selecteren](./media/functions-premium-create/hosting-plan.png) 

1. Selecteer **OK** om het plan te maken en gebruik vervolgens de resterende functie-app-instellingen, zoals opgegeven in de tabel onder de afbeelding. 

    ![Het app service-abonnement is voltooid](./media/functions-premium-create/create-function-app.png)  

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. U kunt ook de voorgestelde waarde gebruiken. |
    | **Besturingssysteem** | Voorkeurs besturingssysteem | Zowel Linux als Windows worden ondersteund in het Premium-abonnement. |
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. Alleen talen die worden ondersteund op het gekozen **besturings systeem** worden weer gegeven. |
    | **[Opslag](../articles/storage/common/storage-quickstart-create-account.md)** |  Wereldwijd unieke naam |  Maak een opslagaccount die wordt gebruikt door uw functie-app. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Standaard | Hiermee maakt u een Application Insights bron van dezelfde *app-naam* in de dichtstbijzijnde ondersteunde regio. Door deze instelling uit te breiden, kunt u de **nieuwe resource naam** wijzigen of een andere **locatie** in een [Azure-regio](https://azure.microsoft.com/global-infrastructure/geographies/) kiezen waar u uw gegevens wilt opslaan. |

1. Nadat uw instellingen zijn gevalideerd, selecteert u **maken**.

1. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-premium-create/function-app-create-notification.png)

1. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken. U kunt ook **vastmaken aan dash board**selecteren. Vastmaken maakt het gemakkelijker om terug te gaan naar deze functie-app-resource vanuit uw dash board.