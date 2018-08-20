---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 07/17/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 75ff9ec8cdf4cd2747ab00f2c23de509607a5429
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "40100970"
---
1. Selecteer de knop **Nieuw** in de linkerbovenhoek van de Azure-portal. Selecteer vervolgens **Compute** > **Functie-app**. 

    ![Een functie-app maken in de Azure-portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Gebruik voor de functie-app de instellingen die in de tabel onder de afbeelding zijn opgegeven.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. | 
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. | 
    | **Besturingssysteem** | Windows | Hosten zonder server is momenteel alleen beschikbaar wanneer u gebruikmaakt van Windows. Raadpleeg [Create your first function running on Linux using the Azure CLI](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md) (Uw eerste Linux-functie maken met behulp van de Azure CLI) voor meer informatie over hosten met Linux. |
    | **[Hostingplan](../articles/azure-functions/functions-scale.md)** | Verbruiksabonnement | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. In het standaard **Verbruiksabonnement** worden resources dynamisch toegevoegd zoals door uw functies wordt vereist. Met hosten [zonder server](https://azure.microsoft.com/overview/serverless-computing/) betaalt u alleen voor de tijd dat uw functies actief zijn. Wanneer u in een App Service-plan uitvoert, moet u het [Schalen van uw functie-app](../articles/azure-functions/functions-scale.md) beheren.  |
    | **Locatie** | Europa -west | Kies een [regio](https://azure.microsoft.com/regions/) in de buurt of in de buurt van andere services die door uw functie worden gebruikt. |
    | **[Opslagaccount](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Wereldwijd unieke naam |  Naam van het nieuwe opslagaccount dat door uw functie-app wordt gebruikt. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](../articles/azure-functions/functions-scale.md#storage-account-requirements). |

3. Selecteer **Maken** om de functie-app in te richten en te implementeren.

4. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**. 

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-create-function-app-portal/function-app-create-notification.png)

5. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken.

> [!TIP]
> Als u problemen ondervindt bij het vinden van de functie-app in de portal, [voegt u functie-apps toe aan uw favorieten in de Azure-portal](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#favorite).   
