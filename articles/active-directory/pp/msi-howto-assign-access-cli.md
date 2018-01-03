---
title: Een gebruiker toegewezen MSI toegang toewijzen aan een Azure-resource met Azure CLI
description: Stapsgewijze toegang instructies voor het toewijzen van een gebruiker toegewezen MSI op een resource, tot een andere bron, met Azure CLI.
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 732fc1981bdf95e7548ea0ebe0ca8ece00f483ce
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-user-assigned-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Een beheerde Service identiteit (MSI) gebruiker toegewezen toegang toewijzen aan een resource met Azure CLI

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Als u een gebruiker toegewezen MSI hebt gemaakt, kunt u de MSI-toegang geven tot een andere resource, net als elke beveiligings-principal. In dit voorbeeld ziet u hoe een door de gebruiker toegewezen MSI-toegang te verlenen tot een Azure storage-account met Azure CLI.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de knop 'Deze probeer' zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken. Meld u aan met behulp van Azure [az aanmelding](/cli/azure/#login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt de gebruiker toegewezen MSI en VM implementeren:

   ```azurecli
   az login
   ```

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Gebruikmaken van RBAC de MSI-toegang toewijzen aan een andere bron

Als u wilt gebruiken een MSI-bestand met de bron voor een host (zoals een VM), voor aanmelden of resource toegang, moet het MSI-bestand eerst worden verleend toegang tot bedrijfsbronnen via roltoewijzing. U kunt dit doen voordat u koppelt het MSI-bestand met de host of na. Zie voor de volledige procedure voor het maken en koppelen aan een virtuele machine [een MSI gebruiker toegewezen configureren voor een virtuele machine met Azure CLI](msi-qs-configure-cli-windows-vm.md).

In het volgende voorbeeld wordt een MSI gebruiker toegewezen toegang krijgt tot een opslagaccount.  

1. Om te kunnen geven de MSI-toegang, moet u de client-ID (ook wel bekend als een app-ID) van het MSI service-principal. De client-ID is opgegeven door [az identiteit maken](/cli/azure/identity#az_identity_create), bij het inrichten van het MSI-bestand en de service-principal (ter referentie hier weergegeven):

   ```azurecli-interactive
   az identity create -g rgPrivate -n msiServiceApp
   ```

   Een geslaagde reactie bevat de client-ID van de gebruiker toegewezen MSI service-principal in de `clientId` eigenschap:

   ```json
   {
        "clientId": "9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp/credentials?tid=733a8f0p-ec41-4e69-8adz-971fc4b533bl&oid=z4baa4fc-fce4-4202-8250-5fb359abblll&aid=9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp",
        "location": "westcentralus",
        "name": "msiServiceApp",
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "resourceGroup": "rgPrivate",
        "tags": {},
        "tenantId": "733a8f0p-ec41-4e69-8adz-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
   }
   ```

2. Zodra de client-ID bekend is, gebruikt u [az roltoewijzing maken](/cli/azure/role/assignment#az_role_assignment_create) de MSI-toegang toewijzen aan een andere resource. Zorg ervoor dat u uw client-ID voor de `--assignee` parameter en de overeenkomende abonnement-ID en resource groep een naam, geretourneerd tijdens het uitvoeren van `az identity create`. Hier wordt het MSI-bestand 'Lezer' toegang toegewezen aan een opslagaccount 'myStorageAcct' genoemd:

   ```azurecli-interactive
   az role assignment create --assignee 9391e5b1-dada-4a8z-834a-43ad44f296bl --role Reader --scope /subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

   Een geslaagde reactie lijkt op de volgende uitvoer:

   ```json
   {
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct/providers/Microsoft.Authorization/roleAssignments/f4766864-9493-43c6-91d7-abd131c3c017",
        "name": "f4766864-9493-43c6-91d7-abd131c3c017",
        "properties": {
            "additionalProperties": {
            "createdBy": null,
            "createdOn": "2017-12-21T20:49:37.5590544Z",
            "updatedBy": "pd78b21f-17a4-41az-b7db-9aadec3376bl",
            "updatedOn": "2017-12-21T20:49:37.5590544Z"
            },
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "roleDefinitionId": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "scope": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct"
        },
        "resourceGroup": "rgPrivate",
        "type": "Microsoft.Authorization/roleAssignments"
   }
   ```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor het inschakelen van een gebruiker toegewezen MSI op een Azure VM [configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine met Azure CLI](msi-qs-configure-cli-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

