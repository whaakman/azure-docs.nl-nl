---
title: Voorbeeld van DMZ – een DMZ netwerken met een Firewall, UDR en NSG beschermen bouwen | Microsoft Docs
description: Bouw een DMZ met Firewall, de gebruiker gedefinieerde Routering en Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 36d6733ddc73ace2026ea838cf8f701db95469e6
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448463"
---
# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: een DMZ netwerken met een Firewall, UDR en NSG beschermen bouwen
[Ga terug naar de grens Best Practices pagina][HOME]

In dit voorbeeld wordt een DMZ met firewall, vier windows-servers, gebruiker gedefinieerde routering, doorsturen via IP en Netwerkbeveiligingsgroepen maken. Het helpt ook bij elk van de relevante opdrachten voor een beter begrip van elke stap. Er is ook een sectie verkeer Scenario voor een gedetailleerde stapsgewijze hoe verkeer wordt uitgevoerd in de defensieve in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige code en instructies voor het bouwen van deze omgeving om te testen en te experimenteren met verschillende scenario's. 

![Bidirectionele DMZ met NVA, NSG en UDR][1]

## <a name="environment-setup"></a>Omgeving instellen
In dit voorbeeld is er een abonnement dat u het volgende bevat:

* Drie cloudservices: “SecSvc001”, “FrontEnd001”, and “BackEnd001”
* Een Virtueelnetwerk 'CorpNetwork', met drie subnetten: “SecNet”, “FrontEnd”, and “BackEnd”
* Een virtueel netwerkapparaat, in dit voorbeeld een firewall, die is verbonden met het subnet SecNet
* Een Windows-Server die staat voor een toepassing webserver ("IIS01")
* Twee windows-servers die staan voor toepassing back end servers ("AppVM01", "AppVM02")
* Een Windows-server die staat voor een DNS-server ("DNS01")

In het gedeelte hieronder met verwijzingen wordt er een PowerShell-script dat wordt gemaakt van de meeste van de omgeving die hierboven worden beschreven. Het bouwen van de virtuele machines en virtuele netwerken, maar moeten worden uitgevoerd door het voorbeeldscript worden niet beschreven in dit document.

De omgeving bouwen:

1. Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario)
2. Bijwerken van de Gebruikersvariabelen voor de in het script moet overeenkomen met de omgeving die het script uitgevoerd op basis van wordt (abonnementen, servicenamen, enzovoort)
3. Voer het script uit in PowerShell

**Opmerking**: De regio aangegeven in het PowerShell-script moet overeenkomen met de regio aangegeven in het netwerk van het XML-configuratiebestand.

Nadat het script is uitgevoerd, de volgende stappen van script dat volgt uit kunnen worden genomen:

1. De firewall-regels hebt ingesteld, wordt dit in de onderstaande sectie met de titel behandeld: Beschrijving van de firewall-regel.
2. (Optioneel) zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en appserver met een eenvoudige webtoepassing waarmee de testen met deze configuratie DMZ.

Nadat het script uitgevoerd in de firewall regels moeten worden voltooid, wordt dit wordt behandeld in de sectie met de titel: Firewall-regels.

## <a name="user-defined-routing-udr"></a>Gebruiker gedefinieerde Routering
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

De VNETLocal is altijd de prefix(en) opgegeven adres van het VNet voor dat specifieke netwerk (bijvoorbeeld het wordt gewijzigd van VNet naar VNet, afhankelijk van hoe elke specifieke VNet wordt gedefinieerd). De resterende systeemroutes statisch zijn en de standaardinstellingen als hierboven.

Als voor de prioriteit, routes worden verwerkt via het langste voorvoegsel overeenkomende LPM (longest prefix)-methode, dus de meest specifieke route in de tabel wilt toepassen op een bepaalde bestemming-adres.

Daarom zou verkeer (bijvoorbeeld voor de server DNS01 10.0.2.4) die bestemd zijn voor het lokale netwerk (10.0.0.0/16) worden gerouteerd via de VNet-naar de bestemming vanwege de route 10.0.0.0/16. Met andere woorden, voor 10.0.2.4, de 10.0.0.0/16 route is de meest specifieke route, zelfs als de 10.0.0.0/8 en 0.0.0.0/0 ook kunnen worden toegepast, maar omdat ze minder zijn specifieke ze niet van invloed zijn op dit verkeer. Het verkeer naar 10.0.2.4 zou dus hebben als de volgende hop van de lokale VNet, en gewoon routeren naar de bestemming.

Als het verkeer is bedoeld voor 10.1.1.1 bijvoorbeeld, de route 10.0.0.0/16 wouldn't toepast, maar de 10.0.0.0/8 zou zijn het meest specifiek en het verkeer zou dit verwijderd ("black gaan') omdat de volgende hop Null is. 

Als het doel is niet van toepassing op een van de Null-voorvoegsels of de voorvoegsels VNETLocal, dan de minst specifiek volgt routeren, 0.0.0.0/0 en om worden doorgestuurd naar het Internet als de volgende hop en dus uit van Azure internet edge.

Als er twee identieke voorvoegsels in de routetabel, wordt het volgende is de volgorde van voorkeur op basis van de routes 'bron'-kenmerk:

1. "VirtualAppliance" = een handmatig worden toegevoegd aan de tabel de gebruiker gedefinieerde Route
2. 'VPN-gateway' een dynamische Route BGP (gebruikt in combinatie met hybride netwerken), = toegevoegd door een dynamische netwerkprotocol, deze routes kunnen worden gewijzigd na verloop van tijd als de dynamische-protocol wordt automatisch in gekoppelde netwerk aangepast
3. 'Standaard' = de Systeemroutes, het lokale VNet en de statische-vermeldingen zoals wordt weergegeven in de bovenstaande routetabel.

> [!NOTE]
> U kunt nu de gebruiker gedefinieerde routering (UDR) gebruiken met ExpressRoute en VPN-Gateways om af te dwingen uitgaand en binnenkomend cross-premises verkeer worden doorgestuurd naar een virtueel netwerkapparaat (NVA).
> 
> 

#### <a name="creating-the-local-routes"></a>Het maken van de lokale routes
In dit voorbeeld worden twee routeringstabellen nodig, één voor de front-end en back-end-subnetten. Elke tabel is geladen met statische routes die geschikt is voor het opgegeven subnet. In dit voorbeeld heeft elke tabel drie routes:

1. Lokaal subnetverkeer met geen ' volgende hop ' gedefinieerd voor het lokale subnetverkeer toestaan om over te slaan van de firewall
2. Verkeer in virtuele netwerken met een ' volgende hop ' gedefinieerd als een firewall, overschrijft dit de standaardregel waarmee lokale VNet-verkeer op het rechtstreeks doorsturen
3. Alle resterende verkeer (0/0) met een ' volgende hop ' gedefinieerd als de firewall

Zodra de routeringstabel zijn gemaakt zijn ze gekoppeld aan hun subnetten. Voor het subnet Frontend routeringstabel, nadat deze zijn gemaakt en gekoppeld aan het subnet als volgt uitzien:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


In dit voorbeeld de volgende opdrachten worden gebruikt om te maken van de routetabel, een door de gebruiker gedefinieerde route toevoegen en vervolgens verbindt de routetabel aan een subnet (Houd er rekening mee; alle items die onder die begint met een dollarteken (bijvoorbeeld: $BESubnet) zijn door de gebruiker gedefinieerde variabelen van het script in de naslaginformatie over sectie van dit document):

1. De basis-routeringstabel moet eerst worden gemaakt. Dit fragment toont het maken van de tabel voor de back-end-subnet. In het script wordt ook een bijbehorende tabel gemaakt voor de front-end-subnet.
   
     New-AzureRouteTable -Name $BERouteTableName `
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Nadat de routetabel is gemaakt, kunnen de specifieke gebruiker gedefinieerde routes worden toegevoegd. In deze snipped, wordt al het verkeer (0.0.0.0/0) worden gerouteerd via het virtuele apparaat (een variabele, $VMIP [0] wordt gebruikt om door te geven in het IP-adres toegewezen wanneer het virtuele apparaat eerder in het script is gemaakt). In het script wordt ook een overeenkomende regel gemaakt in de Frontend-tabel.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. De routevermelding van de bovenstaande overschrijft de '0.0.0.0/0' standaardroute, maar de standaardregel 10.0.0.0/16 nog steeds bestaande waarmee verkeer binnen het VNet te routeren rechtstreeks naar de bestemming en niet naar het virtuele netwerkapparaat. Juiste dit gedrag van de volgende regel moet worden toegevoegd.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Op dit moment is er een keuze worden gemaakt. Met de bovenstaande twee routes wordt al het verkeer gerouteerd naar de firewall voor evaluatie, zelfs verkeer binnen één subnet. Dit kan nodig zijn, maar dat verkeer binnen een subnet voor het routeren lokaal zonder tussenkomst van de firewall van een derde, zeer specifieke regel kan worden toegevoegd. Deze route statussen die elk adres voor het lokale subnet kan alleen destine routeren er rechtstreeks (NextHopType VNETLocal =).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Ten slotte, met de routeringstabel gemaakt en gevuld met een gebruiker gedefinieerde routes, de tabel moet nu worden gebonden aan een subnet. De routetabel front-end is in het script ook gebonden aan het Frontend-subnet. Dit is het script van de binding voor de back-end-subnet.
   
     Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Doorsturen via IP
Een functie companion UDR, is doorsturen via IP. Dit is een instelling op een virtueel apparaat dat kan worden ontvangen van verkeer niet specifiek zijn gericht op het apparaat en stuur deze vervolgens dat verkeer naar de uiteindelijke bestemming.

Een voorbeeld: als verkeer van AppVM01 een verzoek naar de server DNS01 zendt UDR wilt routeren dit aan de firewall. Met het IP-doorsturen is ingeschakeld, wordt het verkeer voor de doel-DNS01 (10.0.2.4) worden geaccepteerd door het apparaat (10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder doorsturen via IP op de Firewall is ingeschakeld, wordt verkeer niet geaccepteerd door het apparaat zelfs als de routetabel de firewall als de volgende hop is. 

> [!IMPORTANT]
> Het is essentieel om te onthouden om in te schakelen doorsturen via IP in combinatie met de gebruiker gedefinieerde routering.
> 
> 

Instellen van het doorsturen via IP is slechts één opdracht en tijdens de aanmaak van virtuele machine kan worden gedaan. De stroom van dit voorbeeld wordt het codefragment is aan het einde van het script en gegroepeerd met de UDR-opdrachten:

1. Aanroepen van het VM-exemplaar dat in dit geval is van uw virtueel apparaat, de firewall en doorsturen via IP inschakelen (Houd er rekening mee; een item in rood die begint met een dollarteken (bijvoorbeeld: $VMName[0]) is een door de gebruiker gedefinieerde variabele van het script in de sectie Verwijzingen van dit document. De op nul tussen vierkante haken, [0] Hiermee geeft u de eerste virtuele machine in de matrix van virtuele machines, voor de voorbeeldscript om te werken zonder aanpassingen, de eerste virtuele machine (VM 0) moet de firewall):
   
     Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een NSG-groep gemaakt en vervolgens met een enkele regel geladen. Deze groep is vervolgens alleen aan de front-end en back-end-subnetten (niet de SecNet) gebonden. Declaratief wordt met de volgende regel gemaakt:

1. Al het verkeer (alle poorten) van het Internet naar het hele VNet (alle subnetten) is geweigerd

Hoewel nsg's in dit voorbeeld worden gebruikt, is het belangrijkste doel als een secundaire laag van beveiliging tegen handmatige onjuiste configuratie. We willen dat moet worden geblokkeerd dat alle inkomende verkeer vanaf internet aan de front-end of back-end-subnetten, verkeer alleen via het subnet SecNet aan de firewall moeten stromen (en vervolgens indien nodig u aan bij de front-end of back-end-subnetten). Bovendien aan de regels UDR aanwezig is, zou al het verkeer dat gemakkelijk in de front-end of back-end-subnetten worden omgeleid om aan de firewall (dankzij de udr-route). De firewall ziet dit als een asymmetrische stroom en het uitgaande verkeer wilt verwijderen. Er zijn dus drie beveiligingslagen aan de front-end en back-end-subnetten; beveiligen (1) er is geen open eindpunten op de FrontEnd001 en BackEnd001 cloud-services, 2) nsg's verkeer vanaf Internet, 3) de firewall laten vallen asymmetrische verkeer weigeren.

Een interessante met betrekking tot de Netwerkbeveiligingsgroep in dit voorbeeld is dat deze slechts één regel, hieronder weergegeven bevat, dit is om verkeer van internet naar het hele virtuele netwerk waaronder het subnet van de beveiliging te weigeren. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Echter, omdat de NSG wordt alleen gekoppeld aan de front-end en back-end-subnetten, de regel is niet verwerkt op het verkeer inkomend in het subnet van de beveiliging. Als gevolg hiervan, zelfs als de NSG-regel geen verkeer van Internet naar een adres op het VNet, staat omdat de NSG nooit is gebonden aan het subnet van de beveiliging, het verkeer naar het subnet van de beveiliging.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewallregels
Op de firewall moet regels voor doorsturen worden gemaakt. Omdat de firewall blokkeert of doorsturen van alle binnenkomende, uitgaande en intra-VNet-verkeer worden veel firewallregels die nodig zijn. Alle binnenkomend verkeer bereikt de beveiligingsservice openbare IP-adres (op verschillende poorten), ook moeten worden verwerkt door de firewall. Een best practice is om het diagram van de logische stromen voordat het instellen van de subnetten en firewall-regels om te voorkomen dat later bijwerken. De volgende afbeelding is een logische weergave van de firewall-regels voor dit voorbeeld:

![Logische weergave van de Firewall-regels][2]

> [!NOTE]
> Op basis van het virtuele netwerkapparaat gebruikt, varieert de beheerpoorten. In dit voorbeeld wordt verwezen naar een Barracuda NextGen Firewall die gebruikmaakt van poort 22, 801 en 807. Raadpleeg de documentatie van de leverancier apparaat voor de exacte poorten gebruikt voor het beheer van het apparaat wordt gebruikt.
> 
> 

### <a name="logical-rule-description"></a>Beschrijving van de logische regel
In het bovenstaande logische diagram wordt het subnet van de beveiliging niet weergegeven omdat de firewall is de enige bron op dat subnet, en dit diagram wordt weergegeven de firewall-regels en hoe ze logisch toestaan of weigeren van verkeersstromen en niet de werkelijke gerouteerde pad. Ook de externe poorten voor de RDP-verkeer hoger zijn geselecteerd variabele poorten (8014 – 8026) en zijn geselecteerd om te enigszins zijn afgestemd op de laatste twee octetten van het lokale IP-adres voor de leesbaarheid te vergemakkelijken (bijvoorbeeld 10.0.1.4-adres van de lokale server is gekoppeld aan de externe poort 8014), maar geen hogere poorten voor niet-conflicterende kunnen worden gebruikt.

In dit voorbeeld moet 7 typen regels, deze regeltypen worden beschreven als volgt:

* Externe regels (voor binnenkomend verkeer):
  1. Firewallregel voor beheer: Deze omleidings-App-regel staat verkeer moet worden doorgegeven aan de beheerpoorten van het virtuele netwerkapparaat.
  2. RDP-regels (voor elke WindowsServer): Deze vier regels (één voor elke server) kunt beheer van de afzonderlijke servers via RDP. Dit kan ook worden gebundeld in één regel, afhankelijk van de mogelijkheden van het virtuele netwerkapparaat wordt gebruikt.
  3. Regels voor netwerkverkeer van toepassing: Er zijn twee toepassing verkeersregels, de eerste voor het verkeer van de web-front-end en de seconde voor het back-end-verkeer (bijvoorbeeld webserver als de gegevenslaag). De configuratie van deze regels zal afhankelijk zijn van de netwerkarchitectuur (waarin uw servers worden geplaatst) en stromen-verkeer (welke richting het verkeer verloopt, en op welke poorten worden gebruikt).
     * De eerste regel wordt het verkeer de feitelijke toepassing bereiken van de toepassingsserver toestaan. Terwijl de andere regels voor de beveiliging, beheer, enzovoort, zijn regels voor Application wat kunnen externe gebruikers of -services tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus een firewallregel voor één toepassing binnenkomend verkeer wordt omgeleid naar het externe IP-adres, naar het interne IP-adres van web-servers. De sessie omgeleide verkeer zouden worden NAT moest de interne server.
     * De tweede regel van toepassing verkeer is de back-end-regel waarmee de webserver om te communiceren met de server AppVM01 (maar niet AppVM02) via een willekeurige poort.
* Interne regels (voor intra-VNet-verkeer)
  1. Uitgaand naar Internetregel: Deze regel kan verkeer van elk netwerk moeten worden doorgegeven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel al op de firewall, maar een uitgeschakelde status. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  2. DNS-regel: Deze regel kunnen alleen DNS (poort 53)-verkeer moeten worden doorgegeven aan de DNS-server. Voor deze omgeving die meeste verkeer van de front-end naar de back-end wordt geblokkeerd, kan met deze regel specifiek DNS vanuit een lokale subnet.
  3. Subnet naar Subnet regel: Deze regel is dat elke server in het back-end-subnet verbinding maken met elke server in de front-end-subnet (maar niet omgekeerd).
* Failsafe regel (voor verkeer dat niet voldoet aan een van de bovenstaande):
  1. Alle Verkeersregel voor weigeren: Dit moet altijd de laatste regel (voor wat betreft prioriteit) en als zodanig als een verkeer stroomt zodat deze overeenkomt met een van de vorige regels die wordt deze verwijderd door deze regel is mislukt. Dit is een standaardregel en zijn meestal geactiveerd, worden geen wijzigingen zijn aangebracht in het algemeen nodig.

> [!TIP]
> Een willekeurige poort is toegestaan voor eenvoudige van dit voorbeeld is in een echte scenario het meest specifieke poort op de tweede toepassing verkeersregel en adresbereiken moeten worden gebruikt voor het verminderen van de kwetsbaarheid voor aanvallen van deze regel.
> 
> 

<br />

> [!IMPORTANT]
> Nadat u alle bovenstaande regels hebt gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om te controleren of verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in volgorde van prioriteit. Het is eenvoudig te worden vergrendeld buiten de firewall vanwege onjuiste geordende regels. Ten minste, zorg ervoor dat het beheer van de firewall zelf is altijd de absolute hoogste prioriteit regel.
> 
> 

### <a name="rule-prerequisites"></a>Regel voor vereisten
Een vereiste voor de virtuele Machine met de firewall zijn openbare eindpunten. De juiste openbare eindpunten moeten zijn geopend voor de firewall verkeer verwerken. Er zijn drie typen verkeer in dit voorbeeld. De RDP-verkeer (1) beheer van verkeer naar het besturingselement voor de firewall en firewall-regels, 2) voor het beheren van de windows-servers en (3) toepassing verkeer. Dit zijn de drie kolommen van de verkeerstypen in de rechterbovenhoek de helft van een logische weergave van de firewallregels hierboven.

> [!IMPORTANT]
> Een sleutel takeway hier is om te weten dat **alle** verkeer wordt geleverd via de firewall. Dus met extern bureaublad op de server IIS01, zelfs al is het in de Front-End-Cloudservice en op de Front-End-subnet, voor toegang tot deze server we RDP moet aan de firewall op poort 8014 en vervolgens de firewall voor het routeren van de RDP-aanvraag intern met de RDP-Por IIS01 toestaan t. De knop 'Verbinden' van de Azure portal werkt niet omdat er geen directe RDP-pad naar IIS01 (zo lang de portal kunt zien). Dit betekent dat alle verbindingen vanuit het internet worden naar de Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx (verwijzing naar het bovenstaande diagram van de toewijzing van de poort van de externe en interne IP en poort).
> 
> 

Een eindpunt kan worden geopend op het moment dat de VM gemaakt of build plaatsen, zoals is gedaan in het voorbeeldscript en in dit codefragment hieronder wordt weergegeven (Houd er rekening mee; een item die begint met een dollarteken (bijvoorbeeld: $VMName[$i]) is een door de gebruiker gedefinieerde variabele van het script in de ver verwijzing n van dit document. De "$i" tussen vierkante haken, [$i] Hiermee geeft u het nummer van de matrix van een specifieke virtuele machine in een matrix van VM's):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Hoewel het niet duidelijk hier weergegeven vanwege het gebruik van variabelen, maar eindpunten zijn **alleen** geopend op de Cloud Security Service. Dit is om ervoor te zorgen dat al het binnenkomende verkeer wordt afgehandeld (gerouteerd, NAT, verloren) door de firewall.

Een management-client moet worden geïnstalleerd op een PC voor het beheren van de firewall en het maken van de configuraties die nodig zijn. Zie de documentatie van uw firewall (of andere NVA)-leverancier over het beheren van het apparaat. De rest van deze sectie en de volgende sectie, Firewall-regels maken, wordt de configuratie van de firewall zelf, via de leveranciers management-client (dat wil zeggen niet in de Azure portal of PowerShell) beschreven.

Instructies voor de client downloaden en verbinding maken met de Barracuda gebruikt in dit voorbeeld kunt u hier vinden: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Wanneer u bent aangemeld bij de firewall, maar voordat u het maken van firewallregels, zijn er twee vereiste objectklassen waardoor het maken van de regels gemakkelijker; Netwerk- en Service-objecten.

In dit voorbeeld moet drie benoemde netwerkobjecten gedefinieerd (één voor de front-end-subnet en het back-end-subnet, ook een object voor het IP-adres van de DNS-server). Het maken van een netwerk met de naam; vanaf de Barracuda NG-Beheerdashboard, gaat u naar het tabblad configuratie, in de sectie configuratie van operationele Ruleset, vervolgens klikt u op 'Netwerken' onder het menu Firewallobjecten klikken en vervolgens klikt u op Nieuw in het menu Bewerken netwerken. Het object network kan nu worden gemaakt, door de naam en het voorvoegsel toe te voegen:

![Maken van een Object front-end][3]

Hiermee maakt u een netwerk met de naam voor de front-end-subnet, een vergelijkbaar object moet worden gemaakt voor de back-end-subnet. De subnetten kunnen nu nog eenvoudiger verwezen met de naam in de firewall-regels.

Voor het Object van de DNS-Server:

![Een DNS-Server-Object maken][4]

Deze één verwijzing naar een IP-adres wordt in een DNS-regel verderop in dit document worden gebruikt.

De tweede vereiste objecten zijn Services-objecten. Deze vertegenwoordigt de poorten van de RDP-verbinding voor elke server. Omdat het bestaande object van de RDP-service is gekoppeld aan de standaardpoort voor RDP, kunnen 3389, nieuwe Services worden gemaakt zodat verkeer vanaf de externe poorten (8014-8026). De nieuwe poorten kunnen ook worden toegevoegd aan de bestaande RDP-service, maar voor een eenvoudige demonstratie, een afzonderlijke regel voor elke server worden gemaakt. Het maken van een nieuwe regel voor RDP voor een server. vanaf de Barracuda NG-Beheerdashboard, gaat u naar het tabblad configuratie, in de sectie configuratie van operationele op Ruleset, en vervolgens klikt u op 'Services' onder het menu Firewallobjecten, schuif omlaag in de lijst met services en selecteer de service 'RDP'. Met de rechtermuisknop op en selecteer kopiëren, en vervolgens met de rechtermuisknop op en selecteer plakken. Er is nu een RDP-Copy1 Service-Object dat kan worden bewerkt. Met de rechtermuisknop op de RDP-Copy1 en selecteer bewerken, het serviceobject bewerken venster maximaal zoals hieronder wordt weergegeven:

![Exemplaar van standaard RDP-regel][5]

De waarden kunnen worden bewerkt om weer te geven van de RDP-service voor een specifieke server. Voor AppVM01 de bovenstaande standaard RDP-regel moet worden gewijzigd om een nieuwe Service-naam, beschrijving en externe RDP-poort die wordt gebruikt in het diagram afbeelding 8 weer te geven (Opmerking: de poorten van de RDP-standaardwaarde van 3389 zijn gewijzigd in de externe poort die wordt gebruikt voor deze specifieke server in het geval van AppVM01 is de externe poort 8025) de gewijzigde service wordt hieronder weergegeven:

![AppVM01 regel][6]

Dit proces moet worden herhaald RDP-Services voor de overige servers; te maken AppVM02, DNS01 en IIS01. Het maken van deze Services maakt het maken van de regel eenvoudiger en duidelijker in de volgende sectie.

> [!NOTE]
> Een RDP-service voor de Firewall is niet nodig om twee redenen; (1) eerst de virtuele machine van de firewall een installatiekopie op basis van Linux is, zodat SSH op poort 22 zouden worden gebruikt voor VM-beheer in plaats van RDP, 2) poort 22 en twee andere beheerpoorten zijn toegestaan in de eerste management-regel die hieronder worden beschreven om toe te staan voor de connectiviteit van het beheer.
> 
> 

### <a name="firewall-rules-creation"></a>Firewall-regels maken
Er zijn drie typen firewallregels die in dit voorbeeld worden gebruikt, ze allemaal zijn er verschillende pictogrammen:

De omleidings-toepassing-regel: ![Omleidings-toepassingspictogram][7]

De doel-NAT-regel: ![Bestemming NAT-pictogram][8]

De Pass-regel: ![Pass-pictogram][9]

Meer informatie over deze regels kan worden gevonden op de Barracuda web site.

Als u wilt maken van de volgende regels (of Controleer of de bestaande standaardregels die), vanaf het dashboard van de client Barracuda NG beheerder gaat u naar het tabblad configuratie, in de configuratie van de operationele sectie klikt u op Ruleset. Een raster met de naam 'Main-regels worden de bestaande regels die actief is en gedeactiveerd weergegeven op de firewall. In de rechterbovenhoek van dit raster is een kleine groene '+' knop, klik hierop om een nieuwe regel te maken (Opmerking: uw firewall mogelijk "vergrendeld" voor wijzigingen, als u ziet een knop 'Vergrendelen' gemarkeerd en u zich niet maken of regels bewerken, klikt u op deze knop om de regelset 'ontgrendelen' en  bewerken toestaan). Als u een bestaande regel bewerken wilt, selecteert u deze regel, met de rechtermuisknop op en selecteer regel bewerken.

Zodra uw regels zijn gemaakt en/of gewijzigd, moet worden gepusht naar de firewall en wordt geactiveerd, als dit gebeurt niet de wijzigingen in de regel wordt pas van kracht. Het proces van push- en -activering wordt hieronder de beschrijvingen van de regel details beschreven.

De details van elke regel vereist voor het voltooien van dit voorbeeld worden als volgt beschreven:

* **Firewall-regel Management**: Deze omleidings-App-regel staat verkeer moet worden doorgegeven aan de beheerpoorten van het virtuele netwerkapparaat, in dit voorbeeld een Barracuda NextGen Firewall. De poorten zijn 801, 807 en eventueel 22. De interne en externe poorten zijn hetzelfde (dat wil zeggen geen poortvertaling genoemd). Deze regel, SETUP-MGMT-toegang, is een standaardregel en standaard ingeschakeld (in de Barracuda NextGen Firewall versie 6.1).
  
    ![Firewallregel voor beheer][10]

> [!TIP]
> De bron-adresruimte in deze regel is aanwezig, als het beheer van IP-adresbereiken bekend zijn, waardoor dit bereik ook sneller de kwetsbaarheid voor aanvallen op de beheerpoorten.
> 
> 

* **Regels voor RDP**:  Deze bestemming NAT-regels kunt beheer van de afzonderlijke servers via RDP.
  Er zijn vier belangrijke velden die nodig zijn voor deze regel maakt:
  
  1. Bron: om toe te staan RDP 'Een' vanaf elke locatie en de verwijzing wordt gebruikt in het veld voor de gegevensbron.
  2. Service – gebruik de juiste Service-Object dat eerder hebt gemaakt, in dit geval 'AppVM01 RDP', de externe poorten doorsturen naar het lokale IP-adres van servers en naar poort 3386 (de standaard RDP-poort). Deze specifieke regel is voor de RDP-toegang tot AppVM01.
  3. Doel: moet de *lokale poort op de firewall*, 'DCHP 1 lokale IP'- of eth0 als statische IP-adressen. Het rangtelwoord (eth0, eth1, enzovoort) kan afwijken als uw netwerkapparaat meerdere lokale interfaces heeft. Dit is de poort van de firewall is verzenden (kunnen niet dezelfde zijn als de ontvangende poort), de werkelijke gerouteerde bestemming zich in de doellijst-veld.
  4. Omleiding: in deze sectie weet het virtuele apparaat waar uiteindelijk dit verkeer omleiden. De eenvoudigste omleiding is te IP en poort (optioneel) te plaatsen in de doellijst-veld. Als er geen poort wordt gebruikt de doelpoort voor de binnenkomende aanvraag is (bijvoorbeeld geen vertaling) gebruikt als een poort is de poort ingesteld worden ook NAT wordt samen met de IP-adres.
     
     ![RDP-firewallregel][11]
     
     Een totaal van vier regels voor RDP moet worden gemaakt: 
     
     | Regelnaam | Server | Service | De lijst van doel |
     | --- | --- | --- | --- |
     | RDP-to-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP-to-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP-to-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP-to-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Het bereik van de bron- en Service-velden technologiegebied vermindert de kwetsbaarheid voor aanvallen. De meest beperkte mogelijkheden die zorgen functionaliteit dat ervoor moet worden gebruikt.
> 
> 

* **Regels voor netwerkverkeer toepassing**: Er zijn twee toepassing verkeersregels, de eerste voor het verkeer van de web-front-end en de seconde voor het back-end-verkeer (bijvoorbeeld webserver als de gegevenslaag). Deze regels wordt afhankelijk van de netwerkarchitectuur (waarin uw servers worden geplaatst) en stromen-verkeer (welke richting het verkeer verloopt, en op welke poorten worden gebruikt).
  
    Eerst besproken, is de front-end-regel voor webverkeer te genereren:
  
    ![Web-firewallregel][12]
  
    Deze bestemming NAT-regel staat de feitelijke toepassingsverkeer bereiken van de toepassingsserver. Terwijl de andere regels voor de beveiliging, beheer, enzovoort, zijn regels voor Application wat kunnen externe gebruikers of -services tot de toepassingen. In dit voorbeeld is er één webserver op poort 80, dus de firewallregel voor één toepassing binnenkomend verkeer wordt omgeleid naar het externe IP-adres, naar het interne IP-adres van web-servers.
  
    **Houd er rekening mee**: dat er is geen poort toegewezen in het veld doellijst, dus de binnenkomende poort 80 (of 443 voor de geselecteerde Service) worden gebruikt voor het omleiden van de webserver. Als de webserver op een andere poort luistert, bijvoorbeeld poort 8080, de doellijst veld kan worden bijgewerkt naar 10.0.1.4:8080 om toe te staan voor de poort-omleiding.
  
    De volgende regel van toepassing verkeer is de back-end-regel waarmee de webserver om te communiceren met de server AppVM01 (maar niet AppVM02) via een service:
  
    ![Firewallregel AppVM01][13]
  
    Met deze regel Pass kunnen elke IIS-server op het subnet Frontend naar de AppVM01 bereiken (IP-adres 10.0.2.5) met behulp van elk Protocol voor toegang tot gegevens die nodig zijn voor de web-App op een willekeurige poort.
  
    In deze schermopname een "\<expliciete-dest\>' in het veld bestemming wordt gebruikt om aan te duiden 10.0.2.5 als doel. Dit kan zijn expliciete zoals wordt weergegeven of een met de naam netwerkobject (zoals is gedaan in de vereisten voor de DNS-server). Dit is aan de beheerder van de firewall in welke methode wordt gebruikt. Als u wilt toevoegen 10.0.2.5 als een Explict Desitnation, dubbelklikt u op de eerste lege rij onder \<expliciete-dest\> en voer het adres in het venster dat verschijnt.
  
    Met deze regel doorgeeft, is geen NAT omdat dit intern verkeer, zodat de verbindingsmethode kan worden ingesteld op 'Geen SNAT' nodig.
  
    **Opmerking**: Het Bronnetwerk in deze regel is een resource op het subnet FrontEnd als er slechts één of een bekende specifiek aantal webservers, een resource-netwerkobject kan worden gemaakt als u wilt meer specifieke zodat deze exacte IP-adressen in plaats van de volledige-front-endsubnet.

> [!TIP]
> Met deze regel maakt gebruik van de service 'Alle' om de voorbeeld-App gemakkelijker instellen en gebruiken, kan dit ICMPv4 (ping) in een enkele regel. Dit is echter niet aanbevolen. De poorten en protocollen ("Services") moeten worden teruggebracht tot het minimum mogelijk waarmee de toepassing opnieuw te verminderen van de kwetsbaarheid voor aanvallen via deze grens.
> 
> 

<br />

> [!TIP]
> Hoewel met deze regel een verwijzing naar een expliciete-doel wordt gebruikt bevat, moet een consistent benadering worden gebruikt in de gehele configuratie van de firewall. Het verdient aanbeveling dat het benoemde netwerkobject in de gehele worden gebruikt voor de leesbaarheid te vergemakkelijken en ondersteuningsmogelijkheden. De expliciete-dest hier alleen om weer te geven van een andere referentiemethode wordt gebruikt en wordt in het algemeen niet aanbevolen (met name voor complexe configuraties).
> 
> 

* **Uitgaand naar Internetregel**: Deze regel Pass wordt verkeer van elk Bronnetwerk moeten worden doorgegeven aan de geselecteerde bestemming netwerken toestaan. Deze regel is een standaardregel gewoonlijk al op de Barracuda NextGen firewall, maar heeft een uitgeschakelde status. Met de rechtermuisknop op deze regel hebben toegang tot de opdracht activeren regel. De regel die hier worden weergegeven is als u wilt toevoegen van de twee lokale subnetten die zijn gemaakt als verwijzingen in de sectie vereisten van dit document aan het bronkenmerk van deze regel gewijzigd.
  
    ![Uitgaande-firewallregel][14]
* **Een DNS-regel**: Deze regel Pass kunnen alleen DNS (poort 53)-verkeer moeten worden doorgegeven aan de DNS-server. Deze regel kunnen voor deze omgeving die meeste verkeer van de front-end naar de back-end wordt geblokkeerd, specifiek DNS.
  
    ![DNS-firewallregel][15]
  
    **Opmerking**: In dit scherm is de verbindingsmethode shot opgenomen. Omdat met deze regel voor interne IP-adres op het interne IP-adres verkeer is, wordt geen NATing is vereist, wordt dit de verbindingsmethode is ingesteld op "Geen SNAT" voor deze regel Pass.
* **Subnet naar Subnet regel**: Deze regel Pass is een standaardregel die is geactiveerd en gewijzigd, zodat elke server in de back-end-subnet verbinding maken met elke server in de front-end-subnet. Deze regel is alle intern verkeer, zodat de verbindingsmethode kan worden ingesteld op Nee SNAT.
  
    ![Intra-VNet-firewallregel][16]
  
    **Opmerking**: Het selectievakje bidirectionele niet is ingeschakeld (en er wordt gecontroleerd in de meeste regels), dit is van belang voor deze regel wordt deze regel 'eenrichtings', een verbinding kan worden gestart vanuit de back-end-subnet op de front-endnetwerk, maar niet andersom. Als dit selectievakje is ingeschakeld, zou deze regel bidirectioneel verkeer, die uit onze logisch diagram niet gewenst is inschakelen.
* **Alle Verkeersregel voor weigeren**: Dit moet altijd de laatste regel (voor wat betreft prioriteit) en als zodanig als een verkeer stroomt zodat deze overeenkomt met een van de vorige regels die wordt deze verwijderd door deze regel is mislukt. Dit is een standaardregel en zijn meestal geactiveerd, worden geen wijzigingen zijn aangebracht in het algemeen nodig. 
  
    ![Regel voor weigeren van Firewall][17]

> [!IMPORTANT]
> Nadat u alle bovenstaande regels hebt gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om te controleren of verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in de volgorde waarin die ze moeten worden weergegeven in het raster Main regels in de Barracuda-Management-Client door te sturen.
> 
> 

## <a name="rule-activation"></a>Activering van de regel
Met het ruleset is gewijzigd, zodat de specificatie van het diagram logica, moet de regelset worden geüpload naar de firewall en wordt geactiveerd.

![Activering van Firewall-regel][18]

In de rechterbovenhoek van de management-client zijn een cluster van de knoppen. Klik op de knop 'Wijzigingen verzenden' voor het verzenden van de gewijzigde regels aan de firewall en klik vervolgens op de knop 'Activeren'.

Met de activering van de regelset van de firewall zijn in dit voorbeeld omgeving build is voltooid.

## <a name="traffic-scenarios"></a>Scenario's voor verkeer
> [!IMPORTANT]
> Een sleutel takeway is om te weten dat **alle** verkeer wordt geleverd via de firewall. Dus met extern bureaublad op de server IIS01, zelfs al is het in de Front-End-Cloudservice en op de Front-End-subnet, voor toegang tot deze server we RDP moet aan de firewall op poort 8014 en vervolgens de firewall voor het routeren van de RDP-aanvraag intern met de RDP-Por IIS01 toestaan t. De knop 'Verbinden' van de Azure portal werkt niet omdat er geen directe RDP-pad naar IIS01 (zo lang de portal kunt zien). Dit betekent dat alle verbindingen vanuit het internet worden naar de Service en een poort, bijvoorbeeld secscv001.cloudapp.net:xxxx.
> 
> 

Voor deze scenario's moeten de volgende firewallregels zijn voldaan:

1. Firewallbeheer
2. RDP-verbinding IIS01
3. RDP-verbinding DNS01
4. RDP-verbinding AppVM01
5. RDP-verbinding AppVM02
6. Verkeer naar de Web-App
7. App-verkeer naar AppVM01
8. Uitgaand naar Internet
9. Frontend naar DNS01
10. Intra-Subnet-verkeer (back-end aan de front-end alleen)
11. Alles negeren

De werkelijke firewall ruleset waarschijnlijk veel andere regels naast deze hebben, de regels op elke opgegeven firewall heeft ook verschillende prioriteitsnummers dan blobs die hier worden vermeld. Deze lijst en de bijbehorende nummers zijn voor relevantie tussen alleen deze elf regels en de relatieve prioriteit onder deze. Met andere woorden; op de werkelijke firewall kan de "RDP naar IIS01" regel getal 5, maar als deze zich onder de regel 'Firewall Management' en boven de regel "RDP-DNS01" het zou uitlijnen met de bedoeling van deze lijst. De lijst met helpt ook bij de onderstaande scenario's zodat beknoptheid; bijv. "FW regel 9 (DNS)". Ook voor kort te houden, de vier regels voor RDP gezamenlijk genoemd, 'de regels voor RDP' wanneer het verkeer scenario heeft geen betrekking op RDP.

Ook intrekken dat Netwerkbeveiligingsgroepen in-place worden voor inkomend internetverkeer op de front-end en back-end-subnetten.

#### <a name="allowed-internet-to-web-server"></a>(Toegestaan) Internet-webserver
1. Internet gebruiker aanvragen HTTP-pagina van SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service-passen verkeer via de open-eindpunt op poort 80 voor firewall-interface op 10.0.0.4:80
3. Er is geen NSG die is toegewezen aan Security subnet, dus system NSG-regels verkeer firewall toestaan
4. Verkeer komt binnen via het interne IP-adres van de firewall (10.0.1.4)
5. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW-regels (regels voor RDP) 2-5 niet van toepassing, verplaatsen naar de volgende regel
   3. FW regel 6 (App: Web) is van toepassing, verkeer is toegestaan, firewall NAT's worden 10.0.1.4 (IIS01)
6. Het subnet Frontend begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT wordt door de firewall, het adres van de bronserver is dus nu de firewall die zich in het subnet van de beveiliging en "local" verkeer worden gezien door de front-end-subnet-NSG en dus is toegestaan), verplaatsen naar de volgende regel
   2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
7. IIS01 luistert voor webverkeer te genereren, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
8. Pogingen tot IIS01 initieert een FTP-sessie naar AppVM01 op back-end-subnet
9. De UDR-route voor subnet Frontend maakt de firewall van de volgende hop
10. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
11. Firewall begint regelverwerking:
    1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
    2. FW-regel 2-5 (regels voor RDP) niet van toepassing, verplaatsen naar de volgende regel
    3. FW regel 6 (App: Web) niet van toepassing, verplaatsen naar de volgende regel
    4. FW regel 7 (App: Back-end) is van toepassing, verkeer is toegestaan, firewall doorgestuurd verkeer naar 10.0.2.5 (AppVM01)
12. De back-end-subnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
13. AppVM01 ontvangt de aanvraag en de sessie start en reageert
14. De UDR-route op back-end-subnet wordt de firewall van de volgende hop
15. Omdat er zijn geen uitgaande NSG-regels op de back-end-subnet dat het antwoord is toegestaan
16. Omdat dit verkeer voor een vastgestelde sessie retourneert het antwoord terug naar de webserver (IIS01) wordt doorgegeven door de firewall
17. Front-endsubnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
18. De IIS-server-antwoord is ontvangen, is de transactie met AppVM01 voltooid en daarna voltooit het bouwen van het HTTP-antwoord, is deze HTTP-antwoord verzonden naar de aanvrager
19. Omdat er zijn geen uitgaande NSG-regels op het subnet Frontend dat het antwoord is toegestaan
20. Het HTTP-antwoord komt binnen via de firewall, en omdat dit de reactie op een vastgestelde NAT-sessie is door de firewall wordt geaccepteerd
21. De firewall vervolgens het antwoord terug naar de Internet-gebruiker wordt omgeleid
22. Omdat er geen uitgaande ontvangt NSG-regels of UDR-hops op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker de webpagina die is aangevraagd.

#### <a name="allowed-internet-rdp-to-backend"></a>(Toegestaan) Internet RDP naar back-end
1. De serverbeheerder op internet vraagt RDP-sessie naar AppVM01 via SecSvc001.CloudApp.Net:8025, waarbij 8025 het poortnummer van de gebruiker die is toegewezen voor de firewallregel "RDP-AppVM01 is"
2. De cloudservice wordt doorgegeven verkeer via het open-eindpunt op poort 8025 firewall-interface op 10.0.0.4:8025
3. Er is geen NSG die is toegewezen aan Security subnet, dus system NSG-regels verkeer firewall toestaan
4. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW regel 2 (RDP IIS) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regel 3 (RDP DNS01) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 4 (RDP AppVM01) is van toepassing, verkeer is toegestaan, firewall NAT's worden 10.0.2.5:3386 (RDP-poort op AppVM01)
5. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) is niet van toepassing (dit verkeer is NAT wordt door de firewall, het adres van de bronserver is dus nu de firewall die zich in het subnet van de beveiliging en "local" verkeer worden gezien door de back-end-subnet-NSG en dus is toegestaan), verplaatsen naar de volgende regel
   2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
6. AppVM01 luistert voor RDP-verkeer en reageert
7. Standaardregels toepassen met geen uitgaande NSG-regels, en terugkerend verkeer is toegestaan
8. UDR-routes voor uitgaand verkeer naar de firewall als de volgende hop
9. Omdat dit verkeer voor een vastgestelde sessie retourneert het antwoord terug naar de gebruiker internet wordt doorgegeven door de firewall
10. RDP-sessie is ingeschakeld
11. AppVM01 vraagt om wachtwoord van de naam van gebruiker

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server-DNS-zoekactie op DNS-server
1. Web Server, IIS01, een gegevensfeed op www.data.gov behoeften, maar moet het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op de back-end-subnet) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. UDR-routes voor uitgaand verkeer naar de firewall als de volgende hop
4. Er is geen uitgaande NSG-regels zijn gebonden aan het Frontend-subnet, is verkeer toegestaan
5. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW-regel 2-5 (regels voor RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) is van toepassing, verkeer is toegestaan, firewall doorgestuurd verkeer naar 10.0.2.4 (DNS01)
6. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
   2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
7. DNS-server ontvangt de aanvraag
8. DNS-server beschikt niet over het adres in de cache opgeslagen en wordt u gevraagd een basis-DNS-server op het internet
9. UDR-routes voor uitgaand verkeer naar de firewall als de volgende hop
10. Er is geen uitgaande NSG-regels op back-end-subnet, verkeer is toegestaan.
11. Firewall begint regelverwerking:
    1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
    2. FW-regel 2-5 (regels voor RDP) niet van toepassing, verplaatsen naar de volgende regel
    3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
    4. FW regel 8 (met Internet) is van toepassing, verkeer is toegestaan, sessie is SNAT uit naar basis-DNS-server op het Internet
12. Internet-DNS-server reageert, omdat deze sessie is geïnitieerd door de firewall, het antwoord wordt geaccepteerd door de firewall
13. Omdat deze een vastgestelde sessie, stuurt de firewall het antwoord op de initiërende DNS01-server
14. De back-end-subnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
15. De DNS-server ontvangt en plaatst het antwoord en klik vervolgens op de eerste aanvraag terug naar IIS01 reageert
16. De UDR-route op back endsubnet wordt de firewall van de volgende hop
17. Er zijn geen uitgaande NSG-regels bestaan in de back-end-subnet, is verkeer toegestaan
18. Dit is een vastgestelde sessie op de firewall, het antwoord wordt doorgestuurd door de firewall terug naar de IIS-server
19. Front-endsubnet begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan
20. IIS01-antwoord is ontvangen van DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Toegestaan) Back-endserver met front-end-server
1. Een beheerder is aangemeld bij AppVM02 via RDP vraagt een bestand rechtstreeks vanaf de server IIS01 via windows Verkenner
2. De UDR-route op back-end-subnet wordt de firewall van de volgende hop
3. Omdat er zijn geen uitgaande NSG-regels op de back-end-subnet dat het antwoord is toegestaan
4. Firewall begint regelverwerking:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW-regel 2-5 (regels voor RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. FW regels 6 en 7 (App-regels) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   6. FW regel 10 (Intra-Subnet) is van toepassing, verkeer is toegestaan, firewall verkeer doorgegeven aan 10.0.1.4 (IIS01)
5. Front-endsubnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
   2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
6. Ervan uitgaande dat het juiste verificatie en autorisatie, IIS01 worden aanvragen geaccepteerd en reageert
7. De UDR-route voor subnet Frontend maakt de firewall van de volgende hop
8. Omdat er zijn geen uitgaande NSG-regels op het subnet Frontend dat het antwoord is toegestaan
9. Als dit een bestaande sessie op de firewall is deze reactie is toegestaan en de firewall retourneert het antwoord op AppVM02
10. Back-end-subnet begint verwerking van inkomende regel:
    1. NSG-regel 1 (Internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
    2. Standaard-NSG-regels dat subnet naar subnet verkeer, verkeer is toegestaan, stopt u verwerking van NSG-regels
11. AppVM02-antwoord is ontvangen

#### <a name="denied-internet-direct-to-web-server"></a>(Geweigerd) Rechtstreeks naar de webserver van Internet
1. Internet-gebruiker probeert te krijgen van de webserver, IIS01, via de service FrontEnd001.CloudApp.Net
2. Aangezien er geen eindpunten zijn geopend voor HTTP-verkeer, deze zou worden niet doorgegeven via de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een of andere reden zijn, blokkeren de NSG (Internet blokkeren) op het subnet Frontend dit verkeer
4. Tot slot de Frontend-subnet UDR-route wordt al het uitgaande verkeer verzenden vanuit IIS01 aan de firewall als de volgende hop, en de firewall zou dit zien als asymmetrische verkeer en het uitgaande antwoord er ten minste drie onafhankelijke defensieve tussen zijn Thus verwijderen het internet en IIS01 via de service in de cloud te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-backend-server"></a>(Geweigerd) Internet naar back-endserver
1. Internet-gebruiker probeert te krijgen van een bestand op AppVM01 via de service BackEnd001.CloudApp.Net
2. Omdat er geen eindpunten openen voor bestandsshare zijn, dit zou de Cloudservice niet doorgeven en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een of andere reden zijn, blokkeren de NSG (Internet blokkeren) dit verkeer
4. Tot slot de UDR-route wordt al het uitgaande verkeer verzenden vanuit AppVM01 aan de firewall als de volgende hop, en de firewall zou zien dit als asymmetrische verkeer en verwijderen van het uitgaande antwoord, dus er zijn ten minste drie onafhankelijke defensieve tussen internet en AppVM01 via de service in de cloud te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-frontend-server-to-backend-server"></a>(Geweigerd) Front-end-server om back-endserver
1. Wordt ervan uitgegaan dat IIS01 is geïnfecteerd en schadelijke code het opvragen van de back-end-subnet-servers wordt uitgevoerd.
2. De UDR-route van Frontend-subnet wilt verzenden al het uitgaande verkeer vanuit IIS01 aan de firewall als de volgende hop. Dit is niet iets dat kan worden gewijzigd door de aangetaste virtuele machine.
3. De firewall zou het verkeer verwerken als de aanvraag is AppVM01 of de DNS-server voor DNS-zoekacties die verkeer kan mogelijk worden toegestaan door de firewall (als gevolg van FW regels 7 en 9). Al het andere verkeer zouden worden geblokkeerd door FW regel 11 (Alles weigeren).
4. Als u geavanceerde detectie van bedreigingen op de firewall is ingeschakeld (Zie de documentatie van de leverancier voor uw specifieke netwerkapparaat advanced threat mogelijkheden, die niet wordt gedekt in dit document), zelfs het verkeer dat zou worden toegestaan door de regels voor doorsturen van basic beschreven in dit document kan worden voorkomen als het verkeer opgenomen handtekeningen van bekende of patronen die markering van een regel voor geavanceerde bedreigingen.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Geweigerd) Internet-DNS-zoekactie op DNS-server
1. Internet-gebruiker probeert voor het opzoeken van een interne DNS-record op DNS01 via BackEnd001.CloudApp.Net service 
2. Omdat er geen eindpunten openen voor DNS-verkeer zijn, deze zou worden niet doorgegeven via de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een of andere reden zijn, blokkeren de NSG-regel (Internet blokkeren) op het subnet Frontend dit verkeer
4. Tot slot de UDR-route van back-end-subnet wordt al het uitgaande verkeer verzenden vanuit DNS01 aan de firewall als de volgende hop, en de firewall zou zien dit als asymmetrische verkeer en verwijderen van het uitgaande antwoord Thus er ten minste drie onafhankelijke defensieve tussen zijn de Internet en DNS01 via de service in de cloud te voorkomen dat onbevoegde/ongeoorloofde toegang.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Geweigerd) SQL-toegang via de Firewall voor Internet
1. Internetgebruiker opvraagt SQL gegevens uit SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Omdat er geen eindpunten openen voor SQL zijn, dit zou niet doorgeven voor de Cloudservice en de firewall wouldn't bereiken
3. Als SQL-eindpunten geopend voor een of andere reden zijn, zou de firewall regelverwerking beginnen:
   1. FW regel 1 (FW Mgmt) niet van toepassing, verplaatsen naar de volgende regel
   2. FW-regels (regels voor RDP) 2-5 niet van toepassing, verplaatsen naar de volgende regel
   3. FW regel 6 en 7 (regels van toepassing) niet van toepassing, verplaatsen naar de volgende regel
   4. FW regel 8 (met Internet) niet van toepassing, verplaatsen naar de volgende regel
   5. FW regel 9 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   6. FW regel 10 (Intra-Subnet) niet van toepassing, verplaatsen naar de volgende regel
   7. FW regel 11 (Alles weigeren) is van toepassing, het verkeer wordt geblokkeerd, stoppen, regelverwerking

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en de netwerkconfiguratie
Het volledige Script opslaan in een PowerShell-scriptbestand. Sla de netwerk-configuratie in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen zo nodig wijzigen. Voer het script uit en volg de bovenstaande voor setup instructies van Firewall-regel.

#### <a name="full-script"></a>Volledige Script
Met dit script wordt op basis van de gebruiker gedefinieerde variabelen:

1. Verbinding maken met een Azure-abonnement
2. Een nieuw opslagaccount maken
3. Een nieuw VNet en drie subnetten zoals gedefinieerd in het netwerk-configuratiebestand maken
4. Bouw vijf virtuele machines; firewall voor 1 en 4 WindowsServer-VM 's
5. Configureren van UDR met inbegrip van:
   1. Het maken van twee nieuwe routetabellen
   2. Routes toevoegen aan de tabellen
   3. Tabellen aan de juiste subnetten binden
6. Doorsturen via IP op de NVA inschakelen
7. Configureren van NSG met inbegrip van:
   1. Het maken van een NSG
   2. Een regel toevoegen
   3. De NSG-binding aan de juiste subnetten

Dit PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of de server.

> [!IMPORTANT]
> Wanneer dit script wordt uitgevoerd, zijn er mogelijk andere informatieve berichten die de pop-in PowerShell of waarschuwingen worden gegenereerd. Alleen foutberichten worden weergegeven in het rood zijn oorzaak zouden kunnen gaan maken.
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
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
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
Dit xml-bestand opslaan met bijgewerkte locatie en de koppeling naar dit bestand naar de variabele $NetworkConfigFile in het bovenstaande script toevoegen.

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

#### <a name="sample-application-scripts"></a>Voorbeeldscripts voor toepassing
Als u een voorbeeldtoepassing voor deze en andere voorbeelden DMZ installeren wilt, is een opgegeven op de volgende koppeling: [Voorbeeldscript voor toepassing][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirectionele DMZ met NVA, NSG en UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische weergave van de Firewall-regels"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Maken van een Object front-end"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Een DNS-Server-Object maken"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Exemplaar van standaard RDP-regel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Omleidings-toepassingspictogram"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Bestemming NAT-pictogram"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass-pictogram"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewallregel voor beheer"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP-firewallregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web-firewallregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewallregel AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Uitgaande-firewallregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-firewallregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Intra-VNet-firewallregel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Regel voor weigeren van Firewall"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Activering van Firewall-regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
