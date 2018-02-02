---
title: Azure datacenter integratie Stack - eindpunten publiceren
description: Informatie over het publiceren van Azure-Stack-eindpunten in uw datacenter
services: azure-stack
author: jeffgilb
ms.service: azure-stack
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: 
ms.openlocfilehash: e368109adc7db4c589ac37b28c4891cb3ec5346f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure datacenter integratie Stack - eindpunten publiceren

*Van toepassing op: Azure Stack geïntegreerd systemen*

Azure Stack stelt u verschillende eindpunten (VIP's - virtuele IP-adressen) voor de functies van de infrastructuur. Deze VIP's worden toegewezen vanuit de openbare IP-adresgroep. Elke VIP is beveiligd met een toegangsbeheerlijst (ACL) in de software gedefinieerde netwerklaag. ACL's worden ook gebruikt voor de fysieke switches (blijven en BMC) voor het verder beperken van de oplossing. Een DNS-vermelding wordt voor elk eindpunt dat in de externe DNS-zone die is opgegeven tijdens de implementatie gemaakt.


Het volgende architecturaal diagram toont de verschillende lagen en ACL's:

![Architectuurdiagram](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Poorten en protocollen (inkomend)

De infrastructuur VIP's die vereist voor publicatie Stack Azure-eindpunten met externe netwerken zijn worden vermeld in de volgende tabel. De lijst ziet u alle eindpunt, de vereiste poort en protocol. Eindpunten die vereist zijn voor de extra resourceproviders, zoals de resourceprovider voor SQL en andere, vallen in de documentatie van bepaalde resource provider-implementatie.

Interne infrastructuur VIP's worden niet weergegeven omdat ze niet vereist voor publishing Azure-Stack.

> [!NOTE]
> Gebruiker VIP's zijn dynamisch, gedefinieerd door de gebruikers zelf geen controle door de Azure-Stack-operator.


|Eindpunt (VIP)|DNS-hostnaam een record|Protocol|Poorten|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (beheerder)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (beheerder)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (gebruiker)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
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
|MySQL Resource Provider|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304

## <a name="ports-and-urls-outbound"></a>Poorten en URL's (uitgaand)

Azure-Stack ondersteunt alleen transparentproxy servers. In een implementatie waarbij een transparentproxy uplinks met een traditioneel proxyserver, moet u toestaan de volgende poorten en URL's voor uitgaande communicatie:


|Doel|URL|Protocol|Poorten|
|---------|---------|---------|---------|
|Identiteit|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Marketplace-syndicatie|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patch & bijwerken|https://&#42;.azureedge.net|HTTPS|443|
|Registratie|https://management.azure.com|HTTPS|443|
|Gebruik|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Volgende stappen
[Azure-Stack PKI-vereisten](azure-stack-pki-certs.md)