---
title: Azure Stack Public Key Infrastructure-certificaatvereisten voor Azure Stack-geïntegreerde systemen | Microsoft Docs
description: Beschrijft de vereisten voor implementatie Azure Stack PKI-certificaat voor geïntegreerde Azure Stack-systemen.
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
ms.date: 10/16/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 86e2f328968cb5e45b9aec71aac8e8ac9e6d656b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403897"
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure Stack-certificaatvereisten voor openbare-sleutelinfrastructuur

Azure Stack is een infrastructuur voor openbare-netwerk met behulp van extern toegankelijke openbare IP-adressen toegewezen aan een kleine set Azure Stack-services en mogelijk tenant-VM's. PKI-certificaten met de juiste DNS-namen voor deze eindpunten van de infrastructuur voor openbare Azure Stack zijn vereist tijdens de implementatie van Azure Stack. Dit artikel bevat informatie over:

- Welke certificaten zijn vereist voor het implementeren van Azure Stack
- Het proces voor het verkrijgen van certificaten die overeenkomt met deze specificaties
- Over het voorbereiden en gebruikt u de certificaten tijdens de implementatie valideren

> [!Note]  
> Tijdens de implementatie moet u certificaten kopiëren naar de implementatiemap die overeenkomt met de id-provider die u op basis van (Azure AD of AD FS implementeert). Als u één certificaat voor alle eindpunten gebruikt, kopieert u dit certificaatbestand in elke map implementeren zoals wordt beschreven in de onderstaande tabellen. De mapstructuur vooraf is ingebouwd in de implementatie van virtuele machine en kunt u vinden op: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Vereisten voor certificaten
De volgende lijst beschrijft de vereisten voor certificaten die nodig zijn voor Azure Stack implementeren: 
- Certificaten moeten worden uitgegeven door een interne certificeringsinstantie of een openbare certificeringsinstantie. Als een openbare certificeringsinstantie wordt gebruikt, moet deze worden opgenomen in de installatiekopie van het besturingssysteem als onderdeel van het Microsoft-programma vertrouwde basis-CA. U vindt hier de volledige lijst: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- Uw Azure Stack-infrastructuur moet toegang tot het netwerk hebben tot de locatie van de certificeringsinstantie certificaat certificaatintrekkingslijst (CRL) gepubliceerd in het certificaat. Dit CRL moet een http-eindpunt
- Wanneer u certificaten, moeten certificaten dat een uitgegeven door de dezelfde interne certificeringsinstantie gebruikt voor het ondertekenen van certificaten op de implementatie of een openbare certificeringsinstantie van bovenstaande
- Het gebruik van zelfondertekende certificaten worden niet ondersteund.
- Voor implementatie en rotatie kunt u één certificaat die betrekking hebben op alle naamruimten in de naam van het onderwerp en SAN Subject Alternative Name () velden van het certificaat gebruiken of kunt u afzonderlijke-certificaten van elk van de naamruimten hieronder die de Azure Stack Services die u van plan bent om te gebruiken is vereist. Beide benaderingen vereisen het gebruik van jokertekens voor eindpunten, waar ze zijn vereist, zoals **KeyVault** en **KeyVaultInternal**. 
- Het certificaat handtekeningalgoritme moet 3DES. Het algoritme mag niet SHA1, zoals het moet sterkere. 
- Opmaak van het certificaat moet PFX, als de openbare en persoonlijke sleutels vereist voor Azure Stack-installatie zijn. 
- De certificaat-pfx-bestanden moeten een waarde 'Digitale handtekening' en 'Keyencipherment-bit' in het veld 'Sleutelgebruik' hebben.
- De certificaat-pfx-bestanden moeten de waarden "Serververificatie (1.3.6.1.5.5.7.3.1)" en 'Clientverificatie (1.3.6.1.5.5.7.3.2)' in het veld 'Enhanced Key Usage' hebben.
- Van het certificaat "verleend aan:" veld mag niet zijn hetzelfde als de ' dat is uitgegeven door: "veld.
- De wachtwoorden op alle certificaat-pfx-bestanden moet hetzelfde zijn op het moment van implementatie
- Wachtwoord voor de certificaat-pfx is een complex wachtwoord in.
- Zorg ervoor dat de namen van certificaathouders en alternatieve namen voor onderwerpen in de overeenkomst onderwerp alternatieve naam-extensie (x509v3_config). Het veld van de alternatieve naam voor onderwerp kunt u extra hostnamen (websites, IP-adressen, algemene namen) om te worden beveiligd met een enkel SSL-certificaat opgeven.

> [!NOTE]  
> Zelf-ondertekend certificaten worden niet ondersteund.

> [!NOTE]  
> De aanwezigheid van certificeringsinstanties intermediair in van een certificaat keten van vertrouwensrelaties wordt ondersteund. 

## <a name="mandatory-certificates"></a>Verplichte certificaten
De tabel in deze sectie beschrijft de Azure Stack openbaar eindpunt PKI-certificaten die vereist voor zowel Azure AD zijn en AD FS Azure Stack-implementaties. Vereisten voor certificaten zijn gegroepeerd op het gebied, evenals de naamruimten die worden gebruikt en de certificaten die vereist zijn voor elke naamruimte. De tabel beschrijft ook de map waarin uw solution provider de verschillende certificaten per openbaar eindpunt worden gekopieerd. 

Certificaten met de juiste DNS-namen voor elk eindpunt van de infrastructuur voor openbare Azure Stack zijn vereist. DNS-naam van elk eindpunt wordt uitgedrukt in de indeling:  *&lt;voorvoegsel >.&lt; regio >. &lt;FQDN-naam >*. 

Voor uw implementatie, de [regio] en [externalfqdn] waarden moeten overeenkomen met de regio en externe domeinnamen die u hebt gekozen voor uw Azure Stack-systeem. Een voorbeeld: als de regionaam van de is *Redmond* en naam van het externe domein is *contoso.com*, de DNS-namen zou hebben de indeling *&lt;voorvoegsel >. redmond.contoso.com*. De  *&lt;voorvoegsel >* waarden zijn vooraf bepaalde door Microsoft om te beschrijven van het eindpunt dat is beveiligd door het certificaat. Bovendien de  *&lt;voorvoegsel >* waarden van de infrastructuur voor externe eindpunten, is afhankelijk van de Azure Stack-service die gebruikmaakt van het specifieke eindpunt. 

> [!note]  
> Certificaten kunnen worden opgegeven als een certificaat met één jokerteken die betrekking hebben op alle naamruimten in de velden onderwerp en SAN Subject Alternative Name (), gekopieerd naar alle mappen of als afzonderlijke certificaten voor elk eindpunt gekopieerd naar de bijbehorende map. Vergeet niet dat beide opties moeten u jokertekens-certificaten gebruiken voor eindpunten, zoals **acs** en Key Vault, waar ze vereist zijn. 

| Implementatiemap | Vereiste certificaatonderwerp en alternatieve namen voor onderwerpen (SAN) | Bereik (per regio) | Subdomein naamruimte |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Openbare-Portal | Portal. &lt;regio >. &lt;FQDN-naam > | Portals | &lt;region>.&lt;fqdn> |
| Beheerportal | adminportal. &lt;regio >. &lt;FQDN-naam > | Portals | &lt;region>.&lt;fqdn> |
| Openbare Azure Resource Manager | beheer. &lt;regio >. &lt;FQDN-naam > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| Azure Resource Manager-beheerder | adminmanagement. &lt;regio >. &lt;FQDN-naam > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| ACSBlob | *.blob.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Blob Storage | blob.&lt;region>.&lt;fqdn> |
| ACSTable | * .table. &lt;regio >. &lt;FQDN-naam ><br>(Wildcard-SSL-certificaat) | Table Storage | de tabel. &lt;regio >. &lt;FQDN-naam > |
| ACSQueue | *.queue.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Queue Storage | wachtrij. &lt;regio >. &lt;FQDN-naam > |
| KeyVault | *.vault.&lt;region>.&lt;fqdn><br>(Wildcard-SSL-certificaat) | Key Vault | kluis. &lt;regio >. &lt;FQDN-naam > |
| KeyVaultInternal | *.adminvault. &lt;regio >. &lt;FQDN-naam ><br>(Wildcard-SSL-certificaat) |  Interne Keyvault |  adminvault. &lt;regio >. &lt;FQDN-naam > |
| Beheerdersuitbreiding Host | *.adminhosting. \<regio >. \<FQDN-naam > (Wildcard-SSL-certificaten) | Beheerdersuitbreiding Host | adminhosting. \<regio >. \<FQDN-naam > |
| Host van de openbare-extensie | * .hosting. \<regio >. \<FQDN-naam > (Wildcard-SSL-certificaten) | Host van de openbare-extensie | die als host fungeert. \<regio >. \<FQDN-naam > |

Als u Azure Stack met behulp van de Azure AD-implementatiemodus implementeert, moet u alleen aanvragen van de certificaten die worden vermeld in de vorige tabel. Als u Azure Stack met behulp van de modus voor AD FS-implementatie implementeert, moet u echter ook de certificaten die worden beschreven in de volgende tabel vragen:

|Implementatiemap|Vereiste certificaatonderwerp en alternatieve namen voor onderwerpen (SAN)|Bereik (per regio)|Subdomein naamruimte|
|-----|-----|-----|-----|
|ADFS|adfs.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat)|ADFS|*&lt;region>.&lt;fqdn>*|
|Graph|graph.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat)|Graph|*&lt;region>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Alle certificaten die worden vermeld in deze sectie moeten hetzelfde wachtwoord hebben. 

## <a name="optional-paas-certificates"></a>Optionele PaaS-certificaten
Als u van plan bent de extra Azure Stack PaaS-services (SQL, MySQL en App Service) implementeren na de Azure Stack is geïmplementeerd en geconfigureerd, moet u extra certificaten om de eindpunten van de PaaS-services aanvragen. 

> [!IMPORTANT]
> De certificaten die u voor App Service, SQL en MySQL resourceproviders gebruikt moeten beschikken over de dezelfde basis-CA als die worden gebruikt voor de globale Azure Stack-eindpunten. 

De volgende tabel beschrijft de eindpunten en de certificaten die nodig zijn voor de SQL- en MySQL-adapters en voor App Service. U hoeft niet te kopiëren van deze certificaten naar de Azure Stack-implementatiemap. In plaats daarvan bieden u deze certificaten tijdens de installatie van de aanvullende resourceproviders. 

|Bereik (per regio)|Certificaat|Vereiste certificaatonderwerp en alternatieve namen voor onderwerpen (SAN's)|Subdomein naamruimte|
|-----|-----|-----|-----|
|SQL, MySQL|SQL- en MySQL|&#42;.dbadapter.*&lt;region>.&lt;fqdn>*<br>(Wildcard-SSL-certificaat)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Web-verkeer standaard SSL-certificaat|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.sso.appservice.*&lt;region>.&lt;fqdn>*<br>(Meerdere domein Wildcard-SSL-certificaat<sup>1</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|App Service|Eenmalige aanmelding|sso.appservice.*&lt;region>.&lt;fqdn>*<br>(SSL-certificaat<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> vereist een certificaat met meerdere jokertekens alternatieve namen voor onderwerpen. Meerdere jokertekens SAN's op één certificaat kan niet worden ondersteund door alle openbare certificeringsinstanties 

<sup>2</sup> A &#42;.appservice. *&lt;regio >. &lt;FQDN-naam >* certificaat met jokerteken kan niet worden gebruikt in plaats van deze drie certificaten (api.appservice. *&lt;regio >. &lt;FQDN-naam >*, ftp.appservice. *&lt;regio >. &lt;FQDN-naam >*, en sso.appservice. *&lt;regio >. &lt;FQDN-naam >*. Azure App service is expliciet het gebruik van afzonderlijke certificaten vereist voor deze eindpunten. 

## <a name="learn-more"></a>Meer informatie
Meer informatie over het [genereren van PKI-certificaten voor Azure Stack-implementatie](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Volgende stappen
[Identiteitintegratie](azure-stack-integrate-identity.md)

