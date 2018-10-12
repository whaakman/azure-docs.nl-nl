---
title: Verifiëren met een Azure container registry
description: Verificatie-opties voor een Azure container registry, met inbegrip van Azure Active Directory-service principals direct en het register aanmelding.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2018
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0c2323d1864be24edbf6005d634ae1d08bba8ea
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116603"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Verifiëren met een persoonlijk Docker-containerregister

Er zijn verschillende manieren om te verifiëren met een Azure container registry, die elk van toepassing op een of meer register gebruiksscenario's is.

U kunt aanmelden bij een register rechtstreeks via [afzonderlijke aanmelding](#individual-login-with-azure-ad), en uw toepassingen en container-orchestrators zonder toezicht of 'headless'-verificatie kunnen uitvoeren met behulp van een Azure Active Directory (Azure AD) [ service-principal](#service-principal).

Azure Container Registry biedt geen ondersteuning voor niet-geverifieerde Docker-bewerkingen of anonieme toegang. Voor openbare-installatiekopieën, kunt u [Docker Hub](https://docs.docker.com/docker-hub/).

## <a name="individual-login-with-azure-ad"></a>Afzonderlijke aanmelding met Azure AD

Als u werkt met het register rechtstreeks, zoals installatiekopieën binnenhaalt op en pusht vanaf uw ontwikkelwerkstation verifiëren met behulp van de [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) opdracht in de [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Wanneer u zich aan met `az acr login`, de CLI gebruikt het token gemaakt wanneer u uitgevoerd `az login` naadloos de sessie met het register te verifiëren. Nadat u bent aangemeld op deze manier, uw referenties zijn in de cache, en de daaropvolgende `docker` opdrachten vereisen geen gebruikersnaam of wachtwoord. Als uw token is verlopen, kunt u het vernieuwen met behulp van de `az acr login` opdracht nogmaals om te verifiëren. Met behulp van `az acr login` met Azure-id's biedt [op basis van de rol](../role-based-access-control/role-assignments-portal.md).

## <a name="service-principal"></a>Service-principal

U kunt een [service-principal](../active-directory/develop/app-objects-and-service-principals.md) naar het register en uw toepassing of service kunt gebruiken voor ' headless '-verificatie. Service-principals toestaan [op basis van de rol](../role-based-access-control/role-assignments-portal.md) naar een register, en u kunt meerdere service-principals toewijzen aan een register. Meerdere service-principals kunnen u voor het definiëren van verschillende toegangsrechten voor verschillende toepassingen.

De beschikbare rollen zijn:

  * **Lezer**: pull
  * **Inzender**: ophalen en pushen
  * **De eigenaar van**: pull, push en rollen toewijzen aan andere gebruikers

Service-principals inschakelen ' headless ' verbinding met een register in zowel push als pull-scenario's als volgt uit:

  * *Lezer*: containerimplementaties vanuit een register op de orchestration-systemen, waaronder Kubernetes, DC/OS en Docker Swarm. U kunt ook ophaalt uit containerregisters naar gerelateerde Azure-services zoals [AKS](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/), en anderen.

  * *Inzender*: continue integratie en implementatie-oplossingen zoals Azure pijplijnen of Jenkins die installatiekopieën compileren en push ze naar een register.

> [!TIP]
> U kunt het wachtwoord van een service-principal opnieuw genereren door het uitvoeren van de [az ad sp reset-credentials](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-reset-credentials) opdracht.
>

U kunt zich ook aanmelden rechtstreeks met een service-principal. De app-ID en het wachtwoord van de service-principal te bieden de `docker login` opdracht:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Wanneer u bent aangemeld, caches Docker de referenties, zodat u niet hoeft te onthouden van de app-ID.

Afhankelijk van de versie van Docker die u hebt geïnstalleerd, ziet u een beveiligingswaarschuwing geadviseerd het gebruik van de `--password-stdin` parameter. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdrachten.

Zie voor meer informatie over het gebruik van een service-principal voor ' headless ' verificatie bij ACR [Azure Container Registry-verificatie met service-principals](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Beheeraccount

Elke container registry bevat een beheeraccount is standaard uitgeschakeld. U kunt de gebruiker met beheerdersrechten inschakelen en beheren van de referenties in de [Azure-portal](container-registry-get-started-portal.md#create-a-container-registry), of met behulp van de Azure CLI.

> [!IMPORTANT]
> Het beheerdersaccount dat is ontworpen voor één gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. We raden niet de referenties van het Administrator-account met meerdere gebruikers delen. Alle gebruikers worden geverifieerd met het beheerdersaccount dat wordt weergegeven als één gebruiker met push- en pull-toegang tot het register. Hiermee schakelt u toegang tot het register voor alle gebruikers die gebruikmaken van de referenties wijzigen of dit account uitschakelen. Individuele identiteit wordt aanbevolen voor gebruikers en service-principals voor ' headless '-scenario's.
>

Het beheerdersaccount dat wordt geleverd met twee wachtwoorden, die beide kunnen worden hersteld. Twee wachtwoorden kunnen u verbinding met het register beheren met behulp van een wachtwoord, terwijl u de andere opnieuw genereren. Als het beheerdersaccount dat is ingeschakeld, kunt u de gebruikersnaam en een wachtwoord voor het doorgeven de `docker login` opdracht voor basisverificatie wordt gebruikt in het register. Bijvoorbeeld:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

Nogmaals, Docker wordt aanbevolen dat u de `--password-stdin` parameter in plaats van de levering op de opdrachtregel voor een betere beveiliging. U kunt ook alleen uw gebruikersnaam zonder opgeven `-p`, en voer uw wachtwoord in wanneer hierom wordt gevraagd.

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
