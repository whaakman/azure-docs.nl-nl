---
title: Netwerk indringers detecteren met Azure Network Watcher en open-source hulpprogramma's | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van Azure Network Watcher en open source-hulpprogramma's voor het netwerk indringers detecteren
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 41b039c6598334d49b9ddb0608d7ad2ae045b223
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57901267"
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Netwerk indringers detecteren met Network Watcher en open-sourcehulpprogramma 's

Pakketopnamen zijn een belangrijk onderdeel voor het implementeren van netwerksystemen voor het detecteren van indringers (id's) en Network Security Monitoring (NSM) uitvoeren. Er zijn verschillende hulpmiddelen van de open-source-id's die pakketopnamen verwerken en zoek naar handtekeningen van mogelijke aanvallen en schadelijke activiteiten. Met behulp van het pakket wordt vastgelegd, opgegeven door de Network Watcher, kunt u uw netwerk voor eventuele schadelijke indringers of beveiligingsproblemen analyseren.

Een dergelijke open-source-hulpprogramma is Suricata, een engine voor id's die gebruikmaakt van rulesets netwerkverkeer te bewaken en activeert waarschuwingen wanneer er verdachte gebeurtenissen plaatsvinden. Suricata biedt een engine voor meerdere threads, wat betekent dat het een netwerkverkeeranalyse met hogere snelheid en efficiëntie kunt uitvoeren. Voor meer informatie over Suricata en de mogelijkheden ervan, bezoekt u hun website op https://suricata-ids.org/.

## <a name="scenario"></a>Scenario

In dit artikel wordt uitgelegd hoe u uw omgeving instellen op het netwerk indringers detecteren met behulp van Network Watcher, Suricata en de Elastic Stack. Network Watcher biedt u de pakketopnamen gebruikt voor het netwerk indringers detecteren. Suricata verwerkt de pakketopnamen en trigger waarschuwingen op basis van pakketten die overeenkomen met de opgegeven ruleset van bedreigingen. Deze waarschuwingen worden opgeslagen in een logboekbestand op uw lokale computer. Met behulp van de Elastic Stack, kunnen de logboeken die worden gegenereerd door Suricata worden geïndexeerd en gebruikt voor het maken van een Kibana-dashboard, zodat u met een visuele weergave van de logboeken en een manier om te snel inzicht krijgt in de mogelijke beveiligingsproblemen van het netwerk.  

![scenario voor eenvoudige web-toepassing][1]

Beide open-sourcehulpprogramma's kunnen worden ingesteld op een Azure-VM, zodat u kunt om uit te voeren van deze analyse uit binnen uw eigen Azure-netwerkomgeving.

## <a name="steps"></a>Stappen

### <a name="install-suricata"></a>Suricata installeren

Voor alle andere methoden van installatie, gaat u naar https://suricata.readthedocs.io/en/latest/install.html

1. Voer de volgende opdrachten in de opdrachtregelterminal van uw virtuele machine:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. Voer de opdracht uit om te controleren of de installatie, `suricata -h` om te zien van de volledige lijst met opdrachten.

### <a name="download-the-emerging-threats-ruleset"></a>Het ruleset opkomende bedreigingen downloaden

In dit stadium is er geen regels voor Suricata om uit te voeren. U kunt uw eigen regels kunt maken als er zijn specifieke bedreigingen voor uw netwerk die u wilt detecteren of u kunt ook gebruik ontwikkeld regelsets uit een aantal providers, zoals opkomende bedreigingen of VRT regels uit Snort. We hier de vrij toegankelijke opkomende bedreigingen ruleset gebruiken:

Download de regelset en kopieer deze naar de map:

```
wget https://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Proces-pakket wordt vastgelegd met Suricata

Voor het verwerken van pakket vastgelegd met behulp van Suricata, voer de volgende opdracht uit:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
Om te controleren of de daaruit voortkomende waarschuwingen, het fast.log-bestand te lezen:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a>Instellen van de Elastic Stack

Terwijl de logboeken die Suricata produceert waardevolle informatie bevatten over wat op ons netwerk gebeurt er, zijn deze logboekbestanden niet het gemakkelijkst worden gelezen en begrepen. Suricata in contact met de Elastic Stack, kunnen we een Kibana-dashboard maken wat kunnen we zoeken, in een grafiek, analyseren en inzichten die zijn afgeleid van onze Logboeken.

#### <a name="install-elasticsearch"></a>Elasticsearch installeren

1. De Elastic Stack versie 5.0 en hoger is vereist voor Java 8. Voer de opdracht `java -version` om uw versie te controleren. Als u geen java geïnstalleerd hebt, Raadpleeg de documentatie op de [Azure ondersteund JDK](https://aka.ms/azure-jdks).

1. Download het juiste binaire pakket voor uw systeem:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Andere installatiemethoden kunnen worden gevonden op [Elasticsearch-installatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Controleren of Elasticsearch wordt uitgevoerd met de opdracht:

    ```
    curl http://127.0.0.1:9200
    ```

    U ziet een antwoord ongeveer als volgt uit:

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

Raadpleeg voor verdere instructies voor installeren elastische zoeken naar de pagina [installatie](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash installeren

1. Voer de volgende opdrachten Logstash installeren:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Vervolgens moeten we Logstash te lezen uit de uitvoer van eve.json bestand configureren. Maak een logstash.conf bestand met:

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

1. Zorg ervoor dat u de juiste machtigingen heeft om het bestand eve.json geven zodat Logstash kan het bestand opnemen.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. Start Logstash voert u de opdracht uit:

    ```
    sudo /etc/init.d/logstash start
    ```

Voor verdere instructies over het installeren van Logstash, raadpleegt u de [officiële documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Install Kibana

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

1. Als u de webinterface voor uw Kibana, gaat u naar `http://localhost:5601`
1. In dit scenario voor de index-patroon gebruikt voor de logboeken Suricata is ' logstash-* "

1. Als u wilt om op afstand de Kibana-dashboard weer te geven, maakt u een inkomende NSG-regel toestaan van toegang tot **poort 5601**.

### <a name="create-a-kibana-dashboard"></a>Een Kibana-dashboard maken

In dit artikel hebben we een voorbeelddashboard voor u trends en details weergeven in uw waarschuwingen verstrekt.

1. Download het dashboardbestand [hier](https://aka.ms/networkwatchersuricatadashboard), het bestand visualisatie [hier](https://aka.ms/networkwatchersuricatavisualization), en het bestand opgeslagen zoekopdracht [hier](https://aka.ms/networkwatchersuricatasavedsearch).

1. Onder de **Management** tabblad van Kibana, gaat u naar **opgeslagen objecten** en alle drie bestanden te importeren. Klik vanuit de **Dashboard** tabblad kunt u openen en het voorbeelddashboard laden.

U kunt ook uw eigen visualisaties en dashboards die zijn afgestemd op uw eigen nuttige meetgegevens beschikbaar maken. Meer informatie over het maken van Kibana visualisaties uit de Kibana [officiële documentatie](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana-dashboard][2]

### <a name="visualize-ids-alert-logs"></a>Id's waarschuwing logboeken te visualiseren

Het voorbeelddashboard van biedt verschillende visualisaties van de waarschuwing zich Suricata:

1. Waarschuwingen op basis van GeoIP – een map waarin de verdeling van waarschuwingen op basis van het land van oorsprong op basis van geografische locatie (bepaald door IP)

    ![geo-ip][3]

1. Top 10 waarschuwingen – een samenvatting van de 10 meest frequente geactiveerde waarschuwingen en de bijbehorende beschrijvingen. Filters te klikken op een afzonderlijke waarschuwing in het dashboard om de gegevens met betrekking tot die specifieke waarschuwing.

    ![afbeelding 4][4]

1. Aantal waarschuwingen: het totale aantal waarschuwingen dat door het ruleset geactiveerd

    ![afbeelding 5][5]

1. Top 20 bron-/ doel-IP-adressen/poorten - cirkeldiagrammen met de top 20 IP-adressen en poorten die waarschuwingen zijn geactiveerd door op. U kunt filteren, omlaag op specifieke IP-adressen/poorten om te zien hoeveel en welke soort waarschuwingen zijn geactiveerd.

    ![afbeelding 6][6]

1. Waarschuwing overzicht-een tabel met een overzicht van specifieke details van elke afzonderlijke waarschuwing. U kunt deze tabel om andere parameters van belang zijn voor elke waarschuwing weer te geven.

    ![afbeelding 7][7]

Zie voor meer documentatie over het maken van aangepaste visualisaties en dashboards [officiële documentatie van Kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Conclusie

Door een combinatie van pakket vastgelegd opgegeven door de Network Watcher en open source-id's hulpprogramma's zoals Suricata, u inbraakdetectie netwerk voor een breed scala aan bedreigingen uitvoeren kunt. Deze dashboards kunnen u om snel trends en afwijkingen in uw netwerk, als ook Duik in de gegevens voor het detecteren van de belangrijkste oorzaken van waarschuwingen, zoals de kwaadwillende Gebruikeragents of kwetsbaar poorten. Met deze opgehaalde gegevens, kunt u onderbouwde beslissingen over het reageren op uw netwerk beschermen tegen schadelijke indringingspogingen, en regels om te voorkomen dat toekomstige indringers om uw netwerk te maken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het activeren van pakketopnamen op basis van waarschuwingen, recentst [pakketopname gebruiken voor proactieve netwerkbewaking met Azure Functions](network-watcher-alert-triggered-packet-capture.md)

Meer informatie over het visualiseren van uw NSG-stroomlogboeken met Power BI recentst [visualiseren NSG-stromen logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
