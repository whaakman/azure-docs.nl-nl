---
title: Beveiliging in Azure App Service en Azure Functions | Microsoft Docs
description: Meer informatie over hoe App Service beveiligde helpt uw app en hoe meer vergrendelt u uw app tegen bedreigingen.
keywords: Azure app service, web-app, mobiele app, api-app, functie-app, beveiliging, veilig, beveiligd, naleving, voldoen aan het beleid, certificaat, certificaten, https, ftps tls, vertrouwen, codering, versleutelen, gecodeerd, IP-beperking, verificatie, autorisatie, authn, autho, msi, beheerde service-identiteit, geheimen, geheim, patches, patch, patches, versie, isolatie, netwerkisolatie, ddos, mitm
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: cephalin
ms.openlocfilehash: 2ca1c1518589e60a03570e1c2063381f749ed9aa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654915"
---
# <a name="security-in-azure-app-service-and-azure-functions"></a>Beveiliging in Azure App Service en Azure Functions

Dit artikel ziet u hoe [Azure App Service](app-service-web-overview.md) helpt bij het beveiligen van uw web-app, mobiele app back-end, API-app en [Azure Functions](/azure/azure-functions/). Ook ziet u hoe u uw app met de ingebouwde App Service-functies verder kunt beveiligen.

De platformonderdelen van App Service, waaronder Azure virtuele machines, opslag, netwerkverbindingen, web frameworks, management en integratie-functies zijn actief beveiligd en bestand. App Service doorloopt NTDS krachtige nalevingscontroles doorlopend om ervoor te zorgen dat:

- Uw app-resources zijn [beveiligde](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) van andere klanten Azure-resources.
- [VM-instanties en runtime-software worden regelmatig bijgewerkt](app-service-patch-os-runtime.md) voor beveiligingsproblemen van het adres onlangs gedetecteerd. 
- Communicatie van geheimen (zoals verbindingsreeksen) tussen uw app en andere Azure-resources (zoals [SQL-Database](/services/sql-database/)) blijft in Azure en niet alle netwerkgrenzen cross. Geheimen worden altijd versleuteld wanneer opgeslagen.
- Alle communicatie via de App Service-connectiviteit functies zoals [hybride verbinding](app-service-hybrid-connections.md), worden versleuteld. 
- Verbindingen met externe beheerprogramma's zoals Azure PowerShell, Azure CLI, Azure SDK's, REST-API's, worden alle versleuteld.
- 24-uurs threat management beveiligt de volgende infrastructuur en platform tegen schadelijke software, gedistribueerde denial-of-service (DDoS) man-in-the-middle (MITM) en andere dreigingen.

Zie voor meer informatie over de infrastructuur- en beveiliging in Azure [Azure Vertrouwenscentrum](https://azure.microsoft.com/overview/trusted-cloud/).

De volgende secties laten zien hoe uw App Service-app verder beschermen tegen bedreigingen.

## <a name="https-and-certificates"></a>HTTPS- en certificaten

App Service kunt u uw apps met secure [HTTPS](https://wikipedia.org/wiki/HTTPS). Wanneer uw app wordt gemaakt, de standaarddomeinnaam (\<app_naam >. azurewebsites.net) is al toegankelijk via HTTPS. Als u [configureren van een aangepast domein voor uw app](app-service-web-tutorial-custom-domain.md), moet u ook [Beveilig deze met een aangepaste certificaat](app-service-web-tutorial-custom-ssl.md) zodat clientbrowsers beveiligde HTTPS-verbindingen met uw aangepaste domein kunnen maken. Er zijn twee manieren doen:

- **App Service-certificaat** -maken van een certificaat rechtstreeks in Azure. Het certificaat is beveiligd [Azure Key Vault](/azure/key-vault/), en kunnen worden geïmporteerd in uw App Service-app. Zie voor meer informatie [kopen en configureren van een SSL-certificaat voor uw Azure App Service](web-sites-purchase-ssl-web-site.md).
- **Certificaat van de derde partij** - uploaden van een aangepaste SSL-certificaat dat u hebt aangeschaft via een vertrouwde certificeringsinstantie en bindt dit aan uw App Service-app. App Service ondersteunt zowel één domein en jokertekens-certificaten. Het ondersteunt ook zelfondertekende certificaten voor testdoeleinden. Zie voor meer informatie [een bestaande aangepaste SSL-certificaat binden aan Azure Web Apps](app-service-web-tutorial-custom-ssl.md).

## <a name="insecure-protocols-http-tls-10-ftp"></a>Onbeveiligde protocollen HTTP, TLS 1.0 FTP)

App Service biedt voor het beveiligen van uw app tegen alle niet-versleutelde (HTTP)-verbindingen, één muisklik configuratie om af te dwingen van HTTPS. Niet-beveiligde aanvragen zijn afwezig ingeschakeld voordat ze uw toepassingscode zelfs bereiken. Zie voor meer informatie [afdwingen HTTPS](app-service-web-tutorial-custom-ssl.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 wordt niet langer als veilig beschouwd door industrienormen, zoals [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). App Service kunt u verouderde protocollen door uitschakelen [afdwingen van TLS 1.1/1.2](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112).

App Service ondersteunt zowel FTP en FTPS voor het implementeren van uw bestanden. Echter moet FTPS worden gebruikt in plaats van FTP, indien mogelijk. Wanneer een of beide van deze protocollen niet gebruikt zijn, moet u [uit te schakelen](app-service-deploy-ftp.md#enforce-ftps).

## <a name="static-ip-restrictions"></a>Statische IP-adresbeperkingen

Uw app in App Service-aanvragen van alle IP-adressen van het internet accepteert, maar u kunt beperken die toegang tot een kleine subset van IP-adressen. Op Windows-App Service kunt u een lijst met IP-adressen die toegang hebben tot uw app definiëren. De lijst met toegestane kan afzonderlijke IP-adressen of een bereik met IP-adressen die zijn gedefinieerd door een subnetmasker. Zie voor meer informatie [Azure App Service statische IP-adresbeperkingen](app-service-ip-restrictions.md).

Voor App-Service op Windows, u kunt ook beperken IP-adressen dynamisch door het configureren van de _web.config_. Zie voor meer informatie [dynamische IP-beveiliging <dynamicIpSecurity> ](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>Clientverificatie en autorisatie

Azure App Service biedt directe verificatie en autorisatie van gebruikers of client apps. Wanneer dit is ingeschakeld, kan deze gebruikers- en client-apps met weinig of geen toepassingscode aanmelden. U kunt uw eigen verificatie en autorisatie-oplossing implementeren of toestaan dat App-Service verwerkt; het voor u in plaats daarvan. De module voor verificatie en autorisatie webaanvragen worden verwerkt voordat het afleveren aan uw toepassingscode en het weigert ongeautoriseerde aanvragen voordat ze uw code bereiken.

App Service-verificatie en autorisatie ondersteuning voor meerdere verificatieproviders, met inbegrip van Azure Active Directory, Microsoft-accounts, Facebook, Google en Twitter. Zie voor meer informatie [verificatie en autorisatie in Azure App Service](app-service-authentication-overview.md).

## <a name="service-to-service-authentication"></a>Verificatie van service-tot-service

Wanneer verificatie ten opzichte van een back-end-service, kunt u App Service twee verschillende methoden, afhankelijk van uw behoeften:

- **Service-identiteit** -aanmelden bij de externe bron via de identiteit van de app zelf. App Service kunt u op eenvoudige wijze een [beheerde service-identiteit](app-service-managed-service-identity.md), dat u kunt gebruiken om te verifiëren met andere services, zoals [Azure SQL Database](/azure/sql-database/) of [Azure Key Vault](/azure/key-vault/). Zie voor een end-to-end zelfstudie van deze benadering [Secure Azure SQL Database-verbinding van App Service met behulp van beheerde service-identiteit](app-service-web-tutorial-connect-msi.md).
- **Op-andere gebruikers-of (OBO)** -gedelegeerde toegang tot externe bronnen namens de gebruiker maken. Met Azure Active Directory als verificatieprovider, kan uw App Service-app uitvoeren gedelegeerd aanmelden bij een externe service zoals [Azure Active Directory Graph API](../active-directory/develop/active-directory-graph-api.md) of een externe API-app in App Service. Zie voor een end-to-end zelfstudie van deze benadering [verifiëren en autoriseren van gebruikers end-to-end in Azure App Service](app-service-web-tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Verbinding met de externe resources

Er zijn drie soorten externe bronnen in die uw app mogelijk nodig hebt voor toegang tot: 

- [Azure-resources](#azure-resources)
- [Bronnen binnen een virtuele Azure-netwerk](#resources-inside-an-azure-virtual-network)
- [Lokale bronnen](#on-premises-resources)

App Service biedt een manier om te maken van beveiligde verbindingen in alle gevallen, maar u aanbevolen procedures voor beveiliging nog steeds rekening moet houden. Bijvoorbeeld, gebruik altijd versleutelde verbindingen zelfs als de back-end-bron niet-versleutelde verbindingen toestaat. Controleer bovendien of back-end van uw Azure-service geeft de minimale set van IP-adressen. U kunt de uitgaande IP-adressen vinden voor uw app op [voor inkomend en uitgaand IP-adressen in Azure App Service](app-service-ip-addresses.md).

### <a name="azure-resources"></a>Azure-resources

Als uw app verbinding maakt met Azure-resources, zoals [SQL-Database](/services/sql-database/) en [Azure Storage](/azure/storage/), de verbinding blijft in Azure en niet alle netwerkgrenzen cross. Echter, de verbinding doorloopt de gedeelde netwerken in Azure, dus altijd voor zorgen dat de verbinding is versleuteld. 

Als uw app wordt gehost een [App Service-omgeving](environment/intro.md), moet u [verbinding maken met Azure-services met behulp van Virtual Network service-eindpunten ondersteund](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="resources-inside-an-azure-virtual-network"></a>Bronnen binnen een virtuele Azure-netwerk

Uw app toegang tot bronnen in een [Azure Virtual Network](/azure/virtual-network/) via [integratie van virtueel netwerk](web-sites-integrate-with-vnet.md). De integratie met een virtueel netwerk met een punt-naar-site VPN tot stand is gebracht. De app vervolgens toegang tot de bronnen in het virtuele netwerk hun privé IP-adressen. De verbinding punt-naar-site echter nog steeds de gedeelde netwerken passeren in Azure. 

Als u wilt isoleren van de verbindingen van de resource volledig uit de gedeelde netwerken in Azure, maakt u uw app in een [App Service-omgeving](environment/intro.md). Omdat een App Service-omgeving altijd op een toegewezen virtueel netwerk is geïmplementeerd, is connectiviteit tussen uw app en bronnen binnen het virtuele netwerk volledig geïsoleerd. Zie voor andere aspecten van netwerkbeveiliging in een App Service-omgeving [netwerkisolatie](#network-isolation).

### <a name="on-premises-resources"></a>Lokale bronnen

U kunt veilig toegang tot lokale bronnen, zoals databases, op drie manieren: 

- [Hybride verbindingen](app-service-hybrid-connections.md) -maakt een point-to-point-verbinding met de externe bron via een TCP-tunnel. De TCP-tunnel wordt ingesteld met behulp van TLS 1.2 met shared access signature (SAS)-sleutels.
- [Virtuele netwerkintegratie](web-sites-integrate-with-vnet.md) met site-naar-site VPN - zoals beschreven in [bronnen binnen een Azure Virtual Network](#resources-inside-an-azure-virtual-network), maar het virtuele netwerk kunnen worden verbonden met uw on-premises netwerk via een [ site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). In deze topologie network kan uw app verbinding maken met lokale bronnen zoals andere bronnen in het virtuele netwerk.
- [App Service-omgeving](environment/intro.md) met site-naar-site VPN - zoals beschreven in [bronnen binnen een Azure Virtual Network](#resources-inside-an-azure-virtual-network), maar het virtuele netwerk kunnen worden verbonden met uw on-premises netwerk via een [site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). In deze topologie network kan uw app verbinding maken met lokale bronnen zoals andere bronnen in het virtuele netwerk.

## <a name="application-secrets"></a>Toepassing geheimen

Geheimen van de toepassing, zoals databasereferenties API tokens en persoonlijke sleutels niet worden opgeslagen in uw code of configuratie-bestanden. De algemeen geaccepteerde aanpak is toegang tot deze als [omgevingsvariabelen](https://wikipedia.org/wiki/Environment_variable) met behulp van het standaard patroon in uw taal van keuze. In App Service, is de manier om omgevingsvariabelen definiëren via [appinstellingen](web-sites-configure.md#app-settings) (en, met name voor .NET-toepassingen, [verbindingsreeksen](web-sites-configure.md#connection-strings)). App-instellingen en verbindingsreeksen worden opgeslagen versleuteld in Azure en ze zijn gedecodeerd alleen voordat wordt opgenomen in het procesgeheugen van uw app wanneer de app wordt gestart. De versleutelingssleutels worden regelmatig gedraaid.

U kunt ook kunt u uw App Service-app integreren [Azure Key Vault](/azure/key-vault/) voor geavanceerde geheimen management. Door [toegang tot de Sleutelkluis met een beheerde service-identiteit](../key-vault/tutorial-web-application-keyvault.md), uw App Service-app veilig toegang krijgen tot de geheimen die u nodig hebt.

## <a name="network-isolation"></a>Netwerkisolatie

Met uitzondering van de **geïsoleerd** prijscategorie, alle lagen uw apps uitvoeren op gedeelde netwerkinfrastructuur in App Service. Bijvoorbeeld, worden het openbare IP-adressen en de front-end netwerktaakverdelers gedeeld met andere tenants. De **geïsoleerd** laag biedt u volledige netwerkisolatie door het uitvoeren van uw apps binnen een speciaal [App Service-omgeving](environment/intro.md). Een App Service-omgeving wordt uitgevoerd in uw eigen exemplaar van [Azure Virtual Network](/azure/virtual-network/). Hiermee kunt u: 

- Beperken van toegang tot het netwerk met [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md). 
- Uw apps via een speciale openbaar eindpunt met speciale front-ends fungeren.
- Interne toepassing met behulp van een interne load balancer (ILB), waarmee alleen toegankelijk is vanaf binnen uw Azure Virtual Network kan fungeren. De ILB heeft een IP-adres van uw persoonlijke subnet, waardoor de totale isolatie van uw apps vanaf internet.
- [Gebruik een ILB achter een web application firewall (WAF)](environment/integrate-with-application-gateway.md). De WAF biedt beveiliging voor uw openbare toepassingen, zoals DDoS-bescherming, URI voor het filteren en preventie van de SQL-injectie op bedrijfsniveau.

Zie voor meer informatie [Inleiding tot Azure App Service-omgevingen](environment/intro.md).