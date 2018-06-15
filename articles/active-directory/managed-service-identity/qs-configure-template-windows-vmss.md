---
title: MSI configureren op een virtuele machine van Azure-schaal instelt met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een Azure-VMSS, met een Azure Resource Manager-sjabloon.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: f7c5d063bfb287de9afe808395b951ecb161da69
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930609"
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Een VMSS beheerde Service-identiteit configureren met behulp van een sjabloon

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe op de volgende Service-identiteit beheerd bewerkingen uitvoeren op een Azure-VMSS, met behulp van Azure Resource Manager-implementatiesjabloon:
- Inschakelen en uitschakelen van het systeem toegewezen identiteit op een Azure-VMSS
- Toevoegen en verwijderen van een gebruiker met de identiteit op een Azure-VMSS

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij de Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS-Code)](../../azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
   - Met de Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de eerste implementatie en opnieuw installeren. Inschakelen van MSI op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Ook standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) voor implementaties.

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

In deze sectie kunt u inschakelen en uitschakelen van het systeem toegewezen identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Automatisch toegewezen identiteit bij het maken van een Azure-VMSS of een bestaande Azure-VMSS inschakelen

1. De sjabloon wordt geladen in een editor, zoek de `Microsoft.Compute/virtualMachineScaleSets` resource van belang zijn binnen de `resources` sectie. Jouw e-mailadres eruit enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor een nieuwe implementatie of een bestaande set.
   
   ![Schermopname van sjabloon: zoek VM](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Toevoegen om de systeemidentiteit die zijn toegewezen, de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachineScaleSets"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Optioneel) Toevoegen van de schaal van de virtuele machine ingesteld MSI-extensie als een `extensionsProfile` element. Deze stap is optioneel, zoals u de identiteit van de Azure exemplaar metagegevens Service (IMDS), kunt evenals tokens op te halen.  Gebruik de volgende syntaxis:

   >[!NOTE] 
   > Het volgende voorbeeld wordt ervan uitgegaan dat een virtuele-machineschaalset Windows extensie instellen (`ManagedIdentityExtensionForWindows`) wordt geïmplementeerd. U kunt ook configureren voor Linux via `ManagedIdentityExtensionForLinux` in plaats daarvan voor de `"name"` en `"type"` elementen.
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

4. Wanneer u bent klaar, ziet de sjabloon er ongeveer als volgt:

   ![Schermafbeelding van de sjabloon na de update](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Een toegewezen identiteit van een Azure virtuele-machineschaalset uitschakelen

> [!NOTE]
> Het uitschakelen van de Service-identiteit beheerd vanaf een virtuele Machine is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een set die niet meer virtuele-machineschaalset moet een systeem toegewezen identiteit maar nog steeds moet identiteiten toegewezen door gebruiker:

- De sjabloon wordt geladen in een teksteditor en wijzig de identiteit aan `'UserAssigned'`

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

In deze sectie wijst u de identiteit van een gebruiker is toegewezen aan een Azure-VMSS met Azure Resource Manager-sjabloon.

> [!Note]
> Zie de identiteit van een gebruiker aan zijn toegewezen met een Azure Resource Manager-sjabloon maken [maken van de identiteit van een gebruiker toegewezen](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Een gebruiker identiteit toegewezen aan een Azure-VMSS toewijzen

1. Onder de `resources` element, de volgende vermelding voor het toewijzen van de identiteit van een gebruiker is toegewezen aan uw VMSS toevoegen.  Zorg ervoor dat u `<USERASSIGNEDIDENTITY>` met de naam van de identiteit van de gebruiker toegewezen die u hebt gemaakt.

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/<USERASSIGNEDIDENTITY>)']"
            ]
        }

    }
    ```
2. (Optioneel) Voeg de volgende vermelding onder de `extensionProfile` element op de extensie beheerde identity toewijzen aan uw VMSS. Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen. Gebruik de volgende syntaxis:
   
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
3.  Wanneer u klaar bent, ziet de sjabloon er ongeveer als volgt:
   
      ![Schermopname van toegewezen gebruikers-id](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

## <a name="next-steps"></a>Volgende stappen

- Lees voor een breder perspectief over MSI de [overzicht van de Service-identiteit beheerd](overview.md).

