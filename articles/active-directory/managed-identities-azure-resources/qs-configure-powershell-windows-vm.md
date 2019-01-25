---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van PowerShell
description: Stapsgewijze beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: priyamo
ms.openlocfilehash: ae6b4751236987aba089b48877069a58924827b0
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54901102"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van PowerShell, u hoe u voor het uitvoeren van de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure-VM.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Installeer [de meest recente versie van Azure PowerShell](/powershell/azure/install-az-ps) als u dat nog niet gedaan hebt.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt in- en uitschakelen van de door het systeem toegewezen beheerde identiteit met Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Het systeem toegewezen beheerde identiteit inschakelen tijdens het maken van een Azure VM

Voor het maken van een Azure-VM met de ingeschakeld door het systeem toegewezen beheerde identiteit, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM').
    
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [New-AzVMConfig](/powershell/module/az.compute/new-azvm) cmdlet-syntaxis. Zorg ervoor dat u toevoegt een `-AssignIdentity:$SystemAssigned` parameter voor het inrichten van de virtuele machine met de door het systeem toegewezen identiteit die wordt ingeschakeld, bijvoorbeeld:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de beheerde identiteiten voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. De beheerde identiteiten voor VM-extensie voor Azure-resources is gepland voor de afschaffing in januari 2019. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Het systeem toegewezen beheerde identiteit van een bestaande VM in Azure inschakelen

Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele machine die oorspronkelijk is ingericht zonder, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzVM` cmdlet. Gebruik vervolgens om in te schakelen op een beheerde identiteit voor het systeem is toegewezen, de `-AssignIdentity` overschakelen op de [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet:

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Optioneel) Toevoegen van de beheerde identiteiten voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

### <a name="add-vm-system-assigned-identity-to-a-group"></a>Systeem toegewezen identiteit van VM toevoegen aan een groep

Nadat u systeem toegewezen identiteit op een virtuele machine hebt ingeschakeld, kunt u deze toevoegen aan een groep.  De volgende procedure wordt van een virtuele machine systeem toegewezen identiteit toegevoegd aan een groep.

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzAccount
   ```

2. Ophalen en noteer de `ObjectID` (zoals opgegeven in de `Id` veld van de geretourneerde waarden) van de service-principal van de virtuele machine:

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Ophalen en noteer de `ObjectID` (zoals opgegeven in de `Id` veld van de geretourneerde waarden) van de groep:

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. Service-principal van de virtuele machine toevoegen aan de groep:

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Uitschakelen door het systeem toegewezen identiteit van een Azure VM beheerd

Als u wilt uitschakelen door het systeem toegewezen beheerde identiteit op een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

Als u een virtuele Machine die niet meer nodig is op het systeem toegewezen beheerde identiteit maar nog steeds moet gebruiker toegewezen identiteiten op beheerde hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzAccount
   ```

2. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzVM` cmdlet en stel de `-IdentityType` parameter `UserAssigned`:

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Als u een virtuele machine die niet langer nodig door het systeem toegewezen beheerde identiteit heeft en er geen beheerde gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Verwijderen van de beheerde identiteiten voor VM-extensie voor Azure-resources, gebruiker de - de naam switch met de [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension) cmdlet, op te geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een virtuele machine tijdens het maken van

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM'). 
  
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [ `New-AzVMConfig` ](/powershell/module/az.compute/new-azvm) cmdlet-syntaxis. Voeg de `-IdentityType UserAssigned` en `-IdentityID ` parameters voor het inrichten van de virtuele machine met de identiteit van een gebruiker toegewezen.  Vervang `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.  Bijvoorbeeld:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de beheerde identiteit voor het gebruik van Azure-resources VM extension de `-Type` parameter op de [Set AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:
      > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. De beheerde identiteiten voor VM-extensie voor Azure-resources is gepland voor de afschaffing in januari 2019.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzAccount
   ```

2. Maak een beheerde identiteit gebruiker toegewezen met de [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet.  Houd er rekening mee de `Id` in de uitvoer omdat u dit in de volgende stap moet.

   > [!IMPORTANT]
   > Het maken van beheerde identiteiten gebruiker toegewezen ondersteunt alleen alfanumerieke tekens en afbreekstreepjes (0-9 of a-z of A-Z of -) tekens. Bovendien moet de naam zijn beperkt tot 24 tekens voor de toewijzing van VM/VMSS goed te laten werken. Controleer later op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzVM` cmdlet. Als u wilt een beheerde identiteit voor de gebruiker toegewezen aan de Azure-VM toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet.  De waarde voor de`-IdentityId` parameter wordt de `Id` u in de vorige stap hebt genoteerd.  Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.

   > [!WARNING]
   > Als u wilt behouden een eerder door de gebruiker toegewezen beheerde identiteiten die aan de virtuele machine is toegewezen, query de `Identity` eigenschap van de VM-object (bijvoorbeeld `$vm.Identity`).  Als een gebruiker toegewezen beheerde identiteiten worden geretourneerd, opnemen in de volgende opdracht samen met de nieuwe gebruiker toegewezen beheerde identiteit die u wilt toewijzen aan de virtuele machine.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Toevoegen van de beheerde identiteit voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set AzVMExtension](/powershell/module/az.compute/set-azvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure VM

Als u wilt verwijderen van een gebruiker toegewezen identiteit aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.

Als uw virtuele machine meerdere beheerde identiteiten gebruiker toegewezen, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY NAME>` is van de gebruiker toegewezen beheerde identiteit van de naam van eigenschap, die op de virtuele machine moet blijven. Deze informatie kan worden gevonden door het opvragen van de `Identity` eigenschap van de VM-object.  Bijvoorbeeld, `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Als uw virtuele machine beschikt niet over een systeem toegewezen beheerd wilt identiteits- en u alle beheerde identiteiten die door de gebruiker toegewezen van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Als uw virtuele machine zowel door het systeem toegewezen als de gebruiker toegewezen beheerde identiteiten is, kunt u alle de gebruiker toegewezen beheerde identiteiten verwijderen door over te schakelen voor het gebruik van beheerde identiteiten alleen door het systeem is toegewezen.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
