---
title: Azure-netwerk-Watcher NSG stroom logboeken met open-source hulpprogramma's visualiseren | Microsoft Docs
description: Deze pagina wordt beschreven hoe open-source hulpprogramma's gebruiken voor het NSG stroom logboeken visualiseren.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 6caff3237e9694a00fc0847d5612b7a6e08d4b69
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Azure-netwerk-Watcher NSG stroom logboeken met open-source hulpprogramma's te visualiseren

Netwerkbeveiligingsgroep stroom logboeken bevatten informatie die kan worden gebruikt inkomende en uitgaande IP-verkeer op Netwerkbeveiligingsgroepen begrijpen. Deze stroom logboeken weergeven binnenkomende en uitgaande stromen op basis van per regel, de NIC die de stroom is van toepassing op, 5-tuple informatie over de stroom (IP bron/het doel, bron/het doel-poort Protocol) en als het verkeer is toegestaan of geweigerd.

Deze stroom logboeken kunnen lastig zijn om handmatig te parseren en inzichten te krijgen. Er zijn echter enkele open-source hulpprogramma's die kunnen helpen bij het visualiseren van deze gegevens. In dit artikel biedt een oplossing voor het visualiseren van deze logboeken met behulp van de elastische Stack, waarmee u om snel te indexeren en visualiseren van uw flow logboeken op een dashboard Kibana.

## <a name="scenario"></a>Scenario

In dit artikel wordt er een oplossing waarmee u kunt het visualiseren van Netwerkbeveiligingsgroep stroom logboeken met behulp van de elastische Stack instellen.  De logboeken van de stroom worden door een Logstash invoer invoegtoepassing rechtstreeks vanuit de blob storage is geconfigureerd voor de logboeken van de stroom die worden opgehaald. Vervolgens wordt met behulp van de elastische Stack de logboeken van de stroom geïndexeerd en gebruikt om een dashboard Kibana visualiseren van de gegevens te maken.

![scenario][scenario]

## <a name="steps"></a>Stappen

### <a name="enable-network-security-group-flow-logging"></a>Netwerkbeveiligingsgroep inschakelen stroom logboekregistratie
U moet voor dit scenario hebben Network Security groep stromen-logboekregistratie is ingeschakeld op ten minste één Netwerkbeveiligingsgroep in uw account. Voor instructies over het inschakelen van de stroom beveiligingslogboeken netwerk, raadpleegt u het volgende artikel [Inleiding tot registratie van de stroom voor Netwerkbeveiligingsgroepen](network-watcher-nsg-flow-logging-overview.md).


### <a name="set-up-the-elastic-stack"></a>Instellen van de elastische Stack
NSG stroom Logboeken verbinding met de elastische Stack, kunnen we een Kibana-dashboard maken wat ons te zoeken, grafiek, analyseren en inzichten worden afgeleid van onze Logboeken is toegestaan.

#### <a name="install-elasticsearch"></a>Elasticsearch installeren

1. De elastische Stack versie 5.0 en hoger vereist Java 8. Voer de opdracht `java -version` uw versie controleren. Als u nog geen java installeren, Raadpleeg de documentatie op [van Oracle-website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Download het juiste binaire pakket voor uw systeem:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Andere installatiemethoden kunnen worden gevonden op [Elasticsearch installatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Controleer of Elasticsearch met de opdracht is uitgevoerd:

    ```
    curl http://127.0.0.1:9200
    ```

    U ziet een reactie vergelijkbaar met het volgende:

    ```
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

Zie voor verdere instructies voor het installeren van elastische zoeken naar de pagina [installatie](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash installeren

1. Voer de volgende opdrachten Logstash installeren:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Vervolgens moet Logstash voor toegang tot en het parseren van de logboeken van de stroom te configureren. Maak een logstash.conf-bestand met:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. De volgende inhoud toevoegen aan het bestand:

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

Raadpleeg voor meer informatie over het installeren van Logstash de [officiële documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Installeren van de invoer Logstash-invoegtoepassing voor Azure blob-opslag

Deze invoegtoepassing Logstash kunt u rechtstreeks toegang krijgen tot de logboeken van de stroom van hun aangewezen storage-account. Voer de opdracht uit de standaardinstallatiemap van Logstash (in dit geval /usr/share/logstash/bin) voor het installeren van deze invoegtoepassing:

```
logstash-plugin install logstash-input-azureblob
```

Voer de opdracht Logstash starten:

```
sudo /etc/init.d/logstash start
```

Raadpleeg de documentatie voor meer informatie over deze invoegtoepassing [hier](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)

### <a name="install-kibana"></a>Kibana installeren

1. Voer de volgende opdrachten voor het installeren van Kibana:

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. Kibana gebruik de opdrachten uitvoeren:

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. Als u wilt de webinterface voor uw Kibana weergeven, gaat u naar`http://localhost:5601`
1. In dit scenario is het patroon index voor de logboeken van de stroom 'nsg-flow-logs'. U kunt het patroon van de index in de sectie 'uitvoer' van het bestand logstash.conf wijzigen.

1. Als u weergeven van het dashboard Kibana op afstand wilt, maakt u een inkomende NSG-regel om toegang te kunnen **5601 poort**.

### <a name="create-a-kibana-dashboard"></a>Een dashboard Kibana maken

We hebben een voorbeelddashboard u trends en details weergeven in uw waarschuwingen opgegeven voor dit artikel.

![afbeelding 1][1]

1. Download het bestand dashboard [hier](https://aka.ms/networkwatchernsgflowlogdashboard), het bestand visualisatie [hier](https://aka.ms/networkwatchernsgflowlogvisualizations), en het bestand opgeslagen zoekopdracht [hier](https://aka.ms/networkwatchernsgflowlogsearch).

1. Onder de **Management** tabblad van Kibana, gaat u naar **opgeslagen objecten** en alle drie bestanden te importeren. Klik vanuit de **Dashboard** tabblad kunt u openen en de voorbeelddashboard laden.

U kunt ook uw eigen visualisaties en -dashboards naar metrische gegevens van uw eigen belang aangepast maken. Meer informatie over het maken van Kibana visualisaties van de Kibana [officiële documentatie](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>NSG-logboeken voor stroom visualiseren

De voorbeelddashboard biedt verschillende visualisaties van de stroom-Logboeken:

1. Loopt door besluit/richting gedurende een periode - time series grafieken met het aantal stromen gedurende de periode. U kunt de tijdseenheid en bereik van deze beide visualisaties bewerken. Stromen beschikking toont het aandeel van toestaan of weigeren beslissingen, hoewel stromen door richting wordt aangegeven dat het aandeel van binnenkomend en uitgaand verkeer. U kunt met deze visuele verkeer trends na verloop van tijd onderzoeken en zoekt u alle pieken of ongebruikelijke patronen.

  ![afbeelding 2][2]

1. Loopt door bestemming/bronpoort – cirkeldiagrammen de uitsplitsing van stromen naar hun respectieve poorten wordt weergegeven. Aan deze weergave ziet u de meest gebruikte poorten. Als u op een specifieke poort binnen het cirkeldiagram klikt, wordt de rest van het dashboard naar beneden stromen van die poort filteren.

  ![figure3][3]

1. Aantal stromen en vroegste logboek tijd – metrische gegevens die u met het aantal overdrachten geregistreerd en de datum van het eerste logboek vastgelegd.

  ![figure4][4]

1. Stromen door het NSG en regel: een staafdiagram waarin u de distributie van stromen binnen elke NSG, evenals de distributie van regels binnen elke NSG. Hier ziet u welke NSG en de regels voor de meeste verkeer gegenereerd.

  ![figure5][5]

1. Top 10 bron/het doel IP-adressen – staafdiagrammen met de top 10 bron en doel-IP-adressen. U kunt deze grafieken om weer te geven van meer of minder bovenste IP-adressen aanpassen. Hier ziet u het meest optreden IP-adressen, evenals het verkeer besluit (toestaan of weigeren) wordt gemaakt voor elke IP.

  ![figure6][6]

1. Stroom Tuples – deze tabel ziet u de informatie in elke tuple stroom, evenals de bijbehorende NGS en de regel.

  ![figure7][7]

Met behulp van de query-balk aan de bovenkant van het dashboard, kunt u filteren op het dashboard op basis van een parameter van de flows zoals abonnements-ID, resourcegroepen, regel of een andere variabele van belang. Raadpleeg voor meer informatie over Kibana van query's en filters, de [officiële documentatie](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Conclusie

Door de logboeken van de stroom van de Netwerkbeveiligingsgroep in combinatie met de elastische Stack, hebben we actief krachtige manier om te visualiseren onze netwerkverkeer. Deze dashboards kunnen u snel toegang en inzichten te delen over uw netwerkverkeer, evenals de filter omlaag en onderzoek op alle mogelijke afwijkingen. Kibana gebruikt, kunt u deze dashboards aanpassen en maak specifieke visualisaties om te voldoen aan de behoeften van elke beveiliging, controle en naleving.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG stroom logboeken met Power BI in via [visualiseren NSG loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
