## <a name="scenario"></a>Scenario
Dit document begeleidt bij een implementatie met meerdere NIC's in virtuele machines in een specifiek scenario. In dit scenario hebt u een IaaS twee lagen werkbelasting gehost in Azure. Elke laag wordt geïmplementeerd in een eigen subnet in een virtueel netwerk (VNet). De front-endlaag bestaat uit verschillende webservers, gegroepeerd in een load balancer ingesteld voor hoge beschikbaarheid. De back-end-laag bestaat uit meerdere databaseservers. Deze databaseservers worden geïmplementeerd met twee NIC's, één voor toegang tot de database, de andere voor beheer. Het scenario omvat ook Netwerkbeveiligingsgroepen (nsg's) om te bepalen welk verkeer is toegestaan voor elk subnet en NIC in de implementatie. De onderstaande afbeelding ziet u de basisarchitectuur van dit scenario.  

![MultiNIC scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

