---
title: Een Azure Database Migration Service-exemplaar maken met Azure Portal | Microsoft Docs
description: Azure Portal gebruiken om een Azure Database Migration Service-exemplaar te maken
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 12/13/2017
ms.openlocfilehash: 9dea80b0a6848bd69541aa9f7e0a0fe111fa0a28
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Een Azure Database Migration Service-exemplaar maken met behulp van Azure Portal
In deze snelstartgids maakt u gebruik van Azure Portal om een Azure Database Migration Service-exemplaar te maken.  Nadat u de service hebt gemaakt, kunt u deze gebruiken om gegevens vanuit een on-premises SQL-server te migreren naar een Azure SQL-database.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Open de webbrowser, navigeer naar [Microsoft Azure Portal](https://portal.azure.com/), en voer vervolgens uw referenties in om u aan te melden bij de portal.

De standaardweergave is uw service-dashboard.

## <a name="register-the-resource-provider"></a>De resourceprovider registreren
Registreer de Microsoft.DataMigration-resourceprovider voordat u uw eerste Database Migration Service-exemplaar maakt.

1. Selecteer in Azure Portal de optie **Alle services** en vervolgens **Abonnementen**.

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

3. Zoek naar 'migration' en selecteer rechts van Microsoft.DataMigration de optie **Registreren**.

![Resourceprovider registreren](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Een exemplaar van de service maken
1. Klik op **+ Een resource maken** om een exemplaar te maken van de Azure Database Migration Service, die zich momenteel in preview bevindt.

2. Zoek in de marketplace naar 'migration', selecteer **Azure Database Migration Service** en klik vervolgens in het scherm **Azure Database Migration Service (Preview)** op **Maken**.

3. Ga in het scherm **Database Migration Service** als volgt te werk: 

    - Kies een unieke en gemakkelijk te onthouden **Servicenaam** om uw Azure Database Migration Service-exemplaar te identificeren.
    - Selecteer het Azure-**abonnement** waarin u het exemplaar wilt maken.
    - Maak een nieuw **netwerk** met een unieke naam.
    - Kies de **locatie** die zich het dichtst bij uw bron- of doelserver bevindt.
    - Selecteer Basis: 1 vCore als **prijscategorie**.

    ![Migratieservice maken](media/quickstart-create-data-migration-service-portal/dms-create-service.png)
4. Selecteer **Maken**.

Na enkele ogenblikken is uw Azure Database Migration Service-exemplaar gemaakt en klaar voor gebruik. De Database Migration Service wordt weergegeven zoals in de volgende afbeelding:

![Migratieservice gemaakt](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Resources opschonen
Alle resources die u in deze snelstartgids hebt gemaakt, kunt u verwijderen door de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) te verwijderen.  U verwijdert de resourcegroep door te navigeren naar het Azure Database Migration Service-exemplaar dat u hebt gemaakt. Selecteer bij **Resourcegroep** de naam van de resourcegroep en selecteer vervolgens **Resourcegroep verwijderen**.  Door deze actie worden alle items in de resourcegroep verwijderd, evenals de groep zelf.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een on-premises SQL-server migreren naar Azure SQL Database](tutorial-sql-server-to-azure-sql.md)