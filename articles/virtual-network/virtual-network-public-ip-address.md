---
title: Maken, wijzigen of verwijderen van een openbaar IP-adres van Azure | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een openbaar IP-adres.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 2dea51b045862659bceb8828f9699e6dfe8efad5
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378224"
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Maken, wijzigen of verwijderen van een openbaar IP-adres

Meer informatie over een openbaar IP-adres en het maken, wijzigen en verwijderen. Een openbaar IP-adres is een resource met zijn eigen configureerbare instellingen. Een openbaar IP-adres toewijzen aan een Azure-resource die ondersteuning biedt voor openbare IP-adressen kunt:
- Binnenkomende communicatie via Internet tot de resource, zoals Azure Virtual Machines (VM), Application Gateways voor Azure, Azure Load Balancers, Azure VPN-Gateways en anderen. U kunt nog steeds met de communiceren met enkele resources, zoals virtuele machines, via Internet, als een virtuele machine geen openbaar IP-adres toegewezen, als de virtuele machine deel uit van een back-end-pool van load balancer maakt, en de load balancer een openbaar IP-adres wordt toegewezen. Om te bepalen of een resource voor een specifieke Azure-service kan worden toegewezen openbaar IP-adres of of deze kan worden gecommuniceerd met via het openbare IP-adres van een andere Azure-resource, Zie de documentatie voor de service. 
- Uitgaande verbinding met Internet via een voorspelbare IP-adres. Een virtuele machine kan bijvoorbeeld communiceren uitgaand naar het Internet zonder een openbaar IP-adres toegewezen aan deze, maar het adres is vertaald door Azure op een onvoorspelbare openbare adres, standaard-netwerkadres. Een openbaar IP-adres toewijzen aan een resource, kunt u weten welk IP-adres wordt gebruikt voor de uitgaande verbindingen. Hoewel voorspelbare, kunt het adres wijzigen, afhankelijk van de gekozen methode voor het toewijzen. Zie voor meer informatie, [een openbaar IP-adres maken](#create-a-public-ip-address). Zie voor meer informatie over uitgaande verbindingen van Azure-resources, [uitleg over uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor deze zelfstudie is moduleversie 5.7.0 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.31 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure, moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

Openbare IP-adressen hebben geringe kosten. Als u wilt weergeven op de prijzen, lees de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. 

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

1. Selecteer in de linkerbovenhoek hoek van de portal, **+ een resource maken**.
2. Voer *openbaar ip-adres* in de *Marketplace doorzoeken* vak. Wanneer **openbaar IP-adres** wordt weergegeven in de zoekresultaten, selecteert u dit.
3. Onder **openbaar IP-adres**, selecteer **maken**.
4. Typ of Selecteer waarden voor de volgende instellingen onder **openbare IP-adres maken**en selecteer vervolgens **maken**:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |Naam|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert.|
    |SKU|Ja|Alle openbare IP-adressen die vóór de introductie van SKU's zijn gemaakt, **Basic** SKU openbare IP-adressen.  U kunt de SKU niet wijzigen nadat het openbare IP-adres is gemaakt. Een zelfstandige virtuele machine, virtuele machines binnen een beschikbaarheidsset of virtuele-machineschaalsets kunt gebruiken, basis of standaard-SKU's.  Met een combinatie van SKU's tussen virtuele machines in beschikbaarheidssets of schaalsets is niet toegestaan. **Basic** SKU: als u een openbaar IP-adres in een regio die ondersteuning voor beschikbaarheidszones biedt, maakt de **binnen een beschikbaarheidszone** is ingesteld op *geen* standaard. U kunt een beschikbaarheidszone om te waarborgen van een specifieke zone voor uw openbare IP-adres te selecteren. **Standard** SKU: standaard-SKU van een openbaar IP-adres kan worden gekoppeld aan een virtuele machine of een load balancer front-end. Als u een openbaar IP-adres in een regio die ondersteuning voor beschikbaarheidszones biedt, maakt de **binnen een beschikbaarheidszone** is ingesteld op *Zone-redundante* standaard. Zie voor meer informatie over beschikbaarheidszones, de **binnen een beschikbaarheidszone** instelling. De standaard-SKU is vereist als u het adres aan een Standard load balancer koppelen. Zie voor meer informatie over de standaardversie van load balancers, [Azure load balancer standaard-SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.|
    |IP-versie|Ja| Schakel IPv4 of IPv6. Tijdens de openbare IPv4-adressen kunnen worden toegewezen aan verschillende Azure-resources, kunt alleen een openbaar IPv6-adres worden toegewezen aan een internetgerichte load balancer. De load balancer kunt taakverdeling IPv6-verkeer naar virtuele machines van Azure. Meer informatie over [load balancing-IPv6-verkeer naar virtuele machines](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u hebt geselecteerd de **standaard-SKU**, u hebt niet de optie te selecteren *IPv6*. U kunt alleen een IPv4-adres maken bij het gebruik van de **standaard-SKU**.|
    |IP-adrestoewijzing|Ja|**Dynamische:** dynamische adressen worden toegewezen nadat een openbaar IP-adres gekoppeld aan een Azure-resource is en de resource voor de eerste keer wordt gestart. Dynamische adressen kunnen veranderen als ze zijn toegewezen aan een resource, zoals een virtuele machine, en de virtuele machine is gestopt (toewijzing opgeheven), en vervolgens opnieuw gestart. Het adres blijft hetzelfde als een virtuele machine is opnieuw gestart of gestopt (maar niet ongedaan is gemaakt). Dynamische adressen worden vrijgegeven wanneer u een openbare IP-adresresource is ontkoppeld van een resource die is gekoppeld aan. **Statische:** statische adressen worden toegewezen als een openbaar IP-adres wordt gemaakt. Statische adressen worden niet vrijgegeven totdat een openbare IP-adresresource wordt verwijderd. Als het adres niet gekoppeld aan een resource is, kunt u de toewijzingsmethode wijzigen nadat het adres is gemaakt. Als het adres gekoppeld aan een resource is, kunt u mogelijk niet de toewijzingsmethode wijzigen. Als u selecteert *IPv6* voor de **IP-versie**, de toewijzingsmethode is *dynamische*. Als u selecteert *Standard* voor **SKU**, de toewijzingsmethode is *statische*.|
    |Time-out voor inactiviteit (minuten)|Nee|Het aantal minuten dat een TCP- of HTTP-verbinding open zonder afhankelijkheid van clients voor het verzenden van keepalive-berichten. Als u IPv6 voor **IP-versie**, deze waarde kan niet worden gewijzigd. |
    |DNS-naamlabel|Nee|Moet uniek zijn binnen de Azure-locatie die voor het maken van de naam van de in (voor alle abonnementen en alle klanten). Azure wordt automatisch geregistreerd de naam en IP-adres in de DNS, zodat u verbinding met een resource met de naam maken kunt. Azure voegt een standaard-subnet, zoals *location.cloudapp.azure.com* (indien de locatie de locatie die u selecteert is) op de naam u opgeeft, voor het maken van de volledig gekwalificeerde DNS-naam. Als u ervoor kiest om beide versies adres te maken, wordt dezelfde DNS-naam toegewezen aan de IPv4- en IPv6-adressen. Van Azure standaard DNS-records voor zowel IPv4 A en AAAA IPv6-naam bevat en reageert met beide records wanneer de DNS-naam wordt opgezocht. De client kiezen welk adres (IPv4 of IPv6) om te communiceren met. In plaats van of naast het gebruik van het DNS-naamlabel met het standaardachtervoegsel, kunt u de Azure DNS-service gebruiken om een DNS-naam met een aangepast achtervoegsel te configureren dat wordt omgezet naar het openbare IP-adres. Zie [Azure DNS gebruiken met een openbaar IP-adres van Azure](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address) voor meer informatie.|
    |Een IPv6-(of IPv4) adres maken|Nee| Of IPv6- of IPv4 wordt weergegeven is afhankelijk van wat u selecteert voor **IP-versie**. Als u bijvoorbeeld **IPv4** voor **IP-versie**, **IPv6** wordt hier weergegeven. Als u selecteert *Standard* voor **SKU**, u niet de optie voor het maken van een IPv6-adres hebben.
    |Naam (alleen zichtbaar als u dit selectievakje is ingeschakeld de **maken van een IPv6-(of IPv4) adres** selectievakje)|Ja, als u de **maken van een IPv6** (of IPv4) selectievakje.|De naam mag niet anders dan de naam die u invoert voor de eerste **naam** in deze lijst. Als u ervoor kiest om zowel een IPv4- en een IPv6-adres te maken, maakt de portal twee afzonderlijke openbare IP-adres-resources, met elk IP-adres-versie zijn toegewezen.|
    |IP-adrestoewijzing (alleen zichtbaar als u dit selectievakje is ingeschakeld de **maken van een IPv6-(of IPv4) adres** selectievakje)|Ja, als u de **maken van een IPv6** (of IPv4) selectievakje.|Als er op het selectievakje **maken van een IPv4-adres**, kunt u een methode voor het toewijzen. Als er op het selectievakje **maken van een IPv6-adres**, u kunt geen een methode voor het toewijzen, zoals het moet selecteren **dynamische**.|
    |Abonnement|Ja|Moet bestaan in dezelfde [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) als de resource die u wilt koppelen aan het openbare IP-adres.|
    |Resourcegroep|Ja|Kan bestaan in de dezelfde of verschillende, [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) als de resource die u wilt koppelen aan het openbare IP-adres.|
    |Locatie|Ja|Moet bestaan in dezelfde [locatie](https://azure.microsoft.com/regions), ook waarnaar wordt verwezen als de regio als de resource die u wilt koppelen van het openbare IP-adres in.|
    |Beschikbaarheidszone| Nee | Deze instelling wordt alleen weergegeven als u een ondersteunde locatie hebt geselecteerd. Zie voor een lijst van ondersteunde locaties, [overzicht van beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u hebt geselecteerd de **Basic** SKU, *geen* automatisch voor u is geselecteerd. Als u liever een specifieke zone garanderen, kunt u een specifieke zone kunt selecteren. De keuze is geen zone-redundante. Als u hebt geselecteerd de **Standard** SKU: Zone-redundante wordt automatisch geselecteerd voor u en uw gegevenspad flexibele wordt bij een storing van de zone. Als u liever om te waarborgen van een specifieke zone, dit geen bestand is tegen fouten van de zone is, kunt u een specifieke zone kunt selecteren.

**Opdrachten**

Hoewel de portal biedt de mogelijkheid twee openbare IP-adres om resources te maken (één IPv4- en één IPv6), maken de volgende CLI en PowerShell-opdrachten één resource met een adres voor één IP-versie of de andere. Als u wilt dat twee openbare IP-adres-resources, één voor elk IP-versie, moet u de opdracht twee keer uitvoeren andere namen en versies voor het openbare IP-adres-resources op te geven aan te geven.

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)|
|PowerShell|[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Weergeven, instellingen voor wijzigen of verwijderen van een openbaar IP-adres

1. In het vak met de tekst *zoeken naar resources* aan de bovenkant van de Azure-portal, typt u *openbaar ip-adres*. Wanneer **openbare IP-adressen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de naam van het openbare IP-adres dat u wilt weergeven, instellingen voor wijzigen of verwijderen uit de lijst.
3. Voer een van de volgende opties, afhankelijk van of u wilt weergeven, verwijderen of wijzigen van het openbare IP-adres.
    - **Weergave**: de **overzicht** sectie bevat belangrijke instellingen voor het openbare IP-adres, zoals de netwerkinterface die het gekoppeld aan (als het adres gekoppeld aan een netwerkinterface is). De versie van het adres (IPv4 of IPv6) niet wordt weergegeven in de portal. Als u wilt de versie-informatie weergeven, gebruikt u de PowerShell of CLI-opdracht om het openbare IP-adres weer te geven. Als de IP-adresversie IPv6 is, wordt het toegewezen adres wordt niet weergegeven op de portal, PowerShell of de CLI.
    - **Verwijder**: Selecteer wilt verwijderen van het openbare IP-adres **verwijderen** in de **overzicht** sectie. Als het adres momenteel gekoppeld aan een IP-configuratie is, kan niet worden verwijderd. Als het adres momenteel gekoppeld aan een configuratie is, selecteert u **ontkoppelen** ontkoppelen van het adres van de IP-configuratie.
    - **Wijziging**: Selecteer **configuratie**. Instellingen wijzigen met behulp van de informatie in stap 4 van [een openbaar IP-adres maken](#create-a-public-ip-address). Als u wilt de toewijzing voor een IPv4-adres van statisch naar dynamisch wijzigen, moet u eerst de openbare IPv4-adres van de IP-configuratie die het gekoppeld aan ontkoppelen. Vervolgens kunt u de toewijzingsmethode wijzigen naar dynamische en selecteert u een **koppelen** de IP-adres koppelen adres naar dezelfde IP-configuratie, een andere configuratie of u kunt dit laten staan ontkoppeld. Ontkoppelen van een openbaar IP-adres, in de **overzicht** sectie, selecteer **ontkoppelen**.

    >[!WARNING]
    >Als u de toewijzingsmethode van statisch naar dynamisch wijzigen, verliest u de IP-adres dat is toegewezen aan het openbare IP-adres. Terwijl de Azure openbare DNS-servers onderhouden een toewijzing tussen de statische of dynamische adressen en een DNS-naamlabel (als u een gedefinieerd), wordt een dynamisch IP-adres kunt wijzigen wanneer de virtuele machine wordt gestart na een gestopt (toewijzing opgeheven). Als u wilt voorkomen dat het adres wijzigen, een statisch IP-adres toewijzen

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[lijst met AZ network public-IP-](/cli/azure/network/public-ip#az-network-public-ip-list) aan de lijst met openbare IP-adressen, [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) om weer te geven van instellingen. [az network public-ip update](/cli/azure/network/public-ip#az-network-public-ip-update) om bij te werken; [az network public-IP-verwijderen](/cli/azure/network/public-ip#az-network-public-ip-delete) verwijderen|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) een openbaar IP-adresobject ophalen en weergeven van de instellingen [Set-AzureRmPublicIpAddress](/powershell/module/azurerm.network/set-azurermpublicipaddress) instellingen; bijwerken [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) verwijderen|

## <a name="assign-a-public-ip-address"></a>Een openbaar IP-adres toewijzen

Meer informatie over een openbaar IP-adres toewijzen aan de volgende bronnen:

- Een [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM (bij het maken van), of naar een [bestaande virtuele machine](virtual-network-network-interface-addresses.md#add-ip-addresses)
- [Internetgerichte Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Site-naar-site-verbinding met behulp van een Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure virtuele-Machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken op openbare IP-adressen, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rol die is toegewezen de nodige acties die worden vermeld in de volgende tabel:

| Bewerking                                                             | Naam                                                           |
| ---------                                                          | -------------                                                  |
| Microsoft.Network/publicIPAddresses/read                           | Lezen van een openbaar IP-adres                                          |
| Microsoft.Network/publicIPAddresses/write                          | Maken of bijwerken van een openbaar IP-adres                           |
| Microsoft.Network/publicIPAddresses/delete                         | Een openbaar IP-adres verwijderen                                     |
| Microsoft.Network/publicIPAddresses/join/action                    | Een openbare IP-adres aan een resource koppelen                    |

## <a name="next-steps"></a>Volgende stappen

- Maken van een openbare IP-adres met [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor openbare IP-adressen
