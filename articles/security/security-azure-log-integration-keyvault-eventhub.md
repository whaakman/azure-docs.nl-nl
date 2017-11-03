---
title: Logboeken van Azure Sleutelkluis integreert met behulp van Event Hubs | Microsoft Docs
description: Zelfstudie waarmee u de benodigde stappen voor Sleutelkluis-logboeken beschikbaar te maken voor een SIEM met behulp van de integratie van Azure-logboek
services: security
author: barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.topic: article
ms.date: 08/07/2017
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 3cd80817bf8b2ef2f66e9942eddc186a3eb5b5e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Zelfstudie voor Azure Log-integratie: proces Azure Key Vault gebeurtenissen met behulp van Event Hubs

Integratie van Azure Log kunt u geregistreerde gebeurtenissen ophalen en deze beschikbaar aan uw informatie en event management (SIEM) beveiligingssysteem. Deze zelfstudie toont een voorbeeld van hoe Azure Log-integratie kunnen worden gebruikt voor het verwerken van logboeken die zijn verkregen via Azure Event Hubs.
 
Gebruik deze zelfstudie om bekend te raken met hoe Azure Log-integratie en Event Hubs samen werken met de voorbeeld-stappen te volgen en kennis van hoe de oplossing biedt ondersteuning voor elke stap. U kunt vervolgens nemen wat u hebt geleerd hier uw eigen stappen ter ondersteuning van de unieke vereisten van uw bedrijf maken.

>[!WARNING]
De stappen en de opdrachten in deze zelfstudie zijn niet bedoeld om te worden gekopieerd en geplakt. Ze zijn alleen voorbeelden. Gebruik de PowerShell-opdrachten 'als zodanig' niet in uw productieomgeving. U moet deze op basis van uw omgeving aanpassen.


Deze zelfstudie wordt u door het proces van nemen Azure Key Vault activiteit vastgelegd in een event hub en het beschikbaar maken als JSON-bestanden naar uw SIEM-systeem. Vervolgens kunt u uw SIEM-systeem voor het verwerken van de JSON-bestanden configureren.

>[!NOTE]
>De meeste van de stappen in deze zelfstudie hebben betrekking op het configureren van sleutelkluizen, opslagaccounts en event hubs. De specifieke Azure-logboek integratiestappen zijn aan het einde van deze zelfstudie. Voer deze stappen niet uit in een productieomgeving. Ze zijn bedoeld voor een testomgeving alleen. Voordat u ze in productie, moet u de stappen aanpassen.

Informatie over de manier helpt u begrijpen van de redenen achter elke stap. Koppelingen naar andere artikelen kunnen u meer details op bepaalde onderwerpen.

Zie voor meer informatie over de services die in deze zelfstudie wordt vermeld: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure-logboekanalyse-integratie](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Eerste installatie

Voordat u de stappen in dit artikel voltooien kunt, moet u het volgende:

1. Een Azure-abonnement en de account op dat abonnement met beheerdersrechten. Als u geen een abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/).
 
2. Een systeem met toegang tot het internet die voldoet aan de vereisten voor het installeren van de integratie van Azure-logboek. Het systeem kan op een cloudservice of lokaal wordt gehost.

3. [Integratie van Azure Log](https://www.microsoft.com/download/details.aspx?id=53324) geïnstalleerd. Om deze te installeren:

   a. Extern bureaublad gebruiken voor verbinding met het systeem dat wordt vermeld in stap 2.   
   b. Kopieer het installatieprogramma van de integratie van Azure Log naar het systeem. U kunt [downloaden van de installatiebestanden](https://www.microsoft.com/download/details.aspx?id=53324).   
   c. Het installatieprogramma start en accepteer de licentievoorwaarden voor Microsoft-Software.   
   d. Als u telemetrie informatie opgeeft wordt, laat u het selectievakje ingeschakeld. Schakel het selectievakje in als u zou in plaats daarvan niet gebruiksgegevens naar Microsoft verzonden.
   
   Zie voor meer informatie over Azure Log-integratie en hoe u deze installeert [Azure Log integratie met Azure Diagnostics logboekregistratie en Windows Event Forwarding](security-azure-log-integration-get-started.md).

4. De nieuwste versie van PowerShell.
 
   Als u Windows Server 2016 geïnstalleerd hebt, hebt u ten minste PowerShell 5.0. Als u een andere versie van Windows Server, hebt u mogelijk een oudere versie van PowerShell is geïnstalleerd. U kunt controleren welke versie door in te voeren ```get-host``` in een PowerShell-venster. Als er geen PowerShell 5.0 is geïnstalleerd, kunt u [downloaden](https://www.microsoft.com/download/details.aspx?id=50395).

   Nadat u ten minste hebt PowerShell 5.0, kunt u doorgaan met de meest recente versie te installeren:
   
   a. Voer in een PowerShell-venster de ```Install-Module Azure``` opdracht. Voltooi de installatiestappen.    
   b. Voer de ```Install-Module AzureRM``` opdracht. Voltooi de installatiestappen.

   Zie voor meer informatie [Azure PowerShell installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).


## <a name="create-supporting-infrastructure-elements"></a>Ondersteunende Infrastructuurelementen maken

1. Open een PowerShell-venster met verhoogde bevoegdheid en Ga naar **C:\Program Files\Microsoft Azure Log integratie**.
2. Importeer de cmdlets AzLog LoadAzLogModule.ps1 van het script uit te voeren. Voer de `.\LoadAzLogModule.ps1` opdracht. (U ziet de '. \ ' in die opdracht.) Deze lijst ziet er ongeveer zo uit:</br>

   ![Lijst met modules geladen](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

3. Voer de `Login-AzureRmAccount` opdracht. Geef de referentie-informatie voor het abonnement dat u voor deze zelfstudie gebruiken wilt in het aanmeldingsvenster.

   >[!NOTE]
   >Als dit de eerste keer dat u bent aangemeld bij Azure vanaf deze computer is, ziet u een bericht over het toestaan van Microsoft voor het verzamelen van gebruiksgegevens van PowerShell. U wordt aangeraden deze gegevensverzameling in te schakelen omdat deze wordt gebruikt voor het verbeteren van Azure PowerShell.

4. U bent aangemeld na een geslaagde authenticatie en u de informatie in de volgende schermafbeelding ziet. Noteer de abonnements-ID en de naam van het abonnement, omdat moet u ze later stappen uit te voeren.

   ![PowerShell-venster](./media/security-azure-log-integration-keyvault-eventhub/login-azurermaccount.png)
5. Variabelen voor het opslaan van de waarden die wordt later gebruikt maken. Voer de volgende PowerShell-regels. Mogelijk moet u de waarden voor uw omgeving aanpassen.
    - ```$subscriptionName = ‘Visual Studio Ultimate with MSDN’```(De abonnementsnaam van uw kan anders zijn. U ziet het als onderdeel van de uitvoer van de vorige opdracht.)
    - ```$location = 'West US'```(Deze variabele wordt gebruikt om door te geven van de locatie waar de bronnen moeten worden gemaakt. U kunt deze variabele om te worden van een willekeurige locatie van uw keuze worden wijzigen.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random```(De naam kan van alles zijn, maar deze alleen kleine letters en cijfers bevatten.)
    - ``` $storageName = $name```(U kunt deze variabele wordt gebruikt voor de opslagaccountnaam.)
    - ```$rgname = $name ```(U kunt deze variabele wordt gebruikt voor naam van de resourcegroep.)
    - ``` $eventHubNameSpaceName = $name```(Dit is de naam van de event hub-naamruimte.)
6. Geef het abonnement dat u met werkt:
    
    ```Select-AzureRmSubscription -SubscriptionName $subscriptionName```
7. Een resourcegroep maken:
    
    ```$rg = New-AzureRmResourceGroup -Name $rgname -Location $location```
    
   Als u `$rg` op dit moment ziet u uitvoer die vergelijkbaar is met deze schermafbeelding:

   ![Uitvoer na het maken van een resourcegroep](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
8. Maak een opslagaccount dat wordt gebruikt om informatie over de status bij te houden:
    
    ```$storage = New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
9. Maak de event hub-naamruimte. Dit is vereist voor het maken van een event hub.
    
    ```$eventHubNameSpace = New-AzureRmEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
10. De regel-ID die wordt gebruikt met de insights-provider niet ophalen:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
11. Ophalen van alle mogelijke Azure locaties en de namen toevoegen aan een variabele die kan worden gebruikt in een latere stap:
    
    a. ```$locationObjects = Get-AzureRMLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Als u `$locations` op dit moment ziet u de locatienamen van de zonder de aanvullende informatie die wordt geretourneerd door Get-AzureRmLocation.
12. Een Azure Resource Manager-logboek-profiel maken: 
    
    ```Add-AzureRmLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Zie voor meer informatie over het profiel van de Azure-logboekanalyse [overzicht van de Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

> [!NOTE]
> U mogelijk een foutbericht weergegeven wanneer u probeert een logboek-profiel maken. Vervolgens kunt u de documentatie voor Get-AzureRmLogProfile en verwijder AzureRmLogProfile bekijken. Als u een Get-AzureRmLogProfile uitvoert, ziet u informatie over het profiel van het logboek. U kunt het bestaande profiel voor het logboek verwijderen door te voeren de ```Remove-AzureRmLogProfile -name 'Log Profile Name' ``` opdracht.
>
>![Fout bij het Resource Manager-profiel](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

1. De sleutelkluis maken:

   ```$kv = New-AzureRmKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

2. Logboekregistratie voor de sleutelkluis configureren:

   ```Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Genereren van een activiteit

Aanvragen moeten worden verzonden naar de Sleutelkluis voor het genereren van een activiteit. Acties zoals genereren van sleutels, geheimen, opslaan of logboekvermeldingen lezen van geheimen van Sleutelkluis maakt.

1. De huidige opslagsleutels weergegeven:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
2. Genereren van een nieuwe **key2**:
    
   ```New-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
3. De sleutels opnieuw weergegeven en u ziet dat **key2** bevat een andere waarde:
    
   ```Get-AzureRmStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
4. Stel en een geheim voor het genereren van aanvullende logboekvermeldingen lezen:
    
   a. ```Set-AzureKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzureKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Geheime geretourneerd](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Azure-logboekanalyse-integratie configureren

Nu dat u hebt de vereiste elementen voor Sleutelkluis naar een event hub-logboekregistratie hebt geconfigureerd, moet u Azure Log-integratie configureren:

1. ```$storage = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $storagename```
2. ```$eventHubKey = Get-AzureRmEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
3. ```$storagekeys = Get-AzureRmStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
4. ``` $storagekey = $storagekeys[0].Value```

Voer de opdracht AzLog voor elke event hub:

1. ```$eventhubs = Get-AzureRmEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
2. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Na een minuut of dat van de laatste twee opdrachten uit te voeren, ziet u JSON-bestanden die worden gegenereerd. U hebt gecontroleerd of door de bewaking van de map **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Volgende stappen

- [Veelgestelde vragen over Azure-logboekanalyse-integratie](security-azure-log-integration-faq.md)
- [Aan de slag met Azure Log-integratie](security-azure-log-integration-get-started.md)
- [Logboeken van de Azure-resources integreren in uw SIEM-systemen](security-azure-log-integration-overview.md)
