---
title: Over het beheren van een gebruiker toegewezen Managed Service Identity (MSI) met behulp van Azure CLI
description: Stapsgewijze beheerde instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met de Azure CLI.
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
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 1989017361c148f9a6c8fcb73537be78555fd650
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160587"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met de Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met Azure CLI.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:

    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console. Aanmelden bij Azure met `az login`, met een account dat is gekoppeld aan het Azure-abonnement waarin u wilt implementeren de identiteit van de gebruiker toegewezen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Maak een beheerde identiteit toegewezen gebruiker 

Gebruik voor het maken van een gebruiker toegewezen identiteit het [az-identiteit maken](/cli/azure/identity#az-identity-create) opdracht. De `-g` parameter geeft u de resourcegroep waar moet worden gemaakt van de identiteit van de gebruiker toegewezen, en de `-n` parameter geeft u de naam ervan. Ten minste uw account moet worden toegewezen de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Lijst met door gebruiker toegewezen identiteiten

Aan de lijst van een gebruiker toegewezen identiteiten, gebruikt u de [az identiteit lijst](/cli/azure/identity#az-identity-list) opdracht.  De `-g` parameter geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt. Ten minste uw account moet worden toegewezen de [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol om de eigenschappen van een gebruiker toegewezen identiteit weer te geven.  Vervang de `<RESOURCE GROUP>` door uw eigen waarde:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
In de json-antwoord, gebruikers-id's hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Verwijderen van een gebruiker toegewezen identiteit

Als u wilt verwijderen van een gebruiker toegewezen identiteit, gebruikt u de [az identiteit verwijderen](/cli/azure/identity#az-identity-delete) opdracht.  De parameter - n Hiermee geeft u de naam en de parameter -g Hiermee geeft u de resourcegroep waarin de gebruiker toegewezen identiteit is gemaakt.  Ten minste uw account moet worden toegewezen de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol verwijderen van de identiteit van een gebruiker toegewezen. Vervang de `<USER ASSIGNED IDENTITY NAME>` en `<RESOURCE GROUP>` parameterwaarden door uw eigen waarden:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Verwijderen van een gebruiker toegewezen identiteit, wordt de verwijzing van een willekeurige resource die deze is toegewezen aan niet verwijderen. Verwijder deze uit met behulp van VM/VMSS de `az vm/vmss identity remove` opdracht

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor een volledige lijst van Azure CLI-opdrachten voor identiteit, [az identiteit](/cli/azure/identity).

Voor meer informatie over het toewijzen van de identiteit van een gebruiker toegewezen aan een virtuele machine van Azure-Zie [configureren Managed Service Identity (MSI) met behulp van Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
