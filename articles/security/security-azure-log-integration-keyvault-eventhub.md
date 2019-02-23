---
title: Logboeken van Azure Key Vault integreren met behulp van Event Hubs | Microsoft Docs
description: Zelfstudie die de benodigde stappen bevat voor Sleutelkluis-logboeken beschikbaar te maken voor een SIEM met behulp van Azure-Logboekintegratie
services: security
author: barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.topic: article
ms.date: 01/14/2019
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 22d4a18ad1c6e80baa6e798be399ab2cd4836fbc
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733400"
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Zelfstudie voor Azure-Logboekintegratie: Azure Key Vault-gebeurtenissen verwerken met behulp van Event Hubs

>[!IMPORTANT]
> De functie van de integratie met Azure Log worden afgeschaft door 06/01/2019. AzLog downloads zijn uitgeschakeld op 27 juni 2018. Voor informatie over wat te doen verplaatsen forward revisie van het bericht [gebruikt Azure monitor om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

U kunt Azure-Logboekintegratie vastgelegde gebeurtenissen ophalen en deze beschikbaar te maken voor uw systeem security information en event management (SIEM). In deze zelfstudie toont een voorbeeld van hoe Azure-Logboekintegratie kan worden gebruikt voor het verwerken van logboeken die worden verkregen via de Azure Event Hubs.

De aanbevolen methode voor het integreren van Logboeken in Azure wordt met behulp van de leverancier van uw SIEM-connector voor Azure Monitor en volgende [instructies](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Echter, als de leverancier van uw SIEM niet een connector voor Azure Monitor biedt, u mogelijk gebruik van Azure-Logboekintegratie als tijdelijke oplossing (als uw SIEM wordt ondersteund door Azure-Logboekintegratie) totdat deze een connector beschikbaar is.

 
Gebruik deze zelfstudie om vertrouwd te raken met hoe Azure-Logboekintegratie en Event Hubs samen werken met de voorbeeldenstappen te volgen en begrijpen hoe de oplossing biedt ondersteuning voor elke stap. U kunt vervolgens nemen wat u hebt geleerd hier om te maken van uw eigen stappen ter ondersteuning van de unieke vereisten van uw bedrijf.

>[!WARNING]
De stappen en de opdrachten in deze zelfstudie zijn niet bedoeld om te worden gekopieerd en geplakt. Ze dienen uitsluitend als voorbeeld. Gebruik de PowerShell-opdrachten "as is" niet in uw productieomgeving. U moet deze op basis van uw unieke omgeving aanpassen.


In deze zelfstudie begeleidt u bij het proces van Azure Key Vault-activiteit vastgelegd in een event hub maken en het beschikbaar maken als JSON-bestanden naar uw SIEM-systeem. Vervolgens kunt u uw SIEM-systeem voor het verwerken van de JSON-bestanden configureren.

>[!NOTE]
>De meeste van de stappen in deze zelfstudie hebben betrekking op het configureren van sleutelkluizen, opslagaccounts en eventhubs. De specifieke Azure-Logboekintegratie-stappen zijn aan het einde van deze zelfstudie. Voer deze stappen niet uit in een productieomgeving. Ze zijn bedoeld voor alleen een testomgeving. Voordat u ze in productie, moet u de stappen aanpassen.

Informatie over de manier krijgt u inzicht in de redenen achter elke stap. Koppelingen naar andere artikelen bieden u meer details over bepaalde onderwerpen.

Zie voor meer informatie over de services die in deze zelfstudie wordt vermeld: 

- [Azure Key Vault](../key-vault/key-vault-whatis.md)
- [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Azure-Logboekintegratie](security-azure-log-integration-overview.md)


## <a name="initial-setup"></a>Eerste installatie

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voordat u de stappen in dit artikel voltooien kunt, moet u het volgende:

* Een Azure-abonnement en een account op dat aan het abonnement met beheerdersrechten. Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/).
 
* Een systeem met toegang tot het internet die voldoet aan de vereisten voor het installeren van Azure-Logboekintegratie. Het systeem kan zich op een service in de cloud of on-premises gehost.

* Azure-Logboekintegratie geïnstalleerd. Om deze te installeren:

   a. Extern bureaublad gebruiken voor het verbinding maken met het systeem vermeld in stap 2.   
   b. Kopieer het installatieprogramma van de Azure-Logboekintegratie naar het systeem. c. Start het installatieprogramma en accepteer de licentievoorwaarden voor Microsoft-Software.

* Als u telemetrie-informatie wordt verstrekt, laat u het selectievakje ingeschakeld. Als u informatie over het gebruik in plaats daarvan niet naar Microsoft verzenden zou, schakelt u het selectievakje in.

   Zie voor meer informatie over Azure-Logboekintegratie en hoe u deze installeert [Azure-Logboekintegratie met Azure Diagnostics-logboeken en Windows Event Forwarding](security-azure-log-integration-get-started.md).

* De meest recente versie van PowerShell.

   Als u Windows Server 2016 is geïnstalleerd hebt, hebt u ten minste PowerShell 5.0. Als u een andere versie van Windows Server, hebt u mogelijk een eerdere versie van PowerShell is geïnstalleerd. U kunt controleren welke versie door in te voeren ```get-host``` in een PowerShell-venster. Als u geen PowerShell 5.0 is geïnstalleerd, kunt u [downloaden](https://www.microsoft.com/download/details.aspx?id=50395).

   Nadat u ten minste hebt PowerShell 5.0, kunt u doorgaan met het installeren van de meest recente versie door de instructies in [Azure PowerShell installeren](/powershell/azure/install-az-ps).


## <a name="create-supporting-infrastructure-elements"></a>Ondersteunende Infrastructuurelementen maken

1. Open een PowerShell-venster met verhoogde bevoegdheid en Ga naar **C:\Program Files\Microsoft Azure-Logboekintegratie**.
1. Importeer de cmdlets AzLog LoadAzLogModule.ps1 van het script uit te voeren. Voer de `.\LoadAzLogModule.ps1` opdracht. (U ziet dat de '. \" in die opdracht.) Deze lijst ziet er ongeveer zo uit:</br>

   ![Lijst met geladen modules](./media/security-azure-log-integration-keyvault-eventhub/loaded-modules.png)

1. Voer de `Connect-AzAccount` opdracht. Voer in het aanmeldingsvenster de referentie-informatie voor het abonnement die u voor deze zelfstudie gebruiken wilt.

   >[!NOTE]
   >Als dit de eerste keer is dat u bent aangemeld bij Azure vanaf deze machine, ziet u een bericht over het toestaan van Microsoft voor het verzamelen van gebruiksgegevens van PowerShell. U wordt aangeraden dat u deze gegevensverzameling niet inschakelen omdat dit wordt gebruikt voor het verbeteren van Azure PowerShell.

1. Na een geslaagde authenticatie, bent u aangemeld. Noteer de abonnements-ID en naam van abonnement, omdat u moet ze om latere stappen te voltooien.

1. Maak variabelen voor het opslaan van waarden die later worden gebruikt. Voer de volgende PowerShell-regels. Mogelijk moet u de waarden zodat deze overeenkomt met uw omgeving aanpassen.
    - ```$subscriptionName = 'Visual Studio Ultimate with MSDN'``` (Naam van uw abonnement kan afwijken. U kunt zien dit als onderdeel van de uitvoer van de vorige opdracht.)
    - ```$location = 'West US'``` (Deze variabele wordt gebruikt om door te geven van de locatie waar de resources moeten worden gemaakt. U kunt deze variabele om te worden van een willekeurige locatie van uw keuze worden wijzigen.)
    - ```$random = Get-Random```
    - ``` $name = 'azlogtest' + $random``` (De naam kan van alles zijn, maar hierbij moet alleen kleine letters en cijfers.)
    - ``` $storageName = $name``` (U kunt deze variabele wordt gebruikt voor naam van het opslagaccount.)
    - ```$rgname = $name ``` (U kunt deze variabele wordt gebruikt voor naam van de resourcegroep.)
    - ``` $eventHubNameSpaceName = $name``` (Dit is de naam van de event hub-naamruimte.)
1. Geef het abonnement dat u met werkt:
    
    ```Select-AzSubscription -SubscriptionName $subscriptionName```
1. Een resourcegroep maken:
    
    ```$rg = New-AzResourceGroup -Name $rgname -Location $location```
    
   Als u `$rg` op dit moment ziet u uitvoer die vergelijkbaar is met deze schermopname:

   ![Uitvoer na het maken van een resourcegroep](./media/security-azure-log-integration-keyvault-eventhub/create-rg.png)
1. Maak een opslagaccount dat wordt gebruikt om informatie over de status bij te houden:
    
    ```$storage = New-AzStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
1. De event hub-naamruimte maken. Dit is vereist voor het maken van een event hub.
    
    ```$eventHubNameSpace = New-AzEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
1. De regel-ID die wordt gebruikt met de insights-provider niet ophalen:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey' ```
1. Ophalen van alle mogelijke Azure-locaties en de namen toevoegen aan een variabele die kan worden gebruikt in een latere stap:
    
    a. ```$locationObjects = Get-AzLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Als u `$locations` op dit moment ziet u de locatienamen van de zonder de aanvullende informatie die wordt geretourneerd door Get-AzLocation.
1. Een Azure Resource Manager logboekprofiel maken: 
    
    ```Add-AzLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Zie voor meer informatie over het profiel van de Azure log [overzicht van de Azure-activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md).

> [!NOTE]
> Wanneer u probeert te maken van een logboekprofiel, kunt u een foutmelding krijgen. Vervolgens kunt u de documentatie voor Get-AzLogProfile en Remove-AzLogProfile bekijken. Als u Get-AzLogProfile uitvoert, ziet u informatie over het logboekprofiel. U kunt de bestaande logboekprofiel verwijderen door te voeren de ```Remove-AzLogProfile -name 'Log Profile Name' ``` opdracht.
>
>![Fout bij het Resource Manager-profiel](./media/security-azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

1. De key vault maken:

   ```$kv = New-AzKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location ```

1. Logboekregistratie voor de key vault configureren:

   ```Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true ```

## <a name="generate-log-activity"></a>Genereren van een activiteit

Aanvragen moeten worden verzonden naar de Key Vault voor het genereren van een activiteit. Acties zoals genereren van sleutels, geheimen, opslaan of vermeldingen in het lezen van geheimen uit Key Vault maakt.

1. De huidige opslagsleutels weergegeven:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Genereer een nieuwe **key2**:
    
   ```New-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
1. De sleutels opnieuw weergegeven en u ziet dat **key2** bevat een andere waarde:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Instellen en lezen van een geheim voor het genereren van extra vermeldingen:
    
   a. ```Set-AzKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Geheime geretourneerd](./media/security-azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Configureren van Azure-Logboekintegratie

Nu dat u hebt de vereiste elementen voor Key Vault logboekregistratie naar een event hub hebt geconfigureerd, moet u Azure-Logboekintegratie configureren:

1. ```$storage = Get-AzStorageAccount -ResourceGroupName $rgname -Name $storagename```
1. ```$eventHubKey = Get-AzEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
1. ```$storagekeys = Get-AzStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
1. ``` $storagekey = $storagekeys[0].Value```

Voer de opdracht AzLog voor elke event hub:

1. ```$eventhubs = Get-AzEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
1. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Na een minuut van de laatste twee opdrachten uitvoert, ziet u de JSON-bestanden die worden gegenereerd. U bevestigen dat door de bewaking van de map **C:\users\AzLog\EventHubJson**.

## <a name="next-steps"></a>Volgende stappen

- [Azure-Logboekintegratie Veelgestelde vragen](security-azure-log-integration-faq.md)
- [Aan de slag met Azure-Logboekintegratie](security-azure-log-integration-get-started.md)
- [Logboeken van Azure-resources in uw SIEM-systemen integreren](security-azure-log-integration-overview.md)
