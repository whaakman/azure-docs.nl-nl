---
title: Verbinding maken via de OMS-Gateway | Microsoft Docs
description: Verbinding maken met uw apparaten en computers Operations Manager bewaakt met de OMS-Gateway om gegevens te verzenden naar de Azure Automation en Log Analytics-service wanneer ze geen toegang tot internet hebben.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: magoedte
ms.openlocfilehash: 66e5444f5346a44cfc8a43cf2b43dbaeacffedc9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="connect-computers-without-internet-access-by-using-the-oms-gateway"></a>Verbinding maken met computers zonder toegang tot internet via de OMS-Gateway
Dit document beschrijft hoe u communicatie met Azure Automation configureert en Log Analytics met behulp van de OMS-Gateway als rechtstreeks verbonden computers of Operations Manager bewaakte computers geen toegang tot internet. De OMS-Gateway is een HTTP-en voorwaarts-proxy die ondersteuning biedt voor HTTP tunneling via de opdracht HTTP-verbinding. Het kan verzamelen van gegevens en verzenden naar Azure Automation en Log Analytics namens de computers zonder toegang tot internet.  

De OMS-Gateway ondersteunt:

* Azure Automation Hybrid Runbook Workers  
* Windows-computers met Microsoft Monitoring Agent rechtstreeks verbonden met een werkruimte voor logboekanalyse
* Linux-computers met de OMS-Agent voor Linux rechtstreeks verbonden met een werkruimte voor logboekanalyse  
* System Center Operations Manager 2012 SP1 met UR7 of Operations Manager 2012 R2 UR3 Operations Manager 2016 en beheergroep van Operations Manager versie 1801 geïntegreerd met Log Analytics  

Als uw IT-beveiligingsbeleid bepaalde computers in uw netwerk verbinding maken met internet (zoals verkooppunt apparaten of servers die ondersteuning bieden voor IT-services) niet is toegestaan, maar ze verbinding te maken met Azure Automation of Log Analytics kunt beheren en controleren, moet u ze kan worden geconfigureerd om te communiceren rechtstreeks met de OMS-Gateway.

 Als deze computers zijn geconfigureerd met de OMS-agent rechtstreeks verbinding maken met een werkruimte voor logboekanalyse, worden alle computers in plaats daarvan communiceren met de OMS-Gateway. De OMS-Gateway worden de gegevens van de agents naar de service rechtstreeks. Het analyseert niet een van de gegevens, terwijl het onderweg is.

Wanneer een beheergroep van Operations Manager is geïntegreerd met Log Analytics, kunnen de beheerservers worden geconfigureerd voor verbinding met de Gateway OMS verzamelde gegevens verzenden en ontvangen van configuratie-informatie. Sommige gegevens, zoals Operations Manager waarschuwingen, beoordeling van de configuratie, exemplaarruimte en capaciteit verzenden Operations Manager-agents naar de beheerserver. Andere grote hoeveelheden gegevens, zoals Logboeken, informatie over prestaties en beveiligingsgebeurtenissen, IIS zijn rechtstreeks naar de OMS-Gateway verzonden.  

Als u een of meer Operations Manager-Gateway-servers die zijn geïmplementeerd in een Perimeternetwerk of andere geïsoleerd netwerk hebt voor het bewaken van niet-vertrouwde systemen, communiceren niet ze met een OMS-Gateway. Operations Manager-gatewayservers kunnen alleen aan een beheerserver rapporteren. 

Wanneer een beheergroep van Operations Manager is geconfigureerd om te communiceren met de OMS-Gateway, wordt de proxy-configuratie-informatie wordt automatisch gedistribueerd naar elke agent beheerde computer die is geconfigureerd voor het verzamelen van gegevens voor logboekanalyse, zelfs als de instelling leeg is.    

Voor maximale beschikbaarheid voor direct verbonden of Operations beheergroepen die met logboekanalyse via de OMS-Gateway communiceren, kunt u Netwerktaakverdeling omgeleid en distributie van het verkeer over meerdere gatewayservers OMS. Als een gatewayserver uitvalt, wordt het verkeer wordt omgeleid naar een ander beschikbaar knooppunt.  

Raden wij u aan de OMS-agent te installeren op de computer waarop de OMS-Gateway-software om te controleren en prestatie- of gebeurtenis gegevens analyseren. Daarnaast helpt de agent bij het identificeren van de service-eindpunten die nodig is om te communiceren met OMS-Gateway.

Alle agents moeten netwerkverbinding met de gateway hebben, zodat ze automatisch gegevens naar en van de gateway kunnen overdragen. Een gateway installeren op een domeincontroller aanbevolen niet.

Het volgende diagram toont de gegevensstroom van directe agents Azure Automation en Log Analytics met behulp van de gateway-server. De configuratie van de proxy van de agent moet dezelfde poort die de OMS-Gateway gebruikt om te communiceren met de service gebruiken.  

![voor directe agentcommunicatie met services diagram](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Het volgende diagram toont de gegevensstroom uit een beheergroep van Operations Manager met logboekanalyse.   

![Operations Manager-communicatie met Log Analytics-diagram](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Vereisten

Wanneer u een computer worden uitgevoerd van de Gateway OMS instelt, zorg er dan voor dat deze bevat de volgende onderdelen:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET Framework 4.5
* Minimum van een 4-core-processor en 8 GB geheugen 

### <a name="language-availability"></a>Beschikbare talen

De OMS-Gateway is beschikbaar in de volgende talen:

- Chinees (Vereenvoudigd)
- Chinees (Traditioneel)
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
De volgende tabel licht het ondersteunde aantal agents die met een gateway-server communiceren. Deze ondersteuning is gebaseerd op agents ongeveer 200 KB aan gegevens uploaden elke 6 seconden. Het gegevensvolume per agent getest is ongeveer 2.7 GB per dag.

|Gateway |Geschatte aantal agents die worden ondersteund|  
|--------|----------------------------------|  
|- CPU: Intel XEON CPU E5-2660 v3 @ 2.6 GHz 2 Cores<br> -Geheugen: 4 GB<br> -Netwerkbandbreedte: 1 Gbps| 600|  
|- CPU: Intel XEON CPU E5-2660 v3 @ 2.6 GHz 4 Cores<br> -Geheugen: 8 GB<br> -Netwerkbandbreedte: 1 Gbps| 1000|  

## <a name="download-the-oms-gateway"></a>Download de OMS-Gateway

Er zijn twee manieren om de nieuwste versie van het installatiebestand van de OMS-Gateway.

1. Downloaden van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Het downloaden van de Azure-portal. Nadat u zich bij de Azure-portal aanmelden, moet u de volgende stappen uitvoeren:  

   1. De lijst met services bladeren en selecteer vervolgens **logboekanalyse**.  
   2. Selecteer een werkruimte.
   3. Uw werkruimte-blade onder **algemene**, selecteer **Quick Start**.
   4. Onder **Kies een gegevensbron verbinding maken met de werkruimte**, selecteer **Computers**.
   5. In de **Direct Agent** Configuratiescherm, selecteer **OMS-Gateway downloaden**.
   
    ![OMS Gateway downloaden](./media/log-analytics-oms-gateway/download-gateway.png)

--OR-- 

   1. Uw werkruimte-blade onder **instellingen**, selecteer **geavanceerde instellingen**.
   2. Selecteer **verbonden gegevensbronnen** > **Windows-Servers**. Selecteer vervolgens **OMS-Gateway downloaden**.

## <a name="install-the-oms-gateway"></a>De OMS-Gateway installeren

Als u wilt een gateway installeren, moet u de volgende stappen uitvoeren. Als u de vorige versie geïnstalleerd, voorheen *Log Analytics doorstuurserver*, wordt dit bijgewerkt naar deze versie.  

1. Selecteer in de doelmap **OMS Gateway.msi**.
2. Op de pagina **Welkom** selecteert u **Volgende**.

 ![Gateway-installatiewizard](./media/log-analytics-oms-gateway/gateway-wizard01.png)
  
3. Op de **License Agreement** pagina **ik ga akkoord met de voorwaarden in deze gebruiksrechtovereenkomst**. Selecteer vervolgens **Volgende**.

4. Op de **poort en proxy-adres** pagina:

   a. Typ het nummer van de TCP-poort moet worden gebruikt voor de gateway. Setup configureert een inkomende regel met dit poortnummer op Windows firewall. De standaardwaarde is 8080. Het geldige bereik van het poortnummer is 1 tot 65535 liggen. Als de invoer niet in dit bereik valt, wordt een foutbericht weergegeven.

   b. Als de server waarop de gateway is geïnstalleerd communiceren via een proxy moet, typ eventueel de proxy waarop de gateway verbinding moet maken. Een voorbeeld hiervan is `http://myorgname.corp.contoso.com:80`, zoals wordt weergegeven in de volgende schermafbeelding. Als u dit veld leeg laat, wordt de gateway probeert rechtstreeks verbinding maken met internet.  Als de proxyserver verificatie vereist, voer een gebruikersnaam en wachtwoord.
   
    ![Gateway-Wizard proxyconfiguratie](./media/log-analytics-oms-gateway/gateway-wizard02.png)

4. Selecteer **Volgende**.

5. Als u niet hebt ingeschakeld voor Microsoft Update, de Microsoft Update-pagina wordt weergegeven en kunt u deze in te schakelen. Maak een keuze en selecteer vervolgens **volgende**. Ga anders verder met de volgende stap.

6. Op de **doelmap** pagina, laat u de standaardmap als **C:\Program Files\OMS Gateway** of typ de locatie waar u de OMS-gateway installeren. Selecteer vervolgens **Volgende**.

7. Op de **gereed voor installatie** pagina **installeren**. Gebruikersaccountbeheer lijken aanvragende machtiging om te installeren. Als u een aanvraag voor machtiging krijgt, selecteert u **Ja**.

8. Nadat setup is voltooid, selecteert u **voltooien**. U kunt controleren of de service wordt uitgevoerd door het openen van de module services.msc om ervoor te zorgen dat **OMS Gateway** wordt weergegeven in de lijst met services, en die de status **met**.

    ![Services – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Netwerktaakverdeling configureren 
U kunt de gateway voor hoge beschikbaarheid configureren met behulp van netwerktaakverdeling (NLB). Microsoft Network Load Balancing of op basis van hardware load balancers gebruiken.  De load balancer beheert verkeer door te leiden van de aangevraagde verbindingen van de OMS-agents of beheerservers van Operations Manager op de knooppunten. Als een gatewayserver uitvalt, wordt het verkeer wordt omgeleid naar andere knooppunten.

Zie voor informatie over het ontwerpen en implementeren van een Windows Server 2016 network load balancing-cluster, [Network load balancing](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  De volgende stappen beschrijven het configureren van een Microsoft network load balancing-cluster.  

1.  Aanmelden met een Administrator-account met de Windows-server die lid is van het NLB-cluster.  

2.  Beheer van netwerktaakverdeling in Serverbeheer openen Selecteer **extra**, en selecteer vervolgens **beheer van netwerktaakverdeling**.

3. Met de rechtermuisknop op het cluster-IP-adres voor een OMS-gatewayserver verbinding met de Microsoft Monitoring Agent is geïnstalleerd, en selecteer vervolgens **Host aan Cluster toevoegen**.

 ![Network Load Balancing Manager--toevoegen Host aan Cluster](./media/log-analytics-oms-gateway/nlb02.png)

4. Geef het IP-adres van de gatewayserver waarmee u verbinding wilt maken.

    ![Beheer van netwerktaakverdeling--netwerk Host aan Cluster toevoegen: verbinding maken](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-the-oms-agent-and-the-operations-manager-management-group"></a>De OMS-agent en de Operations Manager-beheergroep configureren
De volgende secties in dit artikel zijn stappen voor het rechtstreeks verbonden zijn met OMS-agents, een Operations Manager-beheergroep of Azure Automation Hybrid Runbook workers configureren met de OMS-Gateway kan communiceren met Azure Automation of logboekanalyse.  

Zie voor informatie over vereisten en stappen voor het installeren van de OMS-agent op Windows-computers die rechtstreeks verbinding met Log Analytics maken, [verzamelen gegevens van computers in uw omgeving met logboekanalyse](log-analytics-concept-hybrid.md#prerequisites).

 Zie voor Linux-computers [verbinding maken met Linux-computers met logboekanalyse](log-analytics-quick-collect-linux-computer.md). Zie voor informatie die betrekking hebben op de Automation Hybrid Runbook Worker [implementeren Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-the-standalone-oms-agent"></a>De zelfstandige OMS-agent configureren
Zie voor meer informatie over het configureren van een agent voor het gebruik van een proxyserver (die in dit geval is de gateway) [verzamelen gegevens van computers in uw omgeving met logboekanalyse](log-analytics-concept-hybrid.md#prerequisites). Als u meerdere gatewayservers achter een load balancer van netwerk hebt geïmplementeerd, is de configuratie van de OMS-agent-proxy het virtuele IP-adres van de NLB:

![Eigenschappen van Microsoft Monitoring Agent-proxy-instellingen](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager-all-agents-use-the-same-proxy-server"></a>Operations Manager configureren: alle agenten dezelfde proxyserver gebruiken
U configureren Operations Manager om toe te voegen van de gateway-server.  De configuratie van de Operations Manager-proxy wordt automatisch toegepast op alle agents die aan Operations Manager rapporteren, zelfs als de instelling leeg is.  

Als u wilt de OMS-Gateway gebruiken ter ondersteuning van Operations Manager, moeten de volgende onderdelen zijn voldaan:

* Microsoft Monitoring Agent (agentversie **8.0.10900.0** of hoger) op de gatewayserver is geïnstalleerd en geconfigureerd voor de Log Analytics-werkruimten die u wilt communiceren.

* Een gateway die verbinding met internet of een verbinding met een proxyserver die biedt.

> [!NOTE]
> Als u een waarde voor de gateway niet opgeeft, worden de lege waarden geduwd naar alle agents.
> 

Als dit de eerste keer dat de Operations Manager-beheergroep met een werkruimte voor logboekanalyse registreert, zijn de mogelijkheid te geven van de configuratie van de proxy voor de beheergroep niet beschikbaar in de Operations-console. 

De beheergroep heeft met succes worden geregistreerd met de service voordat u deze optie is beschikbaar. Werk de systeem-proxyconfiguratie met behulp van Netsh op hetzelfde systeem waarop u bent met de Operations-console en alle beheerservers in de beheergroep.

1. Open een opdrachtprompt met verhoogde bevoegdheid.

    a. Ga naar **Start**. Typ vervolgens **cmd**.
    
    b. Met de rechtermuisknop op **opdrachtprompt**. Selecteer vervolgens **als administrator uitvoeren**.
    
2. Voer de volgende opdracht en selecteer vervolgens **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Nadat u de integratie met logboekanalyse hebt voltooid, kunt u de wijziging door te voeren `netsh winhttp reset proxy`. Gebruik vervolgens de **proxyserver configureren** optie in de Operations-console voor het opgeven van de OMS-gatewayserver. 

1. Open de Operations Manager-console. Onder **Operations Management Suite**, selecteer **verbinding**. Selecteer vervolgens **proxyserver configureren**.

    ![Operations Manager--proxyserver configureren](./media/log-analytics-oms-gateway/scom01.png)

2. Selecteer **een proxyserver gebruiken voor toegang tot de Operations Management Suite**. Typ vervolgens het IP-adres van de OMS-gatewayserver of het virtuele IP-adres van de NLB. Zorg ervoor dat u met begint de `http://` voorvoegsel.

    ![Operations Manager--proxyserveradres](./media/log-analytics-oms-gateway/scom02.png)

3. Selecteer **Voltooien**. Uw Operations Manager-beheergroep is nu geconfigureerd om te communiceren via de gatewayserver naar de Log Analytics-service.

### <a name="configure-operations-manager-specific-agents-use-proxy-server"></a>Configureer de Operations Manager: specifieke agents proxyserver gebruiken
In omgevingen met grote of complexe mogelijk wilt u alleen specifieke servers (of groepen) de OMS-Gateway-server te gebruiken. Voor deze servers bijwerken u niet Operations Manager-agent rechtstreeks, omdat deze waarde wordt overschreven door de globale waarde voor de beheergroep. In plaats daarvan kunt u de regel die wordt gebruikt voor de push-deze waarden overschrijven.  

> [!NOTE] 
> Deze techniek configuratie kan worden gebruikt om toe te staan het gebruik van meerdere OMS-gatewayservers in uw omgeving. U vereist mogelijk specifieke OMS gatewayservers worden opgegeven op basis van per regio.
>  

1. Open de Operations Manager-console en selecteer vervolgens de **ontwerpen** werkruimte.

2. Selecteer in de werkruimte ontwerpen **regels**. Selecteer vervolgens de **bereik** op de werkbalk Operations Manager. Als deze knop niet beschikbaar is, controleert u of u hebt een object, geen map, dat is geselecteerd in de **bewaking** deelvenster. De **bereik Management Pack-objecten** in het dialoogvenster geeft een lijst met algemene bepaalde klassen, groepen of objecten. 

3. in de **zoekt** veld **Health-Service**. Selecteert u deze uit de lijst. Selecteer **OK**.  

4. Zoeken naar de regel **Advisor Proxy-instelling regel**. Selecteer in de werkbalk Operations-console **overschrijft**. Selecteer vervolgens **overschrijven de Rule\For een specifiek object van klasse: Health-Service**. 

5. Selecteer vervolgens een specifiek object in de lijst. 

6. (Optioneel) Maak een aangepaste groep waarin het object health-service van de servers die u wilt toepassen met deze onderdrukking. Vervolgens kunt u vervolgens de onderdrukking toepassen aan die groep.

7. In de **Onderdrukkingseigenschappen** in het dialoogvenster, klikt u op om een vinkje in de **overschrijven** kolom naast de **WebProxyAddress** parameter.  In de **Onderdrukkingswaarde** en voer de URL van de OMS-gatewayserver ervoor te zorgen dat deze met begint de `http://` voorvoegsel.  

    >[!NOTE]
    > U hoeft niet te regel inschakelen. Er wordt al automatisch beheerd met een overschrijving die is opgenomen in het Microsoft System Center Advisor beveiligde verwijzing Override management pack die de Microsoft System Center Advisor Monitoring Server-groep is gericht.
    >   

8. Selecteer een management pack uit de **selecteert bestemmingsmanagement pack** lijst of maak een nieuw niet-verzegeld management pack door te selecteren **nieuw**. 

9. Wanneer u uw wijzigingen hebt voltooid, selecteert u **OK**. 

### <a name="configure-the-oms-gateway-for-automation-hybrid-runbook-workers"></a>De OMS-Gateway voor Automation Hybrid Runbook Workers configureren
Als u Automation Hybrid Runbook Workers in uw omgeving hebt, u volgt de handmatige, tijdelijke oplossingen voor het configureren van de Gateway OMS om u te ondersteunen.

U moet weten welke Azure-regio waarin het Automation-account zich bevindt in de volgende stappen. Als u wilt zoeken naar de locatie, moet u de volgende stappen uitvoeren:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer de Azure Automation-service.
3. Selecteer het juiste Azure Automation-account.
4. Onder **locatie**, de regio van het account te bekijken.

    ![Azure-portal--locatie van Automation-account](./media/log-analytics-oms-gateway/location.png)  

Gebruik de volgende tabellen in de URL voor elke locatie te identificeren:

**Taak runtime data service-URL 's**

| **Locatie** | **URL** |
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

| **Locatie** | **URL** |
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

Als uw computer wordt automatisch geregistreerd als een hybride Runbook Worker voor patch-doeleinden met behulp van de Update Management-oplossing, volg deze stappen:

1. De taakgegevens Runtime service-URL's toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Typ bijvoorbeeld het volgende: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

2. De OMS-Gateway-service opnieuw starten met de volgende PowerShell-cmdlet: `Restart-Service OMSGatewayService`

Als u ingebouwde Azure Automation als uw computer met de cmdlet registratie van hybride Runbook Worker volgt te werk:

1. De agent service registratie-URL toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Typ bijvoorbeeld het volgende: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`

2. De taakgegevens Runtime service-URL's toevoegen aan de lijst met toegestane Host op de OMS-Gateway. Typ bijvoorbeeld het volgende: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

3. De OMS-Gateway-service opnieuw starten:  `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Handig PowerShell-cmdlets
Cmdlets kunt u taken uitvoeren die nodig zijn voor het bijwerken van de configuratieinstellingen van de OMS-Gateway. Voordat u ze gebruiken, moet u de volgende stappen uit:

1. Installeer de OMS-Gateway (MSI).
2. Open een PowerShell-consolevenster.
3. De module importeren met deze opdracht te typen: `Import-Module OMSGateway`.
4. Als er is geen fout in de vorige stap opgetreden, de module is geïmporteerd en de cmdlets kunnen worden gebruikt. Typ `Get-Module OMSGateway`.
5. Nadat u wijzigingen aanbrengt met behulp van de cmdlets, zorg ervoor dat u de OMS-Gateway-service opnieuw opstarten.

Als u een fout in stap 3 krijgt, wordt de module is niet geïmporteerd. De fout kan optreden wanneer PowerShell is niet gevonden van de module. U kunt deze vinden in de OMS-Gateway-installatiepad: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **Cmdlet** | **Parameters** | **Beschrijving** | **Voorbeeld** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Sleutel |De configuratie van de service opgehaald |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Sleutel (vereist) <br> Waarde |De configuratie van de service wordt gewijzigd |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Het adres van de relay (upstream) proxy opgehaald |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Gebruikersnaam<br> Wachtwoord |Hiermee stelt u het adres (en de referentie) van de relay (upstream)-proxy |1. Stel een relay-proxy en referentie:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Stel een relay-proxy waarvoor geen verificatie nodig: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Schakel de relay proxy-instellingen:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |De host momenteel toegestane opgehaald (alleen lokaal geconfigureerde toegestaan host; bevat geen automatisch gedownloade toegestane hosts) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (vereist) |Voegt de host aan de lijst met toegestane |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (vereist) |Hiermee verwijdert u de host uit de lijst met toegestane |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Onderwerpnaam (vereist) |Het clientcertificaat onderworpen aan de lijst met toegestane toegevoegd |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Onderwerpnaam (vereist) |Hiermee verwijdert u het certificaatonderwerp client uit de lijst met toegestane |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Objecten van het certificaat van de momenteel toegestane client opgehaald (alleen lokaal geconfigureerde toegestaan onderwerpen; bevat geen automatisch gedownloade toegestane onderwerpen) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problemen oplossen
U moet ook beschikken over de OMS-agent is geïnstalleerd voor het verzamelen van gebeurtenissen die worden vastgelegd door de gateway.

![Logboeken--OMS Gateway-logboek](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gateway gebeurtenis-id's en beschrijvingen**

De volgende tabel ziet u de gebeurtenis-id's en beschrijvingen voor logboekgebeurtenissen OMS-Gateway:

| **ID** | **Beschrijving** |
| --- | --- |
| 400 |Een toepassingsfout die geen een specifieke-ID. |
| 401 |Onjuiste configuratie. Bijvoorbeeld: listenPort = 'text' in plaats van een geheel getal. |
| 402 |Uitzondering bij het parseren van TLS-handshake berichten. |
| 403 |Fout bij het netwerk. Bijvoorbeeld: kan geen verbinding met de doelserver. |
| 100 |Algemene informatie. |
| 101 |Service is gestart. |
| 102 |Service is gestopt. |
| 103 |Een opdracht HTTP-verbinding ontvangen van de client. |
| 104 |Geen een HTTP-verbinding-opdracht. |
| 105 |Doelserver zich niet in de lijst met toegestane of de doelpoort is geen beveiligde poort (443). <br> <br> Zorg ervoor dat de MMA-agent op de gatewayserver en de agents die met de Gateway communiceren zijn verbonden met de dezelfde werkruimte voor logboekanalyse. |
| 105 |Fout TcpConnection: ongeldige clientcertificaat: CN = Gateway <br><br> Zorg ervoor dat: <br>    <br> -U gebruikmaakt van een Gateway met versienummer 1.0.395.0 of hoger. <br> -De MMA-agent op de gatewayserver en de agents die met de gateway communiceren zijn verbonden met de dezelfde werkruimte voor logboekanalyse. |
| 106 |De OMS-Gateway ondersteunt alleen TLS 1.0, TLS 1.1 en 1.2.  SSL ondersteunt niet. Voor een niet-ondersteunde TLS/SSL-protocolversie genereert OMS-Gateway gebeurtenis-ID 106.|
| 107 |De TLS-sessie is geverifieerd. |

**Te verzamelen prestatiemeteritems**

De volgende tabel toont de prestatiemeteritems die beschikbaar voor de OMS-Gateway zijn. U kunt de items toevoegen met behulp van de Prestatiemeter.

| **Naam** | **Beschrijving** |
| --- | --- |
| Clientverbinding OMS-Gateway/actief |Aantal actieve client-Netwerkverbindingen (TCP) |
| Aantal OMS-Gateway/fouten |Aantal fouten |
| OMS-Gateway of verbonden Client |Aantal verbonden clients |
| OMS Gateway/afwijzing tellen |Het aantal weigeringen vanwege een validatiefout TLS |

![Prestatiemeteritems OMS-Gateway](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>U hulp nodig hebt
Wanneer u bent aangemeld bij de Azure portal, kunt u een verzoek om hulp kunt maken met de OMS-Gateway of andere Azure-service of functie van een service.

Om hulp vragen, selecteert u het symbool vraagteken in de rechterbovenhoek van de portal. Selecteer vervolgens **nieuw ondersteuningsverzoek**. Voltooi het nieuwe aanvraagformulier voor ondersteuning.

![Nieuw ondersteuningsverzoek](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Volgende stappen
[Voeg gegevensbronnen toe](log-analytics-data-sources.md) voor het verzamelen van gegevens uit uw verbonden bronnen en opslaan in de werkruimte voor logboekanalyse.
