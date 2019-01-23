---
title: Werken met bestaande on-premises proxy-servers en Azure AD | Microsoft Docs
description: Bevat informatie over het werken met bestaande on-premises proxy-servers.
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 6079f1ac1193568da2806efa27c775f97a6c5f95
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464031"
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Werken met bestaande on-premises proxy-servers

In dit artikel wordt uitgelegd hoe u Azure Active Directory (Azure AD) Application Proxy connectors werken met uitgaande proxy-servers configureren. Het is bedoeld voor klanten met netwerkomgevingen die bestaande proxy's.

We beginnen door te kijken deze belangrijkste implementatiescenario's:
* Connectors voor het overslaan van uw on-premises uitgaande proxy configureren.
* Connectors voor het gebruik van een uitgaande proxy voor toegang tot Azure AD Application Proxy configureren.

Zie voor meer informatie over de werking van connectors [over Azure AD Application Proxy connectors](application-proxy-connectors.md).

## <a name="bypass-outbound-proxies"></a>Uitgaande proxy overslaan

Connectors zijn onderliggende OS-componenten die uitgaande aanvragen. Deze onderdelen wordt automatisch geprobeerd om te vinden van een proxyserver op het netwerk met behulp van Web Proxy Auto-Discovery (WPAD).

De OS-componenten proberen te vinden van een proxyserver door middel van een DNS-zoekactie voor wpad.domainsuffix. Als de zoekopdracht wordt omgezet in DNS, wordt vervolgens een HTTP-aanvraag naar de IP-adres voor wpad.dat gemaakt. Deze aanvraag wordt het configuratiescript in uw omgeving. De connector gebruikt dit script om een uitgaande proxy-server selecteren. Echter connector verkeer mogelijk nog steeds niet doorlopen, vanwege de aanvullende configuratie-instellingen die nodig zijn voor de proxy.

U kunt de connector voor het overslaan van uw on-premises proxy om ervoor te zorgen dat deze gebruikmaakt van directe verbinding met de Azure-services configureren. Deze methode wordt aangeraden zo lang uw netwerkbeleid zijn toegestaan, omdat het betekent dat u een minder configuratie hebt te onderhouden.

Voor het gebruik van de uitgaande proxy voor de connector is uitgeschakeld, bewerk het bestand C:\Program Files\Microsoft AAD-App Proxy Connector\ApplicationProxyConnectorService.exe.config en voeg de *system.net* sectie in dit voorbeeld wordt weergegeven:

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
Om ervoor te zorgen dat de Connector Updater-service ook de proxy omzeilt, moet u een vergelijkbare wijziging aanbrengt in het bestand ApplicationProxyConnectorUpdaterService.exe.config. Dit bestand bevindt zich in C:\Program Files\Microsoft AAD-App Proxy Connector Updater.

Vergeet om kopieën van de originele bestanden te maken als u wilt terugkeren naar de standaard .config-bestanden.

## <a name="use-the-outbound-proxy-server"></a>Gebruik de uitgaande proxy-server

Sommige omgevingen moeten al het uitgaande verkeer via een uitgaande proxy, zonder uitzonderingen. Als gevolg hiervan kan overslaan van de proxy niet worden gebruikt.

U kunt de connector-verkeer via de uitgaande proxy configureren zoals wordt weergegeven in het volgende diagram:

 ![Verkeer via een uitgaande proxy naar Azure AD Application Proxy connector configureren](./media/application-proxy-configure-connectors-with-proxy-servers/configure-proxy-settings.png)

Als gevolg van dat u hoeft alleen uitgaand verkeer, is er niet nodig om binnenkomende toegang via de firewall te configureren.

>[!NOTE]
>Application Proxy biedt geen ondersteuning voor verificatie bij andere proxy's. De connector updater/network service-accounts moet kunnen verbinding maken met de proxy zonder genoodzaakt voor verificatie.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Stap 1: De connector en gerelateerde services om te gaan via de uitgaande proxy configureren

Als WPAD is ingeschakeld in de omgeving en op de juiste wijze geconfigureerd, wordt de connector automatisch detecteert de uitgaande proxy-server en de poging om deze te gebruiken. U kunt echter expliciet de connector te gaan via een uitgaande proxy configureren.

Om dit te doen, bewerk het bestand C:\Program Files\Microsoft AAD-App Proxy Connector\ApplicationProxyConnectorService.exe.config en voeg de *system.net* sectie in dit voorbeeld wordt weergegeven. Wijziging *proxyserver:8080* in overeenstemming met de naam van uw lokale proxy-server of IP-adres en de poort waarop deze luistert. De waarde moet het voorvoegsel http:// hebben, zelfs als u een IP-adres.

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

Vervolgens configureert u de Connector Updater-service voor het gebruik van de proxy door een soortgelijke wijziging naar het bestand C:\Program Files\Microsoft AAD-App Proxy-Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Stap 2: Configureer de proxy voor verkeer van de connector en gerelateerde services door kunnen stromen

Er zijn vier aspecten om te overwegen bij de uitgaande proxy:
* Proxy regels voor uitgaand verkeer
* Proxy-verificatie
* Proxy-poorten
* SSL-inspectie

#### <a name="proxy-outbound-rules"></a>Proxy regels voor uitgaand verkeer
Sta toegang tot de volgende URL's toe:

| URL | Hoe dat wordt gebruikt |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Communicatie tussen de connector en de Application Proxy-cloudservice |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure gebruikt deze URL's om certificaten te controleren |
| login.windows.net<br>login.microsoftonline.com | De connector gebruikt deze URL's tijdens het registratieproces. |

Als uw firewall of proxyserver opname in een DNS-whitelist toestaat, kunt u verbindingen met \*msappproxy.net en \* in de whitelist opnemen. Als dat niet het geval is, moet u toegang toestaan tot de [IP-bereiken van Azure DataCenter](https://www.microsoft.com/download/details.aspx?id=41653), die overigens elke week worden bijgewerkt.


Als u kunt geen kunt u met FQDN-naam en moet in plaats daarvan IP-adresbereiken opgeven, gebruikt u deze opties:

* De connector uitgaande toegang geven tot alle bestemmingen.
* Toestaan dat de connector uitgaande toegang tot alle van de [Azure datacenter IP-adresbereiken](https://www.microsoft.com//download/details.aspx?id=41653). De uitdaging bij het gebruik van de lijst met IP-adresbereiken Azure datacenter wordt wekelijks bijgewerkt. U moet een proces opstellen om ervoor te zorgen dat uw toegangsregels dienovereenkomstig worden bijgewerkt. Alleen met behulp van een subset van de IP-adressen, kan dit ertoe leiden dat uw configuratie te verbreken.

#### <a name="proxy-authentication"></a>Proxy-verificatie

Proxy-verificatie wordt momenteel niet ondersteund. Onze huidige aanbeveling is om de connector anonieme toegang tot de Internet-bestemmingen.

#### <a name="proxy-ports"></a>Proxy-poorten

De connector maakt uitgaande op basis van een SSL-verbindingen met behulp van de methode CONNECT. Deze methode wordt in feite stelt u een tunnel via de uitgaande proxy. Configureer de proxyserver om toe te staan op poort 443 en 80 tunneling.

>[!NOTE]
>Wanneer Service Bus wordt uitgevoerd via HTTPS, gebruikt deze poort 443. Echter, standaard, Service Bus probeert rechtstreekse TCP-verbindingen en schakelt terug naar HTTPS alleen als rechtstreekse verbinding mislukt.

#### <a name="ssl-inspection"></a>SSL-inspectie
Gebruik geen SSL-controle voor de connector-verkeer, omdat het ervoor zorgt problemen voor de connector-verkeer dat. De connector gebruikt een certificaat voor verificatie bij de Application Proxy-service kan, en dat certificaat verloren gaan tijdens de SSL-inspectie. 

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Oplossen van problemen met proxy connector en problemen met de netwerkverbinding van de service
Nu ziet u alle verkeer via de proxy. Als u problemen hebt, kunt de volgende informatie voor probleemoplossing.

De beste manier om te identificeren en oplossen van problemen met de netwerkverbinding van de connector is voordat een netwerkopname tijdens het starten van de connector-service. Hier volgen enkele snelle tips voor het vastleggen en Netwerktraceringen filteren.

U kunt het hulpprogramma voor bewaking van uw keuze gebruiken. Voor de doeleinden van dit artikel hebben we Microsoft Message Analyzer gebruikt. U kunt [downloaden vanaf Microsoft](https://www.microsoft.com/download/details.aspx?id=44226).

De volgende voorbeelden zijn specifiek voor Message Analyzer, maar de principes die kunnen worden toegepast op alle analyseprogramma.

### <a name="take-a-capture-of-connector-traffic"></a>Een maken van het verkeer van de connector

Voor het eerste oplossen, moet u de volgende stappen uitvoeren:

1. Via services.msc, de Azure AD Application Proxy Connector-service te stoppen.

   ![Azure AD Application Proxy Connector-service in services.msc](./media/application-proxy-configure-connectors-with-proxy-servers/services-local.png)

2. Message Analyzer uitvoert als beheerder.
3. Selecteer **lokale tracering starten**.

   ![Netwerk opnemen](./media/application-proxy-configure-connectors-with-proxy-servers/start-local-trace.png)

3. Start de service Azure AD Application Proxy-Connector.
4. Stop de netwerkopname.

   ![Netwerkopname stoppen](./media/application-proxy-configure-connectors-with-proxy-servers/stop-trace.png)

### <a name="check-if-the-connector-traffic-bypasses-outbound-proxies"></a>Als de connector-verkeer uitgaande proxy's omzeilt controleren

Als u uw Application Proxy-connector voor het overslaan van de proxy-servers en rechtstreeks verbinding maken met de service voor toepassingsproxy hebt geconfigureerd, dat u wilt zoeken in het netwerk vastleggen voor mislukte TCP-verbindingspogingen. 

Het filter Message Analyzer gebruiken om te identificeren van deze pogingen. Voer `property.TCPSynRetransmit` in het filtervak en selecteer **toepassen**. 

Een pakket SYN is het eerste pakket verzonden naar een TCP-verbinding tot stand brengen. Als dit pakket biedt geen reactie retourneren, wordt de SYN nieuwe. U kunt het bovenstaande filter gebruiken om te zien van alle SYN verzonden. Vervolgens kunt u controleren of deze SYN komen met een connector-verkeer overeen.

Als u verwacht de connector voor het rechtstreeks verbinding maken met de Azure-services dat, de SynRetransmit antwoorden op poort 443 zijn een indicatie dat er een probleem met het netwerk of firewall.

### <a name="check-if-the-connector-traffic-uses-outbound-proxies"></a>Als het verkeer van de connector gebruikmaakt van uitgaande proxy's controleren

Als u uw Application Proxy connector-verkeer via de proxy-servers hebt geconfigureerd, dat u wilt zoeken naar mislukte https-verbindingen met uw proxy. 

Als u wilt het vastleggen van het netwerk voor deze pogingen om verbinding te filteren, voer `(https.Request or https.Response) and tcp.port==8080` in het filter Message Analyzer, vervangt u 8080 met uw proxy-service-poort. Selecteer **toepassen** om de filterresultaten te bekijken. 

Het bovenstaande filter geeft alleen de HTTPs-aanvragen en antwoorden naar/van de proxyserver-poort. U zoekt naar de CONNECT-aanvragen die communicatie met de proxy-server weergeven. Implementatie is geslaagd krijgt u een antwoord HTTP OK (200).

Als u andere responscodes, zoals 407 of 502, ziet dit betekent dat dat de proxy is verificatie vereist of dat niet het verkeer wordt toegestaan om een andere reden. Op dit moment kunt u het ondersteuningsteam van uw proxy-server benaderd.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)

- Als u problemen met problemen met de netwerkverbinding van de connector hebt, stel uw vraag in de [Azure Active Directory-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD&forum=WindowsAzureAD) of maak een ticket met ons ondersteuningsteam.
