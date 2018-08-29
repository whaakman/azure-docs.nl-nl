# [Documentatie voor Azure Active Directory](index.md)

# Overzicht
## [Wat is Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Over Azure-identiteitsbeheer](fundamentals/identity-fundamentals.md)
## [Inzicht krijgen in Azure-identiteitsoplossingen](fundamentals/understand-azure-identity-solutions.md)
## [Een hybride identiteitsoplossing kiezen](choose-hybrid-identity-solution.md)
## [Azure-abonnementen koppelen](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Overwegingen met betrekking tot vestigingsplaats en gegevens](fundamentals/active-directory-data-storage-eu.md)
## [Veelgestelde vragen](fundamentals/active-directory-faq.md)
## [Nieuwe functies](fundamentals/whats-new.md)


# Aan de slag
## [Aan de slag met Azure AD](fundamentals/get-started-azure-ad.md)
## [Meld u aan voor Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Een aangepaste domeinnaam toevoegen](fundamentals/add-custom-domain.md)
## [Een bedrijfshuisstijl toevoegen](fundamentals/customize-branding.md)
## [Gebruikers toevoegen aan Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Licenties toewijzen aan een gebruikers](fundamentals/license-users-groups.md)
## [Self-service voor wachtwoord opnieuw instellen configureren](authentication/quickstart-sspr.md)
## [Privacy-informatie van uw organisatie toevoegen in Azure AD](active-directory-properties-area.md)


# Procedures
## Plannen en ontwerpen
### [Inzicht in de Azure AD-architectuur](fundamentals/active-directory-architecture.md)
### [Claimtoewijzing in Azure Active Directory](active-directory-claims-mapping.md)
### [Een hybride identiteitsoplossing implementeren](active-directory-hybrid-identity-design-considerations-overview.md)
#### Vereisten bepalen
##### [Identity](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Directory-synchronisatie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-Factor Authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Levenscyclus van identiteit plannen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Plan voor gegevensbeveiliging](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Gegevensbeveiliging](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Inhoudsbeheer](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Toegangsbeheer](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Reageren op incidenten](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Levenscyclus van identiteit plannen
##### [Taken](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Acceptatiestrategie](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Volgende stappen](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Hulpprogramma's vergelijken](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Gebruikers beheren
### [Nieuwe gebruikers toevoegen aan Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Gebruikersprofielen beheren](fundamentals/active-directory-users-profile-azure-portal.md)
### [Gebruikerswachtwoorden opnieuw instellen](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Accounts delen](active-directory-sharing-accounts.md)
### [Gebruikers aan beheerdersrollen toewijzen](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Een verwijderde gebruiker herstellen](fundamentals/active-directory-users-restore.md)
### [Gastgebruikers toevoegen van een andere directory (B2B)](b2b/what-is-b2b.md)
#### [Beheerders die B2B-gebruikers toevoegen](b2b/add-users-administrator.md)
#### [Informatiewerkers die B2B-gebruikers toevoegen](b2b/add-users-information-worker.md)
#### [API en aanpassingen](b2b/customize-invitation-api.md)
#### [Google-federatie](b2b/google-federation.md)
#### [Code- en Azure PowerShell-voorbeelden](b2b/code-samples.md)
#### [Voorbeeld self-service-aanmelding portal](b2b/self-service-portal.md)
#### [Uitnodigingse-mail](b2b/invitation-email-elements.md)
#### [Uitnodiging inwisselen](b2b/redemption-experience.md)
#### [B2B-gebruikers zonder uitnodiging toevoegen](b2b/add-user-without-invite.md)
#### [Uitnodigingen toestaan of blokkeren](b2b/allow-deny-list.md)
#### [Voorwaardelijke toegang voor B2B](b2b/conditional-access.md)
#### [Beleid voor B2B-deling](b2b/delegate-invitations.md)
#### [Een B2B-gebruiker toevoegen aan een rol](b2b/add-guest-to-role.md)
#### [Dynamische groepen en B2B-gebruikers](b2b/use-dynamic-groups.md)
#### [Een organisatie verlaten](b2b/leave-the-organization.md)
#### [Controle en rapportage](b2b/auditing-and-reporting.md)
#### [B2B voor hybride organisaties](b2b/hybrid-organizations.md)
##### [B2B-gebruikers toegang geven tot lokale apps](b2b/hybrid-cloud-to-on-premises.md)
##### [Lokale gebruikers toegang geven tot cloud-apps](b2b/hybrid-on-premises-to-cloud.md)
#### [Extern delen B2B en Office 365](b2b/o365-external-user.md)
#### [B2B-licentieverlening](b2b/licensing-guidance.md)
#### [Huidige beperkingen](b2b/current-limitations.md)
#### [Veelgestelde vragen](b2b/faq.md)
#### [Problemen oplossen voor B2B](b2b/troubleshoot.md)
#### [Informatie over de B2B-gebruiker](b2b/user-properties.md)
#### [B2B-gebruikerstoken](b2b/user-token.md)
#### [B2B voor in Azure AD geïntegreerde apps](b2b/configure-saas-apps.md)
#### [B2B-gebruikersclaims toewijzen](b2b/claims-mapping.md)
#### [B2B-samenwerking vergelijken met B2C](b2b/compare-with-b2c.md)
#### [Ondersteuning krijgen voor B2B](b2b/get-support.md)

## [Groepen en leden beheren](fundamentals/active-directory-manage-groups.md)
### [Groepen beheren](fundamentals/active-directory-groups-create-azure-portal.md)
### [Herstellen van een verwijderde Office 365-groep](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Groepsinstellingen beheren](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Rapporten beheren](reports-monitoring/overview-reports.md)
### [Aanmeldingsactiviteit](reports-monitoring/concept-sign-ins.md)
### [Activiteit controleren](reports-monitoring/concept-audit-logs.md)
### [Gebruikers die risico lopen](reports-monitoring/concept-user-at-risk.md)
### [Riskante aanmeldingen](reports-monitoring/concept-risky-sign-ins.md)
### [Risicogebeurtenissen](reports-monitoring/concept-risk-events.md)
### [Logboeken monitoren met behulp van Azure Monitor](reports-monitoring/overview-activity-logs-in-azure-monitor.md)
### [Veelgestelde vragen](reports-monitoring/reports-faq.md)

### Taken
#### [Benoemde locaties configureren](active-directory-named-locations.md)
#### [Activiteitenrapporten vinden](reports-monitoring/howto-find-activity-reports.md)
#### [Het inhoudspakket voor Azure AD Power BI gebruiken](reports-monitoring/howto-power-bi-content-pack.md)
#### [Gebruikers herstellen voor wie wordt aangegeven dat ze risico lopen](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Activiteitenlogboeken naar een Azure Event Hub leiden](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Activiteitenlogboeken in een Azure-opslagaccount archiveren](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Activiteitenlogboeken met Splunk integreren via Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)

### Naslaginformatie
#### [Retentie](reports-monitoring/reference-reports-data-retention.md)
#### [Wachttijden](reports-monitoring/reference-reports-latencies.md)
#### [Activiteitsverwijzing controleren](reports-monitoring/reference-audit-activities.md)
#### [Foutcodes voor aanmeldingsactiviteit](reports-monitoring/reference-sign-ins-error-codes.md)
#### [Het schema van het auditlogboek interpreteren in Azure Monitor](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [Het schema van het aanmeldingslogboek interpreteren in Azure Monitor](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### Problemen oplossen
#### [Ontbrekende gegevens in Azure AD-activiteitenlogboeken](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [Ontbrekende gegevens in downloads](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Fouten in inhoudspakketten Azure AD-activiteitenlogboeken](reports-monitoring/troubleshoot-content-pack.md)
#### [Fouten in de rapportage-API voor Azure AD](reports-monitoring/troubleshoot-graph-api.md)

### [Toegang op programmeerniveau](reports-monitoring/concept-reporting-api.md)
#### [Vereisten](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [Certificaten gebruiken](reports-monitoring/tutorial-access-api-with-certificates.md)

## [Wachtwoorden beheren](authentication/concept-sspr-howitworks.md)
### Gebruikersdocumenten
#### [Uw wachtwoord opnieuw instellen of wijzigen](user-help/active-directory-passwords-update-your-own-password.md)
#### [Registreren voor de selfservice voor wachtwoordherstel](user-help/active-directory-passwords-reset-register.md)


## Apps beheren
### [Overzicht](manage-apps/what-is-application-management.md)
### [Aan de slag](manage-apps/plan-an-application-integration.md)
### [Zelfstudies voor SaaS-app-integratie](saas-apps/tutorial-list.md)

### [Gebruikers voor SaaS-apps inrichten en de inrichting ongedaan maken](active-directory-saas-app-provisioning.md) 
#### [Zelfstudies voor app-integratie](saas-apps/tutorial-list.md) 
#### [Inrichting automatiseren voor apps die SCIM ondersteunen](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md) 
#### [Expressies schrijven voor kenmerktoewijzingen](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Bereikfilters toevoegen](active-directory-saas-scoping-filters.md) 
#### [Rapporteren over het automatisch inrichten van gebruikers](active-directory-saas-provisioning-reporting.md) 
#### [Problemen met het automatisch inrichten van gebruikers oplossen](active-directory-application-provisioning-content-map.md) 

### [Toegang tot apps op afstand via App Proxy](manage-apps/application-proxy.md)
#### Aan de slag
##### [App-Proxy inschakelen](manage-apps/application-proxy-enable.md)
##### [Apps publiceren](manage-apps/application-proxy-publish-azure-portal.md)
##### [Aangepaste domeinen](manage-apps/application-proxy-configure-custom-domain.md)
#### [Eenmalige aanmelding](manage-apps/application-proxy-single-sign-on.md)
##### [Eenmalige aanmelding met KCD](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [Eenmalige aanmelding met koppen](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [Eenmalige aanmelding met wachtwoordkluis](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Concepten
##### [Connectors](manage-apps/application-proxy-connectors.md)
##### [Beveiliging](manage-apps/application-proxy-security.md)
##### [Netwerken](manage-apps/application-proxy-network-topology.md)


##### [Upgraden van TMG of UAG](manage-apps/application-proxy-migration.md)

#### Geavanceerd configuratie
##### [Publiceren op afzonderlijke netwerken](manage-apps/application-proxy-connector-groups.md)
##### [Proxyservers](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Claimbewuste apps](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Systeemeigen client-apps](manage-apps/application-proxy-configure-native-client-application.md)
##### [Stille installatie](manage-apps/application-proxy-register-connector-powershell.md)
##### [Aangepaste startpagina](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Inlinelinks vertalen](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Jokertekens](active-directory-application-proxy-wildcard.md)
##### [Persoonlijke gegevens verwijderen](manage-apps/application-proxy-remove-personal-data.md)


#### Publicatie-overzicht
##### [Extern bureaublad](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Problemen oplossen](manage-apps/application-proxy-troubleshoot.md)

### Enterprise-apps beheren
#### [Een toepassing toevoegen](manage-apps/add-application-portal.md)
#### [Tenant-apps bekijken](manage-apps/view-applications-portal.md)
#### [Eenmalige aanmelding configureren](manage-apps/configure-single-sign-on-portal.md)
#### [Gebruikers toewijzen](manage-apps/assign-user-or-group-access-portal.md)
#### [Huisstijl aanpassen](manage-apps/change-name-or-logo-portal.md)
#### [Gebruikersaanmeldingen uitschakelen](manage-apps/disable-user-sign-in-portal.md)
#### [Gebruikers verwijderen](manage-apps/remove-user-or-group-access-portal.md)

#### [Inrichting van gebruikersaccount beheren](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [Geavanceerde certificaatondertekening voor SAML-apps](manage-apps/certificate-signing-options.md)
#### [Een toepassing verbergen voor een gebruiker](manage-apps/hide-application-from-user-portal.md)
### [Automatisch versnellen van aanmelding configureren met behulp van HRD-beleid](manage-apps/configure-authentication-for-federated-users-portal.md)
### [AD FS-apps migreren naar Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Toegang tot apps beheren](manage-apps/what-is-access-management.md)
#### [Toegang via eenmalige aanmelding](manage-apps/what-is-single-sign-on.md)
#### [Certificaten voor SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Beperkingen voor tenants](manage-apps/tenant-restrictions.md)
#### [SCIM gebruiken voor het inrichten van gebruikers](manage-apps/use-scim-to-provision-users-and-groups.md)


### Problemen oplossen



#### Toegangsvenster
##### [App wordt niet weergegeven](application-access-panel-unexpected-application-not-appearing.md)
##### [App wordt onverwacht weergegeven](application-access-panel-unexpected-application-appears.md)
##### [Het aanmelden is mislukt](application-access-panel-web-sign-in-problem.md)
##### [Fout bij het installeren van browserextensie](application-access-panel-extension-problem-installing.md)
##### [Selfservice-app-toegang gebruiken](application-access-panel-self-service-applications-how-to.md)
##### [Fout bij selfservice-app-toegang gebruiken](application-access-panel-self-service-applications-problem.md)

#### Een app toevoegen
##### [Type app kiezen](application-config-add-app-problem-how-to-choose-application-type.md)
##### [Veelvoorkomende problemen - galerie-apps](application-config-add-app-problem-problem-adding-gallery-app.md)
##### [Veelvoorkomende problemen - niet-galerie-apps](application-config-add-app-problem-problem-adding-non-gallery-app.md)

#### Toepassingsproxy
##### [Probleem bij weergeven app-pagina](application-proxy-page-appearance-broken-problem.md)
##### [Applicatie laden duurt te lang](application-proxy-page-load-speed-problem.md)
##### [Koppelingen op de toepassingspagina werken niet](application-proxy-page-links-broken-problem.md)
##### [Welke poorten moet ik openen voor mijn app?](application-proxy-connectivity-ports-how-to.md)
##### [Geen werkende connector in een connectorgroep voor mijn app](application-proxy-connectivity-no-working-connector.md)
##### [Configureren in beheerportal](application-proxy-config-how-to.md)
##### [Eenmalige aanmelding voor mijn app configureren](application-proxy-config-sso-how-to.md)
##### [Probleem bij het maken van een app in de beheerportal](application-proxy-config-problem.md)
##### [Kerberos-beperkte overdracht configureren](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configureren met PingAccess](application-proxy-back-end-ping-access-how-to.md)
##### [De fout 'Kan deze zakelijke toepassing niet openen'](application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Probleem bij het installeren van de connector voor de toepassingsproxyagent](application-proxy-connector-installation-problem.md)


#### Een toepassing registreren
##### [Geef de velden op voor het toepassingsobject](application-dev-registration-config-specific-application-property-how-to.md)
##### [Standaardinstellingen levensduur van token wijzigen](application-dev-registration-config-change-token-lifetime-how-to.md)

#### Verificatie
##### [Eindpunten configureren](application-dev-registration-config-how-to.md)

#### Voorwaardelijke toegang
##### [Klant voldeed niet aan voorvereisten voor apparaatregistratie](active-directory-conditional-access.md)
##### [De tenant wordt geblokkeerd vanwege verkeerd ingestelde beleidsregels voor voorwaardelijke toegang](active-directory-conditional-access-device-remediation.md)
##### [Hoe en wanneer worden regels buiten het bedrijfsnetwerk actief?](https://aka.ms/calocation)
##### [Hoe kan ik het aantal apparaten die een gebruiker mag registreren in Azure AD verhogen?](active-directory-azureadjoin-setup.md)
##### [Hoe kan ik voorwaardelijke toegang instellen voor Exchange Online?](https://aka.ms/csforexchange)
##### [Hoe kan ik voorwaardelijke toegang voor Windows 7-apparaten instellen](active-directory-conditional-access.md#device-based-conditional-access)
##### [In welke toepassingen wordt voorwaardelijke toegang ondersteund?](active-directory-conditional-access-supported-apps.md)

#### Een API zoeken
##### [Een API zoeken](application-dev-api-find-an-api-how-to.md)

#### Toegang beheren
##### [Gebruikers en groepen toewijzen aan een app](application-access-assignment-how-to-add-assignment.md)
##### [Toegang van gebruikers tot app verwijderen](application-access-assignment-how-to-remove-assignment.md)
##### [Selfservice-app-toewijzing configureren](application-access-self-service-how-to.md)
##### [Onverwachte gebruiker toegewezen](application-access-unexpected-user-assignment.md)
##### [Onverwachte app in de lijst met toepassingen](application-access-unexpected-application.md)

#### Apps met meerdere tenants
##### [Een nieuwe app configureren](application-dev-setup-multi-tenant-app.md)
##### [Toevoegen aan de app-galerie](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Machtigingen
##### [Machtigingen kiezen voor een API](application-dev-perms-for-given-api.md)
##### [Machtigingen verlenen aan mijn app](application-dev-registration-config-grant-permissions-how-to.md)
##### [Gedelegeerde vs. toepassingsmachtigingen](application-dev-delegated-and-app-perms.md)
##### [Toepassingstoestemming](application-dev-consent-framework.md)

#### Inrichten
##### [Hoelang het duurt](application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Uurregistratie - galerie-app](application-provisioning-when-will-provisioning-finish.md)
##### [Gebruikersinrichting configureren - galerie-app](application-provisioning-config-how-to.md)
##### [Probleem bij gebruikersinrichting configureren - galerie-app](application-provisioning-config-problem.md)
##### [Probleem bij het opslaan van administrorreferenties tijdens het configureren van gebruikersinrichting galerie-app](application-provisioning-config-problem-storage-limit.md)
##### [Gebruikers zijn niet ingericht - galerie-app](application-provisioning-config-problem-no-users-provisioned.md)
##### [Verkeerde gebruikers ingericht - galerie-app](application-provisioning-config-problem-wrong-users-provisioned.md)

#### Eenmalige aanmelding
##### [Een methode kiezen](application-config-sso-how-to-choose-sign-on-method.md)
##### [Configureren](application-dev-registration-config-sso-how-to.md)
##### [Federatief configureren - galerie-apps](application-config-sso-how-to-configure-federated-sso-gallery.md)
##### [Veelvoorkomende problemen bij federatief configureren - galerie-apps](application-config-sso-problem-configure-federated-sso-gallery.md)
##### [Federatief configureren - niet-galerie-apps](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
##### [Veelvoorkomende problemen bij federatief configureren - niet-galerie-apps](application-config-sso-problem-configure-federated-sso-non-gallery.md)
##### [Wachtwoord configureren - galerie-apps](application-config-sso-how-to-configure-password-sso-gallery.md)
##### [Veelvoorkomende problemen bij wachtwoord configureren - galerie-apps](application-config-sso-problem-configure-password-sso-gallery.md)
##### [Wachtwoord configureren - niet-galerie-apps](application-config-sso-how-to-configure-password-sso-non-gallery.md)
##### [Veelvoorkomende problemen bij wachtwoord configureren - niet-galerie-apps](application-config-sso-problem-configure-password-sso-non-gallery.md)

#### Problemen bij aanmelden gebruiker
##### [Onverwacht instemmingsprompt](application-sign-in-unexpected-user-consent-prompt.md)
##### [Fout bij toestemming gebruiker](application-sign-in-unexpected-user-consent-error.md)
##### [Problemen met aanmelden vanaf aangepaste portal](application-sign-in-other-problem-deeplink.md)
##### [Problemen met aanmelden vanaf toegangsvenster](application-sign-in-other-problem-access-panel.md)
##### [Fout bij de aanmeldingspagina van toepassing](application-sign-in-problem-application-error.md)
##### [Probleem met wachtwoord eenmalige aanmelding - niet-galerie-app](application-sign-in-problem-password-sso-non-gallery.md)
##### [Probleem met wachtwoord eenmalige aanmelding - galerie-app](application-sign-in-problem-password-sso-gallery.md)
##### [Probleem met aanmelden bij een Microsoft-app](application-sign-in-problem-first-party-microsoft.md)
##### [Probleem met federatieve eenmalige aanmelding - niet-galerie-app](application-sign-in-problem-federated-sso-non-gallery.md)
##### [Probleem met federatieve eenmalige aanmelding - galerie-app](application-sign-in-problem-federated-sso-gallery.md)
##### [Probleem met zelfontwikkelde app](application-sign-in-problem-custom-dev.md)
##### [Probleem met on-premises app - toepassingsproxy](application-sign-in-problem-on-premises-application-proxy.md)

### [Apps ontwikkelen](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Documentbibliotheek](active-directory-apps-index.md)

## Uw directory beheren
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Aangepaste domeinnamen
#### [Snelstartgids](fundamentals/add-custom-domain.md)
### [Uw directory beheren](fundamentals/active-directory-administer.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Instellingen voor groepsbeleid](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10-instellingen](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [On-premises identiteiten integreren met Azure AD Connect](./connect/active-directory-aadconnect.md)

### [De levensduur van tokens configureren](active-directory-configurable-token-lifetimes.md)

## Toegangsbeoordelingen
### [Overzicht toegangsbeoordelingen](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Complete an access review](active-directory-azure-ad-controls-complete-access-review.md) (Een toegangscontrole voltooien)
### [Create an access review](active-directory-azure-ad-controls-create-access-review.md) (Een toegangscontrole maken)
### [How to perform an access review](active-directory-azure-ad-controls-perform-access-review.md) (Een toegangscontrole uitvoeren)
### [Uw toegang controleren](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Toegang voor gasten met toegangsbeoordelingen](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Gebruikerstoegang met beoordelingen beheren](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Programma's en besturingselementen beheren](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Resultaten van de toegangsbeoordeling ophalen](active-directory-azure-ad-controls-retrieve-access-review.md)

## Uw identiteiten beveiligen
### Verificatie op basis van certificaat
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Aan de slag](active-directory-certificate-based-authentication-get-started.md)

### Azure AD-identiteitsbeveiliging
#### [Overzicht](identity-protection/overview.md)
#### [Inschakelen](identity-protection/enable.md)
#### [Beveiligingslekken detecteren](identity-protection/vulnerabilities.md)
#### [Risicogebeurtenissen](active-directory-identity-protection-risk-events.md)
#### [Meldingen](identity-protection/notifications.md)
#### [Aanmeldingservaring](identity-protection/flows.md)
#### [Risicogebeurtenissen simuleren](identity-protection/playbook.md)
#### [Gebruikers deblokkeren](identity-protection/howto-unblock-user.md)
#### [Veelgestelde vragen](identity-protection/faqs.md)
#### [Woordenlijst](identity-protection/glossary.md)
#### [Microsoft Graph](identity-protection/graph-get-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

## [AD FS in Azure implementeren](active-directory-aadconnect-azure-adfs.md)
### [Hoge beschikbaarheid](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Hash-algoritme van de handtekening wijzigen](active-directory-federation-sha256-guidance.md)

## [Problemen oplossen](fundamentals/active-directory-troubleshooting-support-howto.md)

## Azure AD Proof of Concept (PoC) implementeren
### [PoC Playbook: inleiding](active-directory-playbook-intro.md)
### [PoC Playbook: onderdelen](active-directory-playbook-ingredients.md)
### [PoC Playbook: implementatie](active-directory-playbook-implementation.md)
### [PoC Playbook: bouwstenen](active-directory-playbook-building-blocks.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell-cmdlets](/powershell/azure/overview)
## [Naslaginformatie over de Java-API](/java/api)
## [.NET API](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Verwant
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health (Engelstalig)](./connect-health/active-directory-aadconnect-health.md)
## [Azure AD for Developers](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# Resources
## [Azure AD-implementatieplannen](./fundamentals/active-directory-deployment-plans.md)
## [Azure-feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Service-updates](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
