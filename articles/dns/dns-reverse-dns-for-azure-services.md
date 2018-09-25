---
title: Reverse-DNS voor Azure-services | Microsoft Docs
description: Informatie over het configureren van omgekeerde DNS-zoekacties voor services die worden gehost in Azure
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: cbd1a7a3a797cc20be92583bbb5ac163333729fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969798"
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Reverse-DNS voor services die worden gehost in Azure configureren

In dit artikel wordt uitgelegd hoe het configureren van omgekeerde DNS-zoekacties voor services die worden gehost in Azure.

Services in Azure gebruiken IP-adressen die door Azure is toegewezen en beheerd door Microsoft. Deze omgekeerde DNS-records (PTR-records) moeten worden gemaakt in de bijbehorende eigendom van Microsoft DNS-zones voor omgekeerde zoekacties. In dit artikel wordt uitgelegd hoe u dit doet.

In dit scenario moet niet worden verward met de mogelijkheid om [de zones voor omgekeerde DNS-zoekacties voor de toegewezen IP-adresbereiken in Azure DNS hosten](dns-reverse-dns-hosting.md). In dit geval moeten de IP-adresbereiken die wordt vertegenwoordigd door de zone voor reverse lookup worden toegewezen aan uw organisatie, meestal door uw Internetprovider.

Voordat u dit artikel leest, moet u bekend bent met dit [overzicht omgekeerde DNS-en ondersteuning in Azure](dns-reverse-dns-overview.md).

Compute-resources (zoals virtuele machines, virtuele-machineschaalsets of Service Fabric-clusters) worden in Azure DNS, beschikbaar gesteld via een openbare IP-adres-resource. Omgekeerde DNS-zoekacties zijn geconfigureerd met behulp van de eigenschap 'ReverseFqdn' van het openbare IP-adres.


Reverse-DNS is momenteel niet ondersteund voor de Azure App Service.

## <a name="validation-of-reverse-dns-records"></a>Validatie van omgekeerde DNS-records

Een derde partij mag geen omgekeerde DNS-records voor de toewijzing van hun Azure-service om uw DNS-domeinen te maken. Om dit te voorkomen, Azure alleen het maken van een omgekeerde DNS-record waar domeinnaam opgegeven in de omgekeerde DNS-record is gelijk aan of wordt omgezet naar de DNS-naam of IP-adres van een openbare IP-adres of de Service in de Cloud in hetzelfde Azure-abonnement.

Deze validatie wordt alleen uitgevoerd wanneer de omgekeerde DNS-record wordt ingesteld of gewijzigd. Periodiek opnieuw validatie wordt niet uitgevoerd.

Voorbeeld: Stel dat de resource van het openbare IP-adres is de DNS-naam contosoapp1.northus.cloudapp.azure.com en het IP-adres 23.96.52.53. De ReverseFqdn voor het openbare IP-adres kan worden opgegeven als:
* De DNS-naam voor het openbare IP-adres, contosoapp1.northus.cloudapp.azure.com
* De DNS-naam voor een andere openbare IP-adres in hetzelfde abonnement, zoals contosoapp2.westus.cloudapp.azure.com
* Een aangepaste DNS-naam, zoals app1.contoso.com, zolang deze naam *eerste* geconfigureerd als een CNAME contosoapp1.northus.cloudapp.azure.com of een andere openbare IP-adres in hetzelfde abonnement.
* Een aangepaste DNS-naam, zoals app1.contoso.com, zolang deze naam *eerste* geconfigureerd als een A-record naar het IP-adres 23.96.52.53 of het IP-adres van een andere openbare IP-adres in hetzelfde abonnement.

De dezelfde beperkingen gelden voor de reverse-DNS voor Cloud Services.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Reverse-DNS voor PublicIpAddress-resources

Deze sectie bevat gedetailleerde instructies voor het configureren van omgekeerde DNS voor PublicIpAddress-resources in het Resource Manager-implementatiemodel, met behulp van Azure PowerShell, Azure klassieke CLI of Azure CLI. Reverse-DNS voor PublicIpAddress-resources configureren wordt momenteel niet ondersteund via Azure portal.

Azure momenteel ondersteunt reverse-DNS alleen voor IPv4-PublicIpAddress-resources. Het wordt niet ondersteund voor IPv6.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Reverse-DNS toevoegen aan een bestaande PublicIpAddresses

#### <a name="powershell"></a>PowerShell

Reverse-DNS toevoegen aan een bestaande openbare IP-adres:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Reverse-DNS toevoegen aan een bestaande openbare IP-adres dat niet al een DNS-naam, moet u ook een DNS-naam opgeven:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>CLI van Azure classic

Reverse-DNS toevoegen aan een bestaande openbare IP-adres:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Reverse-DNS toevoegen aan een bestaande openbare IP-adres dat niet al een DNS-naam, moet u ook een DNS-naam opgeven:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure-CLI

Reverse-DNS toevoegen aan een bestaande openbare IP-adres:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Reverse-DNS toevoegen aan een bestaande openbare IP-adres dat niet al een DNS-naam, moet u ook een DNS-naam opgeven:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Een openbaar IP-adres maken met omgekeerde DNS

Een nieuwe openbare IP-adres maken met de omgekeerde DNS-eigenschap is al opgegeven:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-classic-cli"></a>CLI van Azure classic

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Weergave omkeren DNS voor een bestaande openbare IP-adres

De geconfigureerde waarde voor een bestaande openbare IP-adres weergeven:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-classic-cli"></a>CLI van Azure classic

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Reverse-DNS verwijderen uit bestaande openbare IP-adressen

Verwijdert een omgekeerde DNS-eigenschap van een bestaande openbare IP-adres:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>CLI van Azure classic

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Reverse-DNS configureren voor Cloudservices

Deze sectie vindt u gedetailleerde instructies voor omgekeerde DNS configureren voor Cloudservices in het klassieke implementatiemodel met behulp van Azure PowerShell. Omgekeerde DNS configureren voor Cloud Services wordt niet ondersteund via de Azure portal, Azure klassieke CLI of Azure CLI.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Reverse-DNS toevoegen aan bestaande Cloudservices

Een omgekeerde DNS-record toevoegen aan een bestaande Cloudservice:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Een Cloudservice maken met omgekeerde DNS

Een nieuwe Cloudservice maken met de omgekeerde DNS-eigenschap is al opgegeven:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Weergave reverse-DNS voor bestaande Cloudservices

De omgekeerde DNS-eigenschap voor een bestaande Cloudservice bekijken:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Reverse-DNS verwijderen uit bestaande Cloudservices

Een omgekeerde DNS-eigenschap van een bestaande Cloudservice verwijderen:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-much-do-reverse-dns-records-cost"></a>Hoeveel reverse-DNS-records kosten?

Ze zijn gratis!  Er is geen extra kosten voor omgekeerde DNS-records en query's.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Wordt mijn omgekeerde DNS-records omzetten via internet?

Ja. Als u de omgekeerde DNS-eigenschap voor uw Azure-service hebt ingesteld, wordt Azure beheert alle DNS-delegeringen en DNS-zones die zijn vereist om ervoor te zorgen dat omgekeerde DNS-record voor alle gebruikers van Internet oplost.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Worden standaard omgekeerde DNS-records gemaakt voor mijn Azure-services?

Nee. Reverse-DNS is een functie zich moet aanmelden. Er zijn geen standaard omgekeerde DNS-records worden gemaakt als u ervoor kiest niet te configureren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wat is de indeling voor de volledig gekwalificeerde domeinnaam (FQDN)?

FQDN-namen zijn opgegeven in oplopende volgorde, en moeten worden afgesloten met een punt (bijvoorbeeld ' app1.contoso.com.").

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Wat gebeurt er als de validatiecontrole voor de omgekeerde DNS-server die ik heb opgegeven mislukt?

Wanneer de omgekeerde DNS-validatiecontrole mislukt, mislukt de bewerking voor het configureren van de omgekeerde DNS-record. Corrigeer de omgekeerde DNS-waarde zoals vereist en probeer het opnieuw.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Kan ik reverse-DNS configureren voor Azure App Service?

Nee. Reverse-DNS wordt niet ondersteund voor de Azure App Service.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Kan ik meerdere omgekeerde DNS-records configureren voor mijn Azure-service?

Nee. Azure ondersteunt een enkele omgekeerde DNS-record voor elke Azure-Cloudservice of openbare IP-adres.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Kan ik reverse-DNS voor IPv6-PublicIpAddress-resources configureren?

Nee. Azure momenteel ondersteunt reverse-DNS alleen voor IPv4-PublicIpAddress-resources en Cloud Services.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kan ik e-mailberichten verzenden naar externe domeinen van mijn Azure Compute-services?

De technische mogelijkheid voor het verzenden van e-mailbericht rechtstreeks vanuit een Azure-implementatie, is afhankelijk van het abonnementstype. Microsoft adviseert, ongeacht het abonnementstype, vertrouwde e-mail-relayservices om uitgaande e-mail te verzenden. Zie voor meer details over de [uitgebreide Azure-beveiliging voor het verzenden van E-mails: November 2017 Update](https://blogs.msdn.microsoft.com/mast/2017/11/15/enhanced-azure-security-for-sending-emails-november-2017-update/).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [omgekeerde DNS-zoekactie voor Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over het [host voor de zone voor reverse lookup voor uw Internet-provider toegewezen IP-adresbereik in Azure DNS](dns-reverse-dns-for-azure-services.md).

