---
title: Verwijderen van de installatiekopie van resources in Azure Container Registry
description: Informatie over hoe u effectief registergrootte beheren door gegevens van container-installatiekopie te verwijderen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/17/2019
ms.author: danlep
ms.openlocfilehash: c603afa61499a615a0882cef06f14fd3d080a9ef
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797773"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Verwijderen van installatiekopieën van containers in Azure Container Registry

Als u wilt de grootte van uw Azure container registry behouden, moet u periodiek verouderde image-gegevens verwijderen. Terwijl sommige containerinstallatiekopieën in productie is geïmplementeerd, vereist op de langere termijn opslag is mogelijk, kunnen anderen doorgaans sneller worden verwijderd. Bijvoorbeeld in een geautomatiseerde build en Testscenario, kunt het register snel vullen met installatiekopieën die mogelijk nooit worden geïmplementeerd en kunnen worden opgeschoond kort na het voltooien van de compilatie- en configuratiefase.

Omdat u de image-gegevens op verschillende manieren verwijderen kunt, is het belangrijk om te begrijpen hoe elk verwijderbewerking is van invloed op gebruik van opslag. In dit artikel bevat informatie over de verschillende methoden voor het verwijderen van image-gegevens:

* Verwijdert een [opslagplaats](#delete-repository): Hiermee verwijdert u alle installatiekopieën en alle unieke lagen binnen de opslagplaats.
* Verwijderen met [tag](#delete-by-tag): Hiermee verwijdert u een afbeelding, de tag, alle unieke lagen waarnaar wordt verwezen door de installatiekopie en alle andere tags die zijn gekoppeld aan de installatiekopie.
* Verwijderen met [manifest digest](#delete-by-manifest-digest): Hiermee verwijdert u een afbeelding, alle unieke lagen waarnaar wordt verwezen door de installatiekopie en alle tags die zijn gekoppeld aan de installatiekopie.

Voorbeeldscripts voor het automatiseren van delete-bewerkingen.

Zie voor een inleiding tot deze concepten, [over registers, -opslagplaatsen en installatiekopieën](container-registry-concepts.md).

## <a name="delete-repository"></a>Opslagplaats verwijderen

Een opslagplaats verwijderen verwijdert u alle installatiekopieën in de opslagplaats, inclusief alle tags, unieke lagen en manifesten. Wanneer u een opslagplaats hebt verwijderd, herstelt u de opslagruimte die wordt gebruikt door de afbeeldingen die verwijzen naar unieke lagen in die opslagplaats.

De volgende Azure CLI opdracht verwijdert u de opslagplaats 'acr-helloworld' en alle tags en manifesten binnen de opslagplaats. Als lagen waarnaar wordt verwezen door de verwijderde manifesten niet door andere afbeeldingen in het register verwezen wordt, is hun laaggegevens ook verwijderd, de opslagruimte worden hersteld.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Tag verwijderen

U kunt afzonderlijke afbeeldingen uit een opslagplaats verwijderen door de naam van de opslagplaats en het label in de delete-bewerking op te geven. Wanneer u op label verwijdert, herstelt u de opslagruimte die wordt gebruikt door een unieke lagen in de afbeelding (lagen niet worden gedeeld door andere afbeeldingen in het register).

Als u wilt verwijderen op label, gebruikt u [az acr repository delete][az-acr-repository-delete] en geeft u de naam van de installatiekopie in de `--image` parameter. Alle lagen die uniek is voor de installatiekopie en andere tags die zijn gekoppeld aan de installatiekopie worden verwijderd.

Bijvoorbeeld: verwijderen van de ' acr-helloworld:latest ' installatiekopie uit het register 'myregistry':

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Verwijderen van *op label* mag niet worden verward met het verwijderen van een label (label). U kunt een label met de Azure CLI-opdracht verwijderen [az acr repository label verwijderen][az-acr-repository-untag]. Er is geen ruimte vrijgemaakt als u een installatiekopie van een label verwijderen, omdat de [manifest](container-registry-concepts.md#manifest) en laaggegevens blijven in het register. De tag-verwijzing zelf wordt verwijderd.

## <a name="delete-by-manifest-digest"></a>Manifest digest verwijderen

Een [manifest digest](container-registry-concepts.md#manifest-digest) kan worden gekoppeld aan een, geen of meerdere labels. Wanneer u verificatiesamenvatting verwijdert, worden alle tags waarnaar wordt verwezen door het manifest verwijderd, omdat laaggegevens voor de lagen die uniek is voor de installatiekopie. Gedeelde laag gegevens worden niet verwijderd.

Als u wilt verwijderen door digest, verwerkingen eerste lijst het manifest voor de opslagplaats met de afbeeldingen die u wilt verwijderen. Bijvoorbeeld:

```console
$ az acr repository show-manifests --name myregistry --repository acr-helloworld
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  }
]
```

Daarna geeft u de samenvatting die u verwijderen wilt de [az acr repository delete][az-acr-repository-delete] opdracht. De indeling van de opdracht is als volgt:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Bijvoorbeeld, het laatste manifest verwijderen die worden vermeld in de bovenstaande uitvoer (met de tag 'v2'):

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

De `acr-helloworld:v2` installatiekopie wordt verwijderd uit het register, omdat alle laaggegevens uniek is voor die installatiekopie. Als een manifest gekoppeld aan meerdere labels is, worden ook alle gekoppelde tags verwijderd.

## <a name="delete-digests-by-timestamp"></a>Verwijderen van verwerkingen met tijdstempel

Voor het onderhouden van de grootte van een opslagplaats of het register, moet u wellicht periodiek verwijderen manifest verwerkingen die ouder zijn dan een bepaalde datum.

De volgende Azure CLI-opdracht worden alle manifest Digest-schema in een opslagplaats die ouder zijn dan een opgegeven timestamp, in oplopende volgorde. Vervang `<acrName>` en `<repositoryName>` met waarden die geschikt is voor uw omgeving. De tijdstempel wordt mogelijk een volledige datum / tijd-expressie of een datum, zoals in dit voorbeeld.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

Na het verlopen manifest verwerkingen identificeren, kunt u het volgende Bash-script als u wilt verwijderen manifest verwerkingen die ouder zijn dan een opgegeven tijdstempel uitvoeren. Hiervoor de Azure CLI en **xargs**. Standaard voert het script niet verwijderen. Wijzig de `ENABLE_DELETE` waarde die moet worden `true` om in te schakelen van de installatiekopie verwijderen.

> [!WARNING]
> Gebruik het volgende voorbeeldscript voorzichtig--verwijderde image-gegevens is ONHERSTELBAAR. Als u systemen die pull installatiekopieën door manifest digest (in plaats van de installatiekopie met de naam) hebt, moet u deze scripts niet uitvoeren. Verwijderen van het manifest verwerkingen wordt voorkomen dat deze systemen de installatiekopieën binnenhaalt uit het register. In plaats van in het manifest van binnenhalen, Overweeg de overstap naar een *unieke tagging* schema, een [aanbevolen best practices][tagging-best-practices]. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Niet-gelabelde afbeeldingen verwijderen

Zoals vermeld in de [Manifest digest](container-registry-concepts.md#manifest-digest) sectie pushen van een aangepaste installatiekopie met behulp van een bestaande tag **untags** de installatiekopie van het eerder gepushte, wat resulteert in een installatiekopie van een zwevende (of 'dangling'). Van de installatiekopie van het eerder gepushte manifest-- en u de laaggegevens--blijft in het register. Houd rekening met de volgende reeks gebeurtenissen:

1. Installatiekopie pushen *acr-helloworld* met tag **nieuwste**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Controleer de manifesten voor de opslagplaats *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. Wijzigen *acr-helloworld* Dockerfile
1. Installatiekopie pushen *acr-helloworld* met tag **nieuwste**: `docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Controleer de manifesten voor de opslagplaats *acr-helloworld*:

   ```console
   $ az acr repository show-manifests --name myregistry --repository acr-helloworld
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Zoals u in de uitvoer van de laatste stap in de reeks ziet, er wordt nu een zwevende manifest waarvan `"tags"` eigenschap is een lege lijst. Dit manifest wordt nog steeds aanwezig in het register, samen met een unieke laaggegevens waarnaar deze verwijst. **Zwevende verwijderen zoals afbeeldingen en hun laaggegevens, moet u verwijderen door manifest digest**.

## <a name="delete-all-untagged-images"></a>Verwijderen van alle niet-gelabelde afbeeldingen

U kunt alle niet-gelabelde afbeeldingen weergeven in uw opslagplaats met de volgende Azure CLI-opdracht. Vervang `<acrName>` en `<repositoryName>` met waarden die geschikt is voor uw omgeving.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

Met deze opdracht in een script, kunt u alle niet-gelabelde afbeeldingen in een opslagplaats verwijderen.

> [!WARNING]
> Gebruik de volgende voorbeeldscripts voorzichtig--verwijderd image-gegevens is ONHERSTELBAAR. Als u systemen die pull installatiekopieën door manifest digest (in plaats van de installatiekopie met de naam) hebt, moet u deze scripts niet uitvoeren. Verwijderen van niet-gelabelde afbeeldingen om deze systemen van het ophalen van de installatiekopieën uit het register te voorkomen. In plaats van in het manifest van binnenhalen, Overweeg de overstap naar een *unieke tagging* schema, een [aanbevolen best practices][tagging-best-practices].

**Azure CLI in Bash**

Het volgende Bash-script worden alle niet-gelabelde afbeeldingen uit een opslagplaats verwijderd. Hiervoor de Azure CLI en **xargs**. Standaard voert het script niet verwijderen. Wijzig de `ENABLE_DELETE` waarde die moet worden `true` om in te schakelen van de installatiekopie verwijderen.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable image deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY --query "[?tags[0]==null]" -o tsv
fi
```

**Azure CLI in PowerShell**

De volgende PowerShell-script worden alle niet-gelabelde afbeeldingen uit een opslagplaats verwijderd. Het vereist dat PowerShell en de Azure CLI. Standaard voert het script niet verwijderen. Wijzig de `$enableDelete` waarde die moet worden `$TRUE` om in te schakelen van de installatiekopie verwijderen.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted."
    Write-Host "Set `$enableDelete = `$TRUE to enable image deletion."
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null]" -o tsv
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de installatiekopie van opslag in Azure Container Registry [afbeeldingopslag Container in Azure Container Registry](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
