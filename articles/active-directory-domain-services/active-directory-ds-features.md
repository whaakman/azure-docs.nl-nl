---
title: 'Azure Active Directory Domain Services: Functies | Microsoft Docs'
description: Functies van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: af0670a44937bb14df9afc206ea4601c0b9289a2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Functies
De volgende functies zijn beschikbaar in Azure AD Domain Services beheerd domeinen.

* **Eenvoudige implementatie-ervaring:** u Azure AD Domain Services voor uw Azure AD-tenant met behulp van een paar klikken kunt inschakelen. Ongeacht of uw Azure AD-tenant is van een cloud-tenant of gesynchroniseerd met uw on-premises directory, kan uw beheerde domein snel worden ingericht.
* **Ondersteuning voor het lid van domein:** kunt u gemakkelijk domein computers in uw beheerde domein is beschikbaar in Azure virtuele netwerk. De domein ervaring op Windows-client en Server-besturingssystemen werkt naadloos met domeinen die door Azure AD Domain Services zijn verwerkt. U kunt ook geautomatiseerde domein tooling tegen die domeinen.
* **Exemplaar van één domein per Azure AD-adreslijst:** kunt u één Active Directory-domein voor elke Azure AD-directory.
* **Domeinen met aangepaste namen maken:** kunt u domeinen met aangepaste namen (bijvoorbeeld, contoso100.com') met behulp van Azure AD Domain Services. U kunt beide geverifieerde of niet-geverifieerde domeinnamen gebruiken. U kunt eventueel ook maken een domein met het ingebouwde domeinachtervoegsel (dat wil zeggen, ' *. onmicrosoft.com') die worden aangeboden door uw Azure AD-directory.
* **Geïntegreerd met Azure AD:** hoeft niet te configureren of beheren van replicatie naar Azure AD Domain Services. Gebruikersaccounts, groepslidmaatschappen en referenties van gebruiker (wachtwoorden) van uw Azure AD-directory zijn automatisch beschikbaar zijn in Azure AD Domain Services. Nieuwe gebruikers, groepen of wijzigingen in kenmerken van uw Azure AD-tenant of uw on-premises directory worden automatisch gesynchroniseerd met Azure AD Domain Services.
* **NTLM en Kerberos-verificatie:** met ondersteuning voor NTLM en Kerberos-verificatie, kunt u toepassingen die afhankelijk van geïntegreerde Windows-verificatie zijn implementeren.
* **Gebruik uw zakelijke referenties/wachtwoorden:** wachtwoorden voor gebruikers in uw Azure AD-tenant werken met Azure AD Domain Services. Gebruikers kunnen hun zakelijke referenties voor domein machines gebruiken, aanmelden, interactief of via Extern bureaublad en verificatie uitvoeren tegen het beheerde domein.
* **LDAP-binding & LDAP lezen ondersteuning:** kunt u toepassingen die afhankelijk van LDAP-bindingen zijn voor verificatie van gebruikers in domeinen die door Azure AD Domain Services zijn verwerkt. Bovendien kunnen ook toepassingen die gebruikmaken van LDAP-leesbewerkingen voor kenmerken van de gebruiker of computer opvragen van de map werken op basis van Azure AD Domain Services.
* **Beveiligde LDAP (LDAPS):** kunt u toegang tot de map via beveiligde LDAP (LDAPS) inschakelen. Beveiligde LDAP-toegang is beschikbaar in het virtuele netwerk standaard. U kunt beveiligde LDAP toegang echter eventueel ook inschakelen via het internet.
* **Groepsbeleid:** kunt u één ingebouwde GPO elke voor gebruikers en computers containers naar het afdwingen van compatibiliteit met beveiligingsbeleid voor gebruikersaccounts en Domeincomputers vereist. U kunt ook uw eigen aangepaste GPO's maken en toewijzen aan aangepaste organisatie-eenheden aan [beheren van Groepsbeleid](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS beheren:** leden van de groep 'AAD DC-beheerders' DNS kunnen beheren voor uw beheerde domein met de bekende DNS-beheerprogramma's zoals de DNS-beheer MMC-module.
* **Aangepaste organisatie-eenheden (OE's) maken:** kunnen leden van de groep 'AAD DC Administrators' aangepaste OE's maken in het beheerde domein. Deze gebruikers worden volledige beheerdersrechten verleend via aangepaste OE's, zodat ze kunnen toevoegen of verwijderen serviceaccounts voor groepen, computers, groepen enzovoort binnen deze aangepaste organisatie-eenheden.
* **Beschikbaar in meerdere Azure-regio's:** Zie de [Azure-services per regio](https://azure.microsoft.com/regions/#services/) pagina om te bekijken van de Azure-regio's waarin Azure AD Domain Services beschikbaar is.
* **Hoge beschikbaarheid:** Azure AD Domain Services biedt hoge beschikbaarheid voor uw domein. Deze functie biedt de zekerheid van de hogere service uptime en herstelmogelijkheden bij fouten. Ingebouwde voor health monitoring biedt automatisch herstel van fouten door draaien nieuwe exemplaren wilt vervangen van mislukte exemplaren en om blijvende service voor uw domein te bieden.
* **Bekend beheerhulpprogramma's gebruiken:** u bekend zijn Windows Server Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum of Active Directory PowerShell kunt gebruiken voor het beheer van beheerde domeinen.
