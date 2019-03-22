---
title: Concepten - netwerken in Azure Kubernetes-Services (AKS)
description: Meer informatie over netwerken in Azure Kubernetes Service (AKS), met inbegrip van kubenet en Azure CNI netwerken, inkomend domeincontrollers, load balancers en statische IP-adressen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: cbdbf7dcd6269991d23c61d316dcee68e6678171
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58175663"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Netwerkconcepten voor toepassingen in Azure Kubernetes Service (AKS)

In een container op basis van microservices-benadering voor de ontwikkeling van toepassingen, moeten onderdelen van de toepassing werken samen om hun taken te verwerken. Kubernetes biedt verschillende bronnen waarmee deze communicatie van de toepassing. U kunt verbinding maken met en toepassingen intern of extern beschikbaar te maken. Voor het bouwen van toepassingen met hoge beschikbaarheid, u kunt laden saldo van uw toepassingen. Complexere toepassingen mogelijk configuratie van inkomend verkeer vereist voor SSL/TLS-beëindiging of de routering van meerdere onderdelen. Uit veiligheidsoverwegingen moet u mogelijk ook beperken van de stroom van het netwerkverkeer naar of tussen schillen en knooppunten.

In dit artikel worden de belangrijkste concepten waarmee u toegang tot uw toepassingen in AKS geïntroduceerd:

- [Services](#services)
- [Virtuele netwerken van Azure](#azure-virtual-networks)
- [Inkomend verkeer domeincontrollers](#ingress-controllers)
- [Beleid voor netwerken](#network-policies)

## <a name="kubernetes-basics"></a>Basisbeginselen voor Kubernetes

Kubernetes biedt voor toegang tot uw toepassingen, of voor onderdelen van de toepassing om te communiceren met elkaar, een abstractielaag voor virtuele netwerken. Kubernetes-knooppunten zijn verbonden met een virtueel netwerk en krijgt u inkomende en uitgaande connectiviteit voor schillen. De *kube-proxy* onderdeel wordt uitgevoerd op elk knooppunt voor deze functies het netwerk.

In Kubernetes, *Services* schillen om toe te staan voor directe toegang via een IP-adres of de DNS-naam en op een specifieke poort, logisch groeperen. U kunt ook verdelen verkeer met behulp van een *netwerktaakverdeler*. Meer complexe routering van toepassingsverkeer kan ook worden bereikt met *inkomend Controllers*. Beveiliging en het filteren van het netwerkverkeer voor schillen er mogelijk is met Kubernetes *netwerkbeleidsregels* (in preview in AKS).

Het Azure-platform helpt ook bij voor het vereenvoudigen van virtuele netwerken voor AKS-clusters. Wanneer u een Kubernetes-load balancer maakt, wordt de onderliggende Azure-load balancer-resource gemaakt en geconfigureerd. Als u netwerkpoorten schillen opent, wordt de bijbehorende Azure regels voor netwerkbeveiligingsgroepen zijn geconfigureerd. Voor het HTTP-toepassingsroutering, Azure ook configureren *externe DNS-server* als nieuwe ingang routes zijn geconfigureerd.

## <a name="services"></a>Services

Ter vereenvoudiging van de netwerkconfiguratie voor werkbelastingen van toepassingen, Kubernetes gebruikt *Services* logisch groeperen van een set schillen en verbinding met het netwerk. De volgende typen services zijn beschikbaar:

- **Cluster-IP** -maakt een interne IP-adres voor gebruik binnen het AKS-cluster. Goed voor intern gebruik bestemde toepassingen die ondersteuning bieden voor andere werkbelastingen binnen het cluster.

    ![Diagram van Cluster-IP-netwerkverkeer in een AKS-cluster][aks-clusterip]

- **NodePort** -een-toewijzing gemaakt op het onderliggende knooppunt dat kan de toepassing rechtstreeks via het knooppunt IP-adres en de poort worden geopend.

    ![Diagram van de verkeersstroom NodePort in een AKS-cluster][aks-nodeport]

- **Load Balancer** : een Azure load balancer-resource maakt, configureert u een extern IP-adres en de aangevraagde schillen verbindt met de back-endpool van de load balancer. Bereik van de toepassing, load balancer-regels worden zodat klanten verkeer naar gemaakt op de gewenste poorten. 

    ![Diagram van netwerkverkeer van de Load Balancer in een AKS-cluster][aks-loadbalancer]

    Voor extra controle- en routering van het inkomende verkeer u in plaats daarvan mag gebruiken een [controller voor binnenkomend verkeer](#ingress-controllers).

- **ExternalName** -maakt een specifieke DNS-vermelding voor een eenvoudigere toegang van toepassing.

Het IP-adres voor load balancers en services kan dynamisch worden toegewezen of kunt u een bestaand statisch IP-adres te gebruiken. Zowel interne als externe statische IP-adressen kunnen worden toegewezen. Deze bestaande statisch IP-adres is vaak gekoppeld aan een DNS-vermelding.

Beide *interne* en *externe* load balancers kunnen worden gemaakt. Interne load balancers worden alleen toegewezen aan een privé IP-adres, zodat kan niet worden geopend via Internet.

## <a name="azure-virtual-networks"></a>Virtuele netwerken van Azure

In AKS implementeert u een cluster dat gebruik maakt van een van de volgende twee modellen:

- *Kubenet* networking - het netwerk resources worden meestal gemaakt en geconfigureerd, zoals het AKS-cluster is geïmplementeerd.
- *Azure Container netwerken Interface (CNI)* networking - de AKS-cluster is verbonden met een bestaande virtuele-netwerkbronnen en configuraties.

### <a name="kubenet-basic-networking"></a>Netwerken Kubenet (basis)

De *kubenet* optie netwerken is de standaardconfiguratie voor het AKS-cluster maken. Met *kubenet*, knooppunten een IP-adres ophalen uit het subnet van de Azure-netwerk. Schillen ontvangen een IP-adres van een logisch verschillende adresruimte op het subnet van de Azure-netwerk van de knooppunten. Netwerkadresomzetting (NAT) wordt vervolgens geconfigureerd zodat de schillen bronnen op het Azure-netwerk kunnen bereiken. De bron-IP-adres van het verkeer is dat NAT wilt aan de primaire IP-adres van het knooppunt oplossen.

Knooppunten maken gebruik van de [kubenet] [ kubenet] Kubernetes-invoegtoepassing. U kunt het Azure-platform maken en configureren van de virtuele netwerken voor u of uw AKS-cluster in een bestaand virtueel netwerksubnet implementeren. Nogmaals, alleen de knooppunten ontvangen een routeerbaar IP-adres en de schillen NAT gebruiken om te communiceren met andere resources buiten het AKS-cluster. Deze aanpak vermindert aanzienlijk het aantal IP-adressen die u reserveren in de adresruimte van uw netwerk voor pods wilt te gebruiken.

Zie voor meer informatie, [kubenet netwerken voor een AKS-cluster configureren][aks-configure-kubenet-networking].

### <a name="azure-cni-advanced-networking"></a>Azure CNI (Geavanceerd)-netwerken

Met Azure CNI, elke pod krijgt een IP-adres van het subnet en kan rechtstreeks worden geopend. Deze IP-adressen moeten uniek zijn in de adresruimte van uw netwerk, en moeten vooraf worden gepland. Elk knooppunt heeft een configuratieparameter voor het maximum aantal schillen worden ondersteund. Het equivalente aantal IP-adressen per knooppunt worden vervolgens een gereserveerd voor dat knooppunt. Deze aanpak vereist meer planning en leidt vaak tot uitputting van IP-adres of de noodzaak voor het opnieuw opbouwen van clusters in een groter subnet aan de vereisten van uw toepassing.

Knooppunten maken gebruik van de [Azure Container netwerken Interface (CNI)] [ cni-networking] Kubernetes-invoegtoepassing.

![Diagram van twee knooppunten met bruggen, elk met een enkel Azure-VNet verbinding maken][advanced-networking-diagram]

Azure CNI biedt de volgende functies ten opzichte van kubenet netwerken:

- Elke pod in het cluster is een IP-adres in het virtuele netwerk worden toegewezen. De pods die kunnen rechtstreeks communiceren met andere schillen in het cluster en andere knooppunten in het virtuele netwerk.
- Schillen in een subnet waarvoor service-eindpunten ingeschakeld kunnen veilig verbinding maken met Azure-services, zoals Azure Storage en SQL-database.
- U kunt de gebruiker gedefinieerde routes (UDR) voor het routeren van verkeer van schillen met een virtueel netwerkapparaat maken.

Zie voor meer informatie, [Azure CNI configureren voor een AKS-cluster][aks-configure-advanced-networking].

## <a name="ingress-controllers"></a>Inkomend verkeer domeincontrollers

Wanneer u een Load Balancer-type Service maakt, wordt een onderliggende Azure-load balancer-resource gemaakt. De load balancer is geconfigureerd voor het distribueren van verkeer naar de schillen in uw Service op een opgegeven poort. De Load Balancer alleen werkt op laag 4: de Service is niet op de hoogte van de werkelijke toepassingen, en eventuele aanvullende overwegingen voor de routering niet maken.

*Inkomend verkeer controllers* werkt op laag 7 en intelligentere regels kunt gebruiken voor het distribueren van toepassingsverkeer. Een algemene gebruik van een controller voor binnenkomend verkeer is voor het routeren van HTTP-verkeer naar verschillende toepassingen op basis van de binnenkomende URL.

![Diagram van inkomend netwerkverkeer in een AKS-cluster][aks-ingress]

U kunt in AKS, een inkomend-resource, bijvoorbeeld met NGINX maken of gebruiken van de functie AKS HTTP-routering. Wanneer u inschakelt dat HTTP-toepassingsroutering voor een AKS-cluster, het Azure-platform wordt gemaakt van de controller voor binnenkomend verkeer en een *externe DNS* controller. Nadat er nieuwe resources voor inkomend verkeer zijn gemaakt in Kubernetes, worden de vereiste DNS A-records gemaakt in een cluster-specifieke DNS-zone. Zie voor meer informatie, [implementeren HTTP-toepassingsroutering][aks-http-routing].

Een andere algemene functie van inkomend verkeer is SSL/TLS-beëindiging. Op grote webtoepassingen benaderd via HTTPS, kan het TLS-beëindiging worden verwerkt door de resource met inkomend verkeer in plaats van in de toepassing zelf. U kunt de resource met inkomend verkeer voor het gebruik van providers, zoals we versleutelen configureren voor het automatisch genereren van de TLS-certificering en de configuratie. Zie voor meer informatie over het configureren van een NGINX-ingangscontroller met we versleutelen [inkomend verkeer en TLS][aks-ingress-tls].

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Een netwerkbeveiligingsgroep filtert het verkeer voor virtuele machines, zoals de AKS-knooppunten. Bij het maken van Services, zoals een Load Balancer, configureert het Azure-platform automatisch alle regels voor netwerkbeveiligingsgroepen die nodig zijn. Geen regels voor netwerkbeveiligingsgroepen om te filteren van verkeer voor schillen in een AKS-cluster handmatig configureren. Alle vereiste poorten en doorsturen via definiëren als onderdeel van de manifesten voor uw Kubernetes-Service en laat het Azure-platform maken of bijwerken van de juiste regels. U kunt ook gebruiken netwerkbeleid, zoals beschreven in de volgende sectie, automatisch toepassen van regels voor netwerkverkeer filteren op schillen.

## <a name="network-policies"></a>Beleid voor netwerken

Standaard kunnen alle schillen in een AKS-cluster verzonden en ontvangen van verkeer zonder beperkingen. Voor betere beveiliging, kunt u regels definiëren die de stroom van het verkeer te regelen. Back-endtoepassingen vaak alleen zichtbaar zijn voor de vereiste front-end-services of databaseonderdelen zijn alleen toegankelijk is voor de toepassingslagen die verbinding met deze maken.

Netwerkbeleid is een Kubernetes-functie die momenteel in preview in AKS waarmee u kunt de verkeersstroom tussen schillen beheren. U kunt toestaan of weigeren van verkeer op basis van de labels van de instellingen zoals die zijn toegewezen, naamruimte of verkeer poort. Netwerkbeveiligingsgroepen zijn meer voor de AKS-knooppunten niet schillen. Het gebruik van beleid voor netwerken is een geschikte, cloud-eigen manier voor het beheren van de stroom van verkeer. Nadat er schillen zijn dynamisch gemaakt in een AKS-cluster, kunnen het vereiste netwerkbeleid automatisch worden toegepast.

Zie voor meer informatie, [beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in Azure Kubernetes Service (AKS)][use-network-policies].

## <a name="next-steps"></a>Volgende stappen

Aan de slag met AKS netwerken, maken en configureren van een AKS-cluster met uw eigen IP-adresbereiken met behulp van [kubenet] [ aks-configure-kubenet-networking] of [Azure CNI] [ aks-configure-advanced-networking].

Zie voor de bijbehorende best practices, [aanbevolen procedures voor verbinding met het netwerk en beveiliging in AKS][operator-best-practices-network].

Zie de volgende artikelen voor meer informatie over core Kubernetes en concepten voor AKS:

- [Kubernetes / AKS-clusters en workloads][aks-concepts-clusters-workloads]
- [Kubernetes / AKS toegang en identiteit][aks-concepts-identity]
- [Kubernetes / AKS-beveiliging][aks-concepts-security]
- [Kubernetes / AKS-opslag][aks-concepts-storage]
- [Kubernetes / AKS schalen][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[aks-configure-advanced-networking]: configure-azure-cni.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[use-network-policies]: use-network-policies.md
[operator-best-practices-network]: operator-best-practices-network.md
