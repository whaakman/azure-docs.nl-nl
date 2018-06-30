---
title: Verbinding maken met computers met behulp van de OMS-Gateway | Microsoft Docs
description: Verbinding maken met uw apparaten en computers Operations Manager bewaakt met de OMS-Gateway om gegevens te verzenden naar de Azure Automation en Log Analytics-service wanneer ze geen toegang tot Internet hebben.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: ecbc88ebaaa93215f85b57becc8a643dc3e168a0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129037"
---
# <a name="connect-computers-without-internet-access-using-the-oms-gateway"></a>Verbinding maken met computers zonder toegang tot het Internet met behulp van de OMS-Gateway
Dit document beschrijft hoe u communicatie met Azure Automation configureert en verbonden met de OMS-Gateway als directe logboekanalyse of Operations Manager bewaakt computers hebben geen toegang tot Internet.  De OMS-Gateway een forward HTTP-proxy die ondersteuning biedt voor HTTP-tunneling met de opdracht HTTP-verbinding is, kunnen gegevens verzamelen en naar Azure Automation en Log Analytics namens hen verzonden.  

De OMS-Gateway ondersteunt:

* Azure Automation Hybrid Runbook Workers  
* Windows-computers met Microsoft Monitoring Agent rechtstreeks verbonden met een werkruimte voor logboekanalyse
* Linux-computers met de OMS-Agent voor Linux rechtstreeks verbonden met een werkruimte voor logboekanalyse  
* System Center Operations Manager 2012 SP1 met UR7 of Operations Manager 2012 R2 UR3 Operations Manager 2016 en beheergroep van Operations Manager versie 1801 geïntegreerd met logboekanalyse.  

Als de beleidsregels van uw IT-beveiliging niet toestaan dat computers in uw netwerk verbinding maken met Internet, zoals de punt van apparaten verkooppunten (POS) of de servers die ondersteuning bieden IT-services, maar u deze verbinding te maken met Azure Automation of Log Analytics moet kunt beheren en controleren ze , ze kunnen worden geconfigureerd voor directe communicatie met de OMS-Gateway voor het ontvangen van configuratie en het doorsturen van gegevens in hun naam.  Als deze computers zijn geconfigureerd met de OMS-agent rechtstreeks verbinding maken met een werkruimte voor logboekanalyse, communiceert alle computers in plaats daarvan met de OMS-Gateway.  De gateway worden de gegevens van de agents naar de service rechtstreeks, analyseert de gegevens onderweg niet.

Wanneer een beheergroep van Operations Manager is geïntegreerd met Log Analytics, kunnen de beheerservers worden geconfigureerd voor verbinding met de OMS-Gateway voor configuratie-informatie te ontvangen en verzenden van verzamelde gegevens afhankelijk van de oplossing die u hebt ingeschakeld.  Sommige gegevens, zoals Operations Manager waarschuwingen, beoordeling van de configuratie, exemplaarruimte en capaciteit verzenden Operations Manager-agents naar de beheerserver. Andere grote hoeveelheden gegevens, zoals IIS-logboeken, prestaties en beveiligingsgebeurtenissen zijn rechtstreeks naar de OMS-Gateway verzonden.  Als u een of meer Operations Manager-Gateway-servers geïmplementeerd in een Perimeternetwerk of andere geïsoleerd netwerk hebt voor het bewaken van niet-vertrouwde systemen, communiceren niet het met een OMS-Gateway.  Operations Manager-gatewayservers kunnen alleen aan een beheerserver rapporteren.  Wanneer een beheergroep van Operations Manager is geconfigureerd om te communiceren met de OMS-Gateway, de proxy-configuratie-informatie automatisch wordt gedistribueerd naar elke agent beheerde computer die is geconfigureerd voor het verzamelen van gegevens voor logboekanalyse, zelfs als de instelling is leeg.    

Hoge beschikbaarheid bieden voor direct verbonden of Operations beheergroepen die met logboekanalyse via de gateway communiceren, kunt u Netwerktaakverdeling omgeleid en distributie van het verkeer over meerdere gatewayservers.  Als een gatewayserver uitvalt, wordt het verkeer wordt omgeleid naar een ander beschikbaar knooppunt.  

De OMS-agent is vereist op de computer met de OMS-Gateway in om deze te identificeren van de service-eindpunten die nodig is om te communiceren met en controleren van de OMS-Gateway voor het analyseren van de prestaties of de gegevens van gebeurtenissen.

Elke agent moet verbinding met het netwerk naar de gateway hebben, zodat de agents automatisch gegevens naar en van de gateway kunnen overdragen. De gateway is geïnstalleerd op een domeincontroller wordt niet aanbevolen.

Het volgende diagram toont de gegevensstroom van directe agents Azure Automation en Log Analytics met behulp van de gateway-server.  Agents moeten hun proxyconfiguratie komt overeen met de dezelfde poort die de OMS-Gateway is geconfigureerd om te communiceren met de service hebben.  

![voor directe agentcommunicatie met services diagram](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Het volgende diagram toont de gegevensstroom uit een beheergroep van Operations Manager met logboekanalyse.   

![Operations Manager-communicatie met Log Analytics-diagram](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Vereisten

Wanneer het toewijzen van een computer worden uitgevoerd van de OMS-Gateway, moet deze computer hebben het volgende:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2,  Windows Server 2008
* .Net Framework 4.5
* Minimum van een 4-core-processor en 8 GB geheugen 
* OMS-Agent voor Windows 

### <a name="language-availability"></a>Beschikbare talen

De OMS-Gateway is beschikbaar in de volgende talen:

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
- Spaans (International)

### <a name="supported-encryption-protocols"></a>Versleuteling van ondersteunde protocollen
De OMS-Gateway ondersteunt alleen Transport Layer Security (TLS) 1.0, 1.1 en 1.2.  Secure Sockets Layer (SSL) worden niet ondersteund.

### <a name="supported-number-of-agent-connections"></a>Aantal ondersteunde agent-verbindingen
De volgende tabel licht de het ondersteunde aantal agents een gatewayserver communiceert.  Deze ondersteuning is gebaseerd op agents ~ 200KB aan gegevens uploaden elke 6 seconden. Het gegevensvolume per agent getest is ongeveer 2.7GB per dag.

|Gateway |Ongeveer aantal agents die worden ondersteund|  
|--------|----------------------------------|  
|-CPU: Intel XEON CPU E5 2660 v3 @ 2.6GHz 2 kernen<br> -Geheugen: 4 GB<br> -Netwerkbandbreedte: 1 Gbps| 600|  
|-CPU: Intel XEON CPU E5 2660 v3 @ 2.6GHz 4 Cores<br> -Geheugen: 8 GB<br> -Netwerkbandbreedte: 1 Gbps| 1000|  

## <a name="download-the-oms-gateway"></a>Download de OMS-Gateway

Er zijn twee manieren om de meest recente versie van het installatiebestand van OMS-Gateway.

1. Downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Downloaden van de Azure-portal.  Nadat u zich aanmeldt bij de Azure-portal:  

   1. De lijst met services bladeren en selecteer vervolgens **logboekanalyse**.  
   2. Selecteer een werkruimte.
   3. Uw werkruimte-blade onder **algemene**, klikt u op **Quick Start**.
   4. Onder **Kies een gegevensbron verbinding maken met de werkruimte**, klikt u op **Computers**.
   5. In de **Direct Agent** blade, klikt u op **OMS-Gateway downloaden**.<br><br> ![OMS Gateway downloaden](./media/log-analytics-oms-gateway/download-gateway.png)

of 

   1. Uw werkruimte-blade onder **instellingen**, klikt u op **geavanceerde instellingen**.
   2. Navigeer naar **verbonden bronnen** > **Windows-Servers** en klik op **OMS-Gateway downloaden**.

## <a name="install-the-oms-gateway"></a>De OMS-Gateway installeren

Als u wilt een gateway installeren, moet u de volgende stappen uitvoeren.  Als u een eerdere versie geïnstalleerd, voorheen *Log Analytics doorstuurserver*, wordt dit bijgewerkt naar deze versie.  

1. Dubbelklik in de doelmap op **OMS Gateway.msi**.
2. Klik op de pagina **Welkom** op **Volgende**.<br><br> ![Gateway-installatiewizard](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br> 
3. Op de **License Agreement** pagina **ik ga akkoord met de voorwaarden in deze gebruiksrechtovereenkomst** gaat akkoord met de gebruiksrechtovereenkomst en klik vervolgens op **volgende**.
4. Op de **poort en proxy-adres** pagina:
   1. Typ het nummer van de TCP-poort moet worden gebruikt voor de gateway. Setup configureert een inkomende regel met dit poortnummer op Windows firewall.  De standaardwaarde is 8080.
      Het geldige bereik van het poortnummer is 1-65535. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.
   2. Als de server waarop de gateway is geïnstalleerd communiceren via een proxy moet, typ desgewenst het proxyadres waarop de gateway verbinding moet maken. Bijvoorbeeld `http://myorgname.corp.contoso.com:80`.  Als niets wordt opgegeven, probeert de gateway rechtstreeks verbinding maken met Internet.  Als de proxyserver verificatie vereist, voert u een gebruikersnaam en wachtwoord.<br><br> ![Gateway-Wizard proxyconfiguratie](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Klik op **Volgende**.
5. Als u geen Microsoft Update is ingeschakeld, wordt de pagina Microsoft Update weergegeven waarin u kunt kiezen in te schakelen. Maak een selectie en klik vervolgens op **volgende**. Ga anders verder met de volgende stap.
6. Op de **doelmap** pagina op de map C:\Program Files\OMS Gateway laat of typ de locatie waar u wilt de gateway installeren en klik vervolgens op **volgende**.
7. Op de **gereed voor installatie** pagina, klikt u op **installeren**. Gebruikersaccountbeheer lijken aanvragende machtiging om te installeren. Als dit het geval is, klikt u op **Ja**.
8. Nadat Setup is voltooid, klikt u op **voltooien**. U kunt controleren of de service wordt uitgevoerd door het openen van de module services.msc en Controleer **OMS Gateway** wordt weergegeven in de lijst met services en deze status is **met**.<br><br> ![Services – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Netwerktaakverdeling configureren 
U kunt de gateway voor hoge beschikbaarheid met netwerktaakverdeling (NLB) met behulp van Microsoft Network Load Balancing (NLB) of op basis van hardware load balancers kunt configureren.  De load balancer beheert verkeer door te leiden van de aangevraagde verbindingen van de OMS-agents of beheerservers van Operations Manager op de knooppunten. Als een Gateway-server uitvalt, wordt het verkeer wordt omgeleid naar andere knooppunten.

Zie voor informatie over het ontwerpen en implementeren van een Windows Server 2016 network load balancing-cluster, [Network load balancing](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  De volgende stappen beschrijven het configureren van een Microsoft network load balancing-cluster.  

1. Meld u aan bij de Windows-server die lid is van het NLB-cluster met een Administrator-account.  
2. Beheer van netwerktaakverdeling in Serverbeheer te openen, klikt u op **extra**, en klik vervolgens op **beheer van netwerktaakverdeling**.
3. Met de rechtermuisknop op het cluster-IP-adres voor een OMS-gatewayserver verbinding met de Microsoft Monitoring Agent is geïnstalleerd, en klik vervolgens op **Host aan Cluster toevoegen**.<br><br> ![Netwerk Load Balancing Manager – toevoegen Host aan Cluster](./media/log-analytics-oms-gateway/nlb02.png)<br> 
4. Geef het IP-adres van de gatewayserver waarmee u verbinding wilt maken.<br><br> ![Network Load Balancing Manager – Host aan Cluster toevoegen: verbinding maken](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-oms-agent-and-operations-manager-management-group"></a>OMS-agent en Operations Manager-beheergroep configureren
De volgende sectie bevat stapsgewijze instructies voor het rechtstreeks verbonden zijn met OMS-agents, een Operations Manager-beheergroep of Azure Automation Hybrid Runbook Workers configureren met de OMS-Gateway kan communiceren met Azure Automation of Log Analytics.  

### <a name="configure-standalone-oms-agent"></a>Zelfstandige OMS-agent configureren
Zie voor informatie over vereisten en stappen voor het installeren van de OMS-agent op Windows-computers rechtstreeks verbinding te maken met Log Analytics, [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agents.md) of voor Linux-computers Zie [verbinding maken met Linux computers met logboekanalyse](log-analytics-quick-collect-linux-computer.md). Plaats het opgeven van een proxyserver bij het configureren van de agent, vervangt u die waarde door het IP-adres van de OMS-Gateway-server en het poortnummer.  Als u meerdere gatewayservers achter een load balancer van netwerk hebt geïmplementeerd, is de configuratie van de OMS-agent-proxy het virtuele IP-adres van de NLB.  

Zie voor informatie die betrekking hebben op de Automation Hybrid Runbook Worker [implementeren Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Configureren van Operations Manager - alle agenten dezelfde proxyserver gebruiken
U configureren Operations Manager om toe te voegen van de gateway-server.  De configuratie van de Operations Manager-proxy wordt automatisch toegepast op alle agents die rapporteren aan Operations Manager, zelfs als de instelling leeg is.  

Voor het gebruik van de Gateway voor de ondersteuning van Operations Manager, moet u het volgende hebben:

* Microsoft Monitoring Agent (agentversie – **8.0.10900.0** of hoger) op de gatewayserver is geïnstalleerd en geconfigureerd voor een Log Analytics-werkruimten die u wilt communiceren.
* De gateway moet verbinding met Internet of zijn verbonden met een proxyserver die biedt.

> [!NOTE]
> Als u een waarde op voor de gateway niet opgeeft, worden de lege waarden geduwd naar alle agents.
> 

Als dit de eerste keer dat de Operations Manager-beheergroep met een werkruimte voor logboekanalyse registreert, is de mogelijkheid te geven van de configuratie van de proxy voor de beheergroep niet beschikbaar in de Operations-console.  Deze optie is pas beschikbaar als de beheergroep bij de service is geregistreerd.  U moet de systeemproxyconfiguratie bijwerken met behulp van Netsh vanaf het systeem waarop de Operations-console wordt uitgevoerd om de integratie en alle beheerservers in de beheergroep te configureren.  

1. Open een opdrachtprompt met verhoogde bevoegdheid.
   a. Ga naar **Start** en het type **cmd**.
   b. Met de rechtermuisknop op **opdrachtprompt** en uitvoeren als beheerder ** selecteren.
2. Voer de volgende opdracht in en druk op **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Na het voltooien van de integratie met Log Analytics, kunt u de wijziging door te voeren `netsh winhttp reset proxy` en gebruik vervolgens de **proxyserver configureren** optie in de Operations-console voor het opgeven van de OMS-gatewayserver. 

1. Open de Operations Manager-console en klikt u onder **Operations Management Suite**, klikt u op **verbinding** en klik vervolgens op **proxyserver configureren**.<br><br> ![Operations Manager – proxyserver configureren](./media/log-analytics-oms-gateway/scom01.png)<br> 
2. Selecteer **een proxyserver gebruiken voor toegang tot de Operations Management Suite** en typt u het IP-adres van de OMS-gatewayserver of virtueel IP-adres van de NLB. Zorg ervoor dat u met begint de `http://` voorvoegsel.<br><br> ![Operations Manager – proxyserveradres](./media/log-analytics-oms-gateway/scom02.png)<br> 
3. Klik op **Voltooien**. Uw Operations Manager-beheergroep is nu geconfigureerd om te communiceren via de gatewayserver naar de Log Analytics-service.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Configureren van Operations Manager - specifieke agents proxyserver gebruiken
Voor grote of complexe omgevingen, alleen kunt u specifieke servers (of groepen) de OMS-Gateway-server te gebruiken.  Voor deze servers u Operations Manager-agent niet bijwerken rechtstreeks met deze waarde wordt overschreven door de globale waarde voor de beheergroep.  In plaats daarvan moet u de regel die wordt gebruikt om deze waarden uiterste onderdrukken.  

> [!NOTE] 
> Deze techniek configuratie kan worden gebruikt om toe te staan het gebruik van meerdere OMS-gatewayservers in uw omgeving.  Specifieke OMS gatewayservers worden opgegeven op basis van de regio-mogelijk nodig.
>  

1. Open de Operations Manager-console en selecteer de **ontwerpen** werkruimte.  
2. Selecteer in de werkruimte ontwerpen **regels** en klik op de **bereik** op de werkbalk Operations Manager. Als deze knop niet beschikbaar is, controleert u om ervoor te zorgen dat u een object, geen map, dat is geselecteerd in het deelvenster bewaking hebt. De **bereik Management Pack-objecten** in het dialoogvenster geeft een lijst met algemene bepaalde klassen, groepen of objecten. 
3. Type **Health-Service** in de **zoekt** veld en selecteert u deze in de lijst.  Klik op **OK**.  
4. Zoeken naar de regel **Advisor Proxy-instelling regel** en klik op de werkbalk Operations-console op **overschrijft** en wijs vervolgens **overschrijven de Rule\For een specifiek object van klasse: Health-Service**  en selecteert u een specifiek object in de lijst.  Desgewenst kunt u een aangepaste groep met het object health-service van de servers die u wilt toepassen met deze overschrijving aan en vervolgens de onderdrukking toepassen op die groep.
5. In de **Onderdrukkingseigenschappen** in het dialoogvenster, klikt u op om een vinkje in de **overschrijven** kolom naast de **WebProxyAddress** parameter.  In de **Onderdrukkingswaarde** en voer de URL van de OMS-Gateway server ervoor te zorgen dat u met begint de `http://` voorvoegsel.  

    >[!NOTE]
    > U hoeft geen regel inschakelen als deze is al automatisch beheerd met een overschrijving die is opgenomen in Microsoft System Center Advisor beveiligde verwijzing Override management pack die gericht is op de Microsoft System Center Advisor Monitoring Server-groep.
    >   

6. Selecteer een management pack uit de **selecteert bestemmingsmanagement pack** lijst of maak een nieuw niet-verzegeld management pack door te klikken **nieuw**. 
7. Wanneer u uw wijzigingen hebt voltooid, klikt u op **OK**. 

### <a name="configure-for-automation-hybrid-workers"></a>Voor automation hybrid workers configureren
Als u Automation Hybrid Runbook Workers in uw omgeving hebt, u volgt de handmatige, tijdelijke oplossingen voor het configureren van de Gateway om te ondersteunen.

U moet weten welke Azure-regio waarin het Automation-account zich bevindt in de volgende stappen. De locatie vinden:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer de Azure Automation-service.
3. Selecteer het juiste Azure Automation-account.
4. De regio onder weergeven **locatie**.<br><br> ![Azure-portal – Automation-accountlocatie](./media/log-analytics-oms-gateway/location.png)  

Gebruik de volgende tabellen in de URL voor elke locatie te identificeren:

**Taak runtime data service-URL 's**

| **location** | **URL** |
| --- | --- |
| Noord-centraal VS |ncus-jobruntimedata-prod-su1.azure-automation.net |
| West-Europa |we-jobruntimedata-prod-su1.azure-automation.net |
| Zuid-centraal VS |scus-jobruntimedata-prod-su1.azure-automation.net |
| VS - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Canada Central |cc-jobruntimedata-prod-su1.azure-automation.net |
| Noord-Europa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Zuidoost-Azië |sea-jobruntimedata-prod-su1.azure-automation.net |
| Centraal-India |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australië |ase-jobruntimedata-prod-su1.azure-automation.net |

**Agent-service-URL 's**

| **location** | **URL** |
| --- | --- |
| Noord-centraal VS |ncus-agentservice-prod-1.azure-automation.net |
| West-Europa |we-agentservice-prod-1.azure-automation.net |
| Zuid-centraal VS |scus-agentservice-prod-1.azure-automation.net |
| VS - oost 2 |eus2-agentservice-prod-1.azure-automation.net |
| Canada Central |cc-agentservice-prod-1.azure-automation.net |
| Noord-Europa |ne-agentservice-prod-1.azure-automation.net |
| Zuidoost-Azië |sea-agentservice-prod-1.azure-automation.net |
| Centraal-India |cid-agentservice-prod-1.azure-automation.net |
| Japan |jpe-agentservice-prod-1.azure-automation.net |
| Australië |ase-agentservice-prod-1.azure-automation.net |

Als uw computer is geregistreerd als een hybride Runbook Worker die automatisch voor het gebruik van de Update-beheeroplossing patchen, volg deze stappen:

1. De taakgegevens Runtime service-URL's toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. De OMS-Gateway-service opnieuw starten met de volgende PowerShell-cmdlet: `Restart-Service OMSGatewayService`

Als uw computer geïntegreerde voor Azure Automation via de cmdlet Hybrid Runbook Worker-registratie, volg deze stappen:

1. De agent service registratie-URL toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. De taakgegevens Runtime service-URL's toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. De OMS-Gateway-service opnieuw starten.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Handig PowerShell-cmdlets
Cmdlets kunt u taken uitvoeren die nodig zijn om bij te werken van configuratie-instellingen de OMS-Gateway. Voordat u ze gebruiken, moet u:

1. Installeer de OMS-Gateway (MSI).
2. Open een PowerShell-consolevenster.
3. U kunt de module importeren door deze opdracht te typen: `Import-Module OMSGateway`
4. Als er is geen fout in de vorige stap opgetreden, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Type `Get-Module OMSGateway`
5. Nadat u wijzigingen aanbrengt met behulp van de cmdlets, zorg ervoor dat u de Gateway-service opnieuw opstarten.

Als u een fout in stap 3 krijgt, wordt de module is niet geïmporteerd. De fout kan optreden wanneer PowerShell is niet gevonden van de module. U kunt deze vinden in de Gateway-installatiepad: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeeld** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Sleutel |De configuratie van de service opgehaald |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Sleutel (vereist) <br> Waarde |De configuratie van de service wordt gewijzigd |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Het adres van de relay (upstream) proxy opgehaald |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Gebruikersnaam<br> Wachtwoord |Hiermee stelt u adres (en referentie) van de relay (upstream) proxy |1. Stel een relay-proxy en referentie:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Stel een relay-proxy waarvoor geen verificatie nodig: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Schakel de relay proxy-instellingen:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Opgehaald van de momenteel toegestane host (alleen de lokaal geconfigureerde toegestane host, omvat geen automatisch gedownloade toegestane hosts) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (vereist) |Voegt de host aan de lijst met toegestane |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (vereist) |Hiermee verwijdert u de host uit de lijst met toegestane |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Onderwerpnaam (vereist) |Het clientcertificaat onderworpen aan de lijst met toegestane toegevoegd |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Onderwerpnaam (vereist) |Hiermee verwijdert u het certificaatonderwerp client uit de lijst met toegestane |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Objecten van het certificaat van de momenteel toegestane client opgehaald (alleen lokaal geconfigureerde onderwerpen mogen, omvat geen automatisch gedownloade toegestane onderwerpen) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problemen oplossen
U moet ook beschikken over de OMS-agent is geïnstalleerd voor het verzamelen van gebeurtenissen die zijn vastgelegd door de gateway.<br><br> ![Logboeken – OMS Gateway-logboek](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gateway gebeurtenis-id's en beschrijvingen**

De volgende tabel ziet u de gebeurtenis-id's en beschrijvingen voor logboekgebeurtenissen OMS-Gateway.

| **ID** | **Beschrijving** |
| --- | --- |
| 400 |Een toepassingsfout die geen een specifieke ID |
| 401 |Onjuiste configuratie. Bijvoorbeeld: listenPort = 'text' in plaats van een geheel getal |
| 402 |Uitzondering bij het parseren van TLS-handshake-berichten |
| 403 |Fout bij het netwerk. Bijvoorbeeld: kan geen verbinding met de doelserver |
| 100 |Algemene informatie |
| 101 |Service is gestart |
| 102 |De service is gestopt |
| 103 |Een opdracht HTTP-verbinding ontvangen van client |
| 104 |Niet een opdracht HTTP-verbinding |
| 105 |Doelserver zich niet in de lijst met toegestane of de doelpoort is geen beveiligde poort (443) <br> <br> Zorg ervoor dat de MMA-agent op de gatewayserver en de agents die communiceren met de Gateway zijn verbonden met de dezelfde werkruimte voor logboekanalyse. |
| 105 |Fout TcpConnection: ongeldige clientcertificaat: CN = Gateway <br><br> Zorg ervoor dat: <br>    <br> &#149;U gebruikt een Gateway met versienummer 1.0.395.0 of hoger. <br> &#149;De MMA-agent op de gatewayserver en de agents die communiceren met de Gateway zijn verbonden met de dezelfde werkruimte voor logboekanalyse. |
| 106 |De OMS-Gateway ondersteunt alleen TLS 1.0, TLS 1.1 en 1.2.  SSL ondersteunt niet. Voor een niet-ondersteunde TLS/SSL-protocolversie genereert OMS-Gateway gebeurtenis-ID 106.|
| 107 |De TLS-sessie is geverifieerd |

**Te verzamelen prestatiemeteritems**

De volgende tabel bevat de beschikbare prestatiemeteritems voor de OMS-Gateway. U kunt de tellers die de Prestatiemeter gebruiken toevoegen.

| **Naam** | **Beschrijving** |
| --- | --- |
| Clientverbinding OMS-Gateway/actief |Aantal actieve client-Netwerkverbindingen (TCP) |
| Aantal OMS-Gateway/fouten |Aantal fouten |
| OMS-Gateway of verbonden Client |Aantal verbonden clients |
| OMS Gateway/afwijzing tellen |Het aantal weigeringen vanwege een validatiefout TLS |

![Prestatiemeteritems OMS-Gateway](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>U hulp nodig hebt
Wanneer u bent aangemeld bij de Azure portal, kunt u een verzoek om hulp kunt maken met de OMS-Gateway of andere Azure-service of functie van een service.
Om hulp vragen, klikt u op het vraagteken symbool in de rechterbovenhoek van de portal en klik vervolgens op **nieuw ondersteuningsverzoek**. Voltooi het nieuwe aanvraagformulier voor ondersteuning.

![Nieuw ondersteuningsverzoek](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Volgende stappen
[Voeg gegevensbronnen toe](log-analytics-data-sources.md) voor het verzamelen van gegevens uit uw verbonden bronnen en opslaan in de werkruimte voor logboekanalyse.
