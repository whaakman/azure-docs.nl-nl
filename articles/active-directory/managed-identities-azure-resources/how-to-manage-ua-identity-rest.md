---
title: Over het beheren van Azure door de gebruiker toegewezen beheerde identiteiten met behulp van REST
description: Stap voor stap beheerd instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen identiteit voor de REST-API-aanroepen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: dc7abd4bdec30ae870ff6add33d4b9b1c08b5bbd
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159636"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-rest-api-calls"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met behulp van REST-API aanroepen

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources Azure-services biedt de mogelijkheid om te verifiëren bij services die ondersteuning voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met CURL REST-API-aanroepen.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- Als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [installeren van de Azure CLI lokale console](/cli/azure/install-azure-cli).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan het Azure-abonnement u wilt implementeren of ophalen van gegevens van beheerde identiteit gebruiker toegewezen.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van een gebruiker toegewezen beheerde identiteit.
- Ophalen van een Bearer access token met `az account get-access-token` om uit te voeren van de volgende bewerkingen uit beheerde identiteit gebruiker toegewezen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Maken van een gebruiker toegewezen beheerde identiteit 

Gebruik de volgende CURL-aanvraag naar de Azure Resource Manager-API voor het maken van een gebruiker toegewezen beheerde identiteit. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, en `<ACCESS TOKEN>` waarden door uw eigen waarden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

Aan de lijst van beheerde identiteiten gebruiker toegewezen, gebruikt u de volgende CURL-aanvraag naar de Azure Resource Manager-API. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<ACCESS TOKEN>` waarden door uw eigen waarden:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, gebruikt u de volgende CURL-aanvraag naar de Azure Resource Manager-API. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<ACCESS TOKEN>` parameterwaarden door uw eigen waarden:

> [!NOTE]
> Verwijderen van een gebruiker toegewezen beheerde identiteit, wordt de verwijzing van een willekeurige deze is toegewezen aan resource niet verwijderen. Verwijderen van een gebruiker toegewezen beheerd vanaf een virtuele machine met CURL Zie [verwijderen van een gebruiker toegewezen identiteit van een Azure VM](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan een Azure-VM/VMSS met CURL, [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van REST-API-aanroepen](qs-configure-rest-vm.md#user-assigned-managed-identity) en [configureren die worden beheerd identiteiten voor een Azure-resources op een VM-schaalset met behulp van REST API-aanroepen](qs-configure-rest-vmss.md#user-assigned-managed-identity).


