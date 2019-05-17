---
title: Azure Blockchain Service consortium management met behulp van PowerShell
description: Over het beheren van de leden van de consortium Azure Blockchain Service met behulp van PowerShell
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/10/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: f15fa3b4972a2ac54d1d9bce916fdd42c2951d2f
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550881"
---
# <a name="manage-consortium-members-in-azure-blockchain-service-using-powershell"></a>Beheren van consortium leden in Azure Blockchain-Service met behulp van PowerShell

U kunt PowerShell gebruiken voor het beheren van blockchain consortium leden voor uw Azure Blockchain-Service. Leden met een administrator-bevoegdheden kunnen uitnodigen, toevoegen, verwijderen, en rollen voor alle deelnemers aan het consortium blockchain wordt gewijzigd. Leden met bevoegdheden van de gebruiker alle deelnemers aan de blockchain-consortium kunnen bekijken en de weergavenaam van het lid kunnen wijzigen.

## <a name="prerequisites"></a>Vereisten

* [Een blockchain-lid met de Azure portal maken](create-member.md)
* Zie voor meer informatie over consortiums, leden en knooppunten [consortium Azure Blockchain-Service](consortium.md)

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/powershell](https://shell.azure.com/powershell) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

## <a name="install-powershell-module"></a>PowerShell-module installeren

Installeer het pakket Microsoft.AzureBlockchainService.ConsortiumManagement.PS vanuit de PowerShell Gallery.

```powershell-interactive
Install-Module -Name Microsoft.AzureBlockchainService.ConsortiumManagement.PS -Scope CurrentUser
Import-Module Microsoft.AzureBlockchainService.ConsortiumManagement.PS
```

## <a name="set-information-preference"></a>Informatie-voorkeur instellen

U kunt meer informatie kunt krijgen bij het uitvoeren van de cmdlets door informatie voorkeursvariabele instellen. Standaard *$InformationPreference* is ingesteld op *SilentlyContinue*.

Voor meer uitgebreide informatie uit cmdlets de voorkeur in PowerShell als volgt instellen:

```powershell-interactive
$InformationPreference = 'Continue'
```

## <a name="establish-a-web3-connection"></a>Een Web3-verbinding tot stand brengen

Voor het beheren van consortium leden, moet u een Web3 verbinding met uw Azure Blockchain-Service-eindpunt lid. U kunt dit script gebruiken om in te stellen van globale variabelen die kunnen worden gebruikt bij het aanroepen van de consortium certificaatmanagement-cmdlets.

```powershell-interactive
$Connection = New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
$MemberAccount = Import-Web3Account -ManagedAccountAddress '<Member account address>' -ManagedAccountPassword '<Member account password>'
$ContractConnection = Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>' -Web3Client $Connection
```

Vervang \<lid accountwachtwoord\> met het wachtwoord van de lid-account gebruikt bij het maken van het lid.

De andere waarden vinden in Azure portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Azure Blockchain Service standaardlid **overzicht** pagina.

    ![Lid-overzicht](./media/manage-consortium-powershell/member-overview.png)

    Vervang \<LEDENACCOUNT\>, en \<RootContract adres\> met de waarden uit de portal.

1. Selecteer voor het adres van het eindpunt **transactie knooppunten** en selecteer de **standaard** transactie-knooppunt. Het standaard-transactie-knooppunt heeft dezelfde naam als de blockchain-lid.
1. Selecteer **verbindingsreeksen**.

    ![Verbindingsreeksen](./media/manage-consortium-powershell/connection-strings.png)

    Vervang \<eindpuntadres\> met de waarde van **HTTPS (toegangssleutel 1)** of **HTTPS (toegangssleutel 2)**.

## <a name="network-and-smart-contract-management"></a>Netwerk- en slimme contract management

De netwerk- en slimme contract cmdlets tot stand brengen van een verbinding met uw blockchain-eindpunt slimme contracten die verantwoordelijk is voor het beheer van consortium gebruiken.

### <a name="import-consortiummanagementcontracts"></a>Import-ConsortiumManagementContracts

Maakt verbinding met de consortium management slimme contracten, die worden gebruikt voor het beheren en afdwingen van leden in het consortium.

`Import-ConsortiumManagementContracts -RootContractAddress <String> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| RootContractAddress | Het contract-adres van de hoofdmap van de consortium management slimme contracten | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
Import-ConsortiumManagementContracts -RootContractAddress '<RootContract address>'  -Web3Client $Connection
```

### <a name="import-web3account"></a>Import-Web3Account

Gebruik deze cmdlet voor het maken van een object voor het opslaan van het externe knooppunt beheeraccount voor informatie.

`Import-Web3Account -ManagedAccountAddress <String> -ManagedAccountPassword <String>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| ManagedAccountAddress | Blockchain lid accountadres | Ja |
| ManagedAccountPassword | Adres accountwachtwoord | Ja |

**Voorbeeld**

```powershell-interactive
Import-Web3Account -ManagedAccountAddress '<Member account address>'  -ManagedAccountPassword '<Member account password>'
```

### <a name="new-web3connection"></a>Nieuwe Web3Connection

Maakt een verbinding met de RPC-eindpunt van een transactie-knooppunt.

`New-Web3Connection [-RemoteRPCEndpoint <String>]`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| RemoteRPCEndpoint | Blockchain lid eindpuntadres | Ja |

**Voorbeeld**

```powershell-interactive
New-Web3Connection -RemoteRPCEndpoint '<Endpoint address>'
```

## <a name="consortium-member-management"></a>Consortium lid management

Consortium lid management-cmdlets gebruiken voor het beheren van leden in het consortium. Beschikbare acties is afhankelijk van uw rol consortium.

### <a name="get-blockchainmember"></a>Get-BlockchainMember

Hiermee haalt u gegevens van lid of leden van het consortium weergeven.

`Get-BlockchainMember [[-Name] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| Name | De naam van het lid van de Azure Blockchain-Service die u wilt ophalen van gegevens op. Als u een gebruikersnaam opgeeft, worden de details van het lid heeft geretourneerd. Als de naam wordt weggelaten, wordt een lijst met alle leden van de consortium geretourneerd. | Nee |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | Get-BlockchainMember -Name <Member Name>
```

**Voorbeeld van uitvoer**

```
Name           : myblockchainmember
CorrelationId  : 0
DisplayName    : myCompany
SubscriptionId : <Azure subscription ID>
AccountAddress : 0x85b911c9e103d6405573151258d668479e9ebeef
Role           : ADMIN
```

### <a name="remove-blockchainmember"></a>Remove-BlockchainMember

Hiermee verwijdert u een blockchain-lid.

`Remove-BlockchainMember -Name <String> -Members <IContract> -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| Name | Naam van het lid verwijderen | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | Remove-BlockchainMember -Name <Member Name> -Web3Account $MemberAccount
```

### <a name="set-blockchainmember"></a>Set-BlockchainMember

Hiermee stelt u blockchain lidkenmerken, met inbegrip van de naam en consortium rol weergeven.

Consortium beheerders kunnen instellen **DisplayName** en **rol** voor alle leden. Consortium lid met de gebruikersrol kan alleen de weergavenaam van hun eigen lid wijzigen.

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

**Voorbeeld**

```powershell-interactive
$ContractConnection | Set-BlockchainMember -Name <Member Name> -DisplayName <Display name> -Web3Account $MemberAccount
```

## <a name="consortium-member-invitation-management"></a>Consortium lid uitnodiging management

Consortium lid uitnodiging management-cmdlets gebruiken voor het beheren van consortium lid uitnodigingen. Beschikbare acties is afhankelijk van uw rol consortium.

### <a name="new-blockchainmemberinvitation"></a>New-BlockchainMemberInvitation

Nieuwe leden aan de consortium uitnodigen.

`New-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van de uitgenodigde leden | Ja |
| Rol | Consortium rol. Waarden mag ADMIN of gebruiker. BEHEERDER is de rol van de beheerder consortium. GEBRUIKER is de rol van consortium lid. | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | New-BlockchainMemberInvitation -SubscriptionId <Azure Subscription ID> -Role USER -Web3Account $MemberAccount
```

### <a name="get-blockchainmemberinvitation"></a>Get-BlockchainMemberInvitation

Haalt of een lijst met de status van de uitnodiging consortium lid.

`Get-BlockchainMemberInvitation [[-SubscriptionId] <String>] -Members <IContract> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van de uitgenodigde leden. Als abonnements-id is opgegeven, worden de details van uitnodiging van de abonnements-ID geretourneerd. Als abonnements-id wordt weggelaten, wordt een lijst met alle uitnodigingen voor lid worden geretourneerd. | Nee |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | Get-BlockchainMemberInvitation – SubscriptionId <Azure subscription ID>
```

**Voorbeeld van uitvoer**

```
SubscriptionId                       Role CorrelationId
--------------                       ---- -------------
<Azure subscription ID>              USER             2
```

### <a name="remove-blockchainmemberinvitation"></a>Remove-BlockchainMemberInvitation

Hiermee wordt een consortium lid uitnodiging ingetrokken.

`Remove-BlockchainMemberInvitation -SubscriptionId <String> -Members <IContract> -Web3Account <IAccount>
 -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van het lid in te trekken | Ja |
| Leden | Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | Remove-BlockchainMemberInvitation -SubscriptionId <Subscription ID> -Web3Account $MemberAccount
```

### <a name="set-blockchainmemberinvitation"></a>Set-BlockchainMemberInvitation

Stelt de **rol** voor een bestaande uitnodiging. Alleen consortium beheerders kunnen uitnodigingen wijzigen.

`Set-BlockchainMemberInvitation -SubscriptionId <String> -Role <String> -Members <IContract>
 -Web3Account <IAccount> -Web3Client <IClient>`

| Parameter | Beschrijving | Vereist |
|-----------|-------------|:--------:|
| SubscriptionId | Azure-abonnement-ID van de uitgenodigde leden | Ja |
| Rol | Nieuwe rol consortium voor uitnodiging. Waarden zijn **gebruiker** of **ADMIN** | Ja |
| Leden |  Leden object verkregen van de Import-ConsortiumManagementContracts | Ja |
| Web3Account | Web3Account object verkregen van de Import-Web3Account | Ja |
| Web3Client | Web3Client object verkregen van New-Web3Connection | Ja |

**Voorbeeld**

```powershell-interactive
$ContractConnection | Set-BlockchainMemberInvitation -SubscriptionId <Azure subscription ID> -Role USER -Web3Account $MemberAccount
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over consortiums, leden en knooppunten:

> [!div class="nextstepaction"]
> [Azure Blockchain Service consortium](consortium.md)