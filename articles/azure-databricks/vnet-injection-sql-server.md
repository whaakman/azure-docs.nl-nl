---
title: Query uitvoeren op een SQL Server Linux Docker-container in een virtueel netwerk van een Azure Databricks-notebook
description: Dit artikel wordt beschreven hoe u Azure Databricks implementeert in uw virtuele netwerk, ook wel bekend als VNet-injectie.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 345e07fac30f4ad0c8e9918cb8a1ff0fb8aeb811
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288949"
---
# <a name="tutorial-query-a-sql-server-linux-docker-container-in-a-virtual-network-from-an-azure-databricks-notebook"></a>Zelfstudie: Query uitvoeren op een SQL Server Linux Docker-container in een virtueel netwerk van een Azure Databricks-notebook

Deze zelfstudie leert u over het integreren van Azure Databricks met een SQL Server Linux Docker-container in een virtueel netwerk. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte aan een virtueel netwerk implementeren
> * Een virtuele Linux-machine installeren in een openbaar netwerk
> * Docker installeren
> * Installeer Microsoft SQL Server op Linux docker-container
> * Query uitvoeren op de SQL-Server met behulp van JDBC uit een Databricks-notebook

## <a name="prerequisites"></a>Vereisten

* Maak een [Databricks-werkruimte in een virtueel netwerk](quickstart-create-databricks-workspace-vnet-injection.md).

* Installeer [Ubuntu voor Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab).

* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) downloaden.

## <a name="create-a-linux-virtual-machine"></a>Een virtuele Linux-machine maken

1. In de Azure-portal, selecteer het pictogram voor **virtuele Machines**. Selecteer **+ toevoegen**.

    ![Toevoegen van nieuwe virtuele machine van Azure](./media/vnet-injection-sql-server/add-virtual-machine.png)

2. Op de **basisbeginselen** tabblad, kies Ubuntu Server 16.04 LTS. Wijzig de VM-grootte in B1ms, met één vcpu's en 2 GB RAM-geheugen. De minimum vereisten voor een Linux SQL Server-Docker-container is 2 GB. Kies een beheerder gebruikersnaam en wachtwoord.

    ![Tabblad van de basisprincipes van nieuwe virtuele-machineconfiguratie](./media/vnet-injection-sql-server/create-virtual-machine-basics.png)

3. Navigeer naar de **netwerken** tabblad. Kies het virtuele netwerk en de openbare subnet, met inbegrip van uw Azure Databricks-cluster. Selecteer **revisie + maken**, klikt u vervolgens **maken** de virtuele machine wilt implementeren.

    ![Tabblad netwerk van de configuratie van een nieuwe virtuele machine](./media/vnet-injection-sql-server/create-virtual-machine-networking.png)

4. Wanneer de implementatie is voltooid, gaat u naar de virtuele machine. U ziet het openbare IP-adres en het virtuele netwerk/subnet in de **overzicht**. Selecteer de **openbaar IP-adres**

    ![Overzicht van virtuele machines](./media/vnet-injection-sql-server/virtual-machine-overview.png)

5. Wijzig de **toewijzing** te **statische** en voer een **DNS-naamlabel**. Selecteer **opslaan**, en start de virtuele machine.

    ![Openbare IP-adresconfiguratie](./media/vnet-injection-sql-server/virtual-machine-staticip.png)

6. Selecteer de **netwerken** tabblad onder **instellingen**. U ziet dat de netwerkbeveiligingsgroep die is gemaakt tijdens de implementatie van Azure Databricks gekoppeld aan de virtuele machine is. Selecteer **regel voor binnenkomende poort toevoegen**.

7. Voeg een regel voor het openen van poort 22 voor SSH. Gebruik de volgende instellingen:
    
    |Instelling|Voorgestelde waarde|Description|
    |-------|---------------|-----------|
    |Bron|IP-adressen|IP-adressen bevat die binnenkomend verkeer van een specifieke bron-IP-adres wordt toegestaan of geweigerd door deze regel.|
    |IP-adressen van bron|< uw openbare IP-adres\>|Voer de uw openbare IP-adres. U kunt uw openbare IP-adres vinden door naar de pagina [bing.com](https://www.bing.com/) en te zoeken naar **'Mijn IP'**.|
    |Poortbereiken van bron|*|Verkeer van een willekeurige poort toestaan.|
    |Doel|IP-adressen|IP-adressen bevat die uitgaand verkeer voor een specifieke bron-IP-adres wordt toegestaan of geweigerd door deze regel.|
    |Doel-IP-adressen|< uw vm openbaar IP-adres\>|Voer het openbare IP-adres van de virtuele machine. U kunt dit vinden op de **overzicht** pagina van uw virtuele machine.|
    |Poortbereiken van doel|22|Open poort 22 voor SSH.|
    |Prioriteit|290|De regel een prioriteit geven.|
    |Name|ssh-databricks-tutorial-vm|De regel een naam geven.|


    ![Inkomende beveiligingsregel voor poort 22 toevoegen](./media/vnet-injection-sql-server/open-port.png)

8. Voeg een regel voor het poort 1433 openen voor SQL met de volgende instellingen:

    |Instelling|Voorgestelde waarde|Description|
    |-------|---------------|-----------|
    |Bron|IP-adressen|IP-adressen bevat die binnenkomend verkeer van een specifieke bron-IP-adres wordt toegestaan of geweigerd door deze regel.|
    |IP-adressen van bron|10.179.0.0/16|Voer het adresbereik voor het virtuele netwerk.|
    |Poortbereiken van bron|*|Verkeer van een willekeurige poort toestaan.|
    |Doel|IP-adressen|IP-adressen bevat die uitgaand verkeer voor een specifieke bron-IP-adres wordt toegestaan of geweigerd door deze regel.|
    |Doel-IP-adressen|< uw vm openbaar IP-adres\>|Voer het openbare IP-adres van de virtuele machine. U kunt dit vinden op de **overzicht** pagina van uw virtuele machine.|
    |Poortbereiken van doel|1433|Open poort 22 voor SQL Server.|
    |Prioriteit|300|De regel een prioriteit geven.|
    |Name|sql-databricks-tutorial-vm|De regel een naam geven.|

    ![Beveiligingsregel voor binnenkomend verkeer van poort 1433 toevoegen](./media/vnet-injection-sql-server/open-port2.png)

## <a name="run-sql-server-in-a-docker-container"></a>Voer SQL Server in een Docker-container

1. Open [Ubuntu voor Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab), of een ander hulpprogramma waarmee u kunt SSH bij de virtuele machine. Navigeer naar uw virtuele machine in Azure portal en selecteer **Connect** om op te halen van de SSH-opdracht die u nodig hebt om verbinding te maken.

    ![Verbinding maken met de virtuele machine](./media/vnet-injection-sql-server/vm-ssh-connect.png)

2. Voer de opdracht in uw terminal Ubuntu en voer het beheerderswachtwoord dat u hebt gemaakt tijdens het configureren van de virtuele machine.

    ![Ubuntu terminal SSH aanmelden](./media/vnet-injection-sql-server/vm-login-terminal.png)

3. Gebruik de volgende opdracht om Docker te installeren op de virtuele machine.

    ```bash
    sudo apt-get install docker.io
    ```

    Controleer of de installatie van Docker met de volgende opdracht:

    ```bash
    sudo docker --version
    ```

4. Installeer de installatiekopie.

    ```bash
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

    Controleer de afbeeldingen.

    ```bash
    sudo docker images
    ```

5. Voer de container van de installatiekopie.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password1234' -p 1433:1433 --name sql1  -d mcr.microsoft.com/mssql/server:2017-latest
    ```

    Controleren of de container wordt uitgevoerd.

    ```bash
    sudo docker ps -a
    ```

## <a name="create-a-sql-database"></a>Een SQL-database maken

1. Open SQL Server Management Studio en maak verbinding met de server met behulp van de servernaam en de SQL-verificatie. De aanmelding gebruikersnaam is **SA** en het wachtwoord is het wachtwoord instellen in de opdracht Docker. Het wachtwoord in het voorbeeld is `Password1234`.

    ![Verbinding maken met SQL Server met behulp van SQL Server Management Studio](./media/vnet-injection-sql-server/ssms-login.png)

2. Als u verbinding met succes gemaakt hebt, selecteert u **nieuwe Query** en voer het volgende codefragment voor het maken van een database, een tabel, en sommige records in de tabel invoegen.

    ```SQL
    CREATE DATABASE MYDB;
    GO
    USE MYDB;
    CREATE TABLE states(Name VARCHAR(20), Capitol VARCHAR(20));
    INSERT INTO states VALUES ('Delaware','Dover');
    INSERT INTO states VALUES ('South Carolina','Columbia');
    INSERT INTO states VALUES ('Texas','Austin');
    SELECT * FROM states
    GO
    ```

    ![Query voor het maken van een SQL Server-database](./media/vnet-injection-sql-server/create-database.png)

## <a name="query-sql-server-from-azure-databricks"></a>Query SQL Server vanuit Azure Databricks

1. Navigeer naar uw Azure Databricks-werkruimte en controleer of u een cluster gemaakt als onderdeel van de vereisten. Selecteer **een notitieblok maken**. Geef een naam, select op voor de notebook *Python* als de taal en selecteer het cluster dat u hebt gemaakt.

    ![Nieuwe instellingen van de Databricks notebook](./media/vnet-injection-sql-server/create-notebook.png)

2. Gebruik de volgende opdracht om te pingen van het interne IP-adres van de SQL Server-machine. Deze ping is voltooid. Als dat niet het geval is, moet u controleren of de container wordt uitgevoerd en controleer de configuratie van network security group (NSG).

    ```python
    %sh
    ping 10.179.64.4
    ```

    U kunt ook de nslookup-opdracht gebruiken om te controleren.

    ```python
    %sh
    nslookup databricks-tutorial-vm.westus2.cloudapp.azure.com
    ```

3. Nadat u de SQL-Server met succes hebt gepingd, kunt u de database en tabellen kunt opvragen. Voer de volgende python-code:

    ```python
    jdbcHostname = "10.179.64.4"
    jdbcDatabase = "MYDB"
    userName = 'SA'
    password = 'Password1234'
    jdbcPort = 1433
    jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, userName, password)

    df = spark.read.jdbc(url=jdbcUrl, table='states')
    display(df)
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, de Azure Databricks-werkruimte en alle gerelateerde resources. De taak wordt verwijderd, voorkomt u onnodig facturering. Als u van plan bent de Azure Databricks-werkruimte in de toekomst te gebruiken, kunt u het cluster stoppen en later opnieuw. Als u niet blijven gebruiken van deze Azure Databricks-werkruimte wilt, verwijdert u alle resources die u in deze zelfstudie hebt gemaakt met behulp van de volgende stappen uit:

1. Klik in het menu links in Azure portal op **resourcegroepen** en klik vervolgens op de naam van de resourcegroep die u hebt gemaakt.

2. Selecteer op de pagina van uw resourcegroep **verwijderen**, typ de naam van de resource wilt verwijderen in het tekstvak in en selecteer vervolgens **verwijderen** opnieuw.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het uitpakken, transformeren en laden van gegevens met behulp van Azure Databricks.
> [!div class="nextstepaction"]
> [Zelfstudie: Gegevens extraheren, transformeren en laden met Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
