# [Documentatie voor Azure Active Directory](index.md)

# Overzicht
## [Wat is Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Over Azure-identiteitsbeheer](fundamentals/identity-fundamentals.md)
## [Inzicht krijgen in Azure-identiteitsoplossingen](fundamentals/understand-azure-identity-solutions.md)
## [Azure-abonnementen koppelen](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Overwegingen met betrekking tot vestigingsplaats en gegevens](fundamentals/active-directory-data-storage-eu.md)
## [Veelgestelde vragen](fundamentals/active-directory-faq.md)
## [Nieuwe functies](fundamentals/whats-new.md)


# Aan de slag
## [Meld u aan voor Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Een aangepaste domeinnaam toevoegen](fundamentals/add-custom-domain.md)
## [Een bedrijfshuisstijl toevoegen](fundamentals/customize-branding.md)
## [Gebruikers toevoegen aan Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Licenties toewijzen aan een gebruikers](fundamentals/license-users-groups.md)
## [Self-service voor wachtwoord opnieuw instellen configureren](authentication/quickstart-sspr.md)
## [Privacy-informatie van uw organisatie toevoegen in Azure AD](active-directory-properties-area.md)
## [Een nieuwe tenant maken in Azure Active Directory](fundamentals/active-directory-access-create-new-tenant.md)


# Procedures
## Plannen en ontwerpen
### [Inzicht in de Azure AD-architectuur](fundamentals/active-directory-architecture.md)
### [Claimtoewijzing in Azure Active Directory](develop/active-directory-claims-mapping.md)
### [Een hybride identiteitsoplossing implementeren](hybrid/plan-hybrid-identity-design-considerations-overview.md)
#### Vereisten bepalen
##### [Identity](hybrid/plan-hybrid-identity-design-considerations-business-needs.md)
##### [Directory-synchronisatie](hybrid/plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-Factor Authentication](hybrid/plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Levenscyclus van identiteit plannen](hybrid/plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Plan voor gegevensbeveiliging](hybrid/plan-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Gegevensbeveiliging](hybrid/plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Inhoudsbeheer](hybrid/plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Toegangsbeheer](hybrid/plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Reageren op incidenten](hybrid/plan-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Levenscyclus van identiteit plannen
##### [Taken](hybrid/plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Acceptatiestrategie](hybrid/plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Volgende stappen](hybrid/plan-hybrid-identity-design-considerations-nextsteps.md)
#### [Hulpprogramma's vergelijken](hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md)

## Gebruikers beheren
### [Nieuwe gebruikers toevoegen aan Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Gebruikersprofielen beheren](fundamentals/active-directory-users-profile-azure-portal.md)
### [Gebruikerswachtwoorden opnieuw instellen](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Gebruikers aan beheerdersrollen toewijzen](fundamentals/active-directory-users-assign-role-azure-portal.md)
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
### [Een groep en de leden ervan verwijderen](fundamentals/active-directory-groups-delete-group.md)
### [Groepsinstellingen beheren](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Rapporten beheren](reports-monitoring/overview-reports.md)
### [Aanmeldingsactiviteit](reports-monitoring/concept-sign-ins.md)
### [Activiteit controleren](reports-monitoring/concept-audit-logs.md)
### [Gebruikers die risico lopen](reports-monitoring/concept-user-at-risk.md)
### [Riskante aanmeldingen](reports-monitoring/concept-risky-sign-ins.md)
### [Risicogebeurtenissen](reports-monitoring/concept-risk-events.md)
### [Logboeken monitoren met behulp van Azure Monitor](reports-monitoring/concept-activity-logs-in-azure-monitor.md)
### [Veelgestelde vragen](reports-monitoring/reports-faq.md)

### Taken
#### [Een aanmeldrapport downloaden](reports-monitoring/quickstart-download-sign-in-report.md)
#### [Een controlerapport downloaden](reports-monitoring/quickstart-download-audit-report.md)
#### [Benoemde locaties configureren](reports-monitoring/quickstart-configure-named-locations.md)
#### [Activiteitenrapporten vinden](reports-monitoring/howto-find-activity-reports.md)
#### [Het inhoudspakket voor Azure AD Power BI gebruiken](reports-monitoring/howto-power-bi-content-pack.md)
#### [Gebruikers herstellen voor wie wordt aangegeven dat ze risico lopen](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Activiteitenlogboeken naar een Azure Event Hub leiden](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Activiteitenlogboeken in een Azure-opslagaccount archiveren](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Activiteitenlogboeken met Splunk integreren via Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [Activiteitenlogboeken met SumoLogic integreren via Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)
#### [Activiteitenlogboeken met Log Analytics integreren via Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

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

## Apps beheren
### [Overzicht](manage-apps/what-is-application-management.md)
### [Aan de slag](manage-apps/plan-an-application-integration.md)
### [Zelfstudies voor SaaS-app-integratie](saas-apps/tutorial-list.md)

### [Gebruikers voor SaaS-apps inrichten en de inrichting ongedaan maken](manage-apps/user-provisioning.md) 
#### [Zelfstudies voor app-integratie](saas-apps/tutorial-list.md) 
#### [Inrichting automatiseren voor apps die SCIM ondersteunen](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Kenmerktoewijzingen aanpassen](manage-apps/customize-application-attributes.md) 
#### [Expressies schrijven voor kenmerktoewijzingen](manage-apps/functions-for-customizing-application-data.md) 
#### [Bereikfilters toevoegen](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [Rapporteren over het automatisch inrichten van gebruikers](manage-apps/check-status-user-account-provisioning.md) 
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
##### [Jokertekens](manage-apps/application-proxy-wildcard.md)
##### [Persoonlijke gegevens verwijderen](manage-apps/application-proxy-remove-personal-data.md)


#### Publicatie-overzicht
##### [Extern bureaublad](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
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

### [Inzicht in ervaringen met Azure AD-toepassingtoestemming](develop/application-consent-experience.md)

### Problemen oplossen



#### Toegangsvenster
##### [App wordt niet weergegeven](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [App wordt onverwacht weergegeven](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [Het aanmelden is mislukt](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [Fout bij het installeren van browserextensie](manage-apps/access-panel-extension-problem-installing.md)
##### [Selfservice-app-toegang gebruiken](manage-apps/access-panel-manage-self-service-access.md)
##### [Fout bij selfservice-app-toegang gebruiken](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### Een app toevoegen
##### [Type app kiezen](manage-apps/choose-application-type.md)
##### [Veelvoorkomende problemen - galerie-apps](manage-apps/adding-gallery-app-common-problems.md)
##### [Veelvoorkomende problemen - niet-galerie-apps](manage-apps/adding-non-gallery-app-common-problems.md)

#### Toepassingsproxy
##### [Probleem bij weergeven app-pagina](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [Applicatie laden duurt te lang](manage-apps/application-proxy-page-load-speed-problem.md)
##### [Koppelingen op de toepassingspagina werken niet](manage-apps/application-proxy-page-links-broken-problem.md)
##### [Welke poorten moet ik openen voor mijn app?](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [Geen werkende connector in een connectorgroep voor mijn app](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [Configureren in beheerportal](manage-apps/application-proxy-config-how-to.md)
##### [Eenmalige aanmelding voor mijn app configureren](manage-apps/application-proxy-config-sso-how-to.md)
##### [Probleem bij het maken van een app in de beheerportal](manage-apps/application-proxy-config-problem.md)
##### [Kerberos-beperkte overdracht configureren](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configureren met PingAccess](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### [De fout 'Kan deze zakelijke toepassing niet openen'](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Probleem bij het installeren van de connector voor de toepassingsproxyagent](manage-apps/application-proxy-connector-installation-problem.md)


#### Een toepassing registreren
##### [Geef de velden op voor het toepassingsobject](develop/registration-config-specific-application-property-how-to.md)
##### [Standaardinstellingen levensduur van token wijzigen](develop/registration-config-change-token-lifetime-how-to.md)

#### Verificatie
##### [Eindpunten configureren](develop/registration-config-how-to.md)

#### Voorwaardelijke toegang
##### [Klant voldeed niet aan voorvereisten voor apparaatregistratie](active-directory-conditional-access.md)
##### [Hoe en wanneer worden regels buiten het bedrijfsnetwerk actief?](https://aka.ms/calocation)
##### [Hoe kan ik het aantal apparaten die een gebruiker mag registreren in Azure AD verhogen?](active-directory-azureadjoin-setup.md)
##### [Hoe kan ik voorwaardelijke toegang instellen voor Exchange Online?](https://aka.ms/csforexchange)
##### [Hoe kan ik voorwaardelijke toegang voor Windows 7-apparaten instellen](active-directory-conditional-access.md#device-based-conditional-access)
##### [In welke toepassingen wordt voorwaardelijke toegang ondersteund?](active-directory-conditional-access-supported-apps.md)

#### Een API zoeken
##### [Een API zoeken](develop/api-find-an-api-how-to.md)

#### Toegang beheren
##### [Gebruikers en groepen toewijzen aan een app](manage-apps/methods-for-assigning-users-and-groups.md)
##### [Toegang van gebruikers tot app verwijderen](manage-apps/methods-for-removing-user-access.md)
##### [Selfservice-app-toewijzing configureren](manage-apps/manage-self-service-access.md)
##### [Onverwachte gebruiker toegewezen](manage-apps/ways-users-get-assigned-to-applications.md)
##### [Onverwachte app in de lijst met toepassingen](manage-apps/application-types.md)

#### Apps met meerdere tenants
##### [Een nieuwe app configureren](develop/setup-multi-tenant-app.md)
##### [Toevoegen aan de app-galerie](develop/registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Machtigingen
##### [Machtigingen kiezen voor een API](develop/perms-for-given-api.md)
##### [Gedelegeerde vs. toepassingsmachtigingen](develop/delegated-and-app-perms.md)
##### [Toepassingstoestemming](develop/consent-framework.md)

#### Inrichten
##### [Hoelang het duurt](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Uurregistratie - galerie-app](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [Gebruikersinrichting configureren - galerie-app](manage-apps/application-provisioning-config-how-to.md)
##### [Probleem bij gebruikersinrichting configureren - galerie-app](manage-apps/application-provisioning-config-problem.md)
##### [Probleem bij het opslaan van administrorreferenties tijdens het configureren van gebruikersinrichting galerie-app](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [Gebruikers zijn niet ingericht - galerie-app](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [Verkeerde gebruikers ingericht - galerie-app](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)
##### [SCIM-compatibiliteitsproblemen - app buiten de galerie](manage-apps/application-provisioning-config-problem-scim-compatibility.md)

#### Eenmalige aanmelding
##### [Een methode kiezen](manage-apps/single-sign-on-modes.md)
##### [Configureren](develop/registration-config-sso-how-to.md)
##### [Federatief configureren - galerie-apps](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [Veelvoorkomende problemen bij federatief configureren - galerie-apps](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [Federatief configureren - niet-galerie-apps](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [Veelvoorkomende problemen bij federatief configureren - niet-galerie-apps](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [Wachtwoord configureren - galerie-apps](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [Veelvoorkomende problemen bij wachtwoord configureren - galerie-apps](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [Wachtwoord configureren - niet-galerie-apps](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [Veelvoorkomende problemen bij wachtwoord configureren - niet-galerie-apps](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### Problemen bij aanmelden gebruiker
##### [Onverwacht instemmingsprompt](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [Fout bij toestemming gebruiker](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [Problemen met aanmelden vanaf aangepaste portal](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [Problemen met aanmelden vanaf toegangsvenster](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [Fout bij de aanmeldingspagina van toepassing](manage-apps/application-sign-in-problem-application-error.md)
##### [Probleem met wachtwoord eenmalige aanmelding - niet-galerie-app](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [Probleem met wachtwoord eenmalige aanmelding - galerie-app](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [Probleem met aanmelden bij een Microsoft-app](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [Probleem met federatieve eenmalige aanmelding - niet-galerie-app](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [Probleem met federatieve eenmalige aanmelding - galerie-app](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [Probleem met zelfontwikkelde app](manage-apps/application-sign-in-problem-custom-dev.md)
##### [Probleem met on-premises app - toepassingsproxy](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)


## Uw directory beheren
### [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
### Aangepaste domeinnamen
#### [Snelstartgids](fundamentals/add-custom-domain.md)
### [Uw directory beheren](fundamentals/active-directory-administer.md)
### [On-premises identiteiten integreren met Azure AD Connect](hybrid/whatis-hybrid-identity.md)

### [De levensduur van tokens configureren](develop/active-directory-configurable-token-lifetimes.md)

## Uw identiteiten beveiligen

### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)



## [Problemen oplossen](fundamentals/active-directory-troubleshooting-support-howto.md)

## Azure AD Proof of Concept (PoC) implementeren
### [PoC-playbook: Inleiding](active-directory-playbook-intro.md)
### [PoC-playbook: Onderdelen](active-directory-playbook-ingredients.md)
### [PoC-playbook: Implementatie](active-directory-playbook-implementation.md)
### [PoC-playbook: Bouwstenen](active-directory-playbook-building-blocks.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell-cmdlets](/powershell/azure/overview)
## [Naslaginformatie over de Java-API](/java/api)
## [.NET API](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Verwant
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
## [Azure AD Connect Health (Engelstalig)](hybrid/whatis-hybrid-identity-health.md)
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
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
