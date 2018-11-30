---
title: Verbinding maken met computers met behulp van de Log Analytics-gateway | Microsoft Docs
description: Verbinding maken met uw apparaten en computers met Operations Manager bewaakt met de gateway Log Analytics om gegevens te verzenden naar de Azure Automation en Log Analytics-service als ze geen toegang tot Internet hebben.
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
ms.date: 08/02/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 5e19c7c1ed15183fdb796a6fa4e537da946b40b9
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642534"
---
# <a name="connect-computers-without-internet-access-using-the-log-analytics-gateway"></a>Verbinding maken met computers zonder toegang tot het Internet met behulp van de Log Analytics-gateway
Dit document wordt beschreven hoe u communicatie configureren met Azure Automation en Log Analytics met behulp van de Log Analytics-gateway als direct verbonden of Operations Manager bewaakt computers geen toegang tot Internet hebben.  De Log Analytics-gateway, die een forward HTTP-proxy die ondersteuning biedt voor HTTP-tunneling met de opdracht HTTP-verbinding maken, kan gegevens verzamelen en te verzenden naar Azure Automation en Log Analytics namens hen.  

De Log Analytics-gateway ondersteunt:

* Azure Automation Hybrid Runbook Workers  
* Windows-computers met de Microsoft Monitoring Agent rechtstreeks verbonden met een Log Analytics-werkruimte
* Linux-computers met de Log Analytics-agent voor Linux rechtstreeks verbonden met een Log Analytics-werkruimte  
* System Center Operations Manager 2012 SP1 met UR7 en Operations Manager 2012 R2 UR3, Operations Manager 2016 en beheergroep van Operations Manager-versie 1801 geïntegreerd met Log Analytics.  

Als uw IT-beveiligingsbeleid doen niet toestaat dat computers in uw netwerk verbinding maken met Internet, zoals punt van apparaten verkooppunten (POS) of de servers die ondersteuning bieden IT-services, maar u moet ze verbinden met Azure Automation of Log Analytics om te beheren en ze te controleren , ze kunnen worden geconfigureerd om te communiceren rechtstreeks met de Log Analytics-gateway voor het ontvangen van configuratie- en doorsturen van gegevens in hun naam.  Als deze computers zijn geconfigureerd met de Log Analytics-agent rechtstreeks verbinding maken met een Log Analytics-werkruimte, worden alle computers in plaats daarvan communiceert met de Log Analytics-gateway.  De gateway gegevens worden overgebracht van de agents met de service rechtstreeks, worden niet geanalyseerd van de gegevens die onderweg zijn.

Wanneer een Operations Manager-beheergroep is geïntegreerd met Log Analytics, kunnen de beheerservers worden geconfigureerd om te verbinden met de Log Analytics-gateway naar configuratie-informatie ontvangen en verzenden van verzamelde gegevens afhankelijk van de oplossing die u hebt ingeschakeld.  Sommige gegevens, zoals waarschuwingen, configuratie-evaluatie, exemplaarruimte en capaciteitsgegevens van Operations Manager verzenden Operations Manager-agents naar de beheerserver. Andere gegevens van de grote volumes, zoals IIS-logboeken, prestaties en beveiligingsgebeurtenissen worden rechtstreeks naar de gateway voor Log Analytics verzonden.  Als u een of meer Operations Manager-Gateway-servers geïmplementeerd in een DMZ of andere geïsoleerd netwerk hebt voor het bewaken van niet-vertrouwde systemen, communiceren niet het met een Log Analytics-gateway.  Operations Manager-gatewayservers kunnen alleen met een beheerserver rapporteren.  Wanneer een Operations Manager-beheergroep is geconfigureerd om te communiceren met de Log Analytics-gateway, wordt de proxy-configuratie-informatie automatisch gedistribueerd naar elke agent beheerde computer die is geconfigureerd voor het verzamelen van gegevens voor Log Analytics, zelfs Als de instelling leeg is.    

Hoge beschikbaarheid bieden voor direct verbonden of Operations-beheergroepen die met Log Analytics via de gateway communiceren, kunt u Netwerktaakverdeling omleiden naar en het verkeer verdelen over meerdere gatewayservers.  Als een gatewayserver uitvalt, wordt het verkeer wordt omgeleid naar een ander beschikbaar knooppunt.  

De Log Analytics-agent is vereist op de computer met de Log Analytics-gateway in volgorde voor het identificeren van de service-eindpunten die nodig is om te communiceren met en controleren van de Log Analytics-gateway voor het analyseren van de prestaties of de gebeurtenisgegevens.

Elke agent moet verbinding met het netwerk naar de gateway hebben, zodat agents kunnen automatisch worden overgedragen gegevens van en naar de gateway. De gateway installeert op een domeincontroller wordt niet aanbevolen.

Het volgende diagram toont de gegevensstroom van directe agents naar Azure Automation en Log Analytics met behulp van de gateway-server.  Agents moeten de proxyconfiguratie komt overeen met de dezelfde poort als die de Log Analytics-gateway is geconfigureerd voor communicatie met de service hebben.  

![directe agentcommunicatie met services diagram](./media/gateway/oms-omsgateway-agentdirectconnect.png)

Het volgende diagram toont de gegevensstroom van een Operations Manager-beheergroep naar Log Analytics.   

![Operations Manager-communicatie met Log Analytics-diagram](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Vereisten

Bij het toewijzen van een computer voor het uitvoeren van de Log Analytics-gateway, moet deze computer hebben het volgende:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2,  Windows Server 2008
* .Net Framework 4.5
* Ten minste een 4-core-processor en 8 GB geheugen 
* Log Analytics-agent voor Windows 

### <a name="language-availability"></a>Taal-beschikbaarheid

De Log Analytics-gateway is beschikbaar in de volgende talen:

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
De gateway Log Analytics biedt alleen ondersteuning voor Transport Layer Security (TLS) 1.0, 1.1 en 1.2.  Secure Sockets Layer (SSL) worden niet ondersteund.  Om te zorgen dat de beveiliging van gegevens die onderweg zijn naar Log Analytics, we raden u aan het configureren van de gateway te gebruiken ten minste Transport Layer Security (TLS) 1.2. Oudere versies van TLS/Secure Sockets Layer (SSL) kwetsbaar zijn gevonden en hoewel ze op dit moment nog steeds werken om toe te staan achterwaartse compatibiliteit, zijn ze onderling **niet aanbevolen**.  Raadpleeg voor meer informatie, [verzenden van gegevens veilig gebruik TLS 1.2](../../log-analytics/log-analytics-data-security.md#sending-data-securely-using-tls-12). 

### <a name="supported-number-of-agent-connections"></a>Aantal ondersteunde agent-verbindingen
De volgende tabel ziet u het ondersteunde aantal agents een gatewayserver communiceert.  Deze ondersteuning is gebaseerd op agents uploaden ~ 200KB aan gegevens van elke 6 seconden. Het gegevensvolume per agent getest is ongeveer 2.7GB per dag.

|Gateway |Ongeveer aantal agents ondersteund|  
|--------|----------------------------------|  
|-CPU: Intel XEON processor E5-2660 v3 \@ 2,6 GHz 2 kernen<br> -Geheugen: 4 GB<br> -Netwerk bandbreedte: 1 Gbps| 600|  
|-CPU: Intel XEON processor E5-2660 v3 \@ 2,6 GHz 4 Cores<br> -Geheugen: 8 GB<br> -Netwerk bandbreedte: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>De Log Analytics-gateway downloaden

Er zijn twee manieren naar de meest recente versie van het installatiebestand van de Log Analytics-gateway.

1. Downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

1. Downloaden van de Azure-portal.  Nadat u zich aanmeldt bij Azure portal:  

   1. Blader door de lijst met services en selecteer vervolgens **Log Analytics**.  
   1. Selecteer een werkruimte.
   1. In de blade van uw werkruimte onder **algemene**, klikt u op **Quick Start**.
   1. Onder **kiest u een gegevensbron verbinding maken met de werkruimte**, klikt u op **Computers**.
   1. In de **Direct Agent** blade, klikt u op **downloaden Log Analytics gateway**.<br><br> ![Log Analytics gateway downloaden](./media/gateway/download-gateway.png)

of 

   1. In de blade van uw werkruimte onder **instellingen**, klikt u op **geavanceerde instellingen**.
   1. Navigeer naar **verbonden bronnen** > **Windows Servers** en klikt u op **downloaden Log Analytics gateway**.

## <a name="install-the-log-analytics-gateway"></a>De Log Analytics-gateway installeren

Als u wilt een gateway installeert, moet u de volgende stappen uitvoeren.  Als u een eerdere versie hebt geïnstalleerd, voorheen *Log Analytics-doorstuurserver*, er wordt een upgrade uitgevoerd naar deze versie.  

1. Dubbelklik in de doelmap op **Log Analytics gateway.msi**.
1. Klik op de pagina **Welkom** op **Volgende**.<br><br> ![Wizard Setup van gateway](./media/gateway/gateway-wizard01.png)<br> 
1. Op de **License Agreement** weergeeft, schakelt **ik ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst** Ga akkoord met de gebruiksrechtovereenkomst en klik vervolgens op **volgende**.
1. Op de **poort- en proxy-adres** pagina:
   1. Typ het nummer van de TCP-poort moet worden gebruikt voor de gateway. Setup configureert een binnenkomende regel met dit poortnummer op Windows firewall.  De standaardwaarde is 8080.
      Het geldige bereik van het poortnummer is 1-65535. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.
   1. (Optioneel) als de server waarop de gateway is geïnstalleerd communiceren via een proxy moet, typt u de proxy-adres waar de gateway nodig heeft om verbinding te maken. Bijvoorbeeld `http://myorgname.corp.contoso.com:80`.  Als dit leeg is, probeert de gateway rechtstreeks verbinding maakt met Internet.  Als uw proxyserver verificatie is vereist, voert u een gebruikersnaam en wachtwoord.<br><br> ![Gateway Wizard proxyconfiguratie](./media/gateway/gateway-wizard02.png)<br>   
   1. Klik op **Volgende**.
1. Als u geen Microsoft Update is ingeschakeld, wordt er in de Microsoft Update-pagina wordt weergegeven waarin u kunt kiezen om te schakelen. Maak een selectie en klik vervolgens op **volgende**. Ga anders verder met de volgende stap.
1. Op de **doelmap** pagina, laat de standaardmap C:\Program Files\OMS Gateway of typ de locatie waar u gateway installeren en klik vervolgens op **volgende**.
1. Op de **gereed voor installatie** pagina, klikt u op **installeren**. User Account Control lijkt aanvragende machtiging om te installeren. Als dit het geval is, klikt u op **Ja**.
1. Nadat Setup is voltooid, klikt u op **voltooien**. U kunt controleren dat de service wordt uitgevoerd door het openen van de module services.msc en Controleer **Log Analytics gateway** wordt weergegeven in de lijst met services en deze status is **met**.<br><br> ![Services – Log Analytics-gateway](./media/gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Netwerktaakverdeling 
U kunt de gateway voor hoge beschikbaarheid met behulp van netwerktaakverdeling (NLB) met behulp van Microsoft Network Load Balancing (NLB) of op basis van hardware load balancers kunt configureren.  De load balancer beheert verkeer door te leiden van de aangevraagde verbindingen van de Log Analytics-agents of beheerservers van Operations Manager op de knooppunten. Als een Gateway-server uitvalt, wordt het verkeer wordt omgeleid naar andere knooppunten.

Zie voor informatie over het ontwerpen en implementeren van een Windows Server 2016 network load balancing-cluster, [Network load balancing](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  De volgende stappen wordt beschreven hoe u een Microsoft network load balancing-cluster configureren.  

1. Meld u aan de Windows-server die deel uitmaakt van het NLB-cluster met een Administrator-account.  
1. Beheer van netwerktaakverdeling in Serverbeheer openen, klikt u op **extra**, en klik vervolgens op **beheer van netwerktaakverdeling**.
1. Als u wilt verbinding maken met een Log Analytics gateway-server met de Microsoft Monitoring Agent is geïnstalleerd, met de rechtermuisknop op het IP-adres van het cluster en klik vervolgens op **Host aan Cluster toevoegen**.<br><br> ![Network Load Balancing Manager – toevoegen Host aan Cluster](./media/gateway/nlb02.png)<br> 
1. Voer het IP-adres van de gateway-server waarmee u verbinding wilt maken.<br><br> ![Network Load Balancing Manager – Host aan Cluster toevoegen: verbinding maken](./media/gateway/nlb03.png) 
    
## <a name="configure-log-analytics-agent-and-operations-manager-management-group"></a>Log Analytics-agent en Operations Manager-beheergroep configureren
De volgende sectie bevat instructies over het configureren van rechtstreeks verbonden zijn met Log Analytics-agents, een beheergroep van Operations Manager of Azure Automation Hybrid Runbook Workers met de Log Analytics-gateway om te communiceren met Azure Automation- of logboekpad Analytics.  

### <a name="configure-standalone-log-analytics-agent"></a>Zelfstandige Log Analytics-agent configureren
Zie voor meer informatie over vereisten en stappen voor het installeren van de Log Analytics-agent op Windows-computers rechtstreeks verbinding te maken met Log Analytics, [verbinding maken met Windows-computers naar Log Analytics](agent-windows.md) of voor Linux-computers naar [ Linux-computers verbinden met Log Analytics](../../log-analytics/log-analytics-quick-collect-linux-computer.md). Plaats het op te geven een proxyserver bij het configureren van de agent, kunt u die waarde vervangen door het IP-adres van de Log Analytics gateway-server en het poortnummer.  Als u meerdere gatewayservers achter een load balancer voor netwerk hebt geïmplementeerd, is de configuratie van de Log Analytics-agent-proxy in het virtuele IP-adres van de NLB.  

Zie voor informatie met betrekking tot Automation Hybrid Runbook Worker [Hybrid Runbook Worker implementeren](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Configureren van Operations Manager - alle agenten dezelfde proxyserver gebruiken
U configureren Operations Manager om toe te voegen van de gateway-server.  De configuratie van de Operations Manager-proxy wordt automatisch toegepast op alle agents die rapporteren aan Operations Manager, zelfs als de instelling leeg is.  

Voor het gebruik van de Gateway voor de ondersteuning van Operations Manager, moet u het volgende hebben:

* Microsoft Monitoring Agent (agentversie – **8.0.10900.0** of hoger) op de gatewayserver is geïnstalleerd en geconfigureerd voor een Log Analytics-werkruimten die u wilt communiceren.
* De gateway moet verbinding met Internet of zijn verbonden met een proxyserver die worden ondersteund.

> [!NOTE]
> Als u een waarde voor de gateway niet opgeeft, worden lege waarden worden gepusht naar alle agents.
> 

Als dit de eerste keer uw Operations Manager-beheergroep wordt geregistreerd met een Log Analytics-werkruimte, is de optie om op te geven van de configuratie van de proxy voor de beheergroep niet beschikbaar in de Operations-console.  Deze optie is pas beschikbaar als de beheergroep bij de service is geregistreerd.  Om de integratie en alle beheerservers in de beheergroep te configureren, moet u Netsh gebruiken om de systeemproxyconfiguratie bij te werken vanaf het systeem waarop de Operations-console wordt uitgevoerd.  

1. Open een opdrachtprompt met verhoogde bevoegdheid.
   a. Ga naar **Start** en het type **cmd**.
   b. Met de rechtermuisknop op **opdrachtprompt** en selecteert u uitvoeren als beheerder **.
1. Voer de volgende opdracht in en druk op **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Na het voltooien van de integratie met Log Analytics, kunt u de wijziging door te voeren `netsh winhttp reset proxy` en gebruik vervolgens de **proxyserver configureren** optie in de Operations-console voor het opgeven van de Log Analytics-gatewayserver. 

1. Open de Operations Manager-console en klikt u onder **Operations Management Suite**, klikt u op **verbinding** en klik vervolgens op **proxyserver configureren**.<br><br> ![Operations Manager – Webtoepassingsproxy-Server configureren](./media/gateway/scom01.png)<br> 
1. Selecteer **een proxyserver gebruiken voor toegang tot de Operations Management Suite** en typ het IP-adres van de Log Analytics-gatewayserver of het virtuele IP-adres van de NLB. Zorg ervoor dat u met begint de `http://` voorvoegsel.<br><br> ![Operations Manager-adres van proxyserver](./media/gateway/scom02.png)<br> 
1. Klik op **Voltooien**. Uw Operations Manager-beheergroep is nu geconfigureerd voor communicatie via de gatewayserver aan de Log Analytics-service.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Configureren van Operations Manager - specifieke agents proxyserver gebruiken
Voor grote of complexe omgevingen wilt u misschien alleen bepaalde servers (of groepen) de Log Analytics-gateway-server te gebruiken.  Voor deze servers, de Operations Manager-agent kan niet worden bijgewerkt rechtstreeks als deze waarde wordt overschreven door de algemene waarde voor de beheergroep.  In plaats daarvan moet u de regel die wordt gebruikt om deze waarden.  

> [!NOTE] 
> Deze techniek configuratie kan worden gebruikt voor het gebruik van meerdere gatewayservers voor Log Analytics in uw omgeving.  U hebt mogelijk bijvoorbeeld specifieke Log Analytics gateway-servers worden opgegeven op basis van de per regio.
>  

1. Open de Operations Manager-console en selecteer de **ontwerpen** werkruimte.  
1. Selecteer in de werkruimte ontwerpen **regels** en klikt u op de **bereik** op de werkbalk Operations Manager. Als deze knop niet beschikbaar is, controleert u om ervoor te zorgen dat u een object, geen map, dat is geselecteerd in het deelvenster bewaking hebt. De **bereik Management Pack-objecten** in het dialoogvenster geeft een lijst van algemene gerichte klassen, groepen of objecten. 
1. Type **Health-Service** in de **zoekt** veld en selecteer deze in de lijst.  Klik op **OK**.  
1. Zoek de regel **Advisor Proxy-instelling van regel** en in de werkbalk van de Operations-console op **onderdrukkingen** en wijs vervolgens **overschrijven de Rule\For een specifiek object van klasse: Health-Service**  en selecteer een specifiek object in de lijst.  Desgewenst kunt u een aangepaste groep met het object health-service van de servers die u wilt toepassen met deze onderdrukking aan en klikt u vervolgens de onderdrukking wordt toegepast aan die groep.
1. In de **Onderdrukkingseigenschappen** in het dialoogvenster, klikt u op om een vinkje in de **overschrijven** kolom naast de **WebProxyAddress** parameter.  In de **Onderdrukkingswaarde** en voer de URL van de Log Analytics gateway server ervoor te zorgen dat u met Start de `http://` voorvoegsel.  

    >[!NOTE]
    > U hoeft niet de regel inschakelen als deze al automatisch met een overschrijving die zijn opgenomen in het beheerpack Microsoft System Center Advisor beveiligde verwijzing overschrijven die gericht is op de Microsoft System Center Advisor Monitoring Server-groep wordt beheerd.
    >   

1. Selecteer een managementpack uit de **selecteert bestemmingsmanagement pack** lijst of een nieuw niet-verzegeld managementpack maken door te klikken op **nieuw**. 
1. Wanneer u uw wijzigingen hebt voltooid, klikt u op **OK**. 

### <a name="configure-for-automation-hybrid-workers"></a>Voor automation hybrid workers configureren
Als u Automation Hybrid Runbook Workers in uw omgeving hebt, u volgt de handmatige, tijdelijke oplossingen voor het configureren van de Gateway ter ondersteuning van deze.

U moet weten van de Azure-regio waarin het Automation-account zich bevindt in de volgende stappen. Op zoek naar de locatie:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer de Azure Automation-service.
1. Selecteer het juiste Azure Automation-account.
1. De regio onder weergeven **locatie**.<br><br> ![Azure-portal – de locatie van Automation](./media/gateway/location.png)  

Gebruik de volgende tabellen in de URL voor elke locatie te identificeren:

**Taak runtime data service-URL 's**

| **location** | **URL** |
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

| **location** | **URL** |
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

Als uw computer is geregistreerd als een Hybrid Runbook Worker die automatisch voor het patchen met behulp van de oplossing Update Management, volg deze stappen:

1. De gegevens van de Runtime-service-URL's toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. De Log Analytics-gateway-service opnieuw starten met behulp van de volgende PowerShell-cmdlet: `Restart-Service OMSGatewayService`

Als uw computer toegevoegd voor Azure Automation is met behulp van de cmdlet voor de Hybrid Runbook Worker-registratie, volg deze stappen:

1. De registratie-URL van de agent-service toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. De gegevens van de Runtime-service-URL's toevoegen aan de lijst met toegestane Host op de Log Analytics-gateway. Bijvoorbeeld: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Start de service Log Analytics-gateway.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nuttige PowerShell-cmdlets
Met behulp van cmdlets kunt u taken die nodig zijn om bij te werken van de Log Analytics-gatewayconfiguratie-instellingen uit te voeren. Voordat u ze gebruiken, moet u naar:

1. De Log Analytics-gateway (MSI) installeren.
1. Open een PowerShell-consolevenster.
1. Typ deze opdracht voor het importeren van de module: `Import-Module OMSGateway`
1. Als er geen fout is opgetreden in de vorige stap, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Type `Get-Module OMSGateway`
1. Nadat u wijzigingen aanbrengt met behulp van de cmdlets, zorg ervoor dat u de Gateway-service opnieuw opstarten.

Als u een fout optreedt in stap 3, wordt de module is niet geïmporteerd. De fout kan optreden wanneer PowerShell is niet gevonden de module. U vindt deze in van de Gateway-installatiepad: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeeld** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Sleutel |Hiermee wordt de configuratie van de service opgehaald |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Sleutel (vereist) <br> Waarde |De configuratie van de service wordt gewijzigd |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Het adres van de relay (upstream) proxy opgehaald |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Gebruikersnaam<br> Wachtwoord |Hiermee stelt u het adres (en de referentie) van de relay (upstream) proxy |1. Stel een relay-proxy en de referentie:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Een relay-proxy die verificatie hoeft niet instellen: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Schakel de relay proxy-instellingen:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Haalt de momenteel toegestane host (alleen de lokaal geconfigureerde toegestane host, bevat geen automatisch gedownloade toegestane hosts) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (vereist) |De host aan de lijst met toegestane toevoegt |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (vereist) |Hiermee verwijdert u de host uit de lijst met toegestane |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Subject (vereist) |Het clientcertificaat onderworpen aan de lijst met toegestane toegevoegd |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Subject (vereist) |Hiermee verwijdert u het certificaatonderwerp client uit de lijst met toegestane |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Haalt de client op dit moment toegestane certificaat onderwerpen (alleen de lokaal geconfigureerde onderwerpen mogen, bevat geen toegestane onderwerpen die automatisch gedownload) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problemen oplossen
Voor het verzamelen van gebeurtenissen vastgelegd door de gateway, moet u ook beschikken over de Log Analytics-agent is geïnstalleerd.<br><br> ![Logboeken: Log Analytics gateway Log](./media/gateway/event-viewer.png)

**Log Analytics gateway gebeurtenis-id's en beschrijvingen**

De volgende tabel worden de gebeurtenis-id's en beschrijvingen voor Log Analytics gateway logboekgebeurtenissen.

| **ID** | **Beschrijving** |
| --- | --- |
| 400 |Een toepassingsfout die geen een specifieke ID |
| 401 |Onjuiste configuratie. Bijvoorbeeld: listenPort = "text" in plaats van een geheel getal zijn |
| 402 |Uitzondering bij het parseren van TLS-handshake berichten |
| 403 |Fout bij het netwerk. Bijvoorbeeld: kan geen verbinding maken met doelserver |
| 100 |Algemene informatie |
| 101 |Service is gestart |
| 102 |Service is gestopt |
| 103 |Een opdracht HTTP-verbinding ontvangen van client |
| 104 |Geen een HTTP-verbinding-opdracht |
| 105 |Doelserver zich niet in de lijst met toegestane of de doelpoort is geen beveiligde poort (443) <br> <br> Zorg ervoor dat de MMA-agent op de gatewayserver en de agents die communiceren met de Gateway zijn verbonden met de dezelfde Log Analytics-werkruimte. |
| 105 |Fout TcpConnection: ongeldig clientcertificaat: CN = Gateway <br><br> Zorg ervoor dat: <br>    <br> &#149;U gebruikmaakt van een Gateway met versienummer 1.0.395.0 of hoger. <br> &#149;De MMA-agent op de gatewayserver en de agents die communiceren met de Gateway zijn verbonden met de dezelfde Log Analytics-werkruimte. |
| 106 |De Log Analytics-gateway biedt alleen ondersteuning voor TLS 1.0, TLS 1.1 en 1.2.  Het biedt geen ondersteuning voor SSL. Voor een niet-ondersteunde TLS/SSL-protocolversie genereert Log Analytics-gateway gebeurtenis-ID 106.|
| 107 |De TLS-sessie is geverifieerd |

**Te verzamelen prestatiemeteritems**

De volgende tabel ziet u de prestatiemeteritems die beschikbaar zijn voor de Log Analytics-gateway. U kunt de tellers die de Prestatiemeter gebruiken toevoegen.

| **Naam** | **Beschrijving** |
| --- | --- |
| Log Analytics Gateway/actieve-clientverbinding |Aantal actieve client-Netwerkverbindingen (TCP) |
| Aantal log Analytics-Gateway/fouten |Aantal fouten |
| Log Analytics-Gateway of verbonden Client |Aantal verbonden clients |
| Log Analytics Gateway/afwijzing tellen |Het aantal weigeringen vanwege een validatiefout TLS |

![Meld u Analytics gateway-prestatiemeteritems](./media/gateway/counters.png)

## <a name="get-assistance"></a>Hulp nodig hebt
Wanneer u bent aangemeld bij Azure portal, maakt u een verzoek om hulp op met de Log Analytics-gateway of een andere Azure-service of functie van een service.
Om hulp vragen, klik op het vraagteken symbool in de rechterbovenhoek van de portal en klik vervolgens op **nieuwe ondersteuningsaanvraag**. Voltooi nieuw aanvraagformulier voor de ondersteuning.

![Nieuw ondersteuningsverzoek](./media/gateway/support.png)

## <a name="next-steps"></a>Volgende stappen
[Gegevensbronnen toevoegen](../../azure-monitor/platform/agent-data-sources.md) te verzamelen van gegevens van uw verbonden bronnen en op te slaan in uw Log Analytics-werkruimte.
