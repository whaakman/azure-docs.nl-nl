---
title: Stroomlogboeken van Azure network security group - Graylog analyseren | Microsoft Docs
description: Informatie over het beheren en analyseren van flow logboeken van netwerkbeveiligingsgroepen in Azure met behulp van Network Watcher en Graylog.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: mareat
ms.openlocfilehash: 3030fdcec95d91b75974465ad30f707837263367
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414774"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-in-azure-using-network-watcher-and-graylog"></a>Beheren en analyseren van flow logboeken van netwerkbeveiligingsgroepen in Azure met behulp van Network Watcher en Graylog

[Stroomlogboeken van het netwerk](network-watcher-nsg-flow-logging-overview.md) bevatten informatie die u gebruiken kunt om te begrijpen van inkomende en uitgaande IP-verkeer voor Azure-netwerkinterfaces. Logboeken van de stroom binnenkomende en uitgaande stromen weergeven op een per regel gefactureerd voor network security group, de netwerkinterface de stroom is van toepassing op, 5-tuple-informatie (bron-/ doel-IP-adres, poort van de bron-/ doel, Protocol) over de stroom, en als het verkeer is toegestaan of geweigerd .

U kunt veel netwerkbeveiligingsgroepen in uw netwerk hebt met flow logboekregistratie is ingeschakeld. Aantal netwerkbeveiligingsgroepen met flow logboekregistratie is ingeschakeld, kunnen u verkeerd ingevoerde adressen te parseren en Verkrijg inzicht in uw Logboeken. In dit artikel biedt een oplossing voor het centraal beheren van deze stroomlogboeken van netwerkbeveiligingsgroepen met behulp van Graylog, een open-source-logboekbeheer en analyseprogramma en Logstash, een open-source-serverzijde gegevensverwerking-pijplijn.

## <a name="scenario"></a>Scenario

Stroomlogboeken van netwerk zijn ingeschakeld met behulp van Network Watcher. Stroom logboeken stroom in naar Azure blob-opslag. Een invoegtoepassing Logstash wordt gebruikt om verbinding maken en verwerken van Logboeken van de stroom van blob-opslag en verzend dit naar Graylog. Nadat de logboeken van de stroom worden opgeslagen in Graylog, kunnen ze worden geanalyseerd en gevisualiseerd in aangepaste dashboards.

![Graylog werkstroom](./media/network-watcher-analyze-nsg-flow-logs-graylog/workflow.png)

## <a name="installation-steps"></a>Installatiestappen

### <a name="enable-network-security-group-flow-logging"></a>Stroomlogboeken van netwerkbeveiligingsgroep netwerk inschakelen

Voor dit scenario moet u netwerk stroomlogboeken van netwerkbeveiligingsgroep ingeschakeld op ten minste één netwerkbeveiligingsgroep in uw account hebben. Voor instructies over het inschakelen van het netwerk stroomlogboeken, raadpleegt u het volgende artikel [Inleiding tot stroomlogboeken voor netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

### <a name="setting-up-graylog"></a>Instellen van Graylog

In dit voorbeeld worden zowel Graylog en Logstash geconfigureerd op een Ubuntu 14.04-Server geïmplementeerd in Azure.

- Raadpleeg de [documentatie](http://docs.graylog.org/en/2.2/pages/installation/os/ubuntu.html) van Graylog, voor stapsgewijze instructies voor het installeren op Ubuntu.
- Zorg ervoor dat u de webinterface van Graylog ook configureren door de [documentatie](http://docs.graylog.org/en/2.2/pages/configuration/web_interface.html#configuring-webif).

In dit voorbeeld wordt de minimale Graylog-instellingen (zoals) slechts één exemplaar van een Graylog), maar Graylog kan worden ontworpen om de behoeften op resources afhankelijk van uw systeem en productie worden geschaald. Zie voor meer informatie over overwegingen voor architecturale of een uitgebreide handleiding voor architectuur van van Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/architecture.html) en [Architectuurhandleiding voor](https://www.slideshare.net/Graylog/graylog-engineering-design-your-architecture).

Graylog kan worden geïnstalleerd op tal van manieren, afhankelijk van uw platform en voorkeuren. Raadpleeg voor een volledige lijst van mogelijke installatiemethoden van Graylog officiële [documentatie](http://docs.graylog.org/en/2.2/pages/installation.html). De servertoepassing Graylog wordt uitgevoerd op Linux-distributies en heeft de volgende vereisten:

-  Java SE 8 of hoger: [Azul Azure JDK-documentatie](https://aka.ms/azure-jdks)
-  Elastische 2.x zoeken (2.1.0 of hoger)- [installatiedocumentatie voor Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/_installation.html)
-  MongoDB 2.4 of hoger: [MongoDB-documentatie voor installatie](https://docs.mongodb.com/manual/administration/install-on-linux/)

### <a name="install-logstash"></a>Logstash installeren

Logstash wordt gebruikt voor het samenvoegen van de logboeken van de stroom JSON-indeling naar een stroom tuple-niveau. Plat maken van de logboeken van de stroom gemakkelijker de logboeken te organiseren en te zoeken in Graylog.

1. Voer de volgende opdrachten voor het installeren van Logstash:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
   sudo dpkg -i logstash-5.2.0.deb
   ```

2. Logstash parseren van de logboeken van de stroom en verzend dit naar Graylog configureren. Maak een bestand Logstash.conf:

   ```bash
   sudo touch /etc/logstash/conf.d/logstash.conf
   ```

3. De volgende inhoud toevoegen aan het bestand. Wijzig de gemarkeerde waarden om de details van uw storage-account weer te geven:

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
Het opgegeven Logstash config-bestand bestaat uit drie delen: de invoer-, filter- en uitvoer. De sectie invoer geeft de invoerbron van de logboeken die Logstash verwerkt: in dit geval gaat u gebruik van een Azure-blog invoer-invoegtoepassing (geïnstalleerd in de volgende stappen) waardoor toegang tot de stroom network security group-logboek JSON-bestanden die zijn opgeslagen in blob-opslag.

De sectie filteren vervolgens het logboekbestand van elke stroom wordt samengevoegd zodat elke tuple afzonderlijke stroom en de bijbehorende eigenschappen verandert in een aparte Logstash-gebeurtenis.

Ten slotte verzendt de sectie uitvoer elke gebeurtenis Logstash met de Graylog-server. Aan de behoeften van uw specifieke bedrijfsbehoeften, wijzigt de Logstash config-bestand, zoals vereist.

   > [!NOTE]
   > Het vorige configuratiebestand wordt ervan uitgegaan dat de Graylog-server is geconfigureerd op de lokale host loopback-IP-adres 127.0.0.1. Als dat niet het geval is, zorg ervoor dat u het wijzigen van de host-parameter in de sectie uitvoer naar het juiste IP-adres.

Zie voor verdere instructies over het installeren van Logstash de Logstash [documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plug-in-for-azure-blob-storage"></a>Installeer de invoegtoepassing voor Azure blob-opslag Logstash-invoer

De Logstash-invoegtoepassing kunt u rechtstreeks toegang krijgen tot de logboeken van de stroom van de opgegeven blob storage-account. Voor het installeren van de invoegtoepassing uit de installatiemap van de standaard-Logstash (in dit geval /usr/share/logstash/bin), voer de volgende opdracht:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Zie voor meer informatie over deze invoegtoepassing de [documentatie](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="set-up-connection-from-logstash-to-graylog"></a>Verbinding vanuit Logstash instellen voor Graylog

Nu dat u hebt een verbinding met de logboeken van de stroom met behulp van Logstash tot stand gebracht en de server Graylog instellen, moet u Graylog voor het accepteren van de binnenkomende logboekbestanden configureren.

1. Navigeer naar uw web-interface van Graylog Server met behulp van de URL die u hebt geconfigureerd voor het. U kunt toegang krijgen tot de interface door te leiden van uw browser naar `http://<graylog-server-ip>:9000/`

2. Ga naar de configuratiepagina, selecteer de **System** vervolgkeuzelijst in de bovenste navigatiebalk aan de rechterkant van de balk en klik vervolgens op **invoer**.
   U kunt ook, gaat u naar `http://<graylog-server-ip>:9000/system/inputs`

   ![Aan de slag](./media/network-watcher-analyze-nsg-flow-logs-graylog/getting-started.png)

3. Selecteer voor het starten van de nieuwe invoer *GELF UDP* in de **Invoerselectie** vervolgkeuzelijst en vul het formulier. GELF staat voor Graylog uitgebreide indeling. De indeling GELF is ontwikkeld door Graylog. Zie voor meer informatie over de voordelen, de Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/gelf.html).

   Zorg ervoor dat u het binden van de invoer naar het IP-adres geconfigureerd van uw server Graylog op. Het IP-adres moet overeenkomen met de **host** veld van de UDP-uitvoer van het bestand Logstash-configuratie. De standaardpoort moet *12201*. Zorg ervoor dat de poort komt overeen met de **poort** veld in het UDP-uitvoer aangegeven in de Logstash config-bestand.

   ![Invoer](./media/network-watcher-analyze-nsg-flow-logs-graylog/inputs.png)

   Als u de invoer start, ziet u deze worden weergegeven onder de **lokale invoer** sectie, zoals wordt weergegeven in de volgende afbeelding:

   ![](./media/network-watcher-analyze-nsg-flow-logs-graylog/local-inputs.png)

   Raadpleeg voor meer informatie over Graylog berichtinvoer, de [documentatie](http://docs.graylog.org/en/2.2/pages/sending_data.html#what-are-graylog-message-inputs).

4. Zodra deze configuraties zijn aangebracht, kun u Logstash om te beginnen met lezen in Logboeken van de stroom met de volgende opdracht: `sudo systemctl start logstash.service`.

### <a name="search-through-graylog-messages"></a>Graylog berichten doorzoeken

Na enige tijd voor uw server Graylog voor het verzamelen van berichten toe te staan, bent u kunnen de berichten doorzoeken. Om te controleren of de berichten worden verzonden naar uw server Graylog vanaf de **invoer** configuratie pagina op de '**Show berichten ontvangen**"knop van het UDP-GELF invoer dat u hebt gemaakt. U bent omgeleid naar een scherm dat op de volgende afbeelding lijkt: 

![Histogram](./media/network-watcher-analyze-nsg-flow-logs-graylog/histogram.png)

Te klikken op de koppeling blue '% {Message}", wordt elk bericht om weer te geven van de parameters van elke tuple stroom uitgebreid, zoals wordt weergegeven in de volgende afbeelding:

![Berichten](./media/network-watcher-analyze-nsg-flow-logs-graylog/messages.png)

Alle berichtvelden zijn standaard opgenomen in het zoekvak, als u een specifieke berichtenveld om te zoeken naar niet selecteert. Als u wilt zoeken naar specifieke berichten (zoals) – stroom tuples van een bepaalde bron-IP) kunt u de querytaal van Graylog zoeken als [beschreven](http://docs.graylog.org/en/2.2/pages/queries.html)

## <a name="analyze-network-security-group-flow-logs-using-graylog"></a>Stroomlogboeken van netwerkbeveiligingsgroepen met behulp van Graylog analyseren

Nu dat Graylog het Stel waarop wordt uitgevoerd, kunt u bepaalde functionaliteit van de logboekgegevens van uw stroom beter te begrijpen. Een dergelijke manier is met behulp van dashboards om specifieke weergaven van uw gegevens te maken.

### <a name="create-a-dashboard"></a>Een dashboard maken

1. Selecteer in de bovenste navigatiebalk **Dashboards** of Ga naar `http://<graylog-server-ip>:9000/dashboards/`

2. Van daaruit, klikt u op de groene **maken dashboard** knop en vul de korte formulier in met de titel en beschrijving van het dashboard. Klik op de **opslaan** knop om het nieuwe dashboard te maken. Ziet u een dashboard die vergelijkbaar is met de volgende afbeelding:

    ![Dashboards](./media/network-watcher-analyze-nsg-flow-logs-graylog/dashboards.png)

### <a name="add-widgets"></a>Widgets toevoegen

U kunt klikken op de titel van het dashboard om te zien, maar de leeg, op dit moment omdat er widgets nog niet hebt toegevoegd. Een widget gemakkelijk en handig zijn om toe te voegen aan het dashboard zijn **snelle waarden** grafieken die een lijst met waarden van het geselecteerde veld en de verdeling ervan weergeven.

1. Ga terug naar de lijst met zoekresultaten van de UDP-invoer waarop de logboeken van de stroom wordt ontvangen door te selecteren **zoeken** in de bovenste navigatiebalk.

2. Onder de **zoekresultaat** aan de linkerkant van het scherm van het deelvenster, zoek de **velden** tabblad geeft een lijst van de verschillende velden van elk binnenkomend bericht van de flow-tuple.

3. Selecteer elke gewenste parameter waarin u kunt visualiseren (in dit voorbeeld wordt de IP-bron is geselecteerd). Als u wilt weergeven in de lijst met mogelijke widgets, klikt u op de blauwe pijl-omlaag links van het veld en selecteer vervolgens **snelle waarden** voor het genereren van de widget. U ziet er ongeveer uitzien als in de volgende afbeelding:

   ![Bron-IP](./media/network-watcher-analyze-nsg-flow-logs-graylog/srcip.png)

4. Van daaruit kunt u de **toevoegen aan dashboard** in de rechterbovenhoek van de widget en selecteer het bijbehorende dashboard om toe te voegen.

5. Ga terug naar het dashboard om te zien van de widget die u zojuist hebt toegevoegd.

   U kunt tal van andere objecten, zoals het aantal fouten en histogrammen toevoegen aan uw dashboard voor het bijhouden van belangrijke metrische gegevens, zoals het voorbeelddashboard weergegeven in de volgende afbeelding:

   ![Flowlogs dashboard](./media/network-watcher-analyze-nsg-flow-logs-graylog/flowlogs-dashboard.png)

    Zie voor verdere uitleg aan dashboards en de andere typen widgets van Graylog [documentatie](http://docs.graylog.org/en/2.2/pages/dashboards.html).

Door de Network Watcher integratie met Graylog, hebt u nu een handige en gecentraliseerde manier om te beheren en netwerk stroomlogboeken visualiseren. Graylog heeft een aantal andere krachtige functies zoals streams en waarschuwingen die kunnen ook worden gebruikt voor het verder beheren-stroomlogboeken en beter inzicht in uw netwerkverkeer. Nu dat u Graylog instellen en met Azure bent verbonden hebt, kunt u doorgaan met het verkennen van de functionaliteit die dit biedt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw stroomlogboeken van netwerkbeveiligingsgroepen met Power BI recentst [Visualize netwerkbeveiligingsgroep stromen logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md).
