---
title: Netwerk groep stromen beveiligingslogboeken met behulp van netwerk-Watcher en Grafana beheren | Microsoft Docs
description: Netwerk groep stromen beveiligingslogboeken in Azure met behulp van de netwerk-Watcher en Grafana analyseren en beheren.
services: network-watcher
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: 44cf074223c88b8fa539144c0d948e68ae6cbd13
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-and-analyze-network-security-group-flow-logs-using-network-watcher-and-grafana"></a>Beheren en analyseren van Netwerkbeveiligingsgroep stroom logboeken met behulp van netwerk-Watcher en Grafana

[Netwerkbeveiligingsgroep (NSG) stroom logboeken netwerk](network-watcher-nsg-flow-logging-overview.md) Geef informatie op die kan worden gebruikt om inkomende en uitgaande IP-verkeer op netwerkinterfaces begrijpen. Deze stroom logboeken weergeven binnenkomende en uitgaande stromen op een per per NSG regel, de NIC de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

U kunt maximum aantal Nsg hebben in uw netwerk met de stroom-logboekregistratie is ingeschakeld. Deze hoeveelheid logboekgegevens maakt het lastig is om te parseren en inzicht in uw logboeken krijgen. In dit artikel biedt een oplossing voor het centraal beheren van deze NSG stroom logboeken met behulp van Grafana, een open-source hulpprogramma, ElasticSearch, een gedistribueerde zoekopdracht en analyse-engine en Logstash die een open-source serverzijde gegevensverwerking pijplijn grafieken.  

## <a name="scenario"></a>Scenario

NSG-stroom logboeken met behulp van netwerk-Watcher zijn ingeschakeld en worden opgeslagen in Azure blob-opslag. Een invoegtoepassing Logstash wordt gebruikt om verbinding te en verwerken van Logboeken van de stroom van blob-opslag en ze verzenden naar ElasticSearch.  Zodra de stroom-logboeken worden opgeslagen in ElasticSearch, kunnen worden geanalyseerd en weergegeven in de aangepaste dashboards in Grafana.

![NSG netwerk-Watcher Grafana](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig1.png)

## <a name="installation-steps"></a>Installatiestappen

### <a name="enable-network-security-group-flow-logging"></a>Netwerkbeveiligingsgroep inschakelen stroom logboekregistratie

U moet voor dit scenario hebben Network Security groep stromen-logboekregistratie is ingeschakeld op ten minste één Netwerkbeveiligingsgroep in uw account. Voor instructies over het inschakelen van de stroom beveiligingslogboeken netwerk, raadpleegt u het volgende artikel [Inleiding tot registratie van de stroom voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

### <a name="setup-considerations"></a>Overwegingen bij de installatie

In dit voorbeeld worden Grafana ElasticSearch en Logstash geconfigureerd op een virtuele Ubuntu 16.04 TNS-Server geïmplementeerd in Azure. Deze minimale instelling wordt gebruikt voor het uitvoeren van de drie onderdelen: ze worden alle uitgevoerd op dezelfde virtuele machine. Deze instelling mag alleen worden gebruikt voor testen en niet-kritieke werkbelastingen. Logstash Elasticsearch en Grafana kunnen alle worden ontworpen om te schalen onafhankelijk over veel exemplaren. Zie voor meer informatie de documentatie voor elk van deze onderdelen.

### <a name="install-logstash"></a>Logstash installeren

Logstash kunt u de logboeken van de stroom JSON-indeling naar een stroom tuple niveau plat.

1. Als u wilt installeren Logstash, voer de volgende opdrachten:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2. Logstash voor het parseren van de stroom-logboeken en ze verzenden naar ElasticSearch configureren. Maak een Logstash.conf-bestand met:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. De volgende inhoud toevoegen aan het bestand. Wijzig de naam en opslagaccountsleutel om de details van uw opslag weer te geven:

    ```bash
    input {
      azureblob
      {
        storage_account_name => "mystorageaccount"
        storage_access_key => "VGhpcyBpcyBhIGZha2Uga2V5Lg=="
        container => "insights-logs-networksecuritygroupflowevent"
        codec => "json"
        # Refer https://docs.microsoft.com/azure/network-watcher/network-watcher-read-nsg-flow-logs
        # Typical numbers could be 21/9 or 12/2 depends on the nsg log file types
        file_head_bytes => 12
        file_tail_bytes => 2
        # Enable / tweak these settings when event is too big for codec to handle.
        # break_json_down_policy => "with_head_tail"
        # break_json_batch_count => 2
      }
    }
    filter {
      split { field => "[records]" }
      split { field => "[records][properties][flows]"}
      split { field => "[records][properties][flows][flows]"}
      split { field => "[records][properties][flows][flows][flowTuples]"}

      mutate {
        split => { "[records][resourceId]" => "/"}
        add_field => { "Subscription" => "%{[records][resourceId][2]}"
          "ResourceGroup" => "%{[records][resourceId][4]}"
          "NetworkSecurityGroup" => "%{[records][resourceId][8]}" 
        }
        convert => {"Subscription" => "string"}
        convert => {"ResourceGroup" => "string"}
        convert => {"NetworkSecurityGroup" => "string"}
        split => { "[records][properties][flows][flows][flowTuples]" => "," }
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
      elasticsearch {
        hosts => "localhost"
        index => "nsg-flow-logs"
      }
    }
    ```

Het opgegeven Logstash-configuratiebestand bestaat uit drie delen: de invoer-, filter- en uitvoer. De invoer sectie geeft de invoerbron van de logboeken die Logstash verwerkt: in dit geval gaan we een 'azureblob' input-invoegtoepassing (geïnstalleerd in de volgende stappen), zodat wij voor toegang tot het NSG stroom JSON logboekbestanden opgeslagen in blob storage gebruiken. 

De sectie filteren vervolgens elk logboekbestand stroom worden samengevoegd zodat elke tuple afzonderlijke stroom en de bijbehorende eigenschappen verandert in een aparte Logstash-gebeurtenis.

Ten slotte, verzendt de sectie uitvoer elke gebeurtenis Logstash naar de server ElasticSearch. U kunt wijzigen van het configuratiebestand Logstash aanpassen aan uw specifieke behoeften.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>De invoer Logstash-invoegtoepassing voor Azure Blob storage installeren

Deze invoegtoepassing Logstash kunt u rechtstreeks toegang krijgen tot de logboeken van de stroom van hun aangewezen blob storage-account. Deze invoegtoepassing, van de Logstash standaardinstallatiemap (in dit geval /usr/share/logstash/bin) Voer de opdracht installeren:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Zie voor meer informatie over deze invoegtoepassing [Logstash invoer-invoegtoepassing voor Azure Storage-Blobs](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-elasticsearch"></a>ElasticSearch installeren

U kunt het volgende script gebruiken voor het installeren van ElasticSearch. Zie voor meer informatie over het installeren van ElasticSearch [elastische Stack](https://www.elastic.co/guide/en/elastic-stack/current/index.html).

```bash
apt-get install apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen -y
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | apt-key add -
echo "deb https://packages.elastic.co/elasticsearch/5.x/debian stable main" | tee -a /etc/apt/sources.list.d/elasticsearch-5.x.list
apt-get update && apt-get install elasticsearch
sed -i s/#cluster.name:.*/cluster.name:\ grafana/ /etc/elasticsearch/elasticsearch.yml
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```

### <a name="install-grafana"></a>Grafana installeren

Als u wilt installeren en uitvoeren van Grafana, voer de volgende opdrachten:

```bash
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.1_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.1_amd64.deb
sudo service grafana-server start
```

Zie voor aanvullende installatie-informatie [installeren op Debian / Ubuntu](http://docs.grafana.org/installation/debian/).

#### <a name="add-the-elasticsearch-server-as-a-data-source"></a>De server ElasticSearch toevoegen als een gegevensbron

Vervolgens moet u de logboeken van de stroom die als een gegevensbron ElasticSearch-index. U kunt een gegevensbron toevoegen door te selecteren **gegevensbron toevoegen** en invullen van het formulier met de relevante informatie. Een voorbeeld van deze configuratie vindt u in de volgende schermafbeelding:

![Gegevensbron toevoegen](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig2.png)

#### <a name="create-a-dashboard"></a>Een dashboard maken

Nu dat u Grafana lezen uit het NSG stroom logboeken met ElasticSearch-index hebt geconfigureerd, kunt u maken en dashboards aanpassen. Als u een nieuw dashboard, selecteert **maken van uw eerste dashboard**. De configuratie van de volgende voorbeeld van grafiek toont stromen gesegmenteerd op NSG regel:

![Dashboard-grafiek](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig3.png)

De volgende schermafbeelding ziet u een grafiek en een grafiek met de hoogste stromen en de frequentie. Stromen worden ook weergegeven door de regel NSG en stromen door beslissing. Grafana is zeer aanpasbare, zodat u aangeraden wordt dat u maakt dashboards aanpassen aan uw specifieke behoeften voor bewaking. Het volgende voorbeeld toont een typische dashboard:

![Dashboard-grafiek](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig4.png)

## <a name="conclusion"></a>Conclusie

Door integratie van netwerk-Watcher met ElasticSearch en Grafana, hebt u nu een handige en gecentraliseerde manier om te beheren en het NSG stroom Logboeken, evenals andere gegevens te visualiseren. Grafana heeft een aantal andere krachtige grafische functies die ook kan worden gebruikt voor het verder beheren stroom logboeken en het netwerkverkeer beter te begrijpen. Nu dat u ingesteld en verbonden met Azure hebt, gerust doorgaan met het verkennen van de functionaliteit die dit biedt een Grafana-exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [netwerk-Watcher](network-watcher-monitoring-overview.md).

