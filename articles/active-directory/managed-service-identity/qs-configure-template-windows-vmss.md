---
title: MSI configureren op een schaalset van de virtuele machine van Azure met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een Managed Service Identity (MSI) op een Azure-VMSS, met een Azure Resource Manager-sjabloon.
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
ms.openlocfilehash: ab3982c85cfb008bde08495f8cb8aa86d066d8c0
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114851"
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Een VMSS beheerde Service-identiteit configureren met behulp van een sjabloon

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende bewerkingen van de beheerde Service-identiteit in een Azure-VMSS, met behulp van Azure Resource Manager-implementatiesjabloon uitvoeren:
- In- en uitschakelen van het systeem toegewezen identiteit in een Azure VMSS
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit in een Azure VMSS

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
   - Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de initiële implementatie en opnieuw implementeren. Inschakelen van MSI op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Bovendien standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementaties.

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie maakt u inschakelen en uitschakelen van het systeem toegewezen identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Inschakelen van systeem toegewezen identiteit tijdens het maken van een Azure-VMSS of een bestaande Azure VMSS

1. Laden van de sjabloon in een editor, Ga naar de `Microsoft.Compute/virtualMachineScaleSets` resource van belang zijn binnen de `resources` sectie. Uw uitzien enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor de implementatie van een nieuwe of bestaande resourcegroep.
   
   ![Schermafbeelding van de sjabloon: zoek VM](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Om in te schakelen op het systeem toegewezen identiteit, voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachineScaleSets"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Optioneel) Voeg de virtuele-machineschaalset MSI-extensie als een `extensionsProfile` element. Deze stap is optioneel, zoals u de identiteit van de Azure Instance Metadata Service (IMDS), kunt evenals tokens op te halen.  Gebruik de volgende syntaxis:

   >[!NOTE] 
   > Het volgende voorbeeld wordt ervan uitgegaan dat een virtuele-machineschaalset in Windows extensie instellen (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux met behulp van `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen.
   >

   ```JSON
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
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

4. Wanneer u klaar bent, is de sjabloon ziet die vergelijkbaar is met het volgende:

   ![Schermafbeelding van de sjabloon na-update](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Een systeem toegewezen identiteit van een schaalset voor virtuele Azure-machine uitschakelen

> [!NOTE]
> Uitschakelen van de beheerde Service-identiteit van een virtuele Machine wordt momenteel niet ondersteund. In de tussentijd kunt u overschakelen tussen het gebruik van het systeem toegewezen en de gebruiker toegewezen identiteiten.

Hebt u een virtuele-machineschaalset die niet meer nodig is op een systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten:

- Laden van de sjabloon in een teksteditor en wijzig de identiteit aan `'UserAssigned'`

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie wijst u de identiteit van een gebruiker toegewezen aan een Azure-VMSS met behulp van Azure Resource Manager-sjabloon.

> [!Note]
> Zie voor het maken van een gebruiker toegewezen identiteit met een Azure Resource Manager-sjabloon, [maken van een gebruiker toegewezen identiteit](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Een gebruiker toegewezen identiteit aan een Azure-VMSS toewijzen

1. Onder de `resources` -element de volgende vermelding voor het toewijzen van de identiteit van een gebruiker toegewezen aan uw VMSS toevoegen.  Vervang `<USERASSIGNEDIDENTITY>` met de naam van de toegewezen gebruikers-id die u hebt gemaakt.

   > [!Important]
   > De `<USERASSIGNEDIDENTITYNAME>` waarde die wordt weergegeven in het volgende voorbeeld moet worden opgeslagen in een variabele.  Ook voor de momenteel ondersteunde implementatie van een gebruiker toegewezen identiteiten toewijzen aan een virtuele machine in een Resource Manager-sjabloon, de api-versie moet overeenkomen met de versie in het volgende voorbeeld. 

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
2. (Optioneel) Voeg de volgende vermelding onder de `extensionProfile` element de extensie beheerde identiteit toewijzen aan uw VMSS. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. Gebruik de volgende syntaxis:
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "MSIWindowsExtension",
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
3.  Wanneer u klaar bent, is de sjabloon ziet die vergelijkbaar is met het volgende:
   
      ![Schermafbeelding van de gebruiker toegewezen identiteit](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

## <a name="next-steps"></a>Volgende stappen

- Voor een breder perspectief over MSI leest de [overzicht van de beheerde Service-identiteit](overview.md).

