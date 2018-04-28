---
title: Het configureren van MSI op een virtuele machine in Azure met behulp van PowerShell
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met behulp van PowerShell.
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
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 0c316a2c73b451e4d8f67ace7b41c38dcfbc52f0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configureren van een VM beheerde Service identiteit (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>In dit artikel leert u hoe op de volgende Service-identiteit beheerd bewerkingen uitvoeren op een virtuele machine van Azure, met behulp van PowerShell:
- 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md). **Lees de [verschil tussen een systeem dat is toegewezen en de gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.

## <a name="system-assigned-identity"></a>Automatisch toegewezen identiteit

In deze sectie leert u hoe inschakelen en uitschakelen van de identiteit van de automatisch toegewezen met Azure PowerShell.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Automatisch toegewezen identiteit bij het maken van een virtuele machine van Azure inschakelen

Als u een virtuele machine in Azure maken met het systeem toegewezen identiteit ingeschakeld:

1. Verwijzen naar een van de volgende Azure VM snelstartgidsen, alleen de benodigde secties voltooid ('Aanmelden bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'De virtuele machine maken').
    
    Wanneer u naar de sectie 'De VM maken' krijgen, moet u een kleine wijziging van de [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Zorg ervoor dat om toe te voegen een `-AssignIdentity "SystemAssigned"` -parameter voor het inrichten van de virtuele machine met de identiteit van de automatisch toegewezen ingeschakeld, bijvoorbeeld:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity "SystemAssigned" ...
    ```

   - [Maken van een virtuele Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Automatisch toegewezen identiteit op een bestaande virtuele machine van Azure inschakelen

Als u een toegewezen identiteit op een bestaande virtuele Machine inschakelen moet:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Gebruik vervolgens om de systeemidentiteit van een die zijn toegewezen, de `-AssignIdentity` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity "SystemAssigned"
   ```

3. (Optioneel) Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token. Zorg ervoor dat u de juiste `-Location` parameter die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Het systeem toegewezen identiteit van een Azure-virtuele machine uitschakelen

> [!NOTE]
>  Het uitschakelen van de Service-identiteit beheerd vanaf een virtuele Machine is momenteel niet ondersteund. In de tussentijd kunt u schakelen tussen het gebruik van het systeem toegewezen en toegewezen gebruikersidentiteiten.

Als u een virtuele Machine die niet langer nodig heeft het systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende cmdlet:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Login-AzureRmAccount
   ```

2. Voer de volgende cmdlet uit: 
    ```powershell       
    Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -IdentityType "UserAssigned"
    ```
Verwijderen van de MSI-VM-extensie, gebruiker-naam switch met de [verwijderen AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Identiteit toegewezen door gebruiker

In deze sectie informatie over het toevoegen en verwijderen van een gebruiker met de identiteit van een virtuele machine met Azure PowerShell.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Een identiteit toegewezen aan een virtuele machine tijdens het maken van gebruiker toewijzen

Toewijzen aan een virtuele machine in Azure een toegewezen gebruikers-id bij het maken van de virtuele machine:

1. Verwijzen naar een van de volgende Azure VM snelstartgidsen, alleen de benodigde secties voltooid ('Aanmelden bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'De virtuele machine maken'). 
  
    Wanneer u naar de sectie 'De VM maken' krijgen, moet u een kleine wijziging van de [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Voeg de `-IdentityType UserAssigned` en `-IdentityID ` parameters voor het inrichten van de virtuele machine met de identiteit van een gebruiker toegewezen.  Vervang `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<MSI NAME>` met uw eigen waarden.  Bijvoorbeeld:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [Maken van een virtuele Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token. Zorg ervoor dat u de juiste `-Location` parameter die overeenkomt met de locatie van de bestaande virtuele machine:
      > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure exemplaar metagegevens Service (IMDS) gebruiken kunt en tokens op te halen.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>De identiteit van een gebruiker toewijzen aan een bestaande virtuele machine in Azure

Toegewezen identiteit naar een bestaande Azure-virtuele machine als een gebruiker wilt toewijzen:

1. Meld u bij het gebruik van Azure `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Maken van een gebruiker toegewezen identiteit via de [nieuw AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Opmerking de `Id` in de uitvoer omdat u dit in de volgende stap moet.

    > [!IMPORTANT]
    > Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

  ```powershell
  New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
  ```
3. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt de identiteit van een gebruiker is toegewezen aan de virtuele machine van Azure toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  De waarde voor de`-IdentityId` -parameter is de `Id` in de vorige stap hebt genoteerd.  Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` met uw eigen waarden.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token. Geef de juiste `-Location` parameter die overeenkomt met de locatie van de bestaande virtuele machine.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Een beheerde identiteit van een Azure VM toegewezen gebruiker verwijderen

> [!NOTE]
>  Alle toegewezen gebruikersidentiteiten verwijderen uit een virtuele Machine is momenteel niet ondersteund, tenzij u een systeem toegewezen identiteit hebt. Controleer regelmatig op updates.

Als uw virtuele machine meerdere identiteiten van de gebruiker die is toegewezen heeft, kunt u alles behalve het laatste bestand met de volgende opdrachten verwijderen. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die op de virtuele machine moet blijven. Deze informatie kunt u vinden door in de sectie van de identiteit van het gebruik van de VM `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm
```

Als uw virtuele machine heeft zowel automatisch toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruikers identiteiten toegewezen door over te schakelen voor het gebruik van alleen automatisch toegewezen. Gebruik de volgende opdracht:

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = $null
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van beheerde Service-identiteit](overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie:
  
  - [Een virtuele Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















