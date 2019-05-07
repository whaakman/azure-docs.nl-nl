---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1cecafb013b5d22557961e98935fc9a2fee2a79b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65198989"
---
1. Selecteer de **een resource maken** knop gevonden in de linkerbovenhoek van Azure portal, klikt u vervolgens selecteren **Compute** > **functie-App**.

    ![Een functie-app maken in Azure Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Gebruik voor de functie-app de instellingen die in de tabel onder de afbeelding zijn opgegeven.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Instelling      | Voorgestelde waarde  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. | 
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. |
    | **Besturingssysteem** | Windows | Hosten zonder server op Linux is momenteel in preview. Zie voor meer informatie, [in dit artikel overwegingen met betrekking tot](https://aka.ms/funclinux).|
    | **[Hostingplan](../articles/azure-functions/functions-scale.md)** | Verbruiksabonnement | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. In het standaard **Verbruiksabonnement** worden resources dynamisch toegevoegd zoals door uw functies wordt vereist. Met hosten [zonder server](https://azure.microsoft.com/overview/serverless-computing/) betaalt u alleen voor de tijd dat uw functies actief zijn. Wanneer u in een App Service-plan uitvoert, moet u het [Schalen van uw functie-app](../articles/azure-functions/functions-scale.md) beheren.  |
    | **Locatie** | Europa -west | Kies een [regio](https://azure.microsoft.com/regions/) in de buurt of in de buurt van andere services die door uw functie worden gebruikt. |
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. |
    | **[Opslag](../articles/storage/common/storage-quickstart-create-account.md)** |  Wereldwijd unieke naam |  Maak een opslagaccount die wordt gebruikt door uw functie-app. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Standaard | Hiermee maakt u een Application Insights-resource met dezelfde *appnaam* in de dichtstbijzijnde ondersteunde regio bevindt. Door het uitbreiden van deze instelling kunt u de **nieuwe resourcenaam** of kies een andere **locatie** in een [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) waar u uw gegevens kunt opslaan. |

3. Selecteer **Maken** om de functie-app in te richten en te implementeren.

4. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/functions-create-function-app-portal/function-app-create-notification.png)

5. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken.
