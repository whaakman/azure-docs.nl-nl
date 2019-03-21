---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 6c430f22a9d4fa0fad95bcaa41675545fffd91ec
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227215"
---
De Azure Functions-projectsjabloon in Visual Studio maakt een project dat kan worden gepubliceerd in een functie-app in Azure. U kunt een functie-app groepsfuncties gebruiken als een logische eenheid voor beheer, implementatie en het delen van resources.

1. In Visual Studio op de **bestand** in het menu **nieuw** > **Project**.

2. In de **nieuw Project** in het dialoogvenster, selecteer **geïnstalleerde** > **Visual C#**   >  **Cloud**  >  **Azure Functions**. Voer een naam voor uw project en selecteer **OK**. De functie-appnaam moet geldig zijn als een C#-naamruimte. Gebruik dus geen onderstrepingstekens, afbreekstreepjes of andere niet-alfanumerieke tekens.

    ![Dialoogvenster Nieuw Project te maken van een functie in Visual Studio](./media/functions-vstools-create/functions-vs-new-project.png)

3. Gebruik de instellingen die zijn weergegeven in de tabel onder de afbeelding.

    ![Dialoogvenster nieuwe functie in Visual Studio](./media/functions-vstools-create/functions-vs-new-function.png) 

    | Instelling      | Voorgestelde waarde  | Description                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Versie** | Azure Functions 2.x <br />(.NET Core) | Deze instelling maakt een functieproject die gebruikmaakt van de runtime versie 2.x van Azure Functions, die ondersteuning biedt voor .NET Core. Azure Functions 1.x ondersteunt .NET Framework. Zie voor meer informatie, [doel Azure Functions runtime-versie](../articles/azure-functions/functions-versions.md).   |
    | **Sjabloon** | HTTP-trigger | Deze instelling maakt u een functie geactiveerd door een HTTP-aanvraag. |
    | **Opslagaccount**  | Opslagemulator | Een HTTP-trigger gebruikt niet de verbinding van Azure Storage-account. Voor alle andere triggertypen is er een geldige verbindingsreeks voor het opslagaccount nodig. |
    | **Toegangsrechten** | Anoniem | De gemaakte functie kan door iedere client worden geactiveerd zonder een sleutel op te geven. Met deze autorisatie-instelling kunt u eenvoudig uw nieuwe functie testen. Zie [Autorisatiesleutels](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) in de [HTTP- en webhookbindingen](../articles/azure-functions/functions-bindings-http-webhook.md) voor meer informatie over sleutels en autorisatie. |
    
    > [!NOTE]
    > Zorg ervoor dat u de **toegangsrechten** naar `Anonymous`. Als u ervoor het standaardniveau van kiest `Function`, u verplicht bent om weer te geven de [functietoets](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) in aanvragen voor toegang tot uw functie-eindpunt.
    
4. Selecteer **OK** de function-project en HTTP-geactiveerde functie te maken.
