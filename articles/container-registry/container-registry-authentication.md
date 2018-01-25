---
title: Verificatie met een Azure container-register
description: Verificatie-opties voor een Azure container register, waaronder Azure Active Directory-service principals direct en het register aanmelding.
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2018
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc29d9a57f1dd452d2b4b77a53f52a5b66a25416
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Verificatie met een persoonlijke Docker-container register

Er zijn verschillende manieren om te verifiëren met een Azure container register, die elk van toepassing op een of meer register gebruiksscenario's is.

U kunt aanmelden met een register rechtstreeks via [afzonderlijke aanmelding](#individual-login-with-azure-ad), en uw toepassingen en container orchestrators kunnen uitvoeren zonder toezicht of 'headless' verificatie met een Azure Active Directory (Azure AD) [ service-principal](#service-principal).

Azure Container register biedt geen ondersteuning voor niet-geverifieerde Docker-bewerkingen of anonieme toegang. Voor openbare afbeeldingen, kunt u [Docker Hub](https://docs.docker.com/docker-hub/).

## <a name="individual-login-with-azure-ad"></a>Meld u aan afzonderlijke met Azure AD

Als u werkt met het register rechtstreeks, zoals afbeeldingen ophalen en opslaan van installatiekopieën van uw ontwikkelwerkstation verifiëren met behulp van de [az acr aanmelding](/cli/azure/acr?view=azure-cli-latest#az_acr_login) opdracht in de [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Wanneer u zich aanmeldt met `az acr login`, de CLI gebruikt het token dat wordt gemaakt wanneer u uitgevoerd `az login` naadloos verifiëren uw sessie met het register. Zodra u op deze manier hebt aangemeld, uw referenties zijn in de cache en volgende `docker` opdrachten vereisen geen gebruikersnaam of wachtwoord. Als uw token is verlopen, kunt u het vernieuwen met behulp van de `az acr login` opdracht opnieuw uit om te verifiëren. Met behulp van `az acr login` met Azure identiteiten biedt [toegangsgroepen op basis van](../active-directory/role-based-access-control-configure.md).

## <a name="service-principal"></a>Service-principal

U kunt een [service-principal](../active-directory/develop/active-directory-application-objects.md) toe aan het register en uw toepassing of service kunt gebruiken voor headless-verificatie. Service-principals toestaan [toegangsgroepen op basis van](../active-directory/role-based-access-control-configure.md) met een register en u kunt meerdere service-principals toewijzen aan een register. Meerdere service-principals kunnen u verschillende toegangsmachtigingen voor verschillende toepassingen definiëren.

De beschikbare rollen zijn:

  * **Lezer**: pull
  * **Inzender**: trek en push
  * **Eigenaar**: pull-, push- en rollen toewijzen aan andere gebruikers

Service-principals inschakelen headless connectiviteit met een register in zowel push als pull-scenario's als volgt:

  * *Lezer*: containerimplementaties uit een register aan de orchestration-systemen, waaronder Kubernetes, DC/OS en Docker Swarm. U kunt ook pull van container registers tot gerelateerde Azure-services zoals [AKS](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), en anderen.

  * *Inzender*: continue integratie en implementatie-oplossingen zoals Visual Studio Team Services (VSTS) of Jenkins die container images maken en toepassen op een register.

> [!TIP]
> U kunt het wachtwoord van een service-principal opnieuw genereren door het uitvoeren van de [az ad sp opnieuw instellen van referenties](/cli/azure/ad/sp?view=azure-cli-latest#az_ad_sp_reset_credentials) opdracht.
>

U kunt zich ook aanmelden rechtstreeks met een service-principal. De app-ID en het wachtwoord van de service-principal te bieden de `docker login` opdracht:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Nadat u bent aangemeld, Docker in de cache opgeslagen de referenties, dus u hoeft niet te herinneren van de app-ID.

Afhankelijk van de versie van Docker die u hebt geïnstalleerd, ziet u een beveiligingswaarschuwing aanbevelen van het gebruik van de `--password-stdin` parameter. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie voor meer informatie de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) -opdrachten.

Zie voor meer informatie over het gebruik van een service-principal voor verificatie van headless ACR [Azure Container register verificatie met service-principals](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Beheeraccount

Elke container register bevat een admin-gebruikersaccount is standaard uitgeschakeld. U kunt de gebruiker met beheerdersrechten en beheren van de referenties in de [Azure-portal](container-registry-get-started-portal.md#create-a-container-registry), of met behulp van de Azure CLI.

> [!IMPORTANT]
> Het beheerdersaccount is ontworpen voor een enkele gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. We raden niet de accountreferenties admin delen met meerdere gebruikers. Alle gebruikers die worden geverifieerd met het beheerdersaccount weergegeven als een enkele gebruiker met push als pull-toegang tot het register. Hiermee schakelt u toegang tot het register voor alle gebruikers die de referenties gebruiken wijzigen of dit account uitschakelen. Afzonderlijke identiteit wordt aanbevolen voor gebruikers en service-principals voor headless scenario's.
>

Het beheerdersaccount is opgegeven met de twee wachtwoorden, die beide kunnen worden hersteld. Twee wachtwoorden kunnen u verbinding met het register onderhouden met behulp van één wachtwoord terwijl u de andere opnieuw genereert. Als de admin-account is ingeschakeld, kunt u de gebruikersnaam en een wachtwoord voor het doorgeven de `docker login` opdracht voor basisverificatie in het register. Bijvoorbeeld:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

Opnieuw Docker raadt aan dat u de `--password-stdin` parameter in plaats van op de opdrachtregel voor een betere beveiliging levering. U kunt alleen de gebruikersnaam ook zonder opgeven `-p`, en voer uw wachtwoord in als u wordt gevraagd.

Om de gebruiker met beheerdersrechten voor een bestaande register, kunt u de `--admin-enabled` parameter van de [az acr update](/cli/azure/acr?view=azure-cli-latest#az_acr_update) opdracht in de Azure CLI:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

U kunt de gebruiker met beheerdersrechten in de Azure portal door te gaan van het register inschakelen selecteren **toegangssleutels** onder **instellingen**, vervolgens **inschakelen** onder **Admin gebruiker**.

![Inschakelen van de gebruiker met beheerdersrechten gebruikersinterface in de Azure portal][auth-portal-01]

## <a name="next-steps"></a>Volgende stappen

* [Uw eerste afbeelding met de Azure CLI push](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
