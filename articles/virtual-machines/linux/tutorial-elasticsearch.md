---
title: ElasticSearch implementeren op een andere virtuele machine in Azure
description: Zelfstudie - installatie van de elastische Stack op een ontwikkel Linux VM in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a>De elastische Stack installeren op een virtuele machine in Azure

Dit artikel begeleidt u bij het implementeren van [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), en [Kibana](https://www.elastic.co/products/kibana), op een Ubuntu VM in Azure. Overzicht van de elastische Stack in actie kunt u eventueel verbinding maken met Kibana en werkt met sommige gegevens in een logboek voorbeeld. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een VM Ubuntu in een Azure-resourcegroep
> * Elasticsearch, Logstash en Kibana installeren op de virtuele machine
> * Voorbeeldgegevens naar Elasticsearch met Logstash verzenden 
> * Openen van poorten en werken met gegevens in de console Kibana


 Deze implementatie is geschikt voor basic ontwikkeling met de elastische Stack. Zie voor meer informatie over de elastische Stack, aanbevelingen voor een productie-omgeving, inclusief de [elastische documentatie](https://www.elastic.co/guide/index.html) en de [Azure architectuur Center](/azure/architecture/elasticsearch/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm#create). 

In het volgende voorbeeld wordt een VM gemaakt met de naam *myVM* en worden er SSH-sleutels gemaakt, als deze nog niet bestaan op een standaardsleutellocatie. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Wanneer de virtuele machine is gemaakt, toont de Azure CLI informatie die lijkt op de informatie in het volgende voorbeeld. Noteer het `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a>SSH in uw virtuele machine

Als u al het openbare IP-adres van uw virtuele machine niet weet, voert u de [az netwerk openbare ip-lijst](/cli/azure/network/public-ip#list) opdracht:

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Gebruik de volgende opdracht om een SSH-sessie te starten voor de virtuele machine. Vervangen door de juiste openbare IP-adres van uw virtuele machine. In dit voorbeeld wordt het IP-adres is *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a>De elastische Stack installeren

De handtekeningsleutel Elasticsearch importeren en bijwerken van de opslagplaats elastische pakket opnemen in de lijst APT bronnen:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

Op de virtuele machine van de virtuele Java installeren en configureren van de variabele deze JAVA_HOME is nodig voor de elastische Stack-onderdelen worden uitgevoerd.

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Voer de volgende opdrachten om te werken Ubuntu pakket bronnen en Elasticsearch, Kibana en Logstash installeren.

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> Gedetailleerde installatie-instructies, met inbegrip van directory-indelingen en de initiële configuratie worden bijgehouden in [elastische van documentatie](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

## <a name="start-elasticsearch"></a>Elasticsearch starten 

Elasticsearch starten op de virtuele machine met de volgende opdracht:

```bash
sudo systemctl start elasticsearch.service
```

Met deze opdracht produceert geen uitvoer, dus controleren of Elasticsearch wordt uitgevoerd op de virtuele machine met deze `curl` opdracht:

```bash
curl -XGET 'localhost:9200/'
```

Als Elasticsearch wordt uitgevoerd, ziet u de volgende uitvoer:

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a>Start Logstash en gegevens toevoegen aan Elasticsearch

Logstash starten met de volgende opdracht:

```bash 
sudo systemctl start logstash.service
```

Test Logstash in interactieve modus om te controleren of dat deze correct werkt:

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

Dit is een eenvoudige logstash [pijplijn](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) die een echo standaardinvoer naar de standaarduitvoer. 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

Logstash instellen voor het doorsturen van de kernel-berichten van deze virtuele machine naar Elasticsearch. Maak een nieuw bestand in een lege map met de naam `vm-syslog-logstash.conf` en plak in de volgende Logstash-configuratie:

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

Deze configuratie te testen en de syslog-gegevens verzenden naar Elasticsearch:

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

U ziet de syslog-vermeldingen in uw terminal als ze worden verzonden naar Elasticsearch teruggestuurd. Gebruik `CTRL+C` om af te sluiten buiten Logstash als u bepaalde gegevens hebt verzonden.

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a>Kibana starten en de gegevens in Elasticsearch visualiseren

Bewerken `/etc/kibana/kibana.yml` en wijzig het IP-adres Kibana luistert op zodat u deze vanuit de webbrowser openen kunt.

```bash
server.host:"0.0.0.0"
```

Kibana starten met de volgende opdracht:

```bash
sudo systemctl start kibana.service
```

Open poort 5601 met Azure CLI voor externe toegang naar de console Kibana:

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

Open de console Kibana en selecteer **maken** voor het genereren van een standaardindex op basis van de syslog-gegevens die u eerder naar Elasticsearch verzonden. 

![Syslog-gebeurtenissen in Kibana bladeren](media/elasticsearch-install/kibana-index.png)

Selecteer **Discover** bladeren op de Kibana-console om te zoeken en filteren via de syslog-gebeurtenissen.

![Syslog-gebeurtenissen in Kibana bladeren](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u de elastische Stack geïmplementeerd in een virtuele machine in Azure-ontwikkeling. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maak een VM Ubuntu in een Azure-resourcegroep
> * Elasticsearch, Logstash en Kibana installeren op de virtuele machine
> * Voorbeeldgegevens Elasticsearch verzenden van Logstash 
> * Openen van poorten en werken met gegevens in de console Kibana