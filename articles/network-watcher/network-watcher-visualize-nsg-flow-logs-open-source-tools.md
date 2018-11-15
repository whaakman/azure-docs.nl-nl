---
title: Azure Network Watcher NSG stroomlogboeken visualiseren met open-sourcehulpprogramma's | Microsoft Docs
description: Deze pagina wordt beschreven hoe NSG-stroomlogboeken visualiseren met open source-hulpprogramma's.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: mareat
ms.openlocfilehash: 6debfaed28be2b5dee5e24a443b791f75f7bea8f
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634189"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Azure Network Watcher NSG stroomlogboeken visualiseren met open-sourcehulpprogramma 's

Stroomlogboeken van Netwerkbeveiligingsgroep Geef informatie op die kan worden gebruikt te begrijpen van inkomende en uitgaande IP-verkeer op Netwerkbeveiligingsgroepen. Deze logboeken van de stroom weergeven binnenkomende en uitgaande stromen op basis van een per regel, de NIC die de stroom is van toepassing op, 5 tuple-informatie over de stroom (bron-/ doel-IP, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

Deze logboeken van de stroom kunnen lastig zijn om handmatig te parseren en hieruit inzichten te verkrijgen. Er zijn echter enkele open-source-hulpprogramma's die kunnen helpen bij het visualiseren van deze gegevens. In dit artikel biedt een oplossing voor het visualiseren van deze logboeken met behulp van de Elastic Stack, waarmee u kunt om snel te indexeren en visualiseren van uw stroom een Kibana-dashboard zich aanmeldt.

## <a name="scenario"></a>Scenario

In dit artikel, wordt er een oplossing waarmee u kunt Network Security Group stroomlogboeken visualiseren met de Elastic Stack instellen.  Een invoer Logstash-invoegtoepassing krijgt de logboeken van de stroom rechtstreeks vanuit de opslag-blob voor de logboeken van de stroom die is geconfigureerd. Klik vervolgens worden met behulp van de Elastic Stack, de logboeken van de stroom geïndexeerd en gebruikt voor het maken van een Kibana-dashboard voor het visualiseren van gegevens.

![scenario][scenario]

## <a name="steps"></a>Stappen

### <a name="enable-network-security-group-flow-logging"></a>Stroomlogboeken van Netwerkbeveiligingsgroep inschakelen
U moet voor dit scenario hebben Network Security Group Flow-logboekregistratie is ingeschakeld op ten minste één Netwerkbeveiligingsgroep in uw account. Zie voor instructies over het inschakelen van de logboeken van de stroom van netwerk, het volgende artikel [Inleiding tot stroomlogboeken voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).

### <a name="set-up-the-elastic-stack"></a>Instellen van de Elastic Stack
NSG-stroomlogboeken in contact met de Elastic Stack, kunnen we een Kibana-dashboard maken wat kunnen we zoeken, in een grafiek, analyseren en inzichten die zijn afgeleid van onze Logboeken.

#### <a name="install-elasticsearch"></a>Elasticsearch installeren

1. De Elastic Stack versie 5.0 en hoger is vereist voor Java 8. Voer de opdracht `java -version` om uw versie te controleren. Als u geen java geïnstalleerd hebt, Raadpleeg de documentatie op de [Azure ondersteund JDK](https://aka.ms/azure-jdks).
2. Download het juiste binaire pakket voor uw systeem:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
   sudo dpkg -i elasticsearch-5.2.0.deb
   sudo /etc/init.d/elasticsearch start
   ```

   Andere installatiemethoden kunnen worden gevonden op [Elasticsearch-installatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

3. Controleren of Elasticsearch wordt uitgevoerd met de opdracht:

    ```bash
    curl http://127.0.0.1:9200
    ```

    U ziet een antwoord ongeveer als volgt uit:

    ```json
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Raadpleeg voor meer instructies voor installeren elastische zoeken [installatie-instructies](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html).

### <a name="install-logstash"></a>Logstash installeren

1. Voer de volgende opdrachten Logstash installeren:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
2. Vervolgens moeten we Logstash om te openen en parseren van de stroomlogboeken configureren. Maak een logstash.conf bestand met:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Voeg de volgende inhoud toe aan het bestand:

   ```
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

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
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
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
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

Voor verdere instructies over het installeren van Logstash, raadpleegt u de [officiële documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html).

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>De invoer Logstash-invoegtoepassing voor Azure blob-opslag installeren

Deze Logstash-invoegtoepassing kunt u rechtstreeks toegang tot de logboeken van de stroom vanuit hun aangewezen storage-account. Voer de opdracht uit de installatiemap van de standaard-Logstash (in dit geval /usr/share/logstash/bin) voor het installeren van deze invoegtoepassing:

```bash
logstash-plugin install logstash-input-azureblob
```

Start Logstash voert u de opdracht uit:

```bash
sudo /etc/init.d/logstash start
```

Raadpleeg voor meer informatie over deze invoegtoepassing de [documentatie](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob).

### <a name="install-kibana"></a>Kibana installeren

1. Voer de volgende opdrachten voor het installeren van Kibana:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
   tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
   ```

2. Kibana gebruik de opdrachten uitvoeren:

   ```bash
   cd kibana-5.2.0-linux-x86_64/
   ./bin/kibana
   ```

3. Als u de webinterface voor uw Kibana, gaat u naar `http://localhost:5601`
4. Voor dit scenario is de index-patroon gebruikt voor de logboeken van de stroom '--stroomlogboeken'. U kunt het patroon index in de sectie 'uitvoer' van het bestand logstash.conf wijzigen.
5. Als u wilt om op afstand de Kibana-dashboard weer te geven, maakt u een inkomende NSG-regel toestaan van toegang tot **poort 5601**.

### <a name="create-a-kibana-dashboard"></a>Een Kibana-dashboard maken

In de volgende afbeelding ziet u een voorbeelddashboard om trends en details in uw waarschuwingen weer te geven:

![afbeelding 1][1]

Download de [dashboardbestand](https://aka.ms/networkwatchernsgflowlogdashboard), wordt de [visualisatie bestand](https://aka.ms/networkwatchernsgflowlogvisualizations), en de [search-bestand hebt opgeslagen](https://aka.ms/networkwatchernsgflowlogsearch).

Onder de **Management** tabblad van Kibana, gaat u naar **opgeslagen objecten** en alle drie bestanden te importeren. Klik vanuit de **Dashboard** tabblad kunt u openen en het voorbeelddashboard laden.

U kunt ook uw eigen visualisaties en dashboards die zijn afgestemd op uw eigen nuttige meetgegevens beschikbaar maken. Meer informatie over het maken van Kibana visualisaties uit de Kibana [officiële documentatie](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>NSG-stroomlogboeken visualiseren

Het voorbeelddashboard van biedt verschillende visualisaties van de logboeken van de stroom:

1. Stromen op besluit/richting gedurende een periode - tijd reeks grafieken met het aantal stromen gedurende de periode. U kunt de eenheid van de tijd en tijdsbestek van beide deze visualisaties bewerken. Stromen op besluit geeft het deel van de toestaan of weigeren van beslissingen, hoewel stromen op richting op het aandeel van binnenkomend en uitgaand verkeer. U kunt met deze visuele elementen onderzoeken verkeer trends na verloop van tijd en zoek naar eventuele pieken of ongewone patronen.

   ![afbeelding 2][2]

2. Stromen op bestemming/bronpoort – cirkeldiagrammen met de verdeling van de stromen naar hun respectieve poorten. Met deze weergave ziet u de meest gebruikte poorten. Als u op een specifieke poort in het cirkeldiagram klikt, wordt de rest van het dashboard wordt gefilterd op stromen van deze poort.

   ![figure3][3]

3. Aantal stromen en vroegste tijd van beveiligingslogboek: metrische gegevens die u het aantal stromen die zijn geregistreerd en de datum van het eerste logboek vastgelegd.

   ![figure4][4]

4. Stromen op Netwerkbeveiligingsgroep en regel: een staafdiagram waarin u de distributie van stromen binnen elke NSG, evenals de verdeling van de regels in elke NSG. Hier ziet u welke NSG en regels voor de meeste verkeer gegenereerd.

   ![figure5][5]

5. Top 10 bron-/ doel-IP-adressen: staafdiagrammen met de top 10-bron en doel-IP-adressen. U kunt deze grafieken om meer of minder bovenste IP-adressen weer te geven. Hier ziet u het meest optreden IP-adressen, evenals de beslissing verkeer (toestaan of weigeren) wordt gemaakt voor elke IP.

   ![figure6][6]

6. Stroom Tuples – deze tabel ziet u de informatie in elke tuple stroom, evenals de bijbehorende NGS en regel.

   ![figure7][7]

Met behulp van de balk voor query's aan de bovenkant van het dashboard, kunt u filteren op het dashboard op basis van een parameter van de stromen, zoals de abonnements-ID, resourcegroepen, regel of een andere variabele van belang zijn. Raadpleeg voor meer informatie over de Kibana-query's en filters, de [officiële documentatie](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Conclusie

Door de stroomlogboeken van Netwerkbeveiligingsgroep combineren met de Elastic Stack, wij hebben zijn beschikbaar met krachtige en aanpasbare manier voor het visualiseren van onze netwerkverkeer. Deze dashboards kunnen u snel toegang en deel inzichten over uw netwerkverkeer, evenals de filter omlaag en onderzoek op elke mogelijke afwijkingen. Met behulp van Kibana, kunt u deze dashboards aanpassen en maken van specifieke visualisaties om te voldoen aan de behoeften voor beveiliging, controle en naleving.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG-stroomlogboeken met Power BI recentst [visualiseren NSG-stromen logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
