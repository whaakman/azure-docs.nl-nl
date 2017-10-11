---
ms.assetid: 
title: Azure Sleutelkluis - voorlopig verwijderen gebruiken met PowerShell
description: Case voorbeelden van voorlopig verwijderen gebruiken met PowerShell codefragmenten
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 8cf0674f7eb139e50da4a3c22a8d8376a86b0dcc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Sleutelkluis voorlopig verwijderen gebruiken met PowerShell

Azure Key Vault van voorlopige verwijderingen functie kunt herstellen van verwijderde kluizen en objecten van de kluis. In het bijzonder voorlopig verwijderen adressen van de volgende scenario's:

- Ondersteuning voor herstelbare verwijdering van een sleutelkluis
- Ondersteuning voor verwijdering van de herstelbare objecten van de sleutelkluis. sleutels, geheimen, en certificaten

## <a name="prerequisites"></a>Vereisten

- Azure PowerShell 4.0.0 of later - als u niet dat al ingesteld hebt, Azure PowerShell installeren en deze aan uw Azure-abonnement koppelen, Zie [installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Er is een verouderde versie van de uitvoer van onze Sleutelkluis PowerShell opmaak bestand dat **kan** in uw omgeving in plaats van de juiste versie worden geladen. We zijn anticiperen op een bijgewerkte versie van PowerShell om te kunnen bevatten de benodigde correcties voor de uitvoer-opmaak en dit onderwerp wordt bijgewerkt op dat moment. De huidige oplossing, moet u dit probleem zich voordoet opmaak, is:
> - Gebruik de volgende query uit als u merkt dat u niet ziet, de soft-verwijderen in dit onderwerp beschreven van de eigenschap enabled: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Zie voor Sleutelkluis refernece voor specifieke informatie voor PowerShell [Azure Key Vault PowerShell-referentie](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Vereiste machtigingen

Sleutelkluis worden afzonderlijk beheerd via op rollen gebaseerde machtigingen voor toegangsbeheer (RBAC) als volgt:

| Bewerking | Beschrijving | Machtigingen van gebruiker |
|:--|:--|:--|
|Lijst|Een lijst met sleutelkluizen verwijderd.|Microsoft.KeyVault/deletedVaults/read|
|Herstellen|Hiermee herstelt u een verwijderde sleutelkluis.|Microsoft.KeyVault/vaults/write|
|Opschonen|Een verwijderde sleutelkluis en alle bijbehorende inhoud verwijderd permanent.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Zie voor meer informatie over de machtigingen en toegangsbeheer [beveiligen van uw sleutelkluis](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Inschakelen van voorlopig verwijderen

Als u een verwijderde sleutelkluis of objecten die zijn opgeslagen in een sleutelkluis herstellen, moet u eerst de soft-het verwijderen van die sleutelkluis inschakelen.

### <a name="existing-key-vault"></a>Bestaande sleutelkluis

Voor een bestaande sleutelkluis ContosoVault met de naam, moet u voorlopig verwijderen als volgt inschakelen. 

>[!NOTE]
>Op dit moment moet u met Azure Resource Manager resource manipulatie rechtstreeks schrijft de *enableSoftDelete* eigenschap aan de Sleutelkluis-resource.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Nieuwe sleutelkluis

Voorlopig verwijderen voor een nieuwe sleutelkluis inschakelen tijdens het maken wordt gedaan door toe te voegen de vlag voorlopig verwijderen inschakelen voor uw opdracht maken.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Controleren inschakelen voor voorlopig verwijderen

Om te controleren of er een sleutelkluis voorlopig verwijderen ingeschakeld is, voer de *ophalen* opdracht en zoekt u de 'voorlopig verwijderen Enabled?' kenmerk en de instelling true of false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Verwijderen van een sleutelkluis beveiligd door voorlopig verwijderen

De opdracht uit om een sleutelkluis verwijderen (of verwijderen) hetzelfde is gebleven, maar het gedrag verandert, afhankelijk van of u voorlopig verwijderen of niet hebt ingeschakeld.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Als u de vorige opdracht voor een sleutelkluis die geen voorlopig verwijderen ingeschakeld uitvoert, wordt u deze sleutelkluis en alle bijbehorende inhoud zonder opties voor herstel permanent verwijderd.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hoe voorlopig verwijderen beschermt uw sleutelkluizen

Ingeschakeld met voorlopig verwijderen:

- Wanneer een sleutelkluis wordt verwijderd, is deze verwijderd uit de resourcegroep en geplaatst in een gereserveerde naamruimte die alleen wordt gekoppeld aan de locatie waar ze werden gemaakt. 
- Objecten in een verwijderde sleutel kluis, zoals sleutels, geheimen en certificaten, zijn niet toegankelijk en blijven ook terwijl hun insluitende sleutelkluis de status verwijderd heeft wordt. 
- De DNS-naam voor een sleutelkluis in een verwijderde status is nog steeds gereserveerd, zodat een nieuwe sleutelkluis met dezelfde naam kan niet worden gemaakt.  

U kunt verwijderde status sleutelkluizen, dat wordt gekoppeld aan uw abonnement weergeven met de volgende opdracht:

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

De *Resource-ID* verwijst in de uitvoer naar de oorspronkelijke bron-ID van deze kluis. Aangezien deze sleutelkluis nu in een verwijderde staat bevindt is, bestaat er is geen resource met die resource-ID. De *Id* veld kan worden gebruikt voor het identificeren van de resource als herstellen of verwijderen. De *opschonen datum gepland* veld geeft aan wanneer de kluis wordt definitief verwijderd (verwijderd) als er geen actie voor deze verwijderde kluis ondernomen. De bewaartermijn gebruikt voor het berekenen van de *opschonen datum gepland*, is 90 dagen.

## <a name="recovering-a-key-vault"></a>Een sleutelkluis herstellen

Om te herstellen een sleutelkluis, moet u de naam van de sleutelkluis, resourcegroep en locatie opgeven. Noteer de locatie en de resourcegroep van de verwijderde sleutelkluis als u deze voor het herstelproces van een sleutelkluis nodig.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Wanneer een sleutelkluis is hersteld, is het resultaat een nieuwe resource met de sleutelkluis oorspronkelijke bron-ID. Als de resourcegroep waar de sleutelkluis bestond is verwijderd, moet een nieuwe resourcegroep met dezelfde naam worden gemaakt voordat de sleutelkluis kan worden hersteld.

## <a name="key-vault-objects-and-soft-delete"></a>Sleutelkluis-objecten en voorlopig verwijderen

Voor een sleutel 'ContosoFirstKey' in een sleutelkluis met de naam 'ContosoVault' met voorlopig verwijderen ingeschakeld, hier van wilt hoe u verwijderen die sleutel.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Aan de sleutelkluis is ingeschakeld voor voorlopig verwijderen, weergegeven een verwijderde sleutel nog steeds zoals deze wordt verwijderd, behalve, wanneer u expliciet aanbiedt of verwijderde sleutels ophalen. De meeste bewerkingen voor een sleutel in de status verwijderd heeft mislukt met uitzondering van de aanbieding een verwijderde sleutel, herstellen of leegmaakt. 

Bijvoorbeeld, om aan te vragen tot sleutels van de lijst verwijderd in een sleutelkluis, gebruik de volgende opdracht:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Transitiestatus 

Wanneer u een sleutel in een sleutelkluis met de soft-delete is ingeschakeld verwijderen, wordt het duurt een paar seconden voor de overgang te voltooien. Tijdens deze transitiestatus zal verschijnen de sleutel is niet in de actieve status of de status verwijderd heeft. Met deze opdracht wordt een lijst alle verwijderde sleutels in de sleutelkluis met de naam 'ContosoVault'.

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Voorlopig verwijderen gebruiken met sleutelkluis-objecten

Net zoals sleutelkluizen, een verwijderde sleutel geheim of certificaat blijft in verwijderde staat bevindt voor maximaal 90 dagen tenzij u deze herstellen of het opschonen. 

#### <a name="keys"></a>Sleutels

Een verwijderde sleutel herstellen:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Een sleutel permanent verwijderen:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Bezig met het verwijderen van een sleutel wordt permanent verwijderd, wat betekent dat deze kan niet worden hersteld.

De **herstellen** en **opschonen** acties hebben hun eigen machtigingen die zijn gekoppeld in een sleutelkluis-beleid. Voor een gebruiker of service-principal te kunnen uitvoeren een **herstellen** of **opschonen** actie ze gemachtigd bent de respectieve voor dat object (sleutel of geheim) in het toegangsbeleid van de sleutelkluis. Standaard de **opschonen** machtiging is niet toegevoegd aan een sleutelkluis toegangsbeleid wanneer de snelkoppeling 'all' wordt gebruikt voor alle machtigingen verlenen aan een gebruiker. U moet expliciet verlenen **opschonen** machtiging. De volgende opdracht bijvoorbeeld verleent user@contoso.com toestemming voor het uitvoeren van verschillende bewerkingen voor sleutels in *ContosoVault* inclusief **opschonen**.

#### <a name="set-a-key-vault-access-policy"></a>Beleid voor sleutelkluis toegang instellen

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Als u een bestaande sleutelkluis die ingeschakeld voor voorlopig verwijderen NET heeft hebt, hebt u geen **herstellen** en **opschonen** machtigingen.

#### <a name="secrets"></a>Geheimen

Zoals sleutels, worden geheimen in een sleutelkluis met hun eigen opdrachten beheerd op. Te volgen, zijn de opdrachten voor het verwijderen van, aanbieden, herstellen en geheimen verwijderen.

- Een geheim dat met de naam SQLPassword verwijderen: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Lijst van alle verwijderde geheimen in een sleutelkluis: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Een geheim in de status verwijderd heeft herstellen: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Opschonen van een geheim in verwijderde staat bevindt: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Bezig met het verwijderen van een geheim wordt permanent verwijderd, wat betekent dat deze kan niet worden hersteld.

## <a name="purging-and-key-vaults"></a>Hiervoor en sleutel kluizen

### <a name="key-vault-objects"></a>Sleutelkluis-objecten

Opruimen van een sleutel wordt geheim of certificaat permanent verwijderd, wat betekent dat deze kan niet worden hersteld. De sleutelkluis die deel uitmaakt van het verwijderde object echter blijven behouden als alle andere objecten in de sleutelkluis. 

### <a name="key-vaults-as-containers"></a>Sleutel-kluizen als containers
Wanneer een sleutelkluis is opgeschoond, worden alle inhoud, met inbegrip van sleutels, geheimen en certificaten, permanent verwijderd. Als u wilt een sleutelkluis verwijderen, gebruikt u de `Remove-AzureRmKeyVault` opdracht met de optie `-InRemovedState` en op te geven van de locatie van de verwijderde sleutelkluis met de `-Location location` argument. U vindt de locatie van een verwijderde kluis met de opdracht `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Bezig met het verwijderen van een sleutelkluis wordt permanent verwijderd, wat betekent dat deze kan niet worden hersteld.

### <a name="purge-permissions-required"></a>Opschonen van de vereiste machtigingen
- Als u wilt verwijderen een verwijderde sleutelkluis, zodat de kluis en alle bijbehorende inhoud permanent worden verwijderd, moet de gebruiker RBAC-machtiging voor het uitvoeren van een *Microsoft.KeyVault/locations/deletedVaults/purge/action* bewerking. 
- Als de verwijderde sleutel wilt weergeven, de kluis van een gebruiker RBAC heeft toestemming nodig om uit te voeren *Microsoft.KeyVault/deletedVaults/read* machtiging. 
- Alleen de abonnementsbeheerder van een heeft standaard deze machtigingen. 

### <a name="scheduled-purge"></a>Geplande opschonen

Uw sleutelkluis verwijderde objecten weergeven geeft aan wanneer ze schedled worden opgeschoond door Sleutelkluis. De *opschonen datum gepland* veld geeft aan wanneer een object van de sleutelkluis, worden definitief verwijderd, als er geen actie ondernomen. De bewaarperiode voor een verwijderde sleutelkluis-object is standaard 90 dagen.

>[!NOTE]
>Een object opgeschoonde kluis, geactiveerd door de *opschonen datum gepland* veld, wordt definitief verwijderd. Het is niet hersteld.

## <a name="other-resources"></a>Meer informatie

- Zie voor een overzicht van de Sleutelkluis voorlopig verwijderen functie [overzicht van Azure Sleutelkluis voorlopig verwijderen](key-vault-ovw-soft-delete.md).
- Zie voor een algemeen overzicht van Azure Sleutelkluis gebruik [aan de slag met Azure Key Vault](key-vault-get-started.md).

