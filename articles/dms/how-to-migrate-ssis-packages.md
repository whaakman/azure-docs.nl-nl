---
title: Opnieuw implementeren van pakketten van SQL Server Integration Services naar Azure SQL Database | Microsoft Docs
description: Leer hoe u SQL Server Integration Services-pakketten migreren naar Azure SQL Database.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 603a9df8e3f499c832bbfdcbef966de86003d6b7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080641"
---
# <a name="redeploy-sql-server-integration-services-packages-to-azure-sql-database"></a>SQL Server Integration Services-pakketten naar Azure SQL Database implementeren

Als u SQL Server Integration Services (SSIS) te gebruiken en wilt migreren van uw SSIS-projecten /-pakketten van de bron SSISDB wordt gehost door de SQL Server naar de bestemming SSISDB wordt gehost door Azure SQL Database, kunt u opnieuw te implementeren met behulp van de Integration Services-implementatie De wizard. U kunt de wizard in in SQL Server Management Studio (SSMS) op te starten.

Als de versie van SSIS die u gebruikt, ouder dan 2012 is, voordat opnieuw implementeren van uw SSIS-projecten /-pakketten in het implementatiemodel van het project, moet u eerst converteren met behulp van de Wizard Integration Services-Project conversie, die ook vanuit een SSMS kunnen worden gestart. Zie voor meer informatie het artikel [projecten converteren naar het Projectimplementatiemodel](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure Database Migration Service (DMS) op dit moment niet ondersteunt de migratie van een bron SSISDB met een Azure SQL Database-server, maar u kunt opnieuw implementeren van uw SSIS-projecten /-pakketten via de volgende procedure.

In dit artikel leert u het volgende:
> [!div class="checklist"]
>
> * Bron SSIS-projecten /-pakketten te beoordelen.
> * SSIS-projecten /-pakketten migreren naar Azure.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze stappen hebt voltooid, hebt u het volgende nodig:

* SSMS 17.2 of hoger.
* Een instantie van uw doel-databaseserver host SSISDB. Als u dit niet al hebt, maakt u een Azure SQL Database-server (zonder een database) met behulp van de Azure-portal door te navigeren naar de SQL-Server (alleen logische server) [formulier](https://ms.portal.azure.com/#create/Microsoft.SQLServer).
* SSIS in Azure Data Factory (ADF) met Azure-SSIS Integration Runtime (IR) met de bestemming SSISDB wordt gehost door het exemplaar van Azure SQL Database-server moet worden ingericht (zoals beschreven in het artikel [inrichten van de Azure-SSIS-integratie Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)).

## <a name="assess-source-ssis-projectspackages"></a>Bron SSIS-projecten /-pakketten beoordelen

Tijdens de evaluatie van de bron SSISDB is nog niet geïntegreerd in de Database Migration Assistant (DMA) of de Azure Database Migration Service (DMS), uw SSIS-projecten /-pakketten worden beoordeeld/gevalideerd als ze zijn geïmplementeerd naar de bestemming SSISDB wordt gehost op een Azure SQL Database-server.

## <a name="migrate-ssis-projectspackages"></a>SSIS-projecten /-pakketten migreren

Als u wilt migreren SSIS-projecten /-pakketten naar Azure SQL Database-server, moet u de volgende stappen uitvoeren.

1. Open SSMS en selecteer vervolgens **opties** om weer te geven de **verbinding maken met Server** in het dialoogvenster.

2. Op de **aanmelding** tabblad, geeft u de informatie die nodig zijn voor het verbinding maken met de Azure SQL Database-server die als voor de bestemming SSISDB host.

    ![Tabblad SSIS-aanmelding](media/how-to-migrate-ssis-packages/dms-ssis-login-tab.png)

3. Op de **verbindingseigenschappen** tabblad, in de **verbinding maken met database** in het tekstvak selecteren of invoeren **SSISDB**, en selecteer vervolgens **Connect**.

    ![Tabblad Eigenschappen van de SSIS-verbinding](media/how-to-migrate-ssis-packages/dms-ssis-conncetion-properties-tab.png)

4. Vouw in Object Explorer met de SSMS, de **Integration Services-catalogussen** knooppunt uitvouwen **SSISDB**, en als er geen bestaande mappen, klik met de rechtermuisknop op **SSISDB** en Maak een nieuwe map.

5. Onder **SSISDB**uit, vouw een map, met de rechtermuisknop op **projecten**, en selecteer vervolgens **Project implementeren**.

    ![SSIS SSISDB knooppunt uitgevouwen](media/how-to-migrate-ssis-packages/dms-ssis-ssisdb-node-expanded.png)

6. In de Wizard Integration Services-implementatie op de **inleiding** pagina, lees de informatie en selecteer vervolgens **volgende**.

    ![Implementatie Wizard Introductiepagina](media/how-to-migrate-ssis-packages/dms-deployment-wizard-introduction-page.png)

7. Op de **bron selecteren** pagina, geeft u het bestaande SSIS-project dat u wilt implementeren.

    Als u SSMS ook is verbonden met de SQL-Server die als host fungeert voor de bron SSISDB, selecteert u **Integration Services-catalogus**, en voer het pad van de naam en project server in uw catalogus rechtstreeks implementeren van uw project.

    U kunt ook selecteren **implementatie projectbestand**, en geef het pad naar een bestaand project implementatiebestand (.ispac) voor het implementeren van uw project.

    ![Implementatiepagina-Wizard-bron selecteren](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-source-page.png)
 
8. Selecteer **Next**.
9. Op de **doel selecteren** pagina, geeft u de doellocatie voor uw project.

    a. Voer in het tekstvak Server, de volledig gekwalificeerde naam van de Azure SQL Database-server (< servernaam >. database.windows.net).

    b. Geef de verificatie-informatie, en selecteer vervolgens **Connect**.

    ![Implementatiepagina-Wizard-doel selecteren](media/how-to-migrate-ssis-packages/dms-deployment-wizard-select-destination-page.png)

    c. Selecteer **Bladeren** naar de doelmap opgeven in SSISDB, en selecteer vervolgens **volgende**.

    > [!NOTE]
    > De **volgende** knop wordt pas ingeschakeld nadat u hebt geselecteerd **Connect**.

10. Op de **valideren** pagina, eventuele fouten/waarschuwingen bekijken en indien nodig, wijzig vervolgens de pakketten dienovereenkomstig.

    ![Implementatie-pagina Wizard valideren](media/how-to-migrate-ssis-packages/dms-deployment-wizard-validate-page.png)

11. Selecteer **Next**.

12. Op de **bekijken** pagina, Controleer uw implementatie-instellingen.

    > [!NOTE]
    > U kunt uw instellingen wijzigen door te selecteren **vorige** of door het selecteren van een van de stap koppelingen in het linkerdeelvenster.

13. Selecteer **implementeren** om het implementatieproces te starten.

14. Nadat het implementatieproces is voltooid, kunt u de pagina met resultaten, waarin het slagen of mislukken van elke implementatieactie kunt weergeven.
    a. Als geen actie is mislukt, in de **resultaat** kolom, selecteer **mislukt** om een uitleg van de fout weer te geven.
    b. Selecteer desgewenst **rapport opslaan** om op te slaan van de resultaten naar een XML-bestand.

15. Selecteer **sluiten** om af te sluiten van de Wizard Integration Services-implementatie.

Als de implementatie van uw project is voltooid zonder fouten, kunt u alle pakketten die deze bevat om uit te voeren op uw Azure-SSIS-IR.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de hulp bij de migratie in de Microsoft [handleiding voor databasemigratie](https://datamigration.microsoft.com/).
