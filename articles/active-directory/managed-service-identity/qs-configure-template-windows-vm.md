---
title: Beheerde Service-identiteit op een Azure-VM configureren met behulp van een sjabloon
description: Stapsgewijze instructies voor het configureren van een beheerde Service-identiteit op een Azure-VM, met een Azure Resource Manager-sjabloon.
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
ms.openlocfilehash: 4305ad8cfab911baccc4b7c4dc7ee9384dab06fd
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579473"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Een virtuele machine beheerde Service-identiteit configureren met behulp van een sjabloon

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de volgende bewerkingen van de beheerde Service-identiteit op een Azure-VM, met behulp van Azure Resource Manager-implementatiesjabloon uit te voeren:

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) toegewezen aan een virtuele machine maken en inschakelen en verwijderen van systeem en/of de gebruiker beheerde identiteit van een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele machine.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Net als bij Azure portal en uitvoeren van scripts, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sjablonen bieden de mogelijkheid om nieuwe of gewijzigde resources die zijn gedefinieerd door een Azure-resourcegroep te implementeren. Er zijn diverse opties beschikbaar voor het bewerken van de sjabloon en implementatie, zowel lokaal als portal-gebaseerd, met inbegrip van:

   - Met behulp van een [aangepaste sjabloon vanuit Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), waarmee u een sjabloon maken van een volledig nieuwe of baseren op een bestaande gemeenschappelijke of [QuickStart-sjabloon](https://azure.microsoft.com/documentation/templates/).
   - Die is afgeleid van een bestaande resourcegroep door een sjabloon exporteren vanuit een [de oorspronkelijke implementatie](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), of vanuit de [huidige status van de implementatie van](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Met behulp van een lokale [JSON-editor (zoals VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), en vervolgens uploaden en implementeren met behulp van PowerShell of CLI.
   - Met Visual Studio [Azure-resourcegroepproject](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) te maken en implementeren van een sjabloon.  

Ongeacht welke optie die u kiest, is de sjabloonsyntaxis van de hetzelfde tijdens de initiële implementatie en opnieuw implementeren. Inschakelen van een systeem of de gebruiker toegewezen identiteit op een nieuwe of bestaande virtuele machine wordt uitgevoerd op dezelfde manier. Bovendien standaard Azure Resource Manager biedt een [incrementele update](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementaties.

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie maakt u inschakelen en uitschakelen van een systeem toegewezen identiteit met een Azure Resource Manager-sjabloon.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Systeem toegewezen identiteit tijdens het maken van een Azure-VM of op een bestaande virtuele machine inschakelen

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Na het laden van de sjabloon in een editor, Ga naar de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Uw uitzien enigszins afwijken van de volgende schermafbeelding, afhankelijk van de editor die u gebruikt en of het bewerken van een sjabloon voor de implementatie van een nieuwe of bestaande resourcegroep.

   >[!NOTE] 
   > Dit voorbeeld wordt aangenomen variabelen zoals `vmName`, `storageAccountName`, en `nicName` zijn gedefinieerd in de sjabloon.
   >

   ![Schermafbeelding van de sjabloon: zoek VM](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Om in te schakelen systeem toegewezen identiteit, voeg de `"identity"` eigenschap op hetzelfde niveau als het `"type": "Microsoft.Compute/virtualMachines"` eigenschap. Gebruik de volgende syntaxis:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Optioneel) Toevoegen van de extensie van de virtuele machine beheerde Service-identiteit als een `resources` element. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.  Gebruik de volgende syntaxis:

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

### <a name="assign-a-role-the-vms-system-assigned-identity"></a>Van de virtuele machine systeem toegewezen identiteit voor een rol toewijzen

Nadat u hebt systeem toegewezen identiteit ingeschakeld op de virtuele machine, kunt u het geven een rol, zoals **lezer** toegang tot de resourcegroep waarin deze is gemaakt.

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

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Uitschakelen van een systeem toegewezen identiteit van een Azure VM

Als u een virtuele machine die een beheerde service-identiteit niet meer nodig hebt:

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Hebt u een virtuele machine die alleen systeem toegewezen identiteit heeft, kunt u deze uitschakelen door het wijzigen van het identiteitstype aan `None`.  Als uw virtuele machine is zowel de systeem- en de gebruiker toegewezen identiteiten, verwijdert u `SystemAssigned` van de id-type en blijf aan de `UserAssigned` samen met de `identityIds` matrix van de gebruiker toegewezen identiteiten.  Het volgende voorbeeld ziet u hoe een systeem toegewezen identiteit van een virtuele machine zonder gebruiker toegewezen identiteiten verwijderen:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie maakt u de identiteit van een gebruiker toegewezen aan een Azure VM met behulp van Azure Resource Manager-sjabloon.

> [!Note]
> Zie voor het maken van een gebruiker toegewezen identiteit met een Azure Resource Manager-sjabloon, [maken van een gebruiker toegewezen identiteit](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Een gebruiker toegewezen identiteit met een Azure VM toewijzen

1. Onder de `resources` -element de volgende vermelding voor het toewijzen van de identiteit van een gebruiker toegewezen aan uw virtuele machine toevoegen.  Vervang `<USERASSIGNEDIDENTITY>` met de naam van de toegewezen gebruikers-id die u hebt gemaakt.
   
   > [!Important]
   > De `<USERASSIGNEDIDENTITYNAME>` waarde die wordt weergegeven in het volgende voorbeeld moet worden opgeslagen in een variabele.  Ook voor de momenteel ondersteunde implementatie van een gebruiker toegewezen identiteiten toewijzen aan een virtuele machine in een Resource Manager-sjabloon, de api-versie moet overeenkomen met de versie in het volgende voorbeeld.
    
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
        },
    ```
    
2. (Optioneel) Vervolgens onder de `resources` -element de volgende vermelding voor het toewijzen van de extensie beheerde identiteit aan uw virtuele machine toevoegen. Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. Gebruik de volgende syntaxis:
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
    
3.  Wanneer u klaar bent, is de sjabloon ziet die vergelijkbaar is met het volgende:

      ![Schermafbeelding van de gebruiker toegewezen identiteit](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)

### <a name="remove-user-assigned-identity-from-an-azure-vm"></a>Verwijder de gebruiker toegewezen identiteit van een Azure-VM

Als u een virtuele machine die een beheerde service-identiteit niet meer nodig hebt:

1. Of u lokaal bij Azure aanmelden of via de Azure portal, gebruikt u een account dat is gekoppeld aan het Azure-abonnement bevat waarmee de virtuele machine.

2. Laden van de sjabloon in een [editor](#azure-resource-manager-templates) en zoek de `Microsoft.Compute/virtualMachines` resource van belang zijn binnen de `resources` sectie. Als u een virtuele machine waarvoor alleen toegewezen gebruikers-id hebt, kunt u deze uitschakelen door het veranderen van de id-type naar `None`.  Als uw virtuele machine heeft zowel de systeem- en de gebruiker toegewezen identiteiten en u wilt behouden systeem toegewezen identiteit, verwijdert u `UserAssigned` van het identiteitstype samen met de `identityIds` matrix van de gebruiker toegewezen identiteiten.
    
   Verwijderen van een een enkele gebruiker toegewezen identiteit van een virtuele machine verwijderen uit de `identityIds` matrix.
   
   Het volgende voorbeeld ziet u hoe alle gebruiker toegewezen identiteiten van een virtuele machine met geen enkel systeem toegewezen identiteit verwijderen:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="related-content"></a>Gerelateerde inhoud

- Lees voor een breder perspectief over beheerde Service-identiteit, de [overzicht van de beheerde Service-identiteit](overview.md).

