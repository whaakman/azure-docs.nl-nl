---
title: Werken met bestaande on-premises proxy-servers en Azure AD | Microsoft Docs
description: Bevat informatie over het werken met bestaande lokale proxyservers.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 85b45a828dbb06325b6a1c9453117849f48a27c8
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Werken met bestaande lokale proxyservers

In dit artikel wordt uitgelegd hoe connectors werken met uitgaande proxyservers toepassingsproxy van Azure Active Directory (Azure AD) configureren. Het is bedoeld voor klanten met een netwerkomgevingen met bestaande proxy's.

We beginnen door te kijken deze belangrijkste implementatiescenario's:
* Connectors configureren voor uw lokale uitgaande proxy overslaan.
* Connectors configureren voor een uitgaande proxyconfiguratie gebruiken voor toegang tot Azure AD-toepassingsproxy.

Zie voor meer informatie over hoe connectors werken [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md).

## <a name="bypass-outbound-proxies"></a>Uitgaande bypass-proxy 's

Connectors hebben onderliggende besturingssysteemonderdelen die uitgaande aanvragen. Deze onderdelen wordt automatisch geprobeerd het vinden van een proxyserver op het netwerk met behulp van Web Proxy Auto-Discovery (WPAD).

De OS-componenten probeert te vinden van een proxyserver door middel van een DNS-zoekactie voor wpad.domainsuffix. Als de zoekopdracht wordt omgezet in DNS, wordt vervolgens een HTTP-aanvraag naar de IP-adres voor wpad.dat gemaakt. Deze aanvraag wordt het configuratiescript in uw omgeving. De connector gebruikt dit script te selecteren van een server voor uitgaande proxyconfiguratie. Echter, connector verkeer mogelijk nog steeds niet via, vanwege de extra configuratie-instellingen op de proxy nodig.

U kunt de connector voor het overslaan van de lokale proxy om ervoor te zorgen dat deze gebruikmaakt van directe verbinding met de Azure-services configureren. Het is raadzaam deze benadering, zolang uw netwerkbeleid zijn toegestaan, omdat het betekent dat u één minder configuratie hebt te onderhouden.

Bewerk het bestand C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config als wilt uitschakelen uitgaande proxyconfiguratie gebruik voor de connector, en voeg de *system.net* sectie in dit voorbeeld wordt weergegeven:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
Om ervoor te zorgen dat de service-Connector Updater ook de proxy omzeilt, moet u een vergelijkbare wijziging aanbrengt in het bestand ApplicationProxyConnectorUpdaterService.exe.config. Dit bestand bevindt zich in C:\Program Files\Microsoft AAD App Proxy Connector Updater.

Zorg kopieën van de originele bestanden te maken als u wilt terugkeren naar de standaard .config-bestanden.

## <a name="use-the-outbound-proxy-server"></a>Uitgaande proxyserver gebruiken

Sommige omgevingen vereisen al het uitgaande verkeer door een uitgaande proxyconfiguratie, zonder uitzondering gaan. Als gevolg hiervan kan omzeilen van de proxy niet worden gebruikt.

U kunt de connector-verkeer via de uitgaande proxyconfiguratie kunt configureren, zoals wordt weergegeven in het volgende diagram:

 ![Connector-verkeer naar Azure AD-toepassingsproxy via een uitgaande proxy configureren](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Als gevolg van gelet alleen uitgaand verkeer, is er niet nodig om binnenkomende toegang via de firewall te configureren.

>[!NOTE]
>Toepassingsproxy biedt geen ondersteuning voor verificatie bij andere proxy's. De connector updater/network service-accounts moeten kunnen verbinding maken met de proxy zonder verificatie wordt gecontroleerd.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Stap 1: De connector en verwante services via de uitgaande proxy configureren

Als WPAD is ingeschakeld in de omgeving en op de juiste wijze geconfigureerd, detecteert de connector automatisch de uitgaande proxyserver en de poging om deze te gebruiken. U kunt echter expliciet de connector te doorlopen een uitgaande proxyconfiguratie configureren.

Om dit te doen, bewerk het bestand C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config en voeg de *system.net* sectie in dit voorbeeld wordt weergegeven. Wijziging *proxyserver:8080* in overeenstemming met uw lokale proxy-servernaam of IP-adres en de poort die deze luistert op.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Configureer vervolgens de service-Connector Updater voor het gebruik van de proxy door een vergelijkbare wijziging aan het bestand C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Stap 2: Configureer de proxy voor verkeer van de connector en verwante services kunnen stromen

Er zijn vier aspecten rekening moet houden bij de uitgaande proxy:
* Uitgaande proxy-regels
* Proxyverificatie
* Proxy-poorten
* SSL-inspectie

#### <a name="proxy-outbound-rules"></a>Uitgaande proxy-regels
Toegang tot de volgende eindpunten voor connector-Servicetoegang toestaan:

* *. msappproxy.net
* *. servicebus.windows.net

Voor de initiële registratie, moet u toegang tot de volgende eindpunten toestaan:

* login.windows.net
* Login.microsoftonline.com

Als u niet kan verbinding met FQDN-naam maken en moet in plaats daarvan IP-adresbereiken opgeven, gebruikt u deze opties:

* De connector uitgaande toegang tot alle bestemmingen toestaan.
* De connector uitgaande toegang tot alle van de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). De uitdaging met behulp van de lijst met Azure datacenter IP-adresbereiken is wekelijks wordt bijgewerkt. U moet het plaatsen van een proces implementeren dat ervoor zorgt dat uw toegangsregels dienovereenkomstig worden bijgewerkt. Alleen een subset van de IP-adressen gebruikt, kan dit ertoe leiden dat uw configuratie wilt afbreken.

#### <a name="proxy-authentication"></a>Proxyverificatie

Proxyverificatie is momenteel niet ondersteund. We bevelen aan huidige is dat de connector anonieme toegang tot de Internet-doelen.

#### <a name="proxy-ports"></a>Proxy-poorten

De connector maakt uitgaande SSL-verbindingen met de methode CONNECT. Deze methode wordt in wezen stelt u een tunnel via de uitgaande proxyconfiguratie. De proxyserver zodat de poorten 443 en 80 tunneling configureren.

>[!NOTE]
>Wanneer Service Bus wordt uitgevoerd via HTTPS, poort 443 gebruikt. Echter, standaard, Service Bus probeert rechtstreekse TCP-verbindingen en terugvalt op HTTPS alleen als directe verbinding mislukt.

#### <a name="ssl-inspection"></a>SSL-inspectie
Gebruik geen SSL-inspectie voor het verkeer van de connector, omdat er problemen optreden voor het verkeer van de connector. De connector gebruikt een certificaat voor verificatie bij de service-toepassingsproxy kan, en dat certificaat verloren gaan tijdens de inspectie van SSL. 

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Connector-proxy problemen en service-verbindingsproblemen oplossen
U ziet nu al het verkeer via de proxy. Als u problemen hebt, moet de volgende informatie voor probleemoplossing helpen.

De beste manier om te identificeren en oplossen van problemen met de netwerkverbinding van de connector is het nemen van een netwerkopname tijdens het starten van de connector-service. Hier volgen enkele snelle tips voor het vastleggen en netwerktracering filteren.

U kunt het programma voor bewaking van uw keuze. Voor de doeleinden van dit artikel hebben we Microsoft Message Analyzer gebruikt. U kunt [downloaden vanaf Microsoft](https://www.microsoft.com/download/details.aspx?id=44226).

De volgende voorbeelden zijn specifiek voor Message Analyzer, maar de beginselen kunnen worden toegepast op een hulpmiddel voor analyse.

### <a name="take-a-capture-of-connector-traffic"></a>Een maken van connector verkeer

Voer de volgende stappen uit voor de eerste probleemoplossing:

1. Stop de service Azure AD-Application Proxy Connector van services.msc.

   ![Azure AD Connector voor toepassingsproxy-service in services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

2. Message Analyzer uitgevoerd als een beheerder.
3. Selecteer **lokale tracering starten**.

   ![Netwerkopnamesoftware starten](./media/application-proxy-working-with-proxy-servers/start-local-trace.png)

3. Start de service Azure AD-Application Proxy Connector.
4. Stop de netwerkopname.

   ![Stop de netwerkopname](./media/application-proxy-working-with-proxy-servers/stop-trace.png)

### <a name="check-if-the-connector-traffic-bypasses-outbound-proxies"></a>Controleer of de connector-verkeer uitgaand proxy's omzeilt

Als u uw Application Proxy connector geconfigureerd om de proxyservers omzeilen en rechtstreeks verbinding maken met de service voor toepassingsproxy, die u wilt zoeken in het netwerk vastleggen voor mislukte pogingen voor TCP-verbinding. 

Het filter Message Analyzer gebruiken om te identificeren van deze pogingen. Voer `property.TCPSynRetransmit` in het filtervak en selecteer **toepassen**. 

Een pakket SYN is het eerste pakket verzonden naar TCP-verbinding wordt gemaakt. Als dit pakket een antwoord retourneren heeft niet, wordt de SYN reattempted. De voorgaande filter kunt u eventuele verzonden SYN Zie. Vervolgens kunt u controleren of deze SYN komen met een connector-verkeer overeen.

Als u verwacht de connector dat voor het rechtstreeks verbinding maken met de Azure-services, zijn SynRetransmit reacties op poort 443 geeft aan dat er een probleem met het netwerk of de firewall.

### <a name="check-if-the-connector-traffic-uses-outbound-proxies"></a>Als het verkeer van de connector wordt gebruikt voor uitgaande proxy's controleren

Als u uw Application Proxy connector-verkeer via de proxyservers hebt geconfigureerd, dat u wilt zoeken naar mislukte https-verbindingen met de proxy. 

Als u wilt filteren op het netwerk vastleggen voor deze verbindingspogingen, voer `(https.Request or https.Response) and tcp.port==8080` in het filter Message Analyzer, waarbij u 8080 vervangt door uw service proxypoort. Selecteer **toepassen** om het filterresultaten te bekijken. 

Het voorgaande filter geeft alleen de HTTPs-aanvragen en antwoorden van de proxypoort. U zoekt naar de CONNECT-aanvragen die communicatie met de proxyserver weergeven. Als dit lukt krijgt u een HTTP-OK (200) antwoord.

Als u de overige reactiecodes zoals 407 of 502, ziet dit betekent dat de proxy die verificatie vereist is of niet het verkeer voor een andere reden toestaat. Nu benaderen u het ondersteuningsteam van proxy server.

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)

- Als u problemen met verbindingsproblemen connector hebt, stel uw vraag in de [Azure Active Directory-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD&forum=WindowsAzureAD) of maak een ticket aan ons ondersteuningsteam.
