---
title: Leden van een consortium Azure Blockchain-Service beheren met behulp van Azure PowerShell
description: Leer hoe u Azure Blockchain Service consortium leden beheren met behulp van Azure PowerShell.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 4bb72bc3fe8b85a8d2aed88e02f5f3150abb6899
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66493638"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-by-using-powershell"></a>Consortium leden in de Azure Blockchain-Service beheren met behulp van PowerShell

U kunt PowerShell gebruiken voor het beheren van blockchain consortium leden voor uw Azure Blockchain-Service. Leden die beheerdersbevoegdheden hebben kunnen uitnodigen, toevoegen, verwijderen en rollen voor alle deelnemers aan de blockchain-consortium wijzigen. Leden van die gebruiker heeft bevoegdheden kunnen alle deelnemers aan de blockchain-consortium weergeven en wijzigen van de weergavenaam van het lid.

## <a name="prerequisites"></a>Vereisten

* Een blockchain-lid maken met behulp van de [Azure-portal](create-member.md).
* Zie voor meer informatie over consortiums, leden en knooppunten [Azure Blockchain Service consortium](consortium.md).

## <a name="open-azure-cloud-shell"></a>Azure Cloud Shell openen

Azure Cloud Shell is een gratis interactieve shell die u gebruiken kunt om uit te voeren van de stappen in dit artikel. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door te gaan naar [shell.azure.com/powershell](https://shell.azure.com/powershell). Selecteer **kopie** voor het kopiëren van de blokken van code, plak deze in Cloud Shell en selecteer **Enter** uit te voeren.

## <a name="install-the-powershell-module"></a>De PowerShell-module installeren

Installeer het pakket Microsoft.AzureBlockchainService.ConsortiumManagement.PS vanuit de PowerShell Gallery.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-the-information-preference"></a>De informatie-voorkeur instellen

U kunt meer informatie kunt krijgen bij het uitvoeren van de cmdlets door in te stellen de voorkeursvariabele informatie. Standaard *$InformationPreference* is ingesteld op *SilentlyContinue*.

Voor meer uitgebreide informatie uit cmdlets de voorkeur in PowerShell als volgt instellen:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Een Web3-verbinding tot stand brengen

Voor het beheren van consortium leden, een Web3 verbinding maken met het eindpunt van de Blockchain-Service-lid. U kunt dit script gebruiken om in te stellen van globale variabelen voor het aanroepen van de consortium certificaatmanagement-cmdlets.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Vervang *\<lid accountwachtwoord\>* met het wachtwoord van de lid-account die u hebt gebruikt tijdens het maken van het lid.

De andere waarden vinden in Azure portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Ga naar uw Blockchain Service standaardlid **overzicht** pagina.

    ![Lid-overzicht](./media/manage-consortium-powershell/member-overview.png)

    Vervang *\<LEDENACCOUNT\>* en *\<RootContract adres\>* met de waarden uit de portal.

1. Selecteer voor het adres van het eindpunt **transactie knooppunten**, en selecteer vervolgens de **standaard transactie knooppunt**. De standaardknooppunt heeft dezelfde naam als de blockchain-lid.
1. Selecteer **verbindingsreeksen**.

    ![Verbindingsreeksen](./media/manage-consortium-powershell/connection-strings.png)

    Vervang *\<eindpuntadres\>* met de waarde van **HTTPS (toegangssleutel 1)** of **HTTPS (toegangssleutel 2)** .

## <a name="manage-the-network-and-smart-contracts"></a>De netwerk- en slimme contracten beheren

De netwerk- en slimme contract cmdlets tot stand brengen van een verbinding met de blockchain-eindpunt slimme contracten die verantwoordelijk is voor het beheer van consortium gebruiken.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Gebruik deze cmdlet verbinding maken met het beheer van de consortium slimme contracten. Deze opdrachten worden gebruikt om te beheren en afdwingen van leden in het consortium.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| RootContractAddress | Adres van de basis-contract van de consortium management slimme contracten | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Deze cmdlet gebruiken om een object voor het opslaan van de gegevens voor een externe knooppunt-account te maken.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Blockchain lid accountadres | Ja |
| ManagedAccountPassword | Adres accountwachtwoord | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>Nieuwe Web3Connection

Gebruik deze cmdlet voor een verbinding met de RPC-eindpunt van een transactie-knooppunt.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blockchain lid eindpuntadres | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="manage-the-consortium-members"></a>De leden consortium beheren

Consortium lid management-cmdlets gebruiken voor het beheren van leden in het consortium. De beschikbare acties is afhankelijk van uw rol consortium.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Gebruik deze cmdlet voor het ophalen van gegevens van lid of leden van het consortium weergeven.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| Name | De naam van het lid van de Blockchain-Service die u ophalen van gegevens wilt over. Wanneer u een naam invoert, wordt de details van het lid. Wanneer een name wordt weggelaten, wordt een lijst met alle leden van de consortium. | Nee |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

#### <a name="example-output"></a>Voorbeeld van uitvoer

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Gebruik deze cmdlet een blockchain-lid te verwijderen.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| Name | Naam van het lid verwijderen | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Deze cmdlet gebruiken om in te stellen blockchain lidkenmerken, met inbegrip van de weergavenaam en de rol consortium.

Consortium beheerders kunnen instellen **DisplayName** en **rol** voor alle leden. Een consortium lid met de gebruikersrol kan alleen de weergavenaam van hun eigen lid kunt wijzigen.

`Set-BlockchainMember -Name <String> [-DisplayName <String>] [-AccountAddress <String>] [-Role <String>]
 -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| Name | Naam van het lid blockchain | Ja |
| DisplayName | Nieuwe weergavenaam | Nee |
| AccountAddress | Accountadres | Nee |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client |  Web3Client object verkregen van New-Web3Connection| Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="manage-the-consortium-members-invitations"></a>De leden consortium uitnodigingen beheren

Gebruik de consortium lid uitnodiging certificaatmanagement-cmdlets voor het beheren van consortium leden uitnodigingen. De beschikbare acties is afhankelijk van uw rol consortium.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Deze cmdlet gebruiken om uit te nodigen nieuwe leden aan de consortium.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van het lid uitnodigen | Ja |
| Rol | De rol consortium. Waarden mag ADMIN of gebruiker. BEHEERDER is de rol van de beheerder consortium. GEBRUIKER is de rol van consortium lid. | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Gebruik deze cmdlet ophalen of weergeven van de status van een consortium lid van de uitnodiging.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | De Azure-abonnement-ID van het lid om uit te nodigen. Als het abonnement-ID is opgegeven, wordt het abonnement-id's details van uitnodiging. Als de abonnements-ID wordt weggelaten, wordt een lijst met alle lid uitnodigingen. | Nee |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

#### <a name="example-output"></a>Voorbeeld van uitvoer

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Deze cmdlet gebruiken om in te trekken van het lid van een consortium uitnodiging.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van het lid in te trekken | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Deze cmdlet gebruiken om in te stellen de **rol** voor een bestaande uitnodiging. Alleen consortium beheerders kunnen uitnodigingen wijzigen.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van het lid uitnodigen | Ja |
| Rol | Nieuwe rol consortium voor uitnodiging. Waarden zijn **gebruiker** of **ADMIN**. | Ja |
| Leden |  Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

#### <a name="example"></a>Voorbeeld

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over consortiums, leden en knooppunten:

> [!div class="nextstepaction"]
> [Azure Blockchain Service consortium](consortium.md)
