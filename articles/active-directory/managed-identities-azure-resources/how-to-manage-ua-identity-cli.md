---
title: Over het beheren van een gebruiker toegewezen beheerde identiteit met Azure CLI
description: Stap voor stap beheerde instructies voor het maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met de Azure CLI.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: priyamo
ms.openlocfilehash: 4c5607a506ba392fbde0308d041ce79b08d04a3f
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882225"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met de Azure CLI

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure CLI.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de CLI-scriptvoorbeelden uitvoeren, hebt u drie opties:
    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit Azure portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van de Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console. Aanmelden bij Azure met `az login`, met een account dat is gekoppeld aan het Azure-abonnement waarin u wilt implementeren door de gebruiker toegewezen beheerde identiteit.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken 

Voor het maken van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Gebruik de [az-identiteit maken](/cli/azure/identity#az-identity-create) opdracht voor het maken van een gebruiker toegewezen beheerde identiteit. De `-g` parameter geeft u de resourcegroep waar moet worden gemaakt van de gebruiker toegewezen beheerde identiteit, en de `-n` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden door uw eigen waarden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

Als u wilt lezen/lijst met een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) of [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Aan de lijst van beheerde identiteiten gebruiker toegewezen, gebruikt u de [az identiteit lijst](/cli/azure/identity#az-identity-list) opdracht. Vervang de `<RESOURCE GROUP>` door uw eigen waarde:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
In de json-antwoord gebruiker toegewezen beheerde identiteiten hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, gebruikt u de [az identiteit verwijderen](/cli/azure/identity#az-identity-delete) opdracht.  De parameter - n Hiermee geeft u de naam en de parameter -g Hiermee geeft u de resourcegroep waarin de gebruiker toegewezen beheerde identiteit is gemaakt. Vervang de `<USER ASSIGNED IDENTITY NAME>` en `<RESOURCE GROUP>` parameterwaarden door uw eigen waarden:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Verwijderen van een gebruiker toegewezen beheerde identiteit, wordt de verwijzing van een willekeurige resource die deze is toegewezen aan niet verwijderen. Verwijder deze uit met behulp van VM/VMSS de `az vm/vmss identity remove` opdracht

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledige lijst van Azure CLI-opdrachten voor identiteit, [az identiteit](/cli/azure/identity).

Beheerde identiteit aan een virtuele machine van Azure-Zie voor meer informatie over het toewijzen van een gebruiker toegewezen [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 
