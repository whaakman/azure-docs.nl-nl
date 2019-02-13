---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 12/14/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 908d42bbecccf4a7c68a6cfde0c8cd960299df89
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213004"
---
## <a name="create-a-service-principal"></a>Een service-principal maken

Voor het maken van een service-principal die toegang heeft tot uw container registry, kunt u het volgende script uitvoeren in de [Azure Cloud Shell](../articles/cloud-shell/overview.md) of een lokale installatie van de [Azure CLI](/cli/azure/install-azure-cli). Het script is geformatteerd voor de Bash-shell.

Voordat u het script is uitgevoerd, werken de `ACR_NAME` variabele met de naam van het containerregister. De `SERVICE_PRINCIPAL_NAME` waarde moet uniek zijn binnen uw Azure Active Directory-tenant. Als u ontvangt een '`'http://acr-service-principal' already exists.`"fout, Geef een andere naam voor de service-principal.

U kunt eventueel de `--role` waarde in de opdracht [az ad sp create-for-rbac][/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac] op als u wilt andere machtigingen verlenen. Zie voor een volledige lijst met rollen, [ACR rollen en machtigingen](https://github.com/Azure/acr/blob/master/docs/roles-and-permissions.md).

Nadat u het script uitvoeren, Let op de service-principal **ID** en **wachtwoord**. Zodra u de referenties hebt, kunt u uw toepassingen en services om te verifiëren bij uw containerregister als de service-principal kunt configureren.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh --> [!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Gebruik een bestaande service-principal

Om registertoegang te verlenen aan een bestaande service-principal, moet u een nieuwe rol toewijzen aan de service-principal. Net als bij het maken van een nieuwe service principal, kunt u pull, push- en pull en eigenaar toegang, onder andere verlenen.

Het volgende script gebruikt de opdracht [az roltoewijzing create][/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create] om te verlenen *pull* machtigingen aan een service-principal die u opgeeft in de `SERVICE_PRINCIPAL_ID`variabele. Pas de `--role` waarde op als u wilt een ander niveau van toegang verlenen.


<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh --> [!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
