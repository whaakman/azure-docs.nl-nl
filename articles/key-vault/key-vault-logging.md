---
title: Azure Key Vault-Logboeken | Microsoft Docs
description: Deze zelfstudie helpt u op weg met de logboekregistratie van Azure Sleutelkluis.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: c9d8dd366ecabe9eb508998d526ddfe7b1da300d
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960502"
---
# <a name="azure-key-vault-logging"></a>Logboekregistratie voor Azure Key Vault

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Nadat u een of meer sleutelkluizen hebt gemaakt, zult u waarschijnlijk om te controleren hoe en wanneer uw sleutelkluizen toegankelijk is, en door wie zijn. U kunt dit doen door registratie inschakelen voor Azure Key Vault, die worden gegevens opgeslagen in Azure storage-account dat u opgeeft. Een nieuwe container met de naam **insights-logs-auditevent** wordt automatisch gemaakt voor het opgegeven opslagaccount. U kunt hetzelfde opslagaccount gebruiken voor het verzamelen van Logboeken voor meerdere sleutelkluizen.

U krijgt toegang tot uw logboekgegevens 10 minuten (maximaal) nadat de sleutelkluis-bewerking. In de meeste gevallen gaat het echter veel sneller.  Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Gebruik standaardmethoden van Azure voor toegangsbeheer om uw logboeken te beveiligen door het aantal gebruikers te beperken dat toegang heeft tot de logboeken.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.

Deze zelfstudie helpt u op weg met de logboekregistratie van Azure Sleutelkluis. U hebt een opslagaccount maken, logboekregistratie inschakelen en interpreteren van de verzamelde gegevens.  

> [!NOTE]
> Deze zelfstudie bevat geen instructies voor het maken van sleutelkluizen, sleutels of geheimen. Voor deze informatie, Zie [wat is Azure Key Vault?](key-vault-overview.md). Of, voor platformoverschrijdende Azure CLI-instructies, Zie [deze equivalente zelfstudie](key-vault-manage-with-cli2.md).
>
> Dit artikel bevat instructies voor Azure PowerShell voor het bijwerken van diagnostische gegevens vastleggen. U kunt ook Diagnostische logboekregistratie bijwerken met behulp van Azure Monitor in de **diagnostische logboeken** sectie van de Azure portal. 
>

Zie voor informatie over Key Vault, [wat is Azure Key Vault?](key-vault-whatis.md). Zie voor informatie over waar de Key Vault beschikbaar is, de [pagina met prijzen](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een bestaande sleutelkluis die u hebt gebruikt.  
* Azure PowerShell, minimaal versie 1.0.0. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren en te koppelen aan uw Azure-abonnement. Als u Azure PowerShell al hebt geïnstalleerd en de versie van de Azure PowerShell-console niet weet, voert u `$PSVersionTable.PSVersion`.  
* Voldoende opslagruimte op Azure voor uw Sleutelkluis-logboeken.

## <a id="connect"></a>Verbinding maken met uw key vault-abonnement

De eerste stap bij het instellen van sleutels logboekregistratie is punt Azure PowerShell aan de sleutelkluis die u wilt registreren.

Start een Azure PowerShell-sessie en aanmelden bij uw Azure-account met behulp van de volgende opdracht uit:  

```PowerShell
Connect-AzAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen die gekoppeld aan dit account zijn. PowerShell gebruikt standaard het eerste item.

Het is mogelijk om op te geven van het abonnement dat u gebruikt voor het maken van uw key vault. Voer de volgende opdracht om te zien van de abonnementen voor uw account:

```PowerShell
Get-AzSubscription
```

Voer vervolgens de volgende opdracht om op te geven van het abonnement dat is gekoppeld aan de sleutelkluis die u zult worden logboekregistratie:

```PowerShell
Set-AzContext -SubscriptionId <subscription ID>
```

PowerShell verwijst naar het juiste abonnement is een belangrijke stap, met name als u meerdere abonnementen die zijn gekoppeld aan uw account hebt. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het configureren van Azure PowerShell.

## <a id="storage"></a>Een opslagaccount voor uw logboeken maken

Hoewel u een bestaand opslagaccount voor uw Logboeken gebruiken kunt, maken we een storage-account, speciaal voor Sleutelkluis-Logboeken. Voor het gemak wanneer we hebben dit later opgeven, moet worden de details opgeslagen in een variabele met de naam **sa**.

Nog gemakkelijker te maken van beheer, we dezelfde resourcegroep ook gebruikt als het account dat die de sleutelkluis bevat. Uit de [aan de slag-zelfstudie](key-vault-get-started.md), deze resourcegroep de naam **ContosoResourceGroup**, en we blijven gebruiken van de locatie Oost-Azië. Vervang deze waarden door uw eigen, zoals van toepassing:

```PowerShell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Als u een bestaand opslagaccount gebruiken, moet hetzelfde abonnement worden gebruikt als uw key vault. En deze moet gebruiken de Azure Resource Manager-implementatiemodel, in plaats van het klassieke implementatiemodel.
>
>

## <a id="identify"></a>De sleutelkluis voor uw logboeken identificeren

In de [aan de slag-zelfstudie](key-vault-get-started.md), de naam van de sleutelkluis is **ContosoKeyVault**. We blijven gebruiken die naam en opslaan van de gegevens in een variabele met de naam **kv**:

```PowerShell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a id="enable"></a>Logboekregistratie inschakelen

Als u wilt logboekregistratie inschakelen voor Key Vault, gebruiken we de **Set AzDiagnosticSetting** cmdlet, samen met de variabelen die we hebben gemaakt voor het nieuwe opslagaccount en de key vault. Stellen we ook de **-ingeschakeld** markering **$true** en de categorie instellen op **AuditEvent** (de enige categorie voor logboekregistratie van Key Vault):

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

De uitvoer ziet er als volgt uit:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0

Deze uitvoer bevestigt dat logboekregistratie nu is ingeschakeld voor uw key vault en informatie wordt opgeslagen in uw storage-account.

U kunt eventueel een bewaarbeleid instellen voor uw Logboeken, zodat oudere logboeken automatisch worden verwijderd. Bijvoorbeeld bewaarbeleid instellen door in te stellen de **- RetentionEnabled** markering **$true**, en stel de **- RetentionInDays** parameter **90**zodat logboeken die ouder zijn dan 90 dagen automatisch worden verwijderd.

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Wat wordt in het logboek vastgelegd?

* Alle geverifieerde REST API-aanvragen, met inbegrip van mislukte aanvragen als gevolg van toegangsmachtigingen, systeemfouten of ongeldige aanvragen.
* Bewerkingen voor de sleutelkluis zelf, met inbegrip van het maken, verwijderen en toegangsbeleid voor key vault instellen en bijwerken van de kenmerken van sleutelkluizen, zoals tags.
* Bewerkingen voor sleutels en geheimen in de key vault, met inbegrip van:
  * Het maken, wijzigen of verwijderen van deze sleutels of geheimen.
  * Ondertekenen, controleren, versleutelen, ontsleutelen, wrapping en uitpakken sleutels, geheimen, en aanbieding sleutels en geheimen (en hun versies) aan.
* Niet-geverifieerde aanvragen die in een 401-respons resulteren. Voorbeelden zijn aanvragen waarvoor geen bearer-token met ongeldige of verlopen, of waarvoor een ongeldig token.  

## <a id="access"></a>Toegang tot uw logboeken

Key Vault-logboeken worden opgeslagen in de **insights-logs-auditevent** container in het opslagaccount die u hebt opgegeven. Als u de logboeken, moet u downloaden van blobs.

Maak eerst een variabele voor de containernaam. U gebruikt deze variabele in de rest van de procedure.

```PowerShell
$container = 'insights-logs-auditevent'
```

Als u alle blobs in deze container, typ:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

De uitvoer ziet er ongeveer als volgt uit:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

- - -
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Zoals u in deze uitvoer zien kunt, volgen de blobs een naamconventie: `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

De datum- en tijdwaarden maken gebruik van UTC.

Omdat u hetzelfde opslagaccount gebruiken kunt voor het verzamelen van Logboeken voor meerdere resources, is de volledige resource-ID in de naam van de blob is nuttig voor toegang tot of het downloaden van alleen de blobs die u nodig hebt. Maar eerst laten we zien hoe u alle blobs kunt downloaden.

Maak een map om de blobs te downloaden. Bijvoorbeeld:

```PowerShell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

Haal vervolgens een lijst met alle blobs op:  

```PowerShell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Sluis deze lijst via **Get-AzStorageBlobContent** voor het downloaden van de blobs naar de doelmap:

```PowerShell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Wanneer u deze tweede opdracht uitvoert de **/** scheidingsteken in de blobnamen een volledige mapstructuur onder de doelmap gemaakt. U gebruikt deze structuur om te downloaden en opslaan van de blobs als bestanden.

Als u alleen specifieke blobs wilt downloaden, moet u jokertekens gebruiken. Bijvoorbeeld:

* Als u meerdere sleutelkluizen hebt en het logboek voor slechts één sleutelkluis wilt downloaden met de naam CONTOSOKEYVAULT3:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Als u meerdere resourcegroepen hebt en logboeken voor slechts één resourcegroep wilt downloaden, gebruikt u `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Als u alle logboeken downloaden die u voor de maand januari 2019 wilt, gebruikt u `-Blob '*/year=2019/m=01/*'`:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

We gaan zo kijken wat er precies in de logboeken staat. Maar voordat we verdergaan met het die, moet u twee meer opdrachten weten:

* De status van diagnostische instellingen voor uw sleutelkluisresource opvragen: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Logboekregistratie voor uw sleutelkluisresource uitschakelen: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a id="interpret"></a>De Sleutelkluis-logboekgegevens interpreteren

Afzonderlijke blobs worden opgeslagen als tekst, die is opgemaakt als een JSON-blob. We bekijken een voorbeeld van invoer voor logboekbestanden. Voer deze opdracht uit:

```PowerShell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

Het retourneert logboek een vermelding die vergelijkbaar is met deze:

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

De volgende tabel bevat de namen en beschrijvingen:

| Veldnaam | Description |
| --- | --- |
| **tijd** |De datum en tijd in UTC. |
| **ResourceId** |Azure Resource Manager resource-ID. Voor Sleutelkluis-Logboeken is dit altijd de Key Vault-resource-ID. |
| **OperationName** |Naam van de bewerking, zoals beschreven in de volgende tabel. |
| **operationVersion** |REST-API-versie is aangevraagd door de client. |
| **Categorie** |Het type van het resultaat. Voor Sleutelkluis-Logboeken, **AuditEvent** is de enige beschikbare waarde. |
| **resultType** |Resultaat van de REST-API-aanvraag. |
| **resultSignature** |HTTP-status. |
| **ResultDescription** |Extra beschrijving van het resultaat, indien beschikbaar. |
| **durationMs** |De tijd die nodig was om de REST-API-aanvraag af te handelen in milliseconden. Hierbij wordt geen rekening gehouden met de netwerklatentie, zodat de tijd die u aan de clientzijde meet mogelijk niet overeenkomt met de tijd die hier wordt weergegeven. |
| **callerIpAddress** |IP-adres van de client die de aanvraag heeft ingediend. |
| **correlationId** |Een optionele GUID die de client kan doorgeven om de logboeken aan de clientzijde te relateren aan (Sleutelkluis-)logboeken aan de servicezijde. |
| **identity** |De identiteit van het token dat is opgegeven in de REST-API-aanvraag. Dit is meestal een 'gebruiker' een 'service-principal' of een combinatie 'user + appId, zoals in het geval van een aanvraag die het resultaat is van een Azure PowerShell-cmdlet. |
| **Eigenschappen** |Informatie die varieert op basis van de bewerking (**operationName**). In de meeste gevallen bevat dit veld clientgegevens (de gebruiker agent tekenreeks doorgegeven door de client), de exacte URI van de REST-API-aanvraag en de HTTP-statuscode. Bovendien, als een object wordt geretourneerd als gevolg van een aanvraag (bijvoorbeeld **KeyCreate** of **VaultGet**), bevat ook de key URI (als 'id'), vault URI of secret URI. |

De **operationName** velwaarden *ObjectVerb* indeling. Bijvoorbeeld:

* Alle sleutelkluisbewerkingen hebben de `Vault<action>` opmaken, zoals `VaultGet` en `VaultCreate`.
* Alle sleutelbewerkingen hebben de `Key<action>` opmaken, zoals `KeySign` en `KeyList`.
* Alle geheime bewerkingen hebben de `Secret<action>` opmaken, zoals `SecretGet` en `SecretListVersions`.

De volgende tabel bevat de **operationName** waarden en de bijbehorende REST-API-opdrachten:

| operationName | REST-API-opdracht |
| --- | --- |
| **Verificatie** |Verificatie via Azure Active Directory-eindpunt |
| **VaultGet** |[Informatie over een sleutelkluis ophalen](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Een sleutelkluis maken of bijwerken](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Een sleutelkluis verwijderen](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Een sleutelkluis bijwerken](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Alle sleutelkluizen in een resourcegroep weergeven](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Een sleutel maken](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Informatie over een sleutel ophalen](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Een sleutel in een kluis importeren](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Back-up van een sleutel](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Een sleutel verwijderen](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Een sleutel herstellen](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Aanmelden met een sleutel](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Verifiëren met een sleutel](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Een sleutel inpakken](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Een sleutel uitpakken](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Versleutelen met een sleutel](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Ontsleutelen met een sleutel](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Een sleutel bijwerken](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[De sleutels in een kluis weergeven](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[De versies van een sleutel weergeven](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Een geheim maken](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Een geheim ophalen](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Een geheim bijwerken](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Een geheim verwijderen](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Geheimen in een kluis weergeven](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Versies van een geheim weergeven](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Log Analytics gebruiken

U kunt de oplossing Key Vault in Azure Log Analytics gebruiken om te controleren van Key Vault **AuditEvent** Logboeken. In Log Analytics gebruikt u Logboeken-query's kunt analyseren en ophalen van de informatie die u nodig hebt. 

Zie [Azure Key Vault solution in Log Analytics](../azure-monitor/insights/azure-key-vault.md) (De oplossing Azure Key Vault in Log Analytics) voor meer informatie. In dit artikel bevat ook instructies als u nodig hebt voor het migreren van de oude Key Vault-oplossing die tijdens de preview van Log Analytics, waar u eerst uw logboeken naar een Azure storage-account geleid en geconfigureerd Log Analytics om van daaruit te lezen.

## <a id="next"></a>Volgende stappen

Zie voor een zelfstudie die gebruikmaakt van Azure Key Vault in een .NET-webtoepassing, [gebruik Azure Key Vault vanuit een webtoepassing](tutorial-net-create-vault-azure-web-app.md).

Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.

Zie voor een lijst van Azure PowerShell 1.0-cmdlets voor Azure Key Vault, [Azure Key Vault-cmdlets](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).

Zie voor een zelfstudie over sleutelrotatie en logboekcontrole met Azure Key Vault, [Key Vault instellen met end-to-end sleutelrotatie en controle](key-vault-key-rotation-log-monitoring.md).
