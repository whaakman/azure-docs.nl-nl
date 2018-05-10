---
title: Het configureren van MSI op een Azure-VMSS met behulp van PowerShell
description: Stap voor stap toegewezen instructies voor het configureren van een systeem- en identiteiten op een VMSS Azure met behulp van PowerShell.
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
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 97c5e2dde3faeaad13317597bef4f70455d22102
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Configureer een VMSS beheerde Service identiteit (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe de volgende bewerkingen voor Service-identiteit beheerd op een Azure virtuele Machine schaal instellen (VMSS), met behulp van PowerShell uit te voeren:
- Inschakelen en uitschakelen van het systeem toegewezen identiteit op een Azure-VMSS
- Toevoegen en verwijderen van een gebruiker met de identiteit op een Azure-VMSS

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt. 

## <a name="system-assigned-managed-identity"></a>Automatisch toegewezen beheerde identiteit

In deze sectie informatie over het inschakelen en de systeemidentiteit van een dat is toegewezen met Azure PowerShell verwijderen.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Automatisch toegewezen identiteit tijdens het maken van een Azure-VMSS inschakelen

Als u een VMSS maken met het systeem toegewezen identiteit ingeschakeld:

1. Raadpleeg *voorbeeld 1* in de [nieuw AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) cmdlet referentieartikel een VMSS maken met de systeemidentiteit van een dat is toegewezen.  De parameter toevoegen `-IdentityType SystemAssigned` naar de `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optioneel) Het gebruik van MSI VMSS uitbreiding toevoegen de `-Name` en `-Type` parameter bij de [toevoegen AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token:

    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Automatisch toegewezen identiteit op een bestaande Azure-VMSS inschakelen

Als u een toegewezen identiteit op een bestaande Azure-VMSS inschakelen moet:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de VMSS met behulp van de [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Gebruik vervolgens om de systeemidentiteit van een die zijn toegewezen, de `-IdentityType` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. Het gebruik van MSI VMSS uitbreiding toevoegen de `-Name` en `-Type` parameter bij de [toevoegen AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Het systeem toegewezen identiteit van een Azure-VMSS uitschakelen

> [!NOTE]
> Managed Service-identiteit van een virtuele-Machineschaalset uitschakelen is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een virtuele-Machineschaalset die niet langer nodig heeft het systeem toegewezen identiteit, maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruik de volgende cmdlet:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

2. Voer de volgende cmdlet uit:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

In deze sectie informatie over het toevoegen en verwijderen van een gebruiker met de identiteit van een VMSS met Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Een gebruiker toegewezen identiteit bij het maken van een Azure-VMSS toewijzen

Een nieuwe VMSS maken met een gebruiker met de identiteit is momenteel niet ondersteund via PowerShell. Zie de volgende sectie over het toevoegen van de identiteit van een gebruiker is toegewezen aan een bestaande VMSS. Controleer regelmatig op updates.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>De identiteit van een gebruiker toewijzen aan een bestaande Azure-VMSS

Een gebruiker identiteit toegewezen aan een bestaande Azure VMSS toewijzen:

1. Meld u bij het gebruik van Azure `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt de identiteit van een gebruiker is toegewezen aan de Azure-VMSS toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet. Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` met uw eigen waarden.

   > [!IMPORTANT]
   > Maken van toegewezen gebruikers-id's ondersteunt alleen alfanumerieke en het koppelteken (0-9 of a-z of A-Z of -) tekens. Bovendien moeten worden beperkt tot 24 tekens voor de toewijzing aan een VM/VMSS goed te laten werken. Controleer regelmatig op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Een gebruiker toegewezen identiteit van een Azure-VMSS verwijderen

> [!NOTE]
> Verwijderen van alle toegewezen gebruikersidentiteiten uit een virtuele-Machineschaalset is momenteel niet ondersteund, tenzij u een systeem toegewezen identiteit hebt. Controleer regelmatig op updates.

Als uw VMSS meerdere identiteiten van de gebruiker die is toegewezen heeft, kunt u alles behalve het laatste bestand met de volgende opdrachten verwijderen. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VMSS NAME>` parameterwaarden met uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die op de VMSS moet blijven. Deze informatie kunt u vinden door in de sectie van de identiteit van het gebruik van de VMSS `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

Als uw VMSS heeft zowel automatisch toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruikers identiteiten toegewezen door over te schakelen voor het gebruik van alleen automatisch toegewezen. Gebruik de volgende opdracht:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van beheerde Service-identiteit](overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie:
  
  - [Een virtuele Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















