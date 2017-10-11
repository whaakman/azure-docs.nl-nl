---
title: "Voorbeeld van de DMZ – bouwen een DMZ om netwerken met een Firewall, UDR en NSG te beveiligen | Microsoft Docs"
description: Maken van een DMZ met een Firewall, de gebruiker gedefinieerde Routering en Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: fdb3c5cbd3acee90386352c6f180a71aa81f54fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: een DMZ netwerken met een Firewall, UDR en NSG beschermen bouwen
[Ga terug naar de grens van Best Practices beveiligingspagina][HOME]

In dit voorbeeld wordt een DMZ gemaakt met een firewall, vier windows-servers, gebruiker gedefinieerde routering, doorsturen via IP en Netwerkbeveiligingsgroepen. Deze begeleidt ook bij elk van de relevante opdrachten voor het bieden van een beter begrip van elke stap. Er is ook een sectie verkeer Scenario voor een gedetailleerd stapsgewijs hoe verkeer wordt uitgevoerd door de lagen verdediging in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige code en de instructies voor het bouwen van deze omgeving om te testen en Experimenteer met verschillende scenario's. 

![Bidirectionele DMZ met NVA, NSG en UDR][1]

## <a name="environment-setup"></a>Instellen van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

* Drie cloudservices: 'SecSvc001', 'FrontEnd001' en 'BackEnd001'
* Een virtueel netwerk 'CorpNetwork' met drie subnetten: 'SecNet', 'FrontEnd' en back-end'
* Een virtueel netwerkapparaat, in dit voorbeeld van een firewall, die is verbonden met het subnet SecNet
* Een Windows-Server waarmee een toepassing webserver ('IIS01')
* Twee windows-servers die toepassing terug vertegenwoordigen end servers ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')

In de sectie Verwijzingen hieronder is er een PowerShell-script dat de meeste van de hierboven beschreven omgeving bouwt. Opbouwen van de virtuele machines en virtuele netwerken, hoewel worden uitgevoerd door het voorbeeldscript worden niet beschreven in dit document uitvoeriger.

De omgeving maken:

1. Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario)
2. Bijwerken van de gebruikersvariabelen in het script moet overeenkomen met de omgeving die het script wordt uitgevoerd tegen (abonnementen, servicenamen, enzovoort)
3. Voer het script in PowerShell

**Opmerking**: de regio die wordt aangegeven in het PowerShell-script moet overeenkomen met de regio die wordt aangegeven in het netwerk van het XML-configuratiebestand.

Nadat het script wordt uitgevoerd kan de volgende stappen voor na script kunnen worden genomen:

1. Stel de firewallregels, deze procedure wordt besproken in de sectie met de titel: Beschrijving van de Firewall-regel.
2. Optioneel zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en de appserver met een eenvoudige webtoepassing waarmee testen met deze configuratie DMZ.

Nadat het script uitgevoerd in de firewall regels moeten worden voltooid, dit wordt beschreven in het gedeelte: Firewall-regels.

## <a name="user-defined-routing-udr"></a>Door de gebruiker gedefinieerde Routering
Standaard worden de volgende systeemroutes gedefinieerd als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd de prefix(en) opgegeven adres van de VNet voor die specifieke netwerk (ie het wordt gewijzigd van VNet naar VNet, afhankelijk van hoe elke specifieke VNet is gedefinieerd). De resterende systeemroutes zijn statisch en standaard als hierboven.

Als voor de prioriteit, routes worden verwerkt via de langste voorvoegsel overeen (LPM)-methode, dus het meest specifiek route in de tabel toepassing zou zijn op een gegeven doeladres.

Daarom zou verkeer (bijvoorbeeld naar de server DNS01 10.0.2.4) voor het lokale netwerk (10.0.0.0/16) worden gerouteerd tussen het VNet naar de bestemming als gevolg van de route 10.0.0.0/16. Met andere woorden, voor 10.0.2.4, de 10.0.0.0/16 route is het meest specifiek route, hoewel de 10.0.0.0/8 en 0.0.0.0/0 ook kunnen toegepast, maar omdat ze minder specifieke ze niet op dit verkeer. Het verkeer naar 10.0.2.4 zou dus een volgende hop van de lokale VNet en gewoon routeren naar de bestemming.

Als verkeer is bedoeld voor 10.1.1.1 bijvoorbeeld, de route 10.0.0.0/16 toepassen wouldn't, maar de 10.0.0.0/8 het meest specifiek zijn zou en het verkeer zou dit ('zwart gaan') verwijderd omdat de volgende hop Null is. 

Als de bestemming niet op een van de voorvoegsels Null of de voorvoegsels VNETLocal toepassen, wordt het minst specifiek volgt routeren, 0.0.0.0/0 en worden doorgestuurd naar het Internet als de volgende hop en dus buiten de Azure internet rand.

Als er twee identieke voorvoegsels in de routetabel aanwezig zijn, wordt het volgende is de volgorde van voorkeur op basis van de routes 'bron'-kenmerk:

1. 'VirtualAppliance' = van een gebruiker gedefinieerde Route handmatig worden toegevoegd aan de tabel
2. 'VPNGateway' een dynamische Route BGP (gebruikt in combinatie met hybride netwerken), = toegevoegd door een dynamische netwerkprotocol, deze routes kunnen op den duur veranderen als het protocol voor dynamische wordt automatisch in het netwerk als peer is ingesteld aangepast
3. 'Standaard' = de Systeemroutes, het lokale VNet en de statische vermeldingen, zoals wordt weergegeven in de bovenstaande routetabel.

> [!NOTE]
> U kunt nu de gebruiker gedefinieerde routering (UDR) gebruiken met ExpressRoute en cross-premises VPN-Gateways om af te dwingen binnenkomend en uitgaand verkeer worden doorgestuurd naar een virtueel netwerkapparaat (NVA).
> 
> 

#### <a name="creating-the-local-routes"></a>De lokale routes maken
In dit voorbeeld worden twee routeringstabellen nodig, één voor de front-end- en back-end-subnetten. Elke tabel is geladen met statische routes die geschikt is voor het opgegeven subnet. In dit voorbeeld heeft elke tabel drie routes:

1. Lokaal subnetverkeer met geen volgende Hop gedefinieerd, zodat lokaal subnetverkeer voor het overslaan van de firewall
2. Virtueel netwerkverkeer met een volgende Hop gedefinieerd als een firewall, overschrijft dit de standaardregel waarmee lokale VNet-verkeer routeren rechtstreeks
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall

Als de routeringstabellen zijn gemaakt zijn ze gekoppeld aan hun subnetten. Voor het subnet Frontend er routeringstabel eenmaal gemaakt en gekoppeld aan het subnet als volgt uit:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Bijvoorbeeld, de volgende opdrachten worden gebruikt voor het bouwen van de routetabel, een gebruiker gedefinieerde route toevoegen en vervolgens de routetabel binden aan een subnet (Opmerking; de items onder die begint met een dollarteken (bijvoorbeeld: $BESubnet) zijn van de gebruiker gedefinieerde variabelen uit het script in de verwijzing naar sectie van dit document):

1. De basistabel routering moet eerst worden gemaakt. In dit fragment toont het maken van de tabel voor de back-end-subnet. In het script wordt ook een bijbehorende tabel gemaakt voor het subnet Frontend.
   
     Nieuwe AzureRouteTable-naam $BERouteTableName '
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Zodra de routetabel is gemaakt, kunnen specifieke gebruiker gedefinieerde routes worden toegevoegd. In deze snipped wordt (0.0.0.0/0) van alle verkeer gerouteerd via het virtuele apparaat (een variabele, $VMIP [0] wordt gebruikt om door te geven in het IP-adres toegewezen wanneer het virtuele apparaat eerder in het script is gemaakt). In het script wordt ook een bijbehorende regel gemaakt in de Frontend-tabel.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. De bovenstaande routevermelding wordt overschreven door de '0.0.0.0/0' standaardroute, maar de standaardregel 10.0.0.0/16 nog steeds bestaande waarmee verkeer binnen het VNet rechtstreeks naar de bestemming en niet naar het virtuele apparaat netwerk routeren. Juiste dit gedrag van de volgende regel moet worden toegevoegd.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Op dit moment is er een keuze kunnen worden uitgevoerd. Al het verkeer wordt met de bovenstaande twee routes route aan de firewall voor de beoordeling, zelfs verkeer binnen één subnet. Dit kan nodig zijn, maar waarmee verkeer binnen een subnet voor het routeren lokaal zonder betrokkenheid van de firewall van een derde zeer specifieke regel kan worden toegevoegd. Deze route statussen van een adres voor het lokale subnet kunt just destine routeren er rechtstreeks (NextHopType = VNETLocal).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Ten slotte wordt met de routeringstabel gemaakt en gevuld met een gebruiker gedefinieerde routes, moet de tabel nu worden gebonden aan een subnet. In het script is ook de front-end-routetabel gebonden aan de front-end-subnet. Hier wordt het script binding voor de back-end-subnet.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName '
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Doorsturen via IP
Een onderdeel companion UDR, is het doorsturen via IP. Dit is een instelling op een virtueel apparaat dat is toegestaan voor het ontvangen verkeer niet specifiek zijn gericht op het toestel en vervolgens doorsturen dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld als verkeer van AppVM01 een aanvraag bij de server DNS01 doet wilt UDR routeren dit aan de firewall. Met het doorsturen via IP is ingeschakeld, wordt het verkeer voor de bestemming DNS01 (10.0.2.4) worden geaccepteerd door het toestel (10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder doorsturen via IP ingeschakeld op de Firewall, wordt verkeer niet geaccepteerd door het toestel ondanks dat de routetabel de firewall als de volgende hop is. 

> [!IMPORTANT]
> Het is essentieel om te onthouden doorsturen via IP inschakelen in combinatie met de gebruiker gedefinieerde routering.
> 
> 

Instellen van het doorsturen via IP is één opdracht en kan worden uitgevoerd tijdens de aanmaak van virtuele machine. Voor de stroom van dit voorbeeld is het codefragment is aan het einde van het script, en met de opdrachten UDR gegroepeerd:

1. Roept het VM-exemplaar dat uw virtuele apparaat, de firewall in dit geval is, en doorsturen via IP inschakelen (Opmerking; een item in rood die begint met een dollarteken (bijvoorbeeld: $VMName[0]) is een door de gebruiker gedefinieerde variabele van het script in de sectie Verwijzingen van dit document. De op nul vierkante haken, [0] Hiermee geeft u de eerste virtuele machine in de matrix van virtuele machines voor het voorbeeldscript werken zonder dat aanpassingen nodig, de eerste virtuele machine (VM 0) moet de firewall):
   
     Get-AzureVM-naam $VMName [0] - ServiceName $ServiceName [0] | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een groep NSG gebouwd en vervolgens geladen met een enkele regel. Deze groep wordt vervolgens uitsluitend gekoppeld aan de front-end- en back-end-subnetten (niet de SecNet). Declaratief wordt samengesteld met de volgende regel:

1. Verkeer (alle poorten) van het Internet naar het hele VNet (alle subnetten) is geweigerd

Hoewel het nsg's in dit voorbeeld worden gebruikt, worden de belangrijkste doel is als een laag secundaire beveiliging tegen handmatige onjuiste configuratie. We willen alle binnenkomende verkeer blokkeren met vanaf het internet aan de front-end of back-end-subnetten, verkeer alleen door het SecNet-subnet aan de firewall moeten stromen (en vervolgens passende bij de Frontend- of back-end subnetten). Plus, aan de regels UDR aanwezig is, verkeer om het in de front-end of back-end-subnetten zou worden omgeleid uit aan de firewall (dankzij UDR). De firewall zou dit zien als een asymmetrische stroom en het uitgaande verkeer wilt verwijderen. Er zijn dus drie lagen van beveiliging voor het beveiligen van de front-end- en back-end-subnetten; 1) er zijn geen open eindpunten op de FrontEnd001 en BackEnd001 cloudservices, 2) nsg's voor het weigeren van verkeer van het Internet, 3) de firewall weggehaald asymmetrische verkeer.

Een interessant punt met betrekking tot de Netwerkbeveiligingsgroep in dit voorbeeld is dat deze slechts één regel, die bevat hieronder worden weergegeven die voor het weigeren van internet-verkeer op het volledige virtuele netwerk, waaronder het subnet van de beveiliging is. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Echter, aangezien het NSG wordt alleen gekoppeld aan de front-end- en back-end-subnetten, de regel is niet verwerkt op het verkeer inkomend verkeer naar het subnet van de beveiliging. Als gevolg hiervan, zelfs als de NSG-regel geen internetverkeer aan een adres op het VNet, staat omdat het NSG nooit is gebonden aan het subnet van de beveiliging, worden verkeer overgebracht naar het subnet van de beveiliging.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewallregels
Op de firewall moet doorsturen regels worden gemaakt. Omdat de firewall geblokkeerd of doorsturen van alle binnenkomende, uitgaande en intra-VNet-verkeer is worden veel firewallregels vereist. Alle binnenkomend verkeer bereikt beveiligingsservice openbaar IP-adres (op verschillende poorten), ook moeten worden verwerkt door de firewall. Er is een best practice diagram van de logische stroom voor het instellen van de subnetten te en firewall-regels om te voorkomen dat later bijwerken. De volgende afbeelding is een logische weergave van de firewallregels voor dit voorbeeld:

![Logische weergave van de Firewall-regels][2]

> [!NOTE]
> Op basis van het virtuele netwerk-apparaat gebruikt, variëren de beheerpoorten. In dit voorbeeld wordt verwezen naar een Barracuda NextGen Firewall gebruikt die poort 22, 801 en 807. Raadpleeg de documentatie van de leverancier toestel om te zoeken welke poorten gebruikt voor het beheer van het apparaat wordt gebruikt.
> 
> 

### <a name="logical-rule-description"></a>Beschrijving van de logische regel
In het bovenstaande logisch diagram wordt het subnet van de beveiliging niet weergegeven omdat de firewall de enige resource in dat subnet is dit diagram wordt weergegeven, de firewall-regels en hoe ze logisch toestaan of verkeersstromen en niet de werkelijke gerouteerde pad weigeren. Ook de externe poorten voor de RDP-verkeer hoger zijn geselecteerd varieerde poorten (8014 – 8026) en enigszins uitgelijnd met de laatste twee octetten van het lokale IP-adres voor de leesbaarheid eenvoudiger zijn geselecteerd (bijvoorbeeld 10.0.1.4-adres van de lokale server is gekoppeld aan de externe poort 8014), maar een hogere poorten voor niet-conflicterende kunnen worden gebruikt.

Bijvoorbeeld, moeten we 7 typen regels, deze regeltypen worden als volgt beschreven:

* Externe regels (voor inkomend verkeer):
  1. Firewallregel voor beheer: Met deze regel omleidings-App kunt verkeer door te geven aan de management-poorten van het virtuele netwerk-apparaat.
  2. RDP-regels (voor elke WindowsServer): deze vier regels (één voor elke server) wordt beheer van de afzonderlijke servers via RDP toestaan. Dit kan ook worden gebundeld in één regel, afhankelijk van de mogelijkheden van het virtuele netwerk-apparaat wordt gebruikt.
  3. Regels voor het verkeer van toepassing: Er zijn twee toepassing verkeersregels, de eerste voor het verkeer van de web-front-end en de tweede voor het back-end-verkeer (bijvoorbeeld webserver gegevenslaag). De configuratie van deze regels, zal afhankelijk zijn van de netwerkarchitectuur (waar uw servers worden geplaatst) en er verkeer stromen (welke richting de verkeersstromen en welke poorten worden gebruikt).
     * De eerste regel wordt het verkeer van de werkelijke toepassing bereiken van de toepassingsserver toestaan. Terwijl de andere regels toestaan voor de beveiliging, beheer, enz., zijn toepassing regels wat externe gebruikers of services voor toegang tot de toepassing(en) toestaan. In dit voorbeeld is er één webserver op poort 80, dus een firewallregel voor één toepassing binnenkomend verkeer worden omgeleid naar het externe IP-adres, het web servers interne IP-adres. De sessie omgeleide verkeer zou worden NAT moest de interne server.
     * De tweede regel van toepassing verkeer is het back-end-regel voor het toestaan dat de webserver Neem contact op met de server AppVM01 (maar niet AppVM02) via een willekeurige poort.
* Interne regels (voor intra-VNet-verkeer)
  1. Uitgaand naar Internet regel: met deze regel wordt verkeer van een netwerk te geven aan de geselecteerde netwerken toestaan. Deze regel is meestal een standaardregel al op de firewall, maar een uitgeschakelde status. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  2. DNS-regel: Met deze regel kunnen alleen DNS (poort 53)-verkeer door te geven aan de DNS-server. Voor deze omgeving die meeste verkeer van de Frontend naar de back-end wordt geblokkeerd, kan deze regel specifiek DNS van een lokale subnet.
  3. Subnet naar Subnet regel: met deze regel wordt als u een server op het subnet van de back-end verbinding maken met een willekeurige server in de front-end-subnet (maar niet andersom).
* Foutveilige regel (voor verkeer dat niet voldoet aan een van de bovenstaande):
  1. Alle Verkeersregel voor weigeren: Dit moet altijd de laatste regel (in termen van prioriteit), en als zodanig als een van de verkeersstromen niet overeen met een van de vorige regels die wordt door deze regel wordt verwijderd. Dit is een standaardregel en meestal geactiveerd, worden er zijn geen wijzigingen in het algemeen nodig.

> [!TIP]
> Een willekeurige poort is toegestaan voor eenvoudige van dit voorbeeld is in een echte scenario de meest specifieke poort op de tweede verkeersregel van de toepassing en adresbereiken moeten worden gebruikt om de kwetsbaarheid van deze regel te beperken.
> 
> 

<br />

> [!IMPORTANT]
> Zodra alle bovenstaande regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om ervoor te zorgen verkeer wordt toegestaan of geweigerd desgewenst. In dit voorbeeld zijn de regels in volgorde van prioriteit. Het is gemakkelijk buiten de firewall vanwege niet-geordende regels worden vergrendeld. Ten minste Zorg ervoor dat het beheer van de firewall zelf is altijd de absolute hoogste prioriteit regel.
> 
> 

### <a name="rule-prerequisites"></a>Regel voor vereisten
Een vereiste voor de virtuele Machine met de firewall zijn openbare eindpunten. De openbare eindpunten moeten zijn geopend voor de firewall verkeer verwerken. Er zijn drie typen verkeer in dit voorbeeld; De RDP-verkeer 1) beheer van verkeer voor beheer op de firewall en firewallregels, 2) om de windows-servers en 3) toepassing verkeer te regelen. Dit zijn de drie kolommen van de verkeerstypen in het bovenste helft van de logische weergave van de firewallregels hierboven.

> [!IMPORTANT]
> Een sleutel takeway hier is om te weten dat **alle** verkeer wordt geleverd door de firewall. Dus met extern bureaublad met de server IIS01 wordt zelfs als het in de Front-End-Cloudservice en op de front-end-subnet voor toegang tot deze server RDP moet aan de firewall op poort 8014, en vervolgens de firewall voor het routeren van de RDP-aanvraag intern met de RDP-Por IIS01 toestaan t. Knop voor de Azure portal 'Verbinden' werkt niet omdat er geen directe RDP-pad naar IIS01 (zo ver mogelijk de portal kunt zien). Dit betekent dat alle verbindingen van internet naar de Security-Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx (verwijzing in het bovenstaande diagram voor de toewijzing van de externe poort en intern IP-adres en poort).
> 
> 

Een eindpunt kan worden geopend op het moment van de virtuele machine maken of build boekt als in het voorbeeldscript en de hieronder weergegeven in dit codefragment (Opmerking; een item die begint met een dollarteken (bijvoorbeeld: $VMName[$i]) is een door de gebruiker gedefinieerde variabele van het script in de cties verwijzing n van dit document. '$I' vierkante haken, [$i] Hiermee geeft u het nummer van de matrix van een specifieke virtuele machine in een matrix van virtuele machines):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Hoewel het niet duidelijk hier weergegeven vanwege het gebruik van variabelen, maar eindpunten zijn **alleen** geopend op de Cloud Security Service. Dit is om ervoor te zorgen dat alle binnenkomend verkeer wordt verwerkt (gerouteerd, NAT waren, verloren) door de firewall.

Een management-client moet worden geïnstalleerd op een computer voor het beheren van de firewall en maken van de configuraties die nodig zijn. Zie de documentatie van uw firewall (of andere NVA)-leverancier voor het beheren van het apparaat. De rest van deze sectie en de volgende sectie, Firewall-regels maken, wordt de configuratie van de firewall zelf, via de leveranciers management-client (d.w.z. niet in de Azure-portal of PowerShell) beschreven.

Instructies voor het downloaden van de client en verbinding maken met de in dit voorbeeld gebruikt Barracuda vindt u hier: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Zodra u aangemeld bent op de firewall, maar voordat u firewallregels maken, zijn er twee vereiste objectklassen die kunnen maken van de regels gemakkelijker; Netwerk- en Service-objecten.

In dit voorbeeld moet drie objecten van het benoemde netwerk gedefinieerde (één voor de front-end-subnet en het subnet ook een netwerkobject voor het IP-adres van de DNS-server van de back-end). Maken van een benoemde netwerk. starten vanuit het dashboard van de client Barracuda NG Admin, gaat u naar het tabblad configuratie, in de operationele configuratiesectie Ruleset, vervolgens klikt u op "Netwerken" in het menu Firewallobjecten, klik op Nieuw in het menu Bewerken netwerken. Het object network kan nu worden gemaakt, door de naam en het voorvoegsel toe te voegen:

![Een Object FrontEnd-netwerk maken][3]

Hiermee maakt u een netwerk met de naam voor de front-end-subnet, een vergelijkbaar object moet worden gemaakt voor de back-end-subnet. De subnetten kunnen nu gemakkelijker verwezen met de naam in de firewall-regels.

Voor het Object van de DNS-Server:

![Een DNS-Server-Object maken][4]

Deze één verwijzing naar een IP-adres wordt gebruikt in een DNS-regel verderop in dit document.

De tweede vereiste objecten zijn Services objecten. Deze vertegenwoordigt het RDP-verbindingspoorten voor elke server. Aangezien het bestaande object van de RDP-service is gekoppeld aan de standaardpoort voor RDP, kunnen 3389, nieuwe Services worden gemaakt om verkeer van de externe (8014 8026)-poorten toestaan. De nieuwe poorten kunnen ook worden toegevoegd aan de bestaande RDP-service, maar voor een eenvoudige demonstratie een afzonderlijke regel voor elke server kan worden gemaakt. Maken van een nieuwe RDP-regel voor een server. vanaf het dashboard van de client Barracuda NG Admin, gaat u naar het tabblad configuratie in de operationele configuratiesectie op Ruleset, en vervolgens klikt u op 'Services' in het menu Firewallobjecten, schuif omlaag in de lijst met services en selecteer de service 'RDP'. Met de rechtermuisknop op en selecteert u kopiëren, en vervolgens met de rechtermuisknop op en selecteer plakken. Er is nu een RDP-Copy1-Object dat kan worden bewerkt. Met de rechtermuisknop op de RDP-Copy1 en selecteer bewerken, het serviceobject bewerken venster maximaal zoals hier wordt weergegeven:

![Kopie van de standaardregel voor RDP][5]

De waarden kunnen worden bewerkt ter vertegenwoordiging van de RDP-service voor een specifieke server. Voor AppVM01 de bovenstaande standaardregel voor RDP moet worden gewijzigd naar aanleiding van een nieuwe Service-naam, beschrijving en externe RDP-poort die wordt gebruikt in het diagram afbeelding 8 (Opmerking: de poorten van de RDP-standaardwaarde van 3389 worden gewijzigd in de externe poort die wordt gebruikt voor deze specifieke server in het geval van AppVM01 is de externe poort 8025) de gewijzigde service worden hieronder weergegeven:

![AppVM01 regel][6]

Dit proces moet worden herhaald voor het maken van RDP-Services voor de overige servers; AppVM02, DNS01 en IIS01. Het maken van deze Services maakt het maken van de regel eenvoudiger en duidelijker in de volgende sectie.

> [!NOTE]
> Een RDP-service voor de Firewall is niet nodig om twee redenen; 1) de eerste de firewall VM een installatiekopie op basis van Linux is SSH op poort 22 zou worden gebruikt voor het beheer van de virtuele machine in plaats van RDP en 2)-poort 22, en twee andere poorten zijn toegestaan in de eerste management regel die hieronder worden beschreven om toe te staan voor de connectiviteit van het beheer.
> 
> 

### <a name="firewall-rules-creation"></a>Firewall-regels maken
Er zijn drie soorten firewallregels die in dit voorbeeld gebruikt, alle hebben verschillende pictogrammen:

De regel voor het omleiden van toepassing: ![toepassing omleidings-pictogram][7]

De doel-NAT-regel: ![bestemming NAT-pictogram][8]

De regel op te geven: ![pictogram doorgeven][9]

Meer informatie over deze regels kan worden gevonden op de website Barracuda.

De volgende regels maken (of Controleer of de bestaande standaardregels) starten vanuit het dashboard van de client Barracuda NG Admin gaat u naar het tabblad configuratie, in de configuratie van de operationele sectie Ruleset op. Een raster aangeroepen, 'Main regels' wordt de bestaande regels voor de actieve als gedeactiveerde op deze firewall weergeven. In de rechterbovenhoek van dit raster is een klein, groene '+' knop, klik hierop om een nieuwe regel te maken (Opmerking: uw firewall mogelijk 'vergrendeld' om wijzigingen, als u ziet een knop 'Vergrendelen' gemarkeerd en u zich niet maken of bewerken van regels, klik op deze knop om de regelset 'ontgrendelen' en  bewerken toestaan). Als u bewerken van een bestaande regel wilt, selecteert u deze regel, met de rechtermuisknop op en selecteer regel bewerken.

Als uw regels zijn gemaakt en/of gewijzigd, moet aan de firewall gepusht en wordt geactiveerd, als u dit niet doet de regel wijzigingen pas van kracht. Het proces push en activering wordt hieronder de beschrijvingen van de regel details beschreven.

De details van elke regel vereist voor het voltooien van dit voorbeeld worden als volgt beschreven:

* **Firewall-regel Management**: deze App omleiden regel staat toe dat verkeer door te geven aan de beheerpoorten van de virtuele netwerkapparaat, in dit voorbeeld van een Barracuda NextGen Firewall. De management-poorten zijn 801, 807 en eventueel 22. De interne en externe poorten zijn hetzelfde (d.w.z. geen poortvertaling). Deze regel, SETUP-MGMT-toegang, is een standaardregel en (in Barracuda NextGen Firewall versie 6.1) standaard ingeschakeld.
  
    ![Firewallregel Management][10]

> [!TIP]
> De bron-adresruimte in deze regel is aanwezig, als het beheer van IP-adresbereiken gekend zijn, waardoor dit bereik ook sneller de kwetsbaarheid voor aanvallen op de management-poorten.
> 
> 

* **Regels voor RDP**: deze bestemming NAT-regels kunnen beheer van de afzonderlijke servers via RDP.
  Er zijn vier kritieke velden die nodig zijn voor het maken van deze regel:
  
  1. Bron: zodat RDP 'Een' vanaf elke locatie en de verwijzing wordt gebruikt in het veld bron.
  2. Service – het juiste serviceobject eerder hebt gemaakt, in dit geval 'AppVM01 RDP' gebruiken, externe poorten doorsturen naar de lokale servers IP-adres en poort 3386 (de standaard RDP-poort). Deze specifieke regel is voor RDP-toegang tot AppVM01.
  3. Doel – moet het *lokale poort op de firewall*, 'DCHP 1 lokale IP-' of eth0 als statische IP-adressen. Het rangtelwoord (eth0, eth1, enzovoort) is mogelijk anders als uw netwerkapparaat meerdere lokale interfaces heeft. Dit is de poort die de firewall van verstuurt (mogelijk dezelfde zijn als de ontvangende poort), de werkelijke gerouteerde bestemming is in het veld voor de lijst met doelservers.
  4. Omleiding – dit gedeelte wordt uitgelegd het virtuele apparaat waar u uiteindelijk dit verkeer worden omgeleid. De eenvoudigste omleiding is het IP- en de poort (optioneel) plaatsen in de lijst met doelservers-veld. Als er geen poort wordt gebruikt de doelpoort op de binnenkomende aanvraag worden (ie geen vertaling) gebruikt als een poort is de poort aangewezen worden ook NAT zou samen met het IP-adres adresseren.
     
     ![RDP-firewallregel][11]
     
     Een totaal van vier RDP regels moet worden gemaakt: 
     
     | Regelnaam | Server | Service | Lijst met doelservers |
     | --- | --- | --- | --- |
     | RDP-naar-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP-naar-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP-naar-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP-naar-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Beperking u het bereik van de bron- en velden beperkt u de kwetsbaarheid voor aanvallen. De meest beperkte mogelijkheden die zorgen de functionaliteit dat ervoor moet worden gebruikt.
> 
> 

* **Toepassing verkeersregels**: Er zijn twee toepassing verkeersregels, de eerste voor het verkeer van de web-front-end en de tweede voor het back-end-verkeer (bijvoorbeeld webserver gegevenslaag). Deze regels, zal afhankelijk zijn van de netwerkarchitectuur (waar uw servers worden geplaatst) en er verkeer stromen (welke richting de verkeersstromen en welke poorten worden gebruikt).
  
    Eerst besproken, is de front-end-regel voor internetverkeer:
  
    ![Web-firewallregel][12]
  
    Deze bestemming NAT-regel kan het verkeer van de toepassing om de toepassingsserver te bereiken. Terwijl de andere regels toestaan voor de beveiliging, beheer, enz., zijn toepassing regels wat externe gebruikers of services voor toegang tot de toepassing(en) toestaan. In dit voorbeeld is er één webserver op poort 80, dus de firewallregel die één toepassing binnenkomend verkeer worden omgeleid naar het externe IP-adres, het web servers interne IP-adres.
  
    **Opmerking**: dat er is geen poort toegewezen in het veld doellijst, dus de binnenkomende poort 80 (of 443 voor de geselecteerde Service) worden gebruikt voor het omleiden van de webserver. Als de webserver luistert op een andere poort, bijvoorbeeld poort 8080, wordt het veld doellijst kan worden bijgewerkt naar 10.0.1.4:8080 om toe te staan voor de poort-omleiding.
  
    De volgende regel van toepassing verkeer is het back-end-regel voor het toestaan dat de webserver Neem contact op met de server AppVM01 (maar niet AppVM02) via elke service:
  
    ![AppVM01 firewallregel][13]
  
    Met deze regel op te geven kunnen elke IIS-server op het subnet Frontend bereiken van de AppVM01 (IP-adres 10.0.2.5) met behulp van elk Protocol voor toegang tot gegevens die nodig is voor de webtoepassing op een willekeurige poort.
  
    In deze schermafdruk een '\<expliciete-dest\>' in het doelveld wordt gebruikt om aan te duiden 10.0.2.5 als doel. Dit kan zijn expliciete zoals wordt weergegeven of een met de naam netwerkobject (net als bij de vereisten voor de DNS-server). Dit is aan de beheerder van de firewall wat betreft welke methode wordt gebruikt. Als u wilt toevoegen 10.0.2.5 als een Explict Desitnation, dubbelklik op de eerste lege rij onder \<expliciete-dest\> en voer het adres in het venster dat wordt weergegeven.
  
    Met deze regel geeft is geen NAT nodig omdat dit interne verkeer, is zodat de verbindingsmethode kan worden ingesteld op 'Nee snat omzetten'.
  
    **Opmerking**: het Bronnetwerk in deze regel wordt een resource in het subnet FrontEnd als er slechts één of een bekende specifiek aantal webservers, een netwerkobject resource kan worden gemaakt als u wilt meer specifiek zijn voor de exacte IP-adressen in plaats van de gehele Frontend-subnet.

> [!TIP]
> Deze regel maakt gebruik van de service 'Any' om de voorbeeldtoepassing vereenvoudigen instellen en gebruiken, ook hierdoor ICMPv4 (ping) in een enkele regel. Dit is echter niet aanbevolen. De poorten en protocollen ('Services') moeten worden teruggebracht tot het minimum mogelijk waarmee de toepassing opnieuw aan de kwetsbaarheid voor aanvallen via deze grens te reduceren.
> 
> 

<br />

> [!TIP]
> Hoewel met deze regel een verwijzing naar een expliciete-dest wordt gebruikt bevat, moet een consistente manier worden gebruikt in de firewallconfiguratie. Het verdient aanbeveling dat het benoemde netwerk-Object worden gebruikt in de gehele voor gemakkelijker leesbaarheid en ondersteuningsmogelijkheden. De expliciete-dest wordt hier alleen gebruikt om een alternatieve verwijzing naar de methode show en wordt over het algemeen niet aanbevolen (met name voor complexe configuraties).
> 
> 

* **Uitgaand naar Internet regel**: doorgeven voor deze regel wordt verkeer van een Bronnetwerk moeten worden doorgegeven aan de geselecteerde doelnetwerken toestaan. Deze regel is een standaardregel meestal al op de Barracuda NextGen firewall, maar is uitgeschakeld. Met de rechtermuisknop op deze regel hebben toegang tot de opdracht activeren regel. De regel die hier worden weergegeven is als u wilt de twee lokale subnetten die zijn gemaakt als verwijzingen in de sectie vereisten van dit document toevoegen aan het bronkenmerk van deze regel gewijzigd.
  
    ![Uitgaande firewallregel][14]
* **Een DNS-regel**: deze doorgeven regel staat toe dat alleen DNS (poort 53)-verkeer door te geven aan de DNS-server. Deze regel kunnen specifiek DNS voor deze omgeving die de meeste verkeer van de FrontEnd naar de back-end is geblokkeerd.
  
    ![DNS-firewallregel][15]
  
    **Opmerking**: In dit scherm opname de verbindingsmethode is opgenomen. Omdat deze regel is voor intern IP-adres voor verkeer van interne IP-adres, wordt geen NATing is vereist, wordt deze de verbindingsmethode is ingesteld op 'Nee snat omzetten' voor deze regel op te geven.
* **Subnet naar Subnet regel**: doorgeven voor deze regel is een standaardregel die zijn geactiveerd en gewijzigd zodat alle servers in het subnet van de back-end verbinding maken met een willekeurige server in de front-end-subnet. Deze regel is alle interne verkeer, zodat de verbindingsmethode kan worden ingesteld op Nee snat omzetten.
  
    ![Intra-VNet-firewallregel][16]
  
    **Opmerking**: het selectievakje in twee richtingen niet is ingeschakeld (noch is ingeschakeld in de meeste regels), dit is van belang voor deze regel maakt deze regel 'eenrichtings', een verbinding kan worden gestart vanuit de back-end-subnet met het netwerk front-end, maar niet de omgekeerde. Als dit selectievakje is ingeschakeld, zou deze regel in twee richtingen verkeer, wat van onze logisch diagram niet inschakelen.
* **Alle Verkeersregel voor weigeren**: dit moet altijd de laatste regel (in termen van prioriteit), en als zodanig als een verkeersstromen niet overeen met een van de voorgaande regels wordt door deze regel wordt verwijderd. Dit is een standaardregel en meestal geactiveerd, worden er zijn geen wijzigingen in het algemeen nodig. 
  
    ![Regel voor weigeren van Firewall][17]

> [!IMPORTANT]
> Zodra alle bovenstaande regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om ervoor te zorgen verkeer wordt toegestaan of geweigerd desgewenst. In dit voorbeeld zijn de regels in de volgorde die ze moeten worden weergegeven in het raster Main regels in de Barracuda Management-Client te sturen.
> 
> 

## <a name="rule-activation"></a>Activering van de regel
Met de ruleset aan de specificatie van het diagram logica is gewijzigd, moet de ruleset worden geüpload naar de firewall en wordt geactiveerd.

![Activering van de firewall-regel][18]

In de rechterbovenhoek van de management-client zijn een cluster van knoppen. Klik op de knop 'Wijzigingen verzenden' voor het verzenden van de gewijzigde regels op de firewall en klik vervolgens op de knop 'Activeren'.

Deze versie van de omgeving voorbeeld is voor de activering van de firewall ruleset voltooid.

## <a name="traffic-scenarios"></a>Verkeer scenario 's
> [!IMPORTANT]
> Een sleutel takeway is om te weten dat **alle** verkeer wordt geleverd door de firewall. Dus met extern bureaublad met de server IIS01 wordt zelfs als het in de Front-End-Cloudservice en op de front-end-subnet voor toegang tot deze server RDP moet aan de firewall op poort 8014, en vervolgens de firewall voor het routeren van de RDP-aanvraag intern met de RDP-Por IIS01 toestaan t. Knop voor de Azure portal 'Verbinden' werkt niet omdat er geen directe RDP-pad naar IIS01 (zo ver mogelijk de portal kunt zien). Dit betekent dat alle verbindingen van internet naar de Security-Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx.
> 
> 

Voor deze scenario's, moeten de volgende firewallregels zijn voldaan:

1. Beheer van Firewall
2. RDP naar IIS01
3. RDP naar DNS01
4. RDP naar AppVM01
5. RDP naar AppVM02
6. App-verkeer naar het Web
7. App-verkeer naar AppVM01
8. Uitgaand naar het Internet
9. Frontend naar DNS01
10. Intra-subnetverkeer (back-end-front-end alleen)
11. Alles weigeren

De werkelijke firewall ruleset wordt waarschijnlijk veel andere regels naast deze informatie, de regels op een bepaalde firewall wordt ook beschikken over verschillende prioriteitsnummers dan degene die hier worden vermeld. Deze lijst en de bijbehorende cijfers worden relevantie tussen alleen deze elf regels en de relatieve prioriteit onder ze bieden. Met andere woorden; op de werkelijke firewall kan de 'RDP naar IIS01' regel getal 5, maar als het zich onder de regel 'Firewallbeheer' en boven de regel 'RDP-DNS01' zou uitgelijnd met de bedoeling van deze lijst. De lijst helpt ook bij het onderstaande scenario's zodat beknopt alternatief bevat; bijvoorbeeld 'FW regel 9 (DNS)'. Ook als beknopt alternatief bevat, de vier regels voor RDP wordt gezamenlijk aangeroepen, 'de RDP-regels' wanneer het verkeer scenario houdt geen verband met RDP.

Ook intrekken dat Netwerkbeveiligingsgroepen in-place zijn voor binnenkomend internetverkeer op de front-end- en back-end-subnetten.

#### <a name="allowed-internet-to-web-server"></a>(Toegestaan) Internet-webserver
1. Internet aanvragen HTTP-gebruikerspagina van SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud-service geeft verkeer via open eindpunten op poort 80 voor firewall-interface op 10.0.0.4:80
3. Er is geen NSG die is toegewezen aan beveiliging subnet, geval system NSG-regels verkeer firewall toestaan
4. Verkeer komt binnen via interne IP-adres van de firewall (10.0.1.4)
5. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regels (regels RDP), 2-5 niet toepassen, verplaatsen naar de volgende regel
   3. FW regel 6 (App: Web) is van toepassing, verkeer wordt toegestaan, firewall NAT's naar 10.0.1.4 (IIS01)
6. Het subnet Frontend begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT zou door de firewall, het adres van de bronserver is dus nu de firewall die zich in het subnet van de beveiliging en zichtbaar zijn voor het subnet Frontend NSG moet 'lokaal' verkeer en is dus toegestaan), verplaatsen naar de volgende regel
   2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
7. IIS01 luistert voor internetverkeer, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
8. IIS01 pogingen tot initieert een FTP-sessie naar AppVM01 op subnet Backend
9. De route UDR op subnet Frontend maakt de firewall van de volgende hop
10. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
11. Firewall begint regelverwerking:
    1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
    2. FW regel 2-5 (RDP-regels) niet van toepassing, verplaatsen naar de volgende regel
    3. FW regel 6 (App: Web) niet van toepassing, verplaatsen naar de volgende regel
    4. FW regel 7 (App: back-end) is van toepassing, verkeer is toegestaan, firewall stuurt het verkeer naar 10.0.2.5 (AppVM01)
12. Het back-end-subnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
13. AppVM01 ontvangt de aanvraag en start van de sessie en antwoordt
14. De route UDR op subnet Backend maakt de firewall van de volgende hop
15. Aangezien er zijn geen uitgaande NSG-regels in het back-end-subnet dat het antwoord is toegestaan
16. Omdat dit verkeer op een vastgestelde sessie retourneert stuurt de firewall het antwoord terug naar de webserver (IIS01)
17. Subnet frontend begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
18. De IIS-server het antwoord ontvangt, de transactie met AppVM01 en daarna voltooit de HTTP-antwoord bouwen, wordt deze HTTP-antwoord verzonden naar de aanvrager
19. Aangezien er zijn geen uitgaande NSG-regels in het antwoord is toegestaan subnet Frontend
20. Het HTTP-antwoord treffers in de firewall en omdat dit het antwoord op een NAT-sessie tot stand gebrachte is wordt geaccepteerd door de firewall
21. De firewall stuurt vervolgens door het antwoord terug naar de Internet-gebruiker
22. Omdat er geen uitgaande ontvangt NSG-regels of UDR hops op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker de aangevraagde webpagina.

#### <a name="allowed-internet-rdp-to-backend"></a>(Toegestaan) Internet RDP met back-end
1. RDP-sessie op AppVM01 via SecSvc001.CloudApp.Net:8025, waarbij 8025 het poortnummer van de gebruiker die is toegewezen voor de firewallregel 'RDP-AppVM01 is' serverbeheerder op internet-aanvragen
2. Geeft de cloudservice-verkeer via de open eindpunten op poort 8025 voor firewall-interface op 10.0.0.4:8025
3. Er is geen NSG die is toegewezen aan beveiliging subnet, geval system NSG-regels verkeer firewall toestaan
4. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regel 2 (RDP IIS) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regel 3 (RDP DNS01) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 4 (RDP AppVM01) is van toepassing, het verkeer wordt toegestaan, firewall NAT's naar 10.0.2.5:3386 (RDP-poort op AppVM01)
5. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT zou door de firewall, het adres van de bronserver is dus nu de firewall die zich in het subnet van de beveiliging en zichtbaar zijn voor het subnet voor back-end NSG moet 'lokaal' verkeer en is dus toegestaan), verplaatsen naar de volgende regel
   2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
6. AppVM01 voor RDP-verkeer luistert en antwoordt
7. Standaardregels toepassen met geen uitgaande NSG-regels en terugkerend verkeer is toegestaan
8. UDR routes uitgaand verkeer naar de firewall als de volgende hop
9. Omdat dit verkeer op een vastgestelde sessie retourneert het antwoord terug naar de internet-gebruiker wordt doorgegeven door de firewall
10. RDP-sessie is ingeschakeld
11. AppVM01 gebruikersnaam wachtwoord gevraagd

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server DNS-lookup op DNS-server
1. Web Server, IIS01, moet een gegevensfeed op www.data.gov, maar moet u het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op het subnet voor back-end) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. UDR routes uitgaand verkeer naar de firewall als de volgende hop
4. Er is geen uitgaand NSG-regels zijn gebonden aan het subnet Frontend, is verkeer toegestaan
5. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regel 2-5 (RDP-regels) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) is van toepassing, verkeer is toegestaan, firewall stuurt het verkeer naar 10.0.2.4 (DNS01)
6. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
   2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
7. DNS-server ontvangt de aanvraag
8. DNS-server beschikt niet over het adres in de cache opgeslagen en wordt u gevraagd een basis-DNS-server op het internet
9. UDR routes uitgaand verkeer naar de firewall als de volgende hop
10. Er is geen uitgaand NSG-regels op subnet Backend, verkeer is toegestaan.
11. Firewall begint regelverwerking:
    1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
    2. FW regel 2-5 (RDP-regels) niet van toepassing, verplaatsen naar de volgende regel
    3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
    4. Geldt FW regel 8 (met Internet), verkeer is toegestaan, sessie snat omzetten uit naar basis-DNS-server op het Internet is
12. Internet-DNS-server reageert, omdat deze sessie is geïnitieerd door de firewall, het antwoord wordt geaccepteerd door de firewall
13. Omdat dit een vastgestelde sessie, stuurt de firewall het antwoord aan de gang worden gezet DNS01-server
14. Het back-end-subnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
15. De DNS-server ontvangt het antwoord plaatst en vervolgens reageert op de eerste aanvraag terug naar IIS01
16. De route UDR op subnet backend maakt de firewall van de volgende hop
17. Er bestaat geen uitgaande NSG-regels op het subnet voor back-end, is verkeer toegestaan
18. Dit is een vastgestelde sessie op de firewall, het antwoord wordt doorgestuurd door de firewall terug naar de IIS-server
19. Subnet frontend begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan
20. IIS01 ontvangt het antwoord van DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Toegestaan) Back-endserver met Frontend-server
1. Een beheerder AppVM02 via RDP aangemeld op opvraagt een bestand rechtstreeks vanaf de server IIS01 via windows Verkenner
2. De route UDR op subnet Backend maakt de firewall van de volgende hop
3. Aangezien er zijn geen uitgaande NSG-regels in het back-end-subnet dat het antwoord is toegestaan
4. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regel 2-5 (RDP-regels) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   6. FW regel 10 (Intra-Subnet) is van toepassing, verkeer is toegestaan, firewall verkeer doorgegeven aan 10.0.1.4 (IIS01)
5. Subnet frontend begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
   2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
6. Als de juiste verificatie en autorisatie, IIS01 accepteert de aanvraag en reageert
7. De route UDR op subnet Frontend maakt de firewall van de volgende hop
8. Aangezien er zijn geen uitgaande NSG-regels in het antwoord is toegestaan subnet Frontend
9. Omdat dit een bestaande sessie op de firewall voor deze reactie is toegestaan en de firewall retourneert het antwoord op AppVM02
10. Subnet backend begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet Block) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard NSG-regels toestaat subnet naar subnet verkeer, verkeer is toegestaan, stopt u de verwerking van NSG-regels
11. AppVM02 ontvangt het antwoord

#### <a name="denied-internet-direct-to-web-server"></a>(Geweigerd) Rechtstreeks naar de webserver van Internet
1. Internet-gebruiker probeert te krijgen van de webserver, IIS01, via de service FrontEnd001.CloudApp.Net
2. Aangezien er geen eindpunten voor HTTP-verkeer is geopend zijn, dit zou niet doorgeven aan de Cloudservice en de server wouldn't bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, blokkeren het NSG (Internet blokkeren) op het subnet Frontend dit verkeer
4. Ten slotte de front-end-subnet UDR route zou uitgaand verkeer verzenden vanuit IIS01 aan de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en het uitgaande antwoord Thus er ten minste drie onafhankelijke lagen verdediging tussen zijn neerzetten het internet en IIS01 via de cloudservice zo wordt voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-backend-server"></a>(Geweigerd) Internet naar back-endserver
1. Internet-gebruiker probeert te krijgen van een bestand op AppVM01 via de service BackEnd001.CloudApp.Net
2. Aangezien er geen eindpunten voor de bestandsshare is geopend zijn, dit zou niet doorgeven voor de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, blokkeren het NSG (Internet blokkeren) dit verkeer
4. Ten slotte de route UDR zou uitgaand verkeer verzenden vanuit AppVM01 aan de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en het uitgaande antwoord, dus er zijn ten minste drie onafhankelijke lagen verdediging tussen internet verwijderen en AppVM01 via de cloudservice zo wordt voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-frontend-server-to-backend-server"></a>(Geweigerd) Back-endserver frontend-server
1. Stel IIS01 is geknoeid en schadelijke code die bij de back-endservers subnet scan wordt uitgevoerd.
2. De route Frontend subnet UDR zou Stuur alle uitgaande verkeer van IIS01 aan de firewall als de volgende hop. Dit is geen iets dat kan worden gewijzigd door de VM waarmee is geknoeid.
3. De firewall zou het verkeer verwerken als de aanvraag is AppVM01 of de DNS-server voor DNS-zoekacties die verkeer kan mogelijk worden toegestaan door de firewall (vanwege FW regels 7 en 9). Al het andere verkeer wordt geblokkeerd door FW regel 11 (alle weigeren).
4. Of geavanceerde detectie van dreigingen in de firewall is ingeschakeld (Zie de documentatie van de leverancier voor uw specifieke netwerkapparaat advanced threat mogelijkheden, die niet in dit document wordt besproken), zelfs als het verkeer dat door de basic doorstuurregels zou worden toegestaan beschreven in dit document kan worden voorkomen als het verkeer bevindt handtekeningen van bekende of patronen die een regel voor geavanceerde threat vlag.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Geweigerd) Internet-DNS-lookup op DNS-server
1. Internet-gebruiker probeert voor het opzoeken van een interne DNS-record op DNS01 via BackEnd001.CloudApp.Net-service 
2. Aangezien er geen eindpunten voor DNS-verkeer is geopend zijn, dit zou niet doorgeven aan de Cloudservice en de server wouldn't bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, wordt het NSG-regel (Internet blokkeren) op het subnet Frontend dit verkeer geblokkeerd
4. Ten slotte de back-end subnet UDR route zou uitgaand verkeer verzenden vanuit DNS01 aan de firewall als de volgende hop en de firewall zou dit zien als asymmetrische verkeer en verwijderen van het uitgaande antwoord Thus er ten minste drie onafhankelijke lagen verdediging tussen zijn de Internet en DNS01 via de cloudservice zo wordt voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Geweigerd) Internet op SQL-toegang via de Firewall
1. Internet-gebruiker opvraagt SQL-gegevens uit SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Omdat er geen eindpunten geopend voor SQL zijn, dit zou niet doorgeven voor de Cloudservice en de firewall wouldn't bereiken
3. Als SQL-eindpunten geopend voor een bepaalde reden zijn, zou de firewall regelverwerking begint:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regels (regels RDP), 2-5 niet toepassen, verplaatsen naar de volgende regel
   3. FW regel 6 en 7 (autorisatieregels) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   6. FW regel 10 (Intra-Subnet) niet van toepassing, verplaatsen naar de volgende regel
   7. FW regel 11 (alle weigeren) is van toepassing, het verkeer wordt geblokkeerd, stoppen, regelverwerking

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Sla het volledige Script in een PowerShell-scriptbestand. De netwerkconfiguratie opslaan in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen zo nodig wijzigen. Voer het script uit en volg de bovenstaande van Firewall regel setup instructies.

#### <a name="full-script"></a>Volledige Script
Dit script wordt, op basis van de gebruiker gedefinieerde variabelen:

1. Verbinding maken met een Azure-abonnement
2. Een nieuw opslagaccount maken
3. Maak een nieuw VNet en drie subnetten zoals gedefinieerd in het netwerk-configuratiebestand
4. Vijf virtuele machines; opzetten firewall 1 en 4 WindowsServer VM 's
5. Configureer UDR inclusief:
   1. Twee nieuwe routetabellen maken
   2. Routes toevoegen aan de tabellen
   3. Tabellen aan de juiste subnetten binden
6. Doorsturen via IP op de NVA inschakelen
7. Configureer het NSG inclusief:
   1. Maken van een NSG
   2. Een regel toe te voegen
   3. Het NSG binden aan de juiste subnetten

Deze PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of de server.

> [!IMPORTANT]
> Wanneer dit script wordt uitgevoerd, kunnen er waarschuwingen of andere informatieve berichten die pop in PowerShell. Alleen foutberichten in rood zijn aanleiding.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Netwerk-configuratiebestand
Dit xml-bestand opslaan met bijgewerkte locatie en de koppeling toevoegen aan dit bestand naar de variabele $NetworkConfigFile in het bovenstaande script.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Voorbeeldscripts toepassing
Als u een voorbeeldtoepassing voor deze en andere voorbeelden DMZ installeren wilt, een is opgegeven op de volgende koppeling: [voorbeeldscript toepassing][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirectionele DMZ met NVA, NSG en UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische weergave van de Firewall-regels"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Een Object FrontEnd-netwerk maken"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Een DNS-Server-Object maken"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie van de standaardregel voor RDP"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Pictogram voor toepassing omleiding"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Bestemming NAT-pictogram"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass-pictogram"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewallregel Management"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP-firewallregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web-firewallregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "AppVM01 firewallregel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Uitgaande firewallregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-firewallregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Intra-VNet-firewallregel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Regel voor weigeren van Firewall"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Activering van de firewall-regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
