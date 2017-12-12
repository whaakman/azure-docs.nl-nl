---
title: Azure Active Directory bewijs van concept playbook bouwstenen | Microsoft Docs
description: Verkennen en snel implementeren scenario's voor Identity and Access Management
services: active-directory
keywords: Azure active directory, playbook, Proof-of-Concept, implementatiemodel
documentationcenter: 
author: dstefanMSFT
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: bc0bc80b45e97efc048d9a9c26b8dd5d5f39ce8d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory bewijs van concept playbook: bouwstenen

## <a name="catalog-of-roles"></a>Catalogus met rollen

| Rol | Beschrijving | Proof-of-concept (PoC) verantwoordelijkheid |
| --- | --- | --- |
| **Architectuur van de identiteit / ontwikkelteam** | Dit team is gewoonlijk degene die de oplossing te ontwerpen, prototypen implementeert, goedkeuringen stations en tot slot draagt aan bewerkingen | Ze bieden de omgevingen en zijn de evaluatie van de verschillende scenario's vanuit het perspectief van de beheerbaarheid |
| **On-Premises Identity operationele team** | Beheert de identiteit van de verschillende bronnen on-premises: Active Directory-Forests, LDAP-adreslijsten, HR-systemen en federatieve id-Providers. | Toegang tot on-premises resources die nodig zijn voor de PoC-scenario's.<br/>Ze moeten zo weinig mogelijk betrokken|
| **Technische Toepassingseigenaars** | Technische eigenaren van de verschillende cloud-apps en services die is geïntegreerd met Azure AD | Gedetailleerde informatie bevat over SaaS-toepassingen (mogelijk exemplaren voor het testen) |
| **Globale beheerder van Azure AD** | Beheert de configuratie van Azure AD | Geef referenties op voor het configureren van de synchronisatieservice. Meestal hetzelfde als de Identity-architectuur in een team tijdens de POC-fase maar afzonderlijke tijdens de fase bewerkingen|
| **Database-team** | Eigenaren van de Database-infrastructuur | Toegang tot SQL-omgeving (AD FS of Azure AD Connect) voor specifieke scenario voorbereidingen bieden.<br/>Ze moeten zo weinig mogelijk betrokken |
| **Netwerkteam** | Eigenaren van de netwerkinfrastructuur | Vereist toegang op het niveau voor de synchronisatieservers goed toegang tot de gegevensbronnen en cloudservices (firewall-regels, poorten die worden geopend, IPSec-regels, enz.) |
| **Beveiligingsteam** | Definieert de beveiligingsstrategie, analyseert beveiligingsrapporten uit diverse bronnen en volgt via op bevindingen. | Doel-beveiliging bieden evaluatie scenario 's |

## <a name="common-prerequisites-for-all-building-blocks"></a>Algemene vereisten voor alle bouwstenen

Hier volgen enkele vereisten die nodig zijn voor een Implementatiemodel met Azure AD Premium.

| Vereiste | Resources |
| --- | --- |
| Azure AD-tenant is gedefinieerd met een geldige Azure-abonnement | [Een Azure Active Directory-tenant verkrijgen](active-directory-howto-tenant.md)<br/>**Opmerking:** als u al een omgeving met Azure AD Premium-licenties hebt, kunt u een nul cap-abonnement downloaden door te navigeren naar https://aka.ms/accessaad <br/>Meer informatie op: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ en https://technet.microsoft.com/library/dn832618.aspx |
| Domeinen gedefinieerd en geverifieerd | [Een aangepaste domeinnaam toevoegen aan Azure Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Opmerking:** sommige werkbelastingen, zoals Power BI kunnen een azure AD-tenant onder de dekt hebt ingericht. Om te zien als een bepaald domein gekoppeld aan een tenant is, gaat u naar https://login.microsoftonline.com/ {domain}/v2.0/.well-known/openid-configuration. Als u een geslaagd antwoord, ophalen en vervolgens het domein al aan een tenant toegewezen is en neemt mogelijk nodig. Als dit het geval is, moet u contact op met Microsoft voor verdere informatie. Meer informatie over de overname-opties op: [wat is Selfserviceregistratie voor Azure?](active-directory-self-service-signup.md) |
| Azure AD Premium of EMS-proefabonnement Enabled | [Azure Active Directory Premium voor één maand gratis](https://azure.microsoft.com/trial/get-started-active-directory/) |
| U hebt Azure AD Premium of EMS-licenties toegewezen aan gebruikers van de POC-fase | [Licentie uzelf en uw gebruikers in Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Azure globale beheerder van AD-referenties | [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) |
| Optioneel maar ten zeerste aanbevolen: parallelle testomgeving als terugval | [Vereisten voor Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Nieuwe installatie van Directory - Wachtwoordhashsynchronisatie (PBS) - synchronisatie

Geschatte tijd om te voltooien: één uur voor minder dan 1000 gebruikers van de POC-fase

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Server voor het uitvoeren van Azure AD Connect | [Vereisten voor Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| Doelgroep is gebruikers van de POC-fase, in hetzelfde domein en deel uitmaken van een beveiligingsgroep en organisatie-eenheid | [Aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Azure AD Connect-functies die nodig zijn voor de POC-fase worden geïdentificeerd | [Verbinding maken met Active Directory met Azure Active Directory - synchronisatie instellen functies](./connect/active-directory-aadconnect.md#configure-sync-features) |
| U hebt referenties nodig voor on-premises en in de cloud omgevingen  | [Azure AD Connect: accounts en machtigingen](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Download de nieuwste versie van Azure AD Connect | [Microsoft Azure Active Directory Connect downloaden](https://www.microsoft.com/download/details.aspx?id=47594) |
| Azure AD Connect installeren met het eenvoudigste pad: Express <br/>1. Filteren met de doel-organisatie-eenheid om de tijd Synchronisatiecyclus<br/>2. Kies doelset gebruikers in de lokale groep.<br/>3. De functies die nodig is voor de overige Implementatiemodel thema's implementeren | [Azure AD Connect: Aangepaste installatie: domein en OE filteren](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Aangepaste installatie: groep gebaseerd filteren](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[Azure AD Connect: Uw on-premises identiteiten integreren met Azure Active Directory: synchronisatie-functies configureren](./connect/active-directory-aadconnect.md#configure-sync-features) |
| De Azure AD Connect-gebruikersinterface openen en bekijken van de actieve profielen voltooide (Import, synchronisatie en exporteren) | [Azure AD Connect sync: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) (Azure AD Connect-synchronisatie: planning) |
| Open de [Azure AD-beheerportal](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), gaat u naar de blade 'Alle gebruikers', toevoegen 'Bron van de autorisaties' kolom en om te zien die de gebruikers worden weergegeven, correct worden gemarkeerd als het afkomstig is van 'WindowsServer AD' | [Azure AD-beheerportal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Overwegingen

1. Bekijk de beveiligingsoverwegingen voor wachtwoordhashsynchronisatie [hier](./connect/active-directory-aadconnectsync-implement-password-synchronization.md).  Als wachtwoordhashsynchronisatie voor productie-gebruikers zich niet definitief een optie, kunt u de volgende alternatieven:
   * Testgebruikers maken in het productiedomein. Zorg ervoor dat u een andere account niet synchroniseren
   * Verplaatsen naar een UAT-omgeving
2.  Als u voortzetten federation wilt, is het nuttig om te begrijpen de kosten die een federatieve oplossing is gekoppeld aan de lokale id-Provider afgezien van de POC-fase en meting die tegen de voordelen die u zoekt:
    * Het is in het kritieke pad zodat u hebt voor een ontwerp voor maximale beschikbaarheid
    * Het is een lokale service u capaciteit plannen moet
    * Het is een lokale service die u bewaken, onderhouden/patch wilt

Meer informatie: [identiteit Understanding Office 365 en Azure Active Directory - federatieve identiteit](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>De huisstijl

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Activa (afbeeldingen, logo's, enzovoort); De activa hebben voor de beste visualisatie Zorg ervoor dat de aanbevolen grootte. | [Huisstijl aan uw pagina aanmelden in de Azure Active Directory toevoegen](active-directory-branding-custom-signon-azure-portal.md) |
| Optioneel: Als de omgeving heeft de ADFS-server, toegang tot de server om aan te passen webthema | [AD FS-gebruiker aanmelden aanpassing](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| De clientcomputer om uit te voeren van de eindgebruikerservaring voor aanmelding |  |
| Optioneel: Mobiele apparaten voor het valideren van de gebruikerservaring |  |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar Azure AD-beheerportal | [Azure AD-beheerportal - huisstijl van uw bedrijf](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Upload de activa voor de aanmeldingspagina (hero logo, kleine logo, labels, enzovoort). Als u AD FS hebt, uitlijnen eventueel de dezelfde activa met AD FS-aanmeldingspagina 's | [Huisstijl aan uw aanmeldingspagina en pagina's toevoegen: aanpasbare elementen](customize-branding.md) |
| Wacht een paar minuten voordat de wijziging doorgevoerd |  |
| Meld u aan met de gebruikersreferenties Implementatiemodel met https://myapps.microsoft.com |  |
| Bevestig het uiterlijk in browser | [Huisstijl aan uw aanmeldingspagina en pagina's toevoegen](customize-branding.md) |
| Eventueel, bevestig het uiterlijk in andere apparaten |  |

### <a name="considerations"></a>Overwegingen

Als het oude uiterlijk na de aanpassing blijft leegmaken van de clientcache browser en probeer het opnieuw.

## <a name="group-based-licensing"></a>Groeperen op basis van de licentieverlening

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Alle gebruikers van de POC-fase uitmaken deel van een beveiligingsgroep (cloud of on-premises) | [Een groep maken en leden toevoegen in Azure Active Directory](active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar de blade licenties in Azure AD-beheerportal | [Azure AD-beheerportal: licentieverlening](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| De licenties toewijzen aan de beveiligingsgroep met gebruikers van de POC-fase. | [Licenties toewijzen aan een groep gebruikers in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md) |

### <a name="considerations"></a>Overwegingen

In geval van problemen, gaat u naar [scenario's, beperkingen en bekende problemen met behulp van groepen voor licentieverlening in Azure Active Directory beheren](active-directory-licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>Configuratie van de SaaS-federatieve SSO

Geschatte tijd om te voltooien: 60 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| De testomgeving van de SaaS-toepassingen beschikbaar. In deze handleiding gebruiken we ServiceNow als voorbeeld.<br/>Wij raden een test-exemplaar gebruiken om u te minimaliseren wrijving op de kwaliteit van de bestaande gegevens en toewijzingen te navigeren. | Ga naar https://developer.servicenow.com/app.do#! / home het proces van het ophalen van een exemplaar van de test te starten |
| Beheerderstoegang tot de beheerconsole ServiceNow | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Het doel ingesteld van de gebruikers de toepassing toewijzen aan. Een beveiligingsgroep met de PoC-gebruikers wordt aanbevolen. <br/>Als het maken van de groep is niet haalbaar, klikt u vervolgens de gebruikers toewijzen aan rechtstreeks naar de toepassing voor de POC-fase | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De zelfstudie voor alle actoren van Microsoft Documentation delen  | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| Stelt een vergadering werken en de zelfstudie stappen met elke acteur. | [Zelfstudie: Azure Active Directory-integratie met ServiceNow](active-directory-saas-servicenow-tutorial.md) |
| De app toewijzen aan de groep die in de vereisten zijn geïdentificeerd. Als de POC-fase voorwaardelijke toegang in het bereik heeft, kunt u dat later terugkeren naar en toevoegen van MFA en vergelijkbare. <br/>Houd er rekening mee dat wordt dit in het inrichtingsproces starten (indien geconfigureerd) |  [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Gebruik van Azure AD management Portal ServiceNow-toepassing toevoegen vanuit galerie| [Azure AD management Portal: bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| "Eenmalige aanmelding' Blade van ServiceNow-App ' op basis van SAML-aanmelding inschakelen" |  |
| Vul 'Aanmelding op URL' en 'Id' velden aan uw ServiceNow-URL<br/>Schakel het selectievakje in om ' nieuwe certificaat activeren '<br/>en instellingen op te slaan |  |
| Open 'ServiceNow configureren' blade aan de onderkant van het paneel aangepaste instructies voor het configureren van ServiceNow weergeven |  |
| Volg de instructies voor het configureren van ServiceNow |  |
| Op de blade 'Inrichten' van ServiceNow App 'Automatisch' inrichting inschakelen | [Het beheer van gebruikersaccount inrichten voor zakelijke apps in de nieuwe Azure portal](active-directory-enterprise-apps-manage-provisioning.md) |
| Wacht een paar minuten tijdens het inrichten is voltooid.  In de tussentijd kunt u controleren in de inrichting rapporten |  |
| Meld u aan bij https://myapps.microsoft.com/ als een testgebruiker die toegang heeft | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |
| Klik op de tegel voor de toepassing die is gemaakt. Toegang bevestigen |  |
| U kunt eventueel de gebruiksrapporten van de toepassing controleren. Opmerking: Er is enige vertraging worden bijgewerkt, dus u wacht enige tijd moet en het verkeer in de rapporten. | [Aanmeldingsactiviteiten rapporten in de Azure Active Directory-portal: informatie over het gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |

### <a name="considerations"></a>Overwegingen

1. Hierboven [zelfstudie](active-directory-saas-servicenow-tutorial.md) verwijst naar de oude Azure AD-beheerervaring. Maar implementatiemodel is gebaseerd op [snel starten](active-directory-enterprise-apps-whats-new-azure-portal.md#quick-start-get-going-with-your-new-application-right-away) optreden.
2. Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één locatie](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>SaaS-wachtwoord SSO-configuratie

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| De testomgeving voor SaaS-toepassingen. Een voorbeeld van een wachtwoord SSO is HipChat en Twitter. Voor een andere toepassing moet u de exacte URL van de pagina met HTML-aanmeldingspagina formulier. | [Twitter op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Accounts voor de toepassingen testen. | [Aanmelden voor Twitter](https://twitter.com/signup?lang=en)<br/>[Aanmelden voor gratis: HipChat](https://www.hipchat.com/sign_up) |
| Het doel ingesteld van de gebruikers de toepassing toewijzen aan. Een beveiligingsgroep bevat de gebruikers wordt aanbevolen. | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokale beheerderstoegang tot een computer voor het implementeren van de uitbreiding van het Configuratiescherm toegang voor Internet Explorer, Chrome of Firefox | [Configuratiescherm-extensie voor IE toegang](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Uitbreiding van het Configuratiescherm toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Uitbreiding van het Configuratiescherm toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De Browseruitbreiding voor de installeren | [Configuratiescherm-extensie voor IE toegang](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Uitbreiding van het Configuratiescherm toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Uitbreiding van het Configuratiescherm toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Toepassing vanuit galerie configureren | [Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: de nieuwe en verbeterde-toepassingsgalerie](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Wachtwoord eenmalige aanmelding configureren | [Het beheren van eenmalige aanmelding voor zakelijke apps in de nieuwe Azure portal: Meld u op wachtwoord gebaseerde](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Toewijzen van de app aan de groep die in de vereisten zijn geïdentificeerd | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Meld u aan bij https://myapps.microsoft.com/ als een testgebruiker die toegang heeft |  |
| Klik op de tegel voor de toepassing die is gemaakt. | [Wat is het toegangsvenster?: SSO zonder identiteit inrichten op basis van wachtwoorden](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| De referentie van de toepassing opgeven | [Wat is het toegangsvenster?: SSO zonder identiteit inrichten op basis van wachtwoorden](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Sluit de browser en Herhaal de aanmelding. De gebruiker ziet dit keer ongeveer naadloze toegang tot de toepassing. |  |
| U kunt eventueel de gebruiksrapporten van de toepassing controleren. Opmerking: Er is enige vertraging worden bijgewerkt, dus u wacht enige tijd moet en het verkeer in de rapporten. | [Aanmeldingsactiviteiten rapporten in de Azure Active Directory-portal: informatie over het gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |

### <a name="considerations"></a>Overwegingen

Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één locatie](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Houd rekening met de volgende vereisten:
   * Toepassing moet een bekende aanmeldings-URL
   * De aanmeldingspagina moet een HTML-formulier met één meer tekstvelden die de browser-extensies kunnen automatisch invullen bevatten. Minimaal de moet gebruikersnaam en het wachtwoord bevatten.

## <a name="saas-shared-accounts-configuration"></a>SaaS gedeelde configuratie van Accounts

Geschatte tijd om te voltooien: 30 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| De lijst met doeltoepassingen en de exacte aanmelden URL's tevoren. U kunt bijvoorbeeld Twitter. | [Twitter op Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Aanmelden voor Twitter](https://twitter.com/signup?lang=en) |
| Gedeelde referenties voor deze SaaS-toepassing. | [Delen van accounts met behulp van Azure AD](active-directory-sharing-accounts.md)<br/>[Azure AD geautomatiseerde wachtwoord roll gedurende Facebook, Twitter en LinkedIn nu in preview! -Enterprise Mobility and Security-Blog] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| De referenties voor ten minste twee teamleden die toegang hebben tot hetzelfde account zijn. Ze moet deel uitmaken van een beveiligingsgroep. | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Lokale beheerderstoegang tot een computer voor het implementeren van de uitbreiding van het Configuratiescherm toegang voor Internet Explorer, Chrome of Firefox | [Configuratiescherm-extensie voor IE toegang](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Uitbreiding van het Configuratiescherm toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Uitbreiding van het Configuratiescherm toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| De Browseruitbreiding voor de installeren | [Configuratiescherm-extensie voor IE toegang](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Uitbreiding van het Configuratiescherm toegang voor Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Uitbreiding van het Configuratiescherm toegang voor Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Toepassing vanuit galerie configureren | [Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: de nieuwe en verbeterde-toepassingsgalerie](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Wachtwoord eenmalige aanmelding configureren | [Het beheren van eenmalige aanmelding voor zakelijke apps in de nieuwe Azure portal: Meld u op wachtwoord gebaseerde](active-directory-enterprise-apps-manage-sso.md#password-based-sign-on) |
| Toewijzen van de app aan de groep die in de vereisten zijn geïdentificeerd tijdens het toewijzen van referenties | [Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md) |
| Meld u aan als verschillende gebruikers die app toegang als de **dezelfde gedeelde account.**  |  |
| U kunt eventueel de gebruiksrapporten van de toepassing controleren. Opmerking: Er is enige vertraging worden bijgewerkt, dus u wacht enige tijd moet en het verkeer in de rapporten. | [Aanmeldingsactiviteiten rapporten in de Azure Active Directory-portal: informatie over het gebruik van beheerde toepassingen](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Azure Active Directory report retention policies](active-directory-reporting-retention.md) (Retentiebeleid voor Azure Active Directory-rapporten) |


### <a name="considerations"></a>Overwegingen

Als de doeltoepassing niet aanwezig in de galerie is, kunt klikt u vervolgens u 'Bring your own app'. Meer informatie: [wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: het toevoegen van aangepaste toepassingen vanaf één locatie](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Houd rekening met de volgende vereisten:
   * Toepassing moet een bekende aanmeldings-URL
   * De aanmeldingspagina moet een HTML-formulier met één meer tekstvelden die de browser-extensies kunnen automatisch invullen bevatten. Minimaal de moet gebruikersnaam en het wachtwoord bevatten.

## <a name="app-proxy-configuration"></a>App-proxyconfiguratie

Geschatte tijd om te voltooien: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Een Microsoft Azure AD basic of premium-abonnement en een Azure AD-directory waarvoor u een globale beheerder bent | [Azure Active Directory-edities](active-directory-editions.md) |
| Een webtoepassing gehost on-premises die u wilt configureren voor externe toegang |  |
| Een server met Windows Server 2012 R2 of Windows 8.1 of hoger, waarop u de Connector voor toepassingsproxy kunt installeren | [Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md) |
| Als er een firewall in het pad, zorg ervoor dat deze geopend is zodat de Connector HTTPS (TCP) aanvragen naar de toepassingsproxy kunt maken | [Toepassingsproxy inschakelen in de Azure-portal: vereisten voor toepassingsproxy](active-directory-application-proxy-enable.md#application-proxy-prerequisites) |
| Als uw organisatie gebruikmaakt van proxy-servers verbinding maken met internet, nemen de blog kijken werken met bestaande lokale proxyservers voor meer informatie over het configureren ervan boeken | [Werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Een connector op de server installeren | [Toepassingsproxy inschakelen in de Azure-portal: Installeer en registreer de Connector](active-directory-application-proxy-enable.md#install-and-register-a-connector) |
| De on-premises toepassing publiceren in Azure AD als een toepassing toepassingsproxy | [Toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md) |
| Testgebruikers toewijzen | [Toepassingen publiceren met Azure AD-toepassingsproxy: een testgebruiker toevoegen](application-proxy-publish-azure-portal.md#add-a-test-user) |
| Configureer desgewenst een ervaring voor eenmalige aanmelding voor uw gebruikers | [Geef eenmalige aanmelding met Azure AD-toepassingsproxy](application-proxy-sso-azure-portal.md) |
| App testen door het aanmelden bij de portal MyApps als toegewezen gebruiker | https://myapps.Microsoft.com |

### <a name="considerations"></a>Overwegingen

1. Terwijl het is raadzaam om de connector in te stellen in uw bedrijfsnetwerk, zijn er gevallen wanneer ziet u betere prestaties plaatsen hiervan op in de cloud. Meer informatie: [aandachtspunten voor topologie netwerk bij gebruik van Azure Active Directory-toepassingsproxy](application-proxy-network-topology-considerations.md)
2. Zie voor meer informatie over de beveiliging en hoe dit biedt een bijzonder veilige externe toegang oplossing door alleen uitgaande verbindingen onderhouden: [beveiligingsoverwegingen voor toegang tot de apps op afstand met behulp van Azure AD-toepassingsproxy](application-proxy-security-considerations.md)

## <a name="generic-ldap-connector-configuration"></a>Algemene configuratie van de LDAP-Connector

Geschatte tijd om te voltooien: 60 minuten

> [!IMPORTANT]
> Dit is een geavanceerde configuratie vereisen enigszins bekend bent met FIM/MIM. Als het gebruikt in productie, wordt geadviseerd de vragen over deze configuratie gaat via [Premier Support](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Azure AD Connect geïnstalleerd en geconfigureerd | Bouwsteen: [adreslijstsynchronisatie - Wachtwoordhashsynchronisatie](#directory-synchronization--password-hash-sync-phs--new-installation) |
| ADLDS exemplaar vergadering vereisten | [Algemene LDAP-Connector, technische naslaginformatie: overzicht van de algemene LDAP-Connector](./connect/active-directory-aadconnectsync-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| Lijst van werkbelastingen die van gebruikers gebruikmaken en kenmerken die zijn gekoppeld aan deze werkbelastingen | [Azure AD Connect-synchronisatie: kenmerken gesynchroniseerd naar Azure Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Algemene LDAP-Connector toevoegen | [Algemene LDAP-Connector, technische naslaginformatie: een nieuwe Connector maken](./connect/active-directory-aadconnectsync-connector-genericldap.md#create-a-new-connector) |
| Uitvoeringsprofielen maken voor gemaakte connector (volledige import, delta-import, volledige synchronisatie, Deltasynchronisatie, exporteren) | [Maken van een beheerprofiel van het uitvoeren van Agent](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Met behulp van connectors met de Azure AD Connect Sync Service Manager](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Volledige import profiel uitvoeren en controleren, er zijn objecten in de connectorruimte | [Zoeken naar een Object van de ruimte Connector](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Met connectors met de Azure AD Connect Sync Service Manager: Connectorgebied zoeken](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Synchronisatieregels maakt, zodat objecten in de Metaverse over vereiste kenmerken voor werkbelastingen | [Azure AD Connect-synchronisatie: aanbevolen procedures voor het wijzigen van de standaardconfiguratie: wijzigingen aan de regels voor synchronisatie](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect-synchronisatie: inzicht declaratieve inrichting](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect-synchronisatie: inzicht declaratieve inrichting expressies](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Volledige synchronisatiecyclus starten | [Azure AD Connect-synchronisatie: Scheduler: Start de planner](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| In geval van problemen wilt oplossen | [Troubleshoot an object that is not synchronizing to Azure AD](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) (Problemen oplossen wanneer een object niet wordt gesynchroniseerd met Azure AD) |
| Controleer of, LDAP-gebruiker kunt aanmelden en toegang tot de toepassing | https://myapps.Microsoft.com |

### <a name="considerations"></a>Overwegingen

> [!IMPORTANT]
> Dit is een geavanceerde configuratie vereisen enigszins bekend bent met FIM/MIM. Als het gebruikt in productie, wordt geadviseerd de vragen over deze configuratie gaat via [Premier Support](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Groepen - overgedragen eigendom

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| SaaS-toepassing (federatieve SSO of wachtwoord SSO) is al geconfigureerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Cloud-groep die de toegang is toegewezen aan de toepassing in #1 wordt geïdentificeerd. | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Er zijn referenties voor de eigenaar van de groep beschikbaar | [Toegang tot resources beheren met Azure Active Directory-groepen](active-directory-manage-groups.md) |
| Referenties voor de Informatiemedewerker toegang tot de apps is geïdentificeerd | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Identificeren van de groep die toegang tot de toepassing heeft gekregen en configureer de eigenaar van de opgegeven groep| [De instellingen voor een groep in Azure Active Directory beheren](active-directory-groups-settings-azure-portal.md) |
| Meld u aan als de eigenaar van de groep, Zie het lidmaatschap van de groepen tabblad van het toegangsvenster | [Azure Active Directory-groepen Management-pagina](https://account.activedirectory.windowsazure.com/r/#/groups) |
| Toevoegen van de Informatiemedewerker die u wilt testen |  |
| Meld u aan als de Informatiemedewerker, Controleer dat de tegel beschikbaar is | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Overwegingen

Als de toepassing heeft de inrichting is ingeschakeld, moet u mogelijk Wacht een paar minuten voor het inrichten is voltooid voordat de toegang tot de toepassing als de Informatiemedewerker.

## <a name="saas-and-identity-lifecycle"></a>SaaS en Identity Lifecycle

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| SaaS-toepassing (federatieve SSO of wachtwoord SSO) is al geconfigureerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Cloud-groep die de toegang is toegewezen aan de toepassing in #1 wordt geïdentificeerd. | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) <br/>[Een groep maken en leden toevoegen in Azure Active Directory](active-directory-groups-create-azure-portal.md) |
| Referenties voor de Informatiemedewerker toegang tot de apps is geïdentificeerd | [Wat is het toegangsvenster?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Verwijder de gebruiker uit de groep die de app is toegewezen aan | [Groepslidmaatschap voor gebruikers in uw Azure Active Directory-tenant beheren](active-directory-groups-members-azure-portal.md) |
| Wacht enkele minuten duren voordat de inrichting ongedaan | [Geautomatiseerde Gebruikersinrichting voor SaaS-App in Azure AD: hoe werkt geautomatiseerde inrichting?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Meld u aan als de Informatiemedewerker naar de portal van mijn apps en Bevestig dat tegel ontbreekt op een afzonderlijke browsersessie | http://myapps.Microsoft.com |


### <a name="considerations"></a>Overwegingen

Het scenario implementatiemodel leavers en/of laat de eigenschap afwezig scenario's afleiden. Als de gebruiker wordt uitgeschakeld in on-premises AD dat of verwijderd, bestaat niet meer kunnen aanmelden bij de SaaS-toepassing.

## <a name="self-service-access-to-application-management"></a>Selfservice voor toegang tot Application Management

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeer Implementatiemodel gebruikers die toegang tot de toepassingen, worden aanvragen als onderdeel van de beveiligingsgroep | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |
| Doeltoepassing geïmplementeerd | Bouwsteen: [SaaS federatieve SSO-configuratie](#saas-federated-sso-configuration) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar de blade bedrijfstoepassingen in Azure AD-beheerportal | [Azure AD-beheerportal: Bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| De toepassing van de vereisten met selfservice configureren | [Wat is er nieuw in Enterprise Toepassingsbeheer in Azure Active Directory: toegang tot selfservice-toepassingen configureren](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Meld u aan als de Informatiemedewerker naar Mijn apps-portal | http://myapps.Microsoft.com |
| U ziet ' + app toevoegen ' knop in op de pagina. Toegang krijgen tot de app gebruiken |  |

### <a name="considerations"></a>Overwegingen

De gekozen toepassingen mogelijk inrichting vereisten, dus gaat onmiddellijk de app kan ertoe leiden dat een aantal fouten. Als de toepassing gekozen ondersteuning biedt voor inrichting met azure ad en deze is geconfigureerd, kunt u dit gebruiken als een goede kans om weer te geven van de hele stroom complete werken. Zie de bouwsteen voor [SaaS federatieve SSO configuratie](#saas-federated-sso-configuration) voor verdere aanbevelingen

## <a name="self-service-password-reset"></a>Selfservice voor wachtwoordherstel

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Schakel het beheer van de selfservice voor wachtwoordherstel in uw tenant. | [Azure Active Directory-wachtwoord opnieuw instellen voor IT-beheerders](active-directory-passwords-update-your-own-password.md) |
| Schakel wachtwoord terugschrijven van wachtwoorden van on-premises beheren. Opmerking hiervoor specifieke Azure AD Connect-versies | [Vereisten voor het terugschrijven van wachtwoorden](active-directory-passwords-writeback.md) |
| Identificeer de PoC-gebruikers die deze functionaliteit gebruiken en zorg ervoor dat ze lid zijn van een beveiligingsgroep. De gebruikers moeten niet-beheerders de mogelijkheid volledig presenteren | [Aanpassen: Azure AD-wachtwoordbeheer: toegang beperken tot wachtwoord opnieuw instellen](active-directory-passwords-writeback.md) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Navigeer naar Azure AD-beheerportal: wachtwoord opnieuw instellen | [Azure AD-beheerportal: Wachtwoord opnieuw instellen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Bepaal dat het beleid voor wachtwoordherstel. Voor de POC-doeleinden, kunt u telefonische oproep en Q & A. Het verdient aanbeveling om in te schakelen moet u aanmelden bij het toegangspaneel-registratie |  |
| Meld u af en meld u aan als een Informatiemedewerker |  |
| Geef de selfservice voor wachtwoordherstel gegevens zoals geconfigureerd per stap 2 | http://aka.MS/ssprsetup |
| Sluit de browser |  |
| De aanmelding beginnen als de Informatiemedewerker die u in stap 4 gebruikt |  |
| Het wachtwoord opnieuw instellen | [Uw eigen wachtwoord bijwerken: Mijn wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md) |
| Probeer aan te melden met uw nieuwe wachtwoord naar Azure AD ook garantie voor lokale bronnen |  |

### <a name="considerations"></a>Overwegingen

1. Als er een upgrade van de Azure AD Connect wrijving veroorzaken, overwegen tegen accounts in de cloud of een demo tegen een afzonderlijke omgeving maken
2. De beheerders hebben een ander beleid en het wachtwoord opnieuw instellen met behulp van het beheerdersaccount mogelijk taint de POC-fase en tot verwarring leiden. Zorg ervoor dat u een normaal gebruikersaccount gebruiken voor het testen van de bewerkingen opnieuw instellen


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Azure multi-factor Authentication met telefoongesprekken

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeer gebruikers Implementatiemodel met MFA  |  |
| Telefoon met goede ontvangst voor MFA uitdaging  | [Wat is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar 'Gebruikers en groepen' blade in Azure AD-beheerportal | [Azure AD-beheerportal: Gebruikers en groepen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| 'Alle gebruikers' blade kiezen |  |
| In de bovenste balk de knop 'Multi-Factor Authentication' kiezen | Directe URL voor Azure MFA-portal: https://aka.ms/mfaportal |
| In de instellingen voor 'Gebruiker' Selecteer de PoC-gebruikers en ze voor MFA inschakelen | [Statuswaarden voor gebruikers in Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) |
| Meld u aan als de gebruiker van de POC-fase en doorloop het proces bewijs-up  |  |

### <a name="considerations"></a>Overwegingen

1. De POC-fase stappen in deze bouwsteen expliciet MFA instellen voor een gebruiker op alle aanmeldingen. Er zijn andere hulpprogramma's zoals voorwaardelijke toegang en Identity Protection MFA op meer benaderen gerichte scenario's. Dit is iets rekening moet houden bij het verplaatsen van de POC-fase naar productie.
2. De PoC-stappen in deze bouwsteen telefoongesprekken expliciet als de MFA-methode voor expedience gebruikt. Als u van de POC-fase naar productie overstappen, wordt u aangeraden toepassingen, zoals de [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) als uw tweede factor indien mogelijk.
Meer informatie: [concept NIST Special Publication 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>MFA voorwaardelijke toegang voor SaaS-toepassingen

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeer gebruikers POC-fase voor het doel van het beleid. Deze gebruikers moeten zich in een beveiligingsgroep als bereik voor het beleid voor voorwaardelijke toegang | [Configuratie van de SaaS-federatieve SSO](#saas-federated-sso-configuration) |
| SaaS-toepassing is al geconfigureerd |  |
| Implementatiemodel gebruikers zijn al toegewezen aan de toepassing |  |
| Er zijn referenties voor de gebruiker Implementatiemodel beschikbaar |  |
| Implementatiemodel gebruiker is geregistreerd voor MFA. Via een telefoon met goede ontvangst | http://aka.MS/ssprsetup |
| Apparaat in het interne netwerk. IP-adres geconfigureerd in het interne-adresbereik | Uw IP-adres vinden: https://www.bing.com/search?q=what%27s+my+ip |
| Apparaat in het externe netwerk (een telefoon met behulp van het mobiele netwerk van de provider kan worden) |  |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Ga naar Azure AD-beheerportal: blade voorwaardelijke toegang | [Azure AD-beheerportal: Voorwaardelijke toegang](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Maak beleid voor voorwaardelijke toegang:<br/>-Gebruikers van de PoC target onder 'Gebruikers en groepen'<br/>-Implementatiemodel doeltoepassing onder 'Cloud-apps'<br/>-Gericht op alle locaties, behalve vertrouwde die onder 'Voorwaarden'-"Locaties" > **Opmerking:** goedgekeurde IP-adressen zijn geconfigureerd in [Portal MFA](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Multi-factor authentication-server onder 'Grant' vereisen | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: configuratiestappen beleid](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Toegang van een toepassing binnen bedrijfsnetwerk | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: het beleid testen](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Toegang tot de toepassing van openbaar netwerk | [Aan de slag met voorwaardelijke toegang in Azure Active Directory: het beleid testen](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Overwegingen

Als u federation gebruikt, kunt u de lokale id-Provider (IdP) gebruiken om te communiceren, de status binnen/buiten-bedrijfsnetwerk met claims. U kunt deze techniek gebruiken zonder het beheren van de lijst met IP-adressen die mogelijk complex om te beoordelen en beheren in grote organisaties. In dat de installatie, moet u rekening voor het scenario 'netwerk roaming' (een gebruiker de logboekregistratie van het interne netwerk en tijdens het aangemelde switches locaties bijvoorbeeld een restaurant) en zorg ervoor dat u de implicaties begrijpt. Meer informatie: [cloudresources beveiligen met Azure multi-factor Authentication en AD FS: goedgekeurde IP-adressen voor federatieve gebruikers](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Beschermde identiteitsbeheer (PIM)

Geschatte tijd om te voltooien: 15 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Identificeren van de globale beheerder die deel van de POC-fase voor PIM uitmaken | [Aan de slag met Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| De globale beheerder die de beveiligingsbeheerder fungeren zal identificeren | [Aan de slag met Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Andere beheerdersrollen in Azure Active Directory PIM](active-directory-privileged-identity-management-roles.md) |
| Optioneel: Als de globale beheerders toegang tot e-mail hebben te oefenen e-mailmeldingen in PIM bevestigen | [Wat is Azure AD Privileged Identity Management?: Configureer de instellingen van de activering](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Aanmelden bij https://portal.azure.com als een globale beheerder (GA) en de PIM-blade bootstrap. De globale beheerder die in deze stap uitvoert is als de beveiligingsbeheerder gemaakt.  Laten we deze acteur GA1 aanroepen | [Gebruik de beveiligingswizard in Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Identificeer de globale beheerder en verplaatsen van permanente naar in aanmerking komt. Dit moet een beheerder van het abonnement dat in stap 1 wordt gebruikt voor de duidelijkheid zijn afzonderlijke. Laten we deze acteur GA2 aanroepen | [Azure AD Privileged Identity Management: Het toevoegen of verwijderen van een gebruikersrol](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[Wat is Azure AD Privileged Identity Management?: Configureer de instellingen van de activering](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Nu aanmelden als GA2 bij https://portal.azure.com en wijzig 'Gebruikersinstellingen'. U ziet enkele opties zijn niet beschikbaar. | |
| In een nieuw tabblad in dezelfde sessie als stap 3, navigeer nu naar https://portal.azure.com en de PIM-blade toevoegen aan het dashboard. | [Activeren of deactiveren van rollen in Azure AD Privileged Identity Management: de Privileged Identity Management-toepassing toevoegen](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Activering van de aanvraag aan de rol globale beheerder | [Activeren of deactiveren van rollen in Azure AD Privileged Identity Management: een rol activeren](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Houd er rekening mee dat als GA2 nooit aangemeld voor MFA, registratie voor Azure MFA moet worden |  |
| Ga terug naar het oorspronkelijke tabblad in stap 3 en klik op de knop Vernieuwen in de browser. Houd er rekening mee dat u nu toegang hebt tot het wijzigen van 'Gebruikersinstellingen' | |
| Optioneel, als uw globale beheerders e-mailbericht is ingeschakeld, u kunt GA1 en GA2 van postvak in en de melding van de rol wordt geactiveerd |  |
| 8 controleren de controlegeschiedenis en bekijk het rapport om te bevestigen dat de uitbreiding van GA2 wordt weergegeven. | [Wat is Azure AD Privileged Identity Management?: rol controleactiviteit](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium-P2 en/of EMS E5

## <a name="discovering-risk-events"></a>Risico's detecteren

Geschatte tijd om te voltooien: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaten met Tor browser gedownload en geïnstalleerd | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Toegang tot de POC-fase gebruiker de aanmelding | [Playbook voor Azure Active Directory: Identity Protection](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Open tor-browser | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Aanmelden bij https://myapps.microsoft.com met het Implementatiemodel gebruikersaccount | [Azure Active Directory: Identity Protection playbook: Risicogebeurtenissen simuleren](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Wacht 5-7 minuten |  |
| Meld u aan als een globale beheerder zijn om https://portal.azure.com en open de blade Identity Protection | https://aka.MS/aadipgetstarted |
| Open de blade van de gebeurtenissen risico. Er is een item onder 'Aanmeldingen vanaf anonieme IP-adressen'  | [Azure Active Directory: Identity Protection playbook: Risicogebeurtenissen simuleren](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium-P2 en/of EMS E5

## <a name="deploying-sign-in-risk-policies"></a>Aanmelden risico beleid implementeren

Geschatte tijd om te voltooien: 10 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaten met Tor browser gedownload en geïnstalleerd | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Toegangsrechten als een gebruiker Implementatiemodel aan het logboek in testen |  |
| Implementatiemodel gebruiker is geregistreerd met MFA. Zorg ervoor dat u gebruikt een telefoon met goede ontvangst | Bouwsteen: [Azure multi-factor Authentication met telefoongesprekken](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| Meld u aan als een globale beheerder zijn om https://portal.azure.com de blade en open Identity Protection | https://aka.MS/aadipgetstarted |
| Een beleid voor aanmelden risico als volgt inschakelen:<br/>-Toegewezen aan: Implementatiemodel gebruiker<br/>-Voorwaarden: Aanmelden risico gemiddeld of hoger (aanmelden vanaf anonieme locatie wordt beschouwd als een gemiddeld risiconiveau)<br/>-Besturingselementen: MFA vereisen | [Azure Active Directory: Identity Protection playbook: aanmelden risico](active-directory-identityprotection-playbook.md#sign-in-risk) |
| Open tor-browser | [Tor Browser downloaden](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Aanmelden bij https://myapps.microsoft.com met het implementatiemodel gebruikersaccount |  |
| U ziet de MFA-controle | [Aanmelden-ervaringen met Azure AD Identity Protection: riskant aanmelden herstel](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Overwegingen

Deze mogelijkheid is onderdeel van Azure AD Premium-P2 en/of EMS E5. Voor meer informatie over de risico's gaat u naar: [risicogebeurtenissen Azure Active Directory](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Gebaseerde certificaatverificatie configureren

Geschatte tijd om te voltooien: 20 minuten

### <a name="pre-requisites"></a>Vereisten

| Vereiste | Resources |
| --- | --- |
| Apparaat met gebruikerscertificaat ingericht (Windows, iOS of Android) van Enterprise PKI | [Gebruikerscertificaten implementeren](https://msdn.microsoft.com/library/cc770857.aspx) |
| Azure AD-domein gefedereerd met AD FS | [Azure AD Connect en federatie](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Overzicht van Active Directory Certificate Services](https://technet.microsoft.com/library/hh831740.aspx)|
| Voor iOS-apparaten Microsoft Authenticator-app hebt geïnstalleerd | [Aan de slag met de Microsoft Authenticator-app](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Stappen

| Stap | Resources |
| --- | --- |
| "Verificatie met gebruikerscertificaat" op de AD FS inschakelen | [Verificatiebeleid configureren: Globaal primaire verificatie in Windows Server 2012 R2 configureren](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Optioneel: Authenticatie via certificaat inschakelen in Azure AD voor Exchange ActiveSync-clients | [Aan de slag met verificatie op basis van certificaten in Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Navigeer naar het Toegangspaneel en zich verifiëren met gebruikerscertificaat | https://myapps.Microsoft.com |

### <a name="considerations"></a>Overwegingen

Voor meer informatie over aanvullende opmerkingen van deze implementatie gaat u naar: [ADFS: verificatie met Azure AD & Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Bezit van gebruikerscertificaat moet worden bewaakt. Door op het beheren van apparaten of met PINCODE in geval van smartcards.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
