---
title: Configureren van Firewalls voor Azure-opslag en virtuele netwerken (preview) | Microsoft Docs
description: Gelaagde netwerkbeveiliging configureren voor uw opslagaccount.
services: storage
documentationcenter: 
author: cbrooksmsft
manager: cbrooks
editor: cbrooks
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/25/2017
ms.author: cbrooks
ms.openlocfilehash: d29f2d180df93f45202e881336e492c45587b276
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks-preview"></a>Configureren van Firewalls voor Azure-opslag en virtuele netwerken (preview)
Azure Storage biedt een gelaagd beveiligingsmodel waarmee u voor het beveiligen van uw storage-accounts op een specifieke set toegestane netwerken.  Wanneer het netwerk regels zijn geconfigureerd, kan alleen toepassingen van toegestane netwerken toegang krijgen tot een opslagaccount.  Bij het aanroepen van een toegestane netwerk, blijven de toepassingen vereisen juiste autorisatie (een geldig toegangssleutel of SAS-token) voor toegang tot het opslagaccount.

## <a name="preview-availability-and-support"></a>Voorbeeld van beschikbaarheid en ondersteuning
Firewalls voor opslag en virtuele netwerken zijn Preview-versie.  Deze mogelijkheid is momenteel beschikbaar voor nieuwe of bestaande opslagaccounts in alle openbare Azure-cloud-regio's.

> [!NOTE]
> Productie-workloads worden niet ondersteund tijdens de preview.
>

## <a name="scenarios"></a>Scenario's
Storage-accounts kunnen worden geconfigureerd voor het weigeren van toegang tot het verkeer van alle netwerken (met inbegrip van internet-verkeer) standaard.  Toegang kan worden verleend voor verkeer van specifieke Azure virtuele netwerken, zodat u de grens van een beveiligd netwerk voor uw toepassingen te bouwen.  Toegang kan ook worden toegekend aan het openbare internet IP-adresbereiken, het inschakelen van verbindingen van specifieke internet of on-premises clients.

Netwerk-regels worden afgedwongen op alle netwerkprotocollen naar Azure-opslag, met inbegrip van REST en SMB.  Toegang tot uw gegevens van de hulpprogramma's zoals de Azure Storage Explorer-portal en AZCopy vereisen expliciete netwerk regels netwerk regels van kracht zijn om toegang te verlenen.

Netwerk-regels kunnen worden toegepast op bestaande opslagaccounts of tijdens het maken van nieuwe Storage-accounts kunnen worden toegepast.

Zodra het netwerk regels worden toegepast, worden ze afgedwongen voor alle aanvragen.  SAS-tokens die toegang tot een specifiek IP-adres-service verlenen leveren aan **limiet** de toegang van de houder token, maar nieuwe toegang niet verlenen buiten de geconfigureerde regels bieden. 

Schijf voor virtuele Machine verkeer (waaronder koppelen en ontkoppelen van bewerkingen, en schijf-i/o) **niet** van invloed op een netwerk-regels.  REST-toegang tot de pagina-blobs is beveiligd met de netwerk-regels.

> [!NOTE]
> Back-up en herstel van virtuele Machines met niet-beheerde schijven in de storage-accounts met netwerk regels toegepast is momenteel niet ondersteund.  Zie voor meer informatie [beperkingen bij een back-up en herstellen van een virtuele machine](/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm)
>

Klassieke opslagaccounts **niet** ondersteuning voor Firewalls en virtuele netwerken.

## <a name="change-the-default-network-access-rule"></a>De standaardregel voor toegang tot netwerk wijzigen
Standaard aanvaarden opslagaccounts verbindingen van clients op een netwerk.  Om te beperken van toegang tot geselecteerde netwerken, moet u eerst de standaardactie wijzigen.

> [!WARNING]
> Wijzigingen aanbrengen in het netwerk regels kan invloed hebben op uw toepassingen kunnen verbinding maken met Azure Storage.  Als u de standaardregel voor het netwerk op **weigeren** blokkeert u de toegang tot de gegevens tenzij specifieke regels *verlenen* toegang worden ook toegepast.  Zorg dat u toegang verlenen tot een toegestane netwerken die gebruikmaken van netwerk regels voordat u de standaardregel voor het weigeren van toegang wijzigen.
>

#### <a name="azure-portal"></a>Azure Portal
1. Ga naar het opslagaccount dat u wilt beveiligen.  
> [!NOTE]
> Zorg ervoor dat uw storage-account in een van de ondersteunde regio's voor de openbare preview.
>

2. Klik op het menu instellingen aangeroepen **Firewalls en virtuele netwerken**.
3. Kies voor het weigeren van toegang standaard, toegang toestaan via geselecteerde-netwerken.  Kies toegang toestaan via 'alle netwerken, zodat verkeer van alle netwerken.
4. Klik op *opslaan* uw wijzigingen toe te passen.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).

2. De status van de standaardregel voor de storage-account weergeven.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. De standaardregel voor toegang tot het netwerk weigeren standaard ingesteld.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. De standaardregel om toegang tot het netwerk standaard ingesteld.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLIv2
1. [Installeer Azure CLI 2.0](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. De status van de standaardregel voor de storage-account weergeven.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
```

3. De standaardregel voor toegang tot het netwerk weigeren standaard ingesteld.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. De standaardregel om toegang tot het netwerk standaard ingesteld.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Toegang verlenen van een virtueel netwerk
Storage-accounts kunnen worden geconfigureerd voor toegang alleen via specifieke Azure Virtual Networks. 

Doordat een [Service-eindpunt](/azure/virtual-network/virtual-network-service-endpoints-overview) voor Azure Storage in het virtuele netwerk, verkeer een optimale route naar de Azure Storage-service is gewaarborgd. De identiteit van het virtuele netwerk en het subnet zijn ook elke aanvraag verzonden.  Beheerders kunnen vervolgens netwerk regels voor het opslagaccount waarmee aanvragen worden ontvangen vanaf de specifieke subnetten in het virtuele netwerk configureren.  Clients toegang via deze regels netwerk moet voldoen aan de autorisatievereisten van het opslagaccount van de voor toegang tot de gegevens worden verleend.

Elk opslagaccount kan ondersteunen maximaal 100 Virtual Network-regels die kunnen worden gecombineerd met [IP-netwerk regels](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Beschikbaar virtueel netwerk regio 's
Service-eindpunten werken in het algemeen tussen virtuele netwerken en service-exemplaren in dezelfde Azure-regio.  Als Service-eindpunten die worden gebruikt met Azure Storage, dit bereik is uitgebreid met de [gekoppelde regio](/azure/best-practices-availability-paired-regions).  Hierdoor continuïteit tijdens een regionale failover, evenals de seemless toegang tot de alleen-lezen geo-reduntant opslagaccount (RA-GRS) exemplaren.  Netwerk-regels waarmee toegang wordt verleend van een virtueel netwerk met een opslagaccount ook verlenen toegang tot een willekeurig exemplaar RA-GRS.

Bij het plannen voor herstel na noodgevallen tijdens een regionale uitval, moet u de virtuele netwerken in het gekoppelde gebied vooraf inrichten. Service-eindpunten voor Azure Storage moet worden ingeschakeld en netwerk regels toegang verlenen op basis van deze alternatieve virtuele netwerken moeten worden toegepast op uw accounts geografisch redundante opslag.

> [!NOTE]
> Service-eindpunten die niet van toepassing op verkeer buiten de regio van het virtuele netwerk en het paar aangewezen regio.  Netwerk regels toegang verlenen op basis van virtuele netwerken op opslagaccounts kunnen alleen worden toegepast voor virtuele netwerken in de primaire regio van een opslagaccount of de aangewezen gekoppelde regio.
>

### <a name="required-permissions"></a>Vereiste machtigingen
Om een virtueel netwerk regel toepassen op een opslagaccount, de gebruiker gemachtigd bent om *Service koppelen aan een Subnet* voor de subnetten die wordt toegevoegd.  Deze machtiging is opgenomen in de *Storage Account Inzender* ingebouwde rol en kan worden toegevoegd aan de aangepaste roldefinities.

Storage-account en de virtuele netwerken toegang verleend **kan** worden tot verschillende abonnementen behoren, maar deze abonnementen moeten deel uitmaken van dezelfde Azure Active Directory-tenant.

### <a name="managing-virtual-network-rules"></a>Regels voor het beheren van het virtuele netwerk
Virtueel netwerk regels voor storage-accounts kunnen worden beheerd via de Azure portal, PowerShell of CLIv2.

#### <a name="azure-portal"></a>Azure Portal
1. Ga naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het menu instellingen aangeroepen **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt toegang toestaan via geselecteerde-netwerken.
4. Klik op 'Bestaande toevoegen' om toegang tot een virtueel netwerk met een nieuwe regel voor het netwerk, onder 'Virtuele netwerken' om een bestaand virtueel netwerk en subnetten hebt geselecteerd, klikt u vervolgens op *toevoegen*.  Voor het maken van een nieuw virtueel netwerk en het toegang geven, klikt u op *nieuwe toevoegen*, geef de benodigde informatie om de nieuwe virtuele netwerk maken en klik vervolgens op *maken*.

> [!NOTE]
> Als een Service-eindpunt voor Azure Storage heeft niet eerder is geconfigureerd voor de geselecteerde virtuele netwerk en subnetten, kan worden geconfigureerd als onderdeel van deze bewerking.
>

5. Voor het verwijderen van een virtueel netwerk of subnet regel '...' om te openen van het snelmenu voor het virtuele netwerk of het subnet op en klik op 'Verwijderen'.
6. Klik op *opslaan* uw wijzigingen toe te passen.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. Lijst met Virtual Network-regels
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Service-eindpunt inschakelen voor Azure Storage op een bestaand virtueel netwerk en Subnet
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Een netwerk-regel voor een virtueel netwerk en subnet toevoegen.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Een netwerk-regel voor een virtueel netwerk en subnet verwijderen.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of regels netwerk heeft geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [Installeer Azure CLI 2.0](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. Lijst met Virtual Network-regels
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Service-eindpunt inschakelen voor Azure Storage op een bestaand virtueel netwerk en Subnet
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Een netwerk-regel voor een virtueel netwerk en subnet toevoegen.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

4. Een netwerk-regel voor een virtueel netwerk en subnet verwijderen. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of regels netwerk heeft geen effect.
>

## <a name="grant-access-from-an-internet-ip-range"></a>Toegang verlenen vanop een internet IP-bereik
Storage-accounts kunnen worden geconfigureerd om toegang te verlenen vanaf internet voor specifieke openbare IP-adresbereiken.  Deze configuratie kunt specifieke op internet gebaseerde services en on-premises netwerken toegang verleent terwijl algemene internetverkeer wordt geblokkeerd.

Internet-adresbereiken worden geleverd met toegestaan [CIDR-notatie](https://tools.ietf.org/html/rfc4632) in het formulier *16.17.18.0/24* of afzonderlijke IP-adressen, zoals *16.17.18.19* .

> [!NOTE]
> Kleine-adresbereiken met '/ 31' of '/ 32' voorvoegsel grootten worden niet ondersteund.  Deze bereiken moeten worden geconfigureerd met behulp van afzonderlijke regels voor IP-adres.
>

IP-netwerk regels zijn alleen toegestaan voor **openbare internet** IP-adressen.  IP-adresbereiken zijn gereserveerd voor particuliere netwerken (zoals gedefinieerd in RFC 1918) zijn niet toegestaan in de IP-regels.  Particuliere netwerken adressen die met beginnen bevatten *10.\** , *172.16.\** , en *192.168.\** .

Alleen IPV4-adressen worden ondersteund op dit moment.

Elk opslagaccount kan ondersteunen maximaal 100 regels van de IP-netwerk die kunnen worden gecombineerd met [Virtual Network-regels](#grant-access-from-a-virtual-network)

### <a name="configuring-access-from-on-premises-networks"></a>Toegang vanaf on-premises netwerken configureren
Om toegang te verlenen vanaf uw on-premises netwerken aan uw storage-account met een regel voor IP-netwerk, moet u de internetverbinding van IP-adressen die worden gebruikt door uw netwerk te identificeren.  Neem contact op met de netwerkbeheerder voor hulp.

Als uw netwerk is verbonden met het Azure-netwerk via [ExpressRoute](/azure/expressroute/expressroute-introduction), elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge die worden gebruikt voor het verbinding maken met Microsoft-Services zoals Azure Storage met [Openbare azure-Peering](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains).  Als u communicatie van uw circuit naar Azure Storage, moet u IP-netwerk regels voor het openbare IP-adressen van uw circuits maken.  Om uw ExpressRoute-circuit openbare IP-adressen vinden [een ondersteuningsticket opent met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via de Azure-portal.


### <a name="managing-ip-network-rules"></a>Regels voor het beheer van IP-netwerk
IP-netwerk regels voor storage-accounts kunnen worden beheerd via de Azure-portal, PowerShell of CLIv2.

#### <a name="azure-portal"></a>Azure Portal
1. Ga naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het menu instellingen aangeroepen **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt toegang toestaan via geselecteerde-netwerken.
4. Verlenen toegang tot een internet-IP-bereik, voert u de IP-adres of -adresbereik (in CIDR-indeling) onder Firewall-adresbereiken.
5. Een IP-netwerk als regel wilt verwijderen, klik op '...' om te openen van het snelmenu voor de regel en klikt u op 'Verwijderen'.
6. Klik op *opslaan* uw wijzigingen toe te passen.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. Lijst met IP-netwerk regels.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
```

3. Een netwerk-regel voor een afzonderlijk IP-adres toevoegen.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19" 
``` 

4. Een netwerk-regel voor een IP-adresbereik toevoegen.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24" 
```    

5. Verwijderen van een netwerk-regel voor een afzonderlijk IP-adres. 
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"  
```

6. Verwijderen van een netwerk-regel voor een IP-adresbereik.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of regels netwerk heeft geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [Installeer Azure CLI 2.0](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. Lijst met IP-netwerk regels
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
```

3. Een netwerk-regel voor een afzonderlijk IP-adres toevoegen.
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

4. Een netwerk-regel voor een IP-adresbereik toevoegen.  
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

5. Verwijderen van een netwerk-regel voor een afzonderlijk IP-adres.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

6. Verwijderen van een netwerk-regel voor een IP-adresbereik.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of regels netwerk heeft geen effect.
>

## <a name="exceptions"></a>Uitzonderingen
Netwerk-regels kunnen inschakelen een veilige configuratie voor de meeste scenario's, maar er zijn enkele gevallen waarbij uitzonderingen moeten worden toegekend aan het inschakelen van de volledige functionaliteit van.  Storage-accounts kunnen worden geconfigureerd met uitzonderingen voor vertrouwde Microsoft-services en voor toegang tot opslag analytische gegevens.

### <a name="trusted-microsoft-services"></a>Vertrouwde Microsoft-Services
Sommige Microsoft-services die interactie met Storage-accounts hebben werken van netwerken die toegang via het netwerk regels kunnen niet worden toegekend. 

Als u wilt toestaan dat dit type service werken zoals verwacht, kunt u de set vertrouwde Microsoft-services voor het overslaan van de regels van het netwerk verlenen. Deze services wordt sterke verificatie gebruikt voor toegang tot het opslagaccount.

Wanneer de uitzondering 'Microsoft-Services vertrouwde' is ingeschakeld, krijgen de volgende services (wanneer ingeschreven in uw abonnement) toegang tot het opslagaccount:

|Service|De naam van de Resource-Provider|Doel|
|:------|:---------------------|:------|
|Azure DevTest Labs|Microsoft.DevTestLab|Aangepaste installatiekopie maken en artefact installatie.  [Meer informatie](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|Blob Storage gebeurtenis publicaties mogelijk maken.  [Meer informatie](https://docs.microsoft.com/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Archiveren van gegevens met Event Hubs vastleggen.  [Meer informatie](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview).|
|Azure-netwerken|Microsoft.Networking|Opslaan en analyseren van logboeken over webverkeer netwerk.  [Meer informatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview).|
||||

### <a name="storage-analytics-data-access"></a>Toegang tot de gegevens van de opslag analytics
In sommige gevallen is de toegang tot het lezen van diagnostische logboeken en metrische gegevens vereist is van buiten de netwerkgrens.  Uitzonderingen voor de netwerk-regels kunnen worden verleend waarmee leestoegang tot Storage account logboekbestanden, metrische gegevens tabellen of beide. [Meer informatie over het werken met storage analytics.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Het beheren van uitzonderingen
Regeluitzonderingen netwerk kunnen worden beheerd via de Azure-portal, PowerShell of Azure CLI v2.

#### <a name="azure-portal"></a>Azure Portal
1. Ga naar het opslagaccount dat u wilt beveiligen.  
2. Klik op het menu instellingen aangeroepen **Firewalls en virtuele netwerken**.
3. Zorg ervoor dat u ervoor gekozen hebt toegang toestaan via geselecteerde-netwerken.
4. Selecteer onder uitzonderingen, de uitzonderingen die u wilt verlenen.
5. Klik op *opslaan* uw wijzigingen toe te passen.

#### <a name="powershell"></a>PowerShell
1. Installeer de meest recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) en [aanmelding](/powershell/azure/authenticate-azureps).
2. De uitzonderingen voor de storage-account netwerk regels worden weergegeven.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. De uitzonderingen aan de regels van storage account netwerk configureren.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Hiermee verwijdert u de uitzonderingen aan de regels van storage account netwerk.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of het verwijderen van uitzonderingen heeft geen effect.
>

#### <a name="cliv2"></a>CLIv2
1. [Installeer Azure CLI 2.0](/cli/azure/install-azure-cli) en [aanmelding](/cli/azure/authenticate-azure-cli).
2. De uitzonderingen voor de storage-account netwerk regels worden weergegeven.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
```

3. De uitzonderingen aan de regels van storage account netwerk configureren.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Hiermee verwijdert u de uitzonderingen aan de regels van storage account netwerk.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Zorg ervoor dat [de standaardregel ingesteld](#change-the-default-network-access-rule) op weigeren, of het verwijderen van uitzonderingen heeft geen effect.
>

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure Network Service-eindpunten in [Service-eindpunten](/azure/virtual-network/virtual-network-service-endpoints-overview).

Dig dieper in Azure Storage-beveiliging in [Azure Storage-beveiligingshandleiding](storage-security-guide.md).
