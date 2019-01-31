---
title: Verifiëren met een Azure container registry
description: Verificatie-opties voor een Azure container registry, met inbegrip van aanmelden met een Azure Active Directory-identiteit, met behulp van service-principals en het gebruik van optionele beheerdersreferenties.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 12/21/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 66f9c41e2551dffc32932f1cfa53fa444251b303
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55301047"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Verifiëren met een persoonlijk Docker-containerregister

Er zijn verschillende manieren om te verifiëren met een Azure container registry, die elk van toepassing op een of meer register gebruiksscenario's is.

U kunt aanmelden bij een register rechtstreeks via [afzonderlijke aanmelding](#individual-login-with-azure-ad), of uw toepassingen en container-orchestrators zonder toezicht of 'headless'-verificatie kunnen uitvoeren met behulp van een Azure Active Directory (Azure AD) [ service-principal](#service-principal).

Azure Container Registry biedt geen ondersteuning voor niet-geverifieerde Docker-bewerkingen of anonieme toegang. Voor openbare-installatiekopieën, kunt u [Docker Hub](https://docs.docker.com/docker-hub/).

## <a name="individual-login-with-azure-ad"></a>Afzonderlijke aanmelding met Azure AD

Als u werkt met het register rechtstreeks, zoals installatiekopieën binnenhaalt op en pusht vanaf een ontwikkelwerkstation verifiëren met behulp van de [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) opdracht in de [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Wanneer u zich aan met `az acr login`, de CLI gebruikt het token gemaakt wanneer u uitgevoerd [az login](/cli/azure/reference-index#az-login) naadloos de sessie met het register te verifiëren. Nadat u bent aangemeld op deze manier, uw referenties zijn in de cache, en de daaropvolgende `docker` opdrachten in uw sessie vereisen geen gebruikersnaam of wachtwoord. 

Voor toegang tot het register, het token wordt gebruikt door `az acr login` is geldig voor 1 uur, raden wij aan dat u altijd bij het register voordat u aanmelden een `docker` opdracht. Als uw token is verlopen, kunt u het vernieuwen met behulp van de `az acr login` opdracht nogmaals om te verifiëren. 

Met behulp van `az acr login` met Azure-id's biedt [op basis van de rol](../role-based-access-control/role-assignments-portal.md). Voor sommige scenario's kunt u zich aanmeldt bij een register met uw eigen individuele identiteit in Azure AD. Voor verschillende scenario's of voor het afhandelen van de behoeften van een werkgroep niet waar u afzonderlijke toegang beheren, u kunt zich ook aanmelden met een [beheerde identiteit voor de Azure-resources](container-registry-authentication-managed-identity.md).

## <a name="service-principal"></a>Service-principal

Als u een [service-principal](../active-directory/develop/app-objects-and-service-principals.md) naar het register, uw toepassing of service kan deze gebruiken voor verificatie ' headless '. Service-principals toestaan [op basis van de rol](../role-based-access-control/role-assignments-portal.md) naar een register, en u kunt meerdere service-principals toewijzen aan een register. Meerdere service-principals kunnen u voor het definiëren van verschillende toegangsrechten voor verschillende toepassingen.

De beschikbare rollen voor een container registry zijn onder andere:

* **AcrPull**: pull

* **AcrPush**: ophalen en pushen

* **De eigenaar van**: pull, push en rollen toewijzen aan andere gebruikers

Zie voor een volledige lijst met rollen, [Azure Container Registry-rollen en machtigingen](container-registry-roles.md).

Zie voor de CLI-scripts te maken van een service-principal app-ID en wachtwoord voor verificatie met een Azure container registry of gebruik een bestaande service-principal, [Azure Container Registry-verificatie met service-principals](container-registry-auth-service-principal.md).

Service-principals inschakelen ' headless ' verbinding met een register in zowel push als pull-scenario's als volgt uit:

  * *Pull-*: Containers implementeren van een register op de orchestration-systemen, waaronder Kubernetes, DC/OS en Docker Swarm. U kunt ook ophaalt uit containerregisters naar gerelateerde Azure-services zoals [Azure Kubernetes Service](container-registry-auth-aks.md), [Azure Container Instances](container-registry-auth-aci.md), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/), enzovoort.

  * *Push-*: Installatiekopieën compileren en push ze naar een register met behulp van continue integratie en implementatie-oplossingen zoals Azure pijplijnen of Jenkins.

U kunt zich ook aanmelden rechtstreeks met een service-principal. Wanneer u de volgende opdracht uitvoert, geeft u interactief de service-principal appID (gebruikersnaam) en het wachtwoord wanneer hierom wordt gevraagd. Zie voor aanbevolen procedures voor het beheren van referenties voor clusteraanmelding de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdrachten:

```Docker
docker login myregistry.azurecr.io
```

Wanneer u bent aangemeld, caches Docker de referenties, zodat u niet hoeft te onthouden van de app-ID.

> [!TIP]
> U kunt het wachtwoord van een service-principal opnieuw genereren door het uitvoeren van de [az ad sp reset-credentials](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-reset-credentials) opdracht.
>

## <a name="admin-account"></a>Beheeraccount

Elke container registry bevat een beheeraccount is standaard uitgeschakeld. U kunt de gebruiker met beheerdersrechten inschakelen en beheren van de referenties in de Azure portal of met behulp van de Azure CLI of andere Azure-hulpprogramma's.

> [!IMPORTANT]
> Het beheerdersaccount dat is ontworpen voor één gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. We raden niet de referenties van het Administrator-account met meerdere gebruikers delen. Alle gebruikers worden geverifieerd met het beheerdersaccount dat wordt weergegeven als één gebruiker met push- en pull-toegang tot het register. Hiermee schakelt u toegang tot het register voor alle gebruikers die gebruikmaken van de referenties wijzigen of dit account uitschakelen. Individuele identiteit wordt aanbevolen voor gebruikers en service-principals voor ' headless '-scenario's.
>

Het beheerdersaccount dat wordt geleverd met twee wachtwoorden, die beide kunnen worden hersteld. Twee wachtwoorden kunnen u verbinding met het register beheren met behulp van een wachtwoord, terwijl u de andere opnieuw genereren. Als het beheerdersaccount dat is ingeschakeld, kunt u de gebruikersnaam en een wachtwoord voor het doorgeven de `docker login` opdracht wanneer u hierom wordt gevraagd voor basisverificatie in het register. Bijvoorbeeld:

```Docker
docker login myregistry.azurecr.io 
```


Als u wilt de gebruiker met beheerdersrechten voor een bestaand register inschakelt, kunt u de `--admin-enabled` parameter van de [az acr update](/cli/azure/acr?view=azure-cli-latest#az-acr-update) opdracht in de Azure CLI:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

U kunt de gebruiker met beheerdersrechten in de Azure-portal inschakelen door het register te navigeren selecteren **toegangssleutels** onder **instellingen**, klikt u vervolgens **inschakelen** onder **Admin gebruiker**.

![Gebruiker met beheerdersrechten gebruikersinterface in Azure portal inschakelen][auth-portal-01]

## <a name="next-steps"></a>Volgende stappen

* [Uw eerste installatiekopie met de Azure CLI pushen](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
