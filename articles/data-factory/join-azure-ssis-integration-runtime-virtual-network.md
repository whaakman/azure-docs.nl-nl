---
title: Azure-SSIS-integratie runtime toevoegen aan een VNET | Microsoft Docs
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
ms.date: 09/27/2017
ms.author: spelluru
ms.openlocfilehash: aa570379890023c83383d291aa5d57fb79b2d5aa
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Een Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk
Als een van de volgende voorwaarden voldaan wordt, moet u de runtime van Azure SSIS-integratie (IR) toevoegen aan een Azure-netwerk (VNet): 

- U host de SSIS-catalogusdatabase in een SQL Server Managed Instance (private preview) die deel uitmaakt van een VNet.
- U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime.

 In Azure Data Factory versie 2 (preview) kunt u uw Azure-SSIS Integration Runtime toevoegen aan een klassiek VNet. Azure Resource Manager VNet is momenteel niet ondersteund. Echter, kunt u werken het rond zoals weergegeven in de volgende sectie. 

 > [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

## <a name="access-on-premises-data-stores"></a>Toegang tot on-premises gegevensopslag
Als SSIS-pakketten toegang krijgen de gegevensarchieven alleen openbare cloud tot, moet u geen Azure-SSIS-IR toevoegen aan een VNet. Als SSIS-pakketten toegang on-premises gegevensopslagexemplaren tot, moet u Azure SSIS-IR toevoegen aan een VNet dat is verbonden met de on-premises netwerk. Als de SSIS-catalogus wordt gehost in Azure SQL Database die zich niet in het VNet, moet u de juiste poorten open. Als de SSIS-catalogus wordt gehost in Azure SQL beheerd-exemplaar dat in een klassiek VNet, kunt u Azure SSIS-IR kan toevoegen aan hetzelfde klassieke VNet (of) een andere klassieke VNet met een klasse voor klassieke VNet-verbinding met de Azure SQL beheerd-exemplaar met. De volgende secties vindt u meer informatie.

Hier volgen enkele belangrijke punten met: 

- Als er geen bestaande VNet verbonden met uw on-premises netwerk is, maakt u eerst een [klassieke VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie runtime om toe te voegen. Configureer vervolgens een site-naar-site [VPN-gatewayverbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) verbinding van dit VNet naar uw on-premises netwerk.
- Als er een bestaande klassieke VNet dat is verbonden met uw on-premises netwerk in dezelfde locatie als uw Azure-SSIS-integratie runtime, kunt u uw Azure-SSIS-integratie runtime aan koppelen.
- Als er een bestaande klassieke VNet met uw on-premises netwerk in een andere locatie van de Runtime van uw Azure-SSIS-integratie verbonden is, kunt u eerst maken een [klassieke VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie Runtime om toe te voegen. Configureer vervolgens een [klassiek naar klassieke VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) verbinding.
- Als er een bestaande Azure Resource Manager VNet verbonden met uw on-premises netwerk is, maakt u eerst een [klassieke VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie runtime om toe te voegen. Configureer vervolgens een [klassieke Azure Resource Manager VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding.

## <a name="domain-name-services-server"></a>Domain Name Services-server 
Als u uw eigen server Services DNS (Domain Name) te gebruiken in een VNet die worden toegevoegd door de runtime van uw Azure-SSIS-integratie wilt, voert u de richtlijnen voor [ervoor zorgen dat de knooppunten van de runtime van uw Azure-SSIS-integratie in VNet een Azure-eindpunten kunnen omzetten](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Netwerkbeveiligingsgroep
Als u nodig hebt voor het implementeren van Netwerkbeveiligingsgroep (NSG) in een VNet die worden toegevoegd door de Runtime van uw Azure-SSIS-integratie, kunt u binnenkomend en uitgaand traffics via de volgende poorten:

| Poorten | Richting | Protocol-Transport | Doel | Inkomende bron/uitgaande bestemming |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Inkomend | TCP | Deze poorten Azure-services gebruiken om te communiceren met de knooppunten van de runtime van uw Azure-SSIS-integratie in VNet. | Internet | 
| 443 | Uitgaand | TCP | De knooppunten van de runtime van uw Azure-SSIS-integratie in VNet deze poort gebruiken voor toegang tot Azure-services, bijvoorbeeld de Azure Storage, de Event Hub, enzovoort. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Uitgaand | TCP | De knooppunten van de runtime van uw Azure-SSIS-integratie in VNet via deze poorten SSISDB gehost door uw Azure SQL Database-server (niet van toepassing op SSISDB gehost door beheerde exemplaar van Azure SQL). | Internet | 

## <a name="configure-vnet"></a>VNet configureren
U moet eerst configureren VNet met een van de volgende manieren (vs script. Azure-portal) voordat u kunt deelnemen aan een Azure-SSIS-IR naar het VNet. 

### <a name="script-to-configure-vnet"></a>Script voor het configureren van VNet 
Voeg het volgende script om automatisch te configureren VNet machtigingsinstellingen voor uw Azure-SSIS-integratie runtime lid worden van het VNet.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

### <a name="use-portal-to-configure-vnet"></a>Portal gebruiken voor het VNet configureren
Uitvoeren van het script is de eenvoudigste manier om het VNet configureren. Als u geen hebt toegang tot het configureren van die VNet of de automatische configuratie is mislukt, de eigenaar van dit VNet / u kunt proberen handmatig configureren in de volgende stappen:

### <a name="find-the-resource-id-for-your-azure-vnet"></a>De resource-ID vinden voor uw Azure VNet.
 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **meer services**. Filteren op en selecteer **virtuele netwerken (klassiek)**.
3. Filteren op en selecteer uw **virtueel netwerk** in de lijst. 
4. Selecteer in de pagina virtuele netwerk (klassiek) **eigenschappen**. 

    ![klassieke VNet resource-ID](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Klik op de knop kopiëren voor de **RESOURCE-ID** de resource-ID voor het klassieke netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of een bestand.
6. Klik op **subnetten** in het menu links en zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS-integratie-runtime.

    ![Aantal beschikbare adressen in VNet](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Deelnemen aan **MicrosoftAzureBatch** naar **klassieke Virtual Machine Contributor** rol voor het VNet. 
    1. Toegangsbeheer (IAM) in het menu links op en klik op **toevoegen** op de werkbalk.
    
        ![Toegangsbeheer-toevoegen >](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png) 
    2. In **machtigingen toevoegen** pagina **klassieke Virtual Machine Contributor** voor **rol**. Kopiëren en plakken **ddbf3205-c6bd-46ae-8127-60eb93363864** in de **Selecteer** tekstvak in en selecteer vervolgens **Microsoft Azure Batch** uit de lijst met zoekresultaten. 
    
        ![Machtigingen toevoegen - zoeken](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Klik op Opslaan om de instellingen en om de pagina te sluiten.
    
        ![Opslaan van instellingen voor toegang](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Controleer of u **MicrosoftAzureBatch** in de lijst met inzenders.
    
        ![AzureBatch toegang bevestigen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Controleer of dat die Azure Batch-provider is geregistreerd in het Azure-abonnement met het VNet of de Azure Batch-provider geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch.
    1. Klik in de Azure-portal op **abonnementen** in het menu links. 
    2. Selecteer uw **abonnement**. 
    3. Klik op **resourceproviders** aan de linkerkant en Bevestig dat `Microsoft.Batch` is een geregistreerde provider. 
    
        ![bevestiging batch geregistreerd](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Als er geen `Microsoft.Batch` is in de lijst om te registreren, [leeg Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

## <a name="create-an-azure-ssis-ir-and-join-it-to-a-vnet"></a>Een Azure-SSIS-IR maken en toevoegen aan een VNet
U kunt een Azure-SSIS-IR maken en toevoegen aan het VNet op hetzelfde moment. Zie voor de volledige script en instructies voor het maken van een Azure-SSIS-IR en toevoegen aan een VNet op hetzelfde moment [maken Azure SSIS-IR](create-azure-ssis-integration-runtime.md).

## <a name="join-an-existing-azure-ssis-ir-to-a-vnet"></a>Een bestaande Azure SSIS-IR toevoegen aan een VNet
Het script in de [maken Azure SSIS-integratie runtime](create-azure-ssis-integration-runtime.md) artikel laat zien hoe een Azure-SSIS-IR maken en toevoegen aan een VNet in hetzelfde script. Als u een bestaande Azure-SSIS, voert de volgende stappen uit als u wilt toevoegen aan het VNet. 

1. De Azure-SSIS-IR stoppen
2. De Azure-SSIS-IR om lid van het VNet te configureren. 
3. De Azure-SSIS-IR starten 

## <a name="define-the-variables"></a>De variabelen definiëren

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: 
# $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"
$VnetId = "<Name of your Azure classic virtual netowrk>"
$SubnetName = "<Name of the subnet in VNet>"
```

### <a name="stop-the-azure-ssis-ir"></a>De Azure-SSIS-IR stoppen
De runtime Azure SSIS-integratie te stoppen voordat u deze met een VNet kan worden toegevoegd. Met deze opdracht alle knooppunten vrijgegeven en wordt gestopt facturering.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-vnet-settings-for-the-azure-ssis-ir-to-join"></a>VNet-instellingen voor de Azure-SSIS-IR om lid te configureren
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
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

### <a name="configure-the-azure-ssis-ir"></a>De Azure-SSIS-IR configureren
Voer de opdracht Set-AzureRmDataFactoryV2IntegrationRuntime de runtime Azure SSIS-integratie om lid van het VNet te configureren: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

## <a name="start-the-azure-ssis-ir"></a>De Azure-SSIS-IR starten
Voer de volgende opdracht uit om de Azure SSIS Integration Runtime te starten: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Het duurt **20-30 minuten** voordat deze opdracht is voltooid.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Azure-SSIS-runtime, in de volgende onderwerpen: 

- [Azure-SSIS-integratie Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-deploy-ssis-packages-azure.md). Dit artikel biedt stapsgewijze instructies voor het maken van een Azure-SSIS IR en maakt gebruik van een Azure SQL database voor het hosten van de SSIS-catalogus. 
- [Procedure: Een Azure SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Managed Instance (Private Preview) en het toevoegen van de IR aan een VNet. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Er wordt ook uitgelegd hoe u een Azure-SSIS IR kunt uitschalen door meer knooppunten toe te voegen aan de IR. 
