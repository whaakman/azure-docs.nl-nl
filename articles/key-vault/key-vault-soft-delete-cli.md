---
ms.assetid: 
title: Azure Sleutelkluis - voorlopig verwijderen gebruiken met CLI
description: Case voorbeelden van voorlopig verwijderen gebruiken met CLI codefragmenten
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 3ee2c5dfb99d734cde25894174466b8e49823c67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Sleutelkluis voorlopig verwijderen gebruiken met CLI

Azure Key Vault van voorlopige verwijderingen functie kunt herstellen van verwijderde kluizen en objecten van de kluis. In het bijzonder voorlopig verwijderen adressen van de volgende scenario's:

- Ondersteuning voor herstelbare verwijdering van een sleutelkluis
- Ondersteuning voor verwijdering van de herstelbare objecten van de sleutelkluis. sleutels, geheimen, en certificaten

## <a name="prerequisites"></a>Vereisten

- Azure CLI 2.0 - als u deze instelling niet voor uw omgeving hebt, Zie [Key Vault beheren met CLI 2.0](key-vault-manage-with-cli2.md).

Zie voor Sleutelkluis specifieke informatie voor CLI [Azure CLI 2.0 Sleutelkluis verwijzing](https://docs.microsoft.com/cli/azure/keyvault).

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

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Nieuwe sleutelkluis

Voorlopig verwijderen voor een nieuwe sleutelkluis inschakelen tijdens het maken wordt gedaan door toe te voegen de vlag voorlopig verwijderen inschakelen voor uw opdracht maken.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Controleren inschakelen voor voorlopig verwijderen

Om te controleren of er een sleutelkluis voorlopig verwijderen ingeschakeld is, voer de *weergeven* opdracht en zoekt u de 'voorlopig verwijderen Enabled?' kenmerk en de instelling true of false.

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Verwijderen van een sleutelkluis beveiligd door voorlopig verwijderen

De opdracht uit om een sleutelkluis verwijderen (of verwijderen) hetzelfde is gebleven, maar het gedrag verandert, afhankelijk van of u voorlopig verwijderen of niet hebt ingeschakeld.

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
>Als u de vorige opdracht voor een sleutelkluis die geen voorlopig verwijderen ingeschakeld uitvoert, wordt u deze sleutelkluis en alle bijbehorende inhoud zonder opties voor herstel permanent verwijderd.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hoe voorlopig verwijderen beschermt uw sleutelkluizen

Ingeschakeld met voorlopig verwijderen:

- Wanneer een sleutelkluis wordt verwijderd, is deze verwijderd uit de resourcegroep en geplaatst in een gereserveerde naamruimte die alleen wordt gekoppeld aan de locatie waar ze werden gemaakt. 
- Objecten in een verwijderde sleutel kluis, zoals sleutels, geheimen en certificaten, zijn niet toegankelijk en blijven ook terwijl hun insluitende sleutelkluis de status verwijderd heeft wordt. 
- De DNS-naam voor een sleutelkluis in een verwijderde status is nog steeds gereserveerd, zodat een nieuwe sleutelkluis met dezelfde naam kan niet worden gemaakt.  

U kunt verwijderde status sleutelkluizen, dat wordt gekoppeld aan uw abonnement weergeven met de volgende opdracht:

```azurecli
az keyvault list-deleted
```

De *Resource-ID* verwijst in de uitvoer naar de oorspronkelijke bron-ID van deze kluis. Aangezien deze sleutelkluis nu in een verwijderde staat bevindt is, bestaat er is geen resource met die resource-ID. De *Id* veld kan worden gebruikt voor het identificeren van de resource als herstellen of verwijderen. De *opschonen datum gepland* veld geeft aan wanneer de kluis wordt definitief verwijderd (verwijderd) als er geen actie voor deze verwijderde kluis ondernomen. De bewaartermijn gebruikt voor het berekenen van de *opschonen datum gepland*, is 90 dagen.

## <a name="recovering-a-key-vault"></a>Een sleutelkluis herstellen

Om te herstellen een sleutelkluis, moet u de naam van de sleutelkluis, resourcegroep en locatie opgeven. Noteer de locatie en de resourcegroep van de verwijderde sleutelkluis als u deze voor het herstelproces van een sleutelkluis nodig.

```azurecli
az keyvault recover --location westus --name ContosoVault
```

Wanneer een sleutelkluis is hersteld, is het resultaat een nieuwe resource met de sleutelkluis oorspronkelijke bron-ID. Als de resourcegroep waar de sleutelkluis bestond is verwijderd, moet een nieuwe resourcegroep met dezelfde naam worden gemaakt voordat de sleutelkluis kan worden hersteld.

## <a name="key-vault-objects-and-soft-delete"></a>Sleutelkluis-objecten en voorlopig verwijderen

Voor een sleutel 'ContosoFirstKey' in een sleutelkluis met de naam 'ContosoVault' met voorlopig verwijderen ingeschakeld, hier van wilt hoe u verwijderen die sleutel.

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Aan de sleutelkluis is ingeschakeld voor voorlopig verwijderen, weergegeven een verwijderde sleutel nog steeds zoals deze wordt verwijderd, behalve, wanneer u expliciet aanbiedt of verwijderde sleutels ophalen. De meeste bewerkingen voor een sleutel in de status verwijderd heeft mislukt met uitzondering van de aanbieding een verwijderde sleutel, herstellen of leegmaakt. 

Bijvoorbeeld, om aan te vragen tot sleutels van de lijst verwijderd in een sleutelkluis, gebruik de volgende opdracht:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Transitiestatus 

Wanneer u een sleutel in een sleutelkluis met de soft-delete is ingeschakeld verwijderen, wordt het duurt een paar seconden voor de overgang te voltooien. Tijdens deze transitiestatus zal verschijnen de sleutel is niet in de actieve status of de status verwijderd heeft. Met deze opdracht wordt een lijst alle verwijderde sleutels in de sleutelkluis met de naam 'ContosoVault'.

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Voorlopig verwijderen gebruiken met sleutelkluis-objecten

Net zoals sleutelkluizen, een verwijderde sleutel geheim of certificaat blijft in verwijderde staat bevindt voor maximaal 90 dagen tenzij u deze herstellen of het opschonen. 

#### <a name="keys"></a>Sleutels

Een verwijderde sleutel herstellen:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Een sleutel permanent verwijderen:

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
>Bezig met het verwijderen van een sleutel wordt permanent verwijderd, wat betekent dat deze kan niet worden hersteld.

De **herstellen** en **opschonen** acties hebben hun eigen machtigingen die zijn gekoppeld in een sleutelkluis-beleid. Voor een gebruiker of service-principal te kunnen uitvoeren een **herstellen** of **opschonen** actie ze gemachtigd bent de respectieve voor dat object (sleutel of geheim) in het toegangsbeleid van de sleutelkluis. Standaard de **opschonen** machtiging is niet toegevoegd aan een sleutelkluis toegangsbeleid wanneer de snelkoppeling 'all' wordt gebruikt voor alle machtigingen verlenen aan een gebruiker. U moet expliciet verlenen **opschonen** machtiging. De volgende opdracht bijvoorbeeld verleent user@contoso.com toestemming voor het uitvoeren van verschillende bewerkingen voor sleutels in *ContosoVault* inclusief **opschonen**.

#### <a name="set-a-key-vault-access-policy"></a>Beleid voor sleutelkluis toegang instellen

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Als u een bestaande sleutelkluis die ingeschakeld voor voorlopig verwijderen NET heeft hebt, hebt u geen **herstellen** en **opschonen** machtigingen.

#### <a name="secrets"></a>Geheimen

Zoals sleutels, worden geheimen in een sleutelkluis met hun eigen opdrachten beheerd op. Te volgen, zijn de opdrachten voor het verwijderen van, aanbieden, herstellen en geheimen verwijderen.

- Een geheim dat met de naam SQLPassword verwijderen: 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- Lijst van alle verwijderde geheimen in een sleutelkluis: 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- Een geheim in de status verwijderd heeft herstellen: 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- Opschonen van een geheim in verwijderde staat bevindt: 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
>Bezig met het verwijderen van een geheim wordt permanent verwijderd, wat betekent dat deze kan niet worden hersteld.

## <a name="purging-and-key-vaults"></a>Hiervoor en sleutel kluizen

### <a name="key-vault-objects"></a>Sleutelkluis-objecten

Opruimen van een sleutel wordt geheim of certificaat permanent verwijderd, wat betekent dat deze kan niet worden hersteld. De sleutelkluis die deel uitmaakt van het verwijderde object echter blijven behouden als alle andere objecten in de sleutelkluis. 

### <a name="key-vaults-as-containers"></a>Sleutel-kluizen als containers
Wanneer een sleutelkluis is opgeschoond, worden alle inhoud, met inbegrip van sleutels, geheimen en certificaten, permanent verwijderd. Als u wilt een sleutelkluis verwijderen, gebruikt u de `az keyvault purge` opdracht. U kunt zoeken naar de locatie van uw abonnement verwijderd sleutelkluizen met de opdracht `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
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

