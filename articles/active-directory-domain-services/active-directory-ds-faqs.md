---
title: Veelgestelde vragen over - Azure Active Directory Domain Services | Microsoft Docs
description: Veelgestelde vragen over Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: 41e5d515dcea4a4bc2ebfec8493d44de877a263f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214726"
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Veelgestelde vragen (FAQ's)
Deze pagina antwoorden op veelgestelde vragen over Azure Active Directory Domain Services. Houd regelmatig op updates controleren.

## <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
Raadpleeg de [Troubleshooting guide](active-directory-ds-troubleshooting.md) oplossingen voor algemene problemen met het configureren van of het beheer van Azure AD Domain Services.

## <a name="configuration"></a>Configuratie
### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>Kan ik meerdere beheerde domeinen voor één maken Azure AD-directory?
Nee. U kunt slechts één beheerde domein onderhouden door Azure AD Domain Services voor één maken Azure AD-directory.  

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kan ik Azure AD Domain Services in een virtueel netwerk van Azure Resource Manager inschakelen?
Ja. Azure AD Domain Services kan worden ingeschakeld in een virtueel netwerk van Azure Resource Manager. Klassieke Azure-virtuele netwerken worden niet langer ondersteund voor het maken van nieuwe beheerde domeinen.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Kan ik mijn bestaande beheerde domein van een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager migreren?
Momenteel niet. Microsoft levert een mechanisme voor het migreren van uw bestaande beheerde domein van een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager in de toekomst.

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Kan ik Azure AD Domain Services in een Azure-CSP (Cloud Solution Provider)-abonnement inschakelen?
Ja. Zie het inschakelen van [Azure AD Domain Services in Azure CSP abonnementen](active-directory-ds-csp.md).

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Kan ik inschakelen Azure AD Domain Services in een federatieve Azure AD-directory? Ik Synchroniseer geen wachtwoord-hashes naar Azure AD. Kan ik Azure AD Domain Services voor deze directory inschakelen?
Nee. Azure AD Domain Services moet toegang tot de wachtwoord-hashes van gebruikersaccounts, gebruikers verifiëren via NTLM of Kerberos. In een federatieve map worden wachtwoord-hashes niet opgeslagen in de Azure AD-directory. Azure AD Domain Services werkt daarom niet met dergelijke Azure AD-mappen.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kan ik Azure AD Domain Services beschikbaar maken in meerdere virtuele netwerken in mijn abonnement?
De service zelf ondersteunt dit scenario niet direct. Uw beheerde domein is beschikbaar in slechts één virtueel netwerk tegelijk. U kunt echter verbinding tussen meerdere virtuele netwerken te kunnen stellen Azure AD Domain Services met andere virtuele netwerken configureren. Zie hoe u kunt [verbinding maken met virtuele netwerken in Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kan ik Azure AD Domain Services met behulp van PowerShell inschakelen?
Ja. Zie [hoe het inschakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md).

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Kan ik Azure AD Domain Services met een Resource Manager-sjabloon inschakelen?
Ja. Zie [hoe het inschakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md).

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kan ik domeincontrollers toevoegen aan een beheerd domein van Azure AD Domain Services
Nee. Het domein dat is verstrekt door Azure AD Domain Services is een beheerd domein. U hoeft niet inrichten, configureren of anderszins beheren domeincontrollers voor dit domein - worden deze activiteiten management als een service door Microsoft geleverd. U kunt extra domeincontrollers (alleen-lezen of alleen-lezen) voor het beheerde domein daarom niet toevoegen.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>Kunnen gastgebruikers uitgenodigd voor mijn directory Azure AD Domain Services gebruiken?
Nee. Gastgebruikers uitgenodigd voor uw Azure AD-directory via de [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) uitnodiging proces zijn gesynchroniseerd met uw Azure AD Domain Services beheerd domein. Wachtwoorden voor deze gebruikers worden echter niet opgeslagen in uw Azure AD-directory. Daarom Azure AD Domain Services heeft geen manier om te synchroniseren van NTLM en Kerberos-hashes voor deze gebruikers in uw beheerde domein. Als gevolg hiervan kunnen dergelijke gebruikers aanmelden bij het beheerde domein of computers toevoegen aan het beheerde domein.

## <a name="administration-and-operations"></a>Beheer en bewerkingen
### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kan ik verbinden met de domeincontroller voor mijn beheerde domein maken met extern bureaublad?
Nee. U bent niet gemachtigd verbinding maken met de domeincontrollers voor het beheerde domein via Extern bureaublad. Leden van de groep 'AAD DC Administrators' beheren het beheerde domein met AD-beheerprogramma's zoals de Active Directory Administration Center (ADAC) of AD PowerShell. Deze hulpprogramma's worden geïnstalleerd met behulp van de functie 'Remote Server Administration Tools' op een Windows-server toegevoegd aan het beheerde domein.

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ik heb de Azure AD Domain Services hebt ingeschakeld. Welke gebruikersaccount moet ik gebruiken voor de join-computers domein aan dit domein?
Leden van de beheergroep 'AAD DC-beheerders' kunnen domein machines. Bovendien krijgen leden van deze groep de toegang tot extern bureaublad op computers die aan het domein zijn samengevoegd.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Heb ik domeinadministratorrechten voor het beheerde domein verstrekt door Azure AD Domain Services?
Nee. U hebt Administrator-bevoegdheden op het beheerde domein niet krijgen. Bevoegdheden zowel ' domeinbeheerder ' en ' ondernemingsbeheerder ' zijn niet beschikbaar voor gebruik binnen het domein. Leden van de domeinbeheerder of enterprise-beheerder de groepen in uw lokale Active Directory-domein/enterprise administrator-bevoegdheden op het beheerde domein ook niet krijgen.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Kan ik met LDAP-Adreslijst of andere beheerprogramma's van AD op beheerde domeinen groepslidmaatschappen wijzigen?
Nee. Groepslidmaatschappen kunnen niet worden gewijzigd in domeinen die door Azure AD Domain Services zijn verwerkt. Hetzelfde geldt voor gebruikerskenmerken. U kunt echter groepslidmaatschappen of gebruikerskenmerken wijzigen in Azure AD of op uw lokale domein. Dergelijke wijzigingen worden automatisch gesynchroniseerd met Azure AD Domain Services.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Hoe lang duurt het voor wijzigingen ik mijn Azure AD-directory aanbrengen om te worden weergegeven in mijn beheerde domein?
Wijzigingen in uw Azure AD-map met de Azure AD-gebruikersinterface of PowerShell worden gesynchroniseerd met uw beheerde domein. Dit synchronisatieproces wordt uitgevoerd op de achtergrond. Zodra de initiële synchronisatie is voltooid, doorgaans duurt het ongeveer 20 minuten om de wijzigingen in Azure AD worden weergegeven in uw beheerde domein.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Kan ik het schema van het beheerde domein verstrekt door Azure AD Domain Services uitbreiden?
Nee. Het schema wordt beheerd door Microsoft voor het beheerde domein. Schema-uitbreidingen worden niet ondersteund door Azure AD Domain Services.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kan ik wijzigen of de DNS-records toevoegen aan mijn beheerde domein?
Ja. Leden van de groep 'AAD DC Administrators' krijgen ' DNS-beheerder '-rechten voor het wijzigen van DNS-records in het beheerde domein. De console DNS-beheer kan worden gebruikt op een computer met Windows Server is toegevoegd aan het beheerde domein voor het beheren van DNS. Voor het gebruik van de console DNS-beheer, DNS-Server hulpprogramma's installeren ', dat deel uitmaakt van de optionele functie 'Remote Server Administration Tools' op de server. Meer informatie over [hulpprogramma's voor beheer, bewaking en probleemoplossing van DNS](https://technet.microsoft.com/library/cc753579.aspx) is beschikbaar op TechNet.

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>Wat is het wachtwoordbeleid van de levensduur van een beheerd domein?
De standaardlevensduur wachtwoord op een Azure AD Domain Services beheerd domein is 90 dagen. De levensduur van dit wachtwoord is niet gesynchroniseerd met de levensduur van het wachtwoord in Azure AD is geconfigureerd. Mogelijk hebt u daarom een situatie waarbij wachtwoorden van gebruikers verloopt in uw beheerde domein, maar nog steeds geldig zijn in Azure AD. In dergelijke scenario's, moeten gebruikers hun wachtwoord wijzigen in Azure AD en het nieuwe wachtwoord wordt gesynchroniseerd met uw beheerde domein. Bovendien worden de 'wachtwoord-biedt-niet-verloopt' en 'user-must-change-password-at-next-logon' kenmerken voor gebruikersaccounts niet gesynchroniseerd met uw beheerde domein.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>Biedt Azure AD Domain Services AD-account lockout beveiliging?
Ja. Vijf ongeldig wachtwoordpogingen binnen twee minuten op het beheerde domein leiden tot een gebruikersaccount worden buitengesloten gedurende 30 minuten. Het gebruikersaccount is na 30 minuten automatisch ontgrendeld. Ongeldig wachtwoordpogingen op het beheerde domein komen niet het gebruikersaccount vergrendeld in Azure AD. Het gebruikersaccount vergrendeld alleen binnen uw Azure AD Domain Services beheerd domein.

## <a name="billing-and-availability"></a>Facturering en beschikbaarheid
### <a name="is-azure-ad-domain-services-a-paid-service"></a>Is dat Azure AD Domain Services een betaalde service?
Ja. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/active-directory-ds/) voor meer informatie.

### <a name="is-there-a-free-trial-for-the-service"></a>Is er een gratis proefversie voor de service?
Deze service is opgenomen in de gratis proefversie van Azure. U kunt zich aanmelden voor een [gratis proefversie van Azure één maand](https://azure.microsoft.com/pricing/free-trial/).

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Kan ik een beheerd domein van Azure AD Domain Services onderbreken? 
Nee. Wanneer u een beheerd domein van Azure AD Domain Services hebt ingeschakeld, is de service is beschikbaar in de geselecteerde virtuele netwerk totdat u uitschakelen/verwijderen. het beheerde domein. Er is geen manier om de service te onderbreken. Facturering blijft op uurbasis totdat u het beheerde domein verwijdert.

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Kan ik Azure AD Domain Services krijgen als onderdeel van Enterprise Mobility Suite (EMS)? Heb ik nodig Azure AD Premium, Azure AD Domain Services gebruiken?
Nee. Azure AD Domain Services is een betalen naar gebruik Azure-service en maakt geen deel uit van EMS. Azure AD Domain Services kan worden gebruikt met alle edities van Azure AD (gratis, Basic, en Premium). U wordt gefactureerd op uurbasis, afhankelijk van gebruik.

### <a name="what-azure-regions-is-the-service-available-in"></a>Welke Azure-regio's is de service beschikbaar is in?
Raadpleeg de [Azure-Services per regio](https://azure.microsoft.com/regions/#services/) pagina voor een overzicht van Azure-regio's waar Azure AD Domain Services beschikbaar is.
