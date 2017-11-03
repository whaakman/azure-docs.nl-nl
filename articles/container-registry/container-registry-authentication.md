---
title: Verificatie met een Azure container registry | Microsoft Docs
description: Hoe u zich aanmelden bij een Azure container register met een Azure Active Directory-service-principal of een beheerdersaccount te gebruiken
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d7d2ae0e9b1f7850332d151d78a4a5fdb013777
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Verificatie met een persoonlijke Docker-container register
Om te werken met installatiekopieën van de container in het register van een Azure-container, u zich aanmelden met behulp van de `docker login` opdracht. U kunt aanmelden met behulp van een  **[Azure Active Directory-service-principal](../active-directory/active-directory-application-objects.md)**  of een register-specifieke **beheerdersaccount**. Dit artikel vindt meer informatie over deze identiteiten.

## <a name="service-principal"></a>Service-principal

U kunt een service-principal toewijzen aan het register en deze gebruiken voor Docker basisverificatie. Met behulp van een service-principal wordt aanbevolen voor de meeste scenario's. De app-ID en het wachtwoord van de service-principal te bieden de `docker login` opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Nadat u bent aangemeld, Docker in de cache opgeslagen de referenties, dus u hoeft niet te herinneren van de app-ID.

> [!TIP]
> Als u wilt, kunt u het wachtwoord van een service-principal opnieuw genereren door het uitvoeren van de `az ad sp reset-credentials` opdracht.
>

Service-principals toestaan [toegangsgroepen op basis van](../active-directory/role-based-access-control-configure.md) met een register. Beschikbare rollen zijn:
  * Lezer (alleen pull-toegang).
  * Inzender (pull-abonnementen en push).
  * Eigenaar (pull-, push- en -rollen toewijzen aan andere gebruikers).

Anonieme toegang is niet beschikbaar op Azure Container registers. U kunt gebruiken voor openbare afbeeldingen [Docker Hub](https://docs.docker.com/docker-hub/).

U kunt meerdere service-principals toewijzen aan een register, zodat u kunt definiëren de toegang voor verschillende gebruikers of toepassingen. Service-principals ook inschakelen 'headless' verbinding met een register in ontwikkelaar of DevOps-scenario's zoals de volgende voorbeelden:

  * Containerimplementaties van een register met DC/OS, Docker Swarm en Kubernetes orchestration-systemen. U kunt ook pull-container registers gerelateerde Azure-services zoals [Containerservice](../container-service/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), en andere.

  * Continue integratie en implementatie-oplossingen (zoals Visual Studio Team Services of Jenkins) die container images maken en toepassen op een register.


## <a name="admin-account"></a>Beheerdersaccount
Een beheerdersaccount opgehaald met elke register die u maakt, automatisch gemaakt. Het account is standaard uitgeschakeld, maar u kunt deze inschakelen en de referenties beheren, bijvoorbeeld via de [portal](container-registry-get-started-portal.md#create-a-container-registry) of met behulp van de [Azure CLI 2.0 opdrachten](container-registry-get-started-azure-cli.md#create-a-container-registry). Elke beheerdersaccount wordt geleverd met twee wachtwoorden, die beide kunnen worden hersteld. De twee wachtwoorden kunnen u verbindingen met het register onderhouden met behulp van één wachtwoord terwijl u het andere wachtwoord opnieuw gegenereerd. Als het account is ingeschakeld, kunt u de gebruikersnaam en een wachtwoord voor het doorgeven de `docker login` opdracht voor basisverificatie in het register. Bijvoorbeeld:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> Het beheerdersaccount is ontworpen voor een enkele gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. Dit wordt niet aanbevolen voor het delen van de beheerder accountreferenties met andere gebruikers. Alle gebruikers worden weergegeven als een enkele gebruiker in het register. Hiermee schakelt u toegang tot het register voor alle gebruikers die gebruikmaken van de referenties wijzigen of dit account uitschakelen.
>

### <a name="next-steps"></a>Volgende stappen
* [Uw eerste afbeelding met de Docker CLI push](container-registry-get-started-docker-cli.md).
* Zie voor meer informatie over verificatie in het register van de Container-voorbeeld de [blogbericht](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
