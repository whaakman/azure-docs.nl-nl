---
title: Verbinding maken met computers met behulp van de Log Analytics-gateway | Microsoft Docs
description: Verbinding maken met uw apparaten en computers met Operations Manager bewaakt met behulp van de gateway Log Analytics om gegevens te verzenden naar de Azure Automation en Log Analytics-service als ze geen toegang tot internet hebben.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: magoedte
ms.openlocfilehash: 81005c2c95c9cccb32796d1afca4208f5ff8b919
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437336"
---
# <a name="connect-computers-without-internet-access-by-using-the-log-analytics-gateway-in-azure-monitor"></a>Verbinding maken met computers zonder toegang tot het internet met behulp van de Log Analytics-gateway in Azure Monitor

>[!NOTE]
>Als Microsoft Operations Management Suite (OMS) in Microsoft Azure Monitor verandert, wordt terminologie gewijzigd. In dit artikel verwijst naar de OMS-Gateway als de Azure Log Analytics-gateway. 
>

In dit artikel wordt beschreven hoe u communicatie met Azure Automation en Azure Monitor configureren met behulp van de Log Analytics-gateway wanneer de computers die rechtstreeks zijn verbonden of die worden bewaakt door Operations Manager hebben geen toegang tot internet. 

De Log Analytics-gateway is een forward HTTP-proxy die ondersteuning biedt voor HTTP-tunneling met de opdracht HTTP-verbinding. Deze gateway kan gegevens verzamelen en te verzenden naar Azure Automation en Log Analytics-werkruimte in Azure Monitor namens de computers die niet zijn verbonden met internet.  

De Log Analytics-gateway ondersteunt:

* Rapportage tot de dezelfde vier Log Analytics werkruimte agents die erachter zijn en die zijn geconfigureerd met Azure Automation Hybrid Runbook Workers.  
* Windows-computers waarop de Microsoft Monitoring Agent rechtstreeks is verbonden met een Log Analytics-werkruimte in Azure Monitor.
* Linux-computers waarop een Log Analytics-agent voor Linux rechtstreeks is verbonden met een Log Analytics-werkruimte in Azure Monitor.  
* System Center Operations Manager 2012 SP1 UR7, Operations Manager 2012 R2 met UR3 of een beheergroep in Operations Manager 2016 of hoger die is geïntegreerd met Log Analytics.  

Sommige het beveiligingsbeleid van verbinding met internet voor computers in het netwerk niet is toegestaan. Deze niet-verbonden computers mogelijk punt van apparaten verkooppunten (POS) of de servers die ondersteuning bieden IT-services, bijvoorbeeld. Als u wilt deze apparaten verbinden met Azure Automation of een Log Analytics-werkruimte, zodat u kunt beheren en bewaken configureren, zodat deze communiceren rechtstreeks met de Log Analytics-gateway. De Log Analytics-gateway kan ontvangen configuratie-informatie en doorsturen van gegevens in hun naam. Als de computers zijn geconfigureerd met de Log Analytics-agent rechtstreeks verbinding maken met een Log Analytics-werkruimte, worden de computers in plaats daarvan communiceren met de Log Analytics-gateway.  

De Log Analytics-gateway gegevens worden overgebracht van de agents met de service rechtstreeks. Deze een van de gegevens die onderweg zijn niet geanalyseerd.

Wanneer een Operations Manager-beheergroep is geïntegreerd met Log Analytics, worden de beheerservers geconfigureerd om te verbinden met de Log Analytics-gateway naar configuratie-informatie ontvangen en verzenden van verzamelde gegevens, afhankelijk van de oplossing die u hebt ingeschakeld .  Operations Manager-agents verzenden gegevens naar de beheerserver. Voor agents worden verzonden, Operations Manager waarschuwingen, configuratie-evaluatiegegevens ruimte Instantiegegevens en capaciteitsgegevens. Andere gegevens van hoge volumes, zoals Internet Information Services (IIS)-Logboeken, prestatiegegevens en beveiligingsgebeurtenissen, wordt rechtstreeks naar de gateway voor Log Analytics verzonden. 

Als een of meer Operations Manager-Gateway-servers worden geïmplementeerd voor het controleren van niet-vertrouwde systemen in een perimeternetwerk of in een geïsoleerd netwerk, worden deze servers kunnen niet communiceren met een Log Analytics-gateway.  Operations Manager-gatewayservers kunnen alleen met een beheerserver rapporteren.  Wanneer een Operations Manager-beheergroep is geconfigureerd om te communiceren met de Log Analytics-gateway, wordt de proxy-configuratie-informatie automatisch gedistribueerd naar elke agent beheerde computer die is geconfigureerd voor het verzamelen van gegevens aan het logboek voor Azure Monitor zelfs als de instelling leeg is.    

Voor maximale beschikbaarheid voor direct verbonden of bewerkingen beheergroepen die communiceren met Log Analytics-werkruimte via de gateway gebruiken voor netwerktaakverdeling (NLB) om te leiden en distribueren van verkeer tussen meerdere gatewayservers. Op die manier als een gatewayserver uitvalt, wordt het verkeer doorgestuurd naar een ander beschikbaar knooppunt.  

De Log Analytics-Windows-agent voor het identificeren van de service-eindpunten die de gateway nodig heeft om te communiceren met vereist de computer met de Log Analytics-gateway De agent moet ook om te leiden van de gateway om te rapporteren aan de dezelfde werkruimten die de agents of Operations Manager-beheergroep achter de gateway zijn geconfigureerd met. Deze configuratie kunt de gateway en de agent kan communiceren met hun toegewezen werkruimte.

Een gateway kan multihomed naar maximaal vier werkruimten zijn. Dit is het totale aantal werkruimten die een Windows-agent ondersteunt.  

Elke agent moet de netwerkverbinding met de gateway hebben, zodat agents kunnen automatisch worden overgedragen gegevens van en naar de gateway. Voorkomen dat u de gateway installeert op een domeincontroller.

Het volgende diagram toont gegevens die binnenkomen van directe agents, via de gateway, naar Azure Automation en Log Analytics. De configuratie van de agent-proxy moet overeenkomen met de poort die de Log Analytics-gateway is geconfigureerd met.  

![Diagram van directe agentcommunicatie met services](./media/gateway/oms-omsgateway-agentdirectconnect.png)

Het volgende diagram toont de gegevensstroom van een Operations Manager-beheergroep naar Log Analytics.   

![Diagram van de Operations Manager-communicatie met Log Analytics](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="set-up-your-system"></a>Instellen van uw systeem

Computers die worden uitgevoerd van de Log Analytics-gateway moeten beschikken over de volgende configuratie:

* Windows 10, Windows 8.1 of Windows 7
* WindowsServer 2016, Windows Server 2012 R2, WindowsServer 2012, Windows Server 2008 R2 of WindowsServer 2008
* Microsoft .NET Framework 4.5
* Ten minste een 4-core-processor en 8 GB geheugen 
* Een [Log Analytics-agent voor Windows](agent-windows.md) die is geconfigureerd om te rapporteren aan dezelfde werkruimte als de agents die via de gateway communiceren

### <a name="language-availability"></a>Taal-beschikbaarheid

De Log Analytics-gateway is beschikbaar in deze talen:

- Chinees (vereenvoudigd)
- Chinees (traditioneel)
- Tsjechisch
- Nederlands
- Nederlands
- Frans
- Duits
- Hongaars
- Italiaans
- Japans
- Koreaans
- Pools
- Portugees (Brazilië)
- Portugees (Portugal)
- Russisch
- Spaans (internationaal)

### <a name="supported-encryption-protocols"></a>Ondersteunde codering protocollen
De Log Analytics-gateway ondersteunt alleen Transport Layer Security (TLS) 1.0, 1.1 en 1.2.  Het biedt geen ondersteuning voor Secure Sockets Layer (SSL).  Om te controleren of de beveiliging van gegevens die onderweg zijn naar Log Analytics, configureert u de gateway te gebruiken ten minste TLS 1.2. Oudere versies van TLS of SSL zijn kwetsbaar. Hoewel ze toestaan op dit moment voor neerwaartse compatibiliteit, te voorkomen dat het gebruik ervan.  

Raadpleeg voor meer informatie, [verzenden van gegevens veilig gebruik TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

### <a name="supported-number-of-agent-connections"></a>Aantal ondersteunde agent-verbindingen
De volgende tabel ziet u ongeveer hoeveel agents kunnen communiceren met een gateway-server. Ondersteuning is gebaseerd op agents die ongeveer 200 KB aan gegevens om de 6 seconden uploaden. Gegevensvolume staat voor elke agent die wordt getest, ongeveer 2.7 GB per dag.

|Gateway |Agents ondersteund (bij benadering)|  
|--------|----------------------------------|  
|CPU: Intel Xeon Processor E5-2660 v3 \@ 2,6 GHz 2 kernen<br> Geheugen: 4 GB<br> Bandbreedte van het netwerk: 1 Gbps| 600|  
|CPU: Intel Xeon Processor E5-2660 v3 \@ 2,6 GHz 4 Cores<br> Geheugen: 8 GB<br> Bandbreedte van het netwerk: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>De Log Analytics-gateway downloaden

Download de nieuwste versie van het installatiebestand van de Log Analytics-gateway van een [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443) of de Azure-portal.

Als u de Log Analytics-gateway vanuit de Azure-portal, de volgende stappen uit:

1. Blader door de lijst met services en selecteer vervolgens **Log Analytics**. 
1. Selecteer een werkruimte.
1. In de blade van uw werkruimte onder **algemene**, selecteer **Quick Start**. 
1. Onder **kiest u een gegevensbron verbinding maken met de werkruimte**, selecteer **Computers**.
1. In de **Direct Agent** Selecteer **downloaden Log Analytics gateway**.
 
   ![Schermafbeelding van de stappen voor het downloaden van de Log Analytics-gateway](./media/gateway/download-gateway.png)

of 

1. In de blade van uw werkruimte onder **instellingen**, selecteer **geavanceerde instellingen**.
1. Ga naar **verbonden bronnen** > **Windows Servers** en selecteer **downloaden Log Analytics gateway**.

## <a name="install-the-log-analytics-gateway"></a>De Log Analytics-gateway installeren

Volg deze stappen voor het installeren van een gateway.  (Als u een eerdere versie aangeroepen van Log Analytics Forwarder hebt geïnstalleerd, er wordt een upgrade uitgevoerd naar deze versie.)

1. Dubbelklik in de doelmap op **Log Analytics gateway.msi**.
1. Op de pagina **Welkom** selecteert u **Volgende**.

   ![Schermafbeelding van de welkomstpagina van de wizard Setup voor Gateway](./media/gateway/gateway-wizard01.png)

1. Op de **License Agreement** weergeeft, schakelt **ik ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst** moet akkoord gaan met de licentievoorwaarden voor Microsoft-Software, en selecteer vervolgens **volgende**.
1. Op de **poort- en proxy-adres** pagina:

   a. Voer het nummer van de TCP-poort moet worden gebruikt voor de gateway. Dit poortnummer gebruikt het installatieprogramma een inkomende regel configureren op Windows-Firewall.  De standaardwaarde is 8080.
      Het geldige bereik van het poortnummer ligt tussen 1 en 65535 zijn. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.

   b. Als de server waarop de gateway is geïnstalleerd communiceren via een proxy moet, voert u de proxy-adres waar de gateway nodig heeft om verbinding te maken. Geef bijvoorbeeld `http://myorgname.corp.contoso.com:80` op.  Als u dit veld leeg laat, wordt de gateway probeert rechtstreeks verbinding maakt met internet.  Als uw proxyserver verificatie is vereist, voert u een gebruikersnaam en wachtwoord.

   c. Selecteer **Volgende**.

   ![Schermopname van configuratie van de gatewayproxy](./media/gateway/gateway-wizard02.png)

1. Als u nog geen Microsoft Update is ingeschakeld, de Microsoft Update-pagina wordt weergegeven en u kunt deze inschakelen. Maak een selectie en selecteer vervolgens **volgende**. Ga anders verder met de volgende stap.
1. Op de **doelmap** pagina, laat de standaardmap C:\Program Files\OMS Gateway of geef de locatie waar u de gateway te installeren. Selecteer vervolgens **Volgende**.
1. Op de **gereed voor installatie** weergeeft, schakelt **installeren**. Als Gebruikersaccountbeheer toestemming om te installeren vraagt, selecteert u **Ja**.
1. Nadat de installatie is voltooid, schakelt u **voltooien**. Open de module services.msc om te controleren of de service wordt uitgevoerd, en Controleer **OMS-Gateway** wordt weergegeven in de lijst met services en die de status **met**.

   ![Schermafbeelding van lokale services of OMS-Gateway wordt uitgevoerd](./media/gateway/gateway-service.png)


## <a name="configure-network-load-balancing"></a>Netwerktaakverdeling 
U kunt de gateway voor hoge beschikbaarheid met behulp van netwerktaakverdeling (NLB) met behulp van Microsoft configureren [Network Load Balancing (NLB)](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing), [Azure Load Balancer](../../load-balancer/load-balancer-overview.md), of op basis van hardware load balancers. De load balancer beheert verkeer door te leiden van de aangevraagde verbindingen van de Log Analytics-agents of beheerservers van Operations Manager op de knooppunten. Als een Gateway-server uitvalt, wordt het verkeer wordt omgeleid naar andere knooppunten.

### <a name="microsoft-network-load-balancing"></a>Microsoft Network Load Balancing
Zie voor informatie over het ontwerpen en implementeren van een Windows Server 2016 network load balancing-cluster, [Network load balancing](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing). De volgende stappen wordt beschreven hoe u een Microsoft network load balancing-cluster configureren.  

1. Meld u aan de Windows-server die deel uitmaakt van het NLB-cluster met een Administrator-account.  
2. Beheer van netwerktaakverdeling in Serverbeheer openen, klikt u op **extra**, en klik vervolgens op **beheer van netwerktaakverdeling**.
3. Als u wilt verbinding maken met een Log Analytics gateway-server met de Microsoft Monitoring Agent is geïnstalleerd, met de rechtermuisknop op het IP-adres van het cluster en klik vervolgens op **Host aan Cluster toevoegen**. 

    ![Network Load Balancing Manager – toevoegen Host aan Cluster](./media/gateway/nlb02.png)
 
4. Voer het IP-adres van de gateway-server waarmee u verbinding wilt maken. 

    ![Network Load Balancing Manager – toevoegen Host aan Cluster: Verbinding maken](./media/gateway/nlb03.png) 

### <a name="azure-load-balancer"></a>Azure Load Balancer
Zie voor informatie over het ontwerpen en implementeren van een Azure Load Balancer, [wat is Azure Load Balancer?](../../load-balancer/load-balancer-overview.md). Voor het implementeren van een basic load balancer, volg de stappen in deze [snelstartgids](../../load-balancer/quickstart-create-basic-load-balancer-portal.md) met uitzondering van de stappen die worden beschreven in de sectie **maken van back-endservers**.   

> [!NOTE]
> Configureren van de Azure Load Balancer met behulp van de **basis-SKU**, vereist dat virtuele Azure-machines deel uitmaken van een Beschikbaarheidsset. Zie voor meer informatie over beschikbaarheidssets, [de beschikbaarheid van Windows virtuele machines in Azure beheren](../../virtual-machines/windows/manage-availability.md). Bestaande virtuele machines toevoegen aan een beschikbaarheidsset, verwijzen naar [ingesteld Azure Resource Manager VM Availability Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4).
> 

Nadat de load balancer is gemaakt, moet een back-endgroep worden gemaakt, die wordt gedistribueerd naar een of meer gatewayservers. Volg de stappen in de sectie quickstart-artikel [resources maken voor de load balancer](../../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-resources-for-the-load-balancer).  

>[!NOTE]
>Bij het configureren van de statustest moet deze worden geconfigureerd voor het gebruik van de TCP-poort van de gateway-server. De statustest voegt dynamisch of verwijdert u de gateway-servers uit de load balancer-rotatie op basis van hun reactie op statuscontroles. 
>

## <a name="configure-the-log-analytics-agent-and-operations-manager-management-group"></a>De Log Analytics-agent en Operations Manager-beheergroep configureren
In deze sectie ziet u hoe u rechtstreeks verbonden zijn met Log Analytics-agents, een beheergroep van Operations Manager of Azure Automation Hybrid Runbook Workers configureren met de Log Analytics-gateway om te communiceren met Azure Automation of Log Analytics.  

### <a name="configure-a-standalone-log-analytics-agent"></a>Een zelfstandige Log Analytics-agent configureren
Bij het configureren van de Log Analytics-agent, vervang u de waarde van de proxy door het IP-adres van de Log Analytics gateway-server en het poortnummer. Als u meerdere gatewayservers achter een load balancer hebt geïmplementeerd, is de configuratie van de Log Analytics-agent-proxy in het virtuele IP-adres van de load balancer.  

>[!NOTE]
>Zie voor informatie over het installeren van de Log Analytics-agent op de gateway en de Windows-computers die rechtstreeks verbinding met Log Analytics maken [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](agent-windows.md). Zie voor Linux-computers verbinding, [configureren van een Log Analytics-agent voor Linux-computers in een hybride omgeving](../../azure-monitor/learn/quick-collect-linux-computer.md). 
>

Nadat u de agent op de gateway-server installeert, configureert om te rapporteren aan de werkruimte of werkruimte agents die met de gateway communiceren. Als de Log Analytics-Windows-agent is niet geïnstalleerd op de gateway, wordt de gebeurtenis 300 geschreven naar het gebeurtenislogboek van de OMS-Gateway, die aangeeft dat de agent moet worden geïnstalleerd. Als de agent is geïnstalleerd, maar niet is geconfigureerd om te rapporteren aan dezelfde werkruimte als de agents die via het communiceren, is 105 gebeurtenis geschreven naar hetzelfde logboek, waarmee wordt aangegeven dat de agent op de gateway moet worden geconfigureerd om te rapporteren aan dezelfde werkruimte als de agents die co mmunicate met de gateway.

Nadat u de configuratie hebt voltooid, start u de OMS-Gateway-service om de wijzigingen te laten. Anders wordt de gateway de agents die probeert te communiceren met Log Analytics en rapporteert gebeurtenis 105 in het gebeurtenislogboek van de OMS-Gateway geweigerd. Dit gebeurt ook wanneer u toevoegen of verwijderen van een werkruimte van de configuratie van de agent op de gatewayserver.   

Zie voor informatie met betrekking tot Automation Hybrid Runbook Worker [resources in uw datacentrum en de cloud automatiseren met behulp van Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager-where-all-agents-use-the-same-proxy-server"></a>Configureren van Operations Manager, waar alle agenten dezelfde proxyserver gebruiken
De configuratie van de Operations Manager-proxy wordt automatisch toegepast op alle agents die aan Operations Manager rapporteren, zelfs als de instelling leeg is.  

Voor het gebruik van OMS-Gateway voor de ondersteuning van Operations Manager, moet u het volgende hebben:

* Microsoft Monitoring Agent (versie 8.0.10900.0 of hoger) op de server van de OMS-Gateway is geïnstalleerd en geconfigureerd met de dezelfde Log Analytics-werkruimten die uw beheergroep is geconfigureerd voor rapportage aan.
* Verbinding met Internet. U kunt ook moet OMS-Gateway worden verbonden met een proxyserver die is verbonden met internet.

> [!NOTE]
> Als u geen waarde voor de gateway opgeeft, worden lege waarden worden gepusht naar alle agents.
>

Als uw Operations Manager-beheergroep voor het eerst bij een Log Analytics-werkruimte registreren is, ziet u de optie om op te geven van de configuratie van de proxy voor de beheergroep die u in de Operations-console. Deze optie is alleen beschikbaar als de beheergroep is geregistreerd bij de service.  

Bijwerken om integratie te configureren, de systeemconfiguratie van de proxyserver met behulp van Netsh op het systeem waarop u de Operations-console wordt uitgevoerd en op alle beheerservers in de beheergroep. Volg deze stappen:

1. Open een opdrachtprompt met verhoogde bevoegdheid:

   a. Selecteer **Start** en voer **cmd**.  

   b. Met de rechtermuisknop op **opdrachtprompt** en selecteer **als administrator uitvoeren**.  

1. Voer de volgende opdracht in:

   `netsh winhttp set proxy <proxy>:<port>`

Na het voltooien van de integratie met Log Analytics, verwijdert u de wijziging door te voeren `netsh winhttp reset proxy`. Gebruik vervolgens in de Operations-console de **proxyserver configureren** optie om op te geven van de Log Analytics-gatewayserver. 

1. Op de Operations Manager-console onder **Operations Management Suite**, selecteer **verbinding**, en selecteer vervolgens **proxyserver configureren**.

   ![Schermafbeelding van Operations Manager, waarin de selectie van Proxy-Server configureren](./media/gateway/scom01.png)

1. Selecteer **een proxyserver gebruiken voor toegang tot de Operations Management Suite** en voer het IP-adres van de Log Analytics-gatewayserver of virtueel IP-adres van de load balancer. Zorg ervoor dat u beginnen met het voorvoegsel `http://`.

   ![Schermafbeelding van Operations Manager, waarin de proxy-serveradres](./media/gateway/scom02.png)

1. Selecteer **Voltooien**. Uw Operations Manager-beheergroep is nu geconfigureerd voor communicatie via de gatewayserver aan de Log Analytics-service.

### <a name="configure-operations-manager-where-specific-agents-use-a-proxy-server"></a>Operations Manager, waarin specifieke agents wordt gebruikt een proxyserver configureren
Voor grote of complexe omgevingen, kunt u alleen specifieke servers (of groepen) de Log Analytics-gateway-server te gebruiken.  Voor deze servers bijwerken u niet Operations Manager-agent rechtstreeks omdat deze waarde wordt overschreven door de algemene waarde voor de beheergroep.  In plaats daarvan de regel die wordt gebruikt om deze waarden onderdrukken.  

> [!NOTE] 
> Deze techniek configuratie gebruiken als u wilt toestaan voor meerdere Log Analytics gateway-servers in uw omgeving.  U kunt bijvoorbeeld vereisen dat specifieke Log Analytics gateway-servers worden opgegeven op basis van landinstellingen.
>

Het configureren van specifieke servers of groepen de Log Analytics-gateway-server te gebruiken: 

1. Open de Operations Manager-console en selecteer de **ontwerpen** werkruimte.  
1. Selecteer in de werkruimte ontwerpen **regels**. 
1. Selecteer op de werkbalk Operations Manager de **bereik** knop. Als deze knop niet beschikbaar is, controleert u of u een object, en geen map, hebt geselecteerd in de **bewaking** deelvenster. De **bereik Management Pack-objecten** in het dialoogvenster geeft een lijst van algemene gerichte klassen, groepen of objecten. 
1. In de **zoekt** veld **Health-Service** en selecteer deze in de lijst. Selecteer **OK**.  
1. Zoeken naar **Advisor Proxy-instelling van regel**. 
1. Selecteer op de werkbalk Operations Manager **onderdrukkingen** en wijs vervolgens **overschrijven de Rule\For een specifiek object van klasse: Health-Service** en selecteer een object in de lijst.  Of een aangepaste groep waarin de health-service-object van de servers die u wilt toepassen met deze onderdrukking te maken. De onderdrukking toepassen op uw aangepaste groep.
1. In de **Onderdrukkingseigenschappen** dialoogvenster vak, voegt u een vinkje in de **overschrijven** kolom naast de **WebProxyAddress** parameter.  In de **Onderdrukkingswaarde** en voer de URL van de Log Analytics-gatewayserver. Zorg ervoor dat u beginnen met het voorvoegsel `http://`.  

    >[!NOTE]
    > U hoeft niet te de regel in te schakelen. Er is al automatisch beheerd met een onderdrukking in het managementpack voor Microsoft System Center Advisor beveiligde verwijzing overschrijven die is gericht op de Microsoft System Center Advisor Monitoring Server-groep.
    > 

1. Selecteer een managementpack uit de **selecteert bestemmingsmanagement pack** lijst of maak een nieuw niet-verzegeld managementpack door te selecteren **nieuw**. 
1. Selecteer **OK** wanneer u klaar bent. 

### <a name="configure-for-automation-hybrid-runbook-workers"></a>Voor Automation Hybrid Runbook Workers configureren
Als u Automation Hybrid Runbook Workers in uw omgeving hebt, volgt u deze stappen voor handmatige, tijdelijke oplossingen ter ondersteuning van de werknemers OMS-Gateway configureren.

Als u wilt volgen de stappen in deze sectie, moet u weten van de Azure-regio waarin het Automation-account zich bevindt. Om te zoeken die locatie:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer de Azure Automation-service.
1. Selecteer het juiste Azure Automation-account.
1. De regio onder weergeven **locatie**.

   ![Schermafbeelding van de locatie van de Automation-account in Azure portal](./media/gateway/location.png)

Gebruik de volgende tabellen om te identificeren van de URL voor elke locatie.

**Gegevens van de Runtime-URL 's**

| **Locatie** | **URL** |
| --- | --- |
| US - noord-centraal |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Europa -west |we-jobruntimedata-prod-su1.azure-automation.net |
| US - zuid-centraal |scus-jobruntimedata-prod-su1.azure-automation.net |
| US - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Canada centraal |cc-jobruntimedata-prod-su1.azure-automation.net |
| Europa - noord |ne-jobruntimedata-prod-su1.azure-automation.net |
| Azië - zuidoost |sea-jobruntimedata-prod-su1.azure-automation.net |
| India - centraal |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australië |ase-jobruntimedata-prod-su1.azure-automation.net |

**Agent-service-URL 's**

| **Locatie** | **URL** |
| --- | --- |
| US - noord-centraal |ncus-agentservice-prod-1.azure-automation.net |
| Europa -west |we-agentservice-prod-1.azure-automation.net |
| US - zuid-centraal |scus-agentservice-prod-1.azure-automation.net |
| US - oost 2 |eus2-agentservice-prod-1.azure-automation.net |
| Canada centraal |cc-agentservice-prod-1.azure-automation.net |
| Europa - noord |ne-agentservice-prod-1.azure-automation.net |
| Azië - zuidoost |sea-agentservice-prod-1.azure-automation.net |
| India - centraal |cid-agentservice-prod-1.azure-automation.net |
| Japan |jpe-agentservice-prod-1.azure-automation.net |
| Australië |ase-agentservice-prod-1.azure-automation.net |

Als uw computer wordt automatisch geregistreerd als een Hybrid Runbook Worker, gebruikt u de oplossing Update Management voor het beheren van de patch. Volg deze stappen:

1. De gegevens van de Runtime-service-URL's toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. De Log Analytics-gateway-service opnieuw starten met behulp van de volgende PowerShell-cmdlet: `Restart-Service OMSGatewayService`

Als uw computer is toegevoegd aan Azure Automation met behulp van de cmdlet voor de Hybrid Runbook Worker-registratie, volg deze stappen:

1. De registratie-URL van de agent-service toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. De gegevens van de Runtime-service-URL's toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Start de service Log Analytics-gateway.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nuttige PowerShell-cmdlets
U kunt cmdlets gebruiken om de taken voor het bijwerken van de Log Analytics-gatewayconfiguratie-instellingen te voltooien. Voordat u cmdlets gebruikt, moet u naar:

1. De Log Analytics-gateway (Microsoft Windows-installatieprogramma) installeren.
1. Open een PowerShell-consolevenster.
1. Importeer de module met deze opdracht te typen: `Import-Module OMSGateway`
1. Als er geen fout is opgetreden in de vorige stap, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Voer `Get-Module OMSGateway` in
1. Nadat u de cmdlets gebruikt om wijzigingen aanbrengen, moet u de OMS-Gateway-service opnieuw starten.

Een fout in stap 3 betekent dat de module is niet geïmporteerd. De fout kan optreden wanneer PowerShell kan de module niet vinden. U kunt de module niet vinden in de OMS-Gateway-installatiepad: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeeld** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Sleutel |Hiermee wordt de configuratie van de service opgehaald |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Sleutel (vereist) <br> Waarde |De configuratie van de service wordt gewijzigd |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Het adres van de relay (upstream) proxy opgehaald |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Gebruikersnaam<br> Wachtwoord |Hiermee stelt u het adres (en de referentie) van de relay (upstream) proxy |1. Stel een relay-proxy en de referentie:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Een relay-proxy die verificatie hoeft niet instellen: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Schakel de relay proxy-instellingen:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |De host momenteel toegestane opgehaald (alleen de lokaal geconfigureerde toegestane toegestane host is, niet automatisch gedownload hosts) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (vereist) |De host aan de lijst met toegestane toevoegt |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (vereist) |Hiermee verwijdert u de host uit de lijst met toegestane |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Subject (vereist) |Het clientcertificaat onderworpen aan de lijst met toegestane toegevoegd |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Subject (vereist) |Hiermee verwijdert u het certificaatonderwerp client uit de lijst met toegestane |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Haalt de client op dit moment toegestane (alleen de lokaal geconfigureerde toegestane onderwerpen, niet automatisch gedownload toegestaan onderwerpen) certificaat onderwerpen |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problemen oplossen
U hebt voor het verzamelen van gebeurtenissen vastgelegd door de gateway, de Log Analytics-agent is geïnstalleerd.

![Schermafbeelding van de Event Viewer-lijst in het logboek voor de Log Analytics-gateway](./media/gateway/event-viewer.png)

### <a name="log-analytics-gateway-event-ids-and-descriptions"></a>Log Analytics-gateway gebeurtenis-id's en beschrijvingen

De volgende tabel ziet u de gebeurtenis-id's en beschrijvingen voor logboekgebeurtenissen van Log Analytics-gateway.

| **ID** | **Beschrijving** |
| --- | --- |
| 400 |Een toepassingsfout die geen specifieke id heeft. |
| 401 |Onjuiste configuratie. Bijvoorbeeld, listenPort = "text" in plaats van een geheel getal zijn. |
| 402 |Uitzondering bij het parseren van TLS-handshake berichten. |
| 403 |Fout bij het netwerk. Bijvoorbeeld: kan geen verbinding maken met doelserver. |
| 100 |Algemene informatie. |
| 101 |Service is gestart. |
| 102 |Service is gestopt. |
| 103 |Een opdracht HTTP-verbinding is ontvangen van de client. |
| 104 |Geen een HTTP-verbinding maken met de opdracht. |
| 105 |Doelserver zich niet in de lijst met toegestane of doelpoort is niet beveiligd (443). <br> <br> Zorg ervoor dat de MMA-agent op uw OMS-Gateway-server en de agents die met OMS-Gateway communiceren zijn verbonden met de dezelfde Log Analytics-werkruimte. |
| 105 |Fout TcpConnection: ongeldig clientcertificaat: CN=Gateway. <br><br> Zorg ervoor dat u OMS-Gateway versie 1.0.395.0 of hoger. Zorg er ook voor dat de MMA-agent op uw OMS-Gateway-server en de agents die communiceren met OMS-Gateway zijn verbonden met de dezelfde Log Analytics-werkruimte. |
| 106 |Niet-ondersteunde versie van TLS/SSL-protocol.<br><br> De Log Analytics-gateway ondersteunt alleen TLS 1.0, TLS 1.1 en 1.2. Het biedt geen ondersteuning voor SSL.|
| 107 |De TLS-sessie is geverifieerd. |

### <a name="performance-counters-to-collect"></a>Te verzamelen prestatiemeteritems

De volgende tabel ziet u de prestatiemeteritems die beschikbaar zijn voor de Log Analytics-gateway. Prestatiemeter gebruiken voor het toevoegen van de items.

| **Naam** | **Beschrijving** |
| --- | --- |
| Log Analytics Gateway/actieve-clientverbinding |Aantal actieve client-Netwerkverbindingen (TCP) |
| Aantal log Analytics-Gateway/fouten |Aantal fouten |
| Log Analytics-Gateway of verbonden Client |Aantal verbonden clients |
| Log Analytics Gateway/afwijzing tellen |Het aantal weigeringen vanwege een validatiefout TLS |

![Schermafbeelding van Log Analytics gateway interface, prestatiemeteritems weergeven](./media/gateway/counters.png)

## <a name="assistance"></a>Hulp
Wanneer u bent aangemeld bij de Azure-portal, kunt u hulp bij de Log Analytics-gateway of een andere Azure-service of functie.
Hulp bij, selecteert u het vraagtekenpictogram in de rechterbovenhoek van de portal en selecteer **nieuwe ondersteuningsaanvraag**. Voltooi nieuw aanvraagformulier voor de ondersteuning.

![Schermafbeelding van een nieuwe ondersteuningsaanvraag](./media/gateway/support.png)

## <a name="next-steps"></a>Volgende stappen
[Gegevensbronnen toevoegen](../../azure-monitor/platform/agent-data-sources.md) voor het verzamelen van gegevens vanuit verbonden bronnen, en opslaan van de gegevens in uw Log Analytics-werkruimte.