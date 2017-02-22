

Voor toepassingen die compute-resources in en uit moeten schalen, worden schaalaanpassingen impliciet verdeeld over fout- en updatedomeinen. Raadpleeg de recentelijke [aankondiging in het Azure-blog](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) voor een introductie over VM-schaalsets.

Bekijk deze video's voor meer informatie over VM-schaalsets:

* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>VM-schaalsets maken en beheren
VM-schaalsets kunnen worden gedefinieerd en geïmplementeerd met behulp van JSON-sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx), net als individuele virtuele machines in Azure Resource Manager. Dit betekent dat alle standaardimplementatiemethoden van Azure Resource Manager kunnen worden toegepast. Zie [Azure Resource Manager-sjablonen samenstellen](../articles/resource-group-authoring-templates.md) voor meer informatie over sjablonen.

Hier in de GitHub-opslagplaats voor Azure Quickstart-sjablonen vindt u een aantal voorbeeldsjablonen voor VM-schaalsets:

[https://github.com/Azure/Azure-QuickStart-templates](https://github.com/Azure/azure-quickstart-templates). Zoek naar sjablonen met *vmss* in de titel.

Op de specifieke pagina's voor de sjablonen ziet u een knop voor portalimplementatie. Als u de VM-schaalset wilt implementeren, klikt u op de knop en vult u vervolgens de vereiste parameters in de portal in. Als u niet zeker weet of een resource hoofdletters of hoofdletters en kleine letters door elkaar ondersteunt, kunt u het beste parameterwaarden met kleine letters gebruiken. U kunt ook deze handige video bekijken waarin de sjabloon voor VM-schaalsets wordt toegelicht:

[Toelichting sjabloon voor VM-schaalset](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Een VM-schaalset in- en uitschalen
Als u het aantal virtuele machines in een VM-schaalset wilt verhogen of verlagen, wijzigt u de eigenschap *capaciteit* en implementeert u de sjabloon opnieuw. Deze gebruiksvriendelijke functie maakt het eenvoudig om uw eigen aangepaste schaallaag te schrijven als u aangepaste schaalgebeurtenissen wilt definiëren die niet door Automatisch schalen van Azure worden ondersteund.

Als u een sjabloon opnieuw wilt implementeren om de capaciteit te wijzigen, kunt u een veel kleinere sjabloon definiëren dat alleen de SKU en bijgewerkte capaciteit bevat. Een voorbeeld hiervan wordt hier weergegeven: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Raadpleeg [Automatisch machines schalen in een virtuele-machineschaalset](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor een stapsgewijze uitleg van het maken van een schaalset die automatisch wordt geschaald

## <a name="monitoring-your-vm-scale-set"></a>De VM-schaalset controleren
Het wordt momenteel aanbevolen om de [Azure Resource Explorer](https://resources.azure.com) te gebruiken om VM-schaalsets te bekijken. VM-schaalsets zijn een resource onder Microsoft.Compute. U kunt ze vanaf deze site bekijken door de volgende koppelingen uit te vouwen:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Scenario's voor VM-schaalsets
In dit gedeelte wordt een aantal typische scenario's voor VM-schaalsets genoemd. Deze scenario's worden toegepast voor een aantal van de hogere Azure-services, zoals Batch, Service Fabric en Azure Container Service.

* **RDP/SSH naar instanties van VM-schaalsets**: een VM-schaalset wordt binnen een VNet gemaakt en er worden geen openbare IP-adressen toegewezen aan individuele virtuele machines. Zo vermijdt u de kosten en overhead van het toewijzen van afzonderlijke IP-adressen aan alle staatloze resources in uw compute-raster. Bovendien kunt u gemakkelijk verbinding maken met deze virtuele machines vanuit andere resources in uw VNet, inclusief resources met openbare IP-adressen, zoals load balancers of losstaande virtuele machines.
* **Verbinding maken met VM's met behulp van NAT-regels:** u kunt een openbaar IP-adres maken, dit toewijzen aan een load balancer en binnenkomende NAT-regels definiëren die een poort van het IP-adres toewijzen aan een poort van een VM in de VM-schaalset. Bijvoorbeeld
  
   Openbaar IP-adres->Poort 50000-> vmss\_0->Poort 22 Openbaar IP-adres->Poort 50001 -> vmss\_1->Poort 22 Openbaar IP-adres->Poort 50002-> vmss\_2->Poort 22
  
   Hier volgt een voorbeeld van het maken van een VM-schaalset waarbij NAT-regels zijn gebruikt om een SSH-verbinding met elke VM in een schaalset mogelijk te maken via een enkel openbaar IP-adres: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Hier volgt een voorbeeld van hoe u hetzelfde doet met RDP en Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Verbinding maken met VM's met behulp van een 'jumpbox'**: als u een VM-schaalset en losstaande VM in hetzelfde VNET maakt, kunnen de losse VM en de VM's in de VM-schaalset met elkaar verbinding maken via de interne IP-adressen zoals gedefinieerd in het VNET/Subnet. As u een openbaar IP-adres maakt en dit toewijst aan de losstaande VM, kunt u via RDP of SSH verbinding maken met de losse VM en vervolgens vanaf die machine verbinding maken met de instanties van uw VM-schaalset. Het valt u misschien op dat een eenvoudige VM-schaalset inherent veiliger is dan een eenvoudige losse VM met een openbaar IP-adres in de standaardconfiguratie.
  
   Deze sjabloon geeft een voorbeeld van deze benadering met het maken van een eenvoudig Mesos-cluster dat bestaat uit een losse hoofd-VM die een cluster van virtuele machines gebaseerd op een VM-schaalset aanstuurt: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)
* **Round-robintaakverdeling voor instanties van een VM-schaalset:** als u middels een round robin-benadering taken aan een rekencluster van virtuele machines wilt toewijzen, kunt u een Azure Load Balancer configureren met toepasselijke taakverdelingsregels. U kunt ook tests definiëren om te verifiëren of uw toepassing wordt uitgevoerd, door poorten te pingen met een opgegeven protocol, interval en aanvraagpad.
  
   Hier volgt een voorbeeld van een VM-schaalset met virtuele machines die worden uitgevoerd op een IIS-webserver. Hierbij wordt gebruikgemaakt van een load balancer om de taken te verdelen die elke virtuele machine ontvangt. Ook wordt het HTTP-protocol gebruikt om een specifieke URL op elke virtuele machine te pingen: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json). Zoek het brontype Microsoft.Network/loadBalancers en networkProfile en extensionProfile in de virtualMachineScaleSet.
* **Een VM-schaalset implementeren als een rekencluster in PaaS-clusterbeheer**: VM-schaalsets worden soms omschreven als de werkrol van de volgende generatie. Dit is een accurate beschrijving, maar deze kan er wel voor zorgen dat schaalsetfuncties worden verward met PaaS v1-werkrolfuncties. VM-schaalsets bieden een echte werkrol of werkrol-resource in dat opzicht dat ze een algemene compute-resource bieden die losstaat van platform/runtime, aanpasbaar is en integreert met Azure Resource Manager IaaS.
  
   Een PaaS v1-werkrol is beperkt in het kader van ondersteuning voor platform/runtime (alleen Windows-platforminstallatiekopieën), maar omvat wel services als wisselen van VIP, configureerbare upgrade-instellingen en specifieke instellingen voor runtime/toepassingsimplementatie die of *nog* niet beschikbaar zijn in VM-schaalsets of worden geleverd voor andere, hogere PaaS-services zoals Service Fabric. Dit betekent dat u VM-schaalsets kunt beschouwen als infrastructuur die PaaS ondersteunt. Dit betekent dat PaaS-oplossingen zoals Service Fabric of clusterbeheer zoals Mesos op VM-schaalsets kunnen bouwen als een schaalbare rekenlaag.
  
   Hier volgt een voorbeeld van een Mesos-cluster dat een VM-schaalset implementeert in hetzelfde VNet als een zelfstandige virtuele machine. De zelfstandige virtuele machine is een Mesos-model en de VM-schaalset vertegenwoordigt een set knooppunten op een lager niveau: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Toekomstige versies van de [Azure Container Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) zullen complexere/sterkere versies van dit scenario implementeren op basis van VM-schaalsets.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Richtlijnen voor prestaties en schaal van VM-schaalsets
* Maak tijdens de openbare proefperiode niet meer dan 500 virtuele machines in verschillende VM-schaalsets tegelijkertijd.
* Plan niet meer dan 40 virtuele machines per opslagaccount.
* Spreid de eerste letters van de opslagaccountnamen zo veel mogelijk.  De VMSS-sjablonen in [Azure-snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates/) bieden voorbeelden van hoe u dit doet.
* Plan niet meer dan 40 VM's in per VM-schaalset voor een enkel opslagaccount als u aangepaste VM's gebruikt.  De installatiekopie dient vooraf naar het opslagaccount gekopieerd te zijn voordat u de VM-schaalset kunt implementeren. Bekijk de veelgestelde vragen voor meer informatie.
* Plan niet meer dan 2048 virtuele machines per VNet.  Deze limiet wordt in de toekomst verhoogd.
* Het aantal virtuele machines dat u kunt maken, wordt beperkt door het Compute-quotum van uw kerngeheugen in elke regio. Mogelijk moet u contact opnemen met de klantenservice om uw limiet voor rekenquota te verhogen, zelfs als u nu een hoge limiet voor het aantal cores voor gebruik met cloudservices of IaaS v1 hebt. Als u uw quota wilt opvragen, kunt u de Azure CLI-opdracht *azure vm list-usage* en de PowerShell-opdracht *Get-AzureRmVMUsage* uitvoeren (als u een versie van PowerShell lager dan 1.0 gebruikt, gebruik dan *Get-AzureVMUsage*).

## <a name="vm-scale-set-frequently-asked-questions"></a>Veelgestelde vragen over VM-schaalsets
**V:** Hoeveel VM's kan een VM-schaalset bevatten?

**A:** 100 als u platforminstallatiekopieën gebruikt die over meerdere opslagaccounts kunnen worden verspreid. Als u aangepaste installatiekopieën gebruikt, is dit maximaal 40, omdat de aangepaste installatiekopieën tijdens de proefperiode zijn beperkt tot één opslagaccount.

**V** Gelden er nog andere resourcelimieten voor VM-schaalsets?

**A:** U kunt binnen de proefperiode niet meer dan 500 virtuele machines per regio maken voor meerdere schaalsets. De bestaande [Servicebeperkingen van het Azure-abonnement](../articles/azure-subscription-service-limits.md) zijn van toepassing.

**V:** Worden gegevensschijven binnen VM-schaalsets ondersteund?

**A:** Niet in de eerste release. Uw opties voor het opslaan van gegevens zijn:

* Azure-bestanden (gedeelde SMB-stations)
* Station van het besturingssysteem
* Tijdelijk station (lokaal, niet ondersteund door Azure Storage)
* Externe gegevensservice (bijvoorbeeld externe databases, Azure-tabellen, Azure-blobs)

**V:** Welke Azure-regio's ondersteunen VM-schaalsets?

**A:** Elke regio die Azure Resource Manager ondersteunt, ondersteunt VM-schaalsets.

**V:** Hoe maak ik een VM-schaalset met behulp van een aangepaste installatiekopie?

**A:** Laat de eigenschap vhdContainers leeg, bijvoorbeeld:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**V:** Als ik de capaciteit van mijn schaalset verlaag van 20 naar 15, welke VM's worden er dan verwijderd?

**A:** Virtuele machines worden gelijkmatig uit upgradedomeinen en foutdomeinen van de schaalset verwijderd om de beschikbaarheid te maximaliseren.

**V:** Wat gebeurt er dan als ik de capaciteit verhoog van 15 naar 18?

**A:** Als u de capaciteit verhoogt tot 18, worden de virtuele machines met index 15, 16 en 17 gemaakt. In beide gevallen worden de virtuele machines verdeeld over upgrade- en foutdomeinen.

**V:** Kan ik een uitvoeringsvolgorde toepassen wanneer ik meerdere extensies in een VM-schaalset gebruik?

**A:** Niet rechtstreeks, maar voor de uitbreiding customScript kan uw script wachten tot een andere extensie is voltooid (bijvoorbeeld door bewaking van het extensielogboek; zie [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**V:** Maken VM-schaalsets gebruik van beschikbaarheidssets van Azure?

**A:** Ja. Een VM-schaalset is een impliciete beschikbaarheidsset met drie foutdomeinen en vijf upgradedomeinen. U hoeft onder virtualMachineProfile niets te configureren. In toekomstige releases gaan VM-schaalsets waarschijnlijk meerdere tenants omvatten, maar een schaalset is momenteel een enkele beschikbaarheidsset.



<!--HONumber=Jan17_HO5-->


