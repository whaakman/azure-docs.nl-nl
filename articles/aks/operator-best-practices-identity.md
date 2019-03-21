---
title: Operator aanbevolen procedures - identiteit in Azure Kubernetes Services (AKS)
description: Meer informatie over de cluster-operator best practices voor het beheren van verificatie en autorisatie voor clusters in Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 201fef6b3e773daa18ae252d1d5734d8d87419b5
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287125"
---
# <a name="best-practices-for-authentication-and-authorization-in-azure-kubernetes-service-aks"></a>Aanbevolen procedures voor verificatie en autorisatie in Azure Kubernetes Service (AKS)

Implementeren en onderhouden van clusters in Azure Kubernetes Service (AKS), moet u methoden voor het beheren van toegang tot resources en services implementeren. Zonder deze besturingselementen kunnen accounts toegang tot resources en services die ze niet nodig hebben. Kan ook het moeilijk zijn om bij te houden welke set referenties zijn gebruikt voor het aanbrengen van wijzigingen.

Deze aanbevolen procedures voor richt zich op hoe een cluster-operator toegang en identiteit voor de AKS-clusters kunt beheren. In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Verificatie van gebruikers van de AKS-cluster met Azure Active Directory
> * Toegang tot bronnen met op rollen gebaseerd toegangsbeheer (RBAC) beheren
> * Gebruik van een beheerde identiteit worden geverifieerd met andere services

## <a name="use-azure-active-directory"></a>Use Azure Active Directory

**Aanbevolen procedurerichtlijn** -AKS implementeren-clusters met Azure AD-integratie. Met behulp van Azure AD, centraliseert het onderdeel-id-beheer. Eventuele wijzigingen in de account of groep van de gebruiker wordt automatisch bijgewerkt in toegang tot het AKS-cluster. Gebruik functies of ClusterRoles en bindingen, zoals beschreven in de volgende sectie, bereik gebruikers of groepen aan de benodigde machtigingen zo min mogelijk.

De ontwikkelaars en toepassingseigenaren van uw Kubernetes-cluster moeten toegang tot verschillende bronnen. Kubernetes biedt geen een oplossing voor identiteitsbeheer om te bepalen welke gebruikers met de bronnen kunnen werken. In plaats daarvan integreren u doorgaans in uw cluster een bestaande oplossing voor identiteit. Azure Active Directory (AD) biedt een oplossing voor identiteitsbeheer, bedrijfsklare en kan worden geïntegreerd met AKS-clusters.

Met Azure AD geïntegreerde clusters in AKS, maakt u *rollen* of *ClusterRoles* waarmee machtigingen voor toegang tot resources worden gedefinieerd. U vervolgens *binden* de rollen aan gebruikers of groepen van Azure AD. Deze Kubernetes op rollen gebaseerd toegangsbeheer (RBAC) zijn in de volgende sectie besproken. De integratie van Azure AD en hoe u toegang tot resources beheren in het volgende diagram weergegeven:

![Verificatie voor Azure Active Directory-integratie met AKS-cluster op](media/operator-best-practices-identity/cluster-level-authentication-flow.png)

1. Ontwikkelaar wordt geverifieerd met Azure AD.
1. Het eindpunt van de Azure AD-token-uitgifte problemen met het toegangstoken.
1. De ontwikkelaar uitvoert een actie met behulp van de Azure AD-token, zoals `kubectl create pod`
1. Kubernetes valideert het token met Azure Active Directory en haalt de groepslidmaatschappen van de ontwikkelaar.
1. Kubernetes op rollen gebaseerd beheer (RBAC) en het cluster toegang krijgen tot het beleid wordt toegepast.
1. Ontwikkelaar van de aanvraag is geslaagd of niet op basis van de vorige validatie van het lidmaatschap van Azure AD en Kubernetes RBAC en beleid.

Zie voor het maken van een AKS-cluster dat gebruik maakt van Azure AD, [Integreer Azure Active Directory met AKS][aks-aad].

## <a name="use-role-based-access-controls-rbac"></a>Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken

**Aanbevolen procedurerichtlijn** -gebruik Kubernetes RBAC voor het definiëren van de machtigingen die gebruikers of groepen hebben tot resources in het cluster. Rollen en -bindingen die toewijzen van machtigingen die vereist zijn zo min mogelijk maken. Integreren met Azure AD zodat de wijziging van de gebruikersstatus of het lidmaatschap wordt automatisch bijgewerkt en toegang tot clusterresources actueel is.

In Kubernetes, kunt u gedetailleerde controle van toegang tot resources in het cluster opgeven. Machtigingen kunnen worden gedefinieerd op het clusterniveau van het, of naar specifieke naamruimten. U kunt bepalen welke resources kunnen worden beheerd, en met welke machtigingen. Deze rollen zijn de toegepaste aan gebruikers of groepen met een binding. Voor meer informatie over *rollen*, *ClusterRoles*, en *bindingen*, Zie [opties voor toegang en identiteit voor Azure Kubernetes Service (AKS)] [aks-concepts-identity].

Als u bijvoorbeeld kunt u een rol die volledige toegang tot resources in de naamruimte met de naam verleent *Financiën-app*, zoals wordt weergegeven in het volgende voorbeeld YAML-manifest:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role
  namespace: finance-app
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
```

Een RoleBinding wordt vervolgens gemaakt dat een binding de Azure AD-gebruiker heeft *developer1\@contoso.com* naar de RoleBinding, zoals wordt weergegeven in de volgende YAML-manifest:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role-binding
  namespace: finance-app
subjects:
- kind: User
  name: developer1@contoso.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: finance-app-full-access-role
  apiGroup: rbac.authorization.k8s.io
```

Wanneer *developer1\@contoso.com* wordt geverifieerd op basis van het AKS-cluster, hebben volledige machtigingen voor resources in de *Financiën-app* naamruimte. In deze manier, u logische manier apart en beheer de toegang tot resources. Kubernetes RBAC moet worden gebruikt in combinatie met Azure AD-integratie, zoals beschreven in de vorige sectie.

## <a name="use-pod-identities"></a>Pod-identiteiten gebruiken

**Aanbevolen procedurerichtlijn** -vaste referenties binnen schillen of containerinstallatiekopieën niet gebruiken omdat ze zich op risico's van blootstelling of misbruik. Gebruik in plaats daarvan pod-id's automatisch om toegang te vragen met behulp van een centraal oplossing voor Azure AD-identiteit.

Wanneer schillen toegang hebben tot andere Azure-services, zoals Cosmos DB, Key Vault of Blob Storage moeten, moet de schil referenties voor toegang. Deze referenties voor toegang kunnen worden gedefinieerd met behulp van de containerinstallatiekopie of ingevoerd als een Kubernetes-geheim, maar moeten handmatig worden gemaakt en toegewezen. Vaak de referenties worden hergebruikt voor schillen en zijn niet regelmatig worden gedraaid.

Beheerde identiteiten voor Azure-resources (momenteel geïmplementeerd als een gekoppelde AKS open source-project) kunnen u automatisch toegang vragen tot services via Azure AD. Kunt u geen referenties handmatig definiëren voor schillen, in plaats daarvan ze een toegangstoken in realtime en deze kunt gebruiken voor toegang tot alleen de services van hun toegewezen. In AKS, worden de twee onderdelen geïmplementeerd door de cluster-operator waarmee schillen om beheerde identiteiten te gebruiken:

* **Het knooppunt beheer van identiteit (NMI)-server** is een schil die wordt uitgevoerd als een DaemonSet op elk knooppunt in het AKS-cluster. De NMI-server luistert naar aanvragen van de schil tot Azure-services.
* **De beheerde identiteit Controller (MIC)** is een centrale schil met machtigingen voor het opvragen van de Kubernetes API-server en wordt gecontroleerd op een Azure-identiteit toewijzen die overeenkomt met een schil.

Wanneer schillen toegang tot een Azure-service aanvragen, netwerkregels het verkeer worden omgeleid naar het knooppunt beheer van identiteit (NMI)-server. De server NMI identificeert schillen die aanvragen van toegang tot Azure-services op basis van hun extern adres en query's van de beheerde identiteit Controller (MIC). De MIC Azure identity-toewijzingen in het AKS-cluster en de server NMI controleert vervolgens vraagt dat een toegangstoken van Azure Active Directory (AD) op basis van de toewijzing van de schil. Azure AD biedt toegang tot de server NMI, die wordt geretourneerd naar de schil. Dit toegangstoken kan worden gebruikt door de schil om aan te vragen vervolgens toegang tot services in Azure.

In het volgende voorbeeld maakt u een ontwikkelaar een schil die gebruikmaakt van een beheerde identiteit voor het aanvragen van toegang tot een Azure SQL Server-exemplaar:

![Pod identiteiten kunnen een schil om aan te vragen automatisch toegang tot andere services](media/operator-best-practices-identity/pod-identities.png)

1. Cluster-operator maakt eerst een serviceaccount dat kan worden gebruikt om toe te wijzen identiteiten wanneer schillen toegang tot services aanvragen.
1. De NMI-server en de MIC worden geïmplementeerd voor de relay-schil verzoeken om toegangstokens aan Azure AD.
1. Een ontwikkelaar implementeert een schil met een beheerde identiteit die een toegangstoken via de NMI-server vraagt.
1. Het token wordt geretourneerd naar de schil en gebruikt voor toegang tot een Azure SQL Server-exemplaar.

Beheerde pod identiteiten is een open-SourceProject van AKS en wordt niet ondersteund door Azure technische ondersteuning. Dit wordt geleverd met het verzamelen van fouten en feedback van onze community. Het project wordt niet aanbevolen voor gebruik in productieomgevingen.

Zie voor het gebruik van de schil identiteiten [Azure Active Directory-identiteiten voor toepassingen met Kubernetes][aad-pod-identity].

## <a name="next-steps"></a>Volgende stappen

Deze aanbevolen procedures voor gericht op verificatie en autorisatie voor uw cluster en de resources. Voor het implementeren van sommige van deze aanbevolen procedures, Zie de volgende artikelen:

* [Azure Active Directory integreren met AKS][aks-aad]
* [Gebruik van beheerde identiteiten voor Azure-resources met AKS][aad-pod-identity]

Zie voor meer informatie over de bewerkingen voor een cluster in AKS, de volgende aanbevolen procedures:

* [Multitenancy en cluster-isolatie][aks-best-practices-scheduler]
* [Kubernetes scheduler basisfuncties][aks-best-practices-scheduler]
* [Geavanceerde functies voor Kubernetes-scheduler][aks-best-practices-advanced-scheduler]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-aad]: aad-integration.md
[managed-identities:]: ../active-directory/managed-identities-azure-resources/overview.md
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
