---
title: Web apps en API apps beschikbaar te maken voor uw Azure Stack-gebruikers | Microsoft Docs
description: Zelfstudie voor het installeren van de resourceprovider App Service en maak biedt die uw Azure Stack-gebruikers bieden de mogelijkheid te maken van web apps en API apps.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 0a9b87ccfd49ba04a8dff8ef48bea023ff94b222
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340714"
---
# <a name="tutorial-make-web-and-api-apps-available-to-your-azure-stack-users"></a>Zelfstudie: web-apps en API-apps beschikbaar te maken voor uw Azure Stack-gebruikers

Als de beheerder van een Azure Stack-cloud, kunt u aanbiedingen waarmee uw gebruikers kunnen maken (tenants) maken van Azure Functions- en web- en API-toepassingen. Door uw gebruikers toegang geven tot deze op aanvraag, op basis van cloud-apps, kunt u ze besparen tijd en resources.

Dit als u wilt instellen, u het volgende doen:

> [!div class="checklist"]
> * De App Service-resourceprovider implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

## <a name="deploy-the-app-service-resource-provider"></a>De App Service-resourceprovider implementeren

1. [Voorbereiden van de host van de Azure Stack Development Kit](azure-stack-app-service-before-you-get-started.md). Dit omvat de resourceprovider van SQL Server, die vereist is voor het maken van sommige apps implementeren.
2. [Het installatieprogramma en helper scripts downloaden](azure-stack-app-service-deploy.md).
3. [Voer het script helper voor het maken van de vereiste certificaten](azure-stack-app-service-deploy.md).
4. [Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md) (het duurt een paar uur te installeren en voor alle worker-rollen worden weergegeven.)
5. [De installatie valideren](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Een aanbieding maken

U kunt bijvoorbeeld een aanbieding waarmee gebruikers kunnen maken van DNN web content management systems maken. De SQL Server-service die u al ingeschakeld door het installeren van de resourceprovider van SQL Server vereist.

1.  [Definieert een quotum op](azure-stack-setting-quotas.md) en noem het *AppServiceQuota*. Selecteer **Microsoft.Web** voor de **Namespace** veld.
2.  [Maak een plan](azure-stack-create-plan.md). Geef het de naam *TestAppServicePlan*, selecteer de **Microsoft.SQL** service en de **AppService quotum** quotum.

    > [!NOTE]
    > Als u wilt dat gebruikers andere apps te maken, andere services vereist zijn in het plan. Bijvoorbeeld: Azure Functions heeft de **Microsoft.Storage** service in het abonnement, terwijl Wordpress vereist **Microsoft.MySQL**.

3.  [Maak een aanbieding](azure-stack-create-offer.md), geef deze de naam **TestAppServiceOffer** en selecteer de **TestAppServicePlan** plan.

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u de App Service-resourceprovider hebt geïmplementeerd en een aanbieding hebt gemaakt, kunt u aanmelden als een gebruiker, Abonneer u op de aanbieding, en een app maken.

In dit voorbeeld maken we een DNN Platform content management-systeem. U maakt eerst een SQL-database en vervolgens de DNN web-app.

### <a name="subscribe-to-the-offer"></a>Abonneer u op de aanbieding

1. Aanmelden bij de Azure Stack-portal (https://portal.local.azurestack.external) als een tenant.
2. Selecteer **Neem een abonnement** >, voer **TestAppServiceSubscription** onder **weergavenaam** > **selecteert u een aanbieding**  >  **TestAppServiceOffer** > **maken**.

### <a name="create-a-sql-database"></a>Een SQL-database maken

1. Selecteer **+**  >  **gegevens en opslag** > **SQL-Database**.
2. Behoud de standaardwaarden, met uitzondering van de volgende velden:

    - **Databasenaam**: DNNdb
    - **Maximale grootte in MB**: 100
    - **Abonnement**: TestAppServiceOffer
    - **Resourcegroep**: DNN-RG

3. Selecteer **Login Settings**, geef referenties op voor de database en selecteer vervolgens **OK**. U gebruikt deze referenties verderop in deze zelfstudie.
4. Onder **SKU** > Selecteer de SQL-SKU die u hebt gemaakt voor de SQL-Server die als host fungeert > en selecteer vervolgens **OK**.
5. Selecteer **Maken**.

### <a name="create-a-dnn-app"></a>Een DNN-app maken

1. Selecteer **+**  >  **alle** > **DNN Platform preview** > **maken** .
2. Voer *DNNapp* onder **appnaam** en selecteer **TestAppServiceOffer** onder **abonnement**.
3. Selecteer **vereiste instellingen configureren** > **nieuw** > in te voeren een **App Service-plan** naam.
4. Selecteer **prijscategorie** > **F1 gratis** > **Selecteer** > **OK**.
5. Selecteer **Database** en voer de referenties voor de SQL-database die u eerder hebt gemaakt.
6. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De App Service-resourceprovider implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

Ga verder met de volgende zelfstudie voor meer informatie over het:

> [!div class="nextstepaction"]
> [Apps implementeren op Azure en Azure Stack](user/azure-stack-solution-pipeline.md)
