---
title: Met Apache Storm en HBase-sensorgegevens analyseren | Microsoft Docs
description: Informatie over het met een virtueel netwerk verbinding maken met Apache Storm. Storm gebruiken met HBase sensor om gegevens te verwerken van een event hub en deze te visualiseren met D3.js.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/19/2017
ms.author: larryfr
ms.openlocfilehash: 8c8cda26f2b9b564dee330e4883ec12f39feb652
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analyseren van sensorgegevens met Apache Storm, Event Hub en HBase in HDInsight (Hadoop)

Informatie over het gebruik van Apache Storm op HDInsight om te verwerken van sensorgegevens uit Azure Event Hub. De gegevens vervolgens naar Apache HBase in HDInsight wordt opgeslagen en weergegeven met behulp van D3.js.

De Azure Resource Manager-sjabloon die wordt gebruikt in dit document wordt gedemonstreerd hoe meerdere Azure-resources in een resourcegroep maken. De sjabloon maakt u een Azure-netwerk, twee HDInsight-clusters (Storm en HBase) en een Azure-Web-App. Een node.js-implementatie van een realtime webdashboard is automatisch geïmplementeerd naar de web-app.

> [!NOTE]
> De informatie in dit document en deze in dit document HDInsight versie 3.6 vereisen.
>
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="architecture"></a>Architectuur

![Architectuurdiagram](./media/apache-storm-sensor-data-analysis/devicesarchitecture.png)

In dit voorbeeld bestaat uit de volgende onderdelen:

* **Azure Event Hubs**: bevat gegevens die worden verzameld van sensoren.
* **Storm op HDInsight**: biedt realtime verwerking van gegevens uit Event Hub.
* **HBase in HDInsight**: biedt een permanente NoSQL-gegevensopslag voor gegevens nadat deze is verwerkt door Storm.
* **Azure Virtual Network service**: beveiligde communicatie tussen de Storm op HDInsight en HBase op HDInsight-clusters mogelijk maakt.
  
  > [!NOTE]
  > Een virtueel netwerk is vereist wanneer de client Java HBase API. Het is niet toegankelijk via de openbare gateway voor HBase-clusters. HBase en Storm-clusters in hetzelfde virtuele netwerk te installeren, kunt de Storm-cluster (of elk ander systeem in het virtuele netwerk) rechtstreeks toegang krijgen tot HBase met client-API.

* **Dashboard website**: een voorbeeld van dashboard dat gegevens in real-time grafieken.
  
  * De website is geïmplementeerd in Node.js.
  * [Socket.IO](http://socket.io/) voor realtime-communicatie tussen de Storm-topologie en de website wordt gebruikt.
    
    > [!NOTE]
    > Met Socket.io voor communicatie is een implementatiedetail. U kunt elk framework communicatie zoals onbewerkte WebSockets of SignalR gebruiken.

  * [D3.js](http://d3js.org/) wordt gebruikt voor de gegevens die worden verzonden naar de website in een grafiek.

> [!IMPORTANT]
> Twee clusters zijn vereist, omdat er geen ondersteunde methode is voor het maken van een HDInsight-cluster voor Storm en HBase.

De topologie leest de gegevens uit Event Hub met behulp van de `org.apache.storm.eventhubs.spout.EventHubSpout` klasse en schrijft gegevens in HBase met behulp van de `org.apache.storm.hbase.bolt.HBaseBolt` klasse. Communicatie met de website kan worden bereikt via [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Het volgende diagram wordt de indeling van de topologie uitgelegd:

![topologiediagram](./media/apache-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Dit diagram is een vereenvoudigde weergave van de topologie. Een exemplaar van elk onderdeel is gemaakt voor elke partitie in uw Event Hub. Deze instanties zijn verdeeld over de knooppunten in het cluster en gegevens worden gerouteerd tussen deze als volgt:
> 
> * Gegevens van de spout naar de parser gelijkmatig wordt verdeeld.
> * Gegevens van de parser naar het Dashboard en de HBase worden gegroepeerd per apparaat-ID, zodat berichten van hetzelfde apparaat altijd naar hetzelfde onderdeel stromen.

### <a name="topology-components"></a>Topologie-onderdelen

* **Event Hub Spout**: de spout is opgegeven als onderdeel van Apache Storm versie 0.10.0 of hoger.
  
  > [!NOTE]
  > De Event Hub spout gebruikt in dit voorbeeld is een Storm op HDInsight-cluster versie 3.5 of 3.6 vereist.

* **ParserBolt.java**: de gegevens die worden verstrekt door de spout is onbewerkte JSON en van tijd tot tijd meer dan een gebeurtenis op een tijdstip is verzonden. Deze bolt leest de gegevens die door de spout en parseert het JSON-bericht. De bolt verzendt vervolgens de gegevens als een tuple die meerdere velden bevat.
* **DashboardBolt.java**: dit onderdeel wordt getoond hoe u van de clientbibliotheek Socket.io voor Java gegevens in realtime te verzenden naar de webdashboard.
* **Er is geen hbase.yaml**: de definitie van de topologie bij uitvoering in lokale modus gebruikt. Het wordt HBase-onderdelen gebruikt.
* **met hbase.yaml**: de definitie van de topologie die wordt gebruikt bij het uitvoeren van de topologie op het cluster. Dit maakt gebruik van HBase-onderdelen.
* **dev.Properties**: de configuratiegegevens voor de Event Hub spout, HBase bolt en dashboard onderdelen.

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

Voordat u dit voorbeeld gebruiken, moet u uw ontwikkelomgeving voorbereiden. U moet ook een Azure Event Hub die wordt gebruikt door dit voorbeeld maken.

U moet de volgende items op uw ontwikkelomgeving geïnstalleerd:

* [Node.js](http://nodejs.org/): gebruikt om een voorbeeld van het webdashboard lokaal op uw ontwikkelomgeving.
* [Java en de JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): gebruikt voor het ontwikkelen van de Storm-topologie.
* [Maven](http://maven.apache.org/what-is-maven.html): gebruikt voor het bouwen en compileren van het project.
* [GIT](http://git-scm.com/): gebruikt voor het downloaden van het project vanuit GitHub.
* Een **SSH** client: verbinding maken met de Linux gebaseerde HDInsight-clusters. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Gebruik de stappen in voor het maken van een event hub [maken van een event hub](../../event-hubs/event-hubs-create.md) document.

> [!IMPORTANT]
> De naam van de event hub, naamruimte en de sleutel voor de RootManageSharedAccessKey opslaan. Deze informatie wordt gebruikt voor het configureren van de Storm-topologie.

U hoeft niet in een HDInsight-cluster. De stappen in dit document bevatten een Azure Resource Manager-sjabloon die de resources die nodig is voor dit voorbeeld maakt. De sjabloon wordt gemaakt van de volgende bronnen:
 
* Een Azure-netwerk
* Een Storm op HDInsight-cluster (op basis van Linux twee worker-knooppunten)
* Een HBase op HDInsight-cluster (op basis van Linux twee worker-knooppunten)
* Een Azure-Web-App die als host fungeert voor de webdashboard

## <a name="download-and-configure-the-project"></a>Downloaden en configureren van het project

Gebruik de volgende voor het downloaden van het project vanuit GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nadat de opdracht is voltooid, hebt u de volgende mapstructuur:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> Dit document niet naar alle details van de code die is opgenomen in dit voorbeeld. De code is echter volledig toegelicht.

Voor het configureren van het project lezen uit Event Hub, opent u de `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` -bestand en de informatie van uw Event Hub toevoegen aan de volgende regels:

```bash
eventhub.policy.key: the_key_for_RootManageSharedAccessKey
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Compileren en lokaal te testen

> [!IMPORTANT]
> De topologie lokaal, moet een werkende Storm-ontwikkelomgeving. Zie voor meer informatie [een Storm-ontwikkelomgeving instellen](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) op Apache.org.

Voordat u test, moet u het dashboard voor het weergeven van de uitvoer van de topologie en genereren van gegevens op te slaan in de Event Hub starten.

> [!IMPORTANT]
> Het HBase-onderdeel van deze topologie is niet actief bij lokaal te testen. De Java-API voor het HBase-cluster niet toegankelijk van buiten het virtuele netwerk van Azure die de clusters bevat.

### <a name="start-the-web-application"></a>De webtoepassing starten

1. Open een opdrachtprompt en wijzig de mappen te `hdinsight-eventhub-example/dashboard`. Gebruik de volgende opdracht voor het installeren van de afhankelijkheden die nodig is voor de webtoepassing:
   
    ```bash
    npm install
    ```

2. Gebruik de volgende opdracht om de webtoepassing te starten:
   
    ```bash
    node server.js
    ```
   
    U ziet een bericht dat lijkt op de volgende tekst:
   
        Server listening at port 3000

3. Open een webbrowser en voer `http://localhost:3000/` als het adres. Een pagina zoals in de volgende afbeelding wordt weergegeven:
   
    ![webdashboard](./media/apache-storm-sensor-data-analysis/emptydashboard.png)
   
    Laat deze opdrachtprompt geopend. Nadat u hebt getest, gebruik Ctrl + C om te stoppen van de webserver.

### <a name="generate-data"></a>Gegevens moeten genereren

> [!NOTE]
> De stappen in deze sectie gebruiken Node.js, zodat ze kunnen worden gebruikt op elk platform. Zie voor voorbeelden van andere taal de `SendEvents` directory.

1. Open een nieuw opdrachtprompt, shell of terminal en wijzig de mappen te `hdinsight-eventhub-example/SendEvents/nodejs`. Gebruik de volgende opdracht voor het installeren van de afhankelijkheden die nodig is voor de toepassing:

    ```bash
    npm install
    ```

2. Open de `app.js` -bestand in een teksteditor en voeg de Event Hub-informatie die u eerder hebt verkregen toe:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'Your-eventhub-namespace';
    // Event Hub Name
    var hubname ='Your-eventhub-name';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'RootManageSharedAccessKey';
    var my_key = 'Your-Key';
    ```
   
   > [!NOTE]
   > In dit voorbeeld wordt ervan uitgegaan dat u gebruikmaakt van `RootManageSharedAccessKey` voor toegang tot uw event hub.

3. Gebruik de volgende opdracht voor het invoegen van nieuwe vermeldingen in de Event Hub:
   
    ```bash
    node app.js
    ```
   
    Er zijn meerdere regels van uitvoer met de gegevens verzonden naar de Event Hub:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a>Bouwen en starten van de topologie

1. Open een nieuw opdrachtpromptvenster en wijzig de mappen te `hdinsight-eventhub-example/TemperatureMonitor`. Voor het bouwen en de topologie van het pakket, gebruikt u de volgende opdracht: 

    ```bash
    mvn clean package
    ```

2. Voor het starten van de topologie in de lokale modus, moet u de volgende opdracht gebruiken:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`Start de topologie in lokale modus.
    * `--filter`maakt gebruik van de `dev.properties` bestand voor het vullen van de parameters in de definitie van de topologie.
    * `resources/no-hbase.yaml`maakt gebruik van de `no-hbase.yaml` topologie definitie.
 
   Eenmaal gestart, wordt de topologie vermeldingen van Event Hub leest en zendt deze naar het dashboard op uw lokale computer uitgevoerd. Hier ziet u regels worden weergegeven in het webdashboard, vergelijkbaar met de volgende afbeelding:
   
    ![dashboard met gegevens](./media/apache-storm-sensor-data-analysis/datadashboard.png)

2. Terwijl het dashboard wordt uitgevoerd, gebruikt u de `node app.js` opdracht van de vorige stappen voor het nieuwe gegevens verzenden naar Event Hubs. Omdat de temperatuur-waarden worden willekeurig gegenereerd, de grafiek moet worden bijgewerkt voor het weergeven van grote wijzigingen in de temperatuur.
   
   > [!NOTE]
   > U moet zich in de **hdinsight-eventhub-voorbeeld/SendEvents/Nodejs** directory wanneer u de `node app.js` opdracht.

3. Nadat u hebt gecontroleerd dat het dashboard worden bijgewerkt, stopt u de topologie met Ctrl + C. Ctrl + C kunt u ook de lokale webserver stoppen.

## <a name="create-a-storm-and-hbase-cluster"></a>Een Storm en HBase-cluster maken

De stappen in deze sectie gebruiken een [Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-template-deploy.md) voor het maken van een virtueel Azure-netwerk en een Storm en HBase-cluster op het virtuele netwerk. De sjabloon ook een Azure-Web-App maakt en implementeert een kopie van het dashboard in de App.

> [!NOTE]
> Een virtueel netwerk wordt gebruikt zodat de topologie die wordt uitgevoerd op het Storm-cluster kan rechtstreeks communiceren met de HBase-cluster met de HBase-Java-API.

De Resource Manager-sjabloon gebruikt in dit document bevindt zich in een openbare blob-container op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Klik op de knop Volgende om te melden bij Azure en open de Resource Manager-sjabloon in de Azure-portal.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/apache-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Van de **aangepaste implementatie** sectie, voer de volgende waarden:
   
    ![HDInsight-parameters](./media/apache-storm-sensor-data-analysis/parameters.png)
   
   * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Storm en HBase-clusters. Bijvoorbeeld, voeren **abc** maakt een Storm-cluster met de naam **storm abc** en een HBase-cluster met de naam **hbase abc**.
   * **Aanmeldingsnaam van gebruiker cluster**: de gebruikersnaam van de beheerder voor de Storm en HBase-clusters.
   * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de Storm en HBase-clusters.
   * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor de Storm en HBase-clusters.
   * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Storm en HBase-clusters.
   * **Locatie**: de regio die in de clusters worden gemaakt.
     
     Klik op **OK** om de parameters op te slaan.

3. Gebruik de **basisbeginselen** sectie een resourcegroep maken of een bestaande set selecteren.
4. In de **locatie voor resourcegroep** vervolgkeuzemenu Selecteer dezelfde locatie als u geselecteerd voor de **locatie** parameter in de **instellingen** sectie.
5. Lees de voorwaarden en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.
6. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

Zodra de resources zijn gemaakt, wordt informatie over de resourcegroep weergegeven.

![De resourcegroep voor het vnet en clusters](./media/apache-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Merk op dat de namen van de HDInsight-clusters **storm BASENAME** en **hbase BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven in de sjabloon. U gebruikt deze namen in een latere stap bij het verbinden met de clusters. Ook Let op: de naam van de dashboardsite is **basename dashboard**. Deze waarde wordt verderop in dit document gebruikt.

## <a name="configure-the-dashboard-bolt"></a>Het Dashboard bolt configureren

Om gegevens te verzenden naar het dashboard dat is geïmplementeerd als een web-app, moet u de volgende regel in wijzigen de `dev.properties`bestand:

```yaml
dashboard.uri: http://localhost:3000
```

Wijziging `http://localhost:3000` naar `http://BASENAME-dashboard.azurewebsites.net` en sla het bestand. Vervang **BASENAME** met de basisnaam die u hebt opgegeven in de vorige stap. Ook kunt u de resourcegroep die eerder hebt gemaakt om te selecteren van het dashboard en de URL.

## <a name="create-the-hbase-table"></a>De HBase-tabel maken

Voor het opslaan van gegevens in HBase, moeten we eerst een tabel maken. Resources die Storm nodig heeft om te schrijven naar vooraf maken, als bij het maken van bronnen van binnen een Storm topologie kan leiden tot meerdere exemplaren wilt maken van dezelfde resource. Maken van de bronnen buiten de topologie en Storm gebruiken voor lezen/schrijven en analyses.

1. SSH gebruiken voor verbinding met de HBase-cluster met behulp van de SSH-gebruiker en het wachtwoord die u hebt opgegeven in de sjabloon tijdens het maken van het cluster. Bijvoorbeeld, als u verbinding maakt met behulp van de `ssh` opdracht, gebruikt u de volgende syntaxis:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Vervang `sshuser` met de SSH-gebruikersnaam die u hebt opgegeven bij het maken van het cluster. Vervang `clustername` met de naam van het HBase-cluster.

2. Start de HBase-shell op de SSH-sessie.
   
    ```bash
    hbase shell
    ```
   
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` prompt.

3. Voer de volgende opdracht om een tabel voor het opslaan van de sensorgegevens maken vanuit de HBase-shell:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Controleer of dat de tabel is gemaakt met behulp van de volgende opdracht:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Hiermee wordt informatie vergelijkbaar met het volgende voorbeeld, wat betekent dat er 0 rijen in de tabel.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Voer `exit` om af te sluiten van de HBase-shell:

## <a name="configure-the-hbase-bolt"></a>De HBase-bolt configureren

Voor het schrijven naar HBase uit het Storm-cluster, moet u de HBase-bolt opgeven met de configuratiedetails van uw HBase-cluster.

1. Gebruik een van de volgende voorbeelden voor het ophalen van het quorum Zookeeper voor uw HBase-cluster:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Vervang `your_HDInsight_cluster_name` door de naam van uw HDInsight-cluster. Voor meer informatie over het installeren van de `jq` hulpprogramma, Zie [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Voer desgevraagd het wachtwoord voor aanmelding bij de HDInsight-beheerder.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name'
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Vervang ' your_HDInsight_cluster_name met de naam van uw HDInsight-cluster. Voer desgevraagd het wachtwoord voor aanmelding bij de HDInsight-beheerder.
    >
    > Dit voorbeeld vereist Azure PowerShell. Zie voor meer informatie over het gebruik van Azure PowerShell [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    De informatie die wordt geretourneerd door deze voorbeelden is vergelijkbaar met de volgende tekst:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Deze informatie wordt gebruikt door Storm om te communiceren met de HBase-cluster.

2. Wijzig de `dev.properties` -bestand en de Zookeeper quorum informatie toevoegen aan de volgende regel:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Bouwen, pakket, en de oplossing implementeren in HDInsight

Gebruik de volgende stappen voor het implementeren van de Storm-topologie met het storm-cluster in uw ontwikkelomgeving.

1. Van de `TemperatureMonitor` directory, gebruik de volgende opdracht voor het uitvoeren van een nieuwe build en maak een JAR-pakket van uw project:
   
        mvn clean package
   
    Deze opdracht maakt u een bestand met de naam `TemperatureMonitor-1.0-SNAPSHOT.jar in the `doel ' map van uw project.

2. Scp gebruiken voor het uploaden van de `TemperatureMonitor-1.0-SNAPSHOT.jar` en `dev.properties` bestanden naar uw Storm-cluster. Vervang in het volgende voorbeeld wordt `sshuser` met de SSH-gebruiker die u hebt opgegeven bij het maken van het cluster en `clustername` met de naam van uw Storm-cluster. Voer desgevraagd het wachtwoord voor de SSH-gebruiker.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Dit kan enige tijd duren om de bestanden te uploaden.

    Voor meer informatie over het gebruik van de `scp` en `ssh` opdrachten met HDInsight, Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

3. Zodra het bestand zijn geüpload, moet u een verbinding maken met de Storm-cluster via SSH.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` met de SSH-gebruikersnaam. Vervang `clustername` met de naam van het Storm-cluster.

4. Gebruik de volgende opdracht bij de SSH-sessie voor het starten van de topologie:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`verzendt de topologie van de Nimbus-service, die u het naar de supervisor knooppunten in het cluster distribueert.
    * `--filter`maakt gebruik van de `dev.properties` bestand voor het vullen van de parameters in de definitie van de topologie.
    * `-R /with-hbase.yaml`maakt gebruik van de `with-hbase.yaml` topologie die zijn opgenomen in het pakket.

5. Nadat de topologie is gestart, open een browser naar de website die u in Azure hebt gepubliceerd, gebruikt u vervolgens de `node app.js` opdracht om gegevens te verzenden naar Event Hub. Hier ziet u de webdashboard bijwerken om de informatie weer te geven.
   
    ![dashboard](./media/apache-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase-gegevens weergeven

Gebruik de volgende stappen voor het verbinding maken met HBase en te controleren dat de gegevens is geschreven naar de tabel:

1. SSH gebruiken voor verbinding met de HBase-cluster.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Vervang `sshuser` met de SSH-gebruikersnaam. Vervang `clustername` met de naam van het HBase-cluster.

2. Start de HBase-shell op de SSH-sessie.
   
    ```bash
    hbase shell
    ```
   
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` prompt.

3. Weergave rijen uit de tabel:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Met deze opdracht retourneert informatie vergelijkbaar met de volgende tekst, die aangeeft dat er gegevens in de tabel.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Deze scanbewerking retourneert maximaal 10 rijen uit de tabel.

## <a name="delete-your-clusters"></a>Uw clusters verwijderen

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Als u wilt de clusters-, opslag- en web-app in één keer verwijderen, de resourcegroep waarin ze te verwijderen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeelden van Storm-topologieën met HDInsight [voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md)

Zie voor meer informatie over de Apache Storm, de [Apache Storm](https://storm.incubator.apache.org/) site.

Zie voor meer informatie over HBase in HDInsight, het [HBase met HDInsight Overview](../hbase/apache-hbase-overview.md).

Zie voor meer informatie over Socket.io de [socket.io](http://socket.io/) site.

Zie voor meer informatie over D3.js [D3.js - documenten met aangestuurd](http://d3js.org/).

Zie voor meer informatie over het maken van topologieën in Java [ontwikkelen van Java-topologieën voor Apache Storm op HDInsight](apache-storm-develop-java-topology.md).

Zie voor meer informatie over het maken van topologieën in .NET [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
