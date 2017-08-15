## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Betekenis van de migratie van IaaS-resources van het klassieke implementatiemodel naar Resource Manager
Voordat we op de details inzoomen, bekijken we het verschil tussen bewerkingen van de IaaS-resources op het gegevensvlak en op het beheervlak.

* Het *beheer-/controlevlak* beschrijft de aanroepen die in het beheer-/controlevlak of de API worden gedaan om resources te wijzigen. Bewerkingen zoals het maken van een VM, opnieuw starten van een VM en het bijwerken van een virtueel netwerk met een nieuw subnet beheren de actieve resources. Ze niet rechtstreeks van invloed op de verbinding met de exemplaren.
* Het *gegevensvlak* (toepassing) beschrijft de runtime van de toepassing zelf en heeft te maken met de interactie met exemplaren die niet door de Azure API hoeven. Toegang verkrijgen tot uw website of het binnenhalen van gegevens vanaf een actief SQL Server-exemplaar of een MongoDB-server kan worden gezien als interactie op het gegevensvlak of toepassingsinteractie. Het kopiëren van een blob in een opslagaccount en toegang verkrijgen tot een openbaar IP-adres via RDP of SSH in de virtuele machine vallen ook binnen het gegevensvlak. Deze bewerkingen zorgen ervoor dat de toepassing uitgevoerd blijft worden bij het berekenen, het netwerken en de opslag.

![Schermopname die het verschil tussen het beheer-/controlevlak en het gegevensvlak weergeeft](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)

> [!NOTE]
> In sommige scenario's voor migratie stopt het Azure-platform, maakt de toewijzingen ongedaan en start uw virtuele machines opnieuw op. Dit leidt tot een korte uitvaltijd voor het gegevensvlak.
>
>

## <a name="the-migration-experience"></a>De migratie-ervaring
We raden u aan het volgende te doen voordat u de migratie-ervaring start:

* Zorg ervoor dat de resources die u wilt migreren geen niet-ondersteunde functies of configuraties gebruiken. Meestal detecteert het platform deze problemen en genereert een fout.
* Als u virtuele machines hebt die zich niet in een virtueel netwerk bevinden, worden deze gestopt en de toewijzing ongedaan gemaakt als onderdeel van de voorbereidingsbewerking. Als u het openbare IP-adres niet wilt verliezen, kunt u kijken of u het IP-adres kunt reserveren voordat u de voorbereidingsbewerking activeert. Als de virtuele machines zich echter in een virtueel netwerk bevinden, worden ze niet gestopt en wordt de toewijzing niet ongedaan gemaakt.
* Plan de migratie buiten kantooruren zodat u de tijd hebt om onverwachte fouten die kunnen ontstaan tijdens de migratie op te lossen.
* Download de huidige configuratie van uw virtuele machines met behulp van PowerShell, CLI-opdrachten (opdrachtregelinterface) of REST API's om de validatie eenvoudiger te maken nadat de voorbereidingsstap is voltooid.
* Werk voordat u de migratie start uw scripts voor automatisering/uitvoering bij zodat ze het implementatiemodel van Resource Manager kunnen verwerken. Desgewenst kunt u GET-bewerkingen uitvoeren wanneer de resources de status Voorbereid hebben.
* Evalueer de RBAC-beleidsregels die zijn geconfigureerd op de klassieke IaaS-resources en plan ze in voor wanneer de migratie voltooid is.

De migratiewerkstroom verloopt als volgt

![Schermafbeelding van de migratiewerkstroom](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Alle bewerkingen die worden beschreven in de volgende secties zijn idempotent. Als er een probleem optreedt dat niets te maken heeft met een niet-ondersteunde functie of een configuratiefoutbericht, raden we u aan om de voorbereidings-, afbrekings- of doorvoerbewerking opnieuw te proberen. Het Azure-platform probeert de actie opnieuw uit te voeren.
>
>

### <a name="validate"></a>Valideren
De bewerking Valideren is de eerste stap in het migratieproces. Het doel van deze stap is het op de achtergrond analyseren van gegevens voor de resources die worden gemigreerd en rapporteren of de resources geschikt zijn voor migratie.

U selecteert het virtuele netwerk dat of de gehoste service die (indien dit niet een virtueel netwerk is) u wilt valideren voor migratie.

* Als de resource niet geschikt is voor migratie, geeft het Azure-platform alle redenen weer waarom deze niet wordt ondersteund voor migratie.

Wanneer u opslagservices valideert, wordt het gemigreerde account in een resourcegroep geplaatst met de naam van uw opslagaccount met '-gemigreerd' toegevoegd.  Als de naam van uw opslagaccount bijvoorbeeld 'mijnopslag' is, vindt u de resource met Azure Resource Manager ingeschakeld in een resourcegroep met de naam 'mijnopslag-gemigreerd'. Deze groep bevat een opslagaccount met de naam 'mijnopslag'.

### <a name="prepare"></a>Voorbereiden
De bewerking Voorbereiden is de tweede stap in het migratieproces. Het doel van deze stap is om de transformatie van de IaaS-resources van klassieke resources naar Resource Manager-resources te simuleren en deze naast elkaar weer te geven zodat u ze kunt bekijken.

U selecteert het virtuele netwerk dat of de gehoste service die (indien dit niet een virtueel netwerk is) u wilt voorbereiden voor migratie.

* Als de resource niet geschikt is voor migratie, stopt het Azure-platform het migratieproces en wordt de reden waarom de bewerking Voorbereiden is mislukt weergegeven.
* Als de resource kan worden gemigreerd, vergrendelt het Azure-platform eerst de bewerkingen binnen het beheervlak voor de resources die worden gemigreerd. U kunt bijvoorbeeld geen gegevensschijf toevoegen aan een virtuele machine die wordt gemigreerd.

Het Azure-platform start vervolgens de migratie van metagegevens van de klassieke versie naar Resource Manager voor de resources die worden gemigreerd.

Nadat de bewerking Voorbereiden is voltooid, hebt u de optie om de resources in zowel de klassieke versie als de Resource Manager te visualiseren. Voor elke cloudservice in het klassieke implementatiemodel maakt het Azure-platform een resourcegroepnaam met de indeling `cloud-service-name>-Migrated`.

> [!NOTE]
> Het is niet mogelijk de naam van de resourcegroep die is gemaakt voor de gemigreerde resources te selecteren ('-gemigreerd'), maar nadat de migratie is voltooid, kunt u de Azure Resource Manager-functie gebruiken om resources te verplaatsen naar een resourcegroep naar keuze. Zie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../articles/resource-group-move-resources.md) voor meer informatie

Hier vindt u twee schermafbeeldingen die het resultaat van een geslaagde voorbereidingsbewerking weergegeven. De eerste schermafbeelding geeft een resourcegroep met de oorspronkelijke cloudservice weer. De tweede schermafbeelding geeft de nieuwe '-gemigreerd ' resourcegroep met de equivalente Azure Resource Manager-resources weer.

![Schermafbeelding van portal met klassieke cloudservice](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Schermafbeelding van Azure Resource Manager-resources in voorbereiding](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

> [!NOTE]
> Virtuele machines die zich niet in een klassiek virtueel netwerk bevinden, worden in deze fase van de migratie gestopt en de toewijzing wordt ongedaan gemaakt.
>
>

### <a name="check-manual-or-scripted"></a>Controleren (handmatig of gepland)
In de stap Controleren kunt desgewenst u de configuratie gebruiken die u eerder hebt gedownload om te valideren dat de migratie is gelukt. U kunt zich ook aanmelden bij de portal en de eigenschappen en resources controleren om er zeker van te zijn dat de migratie van de metagegevens er goed uitziet.

Als u een virtueel netwerk migreert, wordt de configuratie van virtuele machines meestal niet opnieuw opgestart. U kunt valideren of de toepassingen op deze VM’s nog steeds actief zijn.

U kunt uw bewaking/automatisering en operationele scripts testen om te zien of de virtuele machines werken zoals verwacht en of uw bijgewerkte scripts goed werken. Alleen GET-bewerkingen worden ondersteund wanneer de resources de status Voorbereid hebben.

U hoeft de migratie niet binnen een bepaalde tijd door te voeren. U kunt zoveel tijd nemen als u wilt in deze fase. Het beheervlak is echter wel vergrendeld voor deze resources totdat u de migratie afbreekt of doorvoert.

Als er problemen zijn, kunt u altijd de migratie altijd afbreken en terugkeren naar het klassieke implementatiemodel. Nadat u bent teruggegaan, opent het Azure-platform de beheervlakbewerkingen voor de resources zodat u kunt de normale bewerkingen op deze virtuele machines in het klassieke implementatiemodel kunt hervatten.

### <a name="abort"></a>Afbreken
Afbreken is een optionele stap waarmee u uw wijzigingen aan het klassieke implementatiemodel ongedaan kunt maken en de migratie kunt stoppen.

> [!NOTE]
> Deze bewerking kan niet meer worden uitgevoerd nadat u de doorvoerbewerking hebt geactiveerd.     
>
>

### <a name="commit"></a>Doorvoeren
Nadat de validatie is voltooid, kunt u de migratie doorvoeren. Resources worden niet meer weergegeven in de klassieke versie en zijn alleen beschikbaar in het Resource Manager-implementatiemodel. De gemigreerde resources kunnen alleen in de nieuwe portal worden beheerd.

> [!NOTE]
> Dit is een idempotente bewerking. Als deze mislukt, raden we aan om de bewerking opnieuw te proberen. Als de bewerking blijft mislukken, maakt u een ondersteuningsticket of een bericht met de tag ClassicIaaSMigration op ons [VM-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>
<br>
Hier volgt een stroomdiagram van de stappen tijdens een migratie

![Schermafbeelding van de migratiestappen](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-to-azure-resource-manager-resources"></a>De vertaling van klassieke resources naar Azure Resource Manager-resources
U vindt de klassieke en de Resource Manager-versies van de resources in de volgende tabel. Andere functies en resources worden momenteel niet ondersteund.

| Klassieke weergave | Weergave van de Resource Manager | Gedetailleerde opmerkingen |
| --- | --- | --- |
| Naam cloudservice |DNS-naam |Tijdens de migratie wordt een nieuwe resourcegroep gemaakt voor elke cloudservice met het naamgevingspatroon `<cloudservicename>-migrated`. Deze resourcegroep bevat al uw resources. De naam van de cloudservice wordt een DNS-naam die is gekoppeld aan het openbare IP-adres. |
| Virtuele machine |Virtuele machine |VM-specifieke eigenschappen worden ongewijzigd gemigreerd. Bepaalde osProfile-gegevens, zoals de naam van de computer, worden niet opgeslagen in het klassieke implementatiemodel en blijven leeg na de migratie. |
| Schijfresources die zijn gekoppeld aan de VM |Impliciete schijven die zijn gekoppeld aan de VM |Schijven worden niet gemodelleerd als resources op het hoogste niveau in het Resource Manager-implementatiemodel. Ze worden gemigreerd als impliciete schijven onder de virtuele machine. Alleen de schijven die zijn gekoppeld aan een virtuele machine worden momenteel ondersteund. Virtuele Resource Manager-machines kunnen nu gebruikmaken van klassieke opslagaccounts, waardoor de schijven eenvoudig zonder de updates kunnen worden gemigreerd. |
| VM-extensies |VM-extensies |Alle de resource-extensies, met uitzondering van XML-extensies, worden gemigreerd vanuit het klassieke implementatiemodel. |
| Certificaten van virtuele machine |Certificaten in Azure Key Vault |Als een cloudservice servicecertificaten bevat, wordt er een nieuwe Azure-sleutelkluis per cloudservice gemaakt en worden de certificaten verplaatst naar de sleutelkluis. De virtuele machines worden bijgewerkt zodat ze verwijzen naar de certificaten van de sleutelkluis. <br><br> **OPMERKING:** Verwijder de sleutelkluis niet. Dit kan ertoe leiden dat de VM de status Mislukt krijgt. We proberen de back-end te verbeteren zodat sleutelkluizen veilig kunnen worden verwijderd of samen met de virtuele machine naar een nieuw abonnement kunnen worden verplaatst. |
| WinRM-configuratie |WinRM-configuratie in osProfile |De Windows Remote Management-configuratie wordt ongewijzigd verplaatst als onderdeel van de migratie. |
| Eigenschap van beschikbaarheidsset |Resource van beschikbaarheidsset | Het specificeren van beschikbaarheidssets was een eigenschap van de virtuele machine in het klassieke implementatiemodel. Beschikbaarheidssets worden een resource op het hoogste niveau als onderdeel van de migratie. De volgende configuraties worden niet ondersteund: meerdere beschikbaarheidssets per cloudservice of één of meer beschikbaarheidssets samen met de virtuele machines die zich niet in een beschikbaarheidsset in een cloudservice bevinden. |
| Netwerkconfiguratie op een virtuele machine |Primaire netwerkinterface |De netwerkconfiguratie op een virtuele machine wordt weergegeven als de resource primaire netwerkinterface na de migratie. Het interne IP-adres van virtuele machines die zich niet in een virtueel netwerk bevinden, wordt gewijzigd tijdens de migratie. |
| Meerdere netwerkinterfaces in een VM |Netwerkinterfaces |Als aan een virtuele machine meerdere netwerkinterfaces zijn gekoppeld, wordt elke netwerkinterface een resource op het hoogste niveau als onderdeel van de migratie naar het Resource Manager-implementatiemodel, samen met alle eigenschappen. |
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
| Load balancer met meerdere IP-adressen |Load balancer met meerdere openbare IP-resources |Elke openbaar IP-adres dat is gekoppeld aan de load balancer wordt omgezet in een openbare IP-resource en gekoppeld aan de load balancer na de migratie. |
| Interne DNS-namen op de virtuele machine |Interne DNS-namen op de NIC |Tijdens de migratie worden de interne DNS-achtervoegsels voor de virtuele machines gemigreerd naar een alleen-lezeneigenschap met de naam 'InternalDomainNameSuffix' op de NIC. Het achtervoegsel blijft ongewijzigd na de migratie en de VM-resolutie moet blijven werken als voorheen. |
| Gateway voor een virtueel netwerk |Gateway voor een virtueel netwerk |Eigenschappen van de gateway voor virtuele netwerken worden ongewijzigd gemigreerd. Het VIP dat is gekoppeld aan de gateway wordt ook niet gewijzigd. |
| Lokale netwerksite |Lokale netwerkgateway |Eigenschappen van de lokale netwerksite worden ongewijzigd gemigreerd aan een nieuwe resource met de naam Lokale netwerkgateway. Deze bevat on-premises adresvoorvoegsels en een externe gateway-IP. |
| Verwijzingen naar verbindingen |Verbinding |Na de migratie worden de verwijzingen naar verbindingen tussen de gateway en de lokale netwerksite in de netwerkconfiguratie in Resource Manager vertegenwoordigd door een nieuwe resource met de naam Verbinding. Alle eigenschappen van verwijzingen naar verbindingen tussen netwerkconfiguratiebestanden worden ongewijzigd gekopieerd naar de nieuwe resource Verbinding. Verbindingen tussen VNets worden in de klassieke versie tot stand gebracht door twee IPsec-tunnels te maken naar lokale netwerksites die de VNets vertegenwoordigen. Dit wordt in het Resource Manager-model omgezet naar het verbindingstype Vnet2Vnet. Hiervoor zijn geen lokale netwerkgateways vereist. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Wijzigingen aanbrengen in uw automatisering en hulpprogramma’s na de migratie
U moet uw bestaande automatisering of hulpprogramma’s bijwerken om ervoor te zorgen dat de blijven werken blijft werken na de migratie. Dit is onderdeel van de migratie van uw resources van het klassieke implementatiemodel naar het Resource Manager-implementatiemodel.
