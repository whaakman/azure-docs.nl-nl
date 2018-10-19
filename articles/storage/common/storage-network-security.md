---
title: Azure Storage-Firewalls en virtuele netwerken configureren | Microsoft Docs
description: Gelaagde netwerkbeveiliging voor uw storage-account configureren.
services: storage
author: cbrooksmsft
ms.service: storage
ms.topic: article
ms.date: 10/25/2017
ms.author: cbrooks
ms.component: common
ms.openlocfilehash: 98972b0c52470e6a404090d993c21a47b11cd660
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427142"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Azure Storage-Firewalls en virtuele netwerken configureren
Azure Storage biedt een gelaagd beveiligingsmodel waarmee u uw storage-accounts naar een specifieke set toegestane netwerken beveiligen.  Als network-regels zijn geconfigureerd, kunnen alleen de toepassingen van toegestane netwerken toegang krijgen tot een storage-account.  Bij het aanroepen van een toegestane netwerk, blijven toepassingen juiste autorisatie (een geldige toegangssleutel of SAS-token) voor toegang tot het opslagaccount nodig.

> [!IMPORTANT]
> Inschakelen van Firewall-regels voor uw opslagaccount, wordt de toegang tot binnenkomende aanvragen voor gegevens, met inbegrip van andere Azure-services geblokkeerd.  Dit omvat het gebruik van de Portal voor het schrijven van Logboeken, enzovoort.  Azure-services die worden uitgevoerd van binnen een VNet kunnen toegang worden verleend door toe te staan van het subnet van het service-exemplaar.  Azure-services die niet uit binnen een VNet werken wordt geblokkeerd door de firewall.  Een beperkt aantal scenario's kan worden ingeschakeld via de [uitzonderingen](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) mechanisme die hieronder worden beschreven.  Voor toegang tot de Portal moet u dat doen op een computer in de vertrouwde grens (IP- of VNet) die u hebt ingesteld.
>

## <a name="scenarios"></a>Scenario's
Storage-accounts kunnen worden geconfigureerd voor het weigeren van toegang tot verkeer via alle netwerken (met inbegrip van internetverkeer) standaard.  Toegang kan worden verleend voor verkeer van specifieke Azure virtuele netwerken, zodat u een beveiligde netwerkgrens voor uw toepassingen kunt maken.  Toegang kan ook worden verleend aan het openbare internet IP-adresbereiken, het inschakelen van verbindingen met specifieke on-premises of internet-clients.

Netwerkregels worden toegepast op alle netwerkprotocollen naar Azure-opslag, met inbegrip van REST en SMB.  Toegang tot uw gegevens van hulpprogramma's als de Azure Storage Explorer-portal en AZCopy vereisen expliciete netwerkregels wanneer netwerkregels van kracht zijn.

Netwerkregels kunnen worden toegepast op bestaande opslagaccounts of kunnen worden toegepast tijdens het maken van nieuwe Storage-accounts.

Zodra het netwerkregels worden toegepast, worden ze afgedwongen voor alle aanvragen.  SAS-tokens die toegang tot een specifiek IP-adres-service verlenen om te fungeren **limiet** de toegang van de houder token, maar nieuwe toegang niet verlenen buiten de geconfigureerde regels bieden. 

VM-schijfverkeer (inclusief koppelen en ontkoppelen van bewerkingen en schijf-i/o) is **niet** beïnvloed door netwerkregels.  REST-toegang tot pagina-blobs is beveiligd door netwerkregels.

Klassieke opslagaccounts **niet** ondersteuning voor Firewalls en virtuele netwerken.

Back-up en herstellen van virtuele Machines met niet-beheerde schijven in opslagaccounts met netwerkregels toegepast wordt ondersteund via een uitzondering maken zoals beschreven in de [uitzonderingen](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) sectie van dit artikel.  Firewall-uitzonderingen zijn niet van toepassing met Managed Disks, omdat ze al worden beheerd door Azure.

## <a name="change-the-default-network-access-rule"></a>Wijzigen van de standaardtoegangsregel voor netwerk
Standaard accepteren opslagaccounts verbindingen van clients op een netwerk.  Als u wilt beperken de toegang tot geselecteerde netwerken, moet u eerst de standaardactie wijzigen.

> [!WARNING]
> Wijzigingen aanbrengen in netwerkregels kan invloed hebben op uw toepassingen kunnen verbinding maken met Azure Storage.  Als u de standaardregel voor het netwerk op **weigeren** blokkeert de toegang tot de gegevens, tenzij specifieke netwerkregels *verlenen* toegang worden ook toegepast.  Zorg ervoor dat toegang verlenen tot een toegestane netwerken met behulp van netwerkregels voordat u de standaardregel voor het weigeren van toegang wijzigen.
>

#### <a name="azure-portal"></a>Azure Portal
1. Navigeer naar het opslagaccount dat u wilt beveiligen.  

2. Klik op het instellingenmenu met de naam **Firewalls en virtuele netwerken**.
3. Kies voor het weigeren van toegang standaard, zodat toegang vanaf 'Geselecteerde netwerken'.  Kies als u wilt dat verkeer via alle netwerken, zodat toegang vanaf 'Alle netwerken'.
4. Klik op *opslaan* de wijzigingen worden toegepast.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).

2. De status van de standaardregel voor de storage-account weergeven.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. De standaardregel voor het weigeren van toegang tot het netwerk standaard instellen.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. De standaardregel waarmee toegang tot het netwerk standaard instellen.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLIv2
1. [De Azure CLI installeren](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. De status van de standaardregel voor de storage-account weergeven.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
```

3. De standaardregel voor het weigeren van toegang tot het netwerk standaard instellen.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. De standaardregel waarmee toegang tot het netwerk standaard instellen.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Toegang verlenen vanuit een virtueel netwerk
Storage-accounts kunnen worden geconfigureerd zodat toegang alleen vanaf specifieke Azure Virtual Networks. 

Doordat een [Service-eindpunt](/azure/virtual-network/virtual-network-service-endpoints-overview) voor Azure Storage in het Virtueelnetwerk, verkeer een optimale route naar de Azure Storage-service is gewaarborgd. De identiteit van het virtuele netwerk en het subnet worden ook verzonden bij elke aanvraag.  Beheerders kunnen vervolgens netwerkregels voor het opslagaccount waarmee aanvragen worden ontvangen van specifieke subnetten in het Virtueelnetwerk configureren.  Clients krijgen toegang via deze netwerkregels moet blijven om te voldoen aan de autorisatievereisten van het Storage-account voor toegang tot de gegevens.

Elk opslagaccount kan ondersteunen maximaal 100 regels voor Virtueelnetwerk die kunnen worden gecombineerd met [IP-netwerkregels](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Beschikbare regio's voor Virtual Network
Service-eindpunten werken in het algemeen tussen virtuele netwerken en service-exemplaren in dezelfde Azure-regio.  Wanneer Service-eindpunten worden gebruikt met Azure Storage, dit bereik is uitgebreid met de [gekoppelde regio](/azure/best-practices-availability-paired-regions).  Hiermee bedrijfscontinuïteit tijdens een regionale failover, evenals de naadloze toegang tot exemplaren van de alleen-lezen geografisch redundante opslag (RA-GRS).  Netwerkregels die toegang vanuit een virtueel netwerk naar een opslagaccount verlenen ook verlenen toegang tot een RA-GRS-exemplaar.

Bij het plannen voor herstel na noodgevallen tijdens een regionale storing, moet u de virtuele netwerken in de gekoppelde regio vooraf inrichten. Service-eindpunten voor Azure Storage moet zijn ingeschakeld en netwerkregels toegang verlenen vanuit deze alternatieve virtuele netwerken moeten worden toegepast op uw geografisch redundante opslag-accounts.

> [!NOTE]
> Service-eindpunten niet van toepassing op het verkeer buiten de regio van het Virtueelnetwerk en het paar aangewezen regio.  Netwerkregels toegang verlenen vanuit virtuele netwerken voor Storage-accounts kunnen alleen worden toegepast voor virtuele netwerken in de primaire regio van een opslagaccount of in de aangewezen gekoppelde regio.
>

### <a name="required-permissions"></a>Vereiste machtigingen
Als u wilt een Virtueelnetwerk-regel van toepassing op een Storage-account, moet de gebruiker de machtiging voor hebben *Service koppelen aan een Subnet* voor de subnetten die worden toegevoegd.  Deze machtiging is opgenomen in de *Inzender voor Opslagaccounts* ingebouwde rol en kunnen worden toegevoegd aan de aangepaste roldefinities.

Storage-account en de virtuele netwerken toegang verleend **kan** zich in verschillende abonnementen, maar deze abonnementen moeten deel uitmaken van dezelfde Azure Active Directory-tenant.

### <a name="managing-virtual-network-rules"></a>Beheren van Virtual Network-regels
Virtuele-netwerkregels voor storage-accounts kunnen worden beheerd via Azure portal, PowerShell of CLIv2.

#### <a name="azure-portal"></a>Azure Portal
1. Navigeer naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het instellingenmenu met de naam **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt zodat toegang vanaf 'Geselecteerde netwerken'.
4. Als u wilt toegang verlenen tot een virtueel netwerk met een nieuwe regel voor het netwerk, onder "Virtuele netwerken", klikt u op 'Bestaande toevoegen' om te selecteren van een bestaand virtueel netwerk en subnetten, klikt u vervolgens op *toevoegen*.  Voor het maken van een nieuw virtueel netwerk en het toegang geven, klikt u op *nieuwe toevoegen*, geef de informatie die nodig zijn voor de nieuwe virtuele netwerk maken en klik vervolgens op *maken*.

> [!NOTE]
> Als een Service-eindpunt voor Azure Storage is niet eerder is geconfigureerd voor het geselecteerde virtuele netwerk en subnetten, kan deze worden geconfigureerd als onderdeel van deze bewerking.
>

5. Als u wilt een virtueel netwerk of subnet regel verwijderen, klikt u op '...' om te openen van het snelmenu voor het virtuele netwerk of het subnet en klikt u op 'Verwijderen'.
6. Klik op *opslaan* de wijzigingen worden toegepast.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. Lijst met Virtual Network-regels
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Service-eindpunt inschakelen voor Azure Storage op een bestaand Virtueelnetwerk en Subnet
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Een regel voor een Virtueelnetwerk en subnet toevoegen.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Een regel voor een Virtueelnetwerk en subnet verwijderen.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of netwerkregels heeft geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [De Azure CLI installeren](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. Lijst met Virtual Network-regels
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Service-eindpunt inschakelen voor Azure Storage op een bestaand Virtueelnetwerk en Subnet
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Een regel voor een Virtueelnetwerk en subnet toevoegen.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

4. Een regel voor een Virtueelnetwerk en subnet verwijderen. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of netwerkregels heeft geen effect.
>

## <a name="grant-access-from-an-internet-ip-range"></a>Toegang verlenen vanuit een IP-bereik
Storage-accounts kunnen worden geconfigureerd voor toegang via internet voor specifieke openbare IP-adresbereiken.  Deze configuratie kunt specifieke internet gebaseerde services en on-premises netwerken toegang worden verleend, terwijl de algemene internetverkeer wordt geblokkeerd.

Toegestane adresbereiken van internet kunnen worden opgegeven met behulp van [CIDR-notatie](https://tools.ietf.org/html/rfc4632) in het formulier *16.17.18.0/24* of afzonderlijke IP-adressen, zoals *16.17.18.19* .

> [!NOTE]
> Kleine adresbereiken met behulp van '/ 31' of '/ 32' voorvoegsel grootten worden niet ondersteund.  Deze bereiken moeten worden geconfigureerd met behulp van afzonderlijke regels voor IP-adres.
>

IP-netwerkregels zijn alleen toegestaan voor **openbare internet** IP-adressen.  IP-adresbereiken die zijn gereserveerd voor particuliere netwerken (zoals gedefinieerd in [RFC 1918](https://tools.ietf.org/html/rfc1918#section-3)) zijn niet toegestaan in de IP-regels.  Particuliere netwerken bevatten adressen die met beginnen *10.\** , *172.16.\**   -  *172.31.\**, en *192.168.\** .

Houd er rekening mee dat regels voor IP-netwerk heeft geen effect op aanvragen die afkomstig zijn van dezelfde Azure-regio als het opslagaccount.  Gebruik virtual network-regels waarmee aanvragen voor dezelfde regio.

Alleen IPV4-adressen worden ondersteund op dit moment.

Elk opslagaccount kan ondersteunen maximaal 100 IP-netwerkregels die kunnen worden gecombineerd met [Virtual Network-regels](#grant-access-from-a-virtual-network)

### <a name="configuring-access-from-on-premises-networks"></a>Toegang vanaf on-premises netwerken configureren
Als u wilt toegang verlenen vanuit uw on-premises netwerken aan uw storage-account met een regel voor IP-netwerk, moet u het internetgerichte IP-adressen die worden gebruikt door uw netwerk te identificeren.  Neem contact op met de netwerkbeheerder voor hulp.

Als uw netwerk is verbonden met de Azure-netwerk met behulp van [ExpressRoute](/azure/expressroute/expressroute-introduction), elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge die worden gebruikt voor het verbinding maken met Microsoft-Services zoals Azure Storage met behulp van [Openbare azure-Peering](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains).  Als u wilt toestaan dat communicatie tussen uw circuit en Azure Storage, moet u IP-netwerkregels voor openbare IP-adressen van uw circuits maken.  Om te kunnen vinden van openbare IP-adressen van uw ExpressRoute-circuit, [open een ondersteuningsticket met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via Azure portal.


### <a name="managing-ip-network-rules"></a>IP-netwerkregels beheren
IP-netwerkregels voor storage-accounts kunnen worden beheerd via de Azure portal, PowerShell of CLIv2.

#### <a name="azure-portal"></a>Azure Portal
1. Navigeer naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het instellingenmenu met de naam **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt zodat toegang vanaf 'Geselecteerde netwerken'.
4. Om te verlenen toegang tot een internet-IP-bereik, voer het IP-adres of adresbereik (in CIDR-indeling) onder Firewall-adresbereiken.
5. Als u wilt verwijderen van een regel voor IP-netwerk, klikt u op het prullenbakpictogram naast de regel van het netwerk.
6. Klik op *opslaan* de wijzigingen worden toegepast.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. Lijst met regels voor IP-netwerk.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
```

3. Voeg een regel voor een afzonderlijk IP-adres toe.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19" 
``` 

4. Voeg een regel voor een IP-adresbereik toe.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24" 
```    

5. Verwijderen van een regel voor een afzonderlijk IP-adres. 
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"  
```

6. Een regel voor een IP-adresbereik verwijderen.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of netwerkregels heeft geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [De Azure CLI installeren](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. Lijst met IP-netwerkregels
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
```

3. Voeg een regel voor een afzonderlijk IP-adres toe.
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

4. Voeg een regel voor een IP-adresbereik toe.  
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

5. Verwijderen van een regel voor een afzonderlijk IP-adres.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

6. Een regel voor een IP-adresbereik verwijderen.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of netwerkregels heeft geen effect.
>

## <a name="exceptions"></a>Uitzonderingen
Netwerkregels kunnen zorgen ervoor dat de configuratie van een beveiligd netwerk voor de meeste scenario's, maar er zijn enkele gevallen waarbij uitzonderingen moeten worden toegekend om in te schakelen van de volledige functionaliteit.  Storage-accounts kunnen worden geconfigureerd met uitzonderingen voor vertrouwde Microsoft-services, en voor toegang tot gegevens van Storage analytics.

### <a name="trusted-microsoft-services"></a>Vertrouwde Microsoft-Services
Sommige Microsoft-services die interactie met de Storage-accounts hebben werken van netwerken die toegang via netwerkregels kunnen niet worden toegekend. 

Als u wilt toestaan dat dit type service werken zoals verwacht, kunt u de set vertrouwde Microsoft-services om over te slaan van de netwerkregels toestaan. Deze services wordt sterke verificatie gebruikt voor toegang tot het opslagaccount.

Als de uitzondering 'Vertrouwde Microsoft Services' is ingeschakeld, worden de volgende services (indien geregistreerd in uw abonnement) toegang tot het Opslagaccount dat is verleend:

|Service|Resource-providernaam|Doel|
|:------|:---------------------|:------|
|Azure Backup|Microsoft.Backup|Back-ups en herstellen van niet-beheerde schijven in virtuele machines van IAAS uitvoeren. (niet vereist voor beheerde schijven). [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup).|
|Azure DevTest Labs|Microsoft.DevTestLab|Aangepaste installatiekopie maken en artefact installatie.  [Meer informatie](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|Blob-opslag gebeurtenis publiceren inschakelen.  [Meer informatie](https://docs.microsoft.com/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Gegevens met Event Hubs Capture archiveren.  [Meer informatie](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview).|
|Azure-netwerken|Microsoft.Networking|Store en netwerk-verkeerslogboeken te analyseren.  [Meer informatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview).|
|Azure Monitor|Microsoft.Insights| Kan het schrijven van gegevens naar een beveiligde storaage-account te controleren [meer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security#monitoring-and-secured-Azure-storage-and-networks).|
|


### <a name="storage-analytics-data-access"></a>Toegang tot gegevens van Storage analytics
In sommige gevallen is de toegang tot diagnostische logboeken en metrische gegevens lezen vereist van buiten de grens van het netwerk.  Uitzonderingen aan de netwerkregels kunnen worden verleend waarmee leestoegang tot Storage account logboekbestanden, metrische gegevens over tabellen, of beide. [Meer informatie over het werken met opslaganalyse.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Uitzonderingen beheren
Uitzonderingen op netwerk kunnen worden beheerd via de Azure portal, PowerShell of Azure CLI versie 2.

#### <a name="azure-portal"></a>Azure Portal
1. Navigeer naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het instellingenmenu met de naam **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt zodat toegang vanaf 'Geselecteerde netwerken'.
4. Selecteer onder uitzonderingen, de uitzonderingen die u wilt verlenen.
5. Klik op *opslaan* de wijzigingen worden toegepast.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. De uitzonderingen voor de storage-account-netwerkregels worden weergegeven.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. De uitzonderingen aan de storage-account network-regels configureren.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Verwijder de uitzonderingen voor de netwerkregels voor storage-account.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of verwijderen van uitzonderingen hebben geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [De Azure CLI installeren](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. De uitzonderingen voor de storage-account-netwerkregels worden weergegeven.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
```

3. De uitzonderingen aan de storage-account network-regels configureren.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Verwijder de uitzonderingen voor de netwerkregels voor storage-account.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Zorg ervoor dat u [de standaardregel instellen](#change-the-default-network-access-rule) te weigeren, of verwijderen van uitzonderingen hebben geen effect.
>

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure Service-eindpunten in [Service-eindpunten](/azure/virtual-network/virtual-network-service-endpoints-overview).

Duik dieper in Azure Storage-beveiliging in [Azure Storage-beveiligingshandleiding](storage-security-guide.md).
