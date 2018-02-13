---
title: Azure Functions gebruiken om gegevens te verzenden naar Kafka op HDInsight | Microsoft Docs
description: Informatie over het gebruik van een Azure-functie gegevens schrijven naar Kafka op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2018
ms.author: larryfr
ms.openlocfilehash: c1c03cfcbcb7e0bfdb4a631b9e2ae568f0684069
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="use-kafka-on-hdinsight-from-an-azure-function-app"></a>Kafka op HDInsight gebruiken van een Azure-functie-app

Informatie over het maken van een Azure-functie-app die gegevens naar Kafka op HDInsight verzendt.

[Azure Functions](https://docs.microsoft.com/azure/azure-functions/) is een service zonder server compute. Hiermee kunt u code op aanvraag uitvoeren zonder expliciet inrichten of infrastructuur beheren.

[Apache Kafka](https://kafka.apache.org) is een open-source gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van pijplijnen en toepassingen voor realtime streaming van gegevens. Kafka biedt ook berichtenbrokerfunctionaliteit vergelijkbaar met een berichtenwachtrij, waarmee u benoemde gegevensstromen kunt publiceren en zich erop kunt abonneren. Kafka in HDInsight biedt u een beheerde, zeer schaalbare en maximaal beschikbare service in Microsoft Azure Cloud.

Kafka op HDInsight biedt geen een API op het openbare internet. Als u wilt publiceren of gegevens van Kafka gebruiken, moet u verbinding met het Kafka-cluster met behulp van een Azure-netwerk. Azure Functions biedt een handige manier om een openbaar eindpunt die gegevens in Kafka pushes in HDInsight via een virtuele netwerkgateway te maken.

> [!NOTE]
> De focus van dit document is op de stappen die zijn vereist voor het inschakelen van Azure Functions om te communiceren met Kafka op HDInsight. Het voorbeeld zelf is alleen een eenvoudige Kafka producent om aan te tonen dat de configuratie werkt.

## <a name="prerequisites"></a>Vereisten

* Een Azure-functie-app. Zie voor meer informatie de [maken van uw eerste functie](../../azure-functions/functions-create-first-azure-function.md) documentatie.

* Een Azure-netwerk. Om te werken met Azure Functions, moet het virtuele netwerk een van de volgende IP-adresbereiken gebruiken:

    * 10.0.0.0-10.255.255.255
    * 172.16.0.0-172.31.255.255
    * 192.168.0.0-192.168.255.255

    Zie voor meer informatie de [uw app integreren met een Azure Virtual Network](../../app-service/web-sites-integrate-with-vnet.md) document.

* Een Kafka op HDInsight-cluster. Zie voor meer informatie over het maken van een Kafka op HDInsight-cluster de [maken van een cluster Kafka](apache-kafka-get-started.md) document.

    > [!IMPORTANT]
    > Tijdens de configuratie van het cluster, moet u de __geavanceerde instellingen__ stap de Azure-netwerk en subnet dat gebruikmaakt van HDInsight selecteren. Selecteer het virtuele netwerk en subnet in de vorige stap hebt gemaakt.

    Zie voor meer informatie over Kafka en virtuele netwerken, de [Kafka via een virtueel netwerk verbinding maken met](apache-kafka-connect-vpn-gateway.md) document.

## <a name="architecture"></a>Architectuur

Kafka op HDInsight bevindt zich in een Azure-netwerk. Azure Functions kunnen communiceren met het virtuele netwerk met behulp van een punt-naar-Site-gateway. De volgende afbeelding is een diagram van de topologie van dit netwerk:

![Architectuur van Azure Functions verbinding te maken met HDInsight](./media/apache-kafka-azure-functions/kafka-azure-functions.png)

## <a name="configure-kafka"></a>Kafka configureren

De informatie in deze sectie wordt voorbereid voor het cluster Kafka om gegevens van de Azure-functie te accepteren. Dit omvat de volgende Configuratieacties:

* IP-advertenties
* Verzamelen Kafka Broker IP-adressen
* Maken van een onderwerp Kafka

### <a name="configure-kafka-for-ip-advertising"></a>Kafka configureren voor IP-advertenties

Standaard retourneert Zookeeper de domeinnaam van de beleggingsmakelaars Kafka aan clients. Deze configuratie werkt niet zonder een DNS-server, zoals namen voor het virtuele netwerk kan niet worden omgezet door de client (Azure Functions). Gebruik de volgende stappen voor het configureren van Kafka voor het adverteren van IP-adressen in plaats van domeinnamen voor deze configuratie:

1. Met een webbrowser, gaat u naar https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ met de naam van de Kafka op HDInsight-cluster.

    Wanneer u wordt gevraagd, gebruikt u de HTTPS-gebruikersnaam en het wachtwoord voor het cluster. De Ambari-Webgebruikersinterface voor het cluster wordt weergegeven.

2. Als u informatie op Kafka, selecteer __Kafka__ uit de lijst aan de linkerkant.

    ![Lijst met Kafka service gemarkeerd](./media/apache-kafka-azure-functions/select-kafka-service.png)

3. Selecteer om te geven Kafka configuratie, __Configs__ uit het midden van de bovenste.

    ![Koppelingen naar de configuraties voor Kafka](./media/apache-kafka-azure-functions/select-kafka-config.png)

4. Vinden de __kafka env__ configuratie, voer `kafka-env` in de __Filter__ op de rechterbovenhoek.

    ![Kafka-configuratie voor kafka env](./media/apache-kafka-azure-functions/search-for-kafka-env.png)

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

    ![Configuratie knop Opslaan](./media/apache-kafka-azure-functions/save-button.png)

9. Gebruiken om fouten te voorkomen bij het opnieuw opstarten Kafka, de __serviceacties__ en selecteer __inschakelen op onderhoudsmodus__. Klik op OK om deze bewerking te voltooien.

    ![Service-acties, met inschakelen onderhoud gemarkeerd](./media/apache-kafka-azure-functions/turn-on-maintenance-mode.png)

10. Als opnieuw Kafka, wilt u de __opnieuw__ en selecteer __start opnieuw alle van invloed op een__. Bevestig het opnieuw opstarten en gebruik vervolgens de __OK__ knop nadat de bewerking is voltooid.

    ![Start opnieuw op de knop met alle van invloed op een opnieuw opstarten gemarkeerd](./media/apache-kafka-azure-functions/restart-button.png)

11. U schakelt onderhoudsmodus uit de __serviceacties__ en selecteer __inschakelen uit de onderhoudsmodus__. Selecteer **OK** om deze bewerking te voltooien.

### <a name="get-the-kafka-broker-ip-address"></a>De broker Kafka IP-adres ophalen

Gebruik een van de volgende methoden voor het ophalen van de volledig gekwalificeerde domeinnaam (FQDN) en IP-adressen van de knooppunten in het cluster Kafka:

```powershell
$resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

$clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

$nodes = @()
foreach($nic in $clusterNICs) {
    $node = new-object System.Object
    $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
    $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
    $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
    $nodes += $node
}
$nodes | sort-object Type
```

```azurecli
az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
```

Met deze opdracht wordt ervan uitgegaan dat `<resourcegroupname>` is de naam van de Azure-resourcegroep met het virtuele netwerk.

Selecteer in de lijst met namen die zijn geretourneerd, het IP-adres van een workernode. De interne FQDN-naam van het knooppunt begint met de letters `wn`. Dit IP-adres wordt gebruikt door de functie om te communiceren met Kafka.

### <a name="create-a-kafka-topic"></a>Maken van een onderwerp Kafka

Kafka slaat gegevens op in __onderwerpen__. Voordat het verzenden van gegevens naar Kafka van een Azure-functie, moet u de functie maken.

Als u wilt een onderwerp maakt, gebruikt u de stappen in de [maken van een cluster Kafka](apache-kafka-get-started.md) document.

## <a name="create-a-function-that-sends-to-kafka"></a>Maken van een functie die u naar Kafka verzendt

> [!NOTE]
> De stappen in deze sectie maakt u een JavaScript-functie die gebruikmaakt van de [kafka-knooppunt](https://www.npmjs.com/package/kafka-node) pakket om gegevens te publiceren naar Kafka:

1. Maak een nieuwe __WebHook + API__ functioneren en selecteer __JavaScript__ als de taal. Zie voor meer informatie over het maken van nieuwe functies, de [maken van uw eerste functie](../../azure-functions/functions-create-first-azure-function.md#create-function) document.

2. De volgende code gebruiken als de hoofdtekst van de functie:

    ```javascript
    var kafka = require('kafka-node');

    // The topic and a Kafka broker host for your HDInsight cluster
    var topic = 'test';
    var brokerHost = '10.1.0.7:9092';
    // Create the client
    var client= new kafka.KafkaClient({kafkaHost: brokerHost});

    module.exports = function (context, req) {
        // Create the producer
        var producer = new kafka.Producer(client, {requireAcks: 1});

        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            var name = req.query.name || req.body.name
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + name
            };
            // Create the payload, using the name as the body
            var payloads = [
                    { topic: topic, messages: [name]}
            ];
            context.log("calling producer.send");
            // Send the data to Kafka
            producer.send(payloads, function(err, data) {
                if(err) {
                    context.log(err);
                } else {
                    context.log(data);
                }
            });
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    Vervang `'test'` met de naam van het Kafka-onderwerp dat u op uw HDInsight-cluster gemaakt.

    Vervang `10.1.0.7` met het IP-adres dat u eerder hebt opgehaald. Laat de `:9092` waarde. 9092 is de poort waarop Kafka luistert.

    Gebruik de __opslaan__ knop de wijzigingen wilt opslaan.

    ![Editor met knop Opslaan](./media/apache-kafka-azure-functions/function-editor.png)

3. Selecteer in het recht van de functie editor __bestanden bekijken__. Selecteer __+ toevoegen__ en voeg een nieuw bestand met de naam `package.json`. Dit bestand wordt gebruikt om de afhankelijkheid geven op de `kafka-node` pakket.

    ![Een bestand toevoegen](./media/apache-kafka-azure-functions/add-files.png)

4. Als het nieuwe bestand bewerken, selecteert u `package.json` van de __bestanden bekijken__ lijst. Gebruik de volgende tekst als de inhoud van het bestand:

    ```json
    {
    "name": "kafkatest",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "author": "",
    "license": "ISC",
    "dependencies": {
            "kafka-node": "^2.4.1"
        }
    }
    ```

    Gebruik de __opslaan__ knop de wijzigingen wilt opslaan.

5. Voor het installeren van de `kafka-node` van het pakket, gebruikt u de _knooppunt version- en package management_ sectie van de [Ontwikkelaarshandleiding voor Azure Functions JavaScript](../../azure-functions/functions-reference-node.md#node-version-and-package-management).

    > [!NOTE]
    > Het foutbericht verschillende fouten als het pakket kafka-knooppunt is geïnstalleerd. U kunt deze fouten negeren.

## <a name="run-the-function"></a>De functie uitvoeren

Selecteer in het recht van de functie editor __Test__. Laat u de standaardinstellingen voor de test en selecteer vervolgens __uitvoeren__. Als de test wordt uitgevoerd, geeft een `name` -parameter voor de functie. Deze parameter bevat een waarde van `Azure`, die de functie wordt ingevoegd in Kafka.

![Test-dialoogvenster-schermafbeelding](./media/apache-kafka-azure-functions/function-test-dialog.png)

Als u informatie die zijn vastgelegd door de functie terwijl de test wordt uitgevoerd, selecteert u __logboeken__ aan de onderkant van de pagina. De test om logboekgegevens genereren opnieuw te worden uitgevoerd.

![Voorbeeld van de uitvoer van de functie](./media/apache-kafka-azure-functions/function-log.png)

## <a name="verify-the-data-is-in-kafka"></a>Controleer of de gegevens in Kafka

Om te bevestigen dat de gegevens in het onderwerp Kafka ontvangen, gebruik de informatie in de _produceren en gebruiken van records_ sectie van de [maken van een cluster Kafka](apache-kafka-get-started.md#produce-and-consume-records) document. De `kafka-console-consumer` gegevens van het onderwerp leest en geeft een lijst met berichten die zijn opgeslagen in het onderwerp.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppelingen voor meer informatie over het gebruik van Apache Kafka in HDInsight:

* [Aan de slag met Kafka in HDInsight](apache-kafka-get-started.md)

* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](apache-kafka-mirroring.md)

* [Apache Storm gebruiken met Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark gebruiken met Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Verbinding maken met Kafka via een Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)