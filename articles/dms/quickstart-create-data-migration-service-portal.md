---
title: Azure-Database migratieservice-exemplaar met de Azure portal maken | Microsoft Docs
description: De Azure portal gebruiken voor het maken van een exemplaar van de Service Azure Database migreren
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/08/2017
ms.openlocfilehash: 4213a0ae73c38dcad403140e3d45e28f2d338020
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-database-migration-service-instance-using-the-azure-portal"></a>Maak een Database migratie-Service-exemplaar met de Azure portal
In deze snel starten gebruikt u de Azure portal een exemplaar van de migratie van Azure databaseservice te maken.  Nadat u de service hebt gemaakt, kunt u zich kunt gebruiken om te migreren van gegevens uit SQL Server on-premises naar een Azure SQL database.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Open uw webbrowser en ga naar de [Microsoft Azure Portal](https://portal.azure.com/). Voer uw referenties in om u aan te melden bij de portal. De standaardweergave is uw service-dashboard.

## <a name="create-azure-database-migration-service"></a>Migratie-Service van Azure-Database maken
1. Klik op  **+**  een nieuwe service maken.  Migratie-database-Service is nog in preview.  

1. Zoek de marketplace 'migreren', 'Database migratie Service (preview)' selecteren en klik op **maken**.

    ![Migratieservice maken](media/quickstart-create-data-migration-service-portal/dms-create-service.png)

    - Kies een **servicenaam** die gemakkelijk te onthouden en uniek is voor het identificeren van uw Azure-Database migratie Service-exemplaar is.
    - Selecteer uw Azure **abonnement** in die u wilt maken van de Database-Service voor migratie.
    - Maak een nieuwe **netwerk** met een unieke naam.
    - Kies de **locatie** die zich het dichtst bij de bron- of -server.
    - Selecteer Basic: 1 vCore voor de **prijscategorie**.

1. Klik op **Create**.

Na enkele ogenblikken uw Migratieservice voor Azure-Database worden gemaakt en klaar voor gebruik.  U ziet de databaseservice migratie zoals weergegeven in de afbeelding.

![Migratieservice gemaakt](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Resources opschonen
U kunt opschonen van de resources die u in de Quick Start hebt gemaakt door het verwijderen van de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md).  Als u wilt verwijderen van de resourcegroep, navigeer naar de Database migratie-Service die u hebt gemaakt, klikt u op de **resourcegroep** een naam geven en selecteer vervolgens **resourcegroep verwijderen**.  Deze actie verwijdert alle activa in de resourcegroep, evenals de groep zelf.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Migreren van SQL Server on-premises naar Azure SQL-database](tutorial-sql-server-to-azure-sql.md)