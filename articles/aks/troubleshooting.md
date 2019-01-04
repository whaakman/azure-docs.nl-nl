---
title: Algemene problemen met Azure Kubernetes Service oplossen
description: Meer informatie over het oplossen van veelvoorkomende problemen bij het gebruik van Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: fd3d1c464c6f2d4cbecd715db0689581ca141769
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654067"
---
# <a name="aks-troubleshooting"></a>Het oplossen van AKS

Wanneer u maken of beheren van clusters met Azure Kubernetes Service (AKS), kunt u soms problemen ondervinden. Dit artikel worden enkele veelvoorkomende problemen en stappen voor probleemoplossing.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>In het algemeen waar vind ik informatie over het oplossen van problemen van Kubernetes?

Probeer de [officiële handleiding voor het oplossen van Kubernetes-clusters](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Er is ook een [problemen oplossen met](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), gepubliceerd door een Microsoft-technicus voor het oplossen van schillen, knooppunten, clusters en andere functies.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Ik krijg een 'quotum overschreden'-Fout tijdens het maken of bijwerken. Wat moet ik doen? 

U moet [kernen aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>Wat is de maximale schillen-per-knooppunt-instelling voor AKS?

De maximale schillen-per-knooppunt-instelling is standaard 30 als u een AKS-cluster in Azure portal implementeert.
De maximuminstelling van schillen-per-knooppunt is 110 standaard als u een AKS-cluster in de Azure CLI implementeren. (Zorg ervoor dat u de nieuwste versie van de Azure CLI). Deze instelling kan worden gewijzigd met behulp van de `–-max-pods` markering waarmee wordt aangegeven de `az aks create` opdracht.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Ik krijg een insufficientSubnetSize-fout tijdens het implementeren van een AKS-cluster met geavanceerde netwerken. Wat moet ik doen?

In de aangepaste Azure Virtual Network-optie voor het netwerk tijdens het maken van AKS, Azure Container netwerk Interface (CNI) gebruikt voor IP-adresbeheer (IPAM). Het aantal knooppunten in een AKS-cluster kan overal liggen tussen 1 en 100 liggen. Op basis van de vorige sectie, moet de grootte van het gatewaysubnet groter zijn dan het product van het aantal knooppunten en de maximale schillen per knooppunt. De relatie kan worden uitgedrukt in op deze manier: grootte van het gatewaysubnet > aantal knooppunten in het cluster * maximale schillen per knooppunt.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mijn schil is vastgelopen in de modus CrashLoopBackOff. Wat moet ik doen?

Er zijn diverse redenen voor de schil wordt vastgelopen in deze modus. U kunt bekijken:

* De schil zelf, met behulp van `kubectl describe pod <pod-name>`.
* De logboeken, met behulp van `kubectl log <pod-name>`.

Zie voor meer informatie over het oplossen van problemen met pod [fouten opsporen in toepassingen](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Ik probeer om in te schakelen van RBAC in een bestaand cluster. Hoe kan ik dat doen?

Helaas, op rollen gebaseerd toegangsbeheer (RBAC) inschakelen voor bestaande clusters wordt niet ondersteund op dit moment. U moet expliciet nieuwe clusters maken. Als u de CLI gebruikt, worden RBAC is standaard ingeschakeld. Als u de AKS-portal, is een wisselknop om in te schakelen RBAC beschikbaar in de werkstroom voor het maken.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Ik heb een cluster gemaakt met RBAC ingeschakeld met behulp van de Azure-CLI met de standaardinstellingen of de Azure-portal en nu kan ik veel waarschuwingen ziet op de Kubernetes-dashboard. Het dashboard dat wordt gebruikt om te werken zonder eventuele waarschuwingen. Wat moet ik doen?

De reden voor de waarschuwingen op het dashboard is dat het cluster is nu ingeschakeld met RBAC en toegang tot deze is standaard uitgeschakeld. Deze aanpak is in het algemeen aanbevolen omdat de blootstelling van het dashboard van de standaard voor alle gebruikers van het cluster tot beveiligingsrisico's leiden kan. Als u nog steeds het dashboard inschakelen wilt, volgt u de stappen in [dit blogbericht](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Ik kan geen verbinding maken met het dashboard. Wat moet ik doen?

De eenvoudigste manier om toegang tot uw service buiten het cluster is om uit te voeren `kubectl proxy`, welke aanvragen proxy's verzonden naar de localhost-poort 8001 naar de Kubernetes API-server. Van daaruit, kan de API-server proxy met uw service: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Als u het Kubernetes-dashboard niet ziet, controleert u of de `kube-proxy` pod wordt uitgevoerd in de `kube-system` naamruimte. Als deze niet actief is, verwijdert de schil en deze zal opnieuw worden opgestart.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ik kan geen logboeken ophalen met behulp van kubectl Logboeken of ik kan geen verbinding maken met de API-server. Krijg ik ' fout van de server: fout nummer inspreken back-end: bellen tcp... ' Wat moet ik doen?

Zorg ervoor dat de standaard-netwerkbeveiligingsgroep (NSG) wordt niet gewijzigd en dat poort 22 geopend voor verbinding met de API-server is. Controleer of de `tunnelfront` pod wordt uitgevoerd in de `kube-system` naamruimte. Als dat niet, opnieuw geforceerd verwijderen van de schil en het.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-problem"></a>Ik heb geprobeerd om te upgraden of schalen en krijg een "bericht: Fout bij voor het wijzigen van de eigenschap 'imageReference' is niet toegestaan'.  Hoe kan ik dit probleem oplossen?

Mogelijk worden aan te bieden u deze fout omdat u de tags in de agentknooppunten binnen het AKS-cluster hebt gewijzigd. Wijzigen en verwijderen van tags en andere eigenschappen van bronnen in de resourcegroep MC_ * kunnen leiden tot onverwachte resultaten. Wijzigen van de resources in de groep MC_ * in de AKS-cluster verbreekt de service level objective (SLO).

## <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>Hoe verleng ik de service-principal-geheim op mijn AKS-cluster?

AKS-clusters worden standaard gemaakt met een service-principal met een verlooptijd van één jaar. U kunt de referenties voor het uitbreiden van de service-principal voor een aanvullende periode opnieuw als u in de buurt van de vervaldatum.

Het volgende voorbeeld voert deze stappen uit:

1. Haalt de service-principal-ID van het cluster met behulp van de [az aks show](/cli/azure/aks#az-aks-show) opdracht.
1. Geeft een lijst van de service principalclientgeheim met behulp van de [az ad sp referentie lijst](/cli/azure/ad/sp/credential#az-ad-sp-credential-list).
1. De service-principal is een uitbreiding voor een andere één jaar met behulp van de [az ad sp referentie naar de fabrieksinstellingen](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) opdracht. De service principalclientgeheim moet blijven hetzelfde voor de AKS-cluster correct uit te voeren.

```azurecli
# Get the service principal ID of your AKS cluster.
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret.
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for one year.
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
