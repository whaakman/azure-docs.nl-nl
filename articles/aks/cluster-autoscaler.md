---
title: Gebruik het cluster automatisch schalen in Azure Kubernetes Service (AKS)
description: Informatie over het gebruik van het cluster automatisch schalen voor het automatisch schalen van uw cluster om te voldoen aan de eisen van de toepassing in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/29/2019
ms.author: iainfou
ms.openlocfilehash: bfdea1d5380750ec23964cd8564db9b3a9539f15
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754638"
---
# <a name="automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Automatisch schalen van een cluster om te voldoen aan de eisen van de toepassing in Azure Kubernetes Service (AKS)

Als u wilt bijhouden toepassingsbehoeften in Azure Kubernetes Service (AKS), moet u mogelijk het aantal knooppunten waarop uw workloads aanpassen. Het clusteronderdeel automatisch schalen kunt van schillen in uw cluster kan niet worden gepland vanwege resourcebeperkingen bekijken. Wanneer er problemen zijn gedetecteerd, wordt het aantal knooppunten verhoogd om te voldoen aan de vraag van de toepassing. Knooppunten ook regelmatig gecontroleerd op een gebrek aan schillen, uitgevoerd met het aantal knooppunten vervolgens weer afneemt indien nodig. Deze mogelijkheid automatisch omhoog of omlaag te schalen het aantal knooppunten in uw AKS-cluster maakt u een efficiënte en voordelige-cluster.

Dit artikel laat u het inschakelen en beheren van het cluster automatisch schalen in een AKS-cluster.

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel is vereist dat u de Azure CLI versie 2.0.55 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

AKS-clusters die ondersteuning bieden voor het cluster automatisch schalen moeten het gebruik van virtuele-machineschaalsets en Kubernetes-versie uitvoeren *1.12.4* of hoger. Deze scale set-ondersteuning is in preview. Aanmelden en maken van clusters met schaalsets gebruiken, installeert de *aks-preview* Azure CLI-extensie met de [az-extensie toevoegen] [ az-extension-add] opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Wanneer u installeert de *aks-preview* extensie, elke AKS-cluster dat u gebruikt het scale set preview-implementatiemodel. Als u wilt zich afmelden en regular, volledig ondersteunde clusters maken, de extensie verwijderen met `az extension remove --name aks-preview`.

## <a name="about-the-cluster-autoscaler"></a>Over het cluster automatisch schalen

Om aan te passen aan veranderende eisen van de toepassing, zoals tussen workday en 's avonds of in een weekend moeten clusters vaak een manier om automatisch te schalen. AKS-clusters kunnen worden geschaald op twee manieren:

* De **automatisch schalen van cluster** watches voor schillen die kunnen niet worden gepland op knooppunten vanwege eventuele resourcebeperkingen. Het cluster automatisch vervolgens verhoogt het aantal knooppunten.
* De **horizontale schillen automatisch schalen** maakt gebruik van de Server van de metrische gegevens in een Kubernetes-cluster voor het bewaken van de vraag naar resources van schillen. Als een service nodig meer bronnen heeft, wordt het aantal schillen automatisch verhoogd om te voldoen aan de vraag.

![Het cluster automatisch schalen en horizontale schillen automatisch schalen werken vaak samen om ondersteuning voor de vereiste toepassing-vereisten](media/autoscaler/cluster-autoscaler.png)

De horizontale schillen automatisch schalen en de cluster automatisch schalen kunnen u het aantal schillen en de knooppunten zo nodig dan ook verlagen. Het cluster automatisch schalen vermindert het aantal knooppunten wanneer er van niet-gebruikte capaciteit voor een bepaalde periode is sprake. Schillen op een knooppunt moet worden verwijderd door het cluster automatisch schalen gepland zijn veilig elders in het cluster. Het cluster automatisch schalen mogelijk kan niet omlaag schalen als schillen kunnen niet worden verplaatst, zoals de volgende situaties:

* Een schil wordt direct gemaakt en wordt niet ondersteund door een netwerkcontroller-object, zoals een implementatie of replica instellen.
* Een pod onderbreking van de budget (PDB) te beperken en het aantal schillen worden vallen onder een bepaalde drempelwaarde komt niet is toegestaan.
* Een schil maakt gebruik van selectoren knooppunt of anti-affiniteit die niet worden uitgevoerd als gepland op een ander knooppunt.

Zie voor meer informatie over hoe het cluster automatisch schalen mogelijk geen omlaag schalen, [welke typen schillen kan voorkomen dat het cluster automatisch schalen verwijderen van een knooppunt?][autoscaler-scaledown]

Het cluster automatisch schalen gebruikt opstartparameters voor zaken zoals tijdsintervallen tussen schaalgebeurtenissen en resourcedrempelwaarden voor de. Deze parameters worden gedefinieerd door de Azure-platform en op dit moment worden niet weergegeven voor u om aan te passen. Voor meer informatie over welke parameters het cluster automatisch schalen gebruikt, Zie [wat zijn de cluster automatisch schalen parameters?] [autoscaler-parameters].

De twee autoscalers kunnen samenwerken, en worden vaak beide geïmplementeerd in een cluster. Combinatie, worden het horizontale schillen automatisch schalen is gericht op het uitvoeren van het aantal pods die nodig zijn om te voldoen aan de vraag van toepassing. Het cluster automatisch schalen is gericht op het aantal knooppunten die zijn vereist ter ondersteuning van de geplande schillen uitgevoerd.

> [!NOTE]
> Handmatig schalen is uitgeschakeld wanneer u het cluster automatisch schalen. Laat het cluster automatisch schalen te bepalen van het vereiste aantal knooppunten. Als u wilt uw cluster, handmatig schalen [het cluster automatisch schalen uitschakelen](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Een AKS-cluster maken en inschakelen van het cluster automatisch schalen

Als u maken van een AKS-cluster wilt, gebruikt u de [az aks maken] [ az-aks-create] opdracht. Geef een *--kubernetes-versie* die voldoet aan of groter is dan de minimale versienummer dat is vereist, zoals wordt beschreven in de voorgaande [voordat u begint met](#before-you-begin) sectie. Als u wilt inschakelen en configureren van het cluster automatisch schalen, gebruikt u de *--inschakelen-cluster-automatisch schalen* parameter, en geeft u een knooppunt *--min-count* en *--maximum aantal*.

> [!IMPORTANT]
> Het cluster automatisch schalen is een Kubernetes-onderdeel. Hoewel het AKS-cluster maakt gebruik van een virtuele-machineschaalset voor de knooppunten, niet handmatig inschakelen of bewerken van instellingen voor scale set automatisch schalen in Azure portal of met de Azure CLI. Laat het Kubernetes-cluster automatisch schalen de schaalinstellingen van de vereiste beheren. Zie voor meer informatie, [kan ik de AKS-resources in de resourcegroep MC_ wijzigen?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group)

Het volgende voorbeeld maakt u een AKS-cluster met virtuele-machineschaalset en het cluster automatisch schalen ingeschakeld en maakt gebruik van een minimum van *1* en maximaal *3* knooppunten:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location canadaeast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --kubernetes-version 1.12.4 \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Het duurt een paar minuten maken van het cluster en de clusterinstellingen voor automatisch schalen configureren.

### <a name="enable-the-cluster-autoscaler-on-an-existing-aks-cluster"></a>Inschakelen van het cluster automatisch schalen op een bestaand AKS-cluster

U kunt het cluster automatisch schalen op een bestaand AKS-cluster dat voldoet aan de vereisten, zoals wordt beschreven in de voorgaande inschakelen [voordat u begint met](#before-you-begin) sectie. Gebruik de [az aks bijwerken] [ az-aks-update] beheren en ervoor kiest om te *--inschakelen-cluster-automatisch schalen*, geef vervolgens een knooppunt *--min-count* en *--maximum aantal*. Het volgende voorbeeld wordt de cluster automatisch schalen op een bestaand cluster die gebruikmaakt van een minimum van *1* en maximaal *3* knooppunten:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Als het minimum aantal knooppunten groter is dan het bestaande aantal knooppunten in het cluster, duurt het maken van de extra knooppunten in een paar minuten.

## <a name="change-the-cluster-autoscaler-settings"></a>De instellingen voor het automatisch schalen van cluster wijzigen

In de vorige stap om te maken of bijwerken van een bestaand AKS-cluster, het cluster automatisch schalen minimum aantal knooppunten is ingesteld op *1*, en het maximum aantal knooppunten is ingesteld op *3*. Als uw toepassing de belasting verandert, moet u mogelijk het aantal clusterknooppunten automatisch schalen aanpassen.

U kunt het aantal knooppunten wijzigen met de [az aks bijwerken] [ az-aks-update] opdracht en geeft u een minimale en maximale waarde. Het volgende voorbeeld wordt de *--min-count* naar *1* en de *--maximum aantal* naar *5*:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

> [!NOTE]
> Tijdens de preview, kunt u een hogere minimum aantal knooppunten dan is momenteel ingesteld voor het cluster niet instellen. Bijvoorbeeld, als u hebt momenteel ingesteld op het aantal min *1*, kunt u het aantal minuten dat moet worden niet bijwerken *3*.

De prestaties van uw toepassingen en services controleren en past u de cluster automatisch schalen knooppunt aantallen zodat deze overeenkomt met de vereiste prestaties.

## <a name="disable-the-cluster-autoscaler"></a>Het cluster automatisch schalen uitschakelen

Als u niet meer gebruiken van het cluster automatisch schalen wilt, kunt u uitschakelen met behulp van de [az aks bijwerken] [ az-aks-update] opdracht. Knooppunten worden niet verwijderd wanneer het cluster automatisch schalen is uitgeschakeld.

Als u wilt verwijderen van het cluster automatisch schalen, geef de *--disable-cluster-automatisch schalen* parameter, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

U kunt handmatig schalen uw cluster met de [az aks schaal] [ az-aks-scale] opdracht. Als u de horizontale schillen automatisch schalen, die de functie nog steeds om uit te voeren met het cluster automatisch schalen die is uitgeschakeld, maar schillen dat er uiteindelijk kan niet worden gepland als het knooppunt resources bevinden zich allemaal in gebruik.

## <a name="next-steps"></a>Volgende stappen

In dit artikel laat zien hoe u het aantal AKS-knooppunten automatisch schalen. U kunt ook het horizontale schillen automatisch schalen gebruiken om aan te passen automatisch het aantal schillen waarop uw toepassing wordt uitgevoerd. Zie voor instructies over het gebruik van de horizontale schillen automatisch schalen [toepassingen schalen in AKS][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
