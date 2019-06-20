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
ms.openlocfilehash: ec0425ff2188ecf1816d5f5841394c8e32f301d2
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175832"
---
## <a name="publish-the-project-to-azure"></a>Het project naar Azure publiceren

In Visual Studio Code kunt u uw functieproject rechtstreeks in Azure publiceren. In dit proces maakt u een functie-app en de bijbehorende resources in uw Azure-abonnement. De functie-app biedt een context waar u uw functies kunt uitvoeren. Het project wordt in uw Azure-abonnement verpakt en geïmplementeerd in de nieuwe functie-app.

In dit artikel wordt ervan uitgegaan dat u een nieuwe functie-app maakt. 

> [!IMPORTANT]
> Als u in een bestaande functie-app publiceert, wordt de inhoud van die app in Azure overschreven.

1. In de **Azure: Functies** gebied, selecteer de implementeren naar functie-App-pictogram.

    ![Instellingen voor functie-app](./media/functions-publish-project-vscode/function-app-publish-project.png)

1. Als niet aangemeld, u wordt gevraagd te **aanmelden bij Azure**. U kunt ook **maken van een gratis Azure-account**. Na geslaagde zich heeft aangemeld vanuit de browser, Ga terug naar Visual Studio Code. 

1. Als u meerdere abonnementen hebt **Selecteer een abonnement** voor de functie-app, kies **+ nieuwe functie-App maken in Azure**.

1. Typ een unieke naam waarmee de functie-app wordt geïdentificeerd en druk op Enter. Geldige tekens voor de naam van en functie-app zijn `a-z`, `0-9` en `-`.

1. Kies **+ Create New Resource Group** (+ Nieuwe resourcegroep maken), typ de naam van een resourcegroep, bijvoorbeeld `myResourceGroup`, en druk op Enter. U kunt ook een bestaande resourcegroep gebruiken.

1. Kies **+Create New Storage Account**, typ een unieke naam voor het nieuwe opslagaccount dat door de functie-app wordt gebruikt en druk op Enter. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken.

1. Kies een locatie in een [regio](https://azure.microsoft.com/regions/) bij u in de buurt of in de buurt van andere services die door uw functies worden gebruikt.

    Wanneer u op Enter drukt, worden de volgende Azure-resources in uw abonnement gemaakt:

    * **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** : Bevat alle van de gemaakte Azure-resources. De naam is gebaseerd op de naam van uw functie-app.
    * **[Storage-account](../articles/storage/common/storage-quickstart-create-account.md)** : Een standard Storage-account wordt gemaakt met een unieke naam die is gebaseerd op de naam van uw functie-app.
    * **[Hostingabonnement](../articles/azure-functions/functions-scale.md)** : Een verbruiksabonnement wordt gemaakt in de regio VS-West voor het hosten van uw serverloze functie-app.
    * **Functie-app**: Uw project wordt geïmplementeerd op en wordt uitgevoerd in deze nieuwe functie-app.

    Nadat de functie-app is gemaakt en het implementatiepakket is toegepast, wordt er een melding weergegeven. Selecteer **weergave uitvoer** in deze melding om het maken en de Implementatieresultaten weer te geven, met inbegrip van de Azure-resources die u hebt gemaakt.

1. Klik in de **Azure: Functies** gebied, vouw de nieuwe functie-app in uw abonnement. Vouw **functies**, met de rechtermuisknop op **HttpTrigger**, en kies vervolgens **functie-URL kopiëren**.

    ![Kopieer de functie-URL voor de nieuwe HTTP-trigger](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
