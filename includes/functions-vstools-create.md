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
ms.openlocfilehash: 17b7626f79d7d356e3e8f3440e4a6526f2df776d
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68593831"
---
De Azure Functions-projectsjabloon in Visual Studio maakt een project dat kan worden gepubliceerd in een functie-app in Azure. U kunt een functie-app gebruiken om functies te groeperen als een logische eenheid voor beheer, implementatie en delen van resources.

1. Selecteer in Visual Studio in het menu **File** de optie **New** > **project**.

1. Zoek  `functions`in het dialoog venster **een nieuw project maken** naar, kies de Azure functions sjabloon en selecteer **volgende**.

1. Voer een naam in voor het project en selecteer **maken**. De functie-appnaam moet geldig zijn als een C#-naamruimte. Gebruik dus geen onderstrepingstekens, afbreekstreepjes of andere niet-alfanumerieke tekens.

1. Gebruik in **een nieuwe Azure functions-toepassing maken**de volgende opties:

    + **Azure functions v2 (.net core)** 1
    + **HTTP-trigger**
    + **Opslag account**: **Opslag emulator**
    + **Autorisatie niveau**: **Toegang** 

    | Optie      | Voorgestelde waarde  | Description                      |
    | ------------ |  ------- |----------------------------------------- |
    | Functions runtime | **Azure functions 2. x <br />(.net core)** | Met deze instelling maakt u een functie project dat gebruikmaakt van versie 2. x runtime van Azure Functions, dat .NET core ondersteunt. Azure Functions 1.x ondersteunt .NET Framework. Zie [doel-Azure functions runtime-versie](../articles/azure-functions/functions-versions.md)voor meer informatie.   |
    | Functie sjabloon | **HTTP-trigger** | Met deze instelling maakt u een functie die wordt geactiveerd door een HTTP-aanvraag. |
    | **Opslagaccount**  | **Opslag emulator** | Een HTTP-trigger maakt geen gebruik van de Azure Storage-account verbinding. Voor alle andere triggertypen is er een geldige verbindingsreeks voor het opslagaccount nodig. Omdat voor functies een opslag account vereist is, wordt er een toegewezen of gemaakt wanneer u uw project naar Azure publiceert. |
    | **Verificatieniveau** | **Toegang** | De gemaakte functie kan door iedere client worden geactiveerd zonder een sleutel op te geven. Met deze autorisatie-instelling kunt u eenvoudig uw nieuwe functie testen. Zie [Autorisatiesleutels](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) in de [HTTP- en webhookbindingen](../articles/azure-functions/functions-bindings-http-webhook.md) voor meer informatie over sleutels en autorisatie. |
    
    > [!NOTE]
    > Zorg ervoor dat u het **autorisatie niveau** instelt `Anonymous`op. Als u het standaard niveau van `Function`kiest, moet u de [functie sleutel](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) in aanvragen voor toegang tot uw functie-eind punt presen teren.
    
4. Selecteer **maken** om het functie project en de http-geactiveerde functie te maken.
