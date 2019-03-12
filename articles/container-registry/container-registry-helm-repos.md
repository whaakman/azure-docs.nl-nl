---
title: Gebruik Helm opslagplaatsen in Azure Container Registry
description: Informatie over het gebruik van een Helm-opslagplaats met Azure Container Registry voor het opslaan van grafieken voor uw toepassingen
services: container-registry
author: iainfoulds
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: iainfou
ms.openlocfilehash: ba0e1386d67e920f1805d244f9042044bb462ec9
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57551990"
---
# <a name="use-azure-container-registry-as-a-helm-repository-for-your-application-charts"></a>Azure Container Registry gebruiken als een Helm-opslagplaats voor uw toepassing grafieken

Als u wilt snel beheren en implementeren van toepassingen voor Kubernetes, kunt u de [open-source Helm-Pakketbeheer][helm]. Met Helm, de toepassingen die zijn gedefinieerd als *grafieken* die zijn opgeslagen in een Helm-grafiek-opslagplaats. Deze grafieken definieert configuraties en de afhankelijkheden, en kunnen worden samengesteld gedurende de levensduur van toepassingen. Azure Container Registry kan worden gebruikt als de host voor Helm-opslagplaats voor grafieken.

Met Azure Container Registry hebt u een veilig Helm-grafiek-opslagplaats, die kan worden geïntegreerd met het bouwen van pijplijnen of andere Azure-services. Helm-opslagplaats voor grafieken in Azure Container Registry zijn functies van de geo-replicatie om te voorkomen dat uw grafieken dicht bij implementaties en voor de redundantie. U alleen betaalt voor de opslag die wordt gebruikt door de grafieken en zijn beschikbaar voor alle lagen van Azure Container Registry prijs.

In dit artikel leest u hoe u een Helm-grafiek opslagplaats opgeslagen in Azure Container Registry.

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in dit artikel hebt voltooid, moeten de volgende vereisten worden voldaan:

- **Azure Container Registry** -een containerregister maken in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
- **Helm-clientversie 2.11.0 (niet een RC-versie) of hoger** - uitvoeren `helm version` om uw huidige versie te vinden. U moet ook een Helm-server (Tiller) geïnitialiseerd in een Kubernetes-cluster. Indien nodig, kunt u [maken van een cluster Azure Kubernetes Service][aks-quickstart]. Zie voor meer informatie over het installeren en upgraden van Helm [Helm installeren][helm-install].
- **Azure CLI versie 2.0.46 of hoger** - uitvoeren `az --version` de versie te vinden. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="add-a-repository-to-helm-client"></a>Een opslagplaats voor Helm-client toevoegen

Een Helm-opslagplaats is een HTTP-server die Helm-grafieken kan opslaan. Azure Container Registry kunt deze opslag bieden voor Helm-grafieken en beheren van de definitie van de index toevoegen en verwijderen van de grafieken om de opslagplaats.

Als u wilt uw Azure Container Registry toevoegen als een Helm-grafiek-opslagplaats, moet u de Azure CLI gebruiken. Met deze methode wordt de Helm-client bijgewerkt met de URI en de referenties voor de opslagplaats die wordt ondersteund door Azure Container Registry. U hoeft niet te deze informatie opslagplaats handmatig opgeven, zodat de referenties worden niet in de opdrachtgeschiedenis bijvoorbeeld weergegeven.

Indien nodig, meld u aan bij de Azure CLI en volg de aanwijzingen:

```azurecli
az login
```

De standaardinstellingen van Azure CLI configureren met de naam van uw Azure Container Registry met de [az configureren] [ az-configure] opdracht. Vervang in het volgende voorbeeld wordt `<acrName>` met de naam van het register:

```azurecli
az configure --defaults acr=<acrName>
```

Nu uw Azure Container Registry Helm-grafiek opslagplaats toevoegen aan de Helm-client met behulp van de [az acr helm-opslagplaats toevoegen] [ az-acr-helm-repo-add] opdracht. Deze opdracht wordt een verificatie token voor uw Azure container registry die wordt gebruikt door de Helm-client. Het verificatietoken is 1 uur geldig. Vergelijkbaar met `docker login`, kunt u deze opdracht uitvoeren in toekomstige CLI-sessies om uw Helm-client met de Azure Container Registry Helm-grafiek-opslagplaats te verifiëren:

```azurecli
az acr helm repo add
```

## <a name="add-a-chart-to-the-repository"></a>Een grafiek toevoegen aan de opslagplaats

In dit artikel laten we ontvangt u een bestaande Helm-grafiek van de openbare Helm *stabiel* opslagplaats. De *stabiel* opslagplaats is een samengestelde, openbare opslagplaats met algemene toepassing grafieken. Pakket instaan kunnen indienen hun grafieken om de *stabiel* opslagplaats, op dezelfde manier dat Docker Hub een openbaar register voor algemene containerinstallatiekopieën biedt. De grafiek worden gedownload uit het publiek *stabiel* opslagplaats kan vervolgens worden gepusht naar uw persoonlijke Azure Container Registry-opslagplaats. In de meeste gevallen zou u maken en uploaden van uw eigen diagrammen voor het ontwikkelen van toepassingen. Zie voor meer informatie over het bouwen van uw eigen Helm-grafieken [Helm-grafieken ontwikkelen][develop-helm-charts].

Maak eerst een map op *~/acr-helm*, downloadt u de bestaande *stable/wordpress* grafiek:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm fetch stable/wordpress
```

Lijst van de gedownloade grafiek, en noteer de Wordpress-versie die is opgenomen in de bestandsnaam. De `helm fetch stable/wordpress` opdracht heeft een bepaalde versie opgegeven zodat de *nieuwste* versie is opgehaald. Alle Helm-grafieken een uniek versienummer opnemen in de bestandsnaam die u volgt de [SemVer 2] [ semver2] standard. In de volgende voorbeelduitvoer wordt de grafiek Wordpress versie *2.1.10*:

```
$ ls

wordpress-2.1.10.tgz
```

Push nu de grafiek aan uw grafiek Helm-opslagplaats in Azure Container Registry met de Azure CLI [az acr helm push] [ az-acr-helm-push] opdracht. Geef de naam van de Helm-grafiek gedownload in de vorige stap, zoals *wordpress-2.1.10.tgz*:

```azurecli
az acr helm push wordpress-2.1.10.tgz
```

Na een paar seconden rapporteert de Azure CLI dat het diagram is opgeslagen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ az acr helm push wordpress-2.1.10.tgz

{
  "saved": true
}
```

## <a name="list-charts-in-the-repository"></a>Lijst met grafieken in de opslagplaats

De client Helm onderhoudt een lokale cache kopie van de inhoud van de RAS-opslagplaatsen. Wijzigingen naar een externe opslagplaats doorgevoerd niet automatisch in de lijst met beschikbare diagrammen die lokaal bekend is door de Helm-client. Wanneer u zoeken naar grafieken in opslagplaatsen, Helm maakt gebruik van de lokale cache index. Voor het gebruik van de grafiek in de vorige stap hebt geüpload, moet de lokale index van de Helm-opslagplaats worden bijgewerkt. U kunt indexeren van de opslagplaatsen in de Helm-client of de Azure CLI gebruiken voor het bijwerken van de index van de opslagplaats. Telkens wanneer die u een grafiek aan uw opslagplaats toevoegen moet in deze stap worden voltooid:

```azurecli
az acr helm repo add
```

Met een grafiek die lokaal zijn opgeslagen in uw opslagplaats en de bijgewerkte index beschikbaar is, kunt u de gewone opdrachten van de Helm-client te zoeken en installeren. Als de grafieken in uw opslagplaats wilt weergeven, gebruikt `helm search <acrName>`. Geef de naam van uw eigen Azure Container Registry:

```console
helm search <acrName>
```

De Wordpress-grafiek gepusht in de vorige stap wordt weergegeven, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ helm search myacrhelm

NAME                CHART VERSION   APP VERSION DESCRIPTION
helmdocs/wordpress  2.1.10          4.9.8       Web publishing platform for building blogs and websites.
```

U kunt ook de grafieken met de Azure CLI, een lijst met behulp van [az acr ' helm list '][az-acr-helm-list]:

```azurecli
az acr helm list
```

## <a name="show-information-for-a-helm-chart"></a>Informatie weergeven voor een Helm-diagram

Als u informatie voor een specifieke grafiek in de opslagplaats, kunt u de reguliere Helm-client opnieuw gebruiken. Voor informatie voor de grafiek met de naam *wordpress*, gebruikt u `helm inspect`.

```console
helm inspect <acrName>/wordpress
```

Als er geen versienummer is opgegeven, de *nieuwste* versie wordt gebruikt. Helm retourneert gedetailleerde informatie over de grafiek, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ helm inspect myacrhelm/wordpress

appVersion: 4.9.8
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: https://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: bitnami-bot
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 2.1.10
[...]
```

U kunt ook de informatie voor een grafiek met de Azure CLI weergeven [az acr helm show] [ az-acr-helm-show] opdracht. Nogmaals, de *nieuwste* versie van een grafiek is geretourneerd door standaard. U kunt toevoegen aan `--version` om, zoals een specifieke versie van een grafiek weer te geven *2.1.10*:

```azurecli
az acr helm show wordpress
```

## <a name="install-a-helm-chart-from-the-repository"></a>Een Helm-diagram installeren vanuit de opslagplaats

Het Helm-diagram in de opslagplaats wordt geïnstalleerd door de naam van de opslagplaats op te geven en vervolgens de naam van grafiekgebied. Gebruik de Helm-client voor het installeren van de Wordpress-grafiek:

```console
helm install <acrName>/wordpress
```

> [!TIP]
> Als u naar uw Azure Container Registry Helm-grafiek opslagplaats forceren en later in een nieuwe CLI-sessie terugkeren, moet uw lokale Helm-client een bijgewerkte verificatietoken. Voor een nieuwe verificatietoken verkrijgen, gebruikt u de [az acr helm-opslagplaats toevoegen] [ az-acr-helm-repo-add] opdracht.

De volgende stappen zijn voltooid tijdens de installatie:

- De Helm-client zoekt naar de index van de lokale opslagplaats.
- Het bijbehorende schema wordt gedownload vanuit de opslagplaats van de Azure Container Registry.
- De grafiek wordt geïmplementeerd met behulp van de Tiller in uw Kubernetes-cluster.

De uitvoer van de volgende verkorte voorbeeld ziet u de Kubernetes-resources geïmplementeerd via het Helm-diagram:

```
$ helm install myacrhelm/wordpress

NAME:   irreverent-jaguar
LAST DEPLOYED: Thu Sep 13 21:44:20 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                          READY  STATUS   RESTARTS  AGE
irreverent-jaguar-wordpress-7ff46d9b8c-b7v6m  0/1    Pending  0         1s
irreverent-jaguar-mariadb-0                   0/1    Pending  0         1s
[...]
```

## <a name="delete-a-helm-chart-from-the-repository"></a>Een Helm-grafiek verwijderen uit de opslagplaats

Als een grafiek verwijderen uit de opslagplaats, gebruikt u de [az acr helm verwijderen] [ az-acr-helm-delete] opdracht. Geef de naam van de grafiek, zoals *wordpress*, en de versie die u wilt verwijderen, zoals *2.1.10*.

```azurecli
az acr helm delete wordpress --version 2.1.10
```

Als u verwijderen van alle versies van de grafiek met de naam wilt, laat u uit de `--version` parameter.

De grafiek moet worden geretourneerd blijft `helm search <acrName>`. De Helm-client niet opnieuw, automatisch bijgewerkt voor de lijst met beschikbare diagrammen in een opslagplaats. Voor het bijwerken van de index van Helm client repo, gebruikt u de [az acr helm-opslagplaats toevoegen] [ az-acr-helm-repo-add] opdracht opnieuw uit:

```azurecli
az acr helm repo add
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel gebruikt u een bestaande Helm-diagram van het publiek *stabiel* opslagplaats. Zie voor meer informatie over het maken en implementeren van Helm-grafieken [ontwikkelen Helm-grafieken][develop-helm-charts].

Helm-grafieken kunnen worden gebruikt als onderdeel van het bouwproces container. Zie voor meer informatie, [gebruik Azure Container Registry taken][acr-tasks].

Zie voor meer informatie over het gebruik en beheer van Azure Container Registry, de [aanbevolen procedures][acr-bestpractices].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[develop-helm-charts]: https://docs.helm.sh/developing_charts/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
