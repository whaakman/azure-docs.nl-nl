---
title: Maken van een virtuele-Machineschaalsets voor Linux in Azure | Microsoft Docs
description: Maken en implementeren van een maximaal beschikbare toepassing op virtuele Linux-machines met behulp van een virtuele-machineschaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 8703d0c06f2507cc3c21d4280d887a8772145a28
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Een virtuele-Machineschaalset maken en implementeren van een maximaal beschikbare app op Linux
Een virtuele-machineschaalset kunt u om te implementeren en beheren van een reeks identiek zijn, automatisch schalen virtuele machines. U kunt het aantal virtuele machines in de schaalset handmatig schalen of regels automatisch te schalen op basis van het gebruik van bronnen zoals CPU, geheugen-aanvraag of netwerkverkeer definiëren. In deze zelfstudie maakt implementeren u een virtuele-machineschaalset instellen in Azure. Procedures voor:

> [!div class="checklist"]
> * Cloud-init gebruiken voor het maken van een app schalen
> * Maken van een virtuele-machineschaalset
> * Vergroten of verkleinen het aantal exemplaren in een schaalset
> * Maken van regels voor automatisch schalen
> * Verbindingsgegevens voor scale set exemplaren weergeven
> * Gegevensschijven in een schaalset gebruiken


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.22 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Overzicht van Scale Set
Een virtuele-machineschaalset kunt u om te implementeren en beheren van een reeks identiek zijn, automatisch schalen virtuele machines. Virtuele machines in een schaalset worden gedistribueerd over logica probleem- en domeinen in een of meer *plaatsing groepen*. Dit zijn groepen op vergelijkbare wijze geconfigureerde virtuele machines, vergelijkbaar met [beschikbaarheidssets](tutorial-availability-sets.md).

Virtuele machines worden gemaakt als nodig in een schaalset. Definieert u de regels voor automatisch schalen om te bepalen hoe en wanneer virtuele machines worden toegevoegd of verwijderd uit de schaalaanpassingsset. Deze regels kunnen worden geactiveerd op basis van de metrische gegevens zoals CPU-belasting, geheugengebruik of netwerkverkeer.

Schaal wordt ondersteuning voor maximaal 1000 VMs ingesteld wanneer u een installatiekopie van een Azure-platform. Voor werkbelastingen met aanzienlijke installatie of VM aanpassingsvereisten mogelijk wilt u [maken van een aangepaste VM-installatiekopie](tutorial-custom-images.md). U kunt maximaal 300 virtuele machines in een schaal instelt met behulp van een aangepaste installatiekopie maken.


## <a name="create-an-app-to-scale"></a>Maak een app schalen
Voor gebruik in productieomgevingen, u kunt desgewenst [maken van een aangepaste VM-installatiekopie](tutorial-custom-images.md) waarin uw toepassing geïnstalleerd en geconfigureerd. Voor deze zelfstudie kunt aanpassen van de virtuele machines op de eerste keer wordt opgestart om snel een schaal instelt in actie zien.

In een vorige zelfstudie hebt u geleerd [het aanpassen van een virtuele Linux-machine op de eerste keer opstarten](tutorial-automate-vm-deployment.md) met cloud-init. U kunt het bestand met dezelfde configuratie cloud init NGINX installeren en uitvoeren van een eenvoudige 'Hallo wereld' Node.js-app. 

Maak een bestand met de naam in uw huidige shell *cloud init.txt* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. Voer `sensible-editor cloud-init.txt` voor het maken van het bestand en een overzicht van beschikbare editors. Controleer of het hele cloud-init-bestand correct is gekopieerd met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="create-a-scale-set"></a>Maken van een schaalset
Voordat u een schaalset maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupScaleSet* in de *eastus* locatie:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Maak nu de schaal van een virtuele machine in te stellen [az vmss maken](/cli/azure/vmss#create). Het volgende voorbeeld wordt een set met de naam scale *myScaleSet*, gebruikt u het cloud-init-bestand voor het aanpassen van de virtuele machine en SSH-sleutels worden gegenereerd als deze nog niet bestaan:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```

Het duurt enkele minuten voor het maken en configureren van de schaal set resources en virtuele machines. Er zijn achtergrondtaken die uitvoeren blijven nadat de Azure CLI keert u terug naar de prompt. Het is mogelijk een andere paar minuten voordat u toegang hebt tot de app.


## <a name="allow-web-traffic"></a>Webverkeer toestaan
Een load balancer is automatisch als onderdeel van de virtuele-machineschaalset gemaakt. De load balancer wordt verkeer over een reeks gedefinieerde virtuele machines met regels voor load balancer. U kunt meer informatie over de load balancer concepten en configuratie in de volgende zelfstudie [het laden van virtuele machines in Azure](tutorial-load-balancer.md).

Om verkeer te staan om te bereiken van de web-app, maakt u een regel met [az network Load Balancer-regel maken](/cli/azure/network/lb/rule#create). Het volgende voorbeeld wordt een regel met naam *myLoadBalancerRuleWeb*:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Uw app testen
Uw Node.js-app op het web vindt verkrijgen van het openbare IP-adres van de load balancer met [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#show). Het volgende voorbeeld verkrijgt het IP-adres voor *myScaleSetLBPublicIP* gemaakt als onderdeel van de schaal is ingesteld:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Geef het openbare IP-adres in naar een webbrowser. De app wordt weergegeven, inclusief de hostnaam van de virtuele machine die de load balancer verkeer naar gedistribueerde:

![Actieve Node.js-app](./media/tutorial-create-vmss/running-nodejs-app.png)

Overzicht van de schaal instelt in actie, u kunt force vernieuwen uw webbrowser als u wilt zien van de load balancer verkeer verdelen over alle virtuele machines waarop uw app wordt uitgevoerd.


## <a name="management-tasks"></a>Beheertaken
Gedurende de levenscyclus van de schaal is ingesteld, moet u wellicht een of meer beheertaken uitvoeren. Bovendien wilt u scripts maken die verschillende lifecycle-taken automatiseren. De Azure CLI 2.0 biedt een snelle manier om deze taken. Hier volgen enkele algemene taken.

### <a name="view-vms-in-a-scale-set"></a>Weergave virtuele machines in een schaalset
Een lijst van virtuele machines die worden uitgevoerd in de schaalset wilt weergeven, gebruikt u [az vmss lijstexemplaren](/cli/azure/vmss#list-instances) als volgt:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

De uitvoer lijkt op die in het volgende voorbeeld:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Vergroten of verkleinen van VM-exemplaren
Als het aantal exemplaren dat u momenteel in een schaalset hebt wilt weergeven, gebruikt [az vmss weergeven](/cli/azure/vmss#show) en query's uitvoeren op *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

U kunt vervolgens handmatig vergroten of verkleinen het aantal virtuele machines in de schaal in te stellen [az vmss scale](/cli/azure/vmss#scale). Het volgende voorbeeld wordt het aantal VM's in uw ingesteld op schaal *3*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 3
```


### <a name="configure-autoscale-rules"></a>Regels voor automatisch schalen configureren
In plaats van het aantal exemplaren handmatig schalen in uw scale is ingesteld, kunt u regels voor automatisch schalen kunt definiëren. Deze regels de exemplaren in de schaalset bewaken en hierop reageren dienovereenkomstig op basis van metrische gegevens en drempels die u definieert. Het volgende voorbeeld uitgeschaald het aantal exemplaren van een wanneer de gemiddelde CPU-belasting groter dan 60% gedurende een periode van 5 minuten is. Als de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode van 5 minuten, worden de instanties geschaald in door één exemplaar. Uw abonnements-ID wordt gebruikt voor het bouwen van de resource-URI's voor de verschillende schaal set onderdelen. Maken van deze regels met [az monitor automatisch schalen-instellingen maken](/cli/azure/monitor/autoscale-settings#create), kopiëren en plakken van de volgende opdracht profiel voor automatisch schalen:

```azurecli-interactive 
sub=$(az account show --query id -o tsv)

az monitor autoscale-settings create \
    --resource-group myResourceGroupScaleSet \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "East US",
      "notifications": [],
      "profiles": [
        {
          "name": "Auto created scale condition",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
    }'
```

Als u wilt gebruiken in het profiel voor automatisch schalen, kunt u een JSON (JavaScript Object Notation)-bestand maken en geeft die aan de `az monitor autoscale-settings create` opdracht met de `--parameters @autoscale.json` parameter. Zie voor meer ontwerpinformatie over het gebruik van automatisch schalen, [aanbevolen procedures voor automatisch schalen](/azure/architecture/best-practices/auto-scaling).


### <a name="get-connection-info"></a>Verbindingsgegevens ophalen
Gebruik om verbindingsinformatie over de VM's in uw schaalsets [az vmss lijst--verbinding-Instantiegegevens](/cli/azure/vmss#list-instance-connection-info). Met deze opdracht levert de openbare IP-adres en poort voor elke virtuele machine waarmee u verbinding maken met SSH:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Gegevensschijven met schaalsets gebruiken
U kunt maken en gebruiken van gegevensschijven met-schaalsets. In een vorige zelfstudie hebt u geleerd hoe u [Azure beheren schijven](tutorial-manage-disks.md) die licht de aanbevolen procedures en verbeterde prestaties voor het bouwen van apps op gegevensschijven in plaats van de besturingssysteemschijf.

### <a name="create-scale-set-with-data-disks"></a>Schaalset met gegevensschijven maken
Voor het maken van een schaalset en gegevensschijven koppelen, voeg de `--data-disk-sizes-gb` -parameter voor de [az vmss maken](/cli/azure/vmss#create) opdracht. Het volgende voorbeeld wordt een instellen met schaal *50*Gb gegevensschijven gekoppeld aan elk exemplaar:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Wanneer instanties zijn verwijderd uit een schaalset, worden ook eventuele gekoppelde gegevensschijven verwijderd.

### <a name="add-data-disks"></a>Gegevensschijven toevoegen
U kunt een gegevensschijf toevoegen aan exemplaren in de schaalset met [az vmss schijf koppelen](/cli/azure/vmss/disk#attach). Het volgende voorbeeld wordt een *50*Gb schijfruimte op elk exemplaar:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Loskoppelen van gegevensschijven
U kunt een gegevensschijf aan exemplaren in de schaalset verwijderen [az vmss schijf loskoppelen](/cli/azure/vmss/disk#detach). Het volgende voorbeeld verwijdert u de gegevensschijf met LUN *2* van elk exemplaar:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een virtuele-machineschaalset gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Cloud-init gebruiken voor het maken van een app schalen
> * Maken van een virtuele-machineschaalset
> * Vergroten of verkleinen het aantal exemplaren in een schaalset
> * Maken van regels voor automatisch schalen
> * Verbindingsgegevens voor scale set exemplaren weergeven
> * Gegevensschijven in een schaalset gebruiken

Ga naar de volgende zelfstudie voor meer informatie over taakverdeling concepten voor virtuele machines.

> [!div class="nextstepaction"]
> [Load balance virtuele machines](tutorial-load-balancer.md)
