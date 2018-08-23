---
ms.assetid: ''
title: Azure Key Vault - functie voor voorlopig verwijderen gebruiken met PowerShell
description: De aanvraag voorbeelden van voorlopig verwijderen gebruiken met PowerShell-codefragmenten
services: key-vault
author: bryanla
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bryanla
ms.openlocfilehash: 174a5b41e6a48ea74cd813746b7c070463a8185b
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42055465"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell

Functie voor voorlopig verwijderen van Azure Key Vault de maakt het herstellen van verwijderde kluizen en -objecten van de kluis. Met name voorlopig verwijderen adressen de volgende scenario's:

- Ondersteuning voor het herstelbare verwijderen van een key vault
- Ondersteuning voor verwijdering van de herstelbare objecten van de sleutelkluis. sleutels, geheimen, en certificaten

## <a name="prerequisites"></a>Vereisten

- Azure PowerShell 4.0.0 of later - als u niet dat al ingesteld hebt, Azure PowerShell installeren en deze aan uw Azure-abonnement koppelen, Zie [hoe u Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Er is een verouderde versie van onze Key Vault PowerShell uitvoeropmaak bestand dat de **kan** worden geladen in uw omgeving in plaats van de juiste versie. We zijn anticiperen op een bijgewerkte versie van PowerShell bevat de benodigde correctie voor de opmaak van de uitvoer en dit onderwerp wordt bijgewerkt op dat moment. De huidige oplossing, moet u dit probleem zich voordoet opmaak, is:
> - Gebruik de volgende query uit als u merkt dat u de functie voor voorlopig verwijderen niet ziet in dit onderwerp beschreven van de eigenschap enabled: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Zie voor Key Vault specifieke refernece informatie voor PowerShell [Azure Key Vault PowerShell-referentie](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Vereiste machtigingen

Key Vault-bewerkingen worden afzonderlijk beheerd via machtigingen voor op rollen gebaseerd toegangsbeheer (RBAC) als volgt:

| Bewerking | Beschrijving | Machtigingen van gebruiker |
|:--|:--|:--|
|Lijst|Een lijst met verwijderde sleutelkluizen.|Microsoft.KeyVault/deletedVaults/read|
|Herstellen|Hiermee herstelt u een verwijderde key vault.|Microsoft.KeyVault/vaults/write|
|Opschonen|Een verwijderde key vault en alle bijbehorende inhoud verwijderd permanent.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Zie voor meer informatie over de machtigingen en access control [uw key vault beveiligen](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Inschakelen van voorlopig verwijderen

Als u een verwijderde key vault of objecten die zijn opgeslagen in een key vault herstellen, moet u eerst de functie voor voorlopig verwijderen voor die key vault inschakelen.

### <a name="existing-key-vault"></a>Bestaande key vault

Voor een bestaande sleutelkluis met de naam ContosoVault, moet u voorlopig verwijderen als volgt inschakelen. 

>[!NOTE]
>Op dit moment moet u Azure Resource Manager resource manipulatie rechtstreeks schrijven gebruiken de *enableSoftDelete* eigenschap naar de Key Vault-resource.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Nieuwe key vault

Door toe te voegen van de functie voor voorlopig verwijderen inschakelen-vlag voor het inschakelen van voorlopig verwijderen voor een nieuwe key vault wordt uitgevoerd op Aanmaaktijd uw opdracht maken.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Controleer of de activering van de functie voor voorlopig verwijderen

Uitvoeren om te controleren of een key vault voorlopig verwijderen ingeschakeld heeft, de *ophalen* opdracht en zoekt u de 'voorlopig verwijderen ingeschakeld?' kenmerk en de instelling, true of false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Verwijderen van een key vault beveiligd door de functie voor voorlopig verwijderen

De opdracht uit om een sleutelkluis verwijderen (of verwijderen) blijft hetzelfde, maar het gedrag ervan wordt gewijzigd, afhankelijk van of u voorlopig verwijderen zijn ingeschakeld of niet.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Als u de vorige opdracht voor een key vault die geen functie voor voorlopig verwijderen ingeschakeld uitvoert, wordt u deze sleutelkluis en alle bijbehorende inhoud zonder opties voor herstel permanent verwijderd.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hoe uw sleutelkluizen worden beveiligd met voorlopig verwijderen

Met de functie voor voorlopig verwijderen ingeschakeld:

- Wanneer een key vault wordt verwijderd, wordt deze verwijderd uit de resourcegroep en geplaatst in een gereserveerde naamruimte die is alleen die zijn gekoppeld aan de locatie waar deze is gemaakt. 
- Objecten in een verwijderde sleutel kluis, zoals sleutels, geheimen en certificaten, zijn niet toegankelijk en blijven, terwijl hun met key vault de status verwijderd heeft is. 
- De DNS-naam voor een key vault in een verwijderde status is nog steeds gereserveerd, zodat een nieuwe sleutelkluis met dezelfde naam kan niet worden gemaakt.  

U kunt verwijderde sleutelkluizen, dat wordt gekoppeld aan uw abonnement weergeven met de volgende opdracht:

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

De *Resource-ID* verwijst in de uitvoer naar de oorspronkelijke bron-ID van deze kluis. Omdat deze sleutelkluis nu in een verwijderde status is, bestaat er is geen bron met die resource-ID. De *Id* veld kan worden gebruikt voor het identificeren van de resource bij het herstellen of verwijderen. De *opschonen datum gepland* veld geeft aan wanneer de kluis wordt definitief verwijderd (opgeschoond) als er geen actie voor deze kluis verwijderd ondernomen. De bewaartermijn, die wordt gebruikt voor het berekenen van de *opschonen datum gepland*, is 90 dagen.

## <a name="recovering-a-key-vault"></a>Herstellen van een key vault

Als u wilt herstellen van een key vault, moet u de key vault-naam, resourcegroep en locatie opgeven. Houd er rekening mee de locatie en de resourcegroep van de verwijderde key vault, u deze voor het herstelproces van een sleutelkluis moet.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Wanneer een key vault is hersteld, is het resultaat een nieuwe resource met de key vault oorspronkelijke resource-ID. Als de resourcegroep waar de key vault bestond is verwijderd, moet een nieuwe resourcegroep met dezelfde naam worden gemaakt voordat de key vault kan worden hersteld.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault-objecten en de functie voor voorlopig verwijderen

Voor een sleutel, 'ContosoFirstKey', in een key vault met de naam 'ContosoVault' met voorlopig verwijderen ingeschakeld, hier van verwijdert hoe u deze sleutel.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Met uw key vault voor voorlopig verwijderen zijn ingeschakeld, een verwijderde sleutel nog steeds wordt weergegeven, zoals deze wordt verwijderd, behalve, wanneer u expliciet lijst of ophalen van verwijderde sleutels. De meeste bewerkingen voor een sleutel in de status verwijderd heeft mislukt met uitzondering van de aanbieding van een verwijderde sleutel, herstellen of deze leeg te maken. 

Bijvoorbeeld, om aan te vragen voor een lijst met verwijderde sleutels in een key vault, gebruikt u de volgende opdracht uit:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Status van de overgang 

Wanneer u een sleutel in een key vault met de functie voor voorlopig verwijderen ingeschakeld verwijdert, wordt het duurt een paar seconden voor de overgang te voltooien. Tijdens deze status van de overgang, wordt deze mogelijk weergegeven dat de sleutel niet in de actieve status of de status verwijderd heeft is. Met deze opdracht worden alle verwijderde sleutels in uw key vault met de naam 'ContosoVault' weergegeven.

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

### <a name="using-soft-delete-with-key-vault-objects"></a>Voorlopig verwijderen gebruiken met key vault-objecten

Net als sleutelkluizen, een verwijderde sleutel geheim of certificaat blijft status verwijderd voor maximaal 90 dagen, tenzij u deze herstellen of het opschonen. 

#### <a name="keys"></a>Sleutels

Een verwijderde sleutel herstellen:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Permanent verwijderen van een sleutel:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Bezig met het verwijderen van een sleutel wordt permanent verwijderd, wat betekent dat deze niet worden hersteld.

De **herstellen** en **opschonen** acties hebben hun eigen machtigingen die zijn gekoppeld in een toegangsbeleid voor key vault. Voor een gebruiker of service-principal te kunnen zijn om uit te voeren een **herstellen** of **opschonen** actie moet hebben de respectieve machtiging voor dat object (sleutel of geheim) in het toegangsbeleid voor key vault. Standaard de **opschonen** machtiging is niet toegevoegd aan een key vault-toegangsbeleid wanneer de snelkoppeling naar de 'all' wordt gebruikt voor alle machtigingen verlenen aan een gebruiker. U moet expliciet verlenen **opschonen** machtiging. Bijvoorbeeld, de volgende opdracht verleent user@contoso.com machtiging voor het uitvoeren van verschillende bewerkingen voor sleutels in *ContosoVault* inclusief **opschonen**.

#### <a name="set-a-key-vault-access-policy"></a>Een toegangsbeleid voor key vault instellen

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Als u een bestaande sleutelkluis die zojuist voorlopig verwijderen ingeschakeld heeft hebt, hebt u geen **herstellen** en **opschonen** machtigingen.

#### <a name="secrets"></a>Geheimen

Zoals sleutels, worden geheimen in een key vault met hun eigen opdrachten uitgevoerd op. Te volgen, zijn de opdrachten voor het verwijderen, vermelden, herstellen en geheimen verwijderen.

- Een geheim met de naam SQLPassword verwijderen: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Een overzicht van alle verwijderde geheimen in een key vault: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Een geheim in de status verwijderd heeft herstellen: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Opschonen van een geheim status verwijderd: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Bezig met het verwijderen van een geheim verwijderd definitief, wat betekent dat deze niet worden hersteld.

## <a name="purging-and-key-vaults"></a>Hiervoor en key vaults

### <a name="key-vault-objects"></a>Key vault-objecten

Bezig met het verwijderen van een sleutel, verwijderd geheim of certificaat definitief, wat betekent dat deze niet worden hersteld. De sleutelkluis die deel uitmaakt van het verwijderde object echter blijven behouden als alle andere objecten in de key vault. 

### <a name="key-vaults-as-containers"></a>Sleutelkluizen als containers
Wanneer een key vault wordt verwijderd, worden alle inhoud, met inbegrip van sleutels, geheimen en certificaten, permanent verwijderd. Als u wilt verwijderen van een key vault, gebruikt u de `Remove-AzureRmKeyVault` opdracht met de optie `-InRemovedState` en door te geven van de locatie van de verwijderde key vault met de `-Location location` argument. U kunt zoeken naar de locatie van een verwijderde kluis met de opdracht `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Bezig met het verwijderen van een key vault wordt definitief verwijderd, wat betekent dat deze niet worden hersteld.

### <a name="purge-permissions-required"></a>Opschonen van de machtigingen die vereist zijn
- Als u wilt verwijderen een verwijderde key vault, zodat de kluis en alle bijbehorende inhoud worden definitief verwijderd, moet de gebruiker RBAC-machtiging om uit te voeren een *Microsoft.KeyVault/locations/deletedVaults/purge/action* bewerking. 
- Als u de verwijderde sleutel, de kluis moet een gebruiker RBAC-machtiging om uit te voeren *Microsoft.KeyVault/deletedVaults/read* machtiging. 
- Alleen de beheerder van een abonnement heeft standaard de machtigingen toegewezen. 

### <a name="scheduled-purge"></a>Geplande opschonen

Uw objecten verwijderde sleutelkluis weergeven, laat zien wanneer ze schedled zijn worden opgeschoond met Key Vault. De *opschonen datum gepland* veld geeft aan wanneer een key vault-object wordt definitief verwijderd, als er geen actie ondernomen. Standaard is de bewaarperiode voor een object verwijderde sleutelkluis 90 dagen.

>[!NOTE]
>Een object opgeschoonde kluis, geactiveerd door de *opschonen datum gepland* veld, wordt definitief verwijderd. Het kan niet worden hersteld.

## <a name="other-resources"></a>Meer informatie

- Zie voor een overzicht van de functie voor voorlopig verwijderen van de Sleutelkluis, [overzicht van Azure Key Vault-functie voor voorlopig verwijderen](key-vault-ovw-soft-delete.md).
- Zie voor een algemeen overzicht van gebruik van Azure Key Vault, [aan de slag met Azure Key Vault](key-vault-get-started.md).

