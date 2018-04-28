---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 04/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 6ed114ea6162c9d4888b6f86998cfb422a3944e8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
## <a name="create-a-service-principal"></a>Een service-principal maken

Als een service-principal maken met toegang tot het register van de container, kunt u het volgende script. Update de `ACR_NAME` variabele met de naam van het register container en desgewenst de `--role` waarde in de [az ad sp maken-voor-rbac] [ az-ad-sp-create-for-rbac] opdracht andere machtigingen te verlenen. U moet hebben de [Azure CLI](/cli/azure/install-azure-cli) dit script is geïnstalleerd.

Nadat u het script uitvoert, dient u de service-principal **ID** en **wachtwoord**. Zodra u de referenties hebt, kunt u uw toepassingen en services om het register van de container als de service-principal te verifiëren.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Gebruik een bestaande service-principal

Voor het verlenen van toegang tot het register naar een bestaande service-principal, moet u een nieuwe rol toewijzen aan de service-principal. Net als bij een nieuwe service principal maakt, kunt u de pull-, push- en pull- en eigenaar toegang verlenen.

Het volgende script maakt gebruik van de [az roltoewijzing maken] [ az-role-assignment-create] opdracht toe te kennen *pull* machtigingen voor een service-principal die u opgeeft in de `SERVICE_PRINCIPAL_ID` variabele. Pas de `--role` waarde als u een ander niveau van toegang verlenen.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
