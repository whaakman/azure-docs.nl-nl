---
title: Een starten vanaf de opdrachtregel gebruiken in Azure Container Instances
description: Het ingangspunt geconfigureerd in een containerinstallatiekopie, wanneer u een Azure container-exemplaar implementeert overschrijven
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: da94a4c79694f511d41e5c8dda8c786fc7049726
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64569647"
---
# <a name="set-the-command-line-in-a-container-instance-to-override-the-default-command-line-operation"></a>Vanaf de opdrachtregel in een containerexemplaar voor de onderdrukking van de bewerking van de opdrachtregel standaard instellen

Wanneer u een containerexemplaar maakt, Geef eventueel een opdracht voor de onderdrukking van de standaard vanaf de opdrachtregel-instructie in de containerinstallatiekopie sparen. Dit gedrag is vergelijkbaar met de `--entrypoint` opdrachtregelargument naar `docker run`.

Als de instelling [omgevingsvariabelen](container-instances-environment-variables.md) voor container instances, op te geven een starten vanaf de opdrachtregel is handig voor batch-taken waar u nodig hebt om voor te bereiden van elke container dynamisch met taak-specifieke configuratie.

## <a name="command-line-guidelines"></a>Richtlijnen voor vanaf de opdrachtregel

* Standaard vanaf de opdrachtregel Hiermee geeft u een *eenmalige proces dat wordt gestart zonder een shell* in de container. De opdrachtregel kan bijvoorbeeld een Python-script of uitvoerbaar bestand uitgevoerd. 

* Voor het uitvoeren van meerdere opdrachten, beginnen met de opdrachtregel door in te stellen van een shell-omgeving die wordt ondersteund in het besturingssysteem van de container. Voorbeelden:

  |Besturingssysteem  |Standaardshell  |
  |---------|---------|
  |Ubuntu     |   `/bin/bash`      |
  |Alpine     |   `/bin/sh`      |
  |Windows     |    `cmd`     |

  Ga als volgt de regels van het combineren van meerdere opdrachten om uit te voeren in de volgorde van de shell.

* Afhankelijk van de containerconfiguratie van de moet u mogelijk een volledige pad naar het uitvoerbare bestand vanaf de opdrachtregel of argumenten in te stellen.

* Stel een geschikte [beleid voor opnieuw opstarten](container-instances-restart-policy.md) voor het containerexemplaar, afhankelijk van of de opdrachtregel Hiermee geeft u een langlopende taak of een taak eenmaal wordt uitgevoerd. Bijvoorbeeld een beleid voor opnieuw opstarten van `Never` of `OnFailure` wordt aanbevolen voor een taak eenmaal wordt uitgevoerd. 

* Als u informatie over de standaard-entrypoint instellen in een containerinstallatiekopie nodig hebt, gebruikt u de [docker-installatiekopie inspecteren](https://docs.docker.com/engine/reference/commandline/image_inspect/) opdracht.

## <a name="command-line-syntax"></a>Opdrachtregelsyntaxis

De syntaxis van de opdrachtregel, is afhankelijk van de Azure-API of hulpprogramma dat wordt gebruikt om de instanties te maken. Als u een shell-omgeving opgeeft, merkt u ook de conventies van de syntaxis van de opdracht van de shell.

* [AZ container maken] [ az-container-create] opdracht: Een tekenreeks met de `--command-line` parameter. Voorbeeld: `--command-line "python myscript.py arg1 arg2"`).

* [Nieuwe-AzureRmContainerGroup] [ new-azurermcontainergroup] Azure PowerShell-cmdlet: Een tekenreeks met de `-Command` parameter. Voorbeeld: `-Command "echo hello"`.

* Azure Portal: In de **opdracht onderdrukking** eigenschap van de containerconfiguratie van de, een door komma's gescheiden lijst met tekenreeksen, zonder aanhalingstekens bevatten. Voorbeeld: `python, myscript.py, arg1, arg2`). 

* Resource Manager-sjabloon of YAML-bestand of een van de Azure SDK's: Geef de opdrachtregel-eigenschap als een matrix met tekenreeksen. Voorbeeld: de JSON-matrix `["python", "myscript.py", "arg1", "arg2"]` in Resource Manager-sjabloon. 

  Als u bekend met bent [Dockerfile](https://docs.docker.com/engine/reference/builder/) syntaxis en voorbeelden, deze indeling is vergelijkbaar met de *exec* vorm van de CMD-instructie.

### <a name="examples"></a>Voorbeelden

|    |  Azure-CLI   | Portal | Template | 
| ---- | ---- | --- | --- |
| Één opdracht | `--command-line "python myscript.py arg1 arg2"` | **Overschrijven opdracht**: `python, myscript.py, arg1, arg2` | `"command": ["python", "myscript.py", "arg1", "arg2"]` |
| Meerdere opdrachten | `--command-line "/bin/bash -c 'mkdir test; touch test/myfile; tail -f /dev/null'"` |**Overschrijven opdracht**: `/bin/bash, -c, mkdir test; touch test/myfile; tail -f /dev/null` | `"command": ["/bin/bash", "-c", "mkdir test; touch test/myfile; tail -f /dev/null"]` |

## <a name="azure-cli-example"></a>Azure CLI-voorbeeld

Bijvoorbeeld: het gedrag van wijzigen de [aci-microsoft-wordcount] [ aci-wordcount] containerinstallatiekopie, die tekst in de Shakespeare analyseert *Hamlet* het meest vinden woorden optreden. In plaats van het analyseren van *Hamlet*, kunt u een opdrachtregel weergegeven die naar een andere tekstbron verwijst instellen.

Om te zien van de uitvoer van de [aci-microsoft-wordcount] [ aci-wordcount] container als deze de standaardtekst uitvoeren met de volgende analyseert [az container maken] [ az-container-create] opdracht. Er is geen start vanaf de opdrachtregel is opgegeven, zodat de standaard-container opdracht wordt uitgevoerd. Ter illustratie wordt in dit voorbeeld wordt [omgevingsvariabelen](container-instances-environment-variables.md) te vinden van de top 3 woorden die ten minste vijf letters lang:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --environment-variables NumWords=3 MinLength=5 \
    --restart-policy OnFailure
```

Zodra de status van de container wordt weergegeven als *beëindigd* (Gebruik [az container show] [ az-container-show] status controleren), weergegeven van het logboek met [az container logs] [ az-container-logs] om de uitvoer te bekijken.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
```

Uitvoer:

```console
[('HAMLET', 386), ('HORATIO', 127), ('CLAUDIUS', 120)]
```

Nu instellen van een tweede voorbeeld van de container voor het analyseren van andere tekst door een andere vanaf de opdrachtregel op te geven. Het Python-script uitgevoerd op de container *wordcount.py*, een URL als een argument accepteert en processen van die pagina-inhoud in plaats van de standaard.

Bijvoorbeeld, om te bepalen van de top 3 woorden die ten minste vijf letters lang in *Valentijn en Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Nogmaals, als de container *beëindigd*, de uitvoer weergeven door Logboeken van de container weer te geven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Uitvoer:

```console
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Volgende stappen

Taakgebaseerde scenario's, zoals een grote gegevensset met meerdere containers voor batchverwerking kunnen profiteren van aangepaste opdrachtregels tijdens runtime. Zie voor meer informatie over het uitvoeren van containers op basis van een taak [taken in containers uitvoeren met beleid voor opnieuw opstarten](container-instances-restart-policy.md).

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
