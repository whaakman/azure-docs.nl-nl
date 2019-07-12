---
title: Over opslagplaatsen en installatiekopieën in Azure Container Registry
description: Inleiding tot de belangrijkste concepten van Azure container Registry, opslagplaatsen en installatiekopieën van containers.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/01/2019
ms.author: danlep
ms.openlocfilehash: a14f0a2a86c5e4922fcddf3c92d48c6dfb1497a3
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800326"
---
# <a name="about-registries-repositories-and-images"></a>Over registers, -opslagplaatsen en installatiekopieën

Dit artikel bevat de belangrijkste concepten van container Registry, opslagplaatsen en installatiekopieën van containers en verwante artefacts. 

## <a name="registry"></a>Register

Een container *register* is een service die wordt opgeslagen en distribueert containerinstallatiekopieën. Docker-Hub is een openbare container registry biedt ondersteuning voor de open source-community en fungeert als een algemene catalogus van installatiekopieën. Azure Container Registry biedt gebruikers met direct beheer van afbeeldingen, met geïntegreerde verificatie [geo-replicatie](container-registry-geo-replication.md) ondersteuning van wereldwijde distributie en betrouwbaarheid voor implementaties dichtbij het netwerk, [ configuratie van de virtuele netwerk- en firewallinstellingen](container-registry-vnet.md), [tag vergrendelen](container-registry-image-lock.md), en nog vele andere functies worden uitgebreid. 

Naast de containerinstallatiekopieën met Docker, Azure Container Registry ondersteunt die betrekking hebben [inhoud artefacten](container-registry-image-formats.md) met inbegrip van Open Container initiatief OCI ()-indeling.

## <a name="content-addressable-elements-of-an-artifact"></a>Inhoud adresseerbare elementen van een artefact

Het adres van een artefact in een Azure container registry bevat de volgende elementen. 

```
[loginUrl]/[namespace]/[artifact:][tag]
```

* **loginUrl** -de volledig gekwalificeerde naam van de Register-host. De Register-host in een Azure container registry heeft de indeling *myregistry*. azurecr.io (zonder hoofdletters). U moet de loginUrl opgeven bij het gebruik van Docker of andere artefacten push of pull-clienthulpprogramma's naar een Azure container registry. 
* **naamruimte** - schuine streep gescheiden logische groepering van gerelateerde afbeeldingen of artefacten - bijvoorbeeld voor een werkgroep of een app
* **artefact** -de naam van een opslagplaats voor een bepaalde installatiekopie of een artefact
* **tag** -een specifieke versie van een afbeelding of de artefacten die zijn opgeslagen in een opslagplaats


Bijvoorbeeld, uitzien de volledige naam van een installatiekopie in een Azure container registry als volgt:

```
myregistry.azurecr.io/marketing/campaign10-18/email-sender:v2
```

Zie de volgende secties voor meer informatie over deze elementen.

## <a name="repository-name"></a>Naam van de opslagplaats

Container Registry beheren *opslagplaatsen*, verzamelingen van installatiekopieën van de container of andere artefacten met dezelfde naam, maar met verschillende codes. De volgende drie installatiekopieën zijn bijvoorbeeld in de opslagplaats 'acr-helloworld':

```
acr-helloworld:latest
acr-helloworld:v1
acr-helloworld:v2
```

Namen van de opslagplaats kunnen ook bevatten [naamruimten](container-registry-best-practices.md#repository-namespaces). Naamruimten kunt u afbeeldingen van de groep met behulp van de namen van forward slash gescheiden opslagplaats, bijvoorbeeld:

```
marketing/campaign10-18/web:v2
marketing/campaign10-18/api:v3
marketing/campaign10-18/email-sender:v2
product-returns/web-submission:20180604
product-returns/legacy-integrator:20180715
```

## <a name="image"></a>Image

Een containerinstallatiekopie of andere artefacten binnen een register is gekoppeld aan een of meer labels, heeft een of meer lagen en wordt geïdentificeerd door een manifest. Informatie over hoe deze onderdelen aan elkaar zijn gerelateerd, kunt u het effectief beheren van uw register.

### <a name="tag"></a>Label

De *tag* voor een afbeelding of andere artefacten geeft de versie ervan. Een enkele artefact binnen een opslagplaats een of meer labels kan worden toegewezen en kan ook worden als "niet-gecodeerde." Dat wil zeggen, kunt u alle tags verwijderen van een installatiekopie van het image-gegevens (de lagen) blijven in het register.

De opslagplaats (of opslagplaats en naamruimte) plus een tag definieert de naam van een installatiekopie. U kunt pushen en ophalen van een installatiekopie van een door de naam op te geven in de bewerking push of pull.

Hoe u containerinstallatiekopieën taggen wordt geleid door uw scenario's voor het ontwikkelen van of deze implementeren. Bijvoorbeeld, worden stabiele tags aanbevolen voor het onderhouden van uw basisinstallatiekopieën en unieke tags voor het implementeren van installatiekopieën. Zie voor meer informatie, [aanbevelingen voor het labelen en versiebeheer containerinstallatiekopieën](container-registry-image-tag-version.md).

### <a name="layer"></a>Laag

Containerinstallatiekopieën bestaan uit een of meer *lagen*, elke overeenkomt met een regel in de Dockerfile die de afbeelding definieert. Afbeeldingen in een register delen algemene lagen, opslagefficiëntie van te vergroten. Bijvoorbeeld, verschillende afbeeldingen in andere opslagplaatsen mogelijk delen de dezelfde Alpine Linux basis-laag, maar slechts één exemplaar van die laag is opgeslagen in het register.

Laag delen optimaliseert tevens de distributie van de laag naar knooppunten met meerdere installatiekopieën van algemene lagen die worden gedeeld. Bijvoorbeeld, als een afbeelding al op een knooppunt de laag Alpine Linux als base bevat, overbrengen niet de volgende pull van een andere afbeelding die verwijzen naar dezelfde laag de laag naar het knooppunt. In plaats daarvan wordt verwezen naar de laag die al op het knooppunt.

Voor beveiligde isolatie en bescherming tegen mogelijke laag manipulatie zijn lagen niet verdeeld over registers.

### <a name="manifest"></a>Manifest

Elke containerinstallatiekopie of artefact naar een container registry gepusht is gekoppeld aan een *manifest*. Het manifest gegenereerd door het register wanneer de installatiekopie wordt gepusht, uniek wordt aangeduid in de afbeelding en Hiermee geeft u de lagen. U kunt een lijst de manifesten voor een opslagplaats met de Azure CLI-opdracht [az acr repository show-manifesten][az-acr-repository-show-manifests]:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Bijvoorbeeld, verwerkingen lijst het manifest voor de opslagplaats 'acr-helloworld':

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

### <a name="manifest-digest"></a>Manifest van de samenvatting

Manifesten worden aangeduid met een unieke SHA-256-hash of *manifest digest*. Elke installatiekopie of artefact--wordt of of niet - gelabelde aangeduid met de samenvatting. De digest-waarde is uniek, zelfs als de laaggegevens identiek zijn aan die van een andere afbeelding. Dit mechanisme is kunt u meerdere keren dezelfde label om installatiekopieën te pushen naar een register. Bijvoorbeeld, u kunt herhaaldelijk pushen `myimage:latest` naar uw register zonder fouten omdat elke afbeelding wordt geïdentificeerd door de unieke verificatiesamenvatting.

U kunt een installatiekopie van een register ophalen door de samenvatting op te geven in de pullbewerking. Sommige systemen kunnen worden geconfigureerd om op te halen door digest omdat dit zorgt ervoor de versie van de installatiekopie worden opgehaald, zelfs als een identieke gelabelde installatiekopie vervolgens naar het register is gepusht.

Afbeelding van de opslagplaats 'acr-helloworld' bijvoorbeeld ophalen door manifest digest:

```console
$ docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108
```

> [!IMPORTANT]
> Als u aangepaste installatiekopieën met identieke tags herhaaldelijk pusht, kunt u zwevende installatiekopieën---installatiekopieën die zijn niet-gecodeerde, maar nog steeds in beslag nemen ruimte in het register kunt maken. Niet-gelabelde afbeeldingen worden niet weergegeven in de Azure CLI of in de Azure portal wanneer u een lijst of afbeeldingen weergeven door de tag. Echter hun lagen nog steeds aanwezig en ruimte in het register gebruiken. Zie voor meer informatie over het vrijmaken van ruimte die wordt gebruikt door niet-gelabelde afbeeldingen [verwijderen van installatiekopieën van containers in Azure Container Registry](container-registry-delete.md).


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [afbeeldingopslag](container-registry-storage.md) en [ondersteunde indelingen voor inhoud](container-registry-image-formats.md) in Azure Container Registry.

<!-- LINKS - Internal -->
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests


