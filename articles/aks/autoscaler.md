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
ms.openlocfilehash: e16c82f7c49bf90fc074732d0a989b9de94a52c5
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49375848"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Automatisch schalen van cluster op Azure Kubernetes Service (AKS) - Preview

Azure Kubernetes Service (AKS) biedt een flexibele oplossing voor het implementeren van een beheerde Kubernetes-cluster in Azure. Als resource vraag-toeneemt, het cluster automatisch schalen kunt u uw cluster uitbreiden om te voldoen aan deze vraag op basis van beperkingen u instelt. Het cluster automatisch schalen (CA) doet dit door de agentknooppunten op basis van in behandeling zijnde schillen schalen. Het scant de cluster regelmatig te controleren op openstaande schillen of lege knooppunten en neemt de omvang indien mogelijk. Standaard de CA scant op in behandeling zijnde schillen elke 10 seconden en een knooppunt wordt verwijderd als het niet nodig voor meer dan 10 minuten. Gebruikt in combinatie met de [horizontale schillen automatisch schalen](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA), de HPA worden schilreplica's en resources aan de hand van aanvraag bijgewerkt. Als er niet voldoende knooppunten of overbodige knooppunten volgens deze pod schalen, de CA reageert en plannen van de schillen in de nieuwe set knooppunten.

In dit artikel wordt beschreven hoe u het cluster automatisch schalen op de agentknooppunten implementeren. Omdat het cluster automatisch schalen wordt geïmplementeerd in de naamruimte kube-systeem, wordt niet automatisch schalen geschaald in het knooppunt dat pod uitgevoerd.

> [!IMPORTANT]
> Integratie van Azure Kubernetes Service (AKS) cluster automatisch schalen is momenteel in **preview**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.
>

## <a name="prerequisites-and-considerations"></a>Vereisten en overwegingen

Dit document wordt ervan uitgegaan dat u een RBAC-functionaliteit AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de [Azure Kubernetes Service (AKS)-snelstartgids][aks-quick-start].

 Voor het gebruik van het cluster automatisch schalen, uw cluster moet worden met behulp van Kubernetes v1.10.X of hoger en moet worden ingeschakeld voor RBAC. Als u wilt uw cluster upgraden, raadpleegt u het artikel [een AKS-cluster upgraden][aks-upgrade].

Resourceaanvragen voor uw schillen definiëren. Het cluster automatisch schalen er ongeveer zo uitziet bij welke resource-aanvragen worden gemaakt door schillen, niet de resources daadwerkelijk in gebruik, zoals het horizontale schillen automatisch schalen heeft. Binnen de `spec: containers` sectie van de implementatiedefinitie van de, definieert u de vereisten voor CPU en geheugen. Het volgende voorbeeld-codefragment vraagt 0,5 vCPU en 64Mb geheugen op het knooppunt:

  ```yaml
  resources:
    requests:
      cpu: 500m
      memory: 64Mb
  ```

Wanneer het cluster automatisch schalen wordt gebruikt, te voorkomen dat het aantal knooppunten handmatig te schalen. Het cluster automatisch schalen niet mogelijk om te bepalen van de juiste hoeveelheid rekenbronnen en conflicteren met het aantal knooppunten die u handmatig definiëren.

## <a name="gather-information"></a>Gegevens verzamelen

Voer deze bash-script voor het genereren van de machtigingen voor uw cluster automatisch schalen om uit te voeren in uw cluster:

```sh
#! /bin/bash
ID=`az account show --query id -o json`
SUBSCRIPTION_ID=`echo $ID | tr -d '"' `

TENANT=`az account show --query tenantId -o json`
TENANT_ID=`echo $TENANT | tr -d '"' | base64`

read -p "What's your cluster name? " cluster_name
read -p "Resource group name? " resource_group

CLUSTER_NAME=`echo $cluster_name | base64`
RESOURCE_GROUP=`echo $resource_group | base64`

PERMISSIONS=`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" -o json`
CLIENT_ID=`echo $PERMISSIONS | sed -e 's/^.*"appId"[ ]*:[ ]*"//' -e 's/".*//' | base64`
CLIENT_SECRET=`echo $PERMISSIONS | sed -e 's/^.*"password"[ ]*:[ ]*"//' -e 's/".*//' | base64`

SUBSCRIPTION_ID=`echo $ID | tr -d '"' | base64 `

NODE_RESOURCE_GROUP=`az aks show --name $cluster_name  --resource-group $resource_group -o tsv --query 'nodeResourceGroup' | base64`

echo "---
apiVersion: v1
kind: Secret
metadata:
    name: cluster-autoscaler-azure
    namespace: kube-system
data:
    ClientID: $CLIENT_ID
    ClientSecret: $CLIENT_SECRET
    ResourceGroup: $RESOURCE_GROUP
    SubscriptionID: $SUBSCRIPTION_ID
    TenantID: $TENANT_ID
    VMType: QUtTCg==
    ClusterName: $CLUSTER_NAME
    NodeResourceGroup: $NODE_RESOURCE_GROUP
---"
```

Nadat u de stappen in het script, het script uw gegevens in de vorm van een geheim wordt uitvoer als volgt te werk:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>$
  ResourceGroup: <base64-encoded-resource-group>  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

Vervolgens de naam van uw knooppuntgroep ophalen door het uitvoeren van de volgende opdracht uit. 

```console
$ kubectl get nodes --show-labels
```

Uitvoer:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Haal vervolgens de waarde van het label **agentpool**. De standaardnaam voor de knooppuntgroep van een cluster is 'nodepool1'.

Nu u uw geheim en knooppunt-groep, kunt u een implementatie-grafiek kunt maken.

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
apiVersion: rbac.authorization.k8s.io/v1
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
apiVersion: rbac.authorization.k8s.io/v1
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
apiVersion: rbac.authorization.k8s.io/v1
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
apiVersion: rbac.authorization.k8s.io/v1
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
apiVersion: extensions/v1
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
      - image: gcr.io/google-containers/cluster-autoscaler:v1.2.2
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

Kopiëren en plakken van het geheim in de vorige stap hebt gemaakt en deze aan het begin van het bestand invoegen.

Vervolgens om in te stellen het bereik van knooppunten, vul in het argument voor `--nodes` onder `command` in het formulier MIN:MAX:NODE_POOL_NAME. Bijvoorbeeld: `--nodes=3:10:nodepool1` stelt het minimum aantal knooppunten tot 3, het maximum aantal knooppunten tot en met 10 en de naam van het knooppunt toepassingen naar nodepool1.

Vul vervolgens in het veld installatiekopie onder **containers** met de versie van het cluster automatisch schalen die u wilt gebruiken. AKS vereist v1.2.2 of hoger. In dit voorbeeld maakt gebruik van v1.2.2.

## <a name="deployment"></a>Implementatie

Cluster-automatisch schalen implementeren door uit te voeren

```console
kubectl create -f aks-cluster-autoscaler.yaml
```

Gebruik de volgende opdracht om te controleren of het cluster automatisch schalen wordt uitgevoerd, en controleer de lijst met schillen. Er moet een schil voorafgegaan door 'cluster-automatisch schalen"uitgevoerd. Als u dit ziet, is uw cluster automatisch schalen is geïmplementeerd.

```console
kubectl -n kube-system get pods
```

Als u wilt weergeven van de status van het cluster automatisch schalen, worden uitgevoerd

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="interpreting-the-cluster-autoscaler-status"></a>Interpreteren van de status van het cluster automatisch schalen

```console
$ kubectl -n kube-system describe configmap cluster-autoscaler-status
Name:         cluster-autoscaler-status
Namespace:    kube-system
Labels:       <none>
Annotations:  cluster-autoscaler.kubernetes.io/last-updated=2018-07-25 22:59:22.661669494 +0000 UTC

Data
====
status:
----
Cluster-autoscaler status at 2018-07-25 22:59:22.661669494 +0000 UTC:
Cluster-wide:
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 registered=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC

NodeGroups:
  Name:        nodepool1
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0 cloudProviderTarget=1 (minSize=1, maxSize=5))
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 cloudProviderTarget=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC


Events:  <none>
```

De status van het cluster automatisch schalen kunt u de status van het cluster automatisch schalen op twee verschillende niveaus: brede, door het cluster en binnen de groep van elk knooppunt. Aangezien AKS momenteel alleen toepassingen met één knooppunt ondersteunt, wordt deze metrische gegevens zijn hetzelfde.

* Status geeft aan dat de algehele status van de knooppunten. Als het cluster automatisch schalen strijd om te maken of knooppunten in het cluster verwijdert, wordt deze status wordt gewijzigd in 'Niet in orde'. Er is ook een uitsplitsing van de status van andere knooppunten:
    * 'Gereed' betekent dat een knooppunt is een kan worden gepland op het gehele product hebben.
    * 'Unready' betekent dat een knooppunt dat defect is geraakt nadat deze is gestart.
    * 'NotStarted' betekent dat een knooppunt volledig nog niet is gestart.
    * 'LongNotStarted' betekent dat een knooppunt kan niet worden gestart binnen een redelijke termijn.
    * "Geregistreerde betekent dat die een knooppunt wordt geregistreerd in de groep
    * 'Niet-geregistreerde' betekent dat een knooppunt aan de kant van de provider cluster aanwezig is, maar is niet geregistreerd in Kubernetes.
  
* ScaleUp kunt u om te controleren wanneer het cluster bepaalt dat een omhoog schalen in uw cluster moet worden uitgevoerd.
    * Een overgang is wanneer het aantal knooppunten in het cluster wordt gewijzigd of de status van een knooppunt wordt gewijzigd.
    * Het aantal knooppunten dat gereed is het aantal knooppunten en dat deze klaar in het cluster. 
    * De cloudProviderTarget is het aantal knooppunten die het cluster automatisch schalen heeft vastgesteld dat het cluster nodig heeft voor het afhandelen van de werkbelasting.

* ScaleDown kunt u controleren of er kandidaten voor schaal omlaag. 
    * Een kandidaat voor omlaag schalen is een knooppunt die heeft vastgesteld dat het cluster automatisch schalen kan worden verwijderd zonder mogelijkheid voor het afhandelen van de belasting van het cluster. 
    * De tijden die zijn opgegeven weergeven de laatste keer dat het cluster is ingeschakeld voor omlaag kandidaten schalen en de laatste overgangstijd.

Ten slotte onder gebeurtenissen, kunt u zien van elke schaal of verkleinen van gebeurtenissen, mislukte of geslaagde, en hun tijden dat het cluster automatisch schalen is uitgevoerd.

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
