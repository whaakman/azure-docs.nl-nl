# Overzicht
## [Informatie over virtuele machines](../../virtual-machines-windows-about.md)
## [Schijven en VHD's](../../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
## [Virtuele netwerken](../../../virtual-network/virtual-networks-overview.md)
## [Veelgestelde vragen](faq.md)
## [VM's, websites en cloudservices vergelijken](../../../app-service-web/choose-web-site-cloud-service-vm.md)
## [Containers](../../virtual-machines-windows-containers.md)

# Aan de slag
## [Een virtuele machine maken via de portal](tutorial.md)
## [Aanmelden bij een VM](connect-logon.md)
## [Azure PowerShell installeren](/powershell/azureps-cmdlets-docs)
## [Azure CLI installeren](../../../cli-install-nodejs.md)

# Procedures

## Storage gebruiken
### [Een gegevensschijf koppelen](attach-disk.md)
### [Een gegevensschijf ontkoppelen](detach-disk.md)
### [D: als gegevensschijf gebruiken](../../virtual-machines-windows-change-drive-letter.md)

## Netwerk
### [Eindpunten instellen](setup-endpoints.md)
### [VM’s verbinden met een VNet of cloudservice](connect-vms.md)
### [Klassieke en Resource Manager VNets verbinden](../../../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
### [Een load balancer maken](../../../load-balancer/load-balancer-get-started-internet-classic-portal.md)
### [NSG’s beheren met Azure PowerShell](../../../virtual-network/virtual-networks-create-nsg-classic-ps.md)

## Implementeren
### [Een aangepaste VM maken](createportal.md)
### [Een VM maken en configureren met Azure PowerShell](create-powershell.md)
### [Een Windows-VM vastleggen](capture-image.md)
### [Een VHD maken en uploaden met PowerShell](createupload-vhd.md)
### [Implementatie van Azure-VM automatiseren met Chef](../../virtual-machines-windows-chef-automation.md)
### [VM’s maken en beheren in Visual Studio](manage-visual-studio.md)
### [Een VM voor een web-app maken met Visual Studio](web-app-visual-studio.md)
### [Een rekenintensieve taak uitvoeren in Java](java-run-compute-intensive-task.md)
### [Django Hello World-webtoepassing](python-django-web-app.md)

## Configureren
### [Een wachtwoord of de Extern bureaublad-service opnieuw instellen](../../virtual-machines-windows-reset-rdp.md)
### [Symantec Endpoint Protection installeren en configureren](install-symantec.md)
### [Trend Micro Deep Security installeren en configureren als een service](install-trend.md)
### [Een beschikbaarheidsset configureren](configure-availability.md)
### [Het formaat van een virtuele machine wijzigen die is gemaakt in het klassieke implementatiemodel](resize-vm.md)

## Beheren
### [Migreren van klassiek naar Resource Manager](../../virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
### [Uw VM’s beheren met Azure PowerShell](manage-psh.md)
### [Informatie over de VM-agent en -extensies](agents-and-extensions.md)
### [VM-extensies beheren](manage-extensions.md)
### [Aangepaste scriptextensie voor VM’s](extensions-customscript.md)
### [Aangepaste gegevens injecteren in een Azure-VM](inject-custom-data.md)

## Plannen
### [Informatie over installatiekopieën](about-images.md)
### [Grootte van VM's](../../virtual-machines-windows-sizes.md)
#### [Over VM's uit de H-serie en rekenintensieve A-serie](../../virtual-machines-windows-a8-a9-a10-a11-specs.md)
### [Gepland onderhoud van Azure VM's](../../virtual-machines-windows-planned-maintenance.md)
### [Implementatierichtlijnen voor Azure-infrastructuurservices](../../virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md)

## Workloads beheren
### [HPC (High Performance Computing)](../../virtual-machines-windows-hpcpack-cluster-options.md)
#### [Automatisch resources schalen](hpcpack-cluster-node-autogrowshrink.md)
#### [Rekenknooppunten beheren](hpcpack-cluster-node-manage.md)
#### [Een cluster maken](hpcpack-cluster-powershell-script.md)
#### [Een cluster voor het uitvoeren van MPI-toepassingen instellen](hpcpack-rdma-cluster.md)
#### [Excel- en SOA-workloads uitvoeren](../../virtual-machines-windows-excel-cluster-hpcpack.md)
#### [Het hoofdknooppunt maken met een Marketplace-installatiekopie](../../virtual-machines-windows-hpcpack-cluster-headnode.md)
#### [Taken van on-premises naar Azure verzenden](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md)
### [MongoDB](install-mongodb.md)
### [MySQL](mysql-2008r2.md)
### [Oracle](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html#support)
####[Oracle Data Guard voor Azure configureren](configure-oracle-data-guard.md)
####[Oracle GoldenGate voor Azure configureren](configure-oracle-goldengate.md)
####[Overige overwegingen voor installatiekopieën](oracle-considerations.md)
####[Lijst met Oracle-VM-installatiekopieën](oracle-images.md)
### [SAP](sap-get-started.md)
### [SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
### [Tomcat](java-run-tomcat-app-server.md)

## Problemen oplossen
### [Verbindingen met extern bureaublad](../../virtual-machines-windows-troubleshoot-rdp-connection.md)
####[Gedetailleerde stappen voor het oplossen van problemen met Extern bureaublad](../../virtual-machines-windows-detailed-troubleshoot-rdp.md)
### [Toegang tot een toepassing](../../virtual-machines-windows-troubleshoot-app-connection.md)
### [Problemen met de klassieke implementatie bij het maken van een nieuwe VM](troubleshoot-deployment-new-vm.md)
### [Problemen met de klassieke implementatie bij het opnieuw opstarten of wijzigen van het formaat van een bestaande VM](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
## [RDP-wachtwoord opnieuw instellen](reset-rdp.md)

# Naslaginformatie
## [PowerShell](/powershell/azureps-cmdlets-docs)
## [Azure-CLI](/cli/azure/vm)
## [Java](/java/api)
## [.NET](/dotnet/api/microsoft.azure.management.compute)
## [Resource Manager-sjablonen schrijven](../../../resource-group-authoring-templates.md)
## [Communitysjablonen](https://azure.microsoft.com/documentation/templates)
## [Compute REST](/rest/api/compute)
## [Network REST](/rest/api)
## [Storage REST](/rest/api/storageservices)

# Bronnen
## [Prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)
## [Regionale beschikbaarheid](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-machine)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=virtual-machines)
