---
title: SQL-databases beschikbaar maken voor de gebruikers van uw Azure-Stack | Microsoft Docs
description: Zelfstudie voor het installeren van de SQL Server-resourceprovider en maken biedt die, kunnen gebruikers van de Stack Azure SQL-databases maken.
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
ms.openlocfilehash: f774888ba3921d0688feddac669ed1dca4667441
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL-databases beschikbaar maken voor uw Azure-Stack-gebruikers

Als de beheerder van een Azure-Stack cloud kunt u aanbiedingen die uw gebruikers laten maken (tenants) maken van SQL-databases die ze met hun cloud-systeemeigen apps, websites en werkbelastingen gebruiken kunnen. Dankzij deze aangepaste, op verzoek, cloud-gebaseerde databases aan uw gebruikers, kunt u ze bespaart tijd en bronnen. Dit als u wilt instellen, kun je het:

> [!div class="checklist"]
> * De resource-provider voor SQL Server implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

## <a name="deploy-the-sql-server-resource-provider"></a>De resource-provider voor SQL Server implementeren

Het implementatieproces is beschreven in de [Gebruik SQL-databases op Azure-Stack artikel](azure-stack-sql-resource-provider-deploy.md), en bestaat uit de volgende primaire stappen:

1. [Implementeren van de SQL-resourceprovider]( azure-stack-sql-resource-provider-deploy.md#deploy-the-resource-provider).
2. [Controleer of de implementatie]( azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Een capaciteit door verbinding te maken met een hosting SQL-server.

## <a name="create-an-offer"></a>Een aanbieding maken

1.  [Een quotum](azure-stack-setting-quotas.md) en noem deze *SQLServerQuota*. Selecteer **Microsoft.SQLAdapter** voor de **Namespace** veld.
2.  [Maak een plan](azure-stack-create-plan.md). Naam *TestSQLServerPlan*, selecteer de **Microsoft.SQLAdapter** service, en **SQLServerQuota** quotum.

    > [!NOTE]
    > Als gebruikers wilt laten maken van andere apps, andere services vereist zijn in het plan. Bijvoorbeeld: Azure Functions is vereist dat het plan ook de **Microsoft.Storage** service gebruikt, terwijl Wordpress vereist **Microsoft.MySQLAdapter**.
    > 
    >

3.  [Maken van een aanbieding](azure-stack-create-offer.md), naam **TestSQLServerOffer** en selecteer de **TestSQLServerPlan** plan.

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u de SQL Server-resourceprovider hebt geïmplementeerd en een aanbieding hebt gemaakt, u als een gebruiker aanmelden kunt, abonneren op de aanbieding en maak een database.

### <a name="subscribe-to-the-offer"></a>Abonneren op de aanbieding
1. Meld u aan de Stack van Azure-portal (https://portal.local.azurestack.external) als een tenant.
2. Klik op **Neem een abonnement op** en typ vervolgens **TestSQLServerSubscription** onder **weergavenaam**.
3. Klik op **selecteert u een aanbieding** > **TestSQLServerOffer** > **maken**.
4. Klik op **meer services** > **abonnementen** > **TestSQLServerSubscription** > **Resource providers**.
5. Klik op **registreren** naast de **Microsoft.SQLAdapter** provider.

### <a name="create-a-sql-database"></a>Een SQL-database maken

1. Klik op  **+**   >  **gegevens en opslag** > **SQL-Database**.
2. Laat de standaardwaarden voor de velden of kunt u deze voorbeelden:
    - **Databasenaam**: SQLdb
    - **Maximale grootte in MB**: 100
    - **Abonnement**: TestSQLOffer
    - **Resourcegroep**: SQL-RG
3. Klik op **aanmeldingsinstellingen**, geef referenties op voor de database en klik vervolgens op **OK**.
4. Klik op **SKU** > Selecteer de SQL-SKU die u hebt gemaakt voor de SQL Server die als host fungeert > **OK**.
5. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De resource-provider voor SQL Server implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

Voorafgaande aan de volgende zelfstudie voor meer informatie over hoe:

> [!div class="nextstepaction"]
> [Web-, mobiele en API-apps beschikbaar te maken voor uw gebruikers]( azure-stack-tutorial-app-service.md)

