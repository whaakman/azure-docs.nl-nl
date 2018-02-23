

![Virtuele machines in een zelfstandige cloudservice](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Als u uw virtuele machines in een virtueel netwerk inschakelt, kunt u bepalen hoeveel cloud-services die u gebruiken wilt voor load balancing en beschikbaarheid sets. Bovendien kunt u de virtuele machines op subnetten indelen op dezelfde manier als uw on-premises netwerk en het virtueel netwerk met uw on-premises netwerk verbinden. Hier volgt een voorbeeld:

![Virtuele machines in een virtueel netwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuele netwerken zijn de aanbevolen manier om verbinding met virtuele machines in Azure. De aanbevolen procedure is voor het configureren van elke laag van uw toepassing in een afzonderlijke cloudservice. Mogelijk moet u echter een aantal virtuele machines van verschillende toepassingslagen combineren in dezelfde cloudservice blijven binnen de maximale van 200 cloud-services per abonnement. Deze en andere beperkingen die daarin Zie [Azure-abonnement en Service-limieten, quota's en beperkingen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Verbinding maken met virtuele machines in een virtueel netwerk
Sluit de virtuele machines in een virtueel netwerk:

1. Maken van het virtuele netwerk in de [Azure-portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) en 'klassieke implementatie' opgeven.
2. Maken van de set van cloud-services voor uw implementatie naar aanleiding van uw ontwerp voor beschikbaarheidssets en taakverdeling. Klik in de Azure-portal op **maken van een resource > berekenen > Cloudservice** voor elke cloudservice.

  Als u de details van de cloud service invullen, kies dezelfde _resourcegroep_ gebruikt met het virtuele netwerk.

3. Elke nieuwe virtuele machine toevoegen, klikt u op **maken van een resource > Compute**, selecteer vervolgens de installatiekopie van het juiste VM van de **aanbevolen apps**.

  In de virtuele machine **basisbeginselen** blade Kies dezelfde _resourcegroep_ gebruikt met het virtuele netwerk.

  ![Basisprincipes van VM-blade wanneer u een VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Als u de virtuele machine invullen **instellingen**, kies de juiste _Cloudservice_ of _virtueel netwerk_ voor de virtuele machine.

  Azure, wordt het andere op basis van uw selectie item geselecteerd.

  ![Blade van de VM-instellingen wanneer u een VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Verbinding maken met virtuele machines in een zelfstandige cloudservice
Sluit de virtuele machines in een zelfstandige cloudservice:

1. Maken van de cloudservice in de [Azure-portal](http://portal.azure.com). Klik op **Nieuw > berekenen > Cloudservice**. Of u kunt de cloudservice voor uw implementatie maken wanneer u uw eerste virtuele machine maken.
2. Wanneer u de virtuele machines maakt, kiest u dezelfde resourcegroep gebruikt met de cloudservice.

  ![Een virtuele machine toevoegen aan een bestaande cloudservice](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Als u de details van de VM invullen, kies de naam van de cloudservice in de eerste stap hebt gemaakt.

  ![Een cloudservice voor een virtuele machine selecteren](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
