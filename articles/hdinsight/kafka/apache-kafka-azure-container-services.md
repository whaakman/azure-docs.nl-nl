---
title: Gebruik Azure Containerservice met Kafka op HDInsight | Microsoft Docs
description: Informatie over het Kafka op HDInsight van container afbeeldingen die worden gehost in Azure Container Service (AKS) gebruiken.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2018
ms.author: larryfr
ms.openlocfilehash: 8074797e2d37f98cc3b219dbf3e51f558bbee8c7
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
---
# <a name="use-azure-container-services-with-kafka-on-hdinsight"></a>Azure-Containerservices gebruiken met Kafka in HDInsight

Informatie over het gebruik van Azure Container Service (AKS) met Kafka op HDInsight-cluster.

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van pijplijnen en toepassingen voor realtime streaming van gegevens. Azure Container Service beheert uw gehoste Kubernetes-omgeving en kunt u snel en eenvoudig om beperkte toepassingen te implementeren. Met behulp van een Azure-netwerk, kunt u de twee services.

> [!IMPORTANT]
> Dit document wordt ervan uitgegaan dat u bekend bent met het maken en gebruiken van de volgende Azure-services:
>
> * Kafka op HDInsight
> * Azure Container Service
> * Virtuele netwerken van Azure
>
> Dit document wordt ervan uitgegaan dat u hebt doorlopen de [Azure Container Services zelfstudie](../../aks/tutorial-kubernetes-prepare-app.md). In deze zelfstudie maakt u een containerservice, maakt u een cluster Kubernetes, een register container en configureert u de `kubectl` hulpprogramma.

> [!NOTE]
> De stappen in dit document gebruiken een Node.js-toepassing die wordt gehost in AKS om te controleren of de verbinding met Kafka. Deze toepassing gebruikt de [kafka-knooppunt](https://www.npmjs.com/package/kafka-node) pakket om te communiceren met Kafka. Hierbij [Socket.io](https://socket.io/) voor gebeurtenis gebaseerde uitwisseling van berichten tussen de browserclient en de back-end gehost in AKS.

## <a name="architecture"></a>Architectuur

### <a name="network-topology"></a>Netwerktopologie

Gebruik een virtueel netwerk van Azure HDInsight zowel AKS als een container rekenresources. Om de communicatie tussen HDInsight en AKS inschakelt, moet u communicatie tussen hun netwerken inschakelen. De stappen in dit document gebruikt Peering van virtueel netwerk met de netwerken. Zie voor meer informatie over het peering de [virtuele netwerk peering](../../virtual-network/virtual-network-peering-overview.md) document.

Het volgende diagram illustreert de netwerktopologie in dit document gebruikt:

![HDInsight in één virtueel netwerk, AKS in een andere en de netwerken die zijn verbonden via peering](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]
> Naamomzetting is niet ingeschakeld tussen de peered netwerken, zodat het IP-adressering wordt gebruikt. Kafka op HDInsight is standaard geconfigureerd om terug te keren hostnamen in plaats van IP-adressen wanneer clients verbinding maken. De stappen in dit document Kafka voor het gebruik van IP wijzigen in plaats daarvan wordt geadverteerd.

## <a name="prerequisites"></a>Vereisten

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Een Azure-abonnement

## <a name="create-an-azure-container-service-aks"></a>Maak een Azure Containerservice (AKS)

Als u nog geen een AKS-cluster, kunt u een van de volgende documenten voor meer informatie over het maken van een gebruiken:

* [Een Azure Container Service (AKS)-cluster - Portal implementeren](../../aks/kubernetes-walkthrough-portal.md)
* [Een Azure Container Service (AKS)-cluster - CLI implementeren](../../aks/kubernetes-walkthrough.md)

## <a name="configure-the-virtual-networks"></a>De virtuele netwerken configureren

1. Van de [Azure-portal](https://portal.azure.com), selecteer __resourcegroepen__, en gaat u naar de resourcegroep die het virtuele netwerk voor uw cluster AKS bevat. Naam van de resourcegroep is `MC_<resourcegroup>_<akscluster>_<location>`. De `resourcegroup` en `akscluster` vermeldingen zijn de naam van de resourcegroep waarin u het cluster hebt gemaakt en de naam van het cluster. De `location` is de locatie die in het cluster is gemaakt.

2. Selecteer in de resourcegroep, de __virtueel netwerk__ resource.

3. Selecteer __adresruimte__. U ziet de adresruimte weergegeven.

4. Voor het maken van een virtueel netwerk voor HDInsight selecteert __+ maken van een resource__, __Networking__, en vervolgens __virtueel netwerk__.

    > [!IMPORTANT]
    > Wanneer u de waarden voor de nieuwe virtuele netwerk, moet u een adresruimte die niet op het clusternetwerk AKS overlapt.

    Gebruik van dezelfde __locatie__ voor het virtuele netwerk die u gebruikt voor het cluster AKS.

    Wacht totdat het virtuele netwerk is gemaakt voordat u doorgaat met de volgende stap.

5. Selecteer het virtuele netwerk voor het configureren van de peering tussen het HDInsight-netwerk en het clusternetwerk AKS, en selecteer vervolgens __Peerings__. Selecteer __+ toevoegen__ en gebruik de volgende waarden voor het vullen van het formulier:

    * __Naam__: Voer een unieke naam voor deze peering configuratie.
    * __Virtueel netwerk__: dit veld gebruiken om te selecteren voor het virtuele netwerk van de **AKS cluster**.

    Laat alle andere velden op de standaardwaarde en selecteer vervolgens __OK__ voor het configureren van de peering.

6. Voor het configureren van de peering tussen het clusternetwerk AKS en het HDInsight-netwerk, selecteer de __AKS cluster virtueel netwerk__, en selecteer vervolgens __Peerings__. Selecteer __+ toevoegen__ en gebruik de volgende waarden voor het vullen van het formulier:

    * __Naam__: Voer een unieke naam voor deze peering configuratie.
    * __Virtueel netwerk__: dit veld gebruiken om te selecteren voor het virtuele netwerk van de __HDInsight-cluster__.

    Laat alle andere velden op de standaardwaarde en selecteer vervolgens __OK__ voor het configureren van de peering.

## <a name="install-kafka-on-hdinsight"></a>Kafka installeren op HDInsight

Wanneer u de Kafka op HDInsight-cluster maakt, moet u lid worden van het virtuele netwerk eerder hebt gemaakt voor HDInsight. Zie voor meer informatie over het maken van een cluster Kafka de [maken van een cluster Kafka](apache-kafka-get-started.md) document.

> [!IMPORTANT]
> Wanneer u het cluster maakt, moet u de __geavanceerde instellingen__ lid worden van het virtuele netwerk dat u hebt gemaakt voor HDInsight.

## <a name="configure-kafka-ip-advertising"></a>IP-promotie Kafka configureren

Gebruik de volgende stappen voor het configureren van Kafka voor het IP-adressen in plaats van domeinnamen adverteren:

1. Met een webbrowser, gaat u naar https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ met de naam van de Kafka op HDInsight-cluster.

    Wanneer u wordt gevraagd, gebruikt u de HTTPS-gebruikersnaam en het wachtwoord voor het cluster. De Ambari-Webgebruikersinterface voor het cluster wordt weergegeven.

2. Als u informatie op Kafka, selecteer __Kafka__ uit de lijst aan de linkerkant.

    ![Lijst met Kafka service gemarkeerd](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Selecteer om te geven Kafka configuratie, __Configs__ uit het midden van de bovenste.

    ![Koppelingen naar de configuraties voor Kafka](./media/apache-kafka-azure-container-services/select-kafka-config.png)

4. Vinden de __kafka env__ configuratie, voer `kafka-env` in de __Filter__ op de rechterbovenhoek.

    ![Kafka-configuratie voor kafka env](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. Toevoegen voor het configureren van Kafka voor het adverteren van IP-adressen, de volgende tekst naar de onderkant van de __kafka-env-sjabloon__ veld:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Voer voor het configureren van de interface die Kafka luistert op `listeners` in de __Filter__ op de rechterbovenhoek.

7. Als u wilt configureren Kafka om te luisteren op alle netwerkinterfaces, wijzig de waarde in de __listeners__ veld `PLAINTEXT://0.0.0.0:9092`.

8. Voor het opslaan van wijzigingen in de configuratie, gebruiken de __opslaan__ knop. Voer een SMS-bericht met een beschrijving van de wijzigingen. Selecteer __OK__ nadat de wijzigingen zijn opgeslagen.

    ![Configuratie knop Opslaan](./media/apache-kafka-azure-container-services/save-button.png)

9. Gebruiken om fouten te voorkomen bij het opnieuw opstarten Kafka, de __serviceacties__ en selecteer __inschakelen op onderhoudsmodus__. Klik op OK om deze bewerking te voltooien.

    ![Service-acties, met inschakelen onderhoud gemarkeerd](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Als opnieuw Kafka, wilt u de __opnieuw__ en selecteer __start opnieuw alle van invloed op een__. Bevestig het opnieuw opstarten en gebruik vervolgens de __OK__ knop nadat de bewerking is voltooid.

    ![Start opnieuw op de knop met alle van invloed op een opnieuw opstarten gemarkeerd](./media/apache-kafka-azure-container-services/restart-button.png)

11. U schakelt onderhoudsmodus uit de __serviceacties__ en selecteer __inschakelen uit de onderhoudsmodus__. Selecteer **OK** om deze bewerking te voltooien.

## <a name="test-the-configuration"></a>Test de configuratie

Op dit moment zijn Kafka en Azure Container Service in de communicatie via de peered virtuele netwerken. Deze om verbinding te testen, gebruikt u de volgende stappen uit:

1. Maak een Kafka-onderwerp dat wordt gebruikt door de testtoepassing. Zie voor meer informatie over het maken van Kafka onderwerpen de [maken van een cluster Kafka](apache-kafka-get-started.md) document.

2. Downloaden van de voorbeeldtoepassing uit [https://github.com/Blackmist/Kafka-AKS-Test](https://github.com/Blackmist/Kafka-AKS-Test). 

3. Bewerk de `index.js` -bestand en wijzig de volgende regels:

    * `var topic = 'mytopic'`: Vervang `mytopic` met de naam van het onderwerp Kafka is gebruikt door deze toepassing.
    * `var brokerHost = '176.16.0.13:9092`: Vervang `176.16.0.13` met het interne IP-adres van een van de broker-hosts voor uw cluster.

        Het interne IP-adres van de broker hosts (workernodes) in het cluster, Zie de [Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) document. Selecteer IP-adres van een van de vermeldingen waarbij de domeinnaam met begint `wn`.

4. Vanaf de opdrachtregel in de `src` directory, afhankelijkheden installeren en gebruiken van Docker voor het bouwen van een afbeelding voor de implementatie:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]
    > Pakketten die zijn vereist voor deze toepassing zijn geselecteerd in de opslagplaats, dus u niet hoeft te gebruiken de `npm` hulpprogramma ze te installeren.

5. Meld u bij uw Azure Container register (ACR) en de naam loginServer zoeken:

    ```bash
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]
    > Als u niet uw Azure-Container register naam zijn die geen ervaring hebt weet met de Azure CLI gebruiken om te werken met de Azure Container Service raadpleegt u de [AKS zelfstudies](../../aks/tutorial-kubernetes-prepare-app.md).

6. Label van de lokale `kafka-aks-test` installatiekopie met de loginServer van uw ACR. Ook toevoegen `:v1` aan het einde om aan te geven van de installatiekopie-versie:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Push de afbeelding in het register:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```
    Deze bewerking duurt enkele minuten in beslag.

8. Het manifestbestand Kubernetes bewerken (`kafka-aks-test.yaml`) en vervang `microsoft` met de naam van de loginServer ACR opgehaald in stap 4.

9. Gebruik de volgende opdracht om de toepassingsinstellingen uit het manifest te implementeren:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. Gebruik de volgende opdracht om de `EXTERNAL-IP` van de toepassing:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Zodra een extern IP-adres is toegewezen, gebruikt u __CTRL + c drukken__ om af te sluiten van de controle

11. Open een webbrowser en voer het externe IP-adres voor de service. U aankomt bij een pagina zoals in de volgende afbeelding:

    ![Afbeelding van de webpagina](./media/apache-kafka-azure-container-services/test-web-page.png)

12. Voer tekst in het veld en selecteer vervolgens de __verzenden__ knop. De gegevens worden verzonden naar Kafka. De consument Kafka in de toepassing leest het bericht en deze toevoegt aan de __berichten van Kafka__ sectie.

    > [!WARNING]
    > U kunt meerdere exemplaren van een bericht ontvangen. Dit probleem meestal gebeurt er wanneer u uw browser vernieuwt nadat u verbinding hebt, of open verbindingen van meerdere browser naar de toepassing.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)