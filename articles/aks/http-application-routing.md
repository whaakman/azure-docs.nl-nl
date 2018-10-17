---
title: HTTP-aanvraag routering invoegtoepassing in Azure Kubernetes Service (AKS)
description: De invoegtoepassing van HTTP-toepassing-Routering in Azure Kubernetes Service (AKS) gebruiken.
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2018
ms.author: laevenso
ms.openlocfilehash: 9a096588c5a8fda64343e001fdbd895d02153f58
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49362701"
---
# <a name="http-application-routing"></a>Routering van HTTP-toepassing

De HTTP-routering oplossing kunt eenvoudig toegang krijgen tot toepassingen die zijn geïmplementeerd in uw cluster Azure Kubernetes Service (AKS). Wanneer u van de oplossing is ingeschakeld, configureert het een controller voor binnenkomend verkeer in uw AKS-cluster. Als toepassingen worden geïmplementeerd, maakt de oplossing ook openbaar toegankelijke DNS-namen voor eindpunten van de toepassingen.

Wanneer de invoegtoepassing is ingeschakeld, wordt een DNS-Zone gemaakt in uw abonnement. Zie voor meer informatie over DNS-kosten [DNS prijzen][dns-pricing].

> [!CAUTION]
> De HTTP-aanvraag routering invoegtoepassing is ontworpen om u snel een controller voor binnenkomend verkeer kunt maken en toegang tot uw toepassingen te laten. Deze invoegtoepassing wordt niet aanbevolen voor gebruik in productieomgevingen. Voor inkomend verkeer gereed is voor productie-implementaties met meerdere replica's en TLS-ondersteuning, Zie [maken van een HTTPS-controller voor binnenkomend verkeer](https://docs.microsoft.com/en-us/azure/aks/ingress-tls).

## <a name="http-routing-solution-overview"></a>Oplossingenoverzicht van de HTTP-routering

De invoegtoepassing implementeert twee onderdelen: een [controller voor binnenkomend verkeer van Kubernetes] [ ingress] en een [externe DNS] [ external-dns] controller.

- **Controller voor binnenkomend verkeer**: de ingangscontroller wordt blootgesteld aan internet met behulp van een Kubernetes-service van het type Load Balancer. De controller voor binnenkomend verkeer controleert en implementeert [inkomend verkeer van Kubernetes-resources][ingress-resource], waarmee routes naar de toepassingseindpunten wordt gemaakt.
- **Externe DNS-controller**: controleert op Kubernetes inkomend resources en DNS A-records gemaakt in de cluster-specifieke DNS-zone.

## <a name="deploy-http-routing-cli"></a>Implementeren van HTTP-routering: CLI

De HTTP-aanvraag routering invoegtoepassing kan worden ingeschakeld met de Azure CLI bij het implementeren van een AKS-cluster. Om dit te doen, gebruikt u de [az aks maken] [ az-aks-create] opdracht met de `--enable-addons` argument.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

U kunt ook inschakelen voor HTTP-routering op een bestaand AKS-cluster met de [az aks inschakelen--invoegtoepassingen] [ az-aks-enable-addons] opdracht. Toevoegen als u wilt inschakelen op http-routering op een bestaand cluster, de `--addons` parameter en geeft u *http_application_routing* zoals wordt weergegeven in het volgende voorbeeld:

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

Nadat het cluster wordt geïmplementeerd of bijgewerkt, gebruikt u de [az aks show] [ az-aks-show] opdracht voor het ophalen van de naam van de DNS-zone. Deze naam is vereist voor het implementeren van toepassingen voor het AKS-cluster.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

Result
-----------------------------------------------------
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>Implementeren van HTTP-routering: Portal

De HTTP-aanvraag routering invoegtoepassing kan worden ingeschakeld via de Azure-portal bij het implementeren van een AKS-cluster.

![De HTTP-routering inschakelen](media/http-routing/create.png)

Nadat het cluster is geïmplementeerd, blader naar de resourcegroep automatisch is gemaakt voor AKS en selecteert u de DNS-zone. Noteer de naam van de DNS-zone. Deze naam is vereist voor het implementeren van toepassingen voor het AKS-cluster.

![De naam van de DNS-zone ophalen](media/http-routing/dns.png)

## <a name="use-http-routing"></a>Gebruik van HTTP-routering

De oplossing voor het routering van HTTP-toepassingen kan alleen worden geactiveerd voor inkomend verkeer bronnen die zijn gemarkeerd als volgt:

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Maak een bestand met de naam **voorbeelden-http-aanvraag-routing.yaml** en kopieer de volgende YAML. Update op regel 43 `<CLUSTER_SPECIFIC_DNS_ZONE>` verzameld met de naam van de DNS-zone in de vorige stap in dit artikel.


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: party-clippy
spec:
  template:
    metadata:
      labels:
        app: party-clippy
    spec:
      containers:
      - image: r.j3ss.co/party-clippy
        name: party-clippy
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        tty: true
        command: ["party-clippy"]
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: party-clippy
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: party-clippy
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: party-clippy
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: party-clippy.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: party-clippy
          servicePort: 80
        path: /
```

Gebruik de [kubectl toepassen] [ kubectl-apply] opdracht om de resources te maken.

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment "party-clippy" created
service "party-clippy" created
ingress "party-clippy" created
```

CURL of een browser gebruiken om te navigeren naar de hostnaam die is opgegeven in de sectie host van de voorbeelden-http-aanvraag-routing.yaml-bestand. De toepassing duurt maximaal één minuut voordat deze beschikbaar zijn via internet.

```bash
$ curl party-clippy.471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io

 _________________________________
/ It looks like you're building a \
\ microservice.                   /
 ---------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/

```

## <a name="remove-http-routing"></a>Verwijderen van de HTTP-routering

De HTTP-routering oplossing kan worden verwijderd met de Azure CLI. Voer de volgende opdracht uit om dit te doen, vervangen door uw AKS-cluster en resource groepsnaam.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

## <a name="troubleshoot"></a>Problemen oplossen

Gebruik de [kubectl logboeken] [ kubectl-logs] opdracht om de toepassingslogboeken voor de externe DNS-toepassing weer te geven. De logboeken te bevestigen dat een A- en DNS TXT-record zijn gemaakt.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'party-clippy' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'party-clippy' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Deze records kunnen ook worden weergegeven op de bron van de DNS-zone in Azure portal.

![De DNS-records ophalen](media/http-routing/clippy.png)

Gebruik de [kubectl logboeken] [ kubectl-logs] opdracht om de toepassingslogboeken voor de Nginx-ingangscontroller weer te geven. De logboeken te bevestigen de `CREATE` van een resource inkomend verkeer en het opnieuw laden van de controller. Alle HTTP-activiteit wordt geregistreerd.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"party-clippy", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/party-clippy
W0426 21:51:57.908771       9 controller.go:775] service default/party-clippy does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-party-clippy-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Opruimen

Verwijder de gekoppelde Kubernetes-objecten in dit artikel hebt gemaakt.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "party-clippy" deleted
service "party-clippy" deleted
ingress "party-clippy" deleted
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het installeren van een HTTPS-beveiligde ingangscontroller in AKS [HTTPS inkomend verkeer op Azure Kubernetes Service (AKS)][ingress-https].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons


<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
