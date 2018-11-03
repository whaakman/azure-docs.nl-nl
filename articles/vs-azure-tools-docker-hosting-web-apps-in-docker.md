---
title: Een ASP.NET-Docker-container implementeren naar Azure Container Registry (ACR) | Microsoft Docs
description: Informatie over het gebruik van Visual Studio Tools voor Docker een ASP.NET Core web-app implementeren naar een containerregister
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
ms.openlocfilehash: 2170fb7eebedcaddb91a4a24940b1e226c104f2c
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969270"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Een ASP.NET-container implementeren naar een containerregister met behulp van Visual Studio
## <a name="overview"></a>Overzicht
Docker is een lichtgewicht container-engine, vergelijkbare enkele manieren om een virtuele machine, kunt u hosting van toepassingen en services.
Deze zelfstudie leert u hoe u via uw containertoepassing te publiceren met behulp van Visual Studio een [Azure Container Registry](https://azure.microsoft.com/services/container-registry).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Vereisten voor het voltooien van deze zelfstudie:

* Installeer de nieuwste versie van [Visual Studio 2017](https://azure.microsoft.com/downloads/) met de workload 'ASP.NET en web-ontwikkeling'
* Installeer [Docker voor Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Een ASP.NET Core-web-app maken
De volgende stappen helpen u bij het maken van een eenvoudige ASP.NET Core-app die wordt gebruikt in deze zelfstudie.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Publiceren van de container naar Azure Container Registry
1. Met de rechtermuisknop op uw project in **Solution Explorer** en kies **publiceren**.
2. Selecteer in het dialoogvenster publiceren doel de **Container Registry** tabblad.
3. Kies **nieuwe Azure Container Registry** en klikt u op **publiceren**.
4. Vul de gewenste waarden in de **maken van een nieuwe Azure Container Registry**.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS-voorvoegsel** | Wereldwijd unieke naam | De naam die een unieke identificatie van uw containerregister. |
    | **Abonnement** | Kies uw abonnement | Het te gebruiken Azure-abonnement. |
    | **[Resourcegroep](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  De naam van de resourcegroep waarin u wilt maken van uw containerregister. Kies **Nieuw** om een nieuwe resourcegroep te maken.|
    | **[SKU](https://docs.microsoft.com/azure/container-registry/container-registry-skus)** | Standard | De servicelaag van het containerregister  |
    | **Registerlocatie** | Een locatie dicht bij u | Kies een locatie in een [regio](https://azure.microsoft.com/regions/) buurt of in de buurt van andere services die uw container registry gebruikt. |
    ![Visual Studio maken Azure Container Registry-dialoogvenster][0]
5. Klik op **Maken**.

U kunt nu ophalen van de container uit het register op een willekeurige host staat van het uitvoeren van Docker-installatiekopieën, bijvoorbeeld [Azure Container Instances](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
