---
title: Concepten - toegang en identiteit in Azure Kubernetes Services (AKS)
description: Meer informatie over toegang en identiteit in Azure Kubernetes Service (AKS), met inbegrip van Azure Active Directory-integratie, Kubernetes-op rollen gebaseerd toegangsbeheer (RBAC), en rollen en -bindingen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: 64150438560d48575b4b65db4cdaf61aa0ada240
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243820"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Opties voor toegang en identiteit voor Azure Kubernetes Service (AKS)

Er zijn verschillende manieren om te verifiëren met en Kubernetes-clusters beveiligen. Met behulp van op rollen gebaseerd toegangsbeheer (RBAC), verleent u gebruikers of groepen toegang tot alleen de resources die ze nodig hebben. Met Azure Kubernetes Service (AKS), kunt u verder verbeteren van de structuur van de beveiliging en machtigingen met behulp van Azure Active Directory. Deze methoden bij het beveiligen van uw werkbelastingen van toepassingen en gegevens van de klant.

In dit artikel worden de belangrijkste concepten die u helpen bij verifiëren en toewijzen van machtigingen in AKS geïntroduceerd:

- [Kubernetes service-accounts](#kubernetes-service-accounts)
- [Azure Active Directory-integratie](#azure-active-directory-integration)
- [Op rollen gebaseerd toegangsbeheer (RBAC)](#role-based-access-controls-rbac)
- [Functies en ClusterRoles](#roles-and-clusterroles)
- [RoleBindings en ClusterRoleBindings](#rolebindings-and-clusterrolebindings)

## <a name="kubernetes-service-accounts"></a>Kubernetes service-accounts

Een van de primaire gebruiker typt in Kubernetes is een *-serviceaccount*. Een service-account bestaat, en wordt beheerd door de Kubernetes-API. De referenties voor service-accounts worden opgeslagen als geheimen voor Kubernetes, waardoor ze moeten worden gebruikt door een geautoriseerde schillen om te communiceren met de API-Server. De meeste API-aanvragen bieden een verificatietoken voor een service-account of een normaal gebruikersaccount.

Normale gebruikersaccounts toestaan meer traditionele toegang voor menselijke beheerders of ontwikkelaars, niet alleen services en processen. Kubernetes zelf biedt geen een oplossing voor identiteitsbeheer waar gewone gebruikersaccounts en wachtwoorden worden opgeslagen. Externe id-oplossingen kunnen in plaats daarvan worden geïntegreerd in Kubernetes. Voor clusters met AKS is deze identiteitsoplossing geïntegreerde Azure Active Directory.

Zie voor meer informatie over de identiteitsopties in Kubernetes [Kubernetes verificatie][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Integratie van Azure Active Directory

De beveiliging van AKS-clusters kan worden uitgebreid met de integratie van Azure Active Directory (AD). Gebouwd op tientallen jaren van enterprise identity management, is Azure AD een multitenant, cloudgebaseerde directory, en identity management-service die belangrijke directoryservices, toegangsbeheer voor toepassingen en Identiteitsbescherming combineert. Met Azure AD, kunt u on-premises identiteiten integreren in AKS-clusters voor één bron voor accountbeheer en beveiliging.

![Azure Active Directory-integratie met AKS-clusters](media/concepts-identity/aad-integration.png)

Met Azure AD geïntegreerde AKS-clusters verleent u gebruikers of groepen toegang tot de Kubernetes-resources binnen een naamruimte of in het cluster. Verkrijgen van een `kubectl` configuratiecontext, een gebruiker kan worden uitgevoerd de [az aks get-credentials] [ az-aks-get-credentials] opdracht. Wanneer een gebruiker vervolgens communiceert met de AKS-cluster met `kubectl`, ze zich aanmelden met hun Azure AD-referenties wordt gevraagd. Deze benadering biedt één bron voor het beheer van gebruikersaccounts en wachtwoord opgeven. De gebruiker kan alleen toegang tot de resources, zoals gedefinieerd door de Clusterbeheerder van de.

Azure AD-verificatie in AKS clusters maakt gebruik van de OpenID Connect, een identiteitslaag is gebaseerd op het OAuth 2.0-protocol. OAuth 2.0 definieert mechanismen voor het verkrijgen en gebruiken van toegangstokens toegang krijgen tot beveiligde bronnen en verificatie als een uitbreiding van het autorisatieproces van OAuth 2.0 OpenID Connect implementeert. Zie voor meer informatie over het OpenID Connect, de [Open ID Connect documentatie][openid-connect]. AKS-clusters gebruiken om te controleren of de verificatietokens ontleend aan het Azure AD via OpenID Connect, Kubernetes Webhook-tokenverificatie. Zie voor meer informatie de [Webhook-tokenverificatie documentatie][webhook-token-docs].

## <a name="role-based-access-controls-rbac"></a>Op rollen gebaseerd toegangsbeheer (RBAC)

Kubernetes maakt gebruik van op rollen gebaseerd toegangsbeheer (RBAC) voor gedetailleerde filteren van de acties die gebruikers kunnen uitvoeren. Dit mechanisme voor toegangsbeheer kunt u gebruikers toe te wijzen, of groepen gebruikers, toestemming voor handelingen zoals maken of wijzigen van resources of weergeven van Logboeken van het uitvoeren van werkbelastingen van toepassingen. Deze machtigingen worden afgestemd op één enkele naamruimte of verleend voor het hele AKS-cluster. Met RBAC Kubernetes, maakt u *rollen* voor het definiëren van machtigingen en vervolgens deze rollen toewijzen aan gebruikers met *rol bindingen*.

Zie voor meer informatie, [met behulp van RBAC-autorisatie][kubernetes-rbac].

### <a name="azure-role-based-access-controls-rbac"></a>Azure op rollen gebaseerd toegangsbeheer (RBAC)
Een aanvullend mechanisme voor het beheren van toegang tot resources is Azure op rollen gebaseerd toegangsbeheer (RBAC). Kubernetes RBAC is ontworpen voor gebruik van resources in uw AKS-cluster en Azure RBAC is ontworpen voor gebruik op resources binnen uw Azure-abonnement. Met Azure RBAC, maakt u een *roldefinitie* die geeft een overzicht van de machtigingen om te worden toegepast. Een gebruiker of groep krijgt vervolgens deze roldefinitie voor een bepaalde *bereik*, die een afzonderlijke resource, een resource, groep of voor het abonnement kan zijn.

Zie voor meer informatie, [wat is Azure RBAC?][azure-rbac]

## <a name="roles-and-clusterroles"></a>Functies en ClusterRoles

Voordat u machtigingen aan gebruikers met Kubernetes RBAC toewijzen, definieert u eerst de juiste machtigingen als een *rol*. Kubernetes-rollen *verlenen* machtigingen. Er is geen concept van een *weigeren* machtiging.

Rollen worden gebruikt voor het verlenen van machtigingen in een naamruimte. Als u nodig hebt om machtigingen te verlenen in het hele cluster of cluster bronnen buiten een bepaalde naamruimte, kunt u in plaats daarvan gebruiken *ClusterRoles*.

Een ClusterRole werkt op dezelfde manier voor het verlenen van machtigingen voor resources, maar kan worden toegepast op resources in het hele cluster, niet een bepaalde naamruimte.

## <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings en ClusterRoleBindings

Nadat rollen zijn gedefinieerd voor het verlenen van machtigingen voor resources, wijst u toe die Kubernetes RBAC-machtigingen met een *RoleBinding*. Als uw AKS-cluster kan worden geïntegreerd met Azure Active Directory, zijn bindingen hoe deze Azure AD-gebruikers zijn gemachtigd om uit te voeren acties binnen het cluster.

Rol bindingen worden gebruikt voor het toewijzen van rollen voor een bepaalde naamruimte. Deze aanpak kunt u een één AKS-cluster, met gebruikers alleen toegang krijgen tot de toepassingsresources in hun toegewezen naamruimte logische manier te scheiden. Als u binden van rollen in het hele cluster of cluster bronnen buiten een bepaalde naamruimte wilt, kunt u in plaats daarvan gebruiken *ClusterRoleBindings*.

Een ClusterRoleBinding werkt op dezelfde manier binden rollen aan gebruikers, maar kan worden toegepast op resources in het hele cluster, niet een bepaalde naamruimte. Deze aanpak kunt u beheerders verlenen of ondersteuning voor technici toegang tot alle resources in het AKS-cluster.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Azure AD en Kubernetes RBAC, Zie [Integreer Azure Active Directory met AKS][aks-aad].

Zie voor de bijbehorende best practices, [aanbevolen procedures voor verificatie en autorisatie in AKS][operator-best-practices-identity].

Zie de volgende artikelen voor meer informatie over core Kubernetes en concepten voor AKS:

- [Kubernetes / AKS-clusters en workloads][aks-concepts-clusters-workloads]
- [Kubernetes / AKS-beveiliging][aks-concepts-security]
- [Kubernetes / AKS virtuele netwerken][aks-concepts-network]
- [Kubernetes / AKS-opslag][aks-concepts-storage]
- [Kubernetes / AKS schalen][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v1-protocols-openid-connect-code.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: aad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
