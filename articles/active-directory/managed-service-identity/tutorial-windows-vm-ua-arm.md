---
title: Gebruik van een virtuele machine van Windows-gebruiker MSI toegewezen voor toegang tot Azure Resource Manager
description: Een zelfstudie die u bij het proces helpt van het gebruik van een gebruiker toegewezen beheerde Service identiteit (MSI) op een Windows-VM voor toegang tot Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: d80e0fc35b8c20bd61a78a68542f3311c1a40952
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) op een Windows-VM te gebruiken voor toegang tot Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie wordt uitgelegd hoe u een gebruiker toegewezen identiteit maken, toewijzen aan een Windows-virtuele Machine (VM) en die identiteit vervolgens gebruiken voor toegang tot de Azure Resource Manager-API. Beheerde Service-identiteiten worden automatisch beheerd door Azure. Hiermee kunt verificatie voor services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties insluiten in uw code. 

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Windows-VM maken 
> * Een gebruiker toegewezen identiteit maken
> * De gebruiker toegewezen identiteit voor uw Windows-VM toewijzen
> * De gebruiker toegewezen identiteit toegang verlenen aan een resourcegroep in Azure Resource Manager 
> * Ophalen van een toegangstoken die door de gebruiker toegewezen identiteit en het aanroepen van Azure Resource Manager 
> * Lezen van de eigenschappen van een resourcegroep

## <a name="prerequisites"></a>Vereisten

- Als uw weet niet bekend met de Service-identiteit beheerd Bekijk de [overzicht](overview.md) sectie. **Lees de [verschillen tussen systeem en de gebruiker toegewezen identiteiten](overview.md#how-does-it-work)**.
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Als u wilt uitvoeren in de vereiste bron maken en de rol management stappen in deze zelfstudie, moet uw account 'Eigenaar'-machtigingen op het juiste bereik (uw abonnement of de resource-group). Als u hulp nodig hebt bij roltoewijzing, Zie [Use Role-Based toegangsbeheer voor het beheren van toegang tot de resources van uw Azure-abonnement](/azure/role-based-access-control/role-assignments-portal).

Als u wilt installeren en gebruiken van PowerShell lokaal, wordt in deze zelfstudie de Azure PowerShell-moduleversie 5.7 of hoger vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-resource-group"></a>Een resourcegroep maken

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *EastUS* regio.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Nadat de resourcegroep wordt gemaakt, maakt u een virtuele machine van Windows.

Stel met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) de gebruikersnaam en het wachtwoord in die nodig zijn voor de beheerdersaccount op de virtuele machine:

```azurepowershell-interactive
$cred = Get-Credential
```
Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="create-a-user-assigned-identity"></a>Een gebruiker toegewezen identiteit maken

Een gebruiker toegewezen identiteit gemaakt als een zelfstandige Azure-resource. Met behulp van de [nieuw AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), Azure maakt een identiteit in uw Azure AD-tenant die kan worden toegewezen aan een of meer exemplaren van Azure-service.

> [!IMPORTANT]
> Toegewezen identiteiten met speciale tekens (dat wil zeggen liggend streepje) in de naam van de gebruiker maken wordt momenteel niet ondersteund. Gebruik alfanumerieke tekens. Controleer regelmatig op updates.  Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Het antwoord bevat de details voor de gebruiker toegewezen identiteit gemaakt, vergelijkbaar met het volgende voorbeeld. Opmerking de `Id` waarde voor de identiteit van de gebruiker die is toegewezen, zoals deze wordt gebruikt in de volgende stap:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Toewijzen van de gebruiker toegewezen identiteit aan een virtuele machine van Windows

Een gebruiker toegewezen identiteit kan worden gebruikt door clients in meerdere Azure-resources. Gebruik de volgende opdrachten voor het toewijzen van de identiteit van de gebruiker toegewezen aan één VM. Gebruik de `Id` eigenschap geretourneerd in de vorige stap voor de `-IdentityID` parameter.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Verleen dat de gebruiker toegewezen MSI toegang aan een resourcegroep in Azure Resource Manager 

Beheerde Service-identiteit (MSI) biedt identiteiten die uw code gebruiken kunt om aan te vragen van de toegangstokens voor verificatie bij resource API's die ondersteuning voor Azure AD-verificatie. In deze zelfstudie wordt uw code toegang tot de Azure Resource Manager-API. 

Voordat uw code toegang heeft tot de API, moet u de identiteit toegang verlenen tot een resource in Azure Resource Manager. In dit geval de resourcegroep waarin de virtuele machine is opgenomen. Werk de waarde voor `<SUBSCRIPTION ID>` afhankelijk van wat geschikt is voor uw omgeving.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Het antwoord bevat de details voor de roltoewijzing gemaakt, vergelijkbaar met het volgende voorbeeld:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Ophalen van een toegangstoken die met de identiteit van de VM en deze gebruiken om aan te roepen Resource Manager 

Voor het restant van de zelfstudie werkt u van de virtuele machine die we eerder hebben gemaakt.

1. Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com)

2. Navigeer in de portal naar **virtuele Machines** en gaat u naar de virtuele machine van Windows en in de **overzicht**, klikt u op **Connect**.

3. Voer de **gebruikersnaam** en **wachtwoord** u tijdens het maken van de virtuele machine van Windows gebruikt.

4. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie.

5. Met behulp van PowerShell `Invoke-WebRequest`, maak een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Resource Manager.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Lezen van de eigenschappen van een resourcegroep

Gebruik het toegangstoken opgehaald in de vorige stap voor toegang tot Azure Resource Manager, en de eigenschappen van de Resource lezen groep die u hebt verleend uw identiteit toegewezen door gebruikerstoegang. Vervang <SUBSCRIPTION ID> met de abonnements-id van uw omgeving.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Het antwoord bevat de specifieke resourcegroep informatie vergelijkbaar met het volgende voorbeeld:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](overview.md).
