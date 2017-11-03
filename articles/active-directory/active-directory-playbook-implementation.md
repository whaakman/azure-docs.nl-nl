---
title: Implementatie van Azure Active Directory-implementatiemodel Playbook | Microsoft Docs
description: Verkennen en snel implementeren scenario's voor Identity and Access Management
services: active-directory
keywords: Azure active directory, playbook, Proof-of-Concept, implementatiemodel
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: dstefan
ms.openlocfilehash: 10877ee33ec04cf0d350417af59d598eab249aa0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Azure Active Directory-bewijs van Concept Playbook: uitvoering

## <a name="foundation---syncing-ad-to-azure-ad"></a>Foundation - AD naar Azure AD synchroniseren 

Een hybride identiteit vormt de basis voor het merendeel van de enterprise-klanten die al een on-premises adreslijst. Dit is opzettelijk besteden aan als minder tijd om weer te geven van de waarde van de werkelijke identiteit en toegang scenario's mogelijk. 

| Scenario | Bouwstenen| 
| --- | --- |  
| [Uitbreiden van uw on-premises identiteits naar de cloud](#extending-your-on-premises-identity-to-the-cloud) | [Synchronisatie van Directory - Wachtwoordhashsynchronisatie](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Opmerking**: als u al DirSync/ADSync of eerdere versies van Azure AD Connect hebt, deze stap is optioneel. Sommige scenario's in deze handleiding mogelijk nieuwere versie van Azure AD Connect.  <br/>[De huisstijl](active-directory-playbook-building-blocks.md#branding) | 
| [Groepen met Azure AD-licenties toewijzen](#assigning-azure-ad-licenses-using-groups) | [Groeperen op basis van de licentieverlening](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-to-the-cloud"></a>Uitbreiden van uw on-premises identiteits naar de cloud 

1. Bob is de Active Directory-beheerder van Contoso. Hij haalt de vereiste identiteit inschakelen als een service voor een groep gebruikers. Na de uitvoering van Azure AD Connect-wizard, de identiteit van de doelgebruikers die beschikbaar zijn in de cloud. 
2. Bob vraagt Susie, een van de doelgebruikers voor toegang tot het toegangsvenster Azure Active Directory en Bevestig dat ze kan worden geverifieerd. Susie ziet een huisstijl aanmeldingspagina en een leeg Toegangsvenster die gereed is voor het inschakelen van toegang voor toekomstige toepassingen.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Groepen met Azure AD-licenties toewijzen 

1. Bob is van de globale beheerder van Azure AD en wil Azure AD-licenties voor een specifieke set gebruikers toewijzen als onderdeel van de initiële implementatie van Azure AD.
2. Bob maakt een groep voor de gebruikers. 
3. Bob toegewezen licenties aan de groep
4. Susie, een van de IT-medewerkers wordt toegevoegd aan de beveiligingsgroep als onderdeel van haar taakfuncties
5. Na enige tijd heeft Susie toegang tot de Azure AD premium-licentie. Hiermee schakelt u meer van de POC-scenario's later op.

## <a name="theme---lots-of-apps-one-identity"></a>Thema - veel apps, één identiteit

| Scenario | Bouwstenen| 
| --- | --- |  
| [Het integreren van SaaS-toepassingen - federatieve SSO](#integrate-saas-applications---federated-sso) | [Configuratie van de SaaS-federatieve SSO](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Groepen - overgedragen eigendom](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [Wachtwoord eenmalige aanmelding voor SaaS-toepassingen - integreren](#integrate-saas-applications---password-sso) | [SaaS-wachtwoord SSO-configuratie](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [Eenmalige aanmelding en Identity Lifecycle Events](#sso-and-identity-lifecycle-events) | [SaaS en Identity Lifecycle](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Veilige toegang tot gedeelde Accounts](#secure-access-to-shared-accounts) | [SaaS gedeelde configuratie van Accounts](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Veilige externe toegang tot On-Premises toepassingen](#secure-remote-access-to-on-premises-applications) | [App-proxyconfiguratie](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [LDAP-identiteiten met Azure AD synchroniseren](#synchronize-ldap-identities-to-azure-ad) |  [Algemene configuratie van de LDAP-Connector](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>Het integreren van SaaS-toepassingen - federatieve SSO 

1. Bob is van de globale beheerder van Azure AD en ontvangt een aanvraag van de marketingafdeling voor toegang tot hun ServiceNow-exemplaar. Bob vindt stapsgewijze zelfstudie in Azure AD-documentatie en volgt en de toewijzing van gebruikers naar de app delegeert naar Kevin, het hoofd van Marketing-team. 
2. Kevin zich aanmeldt als eigenaar van rechten tussen ServiceNow en wijst Susie toe aan de app. Kevin ook meldingen van Susie profiel automatisch in ServiceNow is gemaakt
3. Susie is een Informatiemedewerker van de marketingafdeling. Ze zich aanmelden bij azure AD en vindt alle SaaS-toepassingen die ze aan is toegewezen in myapps-portal. Van daaruit ze naadloos toegang tot ServiceNow opgehaald.
4. De marketingafdeling wil controleren wie ServiceNow geraadpleegd. Bob downloadt van een activiteitenrapport en wordt gedeeld met Kevin via e-mail.  

### <a name="sso-and-identity-lifecycle-events"></a>Eenmalige aanmelding en Identity Lifecycle Events

1. Susie neemt een afwezig laat en door het bedrijfsbeleid de on-premises AD-account is tijdelijk uitgeschakeld. Susie nu niet kunt aanmelden bij Azure AD en daarom geen toegang tot ServiceNow. 
2. Susie maakt een laterale verplaatsing van Marketing te verkopen. Kevin verwijderd haar toegang uit ServiceNow. Susie wordt geregistreerd in de azure ad-myapps en ze niet langer de ServiceNow-tegel ziet. Na 10 minuten bevestigt Kevin dat Susie account is uitgeschakeld in ServiceNow-beheerconsole.

### <a name="integrate-saas-applications---password-sso"></a>Wachtwoord eenmalige aanmelding voor SaaS-toepassingen - integreren

1. Bob configureert toegang tot Atlassian HipChat. HipChat heeft wachtwoord SSO-integratie en toegang verlenen tot Susie
2. Susie zich aanmeldt bij de portal myapps en ziet een koppeling naar de extensie Azure AD-IE browser, die ze downloadt downloaden
3. Wanneer u op klikt, ze opgehaald haar HipChat gebruikersnaam en wachtwoord om referenties gevraagd. Dit is een eenmalige bewerking en na het voltooien van deze zij toegang heeft tot HipChat
4. Een paar dagen later Susie myapps portal en klikt op HipChat opnieuw. Dit keer ongeveer krijgt ze deze naadloos toegang
5. Kevin, de eigenaar van de app HipChat wil controleren wie toegang tot de toepassing. Bob een controlerapport downloadt en gedeeld met Kevin via e-mail. 

### <a name="secure-access-to-shared-accounts"></a>Veilige toegang tot gedeelde Accounts 

1. Bob is te beheren voor het beveiligen van de gedeelde Twitter-ingang voor leden van de verkoop-team. Hij voegt Twitter toe als een SSO-toepassing en toegewezen aan de beveiligingsgroep van de Sales-Team. Hij de referenties aan het gedeelde account is opgegeven en hij deze voorziet in het systeem. 
2. Referenties voor Twitter delen wordt niet meer vertrouwd door meerdere personen zijn geïnstalleerd. Bob kan automatische rollover van het wachtwoord Twitter.
3. Susie, lid zijn van het team verkoop, zich aanmelden bij de portal myapps en ziet een koppeling naar de Browseruitbreiding van de Azure AD-IE downloaden. Ze worden geïnstalleerd.
4. Wanneer u op klikt ze toegang krijgen rechtstreeks tot Twitter. Zij weet het wachtwoord niet.
5. Arnold maakt ook deel uit van het verkoopteam. Hij heeft dezelfde ervaring als Susie in stap 3 en 4
6. De afdeling verkoop wil controleren wie Twitter geraadpleegd. Bob downloadt van een activiteitenrapport en wordt gedeeld met Kevin via e-mail. 

### <a name="secure-remote-access-to-on-premises-applications"></a>Veilige externe toegang tot On-Premises toepassingen

1. Bob, de Azure AD-hoofdbeheerder, is verbeterd talrijke verzoeken om werknemers toegang tot verschillende nuttig lokale resources, zoals de toepassing kosten tijdens het op afstand werken. Hij volgt de [toepassingsproxy documentatie](active-directory-application-proxy-enable.md) voor het installeren van een connector en onkosten publiceren als een toepassingsproxy-toepassing. 
2. De URL van de externe uitgaven delen Bob met Susie, een van de werknemers die extern toegang nodig heeft. Ze toegang heeft tot de koppeling en na verificatie op basis van AAD, ze kan toegang tot de app uitgaven en doorgaan om productief te zijn terwijl deze extern is. 
3. Bob blijft vervolgens extra on-premises toepassingen hetzelfde proces gebruikt en die toegang geeft tot gebruikers naar behoefte publiceren. Hij voegt voorwaardelijke toegang en multi-factor authentication voor de gevoeliger toepassingen die hij publiceert, om ervoor te zorgen extra beveiliging.

### <a name="synchronize-ldap-identities-to-azure-ad"></a>LDAP-identiteiten met Azure AD synchroniseren

1. Berend van het bedrijf heeft complexe identiteitsinfrastructuur kunt koppelen. De meeste van de gebruikers worden beheerd in Windows Server Active Directory Domain Services (toevoegen). Sommige van hen worden beheerd door HR-systeem in Active Directory Lightweight Directory Services (ADLDS).
2. Bob opgedragen toegang tot SaaS-apps voor alle gebruikers (ook deze niet aanwezig in ADDS) inschakelen.
3. Bob configureert algemene LDAP-Connector naar pull-gegevens van ADLDS in Azure AD Connect.
4. Bob maakt synchronisatieregels zodat gebruikers LDAP worden ingevuld in de Metaverse en naar Azure AD
5. Susie wordt LDAP gebruiker toegang krijgt tot haar SaaS app met behulp gesynchroniseerd identiteit



> [!IMPORTANT] 
> Dit is een geavanceerde configuratie vereisen enigszins bekend bent met FIM/MIM. Als het gebruikt in productie, wordt geadviseerd de vragen over deze configuratie gaat via [Premier Support](https://support.microsoft.com/premier).



## <a name="theme---increase-your-security"></a>Thema - verhoging van de beveiliging van uw 

| Scenario | Bouwstenen| 
| --- | --- |  
| [Beveiligde administrator-accounttoegang](#secure-administrator-account-access) | [Azure MFA met telefoongesprekken](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Veilige toegang voor toepassingen](#secure-access-to-applications) | [Voorwaardelijke toegang voor SaaS-toepassingen](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Alleen In de Time-beheer inschakelen](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [Identiteiten op basis van de risico's beschermen](#protect-identities-based-on-risk) | [Risico's detecteren](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Aanmelden risico beleid implementeren](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [Verifiëren zonder verificatie op basis van wachtwoorden](#authenticate-without-passwords-using-certificate-based-authentication) | [Gebaseerde certificaatverificatie configureren](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Beveiligde administrator-accounttoegang

1. Bob is de globale beheerder van Azure AD. Hij geconstateerd Stuart als medebeheerder van de service. 
2. Berend configureert van Stuart account altijd om MFA voor het verbeteren van de beveiligingspostuur
3. Stuart meldt zich aan bij de Azure-portal en kennisgevingen die hij nodig heeft om door te gaan van de aanmelding zijn telefoonnummer op dat registreren
4. Toekomstige aanmeldingen van Stuart nu zijn beveiligd met meervoudige verificatie en wordt nu een telefonische oproep zijn identiteit verifiëren.

### <a name="secure-access-to-applications"></a>Beveiligde toegang tot toepassingen

1. Kevin is de eigenaar van het bedrijf van ServiceNow. Het bedrijf wil nu die gebruikers aanmelden met MFA bij het openen van buiten het bedrijfsnetwerk.
2. Berend onze globale beheerder Azure AD voegt een beleid voor voorwaardelijke toegang tot de ServiceNow-toepassing voor het inschakelen van MFA voor externe toegang
3. Susie, onze Informatiemedewerker wordt geregistreerd in de portal van mijn apps en klikt op de tegel ServiceNow. Ze is nu gecontroleerd met MFA.

### <a name="enable-just-in-time-jit-administration"></a>Just in time (Just in time)-beheer inschakelen

1. Bob en Stuart zijn globale beheerders van Azure AD. Ze willen voor JIT-toegang tot de management-rollen en bij te houden op het gebruik van bevoorrechte rollen.
2. Bob PIM kan in de Azure AD-tenant en wordt de beveiligingsbeheerder. Hij verandert zelf en lidmaatschap van de rol globale beheerder van Stuart van permanente in een in aanmerking komt.
3. Bob en Stuart is nu vereist voor het activeren van hun rol via de Azure portal voordat u wijzigingen naar Azure AD-configuratie uitvoert. 

### <a name="protect-identities-based-on-risk"></a>Identiteiten op basis van de risico's beschermen 

1. Susie, probeert een Informatiemedewerker logboekregistratie in een tor-browser. 
2. Bob controleert de Azure AD identity protection-dashboard en Susie van aanmelding vanuit een anoniem IP-adres krijgt. Het security team wil uitdaging dergelijke gebruikers toegang geeft tot met MFA
3. Bob kan Azure AD Identity Protection-beleid voor MFA-vraag voor gemiddeld of hoger risico 's
4. Tijd gaat door, en Susie in vanuit de browser Tor opnieuw. Deze tijd ziet ze de MFA-controle

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>Verifiëren zonder verificatie op basis van wachtwoorden

1. Bob is een globale beheerder voor financiële instelling, die gebruik van wachtwoorden als een factor authentication voor hun toepassingen verbiedt.
2. Bob kunt en dwingt certificaatverificatie op AD FS en Azure AD
3. Susie tijdens het openen van toepassing is gevraagd om te verifiëren met behulp van certificaat

## <a name="theme---scale-with-self-service"></a>Thema - schaal met selfservice

| Scenario | Bouwstenen| 
| --- | --- |  
| [Selfservice voor wachtwoordherstel](#self-service-password-reset) | [Selfservice voor wachtwoordherstel](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Selfservice voor toegang tot toepassingen](#self-service-access-to-applications) | [Selfservice voor toegang tot toepassingen](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Selfservice voor wachtwoordherstel 

1. Bob is van de globale beheerder van Azure AD en wachtwoordbeheer voor Self Service maakt op een subset van gebruikers, met inbegrip van Susie. 
2. Susie Logboeken in myapps portal en zien een bericht te registreren haar beveiligingsinformatie voor toekomstige wachtwoord opnieuw instellen voor gebeurtenissen.
3. Snel vooruit een paar dagen Susie haar wachtwoord vergeet, en stelt deze via Azure AD-portal

### <a name="self-service-access-to-applications"></a>Selfservice voor toegang tot toepassingen 

1. Kevin is de eigenaar van het bedrijf van ServiceNow. Hij wil dat gebruikers ' aanmelden ' voor het op aanvraag, in plaats van ze toe te voegen in één keer
2. Berend onze globale beheerder van Azure AD, wijzigt de ServiceNow-toepassing voor self-service aanvragen inschakelen
3. Susie, onze Informatiemedewerker wordt geregistreerd in de portal van mijn apps en klikt op de knop 'Meer toepassingen toevoegen' en ServiceNow zien als een van de aanbevolen toepassingen. Vervolgens zij gaat terug naar Mijn apps-portal en de toepassing ServiceNow zien.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]