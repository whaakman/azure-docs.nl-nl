
Als uw Azure probleem niet wordt besproken in dit artikel, gaat u naar de [Azure-forums op MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken @AzureSupport op Twitter. U kunt ook een ondersteuning van Azure-aanvraag indienen door te selecteren **ondersteuning krijgen** op de [ondersteuning van Azure](https://azure.microsoft.com/support/options/) site.

## <a name="general-troubleshooting-steps"></a>Algemene stappen voor probleemoplossing
### <a name="troubleshoot-common-allocation-failures-in-the-classic-deployment-model"></a>Algemene toewijzingsfouten in het klassieke implementatiemodel
Deze stappen oplossen veel toewijzingsfouten in virtuele machines:

* De grootte van de virtuele machine naar een andere VM-grootte.<br>
    Klik op **door alle Bladeren** > **virtuele machines (klassiek)** > uw virtuele machine > **instellingen** > **grootte**. Zie voor gedetailleerde stappen [vergroten of verkleinen van de virtuele machine](https://msdn.microsoft.com/library/dn168976.aspx).
* Alle virtuele machines van de cloudservice verwijderen en opnieuw maken van virtuele machines.<br>
    Klik op **door alle Bladeren** > **virtuele machines (klassiek)** > uw virtuele machine > **verwijderen**. Klik vervolgens op **maken van een resource** > **Compute** > [installatiekopie van virtuele machine].

### <a name="troubleshoot-common-allocation-failures-in-the-azure-resource-manager-deployment-model"></a>Algemene toewijzingsfouten in het Azure Resource Manager-implementatiemodel
Deze stappen oplossen veel toewijzingsfouten in virtuele machines:

* Gestopt (toewijzing ongedaan maken) alle VM's in de dezelfde beschikbaarheid instellen, start vervolgens elke service.<br>
    Om te stoppen: klik op **resourcegroepen** > uw resourcegroep > **Resources** > uw beschikbaarheidsset > **virtuele Machines** > uw virtuele machine >  **Stop**.
  
    Nadat alle virtuele machines stoppen, selecteert u de eerste virtuele machine en klik op **Start**.

## <a name="background-information"></a>Achtergrondinformatie
### <a name="how-allocation-works"></a>De werking van toewijzing
De servers in Azure-datacenters worden in clusters gepartitioneerd. Normaal gesproken wordt er een toewijzingsaanvraag in meerdere clusters uitgevoerd, maar het is mogelijk dat bepaalde beperkingen van de toewijzingsaanvraag het Azure-platform ertoe dwingen de aanvraag in slechts één cluster uit te voeren. In dit artikel verwijzen we naar dit zoals "vastgemaakt aan een cluster." Afbeelding 1 hieronder ziet u dat het geval van een normale toewijzing waartoe in meerdere clusters. Diagram 2 ziet u dat het geval van een toewijzing die is vastgemaakt aan Cluster 2 omdat waar de bestaande Cloud Service CS_1 of beschikbaarheid set wordt gehost.
![Diagram van toewijzing](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Waarom toewijzingsfouten gebeuren
Wanneer een aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, is er een hogere kans niet te vinden van resources vrij, omdat de groep beschikbare resource kleiner is. Bovendien als uw aanvraag voor geheugentoewijzing is vastgemaakt aan een cluster, maar het type resource dat u hebt aangevraagd wordt niet ondersteund door dit cluster, zal uw aanvraag mislukken zelfs als het cluster resources vrij heeft. Diagram 3 hieronder ziet u het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate geen gratis resources. Diagram 4 ziet u het geval waarbij een vastgezette toewijzing is mislukt omdat het cluster alleen candidate biedt geen ondersteuning voor de aangevraagde VM-grootte, zelfs als het cluster resources vrij heeft.

![Vastgemaakte geheugentoewijzing is mislukt](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Gedetailleerde stappen specifieke toewijzing in scenario's fouten in het klassieke implementatiemodel oplossen
Hier vindt u algemene scenario's toewijzing die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt. We je Duik in elk scenario verderop in dit artikel.

* Wijzig het formaat van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
* Gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
* Volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
* Fasering/productie-implementaties (platform als een service alleen)
* Affiniteitsgroep (VM-/ service nabijheid)
* Virtueel netwerk op basis van affiniteit-groep

Wanneer u een toewijzingsfout ontvangt, kunt u zien als een van de scenario's beschreven op fout in uw toepassing. Gebruik de toewijzingsfout geretourneerd door de Azure-platform voor het identificeren van het betreffende scenario. Als uw aanvraag is vastgemaakt, verwijder enkele van de vastmaken beperkingen uw aanvraag om meer clusters, waardoor de kans van slagen van de toewijzing te openen.

In het algemeen, zolang de fout betekent niet 'de aangevraagde VM-grootte wordt niet ondersteund', u kunt altijd opnieuw uitvoeren op een later tijdstip, zoals onvoldoende bronnen is in het cluster vrijgemaakt mogelijk aan uw aanvraag. Als het probleem is dat de aangevraagde VM-grootte niet wordt ondersteund, kunt u een andere VM-grootte. Anders is de enige optie als de vastmaken beperking wilt verwijderen.

Twee algemene scenario's met fouten gerelateerd aan affiniteitsgroepen. In het verleden een affiniteitsgroep is gebruikt om te bieden dicht bij de virtuele machines/service-exemplaren of voor het maken van een virtueel netwerk is gebruikt. Dankzij de introductie van regionale virtuele netwerken, affiniteitsgroepen niet langer zijn vereist voor het maken van een virtueel netwerk. Met de vermindering van de netwerklatentie in Azure-infrastructuur, is de aanbeveling om de affiniteit tussen groepen gebruiken voor het VM-/ service nabijheid gewijzigd.

Diagram 5 hieronder toont de taxonomie van de toewijzing van (vastgemaakt)-scenario's.
![Vastgezette toewijzing taxonomie](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> De volgende fout weergegeven in elk scenario van toewijzing is een korte vorm. Raadpleeg de [fout tekenreeks lookup](#Error string lookup) voor gedetailleerde foutberichten.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Scenario van toewijzing: Wijzig het formaat van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
**Error**

Upgrade_VMSizeNotSupported of GeneralError

**Oorzaak van het cluster vastmaken**

Een aanvraag om de grootte van een virtuele machine of een virtuele machine of een rolinstantie toevoegen aan een bestaande cloudservice moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Een nieuwe cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of resources vrij heeft.

**Tijdelijke oplossing**

Als de fout is Upgrade_VMSizeNotSupported *, kunt u een andere VM-grootte. Als met behulp van een andere VM-grootte kan niet worden gebruikt, maar als het is acceptabel gebruik van een ander virtueel IP-adres (VIP), maakt u een nieuwe cloudservice voor het hosten van de nieuwe virtuele machine en de nieuwe cloudservice toevoegen aan het regionale virtuele netwerk waarin de bestaande virtuele machines worden uitgevoerd. Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, kunt u nog steeds een nieuw virtueel netwerk voor de nieuwe cloudservice maken en maken vervolgens verbinding met uw [bestaand virtueel netwerk met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Als de fout is GeneralError *, is het waarschijnlijk dat het type resource (zoals een bepaalde VM-grootte) wordt ondersteund door het cluster, maar het cluster geen gratis resources op het moment dat heeft. Vergelijkbaar met het bovenstaande scenario, de gewenste compute resource bij het maken van een nieuwe cloudservice (Let erop dat de nieuwe cloudservice heeft tot het gebruik van een andere VIP) toevoegen en gebruiken van een regionaal virtueel netwerk verbinding maken uw cloudservices verkregen.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scenario van toewijzing: gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
**Error**

GeneralError *

**Oorzaak van het cluster vastmaken**

Gedeeltelijke toewijzing is opgeheven betekent dat u (toewijzing ongedaan gemaakt) een of meer, maar niet alle virtuele machines in een cloudservice gestopt. Wanneer u stopt (ongedaan gemaakt) een virtuele machine, de bijbehorende bronnen worden vrijgegeven. Opnieuw starten die gestopt (toewijzing ongedaan gemaakt) VM is daarom een nieuwe aanvraag voor geheugentoewijzing. Opnieuw opstarten van virtuele machines in een gedeeltelijk toewijzing ongedaan is gemaakt van de cloudservice is gelijk aan het toevoegen van virtuele machines aan een bestaande cloudservice. De aanvraag voor geheugentoewijzing moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande cloudservice. Een andere cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of gratis resource heeft.

**Tijdelijke oplossing**

Als het aanvaardbaar is voor gebruik van een andere VIP, verwijderen van de gestopt (toewijzing ongedaan gemaakt) virtuele machines (maar de gekoppelde schijven behouden) en de virtuele machines opnieuw door een andere cloudservice toevoegen. Gebruik een regionaal virtueel netwerk voor de verbinding met uw cloudservices:

* Als uw bestaande cloudservice een regionaal virtueel netwerk gebruikt, moet u gewoon de nieuwe cloudservice toevoegen aan hetzelfde virtuele netwerk.
* Als uw bestaande cloudservice niet voor een regionaal virtueel netwerk gebruikt wordt, een nieuw virtueel netwerk voor de nieuwe cloudservice maken en vervolgens [uw bestaande virtuele netwerk verbinden met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Scenario van toewijzing: volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
**Error**

GeneralError *

**Oorzaak van het cluster vastmaken**

Volledige toewijzing is opgeheven betekent dat u gestopt (toewijzing opgeheven) alle VM's van een cloudservice. De toewijzingsaanvragen voor deze virtuele machines opnieuw moeten worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Een nieuwe cloudservice maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of resources vrij heeft.

**Tijdelijke oplossing**

Als deze aanvaardbaar is voor gebruik van een andere VIP, verwijderen van de oorspronkelijke gestopt (toewijzing ongedaan gemaakt) virtuele machines (maar houd het bijbehorende schijven) en verwijder de bijbehorende cloudservice (de bijbehorende rekenresources zijn al is vrijgegeven wanneer u gestopt (toewijzing ongedaan gemaakt) de virtuele machines). Maak een nieuwe cloudservice voor het toevoegen van de virtuele machines terug.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Scenario van toewijzing: tijdelijke/productie-implementaties (platform als een service alleen)
**Error**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

De implementatie van fasering en productie-implementatie van een cloudservice worden gehost in hetzelfde cluster. Wanneer u de tweede implementatie toevoegt, wordt de bijbehorende toewijzingsaanvraag geprobeerd in hetzelfde cluster die als host fungeert voor de eerste implementatie.

**Tijdelijke oplossing**

Verwijder de eerste implementatie en de oorspronkelijke cloudservice en implementeer de cloudservice opnieuw. Deze actie kan de eerste implementatie terechtkomen in een cluster met onvoldoende resources vrij voor beide implementaties of in een cluster die ondersteuning biedt voor de VM-grootten die u hebt aangevraagd.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Scenario van toewijzing: affiniteitsgroep (VM-/ service nabijheid)
**Error**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

Een compute resource toegewezen aan een affiniteitsgroep is gekoppeld aan één cluster. Nieuwe compute resource aanvragen in de betreffende affiniteitsgroep bevinden worden geprobeerd in hetzelfde cluster waar de bestaande bronnen worden gehost. Dit geldt of de nieuwe resources zijn gemaakt via een nieuwe cloudservice of een bestaande cloudservice.

**Tijdelijke oplossing**

Als een affiniteitsgroep niet nodig is, gebruikt u een affiniteitsgroep of uw rekenresources te groeperen in meerdere affiniteitsgroepen.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Scenario van toewijzing: virtueel netwerk op basis van affiniteit-groep
**Error**

New_General * of New_VMSizeNotSupported *

**Oorzaak van het cluster vastmaken**

Voordat de regionale virtuele netwerken zijn geïntroduceerd, moest u een virtueel netwerk koppelen aan een affiniteitsgroep. Als gevolg hiervan compute resources in een affiniteitsgroep geplaatst zijn gebonden aan de dezelfde beperkingen, zoals beschreven in de ' toewijzing scenario: affiniteitsgroep (VM-/ service nabijheid) ' hierboven. De rekenresources zijn gekoppeld aan één cluster.

**Tijdelijke oplossing**

Als u niet in een affiniteitsgroep hoeft, maakt u een nieuw regionaal virtueel netwerk voor de nieuwe resources die u toevoegen wilt, en vervolgens [uw bestaande virtuele netwerk verbinden met de nieuwe virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

U kunt ook [uw virtueel netwerk voor op basis van affiniteit-groep migreren naar een regionaal virtueel netwerk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en voeg vervolgens de gewenste bronnen opnieuw toe.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-the-azure-resource-manager-deployment-model"></a>Gedetailleerde probleemoplossing stappen specifieke toewijzing in scenario's fouten in het Azure Resource Manager-implementatiemodel
Hier vindt u algemene scenario's toewijzing die ertoe leiden dat een aanvraag voor geheugentoewijzing om te worden vastgemaakt. We je Duik in elk scenario verderop in dit artikel.

* Wijzig het formaat van een virtuele machine of virtuele machines of rolinstanties toevoegen aan een bestaande cloudservice
* Gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
* Volledig gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten

Wanneer u een toewijzingsfout ontvangt, kunt u zien als een van de scenario's beschreven op fout in uw toepassing. Gebruik de toewijzingsfout geretourneerd door de Azure-platform voor het identificeren van het betreffende scenario. Als uw aanvraag is vastgemaakt aan een bestaand cluster, verwijder enkele van de vastmaken beperkingen uw aanvraag om meer clusters, waardoor de kans van slagen van de toewijzing te openen.

In het algemeen, zolang de fout betekent niet 'de aangevraagde VM-grootte wordt niet ondersteund', u kunt altijd opnieuw uitvoeren op een later tijdstip, zoals onvoldoende bronnen is in het cluster vrijgemaakt mogelijk aan uw aanvraag. Als het probleem is dat de aangevraagde VM-grootte niet wordt ondersteund, Zie hieronder voor tijdelijke oplossingen.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Scenario van toewijzing: Wijzig het formaat van een virtuele machine of virtuele machines toevoegen aan een bestaande beschikbaarheidsset
**Error**

Upgrade_VMSizeNotSupported * of GeneralError *

**Oorzaak van het cluster vastmaken**

Een aanvraag om de grootte van een virtuele machine of een virtuele machine toevoegen aan een bestaande beschikbaarheidsset moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande beschikbaarheidsset. Een nieuwe beschikbaarheidsset maakt, wordt de Azure-platform om te zoeken naar een ander cluster die ondersteuning biedt voor de VM-grootte die u hebt aangevraagd of resources vrij heeft.

**Tijdelijke oplossing**

Als de fout is Upgrade_VMSizeNotSupported *, kunt u een andere VM-grootte. Als u met behulp van een andere VM-grootte kan niet worden gebruikt, stopt u alle virtuele machines in de beschikbaarheidsset. Vervolgens kunt u de grootte van de virtuele machine die de virtuele machine naar een cluster die ondersteuning biedt voor de gewenste VM-grootte worden toegewezen.

Als de fout is GeneralError *, is het waarschijnlijk dat het type resource (zoals een bepaalde VM-grootte) wordt ondersteund door het cluster, maar het cluster geen gratis resources op het moment dat heeft. Als de virtuele machine deel van een andere beschikbaarheidsset uitmaken kan, maakt u een nieuwe virtuele machine in een andere beschikbaarheidsset (in dezelfde regio). Deze nieuwe virtuele machine kan vervolgens worden toegevoegd aan hetzelfde virtuele netwerk.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scenario van toewijzing: gedeeltelijk gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten
**Error**

GeneralError *

**Oorzaak van het cluster vastmaken**

Gedeeltelijke toewijzing is opgeheven betekent dat u (toewijzing ongedaan gemaakt) een of meer gestopt, maar niet alle virtuele machines in een beschikbaarheidsset ingesteld. Wanneer u stopt (ongedaan gemaakt) een virtuele machine, de bijbehorende bronnen worden vrijgegeven. Opnieuw starten die gestopt (toewijzing ongedaan gemaakt) VM is daarom een nieuwe aanvraag voor geheugentoewijzing. Opnieuw opstarten van virtuele machines in een gedeeltelijk toewijzing ongedaan is gemaakt beschikbaarheidsset is gelijk aan het toevoegen van virtuele machines aan een bestaande beschikbaarheidsset. De aanvraag voor geheugentoewijzing moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de bestaande beschikbaarheidsset.

**Tijdelijke oplossing**

Stop alle virtuele machines in de beschikbaarheidsset voordat de eerste die opnieuw wordt opgestart. Dit zorgt ervoor dat er een nieuwe poging van de toewijzing wordt uitgevoerd en dat een nieuw cluster kan worden geselecteerd met de beschikbare capaciteit.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Scenario van toewijzing: opnieuw opstarten volledig gestopt (toewijzing ongedaan gemaakt)
**Error**

GeneralError *

**Oorzaak van het cluster vastmaken**

Volledige toewijzing is opgeheven betekent dat u gestopt (toewijzing opgeheven) alle VM's in een beschikbaarheidsset. De aanvraag voor geheugentoewijzing opnieuw opstarten van deze virtuele machines heeft betrekking op alle clusters die ondersteuning bieden voor de gewenste grootte.

**Tijdelijke oplossing**

Selecteer een nieuwe VM-grootte toe te wijzen. Probeer het later opnieuw als dit niet werkt.

<a name="Error string lookup"></a>

## <a name="error-string-lookup"></a>Fout bij het opzoeken tekenreeks
**New_VMSizeNotSupported***

'De VM grootte (of combinatie van VM-formaten) vereist voor deze implementatie kan niet worden ingericht als gevolg van implementatie aanvraag beperkingen. Indien mogelijk, probeer doet beperkingen, zoals de bindingen van virtueel netwerk, implementeren op een gehoste service met geen andere implementatie in het en naar een andere affiniteitsgroep of niets affiniteitsgroep of implementeren op een andere regio."

**New_General***

'Toewijzing is mislukt; kan niet voldoen aan de beperkingen in de aanvraag. De aangevraagde nieuwe service-implementatie is gebonden aan een affiniteitsgroep of er een bestaande implementatie onder deze gehoste service is gericht op een virtueel netwerk. Deze omstandigheden Hiermee beperkt u de nieuwe implementatie voor specifieke Azure-resources. Probeer het later opnieuw of verklein de VM-grootte of het aantal rolinstanties. U kunt ook indien mogelijk, verwijder de hiervoor genoemde beperkingen of probeer te implementeren naar een andere regio.'

**Upgrade_VMSizeNotSupported***

'Kan geen upgrade uitvoeren van de implementatie. De aangevraagde VM-grootte XXX mogelijk niet beschikbaar in de resources die de bestaande implementatie ondersteunen. Probeer het later opnieuw, probeer met een andere VM-grootte of met minder rolinstanties, of maak een implementatie onder een lege gehoste service met een nieuwe affiniteitsgroep of niets affiniteitsgroep binding."

**GeneralError***

'De server heeft een interne fout. Probeer de aanvraag." Of "Voor het produceren van een toewijzing voor de service mislukt."

