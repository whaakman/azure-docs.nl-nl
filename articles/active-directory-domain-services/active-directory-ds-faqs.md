---
title: Veelgestelde vragen - Azure Active Directory Domain Services | Microsoft Docs
description: Veelgestelde vragen over Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: c2f10b1bf3b61577a2b0616be6fa50f7639559ba
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158909"
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Frequently Asked Questions (Veelgestelde vragen)
Deze pagina vindt u antwoorden op veelgestelde vragen over Azure Active Directory Domain Services. Houd regelmatig op updates controleren.

## <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
Raadpleeg de [Troubleshooting guide](active-directory-ds-troubleshooting.md) voor oplossingen voor veelvoorkomende problemen met configureren of beheren van Azure AD Domain Services.

## <a name="configuration"></a>Configuratie
### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>Kan ik maken meerdere beheerde domeinen voor één Azure AD-directory?
Nee. U kunt alleen maken met één beheerde domein afgehandeld door de Azure AD Domain Services voor één Azure AD-directory.  

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kan ik Azure AD Domain Services in een virtueel netwerk van Azure Resource Manager inschakelen?
Ja. Azure AD Domain Services kan worden ingeschakeld in een virtueel netwerk van Azure Resource Manager. Klassieke Azure-netwerken worden niet meer ondersteund voor het maken van nieuwe beheerde domeinen.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Kan ik mijn bestaande beheerde domein migreren van een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager?
Momenteel niet. Microsoft levert een mechanisme voor het migreren van uw bestaande beheerde domein van een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager in de toekomst.

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Kan ik Azure AD Domain Services in een Azure-CSP (Cloud Solution Provider)-abonnement inschakelen?
Ja. Zie hoe u kunt inschakelen [Azure AD Domain Services in Azure CSP-abonnementen](active-directory-ds-csp.md).

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Kan ik Azure AD Domain Services in een federatieve Azure inschakelen AD-directory? Ik Synchroniseer geen wachtwoordhashes met Azure AD. Kan ik Azure AD Domain Services voor deze adreslijst inschakelen?
Nee. Azure AD Domain Services moet toegang hebben tot de wachtwoord-hashes van gebruikersaccounts voor verificatie van gebruikers via NTLM of Kerberos. Wachtwoord-hashes worden in een federatieve map, niet opgeslagen in de Azure AD-directory. Azure AD Domain Services werkt daarom niet met deze Azure AD-directory's.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kan ik Azure AD Domain Services beschikbaar maken in meerdere virtuele netwerken in mijn abonnement?
In dit scenario biedt geen rechtstreeks ondersteuning door de service zelf. Uw beheerde domein is beschikbaar in slechts één virtueel netwerk op een tijdstip. U kunt echter verbinding tussen meerdere virtuele netwerken om andere virtuele netwerken van Azure AD Domain Services beschikbaar te configureren. Ziet u hoe u [verbinding maken met virtuele netwerken in Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kan ik Azure AD Domain Services met behulp van PowerShell inschakelen?
Ja. Zie [hoe om in te schakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md).

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Kan ik Azure AD Domain Services met behulp van Resource Manager-sjabloon inschakelen?
Nee, is het niet mogelijk om in te schakelen van Azure AD Domain Services met behulp van een sjabloon. In plaats daarvan PowerShell gebruiken, raadpleegt u [hoe om in te schakelen van Azure AD Domain Services met behulp van PowerShell](active-directory-ds-enable-using-powershell.md).

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Kan ik domeincontrollers toevoegen aan een beheerd domein van Azure AD Domain Services?
Nee. Het domein dat is opgegeven door Azure AD Domain Services is een beheerd domein. Deze activiteiten worden u niet wilt inrichten, configureren of anderszins beheren-domeincontrollers voor dit domein - geleverd als een service door Microsoft. U kunt extra domeincontrollers (alleen-lezen of alleen-lezen) voor het beheerde domein daarom niet toevoegen.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>Kunnen gastgebruikers uitgenodigd voor mijn directory Azure AD Domain Services gebruiken?
Nee. Gastgebruikers uitgenodigd voor uw Azure AD-directory met de [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) uitnodiging proces zijn gesynchroniseerd met uw Azure AD Domain Services beheerde domein. Wachtwoorden voor deze gebruikers worden echter niet opgeslagen in uw Azure AD-directory. Daarom Azure AD Domain Services heeft geen manier om te synchroniseren van NTLM en Kerberos-hashes voor deze gebruikers in uw beheerde domein. Dergelijke gebruikers kunnen als gevolg hiervan, meld u aan bij het beheerde domein of computers toevoegen aan het beheerde domein.

## <a name="administration-and-operations"></a>Beheer en bewerkingen
### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kan ik verbinding maken met de domeincontroller voor mijn beheerde domein met behulp van extern bureaublad?
Nee. U bent niet gemachtigd om naar de domeincontrollers voor het beheerde domein via Extern bureaublad verbinding te maken. Leden van de groep 'AAD DC Administrators' kunnen het beheerde domein met AD-beheerprogramma's zoals de Active Directory Administration Center (ADAC) of AD PowerShell beheren. Deze hulpprogramma's zijn geïnstalleerd met behulp van de functie 'Remote Server Administration Tools' op een Windows-server toegevoegd aan het beheerde domein.

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Kan ik hebt Azure AD Domain Services ingeschakeld. Welke gebruikersaccount gebruik ik voor domein lid worden van computers aan dit domein?
Leden van de beheergroep 'AAD DC Administrators' kunnen domein-machines. Bovendien krijgen leden van deze groep de toegang tot extern bureaublad op computers die zijn samengevoegd in het domein.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Heb ik domein administrator-bevoegdheden voor het beheerde domein geleverd door Azure AD Domain Services?
Nee. U beheerdersbevoegdheden in het beheerde domein niet krijgen. Zowel ' domeinbeheerder ' en ' ondernemingsbeheerder '-bevoegdheden zijn niet beschikbaar voor gebruik binnen het domein. Leden van de domeinbeheerder of enterprise-beheerder de groepen in uw on-premises Active Directory-domein/enterprise administrator-bevoegdheden in het beheerde domein ook niet worden toegekend.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Kan ik met LDAP of andere beheerprogramma's van AD op beheerde domeinen groepslidmaatschappen wijzigen?
Nee. Lidmaatschap van groepen kunnen niet worden gewijzigd in domeinen die zijn verwerkt door Azure AD Domain Services. Hetzelfde geldt voor de kenmerken van gebruiker. U kunt echter groepslidmaatschappen of gebruikerskenmerken wijzigen in Azure AD of op uw on-premises domein. Dergelijke wijzigingen worden automatisch gesynchroniseerd met Azure AD Domain Services.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Hoe lang duurt het voordat wijzigingen ik mijn Azure AD-directory aanbrengen zichtbaar te zijn in mijn beheerde domein?
Wijzigingen die in uw Azure AD-directory met de Azure AD-gebruikersinterface of PowerShell zijn gesynchroniseerd met uw beheerde domein. Dit synchronisatieproces wordt uitgevoerd op de achtergrond. Zodra de initiële synchronisatie is voltooid, duurt het doorgaans ongeveer 20 minuten om de wijzigingen in Azure AD worden weergegeven in uw beheerde domein.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Kan ik het schema van het beheerde domein geleverd door Azure AD Domain Services verlengen?
Nee. Het schema wordt beheerd door Microsoft voor het beheerde domein. Schema-uitbreidingen worden niet ondersteund door Azure AD Domain Services.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kan ik wijzigen of toevoegen van DNS-records in mijn beheerde domein?
Ja. DNS-beheerder-bevoegdheden, voor het wijzigen van de DNS-records in het beheerde domein zijn verleend voor leden van de groep 'AAD DC Administrators'. Ze kunnen de DNS-beheer-console op een computer met Windows Server toegevoegd aan het beheerde domein gebruiken voor het beheren van DNS. Voor het gebruik van de console DNS-beheer, ' DNS-Server hulpprogramma's installeren ', dat deel van de optionele functie van 'Remote Server Administration Tools' op de server uitmaakt. Meer informatie over [hulpprogramma's voor beheer, bewaking en probleemoplossing van DNS](https://technet.microsoft.com/library/cc753579.aspx) is beschikbaar op TechNet.

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>Wat is het wachtwoordbeleid van de levensduur van een beheerd domein?
De standaardlevensduur van het wachtwoord op een Azure AD Domain Services beheerde domein is 90 dagen. De levensduur van dit wachtwoord is niet gesynchroniseerd met de levensduur van het wachtwoord geconfigureerd in Azure AD. Daarom kan er een situatie waarbij wachtwoorden van gebruikers in uw beheerde domein is verlopen, maar nog steeds geldig zijn in Azure AD. In dergelijke scenario's, moeten gebruikers hun wachtwoord wijzigen in Azure AD en het nieuwe wachtwoord wordt gesynchroniseerd met uw beheerde domein. Bovendien worden de 'wachtwoord-is-niet-verlopen' en 'user-must-change-password-at-next-logon' kenmerken voor gebruikersaccounts niet gesynchroniseerd met uw beheerde domein.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>Biedt Azure AD Domain Services beveiliging met accountvergrendeling AD?
Ja. Vijf ongeldig wachtwoord hebt ingevoerd binnen twee minuten in het beheerde domein ertoe leiden dat een gebruikersaccount om te worden vergrendeld voor 30 minuten. Het gebruikersaccount dat is na 30 minuten wordt automatisch ontgrendelen. Ongeldig wachtwoord hebt ingevoerd in het beheerde domein blokkeren in Azure AD niet van het gebruikersaccount. Het gebruikersaccount dat is alleen binnen uw Azure AD Domain Services beheerde domein vergrendeld.

## <a name="billing-and-availability"></a>Facturering en beschikbaarheid
### <a name="is-azure-ad-domain-services-a-paid-service"></a>Is dat Azure AD Domain Services een betaalde service?
Ja. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/active-directory-ds/) voor meer informatie.

### <a name="is-there-a-free-trial-for-the-service"></a>Is er een gratis proefversie voor de service?
Deze service is opgenomen in de gratis proefversie voor Azure. U kunt zich registreren voor een [gratis proefversie van één maand van Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Kan ik een beheerd domein van Azure AD Domain Services onderbreken? 
Nee. Als u een beheerd domein van Azure AD Domain Services hebt ingeschakeld, is de service is beschikbaar in het geselecteerde virtuele netwerk totdat u uitschakelen/verwijderen. het beheerde domein. Er is geen manier om de service te onderbreken. Facturering wordt op uurbasis voortgezet totdat u het beheerde domein verwijdert.

### <a name="can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event"></a>Kan ik Azure AD Domain Services naar een andere regio voor een gebeurtenis DR-failover?
Nee.  Azure AD Domain Services biedt momenteel geen een geografisch redundante implementatiemodel. Het is beperkt tot één virtueel netwerk in een Azure-regio. Als u gebruikmaken van meerdere Azure-regio's wilt, moet u uw Active Directory-domeincontrollers worden uitgevoerd op Azure IaaS VM's.  Uitleg bij architectuur kan worden gevonden [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain).

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Kan ik Azure AD Domain Services krijgen als onderdeel van Enterprise Mobility Suite (EMS)? Moet ik Azure AD Premium voor het gebruik van Azure AD Domain Services?
Nee. Azure AD Domain Services is een betalen per gebruik van de Azure-service en maakt geen deel uit van EMS. Azure AD Domain Services kan worden gebruikt met alle edities van Azure AD (gratis, Basic en, Premium). U wordt gefactureerd op uurbasis, afhankelijk van gebruik.

### <a name="what-azure-regions-is-the-service-available-in"></a>Welke Azure-regio's is de service beschikbaar is in?
Raadpleeg de [Azure Services per regio](https://azure.microsoft.com/regions/#services/) pagina voor een overzicht van Azure-regio's waar Azure AD Domain Services beschikbaar is.
