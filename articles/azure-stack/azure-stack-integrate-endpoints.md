---
title: Azure datacenter integratie Stack - eindpunten publiceren | Microsoft Docs
description: Informatie over het publiceren van Azure-Stack-eindpunten in uw datacenter
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 06/29/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: 0992846416ce77bccd23fda73f61568eb61c33fb
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127632"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure datacenter integratie Stack - eindpunten publiceren
Azure Stack stelt u de virtuele IP-adressen (VIP's) voor de functies van de infrastructuur. Deze VIP's worden toegewezen vanuit de openbare IP-adresgroep. Elke VIP is beveiligd met een toegangsbeheerlijst (ACL) in de software gedefinieerde netwerklaag. ACL's worden ook gebruikt voor de fysieke switches (blijven en BMC) voor het verder beperken van de oplossing. Een DNS-vermelding wordt voor elk eindpunt dat in de externe DNS-zone die tijdens de implementatie opgegeven gemaakt.


Het volgende architecturaal diagram toont de verschillende lagen en ACL's:

![Structurele afbeelding](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Poorten en protocollen (inkomend)

Een set van infrastructuur VIP's zijn vereist voor publishing Stack Azure-eindpunten met externe netwerken. De *eindpunt (VIP)* tabel ziet u alle eindpunt, de vereiste poort en protocol. Raadpleeg de documentatie specifieke resource provider-implementatie voor eindpunten waarvoor extra resourceproviders, zoals de SQL-resourceprovider.

Interne infrastructuur VIP's worden niet weergegeven omdat ze niet vereist voor publishing Azure-Stack.

> [!NOTE]
> Gebruiker VIP's zijn dynamisch, gedefinieerd door de gebruikers zelf geen controle door de Azure-Stack-operator.


|Eindpunt (VIP)|DNS-hostnaam een record|Protocol|Poorten|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (beheerder)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (beheerder)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (gebruiker)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (gebruiker)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Lijst met ingetrokken certificaten|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP EN UDP|53|
|Sleutelkluis (gebruiker)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Sleutelkluis (beheerder)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Opslagwachtrij|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Table Storage|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Storage Blob|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|SQL-Resourceprovider|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|MySQL-Resourceprovider|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|App Service|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300 (azure Resource Manager)|
|  |ftp.appservice.*&lt;region>.&lt;fqdn>*|TCP, UDP|21, 1021, 10001-101000 (FTP)<br>990 (FTPS)|
|VPN Gateways|     |     |[Zie de VPN-gateway Veelgestelde vragen over](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability).|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>Poorten en URL's (uitgaand)

Azure-Stack ondersteunt alleen transparentproxy servers. In een implementatie waarbij een transparentproxy uplinks met een traditioneel proxyserver, moet u toestaan de volgende poorten en URL's voor uitgaande communicatie:


|Doel|URL|Protocol|Poorten|
|---------|---------|---------|---------|
|Identiteit|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net<br>https://secure.aadcdn.microsoftonline-p.com|HTTP<br>HTTPS|80<br>443|
|Marketplace-syndicatie|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patch & bijwerken|https://&#42;.azureedge.net|HTTPS|443|
|Registratie|https://management.azure.com|HTTPS|443|
|Gebruik|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.NET|HTTPS|443|
|Windows Defender|. wdcp.microsoft.com<br>. wdcpalt.microsoft.com<br>*. updates.microsoft.com<br>*. download.microsoft.com<br>https://msdl.microsoft.com/download/symbols<br>http://www.microsoft.com/pkiops/crl<br>http://www.microsoft.com/pkiops/certs<br>http://crl.microsoft.com/pki/crl/products<br>http://www.microsoft.com/pki/certs<br>https://secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|
|NTP|     |UDP|123|
|DNS|     |TCP<br>UDP|53|
|     |     |     |     |



## <a name="next-steps"></a>Volgende stappen

[Azure-Stack PKI-vereisten](azure-stack-pki-certs.md)
