---
title: Concepten - basisbeginselen voor Kubernetes voor Azure Kubernetes Services (AKS)
description: Informatie over de basic-cluster en de onderdelen van de werkbelasting van Kubernetes en hoe deze in verband met functies in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: iainfou
ms.openlocfilehash: 1b0b3d0db2067a492905d8f828934f0b63fb8f54
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155980"
---
# <a name="kubernetes-core-concepts-for-azure-kubernetes-service-aks"></a>Kubernetes-belangrijkste concepten voor Azure Kubernetes Service (AKS)

Als de ontwikkeling van toepassingen is verplaatst naar een benadering op basis van een container, wordt de noodzaak om te coördineren en beheren van de onderling verbonden bronnen belangrijk. Kubernetes is het toonaangevende platform waarmee u de mogelijkheid te bieden betrouwbare planning van fouttolerantie toepassingsworkloads. Azure Kubernetes Service (AKS) is een beheerde Kubernetes-aanbieding die verder vereenvoudigt de implementatie van toepassing op basis van een container en beheer.

Dit artikel bevat de basisonderdelen van de Kubernetes-infrastructuur, zoals de *cluster master*, *knooppunten*, en *knooppuntgroepen*. Workload-resources zoals *schillen*, *implementaties*, en *ingesteld* ook geïntroduceerd, samen met het om resources te groeperen in *naamruimten*.

## <a name="what-is-kubernetes"></a>Wat is er Kubernetes?

Kubernetes is een snel groeiende platform dat wordt beheerd op containers gebaseerde toepassingen en hun bijbehorende netwerk- en opslagonderdelen. De focus ligt op de werkbelastingen van toepassingen, niet op de onderliggende infrastructuuronderdelen. Kubernetes biedt een declaratieve benadering voor implementaties, ondersteund door een krachtige reeks API's voor beheerbewerkingen.

U kunt bouwen en uitvoeren van moderne, draagbare, op basis van microservices-toepassingen die profiteren van Kubernetes indelen en beheren van de beschikbaarheid van deze onderdelen van de toepassing. Kubernetes biedt ondersteuning voor staatloze en stateful toepassingen als teams uitgevoerd via de acceptatie van toepassingen op basis van microservices.

Als een open platform kunt met Kubernetes u uw toepassingen met uw favoriete programmeertaal, besturingssysteem, bibliotheken of bus messaging. Bestaande continue integratie en continue levering (CI/CD) hulpprogramma's kunnen integreren met Kubernetes voor het plannen en implementeren van releases.

Azure Kubernetes Service (AKS) biedt een beheerde Kubernetes-service die de complexiteit voor implementatie en core-beheertaken, waaronder upgrades coördineren. De AKS-cluster-modellen worden beheerd door de Azure-platform en u betaalt alleen voor de AKS-knooppunten waarop uw toepassingen worden uitgevoerd. AKS is gebaseerd op de open-source Azure Container Service Engine (acs-engine).

## <a name="kubernetes-cluster-architecture"></a>Architectuur van de Kubernetes-cluster

Een Kubernetes-cluster is onderverdeeld in twee onderdelen:

- *Cluster-master* knooppunten bieden de kernservices van Kubernetes en de indeling van de werkbelastingen van toepassingen.
- *Knooppunten* uw werkbelastingen van toepassingen worden uitgevoerd.

![Kubernetes-cluster hoofd- en knooppunt-onderdelen](media/concepts-clusters-workloads/cluster-master-and-nodes.png)

## <a name="cluster-master"></a>Cluster-model

Wanneer u een AKS-cluster maakt, wordt automatisch een cluster-model gemaakt en geconfigureerd. Dit model van het cluster wordt geleverd als een beheerde Azure-resource van de gebruiker wordt geabstraheerd. Er is geen kosten voor de cluster-master, alleen de knooppunten die deel van het AKS-cluster uitmaken.

Het cluster-model bevat de volgende basisonderdelen voor Kubernetes:

- *kube-apiserver* -server om de API is hoe de onderliggende Kubernetes-API's worden weergegeven. Dit onderdeel biedt de interactie van beheerhulpprogramma's, zoals `kubectl` of het Kubernetes-dashboard.
- *etcd* : als u wilt onderhouden van de status van de Kubernetes-cluster en de configuratie, de maximaal beschikbare *etcd* is een sleutel-waardearchief in Kubernetes.
- *kube-scheduler* : wanneer u maken of schaaltoepassingen, de Scheduler wordt bepaald welke knooppunten de werkbelasting kunnen uitvoeren en deze wordt gestart.
- *kube-controller-manager* -de Controller Manager verantwoordelijk is voor een aantal kleinere domeincontrollers die acties uitvoeren zoals het repliceren van schillen en afhandelen van knooppunt-bewerkingen.

AKS biedt een cluster met één tenant-model, met een eigen API-server, Scheduler, enzovoort. U definieert het aantal en de grootte van de knooppunten en het Azure-platform configureert de veilige communicatie tussen de cluster-model en de knooppunten. Interactie met de cluster-master vindt plaats via Kubernetes APIs, zoals `kubectl` of het Kubernetes-dashboard.

Dit model beheerd cluster betekent dat u niet wilt configureren van onderdelen, zoals een maximaal beschikbare *etcd* store, maar dit betekent ook dat u kan niet rechtstreeks toegang hebben tot de cluster-master. Upgrades naar Kubernetes worden beheerd via de Azure CLI of Azure portal, die de cluster-master en klik vervolgens op de knooppunten wordt bijgewerkt. Voor het oplossen van problemen, kunt u de cluster master logboeken via Azure Log Analytics bekijken.

Als u wilt configureren van de cluster-master op een bepaalde manier of directe toegang nodig tot deze, kunt u uw eigen Kubernetes-cluster via te implementeren [acs-engine][acs-engine].

## <a name="nodes-and-node-pools"></a>Knooppunten en knooppuntgroepen

Als u wilt uitvoeren op uw toepassingen en ondersteunende services, moet u een Kubernetes *knooppunt*. Een AKS-cluster heeft een of meer knooppunten, dit is een Azure virtuele machine (VM) die wordt uitgevoerd van de onderdelen van het knooppunt Kubernetes en container-runtime:

- De `kubelet` is de Kubernetes-agent die de orchestration-aanvragen van het cluster hoofd- en planning van het uitvoeren van de aangevraagde containers worden verwerkt.
- Virtuele netwerken wordt verwerkt door de *kube-proxy* op elk knooppunt. De proxy-routes netwerkverkeer en beheert IP-adressering voor services en schillen.
- De *container runtime* is het onderdeel waarmee toepassingen in containers uitvoeren en communiceren met andere resources, zoals het virtuele netwerk en opslag. Docker is in AKS gebruikt als de container-runtime.

![Virtuele Azure-machine en de ondersteunende resources voor een Kubernetes-knooppunt](media/concepts-clusters-workloads/aks-node-resource-interactions.png)

De Azure VM-grootte voor uw knooppunten definieert het aantal CPU's, hoeveel geheugen en de grootte en hetzelfde type van de opslagruimte beschikbaar is (zoals hoogwaardige SSD of reguliere HDD). Als u verwacht nodig is voor toepassingen waarvoor grote hoeveelheden CPU en geheugen of opslag met hoge prestaties dat, plan u de grootte van het knooppunt dienovereenkomstig. U kunt ook het aantal knooppunten opschalen in uw AKS-cluster om te voldoen aan de vraag.

De VM-installatiekopie voor de knooppunten in het cluster is momenteel in AKS gebaseerd op Ubuntu Linux. Wanneer u een AKS-cluster maken of het aantal knooppunten opschalen, wordt het Azure-platform het aangevraagde aantal virtuele machines maakt en configureert u deze. Er is geen handmatige configuratie voor u om uit te voeren.

Als u nodig hebt voor het gebruik van een andere host-besturingssysteem hebt, container-runtime, of aangepaste pakketten bevatten, kunt u uw eigen Kubernetes-cluster via implementeren [acs-engine][acs-engine]. De upstream `acs-engine` vrijgegeven functies en configuratieopties te leveren voordat ze officieel worden ondersteund in AKS-clusters. Bijvoorbeeld, als u gebruiken van Windows-containers of in een container runtime dan Docker wilt, kunt u `acs-engine` configureren en implementeren van een Kubernetes-cluster die voldoet aan de behoeften van uw huidige.

### <a name="resource-reservations"></a>Resource-reserveringen

U hoeft te beheren, de Kubernetes-basisonderdelen op elk knooppunt, zoals de *kubelet*, *kube-proxy*, en *kube-DNS-*, maar ze enkele van de beschikbare gebruiken COMPUTE-resources. Als u wilt behouden knooppunt prestaties en functionaliteit, zijn de volgende rekenresources gereserveerd op elk knooppunt:

- **CPU** - 60ms
- **Geheugen** -20% maximaal 4 GiB

Deze reserveringen betekenen dat de hoeveelheid beschikbare CPU en geheugen voor uw toepassingen kan worden weergegeven kleiner is dan het knooppunt zelf bevat. Als er resourcebeperkingen vanwege het aantal toepassingen die u uitvoert, wordt deze reserveringen Zorg ervoor dat de CPU en geheugen blijft beschikbaar voor de belangrijkste onderdelen die Kubernetes. De resource-reserveringen kunnen niet worden gewijzigd.

Bijvoorbeeld:

- **Standard DS2 versie 2** knooppuntgrootte bevat 2 vCPU en 7 GiB geheugen
    - 20% van 7 GiB geheugen 1,4 GiB =
    - Totaal *(7-1.4) 5.6 GiB =* geheugen beschikbaar is voor het knooppunt
    
- **Standard E4s v3** knooppuntgrootte bevat 4 vCPU en 32 GiB geheugen
    - 20% van 32 GiB geheugen = 6.4 GiB, maar AKS alleen behoudt zich een maximum van 4 GiB
    - Totaal *(32-4) 28 GiB =* is beschikbaar voor het knooppunt
    
Het onderliggende knooppunt OS vereist ook een zekere mate van CPU en geheugen zijn voor het voltooien van een eigen kernfuncties.

### <a name="node-pools"></a>Knooppuntgroepen

Knooppunten met dezelfde configuratie zijn gegroepeerd tot *knooppuntgroepen*. Een Kubernetes-cluster bevat een of meer groepen. Het oorspronkelijke aantal knooppunten en de grootte worden gedefinieerd bij het maken van een AKS-cluster maakt een *knooppuntgroep standaard*. Deze standaardgroep knooppunt in AKS bevat de onderliggende VM's waarop de agent knooppunten.

Wanneer u een AKS-cluster upgraden of schalen, wordt de actie wordt uitgevoerd op basis van de standaardgroep voor het knooppunt. Actieve containers worden gepland op andere knooppunten in het knooppunt van toepassingen voor upgrade-bewerkingen, totdat alle knooppunten worden bijgewerkt.

## <a name="pods"></a>Schillen

Maakt gebruik van Kubernetes *schillen* naar een exemplaar van uw toepassing wordt uitgevoerd. Een schil vertegenwoordigt één exemplaar van uw toepassing. Schillen hebben doorgaans een 1:1 toewijzing met een container, hoewel er worden geavanceerde scenario's waarbij een schil meerdere containers kan bevatten. Deze schillen meerdere containers samen op hetzelfde knooppunt worden gepland en toestaan dat containers om gerelateerde resources te delen.

Wanneer u een schil maakt, kunt u definiëren *resourcelimieten* om aan te vragen van een bepaalde hoeveelheid resources CPU of geheugen. De Kubernetes Scheduler probeert te plannen van het gehele product uit te voeren op een knooppunt met beschikbare bronnen om te voldoen aan de aanvraag. U kunt ook maximale resourcebeperkingen die verhinderen dat een bepaalde schil verbruikt te veel compute-resource van het onderliggende knooppunt opgeven. Er is een best practice om op te nemen voor alle schillen om te helpen bij de Kubernetes-planner te begrijpen welke resources zijn die nodig zijn en toegestaan.

Zie voor meer informatie, [Kubernetes-schillen] [ kubernetes-pods] en [Kubernetes-schil lifecycle][kubernetes-pod-lifecycle].

Een schil is een logische resource, maar de container (s) zijn waar de werkbelastingen van toepassingen worden uitgevoerd. Schillen zijn doorgaans tijdelijke, beschikbare resources en afzonderlijk geplande schillen missen enkele van de hoge beschikbaarheid en redundantie functies biedt voor Kubernetes. In plaats daarvan schillen meestal worden geïmplementeerd en beheerd door Kubernetes *Controllers*, zoals de implementatie-Controller.

## <a name="deployments-and-yaml-manifests"></a>YAML manifesten en implementaties

Een *implementatie* vertegenwoordigt een of meer identieke schillen, beheerd door de netwerkcontroller Kubernetes-implementatie. Een implementatie definieert het aantal *replica's* (schillen) voor het maken, en de Kubernetes Scheduler zorgt ervoor dat als schillen of knooppunten kunt u problemen ondervindt, extra schillen zijn gepland op knooppunten in orde.

U kunt update-implementaties om de configuratie van pods te wijzigen, container-installatiekopie gebruikt, of opslag die is gekoppeld. De Controller implementatie verkeer naar en een bepaald aantal replica's wordt beëindigd, maakt van replica's van de implementatiedefinitie van de nieuwe en het proces wordt herhaald totdat alle replica's in de implementatie worden bijgewerkt.

De meeste stateless toepassingen in AKS moeten gebruiken voor het implementatiemodel in plaats van afzonderlijke schillen plannen. Kubernetes kunt controleren, de status en de status van implementaties om ervoor te zorgen dat de vereiste aantal replica's in het cluster worden uitgevoerd. Wanneer u alleen afzonderlijke schillen plannen, de schillen zijn niet opnieuw opgestart als ze zich een probleem voordoet, en niet op in orde knooppunten worden gepland als het huidige knooppunt een probleem optreedt.

Als een toepassing een quorum van exemplaren altijd beschikbaar zijn voor management beslissingen worden genomen vereist, kunt u een updateproces verstoren die mogelijkheid niet wilt. *Pod onderbreking budgetten* kan worden gebruikt voor het definiëren van het aantal replica's in een implementatie kunnen alleen worden uitgeschakeld tijdens de upgrade van een update of het knooppunt. Als u hebt bijvoorbeeld *5* replica's in uw implementatie, kunt u een onderbreking van de schil van definiëren *4* om toe te staan slechts één replica wordt verwijderd/gewijzigd op een tijdstip. Met pod resourcebeperkingen is een best practice te definiëren pod onderbreking budgetten voor toepassingen waarvoor een minimum aantal replica's altijd aanwezig zijn.

Implementaties worden meestal gemaakt en beheerd met `kubectl create` of `kubectl apply`. Voor het maken van een implementatie, kunt u een manifestbestand definieert in de indeling YAML (YAML Ain't Markup Language). Het volgende voorbeeld wordt een eenvoudige implementatie van de NGINX-webserver. Hiermee geeft u op de implementatie *3* replica's moet worden gemaakt en die poort *80* zijn geopend voor de container. Resourceaanvragen en -limieten zijn ook gedefinieerd voor de CPU en geheugen.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.2
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

Complexere toepassingen kunnen worden gemaakt door ook services zoals load balancers in het manifest YAML.

Zie voor meer informatie, [Kubernetes-implementaties][kubernetes-deployments].

### <a name="package-management-with-helm"></a>Pakketbeheer met Helm

Een veelgebruikte manier voor het beheren van toepassingen in Kubernetes is met [Helm][helm]. U kunt bouwen en gebruiken van bestaande openbare Helm *grafieken* die bevatten een ingepakte versie van toepassingscode en Kubernetes YAML manifesten voor het implementeren van resources. Deze Helm-grafieken kunnen worden opgeslagen lokaal of vaak in een externe opslagplaats, zoals een [opslagplaats voor Azure Container Registry Helm-grafiek][acr-helm].

Voor het gebruik van Helm, een onderdeel van de server met de naam *Tiller* is geïnstalleerd in uw Kubernetes-cluster. De Tiller beheert de installatie van grafieken binnen het cluster. De Helm-client zichzelf lokaal op uw computer is geïnstalleerd, of kan worden gebruikt binnen de [Azure Cloud Shell][azure-cloud-shell]. U kunt zoeken naar of Helm-grafieken maken met de client en deze vervolgens installeren in uw Kubernetes-cluster.

![Helm bevat een clientonderdeel en een serverzijde Tiller-onderdeel dat wordt gemaakt van de resources in het Kubernetes-cluster](media/concepts-clusters-workloads/use-helm.png)

Zie voor meer informatie, [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][aks-helm].

## <a name="statefulsets-and-daemonsets"></a>StatefulSets en DaemonSets

De Controller implementatie maakt gebruik van de Kubernetes-Scheduler om uit te voeren van een bepaald aantal replica's op een beschikbaar knooppunt met beschikbare bronnen. Deze aanpak van het gebruik van implementaties mogelijk voldoende voor staatloze toepassingen, maar niet voor toepassingen waarvoor een permanente naamconventie of opslag. Voor toepassingen waarvoor een replica aanwezig zijn op elk knooppunt, of de geselecteerde knooppunten binnen een cluster, kijken de implementatie-Controller niet hoe replica's worden verdeeld over de knooppunten.

Er zijn twee Kubernetes-resources waarmee u kunnen deze soorten toepassingen beheren:

- *StatefulSets* -onderhouden van de status van toepassingen buiten de levenscyclus van een afzonderlijke pod, zoals opslag.
- *DaemonSets* -Zorg ervoor dat een actief exemplaar op elk knooppunt, al vroeg in de bootstrap-proces voor Kubernetes.

### <a name="statefulsets"></a>StatefulSets

Ontwikkeling van moderne toepassingen vaak is erop gericht voor staatloze toepassingen, maar *StatefulSets* kan worden gebruikt voor stateful toepassingen, zoals toepassingen die databaseonderdelen bevatten. Een StatefulSet is vergelijkbaar met een implementatie in die een of meer identieke schillen worden gemaakt en beheerd. Replica's in een StatefulSet Volg een correcte, sequentiële benadering te implementeren, schalen, upgrades en afsluitingen. Blijven behouden als replica's opnieuw worden gepland met een StatefulSet, de naamconventie netwerknamen en opslag.

Definieert u de toepassing bij het gebruik van een YAML-indeling `kind: StatefulSet`, en de StatefulSet Controller verwerkt vervolgens de implementatie en het beheer van de vereiste replica's. Gegevens worden geschreven naar de permanente opslag, geleverd door Azure Managed Disks of Azure Files. Met StatefulSets, worden de onderliggende permanente opslag zelfs beschikbaar wanneer de StatefulSet wordt verwijderd.

Zie voor meer informatie, [Kubernetes StatefulSets][kubernetes-statefulsets].

Replica's in een StatefulSet zijn gepland en uitgevoerd op een beschikbaar knooppunt in een AKS-cluster. Als u nodig hebt om ervoor te zorgen dat ten minste één pod in kunt de verzameling op een knooppunt wordt uitgevoerd, u in plaats daarvan een DaemonSet.

### <a name="daemonsets"></a>DaemonSets

Voor specifieke logboekverzameling of behoeften op het gebied, u moet mogelijk een bepaalde schil uitvoeren op alle of geselecteerd, knooppunten. Een *DaemonSet* opnieuw wordt gebruikt voor het implementeren van een of meer identieke schillen, maar de DaemonSet Controller zorgt ervoor dat elk knooppunt opgegeven een exemplaar van de schil.

De DaemonSet Controller kunt plannen dat schillen op knooppunten vroeg in het opstartproces cluster voordat u de standaard Kubernetes Taakplanner is gestart. Deze mogelijkheid zorgt ervoor dat de schillen in een DaemonSet worden gestart voordat de traditionele schillen in een implementatie of StatefulSet zijn gepland.

Zoals StatefulSets, een DaemonSet is gedefinieerd als onderdeel van een YAML-definitie met `kind: DaemonSet`.

Zie voor meer informatie, [Kubernetes DaemonSets][kubernetes-daemonset].

## <a name="namespaces"></a>Naamruimten

Kubernetes-resources, zoals schillen en implementaties, logisch zijn gegroepeerd in een *naamruimte*. Deze groeperingen bieden een manier om logisch delen van een AKS-cluster en beperken de toegang als u wilt maken, weergeven of beheren van resources. U kunt naamruimten bedrijfsonderdelen, bijvoorbeeld afzonderlijke maken. Gebruikers kunnen alleen interactief werken met resources binnen hun toegewezen naamruimten.

![Kubernetes-naamruimten logisch verdeeld bronnen en toepassingen](media/concepts-clusters-workloads/namespaces.png)

Wanneer u een AKS-cluster maakt, zijn de volgende naamruimten zijn beschikbaar:

- *standaard* -deze naamruimte is waarbij schillen en implementaties standaard gemaakt worden wanneer niets wordt opgegeven. U kunt toepassingen rechtstreeks in de standaard-naamruimte in kleinere omgevingen implementeren zonder dat er extra logische ontslag nemen. Wanneer u met de Kubernetes-API, werken zoals met `kubectl get pods`, de standaard-naamruimte wordt gebruikt wanneer niets is opgegeven.
- *kube-systeem* -deze naamruimte is waar kernbronnen bestaat, zoals network-functies, zoals DNS- en proxy, of het Kubernetes-dashboard. U implementeren uw eigen toepassingen in deze naamruimte die doorgaans niet.
- *kube-openbare* : deze naamruimte wordt normaal gesproken niet gebruikt, maar kan worden gebruikt voor resources zichtbaar zijn in het hele cluster, en door gebruikers kan worden weergegeven.

Zie voor meer informatie, [Kubernetes-naamruimten][kubernetes-namespaces].

## <a name="next-steps"></a>Volgende stappen

Dit artikel vindt u enkele van de belangrijkste onderdelen die Kubernetes en hoe ze worden toegepast met AKS-clusters. Zie de volgende artikelen voor meer informatie over core Kubernetes en concepten voor AKS:

- [Kubernetes / AKS toegang en identiteit][aks-concepts-identity]
- [Kubernetes / AKS-beveiliging][aks-concepts-security]
- [Kubernetes / AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes / AKS-opslag][aks-concepts-storage]
- [Kubernetes / AKS schalen][aks-concepts-scale]

<!-- EXTERNAL LINKS -->
[acs-engine]: https://github.com/Azure/acs-engine
[kubernetes-pods]: https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
[kubernetes-pod-lifecycle]: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
[kubernetes-deployments]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-statefulsets]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
[kubernetes-namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[helm]: https://helm.sh/
[azure-cloud-shell]: https://shell.azure.com

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[acr-helm]: ../container-registry/container-registry-helm-repos.md
[aks-helm]: kubernetes-helm.md
