---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: dc871b29cdafa57d337f9be6cf01e76212f31b67
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227375"
---
## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>Migreren naar Azure Resource Manager IaaS-resources van het klassieke implementatiemodel
Eerst, is het belangrijk te begrijpen van het verschil tussen data plane en beheervlak bewerkingen op de infrastructuur als een service (IaaS)-resources.

* *Beheer-/ controlevlak* beschrijft de aanroepen die in de beheer-/ controlevlak of de API voor het wijzigen van resources. Bewerkingen zoals het maken van een VM, opnieuw starten van een VM en het bijwerken van een virtueel netwerk met een nieuw subnet beheren de actieve resources. Ze niet rechtstreeks invloed op verbinding maken met de virtuele machines.
* *Gegevenslaag* (toepassing) beschrijft de runtime van de toepassing zelf, en omvat interactie met exemplaren die niet door de API van Azure. Toegang tot uw website of het binnenhalen van gegevens van een actief exemplaar van SQL Server of een MongoDB-server, zijn bijvoorbeeld gegevens gegevensvlak of interacties. Andere voorbeelden zijn een blob kopiëren van een storage-account en toegang tot een openbaar IP-adres voor het gebruik van Remote Desktop Protocol (RDP) of Secure Shell (SSH) bij de virtuele machine. Deze bewerkingen zorgen ervoor dat de toepassing uitgevoerd blijft worden bij het berekenen, het netwerken en de opslag.

Het vlak van gegevens is tussen het klassieke implementatiemodel en Resource Manager-stacks hetzelfde. Het verschil is dat de Microsoft tijdens het migratieproces, de weergave van de resources van het klassieke implementatiemodel met die in het Resource Manager-stack zet. Als gevolg hiervan moet u nieuwe hulpprogramma's, API's en SDK's gebruiken voor het beheren van uw resources in het Resource Manager-stack.

![Diagram met het verschil tussen de beheer-/ controlevlak en de gegevenslaag](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> In sommige scenario's voor migratie stopt het Azure-platform, maakt de toewijzingen ongedaan en start uw virtuele machines opnieuw op. Dit veroorzaakt een korte uitvaltijd voor de gegevenslaag.
>

## <a name="the-migration-experience"></a>De migratie-ervaring
Voordat u de migratie begint:

* Zorg ervoor dat de resources die u wilt migreren geen niet-ondersteunde functies of configuraties gebruiken. Meestal detecteert het platform deze problemen en genereert een fout.
* Als u virtuele machines die zich niet in een virtueel netwerk hebt, zijn gestopt en toewijzing ongedaan gemaakt als onderdeel van de voorbereidingsbewerking. Als u niet wilt verliezen het openbare IP-adres, kunt u het IP-adres reserveren voordat u de voorbereidingsbewerking activeert. Als de virtuele machines zich in een virtueel netwerk, worden ze niet gestopt en toewijzing ongedaan gemaakt.
* Plan de migratie buiten kantooruren zodat u de tijd hebt om onverwachte fouten die kunnen ontstaan tijdens de migratie op te lossen.
* Download de huidige configuratie van uw virtuele machines met behulp van PowerShell, CLI-opdrachten (opdrachtregelinterface) of REST API's om de validatie eenvoudiger te maken nadat de voorbereidingsstap is voltooid.
* Werk uw automatisering en uitoefening scripts voor het afhandelen van de Resource Manager-implementatiemodel, voordat u de migratie start. Desgewenst kunt u GET-bewerkingen uitvoeren wanneer de resources de status Voorbereid hebben.
* Evalueer de Role-Based Access Control (RBAC)-beleidsregels die zijn geconfigureerd op de IaaS-resources in het klassieke implementatiemodel en plannen voor na de migratie voltooid is.

De Migratiewerkstroom verloopt als volgt:

![Diagram met de Migratiewerkstroom](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> De bewerkingen die worden beschreven in de volgende secties zijn alle idempotent. Als er een probleem met uitzondering van een niet-ondersteunde functie of een Configuratiefoutbericht wordt weergegeven, probeer de voorbereiden afgebroken of doorgevoerd, bewerking. Azure probeert de actie opnieuw uit.
>
>

### <a name="validate"></a>Valideren
De bewerking Valideren is de eerste stap in het migratieproces. Het doel van deze stap is voor het analyseren van de status van de resources die u wilt migreren in het klassieke implementatiemodel. De bewerking beoordeelt of de resources geschikt voor migratie (slagen of mislukken zijn).

U selecteert het virtuele netwerk of een cloudservice (indien dit niet in een virtueel netwerk is) die u wilt valideren voor migratie. Als de resource niet kan worden gemigreerd, Azure geeft een lijst van de redenen waarom.

#### <a name="checks-not-done-in-the-validate-operation"></a>Controleert niet uitgevoerd in de bewerking valideren

De bewerking valideren analyseert alleen de status van de resources in het klassieke implementatiemodel. Deze kunt controleren voor alle mislukte aanvragen en niet-ondersteunde scenario's vanwege verschillende configuraties in het klassieke implementatiemodel. Het is niet mogelijk om te controleren voor alle problemen die de Azure Resource Manager-stack aan de resources tijdens de migratie opleggen kan. Deze problemen zijn alleen ingeschakeld wanneer de resources worden onderworpen aan de transformatie in de volgende stap van de migratie (de voorbereidingsbewerking). De volgende tabel geeft een lijst van alle problemen die niet is ingeschakeld in de bewerking valideren:


|Controles van de netwerken niet in de bewerking valideren|
|-|
|Een virtueel netwerk dat u hoeft ER- en VPN-gateway.|
|Een gateway-verbinding voor het virtuele netwerk in een niet-verbonden status.|
|Alle ER circuits vooraf gemigreerd naar Azure Resource Manager-stack.|
|Azure Resource Manager-quotum controleert netwerkresources. Bijvoorbeeld: statisch openbaar IP-adres, dynamische openbare IP-adressen, load balancer, netwerkbeveiligingsgroepen, routetabellen en netwerkinterfaces. |
| Alle load balancer-regels zijn geldig voor implementatie en het virtuele netwerk. |
| Conflicterende privé-IP's tussen virtuele machines in hetzelfde virtuele netwerk stop-toewijzing ongedaan gemaakt. |

### <a name="prepare"></a>Voorbereiden
De bewerking Voorbereiden is de tweede stap in het migratieproces. Het doel van deze stap is om Simuleer de transformatie van de IaaS-resources van het klassieke implementatiemodel naar Resource Manager-resources. Bovendien geeft de voorbereidingsbewerking deze side-by-side voor u om te visualiseren.

> [!NOTE] 
> Uw resources in het klassieke implementatiemodel zijn niet gewijzigd tijdens deze stap. Het is een veilige stap uit te voeren als u om de migratie probeert. 

U selecteert het virtuele netwerk of de service in de cloud (indien dit niet een virtueel netwerk is) dat u wilt voorbereiden voor migratie.

* Als de resource niet kan worden gemigreerd, wordt Azure stopt met het migratieproces en wordt de reden waarom de voorbereidingsbewerking is mislukt.
* Als de resource kan worden gemigreerd, wordt Azure vergrendeld bewerkingen op het beheervlak voor de resources wordt gemigreerd. U kunt bijvoorbeeld geen gegevensschijf toevoegen aan een virtuele machine die wordt gemigreerd.

Azure start vervolgens de migratie van metagegevens van het klassieke implementatiemodel naar Resource Manager voor de resources die worden gemigreerd.

Nadat de voorbereidingsbewerking voltooid is, hebt u de mogelijkheid van de resources in zowel het klassieke implementatiemodel en het resourcemanager te visualiseren. Voor elke cloudservice in het klassieke implementatiemodel maakt het Azure-platform een resourcegroepnaam met de indeling `cloud-service-name>-Migrated`.

> [!NOTE]
> Het is niet mogelijk om de naam van een resourcegroep gemaakt voor de gemigreerde resources te selecteren (dat wil zeggen, '-gemigreerd '). Nadat de migratie is voltooid, maar kunt u de functie van Azure Resource Manager om resources te verplaatsen naar een resourcegroep die u wilt. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../articles/resource-group-move-resources.md).

De volgende twee schermafbeeldingen tonen het resultaat na een geslaagde voorbereiding is. Het eerste certificaat ziet u een resourcegroep met de oorspronkelijke cloudservice. In het tweede voorbeeld ziet u de nieuwe '-gemigreerd ' resourcegroep met de equivalente Azure Resource Manager-resources.

![Schermafbeelding van oorspronkelijke cloudservice](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Schermafbeelding van Azure Resource Manager-resources in de voorbereidingsbewerking](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Hier volgt een schermen overzicht van uw resources na de voltooiing van de fase voorbereiden. Houd er rekening mee dat de resource in het vlak van gegevens hetzelfde is. Deze wordt vertegenwoordigd in zowel de beheerlaag (klassieke implementatiemodel) en het besturingselement vlak (Resource Manager).

![Diagram van de fase voorbereiden](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuele machines die zich niet in een virtueel netwerk in het klassieke implementatiemodel worden gestopt en toewijzing ongedaan gemaakt in deze fase van de migratie.
>

### <a name="check-manual-or-scripted"></a>Controleren (handmatig of gepland)
In de stap controleren hebt u de optie voor het gebruik van de configuratie die u eerder hebt gedownload om te valideren dat de migratie is gelukt. U kunt ook kunt u aanmelden bij de portal en controle de eigenschappen en resources om te valideren dat migratie van de metagegevens er goed uitziet.

Als u een virtueel netwerk migreert, wordt de configuratie van virtuele machines meestal niet opnieuw opgestart. Voor toepassingen op deze virtuele machines, kunt u valideren dat de toepassing nog steeds wordt uitgevoerd.

U kunt testen uw bewakings- en operationele scripts om te controleren of de virtuele machines werken zoals verwacht, en of uw bijgewerkte scripts goed werken. Alleen GET-bewerkingen worden ondersteund wanneer de resources de status Voorbereid hebben.

Er is geen venster instellen van de tijd waarbinnen moet u de migratie doorvoeren. U kunt zoveel tijd nemen als u wilt in deze fase. Het beheervlak is echter wel vergrendeld voor deze resources totdat u de migratie afbreekt of doorvoert.

Als er problemen zijn, kunt u altijd de migratie altijd afbreken en terugkeren naar het klassieke implementatiemodel. Nadat u teruggegaan bent, wordt de beheerlaag bewerkingen op de resources in Azure geopend zodat u normale bewerkingen op deze virtuele machines in het klassieke implementatiemodel kunt hervatten.

### <a name="abort"></a>Afbreken
Dit is een optionele stap als u wilt uw wijzigingen op het klassieke implementatiemodel ongedaan maken en stoppen van de migratie. Met deze bewerking worden de metagegevens van de Resource Manager (gemaakt in de voorbereidingsstap) voor uw resources verwijderd. 

![Diagram van de stap afbreken](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Met deze bewerking kan niet worden uitgevoerd nadat u de doorvoerbewerking hebt geactiveerd.     
>

### <a name="commit"></a>Doorvoeren
Nadat de validatie is voltooid, kunt u de migratie doorvoeren. Resources worden niet meer weergegeven in het klassieke implementatiemodel, en zijn alleen beschikbaar in het Resource Manager-implementatiemodel. De gemigreerde resources kunnen alleen in de nieuwe portal worden beheerd.

> [!NOTE]
> Dit is een idempotente bewerking. Als dit mislukt, probeer het opnieuw. Als de pogingen blijven mislukken, maakt een ondersteuningsticket of een bericht met een 'ClassicIaaSMigration' tag op onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Diagram van de stap doorvoeren](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Stroomdiagram van migratie

Hier volgt een stroomdiagram die laat zien hoe u om door te gaan met de migratie:

![Schermafbeelding van de migratiestappen](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Vertaling van het klassieke implementatiemodel naar Resource Manager-resources
U vindt het klassieke implementatiemodel en Resource Manager-versies van de resources in de volgende tabel. Andere functies en resources worden momenteel niet ondersteund.

| Klassieke weergave | Weergave van de Resource Manager | Opmerkingen |
| --- | --- | --- |
| Naam cloudservice |DNS-naam |Tijdens de migratie wordt een nieuwe resourcegroep gemaakt voor elke cloudservice met het naamgevingspatroon `<cloudservicename>-migrated`. Deze resourcegroep bevat al uw resources. De naam van de cloudservice wordt een DNS-naam die is gekoppeld aan het openbare IP-adres. |
| Virtuele machine |Virtuele machine |VM-specifieke eigenschappen worden ongewijzigd gemigreerd. Bepaalde osProfile-gegevens, zoals de naam van de computer, worden niet opgeslagen in het klassieke implementatiemodel en blijven leeg na de migratie. |
| Schijfresources die zijn gekoppeld aan de VM |Impliciete schijven die zijn gekoppeld aan de VM |Schijven worden niet gemodelleerd als resources op het hoogste niveau in het Resource Manager-implementatiemodel. Ze worden gemigreerd als impliciete schijven onder de virtuele machine. Alleen de schijven die zijn gekoppeld aan een virtuele machine worden momenteel ondersteund. Resourcemanager-VM's kunnen nu gebruikmaken van storage-accounts in het klassieke implementatiemodel, waardoor de schijven eenvoudig zonder de updates worden gemigreerd. |
| VM-extensies |VM-extensies |Alle de resource-extensies, met uitzondering van XML-extensies, worden gemigreerd vanuit het klassieke implementatiemodel. |
| Certificaten van virtuele machine |Certificaten in Azure Key Vault |Als een cloudservice servicecertificaten bevat, wordt de migratie wordt een nieuwe Azure-sleutelkluis per cloudservice gemaakt en de certificaten is verplaatst naar de key vault. De virtuele machines worden bijgewerkt zodat ze verwijzen naar de certificaten van de sleutelkluis. <br><br> De key vault niet verwijderen. Hierdoor kan de virtuele machine in een foutstatus. |
| WinRM-configuratie |WinRM-configuratie in osProfile |De Windows Remote Management-configuratie wordt ongewijzigd verplaatst als onderdeel van de migratie. |
| Eigenschap van beschikbaarheidsset |Resource van beschikbaarheidsset | Beschikbaarheidssets is een eigenschap van de virtuele machine in het klassieke implementatiemodel. Beschikbaarheidssets worden een resource op het hoogste niveau als onderdeel van de migratie. De volgende configuraties worden niet ondersteund: meerdere beschikbaarheidssets per cloudservice of één of meer beschikbaarheidssets samen met de virtuele machines die zich niet in een beschikbaarheidsset in een cloudservice bevinden. |
| Netwerkconfiguratie op een virtuele machine |Primaire netwerkinterface |De netwerkconfiguratie op een virtuele machine wordt weergegeven als de resource primaire netwerkinterface na de migratie. Het interne IP-adres van virtuele machines die zich niet in een virtueel netwerk bevinden, wordt gewijzigd tijdens de migratie. |
| Meerdere netwerkinterfaces in een VM |Netwerkinterfaces |Als een virtuele machine meerdere netwerkinterfaces die zijn gekoppeld, wordt elke netwerkinterface een resource op het hoogste niveau als onderdeel van de migratie, samen met alle eigenschappen. |
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
| Load balancer met meerdere IP-adressen |Load balancer met meerdere openbare IP-resources |Elke openbaar IP-adres dat is gekoppeld aan de load balancer is geconverteerd naar een openbare IP-resource en die zijn gekoppeld aan de load balancer na de migratie. |
| Interne DNS-namen op de virtuele machine |Interne DNS-namen op de NIC |Tijdens de migratie worden de interne DNS-achtervoegsels voor de virtuele machines gemigreerd naar een alleen-lezeneigenschap met de naam 'InternalDomainNameSuffix' op de NIC. Het achtervoegsel blijft ongewijzigd na de migratie en VM-resolutie moet blijven werken als voorheen. |
| Gateway van een virtueel netwerk |Gateway van een virtueel netwerk |Eigenschappen van de gateway virtuele netwerken worden ongewijzigd gemigreerd. Het VIP dat is gekoppeld aan de gateway wordt ook niet gewijzigd. |
| Lokale netwerksite |Lokale netwerkgateway |Lokale netwerk site-eigenschappen worden ongewijzigd gemigreerd aan een nieuwe resource met de naam een lokale netwerkgateway. Hiermee wordt on-premises adresvoorvoegsels en het IP-adres voor de RAS-gateway. |
| Verwijzingen naar verbindingen |Verbinding |Verwijzingen naar verbindingen tussen de gateway en de lokale netwerksite in de configuratie van het netwerk wordt vertegenwoordigd door een nieuwe resource-verbinding genoemd. Alle eigenschappen van verwijzingen naar verbindingen met een netwerk-configuratiebestanden worden ongewijzigd gekopieerd naar de resource verbinding. Connectiviteit tussen virtuele netwerken in het klassieke implementatiemodel wordt bereikt door twee IPsec-tunnels naar lokale netwerksites die de virtuele netwerken maken. Dit wordt omgezet naar het verbindingstype van virtuele-netwerk-naar-virtueel-netwerk in het Resource Manager-model, zonder lokale netwerkgateways. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Wijzigingen aanbrengen in uw automatisering en hulpprogramma’s na de migratie
Als onderdeel van de migratie van uw resources van het klassieke implementatiemodel naar het Resource Manager-implementatiemodel, moet u uw bestaande automatisering of hulpprogramma's om ervoor te zorgen dat het blijft werken na de migratie bijwerken.
