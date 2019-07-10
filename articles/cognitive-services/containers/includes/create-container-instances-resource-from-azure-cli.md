---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een Azure container instance-bron van de Azure CLI.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 5e7a3d849f726ae4dbbd559d541464404e427775
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717078"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Maak een Azure Container Instances-resource van de Azure CLI

De volgende YAML definieert de Azure Container Instances-resource. Kopieer en plak de inhoud in een nieuw bestand met de naam `my-aci.yaml` en vervang de opmerkingen waarden door uw eigen. Raadpleeg de [sjabloonindeling] [sjabloon-formant] voor geldige YAML. Raadpleeg de [container opslagplaatsen en installatiekopieën][repositories-and-images] voor de namen van de beschikbare installatiekopieën en hun bijbehorende opslagplaats.

```YAML
apiVersion: 2018-10-01
location: # < Valid location >
name: # < Container Group name >
imageRegistryCredentials:
  - server: containerpreview.azurecr.io
    username: # < The username for the preview container registry >
    password: # < The password for the preview container registry >
properties:
  containers:
  - name: # < Container name >
    properties:
      image: # < Repository/Image name >
      environmentVariables: # These env vars are required
        - name: eula
          value: accept
        - name: billing
          value: # < Service specific Endpoint URL >
        - name: apikey
          value: # < Service specific API key >
      resources:
        requests:
          cpu: 4 # Always refer to recommended minimal resources
          memoryInGb: 8 # Always refer to recommended minimal resources
      ports:
        - port: 5000
  osType: Linux
  restartPolicy: OnFailure
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 5000
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

> [!NOTE]
> Niet alle locaties hebben dezelfde CPU en geheugen beschikbaarheidsset. Raadpleeg de [locatie en resources][location-to-resource] tabel voor de lijst met beschikbare resources voor containers per locatie en het besturingssysteem.

We zult zijn afhankelijk van het YAML-bestand wordt gemaakt voor de [ `az container create` ][azure-container-create] opdracht. Uitvoeren van de Azure CLI, de `az container create` opdracht vervangt de `<resource-group>` door uw eigen. Voor het beveiligen van waarden in een YAML implementatie Raadpleeg daarnaast [waarden secure][secure-values].

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

De uitvoer van de opdracht is `Running...` als geldig is, na enige tijd de uitvoer verandert in een JSON-tekenreeks voor de zojuist gemaakte ACI-resource. De containerinstallatiekopie is meer dan waarschijnlijk niet meer beschikbaar voor een tijdje, maar de resource is nu geïmplementeerd.

> [!TIP]
> Let vooral op de locaties van de openbare preview van Azure Cognitive Service-aanbiedingen, zoals de YAML moest worden aangepast zodat deze overeenkomt met de locatie.

[azure-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[template-format]: https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format

[repositories-and-images]: ../../cognitive-services-container-support.md#container-repositories-and-images
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md#availability---general
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values