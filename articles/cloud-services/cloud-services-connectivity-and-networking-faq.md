---
title: Problemen met de netwerken en voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: Dit artikel worden de veelgestelde vragen over de connectiviteit en netwerken voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: genli
ms.openlocfilehash: e89549f51abb896c1ddf48a46de78fb5e4988f22
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemen met de netwerken en voor Azure Cloud Services: veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over problemen met de netwerken en voor [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Zie voor informatie over de grootte de [Cloud Services-VM-grootte pagina](cloud-services-sizes-specs.md).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ik kan niet reserveren, een IP-adres in een cloudservice meerdere VIP's.
Controleer eerst of de instantie van de virtuele machine die u probeert te reserveren van de IP voor is ingeschakeld. Controleer vervolgens of u gereserveerde IP's gebruiken voor het Faseren en de productie-implementaties. *Geen* de instellingen niet wijzigen terwijl het upgraden van de implementatie.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>Hoe kan ik extern bureaublad gebruiken wanneer ik een NSG hebben?
Regels toevoegen aan het NSG waarmee verkeer op poorten **3389** en **20000**. Extern bureaublad maakt gebruik van poort **3389**. Cloud service-exemplaren worden verdeeld, zodat u direct welk exemplaar verbinding maken met kan niet bepalen. De *RemoteForwarder* en *RemoteAccess* agents Remote Desktop Protocol (RDP)-verkeer beheren en dat de client verzenden van een cookie RDP en geeft u een afzonderlijk exemplaar verbinding maken met. De *RemoteForwarder* en *RemoteAccess* agents vereisen poort **20000** om u te openen, die mogelijk geblokkeerd als er een NSG.

## <a name="can-i-ping-a-cloud-service"></a>Kan ik een cloudservice pingen?

Nee, niet met behulp van de normale 'pingen' / ICMP-protocol. Het ICMP-protocol is niet toegestaan via de Azure load balancer.

Connectiviteit testen raden we u een poortping. Terwijl Ping.exe ICMP gebruikt, kunt u andere hulpprogramma's, zoals psping om Nmap en telnet, voor het testen van verbinding met een specifieke TCP-poort.

Zie voor meer informatie [poort pings gebruiken in plaats van ICMP Azure VM-connectiviteit testen](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Hoe voorkom ik dat ontvangst duizenden treffers van onbekende IP-adressen die kunnen wijzen op een kwaadwillige aanval naar de cloudservice?
Azure implementeert een gelaagde netwerkbeveiliging om te beschermen tegen aanvallen van gedistribueerde denial-of-service (DDoS) de platform-services. Het systeem Azure DDoS-beveiliging is onderdeel van Azure continue bewaking die voortdurend wordt verbeterd via het binnendringen testen. Dit systeem DDoS-bescherming is ontworpen om niet alleen aanvallen van buitenaf, maar ook van andere tenants Azure tolereren. Zie voor meer informatie [Azure netwerkbeveiliging](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

U kunt ook een starten van de taak voor het selectief blokkeren enkele specifieke IP-adressen maken. Zie voor meer informatie [blokkeren van een specifiek IP-adres](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Als ik wil voor RDP mijn cloud service-exemplaar, krijgt het bericht "het gebruikersaccount is verlopen."
U kunt het foutbericht 'dit gebruikersaccount is verlopen' wanneer u de vervaldatum die is geconfigureerd in uw RDP-instellingen overslaan. U kunt de vervaldatum wijzigen via de portal met de volgende stappen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), gaat u naar de cloudservice en selecteert u de **extern bureaublad** tabblad.

2. Selecteer de **productie** of **fasering** implementatiesleuf.

3. Wijzig de **verloopt op** date en sla de configuratie.

Nu moet u kunnen voor RDP naar uw computer.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Waarom is Azure Load Balancer niet balancing verkeer gelijkmatig?
Zie voor meer informatie over de werking van een interne load balancer [Azure Load Balancer nieuwe distributie modus](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

De verdeling algoritme is een 5-tuple (bron-IP, bronpoort, doel-IP, doelpoort en protocoltype) hash verkeer toewijzen aan beschikbare servers. Het biedt gebruikerspad alleen een transportsessie. Pakketten in dezelfde TCP of UDP-sessie worden omgeleid naar de hetzelfde exemplaar van datacenter IP (DIP) achter het eindpunt met gelijke taakverdeling. Wanneer de client wordt gesloten, opent u de verbinding opnieuw of een nieuwe sessie vanaf dezelfde bron-IP start, wordt de bronpoort wijzigt en zorgt ervoor dat het verkeer naar een ander DIP-eindpunt.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Hoe kan ik binnenkomend verkeer naar de standaard-URL van mijn cloudservice naar een aangepaste URL leiden? 

De module herschrijven van URL's van IIS kan worden gebruikt voor het omleiden van verkeer dat wordt toegevoegd aan de standaard-URL voor de cloudservice (bijvoorbeeld \*. cloudapp.net) naar een aangepaste naam/URL. Omdat de module voor het herschrijven van URL's is standaard ingeschakeld op de web-rollen en de regels zijn geconfigureerd in het bestand web.config van de toepassing, is het altijd beschikbaar zijn op de virtuele machine ongeacht herstarts/reimages. Zie voor meer informatie:

- [Herschrijf regels voor de module voor het herschrijven van URL's maken](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Een standaardkoppeling verwijderen](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Hoe kan ik blok-of uitschakelen inkomend verkeer naar de standaard-URL van de cloudservice? 

U kunt voorkomen dat binnenkomend verkeer naar de standaard URL-/ naam van uw cloudservice (bijvoorbeeld \*. cloudapp.net). Stel de host-header voor een aangepaste DNS-naam (bijvoorbeeld www.MyCloudService.com) onder configuratie van de binding site in de cloud (*.csdef) servicedefinitiebestand, zoals aangegeven: 
 

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
 
Omdat deze host-header-binding wordt afgedwongen door het csdef-bestand, is de service toegankelijk alleen via de aangepaste naam 'www.MyCloudService.com'. Alle inkomende aanvragen op de ' *. cloudapp.net ' domein altijd mislukken. Als u een aangepaste SLB test of een interne load balancer in de service gebruiken, de standaard blokkeren URL/naam van de service mogelijk conflicteren met het Zoek gedrag. 

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Hoe kan ik ervoor zorgen dat het openbare IP-adres van een cloudservice nooit verandert?

Als u wilt controleren of dat het openbare IP-adres van uw cloudservice (VIP) nooit gewijzigd zodat deze gewoonlijk goedgekeurde lijst door enkele specifieke clients worden kan, is het raadzaam dat u een gereserveerd IP-adres zijn gekoppeld hebt. Anders wordt het virtuele IP-adres opgegeven door Azure toewijzing ongedaan wordt gemaakt van uw abonnement als u de implementatie verwijdert. U moet afzonderlijke gereserveerde IP's voor productie- en staging-sleuven voor geslaagde wisselen van VIP. Zonder deze mislukt voor de wisselbewerking. Een IP-adres reserveren en deze koppelen aan uw cloudservice, raadpleegt deze artikelen:
 
- [Het IP-adres van een bestaande cloudservice reserveren](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Koppelen van een gereserveerd IP-adres op een cloudservice met behulp van een service-configuratiebestand](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Als u meer dan één exemplaar voor uw rollen, RIP koppelen aan uw cloudservice mag niet leiden tot uitvaltijd. U kunt ook het IP-adresbereik van uw Azure-datacenter geaccepteerde. U vindt alle Azure-IP-adresbereiken op de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Dit bestand bevat de IP-adresbereiken (inclusief adresbereiken compute, SQL en opslag) gebruikt in Azure-datacenters. Een bijgewerkt bestand geboekt wekelijks die overeenkomt met de huidige geïmplementeerde bereiken en toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacentra voor ten minste één week. Elke week voor de nieuwe XML-bestand downloaden en uitvoeren van de benodigde wijzigingen op de site juist om services te identificeren in Azure wordt uitgevoerd. Azure ExpressRoute-gebruikers mogelijk Houd er rekening mee dat dit bestand moet worden gebruikt voor het bijwerken van de BGP-aankondiging van Azure ruimte in de eerste week van elke maand. 

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Hoe kan ik virtuele netwerken van Azure Resource Manager gebruiken met cloudservices? 

Cloud-services kunnen niet worden geplaatst in virtuele netwerken van Azure Resource Manager. Resource Manager virtuele netwerken en klassieke implementatie virtuele netwerken kunnen worden verbonden via peering. Zie voor meer informatie [virtuele netwerk peering](../virtual-network/virtual-network-peering-overview.md).
