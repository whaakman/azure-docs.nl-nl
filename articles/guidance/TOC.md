# Architectuur

## Basisinformatie over de cloud

### [Tolerante toepassingen ontwerpen](guidance-resiliency-overview.md)
#### [Controlelijst voor tolerantie](guidance-resiliency-checklist.md)
#### [Analyse van foutmodus](guidance-resiliency-failure-mode-analysis.md)
#### [Herstel na noodgevallen](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Herstel na noodgeval en hoge beschikbaarheid](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Hoge beschikbaarheid](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Controlelijst voor hoge beschikbaarheid](..\resiliency\resiliency-high-availability-checklist.md)
#### [Azure-servicespecifieke tolerantierichtlijnen](..\resiliency\resiliency-service-guidance-index.md)
#### [Herstel na gegevensbeschadiging en onbedoelde verwijdering](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Herstel na lokale fouten](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Herstel na een serviceonderbreking in de gehele regio](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Herstel van on-premises naar Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technische richtlijnen voor de tolerantie van Azure](..\resiliency\resiliency-technical-guidance.md)


## Referentiearchitecturen

### [VM-werkbelastingen in Azure uitvoeren](guidance-ra-compute.md)
#### [Een Linux VM uitvoeren op Azure](guidance-compute-single-vm-linux.md)
#### [Een Windows VM uitvoeren op Azure](guidance-compute-single-vm.md)
#### [Meerdere virtuele machines uitvoeren op Azure voor schaalbaarheid en beschikbaarheid](guidance-compute-multi-vm.md)
#### [Linux VM's uitvoeren voor een architectuur met n-aantal lagen](guidance-compute-n-tier-vm-linux.md)
#### [Windows VM's uitvoeren voor een architectuur met n-aantal lagen](guidance-compute-n-tier-vm.md)
#### [Virtuele Linux-machines uitvoeren in meerdere regio’s voor hoge beschikbaarheid](guidance-compute-multiple-datacenters-linux.md)
#### [Virtuele Windows-machines uitvoeren in meerdere regio’s voor hoge beschikbaarheid](guidance-compute-multiple-datacenters.md)

### [Uw on-premises netwerk verbinden met Azure](guidance-ra-hybrid-networking.md)
#### [Hybride netwerkarchitectuur met Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Hybride netwerkarchitectuur met Azure en On-premises VPN](guidance-hybrid-network-vpn.md)
#### [Zeer beschikbare hybride netwerkarchitectuur](guidance-hybrid-network-expressroute-vpn-failover.md)

### [De grens van de cloud beveiligen in Azure](guidance-ra-network-security.md)
#### [Veilige hybride netwerkarchitectuur in Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [DMZ tussen Azure en internet](guidance-iaas-ra-secure-vnet-dmz.md)

### [Identiteit beheren in Azure](guidance-ra-identity.md)
#### [Azure Active Directory implementeren](guidance-identity-aad.md)
#### [ADDS uitbreiden naar Azure](guidance-identity-adds-extend-domain.md)
#### [Een ADDS bron-forest maken in Azure](guidance-identity-adds-resource-forest.md)
#### [ADFS implementeren in Azure](guidance-identity-adfs.md)

### [Webtoepassingsarchitecturen voor Azure App Service](guidance-ra-app-service.md)
#### [Algemene webtoepassing](guidance-web-apps-basic.md)
#### [Webtoepassing met hoge beschikbaarheid](guidance-web-apps-multi-region.md)
#### [De schaalbaarheid verbeteren in een webtoepassing](guidance-web-apps-scalability.md)


## Aanbevolen procedures voor cloudtoepassingen

### [API-ontwerprichtlijnen](..\best-practices-api-design.md)
### [API-implementatierichtlijnen](..\best-practices-api-implementation.md)
### [Richtlijnen voor automatisch schalen](..\best-practices-auto-scaling.md)
### [Beschikbaarheidscontrolelijst](..\best-practices-availability-checklist.md)
### [Richtlijnen voor achtergrondtaken](..\best-practices-background-jobs.md)
### [Gekoppelde Azure-regio 's](..\best-practices-availability-paired-regions.md)
### [Richtlijnen voor caching](..\best-practices-caching.md)
### [Richtlijnen voor Content Delivery Network](..\best-practices-cdn.md)
### [Richtlijnen voor gegevenspartitionering](..\best-practices-data-partitioning.md)
### [Richtlijnen voor controle en diagnose](..\best-practices-monitoring.md)
### [Microsoft-cloudservices en -netwerkbeveiliging](..\best-practices-network-security.md)
### [Patronen voor het ontwerpen van Azure Resource Manager-sjablonen](..\best-practices-resource-manager-design-templates.md)
### [Aanbevolen naamgevingsregels voor Azure-resources](guidance-naming-conventions.md)
### [Status delen in Azure Resource Manager-sjablonen](..\best-practices-resource-manager-state.md)
### [Algemene richtlijnen voor opnieuw proberen](..\best-practices-retry-general.md)
### [Servicespecifieke richtlijnen voor opnieuw proberen](..\best-practices-retry-service-specific.md)
### [Controlelijst voor schaalbaarheid](..\best-practices-scalability-checklist.md)


## Scenariorichtlijnen

### [Richtlijnen voor Elasticsearch op Azure](guidance-elasticsearch.md)
#### [Elasticsearch uitvoeren op Azure](guidance-elasticsearch-running-on-azure.md)
#### [De prestaties van gegevensopname afstemmen](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Gegevenssamenvoeging en queryprestaties afstemmen](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Tolerantie en herstel configureren](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Een prestatietestomgeving maken](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Een JMeter-testplan implementeren](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Een JMeter JUnit-sampler implementeren](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [De geautomatiseerde tolerantietests uitvoeren](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [De geautomatiseerde prestatietests uitvoeren](guidance-elasticsearch-running-automated-performance-tests.md)

### [Identiteitsbeheer voor multitenant-toepassingen](guidance-multitenant-identity.md)
#### [Overzicht](guidance-multitenant-identity-intro.md)
#### [De toepassing Tailspin Surveys](guidance-multitenant-identity-tailspin.md)
#### [Verificatie met Azure AD en OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Op claims gebaseerde identiteiten](guidance-multitenant-identity-claims.md)
#### [Registratie en tenant voorbereiden](guidance-multitenant-identity-signup.md)
#### [Toepassingsrollen](guidance-multitenant-identity-app-roles.md)
#### [Op rollen en resources gebaseerde autorisatie](guidance-multitenant-identity-authorize.md)
#### [Een back-end web-API beveiligen](guidance-multitenant-identity-web-api.md)
#### [Toegangstokens opslaan in cache](guidance-multitenant-identity-token-cache.md)
#### [Federatie met AD FS van een klant](guidance-multitenant-identity-adfs.md)
#### [Clientbevestiging gebruiken om toegangstokens op te halen](guidance-multitenant-identity-client-assertion.md)
#### [Azure Key Vault gebruiken om toepassingsgeheimen te beveiligen](guidance-multitenant-identity-keyvault.md)
#### [Virtuele apparaten implementeren met hoge beschikbaarheid](guidance-nva-ha.md)
# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
