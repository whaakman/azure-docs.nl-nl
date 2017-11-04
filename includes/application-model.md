# <a name="compute"></a>Compute
Azure kunt u implementeren en bewaken van uw toepassingscode uitgevoerd binnen een Microsoft-datacentrum. Wanneer u een toepassing maken en uitvoeren op Azure, de code en configuratie samen heet een Azure gehoste service. Gehoste services zijn gemakkelijk te beheren, omhoog en omlaag geschaald, configureren en werken met nieuwe versies van uw toepassingscode. In dit artikel is gericht op de Azure gehoste service toepassingsmodel.<a id="compare" name="compare"></a>

## Inhoudsopgave<a id="_GoBack" name="_GoBack"></a>
* [Voordelen van Azure-toepassing-Model][Azure Application Model Benefits]
* [Gehoste Service belangrijkste concepten][Hosted Service Core Concepts]
* [Overwegingen bij het ontwerpen van de gehoste Service][Hosted Service Design Considerations]
* [Uw aanvraag voor schaal ontwerpen][Designing your Application for Scale]
* [De definitie van de gehoste Service en -configuratie][Hosted Service Definition and Configuration]
* [Het Servicedefinitiebestand][The Service Definition File]
* [Het configuratiebestand van de Service][The Service Configuration File]
* [Maken en implementeren van een gehoste Service][Creating and Deploying a Hosted Service]
* [Verwijzingen][References]

## <a id="benefits"></a>Model voordelen van azure-toepassing
Wanneer u uw toepassing als een gehoste service implementeert, Azure maakt een of meer virtuele machines (VM's) die uw toepassing code bevatten en de virtuele machines op fysieke computers die zich in een van de Azure-datacenters wordt opgestart. Als aanvragen van clients naar uw gehoste toepassing het datacenter invoert, distribueert een load balancer deze aanvragen evenredig aan de virtuele machines. Terwijl de toepassing wordt gehost in Azure, ophalen het van drie belangrijke voordelen:

* **Hoge beschikbaarheid.** Hoge beschikbaarheid betekent dat Azure zorgt ervoor dat uw toepassing zo veel mogelijk wordt uitgevoerd en kunnen reageren op aanvragen van clients. Als uw toepassing wordt beëindigd (vanwege een onverwerkte uitzondering, bijvoorbeeld), en vervolgens Azure detecteert DPM dit en automatisch wordt opnieuw uw toepassing te starten. Als de machine uw toepassing wordt uitgevoerd op de ervaringen van een soort hardwarefout, wordt vervolgens Azure ook dit detecteren en automatisch een nieuwe virtuele machine maken op een andere werkende fysieke machine en de code van daaruit uitvoeren. Opmerking: In de volgorde voor uw toepassing om op te halen van Microsoft Service Level Agreement van 99,95% die beschikbaar is, moet u ten minste twee virtuele machines met uw toepassingscode hebben. Hiermee kunt één virtuele machine naar de clientaanvragen worden verwerkt terwijl Azure uw code van een mislukte virtuele machine naar een nieuwe, goed VM verplaatst.
* **Schaalbaarheid.** Azure kunt u eenvoudig en het aantal virtuele machines waarop uw toepassingscode wordt uitgevoerd voor het afhandelen van de daadwerkelijke laadbewerking wordt geplaatst op uw toepassing dynamisch te wijzigen. Hiermee kunt u uw toepassing de werkbelasting die uw klanten zijn die erop tijdens betaalt alleen voor de virtuele machines die u nodig hebt wanneer u ze moet aanpassen. Als u wijzigen van het aantal virtuele machines wilt, wordt Azure reageert binnen enkele minuten waardoor het mogelijk om het aantal virtuele machines met zo vaak als gewenste dynamisch te wijzigen.
* **Beheerbaarheid.** Omdat Azure een Platform als een Service (PaaS is) bieden, beheert het de infrastructuur (de hardware zelf, elektriciteit en netwerken) die vereist dat deze machines die worden uitgevoerd. Azure beheert ook het platform, zodat een actuele besturingssysteem met alle de juiste patches en beveiligingsupdates, evenals de updates van de onderdelen zoals de .NET Framework en Internet Information Server. Omdat de virtuele machines op Windows Server 2008 worden uitgevoerd, biedt Azure aanvullende functies, zoals diagnostische bewaking, ondersteuning voor externe bureaublad firewalls en configuratie van gegevensarchief certificaat. Al deze functies vindt u zonder extra kosten. De licentie voor Windows Server 2008-besturingssysteem (OS) is in feite is opgenomen wanneer u uw toepassing in Azure uitvoert. Aangezien alle van de virtuele machines zijn Windows Server 2008 wordt uitgevoerd, werkt de code die wordt uitgevoerd op Windows Server 2008 goed bij uitvoering in Azure.

## <a id="concepts"></a>Gehoste Service belangrijkste concepten
Als uw toepassing wordt geïmplementeerd als een gehoste service in Azure, deze wordt uitgevoerd als een of meer *rollen.* Een *rol* gewoon verwijst naar de toepassingsbestanden en configuratie. U kunt een of meer rollen definiëren voor uw toepassing, elk met een eigen set van toepassingsbestanden en de configuratie. Voor elke rol in uw toepassing, kunt u het aantal virtuele machines, opgeven of *rolinstanties*, om uit te voeren. De afbeelding hieronder twee eenvoudige voorbeelden van een toepassing die is gemodelleerd als een gehoste service met behulp van de rollen en rolinstanties weergeven.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Afbeelding 1: Één rol met drie exemplaren (VM's) uitgevoerd in een Azure-Datacenter
![Afbeelding][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Afbeelding 2: Twee rollen, elk met twee exemplaren (VM's) uitgevoerd in een Azure-Datacenter
![Afbeelding][1]

Rolinstanties verwerken doorgaans aanvragen van internetclients invoeren van het datacenter zogenaamde een *invoereindpunt*. Een enkele rol kan 0 of meer invoer eindpunten hebben. Elk eindpunt geeft aan dat een protocol (HTTP, HTTPS of TCP) en een poort. Het is gebruikelijk voor het configureren van een rol twee invoereindpunten: HTTP luisteren op poort 80 en HTTPS geluisterd op poort 443. De afbeelding hieronder toont een voorbeeld van twee verschillende rollen met verschillende invoereindpunten doorsturen van aanvragen van clients naar deze.

![Afbeelding][2]

Wanneer u een gehoste service in Azure maakt, krijgt het een openbaar adresseerbare IP-adres die clients gebruiken kunnen om deze te openen. U moet ook een URL-voorvoegsel dat is toegewezen aan dat IP-adres selecteren bij het maken van de gehoste service. Dit voorvoegsel moet uniek zijn als u in feite reserveert de *voorvoegsel*. cloudapp.net URL zodat dat niemand anders kunnen deze. Clients communiceren met uw rolinstanties met behulp van de URL. Meestal kunt u niet distribueren of publiceren van de Azure *voorvoegsel*. cloudapp.net URL. In plaats daarvan u koopt van een DNS-naam van uw DNS-registratieservice keuze en configureert de DNS-naam clientaanvragen omleiden naar de Azure-URL. Zie voor meer informatie [een aangepaste domeinnaam configureren in Azure][Configuring a Custom Domain Name in Azure].

## <a id="considerations"></a>Gehoste Service ontwerpoverwegingen
Bij het ontwerpen van een toepassing uit te voeren in een cloudomgeving gelden verschillende overwegingen bij het nadenken over zoals latentie, hoge beschikbaarheid en schaalbaarheid.

Bepalen waar u uw toepassingscode is een belangrijk aandachtspunt bij het uitvoeren van een gehoste service in Azure. Het is gebruikelijk voor het implementeren van uw toepassing-datacenters die zich het dichtst bij uw clients als u wilt de latentie te verminderen en de beste prestaties mogelijk.
U kunt echter een datacenter dichter bij uw bedrijf of dichter bij uw gegevens selecteren als u bepaalde bevoegdheid of rechtspersoon opmerkingen over uw gegevens en waar deze zich bevindt. Er zijn zes datacenters over de hele wereld geschikt is voor het hosten van uw toepassingscode. De onderstaande tabel ziet u de beschikbare locaties:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr>

<td style="width: 100px;">
**Land/regio**

</td>

<td style="width: 200px;">
**Onderliggende regio 's**

</td>
</tr>

<tr>

<td>
Verenigde Staten

</td>

<td>
Zuid-centraal & Noord-centraal

</td>
</tr>

<tr>

<td>
Europa

</td>

<td>
Noord & West

</td>
</tr>

<tr>

<td>
Azië

</td>

<td>
Zuidoost & Oost

</td>
</tr>
</tbody>
</table>
Wanneer een gehoste service maakt, selecteert u een subregio die wijzen op de locatie waarin u wilt dat uw code uit te voeren.

Om te zorgen voor hoge beschikbaarheid en schaalbaarheid, is het zeer belangrijk dat de gegevens van uw toepassing een centrale opslagplaats toegankelijk is voor meerdere rolexemplaren wordt gehandhaafd. Om dit te, biedt Azure verschillende opslagopties zoals blobs, tabellen en SQL-Database. Zie de [Data-oplossingen voor opslag in Azure] [ Data Storage Offerings in Azure] artikel voor meer informatie over deze opslagtechnologieën. De afbeelding hieronder ziet hoe de load balancer in de Azure-Datacenter aanvragen van clients naar andere rolinstanties die toegang tot dezelfde gegevensopslag hebben distribueert.

![Afbeelding][3]

Meestal kunt u wilt uw toepassingscode en uw gegevens niet vinden in hetzelfde Datacenter als Hiermee kunt u lage latentie (betere prestaties) wanneer de gegevens in uw toepassingscode wordt geopend. Bovendien u worden niet in rekening gebracht voor de netwerkbandbreedte wanneer gegevens worden verplaatst om binnen hetzelfde Datacenter.

## <a id="scale"></a>Ontwerpen van uw toepassing voor schaal
U wilt soms één toepassing (zoals een eenvoudige website) duren en dit wordt gehost in Azure. Maar vaak uw toepassing kan bestaan uit verschillende rollen die samenwerken. Bijvoorbeeld in de onderstaande afbeelding zijn er twee exemplaren van de rol van de Website, drie exemplaren van de rol doorvoeren en één exemplaar van het rapportgenerator-rol. Deze rollen alle werken samen en de code voor al deze kan worden samen verpakt en gedistribueerd als één eenheid-up naar Azure.

![Afbeelding][4]

De belangrijkste reden voor het splitsen van een toepassing in verschillende rollen elke uitgevoerd op een eigen set rolexemplaren (dat wil zeggen, VM's) is de rollen afzonderlijk van elkaar schalen. Bijvoorbeeld gedurende de feestdagen kunnen veel klanten worden aanschaffen producten van uw bedrijf, wilt u het aantal rolinstanties met de rol van uw Website, evenals het aantal rolexemplaren met de functie doorvoeren verhogen. Na de feestdagen krijgt u mogelijk een groot aantal producten die worden geretourneerd, dus u nog steeds een groot aantal exemplaren van de Website, maar minder exemplaren doorvoeren moet. Tijdens de rest van het jaar moet u mogelijk alleen exemplaren van een paar Website en doorvoeren. In dit alles moet u slechts één rapportgenerator-exemplaar. De flexibiliteit van implementaties op basis van rollen in Azure kunt u eenvoudig uw toepassing aan de behoeften van uw bedrijf aanpassen.

Het is gebruikelijk om de rol-exemplaren in de gehoste service met elkaar communiceren. Bijvoorbeeld de websiterol accepteert een klant, maar vervolgens wordt de verwerking van de verwerking van de Order-rolexemplaren volgorde verplaatst. De beste manier om door te geven werk formulier één set rolinstanties naar een andere reeks exemplaren met behulp van de queuing technologie die is verstrekt door Azure, de Queue-Service of de Service Bus-wachtrijen. Het gebruik van een wachtrij is een belangrijk onderdeel van de op te nemen. De wachtrij kan de gehoste service voor het schalen van de bijbehorende rollen onafhankelijk zodat u de werkbelasting tegen kosten worden verdeeld. Als het aantal berichten in de wachtrij gedurende een bepaalde periode stijgt, kunt u het aantal rolinstanties doorvoeren opschalen. Als het aantal berichten in de wachtrij gedurende een bepaalde periode afneemt, kunt u het aantal rolinstanties doorvoeren terugschroeven. Op deze manier u alleen betaalt voor de exemplaren die zijn vereist voor het afhandelen van de werkelijke werkbelasting.

De wachtrij biedt ook betrouwbaarheid. Wanneer het verkleinen van het aantal rolinstanties doorvoeren, Azure, besluit welke exemplaren te beëindigen. Kan besluiten een exemplaar dat is bezig met verwerken van een wachtrijbericht is beëindigd. Echter, omdat de berichtverwerking niet wordt voltooid, het bericht zichtbaar opnieuw voor een andere volgorde verwerken rolinstantie die deze opgehaald en verwerkt deze. Vanwege de zichtbaarheid van wachtrij-bericht, berichten gegarandeerd uiteindelijk verwerkt. De wachtrij fungeert ook als een load balancer door de berichten naar alle rolinstanties die berichten aanvraagt effectief te distribueren.

Voor de Website-rolexemplaren kunt u het verkeer naar deze controleren en bepalen het aantal schalen omhoog of omlaag ook. De wachtrij kunt u het aantal rolinstanties Website onafhankelijk van de verwerking van de Order-rolexemplaren schalen. Dit is zeer krachtige en biedt een grote flexibiliteit. Als uw toepassing uit de aanvullende functies bestaat, kunt u aanvullende wachtrijen natuurlijk toevoegen als het kanaal voor de communicatie tussen deze om te kunnen gebruikmaken van dezelfde schaal en de kosten van de voordelen.

## <a id="defandcfg"></a>Gehost servicedefinitie en configuratie
Een gehoste service implementeren in Azure, moet u ook beschikken over een servicedefinitiebestand en een configuratiebestand voor de service. Beide bestanden zijn XML-bestanden en Hiermee kunt u verklarend implementatieopties voor uw gehoste service opgeven. Het servicedefinitiebestand beschrijft alle van de rollen waaruit de gehoste service en hoe ze communiceren. Het configuratiebestand van de service wordt het aantal exemplaren voor elke rol en de instellingen die worden gebruikt voor het configureren van elke rolinstantie beschreven.

## <a id="def"></a>Het Servicedefinitiebestand
Als ik hierboven vermeld de servicedefinitie (CSDEF) is bestand een XML-bestand die beschrijft de verschillende rollen die gezamenlijk uw volledige toepassing. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx] [].
Het CSDEF-bestand bevat een element WebRole of WorkerRole voor elke rol die u wilt dat in uw toepassing. Implementatie van een rol als een Webrol (met behulp van het element WebRole) betekent dat de code wordt uitgevoerd op een rolinstantie met Windows Server 2008 en Internet Information Server (IIS).
Een rol implementeert, zoals een werkrol (met behulp van het element WorkerRole) betekent dat de instantie van Windows Server 2008 erop (wordt IIS niet geïnstalleerd).

U kunt gewoon maken en implementeren van een werkrol die gebruikmaakt van een andere manier om te luisteren naar binnenkomende webaanvragen (bijvoorbeeld uw code kan maken en gebruiken van een .NET-HttpListener). Aangezien de rolinstanties zijn alle Windows Server 2008 uitgevoerd, kunt uw code geen bewerkingen die normaal gesproken beschikbaar zijn voor een toepassing die wordt uitgevoerd op Windows-Server uitvoeren
2008.

Voor elke rol, geeft u aan de gewenste VM-grootte die voor exemplaren van deze rol wordt gebruikt. De onderstaande tabel ziet u de verschillende VM-grootten beschikbaar zijn en de kenmerken van elk:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr align="left" valign="top">

<td>
**VM-grootte**

</td>

<td>
**CPU**

</td>

<td>
**RAM-GEHEUGEN**

</td>

<td>
**Schijf**

</td>

<td>
**Piek netwerk-i/o-**

</td>
</tr>

<tr align="left" valign="top">

<td>
**Extra klein**

</td>

<td>
1 x 1,0 GHz

</td>

<td>
768 MB

</td>

<td>
20GB

</td>

<td>
\~5 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**Kleine**

</td>

<td>
1 x 1,6 GHz

</td>

<td>
1,75 GB

</td>

<td>
225GB

</td>

<td>
\~100 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**Gemiddeld**

</td>

<td>
2 x 1,6 GHz

</td>

<td>
3,5 GB

</td>

<td>
490GB

</td>

<td>
\~200 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**Grote**

</td>

<td>
4 x 1,6 GHz

</td>

<td>
7 GB

</td>

<td>
1TB

</td>

<td>
\~400 Mbps

</td>
</tr>

<tr align="left" valign="top">

<td>
**Extra groot**

</td>

<td>
8 x 1,6 GHz

</td>

<td>
14 GB

</td>

<td>
2TB

</td>

<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
U worden in rekening gebracht per uur voor elke VM die u gebruikt als een rolinstantie en er zijn ook in rekening gebracht voor gegevens die uw rolinstanties buiten het datacenter verzendt. U bent niet in rekening gebracht gegevens invoeren van het datacenter. Zie [prijzen van Azure] [prijzen van Azure] voor meer informatie. In het algemeen is het aangeraden te veel exemplaren van kleine rol in plaats van enkele grote exemplaren gebruiken zodat uw toepassing beter bestand tegen is mislukt is. Nadat alle de minder rolinstanties die u hebt, de meer rampzalig zijn een storing in een van beide is voor uw hele toepassing. Zoals al eerder vermeld, moet u ten minste twee exemplaren voor elke rol implementeren om op te halen van 99,95% service level agreement die Microsoft biedt.

Het servicedefinitiebestand (CSDEF) is ook waarin u veel kenmerken over elke rol in uw toepassing wilt opgeven. Hier volgen enkele van de items nuttiger zijn voor u beschikbaar:

* **Certificaten**. U kunt certificaten gebruiken voor het versleutelen van gegevens of als uw webservice SSL ondersteunt. Alle certificaten moeten worden geüpload naar Azure. Zie voor meer informatie [certificaten beheren in Azure][Managing Certificates in Azure]. Deze instelling XML installeert eerder hebt geüpload certificaten in het certificaatarchief van de rolinstantie zodat deze kan worden gebruikt door de toepassingscode.
* **Namen van de configuratie-instelling**. Voor waarden die u wilt dat uw toepassingen bij het uitvoeren op een rolinstantie lezen. De werkelijke waarde van de configuratie-instellingen is ingesteld in het serviceconfiguratiebestand (CSCFG) die op elk gewenst moment kan worden bijgewerkt zonder dat u uw code te implementeren. In feite kunt u de code van uw toepassingen zodanig voor het detecteren van de configuratiewaarden van gewijzigde zonder enige uitvaltijd.
* **Voer de eindpunten**. Hier geeft u geen HTTP-, HTTPS- of TCP-eindpunten (met poorten) die u wilt weergeven met de buitenwereld via uw *voorvoegsel*. cloadapp.net URL. Wanneer Azure uw rol implementeert, wordt deze automatisch de firewall configureren op de instantie.
* **Interne eindpunten**. Hier geeft u geen HTTP of TCP-eindpunten die u wilt blootgesteld aan andere rolinstanties die worden geïmplementeerd als onderdeel van uw toepassing. Interne eindpunten toestaan van alle rolinstanties in uw toepassing om te communiceren met elkaar, maar zijn niet toegankelijk voor alle rolinstanties die zich buiten uw toepassing.
* **Importeren van Modules**. Deze installeren desgewenst nuttig onderdelen op uw rolinstanties. Onderdelen bestaan voor diagnostische bewaking, extern bureaublad en Azure Connect (waarmee uw rolexemplaar voor toegang tot lokale bronnen via een beveiligd kanaal).
* **Lokale opslag**. Er wordt een submap op de instantie voor uw toepassing te gebruiken. Het wordt uitgebreid beschreven in de [Data-oplossingen voor opslag in Azure] [ Data Storage Offerings in Azure] artikel.
* **Starten van de taken**. Starten van de taken bieden u een manier om de vereiste onderdelen installeren op een rolinstantie als deze wordt opgestart. De taken kunnen uitvoeren met verhoogde bevoegdheden als beheerder indien nodig.

## <a id="cfg"></a>Het serviceconfiguratiebestand
De service-configuratiebestand (CSCFG) is een XML-bestand worden instellingen die kunnen worden gewijzigd beschreven zonder de toepassing opnieuw te implementeren. Het volledige schema voor het XML-bestand vindt u hier: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][http://msdn.microsoft.com/library/windowsazure/ee758710.aspx].
Het CSCFG-bestand bevat een element van de rol voor elke rol in uw toepassing. Hier volgen enkele van de items die u in het CSCFG-bestand opgeven kunt:

* **De versie van besturingssysteem**. Dit kenmerk kunt u het besturingssysteem (OS) versie gewenste gebruikt voor alle rolinstanties met uw toepassingscode te selecteren. Dit besturingssysteem wordt ook wel de *Gast OS*, en elke nieuwe versie bevat de meest recente beveiligingspatches en updates die beschikbaar zijn op het moment dat het gastbesturingssysteem wordt vrijgegeven. Als u de waarde van het osVersion-kenmerk ingesteld op '\*', en vervolgens het gastbesturingssysteem op elk van uw rolinstanties Azure automatisch bijgewerkt wanneer er nieuwe versies van de Gast OS beschikbaar komen. U kunt echter de automatische updates afmelden door het selecteren van een specifieke Gast OS-versie. Het kenmerk bijvoorbeeld instelt op een waarde van ' WA-GUEST-OS-2.8\_201109-01 ' zorgt ervoor dat uw rolinstanties ophalen wat beschreven op deze webpagina is: [http://msdn.microsoft.com/library/hh560567.aspx] [http://msdn.microsoft.com/library/hh560567.aspx]. Zie voor meer informatie over Gast OS-versies, [beheren Upgrades naar de Azure gasten OS].
* **Exemplaren**. Dit element waarde geeft het aantal rolinstanties die u wilt dat ingericht met de code voor een bepaalde rol. Omdat u kunt een nieuw CSCFG-bestand uploaden naar Azure (zonder dat uw toepassing), is het gebruikelijke manier eenvoudige Wijzig de waarde voor dit element en het uploaden van een nieuw CSCFG-bestand dynamisch vergroten of verkleinen het aantal rolinstanties uw toepassingscode wordt uitgevoerd . Hiermee kunt u uw toepassing eenvoudig omhoog of omlaag naar de eisen voldoen aan werkelijke werkbelasting terwijl u ook bedient hoeveel u in rekening gebracht voor het uitvoeren van de rolinstanties schalen.
* **Configuratie-instelling waarden**. Dit element Hiermee geeft u waarden voor instellingen (zoals gedefinieerd in het CSDEF-bestand). Uw rol kan deze waarden lezen terwijl deze wordt uitgevoerd. De waarden van deze configuratie-instellingen worden doorgaans gebruikt voor verbindingsreeksen op SQL-Database of op Azure-opslag, maar ze kunnen worden gebruikt voor andere doeleinden die u wenst.

## <a id="hostedservices"></a>Maken en implementeren van een gehoste Service
Maken van een gehoste service vereist dat u eerst gaat u naar de [Azure Management Portal] en inrichten van een gehoste service door een DNS-voorvoegsel en de gegevens te geven center u uiteindelijk wilt dat uw code die wordt uitgevoerd. Klik in uw ontwikkelomgeving maken van uw service-definitiebestand (CSDEF), bouwen van uw toepassingscode en het pakket (zip) deze alle bestanden in een service-pakketbestand (CSPKG). Ook moet u uw service-configuratiebestand (CSCFG) voorbereiden. Voor het implementeren van uw rol, moet u de CSPKG- en CSCFG-bestanden met de Azure Service Management API uploaden. Zodra geïmplementeerd, Azure, wordt de rolinstanties inrichten in het datacenter (op basis van de configuratiegegevens), uw toepassingscode extraheren uit het pakket, kopiëren naar de rolexemplaren en opstarten van de exemplaren. Uw code is nu actief.

De afbeelding hieronder toont de CSPKG- en CSCFG-bestanden die u op uw ontwikkelcomputer maakt. Het bestand CSPKG bevat het CSDEF-bestand en de code voor twee rollen. Na het uploaden van de CSPKG- en CSCFG-bestanden met de Azure Service Management-API maakt Azure de rolinstanties in het datacenter. In dit voorbeeld wordt het CSCFG-bestand aangegeven dat Azure moet worden gebruikt voor het maken van drie exemplaren van rol \#1 en twee exemplaren van rol \#2.

![Afbeelding][5]

Zie voor meer informatie over het implementeren van, upgraden en opnieuw configureren van uw rollen de [implementeren en bijwerken van Azure-toepassingen] [ Deploying and Updating Azure Applications] artikel.<a id="Ref" name="Ref"></a>

## <a id="references"></a>Verwijzingen
* [Maken van een gehoste Service voor Azure][Creating a Hosted Service for Azure]
* [Gehoste Services in Azure beheren][Managing Hosted Services in Azure]
* [Migreren van toepassingen naar Azure][Migrating Applications to Azure]
* [Configureren van een Azure-toepassing][Configuring an Azure Application]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Geschreven Jeffrey Richter (Wintellect)</p>

</div>

[Azure Application Model Benefits]: #benefits
[Hosted Service Core Concepts]: #concepts
[Hosted Service Design Considerations]: #considerations
[Designing your Application for Scale]: #scale
[Hosted Service Definition and Configuration]: #defandcfg
[The Service Definition File]: #def
[The Service Configuration File]: #cfg
[Creating and Deploying a Hosted Service]: #hostedservices
[References]: #references
[0]: ./media/application-model/application-model-3.jpg
[1]: ./media/application-model/application-model-4.jpg
[2]: ./media/application-model/application-model-5.jpg
[Configuring a Custom Domain Name in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
[Data Storage Offerings in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
[3]: ./media/application-model/application-model-6.jpg
[4]: ./media/application-model/application-model-7.jpg

[Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
[Managing Certificates in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
[beheren Upgrades naar de Azure gasten OS]: http://msdn.microsoft.com/library/ee924680.aspx
[Azure Management Portal]: http://manage.windowsazure.com/
[5]: ./media/application-model/application-model-8.jpg
[Deploying and Updating Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
[Creating a Hosted Service for Azure]: http://msdn.microsoft.com/library/gg432967.aspx
[Managing Hosted Services in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
[Migrating Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
[Configuring an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
