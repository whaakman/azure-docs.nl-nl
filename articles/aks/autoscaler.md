---
title: Kubernetes op Azure - Cluster automatisch schalen
description: Meer informatie over het gebruik van het cluster automatisch schalen met Azure Kubernetes Service (AKS) voor het automatisch schalen van uw cluster om te voldoen aan de vraag.
services: container-service
author: sakthivetrivel
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/19/18
ms.author: sakthivetrivel
ms.custom: mvc
ms.openlocfilehash: 629659a3a5090bae987be77637a574fcbe0abe98
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163914"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Automatisch schalen van cluster op Azure Kubernetes Service (AKS) - Preview

Azure Kubernetes Service (AKS) biedt een flexibele oplossing voor het implementeren van een beheerde Kubernetes-cluster in Azure. Als resource vraag-toeneemt, het cluster automatisch schalen kunt u uw cluster uitbreiden om te voldoen aan deze vraag op basis van beperkingen u instelt. Het cluster automatisch schalen (CA) doet dit door de agentknooppunten op basis van in behandeling zijnde schillen schalen. Het scant de cluster regelmatig te controleren op openstaande schillen of lege knooppunten en neemt de omvang indien mogelijk. Standaard de CA scant op in behandeling zijnde schillen elke 10 seconden en een knooppunt wordt verwijderd als het niet nodig voor meer dan 10 minuten. Gebruikt in combinatie met het horizontale schillen automatisch schalen (HPA), worden de HPA schilreplica's en resources aan de hand van aanvraag bijgewerkt. Als er niet voldoende knooppunten of overbodige knooppunten volgens deze pod schalen, de CA reageert en plannen van de schillen in de nieuwe set knooppunten.

> [!IMPORTANT]
> Integratie van Azure Kubernetes Service (AKS) cluster automatisch schalen is momenteel in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.
>

## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat u een RBAC-functionaliteit AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de [Azure Kubernetes Service (AKS)-snelstartgids][aks-quick-start].

 Voor het gebruik van het cluster automatisch schalen, uw cluster moet worden met behulp van Kubernetes v1.10.X of hoger en moet worden ingeschakeld voor RBAC. Als u wilt uw cluster upgraden, raadpleegt u het artikel [een AKS-cluster upgraden][aks-upgrade].

## <a name="gather-information"></a>Gegevens verzamelen

De volgende tabel bevat alle informatie die u in de definitie van automatisch schalen opgeven moet.

Haal uw abonnements-ID met: 

``` azurecli
az account show --query id
```

Genereert een set Azure-referenties met de volgende opdracht:

```console
$ az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>" --output json

"appId": <app-id>,
"displayName": <display-name>,
"name": <name>,
"password": <app-password>,
"tenant": <tenant-id>
```

De App-ID, het wachtwoord en Tenant-ID is de clientID, clientSecret en tenant-id in de volgende stappen.

Haal de naam van uw knooppuntgroep door het uitvoeren van de volgende opdracht uit. 

```console
$ kubectl get nodes --show-labels
```

Uitvoer:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Haal vervolgens de waarde van het label **agentpool**. De standaardnaam voor de knooppuntgroep van een cluster is 'nodepool1'.

Als u de naam van uw resourcegroep knooppunt, haal de waarde van het label **kubernetes.azure.com<span></span>/cluster**. Naam van de resourcegroep knooppunt is algemeen van het formulier MC_ [resourcegroep]\__ [cluster-naam] [Plaats].

De parameter vmType verwijst naar de service wordt gebruikt, die hier, AKS.

U hebt nu de volgende informatie:

- abonnements-id
- ResourceGroup
- Clusternaam
- ClientID
- ClientSecret
- TenantID
- NodeResourceGroup
- VMType

Vervolgens, codeert u al deze waarden met base64. Als u bijvoorbeeld de waarde VMType met base64 coderen:

```console
$ echo AKS | base64
QUtTCg==
```

## <a name="create-secret"></a>Geheim maken
Met deze gegevens kunt maken van een geheim voor de implementatie van de waarden in de vorige stappen, zoals:

- ClientID: `<base64-encoded-client-id>`
- ClientSecret: `<base64-encoded-client-secret>`
- ResourceGroup: `<base64-encoded-resource-group>` (kleine letters gebruikt)
- Abonnements-id: `<base64-encode-subscription-id>`
- Tenant-id: `<base64-encoded-tenant-id>`
- VMType: `<base64-encoded-vm-type>`
- Clusternaam: `<base64-encoded-clustername>`
- NodeResourceGroup: `<base64-encoded-node-resource-group>` (de waarde van het label verbatim gebruiken. De aanvraag is gevoelig)

in de volgende indeling:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>
  ResourceGroup: <base64-encoded-resource-group>
  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

## <a name="create-a-deployment-chart"></a>Maak een implementatie-grafiek

Maak een bestand met de naam `aks-cluster-autoscaler.yaml`, en de volgende YAML-code naartoe te kopiëren.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
      - image: k8s.gcr.io/cluster-autoscaler:{{ ca_version }}
        imagePullPolicy: Always
        name: cluster-autoscaler
        resources:
          limits:
            cpu: 100m
            memory: 300Mi
          requests:
            cpu: 100m
            memory: 300Mi
        command:
        - ./cluster-autoscaler
        - --v=3
        - --logtostderr=true
        - --cloud-provider=azure
        - --skip-nodes-with-local-storage=false
        - --nodes=1:10:nodepool1
        env:
        - name: ARM_SUBSCRIPTION_ID
          valueFrom:
            secretKeyRef:
              key: SubscriptionID
              name: cluster-autoscaler-azure
        - name: ARM_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: ResourceGroup
              name: cluster-autoscaler-azure
        - name: ARM_TENANT_ID
          valueFrom:
            secretKeyRef:
              key: TenantID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_ID
          valueFrom:
            secretKeyRef:
              key: ClientID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              key: ClientSecret
              name: cluster-autoscaler-azure
        - name: ARM_VM_TYPE
          valueFrom:
            secretKeyRef:
              key: VMType
              name: cluster-autoscaler-azure
        - name: AZURE_CLUSTER_NAME
          valueFrom:
            secretKeyRef:
              key: ClusterName
              name: cluster-autoscaler-azure
        - name: AZURE_NODE_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: NodeResourceGroup
              name: cluster-autoscaler-azure
      restartPolicy: Always
```

Kopieer en plak het geheim in de vorige stap hebt gemaakt en voeg deze aan het begin van het bestand.

Vervolgens om in te stellen het bereik van knooppunten, vul in het argument voor `--nodes` onder `command` in het formulier MIN:MAX:NODE_POOL_NAME. Bijvoorbeeld: `--nodes=3:10:nodepool1` stelt het minimum aantal knooppunten tot 3, het maximum aantal knooppunten tot en met 10 en de naam van het knooppunt toepassingen naar nodepool1.

Vul vervolgens in het veld installatiekopie onder **containers** met de versie van het cluster automatisch schalen die u wilt gebruiken. AKS vereist v1.2.2 of hoger. In dit voorbeeld maakt gebruik van v1.2.2.

## <a name="deployment"></a>Implementatie

Cluster-automatisch schalen implementeren door uit te voeren

```console
kubectl create -f cluster-autoscaler-containerservice.yaml
```

Gebruik de volgende opdracht om te controleren of het cluster automatisch schalen wordt uitgevoerd, en controleer de lijst met schillen. Als er een schil voorafgegaan door 'cluster-automatisch schalen"uitgevoerd, is uw cluster automatisch schalen geïmplementeerd.

```console
kubectl -n kube-system get pods
```

Als u wilt weergeven van de status van het cluster automatisch schalen, worden uitgevoerd

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="next-steps"></a>Volgende stappen

Bekijk voor het gebruik van het cluster automatisch schalen met het horizontale schillen automatisch schalen, [Kubernetes-toepassing en -infrastructuur schalen][aks-tutorial-scale].

U krijgt meer informatie over het implementeren en beheren van AKS in de AKS-zelfstudies.

> [!div class="nextstepaction"]
> [AKS-zelfstudie][aks-tutorial-prepare-app]

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!-- LINKS - external -->
[cluster-autoscale]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
