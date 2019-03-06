---
title: Azure Key Vault - functie voor voorlopig verwijderen gebruiken met PowerShell
description: De aanvraag voorbeelden van voorlopig verwijderen gebruiken met PowerShell-codefragmenten
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: mbaldwin
ms.openlocfilehash: 3d199442564964d4eb70878e7e6460fe550cd4c2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57452485"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell

Functie voor voorlopig verwijderen van Azure Key Vault de maakt het herstellen van verwijderde kluizen en -objecten van de kluis. Met name voorlopig verwijderen adressen de volgende scenario's:

- Ondersteuning voor het herstelbare verwijderen van een key vault
- Ondersteuning voor verwijdering van de herstelbare objecten van de sleutelkluis. sleutels, geheimen, en certificaten

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- Azure PowerShell 1.0.0 of hoger - als u niet dat al ingesteld hebt, Azure PowerShell installeren en deze aan uw Azure-abonnement koppelen, Zie [hoe u Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Er is een verouderde versie van onze Key Vault PowerShell uitvoeropmaak bestand dat de **kan** worden geladen in uw omgeving in plaats van de juiste versie. We zijn anticiperen op een bijgewerkte versie van PowerShell bevat de benodigde correctie voor de opmaak van de uitvoer en dit onderwerp wordt bijgewerkt op dat moment. De huidige oplossing, moet u dit probleem zich voordoet opmaak, is:
> - Gebruik de volgende query uit als u merkt dat u de functie voor voorlopig verwijderen niet ziet in dit onderwerp beschreven van de eigenschap enabled: `$vault = Get-AzKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Zie voor Key Vault specifieke naslaginformatie voor PowerShell, [Azure Key Vault PowerShell-referentie](/powershell/module/az.keyvault).

## <a name="required-permissions"></a>Vereiste machtigingen

Key Vault-bewerkingen worden afzonderlijk beheerd via machtigingen voor op rollen gebaseerd toegangsbeheer (RBAC) als volgt:

| Bewerking | Description | Machtigingen van gebruiker |
|:--|:--|:--|
|Lijst|Een lijst met verwijderde sleutelkluizen.|Microsoft.KeyVault/deletedVaults/read|
|Herstellen|Hiermee herstelt u een verwijderde key vault.|Microsoft.KeyVault/vaults/write|
|Opschonen|Een verwijderde key vault en alle bijbehorende inhoud verwijderd permanent.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Zie voor meer informatie over de machtigingen en access control [uw key vault beveiligen](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Inschakelen van voorlopig verwijderen

U inschakelen "voorlopig verwijderen' om toe te staan van herstel van een verwijderde key vault of objecten die zijn opgeslagen in een key vault.

> [!IMPORTANT]
> Inschakelen van 'voorlopig verwijderen' op een key vault is een onomkeerbare actie. Zodra de functie voor voorlopig verwijderen-eigenschap is ingesteld op 'true', kan niet worden gewijzigd of verwijderd.  

### <a name="existing-key-vault"></a>Bestaande key vault

Voor een bestaande sleutelkluis met de naam ContosoVault, moet u voorlopig verwijderen als volgt inschakelen. 

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Nieuwe key vault

Door toe te voegen van de functie voor voorlopig verwijderen inschakelen-vlag voor het inschakelen van voorlopig verwijderen voor een nieuwe key vault wordt uitgevoerd op Aanmaaktijd uw opdracht maken.

```powershell
New-AzKeyVault -Name "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Controleer of de activering van de functie voor voorlopig verwijderen

Uitvoeren om te controleren of een key vault voorlopig verwijderen ingeschakeld heeft, de *weergeven* opdracht en zoekt u de 'voorlopig verwijderen ingeschakeld?' kenmerk:

```powershell
Get-AzKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Sleutelkluis beveiligd verwijderen van een functie voor voorlopig verwijderen

De opdracht voor het verwijderen van een sleutelkluis wijzigingen in werking, afhankelijk van of de functie voor voorlopig verwijderen is ingeschakeld.

> [!IMPORTANT]
>Als u de volgende opdracht uit voor een key vault die geen functie voor voorlopig verwijderen ingeschakeld uitvoert, wordt u deze sleutelkluis en alle bijbehorende inhoud met geen opties voor herstel permanent verwijderd.

```powershell
Remove-AzKeyVault -VaultName 'ContosoVault'
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hoe uw sleutelkluizen worden beveiligd met voorlopig verwijderen

Met de functie voor voorlopig verwijderen ingeschakeld:

- Een verwijderde key vault wordt verwijderd uit de resourcegroep en geplaatst in een gereserveerde naamruimte, die zijn gekoppeld aan de locatie waar deze is gemaakt. 
- Verwijderde objecten, zoals sleutels, geheimen en certificaten, niet toegankelijk zijn, zolang de betreffende sleutelkluis de status verwijderd heeft is. 
- De DNS-naam voor een verwijderde key vault is gereserveerd, zo wordt voorkomen dat een nieuwe sleutelkluis met dezelfde naam wordt gemaakt.  

U kunt verwijderde sleutelkluizen, dat wordt gekoppeld aan uw abonnement weergeven met de volgende opdracht:

```powershell
PS C:\> Get-AzKeyVault -InRemovedState 
```

- *Id* kan worden gebruikt voor het identificeren van de resource bij het herstellen of verwijderen. 
- *Resource-ID* is de oorspronkelijke bron-ID van deze kluis. Omdat deze sleutelkluis nu in een verwijderde status is, bestaat er is geen bron met die resource-ID. 
- *Geplande datum opschonen* is als de kluis worden definitief verwijderd, als er geen actie ondernomen. De bewaartermijn, die wordt gebruikt voor het berekenen van de *opschonen datum gepland*, is 90 dagen.

## <a name="recovering-a-key-vault"></a>Herstellen van een key vault

Als u wilt herstellen van een key vault, door de key vault-naam, resourcegroep en locatie op te geven. Noteer de locatie en de resourcegroep van de verwijderde key vault, als u deze nodig voor het herstelproces hebt.

```powershell
Undo-AzKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Wanneer een key vault is hersteld, wordt een nieuwe resource gemaakt met de key vault oorspronkelijke resource-ID. Als de oorspronkelijke resourcegroep wordt verwijderd, moet een met dezelfde naam worden gemaakt voordat wordt geprobeerd herstel.

## <a name="deleting-and-purging-key-vault-objects"></a>Verwijderen en het verwijderen van key vault-objecten

De volgende opdracht wordt de sleutel 'ContosoFirstKey' in een key vault met de naam 'ContosoVault', die ingeschakeld voor voorlopig verwijderen is verwijderen:

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Met uw key vault voor voorlopig verwijderen zijn ingeschakeld, een verwijderde sleutel nog steeds weergegeven moet worden verwijderd, tenzij u expliciet verwijderde sleutels weergeven. De meeste bewerkingen voor een sleutel in de status verwijderd heeft mislukken, met uitzondering van de aanbieding, herstellen, verwijderen van een verwijderde sleutel. 

Bijvoorbeeld, de volgende opdracht worden verwijderde sleutels in de key vault 'ContosoVault':

```powershell
Get-AzKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Status van de overgang 

Wanneer u een sleutel in een key vault met de functie voor voorlopig verwijderen ingeschakeld verwijdert, wordt het duurt een paar seconden voor de overgang te voltooien. Tijdens deze overgang, wordt deze mogelijk weergegeven dat de sleutel niet in de actieve status of de status verwijderd heeft is. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Voorlopig verwijderen gebruiken met key vault-objecten

Net als bij sleutelkluizen blijft een verwijderde sleutel, het geheim of het certificaat, in verwijderde status maximaal 90 dagen, tenzij u deze herstellen of het opschonen. 

#### <a name="keys"></a>Sleutels

Een sleutel voorlopig verwijderde herstellen:

```powershell
Undo-AzKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Definitief verwijderen (ook wel bekend als opschonen) een voorlopig verwijderde key:

> [!IMPORTANT]
> Bezig met het verwijderen van een sleutel wordt definitief verwijderen en deze niet worden hersteld. 

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

De **herstellen** en **opschonen** acties hebben hun eigen machtigingen die zijn gekoppeld in een toegangsbeleid voor key vault. Voor een gebruiker of service-principal te kunnen zijn om uit te voeren een **herstellen** of **opschonen** actie, moeten ze de respectieve machtiging voor deze sleutel of geheim hebben. Standaard **opschonen** is niet toegevoegd aan een key vault-toegangsbeleid, wanneer de snelkoppeling naar de 'all' wordt gebruikt om alle machtigingen te verlenen. U moet expliciet verleent **opschonen** machtiging. 

#### <a name="set-a-key-vault-access-policy"></a>Een toegangsbeleid voor key vault instellen

De volgende opdracht verleent user@contoso.com toestemming voor het gebruik van verschillende bewerkingen voor sleutels in *ContosoVault* inclusief **opschonen**:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Als u een bestaande sleutelkluis die zojuist voorlopig verwijderen ingeschakeld heeft hebt, hebt u geen **herstellen** en **opschonen** machtigingen.

#### <a name="secrets"></a>Geheimen

Zoals sleutels, geheimen beheerd met hun eigen opdrachten:

- Een geheim met de naam SQLPassword verwijderen: 
```powershell
Remove-AzKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Een overzicht van alle verwijderde geheimen in een key vault: 
```powershell
Get-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Een geheim in de status verwijderd heeft herstellen: 
```powershell
Undo-AzKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Opschonen van een geheim status verwijderd: 

  > [!IMPORTANT]
  > Bezig met het verwijderen van een geheim definitief verwijderd en deze niet worden hersteld.

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Bezig met het verwijderen van een functie voor voorlopig verwijderen beveiligd sleutelkluis

> [!IMPORTANT]
> Een key vault of een van de daarin opgenomen objecten verwijderen, verwijderd definitief, wat betekent dat deze niet worden hersteld.

De functie opschonen wordt gebruikt om een sleutelkluis-object of een hele sleutelkluis, die is eerder voorlopig verwijderde definitief te verwijderen. Zoals in de vorige sectie wordt gedemonstreerd, kunnen objecten die zijn opgeslagen in een key vault met de functie voor voorlopig verwijderen ingeschakeld, gaat u door meerdere statussen:
- **Actieve**: voordat u deze verwijdert.
- **Voorlopig verwijderde**: nadat u hebt verwijderd, kunnen worden weergegeven en hersteld op actieve status.
- **Definitief verwijderd**: na het opschonen van Logboeken, kan niet worden hersteld.


Hetzelfde geldt voor de key vault. Als u wilt een voorlopig verwijderde sleutelkluis en de inhoud ervan permanent verwijderd, moet u de sleutelkluis zelf verwijderen.

### <a name="purging-a-key-vault"></a>Een sleutelkluis verwijderen

Wanneer een key vault wordt verwijderd, worden de volledige inhoud ervan definitief verwijderd, met inbegrip van sleutels, geheimen en certificaten. Als u wilt een voorlopig verwijderde sleutelkluis leegmaken, gebruikt u de `Remove-AzKeyVault` opdracht met de optie `-InRemovedState` en door te geven van de locatie van de verwijderde key vault met de `-Location location` argument. U kunt zoeken naar de locatie van een verwijderde kluis met de opdracht `Get-AzKeyVault -InRemovedState`.

```powershell
Remove-AzKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

### <a name="purge-permissions-required"></a>Opschonen van de machtigingen die vereist zijn
- Als u wilt verwijderen van een verwijderde key vault, moet de gebruiker RBAC-machtiging voor de *Microsoft.KeyVault/locations/deletedVaults/purge/action* bewerking. 
- Als u een verwijderde key vault, moet de gebruiker RBAC-machtiging voor de *Microsoft.KeyVault/deletedVaults/read* bewerking. 
- Alleen de beheerder van een abonnement heeft standaard de machtigingen toegewezen. 

### <a name="scheduled-purge"></a>Geplande opschonen

Aanbieding verwijderde key vault-objecten wordt ook weergegeven wanneer ze zijn gepland om te worden opgeschoond met Key Vault. *Geplande datum opschonen* geeft aan wanneer een key vault-object wordt definitief verwijderd, als er geen actie ondernomen. Standaard is de bewaarperiode voor een object verwijderde sleutelkluis 90 dagen.

>[!IMPORTANT]
>Een object opgeschoonde kluis, geactiveerd door de *opschonen datum gepland* veld, wordt definitief verwijderd. Het kan niet worden hersteld.

## <a name="other-resources"></a>Meer informatie

- Zie voor een overzicht van de functie voor voorlopig verwijderen van de Sleutelkluis, [overzicht van Azure Key Vault-functie voor voorlopig verwijderen](key-vault-ovw-soft-delete.md).
- Zie voor een algemeen overzicht van gebruik van Azure Key Vault, [wat is Azure Key Vault?](key-vault-overview.md).

