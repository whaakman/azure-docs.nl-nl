---
title: Azure Active Directory bewijs van concept playbook bouwstenen | Microsoft Docs
description: Verken en implementeer snel Identity and Access Management-scenario 's
services: active-directory
keywords: Azure active directory, playbook, Proof of Concept, PoC
documentationcenter: ''
author: dstefanMSFT
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: c5f3904621dcc4fe992b2c2f8293ad706b01f713
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446772"
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory bewijs van concept playbook: bouwstenen

## <a name="catalog-of-roles"></a>Catalogus van rollen

| Rol | Beschrijving | Proof-of-concept (PoC) verantwoordelijkheid |
| --- | --- | --- |
| **Architectuur voor cloudidentiteit / ontwikkelingsteam** | Dit team is meestal degene die de oplossing ontwerpt, prototypen implementeert, goedkeuringen-stations en ten slotte draagt bewerkingen | Ze bieden de omgevingen en die evalueert de verschillende scenario's vanuit het perspectief van de beheerbaarheid |
| **On-Premises Identity operationele team** | Hiermee kunt u de andere identiteit on-premises bronnen beheren: Active Directory-Forests, LDAP-adreslijsten, HR-systemen en Federation-id-Providers. | Toegang tot on-premises resources die nodig zijn voor de PoC-scenario's.<br/>Ze moeten worden uitgevoerd voor zo min mogelijk|
| **Technische Toepassingseigenaren** | Technische eigenaars van de verschillende cloud-apps en services die kan worden geïntegreerd met Azure AD | Geef details op SaaS-toepassingen (mogelijk exemplaren voor het testen) |
| **Globale Azure AD-beheerder** | Hiermee kunt u de configuratie van de Azure AD beheren | Geef referenties op voor het configureren van de synchronisatieservice. Meestal hetzelfde als de architectuur voor Cloudidentiteit tijdens de PoC-team maar afzonderlijke tijdens de fase bewerkingen|
| **Database-team** | Eigenaren van de Database-infrastructuur | Bieden toegang tot SQL-omgeving (AD FS of Azure AD Connect) voor specifieke scenario voorbereidingen.<br/>Ze moeten worden uitgevoerd voor zo min mogelijk |
| **Netwerkteam** | Eigenaren van de netwerkinfrastructuur | Vereist toegang op het niveau van het netwerk voor de synchronisatieservers correct toegang tot de gegevensbronnen en cloudservices (firewall-regels, poorten geopend, IPSec-regels, enz.) |
| **Beveiligingsteam** | Definieert de beveiligingsstrategie, analyseert security rapporten uit verschillende bronnen en volgt via op bevindingen. | Een doel beveiliging evaluatie scenario 's |

## <a name="common-prerequisites-for-all-building-blocks"></a>Algemene vereisten voor alle bouwstenen

Hier volgen enkele vereisten die nodig zijn voor elk POC met Azure AD Premium.

| Vereiste | Resources |
| --- | --- |
| Azure AD-tenant gedefinieerd met een geldig Azure-abonnement | [Een Azure Active Directory-tenant verkrijgen](active-directory-howto-tenant.md)<br/>**Opmerking:** als u al een omgeving met Azure AD Premium-licenties hebt, kunt u een nul cap-abonnement downloaden door te navigeren naar https://aka.ms/accessaad <br/>Meer informatie over: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ en https://technet.microsoft.com/library/dn832618.aspx |
| Domeinen gedefinieerd en geverifieerd | [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Opmerking:** sommige werkbelastingen zoals Power BI kunnen een azure AD-tenant op de achtergrond hebt ingericht. Als u wilt controleren of een bepaald domein gekoppeld aan een tenant is, gaat u naar https://login.microsoftonline.com/{domain}/v2.0/.well-known/openid-configuration. Als u een geslaagde respons ophalen en vervolgens het domein al aan een tenant toegewezen is en overnemen kunnen nodig zijn. Als dit het geval is, contact op met Microsoft voor verdere richtlijnen. Meer informatie over de overname opties op: [wat is Selfserviceregistratie voor Azure?](active-directory-self-service-signup.md) |
| Azure AD Premium of EMS proefversie ingeschakeld | [Azure Active Directory Premium één maand gratis uitproberen](https://azure.microsoft.com/trial/get-started-active-directory/) |
| U hebt de Azure AD Premium of EMS-licenties toegewezen aan de PoC-gebruikers | [Licentie voor uzelf en uw gebruikers in Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Azure AD-hoofdbeheerder referenties | [Beheerdersrollen toewijzen in Azure Active Directory](users-groups-roles/directory-assign-admin-roles.md) |
| Optioneel maar ten zeerste aanbevolen: parallelle testomgeving als terugval | [Vereisten voor Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Directory-synchronisatie - synchronisatie van Wachtwoordhashes (WHS) - nieuwe installatie

Geschatte tijd om te voltooien: één uur voor minder dan 1000 PoC-gebruikers

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Server voor het uitvoeren van Azure AD Connect | [Vereisten voor Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| POC-gebruikers in hetzelfde domein en dezelfde onderdeel van een beveiligingsgroep en organisatie-eenheid | [Aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect-functies die nodig zijn voor de POC worden geïdentificeerd. | [Active Directory verbinden met Azure Active Directory - synchronisatie configureren functies](./connect/active-directory-aadconnect.md#configure-sync-features) |
| U hebt die nodig zijn referenties voor on-premises en cloudomgevingen  | [Azure AD Connect: accounts en machtigingen](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Download de nieuwste versie van Azure AD Connect | [Microsoft Azure Active Directory Connect downloaden](https://www.microsoft.com/download/details.aspx?id=47594) |
| Azure AD Connect installeren met het eenvoudigste pad: Express <br/>1. Filteren met de doel-organisatie-eenheid om de tijd Synchronisatiecyclus<br/>2. Doelset van gebruikers in de on-premises-groep kiezen.<br/>3. De functies die nodig zijn voor de andere POC-thema's implementeren | [Azure AD Connect: Aangepaste installatie: domein en OE filteren](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Aangepaste installatie: groep filteren op basis van](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: Uw on-premises identiteiten integreren met Azure Active Directory: synchronisatie-functies configureren](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Open de Azure AD Connect-gebruikersinterface en de actieve profielen voltooide (Import, synchronisatie en uitvoer) | [Azure AD Connect sync: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) (Azure AD Connect-synchronisatie: planning) |
| Open de [Azure AD-beheerportal](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), gaat u naar de blade 'Alle gebruikers', toevoegen 'Bron van instantie' kolom en om te zien die de gebruikers worden weergegeven, correct worden gemarkeerd als die afkomstig zijn van 'WindowsServer AD' | [Azure AD-beheerportal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Overwegingen

1. Bekijk de beveiligingsoverwegingen voor synchronisatie van wachtwoordhashes [hier](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).  Als de synchronisatie van wachtwoordhashes voor productiegebruikers kan definitief niet worden gebruikt, overweeg dan om de volgende alternatieven:
   * Testgebruikers maken in het productiedomein. Zorg ervoor dat u een ander account niet synchroniseren
   * Verplaatsen naar een UAT-omgeving
2.  Als u voortzetten federation wilt, is het handig zijn om de kosten die zijn gekoppeld een federatieve oplossing met on-premises id-Provider buiten de POC begrijpen en meten die tegen de voordelen die u zoekt:
    * Het is in het kritieke pad, zodat u hoeven te ontwerpen voor hoge beschikbaarheid
    * Het is een on-premises service u capaciteit plannen moet
    * Het is een on-premises service die u bewaken, beheren/patch wilt

Meer informatie: [inzicht in Office 365-identiteits- en Azure Active Directory - federatieve identiteit](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Huisstijl

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Activa (afbeeldingen, logo's, enz.); De activa hebben voor de beste visualisatie Zorg ervoor dat de aanbevolen groottes. | [Huisstijlwijzigingen naar de aanmeldingspagina in de Azure Active Directory](active-directory-branding-custom-signon-azure-portal.md) |
| Optioneel: Als de omgeving een ADFS-server heeft, toegang tot de server om aan te passen van webthema | [AD FS sign-in aanpassing door gebruikers](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Client-computer om uit te voeren van de aanmeldingservaring voor eindgebruikers |  |
| Optioneel: Mobiele apparaten om te valideren ervaring |  |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar Azure AD-beheerportal | [Azure AD-beheerportal - huisstijl van bedrijf](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Upload de items voor de aanmeldingspagina (hero-logo, kleine logo, labels, enz.). (Optioneel) hebt u AD FS, de dezelfde assets met AD FS-aanmeldingspagina's uitlijnen | [Voeg een huisstijl voor uw aanmelding en de Toegangsvensterpagina pagina's toe: aanpasbare elementen](fundamentals/customize-branding.md) |
| Wacht enkele minuten voordat de wijziging doorgevoerd |  |
| Meld u aan met de POC-gebruikersreferenties aan https://myapps.microsoft.com |  |
| Controleer of het uiterlijk in browser | [Huisstijlwijzigingen voor uw aanmelding en de Toegangsvensterpagina pagina 's](fundamentals/customize-branding.md) |
| (Optioneel) Controleer of het uiterlijk in andere apparaten |  |

### <a name="considerations"></a>Overwegingen

Als de oude uiterlijk na de aanpassing van blijft de browser-cache leegmaken en probeer het opnieuw.

## <a name="group-based-licensing"></a>Groep op basis van licentieverlening

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Alle POC gebruikers maken deel uit van een beveiligingsgroep (cloud of on-premises) | [Een groep maken en leden toevoegen in Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar de blade licenties in Azure AD-beheerportal | [Azure AD-beheerportal: licentieverlening](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| De licenties toewijzen aan de beveiligingsgroep met de POC-gebruikers. | [Licenties toewijzen aan een groep gebruikers in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Overwegingen

In het geval van problemen, gaat u naar [scenario's, beperkingen en bekende problemen bij het gebruik van groepen beheren in Azure Active Directory-licentieverlening](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>SaaS van federatieve SSO-configuratie

Geschatte tijd om te voltooien: 60 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| De testomgeving van de SaaS-toepassing beschikbaar. In deze handleiding gebruiken we ServiceNow als voorbeeld.<br/>Wij raden aan het gebruik van een exemplaar van de test om te beperken van problemen voor het navigeren door bestaande gegevens te waarborgen en toewijzingen. | Ga naar https://developer.servicenow.com/app.do#! / start het proces van het ophalen van een exemplaar van de test te starten |
| Beheerderstoegang tot de beheerconsole voor ServiceNow | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](saas-apps/servicenow-tutorial.md) |
| Doel set gebruikers de toepassing toewijzen aan. Een beveiligingsgroep met de PoC-gebruikers wordt aanbevolen. <br/>Als het maken van de groep niet haalbaar is, klikt u vervolgens de gebruikers toewijzen aan rechtstreeks naar de toepassing voor de PoC | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De zelfstudie voor alle actoren van Microsoft Documentation delen  | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](saas-apps/servicenow-tutorial.md) |
| Stel een vergadering werken en volg de stappen in de zelfstudie met elke acteur. | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](saas-apps/servicenow-tutorial.md) |
| De app toewijzen aan de groep die is geïdentificeerd in de vereisten. Als de POC voor voorwaardelijke toegang in het bereik bevat, kunt u dat later terugkeren naar en MFA, toevoegen en dergelijke. <br/>Houd er rekening mee dat hiermee in het inrichtingsproces wordt gestart (indien geconfigureerd) |  [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| Beheer van Azure AD Portal gebruiken om toe te voegen ServiceNow-toepassing vanuit galerie| [Beheer van Azure AD Portal: zakelijke toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Wat is er nieuw in Enterprise Application management in Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| In de blade 'Single sign-on' van het ServiceNow-App ' SAML gebaseerde aanmelding inschakelen " |  |
| Vul 'Meld u op URL' en 'Id' velden met uw ServiceNow-URL<br/>Schakel het selectievakje ' als u wilt nieuw certificaat activeren "<br/>en instellingen opslaan |  |
| Open 'ServiceNow configureren' blade aan de onderkant van het paneel om aangepaste instructies voor het configureren van ServiceNow weer te geven |  |
| Volg de instructies voor het configureren van ServiceNow |  |
| Op de blade 'Inrichten' van ServiceNow-App 'Automatisch' inrichting inschakelen | [Gebruikersaccount inrichten voor enterprise-apps in de nieuwe Azure-portal beheren](manage-apps/configure-automatic-user-provisioning-portal.md) |
| Wacht een paar minuten tijdens het inrichten is voltooid.  In de tussentijd kunt kunt u de inrichtingsrapporten controleren |  |
| Meld u aan bij https://myapps.microsoft.com/ als een testgebruiker die toegang heeft | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |
| Klik op de tegel voor de toepassing die zojuist is gemaakt. Toegang bevestigen |  |
| Eventueel kunt u de toepassing gebruiksrapporten controleren. Let op: Er is enige vertraging, daarom moet u wachten tot enige tijd om te zien van het verkeer in de rapporten. | [Rapporten van aanmeldingsactiviteiten in de Azure Active Directory-portal: gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |

### <a name="considerations"></a>Overwegingen

1. Hierboven [zelfstudie](saas-apps/servicenow-tutorial.md) verwijst naar de oude Azure AD-beheerervaring. Maar PoC is gebaseerd op [snelstartgids](active-directory-enterprise-apps-whats-new-azure-portal.md#quickstart-get-going-with-your-new-application-right-away) optreden.
2. Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Application management in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één plek](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS-wachtwoord SSO-configuratie

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Testomgeving voor SaaS-toepassingen. Een voorbeeld van wachtwoord eenmalige aanmelding is HipChat en Twitter. Voor een andere toepassing moet u de exacte URL van de pagina met HTML-formulier aanmelding. | [Twitter op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Testaccounts voor de toepassingen. | [Aanmelden voor Twitter](https://twitter.com/signup?lang=en)<br/>[Meld u gratis: HipChat](https://www.hipchat.com/sign_up) |
| Doel set gebruikers de toepassing toewijzen aan. Een beveiligingsgroep die de gebruikers wordt aanbevolen. | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Lokale beheerderstoegang tot een computer voor het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer, Chrome of Firefox | [Deelvenster-extensie voor toegang voor Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Deelvenster-extensie voor toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Deelvenster-extensie voor toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De browserextensie installeren | [Deelvenster-extensie voor toegang voor Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Deelvenster-extensie voor toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Deelvenster-extensie voor toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Configureren van de toepassing van de galerie | [Wat is er nieuw in Enterprise Application management in Azure Active Directory: de nieuwe en verbeterde toepassingsgalerie](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Configureren van SSO-wachtwoord | [Beheren van eenmalige aanmelding voor zakelijke apps in de nieuwe Azure portal: wachtwoord gebaseerde aanmelding](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| De app toewijzen aan de groep in de vereisten zijn geïdentificeerd | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Meld u aan bij https://myapps.microsoft.com/ als een testgebruiker die toegang heeft |  |
| Klik op de tegel voor de toepassing die zojuist is gemaakt. | [Wat is het toegangsvenster?: wachtwoord gebaseerde SSO zonder in te richten van identiteit](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Geef de referentie van toepassingen | [Wat is het toegangsvenster?: wachtwoord gebaseerde SSO zonder in te richten van identiteit](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Sluit de browser en herhaalt u de aanmelding. De gebruiker ziet dit keer ongeveer naadloze toegang tot de toepassing. |  |
| Eventueel kunt u de toepassing gebruiksrapporten controleren. Let op: Er is enige vertraging, daarom moet u wachten tot enige tijd om te zien van het verkeer in de rapporten. | [Rapporten van aanmeldingsactiviteiten in de Azure Active Directory-portal: gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |

### <a name="considerations"></a>Overwegingen

Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Application management in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één plek](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Houd rekening met de volgende vereisten:
   * Toepassing moet een bekende aanmeldings-URL
   * De aanmeldingspagina moet een HTML-formulier met een meer tekstvelden die de browser-extensies kunnen automatisch invullen bevatten. Ten minste, moet gebruikersnaam en het wachtwoord bevatten.

## <a name="saas-shared-accounts-configuration"></a>SaaS-configuratie voor gedeelde Accounts

Geschatte tijd om te voltooien: 30 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| De lijst met doeltoepassingen en de exacte aanmelden URL tevoren. U kunt bijvoorbeeld Twitter. | [Twitter op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Aanmelden voor Twitter](https://twitter.com/signup?lang=en) |
| Referentie voor deze SaaS-toepassing wordt gedeeld. | [Delen van accounts met behulp van Azure AD](active-directory-sharing-accounts.md)<br/>[Azure AD geautomatiseerde wachtwoord worden meegenomen Facebook, Twitter en LinkedIn nu in preview. -Enterprise Mobility and Security-Blog] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/ ) |
| Referenties voor ten minste twee teamleden die van hetzelfde account gebruikmaken. Ze moeten deel uitmaken van een beveiligingsgroep. | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Lokale beheerderstoegang tot een computer voor het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer, Chrome of Firefox | [Deelvenster-extensie voor toegang voor Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Deelvenster-extensie voor toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Deelvenster-extensie voor toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De browserextensie installeren | [Deelvenster-extensie voor toegang voor Internet Explorer](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Deelvenster-extensie voor toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Deelvenster-extensie voor toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Configureren van de toepassing van de galerie | [Wat is er nieuw in Enterprise Application management in Azure Active Directory: de nieuwe en verbeterde toepassingsgalerie](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Configureren van SSO-wachtwoord | [Beheren van eenmalige aanmelding voor zakelijke apps in de nieuwe Azure portal: wachtwoord gebaseerde aanmelding](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| De app toewijzen aan de groep die is geïdentificeerd in de vereisten bij het toewijzen van referenties | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Aanmelden als andere gebruikers toegang tot de app als de **hetzelfde gedeelde account.**  |  |
| Eventueel kunt u de toepassing gebruiksrapporten controleren. Let op: Er is enige vertraging, daarom moet u wachten tot enige tijd om te zien van het verkeer in de rapporten. | [Rapporten van aanmeldingsactiviteiten in de Azure Active Directory-portal: gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |


### <a name="considerations"></a>Overwegingen

Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Application management in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één plek](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Houd rekening met de volgende vereisten:
   * Toepassing moet een bekende aanmeldings-URL
   * De aanmeldingspagina moet een HTML-formulier met een meer tekstvelden die de browser-extensies kunnen automatisch invullen bevatten. Ten minste, moet gebruikersnaam en het wachtwoord bevatten.

## <a name="app-proxy-configuration"></a>App-proxyconfiguratie

Geschatte tijd om te voltooien: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Een Microsoft Azure AD basic of premium-abonnement en Azure AD-adreslijst waarvoor u een globale beheerder bent | [Azure Active Directory-edities](fundamentals/active-directory-whatis.md) |
| Een webtoepassing die wordt gehost on-premises die u wilt configureren voor externe toegang |  |
| Een server met Windows Server 2012 R2 of Windows 8.1 of hoger, waarop u de Connector voor toepassingsproxy kunt installeren | [Meer informatie over Azure AD Application Proxy connectors](manage-apps/application-proxy-connectors.md) |
| Als er een firewall in het pad, zorg ervoor dat deze geopend is zodat de Connector aanvragen HTTPS (TCP) naar de toepassingsproxy versturen kan | [Toepassingsproxy inschakelen in Azure portal: Application Proxy-vereisten](manage-apps/application-proxy-enable.md#application-proxy-prerequisites) |
| Als uw organisatie gebruikmaakt van proxy-servers verbinding maken met internet, duurt het eens naar de blog werken met bestaande on-premises proxyservers voor meer informatie over het configureren van deze plaatsen | [Werken met bestaande on-premises proxy-servers](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Een connector op de server installeren | [Toepassingsproxy inschakelen in Azure portal: Installeer en registreer de Connector](manage-apps/application-proxy-enable.md#install-and-register-a-connector) |
| De on-premises toepassing publiceren in Azure AD als een toepassing Application Proxy | [Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md) |
| Toewijzen van testgebruikers | [Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy: een testgebruiker toevoegen](manage-apps/application-proxy-publish-azure-portal.md#add-a-test-user) |
| Configureer desgewenst een eenmalige aanmelding mogelijk voor uw gebruikers | [Eenmalige aanmelding bieden met Azure AD-toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md) |
| De app testen door naar de MyApps-portal aan als toegewezen gebruiker aanmelden | https://myapps.microsoft.com |

### <a name="considerations"></a>Overwegingen

1. Terwijl het is raadzaam om de connector opslaan in uw bedrijfsnetwerk, zijn er gevallen wanneer ziet u betere prestaties in de cloud plaatst. Meer informatie: [topologie overwegingen met betrekking tot het netwerk bij het gebruik van Azure Active Directory-toepassingsproxy](manage-apps/application-proxy-network-topology.md)
2. Zie voor verdere beveiligingsdetails en hoe dit biedt een bijzonder veilige externe toegang-oplossing door alleen uitgaande verbindingen onderhouden: [beveiligingsoverwegingen voor toegang tot de apps op afstand met behulp van Azure AD-toepassingsproxy](manage-apps/application-proxy-security.md)

## <a name="generic-ldap-connector-configuration"></a>Algemene LDAP-Connector-configuratie

Geschatte tijd om te voltooien: 60 minuten

> [!IMPORTANT]
> Dit is een geavanceerde configuratie vereisen bekend zijn met FIM/MIM. Als die wordt gebruikt in productie, we adviseren vragen over deze configuratie gaat via [Premier Support](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Azure AD Connect geïnstalleerd en geconfigureerd | Bouwsteen: [synchronisatie van Directory - synchronisatie van Wachtwoordhashes](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS exemplaar voldoen aan vereisten | [Algemene LDAP-Connector technische documentatie: overzicht van de algemene LDAP-Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#overview-of-the-generic-ldap-connector) |
| Lijst van workloads die van gebruikers gebruikmaken en kenmerken die zijn gekoppeld aan deze werkbelastingen | [Azure AD Connect-synchronisatie: kenmerken gesynchroniseerd naar Azure Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Algemene LDAP-Connector toevoegen | [Algemene LDAP-Connector technische documentatie: een nieuwe Connector maken](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#create-a-new-connector) |
| Uitvoeringsprofielen maken voor de gemaakte connector (volledige import, delta-import, volledige synchronisatie, Deltasynchronisatie, exporteren) | [Maken van een beheerprofiel van het uitvoeren van Agent](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Met behulp van connectors met de Azure AD Connect Sync Service Manager](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Volledige import profiel uitvoeren en controleren, er zijn objecten in het connectorgebied | [Zoeken naar een Connector Space-Object](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Met de Azure AD Connect Sync Service Manager met behulp van connectors: Search Connector Space](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Maken van synchronisatieregels, zodat objecten in de Metaverse-kenmerken die nodig zijn voor workloads | [Azure AD Connect-synchronisatie: Best practices voor het wijzigen van de standaardconfiguratie: wijzigingen in synchronisatieregels](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect-synchronisatie: inzicht in declaratieve inrichting](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect-synchronisatie: inzicht in declaratieve inrichting expressies](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Volledige synchronisatiecyclus Start | [Azure AD Connect sync: Scheduler: Start de scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| In geval van problemen doen probleemoplossing | [Troubleshoot an object that is not synchronizing to Azure AD](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) (Problemen oplossen wanneer een object niet wordt gesynchroniseerd met Azure AD) |
| Controleer of, LDAP-gebruiker kunt aanmelden en toegang tot de toepassing | https://myapps.microsoft.com |

### <a name="considerations"></a>Overwegingen

> [!IMPORTANT]
> Dit is een geavanceerde configuratie vereisen bekend zijn met FIM/MIM. Als die wordt gebruikt in productie, we adviseren vragen over deze configuratie gaat via [Premier Support](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Groepen - gedelegeerde eigendom

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| SaaS-toepassing (federatieve SSO of wachtwoord SSO) is al geconfigureerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Cloud-groep die toegang is toegewezen aan de toepassing in #1 wordt geïdentificeerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| Referenties voor de eigenaar van de groep zijn beschikbaar | [Toegang tot resources beheren met Azure Active Directory-groepen](fundamentals/active-directory-manage-groups.md) |
| Referenties voor de Informatiemedewerker toegang tot de apps is geïdentificeerd | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Identificeren van de groep die toegang tot de toepassing is verleend en configureer de eigenaar van de opgegeven groep| [De instellingen voor een groep in Azure Active Directory beheren ](fundamentals/active-directory-groups-settings-azure-portal.md) |
| Meld u aan als de Groepseigenaar van de, Zie het groepslidmaatschap op groepen-tabblad van het toegangsvenster | [Azure Active Directory-groepen Management-pagina](https://account.activedirectory.windowsazure.com/r#/groups) |
| De Informatiemedewerker die u wilt testen toevoegen |  |
| Meld u aan als de Informatiemedewerker, bevestigt u dat de tegel is beschikbaar | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Overwegingen

Als de toepassing heeft de inrichting ingeschakeld, moet u mogelijk Wacht een paar minuten voor het inrichten is voltooid voordat u toegang tot de toepassing als de Informatiemedewerker.

## <a name="saas-and-identity-lifecycle"></a>SaaS- en levenscyclus van identiteit

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| SaaS-toepassing (federatieve SSO of wachtwoord SSO) is al geconfigureerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Cloud-groep die toegang is toegewezen aan de toepassing in #1 wordt geïdentificeerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| Referenties voor de Informatiemedewerker toegang tot de apps is geïdentificeerd | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De gebruiker te verwijderen uit de groep die de app is toegewezen aan | [Groepslidmaatschap voor gebruikers in uw Azure Active Directory-tenant beheren](fundamentals/active-directory-groups-members-azure-portal.md) |
| Wacht een paar minuten voor het ongedaan maken inrichting | [Geautomatiseerde Gebruikersinrichting voor SaaS-App in Azure AD: hoe werkt automatische inrichting?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Op een afzonderlijke browsersessie, meld u aan als de Informatiemedewerker naar Mijn apps-portal en controleer of deze tegel ontbreekt | http://myapps.microsoft.com |


### <a name="considerations"></a>Overwegingen

Het scenario PoC leavers en/of verlof scenario's afleiden. Als de gebruiker wordt uitgeschakeld in on-premises AD of verwijderd, er is niet langer een manier om aan te melden bij de SaaS-toepassing.

## <a name="self-service-access-to-application-management"></a>Selfservice voor toegang tot Application Management

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| POC-gebruikers die toegang tot de toepassingen als onderdeel van de beveiligingsgroep wordt aanvragen identificeren | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Doeltoepassing geïmplementeerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar de blade voor bedrijfstoepassingen in Azure AD-beheerportal | [Azure AD-beheerportal: Zakelijke toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| De toepassing van de vereisten met selfservice configureren | [Wat is er nieuw in Enterprise Application management in Azure Active Directory: toegang tot Self-servicetoepassingen configureren](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Meld u aan als de Informatiemedewerker naar Mijn apps-portal | http://myapps.microsoft.com |
| U ziet ' + app toevoegen ' knop in op de pagina. Gebruik dit rapport toegang tot de app voor |  |

### <a name="considerations"></a>Overwegingen

De gekozen toepassingen heeft ingericht vereisten, dus gaan direct naar de app sommige fouten kan veroorzaken. Als de toepassing gekozen ondersteunt inrichten met azure ad en deze is geconfigureerd, kunt u dit gebruiken als een goede kans om weer te geven van de hele stroom werkt end-to-end. Zie de bouwsteen voor [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) voor verdere aanbevelingen

## <a name="self-service-password-reset"></a>Selfservice voor wachtwoord opnieuw instellen

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Schakel het beheer van de self-service voor wachtwoord in uw tenant. | [Azure Active Directory-wachtwoord opnieuw instellen voor IT-beheerders](active-directory-passwords-update-your-own-password.md) |
| Schakel het terugschrijven van wachtwoorden van on-premises beheren. Houd er rekening mee moet hiervoor specifieke Azure Active Directory Connect versies | [Vereisten voor het terugschrijven van wachtwoorden](authentication/howto-sspr-writeback.md) |
| Identificeer de PoC-gebruikers die deze functionaliteit gebruiken en zorg ervoor dat ze lid zijn van een beveiligingsgroep. De gebruikers moeten niet-beheerders om te wijzen volledig waartoe de mogelijkheid | [Aanpassen: Azure AD-wachtwoordbeheer: toegang beperken tot wachtwoord opnieuw instellen](authentication/howto-sspr-writeback.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar Azure AD-beheerportal: wachtwoord opnieuw instellen | [Azure AD-beheerportal: Wachtwoord opnieuw instellen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Bepaal dat het wachtwoord opnieuw instellen. Voor testdoeleinden Implementatiemodel, kunt u telefonische oproepen en Q & A. Het verdient aanbeveling om in te schakelen van registratie moet u aan te melden bij Toegangsvenster |  |
| Meld u af en meld u aan als een Informatiemedewerker |  |
| Geef op de selfservice voor wachtwoordherstel-gegevens, zoals geconfigureerd per stap 2 | https://aka.ms/ssprsetup |
| Sluit de browser |  |
| De aanmeldingsprocedure beginnen als de Informatiemedewerker die u in stap 4 hebt gebruikt |  |
| Het wachtwoord opnieuw instellen | [Uw eigen wachtwoord bijwerken: Mijn wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md) |
| Probeer aan te melden met uw nieuwe wachtwoord naar Azure AD ook over on-premises bronnen |  |

### <a name="considerations"></a>Overwegingen

1. Als een upgrade van de Azure AD Connect wordt gebruikt om te leiden tot problemen, kunt u overwegen deze op basis van accounts in de cloud of een demo op basis van een afzonderlijke omgeving maken
2. De beheerders hebben een ander beleid en de met behulp van het beheerdersaccount dat het wachtwoord opnieuw in te kan de PoC taint en tot verwarring leiden. Zorg ervoor dat u een normaal gebruikersaccount gebruiken voor het testen van de bewerkingen opnieuw instellen


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure multi-factor Authentication met telefoongesprekken

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeren van de POC-gebruikers met MFA  |  |
| Telefoon met goede ontvangst voor MFA-controle  | [Wat is Azure Multi-Factor Authentication?](authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Navigeer naar 'Gebruikers en groepen' blade in Azure AD-beheerportal | [Azure AD-beheerportal: Gebruikers en groepen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Kies 'Alle gebruikers'-blade |  |
| In de bovenste balk de knop 'Multi-Factor Authentication' kiezen | Directe URL voor Azure MFA-portal: https://aka.ms/mfaportal |
| In de instellingen voor 'Gebruiker' selecteert u de PoC-gebruikers en ze inschakelen voor MFA | [Statuswaarden voor gebruikers in Azure Multi-Factor Authentication](authentication/howto-mfa-userstates.md) |
| Meld u aan als de PoC-gebruiker en stapsgewijs het proces proof-up  |  |

### <a name="considerations"></a>Overwegingen

1. De PoC-stappen in deze bouwsteen expliciet MFA instellen voor een gebruiker op alle aanmeldingen. Er zijn andere hulpprogramma's, zoals voorwaardelijke toegang en Identity Protection die MFA op meer betrekken gerichte scenario's. Dit is iets te overwegen bij het verplaatsen van de POC naar productie.
2. De PoC-stappen in deze bouwsteen expliciet telefoongesprekken gebruiken als de MFA-methode voor expedience. Als u van POC naar productie overstappen, wordt u aangeraden toepassingen, zoals de [Microsoft Authenticator](authentication/end-user/current/microsoft-authenticator-app-how-to.md) als de tweede factor indien mogelijk.
Meer informatie: [DRAFT NIST Special Publication 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>MFA voor voorwaardelijke toegang voor SaaS-toepassingen

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeer PoC gebruikers wilt richten op het beleid. Deze gebruikers moeten zich in een beveiligingsgroep als bereik voor het beleid voor voorwaardelijke toegang | [SaaS van federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| SaaS-toepassing is al geconfigureerd |  |
| PoC-gebruikers zijn al toegewezen aan de toepassing |  |
| Referenties voor de POC-gebruiker zijn beschikbaar |  |
| POC-gebruiker is geregistreerd voor MFA. Met behulp van een telefoon met goede ontvangst | https://aka.ms/ssprsetup |
| Apparaat in het interne netwerk. IP-adres geconfigureerd in het interne-adresbereik | Uw ip-adres vinden: https://www.bing.com/search?q=what%27s+my+ip |
| Apparaat in het externe netwerk (kan een telefoon met behulp van het mobiele netwerk van de provider zijn) |  |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar Azure AD-beheerportal: blade voorwaardelijke toegang | [Azure AD-beheerportal: Voorwaardelijke toegang](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Maak beleid voor voorwaardelijke toegang:<br/>-Gebruikers van de PoC target onder 'Gebruikers en groepen'<br/>-PoC doeltoepassing onder 'Cloud-apps'<br/>-Alle locaties doel alleen vertrouwde die onder "Omstandigheden"-"Locatie" > **Opmerking:** goedgekeurde IP-adressen zijn geconfigureerd in [MFA-Portal](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Multi-factor authentication onder "Verlenen" vereisen | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: stappen voor het configureren van beleid](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Toegang tot toepassing uit binnen bedrijfsnetwerk | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: het beleid testen](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Toegang tot de toepassing van openbaar netwerk | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: het beleid testen](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Overwegingen

Als u van Federatie gebruikmaakt, kunt u de on-premises id-Provider (IdP) gebruiken om te communiceren, de status binnen/buiten-bedrijfsnetwerk met claims. U kunt deze techniek gebruiken zonder het beheren van de lijst met IP-adressen die mogelijk lastig zijn om te beoordelen en beheren in grote organisaties. In dat de installatie, moet u rekening voor het scenario 'netwerk roaming' (een gebruiker de logboekregistratie van het interne netwerk, en tijdens het aangemelde switches locaties, zoals een restaurant) en zorg ervoor dat u de implicaties begrijpt. Meer informatie: [cloudresources beveiligen met Azure multi-factor Authentication en AD FS: goedgekeurde IP-adressen voor federatieve gebruikers](authentication/howto-mfa-adfs.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeren van de globale beheerder, die deel van de POC voor PIM uitmaken | [Start met behulp van Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| De globale beheerder die de beheerder van de beveiliging identificeren | [Start met behulp van Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Andere beheerdersrollen in Azure Active Directory PIM](active-directory-privileged-identity-management-roles.md) |
| Optioneel: Als de globale beheerders toegang tot e-mail hebben bij de uitoefening van e-mailmeldingen in PIM bevestigen | [Wat is Azure AD Privileged Identity Management?: Configureer de instellingen voor rolactivering](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Meld u aan bij https://portal.azure.com als een globale beheerder (GA) en een bootstrap de PIM-blade. De globale beheerder, waarmee u deze stap is gemaakt als de beveiligingsbeheerder van de.  Noemen we deze actor GA1 | [Met behulp van de beveiligingswizard in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Identificeren van de globale beheerder en ze verplaatsen van permanente naar in aanmerking komt. Dit moet een afzonderlijke admin van de die werd gebruikt in stap 1 voor de duidelijkheid zijn. Noemen we deze actor GA2 | [Azure AD Privileged Identity Management: Het toevoegen of verwijderen van een gebruikersrol](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Wat is Azure AD Privileged Identity Management?: Configureer de instellingen voor rolactivering](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Nu aanmelden als GA2 naar https://portal.azure.com en wijzig 'gebruikersinstellingen'. U ziet een aantal opties zijn niet beschikbaar. | |
| In een nieuw tabblad en in dezelfde sessie als stap 3, gaat u nu naar https://portal.azure.com en de PIM-blade toevoegen aan het dashboard. | [Activeren of deactiveren van rollen in Azure AD Privileged Identity Management: de Privileged Identity Management-toepassing toevoegen](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Activering van de aanvraag aan de rol globale beheerder | [Activeren of deactiveren van rollen in Azure AD Privileged Identity Management: een rol activeren](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Houd er rekening mee dat als GA2 nooit geregistreerd voor MFA, registratie voor Azure MFA niet nodig |  |
| Ga terug naar het oorspronkelijke tabblad in stap 3 en klik op de vernieuwknop in de browser. Houd er rekening mee dat u nu toegang hebt tot het wijzigen van "Gebruikersinstellingen" | |
| (Optioneel) als uw globale beheerders waarvoor e-mail is ingeschakeld hebben, kunt u GA1 en GA2 van postvak in en de melding van de rol wordt geactiveerd |  |
| 8 controleren van de controlegeschiedenis en bekijk het rapport om te bevestigen van de uitbreiding van GA2 wordt weergegeven. | [Wat is Azure AD Privileged Identity Management?: rol activiteiten controleren](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium P2 en/of EMS E5

## <a name="discovering-risk-events"></a>Risicogebeurtenissen detecteren

Geschatte tijd om te voltooien: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaat met Tor browser gedownload en geïnstalleerd | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Toegang tot POC gebruiker de aanmelding | [Playbook voor Azure Active Directory Identity Protection](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Open tor-browser | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Meld u aan bij https://myapps.microsoft.com met de POC-gebruikersaccount | [Azure Active Directory Identity Protection-playbook: Risicogebeurtenissen simuleren](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Wacht 5-7 minuten |  |
| Meld u aan als een globale beheerder zijn om https://portal.azure.com en open de blade Identity Protection | https://aka.ms/aadipgetstarted |
| Open de blade van de gebeurtenissen risico. Ziet u een item onder 'Aanmeldingen vanaf anonieme IP-adressen'  | [Azure Active Directory Identity Protection-playbook: Risicogebeurtenissen simuleren](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium P2 en/of EMS E5

## <a name="deploying-sign-in-risk-policies"></a>Implementeren van beleid aanmelden voor risico 's

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaat met Tor browser gedownload en geïnstalleerd | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Toegang als een gebruiker POC aan het logboek in testen |  |
| POC-gebruiker is geregistreerd met MFA. Zorg ervoor dat u een telefoon met goede ontvangst | Bouwsteen: [Azure multi-factor Authentication met telefoongesprekken](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Meld u aan als een globale beheerder zijn om https://portal.azure.com en open de blade Identity Protection | https://aka.ms/aadipgetstarted |
| Een beleid voor aanmeldingsrisico als volgt inschakelen:<br/>-Toegewezen aan: POC-gebruiker<br/>-Voorwaarden: Aanmeldingsrisico middelgrote of hoger (aanmelding van de anonieme locatie als wordt beschouwd als een gemiddeld risico-niveau)<br/>-Besturingselementen: MFA vereisen | [Azure Active Directory Identity Protection-playbook: aanmeldingsrisico](active-directory-identityprotection-playbook.md) |
| Open tor-browser | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Meld u aan bij https://myapps.microsoft.com met de PoC-gebruikersaccount |  |
| U ziet dat de MFA-controle | [Aanmelden-ervaringen met Azure AD Identity Protection: riskante aanmelding herstel](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium P2 en/of EMS E5. Voor meer informatie over risicogebeurtenissen gaat u naar: [risicogebeurtenissen in Azure Active Directory](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Verificatie op basis van certificaten configureren

Geschatte tijd in beslag: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaat met gebruikerscertificaat ingericht (Windows, iOS of Android) van de Enterprise PKI | [Gebruikerscertificaten implementeren](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD-domein gefedereerd met behulp van AD FS | [Azure AD Connect en federatie](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Overzicht van Active Directory Certificate Services](https://technet.microsoft.com/library/hh831740.aspx)|
| Voor iOS-apparaten Microsoft Authenticator-app hebt geïnstalleerd | [Aan de slag met de Microsoft Authenticator-app](authentication/end-user/current/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| "Verificatie met gebruikerscertificaat" op de AD FS inschakelen | [Verificatiebeleid configureren: Primaire authenticatie globaal in Windows Server 2012 R2 configureren](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Optioneel: Authenticatie via certificaat inschakelen in Azure AD voor Exchange ActiveSync-clients | [Aan de slag met verificatie op basis van certificaten in Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Navigeer naar het toegangsvenster en verificatie met gebruikerscertificaat | https://myapps.microsoft.com |

### <a name="considerations"></a>Overwegingen

Voor meer informatie over aanvullende opmerkingen van deze implementatie gaat u naar: [ADFS: verificatie via certificaat met Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Bezit is van het gebruikerscertificaat moet worden bewaakt. Een door het beheer van apparaten of met een PINCODE in het geval van smartcards.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
