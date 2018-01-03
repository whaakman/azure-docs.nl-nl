---
title: Het configureren van een gebruiker toegewezen MSI voor een Azure-VM met een Azure-sjabloon
description: Stap door stapsgewijze instructies voor het configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine van Azure, met een Azure Resource Manager-sjabloon.
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
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d97f0fa2d6c1c92aaa3d5c74dd6715de00d32438
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-template"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) configureren voor een virtuele machine met een Azure-sjabloon

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u het inschakelen en een gebruiker toegewezen MSI verwijderen voor een virtuele machine van Azure, met een Azure Resource Manager-implementatiesjabloon.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>MSI tijdens het maken van een virtuele machine in Azure of op een bestaande virtuele machine inschakelen

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](~/articles/azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](~/articles/azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](~/articles/azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS-Code)](~/articles/azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
   - Met de Visual Studio [Azure-resourcegroepproject](~/articles/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de eerste implementatie en opnieuw installeren. Het maken en toewijzen van een MSI gebruiker toegewezen aan een nieuwe of bestaande virtuele machine wordt op dezelfde manier gedaan. Ook standaard Azure Resource Manager biedt een [incrementele update](~/articles/azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) voor implementaties:

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de MSI en de VM. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen voor het abonnement of de bronnen (bijvoorbeeld de rol van de 'eigenaar').

2. Na het laden van de sjabloon in een editor, zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Jouw e-mailadres eruit enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor een nieuwe implementatie of een bestaande set.

   >[!NOTE] 
   > Hierbij wordt verondersteld variabelen, zoals `vmName`, `storageAccountName`, en `nicName` zijn gedefinieerd in de sjabloon.
   >

   ![Schermopname van sjabloon: zoek VM](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

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

   ![Schermafbeelding van de sjabloon na de update](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

Als u een virtuele machine die niet langer een MSI-bestand hebt:

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

2. Verwijder de twee elementen die zijn toegevoegd in het vorige gedeelte: van de VM `"identity"` eigenschap en de `"Microsoft.Compute/virtualMachines/extensions"` resource.

## <a name="related-content"></a>Gerelateerde inhoud

- Lees voor een breder perspectief over MSI de [overzicht van de Service-identiteit beheerd](msi-overview.md).

