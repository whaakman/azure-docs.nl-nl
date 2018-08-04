---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/03/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 2174ae44f8e78763c1939aee5e6b86c95a0924ce
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39514074"
---
## <a name="create-a-service-principal"></a>Een service-principal maken

Voor het maken van een service-principal die toegang heeft tot uw container registry, kunt u het volgende script uitvoeren in de [Azure Cloud Shell](../articles/cloud-shell/overview.md) of een lokale installatie van de [Azure CLI](/cli/azure/install-azure-cli). Het script is geformatteerd voor de Bash-shell.

Voordat u het script is uitgevoerd, werken de `ACR_NAME` variabele met de naam van het containerregister. De `SERVICE_PRINCIPAL_NAME` waarde moet uniek zijn binnen uw Azure Active Directory-tenant. Als u ontvangt een '`'http://acr-service-principal' already exists.`"fout, Geef een andere naam voor de service-principal.

U kunt eventueel de `--role` waarde in de [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] opdracht als u wilt andere machtigingen verlenen.

Nadat u het script uitvoeren, Let op de service-principal **ID** en **wachtwoord**. Zodra u de referenties hebt, kunt u uw toepassingen en services om te verifiëren bij uw containerregister als de service-principal kunt configureren.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh --> [!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Gebruik een bestaande service-principal

Om registertoegang te verlenen aan een bestaande service-principal, moet u een nieuwe rol toewijzen aan de service-principal. Net als bij het maken van een nieuwe service principal, kunt u pull, push- en pull en eigenaar toegang verlenen.

Het volgende script maakt gebruik van de [az roltoewijzing maken] [ az-role-assignment-create] opdracht uit om toegang te verlenen *pull* machtigingen aan een service-principal die u opgeeft in de `SERVICE_PRINCIPAL_ID` variabele. Pas de `--role` waarde op als u wilt een ander niveau van toegang verlenen.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh --> [!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
