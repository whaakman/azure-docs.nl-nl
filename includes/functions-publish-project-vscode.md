---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4f3d31fa25ea5781bc3af7297719b77723597e17
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444606"
---
## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren

In Visual Studio Code kunt u uw functieproject rechtstreeks in Azure publiceren. In dit proces maakt u een functie-app en de bijbehorende resources in uw Azure-abonnement. De functie-app biedt een context waar u uw functies kunt uitvoeren. Het project wordt in uw Azure-abonnement verpakt en geïmplementeerd in de nieuwe functie-app.

Visual Studio maakt standaard alle van de Azure-resources vereist voor het maken van uw functie-app. De namen van deze resources zijn gebaseerd op de functie-app die u kiest. Als u hebben volledig beheer van de gemaakte resources wilt, kunt u in plaats daarvan [publiceren met behulp van geavanceerde opties](../articles/azure-functions/functions-develop-vs-code.md#enabled-publishing-with-advanced-create-options).

In deze sectie wordt ervan uitgegaan dat u een nieuwe functie-app in Azure maakt.

> [!IMPORTANT]
> Als u in een bestaande functie-app publiceert, wordt de inhoud van die app in Azure overschreven.

1. Druk op F1 voor het opdrachtenpalet te openen in Visual Studio Code. In het opdrachtenpalet, zoek en selecteer `Azure Functions: Deploy to function app...`.

1. Als niet aangemeld, u wordt gevraagd te **aanmelden bij Azure**. U kunt ook **maken van een gratis Azure-account**. Na geslaagde zich heeft aangemeld vanuit de browser, Ga terug naar Visual Studio Code. 

1. Als u meerdere abonnementen hebt **Selecteer een abonnement** voor de functie-app, kies **+ nieuwe functie-App maken in Azure**.

1. Typ een unieke naam waarmee de functie-app wordt geïdentificeerd en druk op Enter. Geldige tekens voor de naam van en functie-app zijn `a-z`, `0-9` en `-`.

    Wanneer u op Enter drukt, worden de volgende Azure-resources in uw abonnement gemaakt:

    * **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** : Bevat alle van de gemaakte Azure-resources. De naam is gebaseerd op de naam van uw functie-app.
    * **[Storage-account](../articles/storage/common/storage-quickstart-create-account.md)** : Een standard Storage-account wordt gemaakt met een unieke naam die is gebaseerd op de naam van uw functie-app.
    * **[Hostingabonnement](../articles/azure-functions/functions-scale.md)** : Een verbruiksabonnement wordt gemaakt in de regio VS-West voor het hosten van uw serverloze functie-app.
    * **Functie-app**: Uw project wordt geïmplementeerd op en wordt uitgevoerd in deze nieuwe functie-app.

    Nadat de functie-app is gemaakt en het implementatiepakket is toegepast, wordt er een melding weergegeven. Selecteer **weergave uitvoer** in deze melding om het maken en de Implementatieresultaten weer te geven, met inbegrip van de Azure-resources die u hebt gemaakt.

1. Klik in de **Azure: Functies** gebied, vouw de nieuwe functie-app in uw abonnement. Vouw **functies**, met de rechtermuisknop op **HttpTrigger**, en kies vervolgens **functie-URL kopiëren**.

    ![Kopieer de functie-URL voor de nieuwe HTTP-trigger](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
