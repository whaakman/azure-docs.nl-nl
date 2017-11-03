---
title: Geografisch gedistribueerde schaal met App Service-omgevingen
description: Informatie over het horizontaal schalen apps met behulp van geo-distributie met Traffic Manager en App Service-omgevingen.
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: 21f747239e565aba79a84c8e946a71e306b64968
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>Geografisch gedistribueerde schaal met App Service-omgevingen
## <a name="overview"></a>Overzicht
Toepassingsscenario's waarvoor zeer grote schaal kunnen de resource rekencapaciteit beschikbaar is voor een implementatie van een app overschrijden.  Uw stem toepassingen, zijn sportevenementen en televisie entertainment gebeurtenissen alle voorbeelden van scenario's waarvoor zeer grote schaal. Grote schaal vereisten kan worden voldaan door apps, horizontaal uitbreiden met meerdere app-implementaties wordt uitgevoerd in één regio, evenals tussen regio's, voor het verwerken van de vereisten voor het extreme laden.

App Service-omgevingen zijn een ideaal platform voor horizontale scale-out.  Eenmaal een App-serviceomgeving configuratie is geselecteerd die ondersteuning voor een bekende aanvraagsnelheid biedt, ontwikkelaars kunnen implementeren aanvullende App Service-omgevingen in 'deegvorm' manier om een gewenste piek load capaciteit bereiken.

Stel bijvoorbeeld dat een app uitgevoerd op de configuratie van een App Service-omgeving is getest om te verwerken 20K-aanvragen per seconde (RPS).  Als de gewenste piek load capaciteit 100K RPS is, worden vervolgens vijf (5)-App Service-omgevingen gemaakt en geconfigureerd om te controleren of dat de toepassing de maximale verwachte belasting kan verwerken.

Aangezien klanten is doorgaans toegang krijgen apps met behulp van een aangepast (of vanity)-domein tot, moeten ontwikkelaars een manier om de app aanvragen verdelen over alle van de exemplaren van de App Service-omgeving.  Een uitstekende manier om dit te bereiken is om op te lossen het aangepaste domein met behulp van een [Azure Traffic Manager-profiel][AzureTrafficManagerProfile].  Traffic Manager-profiel kan worden geconfigureerd dat alle afzonderlijke App Service-omgevingen.  Traffic Manager verwerkt automatisch door de distributie klanten voor alle van de App Service-omgevingen op basis van de instellingen in het Traffic Manager-profiel voor taakverdeling.  Deze methode werkt ongeacht of alle van de App Service-omgevingen zijn geïmplementeerd in één Azure-regio, of wereldwijd over meerdere Azure-regio's.

Klanten zich niet bewust zijn van het aantal App Service-omgevingen met een app, zelfs nadat u klanten toegang krijgen tot apps via het vanity-domein.  Als gevolg hiervan kunnen ontwikkelaars snel en eenvoudig toevoegen en verwijderen, App Service-omgevingen op basis van waargenomen belasting.

Het conceptuele diagram hieronder ziet u een app horizontaal uitgebreid tussen de drie App Service-omgevingen in één regio.

![Conceptionele architectuur][ConceptualArchitecture] 

De rest van dit onderwerp wordt begeleid bij de stappen die betrokken zijn bij het instellen van een gedistribueerde topologie voor de voorbeeld-app met behulp van meerdere App Service-omgevingen.

## <a name="planning-the-topology"></a>De topologie plannen
Voordat u het opzetten van een gedistribueerde app footprint, is het nuttig om er zijn enkele softwareonderdelen gegevens tevoren.

* **Aangepast domein voor de app:** wat is de aangepaste domeinnaam die klanten gebruiken voor toegang tot de app?  Voor de voorbeeld-app de aangepaste domeinnaam is *www.scalableasedemo.com*
* **Traffic Manager-domein:** een domeinnaam moet worden gekozen bij het maken van een [Azure Traffic Manager-profiel][AzureTrafficManagerProfile].  Deze naam wordt gecombineerd met de *trafficmanager.net* achtervoegsel voor het registreren van een domein-vermelding die wordt beheerd door Traffic Manager.  De gekozen naam is voor de voorbeeld-app *schaalbare-as-omgeving-demo*.  Als gevolg hiervan de volledige domeinnaam die wordt beheerd door Traffic Manager is *schaalbare op as-omgeving demo.trafficmanager.net*.
* **Strategie voor de voetafdruk van de app schalen:** de voetafdruk van de toepassing worden verdeeld over meerdere App Service-omgevingen in één regio?  Meerdere regio's?  Een combinatie-and-match van beide benaderingen?  De beslissing moet worden gebaseerd op de verwachtingen van waar het klantverkeer van de worden verzonden, evenals hoe goed de rest van de back-endinfrastructuur voor ondersteuning van een app kan worden geschaald.  Bijvoorbeeld, met een stateless toepassing 100% kan een app worden massively uitgebreid met een combinatie van meerdere App Service-omgevingen per Azure-regio, vermenigvuldigd met App Service-omgevingen is geïmplementeerd op meerdere Azure-regio's.  Met 15 + openbare Azure-regio's kunt kiezen, kunnen klanten echt een footprint wereldwijd hyperschaal toepassing bouwen.  Voor de voorbeeld-app gebruikt voor dit artikel, zijn de drie App Service-omgevingen in één Azure-regio (Zuid-centraal VS) gemaakt.
* **Naamgevingsregels voor de App Service-omgevingen:** elke App Service-omgeving vereist een unieke naam op.  Afgezien van één of twee App Service-omgevingen is het handig om een naamconventie om elke App Service-omgeving te identificeren.  Een eenvoudige naamconventie is voor de voorbeeld-app gebruikt.  De namen van de drie App Service-omgevingen zijn *fe1ase*, *fe2ase*, en *fe3ase*.
* **Naamgevingsregels voor de apps:** omdat meerdere exemplaren van de app wordt geïmplementeerd, een naam voor elk exemplaar van de geïmplementeerde app is vereist.  Eén weinig bekende maar zeer handige functie van App Service-omgevingen is dat dezelfde naam van de app kan worden gebruikt in meerdere omgevingen van App Service.  Aangezien elke App Service-omgeving een unieke domeinachtervoegsel heeft, kunt ontwikkelaars exact dezelfde naam van de app opnieuw te gebruiken in elke omgeving.  Zo zou een ontwikkelaar apps als volgt met de naam kan hebben: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, enzovoort.  Voor de voorbeeld-app al elk exemplaar van de app ook een unieke naam heeft.  De namen van de app-exemplaar gebruikt *webfrontend1*, *webfrontend2*, en *webfrontend3*.

## <a name="setting-up-the-traffic-manager-profile"></a>Instellen van het Traffic Manager-profiel
Als meerdere exemplaren van een app zijn geïmplementeerd op meerdere App Service-omgevingen, kunnen de exemplaren van de afzonderlijke app met Traffic Manager zijn geregistreerd.  Voor de voorbeeld-app een Traffic Manager-profiel is nodig voor *schaalbare op as-omgeving demo.trafficmanager.net* die klanten met een van de volgende exemplaren van de geïmplementeerde app kunt versturen:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de eerste App Service-omgeving.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de tweede App Service-omgeving.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de derde App Service-omgeving.

De eenvoudigste manier om het registreren van meerdere Azure App Service-eindpunten, alle actieve in de **dezelfde** Azure-regio, wordt met de Powershell [ondersteuning van Azure Resource Manager Traffic Manager][ARMTrafficManager].  

De eerste stap is om een Azure Traffic Manager-profiel te maken.  De onderstaande code ziet u hoe het profiel is gemaakt voor de voorbeeld-app:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

U ziet hoe de *RelativeDnsName* parameter is ingesteld op *schaalbare-as-omgeving-demo*.  Dit is hoe de domeinnaam *schaalbare op as-omgeving demo.trafficmanager.net* is gemaakt en gekoppeld aan een Traffic Manager-profiel.

De *TrafficRoutingMethod* parameter definieert het beleid Traffic Manager wordt gebruikt om te bepalen hoe de belasting van klanten verdeeld over alle beschikbare eindpunten van taakverdeling.  In dit voorbeeld de *gewogen* methode hebt gekozen.  Dit leidt ertoe dat de aanvragen van klanten wordt verdeeld over alle van de geregistreerde toepassing eindpunten op basis van het relatieve gewicht die is gekoppeld aan elk eindpunt. 

Met het profiel dat is gemaakt, wordt elk exemplaar van de app toegevoegd aan het profiel als systeemeigen Azure-eindpunt.  De onderstaande code een verwijzing naar elke front-end web-app kan worden opgehaald en worden vervolgens elke app toegevoegd als een Traffic Manager-eindpunt van de *TargetResourceId* parameter.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

U ziet hoe er wordt een aanroep naar *toevoegen AzureTrafficManagerEndpointConfig* voor elke afzonderlijke app-exemplaar.  De *TargetResourceId* parameter in elke Powershell-opdracht verwijst naar een van de drie geïmplementeerde app-exemplaren.  Traffic Manager-profiel wordt load verdeeld over alle drie eindpunten geregistreerd in het profiel.

Alle van de drie eindpunten dezelfde waarde (10) gebruiken voor de *gewicht* parameter.  Dit resulteert in Traffic Manager verspreiding klantaanvragen over alle exemplaren van de drie app relatief gelijkmatig. 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Aangepast domein van de App op het Traffic Manager-domein aan te wijzen
De laatste stap nodig is voor het aangepaste domein van de app op het Traffic Manager-domein.  Voor de voorbeeld-app betekent dit dat wijzen *www.scalableasedemo.com* op *schaalbare op as-omgeving demo.trafficmanager.net*.  Deze stap moet worden voltooid met de domeinregistrar dat het aangepaste domein beheert.  

Met behulp van uw registrar domein beheerhulpprogramma's, registreert een CNAME moet worden gemaakt die wijst het aangepaste domein in het Traffic Manager-domein.  De volgende afbeelding toont een voorbeeld van hoe deze configuratie CNAME uitziet:

![CNAME voor een aangepast domein][CNAMEforCustomDomain] 

Hoewel het niet wordt gedekt in dit onderwerp, houd er rekening mee dat elk exemplaar van de afzonderlijke Apps moet zijn van het aangepaste domein dat is geregistreerd bij deze ook.  Anders als een aanvraag voor een app-exemplaar gemaakt en de toepassing niet het aangepaste domein dat is geregistreerd bij de app heeft, mislukt de aanvraag.  

In dit voorbeeld het aangepaste domein is *www.scalableasedemo.com*, en elk toepassingsexemplaar heeft het aangepaste domein is gekoppeld.

![Aangepast domain][CustomDomain] 

Zie voor een samenvatting van een aangepast domein registreren met Azure App Service-apps, het volgende artikel op [registreren van aangepaste domeinen][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>De gedistribueerde topologie uitprobeert
Het eindresultaat van de Traffic Manager- en DNS-configuratie is die aanvragen voor *www.scalableasedemo.com* worden overgebracht via de volgende volgorde:

1. Een browser of een apparaat wordt een DNS-zoekopdracht maken voor *www.scalableasedemo.com*
2. De CNAME-vermelding bij de domeinregistrar zorgt ervoor dat de DNS-zoekopdracht wordt omgeleid naar Azure Traffic Manager.
3. Een DNS-zoekopdracht wordt uitgevoerd voor *schaalbare op as-omgeving demo.trafficmanager.net* tegen een van de Azure Traffic Manager-DNS-servers.
4. Op basis van het beleid voor taakverdeling (de *TrafficRoutingMethod* parameter eerder gebruikt bij het maken van Traffic Manager-profiel), Traffic Manager Selecteer een van de geconfigureerde eindpunten en de FQDN-naam van dat eindpunt terug naar de browser of het apparaat.
5. Aangezien de FQDN-naam van het eindpunt de Url van een app-exemplaar op een App Service-omgeving is, vraagt de browser of het apparaat een Microsoft Azure DNS-server de FQDN-naam omzetten in een IP-adres. 
6. De browser of het apparaat stuurt de aanvraag HTTP/S naar het IP-adres.  
7. De aanvraag aankomt in één van de app-exemplaren uitgevoerd op een van de App Service-omgevingen.

De console van de volgende afbeelding ziet u een DNS-zoekopdracht voor een aangepast domein van de voorbeeld-app met succes te herleiden tot een app-exemplaar op een van de drie voorbeeld-App Service-omgevingen (in dit geval de seconde van de drie App Service-omgevingen):

![DNS-zoekopdracht][DNSLookup] 

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Documentatie over de Powershell [ondersteuning van Azure Resource Manager Traffic Manager][ARMTrafficManager].  

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: ../app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
