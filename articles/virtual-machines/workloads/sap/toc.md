# Overzicht
## [Aan de slag](get-started.md)
## [Certificering](sap-certifications.md)
# SAP HANA op Azure (grote exemplaren)
## Overzicht
### [Wat is SAP HANA in Azure (grote exemplaren)?](hana-overview-architecture.md)
### [Inzicht in de voorwaarden](hana-know-terms.md)
### [Certificering](hana-certification.md)
### [Beschikbare SKU's voor HLI](hana-available-skus.md)
### [Grootte aanpassen](hana-sizing.md)
### [Voorbereidingsvereisten](hana-onboarding-requirements.md)
### [SAP HANA-gegevenslagen en extensieknooppunten](hana-data-tiering-extension-nodes.md)
### [Bewerkingsmodel en verantwoordelijkheden](hana-operations-model.md)
## Architectuur
### [Algemene architectuur](hana-architecture.md)
### [Netwerkarchitectuur](hana-network-architecture.md)
### [Opslagarchitectuur](hana-storage-architecture.md)
### [Scenario's met HLI-ondersteuning](hana-supported-scenario.md)
## Infrastructuur en connectiviteit
### [HLI-implementatie](hana-overview-infrastructure-connectivity.md)
### [Azure-VM's verbinden met grote HANA-instanties](hana-connect-azure-vm-large-instances.md)
### [Een VNet verbinden met grote HANA-instanties via ExpressRoute](hana-connect-vnet-express-route.md)
### [Aanvullende netwerkvereisten](hana-additional-network-requirements.md)
## SAP HANA installeren
### [De configuratie valideren](hana-installation.md)
### [Voorbeeld van HANA-installatie](hana-example-installation.md)
## Hoge beschikbaarheid en herstel na noodgevallen
### [Opties en overwegingen](hana-overview-high-availability-disaster-recovery.md)
### [Back-up en herstel](hana-backup-restore.md)
### [Principes en voorbereiding](hana-concept-preparation.md)
### [Procedure bij failover van het noodherstel](hana-failover-procedure.md)
## Probleemoplossing en bewaking
### [Bewaking van HLI](troubleshooting-monitoring.md)
### [Bewaken en problemen oplossen vanaf de HANA-zijde](hana-monitor-troubleshoot.md)
## Procedures
### [HA met STONITH instellen](ha-setup-with-stonith.md)
### [Back-up van het besturingssysteem voor SKU's type II](os-backup-type-ii-skus.md)
### [Bijwerken van het besturingssysteem voor grote HANA-exemplaren](os-upgrade-hana-large-instance.md)
### [SMT-server instellen voor SUSE Linux](hana-setup-smt.md)
# SAP HANA in virtuele machines op Azure
## [SAP HANA-installatie met enkel exemplaar](hana-get-started.md)
## [Implementatiehandleiding voor S/4 HANA of BW/4 HANA SAP CAL](cal-s4h.md)
## [Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure](hana-vm-operations.md)
## Beschikbaarheid van SAP HANA in virtuele Azure-machines
### [Overzicht van de beschikbaarheid van SAP HANA op Azure](sap-hana-availability-overview.md)
### [Beschikbaarheid van SAP HANA op Azure binnen één Azure-regio](sap-hana-availability-one-region.md)
### [Beschikbaarheid van SAP HANA op Azure in meerdere Azure-regio's](sap-hana-availability-across-regions.md)
### [SAP HANA-Systeemreplicatie instellen op SLES](sap-hana-high-availability.md)
### [SAP HANA-Systeemreplicatie instellen op RHEL](sap-hana-high-availability-rhel.md)
### [Problemen oplossen met vergroten van SAP HANA-schaal en Pacemaker op SLES](hana-vm-troubleshoot-scale-out-ha-on-sles.md)
## [Overzicht van back-ups van SAP HANA](sap-hana-backup-guide.md)
## [Back-ups maken van SAP HANA op bestandsniveau](sap-hana-backup-file-level.md)
## [Back-ups maken van SAP HANA-opslagmomentopnamen](sap-hana-backup-storage-snapshots.md)
# SAP NetWeaver en Business One in virtuele Azure-machines
## [Workloadplanning en implementatiechecklist voor SAP](sap-deployment-checklist.md)
## [SAP NetWeaver op Azure plannen en implementeren](planning-guide.md)
## [Implementatiehandleiding voor SAP NetWeaver](deployment-guide.md)
## DBMS-implementatiehandleidingen voor SAP-werkbelastingen
### [DBMS-implementatie voor SAP-werkbelasting in algemene virtuele Azure-machines](dbms_guide_general.md)
### [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SQL Server](dbms_guide_sqlserver.md)
### [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor Oracle](dbms_guide_oracle.md)
### [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor IBM DB2](dbms_guide_ibm.md)
### [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SAP ASE](dbms_guide_sapase.md)
### [Implementatie van SAP MaxDB, liveCache en Content Server in Azure](dbms_guide_maxdb.md)
### Beschikbaarheid van SAP HANA in virtuele Azure-machines
#### [Overzicht van de beschikbaarheid van SAP HANA op Azure](sap-hana-availability-overview.md)
#### [Beschikbaarheid van SAP HANA op Azure binnen één Azure-regio](sap-hana-availability-one-region.md)
#### [Beschikbaarheid van SAP HANA op Azure in meerdere Azure-regio's](sap-hana-availability-across-regions.md)
## [SAP Business One in virtuele Azure-machines](business-one-azure.md)
## [Implementatiehandleiding voor SAP IDES op Windows/SQL Server SAP CAL](cal-ides-erp6-erp7-sp3-sql.md)
## [SAP LaMa-connector voor Azure](lama-installation.md)
## Hoge beschikbaarheid (High Availability, HA) voor Windows en Linux
### [Overzicht](sap-high-availability-guide-start.md)
### Architectuur voor hoge beschikbaarheid
#### [HA-architectuur en -scenario's](sap-high-availability-architecture-scenarios.md)
#### [Architectuur en scenario's voor hogere beschikbaarheid](sap-higher-availability-architecture-scenarios.md)
#### [Hoge beschikbaarheid in Windows met gedeelde schijven voor een (A)SCS-exemplaar](sap-high-availability-guide-wsfc-shared-disk.md)
#### [Hoge beschikbaarheid in Windows met SOFS File Share voor een (A)SCS-exemplaar](sap-high-availability-guide-wsfc-file-share.md)
#### [Hoge beschikbaarheid in SUSE Linux voor een (A)SCS-exemplaar](high-availability-guide-suse.md)
#### [Hoge beschikbaarheid in een Red Hat Enterprise Linux for (A)SCS-exemplaar](high-availability-guide-rhel.md)
### Voorbereiding van Azure-infrastructuur
#### [Windows met gedeelde schijven voor een (A)SCS-exemplaar](sap-high-availability-infrastructure-wsfc-shared-disk.md)
#### [Windows met SOFS File Share voor een (A)SCS-exemplaar](sap-high-availability-infrastructure-wsfc-file-share.md)
#### [Hoge beschikbaarheid voor NFS op Azure VM's in SLES](high-availability-guide-suse-nfs.md)
#### [GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver](high-availability-guide-rhel-glusterfs.md)
#### [Pacemaker op SLES](high-availability-guide-suse-pacemaker.md)
#### [Pacemaker op RHEL](high-availability-guide-rhel-pacemaker.md)
### SAP-installatie
#### [Windows met gedeelde schijven voor een (A)SCS-exemplaar](sap-high-availability-installation-wsfc-shared-disk.md)
#### [Windows met SOFS File Share voor een (A)SCS-exemplaar](sap-high-availability-installation-wsfc-file-share.md)
#### [SUSE Linux met NFS voor een (A)SCS-exemplaar](high-availability-guide-suse.md)
#### [Hoge beschikbaarheid voor SAP NetWeaver op Red Hat Enterprise Linux](high-availability-guide-rhel.md)
### SAP Multi-SID
#### [Windows met gedeelde schijven voor een (A)SCS-exemplaar](sap-ascs-ha-multi-sid-wsfc-shared-disk.md)
#### [Windows met SOFS File Share voor een (A)SCS-exemplaar](sap-ascs-ha-multi-sid-wsfc-file-share.md)
##  [Herstel na noodgevallen van SAP met Azure Site Recovery](../../../site-recovery/site-recovery-workload.md#protect-sap)
# AAD SAP Identity Integration en eenmalige aanmelding
## [Integratie met SAP Cloud](../../../active-directory/saas-apps/sap-customer-cloud-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [AAD-integratie met SAP Cloud Platform-identiteitsverificatie](../../../active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [Eenmalige aanmelding voor SAP Cloud Platform instellen](../../../active-directory/saas-apps/sap-hana-cloud-platform-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [AAD-integratie met SAP NetWeaver](../../../active-directory/saas-apps/sap-netweaver-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [AAD-integratie met SAP Business ByDesign](../../../active-directory/saas-apps/sapbusinessbydesign-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
## [AAD-integratie met SAP HANA DBMS](../../../active-directory/saas-apps/saphana-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
##[Eenmalige aanmelding SAP Fiori Launchpad SAML met Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad)
# Azure Services-integratie met SAP
## [SAP HANA gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-hana)
## [DirectQuery en SAP HANA](https://docs.microsoft.com/power-bi/desktop-directquery-sap-hana)
## [De SAP BW-connector gebruiken in Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-sap-bw-connector)
## [Azure Data Factory biedt integratie van SAP HANA- en Business Warehouse-gegevens](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)
# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/)

