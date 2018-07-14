---
title: Een gebruiker toegewezen MSI configureren voor een Azure-VM met behulp van een Azure-sjabloon
description: Voor stap door stap instructies voor het configureren van een gebruiker toegewezen Managed Service Identity (MSI) voor een Azure-VM met een Azure Resource Manager-sjabloon.
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 1af9770fa899a03e6e3514c539c511ba26fdced4
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036377"
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-template"></a>Configureren van een gebruiker toegewezen Managed Service Identity (MSI) voor een VM met behulp van een Azure-sjabloon

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u inschakelen en verwijderen van een gebruiker toegewezen MSI voor een Azure-VM met een Azure Resource Manager-implementatiesjabloon.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>MSI-bestand tijdens het maken van een Azure-VM, of op een bestaande virtuele machine inschakelen

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](~/articles/azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](~/articles/azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](~/articles/azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS Code)](~/articles/azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
   - Met Visual Studio [Azure-resourcegroepproject](~/articles/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de initiële implementatie en opnieuw implementeren. Het maken en toewijzen van een MSI gebruiker toegewezen aan een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Bovendien standaard Azure Resource Manager biedt een [incrementele update](~/articles/azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementaties:

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat die de MSI-bestand en de VM. Zorg er ook voor dat uw account deel uitmaakt van een rol die u hebt schrijfmachtigingen heeft voor het abonnement of de resources (bijvoorbeeld de rol van de 'eigenaar').

2. Na het laden van de sjabloon in een editor, Ga naar de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Uw uitzien enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor de implementatie van een nieuwe of bestaande resourcegroep.

   >[!NOTE] 
   > Dit voorbeeld wordt aangenomen variabelen zoals `vmName`, `storageAccountName`, en `nicName` zijn gedefinieerd in de sjabloon.
   >

   ![Schermafbeelding van de sjabloon: zoek VM](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Voeg de extensie VM MSI als een `resources` element. Gebruik de volgende syntaxis:

   >[!NOTE] 
   > Het volgende voorbeeld wordt ervan uitgegaan dat een Windows VM-extensie (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux met behulp van `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen.
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

5. Wanneer u klaar bent, is de sjabloon ziet die vergelijkbaar is met het volgende:

   ![Schermafbeelding van de sjabloon na-update](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>MSI-bestand verwijderen uit een Azure-VM

Als u een virtuele machine die een MSI-bestand niet meer nodig hebt:

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u beschikt over machtigingen voor schrijven op de virtuele machine (bijvoorbeeld de rol 'Inzender voor virtuele machines').

2. Verwijderen van de twee elementen die in de vorige sectie zijn toegevoegd: van de virtuele machine `"identity"` eigenschap en de `"Microsoft.Compute/virtualMachines/extensions"` resource.

## <a name="related-content"></a>Gerelateerde inhoud

- Voor een breder perspectief over MSI leest de [overzicht van de beheerde Service-identiteit](msi-overview.md).

