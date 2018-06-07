---
title: Een ASP.NET-Docker-container implementeren naar Azure Container register (ACR) | Microsoft Docs
description: Informatie over het gebruik van Visual Studio Tools voor Docker voor het implementeren van een ASP.NET Core web-app met een container-register
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: ''
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: mlearned
ms.openlocfilehash: 4442c1d763f4ed21a5efeedbe957727254e2a0b8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658468"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Een ASP.NET-container implementeren in een container register met Visual Studio
## <a name="overview"></a>Overzicht
Docker is een lichtgewicht container-engine, qua enkele manieren voor een virtuele machine, kunt u hosttoepassingen en services.
Deze zelfstudie leert u uw beperkte toepassing publiceren met behulp van Visual Studio een [Azure Container register](https://azure.microsoft.com/en-us/services/container-registry).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Vereisten voor het voltooien van deze zelfstudie:

* Installeer de nieuwste versie van [Visual Studio 2017](https://azure.microsoft.com/en-us/downloads/) met de werkbelasting 'ASP.NET en web-ontwikkeling'
* Installeer [Docker voor Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Een ASP.NET Core-web-app maken
De volgende stappen helpen u bij het maken van een eenvoudige ASP.NET Core-app die wordt gebruikt in deze zelfstudie.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. De container in Azure Container register publiceren
1. Met de rechtermuisknop op het project in **Solution Explorer** en kies **publiceren**.
2. Selecteer in het dialoogvenster publish-doel de **Container register** tabblad.
3. Kies **nieuwe Azure-Container register** en klik op **publiceren**.
4. Vul in de gewenste waarden in de **maken van een nieuwe Azure-Container register**.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS-voorvoegsel** | Wereldwijd unieke naam | De naam die een unieke identificatie van het register van de container. |
    | **Abonnement** | Kies uw abonnement | Het te gebruiken Azure-abonnement. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Naam van de resourcegroep waarin u het register van de container maken. Kies **nieuw** om een nieuwe resourcegroep te maken.|
    | **[SKU](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)** | Standard | Servicelaag of het register van de container  |
    | **Registerlocatie** | Een locatie dicht bij u | Kies een locatie in een [regio](https://azure.microsoft.com/regions/) in de buurt of in de buurt van andere services die door het register van de container wordt gebruikt. |
    ![Visual Studio maken Azure Container register dialoogvenster][0]
5. Klik op **Maken**.

U kunt nu ophalen van de container uit het register op een willekeurige host Docker-installatiekopieën, bijvoorbeeld uitgevoerd [Azure Containerexemplaren](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
