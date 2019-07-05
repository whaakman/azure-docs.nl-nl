---
title: Controleer de status van het register in Azure Container Registry
description: Meer informatie over het uitvoeren van een snelle diagnostische opdracht voor het identificeren van veelvoorkomende problemen bij het gebruik van een Azure container registry, met inbegrip van lokale Docker-configuratie en connectiviteit met het register
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: danlep
ms.openlocfilehash: 3e5b5467f9fa25e23f6661c6630d346aa85e2205
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555096"
---
# <a name="check-the-health-of-an-azure-container-registry"></a>Controleer de status van een Azure container registry

Wanneer u een Azure container registry gebruikt, kunt u soms problemen ondervinden. Bijvoorbeeld, u niet mogelijk om op te halen van een containerinstallatiekopie vanwege een probleem met Docker in uw lokale omgeving. Of een netwerkprobleem mogelijk te voorkomen dat u verbinding maken met het register. 

Uitvoeren als een eerste stap van de diagnostische de [az acr-status][az-acr-check-health] command to get information about the health of the environment and optionally access to a target registry. This command is available in Azure CLI version 2.0.67 or later. If you need to install or upgrade, see [Install Azure CLI][azure-cli].

## <a name="run-az-acr-check-health"></a>Voer az acr-status

Deze voorbeelden tonen verschillende manieren om uit te voeren de `az acr check-health` opdracht.

> [!NOTE]
> Als u de opdracht in Azure Cloud Shell uitvoert, is de lokale omgeving niet ingeschakeld. U kunt echter de toegang tot een doelregister controleren.

### <a name="check-the-environment-only"></a>Controleren of alleen de omgeving

Voer de opdracht zonder parameters om te controleren of de lokale Docker-daemon, CLI-versie en configuratie van de Helm-client:

```azurecli
az acr check-health
```

### <a name="check-the-environment-and-a-target-registry"></a>Controleer de omgeving en een doelregister

Om te controleren op toegang tot een register, evenals lokale omgeving controles uitvoeren, geeft u de naam van een doelregister. Bijvoorbeeld:

```azurecli
az acr check-health --name myregistry
```

## <a name="error-reporting"></a>Foutrapportage

De opdracht registreert informatie in de standaarduitvoer. Als er een probleem is gedetecteerd, bevat deze een foutcode en beschrijving. Zie voor meer informatie over de codes en mogelijke oplossingen, de [foutreferentie](container-registry-health-error-reference.md).

Standaard wordt de opdracht stopt wanneer er een fout. U kunt ook de opdracht uitvoeren zodat deze uitvoer voor alle statuscontroles biedt, zelfs als er fouten worden gevonden. Voeg de `--ignore-errors` parameter, zoals wordt weergegeven in de volgende voorbeelden:

```azurecli
# Check environment only
az acr check-health --ignore-errors

# Check environment and target registry
az acr check-health --name myregistry --ignore-errors
```      

Voorbeelduitvoer:

```console
$ az acr check-health --name myregistry --ignore-errors --yes

Docker daemon status: available
Docker version: Docker version 18.09.2, build 6247962
Docker pull of 'mcr.microsoft.com/mcr/hello-world:latest' : OK
ACR CLI version: 2.2.9
Helm version:
Client: &version.Version{SemVer:"v2.14.1", GitCommit:"5270352a09c7e8b6e8c9593002a73535276507c0", GitTreeState:"clean"}
DNS lookup to myregistry.azurecr.io at IP 40.xxx.xxx.162 : OK
Challenge endpoint https://myregistry.azurecr.io/v2/ : OK
Fetch refresh token for registry 'myregistry.azurecr.io' : OK
Fetch access token for registry 'myregistry.azurecr.io' : OK
```  



## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over foutcodes die zijn geretourneerd door de [az acr-status][az-acr-check-health] opdracht, Zie de [Health check foutreferentie](container-registry-health-error-reference.md).

Zie de [Veelgestelde vragen over](container-registry-faq.md) voor antwoorden op veelgestelde vragen en andere bekende problemen met Azure Container Registry.





<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-check-health]: /cli/azure/acr#az-acr-check-health
