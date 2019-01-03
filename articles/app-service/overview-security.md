---
title: Beveiligingsoverzicht - Azure App Service | Microsoft Docs
description: Meer informatie over het hoe App Service beveiligd helpt uw app en hoe u verder kunt uw app tegen bedreigingen kunt vergrendelen.
keywords: Azure appservice, web-app, mobiele Apps, api-app, functie-app, beveiliging, veilige, beveiligd, naleving, voldoen aan het beleid, certificaten, certificaten, https, ftps, tls, vertrouwen, codering, versleutelen, versleuteld, ip-beperking, verificatie, autorisatie, authn, autho, MSI-bestand, beheerde service-identiteit, beheerde identiteit, geheimen, geheim, patches, patch, patches, versie, isolatie, netwerkisolatie, ddos, mitm
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
ms.date: 08/24/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 1973d7e360f77aed028d8a93a625f0f6cb7e1ab8
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731078"
---
# <a name="security-in-azure-app-service"></a>Beveiliging in Azure App Service

In dit artikel ziet u hoe u [Azure App Service](overview.md) helpt uw web-app, back-end van mobiele Apps, API-app, beveiligen en [functie-app](/azure/azure-functions/). Ook ziet u hoe u uw app met de ingebouwde App Service-functies verder kunt beveiligen.

De platformonderdelen van App Service, inclusief Azure Virtual machines, opslag, netwerkverbindingen, web-frameworks, beheer en integratie-functies zijn actief wordt beveiligd en beveiligde. App Service verloopt via krachtige nalevingscontroles doorlopend om ervoor te zorgen dat:

- Uw app-resources zijn [beveiligde](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) van andere klanten Azure-resources.
- [VM-exemplaren en runtime-software worden regelmatig bijgewerkt](overview-patch-os-runtime.md) adres onlangs ontdekte beveiligingslekken. 
- Communicatie van geheimen (zoals tekenreeksen voor databaseverbindingen) tussen uw app en andere Azure-resources (zoals [SQL-Database](https://azure.microsoft.com/services/sql-database/)) blijft in Azure en niet alle netwerkgrenzen cross. Geheimen worden altijd versleuteld wanneer die zijn opgeslagen.
- Alle communicatie via de App Service-connectiviteit-functies, zoals [hybride verbinding](app-service-hybrid-connections.md), worden versleuteld. 
- Verbindingen met externe beheerprogramma's, zoals Azure PowerShell, Azure CLI, Azure-SDK's, REST-API's, worden alle versleuteld.
- 24-uurs threat management beveiligt de infrastructuur en platform tegen schadelijke software, distributed denial-of-service (DDoS), man-in-the-middle (MITM) en andere dreigingen.

Zie voor meer informatie over de infrastructuur en platform beveiliging in Azure [Azure Trust Center](https://azure.microsoft.com/overview/trusted-cloud/).

De volgende secties laten zien hoe uw App Service-app nog beter worden beschermd tegen bedreigingen.

## <a name="https-and-certificates"></a>HTTPS en certificaten

App Service kunt u uw apps met secure [HTTPS](https://wikipedia.org/wiki/HTTPS). Wanneer uw app wordt gemaakt, de standaard domain name (\<app_naam >. azurewebsites.net) is al die toegankelijk is via HTTPS. Als u [een aangepast domein voor uw app configureren](app-service-web-tutorial-custom-domain.md), moet u ook [Beveilig deze met een aangepast certificaat](app-service-web-tutorial-custom-ssl.md) clientbrowsers beveiligde HTTPS-verbindingen met uw aangepaste domein kunnen aanbrengen. Er zijn twee manieren om dit te doen:

- **App Service certificate** -maken van een certificaat rechtstreeks in Azure. Het certificaat is beveiligd [Azure Key Vault](/azure/key-vault/), en kunnen worden geïmporteerd in uw App Service-app. Zie voor meer informatie, [kopen en configureren van een SSL-certificaat voor uw Azure App Service](web-sites-purchase-ssl-web-site.md).
- **Certificaat van derden** : een aangepast SSL-certificaat dat u hebt aangeschaft via een vertrouwde certificeringsinstantie uploaden en binden aan uw App Service-app. App Service ondersteunt zowel één domein en jokertekencertificaten. Het ondersteunt ook zelfondertekende certificaten voor testdoeleinden. Zie voor meer informatie, [een bestaand aangepast SSL-certificaat binden aan Azure App Service](app-service-web-tutorial-custom-ssl.md).

## <a name="insecure-protocols-http-tls-10-ftp"></a>Onbeveiligde protocollen HTTP, TLS 1.0, FTP-protocol)

App Service biedt voor het beveiligen van uw app op basis van alle niet-versleutelde (HTTP)-verbindingen, configuratie van één klik om af te dwingen van HTTPS. Niet-beveiligde aanvragen ingeschakeld, opgeslagen voordat ze zelfs code van uw toepassing bereiken. Zie voor meer informatie, [HTTPS afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 wordt niet meer als veilig beschouwd door industriële standaarden, zoals [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). App Service kunt u verouderde protocollen door uitschakelen [TLS 1.1/1.2 afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions).

App Service biedt ondersteuning voor FTP- en FTPS voor het implementeren van uw bestanden. Echter, FTPS moet worden gebruikt in plaats van de FTP-, indien mogelijk. Wanneer één of beide van deze protocollen niet gebruikt worden, moet u [uit te schakelen](deploy-ftp.md#enforce-ftps).

## <a name="static-ip-restrictions"></a>Statische IP-adresbeperkingen

Uw app in App Service-aanvragen van alle IP-adressen van het internet accepteert, maar u kunt beperken die de toegang tot een kleine subset van IP-adressen. App Service in Windows kunt u een lijst met IP-adressen die zijn toegestaan voor toegang tot uw app te definiëren. De lijst met toegestane kan afzonderlijke IP-adressen of een bereik van IP-adressen die zijn gedefinieerd door een subnetmasker. Zie voor meer informatie, [Azure App Service statische IP-adresbeperkingen](app-service-ip-restrictions.md).

Voor App Service onder Windows, u kunt ook beperken IP-adressen dynamisch door het configureren van de _web.config_. Zie voor meer informatie, [dynamische IP-beveiliging <dynamicIpSecurity> ](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>Clientverificatie en autorisatie

Azure App Service biedt gebruiksklare verificatie en autorisatie van gebruikers of -client apps. Wanneer dit is ingeschakeld, kan deze gebruikers- en client-apps met weinig of geen toepassingscode aanmelden. U kunt uw eigen verificatie en autorisatie-oplossing implementeren of toestaan App-Service verwerkt; het voor u in plaats daarvan. De verificatie en autorisatie-module webaanvragen worden verwerkt voordat het afleveren op uw toepassingscode en het niet-geautoriseerde aanvragen weigert voordat ze zich bij uw code.

App Service-verificatie en autorisatie ondersteuning voor meerdere authentication-providers, waaronder Azure Active Directory, Microsoft-accounts, Facebook, Google en Twitter. Zie voor meer informatie, [verificatie en autorisatie in Azure App Service](overview-authentication-authorization.md).

## <a name="service-to-service-authentication"></a>Verificatie van service-tot-service

Verificatie bij een back-endservice, biedt App Service op twee manieren, afhankelijk van uw behoeften:

- **Service-identiteit** -aanmelden bij de externe resource met behulp van de identiteit van de app zelf. App Service kunt u eenvoudig maken een [beheerde identiteit](overview-managed-identity.md), die u kunt gebruiken om te verifiëren met andere services, zoals [Azure SQL Database](/azure/sql-database/) of [Azure Key Vault](/azure/key-vault/). Zie voor een zelfstudie voor end-to-end van deze benadering, [Secure Azure SQL Database-verbinding vanuit App Service met behulp van een beheerde identiteit](app-service-web-tutorial-connect-msi.md).
- **On-behalf-of (OBO)** -ervoor gedelegeerde toegang tot externe resources namens de gebruiker. Met Azure Active Directory als de verificatieprovider, kan uw App Service-app uitvoeren gedelegeerde aanmelden bij een externe service, zoals [Azure Active Directory Graph API](../active-directory/develop/active-directory-graph-api.md) of een externe API-app in App Service. Zie voor een zelfstudie voor end-to-end van deze benadering, [verifiëren en autoriseren van gebruikers end-to-end in Azure App Service](app-service-web-tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Verbinding met externe bronnen

Er zijn drie typen van externe bronnen die uw app mogelijk nodig hebt voor toegang tot: 

- [Azure-resources](#azure-resources)
- [De resources in een Azure-netwerk](#resources-inside-an-azure-virtual-network)
- [On-premises bronnen](#on-premises-resources)

In elk van deze gevallen is App Service biedt een manier waarop u beveiligde verbindingen kunt maken, maar u aanbevolen beveiligingsprocedures voor het nog steeds in rekening moet houden. Bijvoorbeeld, altijd versleutelde verbindingen gebruiken, zelfs als de back-end-bron niet-versleutelde verbindingen toestaat. Bovendien, zorg ervoor dat uw back-end Azure-service de minimale set IP-adressen geeft. U vindt de uitgaande IP-adressen voor uw app op [inkomende en uitgaande IP-adressen in Azure App Service](overview-inbound-outbound-ips.md).

### <a name="azure-resources"></a>Azure-resources

Als uw app verbinding maakt met Azure-resources, zoals [SQL-Database](https://azure.microsoft.com/services/sql-database/) en [Azure Storage](/azure/storage/), de verbinding blijft in Azure en niet alle netwerkgrenzen cross. Echter, de verbinding verloopt via het gedeelde netwerken in Azure, dus altijd voor zorgen dat de verbinding is versleuteld. 

Als uw app wordt gehost een [App Service-omgeving](environment/intro.md), moet u [verbinding maken met Azure-services met behulp van service-eindpunten ondersteund](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="resources-inside-an-azure-virtual-network"></a>De resources in een Azure-netwerk

Uw app toegang heeft tot resources in een [Azure Virtual Network](/azure/virtual-network/) via [integratie van virtuele netwerken](web-sites-integrate-with-vnet.md). De integratie met een Virtueelnetwerk met een punt-naar-site-VPN-verbinding tot stand is gebracht. De app hebben vervolgens toegang tot de resources in het Virtueelnetwerk met behulp van hun privé IP-adressen. De punt-naar-site-verbinding, passeert echter nog steeds de gedeelde netwerken in Azure. 

Om te isoleren van de verbindingen van de resource volledig vanaf de gedeelde netwerken in Azure, maak uw app in een [App Service-omgeving](environment/intro.md). Omdat een App Service environment wordt altijd geïmplementeerd op een toegewezen Virtueelnetwerk, is connectiviteit tussen uw app en de resources binnen het Virtueelnetwerk is volledig geïsoleerd. Zie voor andere aspecten van netwerkbeveiliging in een App Service-omgeving, [netwerkisolatie](#network-isolation).

### <a name="on-premises-resources"></a>On-premises bronnen

U kunt veilig toegang krijgen tot on-premises bronnen, zoals databases, op drie manieren: 

- [Hybride verbindingen](app-service-hybrid-connections.md) -een point-to-point-verbinding naar uw externe resource via een TCP-tunnel tot stand brengt. De TCP-tunnel is ingesteld met behulp van TLS 1.2 met shared access signature (SAS) sleutels.
- [Virtual Network-integratie](web-sites-integrate-with-vnet.md) met site-naar-site-VPN - zoals is beschreven in [bronnen binnen een Azure Virtual Network](#resources-inside-an-azure-virtual-network), maar het Virtueelnetwerk kunnen worden verbonden met uw on-premises netwerk via een [ site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). In deze topologie van netwerk, kan uw app verbinding maken met on-premises bronnen als andere resources in het Virtueelnetwerk.
- [App Service-omgeving](environment/intro.md) met site-naar-site-VPN - zoals is beschreven in [bronnen binnen een Azure Virtual Network](#resources-inside-an-azure-virtual-network), maar het Virtueelnetwerk kunnen worden verbonden met uw on-premises netwerk via een [site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). In deze topologie van netwerk, kan uw app verbinding maken met on-premises bronnen als andere resources in het Virtueelnetwerk.

## <a name="application-secrets"></a>Toepassingsgeheimen

Toepassingsgeheimen, zoals de databasereferenties, API-tokens en persoonlijke sleutels niet worden opgeslagen in uw code of configuratie bestanden. De algemeen geaccepteerde benadering is voor toegang tot deze als [omgevingsvariabelen](https://wikipedia.org/wiki/Environment_variable) met het standaardpatroon in uw taal naar keuze. In App Service, de manier voor het definiëren van omgevingsvariabelen is via [app-instellingen](web-sites-configure.md#app-settings) (en, met name voor .NET-toepassingen, [verbindingsreeksen](web-sites-configure.md#connection-strings)). App-instellingen en verbindingsreeksen worden versleuteld opgeslagen in Azure en ze zijn ontsleuteld alleen voordat wordt opgenomen in het procesgeheugen van uw app wanneer de app wordt gestart. De versleutelingssleutels worden regelmatig gedraaid.

U kunt ook uw App Service-app met integreren [Azure Key Vault](/azure/key-vault/) voor het beheer van geavanceerde geheimen. Door [toegang tot de Key Vault met een beheerde identiteit](../key-vault/tutorial-web-application-keyvault.md), uw App Service-app veilig toegang krijgen tot de geheimen die u nodig hebt.

## <a name="network-isolation"></a>Netwerkisolatie

Met uitzondering van de **geïsoleerd** prijscategorie, alle lagen uw apps uitgevoerd op de gedeelde netwerkinfrastructuur in App Service. Bijvoorbeeld, worden het openbare IP-adressen en front-load balancers gedeeld met andere tenants. De **geïsoleerd** laag biedt u volledige netwerkisolatie door het uitvoeren van de apps in een toegewezen [App Service-omgeving](environment/intro.md). Een App Service-omgeving wordt uitgevoerd in uw eigen exemplaar van [Azure Virtual Network](/azure/virtual-network/). Hiermee kunt u: 

- Beperken van toegang tot het netwerk met [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-dmz-nsg.md). 
- Uw apps via een toegewezen openbare eindpunt, met speciale front-ends fungeren.
- Interne toepassing via een interne load balancer (ILB), waarmee alleen toegankelijk is vanaf binnen uw Azure Virtual Network kunt leveren. De ILB heeft een IP-adres van uw privé-subnet waarmee de totale isolatie van uw apps vanaf het internet.
- [Gebruik een ILB achter een web application firewall (WAF)](environment/integrate-with-application-gateway.md). De WAF biedt beveiliging voor uw openbare-toepassingen, zoals DDoS protection, URI filteren en SQL-injectie preventie op ondernemingsniveau.

Zie voor meer informatie, [Inleiding tot Azure App Service-omgevingen](environment/intro.md). 
