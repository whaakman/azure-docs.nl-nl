---
title: Over het beheren van Azure-gebruiker toegewezen beheerde identiteiten met behulp van REST
description: Stapsgewijze instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit voor REST-API-aanroepen.
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
ms.openlocfilehash: 70a8c9018cdc2929abc85336211beecf82bf32cb
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188043"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-rest-api-calls"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met behulp van REST-API aanroepen

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteit biedt Azure-services de mogelijkheid om te verifiëren met services die ondersteuning voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u te maken, weergeven, verwijderen van een gebruiker toegewezen beheerde identiteit met CURL REST-API-aanroepen.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u Windows gebruikt, installeert u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of gebruik de [Azure Cloud Shell](../../cloud-shell/overview.md) in Azure portal.
- Als u de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) of een [Linux-distributie](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [installeren van de Azure CLI lokale console](/azure/install-azure-cli).
- Als u van Azure CLI lokale console gebruikmaakt, aanmelden bij Azure met `az login` met een account dat is gekoppeld aan het Azure-abonnement u wilt implementeren of op te halen toegewezen gebruiker gegevens van beheerde identiteit.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van de identiteit van een gebruiker toegewezen.
- Ophalen van een Bearer access token met `az account get-access-token` om uit te voeren van de volgende gebruiker toegewezen bewerkingen van de beheerde identiteit.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Maak een beheerde identiteit toegewezen gebruiker 

Gebruik de volgende CURL-aanvraag naar de Azure Resource Manager-API voor het maken van een gebruiker toegewezen beheerde identiteit. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, en `<ACCESS TOKEN>` waarden door uw eigen waarden:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Lijst met door gebruiker toegewezen beheerde identiteiten

Aan de lijst van beheerde identiteiten toegewezen gebruiker, gebruikt u de volgende CURL-aanvraag naar de Azure Resource Manager-API. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<ACCESS TOKEN>` waarden door uw eigen waarden:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Een beheerde identiteit toegewezen gebruiker verwijderen

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, gebruikt u de volgende CURL-aanvraag naar de Azure Resource Manager-API. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<ACCESS TOKEN>` parameterwaarden door uw eigen waarden:

> [!NOTE]
> Verwijderen van een gebruiker toegewezen identiteit, wordt de verwijzing van een willekeurige deze is toegewezen aan resource niet verwijderen. Het verwijderen van een gebruiker toegewezen beheerd vanaf een virtuele machine met CURL Zie [verwijderen van een gebruiker toegewezen identiteit van een Azure VM](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toewijzen van de identiteit van een gebruiker toegewezen aan een Azure-VM/VMSS met CURL, [beheerde identiteit configureren op een Azure-VM met CURL](qs-configure-rest-vm.md#user-assigned-identity) en [beheerde identiteit configureren op een VM-schaalset met CURL ](qs-configure-rest-vmss.md#user-assigned-identity).


