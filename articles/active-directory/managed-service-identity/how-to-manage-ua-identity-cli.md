---
title: Het beheren van een gebruiker toegewezen beheerde Service identiteit (MSI) met Azure CLI
description: Stap voor stap beheerd instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen service identiteit met behulp van de Azure CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 5262914e469bdc07921c3b82e990d544349b5fd4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met behulp van de Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van de identiteit van een gebruiker aan zijn toegewezen met Azure CLI.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

- De CLI script als voorbeelden wilt uitvoeren, hebt u drie opties:

    - Gebruik [Azure Cloud Shell](../../cloud-shell/overview.md) vanuit de Azure-portal (Zie volgende sectie).
    - Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok.
    - [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever een lokale CLI-console te gebruiken. Meld u bij het gebruik van Azure `az login`, met een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt implementeren de identiteit van de gebruiker toegewezen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Maak een beheerde identiteit toegewezen gebruiker 

Gebruik voor het maken van een toegewezen gebruikers-id de [az identiteit maken](/cli/azure/identity#az-identity-create) opdracht. De `-g` parameter geeft u de resourcegroep waar moet worden gemaakt van de identiteit van de gebruiker die is toegewezen, en de `-n` parameter geeft u de naam ervan. Vervang de `<RESOURCE GROUP>` en `<USER ASSIGNED IDENTITY NAME>` parameterwaarden met uw eigen waarden:

> [!IMPORTANT]
> Maken van toegewezen gebruikers-id's ondersteunt alleen alfanumerieke en het koppelteken (0-9 of a-z of A-Z of -) tekens. Bovendien moeten worden beperkt tot 24 tekens voor de toewijzing aan een VM/VMSS goed te laten werken. Controleer regelmatig op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Identiteiten toegewezen lijst door gebruiker

Aan de lijst met toegewezen gebruikers-id's, gebruikt u de [az identiteit lijst](/cli/azure/identity#az-identity-list) opdracht.  De `-g` parameter geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt.  Vervang de `<RESOURCE GROUP>` met uw eigen waarde:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
In het json-antwoord, gebruikers-id's hebben `"Microsoft.ManagedIdentity/userAssignedIdentities"` waarde geretourneerd voor de sleutel, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Een gebruiker toegewezen identiteit verwijderen

Gebruiken voor het verwijderen van een toegewezen gebruikers-id, de [az identiteit verwijderen](/cli/azure/identity#az-identity-delete) opdracht.  De parameter - n geeft de naam en de parameter -g geeft u de resourcegroep waar de gebruiker toegewezen identiteit is gemaakt.  Vervang de `<USER ASSIGNED IDENTITY NAME>` en `<RESOURCE GROUP>` parameterwaarden door uw eigen waarden:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Verwijdering van een gebruiker toegewezen identiteit, worden de verwijzing van een bron die deze is toegewezen aan niet verwijderd. Verwijder die uit de virtuele machine/VMSS met behulp van de `az vm/vmss identity remove` opdracht

## <a name="related-content"></a>Gerelateerde inhoud

Zie voor een volledige lijst met Azure CLI-opdrachten voor identiteit, [az identiteit](/cli/azure/identity).

Voor informatie over het toewijzen van de identiteit van een gebruiker is toegewezen aan een virtuele machine van Azure-Zie [configureren beheerde Service identiteit (MSI) met Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
