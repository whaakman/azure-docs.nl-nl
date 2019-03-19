---
title: Beheren van stromen logboeken van Netwerkbeveiligingsgroepen met behulp van Network Watcher en Grafana | Microsoft Docs
description: Beheren en analyseren van Flow logboeken van Netwerkbeveiligingsgroepen in Azure met behulp van Network Watcher en Grafana.
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
ms.date: 09/15/2017
ms.author: mareat
ms.openlocfilehash: 73173c144f979d4a10b90a16aec783fe51a3f90e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58000397"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-using-network-watcher-and-grafana"></a>Beheren en analyseren van stroomlogboeken van Netwerkbeveiligingsgroep met behulp van Network Watcher en Grafana

[Network Security Group (NSG) stroomlogboeken](network-watcher-nsg-flow-logging-overview.md) Geef informatie op die kan worden gebruikt om te begrijpen van inkomende en uitgaande IP-verkeer op netwerkinterfaces. Deze logboeken van de stroom binnenkomende en uitgaande stromen weergeven op een per per NSG-regel, de NIC de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP-adres, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

> [!Warning]  
> De volgende stappen werken met flow logboeken versie 1. Zie voor meer informatie, [Inleiding tot stroomlogboeken voor netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md). De volgende instructies werkt niet met versie 2 van de logboekbestanden, zonder aanpassingen.

U kunt veel nsg's in uw netwerk hebt met flow logboekregistratie is ingeschakeld. Deze hoeveelheid gegevens voor logboekregistratie is het lastig zijn om te parseren en Verkrijg inzicht in uw Logboeken. In dit artikel biedt een oplossing voor deze NSG-stroomlogboeken met behulp van Grafana, een open-source hulpprogramma, ElasticSearch, een gedistribueerde zoeken en analyse-engine en Logstash, dit een open-source-serverzijde gegevensverwerking pijplijn is afhankelijkheidsgrafieken centraal te beheren.  

## <a name="scenario"></a>Scenario

NSG-stroomlogboeken zijn ingeschakeld met behulp van Network Watcher en worden opgeslagen in Azure blob-opslag. Een invoegtoepassing Logstash wordt gebruikt om verbinding maken en verwerken van Logboeken van de stroom van blob-opslag en ze verzenden naar ElasticSearch.  Nadat de logboeken van de stroom worden opgeslagen in ElasticSearch, kunnen ze worden geanalyseerd en gevisualiseerd in aangepaste dashboards in Grafana.

![NSG Network Watcher Grafana](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig1.png)

## <a name="installation-steps"></a>Installatiestappen

### <a name="enable-network-security-group-flow-logging"></a>Stroomlogboeken van Netwerkbeveiligingsgroep inschakelen

U moet voor dit scenario hebben Network Security Group Flow-logboekregistratie is ingeschakeld op ten minste één Netwerkbeveiligingsgroep in uw account. Zie voor instructies over het inschakelen van de logboeken van de stroom van netwerk, het volgende artikel [Inleiding tot stroomlogboeken voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

### <a name="setup-considerations"></a>Overwegingen bij de installatie

Grafana en ElasticSearch, Logstash in dit voorbeeld zijn geconfigureerd op een Ubuntu 16.04 LTS-Server geïmplementeerd in Azure. Deze minimale installatie wordt gebruikt voor het uitvoeren van alle drie onderdelen: ze zijn alle op dezelfde virtuele machine uitgevoerd. Deze instelling mag alleen worden gebruikt voor testen en niet-essentiële workloads. Logstash Elasticsearch en Grafana kunnen allemaal worden ontworpen om onafhankelijk worden geschaald over veel exemplaren. Zie voor meer informatie de documentatie voor elk van deze onderdelen.

### <a name="install-logstash"></a>Logstash installeren

Logstash kunt u de logboeken van de stroom JSON-indeling naar een niveau van de tuple stroom afvlakken.

1. Voer de volgende opdrachten voor het installeren van Logstash:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2. Configureer Logstash voor het parseren van de logboeken van de stroom en ze verzenden naar ElasticSearch. Maak een Logstash.conf bestand met:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. De volgende inhoud toevoegen aan het bestand. Wijzig de naam en toegangssleutel van het opslagaccount om de details van uw storage-account weer te geven:

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

Het opgegeven Logstash config-bestand bestaat uit drie delen: de invoer-, filter- en uitvoer.
De sectie invoer geeft de invoerbron van de logboeken die Logstash verwerkt: in dit geval gaan we een 'azureblob' invoer-invoegtoepassing (geïnstalleerd in de volgende stappen) zodat wij voor toegang tot de NSG stroom log JSON-bestanden die zijn opgeslagen in blob-opslag gebruiken. 

De sectie filteren vervolgens het logboekbestand van elke stroom wordt samengevoegd zodat elke tuple afzonderlijke stroom en de bijbehorende eigenschappen verandert in een aparte Logstash-gebeurtenis.

Ten slotte verzendt de sectie uitvoer elke gebeurtenis Logstash met de ElasticSearch-server. U kunt de Logstash config-bestand aan de behoeften van uw specifieke behoeften aanpassen.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>De invoer Logstash-invoegtoepassing voor Azure Blob storage installeren

Deze Logstash-invoegtoepassing kunt u rechtstreeks toegang krijgen tot de logboeken van de stroom van de opgegeven blob storage-account. Map (in dit geval /usr/share/logstash/bin) Voer de opdracht voor het installeren van deze plug in, van de standaard Logstash-installatie:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Zie voor meer informatie over deze invoegtoepassing [invoer Logstash-invoegtoepassing voor Azure Storage-Blobs](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-elasticsearch"></a>ElasticSearch installeren

U kunt het volgende script gebruiken voor het installeren van ElasticSearch. Zie voor meer informatie over het installeren van ElasticSearch [Elastic Stack](https://www.elastic.co/guide/en/elastic-stack/current/index.html).

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

Zie voor aanvullende installatie-informatie, [installeren in Debian / Ubuntu](https://docs.grafana.org/installation/debian/).

#### <a name="add-the-elasticsearch-server-as-a-data-source"></a>De ElasticSearch-server als een gegevensbron toevoegen

Vervolgens moet u de ElasticSearch-index met stroomlogboeken als een gegevensbron toevoegen. U kunt een gegevensbron toevoegen door te selecteren **gegevensbron toevoegen** en het formulier in met de relevante informatie te voltooien. Een voorbeeld van deze configuratie kan worden gevonden in de volgende schermafbeelding:

![Gegevensbron toevoegen](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig2.png)

#### <a name="create-a-dashboard"></a>Een dashboard maken

Nu dat u Grafana te lezen uit de ElasticSearch-index met NSG-stroomlogboeken hebt geconfigureerd, kunt u maken en aanpassen van dashboards. Voor het maken van een nieuw dashboard selecteert **Maak uw eerste dashboard**. De volgende voorbeeldconfiguratie van de grafiek ziet u stromen gesegmenteerd op NSG-regel:

![Dashboard-grafiek](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig3.png)

De volgende schermafbeelding ziet u een grafiek en een grafiek met de hoogste stromen en de frequentie. Stromen worden ook weergegeven door NSG-regel en stromen op beslissing. Grafana is in hoge mate aanpasbaar, zodat u aangeraden wordt dat u maakt dashboards aan de behoeften van uw specifieke controlebehoeften voldoen. Het volgende voorbeeld ziet u een standaard dashboard:

![Dashboard-grafiek](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig4.png)

## <a name="conclusion"></a>Conclusie

Door de Network Watcher integratie met ElasticSearch en Grafana, hebt u nu een handige en gecentraliseerde manier om te beheren en visualiseren van NSG-stroomlogboeken, evenals andere gegevens. Grafana heeft een aantal andere krachtige grafische functies die kunnen ook worden gebruikt voor het verder beheren-stroomlogboeken en beter inzicht in uw netwerkverkeer. Nu dat u een Grafana-instantie instellen en met Azure bent verbonden hebt, gerust doorgaan met het verkennen van de functionaliteit die dit biedt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [Network Watcher](network-watcher-monitoring-overview.md).

