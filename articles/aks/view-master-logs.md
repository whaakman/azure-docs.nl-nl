---
title: Azure Kubernetes Service (AKS)-controller-logboeken weergeven
description: Meer informatie over het inschakelen en weergeven van de logboeken voor het hoofdknooppunt van Kubernetes in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/25/2018
ms.author: iainfou
ms.openlocfilehash: c3256b2b1e8be0e883d0f7b47e66ab1fee95b042
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138157"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Inschakelen en controleren van Kubernetes-hoofdknooppunt in Azure Kubernetes Service (AKS registreert)

Met Azure Kubernetes Service (AKS), de master-onderdelen, zoals de *kube-apiserver* en *kube-controller-manager* dienen alleen als een beheerde service. Maken en beheren van de knooppunten die worden uitgevoerd de *kubelet* en container-runtime en implementeer uw toepassingen via de beheerde Kubernetes API-server. Om op te lossen van uw toepassingen en services, moet u mogelijk om de logboeken die worden gegenereerd door deze master-onderdelen weer te geven. Dit artikel ziet u hoe u met Azure Log Analytics kunt inschakelen en de logboeken van de onderdelen van Kubernetes-master op te vragen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel is een bestaand AKS-cluster die worden uitgevoerd in uw Azure-account vereist. Als u een AKS-cluster niet al hebt, maakt u één met de [Azure CLI] [ cli-quickstart] of [Azure-portal][portal-quickstart]. Log Analytics werkt met beide RBAC en niet-RBAC ingeschakeld AKS-clusters.

## <a name="enable-diagnostics-logs"></a>Logboeken met diagnostische gegevens inschakelen

Om te verzamelen en controleren van gegevens uit meerdere bronnen, biedt Log Analytics een query taal en analytics-engine die inzicht in uw omgeving biedt. Een werkruimte wordt gebruikt voor het verzamelen en analyseren van de gegevens en kan worden geïntegreerd met andere Azure-services, zoals Application Insights en Security Center. U kunt voor het gebruik van een ander platform voor het analyseren van de logboeken, in plaats daarvan voor het verzenden van diagnostische logboeken naar een Azure storage-account of event hub. Zie voor meer informatie, [wat is Azure Log Analytics?] [log-analytics-overview].

Log Analytics is ingeschakeld en beheerd in Azure portal. Om in te schakelen logboekgegevens verzamelen voor de master Kubernetes-onderdelen in uw AKS-cluster, de Azure-portal openen in een webbrowser en voer de volgende stappen uit:

1. Selecteer de resourcegroep voor uw AKS-cluster, zoals *myResourceGroup*. Selecteer de resourcegroep waarin uw afzonderlijke AKS-cluster-resources, zoals niet *MC_myResourceGroup_myAKSCluster_eastus*.
1. Aan de linkerkant, kies **diagnostische instellingen**.
1. Selecteer uw AKS-cluster, zoals *myAKSCluster*, kiest u voor **diagnostische gegevens inschakelen**.
1. Voer een naam, zoals *myAKSLogs*, selecteert u de optie om **verzenden naar Log Analytics**.
    * Kies aan *configureren* Log Analytics, en selecteer vervolgens een bestaande werkruimte of **nieuwe werkruimte maken**.
    * Als u nodig hebt om een werkruimte te maken, geeft u een naam, een resourcegroep en een locatie.
1. In de lijst met beschikbare logboeken, selecteert u de logboeken die u inschakelen wilt, zoals *kube-apiserver*, *kube-controller-manager*, en *kube-scheduler*. U kunt retourneren en de verzamelde Logboeken niet wijzigen wanneer de Log Analytics zijn ingeschakeld.
1. Wanneer u klaar bent, selecteert u **opslaan** om van de geselecteerde logboeken te verzamelen.

Het volgende voorbeeld schermafbeelding van de portal wordt de *diagnostische instellingen* venster en vervolgens de optie voor het maken van een Log Analytics-werkruimte:

![Inschakelen van Log Analytics-werkruimte voor Log Analytics van AKS-cluster](media/view-master-logs/enable-oms-log-analytics.png)

>[!NOTE]
>OMS-werkruimten worden nu aangeduid als Log Analytics-werkruimten.

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Een test-schil op het AKS-cluster plannen

Voor het genereren van sommige logboeken, maakt u een nieuwe schil in uw AKS-cluster. Het volgende voorbeeld YAML-manifest kan worden gebruikt om een eenvoudige NGINX-exemplaar te maken. Maak een bestand met de naam `nginx.yaml` in een editor van uw keuze en plak de volgende inhoud:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

Maken van de schil met de [kubectl maken] [ kubectl-create] opdracht en geeft u het YAML-bestand, zoals wordt weergegeven in het volgende voorbeeld:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Verzamelde logboeken bekijken

Het duurt een paar minuten voor de diagnostische logboeken om te worden ingeschakeld en worden weergegeven in de Log Analytics-werkruimte. Selecteer in de Azure-portal, de resourcegroep voor uw Log Analytics-werkruimte, zoals *myResourceGroup*, kiest u uw Log Analytics-resource, zoals *myAKSLogs*.

![Selecteer de Log Analytics-werkruimte voor uw AKS-cluster](media/view-master-logs/select-log-analytics-workspace.png)

Aan de linkerkant, kies **logboeken**. Om weer te geven de *kube-apiserver*, voert u de volgende query in het tekstvak in:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

Veel logboeken worden waarschijnlijk geretourneerd voor de API-server. Als u wilt richten op de query voor het weergeven van de logboeken over de NGINX-schil in de vorige stap hebt gemaakt, voegt u een extra *waar* instructie om te zoeken naar *schillen/nginx* zoals wordt weergegeven in de volgende voorbeeldquery:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

De specifieke logboeken voor uw schil NGINX worden weergegeven, zoals wordt weergegeven in het volgende voorbeeld:

![Log Analytics-query resultaten voor voorbeeld NGINX pod](media/view-master-logs/log-analytics-query-results.png)

Als u extra logboeken, kunt u de query voor bijwerken de *categorie* naam naar *kube-controller-manager* of *kube-scheduler*, afhankelijk van wat extra Hiermee wordt u inschakelen. Aanvullende *waar* instructies kunnen vervolgens worden gebruikt om de gebeurtenissen die u zoekt te verfijnen.

Zie voor meer informatie over het opvragen en filteren van uw logboekgegevens [weergeven of analyseren van gegevens die zijn verzameld met zoeken in Logboeken Log Analytics][analyze-log-analytics].

## <a name="log-event-schema"></a>Gebeurtenisschema

De volgende tabel worden om te analyseren van de logboekgegevens, het schema voor elke gebeurtenis:

| Veldnaam               | Beschrijving |
|--------------------------|-------------|
| *ResourceId*             | Azure-resource die het logboek geproduceerd |
| *tijd*                   | Timestamp van wanneer het logboek is geüpload |
| *Categorie*               | Naam van container/component genereren van het logboek |
| *OperationName*          | Altijd *Microsoft.ContainerService/managedClusters/diagnositicLogs/Read* |
| *Properties.log*         | Volledige tekst van het logboek van het onderdeel |
| *Properties.Stream*      | *stderr* of *stdout* |
| *Properties.Pod*         | De naam van de schil die het logboek afkomstig zijn uit |
| *properties.containerID* | Id van de docker-container die dit logboek afkomstig zijn uit |

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het inschakelen en bekijk de logboeken voor de master Kubernetes-onderdelen in uw AKS-cluster. Als u wilt bewaken en het probleem verder oplossen, kunt u ook [Bekijk de logboeken Kubelet] [ kubelet-logs] en [SSH knooppunt toegang][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../log-analytics/log-analytics-tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
