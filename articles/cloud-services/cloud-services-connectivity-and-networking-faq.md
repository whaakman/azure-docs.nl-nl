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
ms.openlocfilehash: 7b435b6904b05228a63e3ed3a9fed78747b843c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemen met de netwerken en voor Azure Cloud Services: veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over problemen met de netwerken en voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ik kan niet worden gereserveerd een IP-adres in een cloudservice meerdere VIP 's
Controleer eerst of de instantie van de virtuele machine die u probeert te reserveren van de IP voor is ingeschakeld. Controleer vervolgens of dat u gereserveerde IP's voor het Faseren en de productie-implementaties gebruikt. **Geen** de instellingen niet wijzigen terwijl het upgraden van de implementatie.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hoe kan ik extern bureaublad wanneer ik een NSG hebben?
Regels toevoegen aan het NSG waarmee verkeer op poorten **3389** en **20000**.  Extern bureaublad maakt gebruik van poort **3389**.  Cloud Service-exemplaren worden verdeeld, zodat u direct welk exemplaar verbinding maken met kan niet bepalen.  De *RemoteForwarder* en *RemoteAccess* agents RDP-verkeer te beheren en dat de client verzenden van een cookie RDP en geeft u een afzonderlijk exemplaar verbinding maken met.  De *RemoteForwarder* en *RemoteAccess* agents vereisen die poort **20000** worden geopend die worden geblokkeerd als er een NSG.

## <a name="can-i-ping-a-cloud-service"></a>Kan ik een cloudservice pingen?

Nee, niet met behulp van de normale 'pingen' / ICMP-protocol. Het ICMP-protocol is niet toegestaan via de Azure load balancer.

Connectiviteit testen raden we u een poortping. Terwijl Ping.exe ICMP gebruikt, wordt er andere hulpprogramma's, zoals psping om Nmap en telnet kunnen u voor het testen van verbinding met een specifieke TCP-poort.

Zie voor meer informatie [poort pings gebruiken in plaats van ICMP Azure VM-connectiviteit testen](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Hoe voorkom ik dat ontvangst duizenden treffers van onbekende IP-adressen die wijzen op enige vorm van kwaadaardige aanvallen met de cloudservice?
Azure implementeert een gelaagde netwerkbeveiliging om te beschermen tegen aanvallen van gedistribueerde denial-of-service (DDoS) de platform-services. Het systeem Azure DDoS-beveiliging is onderdeel van Azure continue bewaking die voortdurend wordt verbeterd via het binnendringen testen. Dit systeem DDoS-bescherming is ontworpen om niet alleen aanvallen van buitenaf, maar ook van andere tenants Azure tolereren. Zie voor meer details [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

U kunt ook een starten van de taak voor het selectief blokkeren enkele specifieke IP-adressen maken. Zie voor meer informatie [blokkeren van een specifiek IP-adres](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Wanneer ik wil voor RDP mijn cloud service-exemplaar, het bericht wordt weergegeven, "de gebruikersaccount is verlopen."
Mogelijk dat u de foutmelding 'dit gebruikersaccount is verlopen' wanneer u de vervaldatum die is geconfigureerd in uw RDP-instellingen overslaan. U kunt de vervaldatum wijzigen via de portal met de volgende stappen:
1. Aanmelden bij de Azure Management Console (https://manage.windowsazure.com), gaat u naar de cloudservice en selecteer de **configureren** tabblad.
2. Selecteer **externe**.
3. De datum 'Verloopt op' wijzigen en vervolgens de configuratie op te slaan.

Nu moet u kunnen voor RDP naar uw computer.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Waarom is Load Balancer niet balancing verkeer gelijkmatig?
Zie voor meer informatie over hoe interne load balancer werkt [Azure Load Balancer nieuwe distributie modus](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

De verdeling algoritme is een 5-tuple (bron-IP, bronpoort, doel-IP, doelpoort protocoltype) hash verkeer toewijzen aan beschikbare servers. Het biedt gebruikerspad alleen een transportsessie. Pakketten in dezelfde TCP of UDP-sessie wordt omgeleid naar hetzelfde datacenter IP (DIP) exemplaar achter het eindpunt van de taakverdeling. Wanneer de client wordt gesloten en opnieuw de verbinding wordt geopend, of een nieuwe sessie vanaf dezelfde bron-IP begint, wordt de bronpoort wijzigt en zorgt ervoor dat het verkeer naar een ander DIP-eindpunt.

## <a name="how-can-i-redirect-the-incoming-traffic-to-my-default-url-of-cloud-service-to-a-custom-url"></a>Hoe kan ik het binnenkomende verkeer omleiden naar mijn standaard-URL van de Cloudservice om een aangepaste URL op? 

De URL herschrijven Module van IIS kunnen worden gebruikt voor het omleiden van verkeer dat afkomstig is van de standaard-URL voor de cloudservice (bijvoorbeeld \*. cloudapp.net) naar een aantal aangepaste DNS-naam/URL. Omdat de Module voor het herschrijven van URL standaard ingeschakeld op de Web-rollen is en de regels zijn geconfigureerd in het bestand web.config van de toepassing is, zou het altijd beschikbaar zijn op de virtuele machine ongeacht herstarts/reimages zijn. Zie voor meer informatie:

- [Herschrijf de maken herschrijven regels voor URL van de Module](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Standaardkoppeling naar verwijderen](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-the-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Hoe kan ik blok-of uitschakelen het binnenkomende verkeer naar de standaard-URL van de Cloudservice? 

Het binnenkomende verkeer mogelijk niet de standaard URL-/ naam van uw cloudservice (bijvoorbeeld \*. cloudapp.net) door de host-header in te stellen op een aangepaste DNS-naam (bijvoorbeeld www. MyCloudService.com) onder configuratie van de site-binding in de cloud (*.csdef) servicedefinitiebestand zoals hieronder wordt beschreven: 
 

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
 
Als u deze binding host-header wordt afgedwongen via csdef-bestand, de service zou alleen toegankelijk zijn via de aangepaste naam 'www.MyCloudService.com', dat alle inkomende naar aanvragen de ' *. cloudapp.net' domein altijd mislukken. Standaard-URL/naam van de service blokkeren dat die wordt aangegeven als u echter een aangepaste SLB test of een interne load balancer in de service, het Zoek gedrag kan verstoren. 

## <a name="how-to-make-sure-the-public-facing-ip-address-of-a-cloud-service-aka-vip-never-changes-so-that-it-could-be-customarily-whitelisted-by-few-specific-clients"></a>Hoe zorg ervoor dat het openbare internetgerichte IP-adres van een Cloudservice (aka, VIP) nooit gewijzigd zodat deze gewoonlijk goedgekeurde lijst door enkele specifieke clients worden kan?

Door het IP-adres van uw Cloud-Services, wordt aangeraden dat u een gereserveerd IP-adres die zijn gekoppeld aan deze anders het virtuele IP-adres opgegeven door Azure ongedaan uit uw abonnement hebt als u de implementatie verwijdert. Houd er rekening mee dat voor een geslaagde wisselen van VIP moet u afzonderlijke gereserveerde IP's voor productie- en Staging-sleuven in de afwezigheid van welke wisseling bewerking mislukt. Volg deze artikelen voor het reserveren van een IP-adres en deze koppelen aan uw Cloudservices:  
 
- [Het IP-adres van een bestaande cloudservice reserveren](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Koppelen van een gereserveerd IP-adres op een cloudservice met behulp van een service-configuratiebestand](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Zolang er meer dan één exemplaar voor uw rollen, RIP koppelen aan uw Cloudservice mag niet leiden tot uitvaltijd. U kunt ook het IP-adresbereik van uw Azure-Datacenter geaccepteerde. U vindt alle Azure-IP-adresbereiken [hier](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Dit bestand bevat de IP-adresbereiken (inclusief bereiken voor Compute, SQL en Storage) die worden gebruikt in de Microsoft Azure-datacenters. Er wordt wekelijks een bijgewerkt bestand geplaatst waarin staat welke bereiken momenteel zijn geïmplementeerd en welke wijzigingen in de toekomst aan de IP-bereiken zullen worden aangebracht. Nieuwe bereiken in het bestand worden gedurende ten minste één week nog niet in de datacenters gebruikt. Download elke week het nieuwe XML-bestand en voer de benodigde wijzigingen uit op uw site om de services die in Azure worden uitgevoerd correct te identificeren. Express Route-gebruikers zullen merken dat dit bestand wordt gebruikt om in de eerste week van elke maand de BGP-advertenties van Azure-ruimte bij te werken. 

## <a name="how-can-i-use-azure-resource-manager-vnets-with-cloud-services"></a>Hoe kan ik Azure Resource Manager VNets gebruiken met Cloudservices? 

Cloud-services in een Azure Resource Manager VNets kunnen niet worden geplaatst, maar een Azure Resource Manager VNets en een klassieke vnet's kunnen worden verbonden via peering. Zie voor meer informatie [virtuele netwerk peering](../virtual-network/virtual-network-peering-overview.md).