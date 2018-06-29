---
title: Azure-Stack Public Key Infrastructure-certificaatvereisten voor Azure-Stack geïntegreerd systemen | Microsoft Docs
description: Beschrijft de implementatievereisten van Azure Stack PKI-certificaat voor Azure-Stack geïntegreerd systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 9a43179998e8377dfbbb1a41ba7d46936d63aedd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030152"
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure Stack PKI-certificaatvereisten

Azure-Stack is een infrastructuur voor openbare-netwerk met behulp van extern toegankelijke openbare IP-adressen die zijn toegewezen aan een klein aantal Stack Azure-services en mogelijk tenant-VM's. PKI-certificaten met de juiste DNS-namen voor deze eindpunten van de infrastructuur voor openbare Azure-Stack zijn vereist tijdens de implementatie van Azure-Stack. In dit artikel bevat informatie over:

- Welke certificaten zijn vereist voor het implementeren van Azure-Stack
- Het proces voor het verkrijgen van certificaten die overeenkomt met deze specificaties
- Het voorbereiden, valideren en gebruiken van die certificaten tijdens de implementatie

> [!Note]  
> Tijdens de implementatie moet u certificaten kopiëren naar de implementatiemap die overeenkomt met de id-provider die u tegen (Azure AD of AD FS implementeert). Als u één certificaat voor alle eindpunten gebruikt, kopieert u dit certificaatbestand in de implementatiemap van elke zoals wordt beschreven in de onderstaande tabellen. Structuur van de vooraf is ingebouwd in de implementatie van virtuele machine en kan worden gevonden op: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Certificaatvereisten
De volgende lijst beschrijft de vereisten voor certificaten die nodig zijn voor het implementeren van Azure-Stack: 
- Certificaten moeten zijn uitgegeven door een interne certificeringsinstantie of een openbare certificeringsinstantie. Als een openbare certificeringsinstantie wordt gebruikt, moet worden opgenomen in de installatiekopie van het basisbesturingssysteem als onderdeel van het Microsoft-programma vertrouwde basis-instantie. U vindt hier de volledige lijst: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- De infrastructuur van uw Azure-Stack moet toegang tot het netwerk hebben tot de locatie van de certificeringsinstantie certificaatintrekkingslijst (CRL) gepubliceerd in het certificaat. Deze CRL moet een http-eindpunt
- Wanneer u certificaten, moeten certificaten dat een uitgegeven door de dezelfde interne certificeringsinstantie gebruikt voor het ondertekenen van certificaten die zijn opgegeven bij de implementatie of een openbare certificeringsinstantie van boven
- Het gebruik van zelfondertekende certificaten worden niet ondersteund.
- Het certificaat kan een certificaat voor één jokerteken die betrekking hebben op alle naamruimten in het veld onderwerp alternatieve naam (SAN) zijn. U kunt ook kunt u afzonderlijke certificaten met jokertekens voor eindpunten, zoals **acs** en Sleutelkluis waar ze vereist zijn. 
- Het certificaat handtekeningalgoritme niet SHA1, als sterkere moet. 
- Indeling van het certificaat moet PFX, als de openbare en persoonlijke sleutels vereist voor installatie van de Azure-Stack zijn. 
- Het pfx-certificaatbestand moeten een waarde 'Digitale handtekening' en 'KeyEncipherment' in het veld 'Sleutelgebruik' hebben.
- Het pfx-certificaatbestand moeten de waarden ' Server (1.3.6.1.5.5.7.3.1) ' en 'Clientverificatie (1.3.6.1.5.5.7.3.2)' in het veld 'Enhanced Key Usage' hebben.
- Het certificaat ' verleend aan: "veld moet niet hetzelfde zijn als de ' uitgegeven door: ' veld.
- De wachtwoorden op alle certificate pfx-bestanden moeten hetzelfde zijn op het moment van implementatie
- Wachtwoord voor het certificaat pfx is een complex wachtwoord.
- Zorg ervoor dat de namen van certificaathouders en alternatieve namen in overeenkomst met de extensie (x509v3_config) van de certificaathouder alternatieve naam voor onderwerp. Het veld van de alternatieve naam voor onderwerp kunt u aanvullende hostnamen (websites, IP-adressen, algemene namen) moet worden beveiligd door één SSL-certificaat opgeven.

> [!NOTE]  
> Zelf-ondertekend certificaten worden niet ondersteund.

> [!NOTE]  
> De aanwezigheid van tussenkomst certificeringsinstanties in de keten van vertrouwensrelaties IS van een certificaat wordt ondersteund. 

## <a name="mandatory-certificates"></a>Verplichte certificaten
De tabel in deze sectie beschrijft de Azure-Stack openbaar eindpunt PKI-certificaten die vereist voor beide Azure AD zijn en Azure-Stack van AD FS-implementaties. Vereisten voor certificaten worden gegroepeerd per gebied, evenals de naamruimten die worden gebruikt en de certificaten die vereist zijn voor elke naamruimte. De tabel beschrijft ook de map waarin uw solution provider de verschillende certificaten worden per openbaar eindpunt kopieert. 

Certificaten met de juiste DNS-namen voor elk eindpunt van de infrastructuur voor openbare Azure-Stack zijn vereist. DNS-naam van elk eindpunt wordt uitgedrukt in de indeling:  *&lt;voorvoegsel >.&lt; regio >. &lt;FQDN-naam >*. 

Voor uw implementatie, het [regio] en [externalfqdn] waarden moeten overeenkomen met de regio en externe domeinnamen die u hebt gekozen voor uw Azure-Stack-systeem. Een voorbeeld: als de regionaam van de is *Redmond* en was de naam van het externe domein *contoso.com*, de DNS-namen moeten de indeling *&lt;voorvoegsel >. redmond.contoso.com*. De  *&lt;voorvoegsel >* waarden zijn vooraf bepaalde door Microsoft te beschrijven van het eindpunt dat wordt beveiligd door het certificaat. Bovendien de  *&lt;voorvoegsel >* waarden van de externe infrastructuur eindpunten is afhankelijk van de Azure-Stack-service die gebruikmaakt van de specifieke eindpunt. 

> [!note]  
> Certificaten kunnen worden opgegeven als een certificaat voor één jokerteken die betrekking hebben op alle naamruimten in de velden van het onderwerp en SAN Subject Alternative Name () gekopieerd naar alle mappen, of als afzonderlijke certificaten voor elk eindpunt gekopieerd naar de bijbehorende map. Vergeet niet dat beide opties moeten u jokertekens-certificaten gebruiken voor eindpunten zoals **acs** en Sleutelkluis waar ze vereist zijn. 

| Implementatiemap | Vereiste certificaatonderwerp en de alternatieve namen voor onderwerp (SAN) | Bereik (per regio) | Subdomein naamruimte |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Openbare-Portal | Portal. &lt;regio >. &lt;fqdn > | Portals | &lt;region>.&lt;fqdn> |
| Beheerportal | adminportal. &lt;regio >. &lt;fqdn > | Portals | &lt;region>.&lt;fqdn> |
| Openbare Azure Resource Manager | beheer. &lt;regio >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| Azure Resource Manager-beheerder | adminmanagement. &lt;regio >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| ACSBlob | *.blob.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Blob Storage | blob.&lt;region>.&lt;fqdn> |
| ACSTable | * .table. &lt;regio >. &lt;fqdn ><br>(Wildcard-SSL-certificaat) | Table Storage | de tabel. &lt;regio >. &lt;fqdn > |
| ACSQueue | *.queue.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Queue Storage | wachtrij. &lt;regio >. &lt;fqdn > |
| KeyVault | *.vault.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Key Vault | kluis. &lt;regio >. &lt;fqdn > |
| KeyVaultInternal | *.adminvault. &lt;regio >. &lt;fqdn ><br>(Wildcard-SSL-certificaat) |  Interne Keyvault |  adminvault. &lt;regio >. &lt;fqdn > |

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
|App Service|Web-verkeer standaard SSL-certificaat|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.sso.appservice.*&lt;region>.&lt;fqdn>*<br>(Multi domein Wildcard-SSL-certificaat<sup>1</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|Eenmalige aanmelding|sso.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> vereist een certificaat met meerdere jokertekens alternatieve onderwerpnamen. Meerdere jokertekens SAN's op één certificaat wordt mogelijk niet ondersteund door alle openbare certificeringsinstanties 

<sup>2</sup> A &#42;.appservice. *&lt;regio >. &lt;FQDN-naam >* jokerteken certificaat kan niet worden gebruikt in plaats van deze drie certificaten (api.appservice. *&lt;regio >. &lt;FQDN-naam >*, ftp.appservice. *&lt;regio >. &lt;FQDN-naam >*, en sso.appservice. *&lt;regio >. &lt;FQDN-naam >*. Het gebruik van afzonderlijke certificaten vereist Appservice expliciet voor deze eindpunten. 

## <a name="learn-more"></a>Meer informatie
Meer informatie over hoe [genereren van PKI-certificaten voor de implementatie van Azure-Stack](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Volgende stappen
[Identiteitintegratie](azure-stack-integrate-identity.md)

