---
title: Visualiseer gegevens uit Azure Data Explorer met behulp van Sisense
description: In dit artikel leert u hoe u Azure Data Explorer instellen als een gegevensbron voor Sisense en de gegevens visualiseren.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: f0840b90e1036c23fa58d94515bfeb035299c07f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358181"
---
# <a name="visualize-data-from-azure-data-explorer-in-sisense"></a>Visualiseer gegevens uit Azure Data Explorer in Sisense

Sisense is een platform voor streaminganalyse business intelligence waarmee u het bouwen van apps voor analyses die zeer interactieve gebruikerservaringen bieden. De business intelligence en rapportage van software-dashboard kunt u toegang tot en het combineren van gegevens in een paar klikken. U kunt verbinding maken met gestructureerde en ongestructureerde gegevens bronnen, deelnemen aan tabellen uit meerdere bronnen met minimale scripting en codering en interactieve webtoepassingen dashboards en rapporten maken. In dit artikel leert u hoe u Azure Data Explorer instellen als een gegevensbron voor Sisense en Visualiseer gegevens uit een voorbeeld-cluster.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende om te voltooien in dit artikel nodig:

* [Download en installeer Sisense app](https://documentation.sisense.com/latest/getting-started/download-install.htm) 

* Maak een cluster en de database waarin de voorbeeldgegevens StormEvents. Zie voor meer informatie [Snelstart: Maak een Azure Data Explorer-cluster en de database](create-cluster-database-portal.md) en [voorbeeldgegevens worden opgenomen in Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="connect-to-sisense-dashboards-using-azure-data-explorer-jdbc-connector"></a>Verbinding maken met Sisense dashboards met behulp van Azure Data Explorer JDBC-connector

1. Downloaden en kopiëren van de meest recente versies van de volgende jar-bestanden naar *... \Sisense\DataConnectors\jdbcdrivers\adx* 

    * activering 1.1.jar
    * adal4j-1.6.0.jar
    * commons-codec-1.10.jar
    * commons-collections4-4.1.jar
    * commons-lang3-3.5.jar
    * gson 2.8.0.jar
    * jcip-annotations-1.0-1.jar
    * json-smart-1.3.1.jar
    * lang-tag-1.4.4.jar
    * e-mail-1.4.7.jar
    * mssql-jdbc-7.2.1.jre8.jar
    * nimbus-jose-jwt-7.0.1.jar
    * oauth2-oidc-sdk-5.24.1.jar
    * slf4j-api-1.7.21.jar
    
1. Open **Sisense** app.
1. Selecteer **gegevens** tabblad en selecteer **+ ElastiCube** om een nieuwe ElastiCube-model te maken.
    
    ![Selecteer ElastiCube](media/sisense/data-select-elasticube.png)

1. In **nieuwe ElastiCube Model toevoegen**, naam van het model ElastiCube en **opslaan**.
   
    ![Nieuwe ElastiCube model toevoegen](media/sisense/add-new-elasticube-model.png)

1. Selecteer **+ gegevens**.

    ![Selecteer de knop gegevens](media/sisense/select-data.png)

1. In **Connector selecteren** tabblad **algemene JDBC** connector.

    ![JDBC-connector kiezen](media/sisense/select-connector.png)

1. In de **Connect** tabblad, voert u de volgende velden voor de **algemene JDBC** -connector en selecteer **volgende**.

    ![JDBC-connectorinstellingen](media/sisense/jdbc-connector.png)

    |Veld |Description |
    |---------|---------|
    |Verbindingsreeks     |   `jdbc:sqlserver://<cluster_name.region>.kusto.windows.net:1433;database=<database_name>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.kusto.windows.net;loginTimeout=30;authentication=ActiveDirectoryPassword`      |
    |JDBC JARs map  |    `..\Sisense\DataConnectors\jdbcdrivers\adx`     |
    |De klassenaam van het stuurprogramma    |   `com.microsoft.sqlserver.jdbc.SQLServerDriver`      |
    |Gebruikersnaam   |    Gebruikersnaam van AAD     |
    |Wachtwoord     |   AAD-gebruikerswachtwoord      |

1. In de **Select Data** tabblad, zoeken op **Database selecteren** om te selecteren van de desbetreffende database waarvoor u machtigingen hebt. In dit voorbeeld selecteren *test1*.

    ![database selecteren](media/sisense/select-database.png)

1. In *testen* deelvenster (databasenaam):
    1. Selecteer de naam van de tabel te bekijken in de tabel en de namen van de kolom tabel zien. U kunt overbodige kolommen verwijderen.
    1. Schakel het selectievakje van de betreffende tabel om te selecteren die tabel. 
    1. Selecteer **Done**.

    ![Tabel selecteren](media/sisense/select-table-see-columns.png)    

1. Selecteer **bouwen** aan het bouwen van uw gegevensset. 

    * In de **bouwen** venster **bouwen**.

      ![Venster bouwen](media/sisense/build-window.png)

    * Wacht totdat de build-proces is voltooid en selecteer vervolgens **Build is voltooid**.

      ![Vormen is voltooid](media/sisense/build-succeeded.png)

## <a name="create-sisense-dashboards"></a>Sisense dashboards maken

1. In **Analytics** tabblad **+**  >  **nieuw Dashboard** om dashboards te maken voor deze gegevensset.

    ![Nieuw dashboard](media/sisense/new-dashboard.png)

1. Kies een dashboard en selecteer **maken**. 

    ![dashboard maken](media/sisense/create-dashboard.png)

1. Onder **nieuwe Widget**, selecteer **+ gegevens selecteren** te maken van een nieuwe widget. 

    ![Velden toevoegen aan StormEvents dashboard](media/sisense/storm-dashboard-add-field.png)  

1. Selecteer **+ meer gegevens toevoegen** extra kolommen toevoegen aan uw grafiek. 

    ![Meer gegevens toevoegen aan de grafiek](media/sisense/add-more-data.png)

1. Selecteer **+ Widget** te maken van een andere widget. Sleep en zet widgets aan uw dashboard opnieuw rangschikken.

    ![Storm-dashboard](media/sisense/final-dashboard.png)

U kunt nu Verken uw gegevens met visuele analyse, extra dashboards kunt bouwen en gegevens transformeren in bruikbare inzichten om indruk te maken van uw bedrijf.

## <a name="next-steps"></a>Volgende stappen

* [Query's schrijven voor Azure Data Explorer](write-queries.md)

