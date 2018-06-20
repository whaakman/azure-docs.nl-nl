---
title: 'Azure Active Directory Domain Services: Functies | Microsoft Docs'
description: Functies van Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: 5dd3cde69c6aa36c3d9cb3060dc6deb59ff74a5a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214964"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Functies
De volgende functies zijn beschikbaar in Azure AD Domain Services beheerd domeinen.

* **Eenvoudige implementatie-ervaring:** u Azure AD Domain Services voor uw Azure AD-directory met een paar klikken kunt inschakelen. Uw beheerde domein bevat alleen in de cloud gebruikersaccounts en gebruikersaccounts die zijn gesynchroniseerd vanuit een on-premises adreslijst.
* **Ondersteuning voor het lid van domein:** kunt u gemakkelijk domein computers in uw beheerde domein is beschikbaar in Azure virtuele netwerk. De domein ervaring op Windows-client en Server-besturingssystemen werkt naadloos met domeinen die door Azure AD Domain Services zijn verwerkt. U kunt ook geautomatiseerde domein tooling tegen die domeinen.
* **Exemplaar van één domein per Azure AD-adreslijst:** kunt u één Active Directory-domein voor elke Azure AD-directory.
* **Domeinen met aangepaste namen maken:** kunt u domeinen met aangepaste namen (bijvoorbeeld, contoso100.com') met behulp van Azure AD Domain Services. U kunt beide geverifieerde of niet-geverifieerde domeinnamen gebruiken. U kunt eventueel ook maken een domein met het ingebouwde domeinachtervoegsel (dat wil zeggen, ' *. onmicrosoft.com') die worden aangeboden door uw Azure AD-directory.
* **Geïntegreerd met Azure AD:** hoeft niet te configureren of beheren van replicatie naar Azure AD Domain Services. Gebruikersaccounts, groepslidmaatschappen en referenties van gebruiker (wachtwoorden) van uw Azure AD-directory zijn automatisch beschikbaar zijn in Azure AD Domain Services. Nieuwe gebruikers, groepen of wijzigingen in kenmerken van uw Azure AD-tenant of uw on-premises directory worden automatisch gesynchroniseerd met Azure AD Domain Services.
* **NTLM en Kerberos-verificatie:** met ondersteuning voor NTLM en Kerberos-verificatie, kunt u toepassingen die afhankelijk van Windows-Integrated verificatie zijn implementeren.
* **Gebruik uw zakelijke referenties/wachtwoorden:** wachtwoorden voor gebruikers in uw Azure AD-tenant werken met Azure AD Domain Services. Gebruikers kunnen hun zakelijke referenties voor domein machines gebruiken, aanmelden, interactief of via Extern bureaublad en verificatie uitvoeren tegen het beheerde domein.
* **LDAP-binding & LDAP lezen ondersteuning:** kunt u toepassingen die afhankelijk van LDAP-bindingen zijn voor verificatie van gebruikers in domeinen die door Azure AD Domain Services zijn verwerkt. Bovendien kunnen ook toepassingen die gebruikmaken van LDAP-leesbewerkingen voor kenmerken van de gebruiker of computer opvragen van de map werken op basis van Azure AD Domain Services.
* **Beveiligde LDAP (LDAPS):** kunt u toegang tot de map via beveiligde LDAP (LDAPS) inschakelen. Beveiligde LDAP-toegang is beschikbaar in het virtuele netwerk standaard. U kunt beveiligde LDAP toegang echter eventueel ook inschakelen via het internet.
* **Groepsbeleid:** kunt u één ingebouwde GPO elke voor gebruikers en computers containers naar het afdwingen van compatibiliteit met beveiligingsbeleid voor gebruikersaccounts en Domeincomputers vereist. U kunt ook uw eigen aangepaste GPO's maken en toewijzen aan aangepaste organisatie-eenheden aan [beheren van Groepsbeleid](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS beheren:** leden van de groep 'AAD DC-beheerders' DNS kunnen beheren voor uw beheerde domein met de bekende DNS-beheerprogramma's zoals de DNS-beheer MMC-module.
* **Aangepaste organisatie-eenheden (OE's) maken:** kunnen leden van de groep 'AAD DC Administrators' aangepaste OE's maken in het beheerde domein. Deze gebruikers worden volledige beheerdersrechten verleend via aangepaste OE's, zodat ze kunnen toevoegen of verwijderen serviceaccounts voor groepen, computers, groepen enzovoort binnen deze aangepaste organisatie-eenheden.
* **Beschikbaar in vele Azure globale regio's:** Zie de [Azure-services per regio](https://azure.microsoft.com/regions/#services/) pagina om te bekijken van de Azure-regio's waarin Azure AD Domain Services beschikbaar is.
* **Hoge beschikbaarheid:** Azure AD Domain Services biedt hoge beschikbaarheid voor uw domein. Deze functie biedt de zekerheid van de hogere service uptime en herstelmogelijkheden bij fouten. Ingebouwde voor health monitoring biedt automatisch herstel van fouten door draaien nieuwe exemplaren wilt vervangen van mislukte exemplaren en om blijvende service voor uw domein te bieden.
* **AD-Account lockout beveiliging:** accounts van gebruikers zijn vergrendeld gedurende 30 minuten als vijf ongeldige wachtwoorden worden gebruikt binnen twee minuten. Accounts zijn automatisch gedeblokkeerd na 30 minuten.
* **Bekend beheerhulpprogramma's gebruiken:** u bekend zijn Windows Server Active Directory-beheerprogramma's zoals de Active Directory-beheercentrum of Active Directory PowerShell kunt gebruiken voor het beheer van beheerde domeinen.
