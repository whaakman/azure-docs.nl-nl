---
title: R Server op een HDInsight - Azure operationeel | Microsoft Docs
description: Informatie over het operationeel R Server in Azure HDInsight.
services: HDInsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: 93957b7ee10527039bf2e96cc5470420cdef0651
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="operationalize-r-server-cluster-on-azure-hdinsight"></a>R Server-cluster in Azure HDInsight operationeel maken

Nadat u R Server-cluster in HDInsight met het voltooien van uw gegevens modelleren gebruikt, kunt u het model om te maken van voorspellingen operationeel te maken. In dit artikel bevat instructies over het uitvoeren van deze taak.

## <a name="prerequisites"></a>Vereisten

* **Een R Server-cluster in HDInsight**: Zie voor instructies [aan de slag met op HDInsight R Server](r-server-get-started.md).

* **Een SSH-client (Secure Shell)**: er wordt een SSH-client gebruikt om extern verbinding te maken met het HDInsight-cluster en om opdrachten rechtstreeks uit te voeren op het cluster. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="operationalize-r-server-cluster-with-one-box-configuration"></a>R Server-cluster met één verpakking configuratie operationeel maken

1. SSH op het Edge-knooppunt.  

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Zie voor instructies over het gebruik van SSH met Azure HDInsight [gebruik van SSH met HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Wijzig de directory voor de desbetreffende versie en sudo het DLL-bestand net punt: 

    - Voor Microsoft R Server 9.1:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Voor Microsoft R Server 9.0:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. U wordt kunt kiezen uit de opties weergegeven. De eerste optie, zoals wordt weergegeven in de volgende schermafbeelding tot **R Server configureren voor uitoefening**.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-1.png)

4. U wordt kunt kiezen hoe u wilt R Server operationeel nu weergegeven. Kies het eerste beheerpunt door te voeren in de aangeboden opties **A**.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-2.png)

5. Wanneer u wordt gevraagd, voer en geef het wachtwoord voor een gebruiker met lokale beheerdersrechten opnieuw.

6. Hier ziet u uitvoer opmaken dat de bewerking voltooid is. U wordt ook gevraagd een andere optie te selecteren in het menu. Selecteer E tot de terug te keren naar het hoofdmenu.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-3.png)

7. U kunt eventueel diagnostische controles uitvoeren door het uitvoeren van een diagnostische test als volgt:

    a. Selecteer in het hoofdmenu **6** diagnostische tests uitvoeren.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-1.png)

    b. Selecteer in het menu Diagnostische Tests **A**. Wanneer u wordt gevraagd, typt u het wachtwoord die u hebt opgegeven voor de lokale beheerder.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-2.png)

    c. Controleer of de uitvoer laat zien dat de algehele status geslaagd is.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-3.png)

    d. Van de menuopties gepresenteerd, voer **E** om te keren naar het hoofdmenu en voer **8** om af te sluiten van het hulpprogramma Serverbeheer.

### <a name="long-delays-when-consuming-web-service-on-spark"></a>Lange vertragingen bij webservice op Spark

Als er lange vertragingen optreden bij het gebruiken van een webservice die is gemaakt met mrsdeploy-functies in een Spark-compute-context, moet u mogelijk een aantal ontbrekende mappen toevoegen. De Spark-toepassing is van een gebruiker met de naam *rserve2* wanneer de toepassing wordt aangeroepen vanuit een webservice met behulp van mrsdeploy-functies. Dit probleem omzeilen:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


In dit stadium is de configuratie voor uitoefening voltooid. Nu kunt u de `mrsdeploy` pakket op uw RClient verbinding maken met de uitoefening op de edge-knooppunt en aan de slag met de functies, zoals [uitvoering op afstand](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) en [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Afhankelijk van of het cluster is ingesteld in een virtueel netwerk of niet, moet u mogelijk forward tunneling via SSH-aanmelding instellen voor de poort. In de volgende secties wordt uitgelegd hoe u deze tunnel instelt.

### <a name="r-server-cluster-on-virtual-network"></a>R Server-cluster op het virtuele netwerk

Zorg ervoor dat u verkeer via poort 12800 naar het Edge-knooppunt toestaat. Op deze manier kunt u het Edge-knooppunt gebruiken om verbinding te maken met de functie Uitoefening.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Als de `remoteLogin()` geen verbinding kan maken met het Edge-knooppunt maar als u wel verbinding hebt via SSH, moet u controleren of de regel op basis waarvan verkeer via poort 12800 is toegestaan, juist is ingesteld of niet. Als dit probleem zich blijft voordoen, kunt u een tijdelijke oplossing gebruiken door forward tunneling via SSH in te stellen voor de poort. Zie de volgende sectie voor instructies:

### <a name="r-server-cluster-not-set-up-on-virtual-network"></a>R Server-cluster niet is ingesteld op virtueel netwerk

Als het cluster niet is ingesteld in het virtuele netwerk vnet of als u problemen ondervindt met de connectiviteit via dit netwerk, kunt u forward tunneling via SSH instellen voor de poort:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Zodra de SSH-sessie actief is, wordt het verkeer bij poort 12800 op uw computer via de SSH-sessie doorgestuurd naar poort 12800 van het Edge-knooppunt. Zorg ervoor dat u `127.0.0.1:12800` gebruikt in de `remoteLogin()`-methode. Dit aanmeldt bij het edge-knooppunt uitoefening via poort doorsturen.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>Schaal geoperationaliseerd rekenknooppunten op HDInsight worker-knooppunten

Als u wilt schalen van de rekenknooppunten, schaft u eerst de worker-knooppunten en configureer vervolgens de rekenknooppunten voor het buiten gebruik gestelde worker-knooppunten.

### <a name="step-1-decommission-the-worker-nodes"></a>Stap 1: Uit bedrijf nemen worker-knooppunten

R Server-cluster niet wordt beheerd via YARN. Als de worker-knooppunten niet buiten gebruik wordt gesteld, werkt de YARN-Resource Manager niet zoals verwacht, omdat deze niet op de hoogte van de resources in beslag nemen door de server. Om deze sitatie te voorkomen raden we u aan de werkknooppunten uit bedrijf te nemen voordat u de rekenknooppunten uitschaalt.

Volg deze stappen voor worker-knooppunten uit bedrijf nemen:

1. Aanmelden bij de Ambari-console van het cluster en klik op **Hosts** tabblad.

2. Selecteer de worker-knooppunten (om te worden buiten gebruik wordt gesteld).

3. Klik op **acties** > **Hosts geselecteerd** > **Hosts** > **onderhoudsmodus inschakelen**. In de volgende afbeelding zijn bijvoorbeeld wk3 en wk4 geselecteerd om uit bedrijf te worden genomen.  

   ![worker-knooppunten uit bedrijf nemen](./media/r-server-operationalize/get-started-operationalization.png)  

* Selecteer **acties** > **Hosts geselecteerd** > **DataNodes** > klikt u op **uit bedrijf nemen**.
* Selecteer **acties** > **Hosts geselecteerd** > **NodeManagers** > klikt u op **uit bedrijf nemen**.
* Selecteer **acties** > **Hosts geselecteerd** > **DataNodes** > klikt u op **stoppen**.
* Selecteer **acties** > **Hosts geselecteerd** > **NodeManagers** > Klik op **stoppen**.
* Selecteer **acties** > **Hosts geselecteerd** > **Hosts** > klikt u op **stoppen alle onderdelen**.
* Hef de selectie van de worker-knooppunten op en selecteer de hoofdknooppunten.
* Selecteer **acties** > **Hosts geselecteerd** > "**Hosts** > **alle onderdelen opnieuw**.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Stap 2: Configureer rekenknooppunten op elke buiten gebruik gestelde worker-knooppunten

1. SSH op elk uit bedrijf genomen werkknooppunt.

2. Hulpprogramma voor Serverbeheer met behulp van de relevante DLL voor het cluster R Server die u hebt uitgevoerd. R Server 9.1, voert u de volgende:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Voer **1** optie selecteren **R Server configureren voor uitoefening**.

4. Voer **C** optie `C. Compute node`. Hiermee configureert u het rekenknooppunt op het werkknooppunt.

5. Sluit het beheerprogramma.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>Stap 3: Toevoegen compute details van de knooppunten op web-knooppunt

Zodra alle buiten gebruik gestelde worker-knooppunten zijn geconfigureerd voor het rekenknooppunt worden uitgevoerd, keert u terug op het edge-knooppunt en buiten gebruik gestelde worker-knooppunten IP-adressen in de configuratie van het R Server web knooppunt toevoegen:

1. SSH op het Edge-knooppunt.

2. Voer `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` uit.

3. Zoek naar de sectie 'URI' en werkrolknooppunt van IP- en poortgegevens toevoegen.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Volgende stappen

* [R Server-cluster in HDInsight beheren](r-server-hdinsight-manage.md)
* [Opties voor R Server-cluster in HDInsight context voor berekenen](r-server-compute-contexts.md)
* [Opties voor Azure Storage voor R Server-cluster in HDInsight](r-server-storage.md)