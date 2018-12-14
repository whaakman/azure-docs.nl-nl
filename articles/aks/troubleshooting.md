---
title: Algemene problemen met Azure Kubernetes Service oplossen
description: Meer informatie over het oplossen van veelvoorkomende problemen bij het gebruik van Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: c20f2cc03565ce861dfc6317be8459fdafeef0bf
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384102"
---
# <a name="aks-troubleshooting"></a>Het oplossen van AKS
Wanneer u maakt of manager AKS-clusters, kunnen soms problemen optreden. Dit artikel worden enkele veelvoorkomende problemen en stappen voor probleemoplossing.

### <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-issues"></a>In het algemeen waar vind ik informatie over het opsporen van problemen van Kubernetes?

[Hier](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/) is een officiële koppeling voor het oplossen van kubernetes-clusters.
[Hier](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) is een koppeling naar een gids voor probleemoplossing gepubliceerd door een Microsoft-technicus rond het oplossen van problemen schillen, knooppunten, clusters, enzovoort.

### <a name="i-am-getting-a-quota-exceeded-error-during-create-or-upgrade-what-should-i-do"></a>Ik krijg een fout is overschreden tijdens het maken of bijwerken. Wat moet ik doen? 

U moet aanvragen kernen [hier](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

### <a name="what-is-the-max-pods-per-node-setting-for-aks"></a>Wat is de maximale schillen per knooppunt-instelling voor AKS?

De maximale schillen per knooppunt zijn standaard ingesteld op 30 als u een AKS-cluster in Azure portal implementeert.
De maximale schillen per knooppunt zijn standaard ingesteld op 110 als u een AKS-cluster in de Azure CLI implementeren. (Zorg ervoor dat u de nieuwste versie van de Azure CLI gebruikt). Deze instelling kan worden gewijzigd met behulp van de max-knooppunten-per-pod-vlag in de az aks create-opdracht.

### <a name="i-am-getting-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Ik krijg "insufficientSubnetSize" Fout tijdens het implementeren van een AKS-cluster met geavanceerde netwerken. Wat moet ik doen?

Bij aangepaste VNET-optie is geselecteerd voor netwerken in AKS maakt, wordt de Azure CNI gebruikt voor IPAM. Het aantal knooppunten in een AKS-cluster kan overal liggen tussen 1 en 100 liggen. Op basis van 2) boven het subnet grootte moet groter zijn dan product van het aantal knooppunten en de maximale schil per knooppunt subnetgrootte > aantal knooppunten in het cluster * max schillen per knooppunt.

### <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mijn schil is vastgelopen in de modus 'CrashLoopBackOff'. Wat moet ik doen?

Er zijn diverse redenen voor de schil wordt vastgelopen in deze modus. Om uit te zoeken kunt u de 
* Met behulp van de schil zelf `kubectl describe pod <pod-name>`
* De Logboeken gebruiken  `kubectl log <pod-name>`

### <a name="i-am-trying-to-enable-rbac-on-an-existing-cluster-can-you-tell-me-how-i-can-do-that"></a>Ik probeer om in te schakelen van RBAC in een bestaand cluster. Kunt u meer informatie hoe kan ik dat doen?

Helaas RBAC inschakelen voor bestaande clusters wordt niet ondersteund op dit moment. U moet expliciet nieuwe clusters te maken. Als u de CLI, RBAC is standaard ingeschakeld terwijl een wisselknop waarmee deze beschikbaar in de portal AKS is werkstroom maken.

### <a name="i-created-a-cluster-using-the-azure-cli-with-defaults-or-the-azure-portal-with-rbac-enabled-and-numerous-warnings-in-the-kubernetes-dashboard-the-dashboard-used-to-work-before-without-any-warnings-what-should-i-do"></a>Kan ik een cluster met behulp van de Azure CLI met de standaardinstellingen of de Azure-portal met RBAC is ingeschakeld en talrijke waarschuwingen hebt gemaakt in het kubernetes-dashboard. Het dashboard dat wordt gebruikt om te werken voordat zonder eventuele waarschuwingen. Wat moet ik doen?

De reden voor het ophalen van waarschuwingen op het dashboard is dat nu is ingeschakeld met RBAC'ed en toegang tot deze is standaard uitgeschakeld. Deze benadering is raadzaam om in het algemeen beschouwd omdat de blootstelling van het dashboard van de standaard voor alle gebruikers van het cluster tot beveiligingsrisico's leiden kan. Als u nog steeds het dashboard inschakelen wilt, volgt u deze [blog](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/) inschakelen.

### <a name="i-cant-seem-to-connect-to-the-dashboard-what-should-i-do"></a>Ik kan geen verbinding maken met het dashboard lijken. Wat moet ik doen?

De eenvoudigste manier om toegang tot uw service buiten het cluster wordt kubectl-proxy die proxy om aanvragen in de localhost-poort 8001 op de Kubernetes API-server wordt uitgevoerd. Van daaruit kunnen de apiserver proxy met uw service: http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#! / knooppunt? naamruimte = standaard

Als u het kubernetes-dashboard niet ziet, controleert u of de schil kube-proxy wordt uitgevoerd in de naamruimte kube-systeem. Als deze niet actief is, verwijdert de schil en er wordt opnieuw opgestart.

### <a name="i-could-not-get-logs-using-kubectl-logs-or-cannot-connect-to-the-api-server-getting-the-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ik kan geen logboeken met behulp van Kubectl-logboeken ophalen of kan geen verbinding met de api-server aan de ' fout van de server: fout nummer inspreken back-end: bellen tcp... '. Wat moet ik doen?

Zorg ervoor dat de standaard-NSG is niet gewijzigd en de poort 22 is geopend voor verbinding met de API-server. Controleer of de schil tunnelfront wordt uitgevoerd in de naamruimte kube-systeem. Als dat niet het geval is, het geforceerd verwijderen en deze zal opnieuw worden opgestart.

### <a name="i-am-trying-to-upgrade-or-scale-and-am-getting-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-issue"></a>Ik heb geprobeerd om te upgraden of schalen en krijg "message": "Eigenschap 'imageReference' te wijzigen is niet toegestaan." Fout.  Hoe kan ik dit probleem oplossen?

Het is mogelijk dat u deze fout ontvangt omdat u de tags in de agentknooppunten binnen het AKS-cluster hebt gewijzigd. Wijzigen en verwijderen van tags en andere eigenschappen van bronnen in de resourcegroep MC_ * kunnen leiden tot onverwachte resultaten. Wijzigen van de resources onder de MC_ * in de AKS-cluster, verbreekt de Serviceniveaudoelstelling.

### <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>Hoe verleng ik de service-principal-geheim op mijn AKS-cluster?

Standaard worden AKS clusters gemaakt met een service principal waarvoor een verlooptijd van één jaar. Als u in de buurt van de vervaldatum van één jaar, kunt u de referenties voor het uitbreiden van de service-principal voor een aanvullende periode opnieuw instellen.

Het volgende voorbeeld voert de volgende stappen uit:

1. Haalt de service-principal-ID van het gebruik van uw cluster de [az aks show](/cli/azure/aks#az-aks-show) opdracht.
1. Geeft een lijst van de service principal-client geheim met behulp van de [az ad sp referentie lijst](/cli/azure/ad/sp/credential#az-ad-sp-credential-list)
1. De service-principal voor het gebruik van een andere eenjarig breidt de [az ad sp referentie naar de fabrieksinstellingen](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) opdracht. De service principalclientgeheim moet blijven hetzelfde voor de AKS-cluster correct uit te voeren.

```azurecli
# Get the service principal ID of your AKS cluster
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for 1 year
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
