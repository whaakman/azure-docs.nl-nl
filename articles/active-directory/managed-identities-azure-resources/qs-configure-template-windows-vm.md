---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure VM met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM, met een Azure Resource Manager-sjabloon.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 78920bfe000287daef7b4efcaa8339d599d6f57e
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578498"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-a-templates"></a>Configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van een sjablonen

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van de sjabloon van Azure Resource Manager-implementatie, u hoe u voor het uitvoeren van de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure-VM:

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend bent met met behulp van Azure Resource Manager-implementatiesjabloon, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
   - Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de initiële implementatie en opnieuw implementeren. Inschakelen van een systeem of een beheerde identiteit gebruiker toegewezen op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Bovendien standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/deployment-modes.md) implementaties.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie maakt u inschakelen en uitschakelen van een systeem toegewezen beheerde identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een Azure-VM of op een bestaande virtuele machine inschakelen

Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Om in te schakelen door het systeem toegewezen beheerde identiteit, het laden van de sjabloon in een editor, Ga naar de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie en voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   },
   ```

3. (Optioneel) Toevoegen van de identiteit van de VM beheerd voor uitbreiding van de Azure-resources als een `resources` element. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.  Gebruik de volgende syntaxis:

   >[!NOTE] 
   > De volgende voorbeelden wordt ervan uitgegaan dat een Windows VM-extensie (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux met behulp van `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen. De VM-extensie is gepland voor de afschaffing in januari 2019.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2018-06-01",
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

4. Wanneer u klaar bent, in de volgende secties moet worden toegevoegd aan de `resource` sectie van uw sjabloon en deze is vergelijkbaar met het volgende:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01",
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
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-managed-identity"></a>Beheerde identiteit van de virtuele machine door het systeem toegewezen voor een rol toewijzen

Nadat u het systeem toegewezen beheerde identiteit hebt ingeschakeld op de virtuele machine, kunt u het geven een rol, zoals **lezer** toegang tot de resourcegroep waarin deze is gemaakt.

Als u wilt een rol toewijzen aan van de virtuele machine door het systeem toegewezen identiteit, uw account moet de [Administrator voor gebruikerstoegang](/azure/role-based-access-control/built-in-roles#user-access-administrator) roltoewijzing.

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.
 
2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en voeg de volgende informatie zodat de virtuele machine **lezer** toegang tot de resourcegroep waarin deze is gemaakt.  De structuur van uw sjabloon kan variëren, afhankelijk van de editor en het implementatiemodel dat u kiest.
   
   Onder de `parameters` sectie voegt u het volgende toe:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Onder de `variables` sectie voegt u het volgende toe:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Onder de `resources` sectie voegt u het volgende toe:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-vm"></a>Uitschakelen van een systeem toegewezen beheerde identiteit van een Azure VM

Als u wilt verwijderen door het systeem toegewezen beheerde identiteit van een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Hebt u een virtuele machine die alleen door het systeem toegewezen beheerde identiteit heeft, kunt u deze uitschakelen door het wijzigen van het identiteitstype aan `None`.  
   
   **Microsoft.Compute/virtualMachines API-versie 2018-06-01**

   Als uw virtuele machine zowel de systeem- en de gebruiker toegewezen beheerde identiteiten is, verwijdert u `SystemAssigned` van de id-type en blijf aan de `UserAssigned` samen met de `userAssignedIdentities` woordenlijst waarden.

   **Microsoft.Compute/virtualMachines API-versie 2018-06-01**
   
   Als uw `apiVersion` is `2017-12-01` en uw virtuele machine heeft zowel system en beheerde identiteiten gebruiker toegewezen verwijderen `SystemAssigned` van de id-type en blijf aan de `UserAssigned` samen met de `identityIds` matrix van de gebruiker toegewezen identiteiten beheerd.  
   
Het volgende voorbeeld ziet u hoe een systeem toegewezen beheerde identiteit verwijderen uit een virtuele machine met geen beheerde gebruiker toegewezen identiteiten:

```JSON
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[parameters('vmName')]",
    "location": "[resourceGroup().location]",
    "identity": { 
        "type": "None"
}
```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie maakt u een gebruiker toegewezen beheerde identiteit met een Azure VM met behulp van Azure Resource Manager-sjabloon.

> [!Note]
> Zie voor het maken van een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager-sjabloon, [maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

 ### <a name="assign-a-user-assigned-managed-identity-to-an-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een Azure-VM

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Onder de `resources` -element de volgende vermelding voor het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan uw virtuele machine toevoegen.  Vervang `<USERASSIGNEDIDENTITY>` beheerd door de naam van de gebruiker toegewezen identiteit die u hebt gemaakt.

   **Microsoft.Compute/virtualMachines API-versie 2018-06-01**

   Als uw `apiVersion` is `2018-06-01`, uw beheerde gebruiker toegewezen identiteiten worden opgeslagen in de `userAssignedIdentities` woordenlijst indeling en de `<USERASSIGNEDIDENTITYNAME>` waarde moet worden opgeslagen in een variabele die is gedefinieerd in de `variables` gedeelte van uw sjabloon.

   ```json
   {
       "apiVersion": "2018-06-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachines API-versie 2017-12-01**
    
   Als uw `apiVersion` is `2017-12-01`, uw beheerde gebruiker toegewezen identiteiten worden opgeslagen in de `identityIds` matrix en de `<USERASSIGNEDIDENTITYNAME>` waarde moet worden opgeslagen in een variabele die is gedefinieerd in de `variables` gedeelte van uw sjabloon.
    
   ```json
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```
       

2. (Optioneel) Vervolgens onder de `resources` -element de volgende vermelding voor het toewijzen van de extensie beheerde identiteit aan uw virtuele machine (gepland voor de afschaffing in januari 2019) toevoegen. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. Gebruik de volgende syntaxis:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
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
    
3. Wanneer u klaar bent, in de volgende secties moet worden toegevoegd aan de `resource` sectie van uw sjabloon en deze is vergelijkbaar met het volgende:
   
   **Microsoft.Compute/virtualMachines API-versie 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01-preview",
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
    ]
   ```
   **Microsoft.Compute/virtualMachines API-versie 2017-12-01**
   
   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },
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
    ]
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure VM

Als u wilt verwijderen van een gebruiker toegewezen identiteit van een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Hebt u een virtuele machine die alleen door de gebruiker toegewezen beheerde identiteit heeft, kunt u deze uitschakelen door het wijzigen van het identiteitstype aan `None`.
 
   Het volgende voorbeeld ziet u hoe alle beheerde identiteiten die door de gebruiker toegewezen verwijderen uit een virtuele machine met geen systeem toegewezen beheerde identiteiten:
   
   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```
   
   **Microsoft.Compute/virtualMachines API-versie 2018-06-01**
    
   Als u een enkele gebruiker toegewezen beheerde identiteit van een virtuele machine, wilt verwijderen uit de `useraAssignedIdentities` woordenlijst.

   Als u een beheerde identiteit voor het systeem toegewezen hebt, moet u deze de in de `type` waarde onder de `identity` waarde.
 
   **Microsoft.Compute/virtualMachines API-versie 2017-12-01**

   Als u een enkele gebruiker toegewezen beheerde identiteit van een virtuele machine, wilt verwijderen uit de `identityIds` matrix.

   Als u een beheerde identiteit voor het systeem toegewezen hebt, moet u deze de in de `type` waarde onder de `identity` waarde.
   
## <a name="next-steps"></a>Volgende stappen

- [Identiteiten voor een overzicht van Azure-resources beheerd](overview.md).

