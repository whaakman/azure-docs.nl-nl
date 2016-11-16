# Architectuur

## Basisinformatie over de cloud

### [Robuuste toepassingen ontwerpen voor Azure](guidance-resiliency-overview.md)
#### [Controlelijst voor tolerantie](guidance-resiliency-checklist.md)
#### [Analyse van foutmodus](guidance-resiliency-failure-mode-analysis.md)

#### [Herstel na noodgevallen voor Microsoft Azure-toepassingen](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Herstel na noodgevallen en hoge beschikbaarheid voor Microsoft Azure-toepassingen](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Hoge beschikbaarheid voor Microsoft Azure-toepassingen](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Controlelijst voor hoge beschikbaarheid](..\resiliency\resiliency-high-availability-checklist.md)
#### [Azure-servicespecifieke tolerantierichtlijnen](..\resiliency\resiliency-service-guidance-index.md)
#### [Herstel na gegevensbeschadiging en onbedoelde verwijdering](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Herstel na lokale fouten in Azure](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Herstel na een serviceonderbreking in de gehele regio](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Herstel van on-premises naar Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technische richtlijnen voor de tolerantie van Azure](..\resiliency\resiliency-technical-guidance.md)


## Referentiearchitecturen

### Referentiearchitecturen berekenen
#### [Een virtuele Linux-machine uitvoeren op Azure](guidance-compute-single-vm-linux.md)
#### [Een virtuele Windows-machine uitvoeren op Azure](guidance-compute-single-vm.md)
#### [Meerdere virtuele machines uitvoeren op Azure voor schaalbaarheid en beschikbaarheid](guidance-compute-multi-vm.md)
#### [Virtuele Linux-machines uitvoeren voor een n-tier-architectuur op Azure](guidance-compute-n-tier-vm-linux.md)
#### [Virtuele Windows-machines uitvoeren voor een n-tier-architectuur op Azure](guidance-compute-n-tier-vm.md)
#### [Virtuele Linux-machines uitvoeren in meerdere regio’s voor hoge beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)
#### [Virtuele Windows-machines uitvoeren in meerdere regio’s voor hoge beschikbaarheid](guidance-compute-multiple-datacenters.md)

### [Uw on-premises netwerk verbinden met Azure](guidance-connecting-your-on-premises-network-to-azure.md)
#### [Een hybride netwerkarchitectuur implementeren met Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Een hybride netwerkarchitectuur implementeren met Azure en On-premises VPN](guidance-hybrid-network-vpn.md)
#### [Een hoog beschikbare, hybride netwerkarchitectuur implementeren](guidance-hybrid-network-expressroute-vpn-failover.md)

### De grens van de cloud in Azure beveiligen
#### [Een beveiligde, hybride netwerkarchitectuur implementeren in Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [Een DMZ tussen Azure en internet implementeren](guidance-iaas-ra-secure-vnet-dmz.md)

### [Identiteit beheren in Azure](guidance-ra-identity.md)
#### [Azure Active Directory implementeren](guidance-identity-aad.md)
#### [ADDS (Active Directory Directory Services) uitbreiden naar Azure](guidance-identity-adds-extend-domain.md)
#### [Een ADDS-resourceforest (Active Directory Directory Services) maken in Azure](guidance-identity-adds-resource-forest.md)
#### [ADFS (Active Directory Federation Services) implementeren in Azure](guidance-identity-adfs.md)

### Referentiearchitectuur van PaaS-webtoepassing
#### [Azure-referentiearchitectuur: basiswebtoepassing](guidance-web-apps-basic.md)
#### [Azure referentiearchitectuur: webtoepassing met hoge beschikbaarheid](guidance-web-apps-multi-region.md)
#### [Schaalbaarheid verbeteren in een webtoepassing](guidance-web-apps-scalability.md)


## Cloudontwerppatronen

## Aanbevolen procedures voor cloudtoepassingen

### [API-ontwerprichtlijnen](..\best-practices-api-design.md)
### [API-implementatierichtlijnen](..\best-practices-api-implementation.md)
### [Richtlijnen voor automatisch schalen](..\best-practices-auto-scaling.md)
### [Beschikbaarheidscontrolelijst](..\best-practices-availability-checklist.md)
### [Richtlijnen voor achtergrondtaken](..\best-practices-background-jobs.md)
### [Bedrijfscontinuïteit en herstel na noodgevallen (BCDR: gekoppelde Azure-regio’s](..\best-practices-availability-paired-regions.md)
### [Richtlijnen voor caching](..\best-practices-caching.md)
### [CDN-richtlijnen (Content Delivery Network)](..\best-practices-cdn.md)
### [Richtlijnen voor gegevenspartitionering](..\best-practices-data-partitioning.md)
### [Richtlijnen voor controle en diagnose](..\best-practices-monitoring.md)
### [Microsoft-cloudservices en -netwerkbeveiliging](..\best-practices-network-security.md)
### [Patronen voor het ontwerpen van Azure Resource Manager-sjablonen](..\best-practices-resource-manager-design-templates.md)
### [Aanbevolen naamgevingsregels voor Azure-resources](guidance-naming-conventions.md)
### [Beveiligingsoverwegingen voor Azure Resource Manager](..\best-practices-resource-manager-security.md)
### [Status delen in Azure Resource Manager-sjablonen](..\best-practices-resource-manager-state.md)
### [Algemene richtlijnen voor opnieuw proberen](..\best-practices-retry-general.md)
### [Servicespecifieke richtlijnen voor opnieuw proberen](..\best-practices-retry-service-specific.md)
### [Controlelijst voor schaalbaarheid](..\best-practices-scalability-checklist.md)


## Scenariorichtlijnen

### [Richtlijnen voor Elasticsearch op Azure](guidance-elasticsearch.md)
#### [Elasticsearch uitvoeren op Azure](guidance-elasticsearch-running-on-azure.md)
#### [De gegevensopname optimaliseren voor Elasticsearch op Azure](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [De gegevenssamenvoeging en queryprestaties optimaliseren voor Elasticsearch op Azure](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Tolerantie en herstel configureren voor Elasticsearch op Azure](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Een prestatietestomgeving maken voor Elasticsearch op Azure](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Een JMeter-testplan implementeren voor Elasticsearch](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Een JMeter JUnit-sampler implementeren voor het testen van de prestaties van Elasticsearch](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [De geautomatiseerde Elasticsearch-tolerantietests uitvoeren](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [De geautomatiseerde Elasticsearch-prestatietests uitvoeren](guidance-elasticsearch-running-automated-performance-tests.md)

### [Identiteitsbeheer voor multitenant-toepassingen in Microsoft Azure](guidance-multitenant-identity.md)
#### [Introductie van identiteitsbeheer voor multitenant-toepassingen in Microsoft Azure](guidance-multitenant-identity-intro.md)
#### [Over de toepassing Tailspin Surveys](guidance-multitenant-identity-tailspin.md)
#### [Verificatie in multitenant-apps met Azure AD en OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Werken met op claims gebaseerde identiteiten in multitenant-toepassingen](guidance-multitenant-identity-claims.md)
#### [Registratie en tenant-onboarding in een multitenant-toepassing](guidance-multitenant-identity-signup.md)
#### [Toepassingsrollen in multitenant-toepassingen](guidance-multitenant-identity-app-roles.md)
#### [Op rollen en resources gebaseerde autorisatie in multitenant-toepassingen](guidance-multitenant-identity-authorize.md)
#### [Een back-end web-API beveiligen in een multitenant-toepassing](guidance-multitenant-identity-web-api.md)
#### [Toegangstokens opslaan in cache in een multitenant-toepassing](guidance-multitenant-identity-token-cache.md)
#### [Federeren met AD FS van een klant voor multitenant-apps in Azure](guidance-multitenant-identity-adfs.md)
#### [Clienbevestiging gebruiken om toegangstokens op te halen uit Azure AD](guidance-multitenant-identity-client-assertion.md)
#### [Azure Key Vault gebruiken om toepassingsgeheimen te beveiligen](guidance-multitenant-identity-keyvault.md)

#### [Virtuele apparaten implementeren met hoge beschikbaarheid](guidance-nva-ha.md)


<!--HONumber=Nov16_HO2-->


