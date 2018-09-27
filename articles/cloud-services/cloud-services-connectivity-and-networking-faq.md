---
title: Problemen met connectiviteit en netwerken voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: In dit artikel geeft een lijst van de veelgestelde vragen over connectiviteit en netwerken voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: genli
ms.openlocfilehash: 034d59c39628a08c389c5ceb67c5872bbea10d59
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223165"
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemen met connectiviteit en netwerken voor Azure Cloud Services: veelgestelde vragen

Dit artikel bevat veelgestelde vragen over problemen met connectiviteit en netwerken voor [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Zie voor informatie over de grootte, de [Cloud Services-VM-grootte pagina](cloud-services-sizes-specs.md).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ik kan niet een IP-adres in een multi-VIP-cloudservice reserveren.
Controleer eerst of de virtuele machine-instantie die u probeert te reserveren voor het IP-adres is ingeschakeld. Controleer vervolgens of u gereserveerde IP-adressen gebruiken voor zowel de fasering en productie-implementaties. *Geen* de instellingen niet wijzigen terwijl de implementatie wordt bijgewerkt.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>Hoe gebruik ik extern bureaublad wanneer ik een NSG hebben?
Regels toevoegen aan de NSG die verkeer via poorten **3389** en **20000**. Extern bureaublad maakt gebruik van poort **3389**. Cloud service-exemplaren worden verdeeld, zodat u kunt niet rechtstreeks welk exemplaar verbinding maken bepalen met. De *RemoteForwarder* en *RemoteAccess* agents beheren van Remote Desktop Protocol (RDP)-verkeer en dat de client voor het verzenden van een cookie RDP en verbinding maken met een afzonderlijke-exemplaar opgeven. De *RemoteForwarder* en *RemoteAccess* agents vereisen poort **20000** als u wilt openen, die mogelijk worden geblokkeerd als er een NSG.

## <a name="can-i-ping-a-cloud-service"></a>Kan ik pingen met een cloudservice?

Nee, niet met behulp van de normale 'pingen' / ICMP-protocol. Het ICMP-protocol is niet toegestaan via de Azure load balancer.

Connectiviteit wilt testen, is het raadzaam dat u een poortping doen. Terwijl Ping.exe maakt gebruik van ICMP, kunt u andere hulpprogramma's, zoals PSPing Nmap en Telnet-, test de verbinding met een specifieke TCP-poort.

Zie voor meer informatie, [poort pings gebruiken in plaats van ICMP Azure VM-connectiviteit testen](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Hoe voorkom ik ontvangen duizenden treffers van onbekende IP-adressen die op een kwaadwillige aanval in de cloud-service wijzen kunnen?
Implementatie van een gelaagde netwerkbeveiliging ter bescherming van de platform-services tegen aanvallen van gedistribueerde denial-of-service (DDoS) in Azure. Het Azure DDoS-bescherming-systeem maakt deel uit van Azure van continue bewakingsproces, die voortdurend wordt verbeterd via indringingstests. Dit systeem DDoS-bescherming is ontworpen om niet alleen aanvallen van buitenaf, maar ook van andere tenants Azure beschikken. Zie voor meer informatie, [Azure-netwerkbeveiliging](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

U kunt ook een opstarttaak voor het selectief blokkeren van bepaalde specifieke IP-adressen maken. Zie voor meer informatie, [blokkeren van een specifiek IP-adres](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Wanneer ik RDP naar Mijn cloud service-exemplaar, krijg het bericht 'het gebruikersaccount dat is verlopen'.
U kunt het foutbericht 'dit gebruikersaccount is verlopen' krijgen wanneer u de vervaldatum die is geconfigureerd in uw RDP-instellingen overslaan. U kunt de vervaldatum van de portal wijzigen door de volgende stappen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), gaat u naar uw cloudservice en selecteert u de **extern bureaublad** tabblad.

2. Selecteer de **productie** of **fasering** implementatiesite.

3. Wijzig de **verloopt op** datum, en vervolgens de configuratie op te slaan.

Nu moet u u RDP naar uw computer.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Waarom is Azure Load Balancer verkeer niet gelijkmatig?
Zie voor meer informatie over de werking van een interne load balancer [distributiemodus voor de Azure Load Balancer nieuwe](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

De distributie-algoritme dat wordt gebruikt, is een 5-tuple (bron-IP, bronpoort, doel-IP, doelpoort en protocoltype) hash om toe te wijzen verkeer servers beschikbaar zijn. Het biedt persistentie alleen binnen een transportsessie. Pakketten in de dezelfde TCP of UDP-sessie worden omgeleid naar hetzelfde datacenter-IP (DIP) exemplaar achter de load balancing-eindpunt. Wanneer de client wordt gesloten en opent u de verbinding opnieuw of een nieuwe sessie van de dezelfde bron-IP start, wordt de bron-poort gewijzigd en zorgt ervoor dat het verkeer naar een ander DIP-eindpunt.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Hoe kan ik het inkomende verkeer omleiden naar de standaard-URL van de service in de cloud naar een aangepaste URL? 

De module herschrijven van URL's van IIS kan worden gebruikt voor het omleiden van verkeer dat wordt geleverd aan de standaard-URL voor de cloudservice (bijvoorbeeld \*. cloudapp.net) naar een aangepaste naam/URL. Omdat de module voor het herschrijven van URL's is standaard ingeschakeld op webrollen en de regels zijn geconfigureerd in web.config van de toepassing, is het altijd beschikbaar zijn op de virtuele machine, ongeacht de opnieuw opstarten/reimages. Zie voor meer informatie:

- [Herschrijvingsregels voor de module voor het herschrijven van URL's maken](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Een standaardkoppeling verwijderen](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Hoe kan ik blok-of uitschakelen inkomend verkeer naar de standaard-URL van mijn cloudservice? 

U kunt voorkomen dat inkomend verkeer naar de standaard-URL/naam van uw cloudservice (bijvoorbeeld \*. cloudapp.net). Stel de host-header in een aangepaste DNS-naam (bijvoorbeeld www.MyCloudService.com) onder configuratie van de binding site in de cloud (*.csdef) servicedefinitiebestand, zoals wordt aangegeven: 
 

    <?xml version="1.0" encoding="utf-8"?> 
    <ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6"> 
      <WebRole name="MyWebRole" vmsize="Small"> 
        <Sites> 
          <Site name="Web"> 
            <Bindings> 
              <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" /> 
            </Bindings> 
          </Site> 
        </Sites> 
        <Endpoints> 
          <InputEndpoint name="Endpoint1" protocol="http" port="80" /> 
        </Endpoints> 
        <ConfigurationSettings> 
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" /> 
        </ConfigurationSettings> 
      </WebRole> 
    </ServiceDefinition> 
 
Omdat deze host-header-binding wordt afgedwongen door het csdef-bestand, is de service toegankelijk alleen via de aangepaste naam "www.MyCloudService.com." Alle binnenkomende aanvragen naar de "*. cloudapp.net" domein altijd mislukken. Als u een aangepaste SLB test of een interne load balancer in de service, blokkeren van de standaard-URL/naam van de service kan leiden tot problemen met het testinterval gedrag. 

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Hoe kan ik Zorg ervoor dat het openbare IP-adres van een service in de cloud nooit gewijzigd?

Als u wilt controleren of dat het openbare IP-adres van uw cloudservice (ook wel bekend als een VIP) nooit gewijzigd, zodat deze gewoonlijk in de whitelist opgenomen door een paar specifieke clients worden kan, is het raadzaam dat u een gereserveerd IP-adres dat is gekoppeld aan deze hebt. Anders wordt het virtuele IP-adres verstrekt door Azure toewijzing ongedaan wordt gemaakt van uw abonnement als u de implementatie verwijderen. Voor succesvolle wisselen van VIP moet u afzonderlijke gereserveerde IP-adressen voor productie- als stagingsleuven. Zonder dat ze mislukt de wisselbewerking opnieuw uit. Een IP-adres reserveren en deze koppelen aan uw cloudservice, Zie de volgende artikelen:
 
- [De IP-adres van een bestaande cloudservice reserveren](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Een gereserveerde IP-adres aan een cloudservice koppelen met behulp van een service-configuratiebestand](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Als u meer dan één exemplaar voor uw functies hebben, RIP koppelen aan uw cloudservice mag niet leiden tot uitvaltijd. U kunt ook de lijst met toegestane adressen de IP-adresbereik van uw Azure-datacenter. U vindt alle Azure-IP-adresbereiken op de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Dit bestand bevat de IP-adresbereiken (inclusief compute, SQL en storage) die wordt gebruikt in Azure-datacenters. Een bijgewerkt bestand wordt wekelijks die overeenkomt met de huidige geïmplementeerde bereiken en eventuele toekomstige wijzigingen in de IP-adresbereiken worden geplaatst. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacenters voor ten minste één week. Elke week van het nieuwe XML-bestand downloaden en uitvoeren van de noodzakelijke wijzigingen op uw site naar de services die worden uitgevoerd in Azure correct kan worden geïdentificeerd. Azure ExpressRoute-gebruikers mogelijk Houd er rekening mee dat dit bestand moet worden gebruikt voor het bijwerken van de BGP-aankondiging van Azure-ruimte in de eerste week van elke maand. 

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Hoe kan ik virtuele netwerken van Azure Resource Manager gebruiken met cloudservices? 

Cloudservices kunnen niet worden geplaatst in virtuele netwerken van Azure Resource Manager. Virtuele netwerken van Resource Manager en klassieke implementatie virtuele netwerken kunnen worden verbonden via peering. Zie voor meer informatie, [peering van virtuele netwerken](../virtual-network/virtual-network-peering-overview.md).


## <a name="how-can-i-get-the-list-of-public-ips-used-by-my-cloud-services"></a>Hoe vind ik de lijst met openbare IP-adressen die worden gebruikt door mijn Cloud Services?

U kunt na de PS-script gebruiken om op te halen van de lijst met openbare IP-adressen voor Cloud Services in uw abonnement

    $services = Get-AzureService  | Group-Object -Property ServiceName

    foreach ($service in $services) 
    {
        "Cloud Service '$($service.Name)'"

        $deployment = Get-AzureDeployment -ServiceName $service.Name 
        "VIP - " +  $deployment.VirtualIPs[0].Address
        "================================="
    }
