---
title: Inbraakdetectie netwerk met Azure-netwerk-Watcher en open-source hulpprogramma's uitvoeren | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure-netwerk-Watcher en open source-hulpprogramma's om uit te voeren inbraakdetectie netwerk
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: aff1b5f9e8860d3b8dc09b37684bb8a4ac2bf134
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Inbraakdetectie netwerk met de netwerk-Watcher en open-source hulpprogramma's uitvoeren

Pakket mogelijk zijn een belangrijk onderdeel voor de implementatie van network inbraakdetectie detectiesystemen (id's) en Network Security Monitoring (NSM) uitvoeren. Er zijn verschillende hulpmiddelen van de open-source-id's die pakket opnamen verwerken en zoek naar de handtekeningen van mogelijke aanvallen en schadelijke activiteiten. Met behulp van het pakket worden opgegeven door de netwerk-Watcher, kunt u uw netwerk voor eventuele schadelijke beveiligingsrisico's of beveiligingsproblemen analyseren.

Dergelijke open-source hulpprogramma is Suricata, een id-engine die gebruikmaakt van rulesets netwerkverkeer te bewaken en waarschuwingen wordt geactiveerd wanneer verdachte gebeurtenissen optreden. Suricata biedt een met meerdere threads-engine, wat betekent dat netwerkverkeeranalyse met hogere snelheid en efficiëntie kan uitvoeren. Ga naar de website op https://suricata-ids.org/ voor meer informatie over Suricata en de mogelijkheden ervan.

## <a name="scenario"></a>Scenario

Dit artikel wordt uitgelegd hoe u uw omgeving uit te voeren inbraakdetectie netwerk met behulp van de netwerk-Watcher Suricata en de elastische Stack instelt. Netwerk-Watcher biedt u de pakket-opnamen gebruikt voor het detecteren van indringers netwerk uitvoeren. Suricata verwerkt de opnamen van pakket en de trigger-waarschuwingen op basis van pakketten die overeenkomen met de opgegeven ruleset van bedreigingen. Deze waarschuwingen worden opgeslagen in een logboekbestand op uw lokale machine. Met behulp van de elastische Stack, kunnen de logboeken die worden gegenereerd door Suricata worden geïndexeerd en gebruikt voor het maken van een Kibana dashboard, zonder dat u een visuele representatie van de logboekbestanden en een middel snel om inzicht te krijgen voor mogelijke beveiligingsproblemen van het netwerk.  

![Toepassingsscenario eenvoudige web][1]

Beide open-source hulpprogramma's kunnen worden ingesteld op een virtuele machine van Azure zodat u kunt deze analyse in uw eigen Azure netwerkomgeving uitvoeren.

## <a name="steps"></a>Stappen

### <a name="install-suricata"></a>Suricata installeren

Ga naar http://suricata.readthedocs.io/en/latest/install.html voor alle andere methoden van installatie

1. Voer in de opdrachtregeloptie terminal van uw virtuele machine in de volgende opdrachten:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. Voer de opdracht uit om te controleren of de installatie, `suricata -h` om te zien van de volledige lijst met opdrachten.

### <a name="download-the-emerging-threats-ruleset"></a>Het ruleset opkomende bedreigingen downloaden

In dit stadium hoeft u niet alle regels voor Suricata om uit te voeren. U kunt uw eigen regels kunt maken als er specifieke bedreigingen met uw netwerk die u wilt detecteren of u kunt ook gebruik ontwikkeld regelsets van een aantal providers, zoals opkomende bedreigingen of VRT regels uit Snort. We de vrij toegankelijk opkomende bedreigingen ruleset hier gebruiken:

Download de regelset en kopieer deze naar de map:

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Proces pakket worden vastgelegd met Suricata

Voor het verwerken van pakket worden vastgelegd met behulp van Suricata, voer de volgende opdracht:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
U kunt de resulterende waarschuwingen controleren door het bestand fast.log worden gelezen:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a>Instellen van de elastische Stack

Terwijl de logboeken die Suricata produceert waardevolle informatie bevatten over wat er in onze netwerk gebeurt, zijn deze logboekbestanden niet het gemakkelijkst worden gelezen en begrepen. Door Suricata te koppelen aan de elastische Stack, kunnen we een Kibana-dashboard maken wat ons te zoeken, grafiek, analyseren en inzichten worden afgeleid van onze Logboeken is toegestaan.

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
1. Vervolgens moet Logstash lezen uit de uitvoer van het bestand eve.json configureren. Maak een logstash.conf-bestand met:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. De volgende inhoud toevoegen aan het bestand (Zorg ervoor dat het pad naar het bestand eve.json juist is):

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. Zorg ervoor dat de juiste machtigingen heeft om het bestand eve.json zodat Logstash kan het bestand opnemen.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. Voer de opdracht Logstash starten:

    ```
    sudo /etc/init.d/logstash start
    ```

Raadpleeg voor meer informatie over het installeren van Logstash de [officiële documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

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
1. Voor dit scenario, het patroon index voor de logboeken Suricata is ' logstash-* "

1. Als u weergeven van het dashboard Kibana op afstand wilt, maakt u een inkomende NSG-regel om toegang te kunnen **5601 poort**.

### <a name="create-a-kibana-dashboard"></a>Een dashboard Kibana maken

We hebben een voorbeelddashboard u trends en details weergeven in uw waarschuwingen opgegeven voor dit artikel.

1. Download het bestand dashboard [hier](https://aka.ms/networkwatchersuricatadashboard), het bestand visualisatie [hier](https://aka.ms/networkwatchersuricatavisualization), en het bestand opgeslagen zoekopdracht [hier](https://aka.ms/networkwatchersuricatasavedsearch).

1. Onder de **Management** tabblad van Kibana, gaat u naar **opgeslagen objecten** en alle drie bestanden te importeren. Klik vanuit de **Dashboard** tabblad kunt u openen en de voorbeelddashboard laden.

U kunt ook uw eigen visualisaties en -dashboards naar metrische gegevens van uw eigen belang aangepast maken. Meer informatie over het maken van Kibana visualisaties van de Kibana [officiële documentatie](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana-dashboard][2]

### <a name="visualize-ids-alert-logs"></a>Id's waarschuwing logboeken visualiseren

De voorbeelddashboard biedt verschillende visualisaties van de waarschuwing Suricata-Logboeken:

1. Waarschuwingen per GeoIP – een map waarin de verdeling van waarschuwingen door het land van oorsprong op basis van geografische locatie (zoals bepaald door IP)

    ![geo-ip][3]

1. Top 10 waarschuwingen – een samenvatting van de 10 meest voorkomende geactiveerde waarschuwingen en de bijbehorende beschrijvingen. Te klikken op een afzonderlijke waarschuwing filtert u het dashboard om de informatie die betrekking hebben op die specifieke waarschuwing.

    ![afbeelding 4][4]

1. Aantal waarschuwingen: het totale aantal waarschuwingen geactiveerd door de ruleset

    ![afbeelding 5][5]

1. Top 20 bron/het doel IP-adressen/poorten - cirkeldiagrammen met de top 20 IP-adressen en poorten die waarschuwingen zijn gegenereerd op. U kunt filteren omlaag op specifieke IP-adressen/poorten om te zien hoeveel en welke soort waarschuwingen zijn geactiveerd.

    ![afbeelding 6][6]

1. Waarschuwing samengevat: een tabel van specifieke details van elke afzonderlijke waarschuwingen samen te vatten. U kunt deze tabel om andere parameters van belang zijn voor elke waarschuwing te tonen.

    ![afbeelding 7][7]

Zie voor meer documentatie over het maken van aangepaste visualisaties en dashboards [officiële documentatie van Kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Conclusie

Door een combinatie van pakket worden opgegeven door de netwerk-Watcher en open source-id's hulpprogramma's zoals Suricata, u inbraakdetectie netwerk voor een breed scala aan bedreigingen uitvoeren kunt. Deze dashboards kunnen u om snel trends en afwijkingen in uw netwerk als goed dig in de gegevens voor het detecteren van de belangrijkste oorzaken van waarschuwingen, zoals agents kwaadwillende gebruiker of kwetsbare poorten. Met deze geëxtraheerde gegevens, kunt u weloverwogen beslissingen over het reageren op en uw netwerk beveiligen door schadelijke inbraakdetectie pogingen en regels maken om te voorkomen dat toekomstige beveiligingsrisico's aan uw netwerk.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het activeren van pakket opnamen op basis van waarschuwingen in via [pakketopname voor proactieve netwerkbewaking met Azure Functions gebruiken](network-watcher-alert-triggered-packet-capture.md)

Meer informatie over het visualiseren van uw NSG stroom logboeken met Power BI in via [visualiseren NSG loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
