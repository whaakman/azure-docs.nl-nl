---
title: Gebruik beleidsregels met taken in containers in Azure Container Instances opnieuw starten
description: Informatie over het gebruik van Azure Container Instances voor het uitvoeren van taken die worden uitgevoerd tot voltooiing, zoals in de build-, test- of image-renderingtaken.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 06872eefd0d500a22214109ad5055dd236b5a6ac
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60608117"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Taken in containers uitvoeren met beleid voor opnieuw opstarten

Het gemak en de snelheid van de implementatie van containers in Azure Container Instances biedt een aantrekkelijke platform voor het uitvoeren van taken, zoals build-, test- en beeldweergave eenmalig worden uitgevoerd in een containerexemplaar.

Met een beleid opnieuw kunnen worden geconfigureerd, kunt u opgeven dat uw containers worden gestopt wanneer hun processen zijn voltooid. Omdat container instances wordt gefactureerd per seconde, u betaalt alleen voor de compute-resources die worden gebruikt terwijl de container voor het uitvoeren van de taak wordt uitgevoerd.

De voorbeelden die zijn gepresenteerd in dit artikel gebruiken de Azure CLI. Hebt u Azure CLI versie 2.0.21 of hoger [lokaal geïnstalleerd][azure-cli-install], of gebruik de CLI in de [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Beleid voor opnieuw opstarten van container

Bij het maken van een [containergroep](container-instances-container-groups.md) in Azure Container Instances kunt u een van drie instellingen voor beleid voor het opnieuw.

| Beleid voor opnieuw opstarten   | Description |
| ---------------- | :---------- |
| `Always` | Containers in containergroep altijd opnieuw opgestart. Dit is de **standaard** instelling wordt toegepast wanneer er geen beleid voor opnieuw opstarten is opgegeven bij het maken van container. |
| `Never` | Containers in containergroep nooit opnieuw opgestart. De containers maximaal één keer uitgevoerd. |
| `OnFailure` | Containers in containergroep zijn opnieuw gestart wanneer het proces dat wordt uitgevoerd in de container is mislukt (wanneer deze wordt afgesloten met een andere afsluitcode). De containers zijn ten minste één keer uitgevoerd. |

## <a name="specify-a-restart-policy"></a>Een beleid voor opnieuw opstarten opgeven

Geeft u een beleid voor opnieuw opstarten, is afhankelijk van hoe u uw container instances kunt, zoals met de Azure CLI, Azure PowerShell-cmdlets, of in de Azure portal maken. In de Azure CLI, geeft u de `--restart-policy` parameter bij het aanroepen van [az container maken][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Voorbeeld van de voltooiing worden uitgevoerd

Als u wilt het beleid voor opnieuw opstarten in actie zien, maakt u een containerinstantie van de Microsoft [aci-wordcount] [ aci-wordcount-image] installatiekopie en geef de `OnFailure` beleid voor opnieuw opstarten. Deze voorbeeld-container wordt uitgevoerd een Python-script dat standaard de tekst van de Shakespeare analyseert [Hamlet](http://shakespeare.mit.edu/hamlet/full.html), schrijft de 10 meest voorkomende woorden STDOUT en vervolgens wordt afgesloten.

De voorbeeld-container uitvoeren met de volgende [az container maken] [ az-container-create] opdracht:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances de container begint en stopt wanneer de toepassing (of het script, in dit geval) wordt afgesloten. Wanneer een container met beleid voor opnieuw opstarten is niet meer in Azure Container Instances `Never` of `OnFailure`, van de container de status is ingesteld op **beëindigd**. U kunt de status van een container met controleren de [az container show] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Voorbeelduitvoer:

```bash
"Terminated"
```

Zodra de status van de container van het voorbeeld laat zien *beëindigd*, ziet u de uitvoer van de taak door de containerlogboeken van de weer te geven. Voer de [az container logs] [ az-container-logs] de uitvoer van opdracht om het script weer te geven:

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

In dit voorbeeld ziet u de uitvoer die het script naar de STDOUT verzonden. Uw taken in containers echter mogelijk in plaats daarvan de uitvoer schrijven naar de permanente opslag voor later gebruik. Bijvoorbeeld, voor een [Azure-bestandsshare](container-instances-mounting-azure-files-volume.md).

## <a name="next-steps"></a>Volgende stappen

Taakgebaseerde scenario's, zoals een grote gegevensset met meerdere containers voor batchverwerking kunnen profiteren van aangepaste [omgevingsvariabelen](container-instances-environment-variables.md) of [opdrachtregels](container-instances-start-command.md) tijdens runtime.

Zie voor meer informatie over het behouden van de uitvoer van de containers die worden uitgevoerd tot voltooiing [koppelen van een Azure-bestandsshare met Azure Container Instances](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
