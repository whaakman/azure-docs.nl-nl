---
title: Operator aanbevolen procedures - verbinding met het netwerk in Azure Kubernetes Services (AKS)
description: Meer over de best practices uit de cluster-operator voor virtuele-netwerkbronnen en connectiviteit in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: iainfou
ms.openlocfilehash: 680e3990afa3ed08c69402e9e5403cb9a6f3266a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175452"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor verbinding met het netwerk en beveiliging in Azure Kubernetes Service (AKS)

Bij het maken en beheren van clusters in Azure Kubernetes Service (AKS), kunt u verbinding met het netwerk opgeven voor uw knooppunten en toepassingen. Deze netwerkbronnen omvatten IP-adresbereiken, load balancers en ingress-controllers. Als u wilt een hoge kwaliteit van de service voor uw toepassingen behouden, moet u plannen en configureert u deze resources.

Deze aanbevolen procedures voor richt zich op de verbinding met het netwerk en beveiliging voor clusteroperators. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Vergelijk de kubenet en Azure CNI netwerk modi in AKS
> * Plannen voor de vereiste IP-adressering en connectiviteit
> * Distribueren van netwerkverkeer met load balancers, controllers voor inkomend verkeer of een web application Firewall (WAF)
> * Veilig verbinding maken met de clusterknooppunten

## <a name="choose-the-appropriate-network-model"></a>Kies het juiste netwerk-model

**Aanbevolen procedurerichtlijn** : voor integratie met bestaande virtuele netwerken of on-premises netwerken, Azure CNI netwerken gebruiken in AKS. Dit model netwerk kunt ook meer scheiding van resources en besturingselementen in een bedrijfsomgeving.

Virtuele netwerken bieden de basisconnectiviteit voor AKS-knooppunten en klanten toegang krijgen tot uw toepassingen. Er zijn twee verschillende manieren het implementeren van clusters van AKS in virtuele netwerken:

* **Kubenet netwerken** -Azure beheert de resources van het virtuele netwerk als het cluster is geïmplementeerd en maakt gebruik van de [kubenet] [ kubenet] Kubernetes-invoegtoepassing.
* **Netwerken van Azure CNI** - implementeert in een bestaand virtueel netwerk en maakt gebruik van de [Azure Container netwerken Interface (CNI)] [ cni-networking] Kubernetes-invoegtoepassing. Schillen ontvangen afzonderlijke IP-adressen die naar andere netwerkservices of on-premises bronnen doorsturen kunt.

De Container netwerken Interface (CNI) is een neutrale-protocol waarmee de container-runtime-aanvragen versturen naar de provider van een netwerk. De Azure CNI IP-adressen toewijst aan schillen en knooppunten, en biedt IP-adres management (IPAM)-functies als u verbinding met bestaande virtuele Azure-netwerken maken. Elke resource-knooppunt en pod ontvangt een IP-adres in de Azure-netwerk en geen extra routering is nodig om te communiceren met andere resources of services.

![Diagram van twee knooppunten met bruggen, elk met een enkel Azure-VNet verbinding maken](media/operator-best-practices-network/advanced-networking-diagram.png)

Voor de meeste implementaties van de productie, moet u Azure CNI netwerken. Dit model netwerk kunnen voor scheiding van controle en beheer van resources. Vanuit het beveiligingsoogpunt wilt u vaak verschillende teams beheren en beveiligen van deze resources. Azure CNI netwerken kunt die u verbinding met bestaande Azure-resources, on-premises bronnen of andere services rechtstreeks via IP-adressen toegewezen aan elke schil maken.

Wanneer u Azure CNI netwerken gebruikt, is de VM-resource in een afzonderlijke resource die u wilt het AKS-cluster. Overdragen van machtigingen voor de AKS-service-principal te openen en beheren van deze resources. De service-principal die worden gebruikt door het AKS-cluster moet minimaal beschikken over [Inzender voor netwerken](../role-based-access-control/built-in-roles.md#network-contributor) machtigingen op het subnet binnen uw virtuele netwerk. Als u wilt definiëren een [aangepaste rol](../role-based-access-control/custom-roles.md) in plaats van de ingebouwde rol van inzender voor netwerken, de volgende machtigingen zijn vereist:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

Zie voor meer informatie over AKS-service-principal overdracht [toegang tot andere Azure-resources delegeren][sp-delegation].

Als elk knooppunt en een schil ontvangt een eigen IP-adres, plant de adresbereiken voor de AKS-subnetten. Het subnet moet groot genoeg is voor IP-adressen voor elk knooppunt, schillen en netwerkbronnen die u implementeert. Een AKS-cluster moet worden geplaatst in een eigen subnet. Als u wilt toestaan dat de verbinding met on-premises of gekoppelde netwerken in Azure, gebruik geen IP-adresbereiken die overlappen met bestaande netwerkbronnen. Er gelden standaard beperkingen voor het aantal schillen dat elk knooppunt wordt uitgevoerd met kubenet zowel Azure CNI netwerken. Voor het afhandelen van schaal van gebeurtenissen of upgraden van clusters, moet u ook extra IP-adressen beschikbaar voor gebruik in het toegewezen subnet.

Voor het berekenen van het IP-adres vereist, Zie [netwerken van Azure CNI configureren in AKS][advanced-networking].

### <a name="kubenet-networking"></a>Kubenet netwerken

Hoewel kubenet, u hoeft voor het instellen van de virtuele netwerken voordat het cluster wordt geïmplementeerd, kunnen er zijn nadelen:

* Knooppunten en schillen worden geplaatst op verschillende IP-subnetten. De gebruiker gedefinieerde routering (UDR) en IP-doorsturen wordt gebruikt voor het routeren van verkeer tussen schillen en knooppunten. Deze extra routering kan de prestaties verminderen.
* Verbindingen met bestaande on-premises netwerken of andere Azure-netwerk-peering kunnen lastig zijn.

Kubenet is geschikt voor kleine ontwikkelings- of -werkbelastingen, zoals u hoeft te maken van het virtuele netwerk en subnetten afzonderlijk van het AKS-cluster. Eenvoudige websites met weinig verkeer, of tot het lift- and -shift-workloads in containers, kunnen ook profiteren van de eenvoud van AKS-clusters die zijn geïmplementeerd met kubenet netwerken. Voor de meeste implementaties van de productie, moet u plannen en gebruiken van Azure CNI netwerken. U kunt ook [configureren van uw eigen IP-adresbereiken en virtuele netwerken met behulp van kubenet][aks-configure-kubenet-networking].

## <a name="distribute-ingress-traffic"></a>Inkomend verkeer verdelen

**Aanbevolen procedurerichtlijn** : als u wilt distribueren HTTP of HTTPS-verkeer naar uw toepassingen, gebruikt u inkomend verkeer resources en domeincontrollers. Inkomend verkeer controllers om extra functies te bieden via een gewone Azure load balancer en kunnen worden beheerd als systeemeigen Kubernetes-resources.

Een Azure load balancer klantenverkeer naar toepassingen in uw AKS-cluster kunt distribueren, maar deze beperkt in wat er van dat verkeer begrijpt. Een load balancer-resource werkt op laag 4 en verdeelt het verkeer op basis van het protocol of -poorten. De meeste webtoepassingen die gebruikmaken van HTTP of HTTPS moeten Kuberenetes inkomend resources en controllers, die op laag 7 werkt gebruiken. Inkomend verkeer kunt distribueren van verkeer op basis van de URL van de toepassing en verwerk TLS/SSL-beëindiging. Deze mogelijkheid vermindert ook het aantal IP-adressen die u beschikbaar maken en toewijzen. Met een load balancer moet elke toepassing doorgaans een openbaar IP-adres toegewezen en toegewezen aan de service in het AKS-cluster. Met een resource met inkomend verkeer, kunt één IP-adres distribueren van verkeer naar meerdere toepassingen.

![Diagram van inkomend netwerkverkeer in een AKS-cluster](media/operator-best-practices-network/aks-ingress.png)

 Er zijn twee onderdelen voor inkomende gegevens:

 * Een inkomend *resource*, en
 * Een inkomend *controller*

De resource met inkomend verkeer is een YAML-manifest van `kind: Ingress` die de host, certificaten en regels definieert om verkeer te routeren voor services die worden uitgevoerd in uw AKS-cluster. Het volgende voorbeeld YAML-manifest zou distribueren van verkeer voor *myapp.com* op een van twee services *blogservice* of *storeservice*. De klant wordt omgeleid naar een service of de andere op basis van de URL die ze toegang krijgen tot.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

Een controller voor binnenkomend verkeer is een daemon uit die wordt uitgevoerd op een AKS-knooppunt en controleert op binnenkomende aanvragen. Verkeer wordt dan gedistribueerd op basis van de regels die zijn gedefinieerd in de resource met inkomend verkeer. De meest voorkomende controller voor binnenkomend verkeer is gebaseerd op [NGINX]. AKS niet beperkt u naar een specifieke domeincontroller, zodat u andere domeincontrollers, zoals kunt [Contour][contour], [HAProxy][haproxy], of [ Traefik][traefik].

Er zijn veel scenario's voor inkomend verkeer, met inbegrip van de volgende handleidingen:

* [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
* [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
* [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
* Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Verkeer met een web application firewall (WAF) beveiligen

**Aanbevolen procedurerichtlijn** : als u wilt scannen van inkomend verkeer voor potentiële aanvallen, gebruikt u een web application firewall (WAF), zoals [Barracuda WAF voor Azure] [ barracuda-waf] of Azure Application Gateway. Deze geavanceerde netwerkbronnen kunnen ook worden gerouteerd verkeer dan alleen HTTP en HTTPS-verbindingen of basic SSL-beëindiging.

Een controller voor binnenkomend verkeer dat wordt gedistribueerd naar de services en toepassingen is meestal een Kubernetes-bron in uw AKS-cluster. De controller wordt uitgevoerd als een daemon op een AKS-knooppunt, en sommige van de resources van het knooppunt, zoals CPU, geheugen en netwerkbandbreedte verbruikt. In grotere omgevingen wilt u meestal voor de offload van sommige van deze routering van verkeer of TLS-beëindiging met een netwerkbron buiten het AKS-cluster. U wilt dat ook voor het scannen van inkomend verkeer voor potentiële aanvallen.

![Een web application firewall (WAF), zoals Azure App Gateway kan beveiligen en distribueren van verkeer voor uw AKS-cluster](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Een web application firewall (WAF) biedt een extra laag van beveiliging door het filteren van het binnenkomende verkeer. De Open Web Application Security Project (OWASP) biedt een set regels om te bekijken van aanvallen zoals cross-site scripting of cookie aanvallen. [Azure Application Gateway] [ app-gateway] is een WAF die kan worden geïntegreerd met AKS-clusters voor deze beveiligingsfuncties, voordat het verkeer uw AKS-cluster en toepassingen bereikt. Andere oplossingen van derden ook uitvoeren deze functies, zodat u kunt echter ook doorgaan met het bestaande investeringen expertise in een bepaald product.

Load balancer of ingress resources blijven om uit te voeren in uw AKS-cluster om de distributie van verkeer verder te verfijnen. App-Gateway kan centraal worden beheerd als een controller voor binnenkomend verkeer met een resourcedefinitie. Aan de slag [maken van een Application Gateway-ingangscontroller][app-gateway-ingress].

## <a name="control-traffic-flow-with-network-policies"></a>Controlestroom-verkeer met beleid voor netwerken

**Aanbevolen procedurerichtlijn** -netwerkbeleid wilt toestaan of weigeren van verkeer naar schillen gebruiken. Standaard wordt al het verkeer toegestaan tussen schillen binnen een cluster. Definieert de regels die pod communicatie beperken voor verbeterde beveiliging.

Beleid voor netwerken is een Kubernetes-functie waarmee u kunt de verkeersstroom tussen schillen beheren. U kunt toestaan of weigeren van verkeer op basis van de labels van de instellingen zoals die zijn toegewezen, naamruimte of verkeer poort. Het gebruik van netwerkbeleid profiteert van een cloud-eigen methode voor het beheren van de stroom van het verkeer. Nadat er schillen zijn dynamisch gemaakt in een AKS-cluster, kunnen het vereiste netwerkbeleid automatisch worden toegepast. Gebruik geen beveiligingsgroepen van Azure-netwerk wilt pod-pod-verkeer beheren, gebruikt u netwerkbeleid.

Als u wilt gebruiken, kan de functie moet worden ingeschakeld wanneer u een AKS-cluster maakt. U kunt beleid voor netwerken in een bestaand AKS-cluster niet inschakelen. Plan vooruit om ervoor te zorgen dat u netwerkbeleid op clusters inschakelen en deze indien nodig kunt gebruiken.

Een beleid voor netwerken wordt gemaakt als een Kubernetes-resource met behulp van een YAML-manifest. Het beleid wordt toegepast op de gedefinieerde schillen en vervolgens regels voor binnenkomende of uitgaande bepalen hoe het verkeer kan stromen. Het volgende voorbeeld wordt een beleid voor netwerken voor schillen met de *app: back-end* label toegepast. De regel voor inkomend verkeer vervolgens alleen verkeer toestaat van schillen met de *app: frontend* label:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

Als u wilt aan de slag met beleid, Zie [beveiliging van verkeer tussen schillen met behulp van beleid voor netwerken in Azure Kubernetes Service (AKS)][use-network-policies].

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Veilig verbinding maken met knooppunten via een bastionhost

**Aanbevolen procedurerichtlijn** -geven geen externe verbindingen naar uw AKS-knooppunten. Maken van een bastionhost, of in een virtueel netwerk management vak gaan. Gebruik de bastionhost veilig om verkeer te routeren naar uw AKS-cluster aan taken van extern beheer.

De meeste bewerkingen in AKS kunnen worden uitgevoerd met behulp van de Azure-beheerhulpprogramma's of via de Kubernetes API-server. AKS-knooppunten zijn niet verbonden met het openbare internet, en zijn alleen beschikbaar in een particulier netwerk. Als u verbinding maken met knooppunten en onderhoud uit te voeren of oplossen van problemen, uw verbindingen via een bastionhost routeren of jump-box. Deze host moet zich in een virtueel netwerk van afzonderlijke management die veilig is gekoppeld aan het virtuele netwerk voor AKS-cluster.

![Verbinding maken met AKS-knooppunten met behulp van een bastionhost, of vak gaan](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Het beheernetwerk voor de bastionhost moet worden beveiligd, te. Gebruik een [Azure ExpressRoute] [ expressroute] of [VPN-gateway] [ vpn-gateway] verbinding maken met een on-premises netwerk en beheren van toegang met behulp van netwerkbeveiliging groepen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel gericht op de verbinding met het netwerk en beveiliging. Zie voor meer informatie over de basisprincipes van het netwerk in Kubernetes, [netwerk concepten voor toepassingen in Azure Kubernetes Service (AKS)][aks-concepts-network]

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[use-network-policies]: use-network-policies.md
[advanced-networking]: configure-azure-cni.md
[aks-configure-kubenet-networking]: configure-kubenet.md
