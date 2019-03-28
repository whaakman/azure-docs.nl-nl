---
title: Algemene problemen met Azure Kubernetes Service oplossen
description: Meer informatie over het oplossen van veelvoorkomende problemen bij het gebruik van Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 56d91d7801c576064b941ac6089a52e74b4a3b7b
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540921"
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

Als Azure CNI (Geavanceerd netwerken) wordt gebruikt, AKS preallocates IP-adressen op basis van de "max-schillen" per knooppunt dat is geconfigureerd. Het aantal knooppunten in een AKS-cluster kan overal liggen tussen 1 en 110. Op basis van het geconfigureerde maximum schillen per knooppunt, moet de grootte van het gatewaysubnet groter zijn dan de "product van het aantal knooppunten en de maximale schil per knooppunt". De volgende eenvoudige vergelijking geeft een overzicht van dit:

Grootte van het gatewaysubnet > aantal knooppunten in het cluster (rekening houdend met de toekomstige vereisten voor vergroten/verkleinen) * max schillen per knooppunt.

Zie voor meer informatie, [Plan IP-adressen voor uw cluster](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

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

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ik kan geen logboeken ophalen met behulp van kubectl Logboeken of ik kan geen verbinding maken met de API-server. Krijg ik ' fout van de server: fout nummer inspreken back-end: bellen tcp... '. Wat moet ik doen?

Zorg ervoor dat de standaardnetwerkbeveiligingsgroep wordt niet gewijzigd en dat poort 22 geopend voor verbinding met de API-server is. Controleer of de `tunnelfront` pod wordt uitgevoerd in de *kube-systeem* naamruimte met behulp van de `kubectl get pods --namespace kube-system` opdracht. Als dat niet, opnieuw geforceerd verwijderen van de schil en het.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Ik heb geprobeerd om te upgraden of schalen en krijg een "bericht: Fout bij voor het wijzigen van de eigenschap 'imageReference' is niet toegestaan'. Hoe kan ik dit probleem oplossen?

Mogelijk worden aan te bieden u deze fout omdat u de tags in de agentknooppunten binnen het AKS-cluster hebt gewijzigd. Wijzigen en verwijderen van tags en andere eigenschappen van bronnen in de resourcegroep MC_ * kunnen leiden tot onverwachte resultaten. Wijzigen van de resources in de groep MC_ * in de AKS-cluster verbreekt de service level objective (SLO).

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Ik krijg fouten die mijn cluster zich in de status mislukt en een upgrade of schalen niet werkt totdat het probleem is opgelost

*Deze hulp bij probleemoplossing is uit gericht https://aka.ms/aks-cluster-failed*

Deze fout treedt op wanneer clusters een foutstatus voor meerdere redenen invoeren. Volg de stappen hieronder om de status van uw cluster is niet opgelost voordat u de eerder mislukte bewerking opnieuw uitvoert:

1. Tot het cluster van is `failed` staat, `upgrade` en `scale` upbewerkingen wordt niet voltooid. Algemene root problemen en oplossingen zijn onder andere:
    * Schalen met **onvoldoende rekenquota (CRP)**. Om op te lossen, moet u eerst uw cluster naar een stabiele doelstatus binnen quota schalen. Volg deze [stappen om aan te vragen een compute-quotum verhogen](../azure-supportability/resource-manager-core-quotas-request.md) voordat u probeert opnieuw buiten de eerste quotalimieten kan worden uitgebreid.
    * Schalen van een cluster met geavanceerde netwerken en **onvoldoende (netwerk) subnetresources**. Om op te lossen, moet u eerst uw cluster naar een stabiele doelstatus binnen quota schalen. Volg [deze stappen om aan te vragen van een resourcequotum verhogen](../azure-resource-manager/resource-manager-quota-errors.md#solution) voordat u probeert opnieuw buiten de eerste quotalimieten kan worden uitgebreid.
2. Als de onderliggende oorzaak van de upgrade mislukt, opgelost is, moet uw cluster zich in een status geslaagd. Zodra de status van een geslaagd is geverifieerd, moet u de oorspronkelijke bewerking opnieuw proberen.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Ik krijg fouten op wanneer u probeert te upgraden of schaal die de status van mijn cluster momenteel wordt wordt bijgewerkt of de upgrade is mislukt

*Deze hulp bij probleemoplossing is uit gericht https://aka.ms/aks-pending-upgrade*

Bewerkingen voor een cluster worden beperkt wanneer actieve upgrade bewerkingen plaatsvinden of een upgrade is uitgevoerd, maar later is mislukt. Voor het vaststellen van het probleem uitvoeren `az aks show -g myResourceGroup -n myAKSCluster -o table` om op te halen van gedetailleerde status van uw cluster. Op basis van het resultaat:

* Als het cluster is actief bijwerkt, wacht u totdat de bewerking wordt beëindigd. Als deze is geslaagd, probeert u de eerder mislukte bewerking opnieuw uit.
* Als het cluster is de upgrade mislukt, volgt u stappen die hierboven worden beschreven

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>Kan ik mijn cluster verplaatsen naar een ander abonnement of mijn abonnement met mijn cluster naar een nieuwe tenant?

Als u uw AKS-cluster hebt verplaatst naar een ander abonnement of het cluster abonnement naar een nieuwe tenant die eigenaar is, wordt het cluster functionaliteit vanwege verloren gaat roltoewijzingen en service-principals rechten verbroken. **AKS biedt geen ondersteuning voor clusters verplaatsen over abonnementen of tenants** vanwege deze beperking.
