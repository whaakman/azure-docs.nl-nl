---
title: Uitgaand verkeer in Azure Kubernetes Service (AKS) beperken tot
description: Informatie over welke poorten en -adressen vereist voor uitgaand verkeer voor beheer in Azure Kubernetes Service (AKS zijn)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 13fbb20cde454a0aaab156a74a9fbcbac2d90d07
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418127"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Preview - limiet voor uitgaande verkeer voor de clusterknooppunten en toegang tot de vereiste poorten en services in Azure Kubernetes Service (AKS) beheren

Standaard onbeperkte AKS clusters toegang tot het internet van de uitgaande (egress). Dit niveau van toegang tot het netwerk kan knooppunten en diensten die u voor toegang tot externe resources naar behoefte. Als u beperken het uitgaande verkeer wilt, is een beperkt aantal poorten en -adressen moet toegankelijk zijn voor het onderhouden van onderhoudstaken in orde cluster. Uw cluster wordt dan geconfigureerd voor gebruik alleen basissysteem containerinstallatiekopieën uit Microsoft Container Registry (MCR) of Azure Container Registry (ACR), niet-externe openbare opslagplaatsen.

Dit artikel wordt uitgelegd welke netwerkpoorten en de volledig gekwalificeerde domeinnamen (FQDN's) zijn vereiste en optionele als u uitgaande verkeer in een AKS-cluster beperken.  Deze functie is momenteel beschikbaar als preview-product.

> [!IMPORTANT]
> AKS-preview-functies zijn selfservice, aanmelden. Ze zijn bedoeld om het verzamelen van fouten en feedback van onze community. Preview-versie, worden deze functies zijn niet bedoeld voor gebruik in productieomgevingen. Functies in public preview vallen onder 'best effort'-ondersteuning. Hulp van de AKS-teams voor technische ondersteuning is beschikbaar tijdens kantooruren Pacific tijdzone (PST) alleen. Zie de volgende artikelen ondersteuning voor aanvullende informatie:
>
> * [Ondersteuningsbeleid voor AKS][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning van Azure][aks-faq]

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI versie 2.0.61 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][install-azure-cli].

Voor het maken van een AKS-cluster die uitgaande verkeer kan beperken, moet u eerst een functievlag voor uw abonnement inschakelen. De functieregistratie van deze configureert u een AKS-clusters die u voor het gebruik van de containerinstallatiekopieën basissysteem MCR of ACR maken. Om u te registreren de *AKSLockingDownEgressPreview* vlag functie, gebruikt u de [az functie registreren] [ az-feature-register] opdracht zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Het duurt enkele minuten duren voordat de status om weer te geven *geregistreerde*. U kunt de registratiestatus controleren met behulp van de [az Functielijst] [ az-feature-list] opdracht:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Wanneer u klaar bent, vernieuwt u de registratie van de *Microsoft.ContainerService* resourceprovider met behulp van de [az provider register] [ az-provider-register] opdracht:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Overzicht van uitgaand verkeer

Voor het beheer en operationele doeleinden nodig knooppunten in een AKS-cluster hebt voor toegang tot bepaalde poorten en de volledig gekwalificeerde domeinnamen (FQDN's). Deze acties mogelijk om te communiceren met de API-server, of te downloaden en installeer vervolgens kernonderdelen van het Kubernetes-cluster knooppuntupdates en beveiligingsupdates. Internetverkeer van uitgaande gegevens (uitgaand) is standaard niet beperkt tot knooppunten in een AKS-cluster. Het cluster kan basissysteem containerinstallatiekopieën ophalen uit externe opslagplaatsen.

U kunt desgewenst uitgaande verkeer beperken zodat de beveiliging van uw AKS-cluster. Het cluster is geconfigureerd om op te halen basissysteem containerinstallatiekopieën uit MCR of ACR. Als u het uitgaande verkeer op deze manier te vergrendelen, moet u bepaalde poorten en FQDN's waarmee de AKS-knooppunten correct communiceren met de vereiste externe services definiëren. Deze gemachtigde poorten en FQDN's, kunnen uw AKS-knooppunten communiceren met de API-server of installeren van de belangrijkste onderdelen.

U kunt [Azure Firewall] [ azure-firewall] of een apparaat 3rd derden firewall voor het beveiligen van uw uitgaande verkeer en Definieer deze vereiste poorten en -adressen.

In AKS zijn er twee sets met poorten en -adressen:

* De [benodigde poorten en adres voor clusters met AKS](#required-ports-and-addresses-for-aks-clusters) details van de minimale vereisten voor geautoriseerde uitgaande verkeer.
* De [optionele aanbevolen adressen en poorten voor clusters met AKS](#optional-recommended-addresses-and-ports-for-aks-clusters) niet nodig zijn voor alle scenario's, maar de integratie met andere services zoals Azure Monitor werkt niet correct. Bekijk deze lijst welke poorten en FQDN-namen en autoriseren van de services en onderdelen die worden gebruikt in uw AKS-cluster.

> [!NOTE]
> Uitgaande verkeer beperken werkt alleen op nieuwe AKS-clusters die zijn gemaakt nadat u de registratie van de vlag functie hebt ingeschakeld. Voor bestaande clusters [uitvoeren van een upgrade bewerking van het cluster] [ aks-upgrade] met behulp van de `az aks upgrade` opdracht voordat u het uitgaande verkeer beperken.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Vereiste poorten en -adressen voor clusters met AKS

De volgende uitgaande poorten / netwerkregels zijn vereist voor een AKS-cluster:

* TCP-poort *443*
* TCP-poort *9000* en TCP-poort *22* voor de tunnel-front-schil om te communiceren met het einde van de tunnel op de API-server.

De volgende FQDN-naam / regels van toepassing zijn vereist:

| FQDN-NAAM                      | Poort      | Gebruiken      |
|---------------------------|-----------|----------|
| *.azmk8s.io               | HTTPS:443 | Dit adres is het eindpunt van de API-server. |
| aksrepos.azurecr.io       | HTTPS:443 | Dit adres is vereist voor toegang tot afbeeldingen in Azure Container Registry (ACR). |
| *.blob.core.windows.net   | HTTPS:443 | Dit adres is het archief met back-end voor installatiekopieën die zijn opgeslagen in de ACR. |
| mcr.microsoft.com         | HTTPS:443 | Dit adres is vereist voor toegang tot afbeeldingen in Microsoft Container Registry (MCR). |
| management.azure.com      | HTTPS:443 | Dit adres is vereist voor Kubernetes GET/PUT-bewerkingen. |
| login.microsoftonline.com | HTTPS:443 | Dit adres is vereist voor Azure Active Directory-verificatie. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Optioneel aanbevolen adressen en poorten voor clusters met AKS

De volgende uitgaande poorten / netwerkregels niet nodig zijn voor de AKS-clusters te laten functioneren, maar worden aanbevolen:

* UDP-poort *123* voor tijdsynchronisatie NTP
* UDP-poort *53* voor DNS

De volgende FQDN-naam / toepassing regels worden aanbevolen voor AKS-clusters te laten functioneren:

| FQDN-NAAM                                    | Poort      | Gebruiken      |
|-----------------------------------------|-----------|----------|
| *.ubuntu.com                            | HTTP:80   | Dit adres kunt de Linux-clusterknooppunten de vereiste patches en updates te downloaden. |
| packages.microsoft.com                  | HTTPS:443 | Dit adres is de Microsoft-pakketten-opslagplaats die wordt gebruikt voor in de cache opgeslagen *apt-get* bewerkingen. |
| dc.services.visualstudio.com            | HTTPS:443 | Aanbevolen voor de juiste metrische gegevens en controle via Azure Monitor. |
| *.opinsights.azure.com                  | HTTPS:443 | Aanbevolen voor de juiste metrische gegevens en controle via Azure Monitor. |
| *.monitoring.azure.com                  | HTTPS:443 | Aanbevolen voor de juiste metrische gegevens en controle via Azure Monitor. |
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | Dit adres wordt gebruikt voor de juiste werking van Azure Policy (momenteel in preview in AKS). |
| apt.dockerproject.org                   | HTTPS:443 | Dit adres wordt gebruikt voor de juiste stuurprogramma-installatiebestand en de bewerking op de knooppunten op basis van GPU. |
| nvidia.github.io                        | HTTPS:443 | Dit adres wordt gebruikt voor de juiste stuurprogramma-installatiebestand en de bewerking op de knooppunten op basis van GPU. |

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd welke poorten en -adressen om toe te staan als u uitgaande verkeer voor het cluster beperken. U kunt ook definiëren hoe het gehele product zelf kunnen communiceren en welke beperkingen er binnen het cluster. Zie voor meer informatie, [beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in AKS][network-policy].

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
