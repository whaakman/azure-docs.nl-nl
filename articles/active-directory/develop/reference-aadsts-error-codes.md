---
title: Azure Active Directory-verificatie en autorisatie-foutcodes | Microsoft Docs
description: Meer informatie over de AADSTS foutcodes die zijn geretourneerd door de Azure AD security token service (STS).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin, justhu
ms.custom: aaddev
ms.openlocfilehash: 0643ae39d24f8ef0c1c1c18dcf73cf840c95e277
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956322"
---
# <a name="authentication-and-authorization-error-codes"></a>Foutcodes voor verificatie en autorisatie

Zoek naar informatie over de AADSTS foutcodes die zijn geretourneerd door de security token service (STS) van Azure Active Directory (Azure AD)? Lezen van dit document om te zoeken AADSTS aanbevolen foutbeschrijvingen, correcties en enkele oplossingen.

> [!NOTE]
> Deze informatie is voorlopig en kan worden gewijzigd. Een vraag hebt of niet kunt vinden wat u zoekt? Maken van een GitHub-probleem of Zie [opties voor ondersteuning en hulp voor ontwikkelaars](active-directory-develop-help-support.md) voor meer informatie over andere manieren waarop u hulp en ondersteuning.

## <a name="aadsts-error-codes"></a>AADSTS-foutcodes

| Fout | Beschrijving |
|---|---|
| AADSTS16000 | SelectUserAccount - dit is een interrupt gegenereerd door Azure AD, wat in een gebruikersinterface waarmee de gebruiker resulteert te kiezen uit meerdere geldige SSO-sessies. Deze fout is redelijk gebruikelijk en kunnen worden geretourneerd naar de toepassing als `prompt=none` is opgegeven. |
| AADSTS16001 | UserAccountSelectionInvalid - ziet u deze fout als de gebruiker klikt op een tegel met de optie logica van de sessie is afgewezen. Wanneer ze worden geactiveerd, kan deze fout de gebruiker om te herstellen door verzamelen vanuit een bijgewerkte lijst van tegels/sessies, of door het kiezen van een ander account. Deze fout kan optreden vanwege een code defect of race-voorwaarde. |
| AADSTS16002 | AppSessionSelectionInvalid - de vereiste app opgegeven SID is niet voldaan aan.  |
| AADSTS16003 | SsoUserAccountNotFoundInResourceTenant |
| AADSTS17003 | CredentialKeyProvisioningFailed |
| AADSTS20001 | WsFedSignInResponseError - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS20012 | WsFedMessageInvalid - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS20033 | FedMetadataInvalidTenantName - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS40008 | OAuth2IdPUnretryableServerError - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS40009 | OAuth2IdPRefreshTokenRedemptionUserError - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS40010 | OAuth2IdPRetryableServerError - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS40015 | OAuth2IdPAuthCodeRedemptionUserError - er is een probleem met uw federatieve id-Provider. Neem contact op met uw IDP om dit probleem op te lossen. |
| AADSTS50000 | TokenIssuanceError - er is een probleem met de service aanmelden. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) om dit probleem op te lossen. |
| AADSTS50001 | InvalidResource - de resource is uitgeschakeld of niet bestaat. Controleer de code van uw app om ervoor te zorgen dat u hebt opgegeven dat de exacte bron-URL voor de resource die u probeert te openen.  |
| AADSTS50002 | NotAllowedTenant - aanmelding is mislukt omdat de Proxytoegang beperkt voor de tenant. Als het tenantbeleid voor uw eigen, kunt u uw beperkte tenantinstellingen om dit probleem te verhelpen. |
| AADSTS50003 | MissingSigningKey - aanmelding is mislukt vanwege een ontbrekende ondertekeningssleutel of het certificaat. Dit kan zijn omdat er geen ondertekeningssleutel die is geconfigureerd in de app. Bekijk de oplossingen die worden beschreven in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Als u nog steeds problemen ziet, contact op met de eigenaar van de app of een app-beheerder. |
| AADSTS50005 | DevicePolicyError - gebruiker heeft geprobeerd zich aanmelden bij een apparaat van een platform dat wordt momenteel niet ondersteund via beleid voor voorwaardelijke toegang. |
| AADSTS50006 | InvalidSignature - controle van de handtekening is mislukt vanwege een ongeldige handtekening. |
| AADSTS50007 | PartnerEncryptionCertificateMissing - het partner-versleutelingscertificaat is niet gevonden voor deze app. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met Microsoft om dit vast. |
| AADSTS50008 | InvalidSamlToken - SAML-verklaring ontbreekt of is niet juist geconfigureerd in het token. Neem contact op met uw federatieprovider. |
| AADSTS50010 | AudienceUriValidationFailed - doelgroep URI validatie voor de app is mislukt omdat er geen tokendoelgroep zijn geconfigureerd. |
| AADSTS50011 | InvalidReplyTo - het antwoordadres ontbreekt, is onjuist geconfigureerd, of komt niet overeen met de antwoordadressen die zijn geconfigureerd voor de app. Probeer de oplossing die wordt vermeld in [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Als u nog steeds problemen ziet, contact op met de eigenaar van de app of de app-beheerder. |
| AADSTS50012 | AuthenticationFailed - verificatie is mislukt voor een van de volgende redenen:<ul><li>De onderwerpnaam van het handtekeningcertificaat is niet gemachtigd</li><li>Een overeenkomend beleid van de vertrouwde instantie is niet gevonden voor de geautoriseerde onderwerpnaam</li><li>De certificaatketen is niet geldig</li><li>Het handtekeningcertificaat is niet geldig</li><li>Beleid is niet geconfigureerd op de tenant</li><li>Vingerafdruk van het handtekeningcertificaat is niet gemachtigd</li><li>Client-assertie bevat een ongeldige handtekening</li></ul> |
| AADSTS50013 | InvalidAssertion - bevestiging is ongeldig vanwege verschillende redenen - uitgever van het token komt niet overeen met de api versie binnen het geldige bereik - verlopen - onjuiste - vernieuwingstoken in de verklaring is niet een primaire vernieuwingstoken. |
| AADSTS50014 | GuestUserInPendingState |
| AADSTS50015 | ViralUserLegalAgeConsentRequiredState |
| AADSTS50017 | CertificateValidationFailed - certificering validatie mislukt, redenen voor de volgende redenen:<ul><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Verwacht CrlSegment kan niet worden gevonden</li><li>Kan uitgiftecertificaat niet vinden in de lijst met vertrouwde certificaten</li><li>Delta CRL-distributiepunt is geconfigureerd zonder een bijbehorend CRL-distributiepunt</li><li>Kan geen geldige CRL-segmenten ophalen als gevolg van time-outprobleem</li><li>Kan CRL niet downloaden</li></ul>Neem contact op met de beheerder van de tenant. |
| AADSTS50020 | UserUnauthorized - gebruikers zijn niet geautoriseerd voor dit eindpunt aanroepen. |
| AADSTS50027 | InvalidJwtToken - ongeldige JWT-token door de volgende oorzaken:<ul><li>bevat geen nonceclaim, subclaim</li><li>onderwerp-id komt niet overeen</li><li>duplicaatclaim in idToken-claims</li><li>onverwachte certificaatverlener</li><li>onverwachte doelgroep</li><li>niet binnen geldig tijdsbereik </li><li>indeling van het token is niet juist</li><li>Handtekeningverificatie van extern ID-token van de verlener is mislukt.</li></ul> |
| AADSTS50029 | Ongeldige URI: domeinnaam bevat ongeldige tekens. Neem contact op met de beheerder van de tenant. |
| AADSTS50032 | WeakRsaKey - geeft aan dat de foutieve gebruiker probeert een zwakke RSA-sleutel te gebruiken. |
| AADSTS50033 | RetryableError - geeft aan dat een tijdelijke fout die niet gerelateerd aan de databasebewerkingen. |
| AADSTS50034 | UserAccountNotFound - zich aanmelden bij deze toepassing, het account moet worden toegevoegd aan de directory. |
| AADSTS50042 | UnableToGeneratePairwiseIdentifierWithMissingSalt - de salt vereist voor het genereren van een pairwise id ontbreekt in de principal. Neem contact op met de beheerder van de tenant. |
| AADSTS50043 | UnableToGeneratePairwiseIdentifierWithMultipleSalts |
| AADSTS50048 | SubjectMismatchesIssuer - onderwerp verschillen verlener claim in de client-assertie. Neem contact op met de beheerder van de tenant. |
| AADSTS50049 | NoSuchInstanceForDiscovery - onbekende of ongeldige exemplaar. |
| AADSTS50050 | MalformedDiscoveryRequest - aanvraag is ongeldig. |
| AADSTS50053 | IdsLocked - het account is vergrendeld omdat de gebruiker heeft geprobeerd aan te melden te vaak hebt aangemeld met een onjuiste gebruikersnaam of wachtwoord. |
| AADSTS50055 | InvalidPasswordExpiredPassword - het wachtwoord is verlopen. |
| AADSTS50056 | Ongeldig of null-wachtwoord-wachtwoord bestaat niet in het archief voor deze gebruiker. |
| AADSTS50057 | UserDisabled - account van de gebruiker is uitgeschakeld. Het account is uitgeschakeld door een beheerder. |
| AADSTS50058 | UserInformationNotProvided - dit betekent dat een gebruiker niet is aangemeld. Dit is een veelvoorkomende fout die wordt verwacht wanneer een gebruiker niet-geverifieerde is en nog niet is ondertekend.</br>Als deze fout wordt aangemoedigd in de context van een eenmalige aanmelding waarbij de gebruiker alleen eerder heeft aangemeld, betekent dit dat de sessie voor eenmalige aanmelding is niet gevonden of is ongeldig.</br>Deze fout kan worden geretourneerd naar de toepassing als vragen = niets is opgegeven. |
| AADSTS50059 | MissingTenantRealmAndNoUserInformationProvided - Tenant-identificatiegegevens is niet gevonden in ofwel de aanvraag of impliciet door elk opgegeven referenties. De gebruiker, kunt contact opnemen met de tenantbeheerder om u te helpen bij het oplossen van het probleem. |
| AADSTS50061 | SignoutInvalidRequest - aanvragen voor afmelden is ongeldig. |
| AADSTS50064 | CredentialAuthenticationError |
| AADSTS50068 | SignoutInitiatorNotParticipant |
| AADSTS50070 | SignoutUnknownSessionIdentifier |
| AADSTS50071 | SignoutMessageExpired |
| AADSTS50072 | UserStrongAuthEnrollmentRequiredInterrupt - gebruiker moet zich registreren voor verificatie met tweede factor (interactief). |
| AADSTS50074 | UserStrongAuthClientAuthNRequiredInterrupt - sterke verificatie is vereist en de gebruiker heeft de MFA-controle niet doorstaan. |
| AADSTS50076 | UserStrongAuthClientAuthNRequired - de gebruiker moet meervoudige verificatie gebruiken voor toegang tot deze resource. Probeer het opnieuw met een nieuwe aanvraag machtigen voor de resource. |
| AADSTS50079 | UserStrongAuthEnrollmentRequired - vanwege een wijziging aangebracht door de beheerder of omdat de gebruiker naar een nieuwe locatie verplaatst, de gebruiker is vereist voor multi-factor authentication gebruiken. |
| AADSTS50085 | Vernieuwingstoken vereist een sociale IDP-aanmelding. Laat gebruikers proberen zich opnieuw aan te melden met gebruikersnaam en wachtwoord |
| AADSTS50086 | SasNonRetryableError |
| AADSTS50087 | SasRetryableError |
| AADSTS50089 | Stroomtoken verlopen, verificatie is mislukt. Laat gebruikers proberen zich opnieuw aan te melden met gebruikersnaam en wachtwoord |
| AADSTS50097 | DeviceAuthenticationRequired - apparaatverificatie is vereist. |
| AADSTS50099 | PKeyAuthInvalidJwtUnauthorized - de JWT-handtekening is ongeldig. |
| AADSTS50105 | EntitlementGrantsNotFound - de aangemelde gebruiker is niet toegewezen aan een rol voor de aangemelde app. De gebruiker toewijzen aan de app. Voor meer informatie:[https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role). |
| AADSTS50107 | InvalidRealmUri - de aangevraagde federation realm object bestaat niet. Neem contact op met de beheerder van de tenant. |
| AADSTS50120 | ThresholdJwtInvalidJwtFormat - probleem met JWT-header. Neem contact op met de beheerder van de tenant. |
| AADSTS50124 | ClaimsTransformationInvalidInputParameter - claimtransformatie bevat ongeldige invoerparameter. Neem contact op met de tenantbeheerder om het beleid bij te werken. |
| AADSTS50125 | PasswordResetRegistrationRequiredInterrupt - aanmelding is onderbroken vanwege een wachtwoord opnieuw instellen of de registratie wachtwoordinvoer. |
| AADSTS50126 | InvalidUserNameOrPassword - fout bij het valideren van referenties vanwege een ongeldige gebruikersnaam of wachtwoord. |
| AADSTS50127 | BrokerAppNotInstalled - gebruiker moet een broker-app voor toegang tot deze inhoud te installeren. |
| AADSTS50128 | De domeinnaam is ongeldig: geen tenant-identificatiegegevens gevonden in de aanvraag of impliciet door opgegeven referenties|
| AADSTS50129 | DeviceIsNotWorkplaceJoined - Workplace join is vereist om het apparaat te registreren. |
| AADSTS50131 | ConditionalAccessFailed - geeft aan verschillende fouten voor voorwaardelijke toegang, zoals slechte status van het apparaat voor Windows, aanvraag geblokkeerd vanwege verdachte activiteiten, toegangsbeleid of beveiliging voor beleidsbeslissingen. |
| AADSTS50132 | SsoArtifactInvalidOrExpired - de sessie is niet geldig vanwege het verlopen van wachtwoorden of recente wachtwoord wijzigen. |
| AADSTS50133 | SsoArtifactRevoked - de sessie is niet geldig vanwege het verlopen van wachtwoorden of recente wachtwoord wijzigen. |
| AADSTS50134 | DeviceFlowAuthorizeWrongDatacenter |
| AADSTS50135 | PasswordChangeCompromisedPassword - wachtwoordwijziging is vereist om account gevaar. |
| AADSTS50136 | RedirectMsaSessionToApp - één MSA-sessie gedetecteerd. |
| AADSTS50139 | SessionMissingMsaOAuth2RefreshToken |
| AADSTS50140 | KmsiInterrupt - deze fout is opgetreden vanwege interrupt 'Aangemeld blijven' wanneer de gebruiker aanmelden is. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-id, aanvraag-id en een foutcode voor meer informatie. |
| AADSTS50143 | Sessie komt niet overeen: sessie is ongeldig omdat de gebruikertenant niet overeenkomt met de domeinhint als gevolg van andere resource. [Open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) met correlatie-id, aanvraag-id en een foutcode voor meer informatie. |
| AADSTS50144 | InvalidPasswordExpiredOnPremPassword - Active Directory-wachtwoord van de gebruiker is verlopen. Genereer een nieuw wachtwoord voor de gebruiker of laat de gebruiker het opnieuw instellen van selfservice-hulpprogramma gebruiken om hun wachtwoord opnieuw instellen. |
| AADSTS50146 | MissingCustomSigningKey - deze app is vereist om te worden geconfigureerd met de ondertekeningssleutel van een app-specifiek. De toepassing is niet met een dergelijke sleutel geconfigureerd, of de sleutel is verlopen of nog niet geldig. |
| AADSTS50147 | MissingCodeChallenge |
| AADSTS50155 | DeviceAuthenticationFailed - apparaatverificatie is mislukt voor deze gebruiker. |
| AADSTS50158 | ExternalSecurityChallenge - externe beveiligingsvraag is niet voldaan aan. |
| AADSTS50161 | InvalidExternalSecurityChallengeConfiguration - Claims verzonden door de externe provider is niet voldoende of ontbrekende claim aangevraagd naar externe provider. |
| AADSTS50166 | ExternalClaimsProviderThrottled - aanvraag verzenden naar claimprovider is mislukt. |
| AADSTS50168 | ChromeBrowserSsoInterruptRequired - de client is geschikt voor het verkrijgen van een token voor eenmalige aanmelding via de uitbreiding voor Windows 10-Accounts, maar het token is niet gevonden in de aanvraag of het opgegeven token is verlopen. |
| AADSTS50169 | InvalidRequestBadRealm - de realm is niet een realm geconfigureerde van de huidige Servicenaamruimte. |
| AADSTS50170 | MissingExternalClaimsProviderMapping |
| AADSTS50177 | ExternalChallengeNotSupportedForPassthroughUsers - externe controle wordt niet ondersteund voor passthrough-gebruikers. |
| AADSTS50178 | SessionControlNotSupportedForPassthroughUsers - Sessiebeheer wordt niet ondersteund voor passthrough-gebruikers. |
| AADSTS50180 | WindowsIntegratedAuthMissing - geïntegreerde Windows-verificatie is vereist. Schakel de tenant in voor een naadloze eenmalige aanmelding (SSO). |
| AADSTS50187 | DeviceInformationNotProvided |
| AADSTS51000 | RequiredFeatureNotEnabled |
| AADSTS51001 | DomainHintMustbePresent - domeinhint moet aanwezig zijn met de on-premises beveiligings-id of UPN in on-premises zijn. |
| AADSTS51004 | UserAccountNotInDirectory - account van de gebruiker bestaat niet in de map. |
| AADSTS51005 | TemporaryRedirect - komt overeen met het HTTP-status 307, waarmee wordt aangegeven dat de aangevraagde informatie zich in de URI die is opgegeven in de location-header bevindt. Als u deze status krijgt, volgt u de locatieheader die is gekoppeld aan het antwoord. Wanneer de oorspronkelijke aanvraagmethode POST is, wordt de POST-methode ook gebruiken in de omgeleide-aanvraag. |
| AADSTS51006 | ForceReauthDueToInsufficientAuth - geïntegreerde Windows-verificatie is vereist. De gebruiker is aangemeld met een sessietoken dat de claim van geïntegreerde Windows-verificatie ontbreekt. Vraag de gebruiker zich opnieuw aanmeldt. |
| AADSTS52004 | DelegationDoesNotExistForLinkedIn - de gebruiker is niet opgegeven toestemming voor toegang tot bronnen van LinkedIn. |
| AADSTS53000 | DeviceNotCompliant - beleid voor voorwaardelijke toegang vereist een compatibel apparaat en het apparaat is niet compatibel. De gebruiker moet hun apparaat inschrijven met een goedgekeurde MDM-provider, zoals Intune. |
| AADSTS53001 | DeviceNotDomainJoined - beleid voor voorwaardelijke toegang vereist een domein gekoppelde apparaat en het apparaat is niet toegevoegd aan een domein. Apparaat lid zijn van de gebruiker gebruikt een domein. |
| AADSTS53002 | ApplicationUsedIsNotAnApprovedApp - de app die wordt gebruikt is niet een goedgekeurde app voor voorwaardelijke toegang. Gebruiker moet een van de apps uit de lijst met goedgekeurde apps gebruiken om te gebruiken om toegang te krijgen. |
| AADSTS53003 | BlockedByConditionalAccess - toegang is geblokkeerd door beleid voor voorwaardelijke toegang. Het toegangsbeleid is niet toegestaan voor token-uitgifte. |
| AADSTS53004 | ProofUpBlockedDueToRisk - gebruiker moet de multi-factor authentication-registratieproces voltooien voordat u toegang tot deze inhoud. Gebruiker moet zich registreren voor multi-factor authentication. |
| AADSTS54000 | MinorUserBlockedLegalAgeGroupRule |
| AADSTS65001 | DelegationDoesNotExist - de gebruiker of beheerder heeft niet toegestaan voor het gebruik van de toepassing met ID X. Send een interactieve autorisatieaanvraag voor deze gebruiker en de resource. |
| AADSTS65004 | UserDeclinedConsent - gebruiker geen toestemming voor toegang tot de app. Laat de gebruiker zich opnieuw aanmelden en toestemming geven voor de app|
| AADSTS65005 | MisconfiguredApplication - de app vereiste resourcetoegangslijst bevat geen apps kunnen worden gedetecteerd door de resource of de client-app heeft toegang tot een resource, die niet is opgegeven in de vereiste resourcetoegangslijst of -grafiek service geretourneerd ongeldig aangevraagd aanvraag of resource is niet gevonden. Als de app SAML ondersteunt, hebt u mogelijk de app geconfigureerd met de juiste id (entiteit). Probeer de oplossing voor SAML via de koppeling hieronder: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list) |
| AADSTS67003 | ActorNotValidServiceIdentity |
| AADSTS70000 | InvalidGrant - verificatie is mislukt. Het vernieuwingstoken dat is niet geldig. Fout kan worden veroorzaakt door de volgende redenen:<ul><li>Token binding-header is leeg</li><li>Token binding hash komt niet overeen</li></ul> |
| AADSTS70001 | UnauthorizedClient - de toepassing is uitgeschakeld. |
| AADSTS70002 | InvalidClient - fout bij het valideren van de referenties. De opgegeven waarde voor client_secret komt niet overeen met de verwachte waarde voor deze client. Corrigeer de waarde voor client_secret en probeer het opnieuw. Zie voor meer informatie, [de autorisatiecode gebruiken om aan te vragen van een toegangstoken](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token). |
| AADSTS70003 | UnsupportedGrantType - voor de app heeft een niet-ondersteunde toekenningstype geretourneerd. |
| AADSTS70004 | InvalidRedirectUri - voor de app heeft een ongeldige omleidings-URI geretourneerd. Het omleidingsadres dat is gespecificeerd door de client komt niet overeen met een geconfigureerd adres of een adres op de OIDC-goedkeuringslijst. |
| AADSTS70005 | UnsupportedResponseType - de app geretourneerd een niet-ondersteunde type vanwege de volgende redenen:<ul><li>reactietype 'token' is niet ingeschakeld voor de app.</li><li>reactietype 'id_token' vereist 'OpenID'-bereik - bevat een niet-ondersteunde OAuth-parameterwaarde in de gecodeerde wctx</li></ul> |
| AADSTS70007 | UnsupportedResponseMode - de app heeft een niet-ondersteunde waarde van geretourneerd `response_mode` bij het aanvragen van een token.  |
| AADSTS70008 | ExpiredOrRevokedGrant - het vernieuwingstoken dat is vanwege inactiviteit verlopen. Het token is uitgegeven voor XXX en voor een bepaalde hoeveelheid tijd inactief is. |
| AADSTS70011 | InvalidScope - het bereik dat wordt aangevraagd door de app is ongeldig. |
| AADSTS70012 | MsaServerError - een server-fout is opgetreden tijdens het verifiëren van een gebruiker beheerde Serviceaccounts (consument). Probeer het opnieuw. Als dit probleem blijft bestaan, [opent u een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS70016 | AuthorizationPending - stroom voor OAuth 2.0 apparaatfout. Autorisatie is in behandeling. Het apparaat wordt opnieuw geprobeerd de aanvraag polling. |
| AADSTS70018 | BadVerificationCode - ongeldige verificatiecode vanwege een gebruiker te typen in de verkeerde gebruikerscode waarmee de stroom van apparaat. Autorisatie is niet goedgekeurd. |
| AADSTS70019 | CodeExpired - verificatiecode is verlopen. Laat de gebruiker de aanmelding opnieuw. |
| AADSTS75001 | BindingSerializationError - er is een fout opgetreden tijdens de binding van SAML-bericht. |
| AADSTS75003 | UnsupportedBindingError - de app heeft een fout geretourneerd met betrekking tot niet-ondersteunde binding (SAML-protocol-antwoord kan niet worden verzonden via bindingen dan HTTP POST). |
| AADSTS75005 | Saml2MessageInvalid - Azure AD biedt geen ondersteuning voor de SAML-aanvraag verzonden door de app voor eenmalige aanmelding. |
| AADSTS75008 | RequestDeniedError - de aanvraag van de app is geweigerd omdat de SAML-aanvraag een onverwachte bestemming heeft. |
| AADSTS75011 | NoMatchedAuthnContextInOutputClaims - de verificatiemethode waarmee de gebruiker geverifieerd met de service komt niet overeen met aangevraagde methode voor verificatie. |
| AADSTS75016 | Saml2AuthenticationRequestInvalidNameIDPolicy - SAML2 verificatieverzoek heeft ongeldige NameIdPolicy. |
| AADSTS80001 | OnPremiseStoreIsNotAvailable - de verificatie-Agent kan geen verbinding maken met Active Directory. Zorg ervoor dat agentservers deel uitmaken van hetzelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd en ze zijn geen verbinding maken met Active Directory. |
| AADSTS80002 | OnPremisePasswordValidatorRequestTimedout - wachtwoordvalidatie voor een time-out. Zorg ervoor dat Active Directory beschikbaar en reageert op aanvragen van de agents is. |
| AADSTS80005 | OnPremisePasswordValidatorUnpredictableWebException - er is een onbekende fout opgetreden tijdens het verwerken van het antwoord van de verificatie-Agent. De aanvraag opnieuw. Als de pogingen mislukken blijven, [open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) voor meer informatie over de fout. |
| AADSTS80007 | OnPremisePasswordValidatorErrorOccurredOnPrem - de verificatie-Agent is niet het wachtwoord van gebruiker te valideren. Controleer de agent-logboeken voor meer informatie en dat Active Directory wordt uitgevoerd zoals verwacht. |
| AADSTS80010 | Er is OnPremisePasswordValidationEncryptionException - de Verificatieagent kan wachtwoord niet ontsleutelen. |
| AADSTS80012 | OnPremisePasswordValidationAccountLogonInvalidHours - de gebruikers hebben geprobeerd om aan te melden buiten het toegestane aantal uren (dit is opgegeven in AD). |
| AADSTS80013 | OnPremisePasswordValidationTimeSkew - de poging tot verificatie kan niet worden voltooid vanwege op tijd scheeftrekken tussen de computer met de verificatie-agent en AD. Tijd los problemen met synchronisatie. |
| AADSTS81004 | DesktopSsoIdentityInTicketIsNotAuthenticated - Kerberos-verificatiepoging is mislukt. |
| AADSTS81005 | DesktopSsoAuthenticationPackageNotSupported - pakket voor de verificatie wordt niet ondersteund. |
| AADSTS81006 | DesktopSsoNoAuthorizationHeader - geen autorisatie-header is gevonden. |
| AADSTS81007 | DesktopSsoTenantIsNotOptIn - de tenant is niet ingeschakeld voor naadloze eenmalige aanmelding. |
| AADSTS81009 | DesktopSsoAuthorizationHeaderValueWithBadFormat - kan niet worden gevalideerd Kerberos-ticket van de gebruiker. |
| AADSTS81010 | DesktopSsoAuthTokenInvalid - naadloze eenmalige aanmelding is mislukt omdat het Kerberos-ticket van de gebruiker is verlopen of ongeldig is. |
| AADSTS81011 | DesktopSsoLookupUserBySidFailed - kan geen om gebruikersobject op basis van gegevens in de Kerberos-ticket van de gebruiker te vinden. |
| AADSTS81012 | DesktopSsoMismatchBetweenTokenUpnAndChosenUpn - de gebruiker zich aanmelden bij Azure AD verschilt van de gebruiker is aangemeld bij het apparaat. |
| AADSTS90002 | InvalidTenantName - naam van de tenant is niet gevonden in het gegevensarchief. Controleer of dat u hebt de juiste tenant-ID. |
| AADSTS90004 | InvalidRequestFormat |
| AADSTS90005 | InvalidRequestWithMultipleRequirements |
| AADSTS90006 | ExternalServerRetryableError |
| AADSTS90007 | InvalidSessionId |
| AADSTS90008 | TokenForItselfRequiresGraphPermission |
| AADSTS90009 | TokenForItselfMissingIdenticalAppIdentifier |
| AADSTS90010 | NotSupported |
| AADSTS90012 | RequestTimeout |
| AADSTS90013 | InvalidUserInput |
| AADSTS90014 | MissingRequiredField - deze foutcode kan worden weergegeven in verschillende gevallen wanneer een verwacht veld niet aanwezig in de referentie is. |
| AADSTS90015 | QueryStringTooLong |
| AADSTS90016 | MissingRequiredClaim |
| AADSTS90019 | MissingTenantRealm - Azure AD is niet mogelijk te bepalen van de tenant-id van de aanvraag. |
| AADSTS90022 | AuthenticatedInvalidPrincipalNameFormat - indeling van de principal-naam is niet geldig of voldoet niet aan de verwachte `name[/host][@realm]` indeling. De principal-naam is vereist, host en de realm zijn optioneel en kunnen worden ingesteld op null. |
| AADSTS90023 | InvalidRequest |
| AADSTS90024 | RequestBudgetExceededError |
| AADSTS90033 | MsodsServiceUnavailable |
| AADSTS90036 | MsodsServiceUnretryableFailure |
| AADSTS90038 | NationalCloudTenantRedirection - de opgegeven tenant 'Y' behoort tot de nationale Cloud 'X'. Huidige instantie 'Z' komt niet federeren met X in de cloud. Een omleidings-fout is geretourneerd. |
| AADSTS90043 | NationalCloudAuthCodeRedirection |
| AADSTS90051 | InvalidNationalCloudId |
| AADSTS90055 | TenantThrottlingError - er zijn te veel binnenkomende aanvragen. Deze uitzondering wordt gegenereerd voor geblokkeerde tenants. |
| AADSTS90056 | BadResourceRequest - om in te wisselen van de code voor een toegangstoken, de app moet een POST-aanvraag te verzenden de `/token` eindpunt. Ook daarvóór moet u een autorisatiecode opgeeft en deze in de POST-aanvraag te verzenden de `/token` eindpunt. Raadpleeg dit artikel voor een overzicht van OAuth 2.0-autorisatiecodestroom: [ https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code ](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code). U moet eerst de gebruiker directe de `/authorize` -eindpunt, wat in een authorization_code resulteert. Wanneer een aanvraag naar de `/token` eindpunt, de gebruiker beschikt over het toegangstoken. Meld u in Azure portal en Controleer **App-registraties > eindpunten** om te bevestigen dat de twee eindpunten correct zijn geconfigureerd. |
| AADSTS90072 | PassThroughUserMfaError |
| AADSTS90081 | OrgIdWsFederationMessageInvalid |
| AADSTS90082 | OrgIdWsFederationNotSupported |
| AADSTS90084 | OrgIdWsFederationGuestNotAllowed |
| AADSTS90085 | OrgIdWsFederationSltRedemptionFailed |
| AADSTS90086 | OrgIdWsTrustDaTokenExpired |
| AADSTS90087 | OrgIdWsFederationMessageCreationFromUriFailed |
| AADSTS90090 | GraphRetryableError |
| AADSTS90091 | GraphServiceUnreachable |
| AADSTS90092 | GraphNonRetryableError |
| AADSTS90093 | GraphUserUnauthorized - grafiek geretourneerd met een niet-toegestane foutcode voor de aanvraag. |
| AADSTS90094 | AdminConsentRequired |
| AADSTS90100 | InvalidRequestParameter |
| AADSTS90101 | InvalidEmailAddress |
| AADSTS90102 | InvalidUriParameter |
| AADSTS90107 | InvalidXml |
| AADSTS90114 | InvalidExpiryDate |
| AADSTS90117 | InvalidRequestInput |
| AADSTS90119 | InvalidUserCode |
| AADSTS90120 | InvalidDeviceFlowRequest |
| AADSTS90121 | InvalidEmptyRequest |
| AADSTS90123 | IdentityProviderAccessDenied |
| AADSTS90124 | V1ResourceV2GlobalEndpointNotSupported |
| AADSTS90125 | DebugModeEnrollTenantNotFound |
| AADSTS90126 | DebugModeEnrollTenantNotInferred |
| AADSTS90130 | NonConvergedAppV2GlobalEndpointNotSupported |
| AADSTS120000 | PasswordChangeIncorrectCurrentPassword |
| AADSTS120002 | PasswordChangeInvalidNewPasswordWeak |
| AADSTS120003 | PasswordChangeInvalidNewPasswordContainsMemberName |
| AADSTS120004 | PasswordChangeOnPremComplexity |
| AADSTS120005 | PasswordChangeOnPremSuccessCloudFail |
| AADSTS120008 | PasswordChangeAsyncJobStateTerminated |
| AADSTS120011 | PasswordChangeAsyncUpnInferenceFailed |
| AADSTS120012 | PasswordChangeNeedsToHappenOnPrem |
| AADSTS120013 | PasswordChangeOnPremisesConnectivityFailure |
| AADSTS120014 | PasswordChangeOnPremUserAccountLockedOutOrDisabled |
| AADSTS120015 | PasswordChangeADAdminActionRequired |
| AADSTS120016 | PasswordChangeUserNotFoundBySspr |
| AADSTS120018 | PasswordChangePasswordDoesnotComplyFuzzyPolicy |
| AADSTS120020 | PasswordChangeFailure |
| AADSTS120021 | PartnerServiceSsprInternalServiceError |
| AADSTS130004 | NgcKeyNotFound |
| AADSTS130005 | NgcInvalidSignature |
| AADSTS130006 | NgcTransportKeyNotFound |
| AADSTS130007 | NgcDeviceIsDisabled |
| AADSTS130008 | NgcDeviceIsNotFound |
| AADSTS135010 | KeyNotFound |
| AADSTS140000 | InvalidRequestNonce |
| AADSTS140001 | InvalidSessionKey |
| AADSTS165000 | InvalidRequestCanary |
| AADSTS165004 | InvalidApiCanary |
| AADSTS165900 | InvalidApiRequest |
| AADSTS220450 | UnsupportedAndroidWebViewVersion |
| AADSTS220501 | InvalidCrlDownload |
| AADSTS221000 | DeviceOnlyTokensNotSupportedByResource - de resource is niet geconfigureerd voor het apparaat alleen-lezen-tokens accepteren. |
| AADSTS240001 | BulkAADJTokenUnauthorized |
| AADSTS240002 | RequiredClaimIsMissing |
| AADSTS700020 | InteractionRequired |
| AADSTS700022 | InvalidMultipleResroucesScope |
| AADSTS700023 | InvalidResourcelessScope |
| AADSTS900000 | WebWatsonEnvironmentError |
| AADSTS1000000 | UserNotBoundError |
| AADSTS1000002 | BindCompleteInterruptError |

## <a name="next-steps"></a>Volgende stappen

* Een vraag hebt of niet kunt vinden wat u zoekt? Maken van een GitHub-probleem of Zie [opties voor ondersteuning en hulp voor ontwikkelaars](active-directory-develop-help-support.md) voor meer informatie over andere manieren waarop u hulp en ondersteuning.