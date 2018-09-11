---
title: Configureren van beheerde identiteiten voor Azure-resources op een VM-schaalset met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van beheerde identiteiten voor Azure-resources op een virtuele-machineschaalset instellen met behulp van een Azure Resource Manager-sjabloon.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 8d897e9948a241f39d06968c3438ce2d3b0c6940
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347816"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-azure-virtual-machine-scale-using-a-template"></a>Configureren van beheerde identiteiten voor Azure-resources op een virtuele machine van Azure-schaal met behulp van een sjabloon

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u voor het uitvoeren van de volgende beheerde identiteiten voor bewerkingen op een schaalset voor virtuele machine van Azure, Azure-resources met behulp van Azure Resource Manager-implementatiesjabloon:
- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit in een schaalset voor virtuele Azure-machine
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit in een schaalset voor virtuele Azure-machine

## <a name="prerequisites"></a>Vereiste onderdelen

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:
    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele-machineschaalset maken en inschakelen en verwijderen van systeem en/of de gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele-machineschaalset.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
   - Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de initiële implementatie en opnieuw implementeren. Inschakelen van beheerde identiteiten voor Azure-resources op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Bovendien standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/deployment-modes.md) implementaties.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie maakt u inschakelen en uitschakelen van de door het systeem toegewezen beheerde identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-a-existing-virtual-machine-scale-set"></a>Inschakelen door het systeem toegewezen beheerd identiteit tijdens het maken van het maken van een schaalset voor virtuele machines of een bestaande virtuele-machineschaalset

1. Of u zich aanmeldt bij Azure lokaal of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.
   
2. Om in te schakelen op het systeem toegewezen beheerde identiteit, het laden van de sjabloon in een editor, Ga naar de `Microsoft.Compute/virtualMachinesScaleSets` resource van belang zijn binnen de resources sectie en voeg de `identity` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

3. (Optioneel) Voeg de virtuele-machineschaalset beheerde identiteiten voor uitbreiding van de Azure-resources als een `extensionsProfile` element. Deze stap is optioneel, zoals u de identiteit van de Azure Instance Metadata Service (IMDS), kunt evenals tokens op te halen.  Gebruik de volgende syntaxis:

   >[!NOTE] 
   > Het volgende voorbeeld wordt ervan uitgegaan dat een virtuele-machineschaalset in Windows extensie instellen (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux met behulp van `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen.
   >

   ```json
   "extensionProfile": {
        "extensions": [
            {
                "name": "ManagedIdentityWindowsExtension",
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

4. Wanneer u klaar bent, wordt de volgende secties moeten toegevoegd aan de resource-sectie van uw sjabloon en is vergelijkbaar met het volgende:

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ``` 

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Een systeem toegewezen beheerde identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u een virtuele-machineschaalset die een door het systeem toegewezen beheerde identiteit niet meer nodig hebt:

1. Of u zich aanmeldt bij Azure lokaal of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachineScaleSets` resource van belang zijn binnen de `resources` sectie. Hebt u een virtuele machine die alleen door het systeem toegewezen beheerde identiteit heeft, kunt u deze uitschakelen door het wijzigen van het identiteitstype aan `None`.

   **Microsoft.Compute/virtualMachineScaleSets API-versie 2018-06-01**

   Als uw apiVersion `2018-06-01` en uw virtuele machine heeft zowel system en beheerde identiteiten gebruiker toegewezen verwijderen `SystemAssigned` van de id-type en blijf aan de `UserAssigned` samen met de waarden van de woordenlijst userAssignedIdentities.

   **Microsoft.Compute/virtualMachineScaleSets API-versie 2018-06-01 en oudere versies**

   Als uw apiVersion `2017-12-01` en uw virtuele-machineschaalset is zowel system als beheerde identiteiten gebruiker toegewezen verwijderen `SystemAssigned` van de id-type en blijf aan de `UserAssigned` samen met de `identityIds` matrix van de gebruiker toegewezen beheerd identiteiten. 
   
    

   Het volgende voorbeeld ziet u hoe een systeem toegewezen beheerde identiteit verwijderen uit een virtuele-machineschaalset met geen beheerde gebruiker toegewezen identiteiten:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie maakt toewijzen u een gebruiker toegewezen beheerde identiteit aan een VM-schaalset met behulp van Azure Resource Manager-sjabloon.

> [!Note]
> Zie voor het maken van een gebruiker toegewezen beheerde identiteit met een Azure Resource Manager-sjabloon, [maken van een gebruiker toegewezen beheerde identiteit](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-a-virutal-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een virtuele-machineschaalset

1. Onder de `resources` -element, voeg de volgende vermelding voor het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan uw virtuele-machineschaalset.  Vervang `<USERASSIGNEDIDENTITY>` beheerd door de naam van de gebruiker toegewezen identiteit die u hebt gemaakt.
   
   **Microsoft.Compute/virtualMachineScaleSets API-versie 2018-06-01**

   Als uw apiVersion `2018-06-01`, uw beheerde gebruiker toegewezen identiteiten worden opgeslagen in de `userAssignedIdentities` woordenlijst indeling en de `<USERASSIGNEDIDENTITYNAME>` waarde moet worden opgeslagen in een variabele die is gedefinieerd in de `variables` gedeelte van uw sjabloon.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }
    
   }
   ```   

   **Microsoft.Compute/virtualMachineScaleSets API-versie 2017-12-01**
    
   Als uw `apiVersion` is `2017-12-01` of eerder, uw beheerde gebruiker toegewezen identiteiten worden opgeslagen in de `identityIds` matrix en de `<USERASSIGNEDIDENTITYNAME>` waarde moet worden opgeslagen in een variabele die is gedefinieerd in de sectie met variabelen van uw sjabloon.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ``` 

2. (Optioneel) Voeg de volgende vermelding onder de `extensionProfile` element op de beheerde identiteiten voor uitbreiding van de Azure-resources toewijzen aan uw VMSS. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. Gebruik de volgende syntaxis:
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "ManagedIdentityWindowsExtension",
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

3. Wanneer u klaar bent, is de sjabloon ziet die vergelijkbaar is met het volgende:
   
   **Microsoft.Compute/virtualMachineScaleSets API-versie 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```

   **Eerder Microsoft.Compute/virtualMachines API-versie 2017-12-01-eand**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```
### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Verwijder de gebruiker toegewezen beheerde identiteit van een schaalset voor virtuele Azure-machine

Als u een virtuele-machineschaalset die een gebruiker toegewezen beheerde identiteit niet meer nodig hebt:

1. Of u zich aanmeldt bij Azure lokaal of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachineScaleSets` resource van belang zijn binnen de `resources` sectie. Als u een virtuele-machineschaalset waarvoor alleen beheerde identiteit gebruiker toegewezen hebt, kunt u deze uitschakelen door het veranderen van de id-type naar `None`.

   Het volgende voorbeeld ziet u hoe alle beheerde identiteiten die door de gebruiker toegewezen verwijderen uit een virtuele machine met geen systeem toegewezen beheerde identiteiten:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachineScaleSets API-versie 2018-06-01**
    
   Verwijderen van een een enkele gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset verwijderen uit de `userAssignedIdentities` woordenlijst.

   Als u een systeem toegewezen identiteit hebt, moet u deze de in de `type` waarde onder de `identity` waarde.

   **Microsoft.Compute/virtualMachineScaleSets API-versie 2017-12-01**

   Als u een enkele gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset, wilt verwijderen uit de `identityIds` matrix.

   Als u een beheerde identiteit voor het systeem toegewezen hebt, moet u deze de in de `type` waarde onder de `identity` waarde.
   
## <a name="next-steps"></a>Volgende stappen

- [Identiteiten voor een overzicht van Azure-resources beheerd](overview.md).

