---
title: Het maken en verwijderen van een gebruiker toegewezen beheerde Service-identiteit met Azure Resource Manager
description: Stapsgewijze instructies voor het maken en verwijderen van de gebruiker toegewezen beheerde Service-identiteit met behulp van Azure Resource.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 96fd86a2af3372f53973c95ee11ab35ccee1e63e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Maken, weergeven en verwijderen van de identiteit van een gebruiker aan zijn toegewezen met Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel maakt u een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager.

Het is niet mogelijk weergeven en verwijderen van een gebruiker toegewezen identiteit met een Azure Resource Manager-sjabloon.  Zie de volgende artikelen maken en weergeven van een toegewezen gebruikers-id:

- [Toegewezen identiteit lijst door gebruiker](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Identiteit van de gebruiker toegewezen verwijderen](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

## <a name="template-creation-and-editing"></a>Sjablonen maken en bewerken

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

- Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
- Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Met behulp van een lokale [JSON-editor (zoals VS-Code)](../../azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
- Met de Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon. 

## <a name="create-a-user-assigned-identity"></a>Een gebruiker toegewezen identiteit maken 

Gebruik de volgende sjabloon voor het maken van een gebruiker toegewezen identiteit. Vervang de `<USER ASSIGNED IDENTITY NAME>` waarde met uw eigen waarden:

> [!IMPORTANT]
> Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

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
      "name": "[parameters('<USER ASSIGNED IDENTITY NAME>')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('<USER ASSIGNED IDENTITY NAME>')]"
      }
  }
}
```
## <a name="related-content"></a>Gerelateerde inhoud

Voor informatie over het toewijzen van de identiteit van een gebruiker is toegewezen aan een Azure-VM met een Zie Azure Resource Manager-sjabloon [een VM beheerde Service-identiteit configureren met behulp van een sjabloon](qs-configure-template-windows-vm.md).


 
