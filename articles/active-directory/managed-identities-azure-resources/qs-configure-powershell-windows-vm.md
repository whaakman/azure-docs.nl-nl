---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van PowerShell
description: Stapsgewijze beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f76fef3d5e6515e9d546c709ace0a4a533c0a45
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881168"
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

1. Raadpleeg een van de volgende Quick starts Azure-VM, alleen de benodigde secties ("aanmelden bij Azure ', 'Resourcegroep maken', 'Maken netwerken groeperen', 'de virtuele machine maken') te voltooien.
    
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [New-AzVMConfig](/powershell/module/az.compute/new-azvm) cmdlet-syntaxis. Zorg ervoor dat u toevoegt een `-AssignIdentity:$SystemAssigned` parameter voor het inrichten van de virtuele machine met de door het systeem toegewezen identiteit die wordt ingeschakeld, bijvoorbeeld:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> Richt u eventueel de beheerde identiteiten voor VM-extensie voor Azure-resources, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [migreren van de VM-extensie naar Azure IMDS eindpunt voor de verificatie](howto-migrate-vm-extension.md).

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

> [!NOTE]
> Richt u eventueel de beheerde identiteiten voor VM-extensie voor Azure-resources, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [migreren van de VM-extensie naar Azure IMDS eindpunt voor de verificatie](howto-migrate-vm-extension.md).

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

> [!NOTE]
> Als u de beheerde identiteit voor VM-extensie voor Azure-resources (om te worden afgeschaft) hebt ingericht, moet u deze verwijderen met behulp de [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension). Zie voor meer informatie, [migreren van VM-extensie naar Azure IMDS voor verificatie](howto-migrate-vm-extension.md).

## <a name="user-assigned-managed-identity"></a>Door een gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een virtuele machine tijdens het maken van

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Raadpleeg een van de volgende Quick starts Azure-VM, alleen de benodigde secties ("aanmelden bij Azure ', 'Resourcegroep maken', 'Maken netwerken groeperen', 'de virtuele machine maken') te voltooien. 
  
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [ `New-AzVMConfig` ](/powershell/module/az.compute/new-azvm) cmdlet-syntaxis. Voeg de `-IdentityType UserAssigned` en `-IdentityID` parameters voor het inrichten van de virtuele machine met de identiteit van een gebruiker toegewezen.  Vervang `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.  Bijvoorbeeld:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> Richt u eventueel de beheerde identiteiten voor VM-extensie voor Azure-resources, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [migreren van de VM-extensie naar Azure IMDS eindpunt voor de verificatie](howto-migrate-vm-extension.md).

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzAccount
   ```

2. Maak een beheerde identiteit gebruiker toegewezen met de [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet.  Houd er rekening mee de `Id` in de uitvoer omdat u dit in de volgende stap moet.

   > [!IMPORTANT]
   > Het maken van beheerde identiteiten gebruiker toegewezen ondersteunt alleen alfanumerieke tekens, onderstrepingstekens en afbreekstreepjes (0-9 of a-z of A-Z, \_ of -) tekens. Naam moet bovendien beperkt van 3 tot 128 tekens voor de toewijzing van VM/VMSS goed te laten werken. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

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

> [!NOTE]
> Richt u eventueel de beheerde identiteiten voor VM-extensie voor Azure-resources, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [migreren van de VM-extensie naar Azure IMDS eindpunt voor de verificatie](howto-migrate-vm-extension.md).

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
  
  - [Een virtuele Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
