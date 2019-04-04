---
title: Azure Container Registry - rollen en machtigingen
description: Gebruik Azure op rollen gebaseerd toegangsbeheer (RBAC) en beheer van identiteits- en toegangsbeheer (IAM) voor gedetailleerdere machtigingen tot resources in een Azure container registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 03/20/2019
ms.author: danlep
ms.openlocfilehash: b6e26bfa476c5c13e6e478f40c39978af61d83e7
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894265"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Azure Container Registry-rollen en machtigingen

De Azure Container Registry-service ondersteunt een aantal Azure-rollen die verschillende niveaus van machtigingen aan een Azure container registry bieden. Gebruik Azure [op rollen gebaseerd toegangsbeheer](../role-based-access-control/index.yml) (RBAC) voor specifieke machtigingen toewijzen aan gebruikers of service-principals die moeten communiceren met een register.

| Rolmachtiging /       | [Access Resource Manager](#access-resource-manager) | [Register maken/verwijderen](#create-and-delete-registry) | [Installatiekopie pushen](#push-image) | [Pull-afbeelding](#pull-image) | [Image-gegevens verwijderen](#delete-image-data) | [Beleid wijzigen](#change-policies) |   [Meld u afbeeldingen](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Eigenaar | X | X | X | X | X | X |  |  
| Inzender | X | X | X |  X | X | X |  |  
| Lezer | X |  |  | X |  |  |  |
| AcrPush |  |  | X | X | |  |  |  
| AcrPull |  |  |  | X |  |  |  |  
| AcrDelete |  |  |  |  | X |  |  |
| AcrImageSigner |  |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Onderscheid maken tussen gebruikers en services

Alle machtigingen zijn toegepast, is een best practice voor de meest beperkte set machtigingen voor een persoon of -service, om een taak te volbrengen. De volgende machtigingensets vertegenwoordigen een verscheidenheid aan functies die kunnen worden gebruikt door mensen en headless services.

### <a name="cicd-solutions"></a>CI/CD-oplossingen

Bij het automatiseren van `docker build` opdrachten van CI/CD-oplossingen, moet u `docker push` mogelijkheden. Voor deze service ' headless '-scenario's is raadzaam om toe te wijzen de **AcrPush** rol. Deze rol, in tegenstelling tot de bredere **Inzender** rol, voorkomt u dat het account van het uitvoeren van andere registerbewerkingen of toegang tot Azure Resource Manager.

### <a name="container-host-nodes"></a>Container-knooppunten voor host

Knooppunten die uw containers worden uitgevoerd, moet ook, de **AcrPull** rol, maar mag niet vereist **lezer** mogelijkheden.

### <a name="visual-studio-code-docker-extension"></a>Visual Studio Code Docker-extensie

Voor hulpprogramma's zoals de Visual Studio Code [Docker-extensie](https://code.visualstudio.com/docs/azure/docker), aanvullende resource provider toegang is vereist om de beschikbare Azure-containerregisters weer te geven. In dit geval uw gebruikers toegang bieden tot de **lezer** of **Inzender** rol. Deze rollen toestaan `docker pull`, `docker push`, `az acr list`, `az acr build`, en andere functies. 

## <a name="access-resource-manager"></a>Access Resource Manager

Azure Resource Manager-toegang is vereist voor de Azure portal en het register management met de [Azure CLI](/cli/azure/). Bijvoorbeeld, voor een lijst van registers met behulp van de `az acr list` opdracht, moet u deze machtiging instellen. 

## <a name="create-and-delete-registry"></a>Maken en verwijderen van register

De mogelijkheid om te maken en verwijderen van Azure container Registry.

## <a name="push-image"></a>Installatiekopie pushen

De mogelijkheid om te `docker push` een installatiekopie of een andere push [ondersteund artefact](container-registry-image-formats.md) , zoals een Helm-diagram naar een register. Vereist [verificatie](container-registry-authentication.md) met het register met de identiteit van de gemachtigde. 

## <a name="pull-image"></a>Pull-afbeelding

De mogelijkheid om te `docker pull` een niet-quarantaine installatiekopie of een andere pull [ondersteund artefact](container-registry-image-formats.md) , zoals een Helm-diagram van een register. Vereist [verificatie](container-registry-authentication.md) met het register met de identiteit van de gemachtigde.

## <a name="delete-image-data"></a>Image-gegevens verwijderen

De mogelijkheid om te [verwijderen containerinstallatiekopieën](container-registry-delete.md), of verwijder andere [ondersteund artefacten](container-registry-image-formats.md) zoals Helm-grafieken, vanuit een register.

## <a name="change-policies"></a>Beleid wijzigen

De mogelijkheid om het beleid configureren op een register. Beleid voor bevatten installatiekopie verwijderen, inschakelen in quarantaine plaatsen en ondertekening van de installatiekopie.

## <a name="sign-images"></a>Meld u afbeeldingen

De mogelijkheid voor het ondertekenen van afbeeldingen, meestal toegewezen aan een geautomatiseerd proces, die een service-principal gebruiken. Deze machtiging wordt gewoonlijk gecombineerd met [push installatiekopie](#push-image) om toe te staan een vertrouwde installatiekopie pushen naar een register. Zie voor meer informatie, [inhoud vertrouwen in Azure Container Registry](container-registry-content-trust.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het toewijzen van RBAC-rollen naar een Azure-identiteit met behulp van de [Azure-portal](../role-based-access-control/role-assignments-portal.md), wordt de [Azure CLI](../role-based-access-control/role-assignments-cli.md), of andere Azure-hulpprogramma's.

* Meer informatie over [verificatieopties](container-registry-authentication.md) voor Azure Container Registry.
