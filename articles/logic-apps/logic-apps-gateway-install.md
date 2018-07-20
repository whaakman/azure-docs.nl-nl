---
title: On-premises gegevensgateway - Azure Logic Apps installeren | Microsoft Docs
description: Het downloaden en installeren van de on-premises gegevensgateway voordat u toegang gegevens on-premises vanuit logische apps tot
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/20/2018
ms.reviewer: yshoukry, LADocs
ms.suite: integration
ms.openlocfilehash: 09e3879ed91a0e9c6d27940cae53f3e3f0397d7b
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145203"
---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a>De on-premises gegevensgateway installeren voor Azure Logic Apps

Voordat u uw logische apps verbinding met on-premises gegevensbronnen maken kunt, downloaden en installeren van de on-premises gegevensgateway op een lokale computer. De gateway werkt als een brug waarmee snelle gegevensoverdracht en gegevensversleuteling tussen gegevensbronnen on-premises (niet in de cloud) en uw logische apps. Dit artikel wordt beschreven hoe u kunt downloaden, installeren en instellen van uw on-premises gegevensgateway. 

U kunt dezelfde gatewayinstallatie gebruiken met andere services, zoals Power BI, Microsoft Flow, PowerApps en Azure Analysis Services. Meer informatie over [de werking van de data gateway](#gateway-cloud-service).

<a name="supported-connections"></a>

De gateway ondersteunt [on-premises connectors](../connectors/apis-list.md#on-premises-connectors) in Azure Logic Apps voor deze gegevensbronnen:

*   BizTalk Server 2016
*   Bestandssysteem
*   IBM DB2  
*   IBM Informix
*   IBM MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP Application Server 
*   SAP Message Server
*   SharePoint Server
*   SQL Server
*   Teradata

Zie de volgende artikelen voor meer informatie over het gebruik van de gateway met andere services:

* [Microsoft Power BI on-premises gegevensgateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft PowerApps on-premises gegevensgateway](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Microsoft Flow on-premises gegevensgateway](https://flow.microsoft.com/documentation/gateway-manage/)
* [Azure Analysis Services on-premises gegevensgateway](../analysis-services/analysis-services-gateway.md)

<a name="requirements"></a>

## <a name="prerequisites"></a>Vereisten

* Een [werk- of schoolaccount](../active-directory/fundamentals/sign-up-organization.md) waarvoor een [Azure-abonnement](https://docs.microsoft.com/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer). Tijdens de gatewayinstallatie van de aanmelden u bij dit account, zodat u de installatie van de gateway aan uw Azure-abonnement koppelen kunt. Later kunt u ook gebruiken hetzelfde account wanneer u een Azure-resource voor uw gatewayinstallatie in de Azure-portal maakt. Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* Hier volgen de vereisten voor de lokale computer:

  **Minimale vereisten**
  * .NET Framework 4.5.2
  * 64-bits versie van Windows 7 of Windows Server 2008 R2 (of hoger)

  **Aanbevolen vereisten**
  * 8-core CPU
  * 8 GB geheugen
  * 64-bits versie van Windows Server 2012 R2 (of hoger)

* **Belangrijke overwegingen**

  * U kunt de on-premises gegevensgateway installeren op een lokale computer, niet een domeincontroller. U moet echter niet de gateway installeren op dezelfde computer als de gegevensbron. Bovendien hebt u slechts één gateway nodig voor alle uw gegevensbronnen, dus niet nodig voor het installeren van de gateway voor elke gegevensbron.

    > [!TIP]
    > Om latentie te beperken, kunt u de gateway zo dicht mogelijk bij uw gegevensbron, of op dezelfde computer, ervan uitgaande dat u gemachtigd installeren.

  * De gateway installeren op een computer die *niet* uitschakelen, gaat u naar de slaapstand of verbinding maken met Internet. De gateway kan niet worden uitgevoerd onder deze omstandigheden. 
  Prestaties van een gateway kan ook een draadloos netwerk afnemen.

  * Tijdens de installatie, kunt u alleen zich aanmelden met een [werk- of schoolaccount](../active-directory/sign-up-organization.md) die wordt beheerd door Azure Active Directory (Azure AD) en niet een Microsoft-account. 
  Controleer ook of dit account is niet van een Azure B2B (Gast)-account. 
  U moet ook de dezelfde aanmeldingsaccount gebruiken in Azure portal wanneer u de gatewayinstallatie registreren met het maken van een Azure-resource voor uw gateway. 
  Vervolgens kunt u deze gateway-resource selecteren wanneer u de verbinding van uw logische app met uw on-premises gegevensbron maken. 
  [Waarom moet ik een Azure AD werk- of schoolaccount?](#why-azure-work-school-account)

  > [!TIP]
  > Als u zich hebt geregistreerd voor een Office 365-aanbieding en niet uw zakelijke e-mailadres opgeven, hebt u mogelijk een aanmelding-adres dat in dit voorbeeld lijkt: `username@domain.onmicrosoft.com` 
  >
  > Gebruik van een Microsoft-account met een [abonnement op Visual Studio Standard](https://visualstudio.microsoft.com/vs/pricing/), eerste [Maak een map (tenant) in Azure Active Directory](../active-directory/develop/active-directory-howto-tenant.md), of de standaard-map met uw Microsoft-account gebruiken. 
  > Een gebruiker met een wachtwoord naar de map toevoegen en vervolgens dat gebruikerstoegang geeft tot uw abonnement. 
  > Vervolgens kunt u zich aanmeldt tijdens de gatewayinstallatie van de met deze gebruikersnaam en wachtwoord.

  * De regio die u voor de gatewayinstallatie selecteert bepaalt de locatie waar u later uw gateway in Azure registreren met het maken van een Azure-resource. 
  Wanneer u deze gateway-resource in Azure maakt, moet u de *dezelfde* locatie als de gatewayinstallatie. De standaardregio is dezelfde locatie als uw Azure AD-tenant die uw Azure-account beheert, maar u kunt de locatie wijzigen tijdens de installatie van de gateway.

  * Als u al een gateway die u hebt ingesteld met een installatieprogramma ouder dan versie 14.16.6317.4 hebt, kunt u de locatie van uw gateway niet wijzigen door het uitvoeren van het nieuwste installatieprogramma. Echter, kunt u het nieuwste installatieprogramma van het instellen van een nieuwe gateway met de locatie die u in plaats daarvan wilt.
  
    Als u een gateway-installatieprogramma dat ouder is dan versie 14.16.6317.4 hebben, maar u kunt uw gateway nog niet hebt geïnstalleerd, kunt u downloaden en gebruik in plaats daarvan het nieuwste installatieprogramma van.

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Een gegevensgateway installeren

1. [Downloaden, opslaan en uitvoeren van het installatieprogramma van de gateway op een lokale computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Accepteer het standaardpad voor installatie, of geef de locatie op de computer waar u de gateway te installeren.

3. Controleer en accepteer de gebruiksvoorwaarden en privacyverklaring en kies vervolgens **installeren**.

   ![Gebruiksvoorwaarden en privacyverklaring accepteren](./media/logic-apps-gateway-install/accept-terms.png)

4. Nadat de gateway is geïnstalleerd, geef het e-mailadres voor uw account voor werk of school op en kies **aanmelden**.

   ![Meld u aan met de werk-of schoolaccount](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Kies **een nieuwe gateway registreren op deze computer** > **volgende**, waardoor wordt geregistreerd met de gatewayinstallatie met de [gateway-cloudservice](#gateway-cloud-service). 

   ![Gateway registreren](./media/logic-apps-gateway-install/register-new-gateway.png)

6. Geef deze informatie voor de gatewayinstallatie:

   * De naam die u wilt gebruiken voor de installatie 

   * De herstelsleutel die u maken wilt, die ten minste acht tekens moet

     > [!IMPORTANT]
     > Opslaan en de herstelsleutel op een veilige plaats bewaren. U hebt deze sleutel nodig wanneer u de locatie van de gateway, wijzigt of wanneer u migreren, herstellen of een bestaande gateway overnemen.

   * Bevestiging van de herstelsleutel 

     ![Gateway instellen](./media/logic-apps-gateway-install/set-up-gateway.png)

7. Controleer de regio voor de gateway-cloudservice en de Azure Service Bus die wordt gebruikt door de gatewayinstallatie is geselecteerd. 

   ![Regio controleren](./media/logic-apps-gateway-install/check-region.png)

   > [!IMPORTANT]
   > Als u wilt deze regio wordt gewijzigd nadat u klaar bent met het installeren van de gateway, moet u de herstelsleutel voor die gatewayinstallatie. U moet ook verwijderen en opnieuw installeren van de gateway. Zie voor meer informatie, [locatie wijzigen, migreren, herstellen of overnemen van bestaande gateway](#update-gateway-installation).

   *Waarom de regio voor uw gatewayinstallatie wijzigen?* 

   Om te verminderen latentie, kunt u de regio van uw gateway bijvoorbeeld wijzigen bij dezelfde regio als uw logische app. 
   Of u kunt de regio het dichtst bij uw on-premises gegevensbron selecteren. 
   Uw *gateway-resource in Azure* en uw logische app andere locaties kan hebben.

8. Kies voor het accepteren van de standaardregio, **configureren**. Of, als u wilt wijzigen van de standaardregio, als volgt te werk:

   1. Selecteer naast de huidige regio **regio wijzigen**. 

      ![De regio wijzigen](./media/logic-apps-gateway-install/change-region.png)

   2. Open op de volgende pagina de **regio selecteren** , selecteert u de regio die u wilt gebruiken en kies **gedaan**.

      ![Selecteer een andere regio](./media/logic-apps-gateway-install/select-region-gateway-install.png)

9. Nadat de bevestigingspagina die wordt weergegeven, kiest u **sluiten**. 

   Het installatieprogramma wordt bevestigd dat uw gateway nu online en klaar voor gebruik is.

   ![Voltooide gateway](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

10. Nu registreren van uw gateway in Azure door [het maken van een Azure-resource voor de gatewayinstallatie](../logic-apps/logic-apps-gateway-connection.md). 

## <a name="enable-high-availability"></a>Hoge beschikbaarheid inschakelen

De on-premises gegevensgateway biedt ondersteuning voor hoge beschikbaarheid wanneer u meer dan één gatewayinstallatie en deze als clusters instellen. Als u een bestaande gateway hebt wanneer u gaat u naar een andere gateway maakt, kunt u eventueel clusters met hoge beschikbaarheid maken. Gateways organiseren deze clusters in groepen die kunnen helpen voorkomen single point of failure. Bekijk deze vereisten en overwegingen voor het gebruik van deze mogelijkheid:

* Alleen sommige connectors bieden ondersteuning voor hoge beschikbaarheid, zoals de bestandssysteem-connector en anderen op de manier. 
     
* U moet ten minste één gatewayinstallatie binnen hetzelfde Azure-abonnement als de primaire gateway en de herstelsleutel voor die installatie al hebben. 

* De primaire gateway moet worden uitgevoerd als de gateway-update van November 2017 of later.

Na het voldoen aan deze vereisten, wanneer u de volgende gateway maakt, selecteert **toevoegen aan een bestaand gatewaycluster**, selecteert u de primaire gateway voor uw cluster en de herstelsleutel voor die primaire gateway bevatten.
Zie voor meer informatie, [clusters met hoge beschikbaarheid voor on-premises gegevensgateway](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters).

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>De locatie wijzigen, migreren, herstellen of bestaande gateway overnemen

Als u moet de locatie van uw gateway wijzigen, de gatewayinstallatie verplaatst naar een nieuwe computer, een beschadigde gateway herstellen of eigenaar van een bestaande gateway, moet u de herstelsleutel dat is opgegeven tijdens de installatie van de gateway. Deze actie wordt de oude gateway verbroken.

1. Uit van uw computer **Configuratiescherm**, gaat u naar **programma's en onderdelen**. Selecteer in de lijst met programma's, **On-premises gegevensgateway**, en kies vervolgens **verwijderen**.

2. [De on-premises gegevensgateway installeren](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

3. Nadat het installatieprogramma wordt geopend, kunt u aanmelden met hetzelfde werk- of schoolaccount dat eerder werd gebruikt voor het installeren van de gateway.

4. Selecteer **migreren, herstellen of overnemen van een bestaande gateway**, en kies vervolgens **volgende**.

   ![Selecteer 'Migreren, herstellen of overnemen van een bestaande gateway'](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

5. Onder **beschikbare gateways** of **beschikbare gatewayclusters**, selecteert u de gatewayinstallatie van de die u wilt wijzigen. Geef de herstelsleutel voor de gatewayinstallatie. 

   ![Selecteer de primaire gateway](./media/logic-apps-gateway-install/select-existing-gateway.png)

6. Als de regio wijzigen, selecteert u **regio wijzigen** en de nieuwe regio.

7. Wanneer u klaar bent, kiest u **configureren**.

## <a name="configure-proxy-or-firewall"></a>Proxy of firewall configureren

De on-premises gegevensgateway maakt een uitgaande verbinding met [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Als uw werkomgeving vereist dat verkeer via een proxy voor toegang tot het internet gaat, kan deze beperking te voorkomen dat de data gateway verbinding maken met de gateway-cloudservice. Om te bepalen of uw netwerk een proxyserver gebruikt, controleert u in dit artikel op superuser.com: 

[Hoe weet ik welke proxyserver die ik gebruik? (SuperUser.com)](https://superuser.com/questions/346372/how-do-i-know-what-proxy-server-im-using) 

Voor proxy-informatie voor uw gateway, Zie [proxy-instellingen configureren](https://docs.microsoft.com/power-bi/service-gateway-proxy). Als u wilt controleren of uw proxy of firewall de verbindingen blokkeren mogelijk, Controleer of uw computer daadwerkelijk verbinding met internet maken kan en de [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Vanuit een PowerShell-prompt, moet u deze opdracht uitvoeren:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Met deze opdracht test alleen verbinding met het netwerk en de verbinding met de Azure Service Bus. De opdracht doet niets met de gateway of de gateway-cloudservice die versleutelt en slaat de referenties en -gatewaydetails. 
>
> Ook deze opdracht is alleen beschikbaar op Windows Server 2012 R2 of hoger en Windows 8.1 of hoger. In eerdere versies van het besturingssysteem, kunt u Telnet connectiviteit testen. Meer informatie over [Azure Service Bus en hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Uw resultaten moeten eruitzien zoals in dit voorbeeld met **TcpTestSucceeded** ingesteld op **waar**:

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

Als **TcpTestSucceeded** niet is ingesteld op **waar**, uw gateway mogelijk geblokkeerd door een firewall. Als u worden uitgebreid wilt, vervangt de **ComputerName** en **poort** waarden met de waarden die worden vermeld onder [poorten configureren](#configure-ports) in dit artikel.

De firewall ook blokkeren mogelijk de verbindingen die de Azure Service Bus met de Azure-datacenters maakt. Als dit scenario gebeurt, goedkeuren (deblokkeren) alle IP-adressen voor de datacentra in uw regio. Voor deze IP-adressen, [ophalen van de Azure-IP-adressen lijst hier](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Poorten configureren

De gateway maakt een uitgaande verbinding met [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) en communiceert via uitgaande poorten: TCP 443 (standaard), 5671, 5672, 9350 via 9354. De gateway vereist geen inkomende poorten. Meer informatie over [Azure Service Bus en hybride oplossingen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

De gateway maakt gebruik van deze volledig gekwalificeerde domeinnamen:

| Domeinnamen | Uitgaande poorten | Beschrijving | 
| ------------ | -------------- | ----------- | 
| *.analysis.windows.net | 443 | HTTPS | 
| *.core.windows.net | 443 | HTTPS | 
| *.frontend.clouddatahub.net | 443 | HTTPS | 
| *.login.windows.net | 443 | HTTPS | 
| *.microsoftonline-p.com | 443 | Gebruikt voor verificatie, afhankelijk van de configuratie. | 
| *.msftncsi.com | 443 | Gebruikt voor het testen van verbinding met internet bij de gateway onbereikbaar voor de Power BI-service is. | 
| *.servicebus.windows.net | 443, 9350-9354 | Listeners op Service Bus Relay via TCP (vereist 443 voor het ophalen van tokens Access Control) | 
| *.servicebus.windows.net | 5671-5672 | Geavanceerde voor Message Queuing Protocol (AMQP) | 
| login.microsoftonline.com | 443 | HTTPS | 
||||

In sommige gevallen worden Azure Service Bus-verbindingen met IP-adressen in plaats van een volledig gekwalificeerde domeinnamen gemaakt. Dus, wilt u mogelijk geaccepteerde IP-adressen voor de gegevensregio van uw in uw firewall. Aan de goedgekeurde IP-adressen in plaats van domeinen, kunt u downloaden en gebruiken de [Microsoft Azure Datacenter IP ranges lijst](https://www.microsoft.com/download/details.aspx?id=41653). De IP-adressen in deze lijst zijn [(Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) notatie.

### <a name="force-https-communication-with-azure-service-bus"></a>HTTPS-communicatie met Azure Service Bus afdwingen

Bepaalde proxy's toestaan alleen voor de poorten 80 en 443-verkeer. Standaard treedt communicatie met Azure Service Bus op poorten dan poort 443.
U kunt afdwingen dat de gateway communiceren met de Azure Service Bus via HTTPS in plaats van direct via TCP, maar als dit dus drastisch kan verlagen prestaties. Volg deze stappen voor het uitvoeren van deze taak:

1. Blader naar de locatie voor de on-premises data gatewayclient, die meestal u hier vindt: ```C:\Program Files\On-premises data gateway\Microsoft.PowerBI.EnterpriseGateway.exe```

   Anders, als u de clientlocatie zoekt, open de console Services op dezelfde computer, zoeken **On-premises gegevensgatewayservice**, en bekijk de **pad naar uitvoerbaar bestand** eigenschap.

2. Open dit *configuratie* bestand: **Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config**

3. Wijzig de **ServiceBusSystemConnectivityModeString** waarde uit de **autodetectie** naar **Https**:

   ```html
   <setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
      <value>Https</value>
   </setting>
   ```

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows-serviceaccount

De on-premises gegevensgateway wordt uitgevoerd als een Windows-service met de naam 'On-premises gegevensgatewayservice' maar "NT SERVICE\PBIEgwService" voor de accountreferenties 'Aanmelden als gebruikt'. De on-premises gegevensgateway heeft standaard machtigingen 'Aanmelden als service' voor de computer waarop u de gateway installeert. Als u wilt maken en onderhouden van de gateway in Azure portal, het Windows-serviceaccount moet ten minste **Inzender** machtigingen. 

> [!NOTE]
> Het Windows-serviceaccount verschilt van het account dat wordt gebruikt voor het verbinden met on-premises gegevensbronnen en van het account voor werk of school gebruikt voor aanmelding bij cloud services.

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Gateway opnieuw starten

De data gateway wordt uitgevoerd als een Windows-service, dus als een andere Windows-service, kunt u starten en stoppen van de gateway op verschillende manieren. U kunt bijvoorbeeld, open een opdrachtprompt met verhoogde bevoegdheden op de computer waarop de gateway wordt uitgevoerd en voer de opdracht:

* Als u wilt stoppen met de service, moet u deze opdracht uitvoeren:
  
  `net stop PBIEgwService`

* De service wilt starten, moet u deze opdracht uitvoeren:
  
  `net start PBIEgwService`

## <a name="tenant-level-administration"></a>Beheer op tenantniveau 

Er is momenteel geen enkele locatie waar tenantbeheerders alle gateways die andere gebruikers hebben geïnstalleerd en geconfigureerd kunnen beheren. Als u een tenantbeheerder bent, is het raadzaam om de gebruikers in uw organisatie die u wilt toevoegen als beheerder voor elke gateway die ze installeren. Op deze manier kunt u alle gateways beheren in uw organisatie via de pagina instellingen van de Gateway of via [PowerShell-opdrachten](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters). 

<a name="gateway-cloud-service"></a>

## <a name="how-does-the-gateway-work"></a>Hoe werkt de gateway?

De gegevensgateway vergemakkelijkt de snelle en veilige communicatie tussen uw logische app, de gateway-cloudservice en uw on-premises gegevensbron. De gateway-cloudservice versleutelt en slaat de referenties van de gegevensbron en -gatewaydetails. De service Routering ook query's en de resultaten tussen uw logische app, de on-premises gegevensgateway en uw gegevensbron on-premises. 

De gateway werkt met firewalls en maakt gebruik van alleen uitgaande verbindingen. Er is al het verkeer afkomstig als beveiligde uitgaand verkeer van de gateway-agent. De gateway stuurt de gegevens van on-premises bronnen via versleutelde kanalen via de Azure Service Bus. Deze servicebus-maakt een kanaal tussen de gateway en de aanroepende service, maar niet alle gegevens worden opgeslagen. Alle gegevens die worden uitgewisseld via de gateway is versleuteld.

![diagram-for-on-premises-Data-gateway-flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Deze stappen wordt beschreven wat er gebeurt wanneer een gebruiker in de cloud communiceert met een element dat verbonden met uw on-premises gegevensbron:

1. De gateway-cloudservice maakt een query, samen met de versleutelde referenties voor de gegevensbron en stuurt de query naar de wachtrij voor de gateway om te verwerken.

2. De gateway-cloudservice analyseert de query en verstuurt de aanvraag naar de Azure Service Bus.

3. De on-premises gegevensgateway pollt de Azure Service Bus voor in behandeling zijnde aanvragen.

4. De gateway haalt de query, ontsleutelt de referenties en maakt verbinding met de gegevensbron met deze referenties.

5. De gateway stuurt de query naar de gegevensbron voor uitvoering.

6. De resultaten worden uit de gegevensbron verzonden naar de gateway en vervolgens naar de gateway-cloudservice. De gateway-cloudservice gebruikt vervolgens de resultaten.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

**Q**: heb ik een gateway nodig voor gegevensbronnen in de cloud, zoals Azure SQL Database? <br/>
**Een**: Nee, de gateway verbinding maakt met on-premises gegevensbronnen alleen.

**Q**: de gateway hoeft te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**Een**: Nee, de gateway verbinding maakt met de gegevensbron met behulp van de opgegeven verbindingsgegevens. De gateway als een clienttoepassing in dit opzicht beschouwen. De gateway moet alleen de mogelijkheid om te verbinden met de naam van de server die is opgegeven.

<a name="why-azure-work-school-account"></a>

**Q**: Waarom moet ik een werk- of schoolaccount aanmelden? <br/>
**Een**: U kunt alleen werk-of schoolaccount gebruiken tijdens de installatie van de on-premises gegevensgateway. Uw account aanmelden wordt opgeslagen in een tenant die wordt beheerd door Azure Active Directory (Azure AD). Uw Azure AD-account UPN (User Principal Name) komt meestal overeen met het e-mailadres.

**Q**: waar worden mijn referenties opgeslagen? <br/>
**Een**: de referenties die u voor een gegevensbron invoert, worden versleuteld en opgeslagen in de gatewaycloudservice. De referenties worden bij de on-premises gegevensgateway ontsleuteld.

**Q**: zijn er vereisten voor netwerkbandbreedte? <br/>
**Een**: Controleer of de netwerkverbinding met een goede doorvoersnelheid heeft. Elke omgeving is anders en de hoeveelheid gegevens die worden verzonden, kan invloed hebben op de resultaten. Probeer om te garanderen een doorvoerniveau tussen uw on-premises gegevensbron en de Azure-datacenters, [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Om te helpen uw doorvoer te meten, probeert u een extern hulpprogramma, zoals Azure Speed Test.

**Q**: Wat is de vertraging voor het uitvoeren van query's met een gegevensbron van de gateway? Wat is de beste architectuur? <br/>
**Een**: netwerklatentie, verminderen de gateway zo dicht bij de gegevensbron mogelijk te installeren. Als u de gateway op de daadwerkelijke gegevensbron installeren kunt, wordt deze nabijheid beperkt de latentie minimaal. Overweeg ook nabijheid met Azure-datacenters. Bijvoorbeeld, als de service wordt gebruikt voor het datacentrum VS-West en u SQL Server die wordt gehost in een Azure-VM hebt, kunt klikt u vervolgens u uw Azure-VM in de regio VS-West te. Deze nabijheid latentie wordt geminimaliseerd en voorkomt u kosten voor uitgaand verkeer op de virtuele machine van Azure.

**Q**: hoe worden resultaten teruggezonden naar de cloud? <br/>
**Een**: de resultaten worden verzonden via Azure Service Bus.

**Q**: zijn er inkomende verbindingen met de gateway vanuit de cloud? <br/>
**Een**: Nee, de gateway gebruikt uitgaande verbindingen naar Azure Service Bus.

**Q**: Wat gebeurt er als ik uitgaande verbindingen Blokkeer? Wat moet ik openen? <br/>
**Een**: Zie de poorten en hosts de gateway gebruikt.

**Q**: Wat is de Windows-service met de naam? <br/>
**Een**: op het tabblad webservices in Taakbeheer wordt de servicenaam 'PBIEgwService' of Power BI Enterprise Gateway-Service. In de console Services is de servicenaam 'On-premises gegevensgatewayservice'. De Windows-service gebruikt 'NT SERVICE\PBIEgwService' als de Service-SID (SSID).

**Q**: kan de gateway Windows-service worden uitgevoerd met een Azure Active Directory-account? <br/>
**Een**: Nee, de Windows-service moet een geldig Windows-account.

### <a name="disaster-recovery"></a>Herstel na noodgeval

**Q**: welke opties zijn beschikbaar voor herstel na noodgevallen? <br/>
**Een**: U kunt de herstelsleutel gebruiken om te herstellen of verplaatsen van een gateway. Geef de herstelsleutel op tijdens de installatie van de gateway.

**Q**: Wat is het voordeel van de herstelsleutel? <br/>
**Een**: de herstelsleutel biedt een manier om te migreren of de gateway-instellingen te herstellen na een noodgeval.

## <a name="troubleshooting"></a>Problemen oplossen

In deze sectie worden enkele veelvoorkomende problemen die optreden tijdens het instellen en gebruiken van de on-premises gegevensgateway.

**Q**: Waarom is mijn gatewayinstallatie mislukt? <br/>
**Een**: dit probleem kan zich voordoen als de antivirussoftware op de doelcomputer verouderd is. U kunt de antivirussoftware bijwerken of uitschakelen van de antivirussoftware, maar alleen tijdens de installatie van de gateway en vervolgens de software opnieuw inschakelen.

**Q**: Waarom zie ik geen mijn gatewayinstallatie wanneer ik de gateway-resource in Azure maakt? <br/>
**Een**: dit probleem kan gebeuren om volgende redenen:

* De gatewayinstallatie is al geregistreerd en geclaimd door een andere gateway-resource in Azure. Gateway-installaties worden niet weergegeven in de lijst met instanties nadat de gateway-resources zijn gemaakt voor hen.
Om te controleren of uw gateway registraties in Azure portal, Bekijk al uw Azure-resources met de **On-premises gegevensgateways** Typ voor *alle* Azure-abonnementen. 

* De Azure AD-identiteit voor de persoon die de gateway hebt geïnstalleerd, verschilt van de persoon die heeft aangemeld bij de Azure-portal. Controleer of u bent aangemeld bij de dezelfde identiteit die de gateway hebt geïnstalleerd.

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: waar zijn de gatewaylogboeken? <br/>
**Een**: Zie de [ **logboeken** sectie](#logs) verderop in dit artikel.

**Q**: hoe kan ik zien wat voor query's worden verzonden naar de on-premises gegevensbron? <br/>
**Een**: U kunt querytracering inschakelen, waaronder de query's die worden verzonden. Houd er rekening mee te wijzigen van de query traceren terug naar de oorspronkelijke waarde wanneer klaar oplossen van problemen. Querytracering ingeschakeld laten maakt grotere Logboeken.

U kunt ook kijken welke hulpmiddelen die uw gegevensbron voor query's te traceren biedt. U kunt bijvoorbeeld Extended Events of SQL Profiler gebruiken voor SQL Server en Analysis Services.

### <a name="outdated-gateway-version"></a>Verouderde gateway-versie

Heel wat problemen kunnen surface als de gatewayversie verouderd is. Als de goede en gangbare praktijk, zorg ervoor dat de meest recente versie. Als u kunt de gateway nog niet hebt bijgewerkt, voor een maand of langer, kunt u Overweeg de installatie van de meest recente versie van de gateway en zien als u het probleem zich voordoet.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fout: Kan niet aan gebruiker toevoegen aan groep. (-2147463168 PBIEgwService Prestatielogboekgebruikers)

U kunt deze fout krijgen als u probeert te installeren van de gateway op een domeincontroller wordt niet ondersteund. Zorg ervoor dat u de gateway op een computer die geen domeincontroller implementeert.

<a name="logs"></a>

### <a name="logs"></a>Logboeken

Om u te helpen problemen op, begin altijd te verzamelen en controleren van de gatewaylogboeken. Er zijn verschillende manieren voor het verzamelen van de logboeken, maar de eenvoudigste optie nadat u de gateway hebt geïnstalleerd, is via de gebruikersinterface van het gateway-installatieprogramma. 

1. Open het installatieprogramma van de on-premises data gateway op uw computer.
2. Selecteer in het menu links **Diagnostics**.
3. Onder **gatewaylogboeken**, selecteer **logboeken exporteren**.

   ![Logboeken van installatieprogramma van de gateway exporteren](./media/logic-apps-gateway-install/export-logs.png)

Hier volgen andere locaties waar u verschillende logboeken kunt vinden:

| Logboektype | Locatie | 
|----------|----------| 
| **Logboeken van installatieprogramma** | %localappdata%\Temp\On-premises_data_gateway_ <*JJJJMMDD*>. <*getal*> .log | 
| **Configuratielogboeken** | C:\Users\<*gebruikersnaam*> \AppData\Local\Microsoft\On-premises gegevensgateway\gatewayconfigurator <*JJJJMMDD*>. <*getal*>. logboek | 
| **Logboeken van Enterprise-gatewayservice** | C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises gegevensgateway\gateway <*JJJJMMDD*>. <*getal*> .log | 
||| 

**Gebeurtenislogboeken**

Als u zoekt de gebeurtenislogboeken voor de gateway, de volgende stappen uit:

1. Open op de computer met de gatewayinstallatie, de **logboeken**. 
2. Vouw **Logboeken (lokaal)** > **logboeken toepassingen en Services**. 
3. Selecteer **On-premises gegevensgatewayservice**.

   ![De gebeurtenislogboeken weergeven voor gateway](./media/logic-apps-gateway-install/event-viewer.png)

### <a name="telemetry"></a>Telemetrie

U kunt voor aanvullende controle en probleemoplossing, inschakelen en verzamelen van telemetrie. 

1. Blader naar de locatie voor de on-premises data gatewayclient, die meestal u hier vindt: ```C:\Program Files\On-premises data gateway```

   Anders, als u de clientlocatie zoekt, open de console Services op dezelfde computer, zoeken **On-premises gegevensgatewayservice**, en bekijk de **pad naar uitvoerbaar bestand** eigenschap.

2. Open dit *configuratie* bestand: **Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config**

3. Wijzig de **SendTelemetry** waarde die moet worden **waar**:

   ```html
   <setting name="SendTelemetry" serializeAs="String">
      <value>true</value>
   </setting>
   ```

4. Sla uw wijzigingen op en start de Windows-service opnieuw.

### <a name="review-slow-query-performance"></a>Prestaties van de langzame query's controleren

Als u query's langzaam worden uitgevoerd via de gateway vinden, kunt u aanvullende logboekregistratie die query's en hun duur voert inschakelen. Deze logboeken kunnen helpen u welke query's worden traag of de uitvoering lang duurt. Voor het afstemmen van prestaties van query's, is het wellicht moet uw gegevensbron, bijvoorbeeld wijzigen, aanpassen van indexen voor de SQL Server-query's.

Om te bepalen van de duur voor een query, de volgende stappen uit:

1. Blader naar dezelfde locatie als de gatewayclient, die meestal u hier vindt: ```C:\Program Files\On-premises data gateway```

   Anders, als u de clientlocatie zoekt, open de console Services op dezelfde computer, zoeken **On-premises gegevensgatewayservice**, en bekijk de **pad naar uitvoerbaar bestand** eigenschap.

2. Open en deze configuratiebestanden te bewerken, zoals wordt beschreven:

   * **Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config**

     In dit bestand, wijzigt u de **EmitQueryTraces** waarde uit de **false** naar **waar** , zodat uw gateway zich query's verzonden van de gateway naar een gegevensbron:

     ```html
     <setting name="EmitQueryTraces" serializeAs="String">
        <value>true</value>
     </setting>
     ```

     > [!IMPORTANT]
     > Inschakelen van de instelling EmitQueryTraces, kan de grootte van het logboekbestand op basis van gatewaygebruik aanzienlijk toenemen. Nadat u klaar bent met het controleren van de logboeken, controleert u of u emitquerytraces het beste weer te herstellen **false** opnieuw, in plaats van dat deze instelling zijn ingeschakeld voor de lange termijn.

   * **Microsoft.PowerBI.DataMovement.Pipeline.Diagnostics.dll.config**

     Als u wilt dat uw gateway uitgebreide logboekvermeldingen, met inbegrip van items die worden weergegeven, duur, wijzigt de **TracingVerbosity** waarde uit de **4** naar **5** door het uitvoeren van een stap: 

     * In dit configuratie-item, wijzigt u de **TracingVerbosity** waarde uit de **4** naar **5** 

       ```html
       <setting name="TracingVerbosity" serializeAs="String">
          <value>5</value>
       </setting>
       ```

     * Het installatieprogramma van de gateway openen, selecteert u **Diagnostics**, inschakelen **aanvullende logboekregistratie**, en kies vervolgens **toepassen**:

       ![Aanvullende logboekregistratie inschakelen](./media/logic-apps-gateway-install/turn-on-additional-logging.png)

     > [!IMPORTANT]
     > Inschakelen van de instelling TracingVerbosity, kan de grootte van het logboekbestand op basis van gatewaygebruik aanzienlijk toenemen. Nadat u klaar bent met het controleren van de logboeken, zorg ervoor dat u uitschakelen **aanvullende logboekregistratie** in het installatieprogramma van de gateway of opnieuw instellen van traceverbosity in te stellen **4** opnieuw in het configuratiebestand, in plaats van dat deze instelling zijn ingeschakeld voor de lange termijn.

3. Als u zoekt de duur voor een query, de volgende stappen uit:

   1. [Exporteren](#logs) en open het gatewaylogboek geschreven.

   2. Als u een query zoekt, zoeken naar een activiteitstype, bijvoorbeeld: 

      | Type activiteit | Beschrijving | 
      |---------------|-------------| 
      | MGEQ | Query's die worden uitgevoerd via ADO.NET. | 
      | MGEO | Query's die worden uitgevoerd via OLEDB. | 
      | MGEM | Query's die worden uitgevoerd vanaf de Mashup-engine. | 
      ||| 

   3. Houd er rekening mee de tweede GUID die de aanvraag-ID.

   4. Verder zoeken voor het activiteitstype totdat u een vermelding die met de naam 'FireActivityCompletedSuccessfullyEvent' met een duur in milliseconden. 
   Controleer of dat de vermelding heeft de dezelfde aanvraag-ID, bijvoorbeeld:

      ```text 
      DM.EnterpriseGateway Verbose: 0 : 2016-09-26T23:08:56.7940067Z DM.EnterpriseGateway    baf40f21-2eb4-4af1-9c59-0950ef11ec4a    5f99f566-106d-c8ac-c864-c0808c41a606    MGEQ    21f96cc4-7496-bfdd-748c-b4915cb4b70c    B8DFCF12 [DM.Pipeline.Common.TracingTelemetryService] Event: FireActivityCompletedSuccessfullyEvent (duration=5004)
      ```

      > [!NOTE] 
      > De vermelding 'FireActivityCompletedSuccessfullyEvent' is een uitgebreid item en worden niet geregistreerd, tenzij de instelling 'TracingVerbosity' op het niveau van 5.

### <a name="trace-traffic-with-fiddler"></a>Verkeer traceren met Fiddler

[Fiddler](http://www.telerik.com/fiddler) is een gratis hulpprogramma van Telerik die HTTP-verkeer wordt bewaakt. U kunt dit verkeer met de Power BI-service op de clientcomputer bekijken. Deze service kan fouten en andere verwante informatie weergeven.

## <a name="next-steps"></a>Volgende stappen
    
* [Verbinding maken met on-premises gegevens vanuit logische apps](../logic-apps/logic-apps-gateway-connection.md)
* [Enterprise integration-functies](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Connectors voor Azure Logic Apps](../connectors/apis-list.md)
