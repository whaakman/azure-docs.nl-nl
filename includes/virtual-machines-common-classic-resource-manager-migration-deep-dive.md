## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>IaaS-bronnen migreren vanuit het klassieke implementatiemodel in Azure Resource Manager
Eerst, is het belangrijk om het verschil tussen gegevens vlak- en management vlak bewerkingen op de infrastructuur als een dienst (IaaS)-resources.

* *Controle van beheer/vlak* beschrijving van de oproepen die in het beheer/vlak of de API voor het wijzigen van resources kunnen komen. Bewerkingen zoals het maken van een VM, opnieuw starten van een VM en het bijwerken van een virtueel netwerk met een nieuw subnet beheren de actieve resources. Ze niet rechtstreeks van invloed op verbinding maken met de virtuele machines.
* *Gegevens vlak* beschrijft de runtime van de toepassing zelf (toepassing) en omvat interactie met exemplaren die niet door de Azure-API. Toegang tot uw website of binnenhalen van gegevens van een actief exemplaar van SQL Server of een MongoDB-server, zijn bijvoorbeeld gegevens interacties luchtvervoer of toepassing. Andere voorbeelden zijn een blob kopiëren van een opslagaccount en openen van een openbaar IP-adres voor het gebruik van Remote Desktop Protocol (RDP) of Secure Shell (SSH) in de virtuele machine. Deze bewerkingen zorgen ervoor dat de toepassing uitgevoerd blijft worden bij het berekenen, het netwerken en de opslag.

Het vlak van gegevens is hetzelfde tussen het klassieke implementatiemodel en stacks voor Resource Manager. Het verschil is dat tijdens het migratieproces Microsoft zet om de weergave van de bronnen van het klassieke implementatiemodel met die in de Resource Manager-stack. Als gevolg hiervan moet u nieuwe hulpprogramma's, API's en SDK's gebruiken voor het beheren van uw resources in de Resource Manager-stack.

![Diagram waarin het verschil tussen Beheercontrole/vlak en gegevens vlak](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> In sommige scenario's voor migratie stopt het Azure-platform, maakt de toewijzingen ongedaan en start uw virtuele machines opnieuw op. Dit zorgt ervoor dat een korte gegevens vlak uitvaltijd.
>

## <a name="the-migration-experience"></a>De migratie-ervaring
Voordat u de migratie start:

* Zorg ervoor dat de resources die u wilt migreren geen niet-ondersteunde functies of configuraties gebruiken. Meestal detecteert het platform deze problemen en genereert een fout.
* Als u virtuele machines die zich niet in een virtueel netwerk hebt, zijn gestopt en de toewijzing ongedaan gemaakt als onderdeel van de voorbereidingsbewerking. Als u niet dat het openbare IP-adres verliest wilt, kunt u het IP-adres reserveren voordat de voorbereidingsbewerking. Als de virtuele machines zich in een virtueel netwerk, worden ze niet gestopt en de toewijzing ongedaan gemaakt.
* Plan de migratie buiten kantooruren zodat u de tijd hebt om onverwachte fouten die kunnen ontstaan tijdens de migratie op te lossen.
* Download de huidige configuratie van uw virtuele machines met behulp van PowerShell, CLI-opdrachten (opdrachtregelinterface) of REST API's om de validatie eenvoudiger te maken nadat de voorbereidingsstap is voltooid.
* Werk uw automation en uitoefening scripts voor het afhandelen van het implementatiemodel van Resource Manager voordat u de migratie start. Desgewenst kunt u GET-bewerkingen uitvoeren wanneer de resources de status Voorbereid hebben.
* Denk na over de op rollen gebaseerde toegangsbeheer (RBAC)-beleidsregels die zijn geconfigureerd op de IaaS-resources in het klassieke implementatiemodel en plannen nadat de migratie voltooid is.

De Migratiewerkstroom is als volgt:

![Diagram waarin de Migratiewerkstroom](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> De bewerkingen die in de volgende secties beschreven zijn alle idempotent. Als er een probleem dan een niet-ondersteunde functie of een configuratiefout, probeer de voorbereiden afgebroken of doorgevoerd, bewerking. Azure probeert het opnieuw.
>
>

### <a name="validate"></a>Valideren
De bewerking Valideren is de eerste stap in het migratieproces. Het doel van deze stap is voor het analyseren van de status van de resources die u wilt migreren in het klassieke implementatiemodel. De bewerking beoordeelt of de resources geschikt voor migratie (slagen of mislukken zijn).

Selecteer het virtuele netwerk of een cloudservice (indien dit niet in een virtueel netwerk is) die u wilt valideren voor migratie. Als de bron niet geschikt is voor migratie is, vermeld Azure de redenen waarom.

#### <a name="checks-not-done-in-the-validate-operation"></a>Controleert niet uitgevoerd in de bewerking valideren

De bewerking valideren analyseert alleen de status van de resources in het klassieke implementatiemodel. Om alle fouten en niet-ondersteunde scenario's als gevolg van verschillende configuraties in het klassieke implementatiemodel u kunt controleren. Het is niet mogelijk om te controleren voor alle problemen die de Azure Resource Manager-stack aan de resources tijdens de migratie opleggen kan. Deze problemen zijn alleen gecontroleerd wanneer de resources ondergaan transformatie in de volgende stap van de migratie (de voorbereidingsbewerking). De volgende tabel bevat alle problemen die niet is geselecteerd in de bewerking valideren:


|Netwerken controleert niet in de bewerking valideren|
|-|
|Een virtueel netwerk dat ER- en VPN-gateways.|
|Een virtueel netwerk gatewayverbinding verbroken.|
|Alle ER circuits worden vooraf gemigreerd naar Azure Resource Manager-stack.|
|Azure Resource Manager-quotum controleert netwerkresources. Bijvoorbeeld: statische openbare IP-adres, dynamische openbare IP-adressen, load balancer, netwerkbeveiligingsgroepen, routetabellen en netwerkinterfaces. |
| Alle regels van de load balancer zijn geldig voor implementatie en het virtuele netwerk. |
| Conflicterende privé IPs tussen virtuele machines in hetzelfde virtuele netwerk stop-toewijzing ongedaan gemaakt. |

### <a name="prepare"></a>Voorbereiden
De bewerking Voorbereiden is de tweede stap in het migratieproces. Het doel van deze stap is om te simuleren van de transformatie van de IaaS-bronnen van het klassieke implementatiemodel voor Resource Manager-resources. Bovendien geeft de prepare-bewerking deze side-by-side kunt visualiseren.

> [!NOTE] 
> Uw resources in het klassieke implementatiemodel worden niet gewijzigd tijdens deze stap. Het is een veilige stap uit te voeren als u om de migratie probeert. 

U selecteert het virtuele netwerk of de cloudservice (indien dit niet een virtueel netwerk is) dat u wilt voorbereiden voor migratie.

* Als de bron niet geschikt is voor migratie is, wordt Azure Hiermee stopt u het migratieproces en Hiermee wordt de reden waarom de prepare-bewerking is mislukt.
* Als de bron van de migratie kan, vergrendeld Azure naar de operations management vlak voor de resources onder de migratie. U kunt bijvoorbeeld geen gegevensschijf toevoegen aan een virtuele machine die wordt gemigreerd.

Azure vervolgens de migratie start van metagegevens van het klassieke implementatiemodel naar Resource Manager voor de bronnen migreren.

Nadat de prepare-bewerking voltooid is, hebt u de mogelijkheid van de resources in het klassieke implementatiemodel en de resourcemanager te visualiseren. Voor elke cloudservice in het klassieke implementatiemodel maakt het Azure-platform een resourcegroepnaam met de indeling `cloud-service-name>-Migrated`.

> [!NOTE]
> Het is niet mogelijk is de naam van een resourcegroep gemaakt voor de gemigreerde bronnen selecteren (dat wil zeggen, '-gemigreerd '). Nadat de migratie is voltooid, maar kunt u de functie voor move van Azure Resource Manager resources verplaatsen naar een resourcegroep die u wilt. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../articles/resource-group-move-resources.md).

Het resultaat weergeven de volgende twee schermafbeeldingen nadat een geslaagd bewerking voorbereiden. Het eerste beheerpunt ziet u een resourcegroep met de oorspronkelijke cloudservice. Het tweede voorbeeld ziet u de nieuwe '-gemigreerd ' resourcegroep met de equivalente Azure Resource Manager-resources.

![Schermafbeelding van oorspronkelijke cloudservice](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Schermafbeelding van Azure Resource Manager-resources in de voorbereidingsbewerking](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Hier volgt een achter de schermen Bekijk uw resources na de voltooiing van de fase voorbereiden. Houd er rekening mee dat de resource in het vlak van gegevens hetzelfde is. Deze wordt weergegeven in het management vlak (klassieke implementatiemodel) en het besturingselement vlak (Resource Manager).

![Diagram van de fase voorbereiden](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuele machines die zich niet in een virtueel netwerk in het klassieke implementatiemodel worden gestopt en de toewijzing ongedaan gemaakt in deze fase van de migratie.
>

### <a name="check-manual-or-scripted"></a>Controleren (handmatig of gepland)
In de stap selectievakje hebt u de optie voor het gebruik van de configuratie die u eerder hebt gedownload om te verifiëren dat de migratie er goed uitziet. U kunt ook kunt u aanmelden op de portal en de controle de eigenschappen en resources te valideren dat metagegevens migratie er goed uitziet.

Als u een virtueel netwerk migreert, wordt de configuratie van virtuele machines meestal niet opnieuw opgestart. Voor toepassingen op deze virtuele machines, kunt u nagaan of er nog steeds de toepassing wordt uitgevoerd.

U kunt testen uw bewakings- en -scripts om te zien als de virtuele machines werkt zoals verwacht en als uw bijgewerkte scripts goed werkt. Alleen GET-bewerkingen worden ondersteund wanneer de resources de status Voorbereid hebben.

Er is geen set-venster van de tijd dat u wilt doorvoeren van de migratie. U kunt zoveel tijd nemen als u wilt in deze fase. Het beheervlak is echter wel vergrendeld voor deze resources totdat u de migratie afbreekt of doorvoert.

Als er problemen zijn, kunt u altijd de migratie altijd afbreken en terugkeren naar het klassieke implementatiemodel. Nadat u teruggaan Azure Hiermee opent u de management-vlak-bewerkingen op de bronnen, zodat u kunt de normale bewerkingen op deze virtuele machines in het klassieke implementatiemodel hervatten.

### <a name="abort"></a>Afbreken
Dit is een optionele stap als u wilt herstellen van uw wijzigingen aan het klassieke implementatiemodel en stoppen van de migratie. Deze bewerking worden de metagegevens van de Resource Manager (gemaakt in de voorbereidingsstap) voor uw resources verwijderd. 

![Diagram van stap afbreken](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Deze bewerking kan niet worden uitgevoerd nadat u de doorvoerbewerking hebt geactiveerd.     
>

### <a name="commit"></a>Doorvoeren
Nadat de validatie is voltooid, kunt u de migratie doorvoeren. Resources worden niet meer weergegeven in het klassieke implementatiemodel en zijn alleen beschikbaar in het Resource Manager-implementatiemodel. De gemigreerde resources kunnen alleen in de nieuwe portal worden beheerd.

> [!NOTE]
> Dit is een idempotente bewerking. Als dit mislukt, probeer het opnieuw. Als dit blijft mislukken, maak een ondersteuningsticket of maak een bericht met een 'ClassicIaaSMigration' tag op onze [VM forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Diagram van de commit-stap](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Stroomdiagram van migratie

Hier volgt een stroomdiagram dat toont hoe om door te gaan met de migratie:

![Schermafbeelding van de migratiestappen](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>De vertaling van het klassieke implementatiemodel voor Resource Manager-resources
U vindt het klassieke implementatiemodel en Resource Manager-weergaven van de resources in de volgende tabel. Andere functies en resources worden momenteel niet ondersteund.

| Klassieke weergave | Weergave van de Resource Manager | Opmerkingen |
| --- | --- | --- |
| Naam cloudservice |DNS-naam |Tijdens de migratie wordt een nieuwe resourcegroep gemaakt voor elke cloudservice met het naamgevingspatroon `<cloudservicename>-migrated`. Deze resourcegroep bevat al uw resources. De naam van de cloudservice wordt een DNS-naam die is gekoppeld aan het openbare IP-adres. |
| Virtuele machine |Virtuele machine |VM-specifieke eigenschappen worden ongewijzigd gemigreerd. Bepaalde osProfile gegevens, zoals de naam van de computer, worden niet opgeslagen in het klassieke implementatiemodel en leeg blijft na de migratie. |
| Schijfresources die zijn gekoppeld aan de VM |Impliciete schijven die zijn gekoppeld aan de VM |Schijven worden niet gemodelleerd als resources op het hoogste niveau in het Resource Manager-implementatiemodel. Ze worden gemigreerd als impliciete schijven onder de virtuele machine. Alleen de schijven die zijn gekoppeld aan een virtuele machine worden momenteel ondersteund. Resource Manager virtuele machines kunnen nu gebruikmaken van storage-accounts in het klassieke implementatiemodel, waardoor de schijven eenvoudig zonder de updates worden gemigreerd. |
| VM-extensies |VM-extensies |Alle de resource-extensies, met uitzondering van XML-extensies, worden gemigreerd vanuit het klassieke implementatiemodel. |
| Certificaten van virtuele machine |Certificaten in Azure Key Vault |Als een cloudservice servicecertificaten bevat, wordt de migratie maakt een nieuwe Azure sleutelkluis per cloudservice en de certificaten is verplaatst naar de sleutelkluis. De virtuele machines worden bijgewerkt zodat ze verwijzen naar de certificaten van de sleutelkluis. <br><br> De sleutelkluis niet verwijderen. Hierdoor kan de virtuele machine in een mislukte status. |
| WinRM-configuratie |WinRM-configuratie in osProfile |De Windows Remote Management-configuratie wordt ongewijzigd verplaatst als onderdeel van de migratie. |
| Eigenschap van beschikbaarheidsset |Resource van beschikbaarheidsset | Beschikbaarheidsset specificatie is een eigenschap van de virtuele machine in het klassieke implementatiemodel. Beschikbaarheidssets worden een resource op het hoogste niveau als onderdeel van de migratie. De volgende configuraties worden niet ondersteund: meerdere beschikbaarheidssets per cloudservice of één of meer beschikbaarheidssets samen met de virtuele machines die zich niet in een beschikbaarheidsset in een cloudservice bevinden. |
| Netwerkconfiguratie op een virtuele machine |Primaire netwerkinterface |De netwerkconfiguratie op een virtuele machine wordt weergegeven als de resource primaire netwerkinterface na de migratie. Het interne IP-adres van virtuele machines die zich niet in een virtueel netwerk bevinden, wordt gewijzigd tijdens de migratie. |
| Meerdere netwerkinterfaces in een VM |Netwerkinterfaces |Als een virtuele machine meerdere netwerkinterfaces die zijn gekoppeld heeft, wordt elke netwerkinterface een bron op het hoogste niveau als onderdeel van de migratie, samen met alle eigenschappen. |
| Eindpuntset met gelijke taakverdeling |Load balancer |In het klassieke implementatiemodel wees het platform een impliciete load balancer toe aan elke cloudservice. Tijdens de migratie wordt een nieuwe load-balancerresource gemaakt. De eindpuntset met gelijke taakverdeling wordt omgezet in load-balancerregels. |
| Inkomende NAT-regels |Inkomende NAT-regels |Invoereindpunten die zijn gedefinieerd op de virtuele machine worden tijdens de migratie geconverteerd naar binnenkomende vertaalregels voor netwerkadressen onder de load balancer. |
| VIP-adres |Openbaar IP-adres met DNS-naam |Het virtuele IP-adres verandert in een openbaar IP-adres en is gekoppeld aan de load balancer. Een virtueel IP-adres kan alleen worden gemigreerd als er een invoereindpunt aan is toegewezen. |
| Virtueel netwerk |Virtueel netwerk |Het virtuele netwerk wordt met alle eigenschappen gemigreerd naar het Resource Manager-implementatiemodel. Er wordt een nieuwe resourcegroep gemaakt met de naam `-migrated`. |
| Gereserveerde IP-adressen |Openbaar IP-adres met een statische toewijzingsmethode |Gereserveerd IP-adressen die zijn gekoppeld aan de load balancer worden gemigreerd, samen met de migratie van de cloudservice of de virtuele machine. Niet-gekoppelde gereserveerde IP-migratie wordt momenteel niet ondersteund. |
| Openbaar IP-adres per VM |Openbaar IP-adres met een dynamische toewijzingsmethode |Het openbare IP-adres dat is gekoppeld aan de virtuele machine wordt geconverteerd naar een openbare IP-adresresource. De toewijzingsmethode wordt ingesteld op statisch. |
| NSG's |NSG's |Netwerkbeveiligingsgroepen die zijn gekoppeld aan een subnet worden gekloond als onderdeel van de migratie naar het Resource Manager-implementatiemodel. De NSG in het klassieke implementatiemodel wordt niet verwijderd tijdens de migratie. De bewerkingen op het beheervlak voor de NSG worden echter geblokkeerd wanneer de migratie wordt uitgevoerd. |
| DNS-servers |DNS-servers |DNS-servers die zijn gekoppeld aan een virtueel netwerk of de virtuele machine worden gemigreerd als onderdeel van de bijbehorende resourcemigratie, samen met alle eigenschappen. |
| UDR's |UDR's |Door de gebruiker gedefinieerde routes die zijn gekoppeld aan een subnet worden gekloond als onderdeel van de migratie naar het Resource Manager-implementatiemodel. De UDR in het klassieke implementatiemodel wordt niet verwijderd tijdens de migratie. De bewerkingen op het beheervlak voor de UDR worden geblokkeerd wanneer de migratie wordt uitgevoerd. |
| De eigenschap Doorsturen via IP op de netwerkconfiguratie van een virtuele machine |De eigenschap Doorsturen via IP op de NIC |De eigenschap Doorsturen via IP op een virtuele machine wordt tijdens de migratie geconverteerd naar een eigenschap op de netwerkinterface. |
| Load balancer met meerdere IP-adressen |Load balancer met meerdere openbare IP-resources |Elke openbare IP-adres die zijn gekoppeld aan de load balancer is geconverteerd naar een openbare IP-resource en die zijn gekoppeld aan de load balancer na de migratie. |
| Interne DNS-namen op de virtuele machine |Interne DNS-namen op de NIC |Tijdens de migratie worden de interne DNS-achtervoegsels voor de virtuele machines gemigreerd naar een alleen-lezeneigenschap met de naam 'InternalDomainNameSuffix' op de NIC. Het achtervoegsel ongewijzigd blijft na de migratie en VM-resolutie moet blijven werken als voorheen. |
| Gateway van een virtueel netwerk |Gateway van een virtueel netwerk |Gateway-eigenschappen van virtueel netwerk worden ongewijzigd gemigreerd. Het VIP dat is gekoppeld aan de gateway wordt ook niet gewijzigd. |
| Lokale netwerksite |Lokale netwerkgateway |LAN site-eigenschappen worden gemigreerd naar een nieuwe resource aangeroepen van een lokale netwerkgateway ongewijzigd. Hiermee worden lokale adresvoorvoegsels en de externe gateway IP-adres. |
| Verwijzingen naar verbindingen |Verbinding |Verwijzingen van de verbinding tussen de gateway en de lokale netwerksite in netwerkconfiguratie wordt vertegenwoordigd door een nieuwe resource verbinding genoemd. Alle eigenschappen van connectiviteit verwijzing in de configuratie van netwerkbestanden zijn ongewijzigd gekopieerd naar de verbindingsbron. Connectiviteit tussen virtuele netwerken in het klassieke implementatiemodel wordt bereikt door twee IPsec-tunnels naar lokale netwerksites voor de virtuele netwerken maken. Dit wordt omgezet naar het type virtuele-netwerk-naar-virtueel-netwerkverbinding in het Resource Manager-model, zonder lokale netwerkgateways. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Wijzigingen aanbrengen in uw automatisering en hulpprogramma’s na de migratie
Als onderdeel van de migratie van uw resources van het klassieke implementatiemodel naar het Resource Manager-implementatiemodel, moet u uw bestaande automatisering of tooling om ervoor te zorgen dat het blijft werken na de migratie bijwerken.
