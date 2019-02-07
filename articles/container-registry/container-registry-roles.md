---
title: Azure Container Registry - rollen en machtigingen
description: Gebruik Azure op rollen gebaseerd toegangsbeheer (RBAC) en beheer van identiteits- en toegangsbeheer (IAM) voor gedetailleerdere machtigingen tot resources in een Azure container registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: e2ec1b7ad6d1489836937d30b89d0f0f681a9bfa
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55819583"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Azure Container Registry-rollen en machtigingen

De Azure Container Registry-service ondersteunt een aantal Azure-rollen die verschillende niveaus van machtigingen aan een Azure container registry bieden. Gebruik Azure [op rollen gebaseerd toegangsbeheer](../role-based-access-control/index.yml) (RBAC) voor specifieke machtigingen toewijzen aan gebruikers of service-principals die moeten communiceren met een register.

| Rolmachtiging /       | Access Resource Manager| Register maken/verwijderen | [Installatiekopie pushen](#push-image) | [Pull-afbeelding](#pull-image) | Beleid wijzigen |   [Meld u afbeeldingen](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- |
| Eigenaar | X | X | X | X | X |  |  
| Inzender | X | X | X | X | X |  |  
| Lezer | X |  |  | X |  |  | 
| AcrPush |  |  | X | X |  |  |  
| AcrPull |  |  |  | X |  |  |  
| AcrImageSigner |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Onderscheid maken tussen gebruikers en services

Alle machtigingen zijn toegepast, is een best practice voor de meest beperkte set machtigingen voor een persoon of -service, om een taak te volbrengen. De volgende machtigingensets vertegenwoordigen een verscheidenheid aan functies die kunnen worden gebruikt door mensen en headless services.

### <a name="cicd-solutions"></a>CI/CD-oplossingen

Bij het automatiseren van `docker build` opdrachten van CI/CD-oplossingen, moet u `docker push` mogelijkheden. Voor deze service ' headless '-scenario's is raadzaam om toe te wijzen de **AcrPush** rol. Deze rol, in tegenstelling tot de bredere **Inzender** rol, voorkomt u dat het account van het uitvoeren van andere registerbewerkingen of toegang tot Azure Resource Manager.

### <a name="container-host-nodes"></a>Container-knooppunten voor host

Knooppunten die uw containers worden uitgevoerd, moet ook, de **AcrPull** rol, maar mag niet vereist **lezer** mogelijkheden.

### <a name="visual-studio-code-docker-extension"></a>Visual Studio Code Docker-extensie

Voor hulpprogramma's zoals de Visual Studio Code [Docker-extensie](https://code.visualstudio.com/docs/azure/docker), aanvullende resource provider toegang is vereist om de beschikbare Azure-containerregisters weer te geven. In dit geval uw gebruikers toegang bieden tot de **lezer** of **Inzender** rol. Deze rollen toestaan `docker pull`, `docker push`, `az acr list`, `az acr build`, en andere functies. 

## <a name="access-resource-manager"></a>Access Resource Manager

Azure Resource Manager-toegang is vereist voor de Azure-portal en [Azure CLI](/cli/azure/). Bijvoorbeeld, voor een lijst van registers met behulp van de `az acr list` opdracht, moet u deze machtiging instellen. 

## <a name="createdelete-registry"></a>Register maken/verwijderen

De mogelijkheid om te maken en verwijderen van Azure container Registry.

## <a name="push-image"></a>Installatiekopie pushen

De mogelijkheid om te `docker push` een installatiekopie of een andere ondersteunde artefact, naar een register pushen. Vereist [verificatie](container-registry-authentication.md) met het register met de identiteit van de gemachtigde. 

## <a name="pull-image"></a>Pull-afbeelding

De mogelijkheid om te `docker pull` een niet-quarantaine installatiekopie of een andere ondersteunde-item ophalen uit een register. Vereist [verificatie](container-registry-authentication.md) met het register met de identiteit van de gemachtigde.

## <a name="change-policies"></a>Beleid wijzigen

De mogelijkheid om het beleid configureren op een register. Beleid voor bevatten installatiekopie verwijderen, inschakelen in quarantaine plaatsen en ondertekening van de installatiekopie.

## <a name="sign-images"></a>Meld u afbeeldingen

De mogelijkheid voor het ondertekenen van afbeeldingen, meestal toegewezen aan een geautomatiseerd proces, die een service-principal gebruiken. Deze machtiging wordt gewoonlijk gecombineerd met [push installatiekopie](#push-image) om toe te staan een vertrouwde installatiekopie pushen naar een register. Zie voor meer informatie, [inhoud vertrouwen in Azure Container Registry](container-registry-content-trust.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het toewijzen van RBAC-rollen naar een Azure-identiteit met behulp van de [Azure-portal](../role-based-access-control/role-assignments-portal.md), wordt de [Azure CLI](../role-based-access-control/role-assignments-cli.md), of andere Azure-hulpprogramma's.

* Meer informatie over [verificatieopties](container-registry-authentication.md) voor Azure Container Registry.
