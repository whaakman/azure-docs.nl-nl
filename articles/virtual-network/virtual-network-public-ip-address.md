---
title: Maken, wijzigen of verwijderen van een Azure openbare IP-adres | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een openbaar IP-adres.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: e52dc76608a83d446ccc8503d17445a8d6a61ae4
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Maken, wijzigen of verwijderen van een openbaar IP-adres

Meer informatie over een openbaar IP-adres en het maken, wijzigen en verwijderen. Een openbaar IP-adres is een resource met een eigen configureerbare instellingen. Een openbaar IP-adres toewijzen aan andere Azure-resources kunt:
- Verbinding met Internet tot bronnen, zoals virtuele Machines van Azure, Azure virtuele Machine-Schaalsets, Azure VPN-Gateway, Toepassingsgateways en internetgerichte Azure Load Balancers voor binnenkomend verkeer. Azure-resources kunnen niet ontvangen van binnenkomende communicatie van Internet, zonder een toegewezen openbare IP-adres. Hoewel sommige Azure-resources inherent toegankelijk zijn via openbare IP-adressen, moeten de andere bronnen openbare IP-adressen toegewezen aan deze toegankelijk is vanaf het Internet hebben.
- Uitgaande verbinding met Internet via een voorspelbare IP-adres. Een virtuele machine kan bijvoorbeeld communiceren uitgaande met Internet, zonder een openbaar IP-adres toegewezen, maar het adres is vertaald door Azure op een onvoorspelbare openbaar adres netwerkadres. Een openbaar IP-adres toewijzen aan een resource, kunt u weten welk IP-adres wordt gebruikt voor de uitgaande verbinding. Hoewel voorspelbare, kunt het adres wijzigen, afhankelijk van de gekozen toewijzingsmethode. Zie voor meer informatie [maken van een openbaar IP-adres](#create-a-public-ip-address). Voor meer informatie over uitgaande verbindingen van Azure-resources, lees de [begrijpen uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u alle stappen in elke sectie van dit artikel uitvoert:

- Controleer de [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel voor meer informatie over limieten voor het openbare IP-adressen.
- Aanmelden bij de Azure [portal](https://portal.azure.com), Azure-opdrachtregelinterface (CLI) of Azure PowerShell gebruiken met een Azure-account. Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als PowerShell-opdrachten voor het uitvoeren van taken in dit artikel [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure PowerShell-commandlets geïnstalleerd. Typ voor hulp bij PowerShell-opdrachten, met voorbeelden, `get-help <command> -full`.
- Als Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel [installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com).

Openbare IP-adressen hebben nominaal kosten met zich mee. Als de prijzen, lezen de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. 

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *openbaar IP-adres*. Wanneer **openbare IP-adressen** wordt weergegeven in de zoekresultaten, klik erop.
3. Klik op **+ toevoegen** in de **openbaar IP-adres** blade die wordt weergegeven.
4. Typ of Selecteer waarden voor de volgende instellingen in de **openbare IP-adres maken** blade die wordt weergegeven, klikt u vervolgens op **maken**:

    |Instelling|Vereist?|Details|
    |---|---|---|
    |SKU|Ja|Alle openbare IP-adressen die zijn gemaakt voordat de introductie van SKU's zijn **Basic** SKU openbare IP-adressen.  U kunt de SKU niet wijzigen nadat het openbare IP-adres is gemaakt. Een zelfstandige virtuele machine, virtuele machines in een beschikbaarheidsset of virtuele-machineschaalsets kunt gebruiken, basis of standaard SKU's.  De combinatie van SKU's tussen virtuele machines binnen beschikbaarheidssets of schaalsets is niet toegestaan. **Basic** SKU: als u een openbaar IP-adres in een regio die ondersteuning biedt voor beschikbaarheid zones, maakt de **beschikbaarheid zone** is ingesteld op *geen* standaard. U kunt kiezen om te selecteren van een zone beschikbaarheid te garanderen van een specifieke zone voor uw openbare IP-adres. **Standaard** SKU: standaard SKU van een openbaar IP-adres kan worden gekoppeld aan een virtuele machine of een load balancer-front-end. Als u een openbaar IP-adres in een regio die ondersteuning biedt voor beschikbaarheid zones, maakt de **beschikbaarheid zone** is ingesteld op *Zone-redundante* standaard. Zie voor meer informatie over beschikbaarheid zones de **beschikbaarheid zone** instelling. Standaard SKU is vereist als u het adres met een standaard load balancer koppelen. Zie voor meer informatie over standard netwerktaakverdelers, [Azure load balancer standaard SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). De standaard SKU is in de preview-versie. Voordat u maakt een standaard SKU openbare IP-adres, moet u eerst de stappen in uitvoeren [registreren voor de standaard SKU preview](#register-for-the-standard-sku-preview) en maken van het openbare IP-adres in een ondersteunde locatie (regio). Zie voor een lijst van ondersteunde locaties [beschikbaarheid in regio's](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) en controleren van de [Azure Virtual Network updates](https://azure.microsoft.com/updates/?product=virtual-network) pagina voor de ondersteuning van aanvullende regio. Als u een openbaar IP-adres van een standaard-SKU toewijst aan een netwerkinterface van een virtuele machine, moet u het bedoelde verkeer expliciet toestaan met een [netwerkbeveiligingsgroep](security-overview.md#network-security-groups). Communicatie met de resource mislukt totdat u een netwerkbeveiligingsgroep maakt en koppelt en het gewenste verkeer expliciet toestaat.|
    |Naam|Ja|De naam moet uniek zijn binnen de resourcegroep die u selecteert.|
    |IP-versie|Ja| Selecteer IPv4 of IPv6. Tijdens het openbare IPv4-adressen kunnen worden toegewezen aan meerdere Azure-resources, kan een IPv6-openbare IP-adres alleen worden toegewezen aan een internetgerichte load balancer. De load balancer kunt verdelen IPv6-verkeer naar virtuele machines in Azure. Meer informatie over [taakverdeling IPv6-verkeer naar virtuele machines](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u hebt geselecteerd de **standaard SKU**, u hebt niet de optie te selecteren *IPv6*. U kunt alleen een IPv4-adres maken wanneer u de **standaard SKU**.|
    |IP-adrestoewijzing|Ja|**Dynamische:** dynamische adressen zijn toegewezen alleen nadat het openbare IP-adres is gekoppeld aan een netwerkinterface gekoppeld aan een virtuele machine en de virtuele machine voor het eerst wordt gestart. Dynamische adressen kunnen wijzigen als de netwerkinterface is gekoppeld aan virtuele machine is gestopt (toewijzing ongedaan gemaakt). Het adres blijft hetzelfde als de virtuele machine is opnieuw gestart of gestopt (maar niet de toewijzing ongedaan gemaakt). **Statische:** statische adressen zijn toegewezen wanneer het openbare IP-adres wordt gemaakt. Statische adressen komen niet wijzigen, zelfs als de virtuele machine wordt geplaatst gestopt (toewijzing ongedaan gemaakt). Het adres wordt alleen vrijgegeven wanneer de netwerkinterface wordt verwijderd. U kunt de toewijzingsmethode wijzigen nadat de netwerkinterface is gemaakt. Als u selecteert *IPv6* voor de **IP-versie**, de toewijzingsmethode is *dynamische*. Als u selecteert *standaard* voor **SKU**, de toewijzingsmethode is *statische*.|
    |Time-out voor inactiviteit (minuten)|Nee|Het aantal minuten dat een TCP- of HTTP-verbinding open zonder afhankelijk te zijn op clients om keepalive-berichten te verzenden. Als u IPv6 voor selecteert **IP-versie**, deze waarde kan niet worden gewijzigd. |
    |Label van DNS-naam|Nee|Moet uniek zijn binnen de Azure-locatie die voor het maken van de naam van de in (voor alle abonnementen en alle klanten). Azure automatisch geregistreerd naam en IP-adres in de DNS zodat u verbinding met een resource met de naam maken kunt. Azure voegt een standaard-subnet, zoals *location.cloudapp.azure.com* (indien de locatie is de locatie die u selecteert) om de naam een, voor het maken van de volledig gekwalificeerde DNS-naam. Als u maken van beide versies van het adres wilt, wordt dezelfde DNS-naam toegewezen aan de IPv4- en IPv6-adressen. Azure standaard DNS-records voor zowel IPv4 A en AAAA IPv6-naam bevat en reageert met beide records wanneer de DNS-naam wordt opgezocht. De client kiezen welk adres (IPv4 of IPv6) om te communiceren met. In plaats van of naast, kunt met behulp van het label van DNS-naam met het standaardachtervoegsel u de Azure DNS-service voor het configureren van een DNS-naam met een aangepaste achtervoegsel dat wordt omgezet naar het openbare IP-adres. Zie voor meer informatie [gebruik Azure DNS met een Azure openbaar IP-adres](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
    |Een IPv6-(of IPv4)-adres maken|Nee| Of IPv6- of IPv4 wordt weergegeven is afhankelijk van wat u selecteren voor **IP-versie**. Als u bijvoorbeeld **IPv4** voor **IP-versie**, **IPv6** hier weergegeven. Als u selecteert *standaard* voor **SKU**, hebt u niet de optie voor het maken van een IPv6-adres.
    |De naam (alleen zichtbaar als u dit selectievakje inschakelt de **een adres IPv6 (of IPv4) maakt** selectievakje)|Ja, als u selecteert de **maken van een IPv6** (of IPv4) selectievakje.|De naam moet anders zijn dan de naam die u opgeeft voor de eerste **naam** in deze lijst. Als u ervoor kiest om zowel een IPv4- en een IPv6-adres te maken, maakt de portal twee afzonderlijke openbare IP-adresbronnen, één met elk IP-adresversie toegewezen.|
    |IP-adrestoewijzing (alleen zichtbaar als u dit selectievakje inschakelt de **een adres IPv6 (of IPv4) maakt** selectievakje)|Ja, als u selecteert de **maken van een IPv6** (of IPv4) selectievakje.|Als er op het selectievakje **maken van een IPv4-adres**, kunt u een methode voor het toewijzen. Als er op het selectievakje **maken van een IPv6-adres**, u kunt geen een methode voor het toewijzen, zoals het moet selecteren **dynamische**.|
    |Abonnement|Ja|Moet aanwezig zijn in dezelfde [abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) als de bron die u wilt koppelen van het openbare IP-adres.|
    |Resourcegroep|Ja|Kan bestaan in de dezelfde of verschillende, [resourcegroep](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) als de bron die u wilt koppelen van het openbare IP-adres.|
    |Locatie|Ja|Moet aanwezig zijn in dezelfde [locatie](https://azure.microsoft.com/regions)wordt ook verwezen als regio, als de bron die u wilt koppelen van het openbare IP-adres in.|
    |Beschikbaarheid zone| Nee | Deze instelling wordt alleen weergegeven als u een ondersteunde locatie selecteren. Zie voor een lijst van ondersteunde locaties [beschikbaarheid zones overzicht](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Beschikbaarheid zones zijn momenteel in preview-versie. Voordat u selecteert een zone of zone-redundante optie, moet u eerst de stappen in uitvoeren [registreren voor de beschikbaarheid van zones preview](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview). Als u hebt geselecteerd de **Basic** SKU, *geen* automatisch voor u is geselecteerd. Als u liever een specifieke zone te garanderen, kunt u een specifieke zone kunt selecteren. De keuze is geen zone-redundante. Als u hebt geselecteerd de **standaard** SKU: Zone-redundante wordt automatisch geselecteerd voor u en uw gegevenspad robuuste maakt tot het mislukken van de zone. Als u liever gegarandeerd dat een specifieke zone die geen robuuste tot het mislukken van de zone, kunt u een specifieke zone kunt selecteren.
  

**Opdrachten**

Hoewel de portal biedt de optie voor het maken van twee openbare IP-adresbronnen (één IPv4- en één IPv6), maken de volgende CLI en PowerShell-opdrachten één resource met een adres voor één IP-versie of de andere. Als u wilt dat twee openbare IP-adresbronnen, één voor elk IP-versie moet u de opdracht twee keer uitvoeren verschillende namen en versies voor het openbare IP-adresbronnen geven aan te geven. 

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ netwerk openbare ip-maken](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nieuwe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Weergeven, instellingen voor wijzigen of verwijderen van een openbaar IP-adres

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een account is toegewezen (minimaal) machtigingen voor de rol Inzender netwerk voor uw abonnement. Lees de [ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel voor meer informatie over het toewijzen van rollen en machtigingen aan accounts.
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *openbaar IP-adres*. Wanneer **openbare IP-adressen** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **openbare IP-adressen** blade die wordt weergegeven, klikt u op de naam van het openbare IP-adres dat u wilt weergeven, instellingen voor wijzigen of verwijderen.
4. In de blade die wordt weergegeven voor het openbare IP-adres, voert u een van de volgende opties, afhankelijk van of u wilt weergeven, verwijderen of wijzigen van het openbare IP-adres.
    - **Weergave**: de **overzicht** sectie van de blade wordt registersleutelinstellingen voor het openbare IP-adres, zoals de netwerkinterface die deze gekoppeld (als het adres gekoppeld aan een netwerkinterface is). De versie van het adres (IPv4 of IPv6) niet wordt weergegeven in de portal. Als u wilt de versie-informatie weergeven, gebruikt u de opdracht PowerShell of CLI om het openbare IP-adres weer te geven. Als de IP-adresversie IPv6 is, wordt het toegewezen adres wordt niet weergegeven door de portal, PowerShell of de CLI. 
    - **Verwijder**: voor het verwijderen van het openbare IP-adres, klikt u op **verwijderen** in de **overzicht** sectie van de blade. Als het adres momenteel gekoppeld aan een IP-configuratie is, kan niet worden verwijderd. Als het adres momenteel gekoppeld aan een configuratie is, klikt u op **Dissociate** ontkoppelen van het adres van de IP-configuratie.
    - **Wijziging**: klik op **configuratie**. Instellingen wijzigen met behulp van de informatie in stap 4 van de [maken van een openbaar IP-adres](#create-a-public-ip-address) sectie van dit artikel. Als u wilt de toewijzing voor een IPv4-adres van statisch naar dynamisch wijzigt, moet u eerst ontkoppelen van het openbare IPv4-adres van de IP-adresconfiguratie die deze gekoppeld. U kunt dynamisch de toewijzingsmethode wijzigen en klik op **koppelen** om te koppelen van het IP-adres adres naar dezelfde IP-configuratie of een andere configuratie kunt laten ontkoppeld. Ontkoppelen van een openbaar IP-adres de **overzicht** sectie, klikt u op **Dissociate**.

>[!WARNING]
>Als u de methode voor het toewijzen van statisch naar dynamisch wijzigen, verliest u de IP-adres dat is toegewezen aan het openbare IP-adres. Terwijl de Azure openbare DNS-servers onderhouden van een toewijzing tussen statische of dynamische adressen en elk label DNS-naam (als u een gedefinieerd), wordt een dynamisch IP-adres kunt wijzigen wanneer de virtuele machine wordt gestart nadat wordt gestopt (toewijzing ongedaan gemaakt). Als u wilt voorkomen dat het adres wijzigen, een statisch IP-adres toewijzen

**Opdrachten**

|Hulpprogramma|Opdracht|
|---|---|
|CLI|[AZ lijst van een openbaar ip-netwerk](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) aan de lijst met openbare IP-adressen, [az netwerk openbare-ip-weergeven](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) om weer te geven van instellingen. [az netwerk openbare ip-update](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) bijwerken; [az netwerk openbare ip-verwijderen](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) verwijderen|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) ophalen van een openbare IP-adres-object en de instellingen bekijken [Set AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) instellingen; bijwerken [Verwijderen AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) verwijderen|

## <a name="register-for-the-standard-sku-preview"></a>Registreren voor de standaard SKU-preview

> [!NOTE]
> Functies in de preview-versie kunnen niet dezelfde mate van beschikbaarheid en betrouwbaarheid hebben, zoals functies die in het algemeen beschikbaarheid release. Preview-functies worden niet ondersteund, kunnen hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. 

Voordat u een standaard SKU openbare IP-adres maken kunt, moet u eerst registreren voor de preview. Voer de volgende stappen uit om te registreren voor de preview:

1. Installeren en configureren van Azure [PowerShell](/powershell/azure/install-azurerm-ps).
2. Voer de `Get-Module -ListAvailable AzureRM` opdracht om te zien welke versie van de module AzureRM die u hebt geïnstalleerd. U moet beschikken over versie 4.4.0 of hoger is geïnstalleerd. Als u dit niet doet, kunt u installeren de nieuwste versie van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM).
3. Aanmelden bij Azure met de `login-azurermaccount` opdracht.
4. Voer de volgende opdracht om te registreren voor de preview:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

5. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Volgende stappen
Openbare IP-adressen toewijzen bij het maken van de volgende Azure-resources:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machines
- [Internetgerichte Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Site-naar-site-verbinding met een Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure virtuele-Machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
