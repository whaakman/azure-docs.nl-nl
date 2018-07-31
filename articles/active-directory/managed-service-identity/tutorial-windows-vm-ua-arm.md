---
title: Gebruik een door de gebruiker toegewezen Managed Service Identity op een Windows-VM om toegang te krijgen tot Azure Resource Manager
description: Een zelfstudie die u helpt bij het doorlopen van het proces voor het krijgen van toegang tot Azure Resource Manager met een door de gebruiker toegewezen Managed Service Identity op een Windows-VM.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: 9cc7683b260a9afbe4aee006a22af9c4834c4eb1
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248384"
---
# <a name="tutorial-use-a-user-assigned-managed-service-identity-on-a-windows-vm-to-access-azure-resource-manager"></a>Zelfstudie: Toegang krijgen tot Azure Resource Manager met een door de gebruiker toegewezen Managed Service Identity op een Windows-VM

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

In deze zelfstudie wordt uitgelegd hoe u een door de gebruiker toegewezen identiteit kunt maken, deze toewijzen aan een virtuele Windows-machine (VM) en die identiteit vervolgens gebruiken om toegang te krijgen tot de Azure Resource Manager-API. Beheerde service-identiteiten worden automatisch beheerd door Azure. Ze maken verificatie mogelijk voor services die Azure AD-verificatie ondersteunen, zonder dat er referenties in uw code hoeven te worden ingesloten. 

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Windows-VM maken 
> * Door de gebruiker toegewezen identiteit maken
> * Uw door de gebruiker toegewezen identiteit toewijzen aan uw Windows-VM
> * De door de gebruiker toegewezen identiteit toegang verlenen tot een resourcegroep in Azure Resource Manager 
> * Een toegangstoken ophalen met behulp van de door de gebruiker toegewezen identiteit en daarmee Azure Resource Manager aanroepen 
> * De eigenschappen van een resourcegroep lezen

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend bent met Managed Service Identity, raadpleeg dan de [overzichtssectie](overview.md). **Zorg ervoor dat u de [verschillen tussen door het systeem en door de gebruiker toegewezen identiteiten](overview.md#how-does-it-work)** kent.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Om de stappen voor het maken van de vereiste resources en het rolbeheer in deze zelfstudie uit te voeren, moet uw account 'Eigenaar'-machtigingen hebben voor het juiste bereik (uw abonnement of resourcegroep). Voor hulp bij roltoewijzing gaat u naar [Op rollen gebaseerd toegangsbeheer gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](/azure/role-based-access-control/role-assignments-portal).

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 5.7 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-resource-group"></a>Een resourcegroep maken

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* gemaakt in de regio *VS - Oost*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Virtuele machine maken

Nadat de resourcegroep is gemaakt, maakt u een Windows-VM.

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

## <a name="create-a-user-assigned-identity"></a>Door de gebruiker toegewezen identiteit maken

Een door de gebruiker toegewezen identiteit wordt gemaakt als een zelfstandige Azure-resource. Met de [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) wordt een identiteit gemaakt in uw Azure AD-tenant die kan worden toegewezen aan een of meer Azure-service-exemplaren.

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Het antwoord bevat details voor de gemaakte door de gebruiker toegewezen identiteit, vergelijkbaar met het volgende voorbeeld. Noteer de `Id`-waarde voor uw door de gebruiker toegewezen identiteit, omdat deze bij de volgende stap wordt gebruikt:

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

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>De door de gebruiker toegewezen identiteit toewijzen aan een Windows-VM

Een door de gebruiker toegewezen identiteit kan worden gebruikt door clients op meerdere Azure-resources. Gebruik de volgende opdrachten om de door de gebruiker toegewezen identiteit toe te wijzen aan één VM. Gebruik de eigenschap `Id` die in de vorige stap is geretourneerd voor de parameter `-IdentityID`.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-managed-service-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Uw door de gebruiker toegewezen Managed Service Identity toegang verlenen tot een resourcegroep in Azure Resource Manager 

Managed Service Identity biedt identiteiten die uw code kan gebruiken om toegangstokens aan te vragen voor verificatie bij resource-API's die Azure AD-verificatie ondersteunen. In deze zelfstudie krijgt uw code toegang tot de Azure Resource Manager-API. 

Voordat uw code toegang tot de API kan krijgen, moet u de identiteit toegang geven tot een resource in Azure Resource Manager. In dit geval is dat de resourcegroep waarin de VM zich bevindt. Werk de waarde voor `<SUBSCRIPTION ID>` bij overeenkomstig uw omgeving.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Het antwoord bevat details voor de gemaakte roltoewijzing, vergelijkbaar met het volgende voorbeeld:

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

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en daarmee Resource Manager aanroepen 

Voor de rest van de zelfstudie werkt u op de virtuele machine die we eerder hebben gemaakt.

1. Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com)

2. Navigeer in Azure Portal naar **Virtuele machines**, ga naar de virtuele Windows-machine en klik op de pagina **Overzicht** op **Verbinden**.

3. Voer de **gebruikersnaam** en het **wachtwoord** in die u hebt gebruikt bij het maken van de Windows-VM.

4. Nu u een **Verbinding met extern bureaublad** met de virtuele machine hebt gemaakt, opent u **PowerShell** in de externe sessie.

5. Verzend met `Invoke-WebRequest` van Powershell een aanvraag naar het lokale Managed Service Identity-eindpunt om een toegangstoken voor Azure Resource Manager op te halen.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>De eigenschappen van een resourcegroep lezen

Gebruik het in de vorige stap opgehaalde toegangstoken om toegang te krijgen tot Azure Resource Manager en lees de eigenschappen van de resourcegroep waarvoor u uw door de gebruiker toegewezen identiteit toegang hebt verleend. Vervang <SUBSCRIPTION ID> door de abonnements-id van uw omgeving.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Het antwoord bevat de gegevens van de specifieke resourcegroep, vergelijkbaar met het volgende voorbeeld:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een door de gebruiker toegewezen identiteit te maken en deze te koppelen aan een virtuele machine in Azure voor toegang tot de Azure Resource Manager-API.  Zie voor meer informatie over Azure Resource Manager:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)