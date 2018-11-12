---
title: Azure Kubernetes Service gebruiken met Kafka in HDInsight
description: Informatie over het gebruik van Kafka op HDInsight vanaf containerinstallatiekopieën die worden gehost in Azure Kubernetes Service (AKS).
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: aad23f1b50a3156d01ce127270e29368f82d18b3
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014037"
---
# <a name="use-azure-kubernetes-service-with-kafka-on-hdinsight"></a>Azure Kubernetes Service gebruiken met Kafka in HDInsight

Informatie over het gebruik van Azure Kubernetes Service (AKS) met Kafka op HDInsight-cluster. Een Node.js-toepassing die wordt gehost in AKS de stappen in dit document gebruiken om te controleren of verbinding met Kafka. Deze toepassing maakt gebruik van de [kafka-knooppunt](https://www.npmjs.com/package/kafka-node) pakket om te communiceren met Kafka. Hierbij [Socket.io](https://socket.io/) voor op gebeurtenissen gebaseerde uitwisseling van berichten tussen de browserclient en de back-end die wordt gehost in AKS.

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van pijplijnen en toepassingen voor realtime streaming van gegevens. Azure Kubernetes Service beheert uw gehoste Kubernetes-omgeving en kunt u snel en eenvoudig te implementeren toepassingen in containers. Met behulp van een Azure-netwerk, kunt u de twee services.

> [!NOTE]
> De focus van dit document is op de stappen die nodig zijn om in te schakelen Azure Kubernetes Service om te communiceren met Kafka in HDInsight. Het voorbeeld zelf is alleen een eenvoudige Kafka client om aan te tonen dat de configuratie werkt.

## <a name="prerequisites"></a>Vereisten

* [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Een Azure-abonnement

Dit document wordt ervan uitgegaan dat u bekend bent met het maken en gebruiken van de volgende Azure-services:

* Kafka op HDInsight
* Azure Kubernetes Service
* Virtuele netwerken van Azure

Dit document wordt ervan uitgegaan dat u hebt stapsgewijs de [zelfstudie voor Azure Kubernetes Service](../../aks/tutorial-kubernetes-prepare-app.md). In deze zelfstudie maakt u een containerservice, maakt u een Kubernetes-cluster, een containerregister, en configureert u de `kubectl` hulpprogramma.

## <a name="architecture"></a>Architectuur

### <a name="network-topology"></a>Netwerktopologie

HDInsight zowel AKS maken gebruik van een Azure-netwerk als een container voor compute-resources. Om te communiceren tussen HDInsight en AKS, moet u communicatie tussen hun netwerken inschakelen. De stappen in dit document gebruiken Peering in virtuele netwerken met de netwerken. Andere verbindingen, zoals VPN, moeten ook werken. Zie voor meer informatie over peering, de [peering van virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md) document.


Het volgende diagram illustreert de netwerktopologie die in dit document:

![HDInsight in één virtueel netwerk, AKS in een andere en de netwerken die zijn verbonden via peering](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]
> Naamomzetting is niet ingeschakeld tussen de gekoppelde netwerken, zodat het IP-adressering wordt gebruikt. Kafka in HDInsight is standaard geconfigureerd om terug te keren hostnamen in plaats van IP-adressen wanneer clients verbinding maken. De stappen in dit document wordt gewijzigd Kafka voor het gebruik van IP in plaats daarvan adverteren.

## <a name="create-an-azure-kubernetes-service-aks"></a>Maak een Azure Kubernetes Service (AKS)

Als u nog geen een AKS-cluster, kunt u een van de volgende documenten voor meer informatie over het maken van een gebruiken:

* [Een cluster Azure Kubernetes Service (AKS) - Portal implementeren](../../aks/kubernetes-walkthrough-portal.md)
* [Een cluster Azure Kubernetes Service (AKS) - CLI implementeren](../../aks/kubernetes-walkthrough.md)

> [!NOTE]
> AKS maakt een virtueel netwerk tijdens de installatie. Dit netwerk is gekoppeld aan de gemaakt voor HDInsight in de volgende sectie.

## <a name="configure-virtual-network-peering"></a>Peering op virtueel netwerk configureren

1. Uit de [Azure-portal](https://portal.azure.com), selecteer __resourcegroepen__, en gaat u naar de resourcegroep waarin het virtuele netwerk naar uw AKS-cluster. Naam van de resourcegroep is `MC_<resourcegroup>_<akscluster>_<location>`. De `resourcegroup` en `akscluster` vermeldingen zijn de naam van de resourcegroep die u hebt gemaakt met het cluster in, en de naam van het cluster. De `location` is de locatie die in het cluster is gemaakt.

2. Selecteer in de resourcegroep, de __virtueel netwerk__ resource.

3. Selecteer __adresruimte__. Houd er rekening mee de adresruimte die worden vermeld.

4. Voor het maken van een virtueel netwerk voor HDInsight selecteert __+ een resource maken__, __netwerken__, en vervolgens __virtueel netwerk__.

    > [!IMPORTANT]
    > Bij het invoeren van de waarden voor de nieuwe virtuele netwerk, moet u een adresruimte die wordt gebruikt door het AKS-cluster-netwerk niet overlapt.

    Gebruik dezelfde __locatie__ voor het virtuele netwerk dat u hebt gebruikt voor het AKS-cluster.

    Wacht totdat het virtuele netwerk is gemaakt voordat u verdergaat met de volgende stap.

5. Als u wilt configureren voor de peering tussen het HDInsight-netwerk en het AKS-cluster, selecteer het virtuele netwerk en selecteer vervolgens __Peerings__. Selecteer __+ toevoegen__ en de volgende waarden gebruiken voor het vullen van het formulier:

    * __Naam__: Voer een unieke naam voor deze peering-configuratie.
    * __Virtueel netwerk__: dit veld gebruiken om te selecteren van het virtuele netwerk voor de **AKS-cluster**.

    Laat alle andere velden op de standaardwaarde, en selecteer vervolgens __OK__ het configureren van de peering.

6. Als u wilt configureren voor de peering tussen het AKS-cluster-netwerk en het HDInsight-netwerk, selecteert u de __AKS-cluster virtueel netwerk__, en selecteer vervolgens __Peerings__. Selecteer __+ toevoegen__ en de volgende waarden gebruiken voor het vullen van het formulier:

    * __Naam__: Voer een unieke naam voor deze peering-configuratie.
    * __Virtueel netwerk__: dit veld gebruiken om te selecteren van het virtuele netwerk voor de __HDInsight-cluster__.

    Laat alle andere velden op de standaardwaarde, en selecteer vervolgens __OK__ het configureren van de peering.

## <a name="install-kafka-on-hdinsight"></a>Kafka op HDInsight installeren

Bij het maken van het Kafka op HDInsight-cluster, moet u lid worden van het virtuele netwerk eerder hebt gemaakt voor HDInsight. Zie voor meer informatie over het maken van een Kafka-cluster, de [maken van een Kafka-cluster](apache-kafka-get-started.md) document.

> [!IMPORTANT]
> Bij het maken van het cluster, moet u de __geavanceerde instellingen__ voor deelname aan het virtuele netwerk dat u hebt gemaakt voor HDInsight.

## <a name="configure-kafka-ip-advertising"></a>Kafka IP reclame configureren

Gebruik de volgende stappen uit om te configureren van Kafka voor het adverteren van IP-adressen in plaats van domeinnamen:

1. Met behulp van een webbrowser, gaat u naar https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ met de naam van het Kafka in HDInsight-cluster.

    Wanneer u hierom wordt gevraagd, gebruikt u de HTTPS-gebruikersnaam en het wachtwoord voor het cluster. De Ambari-Webinterface voor het cluster wordt weergegeven.

2. Als u informatie op Kafka, selecteer __Kafka__ in de lijst aan de linkerkant.

    ![Lijst met Services met Kafka gemarkeerd](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Als u wilt weergeven van Kafka-configuratie, selecteer __Peeringconfiguraties__ uit het midden.

    ![Koppelingen naar de configuraties voor Kafka](./media/apache-kafka-azure-container-services/select-kafka-config.png)

4. Om te zoeken de __kafka-env__ configuratie, voer `kafka-env` in de __Filter__ veld in de rechterbovenhoek.

    ![Kafka-configuratie voor kafka-env](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. Voor het configureren van Kafka voor het adverteren van IP-adressen, voeg de volgende tekst toe aan de onderkant van de __kafka-env-template__ veld:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Voer voor het configureren van de interface die Kafka luistert, `listeners` in de __Filter__ veld in de rechterbovenhoek.

7. Voor het configureren van Kafka om te luisteren op alle netwerkinterfaces, wijzigt u de waarde in de __listeners__ veld `PLAINTEXT://0.0.0.0:9092`.

8. Om de wijzigingen in de configuratie hebt opgeslagen, gebruikt u de __opslaan__ knop. Voer een SMS-bericht met een beschrijving van de wijzigingen. Selecteer __OK__ zodra de wijzigingen zijn opgeslagen.

    ![Configuratie van de knop Opslaan](./media/apache-kafka-azure-container-services/save-button.png)

9. Gebruik ter voorkoming van fouten bij het opnieuw opstarten van Kafka het __serviceacties__ en selecteer __onderhoudsmodus inschakelen__. Klik op OK om deze bewerking te voltooien.

    ![Service-acties, met inschakelen voor onderhoud gemarkeerd](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Als u wilt starten Kafka, gebruikt u de __opnieuw__ en selecteer __start opnieuw op alle betrokken__. Bevestig het opnieuw starten en gebruik vervolgens de __OK__ knop nadat de bewerking is voltooid.

    ![Start opnieuw op de knop en alle betrokken opnieuw opgestart gemarkeerd](./media/apache-kafka-azure-container-services/restart-button.png)

11. Als u wilt uitschakelen in de onderhoudsmodus, gebruikt u de __serviceacties__ en selecteer __inschakelen uit de onderhoudsmodus__. Selecteer **OK** om deze bewerking te voltooien.

## <a name="test-the-configuration"></a>De configuratie testen

Op dit moment, zijn Kafka en Azure Kubernetes Service in de communicatie via de gekoppelde virtuele netwerken. Deze om verbinding te testen, gebruikt u de volgende stappen uit:

1. Een Kafka-onderwerp dat wordt gebruikt door de testtoepassing maken. Zie voor meer informatie over het maken van Kafka-onderwerpen de [maken van een Kafka-cluster](apache-kafka-get-started.md) document.

2. Download de voorbeeldtoepassing uit [ https://github.com/Blackmist/Kafka-AKS-Test ](https://github.com/Blackmist/Kafka-AKS-Test).

3. Bewerk de `index.js` -bestand en wijzig de volgende regels:

    * `var topic = 'mytopic'`: Vervang `mytopic` met de naam van het Kafka-onderwerp wordt gebruikt door deze toepassing.
    * `var brokerHost = '176.16.0.13:9092`: Vervang `176.16.0.13` met het interne IP-adres van een van de broker-hosts voor uw cluster.

        Het interne IP-adres van de broker hosts (workernodes) in het cluster, Zie de [Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) document. IP-adres van een van de vermeldingen waar de domeinnaam met begint verzamelen `wn`.

4. Vanaf de opdrachtregel in de `src` map, installeer afhankelijkheden en Docker gebruiken om een installatiekopie voor de implementatie te bouwen:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]
    > Pakketten die zijn vereist voor deze toepassing worden gecontroleerd in de opslagplaats, dus u niet hoeft te gebruiken de `npm` hulpprogramma ze te installeren.

5. Meld u aan uw Azure Container Registry (ACR) en zoek de loginServer-naam:

    ```bash
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]
    > Als u niet weet uw naam Azure Container Registry, of zijn niet bekend bent wat met de Azure CLI gebruiken om te werken met de Azure Kubernetes Service, raadpleegt u de [AKS-zelfstudies](../../aks/tutorial-kubernetes-prepare-app.md).

6. Label van de lokale `kafka-aks-test` installatiekopie met de loginServer van uw ACR. Ook toevoegen `:v1` aan het einde om aan te geven van de versie van de installatiekopie:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Push de installatiekopie naar het register:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```
    Met deze bewerking duurt enkele minuten om te voltooien.

8. Bewerken van het Kubernetes-manifestbestand (`kafka-aks-test.yaml`) en vervang `microsoft` met de naam van de ACR-loginServer opgehaald in stap 4.

9. Gebruik de volgende opdracht om de toepassingsinstellingen van het manifest te implementeren:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. Gebruik de volgende opdracht om te bekijken voor de `EXTERNAL-IP` van de toepassing:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Zodra een extern IP-adres is toegewezen, gebruikt u __CTRL + C__ om af te sluiten van het horloge

11. Open een webbrowser en voer in het externe IP-adres voor de service. Wordt er een pagina zoals in de volgende afbeelding uitziet:

    ![Afbeelding van de webpagina wordt weergegeven](./media/apache-kafka-azure-container-services/test-web-page.png)

12. Voer tekst in het veld en selecteer vervolgens de __verzenden__ knop. De gegevens worden verzonden naar Kafka. De Kafka-consument in de toepassing leest het bericht en toevoegt aan de __berichten van Kafka__ sectie.

    > [!WARNING]
    > U kunt meerdere exemplaren van een bericht ontvangen. Dit probleem meestal gebeurt er wanneer u uw browser vernieuwen nadat u verbinding hebt, of meerdere verbindingen van de browser om de toepassing te openen.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)
