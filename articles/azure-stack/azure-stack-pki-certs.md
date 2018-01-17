---
title: "Azure-Stack Public Key Infrastructure-certificaatvereisten voor Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: "Beschrijft de implementatievereisten van Azure Stack PKI-certificaat voor Azure-Stack geïntegreerd systemen."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 8f0bb2266cb3a8a869ad50c40a46eb82985d17ed
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure-Stack Public Key Infrastructure-certificaatvereisten
Azure-Stack is een infrastructuur voor openbare-netwerk met behulp van extern toegankelijke openbare IP-adressen die zijn toegewezen aan een klein aantal Stack Azure-services en mogelijk tenant-VM's. PKI-certificaten met de juiste DNS-namen voor deze eindpunten van de infrastructuur voor openbare Azure-Stack zijn vereist tijdens de implementatie van Azure-Stack. In dit artikel bevat informatie over:

- Welke certificaten zijn vereist voor het implementeren van Azure-Stack
- Het proces voor het verkrijgen van certificaten die overeenkomt met deze specificaties
- Het voorbereiden, valideren en gebruiken van die certificaten tijdens de implementatie

## <a name="certificate-requirements"></a>Certificaatvereisten
De volgende lijst beschrijft de vereisten voor certificaten die nodig zijn voor het implementeren van Azure-Stack: 
- Certificaten moeten zijn uitgegeven door een interne certificeringsinstantie of een openbare certificeringsinstantie. Als een openbare certificeringsinstantie wordt gebruikt, moet worden opgenomen in de installatiekopie van het basisbesturingssysteem als onderdeel van het Microsoft-programma vertrouwde basis-instantie. U vindt hier de volledige lijst: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- Het certificaat kan een certificaat voor één jokerteken die betrekking hebben op alle naamruimten in het veld onderwerp alternatieve naam (SAN) zijn. U kunt ook afzonderlijke certificaten met jokertekens voor eindpunten zoals opslag en Sleutelkluis waar ze vereist zijn. 
- Het certificaat handtekeningalgoritme niet SHA1, als sterkere moet. 
- Indeling van het certificaat moet PFX, als de openbare en persoonlijke sleutels vereist voor installatie van de Azure-Stack zijn. 
- Het pfx-certificaatbestand moeten een waarde 'Digitale handtekening' en 'KeyEncipherment' in het veld 'Sleutelgebruik' hebben.
- De wachtwoorden op alle certificate pfx-bestanden moeten hetzelfde zijn op het moment van implementatie
- Zorg ervoor dat de namen van certificaathouders en alternatieve onderwerpnamen met alle certificaten overeenkomen met de specificaties die worden beschreven in dit artikel om te voorkomen dat mislukte implementatie.

> [!NOTE]
> De aanwezigheid van tussenkomst certificeringsinstanties in de keten van vertrouwensrelaties IS van een certificaat wordt ondersteund. 

## <a name="mandatory-certificates"></a>Verplichte certificaten
De tabel in deze sectie beschrijft de Azure-Stack openbaar eindpunt PKI-certificaten die vereist voor beide Azure AD zijn en Azure-Stack van AD FS-implementaties. Vereisten voor certificaten worden gegroepeerd per gebied, evenals de naamruimten die worden gebruikt en de certificaten die vereist zijn voor elke naamruimte. De tabel beschrijft ook de map waarin uw solution provider de verschillende certificaten worden per openbaar eindpunt kopieert. 

Certificaten met de juiste DNS-namen voor elk eindpunt van de infrastructuur voor openbare Azure-Stack zijn vereist. DNS-naam van elk eindpunt wordt uitgedrukt in de indeling:  *&lt;voorvoegsel >.&lt; regio >. &lt;FQDN-naam >*. 

Voor uw implementatie, het [regio] en [externalfqdn] waarden moeten overeenkomen met de regio en externe domeinnamen die u hebt gekozen voor uw Azure-Stack-systeem. Een voorbeeld: als de regionaam van de is *Redmond* en was de naam van het externe domein *contoso.com*, de DNS-namen moeten de indeling  *&lt;voorvoegsel >. redmond.contoso.com* . De  *&lt;voorvoegsel >* waarden zijn vooraf bepaalde door Microsoft te beschrijven van het eindpunt dat wordt beveiligd door het certificaat. Bovendien de  *&lt;voorvoegsel >* waarden van de externe infrastructuur eindpunten is afhankelijk van de Azure-Stack-service die gebruikmaakt van de specifieke eindpunt. 

|Implementatiemap|Vereiste certificaatonderwerp en de alternatieve namen voor onderwerp (SAN)|Bereik (per regio)|Subdomein naamruimte|
|-----|-----|-----|-----|
|Openbare-Portal|portal.*&lt;region>.&lt;fqdn>*|Portals|*&lt;region>.&lt;fqdn>*|
|Beheerportal|adminportal.*&lt;region>.&lt;fqdn>*|Portals|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager Public|management.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|Azure Resource Manager-beheerder|adminmanagement.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|ACS<sup>1</sup>|Een multi-subdomein jokertekencertificaat met alternatieve namen voor:<br>&#42;.blob.*&lt;region>.&lt;fqdn>*<br>&#42;.queue.*&lt;region>.&lt;fqdn>*<br>&#42;.table.*&lt;region>.&lt;fqdn>*|Storage|blob.*&lt;region>.&lt;fqdn>*<br>table.*&lt;region>.&lt;fqdn>*<br>queue.*&lt;region>.&lt;fqdn>*|
|KeyVault|&#42;.vault.*&lt;region>.&lt;fqdn>*<br>(Wildcard-SSL-certificaat)|Key Vault|vault.*&lt;region>.&lt;fqdn>*|
|KeyVaultInternal|&#42;.adminvault.*&lt;region>.&lt;fqdn>*<br>(Wildcard-SSL-certificaat)|Internal Keyvault|adminvault.*&lt;region>.&lt;fqdn>*|
|
<sup>1</sup> drie jokertekens SAN's op één certificaat vereist dat de ACS-certificaat. Meerdere jokertekens SAN's op één certificaat kan niet worden ondersteund door alle openbare certificeringsinstanties. 

Als u Azure-Stack in de modus Azure AD-implementatie implementeert, hoeft u alleen de certificaten die worden vermeld in de vorige tabel aanvragen. Als u Azure-Stack in de modus van de implementatie van AD FS implementeert, moet u echter ook de certificaten in de volgende tabel beschreven aanvragen:

|Implementatiemap|Vereiste certificaatonderwerp en de alternatieve namen voor onderwerp (SAN)|Bereik (per regio)|Subdomein naamruimte|
|-----|-----|-----|-----|
|ADFS|adfs.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat)|ADFS|*&lt;region>.&lt;fqdn>*|
|Graph|graph.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat)|Graph|*&lt;region>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Alle certificaten die worden vermeld in deze sectie moeten hetzelfde wachtwoord hebben. 

## <a name="optional-paas-certificates"></a>Optionele PaaS-certificaten
Als u van plan bent de extra Azure-Stack PaaS-services (SQL, MySQL en App Service) implementeren na de Azure-Stack is geïmplementeerd en geconfigureerd, moet u extra certificaten omvatten de eindpunten van de PaaS-services aanvragen. 

> [!IMPORTANT]
> De certificaten die u voor App Service, SQL en MySQL resourceproviders gebruikt moeten hebben de dezelfde basiscertificeringsinstantie die gebruikt worden voor de globale Azure Stack-eindpunten. 

De volgende tabel beschrijft de eindpunten en certificaten zijn vereist voor de SQL- en MySQL-adapters en App-Service. U hoeft niet te kopiëren van deze certificaten naar de Azure-Stack-implementatiemap. In plaats daarvan bieden deze certificaten tijdens de installatie van de extra resourceproviders. 

|Bereik (per regio)|Certificaat|Vereiste certificaatonderwerp en de alternatieve onderwerpnamen (SAN's)|Subdomein naamruimte|
|-----|-----|-----|-----|
|SQL, MySQL|SQL- en MySQL|&#42;.dbadapter.*&lt;region>.&lt;fqdn>*<br>(Wildcard-SSL-certificaat)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Web-verkeer standaard SSL-certificaat|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>(Multi domein Wildcard-SSL-certificaat<sup>1</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|SSO|sso.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> vereist een certificaat met meerdere jokertekens alternatieve onderwerpnamen. Meerdere jokertekens SAN's op één certificaat wordt mogelijk niet ondersteund door alle openbare certificeringsinstanties 

<sup>2</sup> A &#42;. App service.  *&lt;regio >.&lt; FQDN-naam >* jokerteken certificaat kan niet worden gebruikt in plaats van deze drie certificaten (api.appservice. *&lt;regio >. &lt;FQDN-naam >*, ftp.appservice. *&lt;regio >. &lt;FQDN-naam >*, en sso.appservice. *&lt;regio >. &lt;FQDN-naam >*. Het gebruik van afzonderlijke certificaten vereist Appservice expliciet voor deze eindpunten. 


## <a name="next-steps"></a>Volgende stappen
[PKI-certificaten voor de implementatie van Azure-Stack genereren](azure-stack-get-pki-certs.md) 


