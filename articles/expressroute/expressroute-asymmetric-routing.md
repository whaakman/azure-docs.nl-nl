<properties
   pageTitle="Asymmetrische routering | Microsoft Azure"
   description="In dit artikel komen problemen met asymmetrische routering aan de orde die een klant kan tegenkomen in het netwerk wanneer het meerdere koppelingen naar een bestemming bevat."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="osamazia"/>

# Asymmetrische routering met meerdere netwerkpaden

In dit artikel wordt uitgelegd hoe uitgaand en retourverkeer verschillende routes kan nemen wanneer er meerdere paden beschikbaar zijn tussen bron en bestemming.

Om asymmetrische routering te begrijpen, moeten we twee concepten begrijpen. Het ene is de impact van meerdere netwerkpaden. Het andere is het gedrag van de apparaten die de status bijhouden, zoals firewalls. Deze apparaten worden stateful apparaten genoemd. Een combinatie van deze twee factoren zorgt voor scenario's waarin het verkeer door een stateful apparaat wordt verwijderd omdat niet wordt gezien dat het verkeer via zichzelf afkomstig is.

## Meerdere netwerkpaden

Wanneer een bedrijfsnetwerk slechts één koppeling met internet heeft via de internetprovider, komt al het verkeer naar en van internet via hetzelfde pad. Bedrijven schaffen vaak meerdere aansluitingen aan, als redundante paden, ter verbetering van de uptime van het netwerk. In dergelijke gevallen is het mogelijk dat verkeer dat buiten het netwerk naar internet gaat via de ene koppeling gaat, terwijl het retourverkeer via een andere koppeling komt. Dit verschijnsel staat bekend als asymmetrische routering, waarbij het terugkerende verkeer een ander pad neemt dan de oorspronkelijke stroom.

![Routing3](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

De bovenstaande beschrijving is voor internet, maar geldt ook voor andere combinaties van meerdere paden. Voorbeelden zijn een internetpad en een persoonlijk pad naar dezelfde bestemming, meerdere persoonlijke paden naar dezelfde bestemming, enz. 

Elke router langs het pad van bron naar bestemming berekent het beste pad om een bestemming te bereiken, op basis van zijn berekening van het beste pad om de bestemming te bereiken. Het bepalen van het best mogelijke pad is gebaseerd op twee hoofdfactoren.

1.  Routering tussen externe netwerken is gebaseerd op het routeringsprotocol genaamd Border Gateway Protocol, dat ook wel BGP wordt genoemd. BGP kijkt naar aankondigingen van neighbors en voert hier een aantal stappen op uit om het beste pad naar de bestemming te bepalen en installeert dit in zijn routeringstabel.
2.  Lengte van subnetmasker dat is gekoppeld aan een route. Als meerdere aankondigingen voor hetzelfde IP-adres maar met verschillende subnetmaskers worden ontvangen, krijgt de aankondiging met het langere subnetmasker de voorkeur, omdat deze als een meer specifieke route wordt beschouwd.

## Stateful apparaten

Routers kijken naar de IP-header van het pakket voor routeringsdoeleinden. Er zijn echter apparaten die nog dieper in het pakket kijken. Doorgaans kijken deze apparaten naar headers van Layer4 (TCP/UDP) of zelfs Layer7 (toepassingslaag). Dit zijn beveiligingsapparaten of apparaten die de bandbreedte optimaliseren. Een firewall is een algemeen voorbeeld van een stateful apparaat. De firewall staat een pakket toe of weigert een pakket via zijn interfaces op basis van verschillende velden, zoals protocol, TCP/UDP-poort en URL-headers. Deze pakketinspectie geeft veel verwerkingsbelasting op het apparaat. Om de prestaties te verbeteren, inspecteert de firewall het eerste pakket van een stroom. Als het pakket is toegestaan, bewaart de firewall de stroomgegevens in de statustabel. Alle volgende pakketten met betrekking tot deze stroom zijn toegestaan op basis van de eerste beslissing. Dus als een pakket, dat deel uitmaakt van een bestaande stroom, bij de firewall aankomt en de firewall er geen eerdere statusinformatie over heeft, wordt dit pakket door de firewall verwijderd.

## Asymmetrische routering met ExpressRoute

Als u verbinding maakt met Microsoft via ExpressRoute, vinden de volgende wijzigingen plaats in uw netwerk.

1.  U hebt meerdere koppelingen naar Microsoft. De ene koppeling is uw bestaande internetverbinding en de andere gaat via ExpressRoute. Bepaald verkeer naar Microsoft kan via internet gaan maar terugkeren via ExpressRoute of andersom.
2.  U ontvangt specifiekere IP-adressen via ExpressRoute. Verkeer van uw netwerk naar Microsoft voor services die worden aangeboden via ExpressRoute geeft dus altijd de voorkeur aan ExpressRoute. 

Om de gevolgen van de bovenstaande twee punten te begrijpen, doorlopen we een paar scenario's. Stel dat u slechts één aansluiting op internet heeft en alle Microsoft-services via internet gebruikt. Het verkeer van uw netwerk naar Microsoft en terug passeert via dezelfde internetkoppeling en passeert de firewall. De firewall registreert de stroom wanneer deze het eerste pakket ziet en retourpakketten worden toegestaan, aangezien de stroom in de statustabel bestaat.

![Routing1](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)


Nu schakelt u ExpressRoute in en gebruikt u services die worden aangeboden door Microsoft via ExpressRoute. Alle andere services van Microsoft worden via internet gebruikt. U implementeert een afzonderlijke firewall aan de rand van uw verbinding met ExpressRoute. Microsoft zal specifiekere voorvoegsels naar het netwerk via ExpressRoute sturen voor specifieke services. Uw routeringsinfrastructuur zal voor die voorvoegsels kiezen voor ExpressRoute als voorkeurspad. Als u uw openbare IP-adressen niet naar Microsoft stuurt via ExpressRoute, zal Microsoft via internet met uw openbare IP-adressen communiceren. Voorwaarts verkeer van uw netwerk naar Microsoft gebruikt dus ExpressRoute terwijl terugkerend verkeer van Microsoft gebruikmaakt van internet. Wanneer de firewall aan de rand een antwoordpakket voor een stroom ziet die niet wordt gevonden in de tabel, wordt het retourverkeer verwijderd. 

Als u ervoor kiest om dezelfde NAT-pool te gebruiken voor ExpressRoute en voor internet, treden er vergelijkbare problemen op met de clients met persoonlijke IP-adressen in uw netwerk. Aanvragen voor services zoals Windows Update verlopen via internet omdat IP-adressen voor deze services niet worden verzonden via ExpressRoute. Het retourverkeer keert echter terug via ExpressRoute. Als Microsoft een IP-adres met hetzelfde subnetmasker van internet en ExpressRoute ontvangt, verkiest het ExpressRoute boven internet. Als een firewall of een ander stateful apparaat aan de rand van uw netwerk, gericht op ExpressRoute, geen eerdere informatie over de stroom heeft, worden de pakketten die behoren tot deze gegevensstroom verwijderd. 

## Oplossingen voor asymmetrische routering

Er zijn twee manieren voor het oplossen van het probleem met asymmetrische routering. Een is via routering en de andere via op brongebaseerde NAT (SNAT). 

1. Routering 

    - U moet ervoor zorgen dat uw openbare IP-adressen worden verzonden naar de juiste WAN-koppelingen. Bijvoorbeeld als u internet wilt gebruiken voor verificatieverkeer en ExpressRoute voor uw e-mailverkeer. U moet uw openbare IP-adressen van ADFS dan niet verzenden via ExpressRoute. Evenzo moet de on-premises ADFS-server niet worden blootgesteld aan IP-adressen die via ExpressRoute worden ontvangen. Routes die worden ontvangen via ExpressRoute zijn specifieker, zodat ExpressRoute het voorkeurspad wordt voor verificatieverkeer naar Microsoft, en dit veroorzaakt asymmetrische routering.

    - Als u ExpressRoute wilt gebruiken voor verificatie moet u er vervolgens voor zorgen dat u openbare IP-adressen van ADFS via ExpressRoute verzendt zonder NAT. Op deze manier gaat verkeer dat afkomstig is van Microsoft naar een on-premises ADFS-server via ExpressRoute en maakt retourverkeer van de klant naar Microsoft gebruik van ExpressRoute omdat dit de voorkeur krijgt boven internet. 

2. Brongebaseerde NAT

    Een andere manier voor het oplossen van problemen met asymmetrische routering is via bron-NAT (SNAT). Bijvoorbeeld als u niet het openbare IP-adres van de on-premises SMTP-server hebt verzonden via ExpressRoute met de intentie internet te gebruiken voor deze communicatie. Een aanvraag afkomstig van Microsoft naar uw on-premises SMTP-server gaat via internet. U verzendt via bron-NAT de inkomende aanvraag naar een intern IP-adres. Terugkerend verkeer van de SMTP-server gaat naar de firewall aan de rand (gebruikt voor NAT) in plaats van ExpressRoute. Op deze manier keert het retourverkeer terug via internet. 


![Routing2](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## Detectie van asymmetrische routering

Traceroute is de beste manier om ervoor te zorgen dat verkeer via het verwachte pad gaat. Als u verwacht dat verkeer van de on-premises SMTP-server naar Microsoft via het internetpad gaat, pas dan traceroute toe vanaf de SMTP-server naar Office 365. Er wordt gevalideerd of het verkeer inderdaad uw netwerk verlaat richting internet en niet naar ExpressRoute. 





<!--HONumber=ago16_HO5-->


