---
title: Installeren van de lokale data gateway - Azure Logic Apps | Microsoft Docs
description: Voordat u toegang gegevensbronnen on-premises tot, installeert u de lokale data gateway voor snelle gegevensoverdracht en -versleuteling tussen on-premises gegevensbronnen en logic apps
keywords: toegang tot gegevens, op de lokale, gegevensoverdracht, versleuteling en gegevensbronnen
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/14/2017
ms.author: LADocs; millopis; estfan
ms.openlocfilehash: b3c1e2afadea91f010c3e4b43206b6d30a75ec38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a>De lokale data gateway voor Azure Logic Apps installeren

Om uw logische apps toegang on-premises gegevensbronnen tot, moet u het installeren en instellen van de lokale data gateway. De gateway fungeert als een brug waarmee snelle gegevensoverdracht en -versleuteling tussen on-premises systemen en uw logische apps. De gateway stuurt gegevens van lokale bronnen op gecodeerde kanalen via de Azure Service Bus. Al het verkeer afkomstig is als beveiligde uitgaand verkeer van de gateway-agent. Meer informatie over [de werking van de gegevensgateway](#gateway-cloud-service).

De gateway ondersteunt verbindingen met deze gegevensbronnen on-premises:

*   BizTalk Server 2016
*   DB2  
*   Bestandssysteem
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP-toepassingsserver 
*   SAP-berichtenserver
*   SharePoint
*   SQL Server
*   Teradata

Deze stappen laten zien hoe u de lokale data gateway voordat u het eerst installeert [instellen van een verbinding tussen de gateway en uw logische apps](./logic-apps-gateway-connection.md). Zie voor meer informatie over ondersteunde connectors [Connectors voor Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list). 

Zie voor informatie over het gebruik van de gateway met andere services, deze artikelen:

*   [Microsoft Power BI lokale gegevensgateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services op locatie gegevensgateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft-Flow lokale gegevensgateway](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps lokale gegevensgateway](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>

## <a name="requirements"></a>Vereisten

**Minimale**:

* .NET 4.5 framework
* 64-bits versie van Windows 7 of Windows Server 2008 R2 (of hoger)

**Aanbevolen**:

* 8-core CPU
* 8 GB geheugen
* 64-bits versie van Windows 2012 R2 (of hoger)

**Belangrijke overwegingen**:

* De lokale data gateway alleen op een lokale computer geïnstalleerd.
U kunt de gateway niet installeren op een domeincontroller.

   > [!TIP]
   > Er geen gateway installeren op dezelfde computer als uw gegevensbron. Om latentie te beperken, kunt u de gateway zo dicht mogelijk tot de gegevensbron, of op dezelfde computer, ervan uitgaande dat u gemachtigd installeren.

* Installeer de gateway niet op een computer die wordt uitgeschakeld, gaat u naar de slaapstand, of geen verbinding maken met Internet, omdat de gateway kan niet worden uitgevoerd in deze omstandigheden. Ook de prestaties van de gateway via een draadloos netwerk kan afnemen.

* Tijdens de installatie, moet u zich aanmelden met een [werk- of schoolaccount](https://docs.microsoft.com/azure/active-directory/sign-up-organization) die wordt beheerd door Azure Active Directory (Azure AD), niet in een Microsoft-account.

  > [!TIP]
  > Als u wilt gebruiken een Microsoft-account met een Visual Studio met MSDN-abonnement, eerst [(tenant) van een map maken in Azure Active Directory](../active-directory/develop/active-directory-howto-tenant.md) met uw Microsoft-account of gebruik de standaardmap. Een gebruiker met een wachtwoord naar de map, en vervolgens toe aan die gebruikerstoegang geven tot uw abonnement. U kunt vervolgens weer aanmelden tijdens de installatie van de gateway met deze gebruikersnaam en wachtwoord.

  Wanneer u maken en koppelen van een gateway-resource met de gateway-installatie, moet u dezelfde werk- of schoolaccount later in de Azure portal gebruiken. Selecteer deze gateway resource vervolgens in bij het maken van de verbinding tussen uw logische app en de on-premises gegevensbron. [Waarom moet ik een Azure AD werk- of schoolaccount?](#why-azure-work-school-account)

  > [!TIP]
  > Als u zich registreerde voor een Office 365-aanbieding en uw werkelijke Werke-mailadres niet opgeven, uw adres aanmelden als volgt uitzien jeff@contoso.onmicrosoft.com. 

* Als u een bestaande gateway die u hebt ingesteld met een installatieprogramma dat ouder is dan versie 14.16.6317.4 hebt, kunt u uw gateway locatie niet wijzigen door het uitvoeren van de meest recente versie installer. Echter, kunt u het meest recente installatieprogramma voor het instellen van een nieuwe gateway met de locatie die u in plaats daarvan wilt.
  
  Als u een gateway-installatieprogramma dat ouder is dan versie 14.16.6317.4 hebben, maar u kunt uw gateway nog niet geïnstalleerd maar kunt u downloaden en het meest recente installatieprogramma gebruiken.

<a name="install-gateway"></a>

## <a name="install-the-data-gateway"></a>De gegevensgateway installeren

1.  [Downloaden en uitvoeren van het installatieprogramma van de gateway op een lokale computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Lees en accepteer de gebruiksvoorwaarden en de privacyverklaring.

3. Geef het pad op de lokale computer waarop u wilt installeren van de gateway.

4. Wanneer u wordt gevraagd, meld u aan met uw Azure werk- of schoolaccount, niet in een Microsoft-account.

   ![Meld u aan met Azure werk- of schoolaccount](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Nu registreren van uw gateway is geïnstalleerd met de [gateway-cloudservice](#gateway-cloud-service). Kies **registreren van een nieuwe gateway op deze computer**.

   De gateway cloudservice versleutelt en slaat de referenties van de gegevensbron en de details van de gateway. 
   De service Routering ook query's en de resultaten tussen uw logische app, de lokale data gateway en uw gegevensbron on-premises.

6. Geef een naam voor uw gateway-installatie. Een herstelsleutel maken en bevestig vervolgens uw herstelsleutel. 

   > [!IMPORTANT] 
   > De herstelsleutel moet ten minste acht tekens bevatten. Zorg ervoor dat u opslaat en de sleutel op een veilige plaats bewaren. U moet deze sleutel ook als u wilt migreren, herstellen of overnemen van een bestaande gateway.

   1. Als u de standaardregio voor de gateway-cloudservice en de Azure Service Bus gebruikt door de gateway-installatie wijzigen kiezen **regio wijzigen**.

      ![Wijziging regio](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      De standaardregio is de regio die is gekoppeld aan uw Azure AD-tenant.

   2. Open op het volgende venster de **regio selecteren** kiezen van een andere regio.

      ![Selecteer een andere regio](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      U kunt bijvoorbeeld dezelfde regio als uw logische app selecteren of de regio die het dichtst bij uw on-premises gegevensbron selecteren zodat de latentie te verminderen. Uw gateway en logica voor resource app hebben verschillende locaties.

      > [!IMPORTANT]
      > U kunt deze regio niet wijzigen na de installatie. Deze regio bepaalt ook en Hiermee beperkt u de locatie waar u de Azure-resource voor uw gateway kunt maken. Dus als u uw gateway-resource in Azure maakt, zorg ervoor dat de Resourcelocatie overeenkomt met de regio die u hebt geselecteerd tijdens de installatie van de gateway.
      > 
      > Als u wilt een andere regio voor uw gateway om later te gebruiken, moet u een nieuwe gateway instellen.

   3. Als u klaar bent, kiest u **gedaan**.

7. Nu als volgt te werk in de Azure portal zodat u kunt [maken van een Azure-resource voor uw gateway](../logic-apps/logic-apps-gateway-connection.md). 

Meer informatie over [de werking van de gegevensgateway](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Migreren, herstellen of overnemen van een bestaande gateway

Om deze taken uitvoeren, moet u de herstelsleutel die is opgegeven voor de gateway is geïnstalleerd hebben.

1. Kies in het menu Start uw computer **On-premises gegevensgateway**.

2. Nadat het installatieprogramma wordt geopend, meld u aan met hetzelfde Azure werk- of schoolaccount dat eerder werd gebruikt voor het installeren van de gateway.

3. Kies **migreren, herstellen of overnemen van een bestaande gateway**.

4. Geef de naam en herstel sleutel voor de gateway die u wilt migreren, herstellen of overnemen.

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows-serviceaccount

De lokale data gateway wordt uitgevoerd als een Windows-service en is ingesteld om te gebruiken `NT SERVICE\PBIEgwService` aanmeldingsreferenties voor het Windows-service. De gateway heeft standaard het recht 'Aanmelden als service' voor de computer waarop u de gateway installeert. Als u wilt maken en onderhouden van de gateway in de Azure portal, de Windows-serviceaccount moet ten minste beschikken over **Inzender** machtigingen. 

> [!NOTE]
> Het Windows-service-account verschilt van het account dat wordt gebruikt voor het verbinden met on-premises gegevens bronnen, en van de Azure werk- of schoolaccount gebruikt voor aanmelding bij cloud-services.

<a name="restart-gateway"></a>

## <a name="restart-the-gateway"></a>Start de gateway opnieuw

U kunt net als elke andere Windows-service starten en stoppen van de service op verschillende manieren. U kunt bijvoorbeeld open een opdrachtprompt met verhoogde machtigingen op de computer waarop de gateway wordt uitgevoerd, en beide deze opdrachten uitvoeren:

* Voor het stoppen van de service, moet u deze opdracht uitvoeren:
  
    `net stop PBIEgwService`

* De service wilt starten, moet u deze opdracht uitvoeren:
  
    `net start PBIEgwService`

## <a name="configure-a-firewall-or-proxy"></a>Een firewall of proxy configureren

De gateway maakt een uitgaande verbinding met [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Om proxyinformatie te verstrekken voor uw gateway, Zie [proxy-instellingen configureren](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

Controleer of uw machine daadwerkelijk verbinding met internet maken kan om te controleren of uw firewall of proxy, verbindingen mogelijk blokkeren, en de [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Vanuit een PowerShell-prompt, moet u deze opdracht uitvoeren:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Met deze opdracht wordt enkel tests netwerkverbinding en een verbinding met de Azure Service Bus. De opdracht heeft dus niets te maken met de gateway of de gateway-cloudservice die versleutelt en slaat uw referenties en de details van de gateway geen. 
>
> Ook deze opdracht is alleen beschikbaar op Windows Server 2012 R2 of hoger, en Windows 8.1 of hoger. In eerdere versies van het besturingssysteem, kunt u Telnet-connectiviteit testen. Meer informatie over [Azure Service Bus- en hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

De resultaten moeten uitzien in dit voorbeeld:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Als **TcpTestSucceeded** niet is ingesteld op **True**, u mogelijk geblokkeerd door een firewall. Als u worden uitgebreid wilt, vervangen door de **ComputerName** en **poort** waarden met de waarden die worden vermeld onder [poorten configureren](#configure-ports) in dit onderwerp.

De firewall blokkeren mogelijk ook verbindingen waarmee de Azure Service Bus in de Azure-datacenters. Als u dit scenario gebeurt, goedkeuren (deblokkeren) de IP-adressen die datacenters in uw regio. Voor deze IP-adressen [ophalen van de Azure-IP-adressen lijst hier](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Poorten configureren

De gateway maakt een uitgaande verbinding met [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) en op uitgaande poorten communiceert: TCP 443 (standaard), 5671, 5672, 9350 via 9354. De gateway nodig niet poorten voor inkomend verkeer. Meer informatie over [Azure Service Bus- en hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMEINNAMEN | UITGAANDE POORTEN | BESCHRIJVING |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *. servicebus.windows.net | 5671-5672 | Geavanceerde Message Queuing-Protocol (AMQP) | 
| *. servicebus.windows.net | 443, 9350-9354 | Listeners op Service Bus Relay via TCP (443 voor toegangsbeheer token aanschaf vereist) | 
| *. frontend.clouddatahub.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| Login.microsoftonline.com | 443 | HTTPS | 
| *. msftncsi.com | 443 | Gebruikt voor het testen van verbinding met internet wanneer de gateway onbereikbaar zijn door de Power BI-service is. | 

Als u goed te keuren IP-adressen in plaats van de domeinen hebt, kunt u downloaden en gebruiken de [Microsoft Azure Datacenter IP-adresbereiken lijst](https://www.microsoft.com/download/details.aspx?id=41653). In sommige gevallen worden de Azure Service Bus-verbindingen gemaakt met IP-adres in plaats van een volledig gekwalificeerde domeinnamen.

<a name="gateway-cloud-service"></a>
## <a name="how-does-the-data-gateway-work"></a>Hoe werkt de gegevensgateway?

De gegevensgateway vergemakkelijkt de snelle en veilige communicatie tussen uw logische app, het gateway-cloudservice en uw on-premises gegevensbron. 

![diagram-for-on-premises-Data-gateway-flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Wanneer de gebruiker in de cloud werkt met een element dat verbonden met uw on-premises gegevensbron:

1. De gateway cloudservice maakt een query, samen met de versleutelde referenties voor de gegevensbron en verzendt de query naar de wachtrij voor de gateway om te verwerken.

2. De gateway cloudservice analyseert de query en de aanvraag verstuurd naar de Azure Service Bus.

3. De gegevensgateway lokale Azure Service Bus voor aanvragen in behandeling worden opgevraagd.

4. De gateway opgehaald van de query, ontsleutelt de referenties en verbinding maakt met de gegevensbron met deze referenties.

5. De gateway stuurt de query met de gegevensbron voor uitvoering.

6. De resultaten worden uit de gegevensbron verzonden terug naar de gateway en vervolgens naar het gateway-cloudservice. De gateway-cloudservice gebruikt vervolgens de resultaten.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

**Q**: moet ik een gateway voor gegevensbronnen in de cloud, zoals SQL Azure? <br/>
**Een**: Nee. Een gateway maakt verbinding met on-premises gegevensbronnen alleen.

**Q**: de gateway hoeft te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**Een**: Nee. De gateway maakt verbinding met de gegevensbron met de verbindingsgegevens die is opgegeven. Houd rekening met de gateway als een clienttoepassing in deze zin. De gateway moet alleen de mogelijkheid om te verbinden met de naam van de server die is opgegeven.

<a name="why-azure-work-school-account"></a>

**Q**: Waarom moet ik een Azure werk- of schoolaccount aan te melden? <br/>
**Een**: U kunt alleen een Azure werk- of schoolaccount bij het installeren van de lokale data gateway. Uw account wordt opgeslagen in een tenant die wordt beheerd door Azure Active Directory (Azure AD). Uw Azure AD-account UPN (user Principal name) normaal gesproken komt overeen met het e-mailadres.

**Q**: waar mijn referenties worden opgeslagen? <br/>
**Een**: de referenties die u voor een gegevensbron opgeeft zijn versleuteld en opgeslagen in de gateway-cloudservice. De referenties worden op de lokale data gateway ontsleuteld.

**Q**: zijn er vereisten voor de bandbreedte van het netwerk? <br/>
**Een**: het is raadzaam dat de netwerkverbinding goed doorvoer heeft. Elke omgeving is verschillend en de hoeveelheid gegevens die worden verzonden, is van invloed op de resultaten. Met behulp van ExpressRoute kan helpen om bescherming te bieden een mate van doorvoer tussen on-premises en de Azure-datacenters.
U kunt de hulpprogramma van derden Azure snelheid testen-app gebruiken om te meten wat de doorvoer.

**Q**: Wat is de latentie voor het uitvoeren van query's met een gegevensbron van de gateway? Wat is de beste architectuur? <br/>
**Een**: als u netwerklatentie, kunt u de gateway installeren zo dicht mogelijk bij de gegevensbron mogelijk. Als u de gateway op de werkelijke gegevensbron installeren kunt, minimaliseert deze nabijheid de latentie geïntroduceerd. Houd rekening met de datacentra te. Bijvoorbeeld, als uw service het datacenter VS-West gebruikt en u SQL Server gehost in een virtuele machine in Azure hebt, uw Azure VM moet zijn in de VS-West te. Deze nabijheid minimaliseert latentie en vermijdt u kosten voor uitgaande op de virtuele machine in Azure.

**Q**: hoe resultaten, verzonden naar de cloud? <br/>
**Een**: resultaten worden verzonden via de Azure Service Bus.

**Q**: zijn er geen inkomende verbindingen met de gateway vanuit de cloud? <br/>
**Een**: Nee. Uitgaande verbindingen naar Azure Service Bus maakt gebruik van de gateway.

**Q**: Wat gebeurt er als ik uitgaande verbindingen blokkeren? Wat moet ik openen? <br/>
**Een**: Zie de poorten en hosts die gebruikmaakt van de gateway.

**Q**: de werkelijke Windows-service zogenaamd?<br/>
**Een**: In Services Power BI Enterprise Gateway-Service voor de gateway wordt aangeroepen.

**Q**: kan de gateway Windows-service worden uitgevoerd met Azure Active Directory-account? <br/>
**Een**: Nee. De Windows-service moet een geldige Windows-account hebben. De service wordt standaard uitgevoerd met de Service-SID NT SERVICE\PBIEgwService.

### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen

**Q**: welke opties zijn beschikbaar voor herstel na noodgevallen? <br/>
**Een**: U kunt de herstelsleutel gebruiken om te zetten of verplaatsen van een gateway. Wanneer u de gateway installeren, geeft u de herstelsleutel.

**Q**: Wat is het voordeel van de herstelsleutel? <br/>
**Een**: de herstelsleutel biedt een manier om te migreren of de gateway-instellingen herstellen na een noodgeval.

**Q**: zijn er plannen voor het inschakelen van hoge beschikbaarheid scenario's met de gateway? <br/>
**Een**: deze scenario's zijn op de roadmap, maar we een tijdlijn nog geen hebt.

## <a name="troubleshooting"></a>Problemen oplossen

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: hoe kan ik zien wat query's worden verzonden naar de on-premises gegevensbron? <br/>
**Een**: U kunt inschakelen querytracering, waaronder de query's die worden verzonden. Vergeet niet om te wijzigen van de query traceren terug naar de oorspronkelijke waarde gedaan bij het oplossen van problemen. Querytracering ingeschakeld verlaten maakt grotere Logboeken.

U kunt ook hulpprogramma's die de gegevensbron voor tracering van query's heeft bekijken. U kunt bijvoorbeeld de Extended Events of SQL Profiler gebruiken voor SQL Server en Analysis Services.

**Q**: waar zich de logboeken van de gateway? <br/>
**Een**: Zie's verderop in dit onderwerp.

### <a name="update-to-the-latest-version"></a>Bijwerken naar de meest recente versie

Veel problemen kunnen surface wanneer de gatewayversie is verouderd. Zorg ervoor dat u met de meest recente versie als goed over het algemeen. Als u kunt de gateway nog niet hebt bijgewerkt, gedurende een maand of langer, kunt u Overweeg de installatie van de meest recente versie van de gateway en zien als u het probleem kan reproduceren.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fout: Kan gebruiker niet toevoegen aan groep. (-2147463168 PBIEgwService Prestatielogboekgebruikers)

U kunt deze fout kan ophalen, als u probeert de gateway installeren op een domeincontroller wordt niet ondersteund. Zorg ervoor dat u de gateway op een computer die een domeincontroller niet implementeert.

## <a name="tools"></a>Hulpprogramma's

### <a name="collect-logs-from-the-gateway-configurer"></a>Verzamelen van Logboeken van de gateway configurer

U kunt verschillende logboeken voor de gateway verzamelen. Begin altijd met de logboeken!

#### <a name="installer-logs"></a>Installatieprogramma Logboeken

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Configuratielogboeken

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway-service-Logboeken

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Gebeurtenislogboeken

U vindt de logboeken van de Data Management Gateway en PowerBIGateway onder **servicelogboeken voor toepassingen en**.

### <a name="fiddler-trace"></a>Fiddler traceren

[Fiddler](http://www.telerik.com/fiddler) is een gratis hulpprogramma uit Telerik die HTTP-verkeer wordt bewaakt. U ziet dit verkeer met de Power BI-service van de clientcomputer. Deze service mogelijk fouten en andere gerelateerde informatie weergeven.

## <a name="next-steps"></a>Volgende stappen
    
* [Verbinding maken met on-premises gegevens vanuit logic apps](../logic-apps/logic-apps-gateway-connection.md)
* [Enterprise integration-functies](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Connectors voor Azure Logic Apps](../connectors/apis-list.md)
