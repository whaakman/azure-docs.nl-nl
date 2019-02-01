---
title: SQL-databases beschikbaar maken voor uw Azure Stack-gebruikers | Microsoft Docs
description: Zelfstudie voor het installeren van de resourceprovider van SQL Server en maak biedt waarmee SQL-databases maken van Azure Stack-gebruikers.
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
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 11/05/2018
ms.custom: mvc
ms.openlocfilehash: 983e8b279261d3ff8e5d24c8e3a6f61c5a787e5b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240808"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Zelfstudie: SQL-databases beschikbaar te maken voor uw Azure Stack-gebruikers

Als de beheerder van een Azure Stack-cloud, kunt u aanbiedingen waarmee uw gebruikers kunnen maken (tenants) SQL-databases die ze met hun cloud-eigen apps, websites en werkbelastingen gebruiken kunnen maken. Door op te geven deze databases aangepaste, op aanvraag, op basis van cloud naar uw gebruikers, kunt u ze Bespaar tijd en resources. Dit als u wilt instellen, u het volgende doen:

> [!div class="checklist"]
> * De SQL Server-resourceprovider implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

## <a name="deploy-the-sql-server-resource-provider"></a>De SQL Server-resourceprovider implementeren

Het implementatieproces wordt uitgebreid beschreven in de [Gebruik SQL-databases op Azure Stack-artikel](azure-stack-sql-resource-provider-deploy.md), en bestaat uit de volgende primaire stappen uit:

1. [De SQL-resourceprovider implementeren](azure-stack-sql-resource-provider-deploy.md).
2. [Controleer of de implementatie](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Geef capaciteit door verbinding te maken met een host SQL-server. Zie voor meer informatie, [toevoegen die als host fungeert voor servers](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Een aanbieding maken

1.  [Definieert een quotum op](azure-stack-setting-quotas.md) en noem het *SQLServerQuota*. Selecteer **Microsoft.SQLAdapter** voor de **Namespace** veld.
2.  [Maak een plan](azure-stack-create-plan.md). Geef het de naam *TestSQLServerPlan*, selecteer de **Microsoft.SQLAdapter** -service, en **SQLServerQuota** quotum.

    > [!NOTE]
    > Als u wilt dat gebruikers andere apps te maken, andere services vereist zijn in het plan. Bijvoorbeeld: Azure Functions heeft de **Microsoft.Storage** service in het abonnement, terwijl Wordpress vereist **Microsoft.MySQLAdapter**.

3.  [Maak een aanbieding](azure-stack-create-offer.md), geef deze de naam **TestSQLServerOffer** en selecteer de **TestSQLServerPlan** plan.

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u de resourceprovider van SQL Server hebt geïmplementeerd en een aanbieding hebt gemaakt, u als een gebruiker aanmelden kunt, Abonneer u op de aanbieding en maak een database.

### <a name="subscribe-to-the-offer"></a>Abonneer u op de aanbieding

1. Aanmelden bij de Azure Stack-portal (https://portal.local.azurestack.external) als een tenant.
2. Selecteer **Neem een abonnement** en voer vervolgens **TestSQLServerSubscription** onder **weergavenaam**.
3. Selecteer **selecteert u een aanbieding** > **TestSQLServerOffer** > **maken**.
4. Selecteer **alle services** > **abonnementen** > **TestSQLServerSubscription** > **Resource providers**.
5. Selecteer **registreren** naast de **Microsoft.SQLAdapter** provider.

### <a name="create-a-sql-database"></a>Een SQL-database maken

1. Selecteer **+**  >  **gegevens en opslag** > **SQL-Database**.
2. Behoud de standaardwaarden of het gebruik van deze voorbeelden voor de volgende velden:
    - **Databasenaam**: SQLdb
    - **Maximale grootte in MB**: 100
    - **Abonnement**: TestSQLOffer
    - **Resourcegroep**: SQL-RG
3. Selecteer **Login Settings**, geef referenties op voor de database en selecteer vervolgens **OK**.
4. Selecteer **SKU** > Selecteer de SQL-SKU die u hebt gemaakt voor de SQL-Server die als host fungeert > en selecteer vervolgens **OK**.
5. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De SQL Server-resourceprovider implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

Ga verder met de volgende zelfstudie voor meer informatie over het:

> [!div class="nextstepaction"]
> [Web, mobiele Apps en API-apps beschikbaar te maken voor uw gebruikers]( azure-stack-tutorial-app-service.md)