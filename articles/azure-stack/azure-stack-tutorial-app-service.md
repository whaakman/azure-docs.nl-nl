---
title: Web- en API-apps beschikbaar te maken voor de gebruikers van uw Azure-Stack | Microsoft Docs
description: Zelfstudie voor het installeren van de App Service-resourceprovider en maken biedt die uw gebruikers Azure Stack bieden de mogelijkheid om web en API-apps te maken.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 7/03/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: 42d94eea8b9e4db611b821cd84e7d02f1d226293
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web- en API-apps beschikbaar te maken voor uw Azure-Stack-gebruikers

Als de beheerder van een Azure-Stack cloud kunt u aanbiedingen die uw gebruikers laten maken (tenants) Azure Functions- en web- en API-toepassingen maken. Dankzij de toegang tot deze op aanvraag, cloud-gebaseerde apps aan uw gebruikers, kunt u ze bespaart tijd en bronnen. Dit als u wilt instellen, kun je het:

> [!div class="checklist"]
> * Implementeert de bronprovider van App Service
> * Een aanbieding maken
> * Testen van de aanbieding

## <a name="deploy-the-app-service-resource-provider"></a>Implementeert de bronprovider van App Service

1. [Voorbereiden van de host Azure Stack Development Kit](azure-stack-app-service-before-you-get-started.md). Dit omvat de bronprovider van SQL Server die vereist is voor het maken van een aantal apps te implementeren.
2. [Download het installatieprogramma en helper scripts](azure-stack-app-service-deploy.md).
3. [Voer het script helper voor het maken van de vereiste certificaten](azure-stack-app-service-deploy.md).
4. [Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md) (het duurt een paar uur te installeren en voor alle worker-rollen worden weergegeven).
5. [De installatie valideren](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Een aanbieding maken

U kunt bijvoorbeeld een aanbieding waarmee gebruikers maken DNN web content management systems maken. De SQL Server-service die u al hebt ingeschakeld door het installeren van de resource-provider voor SQL Server vereist.

1.  [Een quotum](azure-stack-setting-quotas.md) en noem deze *AppServiceQuota*. Selecteer **Microsoft.Web** voor de **Namespace** veld.
2.  [Maak een plan](azure-stack-create-plan.md). Naam *TestAppServicePlan*, selecteer de **Microsoft.SQL** service, en **AppService quotum** quotum.

    > [!NOTE]
    > Als gebruikers wilt laten maken van andere apps, andere services vereist zijn in het plan. Bijvoorbeeld: Azure Functions is vereist dat het plan ook de **Microsoft.Storage** service gebruikt, terwijl Wordpress vereist **Microsoft.MySQL**.
    > 
    >

3.  [Maken van een aanbieding](azure-stack-create-offer.md), naam **TestAppServiceOffer** en selecteer de **TestAppServicePlan** plan.

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u de App Service-resourceprovider hebt geïmplementeerd en een aanbieding hebt gemaakt, u als een gebruiker aanmelden kunt, Abonneer u op de aanbieding en een app maken. In dit voorbeeld maakt u een inhoudsbeheersysteem DNN Platform. U moet eerst een SQL-database en vervolgens de DNN web-app maken.

### <a name="subscribe-to-the-offer"></a>Abonneren op de aanbieding
1. Meld u aan de Stack van Azure-portal (https://portal.local.azurestack.external) als een tenant.
2. Klik op **Neem een abonnement op** > type **TestAppServiceSubscription** onder **weergavenaam** > **selecteert u een aanbieding**  >  **TestAppServiceOffer** > **maken**.

### <a name="create-a-sql-database"></a>Een SQL-database maken

1. Klik op  **+**   >  **gegevens en opslag** > **SQL-Database**.
2. Laat de standaardwaarden voor de velden, behalve als volgt:
    - **Databasenaam**: DNNdb
    - **Maximale grootte in MB**: 100
    - **Abonnement**: TestAppServiceOffer
    - **Resourcegroep**: DNN RG
3. Klik op **aanmeldingsinstellingen**, geef referenties op voor de database en klik vervolgens op **OK**. U gebruikt deze referenties verderop in deze stappen.
4. Klik op **SKU** > Selecteer de SQL-SKU die u hebt gemaakt voor de SQL Server die als host fungeert > **OK**.
5. Klik op **Create**.

### <a name="create-a-dnn-app"></a>Een DNN-app maken    

1. Klik op  **+**   >  **alle** > **DNN Platform preview** > **maken**.
2. Type *DNNapp* onder **appnaam** en selecteer **TestAppServiceOffer** onder **abonnement**.
3. Klik op **vereiste instellingen configureren** > **nieuw** > type een **App Service-abonnement** naam.
4. Klik op **prijscategorie** > **F1 gratis** > **Selecteer** > **OK**.
5. Klik op **Database** en voer de gegevens voor de SQL-database die u eerder hebt gemaakt.
6. Klik op **Create**.

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Implementeert de bronprovider van App Service
> * Een aanbieding maken
> * Testen van de aanbieding

Voorafgaande aan de volgende zelfstudie voor meer informatie over hoe:

> [!div class="nextstepaction"]
> [Apps implementeren op Azure en Azure Stack](user/azure-stack-solution-pipeline.md)
