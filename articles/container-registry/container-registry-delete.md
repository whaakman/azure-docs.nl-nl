---
title: Verwijderen van de installatiekopie van resources in Azure Container Registry
description: Informatie over hoe u effectief registergrootte beheren door gegevens van container-installatiekopie te verwijderen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/27/2018
ms.author: danlep
ms.openlocfilehash: a1644f68465cffa8cce27257bb91100c111af8a1
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857768"
---
# <a name="delete-container-images-in-azure-container-registry"></a>Verwijderen van installatiekopieën van containers in Azure Container Registry

Als u wilt de grootte van uw Azure container registry behouden, moet u periodiek verouderde image-gegevens verwijderen. Terwijl sommige containerinstallatiekopieën in productie is geïmplementeerd, vereist op de langere termijn opslag is mogelijk, kunnen anderen doorgaans sneller worden verwijderd. Bijvoorbeeld in een geautomatiseerde build en Testscenario, kunt het register snel vullen met installatiekopieën die mogelijk nooit worden geïmplementeerd en kunnen worden opgeschoond kort na het voltooien van de compilatie- en configuratiefase.

Omdat u de image-gegevens op verschillende manieren verwijderen kunt, is het belangrijk om te begrijpen hoe elk verwijderbewerking is van invloed op gebruik van opslag. In dit artikel eerst introduceert de onderdelen van een Docker-register en container-installatiekopieën en bevat informatie over de verschillende methoden voor het verwijderen van image-gegevens.

## <a name="registry"></a>Register

Een container *register* is een service die wordt opgeslagen en distribueert containerinstallatiekopieën. Docker Hub is een openbare Docker-containerregister, terwijl Azure Container Registry privé-Docker-containerregisters in Azure biedt.

## <a name="repository"></a>Opslagplaats

Container Registry beheren *opslagplaatsen*, verzamelingen van installatiekopieën van containers met dezelfde naam, maar met verschillende codes. De volgende drie installatiekopieën zijn bijvoorbeeld in de opslagplaats 'acr-helloworld':

```
acr-helloworld:latest
acr-helloworld:v1
acr-helloworld:v2
```

Namen van de opslagplaats kunnen ook bevatten [naamruimten](container-registry-best-practices.md#repository-namespaces). Naamruimten kunt u afbeeldingen met behulp van de namen van forward slash gescheiden opslagplaats, bijvoorbeeld groep:

```
marketing/campaign10-18/web:v2
marketing/campaign10-18/api:v3
marketing/campaign10-18/email-sender:v2
product-returns/web-submission:20180604
product-returns/legacy-integrator:20180715
```

## <a name="components-of-an-image"></a>Onderdelen van een installatiekopie

Een containerinstallatiekopie binnen een register is gekoppeld aan een of meer labels, heeft een of meer lagen en wordt geïdentificeerd door een manifest. Informatie over hoe deze onderdelen aan elkaar zijn gerelateerd, kunt u bepalen wat de beste methode voor het vrijmaken van ruimte in het register.

### <a name="tag"></a>Label

Van een installatiekopie *tag* Hiermee geeft u de versie ervan. Een één installatiekopie binnen een opslagplaats een of meer labels kan worden toegewezen en kan ook worden als "niet-gecodeerde." Dat wil zeggen, kunt u alle tags verwijderen van een installatiekopie van het image-gegevens (de lagen) blijven het register.

De opslagplaats (of opslagplaats en naamruimte) plus een tag definieert de naam van een installatiekopie. U kunt pushen en ophalen van een installatiekopie van een door de naam op te geven in de bewerking push of pull.

In een privéregister zoals Azure Container Registry bevat naam van de installatiekopie ook de volledig gekwalificeerde naam van de Register-host. De host register voor installatiekopieën in de ACR is in de indeling *acrname.azurecr.io*. Bijvoorbeeld, zou de volledige naam van de eerste afbeelding in de naamruimte 'marketing' in de vorige sectie:

```
myregistry.azurecr.io/marketing/campaign10-18/web:v2
```

Zie voor een discussie over de installatiekopie taggen van aanbevolen procedures, de [Docker tags: Best practices voor docker-installatiekopieën taggen en versiebeheer] [ tagging-best-practices] blogbericht op MSDN.

### <a name="layer"></a>Laag

Afbeeldingen zijn opgebouwd uit een of meer *lagen*, elke overeenkomt met een regel in de Dockerfile die de afbeelding definieert. Afbeeldingen in een register delen algemene lagen, opslagefficiëntie van te vergroten. Bijvoorbeeld, verschillende afbeeldingen in andere opslagplaatsen mogelijk delen de dezelfde Alpine Linux basis-laag, maar slechts één exemplaar van die laag is opgeslagen in het register.

Laag delen optimaliseert tevens de distributie van de laag naar knooppunten met meerdere installatiekopieën van algemene lagen die worden gedeeld. Bijvoorbeeld, als een afbeelding al op een knooppunt de laag Alpine Linux als base bevat, overbrengen niet de volgende pull van een andere afbeelding die verwijzen naar dezelfde laag de laag naar het knooppunt. In plaats daarvan wordt verwezen naar de laag die al op het knooppunt.

### <a name="manifest"></a>Manifest

Elke containerinstallatiekopie gepusht naar een containerregister is gekoppeld aan een *manifest*. Het manifest gegenereerd door het register wanneer de installatiekopie wordt gepusht, uniek wordt aangeduid in de afbeelding en Hiermee geeft u de lagen. U kunt een lijst de manifesten voor een opslagplaats met de Azure CLI-opdracht [az acr repository show-manifesten][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Bijvoorbeeld: het manifest aanbieding verwerkingen voor de opslagplaats 'acr-helloworld':

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
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

Het manifest hier besproken wijkt af van de installatiekopie van het manifest u in Azure portal of met vindt [manifest docker inspecteren][docker-manifest-inspect]. In de volgende sectie, "manifest digest" verwijst naar de samenvatting die worden gegenereerd door de push-bewerking, niet de *config.digest* in het manifest van de installatiekopie. U kunt ophalen en verwijderen van installatiekopieën door **manifest digest**, niet config.digest. De volgende afbeelding ziet u de twee soorten verwerkingen.

![Manifest van de samenvatting en config.digest in Azure portal][manifest-digest]

### <a name="manifest-digest"></a>Manifest van de samenvatting

Manifesten worden aangeduid met een unieke SHA-256-hash of *manifest digest*. Elke afbeelding--wordt of of niet - gelabelde aangeduid met de samenvatting. De digest-waarde is uniek, zelfs als de laaggegevens identiek zijn aan die van een andere afbeelding. Dit mechanisme is kunt u meerdere keren dezelfde label om installatiekopieën te pushen naar een register. Bijvoorbeeld, u kunt herhaaldelijk pushen `myimage:latest` naar uw register zonder fouten omdat elke afbeelding wordt geïdentificeerd door de unieke verificatiesamenvatting.

U kunt een installatiekopie van een register ophalen door de samenvatting op te geven in de pullbewerking. Sommige systemen kunnen worden geconfigureerd om op te halen door digest omdat dit zorgt ervoor de versie van de installatiekopie worden opgehaald, zelfs als een identieke gelabelde installatiekopie vervolgens naar het register is gepusht.

Bijvoorbeeld, binnenhalen van een installatiekopie uit de opslagplaats 'acr-helloworld' door manifest digest:

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Als u aangepaste installatiekopieën met identieke tags herhaaldelijk pusht, kunt u zwevende installatiekopieën---installatiekopieën die zijn niet-gecodeerde, maar nog steeds in beslag nemen ruimte in het register kunt maken. Niet-gelabelde afbeeldingen worden niet weergegeven in de Azure CLI of in de Azure portal wanneer u een lijst of afbeeldingen weergeven door de tag. Echter hun lagen nog steeds aanwezig en ruimte in het register gebruiken. De [verwijderen van niet-gelabelde afbeeldingen](#delete-untagged-images) sectie van dit artikel wordt beschreven die worden gebruikt door niet-gelabelde afbeeldingen vrijmaken van ruimte.

## <a name="delete-image-data"></a>Image-gegevens verwijderen

U kunt image-gegevens op uw containerregister op verschillende manieren verwijderen:

* Verwijdert een [opslagplaats](#delete-repository): Hiermee verwijdert u alle installatiekopieën en alle unieke lagen binnen de opslagplaats.
* Verwijderen met [tag](#delete-by-tag): Hiermee verwijdert u een afbeelding, de tag, alle unieke lagen waarnaar wordt verwezen door de installatiekopie en alle andere tags die zijn gekoppeld aan de installatiekopie.
* Verwijderen met [manifest digest](#delete-by-manifest-digest): Hiermee verwijdert u een afbeelding, alle unieke lagen waarnaar wordt verwezen door de installatiekopie en alle tags die zijn gekoppeld aan de installatiekopie.

## <a name="delete-repository"></a>Opslagplaats verwijderen

Een opslagplaats verwijderen verwijdert u alle installatiekopieën in de opslagplaats, inclusief alle tags, unieke lagen en manifesten. Wanneer u een opslagplaats hebt verwijderd, herstelt u de opslagruimte die wordt gebruikt door de afbeeldingen die in deze opslagplaats.

De volgende Azure CLI opdracht verwijdert u de opslagplaats 'acr-helloworld' en alle tags en manifesten binnen de opslagplaats. Als lagen waarnaar wordt verwezen door de verwijderde manifesten niet door andere afbeeldingen in het register verwezen wordt, wordt hun laaggegevens ook verwijderd.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Tag verwijderen

U kunt afzonderlijke afbeeldingen uit een opslagplaats verwijderen door de naam van de opslagplaats en het label in de delete-bewerking op te geven. Wanneer u op label verwijdert, herstelt u de opslagruimte die wordt gebruikt door een unieke lagen in de afbeelding (lagen niet worden gedeeld door andere afbeeldingen in het register).

Als u wilt verwijderen op label, gebruikt u [az acr repository delete] [ az-acr-repository-delete] en geeft u de naam van de installatiekopie in de `--image` parameter. Alle lagen die uniek is voor de installatiekopie en andere tags die zijn gekoppeld aan de installatiekopie worden verwijderd.

Bijvoorbeeld: verwijderen van de ' acr-helloworld:latest ' installatiekopie uit het register 'myregistry':

```azurecli
$ az acr repository delete --name myregistry --image acr-helloworld:latest
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n): y
```

> [!TIP]
> Verwijderen van *op label* mag niet worden verward met het verwijderen van een label (label). U kunt een label met de Azure CLI-opdracht verwijderen [az acr repository label verwijderen][az-acr-repository-untag]. Er is geen ruimte vrijgemaakt als u een installatiekopie van een label verwijderen, omdat de [manifest](#manifest) en gegevens blijven in het register laag. De tag-verwijzing zelf wordt verwijderd.

## <a name="delete-by-manifest-digest"></a>Manifest digest verwijderen

Een [manifest digest](#manifest-digest) kan worden gekoppeld aan een, geen of meerdere labels. Wanneer u verificatiesamenvatting verwijdert, worden alle tags waarnaar wordt verwezen door het manifest verwijderd, omdat laaggegevens voor de lagen die uniek is voor de installatiekopie. Gedeelde laag gegevens worden niet verwijderd.

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

Daarna geeft u de samenvatting die u verwijderen wilt de [az acr repository delete] [ az-acr-repository-delete] opdracht. De indeling van de opdracht is als volgt:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Bijvoorbeeld, het laatste manifest verwijderen die worden vermeld in de bovenstaande uitvoer (met de tag 'v2'):

```console
$ az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): y
```

De ' acr-helloworld:v2 ' installatiekopie wordt verwijderd uit het register, omdat alle laaggegevens uniek is voor die installatiekopie. Als een manifest gekoppeld aan meerdere labels is, worden ook alle gekoppelde tags verwijderd.

## <a name="delete-untagged-images"></a>Niet-gelabelde afbeeldingen verwijderen

Zoals vermeld in de [Manifest digest](#manifest-digest) sectie pushen van een aangepaste installatiekopie met behulp van een bestaande tag **untags** de installatiekopie van het eerder gepushte, wat resulteert in een installatiekopie van een zwevende (of 'dangling'). Van de installatiekopie van het eerder gepushte manifest-- en u de laaggegevens--blijft in het register. Houd rekening met de volgende reeks gebeurtenissen:

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
       "tags": null,
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Zoals u in de uitvoer van de laatste stap in de reeks ziet, er wordt nu een zwevende manifest waarvan `"tags"` eigenschap `null`. Dit manifest wordt nog steeds aanwezig in het register, samen met een unieke laaggegevens waarnaar deze verwijst. **Zwevende verwijderen zoals afbeeldingen en hun laaggegevens, moet u verwijderen door manifest digest**.

### <a name="list-untagged-images"></a>Lijst met niet-gelabelde afbeeldingen

U kunt alle niet-gelabelde afbeeldingen weergeven in uw opslagplaats met de volgende Azure CLI-opdracht. Vervang `<acrName>` en `<repositoryName>` met waarden die geschikt is voor uw omgeving.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>  --query "[?tags==null].digest"
```

### <a name="delete-all-untagged-images"></a>Verwijderen van alle niet-gelabelde afbeeldingen

Gebruik de volgende voorbeeldscripts voorzichtig--verwijderd image-gegevens is ONHERSTELBAAR.

**Azure CLI in Bash**

Het volgende Bash-script worden alle niet-gelabelde afbeeldingen uit een opslagplaats verwijderd. Hiervoor de Azure CLI en **xargs**. Standaard voert het script niet verwijderen. Wijzig de `ENABLE_DELETE` waarde die moet worden `true` om in te schakelen van de installatiekopie verwijderen.

> [!WARNING]
> Als u systemen die pull installatiekopieën door manifest digest (in plaats van de installatiekopie met de naam) hebt, moet u dit script niet uitvoeren. Verwijderen van niet-gelabelde afbeeldingen om deze systemen van het ophalen van de installatiekopieën uit het register te voorkomen. In plaats van in het manifest van binnenhalen, Overweeg de overstap naar een *unieke tagging* schema, een [aanbevolen best practices][tagging-best-practices].

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
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted. Set ENABLE_DELETE=true to enable image deletion."
fi
```

**Azure CLI in PowerShell**

De volgende PowerShell-script worden alle niet-gelabelde afbeeldingen uit een opslagplaats verwijderd. Het vereist dat PowerShell en de Azure CLI. Standaard voert het script niet verwijderen. Wijzig de `$enableDelete` waarde die moet worden `$TRUE` om in te schakelen van de installatiekopie verwijderen.

> [!WARNING]
> Als u systemen die pull installatiekopieën door manifest digest (in plaats van de installatiekopie met de naam) hebt, moet u dit script niet uitvoeren. Verwijderen van niet-gelabelde afbeeldingen om deze systemen van het ophalen van de installatiekopieën uit het register te voorkomen. In plaats van in het manifest van binnenhalen, Overweeg de overstap naar een *unieke tagging* schema, een [aanbevolen best practices][tagging-best-practices].

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
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted. Set `$enableDelete = `$TRUE to enable image deletion."
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de installatiekopie van opslag in Azure Container Registry [afbeeldingopslag Container in Azure Container Registry](container-registry-storage.md).

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com
[tagging-best-practices]: https://blogs.msdn.microsoft.com/stevelasker/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
