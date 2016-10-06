<properties
   pageTitle="Plannings- en bedieningsgids voor het Beveiligingscentrum | Microsoft Azure"
   description="Dit document helpt u met uw planning voordat u overstapt op het Azure Beveiligingscentrum en bevat aandachtspunten voor het dagelijks gebruik."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>


# Plannings- en bedieningsgids voor Azure Security Center
Deze gids is bedoeld voor IT-specialisten, IT-architecten, gegevensbeveiligingsanalisten en cloudbeheerders die willen gaan werken met Azure Security Center.

## Planningsgids
Deze gids bevat een reeks stappen en taken die u kunt volgen om uw gebruik van Azure Security Center te optimaliseren op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie. Als u optimaal wilt profiteren van Security Center, is het belangrijk te begrijpen hoe verschillende personen of teams binnen uw organisatie de service gaan gebruiken om te voldoen aan alle vereisten voor veilige ontwikkeling en gebruik, bewaking, bestuur en reactie op incidenten. De belangrijkste gebieden waarmee u rekening moet houden bij het plannen van het gebruik van Security Center zijn:

- Beveiligingsrollen en toegangsbeheer
- Beveiligingsbeleid en aanbevelingen
- Gegevensverzameling en -opslag
- Continue beveiligingsbewaking
- Reageren op incidenten

In de volgende sectie leert u hoe u voor elk van deze gebieden een planning maakt en hoe u deze aanbevelingen toepast op grond van uw vereisten.

> [AZURE.NOTE] Raadpleeg de [Azure Security Center frequently asked questions (Veelgestelde vragen over het Azure Beveiligingscentrum)](security-center-faq.md) voor een lijst met veelgestelde vragen die ook nuttig kunnen zijn tijdens de ontwerp- en planningsfase.


## Beveiligingsrollen en toegangsbeheer
Afhankelijk van de grootte en de structuur van uw organisatie kunnen meerdere individuen en teams het Beveiligingscentrum gebruiken om verschillende beveiligingstaken uit te voeren. In het volgende diagram vindt u een voorbeeld met fictieve personen en hun respectieve rollen en beveiligingsverantwoordelijkheden:

![Rollen](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-ga.png)

Met Security Center kunnen deze personen voldoen aan deze verschillende verantwoordelijkheden. Bijvoorbeeld:

**Jeff (eigenaar van workloads in de cloud)**

- Een cloud-workload en de bijbehorende resources beheren
- Verantwoordelijk voor het implementeren en onderhouden van beveiliging volgens het beveiligingsbeleid van het bedrijf

**Ellen (CISO/CIO)**

- Verantwoordelijk voor alle beveiligingsaspecten van het bedrijf
- Wil de beveiliging van het bedrijf in alle cloud-workloads begrijpen
- Moet worden geïnformeerd over belangrijke aanvallen en risico 's

**David (IT-beveiliging)**

- Stelt beveiligingsbeleid in om er zeker van te zijn dat de juiste beveiliging aanwezig is
- Bewaakt naleving van beleid
- Genereert rapporten voor leidinggevenden of auditors

**Judy (beveiligingsbewerkingen)**

- Bewaakt en reageert 24/7 op beveiligingswaarschuwingen
- Escaleert naar eigenaar cloud-workload of IT-beveiligingsanalist

**Sam (beveiligingsanalist)**

- Onderzoekt aanvallen
- Lost waarschuwingen op of werkt samen met eigenaar cloud-workload om herstelstappen toe te passen 


Security Center gebruikt [op rollen gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md), dat [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md) biedt die kunnen worden toegewezen aan gebruikers, groepen en services in Azure. Wanneer gebruikers Security Center openen, zien ze alleen informatie met betrekking tot resources waartoe ze toegang hebben. Dit betekent dat aan de gebruiker de rol van de eigenaar, bijdrager of lezer is toegewezen voor het abonnement of de resourcegroep waartoe een resource behoort. Wanneer gebruik wordt gemaakt van de personen in het vorige diagram, zou de volgende RBAC nodig zijn:

**Jeff (eigenaar van workloads in de cloud)**

- Resourcegroepeigenaar/medewerker

**David (IT-beveiliging)**

- Abonnementseigenaar/medewerker

**Judy (beveiligingsbewerkingen)**

- Abonnementslezer voor het bekijken van waarschuwingen
- Abonnementseigenaar/medewerker die nodig is om waarschuwingen te verwijderen

**Sam (beveiligingsanalist)**

- Abonnementslezer voor het bekijken van waarschuwingen
- Abonnementseigenaar/medewerker die nodig is om waarschuwingen op te lossen of te verwijderen
- Toegang tot opslag is mogelijk vereist

Andere belangrijke informatie om rekening mee te houden:

- Alleen abonnementseigenaren en bijdragers kunnen een beveiligingsbeleid bewerken
- Alleen abonnements- en resourcegroepeigenaren en bijdragers kunnen beveiligingsaanbevelingen voor een resource toepassen

Bij het plannen van het toegangsbeheer met het RBAC voor Security Center moet u zeker weten wie in uw organisatie het Security Center gaat gebruiken. Ook moet u weten welke typen taken zij gaan uitvoeren en RBAC vervolgens dienovereenkomstig configureren.

> [AZURE.NOTE] We raden u aan de rol toe te wijzen die gebruikers minimaal nodig hebben om hun taken uit te voeren. Wijs bijvoorbeeld gebruikers die alleen informatie hoeven te bekijken over de beveiligingstoestand van resources, maar geen maatregelen hoeven te nemen zoals het toepassen van aanbevelingen of het bewerken van beleid, de rol Lezer toe.

## Beveiligingsbeleid en aanbevelingen
Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement of de opgegeven resourcegroep. In Security Center definieert u de beleidsregels op grond van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens.

Beleidsregels die zijn ingeschakeld op abonnementsniveau, worden automatisch doorgegeven naar alle resourcegroepen binnen het abonnement, zoals weergegeven in het volgende diagram:

![Beveiligingsbeleid](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-ga.png)

Zoals weergegeven in de afbeelding hierboven, kan beveiligingsbeleid voor resourcegroepen worden overgenomen van het abonnementsniveau.

In sommige scenario's waarin u resources in een resourcegroep hebt die een andere set beleidsregels vereisen, kunt u overname uitschakelen en aangepaste beleidsregels toepassen op een specifieke resourcegroep.

Als u aangepaste beleidsregels in bepaalde resourcegroepen nodig hebt, moet u overname in de resourcegroep uitschakelen en de beveiligingsbeleidsregels wijzigen. Als u bijvoorbeeld een paar workloads hebt waarvoor het beleid voor SQL Transparent Data Encryption niet nodig is, schakelt u het beleid uit op het abonnementsniveau en schakelt u het alleen in binnen de resourcegroepen waar SQL TDE vereist is.

Wanneer u begint met het maken van aangepaste beleidsregels voor verschillende resourcegroepen, moet u uw beleidsimplementatie plannen in de wetenschap dat bij een beleidsconflict (abonnement versus resourcegroep) het resourcegroepbeleid voorrang heeft.

> [AZURE.NOTE] Als u moet controleren welke beleidsregels zijn gewijzigd, kunt u dat doen met de [Azure controlelogboeken](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Beleidswijzigingen worden altijd geregistreerd in Azure controlelogboeken.

### Aanbevelingen voor beveiliging

Voordat u beleidsregels voor veiligheid configureert, moet u elk van de [aanbevelingen voor beveiliging](security-center-recommendations.md) controleren en bepalen of deze beleidsregels geschikt zijn voor uw verschillende abonnementen en resourcegroepen. Het is ook belangrijk te begrijpen welke actie wordt ondernomen om de aanbevelingen voor beveiliging toe te passen.

**Eindpuntbeveiliging**: als er op een virtuele machine geen oplossing voor eindpuntbeveiliging is ingeschakeld, wordt in Security Center aangeraden er een te installeren. Als u de voorkeur geeft aan een oplossing voor eindpuntbeveiliging die u on-premises al hebt doorgevoerd, moet u besluiten of u dezelfde antimalware voor uw Azure VM’s gaat gebruiken. Security Center biedt verschillende opties voor eindpuntbeveiliging.  U kunt de gratis Microsoft Antimalware gebruiken of u kunt kiezen uit een lijst met oplossingen voor eindpuntbeveiliging van geïntegreerde partners. Lees het artikel [Eindpuntbeveiliging inschakelen in Azure Security Center](security-center-install-endpoint-protection.md) voor meer informatie over het implementeren van antimalware met behulp van Security Center.

**Systeemupdates**: Security Center identificeert virtuele machines waarvoor beveiligings- of kritieke besturingssysteemupdates ontbreken voor IaaS- en cloudservices (PaaS). Bedenk wie verantwoordelijk is voor het toepassen van updates wanneer deze nodig zijn en de manier waarop ze worden toegepast. Veel organisaties gebruiken WSUS, Windows Update of een ander hulpprogramma.

**Basislijnconfiguraties**: als besturingssysteemconfiguraties van virtuele machines niet overeenkomen met de aanbevolen basislijnen, wordt een aanbeveling opgehaald. Controleer de set basislijnen [hier](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) en bekijk hoe besturingssysteemconfiguraties worden toegepast.

**Schijfversleuteling**: als u schijven voor virtuele machines hebt die niet zijn versleuteld, raadt Security Center aan om Azure Disk Encryption toe te passen. Deze functie maakt gebruik van BitLocker voor Windows en DM-Crypt voor Linux om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. Via deze aanbeveling wordt u doorgestuurd naar een [handleiding](security-center-disk-encryption.md) met stap-voor-stap-instructies om deze versleuteling uit te voeren.

Let op: er zijn verschillende versleutelingsscenario's die u moet toepassen. U moet een planning maken voor de unieke vereisten van elk van deze scenario's:

- Versleuteling van nieuwe Azure Virtual Machines vanaf VHD's die u hebt versleuteld met behulp van uw eigen versleutelingssleutels
- Versleuteling van nieuwe Azure Virtual Machines die zijn gemaakt op basis van de Azure-galerie
- Versleuteling van Azure Virtual Machines die al onder Azure worden uitgevoerd

De planningsvereisten zijn verschillend voor elk van deze scenario's. Raadpleeg het [whitepaper Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) voor meer informatie over elk van deze scenario's.

**Web Application Firewall**: in Security Center worden virtuele machines geïdentificeerd waarop webtoepassingen worden uitgevoerd, en wordt aangeraden een Web Application Firewall (WAF) te installeren. Evalueer de beschikbare partneroplossingen om te bepalen welke het meest geschikt is voor uw organisatie en bepaal hoe de oplossing in licentie wordt gegeven (partners ondersteunen mogelijk Bring Your Own License en/of 'pay-as-you-go'-modellen). Lees het artikel [Een firewall voor een webtoepassing toevoegen in Azure Security Center](security-center-add-web-application-firewall.md) voor meer informatie over het implementeren van een firewall voor een webtoepassing in uw Azure VM's met behulp van Azure Security Center.

**Next Generation Firewall**: hiermee kunt u een virtueel apparaat van toonaangevende leveranciers installeren, met inbegrip van Check Point en binnenkort ook Cisco en Fortinet. Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor Next Generation Firewall wordt aanbevolen en biedt u de mogelijkheid een virtuele toepassing in te richten.

**Virtuele netwerken**: in Security Center worden de infrastructuur en configuratie van uw [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) beoordeeld om te controleren of [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) zijn toegepast en correct zijn geconfigureerd met regels voor binnenkomend verkeer. U moet bepalen welke verkeersregels moeten worden gedefinieerd en dit doorgeven aan de personen die de verwante beveiligingsaanbevelingen moeten toepassen.

Security Center raadt u aan om voor de beveiliging contactgegevens voor uw Azure-abonnement te verstrekken. Deze informatie wordt gebruikt door Microsoft om contact met u op te nemen als door Microsoft Security Response Center (MSRC) wordt gedetecteerd dat uw klantgegevens zijn geopend door een illegale of niet-geautoriseerde derde. Lees [Contactgegevens voor beveiliging verstrekken in Azure Security Center](security-center-provide-security-contact-details.md) voor meer informatie over het inschakelen van deze mogelijkheid.

## Gegevensverzameling en -opslag

We raden nadrukkelijk aan om gegevensverzameling in te schakelen voor elk van uw abonnementen, omdat dit ervoor zorgt dat beveiligingsbewaking beschikbaar is voor alle virtuele machines. Gegevensverzameling wordt ingeschakeld via de Azure Monitoring Agent (ASMAgentLauncher.exe) en de extensie Azure Security Monitoring (ASMMonitoringAgent.exe).

De extensie Azure Security Monitoring scant op verschillende, voor beveiliging relevante configuraties en verzamelt beveiligingslogboeken vanaf de virtuele machine. Deze gegevens worden verzonden naar een opslagaccount die u opgeeft. De scanmanager (ASMSoftwareScanner.exe) wordt ook geïnstalleerd op de virtuele machine en wordt gebruikt als een patchscanner.

Nadat gegevensverzameling is ingeschakeld in het beveiligingsbeleid, worden de bewakingsagent en extensies automatisch geïnstalleerd op alle bestaande en nieuw ondersteunde virtuele machines die zijn ingericht in Azure.  Het agentproces is niet-invasief en heeft geen invloed op de prestaties van de VM.

> [AZURE.NOTE] Lees de [Gids voor het oplossen van problemen met Azure Security Center](security-center-troubleshooting-guide.md) om problemen met de Azure Security Monitoring Agent op te lossen.

Als u gegevensverzameling op een bepaald moment wilt uitschakelen, kunt u dat doen in het beveiligingsbeleid. Selecteer de menuoptie Agents verwijderen om eerder geïmplementeerde bewakingsagents te verwijderen.

> [AZURE.NOTE] Lees de [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) als u een lijst met ondersteunde virtuele machines nodig hebt.

Voor elke regio waarin u virtuele machines uitvoert, kiest u de opslagaccount waarin de gegevens worden opgeslagen die zijn verzameld bij deze virtuele machines. Als u geen opslagaccount voor elke regio kiest, wordt er een voor u gemaakt. U kunt de opslaglocatie per regio kiezen of alle gegevens opslaan op een centrale locatie. Hoewel het beveiligingsbeleid kan worden ingesteld op het niveau van het Azure-abonnement en op dat van de resourcegroep, kan de regio voor uw opslagaccount alleen worden geselecteerd op abonnementsniveau.

Als u een opslagaccount gebruikt die wordt gedeeld tussen verschillende Azure-resources, lees dan het artikel [Azure Storage Scalability and Performance Targets](../storage/storage-scalability-targets.md) voor meer informatie over formaatlimieten en beperkingen. Uw abonnement heeft ook limieten voor de opslagaccount. Bekijk [Limieten, quota’s en beperkingen voor Azure-abonnementen en services](../azure-subscription-service-limits.md) voor meer informatie over deze limieten.

> [AZURE.NOTE] Kosten voor deze opslag zijn niet inbegrepen bij de prijs van de service voor Security Center en worden afzonderlijk in rekening gebracht tegen de gebruikelijke [tarieven voor Azure-opslag](https://azure.microsoft.com/pricing/details/storage/).

Prestatie- en schaalbaarheidsoverwegingen moeten ook worden gepland op grond van de grootte van uw Azure-omgeving en de resources die uw opslagaccount gebruiken. Raadpleeg de [Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../storage/storage-performance-checklist.md) voor meer informatie.

## Continue beveiligingsbewaking

Na de eerste configuratie en toepassing van aanbevelingen voor Security Center, komen in de volgende stap de operationele processen voor Security Center ter sprake.

Als u vanuit de Azure Portal naar Security Center wilt gaan, klikt u op **Bladeren** en typt u **Beveiligingscentrum** in het veld **Filter**. De gebruiker ziet de gefilterde resultaten.

Security Center heeft geen invloed op uw normale operationele procedures. Het bewaakt uw implementaties passief en doet aanbevelingen op grond van het beveiligingsbeleid dat u hebt ingeschakeld.

Het dashboard van Security Center bestaat uit twee hoofdonderdelen:

- Preventie
- Detectie

Wanneer u gegevensverzameling in Security Center voor het eerst inschakelt voor uw huidige Azure-omgeving, moet u alle aanbevelingen doornemen. Dit kunt u doen op de blade **Aanbevelingen** of per resource (**Virtuele Machine**, **Netwerken**, **SQL** en **Toepassing**).

Zodra u alle aanbevelingen hebt toegepast, wordt de sectie **Preventie** groen voor alle toegepaste resources. Continue bewaking wordt op dit punt eenvoudiger omdat u alleen acties onderneemt op basis van wijzigingen in de resourcebeveiligingsstatus en aanbevelingstegels.

De sectie **Detectie** is meer reactief en betreft waarschuwingen over problemen die nu plaatsvinden of hebben plaatsgevonden in het verleden en zijn gedetecteerd bij controles door Security Center of door systemen van derden. Met de tegel Beveiligingswaarschuwingen wordt een staafdiagram weergegeven dat het aantal dagelijks geconstateerde waarschuwingen voor gedetecteerde bedreigingen en de verdeling ervan over verschillende ernstigheidscategorieën (laag, gemiddeld, hoog) aangeeft. Lees voor meer informatie over beveiligingswaarschuwingen [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).

> [AZURE.NOTE] U kunt uw gegevens uit Security Center ook visualiseren met Microsoft Power BI. Lees [Managing and responding to security alerts in Azure Security Center](security-center-powerbi.md).

### Bewaking voor nieuwe of gewijzigde resources

De meeste Azure-omgevingen zijn dynamisch, met nieuwe resources die regelmatig omhoog en omlaag worden verplaatst, met configuraties of wijzigingen enz. Security Center zorgt ervoor dat de beveiligingsstatus van deze nieuwe resources voor u goed zichtbaar is.

Wanneer u nieuwe resources (virtuele machines, SQL Databases) toevoegt aan uw Azure-omgeving, detecteert Security Center deze resources automatisch en begint het de beveiliging ervan te bewaken. Dit omvat ook PaaS-webrollen en -werkrollen. Als gegevensverzameling wordt ingeschakeld in het [beveiligingsbeleid](security-center-policies.md), worden er automatisch extra bewakingsmogelijkheden ingeschakeld voor uw virtuele machines.

![Belangrijke gebieden](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-ga.png)

1.  Ga voor virtuele machines naar de tegel **Resourcebeveiligingsstatus** en klik op **Virtuele Machines**. Problemen bij het inschakelen van gegevensverzameling of verwante aanbevelingen worden zichtbaar in de sectie **Aanbevelingen voor bewaking**.
2.  Geef de **aanbevelingen** weer om te zien welke beveiligingsrisico's mogelijk zijn geïdentificeerd voor de nieuwe resource.
3.  Het is heel gebruikelijk dat wanneer nieuwe virtuele machines worden toegevoegd aan uw omgeving, eerst alleen het besturingssysteem wordt geïnstalleerd. De resource-eigenaar heeft wellicht enige tijd nodig om andere apps te implementeren die door deze VM’s worden gebruikt.  In het ideale geval moet u de uiteindelijke doelstelling van deze workload kennen. Wordt het een toepassingsserver? Op grond van wat deze nieuwe workload gaat worden, kunt u het juiste **beveiligingsbeleid** inschakelen. Dit is de derde stap in deze werkstroom.
4.  Naarmate nieuwe resources worden toegevoegd aan uw Azure-omgeving, kunnen er nieuwe waarschuwingen worden weergegeven in de tegel **Beveiligingswaarschuwingen**. Controleer altijd of er nieuwe waarschuwingen in deze tegel worden weergegeven en neem maatregelen op grond van de aanbevelingen van Security Center.

U zult ook regelmatig de status van bestaande resources willen controleren om configuratiewijzigingen te identificeren die hebben geleid tot beveiligingsrisico's, afwijking van aanbevolen basislijnen en beveiligingswaarschuwingen. Begin bij het dashboard van Security Center. Van daaruit hebt u drie belangrijke gebieden die u consequent moet controleren.

![Bewerkingen](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png)

1.  Het venster **Recourcebeveiligingsstatus** biedt snel toegang tot uw belangrijkste resources. Gebruik deze optie om uw virtuele machines, netwerken, SQL en toepassingen te bewaken.
2.  In het venster **Aanbevelingen** kunt u de aanbevelingen van Security Center controleren. Tijdens de continue bewaking merkt u misschien dat u niet dagelijks aanbevelingen krijgt. Dit is normaal omdat u alle aanbevelingen hebt gevolgd bij de eerste configuratie van Security Center. Daarom bevat deze sectie niet elke dag nieuwe informatie en hoeft u deze alleen te raadplegen wanneer dat nodig is.
3.  Het paneel **Detectie** kan ofwel heel vaak, of zeer incidenteel veranderen. Controleer altijd uw beveiligingswaarschuwingen en neem maatregelen op grond van de aanbevelingen van Security Center.

## Reageren op incidenten

Security Center detecteert en waarschuwt voor bedreigingen wanneer deze optreden. Organisaties moeten controleren op nieuwe beveiligingswaarschuwingen en zo nodig maatregelen nemen om het probleem verder te onderzoeken of de aanval af te weren. Lees [Detectiemogelijkheden van Azure Security](security-center-detection-capabilities.md) voor meer informatie over de werking van detectie van bedreigingen van Azure Security Center.

Hoewel dit artikel niet de bedoeling heeft om u te helpen uw eigen plan voor het reageren op incidenten te maken, maken we gebruik van Microsoft Azure Security Response in de cloud-levenscyclus als de basis voor fasen voor het reageren op incidenten. In het volgende diagram ziet u de fasen:

![Verdachte activiteit](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [AZURE.NOTE] U kunt de [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) van het National Institute of Standards and Technology (NIST) gebruiken als richtlijn om uw eigen plan te ontwikkelen.

U kunt waarschuwingen van Security Center gebruiken tijdens de volgende fasen:

- **Detecteren**: een verdachte activiteit in een of meer resources identificeren. 
- **Beoordelen**: de eerste beoordeling uitvoeren voor meer informatie over de verdachte activiteiten.
- **Diagnose uitvoeren**: de herstelstappen gebruiken voor het uitvoeren van de technische procedure om het probleem op te lossen.

Elke beveiligingswaarschuwing bevat informatie die kan worden gebruikt om beter te begrijpen wat de aard van de aanval is en om mogelijke oplossingen voor te stellen. Sommige waarschuwingen bevatten ook koppelingen naar meer informatie of naar andere informatiebronnen binnen Azure. U kunt de verschafte informatie gebruiken om verder onderzoek te doen en om te beginnen met risicobeperking.

Het volgende voorbeeld betreft een verdachte RDP-activiteit die op dat moment plaatsvindt:

![Verdachte activiteit](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Zoals u ziet, bevat deze blade informatie over het tijdstip waarop de aanval plaatsvond, de hostnaam van de bron, de VM die het doelwit was en aanbevolen maatregelen. Onder bepaalde omstandigheden kunnen de brongegevens van de aanval leeg zijn. Lees [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) voor meer informatie over dit type gedrag.

In de video [Reageren op incidenten met Azure Security Center & Microsoft Operations Management Suite](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) ziet u enkele demonstraties waarmee u kunt begrijpen hoe Security Center kan worden gebruikt in elk van deze fasen.

> [AZURE.NOTE] Lees [Azure Security Center gebruiken voor reacties op incidenten](security-center-incident-response.md) voor meer informatie over hoe u Security Center kunt gebruiken wanneer u reageert op incidenten. 

## Zie ook
In dit document hebt u kunnen lezen hoe u een planning kunt maken voor het overstappen op Security Center. Zie de volgende onderwerpen voor meer informatie over Security Center:

- [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
- [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
- [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
- [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.



<!--HONumber=Sep16_HO4-->


