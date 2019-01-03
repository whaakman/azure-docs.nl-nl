---
title: ML-Services op HDInsight - Azure operationeel maken
description: Informatie over het operationeel maken van ML-Services in Azure HDInsight.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: d0cae4210fa313c5d6e1f33987422e7b0d53e76a
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579954"
---
# <a name="operationalize-ml-services-cluster-on-azure-hdinsight"></a>ML-Services-cluster in Azure HDInsight operationeel maken

Nadat u Services ML-cluster in HDInsight gebruikt hebt om uit te voeren van het modelleren van uw gegevens, kunt u operationeel maken van het model om voorspellingen te maken. In dit artikel bevat instructies over het uitvoeren van deze taak.

## <a name="prerequisites"></a>Vereisten

* **Een ML-Services-cluster in HDInsight**: Zie voor instructies [aan de slag met ML-Services op HDInsight](r-server-get-started.md).

* **Een Secure Shell (SSH)-client**: Een SSH-client wordt gebruikt om op afstand verbinding maken met het HDInsight-cluster en opdrachten rechtstreeks op het cluster uitvoeren. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="operationalize-ml-services-cluster-with-one-box-configuration"></a>Cluster met-in-één configuratie ML-Services operationeel maken

> [!NOTE]  
> De onderstaande stappen zijn van toepassing op R Server 9.0 en ML Server 9.1. Raadpleeg voor ML Server 9.3, [gebruiken het beheerprogramma voor het beheren van de configuratie voor uitoefening](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch).

1. SSH op het Edge-knooppunt.

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Zie voor instructies over het gebruik van SSH met HDInsight van Azure [SSH gebruiken met HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Wijzig de directory voor de relevante versie en sudo in de dot net-dll: 

    - Voor Microsoft ML Server 9.1:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Voor Microsoft R Server 9.0:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. U wordt kunt kiezen uit de opties weergegeven. De eerste optie, zoals wordt weergegeven in de volgende schermafbeelding op **ML-Server voor uitoefening configureren**.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-1.png)

1. Nu krijgt u de mogelijkheid om te kiezen hoe u voor het operationeel maken van ML-Server. Kies de eerste record door te voeren in de geboden opties **A**.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-2.png)

1. Wanneer u hierom wordt gevraagd, voer en het wachtwoord voor een gebruiker met lokale beheerdersrechten opnieuw invoeren.

1. Hier ziet u uitvoer die stellen dat de bewerking geslaagd is. U wordt ook gevraagd om een andere optie in het menu. Selecteer E om terug te gaan naar het hoofdmenu.

    ![De optie Alles-in-één](./media/r-server-operationalize/admin-util-one-box-3.png)

1. Desgewenst kunt u diagnostische controles uitvoeren door het uitvoeren van een diagnostische test als volgt:

    a. Selecteer in het hoofdmenu van **6** diagnostische tests uitvoeren.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-1.png)

    b. Selecteer in het menu Diagnostische tests uit **A**. Wanneer u wordt gevraagd, typt u het wachtwoord die u hebt opgegeven voor de gebruiker met lokale beheerdersrechten.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-2.png)

    c. Controleer of dat de uitvoer ziet u dat de algehele status geslaagd is.

    ![De optie Alles-in-één](./media/r-server-operationalize/diagnostic-3.png)

    d. Voer bij de menuopties weergegeven, **E** om te keren naar het hoofdmenu en voer **8** om af te sluiten van het beheerprogramma.

### <a name="long-delays-when-consuming-web-service-on-apache-spark"></a>Lange vertragingen bij het gebruiken van webservice op Apache Spark

Als er lange vertragingen optreden bij het gebruik van een webservice die is gemaakt met mrsdeploy-functies in een Apache Spark-compute-context, moet u mogelijk aantal ontbrekende mappen toevoegen. De Spark-toepassing is van een gebruiker met de naam *rserve2* wanneer de toepassing wordt aangeroepen vanuit een webservice met behulp van mrsdeploy-functies. Dit probleem omzeilen:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


In dit stadium is de configuratie voor uitoefening voltooid. Nu kunt u de `mrsdeploy` pakket op de RClient verbinding maken met het uitoefening op edge-knooppunt en start met behulp van de functies zoals [uitvoering op afstand](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) en [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Afhankelijk van of het cluster is ingesteld in een virtueel netwerk of niet, moet u mogelijk forward tunneling via SSH-aanmelding instellen voor de poort. In de volgende secties wordt uitgelegd hoe u deze tunnel instelt.

### <a name="ml-services-cluster-on-virtual-network"></a>Cluster met ML-Services op virtueel netwerk

Zorg ervoor dat u verkeer via poort 12800 naar het Edge-knooppunt toestaat. Op deze manier kunt u het Edge-knooppunt gebruiken om verbinding te maken met de functie Uitoefening.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Als de `remoteLogin()` geen verbinding kan maken met het Edge-knooppunt maar als u wel verbinding hebt via SSH, moet u controleren of de regel op basis waarvan verkeer via poort 12800 is toegestaan, juist is ingesteld of niet. Als dit probleem zich blijft voordoen, kunt u een tijdelijke oplossing gebruiken door forward tunneling via SSH in te stellen voor de poort. Zie voor instructies in de volgende sectie:

### <a name="ml-services-cluster-not-set-up-on-virtual-network"></a>ML-Services-cluster niet is ingesteld op virtueel netwerk

Als het cluster niet is ingesteld in het virtuele netwerk vnet of als u problemen ondervindt met de connectiviteit via dit netwerk, kunt u forward tunneling via SSH instellen voor de poort:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Zodra de SSH-sessie actief is, wordt het verkeer bij poort 12800 op uw lokale machine doorgestuurd naar poort 12800 op het edge-knooppunt via SSH-sessie. Zorg ervoor dat u `127.0.0.1:12800` gebruikt in de `remoteLogin()`-methode. Dit meldt zich aan bij de uitoefening van het edge-knooppunt via doorsturen via poort.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>Schaal geoperationaliseerd rekenknooppunten op worker-knooppunten voor HDInsight

Als u wilt de compute-knooppunten schalen, moet u eerst uit bedrijf nemen de worker-knooppunten en vervolgens rekenknooppunten configureren op de uit bedrijf genomen worker-knooppunten.

### <a name="step-1-decommission-the-worker-nodes"></a>Stap 1: De worker-knooppunten uit bedrijf nemen

ML-Services cluster niet wordt beheerd via [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html). Als de worker-knooppunten niet uit bedrijf genomen, werkt de resourcemanager YARN niet zoals verwacht, omdat het is niet op de hoogte van de resources in beslag wordt genomen door de server. Om deze sitatie te voorkomen raden we u aan de werkknooppunten uit bedrijf te nemen voordat u de rekenknooppunten uitschaalt.

Volg deze stappen voor het worker-knooppunten uit bedrijf nemen:

1. Meld u aan bij de Ambari-console van het cluster en klik op **Hosts** tabblad.

1. Selecteer worker-knooppunten (worden buiten gebruik gestelde).

1. Klik op **acties** > **geselecteerde Hosts** > **Hosts** > **onderhoudsmodus inschakelen**. In de volgende afbeelding zijn bijvoorbeeld wk3 en wk4 geselecteerd om uit bedrijf te worden genomen.  

   ![worker-knooppunten uit bedrijf nemen](./media/r-server-operationalize/get-started-operationalization.png)  

* Selecteer **acties** > **geselecteerde Hosts** > **DataNodes** > klikt u op **uit bedrijf nemen**.
* Selecteer **acties** > **geselecteerde Hosts** > **NodeManagers** > klikt u op **uit bedrijf nemen**.
* Selecteer **acties** > **geselecteerde Hosts** > **DataNodes** > klikt u op **stoppen**.
* Selecteer **acties** > **geselecteerde Hosts** > **NodeManagers** > Klik op **stoppen**.
* Selecteer **acties** > **geselecteerde Hosts** > **Hosts** > klikt u op **alle onderdelen stoppen**.
* Hef de selectie van de worker-knooppunten op en selecteer de hoofdknooppunten.
* Selecteer **acties** > **geselecteerde Hosts** > "**Hosts** > **alle onderdelen opnieuw starten**.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Stap 2: Rekenknooppunten configureren op elk uit bedrijf genomen worker-knooppunten

1. SSH op elk uit bedrijf genomen werkknooppunt.

1. Hulpprogramma voor beheer met behulp van de relevante dll-bestand voor het ML-Services-cluster dat u hebt uitgevoerd. Voor ML Server 9.1, voert u het volgende:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. Voer **1** optie **ML-Server voor uitoefening configureren**.

1. Voer **C** optie `C. Compute node`. Hiermee configureert u het rekenknooppunt op het werkknooppunt.

1. Sluit het beheerprogramma.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>Stap 3: Details van rekenknooppunten toevoegen op het webknooppunt

Zodra alle uit bedrijf genomen worker-knooppunten zijn geconfigureerd voor het rekenknooppunt worden uitgevoerd, keert u terug naar het edge-knooppunt en toevoegen van IP-adressen uit bedrijf genomen worker-knooppunten in het ML-Server-webknooppunt configuratie:

1. SSH op het Edge-knooppunt.

1. Voer `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` uit.

1. Zoek naar de sectie 'URI' en voeg worker-knooppunt IP- en poortgegevens.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Volgende stappen

* [ML Services-cluster beheren in HDInsight](r-server-hdinsight-manage.md)
* [Opties voor compute-context voor ML Services-cluster in HDInsight](r-server-compute-contexts.md)
* [Opties voor Azure Storage voor ML Services-cluster in HDInsight](r-server-storage.md)
