---
title: Over het maken en verwijderen van een gebruiker toegewezen beheerde Service-identiteit met Azure Resource Manager
description: Stapsgewijze instructies over het maken en verwijderen van de gebruiker toegewezen beheerde Service-identiteit met behulp van Azure-Resource.
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
ms.openlocfilehash: ced2a8354e63288ad9957b6a177b43c97b58698c
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160532"
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel maakt u een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager.

Het is niet mogelijk om te vermelden en verwijderen van een gebruiker toegewezen identiteit met een Azure Resource Manager-sjabloon.  Zie de volgende artikelen voor het maken en weergeven van een toegewezen gebruikers-id:

- [Lijst met door gebruiker toegewezen identiteit](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Door gebruiker toegewezen identiteit verwijderen](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u beschikt over machtigingen voor schrijven op de virtuele machine (bijvoorbeeld de rol 'Inzender voor virtuele machines').

## <a name="template-creation-and-editing"></a>Sjabloon maken en bewerken

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

- Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
- Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
- Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon. 

## <a name="create-a-user-assigned-identity"></a>Door de gebruiker toegewezen identiteit maken 

Gebruik de volgende sjabloon voor het maken van een gebruiker toegewezen identiteit. Ten minste uw account moet worden toegewezen de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen. Vervang de `<USER ASSIGNED IDENTITY NAME>` waarde door uw eigen waarden:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="related-content"></a>Gerelateerde inhoud

Voor informatie over het toewijzen van de identiteit van een gebruiker toegewezen aan een Azure-VM met behulp van een Azure Resource Manager sjabloon-Zie [configureren van een virtuele machine beheerde Service-identiteit met een sjabloon](qs-configure-template-windows-vm.md).


 
