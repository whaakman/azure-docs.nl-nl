---
title: MSI configureren op een virtuele machine van Azure-schaal instelt met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een Azure-VMSS, met een Azure Resource Manager-sjabloon.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 64fe217cf3d845e6a09fe67d03648e79e8a4cadd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
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

Als met de Azure bieden portal en schrijven van scripts en Azure Resource Manager-sjablonen de mogelijkheid voor het implementeren van nieuwe of gewijzigde bronnen die zijn gedefinieerd door een Azure-resourcegroep. Er zijn diverse opties beschikbaar voor het bewerken van sjablonen en implementatie van zowel lokale als portal-gebaseerde, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u kunt een sjabloon maken vanaf het begin of baseren op een bestaande gemeenschappelijke of [snelstartsjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die zijn afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS-Code)](../../azure-resource-manager/resource-manager-create-first-template.md), uploaden en implementeren met behulp van PowerShell of CLI.
   - Met de Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) zowel maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de eerste implementatie en opnieuw installeren. Inschakelen van MSI op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Ook standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) voor implementaties.

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

In deze sectie kunt u inschakelen en uitschakelen van het systeem toegewezen identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Automatisch toegewezen identiteit bij het maken van een Azure-VMSS of een bestaande Azure-VMSS inschakelen

1. Of u bij Azure lokaal of via de Azure-portal aanmelden, moet u een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset gebruiken.

2. Na het laden van de sjabloon in een editor, zoek de `Microsoft.Compute/virtualMachineScaleSets` resource van belang zijn binnen de `resources` sectie. Jouw e-mailadres eruit enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor een nieuwe implementatie of een bestaande set.
   
   ![Schermopname van sjabloon: zoek VM](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

3. Toevoegen om de systeemidentiteit die zijn toegewezen, de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachineScaleSets"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Optioneel) Toevoegen van de schaal van de virtuele machine ingesteld MSI-extensie als een `extensionsProfile` element. Deze stap is optioneel, zoals u de identiteit van de Azure exemplaar metagegevens Service (IMDS), kunt evenals tokens op te halen.  Gebruik de volgende syntaxis:

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

5. Wanneer u bent klaar, ziet de sjabloon er ongeveer als volgt:

   ![Schermafbeelding van de sjabloon na de update](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Een toegewezen identiteit van een Azure virtuele-machineschaalset uitschakelen

> [!NOTE]
> Het uitschakelen van de Service-identiteit beheerd vanaf een virtuele Machine is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een set die niet meer virtuele-machineschaalset moet een systeem toegewezen identiteit maar nog steeds moet identiteiten toegewezen door gebruiker:

1. Of u bij Azure lokaal of via de Azure-portal aanmelden, moet u een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset gebruiken.

2. Het identiteitstype te wijzigen `'UserAssigned'`

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

In deze sectie kunt u een gebruiker toegewezen identiteits- en een Azure-VMSS met een Azure Resource Manager-sjabloon maken.

### <a name="create-and-assign-a-user-assigned-identity-to-an-azure-vmss"></a>Maken en toewijzen van een gebruiker identiteit toegewezen aan een Azure-VMSS

1. Voer de eerste de stap in de sectie [toegewezen identiteit inschakelen bij het maken van een Azure-VMSS, op een bestaande VMSS](qs-configure-template-windows-vmss.md#enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss).

2. Voeg een vermelding voor een toegewezen identiteit gebruikersnaam de volgende strekking onder het gedeelte variabelen die de variabelen voor uw Azure-VMSS bevat.  Dit bevat de waarde van de gebruiker toegewezen identiteit tijdens het maken van Azure VMSS:
    
    > [!IMPORTANT]
    > Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

    ```json
    "variables": {
        "vmssPrefix": "vmss",
        "vmssName": "[concat(variables('vmssPrefix'), uniquestring(resourceGroup().id,deployment().name))]",
        //other vm configuration variables...
        "identityName": "[concat(variables('vmssName'), 'id')]"
    ```
3. Onder de `resources` element toevoegen de volgende vermelding voor het maken van een toegewezen gebruikers-id:

    ```json
    {
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
        "name": "[variables('identityName')]",
        "apiVersion": "2015-08-31-PREVIEW",
        "location": "[resourceGroup().location]"
    },
    ```
4. Vervolgens onder de `resources` element de volgende vermelding voor het toewijzen van de identiteit van de gebruiker toegewezen aan uw VMSS toevoegen:

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/, variables('identityName'))]"
            ]
        }

    }
    ```
5. (Optioneel) Voeg de volgende vermelding onder de `extensionProfile` element op de extensie beheerde identity toewijzen aan uw VMSS. Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen. Gebruik de volgende syntaxis:
   
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
6.  Wanneer u klaar bent, ziet de sjabloon er ongeveer als volgt:
    > [!NOTE]
    > De sjabloon lijst bevat niet alle benodigde variabelen uw VMSS maken.  `//other configuration variables...` Plaats de vereiste configuratie-variabelen voor beknopt alternatief bevat wordt gebruikt.

      ![Schermopname van toegewezen gebruikers-id](../media/msi-qs-configure-template-windows-vmss/template-vmss-user-assigned-identity.png)

## <a name="next-steps"></a>Volgende stappen

- Lees voor een breder perspectief over MSI de [overzicht van de Service-identiteit beheerd](overview.md).

