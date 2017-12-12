---
title: Azure-netwerk groep stroom beveiligingslogboeken - analyseren Graylog | Microsoft Docs
description: Informatie over het beheren en analyseren van netwerk groep stroom beveiligingslogboeken in Azure met behulp van de netwerk-Watcher en Graylog.
services: network-watcher
documentationcenter: na
author: mareat
manager: vitinnan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: mareat
ms.openlocfilehash: 8d82ffa84c3d75ec3acd102a2de2bdce3718a995
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-and-analyze-network-security-group-flow-logs-in-azure-using-network-watcher-and-graylog"></a>Beheren en analyseren van netwerk groep stroom beveiligingslogboeken in Azure met behulp van de netwerk-Watcher en Graylog

[Netwerk beveiligingslogboeken groep stroom](network-watcher-nsg-flow-logging-overview.md) bevatten informatie die u gebruiken kunt om te begrijpen van inkomende en uitgaande IP-verkeer voor Azure netwerkinterfaces. Stroom logboeken tonen binnenkomende en uitgaande stromen op een per regel basis voor network security groep, de netwerkinterface de stroom van toepassing, 5-tuple informatie (bron/het doel-IP, bron/het doel-poort, het Protocol) over de stroom en als het verkeer is toegestaan of geweigerd .

U kunt veel netwerkbeveiligingsgroepen hebben in uw netwerk met de stroom-logboekregistratie is ingeschakeld. Aantal netwerkbeveiligingsgroepen met stroom-logboekregistratie is ingeschakeld, kunnen u omslachtige parseren en Verkrijg inzicht in uw Logboeken. In dit artikel biedt een oplossing voor het centraal beheren deze netwerk groep stroom beveiligingslogboeken met Graylog, een open-source log-beheer en analysehulpprogramma en Logstash, een open-source serverzijde gegevensverwerking-pijplijn.

## <a name="scenario"></a>Scenario

Groep netwerk-stroom beveiligingslogboeken zijn ingeschakeld met behulp van netwerk-Watcher. Stroom logboeken stroom in het vak naar Azure blob storage. Een invoegtoepassing Logstash wordt gebruikt om verbinding te en verwerken van Logboeken van de stroom van blob-opslag en ze verzenden naar Graylog. Zodra de stroom-logboeken worden opgeslagen in Graylog, kunnen worden geanalyseerd en weergegeven in de aangepaste dashboards.

! [Graylog werkstroom]] (. / media/network-watcher-analyze-nsg-flow-logs-graylog/workflow.png)

## <a name="installation-steps"></a>Installatiestappen

### <a name="enable-network-security-group-flow-logging"></a>Network security groep stroom logboekregistratie inschakelen

In dit scenario moet u network security groep stroom logboekregistratie is ingeschakeld voor de beveiligingsgroep ten minste één netwerk in uw account hebben. Voor instructies over het inschakelen van netwerk beveiligingslogboeken groep stroom wordt verwijzen naar het volgende artikel [Inleiding tot registratie van de stroom voor netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

### <a name="setting-up-graylog"></a>Graylog instellen

In dit voorbeeld worden zowel Graylog en Logstash geconfigureerd op een virtuele Ubuntu 14.04-Server geïmplementeerd in Azure.

- Raadpleeg de [documentatie](http://docs.graylog.org/en/2.2/pages/installation/os/ubuntu.html) van Graylog, voor stapsgewijze instructies voor het installeren op Ubuntu.
- Zorg ervoor dat u de webinterface Graylog ook configureren door de [documentatie](http://docs.graylog.org/en/2.2/pages/configuration/web_interface.html#configuring-webif).

In dit voorbeeld wordt de minimale Graylog-instellingen (Internet Explorer slechts één exemplaar van een Graylog), maar Graylog kunnen worden ontworpen om te schalen verschillende resources afhankelijk van uw systeem en productie behoeften. Zie voor meer informatie over de architectuur of een diep architectuur gids van Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/architecture.html) en [architectuur handleiding](https://www.slideshare.net/Graylog/graylog-engineering-design-your-architecture).

Graylog kan worden geïnstalleerd op tal van manieren afhankelijk van uw platform en voorkeuren. Raadpleeg voor een volledige lijst van mogelijke installatiemethoden van Graylog officiële [documentatie](http://docs.graylog.org/en/2.2/pages/installation.html). De Graylog servertoepassing wordt uitgevoerd op Linux-distributies en heeft de volgende vereisten:

-   Oracle Java SE 8 of hoger – [documentatie voor de installatie van Oracle](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
-   Elastische 2.x zoeken (2.1.0 of hoger)- [Elasticsearch installatie documentatie](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/_installation.html)
-   MongoDB 2.4 of hoger: [MongoDB-documentatie voor installatie](https://docs.mongodb.com/manual/administration/install-on-linux/)

### <a name="install-logstash"></a>Logstash installeren

Logstash wordt gebruikt om de logboeken van de stroom JSON-indeling naar een stroom tuple niveau plat. De logboeken van de stroom plat vergemakkelijkt de logboeken organiseren en te zoeken in de Graylog.

1.  Als u wilt installeren Logstash, voer de volgende opdrachten:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2.  Logstash voor het parseren van de stroom-logboeken en ze verzenden naar Graylog configureren. Maak een bestand Logstash.conf:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3.  De volgende inhoud toevoegen aan het bestand. Wijzig de gemarkeerde waarden om de details van uw opslag:

    ```
    input {
        azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "NrUZmx7pJSKaRJzvQbeiZWi5nBRWOTr7Wwr9DrvK7YtDBrADYxT1y0oEExtSlkDnGRt7qcRiZzEBCCyRYND8SxSt"
            container => "insights-logs-networksecuritygroupflowevent"
            registry_create_policy => "start_over"
            codec => "json"
            file_head_bytes => 21
            file_tail_bytes => 9
            # Possible options: `do_not_break`, `with_head_tail`, `without_head_tail`
            break_json_down_policy  => 'with_head_tail'
            break_json_batch_count => 2
            interval => 5
        }
    }
    
    filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"
    }
    
     mutate {
        split => { "[records][resourceId]" => "/"}
        add_field =>{
                    "Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"
        }
        convert => {"Subscription" => "string"}
        convert => {"ResourceGroup" => "string"}
        convert => {"NetworkSecurityGroup" => "string"}
        split => { "[records][properties][flows][flows][flowTuples]" => ","}
        add_field => {
                    "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                    "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                    "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                    "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                    "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                    "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                    "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                    "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
        }
        add_field => {
                    "time" => "%{[records][time]}"
                    "systemId" => "%{[records][systemId]}"
                    "category" => "%{[records][category]}"
                    "resourceId" => "%{[records][resourceId]}"
                    "operationName" => "%{[records][operationName}}"
                    "Version" => "%{[records][properties][Version}}"
                    "rule" => "%{[records][properties][flows][rule]}"
                    "mac" => "%{[records][properties][flows][flows][mac]}"
        }
        convert => {"unixtimestamp" => "integer"}
        convert => {"srcPort" => "integer"}
        convert => {"destPort" => "integer"}
        add_field => { "message" => "%{Message}" }
    }
        date {
            match => ["unixtimestamp" , "UNIX"]
        }
    }
    output {
        stdout { codec => rubydebug }
        udp {
            host => "127.0.0.1"
            port => 12201
        }
    }
    ```
Het opgegeven Logstash-configuratiebestand bestaat uit drie delen: de invoer-, filter- en uitvoer. De invoer sectie geeft de invoerbron van de logboeken die Logstash verwerkt: in dit geval gaan we gebruiken een Azure-blog invoer-invoegtoepassing (geïnstalleerd in de volgende stappen) waarmee we voor toegang tot de stroom van network security groep JSON logboekbestanden opgeslagen in blob storage.

De sectie filteren vervolgens elk logboekbestand stroom worden samengevoegd zodat elke tuple afzonderlijke stroom en de bijbehorende eigenschappen verandert in een aparte Logstash-gebeurtenis.

Ten slotte, verzendt de sectie uitvoer elke gebeurtenis Logstash naar de server Graylog. Aanpassen aan uw specifieke bedrijfsbehoeften, wijzigt het configuratiebestand Logstash, zoals vereist.

   > [!NOTE]
   > Het vorige configuratiebestand wordt ervan uitgegaan dat de Graylog-server is geconfigureerd op de lokale host loopback-IP-adres 127.0.0.1. Als dat niet het geval is, moet u de parameter host in de sectie uitvoer naar het juiste IP-adres wijzigen.

Zie voor meer informatie over het installeren van Logstash de Logstash [documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plug-in-for-azure-blob-storage"></a>Installeren van de invoer Logstash invoegtoepassing voor Azure blob-opslag

De Logstash-invoegtoepassing kunt u rechtstreeks toegang krijgen tot de logboeken van de stroom van hun aangewezen blob storage-account. Voor het installeren van de invoegtoepassing van de standaardinstallatiemap van Logstash (in dit geval /usr/share/logstash/bin), voer de volgende opdracht:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Zie voor meer informatie over deze plug in de [documentatie](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="set-up-connection-from-logstash-to-graylog"></a>Verbinding van Logstash instellen voor Graylog

Nu dat we hebben een verbinding naar de stroom-logboeken met behulp van Logstash en de server Graylog instellen, moet we Graylog voor het accepteren van de binnenkomende logboekbestanden configureren.

1.  Navigeer naar uw web-interface Graylog Server via de URL die u hebt geconfigureerd voor het. U kunt toegang krijgen tot de interface door uw browser`http://<graylog-server-ip>:9000/`

2.  Om te navigeren naar de configuratiepagina, selecteer de **System** vervolgkeuzelijst in de bovenste navigatiebalk balk naar rechts en klik vervolgens op **invoer**.
    U kunt ook gaat u naar`http://<graylog-server-ip>:9000/system/inputs`

    ![Aan de slag](./media/network-watcher-analyze-nsg-flow-logs-graylog/getting-started.png)

3.  Start de nieuwe invoer, selecteer *GELF UDP* in de **Selecteer invoer** vervolgkeuzelijst en vervolgens het formulier invullen. GELF staat voor Graylog uitgebreide indeling voor logboekbestanden. De indeling GELF is ontwikkeld door Graylog. Zie voor meer informatie over de voordelen, de Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/gelf.html).

    Zorg voor het binden van de invoer van het IP-adres dat u op uw server Graylog geconfigureerd. Het IP-adres moet overeenkomen met de **host** veld van de UDP-uitvoer van het configuratiebestand Logstash. De standaardpoort moet *12201*. Zorg ervoor dat de poort komt overeen met de **poort** veld in de UDP-uitvoer aangegeven in het configuratiebestand Logstash.

    ![Invoer](./media/network-watcher-analyze-nsg-flow-logs-graylog/inputs.png)

    Als u de invoer start, ziet u het weergegeven onder de **lokale invoer** sectie, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/network-watcher-analyze-nsg-flow-logs-graylog/local-inputs.png)

    Raadpleeg voor meer informatie over Graylog berichtinvoer, de [documentatie](http://docs.graylog.org/en/2.2/pages/sending_data.html#what-are-graylog-message-inputs).

4.  Wanneer deze configuraties zijn aangebracht, kunt u beginnen met Logstash om te beginnen met lezen in Logboeken van de stroom met de volgende opdracht:

    `sudo systemctl start logstash.service`

### <a name="search-through-graylog-messages"></a>Graylog berichten doorzoeken

Na enige tijd voor uw server Graylog berichten verzamelt toe te staan, zich u voor het doorzoeken van de berichten. Controleer de berichten worden verzonden naar uw server Graylog vanaf de **invoer** configuratie pagina op de '**weergeven ontvangen berichten**' knop van de GELF UDP invoer dat u hebt gemaakt. U wordt omgeleid naar een scherm weergegeven dat op de volgende afbeelding lijkt: 

![Histogram](./media/network-watcher-analyze-nsg-flow-logs-graylog/histogram.png)

Te klikken op de koppeling blue '% {Message}", wordt elk bericht om weer te geven van de parameters van elke tuple stroom uitgebreid, zoals wordt weergegeven in de volgende afbeelding:

![Berichten](./media/network-watcher-analyze-nsg-flow-logs-graylog/messages.png)

Alle message velden zijn opgenomen in de zoekopdracht standaard als u een specifieke bericht-veld om te zoeken naar niet selecteert. Als u wilt zoeken naar specifieke berichten (Internet Explorer – stroom tuples met een specifiek bron-IP) kunt u de Graylog zoeken querytaal als [beschreven](http://docs.graylog.org/en/2.2/pages/queries.html)


## <a name="analyze-network-security-group-flow-logs-using-graylog"></a>Netwerk groep stroom beveiligingslogboeken met Graylog analyseren

Nu Graylog het stellen uitgevoerd, we kunt gebruiken enkele van de functies voor een beter begrip van de logboekgegevens van onze stroom. Een dergelijke manier is met behulp van dashboards te maken van de specifieke weergaven van onze gegevens.

### <a name="create-a-dashboard"></a>Een dashboard maken

1.  Selecteer in de bovenste navigatiebalk **Dashboards** of gaat u naar`http://<graylog-server-ip>:9000/dashboards/`

2.  Klik op de groene van daaruit **maken dashboard** knop en de korte vorm met de titel en beschrijving van het dashboard invullen. Klik op de **opslaan** knop om het nieuwe dashboard te maken. Ziet u een dashboard vergelijkbaar met de volgende afbeelding:

    ![Dashboards](./media/network-watcher-analyze-nsg-flow-logs-graylog/dashboards.png)

### <a name="add-widgets"></a>Widgets toevoegen

U kunt klikken op de titel van het dashboard te zien, maar rechtermuisknop nu de leeg, omdat we een widgets nog niet hebt toegevoegd. Een eenvoudig en nuttig type object dat u wilt toevoegen aan het dashboard worden **snelle waarden** grafieken die een lijst met waarden van het geselecteerde veld en de verdeling ervan weergeven.

1.  Ga terug naar de zoekresultaten van de UDP-invoer waarin logboeken van de stroom ontvangen door het selecteren van **Search** van de bovenste navigatiebalk.

2.  Onder de **zoekresultaat** aan de linkerkant van het scherm van het deelvenster, zoek de **velden** tabblad waarin de verschillende velden van elk binnenkomend bericht van de stroom-tuple.

3.  Selecteer alle gewenste parameter waarin visualiseren (in dit voorbeeld we hebt geselecteerd bron-IP). Als u wilt weergeven in de lijst met mogelijke widgets, klikt u op de blauwe pijl-omlaag aan de linkerkant van het veld en selecteer vervolgens **snelle waarden** voor het genereren van de widget. U ziet er ongeveer uitzien als in de volgende afbeelding:

    ![Bron-IP](./media/network-watcher-analyze-nsg-flow-logs-graylog/srcip.png)

4.  Daar kunt u de **toevoegen aan dashboard** in de rechterbovenhoek van het object en selecteer het bijbehorende dashboard om toe te voegen.

5.  Ga terug naar het dashboard om te zien van het object dat u zojuist hebt toegevoegd.

    U kunt tal van andere widgets zoals histogrammen en aantallen toevoegen aan uw dashboard voor het bijhouden van belangrijke metrische gegevens, zoals de voorbeelddashboard weergegeven in de volgende afbeelding:

    ![Flowlogs-dashboard](./media/network-watcher-analyze-nsg-flow-logs-graylog/flowlogs-dashboard.png)

    Raadpleeg voor meer uitleg over dashboards en de andere typen van widgets van Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/dashboards.html).

Door de netwerk-Watcher integreren met Graylog, hebben we nu een handige en gecentraliseerde manier om te beheren en groep netwerk-stroom beveiligingslogboeken visualiseren. Graylog heeft een aantal andere krachtige functies zoals stromen en waarschuwingen die kunnen ook worden gebruikt voor verdere beheren stroom logboeken en het netwerkverkeer beter te begrijpen. Nu dat u hebt Graylog ingesteld en verbonden met Azure, gerust doorgaan met het verkennen van de functionaliteit die dit biedt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw netwerk groep stroom beveiligingslogboeken met Power BI in via [Visualize netwerkbeveiligingsgroep loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md).
