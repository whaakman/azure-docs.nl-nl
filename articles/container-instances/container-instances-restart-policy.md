---
title: Beperkte taken uitvoeren in Azure Containerexemplaren
description: Informatie over het gebruik van Azure Containerexemplaren uit te voeren taken die worden voltooid, zoals in de build-, test- of image-rendering taken worden uitgevoerd.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 11/16/2017
ms.author: marsma
ms.openlocfilehash: a922525970eac9af6657e58daae971912183b369
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="run-a-containerized-task-in-azure-container-instances"></a>Een beperkte taak uitvoeren in Azure Containerexemplaren

Het gemak en de snelheid van de implementatie van containers in Azure Containerexemplaren biedt een aantrekkelijke platform voor het uitvoeren van taken zoals bouwen, testen en de rendering van afbeeldingen eenmaal wordt uitgevoerd in een exemplaar van de container.

U kunt opgeven dat uw containers zijn gestopt wanneer hun processen zijn voltooid met een beleid configureerbare opnieuw opstarten. Omdat containerexemplaren worden gefactureerd door het tweede, bent u in rekening gebracht alleen voor de rekenresources gebruikt terwijl de container voor het uitvoeren van de taak wordt uitgevoerd.

De voorbeelden weergegeven in dit artikel de Azure CLI. Hebt u Azure CLI versie 2.0.21 of hoger [lokaal geïnstalleerd][azure-cli-install], of gebruik de CLI in de [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Beleid voor opnieuw opstarten van container

Wanneer u een container in Azure Container instanties maakt, kunt u een van drie instellingen voor het beleid opnieuw.

| Beleid voor opnieuw opstarten   | Beschrijving |
| ---------------- | :---------- |
| `Always` | Containers in de containergroep worden altijd opnieuw opgestart. Dit is de **standaard** instelling toegepast wanneer er geen beleid voor opnieuw opstarten is opgegeven bij het maken van de container. |
| `Never` | Containers in de containergroep worden nooit opnieuw gestart. De containers maximaal één keer uitgevoerd. |
| `OnFailure` | Containers in de containergroep zijn opnieuw gestart alleen wanneer het proces dat wordt uitgevoerd in de container mislukt (wanneer deze wordt beëindigd met de afsluitcode van een andere waarde dan nul). De containers worden ten minste één keer uitgevoerd. |

## <a name="specify-a-restart-policy"></a>Een beleid voor opnieuw opstarten opgeven

Geeft u een beleid voor opnieuw opstarten, is afhankelijk van hoe u uw containerexemplaren zoals met de Azure CLI, Azure PowerShell-cmdlets of in de Azure-portal maken. Geef in de Azure CLI, de `--restart-policy` parameter bij het aanroepen van [az container maken][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Uitvoeren als voorbeeld voor voltooiing

Als u wilt een beleid voor het opnieuw opstarten in actie zien, maakt u een exemplaar van de container van de [aci-microsoft-wordcount] [ aci-wordcount-image] installatiekopie en geef de `OnFailure` beleid opnieuw opstarten. Deze container voorbeeld wordt uitgevoerd een pythonscript dat standaard de tekst van de Shakespeare analyseert [Hamlet](http://shakespeare.mit.edu/hamlet/full.html), schrijft de 10 meest voorkomende woorden naar STDOUT en vervolgens wordt afgesloten.

Voer de voorbeeld-container met de volgende [az container maken] [ az-container-create] opdracht:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Containerexemplaren begint de container en stopt wanneer de toepassing (of het script in dit geval) wordt afgesloten. Wanneer Azure Containerexemplaren een container met beleid voor opnieuw opstarten is stopt `Never` of `OnFailure`, de status van de container is ingesteld op **beëindigd**. U kunt de status van een container met controleren de [az container weergeven] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Voorbeelduitvoer:

```bash
"Terminated"
```

Zodra de status van de voorbeeld-container toont *beëindigd*, ziet u de uitvoer van de taak door de logboeken van de container weer te geven. Voer de [az container logboeken] [ az-container-logs] opdracht voor het weergeven van het script-uitvoer:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Uitvoer:

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

In dit voorbeeld ziet u de uitvoer die het script naar STDOUT verzonden. Uw beperkte taken, echter mogelijk in plaats daarvan schrijven hun uitvoer naar de permanente opslag voor later gebruik. Bijvoorbeeld naar een [Azure-bestandsshare](container-instances-mounting-azure-files-volume.md).

## <a name="configure-containers-at-runtime"></a>Containers configureren tijdens runtime

Wanneer u een exemplaar van de container maakt, kunt u instellen de **omgevingsvariabelen**en geef een aangepaste **opdrachtregel** moet worden uitgevoerd wanneer de container is gestart. U kunt deze instellingen gebruiken in uw batchtaken voor het voorbereiden van elke container met taak-specifieke configuratie.

## <a name="environment-variables"></a>Omgevingsvariabelen

Omgevingsvariabelen worden ingesteld in de container voor dynamische configuratie van de toepassing of het script uitgevoerd op de container. Dit is vergelijkbaar met de `--env` opdrachtregelargument naar `docker run`.

U kunt bijvoorbeeld het gedrag van het script in de container voorbeeld wijzigen door op te geven van de volgende omgevingsvariabelen bij het maken van het exemplaar van de container:

*NumWords*: het aantal woorden naar STDOUT verzonden.

*MinLength*: het minimum aantal tekens in een woord om te worden geteld. Een hoger getal worden genegeerd veelvoorkomende woorden zoals 'van' en 'het'.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Door op te geven `NumWords=5` en `MinLength=8` voor de container omgevingsvariabelen, de logboeken van de container andere uitvoer moeten worden weergegeven. Zodra de status van de container wordt weergegeven als *beëindigd* (Gebruik `az container show` om de status te controleren), de logboeken om te zien van de nieuwe uitvoer weergegeven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Uitvoer:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Onderdrukking van de opdrachtregel

Geef een opdrachtregel te gebruiken bij het maken van een exemplaar van de container voor het onderdrukken van de opdrachtregel standaard uitbreidbaar in de installatiekopie van de container. Dit is vergelijkbaar met de `--entrypoint` opdrachtregelargument naar `docker run`.

U kunt bijvoorbeeld de voorbeeld-container tekst dan analyseren hebben *Hamlet* door te geven van een andere vanaf de opdrachtregel. Het Python-script uitgevoerd op de container *wordcount.py*, een URL als een argument accepteert en verwerkt die pagina-inhoud in plaats van de standaard.

Om bijvoorbeeld te bepalen van de top-3 vijf letters woorden in *Valentijn en Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Opnieuw zodra de container is *beëindigd*, de uitvoer weergeven door Logboeken van de container weer te geven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Uitvoer:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Volgende stappen

### <a name="persist-task-output"></a>Taakuitvoer behouden

Zie voor meer informatie over het behouden van de uitvoer van de containers die voor uitvoering [koppelen van een Azure-bestandsshare met Azure Containerexemplaren](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
