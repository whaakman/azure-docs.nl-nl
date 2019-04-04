---
title: On-premises gegevensgateway
description: Er is een On-premises gateway nodig als uw Analysis Services-server in Azure maakt verbinding met on-premises gegevensbronnen.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f13dd1282a6384a0acca4c6936fe7900a051795f
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896020"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Verbinding maken met on-premises gegevensbronnen met On-premises gegevensgateway
De on-premises gegevensgateway biedt veilige gegevensoverdracht tussen uw Azure Analysis Services-servers in de cloud en on-premises gegevensbronnen. Naast het werken met meerdere Azure Analysis Services-servers in dezelfde regio bevinden, worden de meest recente versie van de gateway ook werkt met Azure Logic Apps, Power BI, Powerapps en Microsoft Flow. U kunt meerdere services in hetzelfde abonnement en dezelfde regio koppelen met een enkele gateway. 

Installatie met behulp van de gateway aan de eerste keer is een vierdelige proces:

- **Downloaden en uitvoeren van setup** -in deze stap installeert u een gatewayservice op een computer in uw organisatie. U ook aanmelden bij Azure met behulp van een account in uw [van tenant](/previous-versions/azure/azure-services/jj573650(v=azure.100)#BKMK_WhatIsAnAzureADTenant) Azure AD. Azure B2B (Gast)-accounts worden niet ondersteund.

- **Registreren van uw gateway** : In deze stap maakt u een naam en herstel voor uw gateway sleutel en selecteer een regio, uw gateway registreren bij de Gateway-Cloudservice. Uw gateway-resource kan worden geregistreerd in andere regio's, maar het is raadzaam dat deze zich in dezelfde regio als de Analysis Services-servers. 

- **Een gateway-resource maken in Azure** -In deze stap maakt u een gateway-resource maken in uw Azure-abonnement.

- **Verbind uw servers met een resource van uw** -wanneer u een gateway-resource in uw abonnement hebt, kunt u beginnen met uw servers verbinden. Kunt u meerdere servers en andere resources, mits ze zich in hetzelfde abonnement en dezelfde regio bevinden.

U meteen aan de slag, Zie [installeren en configureren van on-premises gegevensgateway](analysis-services-gateway-install.md).

## <a name="how-it-works"> </a>Hoe het werkt
De gateway die u op een computer in uw organisatie installeert wordt uitgevoerd als een Windows-service, **On-premises gegevensgateway**. Deze lokale service is geregistreerd bij de Gateway-Cloudservice via Azure Service Bus. Vervolgens maakt u een resource van een Gateway-Cloudservice voor uw Azure-abonnement. Uw Azure Analysis Services-servers zijn vervolgens verbonden met uw gateway-resource. Wanneer modellen op de server verbinding maken met uw on-premises gegevens bronnen voor query's of verwerking moeten, wordt een stroom query's en passeert de gateway-resource, Azure Service Bus, de lokale on-premises data gateway-service en uw gegevensbronnen. 

![Hoe werkt het?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Query's en -stroom:

1. Een query is gemaakt door de cloudservice met de versleutelde referenties voor de on-premises gegevensbron. Deze wordt verzonden naar een wachtrij voor de gateway om te verwerken.
2. De gateway-cloudservice analyseert de query en stuurt de aanvraag naar de [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. De on-premises gegevensgateway pollt de Azure Service Bus voor in behandeling zijnde aanvragen.
4. De gateway haalt de query, ontsleutelt de referenties en maakt verbinding met de gegevensbronnen met deze referenties.
5. De gateway stuurt de query naar de gegevensbron voor uitvoering.
6. De resultaten worden uit de gegevensbron verzonden naar de gateway en vervolgens naar de cloudservice en de server.

## <a name="windows-service-account"> </a>Windows-serviceaccount
De on-premises gegevensgateway is geconfigureerd voor het gebruik van *NT SERVICE\PBIEgwService* voor de aanmeldingsreferenties voor Windows-service. Standaard heeft deze het recht Aanmelden als een service; in de context van de computer waarop u de gateway installeert. Deze referentie is niet hetzelfde account gebruikt voor verbinding met on-premises gegevensbronnen of uw Azure-account.  

Als u problemen met uw proxyserver vanwege de verificatie ondervindt, kunt u het Windows-serviceaccount wijzigen in een domeingebruiker of beheerd serviceaccount.

## <a name="ports"> </a>Poorten
De gateway maakt een uitgaande verbinding met Azure Service Bus. Deze communiceert via uitgaande poorten: TCP 443 (standaard), 5671, 5672, 9350 via 9354.  De gateway vereist geen inkomende poorten.

We raden je lijst met toegestane adressen de IP-voor de gegevensregio van uw in uw firewall adressen. U kunt downloaden de [lijst met Microsoft Azure Datacenter IP](https://www.microsoft.com/download/details.aspx?id=41653). Deze lijst wordt wekelijks bijgewerkt.

> [!NOTE]
> De IP-adressen die worden vermeld in de lijst met Azure Datacenter IP zijn in CIDR-notatie. Zie voor meer informatie, [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
>
>

Hieronder vindt u de volledig gekwalificeerde domeinnamen die worden gebruikt door de gateway.

| Domeinnamen | Uitgaande poorten | Beschrijving |
| --- | --- | --- |
| *.powerbi.com |80 |HTTP gebruikt voor het downloaden van het installatieprogramma. |
| *.powerbi.com |443 |HTTPS |
| *.analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Advanced Message Queuing Protocol (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Listeners op Service Bus Relay via TCP (vereist 443 voor het ophalen van tokens Access Control) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Gebruikt voor het testen van verbinding met internet als de gateway onbereikbaar voor de Power BI-service is. |
| *.microsoftonline-p.com |443 |Gebruikt voor verificatie, afhankelijk van de configuratie. |

### <a name="force-https"></a>HTTPS-communicatie met Azure Service Bus afdwingen
U kunt afdwingen dat de gateway om te communiceren met Azure Service Bus via HTTPS in plaats van direct via TCP; echter, als dit dus sterk vermindert de prestaties. U kunt de *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* bestand door de waarde van `AutoDetect` naar `Https`. Dit bestand bevindt zich doorgaans in *C:\Program Files\On-premises gegevensgateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="tenant-level-administration"></a>Beheer op tenantniveau 

Er is momenteel geen enkele locatie waar tenantbeheerders alle gateways die andere gebruikers hebben geïnstalleerd en geconfigureerd kunnen beheren.  Als u een tenantbeheerder bent, wordt het aanbevolen vraag van gebruikers in uw organisatie u als beheerder toevoegen aan elke gateway die ze installeren. Hiermee kunt u voor het beheren van alle gateways in uw organisatie via de pagina instellingen van de Gateway of via [PowerShell-opdrachten](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters). 


## <a name="faq"></a>Veelgestelde vragen

### <a name="general"></a>Algemeen

**Q**: Heb ik een gateway nodig voor gegevensbronnen in de cloud, zoals Azure SQL Database? <br/>
**A**: Nee. Een gateway is nodig om verbinding te maken met on-premises gegevensbronnen alleen.

**Q**: Hoeft de gateway te worden geïnstalleerd op dezelfde computer als de gegevensbron? <br/>
**A**: Nee. De gateway moet alleen de mogelijkheid om te verbinden met de server, doorgaans op hetzelfde netwerk.

<a name="why-azure-work-school-account"></a>

**Q**: Waarom moet ik werk-of schoolaccount gebruikt om te melden? <br/>
**A**: U kunt alleen een organisatie-werk- of schoolaccount gebruiken tijdens de installatie van de on-premises gegevensgateway. En dat account moet zich in dezelfde tenant als het abonnement dat u configureert de bron van de gateway in. Uw account aanmelden wordt opgeslagen in een tenant die wordt beheerd door Azure Active Directory (Azure AD). Uw Azure AD-account UPN (User Principal Name) komt meestal overeen met het e-mailadres.

**Q**: Waar worden mijn referenties opgeslagen? <br/>
**A**: De referenties die u voor een gegevensbron invoert, worden versleuteld en opgeslagen in de Gatewaycloudservice. De referenties worden bij de on-premises gegevensgateway ontsleuteld.

**Q**: Zijn er vereisten voor netwerkbandbreedte? <br/>
**A**: Het verdient aanbeveling dat uw netwerkverbinding heeft met een goede doorvoersnelheid. Elke omgeving is anders en de hoeveelheid gegevens die worden verzonden, is van invloed op de resultaten. Door te ExpressRoute gebruiken om te waarborgen van een bepaalde doorvoer tussen on-premises en de Azure-datacenters.
U kunt het hulpprogramma van derden Azure Speed Test-app gebruiken om te meten van de doorvoer.

**Q**: Wat is de latentie voor het uitvoeren van query's met een gegevensbron van de gateway? Wat is de beste architectuur? <br/>
**A**: U kunt de netwerklatentie beperken door de gateway als dicht bij de gegevensbron mogelijk te installeren. Als u de gateway op de daadwerkelijke gegevensbron installeren kunt, wordt deze nabijheid beperkt de latentie minimaal. De datacenters te overwegen. Bijvoorbeeld, als de service wordt gebruikt voor het datacentrum VS-West en u SQL Server die wordt gehost in een Azure-VM hebt, moet uw Azure-VM zijn in de VS-West te. Deze nabijheid latentie wordt geminimaliseerd en voorkomt u kosten voor uitgaand verkeer op de virtuele machine van Azure.

**Q**: Hoe worden resultaten teruggezonden naar de cloud? <br/>
**A**: Resultaten worden verzonden via de Azure Service Bus.

**Q**: Zijn er inkomende verbindingen met de gateway vanuit de cloud? <br/>
**A**: Nee. De gateway gebruikt uitgaande verbindingen naar Azure Service Bus.

**Q**: Wat gebeurt er als ik uitgaande verbindingen Blokkeer? Wat moet ik openen? <br/>
**A**: Zie de poorten en hosts de gateway gebruikt.

**Q**: Wat heet de Windows-service?<br/>
**A**: De gateway heet in Services, On-premises data gateway-service.

**Q**: Kan de Windows-gatewayservice uitvoeren met Azure Active Directory-account? <br/>
**A**: Nee. De Windows-service moet een geldig Windows-account hebben. De service wordt standaard uitgevoerd met de Service-SID, NT SERVICE\PBIEgwService.

**Q**: Hoe kan ik overname een gateway? <br/>
**A**: Aan de overname van een gateway (door het uitvoeren van Setup/wijzigen in het Configuratiescherm > programma's), moet u een eigenaar van de gateway-resource in Azure en de herstelsleutel hebben. Gateway-resource-eigenaren kunnen worden geconfigureerd in de Access Control.

### <a name="high-availability"></a>Hoge beschikbaarheid en herstel na noodgevallen

**Q**: Hoe kunnen we de hoge beschikbaarheid hebben?  
**A**: U kunt een gateway installeren op een andere computer om een cluster te maken. Zie voor meer informatie, [clusters met hoge beschikbaarheid voor On-premises data gateway](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters) in de Power BI Gateway-documenten.

**Q**: Welke opties zijn beschikbaar voor herstel na noodgevallen? <br/>
**A**: U kunt de herstelsleutel gebruiken om te herstellen of verplaatsen van een gateway. Geef de herstelsleutel op tijdens de installatie van de gateway.

**Q**: Wat is het voordeel van de herstelsleutel? <br/>
**A**: De herstelsleutel biedt een manier om te migreren of de gateway-instellingen herstellen na een noodgeval.

## <a name="troubleshooting"> </a>Problemen oplossen

**Q**: Waarom zie ik niet mijn gateway in de lijst met gatewayexemplaren bij het maken van de gateway-resource in Azure? <br/>
**A**: Er zijn twee mogelijke redenen. Eerst wordt dat een resource al is gemaakt voor de gateway in de huidige of een ander abonnement. Als u wilt voorkomen, die het inventariseren van resources van het type **On-premises gegevensgateways** vanuit de portal. Zorg ervoor dat u alle abonnementen selecteren bij het inventariseren van alle resources. Zodra de resource is gemaakt, wordt de gateway wordt niet weergegeven in de lijst met gatewayexemplaren in de portal-ervaring voor Gateway-Resource maken. De tweede mogelijkheid is dat de Azure AD-identiteit van de gebruiker die de gateway hebt geïnstalleerd van de gebruiker is aangemeld bij Azure portal verschilt. Als u wilt oplossen, moet u zich aanmelden bij de portal met hetzelfde account als de gebruiker die de gateway hebt geïnstalleerd.

**Q**: Hoe kan ik zien wat voor query's worden verzonden naar de on-premises gegevensbron? <br/>
**A**: U kunt querytracering inschakelen, waaronder de query's die worden verzonden. Houd er rekening mee te wijzigen van de query traceren terug naar de oorspronkelijke waarde wanneer klaar oplossen van problemen. Querytracering ingeschakeld laten maakt grotere Logboeken.

U kunt ook kijken welke hulpmiddelen die uw gegevensbron voor query's te traceren biedt. U kunt bijvoorbeeld Extended Events of SQL Profiler gebruiken voor SQL Server en Analysis Services.

**Q**: Waar zijn de gatewaylogboeken? <br/>
**A**: Zie logboekbestanden verderop in dit artikel.

### <a name="update"></a>Bijwerken naar de nieuwste versie

Heel wat problemen kunnen surface als de gatewayversie verouderd is. Als de goede en gangbare praktijk, zorg ervoor dat u de meest recente versie. Als u kunt de gateway nog niet hebt bijgewerkt, voor een maand of langer, kunt u Overweeg de installatie van de meest recente versie van de gateway en zien als u het probleem zich voordoet.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fout: Gebruiker toevoegen aan groep is mislukt. (-2147463168 PBIEgwService Prestatielogboekgebruikers)

U kunt deze fout krijgen als u probeert te installeren van de gateway op een domeincontroller wordt niet ondersteund. Zorg ervoor dat u de gateway op een computer die geen domeincontroller implementeert.

## <a name="logs"></a>Logs

Logboekbestanden zijn een belangrijke hulpbron bij het oplossen van problemen.

#### <a name="enterprise-gateway-service-logs"></a>Logboeken van Enterprise-gatewayservice

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Configuratielogboeken

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`


#### <a name="event-logs"></a>Gebeurtenislogboeken

U vindt de logboeken van de Data Management Gateway en PowerBIGateway onder **logboeken toepassingen en Services**.


## <a name="telemetry"></a>Telemetry
Telemetrie kan worden gebruikt voor controle en probleemoplossing. Standaard

**Telemetrie inschakelen**

1.  Controleer de On-premises data gateway clientmap op de computer. Dit is meestal **%systemdrive%\Program Files\On-premises gegevensgateway**. Of u kunt een servicesconsole opent en controleer het pad naar uitvoerbaar bestand: Een eigenschap van de On-premises data gateway-service.
2.  In het bestand Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config van client-directory. Wijzig de instelling SendTelemetry op ' True '.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Sla uw wijzigingen en de Windows-service opnieuw starten: On-premises data gateway-service.




## <a name="next-steps"></a>Volgende stappen
* [Installeren en configureren van on-premises gegevensgateway](analysis-services-gateway-install.md).   
* [Analyseservices beheren](analysis-services-manage.md)
* [Gegevens ophalen uit Azure Analysis Services](analysis-services-connect.md)
