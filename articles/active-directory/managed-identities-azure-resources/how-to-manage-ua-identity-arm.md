---
title: Over het maken en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure Resource Manager
description: Stapsgewijze beheerde instructies over het maken en verwijderen door de gebruiker toegewezen identiteiten met Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 611a2deccffeb142dc9a76bd260c9a8a080eb8bb
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56816331"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met Azure Resource Manager

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel maakt u een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager.

Het is niet mogelijk om te vermelden en verwijderen van een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager-sjabloon.  Zie de volgende artikelen voor het maken en een gebruiker toegewezen lijst met beheerde identiteit:

- [Lijst met door de gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Verwijderen van de gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="template-creation-and-editing"></a>Sjabloon maken en bewerken

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

- Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
- Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
- Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon. 

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken 

Voor het maken van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Gebruik de volgende sjabloon voor het maken van een gebruiker toegewezen beheerde identiteit. Vervang de `<USER ASSIGNED IDENTITY NAME>` waarde door uw eigen waarden:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

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
      "apiVersion": "2018-11-30",
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
## <a name="next-steps"></a>Volgende stappen

Beheerde identiteit met een Azure-VM met behulp van een Azure Resource Manager-sjabloon Zie, voor informatie over het toewijzen van een gebruiker toegewezen [configureren beheerde identiteiten voor een Azure-resources op een Azure-VM met behulp van een sjablonen](qs-configure-template-windows-vm.md).


 
