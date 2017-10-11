---
title: Azure Sleutelkluis-oplossing in Log Analytics | Microsoft Docs
description: U kunt de oplossing voor Azure Sleutelkluis in logboekanalyse Azure Sleutelkluis-Logboeken.
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 651586e0846ffb22a23e64b73c2cc614980d9b92
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a>Azure Key Vault Analytics-oplossing in Log Analytics

![Sleutelkluis symbool](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

Met de oplossing Azure Key Vault in Log Analytics kunt u de AuditEvent-logboeken van Azure Key Vault controleren.

Voor het gebruik van de oplossing, moet u logboekregistratie van Azure Sleutelkluis diagnostische gegevens inschakelen en het omleiden van de diagnostische gegevens naar een werkruimte voor logboekanalyse. Het is niet nodig zijn voor het schrijven van de logboeken naar Azure Blob-opslag.

> [!NOTE]
> In januari 2017, de ondersteunde manier van het verzenden van Logboeken van de Sleutelkluis met logboekanalyse gewijzigd. Als de Sleutelkluis-oplossing u toont *(afgeschaft)* in de titel verwijzen naar [migreren van de oude Sleutelkluis oplossing](#migrating-from-the-old-key-vault-solution) voor stappen die u moet volgen.
>
>

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies voor het installeren en configureren van de Azure Sleutelkluis-oplossing:

1. Inschakelen van de Azure Sleutelkluis-oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Inschakelen van logboekregistratie van diagnostische gegevens voor de Sleutelkluis-bronnen te bewaken, met behulp van de [portal](#enable-key-vault-diagnostics-in-the-portal) of [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>Sleutelkluis diagnostische gegevens in de portal inschakelen

1. Navigeer in de Azure-portal naar de Sleutelkluis-bron om te controleren
2. Selecteer *diagnostische logboeken* om de volgende pagina te openen

   ![afbeelding van Azure Sleutelkluis-tegel](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van Azure Sleutelkluis-tegel](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. Om diagnostische gegevens inschakelen, klikt u op *op* onder *Status*
5. Klik op het selectievakje voor *verzenden met Log Analytics*
6. Selecteer een bestaande werkruimte voor logboekanalyse of een werkruimte maken
7. Om in te schakelen *AuditEvent* Logboeken, klikt u op het selectievakje onder logboek
8. Klik op *opslaan* waarmee de logboekregistratie van diagnostische gegevens met Log Analytics

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Sleutelkluis diagnostische gegevens met behulp van PowerShell inschakelen
De volgende PowerShell-script bevat een voorbeeld van het gebruik van `Set-AzureRmDiagnosticSetting` Diagnostische logboekregistratie voor Sleutelkluis in te schakelen:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Bekijk de details van Azure Sleutelkluis gegevens verzamelen
Azure Sleutelkluis-oplossing verzamelt diagnostische logboeken rechtstreeks uit de Sleutelkluis.
Het is niet nodig om te schrijven van de logboeken naar Azure Blob-opslag en geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel bevat de methoden van de collectie en andere informatie over hoe gegevens worden verzameld voor Azure Sleutelkluis.

| Platform | Directe agent | Systems Center Operations Manager-agent | Azure | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | bij ontvangst |

## <a name="use-azure-key-vault"></a>Azure Key Vault gebruiken
Nadat u [installeren van de oplossing](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), de Sleutelkluis-gegevens weergeven door te klikken op de **Azure Key Vault** tegel van de **overzicht** pagina van logboekanalyse.

![afbeelding van Azure Sleutelkluis-tegel](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Nadat u op de **overzicht** herhalen, kunt u samenvattingen van de logboekbestanden weergeven en ga vervolgens omlaag de gedetailleerde informatie voor de volgende categorieën:

* Volume van alle sleutelkluisbewerkingen gedurende een periode
* Kan bewerking volumes gedurende een periode
* Gemiddelde latentie van operationele per bewerking
* Quality of service voor bewerkingen met het aantal bewerkingen die meer dan 1000 ms te nemen en een lijst met bewerkingen die meer dan 1000 ms uitvoeren

![afbeelding van Azure Sleutelkluis-dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![afbeelding van Azure Sleutelkluis-dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Details voor elke bewerking weergeven
1. Op de **overzicht** pagina, klikt u op de **Azure Key Vault** tegel.
2. Op de **Azure Key Vault** dashboard, Controleer de overzichtsgegevens in een van de blades, en klik op een om gedetailleerde informatie over het in de zoekpagina logboek weer te geven.

    U kunt op elk van de zoekpagina logboek kunt resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="log-analytics-records"></a>Log Analytics-records
De Azure Sleutelkluis-oplossing analyseert records die een soort **KeyVaults** die worden verzameld van [AuditEvent logboeken](../key-vault/key-vault-logging.md) in Azure Diagnostics.  Eigenschappen voor deze records zijn in de volgende tabel:  

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |IP-adres van de client die de aanvraag heeft ingediend |
| Category | *AuditEvent* |
| CorrelationId |Een optionele GUID die de client kan doorgeven om de logboeken aan de clientzijde te relateren aan (Sleutelkluis-)logboeken aan de servicezijde. |
| DurationMs |De tijd die nodig was om de REST-API-aanvraag af te handelen in milliseconden. Dit is niet opgenomen netwerklatentie, zodat de tijd die u aan de clientzijde meet mogelijk niet overeenkomt met deze tijd. |
| httpStatusCode_d |HTTP-statuscode geretourneerd door de aanvraag (bijvoorbeeld *200*) |
| id_s |De unieke ID van de aanvraag |
| identity_claim_appid_g | GUID voor de toepassings-id |
| OperationName |Naam van de bewerking, zoals beschreven in [logboekregistratie van Azure Sleutelkluis](../key-vault/key-vault-logging.md) |
| OperationVersion |REST-API-versie die door de client aangevraagde (bijvoorbeeld *2015-06-01*) |
| requestUri_s |URI van de aanvraag |
| Resource |Naam van de sleutelkluis |
| ResourceGroup |Resourcegroep van de sleutelkluis |
| ResourceId |Azure Resource Manager-resource-id. Voor Sleutelkluis-Logboeken is dit de Sleutelkluis-bron-ID. |
| ResourceProvider |*MICROSOFT CORPORATION. KEYVAULT* |
| ResourceType | *KLUIZEN* |
| ResultSignature |HTTP-status (bijvoorbeeld *OK*) |
| ResultType |Resultaat van de REST-API-aanvraag (bijvoorbeeld *geslaagd*) |
| SubscriptionId |Azure-abonnement-ID van het abonnement met de Sleutelkluis |

## <a name="migrating-from-the-old-key-vault-solution"></a>Migreren van de oude Sleutelkluis-oplossing
In januari 2017, de ondersteunde manier van het verzenden van Logboeken van de Sleutelkluis met logboekanalyse gewijzigd. Deze wijzigingen bieden de volgende voordelen:
+ Logboeken geschreven rechtstreeks met logboekanalyse zonder te hoeven gebruiken van een opslagaccount
+ Minder latentie vanaf het moment wanneer logboeken worden gegenereerd voor hen beschikbaar worden gesteld in Log Analytics
+ Minder configuratiestappen
+ Een algemene indeling voor alle typen Azure diagnostics

De bijgewerkte oplossing gebruiken:

1. [Diagnostische gegevens worden verzonden met logboekanalyse rechtstreeks vanuit de Sleutelkluis configureren](#enable-key-vault-diagnostics-in-the-portal)  
2. De oplossing Azure Key Vault inschakelen met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md)
3. Bijwerken van een opgeslagen query's, dashboards of waarschuwingen te gebruiken van het nieuwe gegevenstype
  + Type is gewijzigd van: KeyVaults naar AzureDiagnostics. Het ResourceType kunt u filteren op Sleutelkluis Logboeken.
  - In plaats van: `Type=KeyVaults`, gebruiken`Type=AzureDiagnostics ResourceType=VAULTS`
  + Velden: (veldnamen zijn hoofdlettergevoelig)
  - Voor elk veld dat een achtervoegsel van \_s, \_d, of \_g in de naam wijzigen van het eerste teken in kleine letters
  - Voor elk veld dat een achtervoegsel van \_o in naam van de gegevens is opgesplitst in afzonderlijke velden op basis van de geneste veldnamen. Bijvoorbeeld, wordt de UPN van de aanroepfunctie opgeslagen in een veld`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   - Veld CallerIpAddress gewijzigd in CallerIPAddress
   - Veld RemoteIPCountry is niet meer aanwezig
4. Verwijder de *Sleutelkluis Analytics (afgeschaft)* oplossing. Als u met behulp van PowerShell, gebruikt u`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Gegevens verzameld voordat de wijziging niet zichtbaar in de nieuwe oplossing is. U kunt blijven opvragen voor deze gegevens met behulp van de oude Type en de veldnamen.

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) om gedetailleerde gegevens voor Azure Sleutelkluis te bekijken.
