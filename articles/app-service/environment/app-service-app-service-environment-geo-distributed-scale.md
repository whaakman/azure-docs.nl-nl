---
title: Geografisch gedistribueerde schaal met App Service-omgevingen
description: Leer hoe u apps met behulp van geo-distributie met Traffic Manager en App Service-omgevingen horizontaal te schalen.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: bc85139dfa3589baf6505fac2269f8755dcaddc8
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213245"
---
# <a name="geo-distributed-scale-with-app-service-environments"></a>Geografisch gedistribueerde schaal met App Service-omgevingen
## <a name="overview"></a>Overzicht
Toepassingsscenario's waarvoor zeer grote schaal kunnen groter zijn dan de capaciteit van compute resource beschikbaar is voor een implementatie van een app.  Uw stem toepassingen, zijn sportevenementen en televisie entertainment gebeurtenissen alle voorbeelden van scenario's waarin zeer grote schaal. Grote schaalvereisten kan worden voldaan door apps, horizontaal uitschalen met meerdere app-implementaties binnen één regio, evenals in regio's worden ingediend bij het verwerken van de vereisten voor het extreme laden.

App Service-omgevingen zijn een ideaal platform voor horizontaal uitschalen.  Zodra een App Service Environment configuration is geselecteerd die ondersteuning voor een bekende aanvraagsnelheid bieden, ontwikkelaars kunnen implementeren extra App Service-omgevingen in "cookie snijden" gestart voor het bereiken van een gewenste piek load-capaciteit.

Stel bijvoorbeeld dat een app die wordt uitgevoerd op de configuratie van een App Service-omgeving is getest voor het afhandelen van 20K aanvragen per seconde (RPS).  Als de gewenste piek load-capaciteit 100K RPS is, worden klikt u vervolgens vijf (5) App Service-omgevingen gemaakt en geconfigureerd om te controleren of dat de toepassing de verwachte maximumbelasting kan verwerken.

Aangezien klanten hebben doorgaans toegang apps met behulp van een aangepaste (of vanity)-domein tot, moeten ontwikkelaars een manier voor het distribueren van app-aanvragen voor alle de App Service-omgeving.  Een uitstekende manier om dit te doen is om op te lossen de aangepast domein met behulp van een [Azure Traffic Manager-profiel][AzureTrafficManagerProfile].  Traffic Manager-profiel kan worden geconfigureerd om te verwijzen naar alle van de afzonderlijke App Service-omgevingen.  Traffic Manager verwerkt automatisch naar klanten op alle van de App Service-omgevingen op basis van de load balancer-instellingen in het Traffic Manager-profiel.  Deze benadering werkt onafhankelijk van of alle van de App Service-omgevingen zijn geïmplementeerd in één Azure-regio of wereldwijd geïmplementeerd in meerdere Azure-regio's.

Klanten zijn niet op de hoogte van het aantal App Service-omgevingen met een app, zelfs nadat klanten toegang apps via het aangepaste domein tot.  Als gevolg hiervan kunnen ontwikkelaars snel en eenvoudig toevoegen en verwijderen, App Service-omgevingen op basis van waargenomen te verdelen.

Het conceptuele diagram hieronder ziet u een app horizontaal uitgeschaald over drie App Service-omgevingen binnen één regio.

![Conceptuele architectuur][ConceptualArchitecture] 

De rest van dit onderwerp worden de stappen beschreven die betrokken zijn bij het instellen van een gedistribueerde topologie voor de voorbeeld-app met behulp van meerdere App Service-omgevingen.

## <a name="planning-the-topology"></a>Planning van de topologie
Voordat u het opzetten van een distributed app footprint kunt zo u een aantal onderdelen gestroomlijnder wanneer u gegevens hebt.

* **Het aangepaste domein voor de app:** wat is de naam van het aangepaste domein dat klanten toegang tot de app wordt gebruikt?  Voor de voorbeeld-app de aangepaste domeinnaam is *www.scalableasedemo.com*
* **Traffic Manager-domein:** een domeinnaam moet worden gekozen bij het maken van een [Azure Traffic Manager-profiel][AzureTrafficManagerProfile].  Deze naam wordt gecombineerd met de *trafficmanager.net* achtervoegsel voor het registreren van een domein-item dat wordt beheerd door Traffic Manager.  Voor de voorbeeld-app, is het de naam van de gekozen *schaalbare-as-omgeving-demo*.  De volledige domeinnaam die wordt beheerd door Traffic Manager is daardoor *schaalbare-as-omgeving-demo.trafficmanager.net*.
* **Strategie voor het schalen van de app-voetafdruk:** wordt de voetafdruk van de toepassing worden verdeeld over meerdere App Service-omgevingen in één regio?  Meerdere regio's?  Een en-combineren van beide methoden?  De beslissing moet worden gebaseerd op de verwachtingen van waaruit klantenverkeer wordt uitgevoerd, evenals hoe goed de rest van de back-endinfrastructuur voor ondersteuning van van een app kunt schalen.  Bijvoorbeeld, met een 100% staatloze toepassingen, kan een app worden zeer geschaald met behulp van een combinatie van meerdere App Service-omgevingen per Azure-regio, vermenigvuldigd met App Service-omgevingen in meerdere Azure-regio's.  Klanten met 15 + openbare Azure-regio's beschikbaar om de verkeersbelasting, kunnen echt een footprint wereldwijd grootschalige toepassingen bouwen.  Voor de voorbeeldapp die wordt gebruikt voor dit artikel, zijn drie App Service-omgevingen gemaakt in één Azure-regio (Zuid-centraal VS).
* **Naamgevingsregels voor de App Service-omgevingen:** voor elke App Service-omgeving moet een unieke naam.  Meer dan één of twee App Service-omgevingen is het handig om een naamconventie gebruikt voor het identificeren van elke App Service-omgeving.  Een eenvoudige naamconventie is voor de voorbeeld-app gebruikt.  De namen van de drie App Service-omgevingen zijn *fe1ase*, *fe2ase*, en *fe3ase*.
* **Naamgevingsregels voor de apps:** omdat meerdere exemplaren van de app worden geïmplementeerd, een naam is vereist voor elk exemplaar van de geïmplementeerde app.  Een beetje bekende maar zeer handige functie van App Service-omgevingen is dat de naam van de dezelfde app kan worden gebruikt voor meerdere App Service-omgevingen.  Omdat elke App Service-omgeving een unieke domeinachtervoegsel heeft, kunnen ontwikkelaars kiezen om exact dezelfde naam van de app opnieuw te gebruiken in elke omgeving.  Bijvoorbeeld een ontwikkelaar van apps met de naam als volgt kan hebben: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, enzovoort.  Voor de voorbeeld-app echter elk exemplaar van de app heeft ook een unieke naam.  De namen van de app-exemplaar gebruikt *webfrontend1*, *webfrontend2*, en *webfrontend3*.

## <a name="setting-up-the-traffic-manager-profile"></a>Instellen van Traffic Manager-profiel
Als meerdere exemplaren van een app worden geïmplementeerd op meerdere App Service-omgevingen, kunnen de afzonderlijke app-exemplaren met Traffic Manager worden geregistreerd.  Voor de voorbeeld-app een Traffic Manager-profiel is nodig voor *schaalbare-as-omgeving-demo.trafficmanager.net* die klanten kunnen routeren naar een van de volgende exemplaren van de door u geïmplementeerde app:

* **webfrontend1.fe1ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de eerste App Service-omgeving.
* **webfrontend2.fe2ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de tweede App Service-omgeving.
* **webfrontend3.fe3ase.p.azurewebsites.NET:** een exemplaar van de voorbeeld-app geïmplementeerd op de derde App Service-omgeving.

De eenvoudigste manier om het registreren van meerdere Azure App Service-eindpunten, alle actieve in de **dezelfde** Azure-regio, is met de Powershell [ondersteuning voor Azure Resource Manager Traffic Manager] [ ARMTrafficManager].  

De eerste stap is het maken van een Azure Traffic Manager-profiel.  De code hieronder laat zien hoe het profiel is gemaakt voor de voorbeeld-app:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

U ziet hoe de *RelativeDnsName* parameter is ingesteld op *schaalbare-as-omgeving-demo*.  Dit is hoe de domeinnaam *schaalbare-as-omgeving-demo.trafficmanager.net* wordt gemaakt en die zijn gekoppeld aan een Traffic Manager-profiel.

De *TrafficRoutingMethod* parameter bepaalt de load balancer-beleid voor Traffic Manager wordt gebruikt om te bepalen hoe de beschikbare eindpunten worden verdeeld aan belasting van klanten.  In dit voorbeeld de *gewogen* methode hebt gekozen.  Hierdoor wordt de aanvragen van klanten wordt verdeeld over alle van de geregistreerde toepassingseindpunten op basis van het relatieve gewicht dat is gekoppeld aan elk eindpunt. 

Elke app-instantie wordt het profiel is gemaakt, als een systeemeigen Azure-eindpunt toegevoegd aan het profiel.  De onderstaande code een verwijzing naar elke front-end web-app kan worden opgehaald en worden vervolgens elke app toegevoegd als een Traffic Manager-eindpunt van de *TargetResourceId* parameter.

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

U ziet hoe er wordt één aanroep naar *toevoegen AzureTrafficManagerEndpointConfig* voor elke afzonderlijke app-instantie.  De *TargetResourceId* parameter in elke Powershell-opdracht verwijst naar een van de drie exemplaren van de door u geïmplementeerde app.  Traffic Manager-profiel wordt geladen verdeeld over alle drie eindpunten die zijn geregistreerd in het profiel.

Alle van de drie eindpunten gebruik dezelfde waarde (10) voor de *gewicht* parameter.  Dit resulteert in Traffic Manager het spreiden van klantaanvragen voor alle drie app-instanties relatief gelijk. 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Aangepast domein in het Traffic Manager-domein van de App aan te wijzen
De laatste stap nodig is zodat het aangepaste domein van de app op het Traffic Manager-domein.  Voor de voorbeeld-app betekent dit dat wijzen *www.scalableasedemo.com* op *schaalbare-as-omgeving-demo.trafficmanager.net*.  Deze stap moet worden voltooid met de domeinregistrar waarmee het aangepaste domein wordt beheerd.  

Met behulp van beheerhulpprogramma's van uw registrar domein, registreert een CNAME moet worden gemaakt die het aangepaste domein in het Traffic Manager-domein verwijst.  De volgende afbeelding toont een voorbeeld van hoe deze configuratie CNAME uitziet:

![CNAME voor aangepast domein][CNAMEforCustomDomain] 

Hoewel in dit onderwerp niet wordt gedekt, houd er rekening mee dat elke afzonderlijke app-instantie moet het aangepaste domein dat is geregistreerd bij deze ook zijn.  Anders als een aanvraag het om een app-exemplaar is en de toepassing heeft geen het aangepaste domein dat is geregistreerd bij de app, wordt de aanvraag mislukt.  

In dit voorbeeld het aangepaste domein is *www.scalableasedemo.com*, en elk exemplaar heeft het aangepaste domein is gekoppeld.

![Aangepast domain][CustomDomain] 

Zie het volgende artikel voor een samenvatting van het registreren van een aangepast domein met Azure App Service-apps, op [registreren van aangepaste domeinen][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Proberen van de gedistribueerde topologie
Het eindresultaat van de Traffic Manager- en DNS-configuratie is dat aanvragen voor *www.scalableasedemo.com* worden overgebracht via de volgende volgorde:

1. Een browser of apparaat wordt een DNS-zoekopdracht maken voor *www.scalableasedemo.com*
2. De CNAME-vermelding op de domeinregistrar zorgt ervoor dat de DNS-zoekactie worden omgeleid naar Azure Traffic Manager.
3. Een DNS-zoekopdracht wordt gemaakt voor *schaalbare-as-omgeving-demo.trafficmanager.net* op basis van een van de Azure Traffic Manager-DNS-servers.
4. Op basis van de load balancer-beleid (de *TrafficRoutingMethod* parameter eerder hebt gebruikt bij het maken van Traffic Manager-profiel), Traffic Manager Selecteer een van de geconfigureerde eindpunten en de FQDN-naam van dat eindpunt om te retourneren de browser of apparaat.
5. Omdat de FQDN-naam van het eindpunt de Url van een app-exemplaar waarop een App Service-omgeving is, vraagt de browser of het apparaat een Microsoft Azure DNS-server de FQDN-naam omzetten in een IP-adres. 
6. De browser of apparaat stuurt de aanvraag HTTP/S naar het IP-adres.  
7. De aanvraag wordt ontvangen op een van de app-exemplaren die worden uitgevoerd op een van de App Service-omgevingen.

De console van de volgende afbeelding ziet u een DNS-zoekactie voor de voorbeeld-app aangepaste domein is omgezet naar een app-exemplaar die wordt uitgevoerd op een van de drie voorbeeld-App Service-omgevingen (in dit geval de seconde van de drie App Service-omgevingen):

![DNS-zoekactie][DNSLookup] 

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Documentatie over de Powershell [ondersteuning voor Azure Resource Manager Traffic Manager][ARMTrafficManager].  

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
