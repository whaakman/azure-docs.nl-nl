---
title: Lokale gegevensgateway | Microsoft Docs
description: Een On-premises gateway is nodig als uw server Analysis Services in Azure maakt verbinding met on-premises gegevensbronnen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 10/30/2017
ms.author: owend
ms.openlocfilehash: 0b11c005ddcf4a3416104e7cef39a7ce97957ba3
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>Verbinding maken met on-premises gegevensbronnen met Azure On-premises Data Gateway
De lokale data gateway fungeert als een brug, bieden veilige gegevensoverdracht tussen uw Azure Analysis Services-servers in de cloud en on-premises gegevensbronnen. Naast het werken met meerdere Azure Analysis Services-servers in dezelfde regio, werkt de meest recente versie van de gateway ook met Azure Logic Apps, Power BI, Power-Apps en Microsoft-Flow. U kunt meerdere services in dezelfde regio koppelen met een enkele gateway. 

Uitvoeren van setup met de gateway de eerste keer is een vierdelige proces:

- **Downloaden en uitvoeren van setup** -deze stap installeert u een gatewayservice op een computer in uw organisatie. U ook aanmelden bij Azure met behulp van een account in uw [van tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) Azure AD. Azure B2B (Gast) accounts worden niet ondersteund.

- **Registreren van uw gateway** - In deze stap maakt u een naam opgeven en herstel van belangrijke voor uw gateway, en selecteer een regio, uw gateway registreren met de Gateway-Cloudservice. Uw gateway resource **moeten worden geregistreerd in dezelfde regio** als Analysis Services-servers. 

- **Een gateway-resource maken in Azure** -In deze stap maakt u een gateway-bron in uw Azure-abonnement.

- **Verbind uw servers met uw gateway resource** -zodra u een gateway-bron in uw abonnement hebt, kunt u beginnen uw servers verbinding mee te maken. U kunt meerdere servers en andere resources verbinden, mits ze zich in de regio.

Als u wilt meteen aan de slag, Zie [installeren en configureren van lokale gegevensgateway](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Hoe het werkt
De gateway die u op een computer in uw organisatie installeert wordt uitgevoerd als een Windows-service **On-premises gegevensgateway**. Deze lokale service is geregistreerd bij de Gateway Cloud Service via Azure Service Bus. Vervolgens maakt u een gateway resource Gateway-Cloudservice voor uw Azure-abonnement. Uw Azure Analysis Services-servers zijn vervolgens verbonden met uw gateway-resource. Wanneer modellen op uw server verbinding moeten maken met uw on-premises gegevens bronnen voor query's of verwerking, passeert een stroom query's en de gateway-resource, Azure Service Bus de gatewayservice voor lokale lokale gegevens en uw gegevensbronnen. 

![Hoe werkt het?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Query's en gegevensstroom:

1. Een query is gemaakt door de cloudservice met de versleutelde referenties voor de on-premises gegevensbron. Vervolgens wordt het verzonden naar een wachtrij voor de gateway om te verwerken.
2. De gateway-cloudservice analyseert de query en stuurt de aanvraag voor de [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. De gegevensgateway lokale Azure Service Bus voor aanvragen in behandeling worden opgevraagd.
4. De gateway opgehaald van de query, ontsleutelt de referenties en verbinding maakt met de gegevensbronnen met deze referenties.
5. De gateway stuurt de query met de gegevensbron voor uitvoering.
6. De resultaten worden uit de gegevensbron verzonden naar de gateway en vervolgens naar de cloudservice en de server.

## <a name="windows-service-account"></a>Windows-serviceaccount
De lokale data gateway is geconfigureerd voor gebruik *NT SERVICE\PBIEgwService* voor de aanmeldingsreferenties van de Windows-service. Deze heeft standaard het recht Aanmelden als service; in de context van de machine die u installeert de gateway op. Deze referentie is niet hetzelfde account gebruikt voor verbinding met on-premises gegevensbronnen of uw Azure-account.  

Als u problemen met de proxyserver als gevolg van verificatie ondervindt, wilt u mogelijk de Windows-serviceaccount wijzigen in een domeingebruiker of beheerd serviceaccount.

## <a name="ports"></a>Poorten
De gateway maakt een uitgaande verbinding met Azure Service Bus. Deze communiceert op uitgaande poorten: TCP 443 (standaard), 5671, 5672, 9350 via 9354.  De gateway is niet vereist voor poorten voor inkomend verkeer.

We raden u goedgekeurde IP-adressen de IP-voor de regio van uw gegevens in uw firewall adressen. U kunt downloaden via de [lijst met Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653). Deze lijst is wekelijks bijgewerkt.

> [!NOTE]
> De IP-adressen die worden vermeld in de lijst met Azure Datacenter IP zijn in CIDR-notatie. 10.0.0.0/24 betekent bijvoorbeeld niet 10.0.0.0 via 10.0.0.24. Meer informatie over de [CIDR-notatie](http://whatismyipaddress.com/cidr).
>
>

Hieronder vindt u de volledig gekwalificeerde domeinnamen gebruikt door de gateway.

| Domeinnamen | Uitgaande poorten | Beschrijving |
| --- | --- | --- |
| *. powerbi.com |80 |HTTP gebruikt voor het downloaden van het installatieprogramma. |
| *. powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Geavanceerde Message Queuing-Protocol (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |Listeners op Service Bus Relay via TCP (443 voor toegangsbeheer token aanschaf vereist) |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| Login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Gebruikt voor het testen van verbinding met internet, als de gateway onbereikbaar zijn door de Power BI-service is. |
| *.microsoftonline p.com |443 |Gebruikt voor verificatie, afhankelijk van de configuratie. |

### <a name="force-https"></a>HTTPS-communicatie met Azure Service Bus forceren
U kunt de gateway om te communiceren met Azure Service Bus via HTTPS in plaats van rechtstreekse TCP; afdwingen echter, doen dus aanzienlijk vermindert de prestaties. U kunt de *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* bestand door het wijzigen van de waarde van `AutoDetect` naar `Https`. Dit bestand bevindt zich doorgaans in *C:\Program Files\On-premises gegevensgateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

**Q**: moet ik een gateway voor gegevensbronnen in de cloud, zoals Azure SQL Database? <br/>
**Een**: Nee. Een gateway is nodig voor het verbinden met on-premises gegevensbronnen alleen.

**Q**: de gateway hoeft te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**Een**: Nee. De gateway moet alleen de mogelijkheid om te verbinden met de server, doorgaans op hetzelfde netwerk.

<a name="why-azure-work-school-account"></a>

**Q**: Waarom moet ik een werk- of schoolaccount gebruiken om te melden? <br/>
**Een**: U kunt alleen gebruiken een organisatie werk of schoolaccount tijdens de installatie van de lokale data gateway. En dat account moet zich in dezelfde tenant als het abonnement u configureert de bron van de gateway in. Uw account wordt opgeslagen in een tenant die wordt beheerd door Azure Active Directory (Azure AD). Uw Azure AD-account UPN (user Principal name) normaal gesproken komt overeen met het e-mailadres.

**Q**: waar mijn referenties worden opgeslagen? <br/>
**Een**: de referenties die u voor een gegevensbron opgeeft zijn versleuteld en opgeslagen in de Gateway-Cloudservice. De referenties worden op de lokale data gateway ontsleuteld.

**Q**: zijn er vereisten voor de bandbreedte van het netwerk? <br/>
**Een**: het raadzaam de netwerkverbinding goed doorvoer heeft. Elke omgeving is verschillend en de hoeveelheid gegevens die worden verzonden, is van invloed op de resultaten. Met behulp van ExpressRoute kan helpen om bescherming te bieden een mate van doorvoer tussen on-premises en de Azure-datacenters.
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
**Een**: In Services, On-premises data gateway-service voor de gateway wordt aangeroepen.

**Q**: kan de gateway Windows-service worden uitgevoerd met Azure Active Directory-account? <br/>
**Een**: Nee. De Windows-service moet een geldige Windows-account hebben. De service wordt standaard uitgevoerd met de Service-SID NT SERVICE\PBIEgwService.

**Q**: hoe kan ik overname een gateway? <br/>
**Een**: In zodat overname een gateway (door het uitvoeren van Setup/wijzigen in het Configuratiescherm > programma's) moet u om te worden van een eigenaar voor de gateway-resource in Azure en de herstelsleutel. Gateway resource eigenaren kunnen worden geconfigureerd in de Access Control.

### <a name="high-availability"></a>Hoge beschikbaarheid en herstel na noodgevallen

**Q**: welke opties zijn beschikbaar voor herstel na noodgevallen? <br/>
**Een**: U kunt de herstelsleutel gebruiken om te zetten of verplaatsen van een gateway. Wanneer u de gateway installeren, geeft u de herstelsleutel.

**Q**: Wat is het voordeel van de herstelsleutel? <br/>
**Een**: de herstelsleutel biedt een manier om te migreren of de gateway-instellingen herstellen na een noodgeval.

## <a name="troubleshooting"></a>Probleemoplossing

**Q**: Waarom zie ik niet mijn gateway in de lijst met gatewayexemplaren bij het maken van de bron van de gateway in Azure? <br/>
**Een**: Er zijn twee mogelijke oorzaken. De eerste is dat een resource al is gemaakt voor de gateway in de huidige of een ander abonnement. Om te voorkomen dat mogelijkheid, opsommen resources van het type **gegevensgateways On-premises** vanuit de portal. Zorg ervoor dat alle abonnementen selecteren bij het inventariseren van alle resources. Houd er rekening mee dat zodra de resource is gemaakt, de gateway niet in de lijst met de gatewayexemplaren in de portal ervaring voor de Gateway-Resource maken weergegeven wordt. De tweede mogelijkheid is dat de identiteit van de Azure AD van de gebruiker die de gateway geïnstalleerd van de gebruiker is aangemeld bij Azure Portal verschilt. U kunt dit oplossen door het aanmelden bij de portal met hetzelfde account als de gebruiker die de gateway wordt geïnstalleerd.

**Q**: hoe kan ik zien wat query's worden verzonden naar de on-premises gegevensbron? <br/>
**Een**: U kunt inschakelen querytracering, waaronder de query's die worden verzonden. Vergeet niet om te wijzigen van de query traceren terug naar de oorspronkelijke waarde gedaan bij het oplossen van problemen. Querytracering ingeschakeld verlaten maakt grotere Logboeken.

U kunt ook hulpprogramma's die de gegevensbron voor tracering van query's heeft bekijken. U kunt bijvoorbeeld de Extended Events of SQL Profiler gebruiken voor SQL Server en Analysis Services.

**Q**: waar zich de logboeken van de gateway? <br/>
**Een**: Zie de logboeken verderop in dit artikel.

### <a name="update"></a>Bijwerken naar de nieuwste versie

Veel problemen kunnen surface wanneer de gatewayversie is verouderd. Zorg ervoor dat u met de meest recente versie als goed over het algemeen. Als u kunt de gateway nog niet hebt bijgewerkt, gedurende een maand of langer, kunt u Overweeg de installatie van de meest recente versie van de gateway en zien als u het probleem kan reproduceren.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fout: Kan gebruiker niet toevoegen aan groep. (-2147463168 PBIEgwService Prestatielogboekgebruikers)

U kunt deze fout kan ophalen, als u probeert de gateway installeren op een domeincontroller wordt niet ondersteund. Zorg ervoor dat u de gateway op een computer die een domeincontroller niet implementeert.

## <a name="logs"></a>Logboeken

Logboekbestanden zijn een belangrijke bron wanneer het oplossen van problemen.

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway-service-Logboeken

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Configuratielogboeken

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Gebeurtenislogboeken

U vindt de logboeken van de Data Management Gateway en PowerBIGateway onder **servicelogboeken voor toepassingen en**.


## <a name="telemetry"></a>Telemetrie
Telemetrie kan worden gebruikt voor bewaking en probleemoplossing. Standaard

**Telemetrie inschakelen**

1.  Controleer de clientmap in On-premises gateway op de computer. Dit is meestal **%systemdrive%\Program Files\On-premises gegevensgateway**. U kunt een servicesconsole opent en controleer het pad naar het uitvoerbare bestand: een eigenschap van de On-premises gegevens gateway-service.
2.  In het bestand Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config van clientmap. Wijzig de instelling SendTelemetry op true.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  De wijzigingen opslaan en de Windows-service opnieuw starten: On-premises data gateway-service.




## <a name="next-steps"></a>Volgende stappen
* [Installeren en configureren van lokale gegevensgateway](analysis-services-gateway-install.md).   
* [Analyseservices beheren](analysis-services-manage.md)
* [Gegevens ophalen uit Azure Analysis Services](analysis-services-connect.md)
