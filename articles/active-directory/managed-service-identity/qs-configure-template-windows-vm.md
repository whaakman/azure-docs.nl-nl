---
title: Het configureren van MSI op een virtuele machine in Azure met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met een Azure Resource Manager-sjabloon.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 324a1e08e92a2c7ae76d7a6df56536540dc772a1
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Een VM beheerde Service-identiteit configureren met behulp van een sjabloon

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe op de volgende Service-identiteit beheerd bewerkingen uitvoeren op een virtuele machine van Azure, Azure Resource Manager-implementatiesjabloon met:

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij de Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS-Code)](../../azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
   - Met de Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de eerste implementatie en opnieuw installeren. Inschakelen van een systeem of de gebruiker toegewezen identiteit op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Ook standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) voor implementaties.

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

In deze sectie kunt u inschakelen en uitschakelen van een systeem toegewezen identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Automatisch toegewezen identiteit tijdens het maken van een virtuele machine in Azure of op een bestaande virtuele machine inschakelen

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

2. Na het laden van de sjabloon in een editor, zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Jouw e-mailadres eruit enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor een nieuwe implementatie of een bestaande set.

   >[!NOTE] 
   > Hierbij wordt verondersteld variabelen, zoals `vmName`, `storageAccountName`, en `nicName` zijn gedefinieerd in de sjabloon.
   >

   ![Schermopname van sjabloon: zoek VM](../media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Om automatisch toegewezen identiteit, voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Optioneel) Toevoegen van de VM MSI-extensie als een `resources` element. Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen.  Gebruik de volgende syntaxis:

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

   ![Schermafbeelding van de sjabloon na de update](../media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Uitschakelen van een systeem toegewezen identiteit van een Azure VM

> [!NOTE]
> Het uitschakelen van de Service-identiteit beheerd vanaf een virtuele Machine is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een virtuele machine die niet langer nodig een beheerde service-identiteit heeft hebt:

1. Of u lokaal bij Azure aanmelden of gebruik een account dat is gekoppeld aan het Azure-abonnement via de Azure-portal bevat die de virtuele machine. Zorg er ook voor dat uw account deel uitmaakt van een functie waarmee u schrijfmachtigingen heeft op de virtuele machine (bijvoorbeeld de rol 'Virtual Machine Contributor').

2. Wijzigen van het identiteitstype `UserAssigned`.

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

In deze sectie wijst u een toegewezen gebruikers-id aan een virtuele machine in Azure met Azure Resource Manager-sjabloon.

> [!Note]
> Zie de identiteit van een gebruiker aan zijn toegewezen met een Azure Resource Manager-sjabloon maken [maken van de identiteit van een gebruiker toegewezen](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Een gebruiker identiteit toegewezen aan een Azure VM toewijzen

1. Onder de `resources` element, de volgende vermelding voor het toewijzen van de identiteit van een gebruiker toegewezen aan uw virtuele machine toevoegen.  Zorg ervoor dat u `<USERASSIGNEDIDENTITY>` met de naam van de identiteit van de gebruiker toegewezen die u hebt gemaakt.
    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/<USERASSIGNEDIDENTITYNAME>)']"
            ]
        },
    ```
    
2. (Optioneel) Vervolgens onder de `resources` element, de volgende vermelding voor de extensie beheerde identity toewijzen aan uw virtuele machine toevoegen. Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen. Gebruik de volgende syntaxis:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2015-05-01-preview",
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
            }
        }
    }
    ```
    
3.  Wanneer u klaar bent, ziet de sjabloon er ongeveer als volgt:

      ![Schermopname van toegewezen gebruikers-id](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)


## <a name="related-content"></a>Gerelateerde inhoud

- Lees voor een breder perspectief over MSI de [overzicht van de Service-identiteit beheerd](overview.md).

