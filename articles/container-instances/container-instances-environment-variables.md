---
title: Omgevingsvariabelen worden ingesteld in Azure Containerexemplaren
description: Informatie over het instellen van omgevingsvariabelen in Azure Containerexemplaren
services: container-instances
author: david-stanford
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/13/2018
ms.author: dastanfo
ms.openlocfilehash: 37fde41b6dc2ea0a4d3b4b38a0e3df81a297c125
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

Omgevingsvariabelen instellen in uw containerexemplaren, kunt u dynamische configuratie van de toepassing of het script uitgevoerd op de container bieden.

Kun je je momenteel omgevingsvariabelen instellen vanuit de CLI en PowerShell. In beide gevallen gebruikt u een markering op de opdrachten de omgevingsvariabelen instellen. Omgevingsvariabelen instellen in ACI is vergelijkbaar met de `--env` opdrachtregelargument naar `docker run`. Als u de afbeelding aci-microsoft-wordcount container gebruiken kunt u het gedrag wijzigen door op te geven van de volgende omgevingsvariabelen:

*NumWords*: het aantal woorden naar STDOUT verzonden.

*MinLength*: het minimum aantal tekens in een woord om te worden geteld. Een hoger getal worden genegeerd veelvoorkomende woorden zoals 'van' en 'het'.

## <a name="azure-cli-example"></a>Voorbeeld van Azure CLI

Om te zien in de standaarduitvoer van de container de volgende opdracht uitvoeren:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Door op te geven `NumWords=5` en `MinLength=8` voor de container omgevingsvariabelen, de logboeken van de container andere uitvoer moeten worden weergegeven.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Zodra de status van de container wordt weergegeven als *beëindigd* (Gebruik [az container weergeven] [ az-container-show] naar de status ervan controleren), de logboeken om te zien van de uitvoer weergegeven.  De uitvoer van de container, waarbij geen omgeving variabelen ingesteld--naam mycontainer1 in plaats van mycontainer2 weergeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

## <a name="azure-powershell-example"></a>Azure PowerShell-voorbeeld

Om te zien in de standaarduitvoer van de container de volgende opdracht uitvoeren:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Door op te geven `NumWords=5` en `MinLength=8` voor de container omgevingsvariabelen, de logboeken van de container andere uitvoer moeten worden weergegeven.

```azurepowershell-interactive
$envVars = @{NumWord=5;MinLength=8}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Zodra de status van de container *beëindigd* (Gebruik [Get-AzureRmContainerInstanceLog] [ azure-instance-log] naar de status ervan controleren), de logboeken om te zien van de uitvoer weergegeven. Logboeken met geen omgevingsvariabelen ingesteld om weer te geven van de container ContainerGroupName mycontainer1 in plaats van mycontainer2 worden.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog `
    -ResourceGroupName myResourceGroup `
    -ContainerGroupName mycontainer2
```

## <a name="example-output-without-environment-variables"></a>Voorbeeld van uitvoer zonder omgevingsvariabelen

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

## <a name="example-output-with-environment-variables"></a>Voorbeeld van uitvoer met omgevingsvariabelen

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe weet voor het aanpassen van de invoer voor de container, informatie over het behouden van de uitvoer van de containers die volledig worden uitgevoerd.
> [!div class="nextstepaction"]
> [Koppelen van een Azure-bestandsshare met exemplaren van Azure-Container](container-instances-mounting-azure-files-volume.md)

<!-- LINKS Internal -->
[azure-cloud-shell]: ../cloud-shell/overview.md
[azure-cli-install]: /cli/azure/
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show