---
title: Het configureren van MSI op een virtuele machine in Azure met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met een Azure Resource Manager-sjabloon.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 047c4f69ae520c730d063246d41702207c55a0b0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Een VM beheerde Service-identiteit configureren met behulp van een sjabloon

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit (MSI) biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor een virtuele machine van Azure, met een Azure Resource Manager-implementatiesjabloon.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>MSI tijdens het maken van een virtuele machine in Azure of op een bestaande virtuele machine inschakelen

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS-Code)](../azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
   - Met de Visual Studio [Azure-resourcegroepproject](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de eerste implementatie en opnieuw installeren. Inschakelen van MSI op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Ook standaard Azure Resource Manager biedt een [incrementele update](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) voor implementaties:

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

2. Na het laden van de sjabloon in een editor, zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Jouw e-mailadres eruit enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor een nieuwe implementatie of een bestaande set.

   >[!NOTE] 
   > Hierbij wordt verondersteld variabelen, zoals `vmName`, `storageAccountName`, en `nicName` zijn gedefinieerd in de sjabloon.
   >

   ![Schermopname van sjabloon: zoek VM](./media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Voeg de uitbreiding VM MSI als een `resources` element. Gebruik de volgende syntaxis:

   >[!NOTE] 
   > Het volgende voorbeeld wordt ervan uitgegaan dat een Windows-VM-extensie (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux via `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

5. Wanneer u bent klaar, ziet de sjabloon er ongeveer als volgt:

   ![Schermafbeelding van de sjabloon na de update](./media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

Als u een virtuele machine die niet langer een MSI-bestand hebt:

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

2. Verwijder de twee elementen die zijn toegevoegd in het vorige gedeelte: van de VM `"identity"` eigenschap en de `"Microsoft.Compute/virtualMachines/extensions"` resource.

## <a name="related-content"></a>Gerelateerde inhoud

- Lees voor een breder perspectief over MSI de [overzicht van de Service-identiteit beheerd](msi-overview.md).

