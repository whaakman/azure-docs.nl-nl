---
title: SQL-databases beschikbaar maken voor de gebruikers van uw Azure-Stack | Microsoft Docs
description: Zelfstudie voor het installeren van de SQL Server-resourceprovider en maken biedt die, kunnen gebruikers van de Stack Azure SQL-databases maken.
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
ms.date: 06/05/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: b9ba2bb89bb0d7e16a28a165cf14530a7a10f71b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234747"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Zelfstudie: SQL-databases beschikbaar te maken voor uw Azure-Stack-gebruikers

Als de beheerder van een Azure-Stack cloud kunt u aanbiedingen die uw gebruikers laten maken (tenants) maken van SQL-databases die ze met hun cloud-systeemeigen apps, websites en werkbelastingen gebruiken kunnen. Dankzij deze aangepaste, op verzoek, cloud-gebaseerde databases aan uw gebruikers, kunt u ze bespaart tijd en bronnen. Dit als u wilt instellen, kun je het:

> [!div class="checklist"]
> * De resource-provider voor SQL Server implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

## <a name="deploy-the-sql-server-resource-provider"></a>De resource-provider voor SQL Server implementeren

Het implementatieproces is beschreven in de [Gebruik SQL-databases op Azure-Stack artikel](azure-stack-sql-resource-provider-deploy.md), en bestaat uit de volgende primaire stappen:

1. [Implementeren van de SQL-resourceprovider](azure-stack-sql-resource-provider-deploy.md).
2. [Controleer of de implementatie](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Een capaciteit door verbinding te maken met een hosting SQL-server. Zie voor meer informatie [toevoegen die als host fungeert voor servers](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Een aanbieding maken

1.  [Een quotum](azure-stack-setting-quotas.md) en noem deze *SQLServerQuota*. Selecteer **Microsoft.SQLAdapter** voor de **Namespace** veld.
2.  [Maak een plan](azure-stack-create-plan.md). Naam *TestSQLServerPlan*, selecteer de **Microsoft.SQLAdapter** service, en **SQLServerQuota** quotum.

    > [!NOTE]
    > Als gebruikers wilt laten maken van andere apps, andere services vereist zijn in het plan. Azure Functions moet bijvoorbeeld de **Microsoft.Storage** service in het plan, terwijl Wordpress vereist **Microsoft.MySQLAdapter**.

3.  [Maken van een aanbieding](azure-stack-create-offer.md), naam **TestSQLServerOffer** en selecteer de **TestSQLServerPlan** plan.

## <a name="test-the-offer"></a>Testen van de aanbieding

Nu dat u de SQL Server-resourceprovider hebt geïmplementeerd en een aanbieding hebt gemaakt, u als een gebruiker aanmelden kunt, abonneren op de aanbieding en maak een database.

### <a name="subscribe-to-the-offer"></a>Abonneren op de aanbieding

1. Aanmelden bij de Azure-Stack-portal (https://portal.local.azurestack.external) als een tenant.
2. Selecteer **Neem een abonnement op** en voer vervolgens **TestSQLServerSubscription** onder **weergavenaam**.
3. Selecteer **selecteert u een aanbieding** > **TestSQLServerOffer** > **maken**.
4. Selecteer **meer services** > **abonnementen** > **TestSQLServerSubscription** > **Resource providers**.
5. Selecteer **registreren** naast de **Microsoft.SQLAdapter** provider.

### <a name="create-a-sql-database"></a>Een SQL Database maken

1. Selecteer **+**  >  **gegevens en opslag** > **SQL-Database**.
2. Behoud de standaardwaarden of het gebruik van deze voorbeelden voor de volgende velden:
    - **Databasenaam**: SQLdb
    - **Maximale grootte in MB**: 100
    - **Abonnement**: TestSQLOffer
    - **Resourcegroep**: SQL-RG
3. Selecteer **aanmeldingsinstellingen**, geef referenties op voor de database en selecteer vervolgens **OK**.
4. Selecteer **SKU** > Selecteer de SQL-SKU die u hebt gemaakt voor de SQL Server die als host fungeert > en selecteer vervolgens **OK**.
5. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De resource-provider voor SQL Server implementeren
> * Een aanbieding maken
> * Testen van de aanbieding

Voorafgaande aan de volgende zelfstudie voor meer informatie over hoe:

> [!div class="nextstepaction"]
> [Web-, mobiele en API-apps beschikbaar te maken voor uw gebruikers]( azure-stack-tutorial-app-service.md)