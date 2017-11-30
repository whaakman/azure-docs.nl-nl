---
title: Omgekeerde DNS voor Azure-services | Microsoft Docs
description: Informatie over het configureren van de reverse DNS-zoekacties voor services die worden gehost in Azure
services: dns
documentationcenter: na
author: KumudD
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: kumud
ms.openlocfilehash: 0c5d12e9d6b5ddbee2a930e4e537b8180b7a9c7b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Omgekeerde DNS voor services die worden gehost in Azure configureren

In dit artikel wordt uitgelegd hoe het configureren van de reverse DNS-zoekacties voor services die worden gehost in Azure.

Services in Azure gebruiken IP-adressen toegewezen door Azure en het eigendom van Microsoft. Deze omgekeerde DNS-records (PTR-records) moeten worden gemaakt in de bijbehorende die eigendom zijn van Microsoft omgekeerde DNS-lookup zones. Dit artikel wordt uitgelegd hoe u dit doet.

Dit scenario moet niet de mogelijkheid om te verwarren [de zones voor reverse DNS-lookup voor uw toegewezen IP-adresbereiken in Azure DNS hosten](dns-reverse-dns-hosting.md). In dit geval moeten de IP-adresbereiken dat wordt vertegenwoordigd door de zone voor reverse lookup worden toegewezen aan uw organisatie gewoonlijk door uw Internetprovider.

Voordat u dit artikel leest, moet u bekend bent met dit [overzicht van de reverse DNS- en biedt ondersteuning in Azure](dns-reverse-dns-overview.md).

In Azure DNS compute-bronnen (zoals virtuele machines, virtuele-machineschaalsets of Service Fabric-clusters) beschikbaar worden gemaakt via een PublicIpAddress-resource. Omgekeerde DNS-zoekacties zijn geconfigureerd met de eigenschap 'ReverseFqdn' van de PublicIpAddress.


Omgekeerde DNS is momenteel niet ondersteund voor de Azure App Service.

## <a name="validation-of-reverse-dns-records"></a>Validatie van de reverse DNS-records

Een derde partij zou niet mogen maken van de reverse DNS-records voor de Azure-service wordt toegewezen aan uw DNS-domeinen. Om dit te voorkomen, Azure alleen het maken van een omgekeerde DNS-record domeinnaam opgegeven in de omgekeerde DNS-record hetzelfde als waar is of wordt omgezet naar de DNS-naam of IP-adres van een PublicIpAddress of de Cloudservice in dezelfde Azure-abonnement.

Deze validatie wordt alleen uitgevoerd wanneer de omgekeerde DNS-record wordt ingesteld of gewijzigd. Periodieke opnieuw validatie is niet uitgevoerd.

Voorbeeld: Stel dat de PublicIpAddress-resource heeft de DNS-naam contosoapp1.northus.cloudapp.azure.com en het IP-adres 23.96.52.53. De ReverseFqdn voor de PublicIpAddress kan worden opgegeven als:
* De DNS-naam voor de PublicIpAddress contosoapp1.northus.cloudapp.azure.com
* De DNS-naam voor een andere PublicIpAddress in hetzelfde abonnement behoren, zoals contosoapp2.westus.cloudapp.azure.com
* Een vanity DNS-naam, zoals app1.contoso.com, zolang deze naam *eerste* geconfigureerd als een CNAME contosoapp1.northus.cloudapp.azure.com of een andere PublicIpAddress in hetzelfde abonnement.
* Een vanity DNS-naam, zoals app1.contoso.com, zolang deze naam *eerste* geconfigureerd als een A-record in de IP-adres 23.96.52.53 of het IP-adres van een andere PublicIpAddress in hetzelfde abonnement.

De beperkingen voor dezelfde toepassing omgekeerde DNS voor Cloud-Services.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Omgekeerde DNS voor PublicIpAddress resources

Deze sectie bevat gedetailleerde instructies voor het configureren van omgekeerde DNS voor PublicIpAddress bronnen in het Resource Manager-implementatiemodel, met Azure PowerShell, Azure CLI 1.0 of 2.0 voor Azure CLI. Configureren van omgekeerde DNS voor PublicIpAddress bronnen is momenteel niet ondersteund via de Azure-portal.

Azure momenteel ondersteunt reverse DNS alleen voor IPv4 PublicIpAddress resources. Dit wordt niet ondersteund voor IPv6.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Omgekeerde DNS toevoegen aan een bestaande PublicIpAddresses

#### <a name="powershell"></a>PowerShell

Omgekeerde DNS toevoegen aan een bestaande PublicIpAddress:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

Als omgekeerde DNS toevoegen aan een bestaande PublicIpAddress die nog niet over een DNS-naam, moet u ook een DNS-naam opgeven:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

Omgekeerde DNS toevoegen aan een bestaande PublicIpAddress:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Als omgekeerde DNS toevoegen aan een bestaande PublicIpAddress die nog niet over een DNS-naam, moet u ook een DNS-naam opgeven:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

Omgekeerde DNS toevoegen aan een bestaande PublicIpAddress:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Als omgekeerde DNS toevoegen aan een bestaande PublicIpAddress die nog niet over een DNS-naam, moet u ook een DNS-naam opgeven:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Maken van een openbaar IP-adres met de reverse DNS

Een nieuwe PublicIpAddress maken met de reverse DNS-eigenschap is al opgegeven:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Weergave omgekeerde DNS voor een bestaande PublicIpAddress

De geconfigureerde waarde voor een bestaande PublicIpAddress bekijken:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Omgekeerde DNS verwijderen uit bestaande openbare IP-adressen

Verwijdert een omgekeerde DNS-eigenschap van een bestaande PublicIpAddress:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Omgekeerde DNS configureren voor Cloud-Services

Deze sectie bevat gedetailleerde instructies voor het omgekeerde DNS configureren voor Cloud-Services in het klassieke implementatiemodel met Azure PowerShell. Omgekeerde DNS configureren voor Cloud-Services wordt niet ondersteund via de Azure-portal, Azure CLI 1.0 of 2.0 voor Azure CLI.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Omgekeerde DNS toevoegen aan bestaande Cloud-Services

Een omgekeerde DNS-record toevoegen aan een bestaande Cloudservice:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Maak een Cloudservice met omgekeerde DNS

Een nieuwe Cloudservice maken met de reverse DNS-eigenschap is al opgegeven:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Weergave omgekeerde DNS voor bestaande Cloud-Services

De omgekeerde DNS-eigenschap voor een bestaande Cloudservice bekijken:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Omgekeerde DNS verwijderen uit bestaande Cloud-Services

Een omgekeerde DNS-eigenschap van een bestaande Cloudservice verwijderen:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="how-much-do-reverse-dns-records-cost"></a>Hoeveel reverse DNS-records kosten?

Ze zijn gratis!  Er is geen extra kosten voor omgekeerde DNS-records en query's.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Wordt mijn omgekeerde DNS-records van het internet oplossen?

Ja. Als u de reverse DNS-eigenschap voor uw Azure-service hebt ingesteld, wordt Azure beheert alle DNS-delegeringen en DNS-zones die zijn vereist om ervoor te zorgen dat de reverse DNS-record wordt herleid voor alle gebruikers van Internet.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Worden standaard omgekeerde DNS-records gemaakt voor mijn Azure-services?

Nee. Omgekeerde DNS is een opt-in-functie. Er zijn geen standaard omgekeerde DNS-records worden gemaakt als u niet om ze te configureren.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Wat is de indeling voor de volledig gekwalificeerde domeinnaam (FQDN)?

FQDN-namen zijn opgegeven in oplopende volgorde en moeten worden afgesloten met een punt (bijvoorbeeld ' app1.contoso.com.').

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>Wat gebeurt er als de validatiecontrole voor de reverse DNS-server die ik hebt opgegeven mislukt?

Wanneer de omgekeerde DNS-validatiecontrole mislukt, mislukt de bewerking voor het configureren van de reverse DNS-record. Corrigeer de reverse DNS-waarde als vereist en probeer het opnieuw.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Kan ik omgekeerde DNS configureren voor Azure App Service?

Nee. Omgekeerde DNS wordt niet ondersteund voor de Azure App Service.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Kan ik meerdere omgekeerde DNS-records configureren voor mijn Azure-service?

Nee. Azure biedt ondersteuning voor één omgekeerde DNS-record voor elk Azure Cloud Service of PublicIpAddress.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Kan ik omgekeerde DNS voor IPv6-PublicIpAddress-resources configureren?

Nee. Azure momenteel ondersteunt reverse DNS alleen voor IPv4 PublicIpAddress resources en Cloudservices.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Kan ik e-mailberichten verzenden naar externe domeinen uit mijn Azure Compute-services?

Nee. [Azure Compute-services bieden geen ondersteuning voor externe domeinen verzenden e-mailberichten](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [achterwaartse DNS-zoekopdracht op Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over hoe [zone voor reverse lookup voor uw Internetprovider toegewezen IP-adresbereik in Azure DNS hosten](dns-reverse-dns-for-azure-services.md).

