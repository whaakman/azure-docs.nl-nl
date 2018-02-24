---
title: Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk | Microsoft Docs
description: Informatie over hoe Azure-SSIS-integratie runtime koppelen aan een virtuele Azure-netwerk.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
ms.openlocfilehash: 6f0038ea750d05b8d59bde07b54cb4aa22ab71b9
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Een Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk
Aanmelden bij uw Azure-SSIS-integratie runtime (IR) naar een Azure-netwerk in de volgende scenario's: 

- Als u beheert de catalogusdatabase van de SQL Server Integration Services (SSIS)-op Azure SQL Database beheerd-instantie (afgeschermd voorbeeld) in een virtueel netwerk.
- U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.

 Azure Data Factory versie 2 (Preview) kunt u uw Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk gemaakt met behulp van het klassieke implementatiemodel of het Azure Resource Manager-implementatiemodel. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u gebruikmaakt van versie 1 van de Data Factory-service is in het algemeen beschikbaarheid (GA), raadpleegt u de [Data Factory-documentatie voor versie 1](v1/data-factory-introduction.md).

## <a name="access-to-on-premises-data-stores"></a>Toegang tot on-premises gegevensopslagexemplaren
Als SSIS-pakketten toegang krijgen de gegevensarchieven alleen openbare cloud tot, moet u niet de Azure-SSIS-IR toevoegen aan een virtueel netwerk. Als SSIS-pakketten toegang on-premises gegevensopslagexemplaren tot, moet u de Azure-SSIS-IR toevoegen aan een virtueel netwerk dat is verbonden met de on-premises netwerk. 

Als de SSIS-catalogus wordt gehost in een Azure SQL Database-exemplaar dat niet in het virtuele netwerk, moet u de juiste poorten open. 

Als de SSIS-catalogus wordt gehost in beheerde exemplaar van SQL Database in een virtueel netwerk, kunt u deelnemen aan een Azure-SSIS-IR naar:

- Hetzelfde virtuele netwerk.
- Een ander virtueel netwerk met een netwerk-naar-netwerkverbinding met beheerde exemplaar van SQL Database met. 

Het virtuele netwerk kan worden geïmplementeerd via het klassieke implementatiemodel of het Azure Resource Manager-implementatiemodel. Als u van plan bent om te worden toegevoegd aan de Azure-SSIS-IR in de *hetzelfde virtuele netwerk* met beheerde exemplaar van SQL Database, zorg ervoor dat de Azure-SSIS-IR een *ander subnet* uit met SQL-Database Beheerde exemplaar.   

De volgende secties vindt u meer informatie.

Hier volgen enkele belangrijke punten met: 

- Als er zijn geen virtuele netwerk is verbonden met uw on-premises netwerk, maakt u eerst een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) of een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie de runtime om toe te voegen. Configureer vervolgens een site-naar-site [VPN-gatewayverbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) of [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) verbinding van dit virtuele netwerk naar uw on-premises netwerk.
- Als er een klassiek virtueel netwerk is verbonden met uw on-premises netwerk in dezelfde locatie als uw Azure-SSIS-IR of een bestaande Azure Resource Manager, kunt u de IR kan toevoegen aan dit virtuele netwerk.
- Als er een bestaand klassiek virtueel netwerk verbonden met uw on-premises netwerk in een andere locatie van uw Azure-SSIS-IR, kunt u eerst maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-IR om toe te voegen. Configureer vervolgens een [klassiek naar Klassiek virtueel netwerk](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) verbinding. Kunt u een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) voor uw Azure-SSIS-integratie runtime om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding.
- Als er een bestaande Azure Resource Manager virtueel netwerk met uw on-premises netwerk in een andere locatie van uw Azure-SSIS-IR verbonden, kunt u eerst maken een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) voor uw Azure-SSIS IR om toe te voegen. Configureer vervolgens de verbinding van een Azure Resource Manager naar Azure Resource Manager virtueel netwerk. U kunt ook maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-IR om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding.

## <a name="domain-name-services-server"></a>Domain Name Services-server 
Als u uw eigen server Services DNS (Domain Name) te gebruiken in een virtueel netwerk die worden toegevoegd door de runtime van uw Azure-SSIS-integratie wilt, voert u de richtlijnen voor [ervoor zorgen dat de knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk Azure kunnen omzetten eindpunten](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Netwerkbeveiligingsgroep
Als u nodig hebt voor het implementeren van een netwerkbeveiligingsgroep (NSG) in een virtueel netwerk die worden toegevoegd door de runtime van uw Azure-SSIS-integratie, kunt u binnenkomend en uitgaand verkeer via de volgende poorten:

| Poorten | Richting | Protocol-transport | Doel | Inkomende bron/uitgaande bestemming |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100, 20100, 30100 (als u de IR aan een klassiek virtueel netwerk koppelt)<br/><br/>29876, 29877 (als u de IR aan een virtueel netwerk van Azure Resource Manager koppelt) | Inkomend | TCP | Deze poorten Azure-services gebruiken om te communiceren met de knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk. | Internet | 
| 443 | Uitgaand | TCP | De knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk deze poort gebruiken voor toegang tot Azure-services, zoals Azure Storage en Azure Event Hubs. | Internet | 
| 1433<br/>11000-11999<br/>14000-14999  | Uitgaand | TCP | De knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk deze poorten gebruiken voor toegang tot SSISDB gehost door uw Azure SQL Database-server (dit doel is niet van toepassing op SSISDB gehost door beheerde exemplaar van SQL-Database.) | Internet | 

## <a name="azure-portal-data-factory-ui"></a>Azure-portal (Data Factory-UI)
Deze sectie wordt beschreven hoe u een bestaande Azure SSIS-runtime toevoegen aan een virtueel netwerk (klassiek of Azure resourcemanager) met behulp van de Azure-portal en de Data Factory-gebruikersinterface. Eerst moet u het virtuele netwerk op de juiste wijze configureren voordat u uw Azure-SSIS-IR toevoegt aan het. Ga via een van de volgende twee secties op basis van het type van het virtuele netwerk (klassiek of Azure resourcemanager). Ga vervolgens verder met de derde sectie uw Azure-SSIS-IR koppelen aan het virtuele netwerk. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>De portal gebruiken voor het configureren van een klassiek virtueel netwerk
U moet een virtueel netwerk configureren voordat u kunt een Azure-SSIS-IR deelnemen aan.

1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze browsers.
2. Meld u aan bij [Azure Portal](https://portal.azure.com).
3. Selecteer **meer services**. Filteren op en selecteer **virtuele netwerken (klassiek)**.
4. Filteren op en selecteert u het virtuele netwerk in de lijst. 
5. Op de **virtuele netwerk (klassiek)** pagina **eigenschappen**. 

    ![De resource-ID klassiek virtueel netwerk](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Selecteer de knop kopiëren voor **RESOURCE-ID** de resource-ID voor het klassieke netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of een bestand.
6. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS-integratie-runtime.

    ![Aantal beschikbare adressen in het virtuele netwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Deelnemen aan **MicrosoftAzureBatch** naar de **klassieke Virtual Machine Contributor** rol voor het virtuele netwerk.

    a. Selecteer **toegangsbeheer (IAM)** op het menu aan de linkerkant en selecteer **toevoegen** op de werkbalk. 
       !['Toegangsbeheer' en 'Add' knoppen](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Op de **machtigingen toevoegen** pagina **klassieke Virtual Machine Contributor** voor **rol**. Plakken **ddbf3205-c6bd-46ae-8127-60eb93363864** in de **Selecteer** vak en selecteer vervolgens **Microsoft Azure Batch** uit de lijst met zoekresultaten.   
       ![Zoekresultaten op de pagina 'Machtigingen toevoegen'](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Selecteer **opslaan** de instellingen op te slaan en te sluiten van de pagina.  
       ![Opslaan van instellingen voor toegang](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Controleer of u **Microsoft Azure Batch** in de lijst met inzenders.  
       ![Azure Batch toegang bevestigen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

5. Controleer of de Azure Batch-provider is geregistreerd in de Azure-abonnement dat het virtuele netwerk heeft. Of de Azure Batch-provider geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch.

   a. Selecteer in Azure portal **abonnementen** in het menu links.

   b. Selecteer uw abonnement.

   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider.     
      ![Bevestiging van de status 'Geregistreerd'](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [leeg Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>De portal gebruiken voor het configureren van een virtueel netwerk van Azure Resource Manager
U moet een virtueel netwerk configureren voordat u kunt een Azure-SSIS-IR deelnemen aan.

1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze webbrowsers.
2. Meld u aan bij [Azure Portal](https://portal.azure.com).
3. Selecteer **meer services**. Filteren op en selecteer **virtuele netwerken**.
4. Filteren op en selecteert u het virtuele netwerk in de lijst. 
5. Op de **virtueel netwerk** pagina **eigenschappen**. 
6. Selecteer de knop kopiëren voor **RESOURCE-ID** de resource-ID voor het virtuele netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of een bestand.
7. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS-integratie-runtime.
8. Controleer of de Azure Batch-provider is geregistreerd in de Azure-abonnement dat het virtuele netwerk heeft. Of de Azure Batch-provider geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch.

   a. Selecteer in Azure portal **abonnementen** in het menu links.

   b. Selecteer uw abonnement. 
   
   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider.     
      ![Bevestiging van de status 'Geregistreerd'](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [leeg Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen.

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>De Azure-SSIS-IR toevoegen aan een virtueel netwerk


1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze webbrowsers.
2. In de [Azure-portal](https://portal.azure.com), selecteer **gegevensfactory** in het menu links. Als er geen **gegevensfactory** selecteren in het menu **meer services**, en selecteer **gegevensfactory** in de **INTELLIGENCE en analyse**sectie. 
    
   ![Lijst met gegevensfactory 's](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. Selecteer uw gegevensfactory met de Azure-SSIS-integratie runtime in de lijst. U Zie de startpagina van uw gegevensfactory. Selecteer de **auteur & implementeren** tegel. Op een afzonderlijke tabblad ziet u de Data Factory-gebruikersinterface. 

   ![Startpagina van de gegevensfactory](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. In de Data Factory-gebruikersinterface overschakelen naar de **bewerken** tabblad **verbindingen**, en schakel over naar de **integratie Runtimes** tabblad. 

   ![Tabblad 'Integratie runtimes'](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Als uw Azure-SSIS-IR wordt uitgevoerd in de lijst van de runtime integratie selecteert u de **stoppen** knop in de **acties** kolom voor uw Azure-SSIS-IR U kunt een IR niet bewerken, totdat u het stopt. 

   ![De IR stoppen](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. Selecteer in de lijst van de runtime integratie de **bewerken** knop in de **acties** kolom voor uw Azure-SSIS-IR

   ![De runtime integratie bewerken](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. Op de **algemene instellingen** pagina van de **integratie Runtime-instellingen** Selecteer **volgende**. 

   ![Algemene instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. Op de **SQL-instellingen** pagina, voer het wachtwoord in en selecteer **volgende**.

   ![SQL Server-instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. Op de **geavanceerde instellingen** pagina, de volgende acties uitvoeren: 

   a. Schakel het selectievakje voor **selecteert u een VNet-naar-VNet-machtigingen/instellingen configureren voor uw Azure-SSIS-integratie-Runtime voor het koppelen van Azure-services toestaan**.

   b. Voor **Type**, opgeven of het virtuele netwerk een klassiek virtueel netwerk of een virtueel netwerk van Azure Resource Manager. 

   c. Voor **VNet-naam**, selecteert u het virtuele netwerk.

   d. Voor **subnetnaam**, selecteert u uw subnet in het virtuele netwerk.

   e. Selecteer **Update**. 

   ![Geavanceerde instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. U kunt nu de IR starten met behulp van de **Start** knop in de **acties** kolom voor uw Azure-SSIS-IR Het duurt ongeveer 20 minuten een Azure-SSIS-IR starten 


## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Een virtueel netwerk configureren
U moet een virtueel netwerk configureren voordat u kunt een Azure-SSIS-IR deelnemen aan. Toevoegen voor het automatisch configureren van virtueel netwerk machtigingsinstellingen voor uw Azure-SSIS-integratie runtime lid worden van het virtuele netwerk, het volgende script:

```powershell
# Register to the Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk
U kunt een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk op hetzelfde moment. Zie voor de volledige script en instructies [maken van een Azure-SSIS-integratie runtime](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Een bestaande Azure SSIS-IR toevoegen aan een virtueel netwerk
Het script in de [maken van een Azure-SSIS-integratie runtime](create-azure-ssis-integration-runtime.md) artikel laat zien hoe een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk in hetzelfde script. Als u een bestaande Azure SSIS-IR hebt, voert u de volgende stappen uit als u wilt toevoegen aan het virtuele netwerk: 

1. De Azure-SSIS-IR stoppen
2. De Azure-SSIS-IR om lid van het virtuele netwerk te configureren. 
3. De Azure-SSIS-IR starten 

### <a name="define-the-variables"></a>De variabelen definiëren

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a virtual network and Azure SQL Database Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network.
$VnetId = "<Name of your Azure virtual network>"
$SubnetName = "<Name of the subnet in the virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>De Azure-SSIS-IR stoppen
De runtime Azure SSIS-integratie te stoppen voordat u deze met een virtueel netwerk kan worden toegevoegd. Met deze opdracht alle knooppunten worden vrijgegeven en stopt facturering:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Instellingen voor virtueel netwerk voor de Azure-SSIS-IR om lid te configureren
Registreren bij de Azure Batch-resourceprovider:

```powershell
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>De Azure-SSIS-IR configureren
Voor het configureren van de Azure-SSIS-integratie runtime als u wilt deelnemen aan het virtuele netwerk, voer de `Set-AzureRmDataFactoryV2IntegrationRuntime` opdracht: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>De Azure-SSIS-IR starten
Voer de volgende opdracht voor het starten van de runtime Azure SSIS-integratie: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Deze opdracht genereert 20-30 minuten duren.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure-SSIS-runtime, in de volgende onderwerpen: 

- [Azure-SSIS-integratie runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen, met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). In dit artikel vindt stapsgewijze instructies voor het maken van een Azure-SSIS-IR Een Azure SQL database wordt gebruikt voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel wordt op de zelfstudie wordt uitgebreid en bevat instructies over het gebruik van Azure SQL Database beheerd-instantie (afgeschermd voorbeeld) en de IR toevoegen aan een virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Deze ook wordt beschreven hoe u uw Azure-SSIS-IR uitbreiden door knooppunten toe te voegen. 
