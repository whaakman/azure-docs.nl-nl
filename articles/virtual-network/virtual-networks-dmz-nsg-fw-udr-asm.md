---
title: Voorbeeld van de perimeter network – netwerken beveiligen met perimeternetwerk die bestaan uit een firewall, UDR en nsg's | Microsoft Docs
description: Bouw een perimeternetwerk (ook wel een DMZ genoemd) met een firewall, de gebruiker gedefinieerde routering (UDR) en de netwerkbeveiligingsgroepen (nsg's).
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
ms.openlocfilehash: 668862714b416bd89d3b5f82caf8b0305fccae54
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60363009"
---
# <a name="example-3-build-a-perimeter-network-to-protect-networks-with-a-firewall-udr-and-nsgs"></a>Voorbeeld 3: Een perimeternetwerk ter bescherming van netwerken met een firewall, UDR en nsg's maken

[Ga terug naar de grens Best Practices pagina][HOME]

In dit voorbeeld maakt u een perimeternetwerk (ook bekend als een DMZ, gedemilitariseerde zone en gescreend subnet genoemd). Het voorbeeld implementeert een firewall, vier Windows-servers (UDR) gebruiker gedefinieerde routering, doorsturen via IP en netwerkbeveiligingsgroepen (nsg's). Dit artikel begeleidt u bij elk van de relevante opdrachten voor een beter begrip van elke stap. Het verkeer scenario gedeelte ook wordt uitgelegd in de diepte hoe verkeer wordt uitgevoerd in de defensieve in het perimeternetwerk. Ten slotte bevat de sectie Verwijzingen de code en instructies voor het bouwen van deze omgeving zodat u kunt testen en Experimenteer met verschillende scenario's.

![Bidirectionele perimeternetwerk met NVA, NSG en UDR][1]

## <a name="environment-setup"></a>Omgeving instellen

In dit voorbeeld wordt een abonnement met de volgende onderdelen:

* Drie cloudservices: SecSvc001, FrontEnd001, and BackEnd001
* Een virtueel netwerk (CorpNetwork) met drie subnetten: SecNet, FrontEnd, and BackEnd
* Een virtueel netwerkapparaat: een firewall die is verbonden met het subnet SecNet
* Een Windows-server die staat voor een web application server: IIS01
* Twee Windows-servers die staan voor back-end-toepassingsservers: AppVM01, AppVM02
* Een Windows-server die staat voor een DNS-server: DNS01

De [verwijst naar de sectie](#references) bevat een PowerShell-script dat de meeste van de omgeving die hier worden beschreven. In dit artikel geeft geen anders gedetailleerde instructies voor het bouwen van de virtuele machines (VM's) en de virtuele netwerken.

De omgeving bouwen:

1. Het netwerk config XML-bestand zijn opgenomen in de [in de sectie over](#references). U moet bijwerken met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario.
1. Werk de Gebruikersvariabelen voor de in het volledige script moet overeenkomen met uw specifieke omgeving (bijvoorbeeld abonnementen, servicenamen, enzovoort).
1. Voer het script in PowerShell.

> [!NOTE]
> De regio die is opgegeven in het PowerShell-script moet overeenkomen met de regio die is opgegeven in het netwerk van het XML-configuratiebestand.

Nadat het script is uitgevoerd, kunt u de volgende stappen:

1. Instellen van de firewall-regels. Zie de [firewall-regels](#firewall-rules) sectie.
1. Gebruik eventueel de twee scripts in de sectie referenties voor het instellen van een web-App op de webserver en app-server om toe te staan van deze configuratie DMZ testen.

## <a name="user-defined-routing"></a>Gebruiker gedefinieerde routering

Standaard worden de volgende systeemroutes gedefinieerd als:

    Effective routes :
     Address Prefix    Next hop type    Next hop IP address Status   Source
     --------------    -------------    ------------------- ------   ------
     {10.0.0.0/16}     VNETLocal                            Active   Default
     {0.0.0.0/0}       internet                             Active   Default
     {10.0.0.0/8}      Null                                 Active   Default
     {100.64.0.0/10}   Null                                 Active   Default
     {172.16.0.0/12}   Null                                 Active   Default
     {192.168.0.0/16}  Null                                 Active   Default

VNETLocal is altijd de prefix(en) opgegeven adres voor dat specifieke virtuele netwerk. Bijvoorbeeld, verandert deze vanaf het virtuele netwerk naar virtueel netwerk, afhankelijk van hoe elk specifieke virtueel netwerk is gedefinieerd. De resterende systeemroutes statisch zijn en de standaardinstellingen van zoals wordt weergegeven.

Als voor de prioriteit, worden via het langste voorvoegsel overeenkomende LPM (longest prefix)-methode routes verwerkt. Dus de meest specifieke route in de tabel is van toepassing op een bepaalde bestemming-adres.

Daarom verkeer dat is bedoeld voor een server, zoals DNS01 (10.0.2.4) op het lokale netwerk (10.0.0.0/16) vanwege de route 10.0.0.0/16 via het virtuele netwerk wordt doorgestuurd.  Voor 10.0.2.4 is de route 10.0.0.0/16 het meest specifieke route. Deze regel geldt zelfs als de 10.0.0.0/8 en 0.0.0.0/0 kunnen ook van toepassing zijn. Ze zijn echter minder specifiek, zodat ze niet van invloed zijn op dit verkeer. Verkeer naar 10.0.2.4 is het lokale virtuele netwerk als de volgende hop, zodat wordt doorgestuurd naar de bestemming.

Bijvoorbeeld, is de route 10.0.0.0/16 niet van toepassing op het verkeer dat bestemd is voor 10.1.1.1. De 10.0.0.0/8 system-route is de meest specifiek, zodat het verkeer wordt verwijderd of 'black gaan' omdat de volgende hop Null is.

Als de bestemming niet van toepassing op een van de Null-voorvoegsels of de voorvoegsels VNETLocal, volgt verkeer specifieke routes (0.0.0.0/0). Deze wordt doorgestuurd uit met het internet als de volgende hop en naar internet-edge van Azure.

Als er twee identieke voorvoegsels in de routetabel, wordt de volgorde van voorkeur is gebaseerd op het bronkenmerk van de route:

1. VirtualAppliance: Een door de gebruiker gedefinieerde Route handmatig worden toegevoegd aan de tabel.
1. VPNGateway: Een dynamische Route (BGP gebruikt in combinatie met hybride netwerken) die zijn toegevoegd door een dynamische netwerkprotocol. Deze routes kunnen na verloop van tijd worden gewijzigd als de dynamische-protocol wordt automatisch in gekoppelde netwerk aangepast.
1. Standaard: De systeemroutes, het lokale virtuele netwerk en de statische-vermeldingen zoals weergegeven in de bovenstaande routetabel.

> [!NOTE]
> U kunt nu de gebruiker gedefinieerde Routering met ExpressRoute en VPN-Gateways gebruiken om af te dwingen van binnenkomende en uitgaande cross-premises verkeer worden doorgestuurd naar een virtueel netwerkapparaat (NVA).

### <a name="create-local-routes"></a>Lokale routes maken

In dit voorbeeld maakt gebruik van twee routeringstabellen, één voor de front-end en back-end-subnetten. Elke tabel is geladen met statische routes die geschikt is voor het opgegeven subnet. In dit voorbeeld heeft elke tabel drie routes:

1. Lokaal subnetverkeer met geen volgende hop gedefinieerd. Deze route kan lokaal subnetverkeer om over te slaan van de firewall.
2. Verkeer in virtuele netwerken met de volgende hop gedefinieerd als een firewall. Deze route overschrijft de standaardregel waarmee lokale verkeer in virtuele netwerken te routeren rechtstreeks.
3. Alle resterende verkeer (0/0) met de volgende hop gedefinieerd als de firewall.

Nadat de routeringstabel zijn gemaakt, zijn ze gekoppeld aan hun subnetten. De front-end-subnet-routeringstabel er moet uitzien:

    Effective routes :
     Address Prefix    Next hop type       Next hop IP address  Status   Source
     --------------    ------------------  -------------------  ------   ------
     {10.0.1.0/24}     VNETLocal                                Active
     {10.0.0.0/16}     VirtualAppliance    10.0.0.4             Active
     {0.0.0.0/0}       VirtualAppliance    10.0.0.4             Active

In dit voorbeeld gebruikt de volgende opdrachten voor het bouwen van de routetabel, een door de gebruiker gedefinieerde route toevoegen en vervolgens de routetabel binden aan een subnet. Items die met beginnen `$`, zoals `$BESubnet`, zijn van de gebruiker gedefinieerde variabelen in het script in de sectie documentatie.

1. Maak eerst de basis-routeringstabel. Het volgende codefragment maakt u de tabel voor de back-end-subnet. Het volledige script maakt ook een bijbehorende tabel voor de front-end-subnet.

   ```powershell
   New-AzureRouteTable -Name $BERouteTableName `
       -Location $DeploymentLocation `
       -Label "Route table for $BESubnet subnet"
   ```

1. Nadat u de routetabel hebt gemaakt, kunt u specifieke, door de gebruiker gedefinieerde routes toevoegen. Het volgende codefragment geeft aan dat alle verkeer (0.0.0.0/0) wordt doorgestuurd via het virtuele apparaat. Een variabele `$VMIP[0]` wordt gebruikt om door te geven in het IP-adres toegewezen wanneer het virtuele apparaat eerder in het script is gemaakt. Het volledige script maakt ook een overeenkomende regel in de front-end-tabel.

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. Het vorige item in de route overschrijft de standaardroute voor '0.0.0.0/0', maar nog steeds mogelijk in de standaardregel 10.0.0.0/16 verkeer binnen het virtuele netwerk voor het routeren van rechtstreeks naar de bestemming en niet naar het virtuele netwerkapparaat. U lost dit probleem, moet u de volgende regel toevoegen:

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. Op dit moment hebt u een keuze te maken. De vorige twee regels wordt al het verkeer gerouteerd naar de firewall voor evaluatie, met inbegrip van verkeer binnen één subnet. U kunt dit gedrag. Als u dit niet doet, maar kunt u verkeer binnen een subnet voor het routeren van lokaal zonder tussenkomst van de firewall toestaan. Toevoegen van een derde, specifieke regel die rechtstreeks een adres dat is bestemd voor het lokale subnet (NextHopType VNETLocal =).

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
           -NextHopType VNETLocal
   ```

1. Nadat de routeringstabel is gemaakt en ingevuld met de gebruiker gedefinieerde routes, moet u ten slotte de tabel binden aan een subnet. Het volgende codefragment verbindt met de tabel voor de back-end-subnet. De front-end-routetabel het volledige script ook gebonden aan de front-end-subnet.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
       -SubnetName $BESubnet `
       -RouteTableName $BERouteTableName
   ```

## <a name="ip-forwarding"></a>Doorsturen via IP

Doorsturen via IP is een functie companion UDR. Deze instelling op een virtueel apparaat kan worden ontvangen van verkeer dat niet is geadresseerd aan het apparaat en stuur deze vervolgens dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld, als het verkeer van AppVM01 dient een aanvraag naar de server DNS01, UDR het verkeer routeert naar de firewall. Met het IP-doorsturen is ingeschakeld, wordt het verkeer met het doel DNS01 (10.0.2.4) geaccepteerd door de firewall-apparaat (10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder doorsturen via IP op de firewall is ingeschakeld, wordt verkeer niet geaccepteerd door het toestel zelfs als de routetabel de firewall als de volgende hop is.

> [!IMPORTANT]
> Vergeet niet om in te schakelen doorsturen via IP in combinatie met de gebruiker gedefinieerde routering.

Doorsturen via IP kan worden ingeschakeld met slechts één opdracht tijdens de aanmaak van virtuele machine. Roept u de VM-exemplaar waarop uw virtueel firewallapparaat en doorsturen via IP van inschakelen. Houd er rekening mee dat de artikelen in het rood die beginnen met `$`, bijvoorbeeld `$VMName[0]`, zijn van de gebruiker gedefinieerde variabelen in het script in de sectie Verwijzingen van dit document. De op nul tussen vierkante haken, `[0]`, Hiermee geeft u de eerste virtuele machine in de matrix van virtuele machines. Voor de voorbeeldscript zonder aanpassingen werken, moet de eerste virtuele machine (VM 0) de firewall. In het volledige script, wordt het relevante codefragment gegroepeerd met de UDR-opdrachten aan het einde.

```powershell
Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
    Set-AzureIPForwarding -Enable
```

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

In dit voorbeeld wordt een netwerkbeveiligingsgroep (NSG) bouwen en vervolgens laden met een enkele regel. Het voorbeeld wordt vervolgens de NSG wordt alleen aan de front-end en back-end-subnetten (niet de SecNet). De regel die u in de NSG laden is als volgt:

* Al het verkeer (alle poorten) van het internet naar het hele virtuele netwerk (alle subnetten) is geweigerd

Hoewel nsg's in dit voorbeeld worden gebruikt, is het belangrijkste doel als een secundaire laag van beveiliging tegen handmatige onjuiste configuratie. Wilt u alle binnenkomend verkeer van internet aan de front-end en back-end subnetten blokkeren. Verkeer moet alleen stromen via het subnet SecNet aan de firewall, waarna alleen al het verkeer moet ophalen gerouteerd u aan bij de front-end en back-end-subnetten. Bovendien routeren de UDR-regels van al het verkeer die geschikt is voor de front-end en back-end-subnetten aan de firewall. De firewall krijgt te zien als een asymmetrische stroom en het uitgaande verkeer komt.

Drie beveiligingslagen beveiligen de front-end en back-end-subnetten:

1. Er is geen open eindpunten op FrontEnd001 en BackEnd001 cloudservices
1. Nsg's weigeren verkeer vanaf internet
1. De firewall blokkeert asymmetrische verkeer

Een interessante over de NSG in dit voorbeeld is dat deze slechts één regel bevat, hieronder weergegeven. Deze regel weigert internetverkeer naar het hele virtuele netwerk, met inbegrip van het subnet van de beveiliging.

```powershell
Get-AzureNetworkSecurityGroup -Name $NSGName | `
    Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
    from the internet" `
    -Type Inbound -Priority 100 -Action Deny `
    -SourceAddressPrefix INTERNET -SourcePortRange '*' `
    -DestinationAddressPrefix VIRTUAL_NETWORK `
    -DestinationPortRange '*' `
    -Protocol *
```

Omdat de NSG alleen is gebonden aan de front-end en back-end-subnetten, wordt de regel wordt niet toegepast voor het netwerkverkeer dat binnenkomt op het subnet van de beveiliging. Als gevolg hiervan stroomt verkeer met het subnet van de beveiliging.

```powershell
Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $FESubnet -VirtualNetworkName $VNetName

Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $BESubnet -VirtualNetworkName $VNetName
```

## <a name="firewall-rules"></a>Firewall-regels

Op de firewall, moet u maken van regels voor doorsturen. Omdat de firewall blokkeert of alle binnenkomende, uitgaande en intra-virtual-network-verkeer wordt doorgestuurd, moet u veel firewall-regels. Bovendien is de firewall voor het verwerken van al het binnenkomende verkeer naar de beveiligingsservice openbare IP-adres (op verschillende poorten). Om te voorkomen later dubbel, de aanbevolen procedures worden gevolgd door de diagrammen van de logische stromen voordat het instellen van de subnetten en firewall-regels. De volgende afbeelding is een logische weergave van de firewall-regels voor dit voorbeeld:

![Logische weergave van de Firewall-regels][2]

> [!NOTE]
> Beheerpoorten variëren, afhankelijk van het virtuele netwerkapparaat. Dit voorbeeld toont een Barracuda NextGen Firewall dat gebruikmaakt van poort 22, 801 en 807. Raadpleeg de documentatie van de leverancier apparaat voor de exacte poorten voor het beheren van het apparaat.

### <a name="logical-rule-description"></a>Beschrijving van de logische regel

In het bovenstaande logische diagram niet het subnet van de beveiliging niet weergeven omdat de firewall de enige bron op dat subnet is. Dit diagram ziet u de firewall-regels, hoe ze logisch toestaan of weigeren verkeersstromen, maar niet de werkelijke pad geleid. De externe poorten geselecteerd voor het remote desktop (RDP)-protocolverkeer zijn ook hoger variabele poorten (8014 – 8026) gekozen voor de leesbaarheid te vergemakkelijken uitgelijnd met de laatste twee octetten van het lokale IP-adressen. Adres van de lokale server 10.0.1.4 is bijvoorbeeld gekoppeld aan de externe poort 8014. U kunt echter hogere niet-conflicterende poorten gebruiken.

In dit voorbeeld moet u de volgende typen regels:

* Externe regels voor binnenkomend verkeer:
  1. Firewallregel management: staat verkeer moet worden doorgegeven aan de beheerpoorten van het virtuele netwerkapparaat.
  2. RDP-regels voor elke windows-server: beheer van de afzonderlijke servers via RDP toegestaan.  Afhankelijk van de mogelijkheden van uw virtueel netwerkapparaat, is het mogelijk dat u de regels bundelen in één.
  3. Regels voor netwerkverkeer van toepassing: één voor webverkeer-front-end en één voor back-end-verkeer (bijvoorbeeld webserver als de gegevenslaag). De configuratie van deze regels is afhankelijk van de netwerkarchitectuur en verkeer stromen.

     * De eerste regel staat de feitelijke toepassingsverkeer om de toepassingsserver te bereiken. In tegenstelling tot de regels voor beveiliging, beheer, enzovoort kunnen regels voor application externe gebruikers of -services tot de toepassingen. In dit voorbeeld heeft één webserver op poort 80, waardoor één toepassing firewallregel omleiden van verkeer dat bestemd is voor een extern IP-adres aan in plaats daarvan wordt doorgestuurd naar het interne IP-adres van de webserver. De sessie omgeleide verkeer opnieuw door NAT wordt toegewezen aan de interne server.
     * De tweede regel van toepassing verkeer is de back-end-regel waarmee de webserver een willekeurige poort gebruiken voor het routeren van verkeer naar de server AppVM01, maar niet aan de server AppVM02.

* Interne regels voor intra-virtual-network-verkeer:
  1. Uitgaand naar internetregel: verkeer van elk netwerk moeten worden doorgegeven aan de geselecteerde netwerken toestaat. Deze regel is meestal een standaard op de firewall, maar een uitgeschakelde status. Deze regel voor dit voorbeeld inschakelen.
  2. Een DNS-regel: staat alleen verkeer van DNS (poort 53) moeten worden doorgegeven aan de DNS-server. De meeste verkeer van de front-end naar de back-end is geblokkeerd voor deze omgeving. Deze regel kunnen specifiek DNS vanuit een lokale subnet.
  3. Subnet-naar-subnet-regel: Hiermee staat u een server op de back-end-subnet verbinding maken met een server op het front-end-subnet, maar niet andersom.

* Failsafe regel voor het verkeer dat niet voldoet aan een van de bovenstaande criteria:
  1. Alle verkeersregel voor weigeren: altijd de laatste regel in termen van prioriteit. Als de verkeersstroom niet overeenkomt met een van de vorige regels, wordt het door deze regel blokkeert. Het is een standaardregel geldt. Omdat deze meestal wordt geactiveerd, worden geen wijzigingen zijn aangebracht die nodig zijn.

> [!TIP]
> In de tweede toepassing verkeersregel mag een willekeurige poort in dit voorbeeld eenvoudig te houden. In een echte scenario, moet u specifieke poorten en adresbereiken gebruiken om het verminderen van de kwetsbaarheid voor aanvallen van deze regel.


> [!IMPORTANT]
> Nadat u de regels hebt gemaakt, is het belangrijk dat u de prioriteit van elke regel om ervoor te zorgen dat verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in volgorde van prioriteit. Dit kan eenvoudig worden buitengesloten wordt uit de firewall als de regels onjuiste geordende zijn. Zorg ervoor dat het instellen van de firewallregel management als de absolute hoogste prioriteit.

### <a name="rule-prerequisites"></a>Regel voor vereisten

Openbare eindpunten zijn vereist voor de virtuele machine met de firewall. Deze openbare eindpunten moeten geopend zijn zodat de firewall verkeer kan verwerken. Er zijn drie typen verkeer in dit voorbeeld:

1. Beheer van verkeer voor het beheren van de firewall en firewall-regels
1. RDP-verkeer voor het beheren van de windows-servers
1. Toepassingsverkeer

De verkeerstypen worden weergegeven in het bovenste gedeelte van de firewall-regels bovenstaande logische diagram.

> [!IMPORTANT]
> Houd er rekening mee dat *alle* verkeer binnenkomt via de firewall. Met extern bureaublad op de server IIS01 moet u verbinding maken met de firewall op poort 8014 en laat de firewall aan de aanvraag RDP intern wordt doorgestuurd naar de IIS01 RDP-poort. De Azure-portal **Connect** knop werkt niet omdat er geen directe RDP-pad naar IIS01 zo lang de portal kunt zien. Alle verbindingen via internet zijn naar de service en een poort (bijvoorbeeld secscv001.cloudapp.net:xxxx). Verwijzen naar het bovenstaande diagram van de toewijzing van de poort van de externe en interne IP en poort.

U kunt een eindpunt op het moment van de VM wordt gemaakt of na de build openen. De voorbeeldscript en het volgende codefragment een eindpunt geopend nadat de virtuele machine is gemaakt.

Houd er rekening mee dat items die beginnen met `$`, zoals `$VMName[$i]`, zijn van de gebruiker gedefinieerde variabelen in het script in de sectie documentatie. De `[$i]` vertegenwoordigt het nummer van de matrix van een specifieke virtuele machine in een matrix van virtuele machines.

```powershell
Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
    -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
    Update-AzureVM
```

Hoewel het niet duidelijk hieronder vanwege variabelen, moet u alleen eindpunten in de cloud security service openen. Deze voorzorgsmaatregel zorgt ervoor dat de firewall al het binnenkomende verkeer, verwerkt of is deze gerouteerd, opnieuw door NAT worden toegewezen of verwijderd.

Een management-client installeren op een PC om de firewall beheren en de vereiste configuratie-instellingen te maken. Raadpleeg de documentatie van de leverancier voor meer informatie over het beheren van uw firewall of andere NVA. De rest van deze sectie, evenals de **firewallregels maken** sectie beschrijven de configuratie van de firewall. Gebruik van de leverancier management-client, niet de Azure portal of PowerShell.

Ga naar [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin) de management-client downloaden en meer informatie over het verbinding maken met de Barracuda-firewall.

Nadat u bent aangemeld bij de firewall, definieert u de netwerk- en service-objecten voordat u de firewall-regels maakt. Deze twee vereisten objectklassen kunnen u het maken van de regels gemakkelijker.

In dit voorbeeld definieert u drie benoemde netwerkobjecten voor elk:

* Front-end-subnet
* Back-end-subnet
* IP-adres van de DNS-server

Een netwerk te maken met de naam van de Barracuda NG-Beheerdashboard:

1. Ga naar de **tabblad configuratie**.
1. Selecteer **Ruleset** in de **operationele configuratie** sectie
1. Selecteer **netwerken** onder de **Firewallobjecten** menu.
1. Selecteer **nieuw** in de **netwerken bewerken** menu.
1. De netwerkobject maken door de naam en het voorvoegsel toe te voegen:

   ![Maken van een object front-ed][3]

De voorgaande stappen maken een benoemde netwerk voor de front-end-subnet. Een soortgelijk object voor de back-end-subnet maken. De subnetten kunnen nu nog eenvoudiger verwezen met de naam in de firewall-regels.

Voor het object van de DNS-server:

![Een DNS-server-object maken][4]

Deze één verwijzing naar een IP-adres wordt gebruikt in een DNS-regel verderop in dit document.

De objectklasse bevat services-objecten, die staan voor de RDP-verbindingspoorten voor elke server. Het bestaande object van de RDP-service is gebonden aan de standaard RDP-poort 3389. Zo kunt u nieuwe services zodat verkeer vanaf de externe poorten (8014-8026). U kunt ook de nieuwe poorten toevoegen aan de bestaande RDP-service. Voor een eenvoudige demonstratie, kunt u echter een afzonderlijke regel maken voor elke server. Een nieuwe regel voor RDP voor een server uit de Barracuda NG-Beheerdashboard maken:

1. Ga naar de **tabblad configuratie**.
1. Selecteer **Ruleset** in de **operationele configuratie** sectie.
1. Selecteer **Services** onder de **Firewallobjecten** menu.
1. Schuif omlaag in de lijst met services en selecteer **RDP**.
1. Met de rechtermuisknop op en selecteer kopiëren, klikt u vervolgens met de rechtermuisknop op en selecteer plakken.
1. Er is nu een RDP-Copy1 service-object dat kan worden bewerkt. Met de rechtermuisknop op **RDP-Copy1** en selecteer **bewerken**.
1. De **bewerken-serviceobject** venster maximaal zoals hieronder wordt weergegeven:

   ![Exemplaar van standaard RDP-regel][5]

1. Bewerk de waarden voor de RDP-service voor een specifieke server. Voor AppVM01, de standaard-RDP-regel moet worden gewijzigd zodat een nieuwe service **naam**, **beschrijving**, en externe RDP-poort die wordt gebruikt in het diagram afbeelding 8. Houd er rekening mee dat de poorten van de RDP-standaardwaarde van 3389 zijn gewijzigd in de externe poort voor deze specifieke server. Bijvoorbeeld, is de externe poort voor AppVM01 8025. De gewijzigde service-regel wordt hier weergegeven:

   ![AppVM01 regel][6]

Herhaal dit proces voor het maken van RDP-services voor de overige servers: AppVM02, DNS01 en IIS01. Deze services maken de regels in de volgende sectie eenvoudiger te maken en duidelijker.

> [!NOTE]
> Een RDP-service voor de firewall is niet nodig omdat de virtuele machine van de firewall een installatiekopie op basis van Linux is, zodat SSH op poort 22 wordt gebruikt voor VM-beheer in plaats van RDP. Ook poort 22 en twee andere poorten worden toegestaan voor de connectiviteit van het beheer. Zie de **management firewallregel** in de volgende sectie.

### <a name="firewall-rules-creation"></a>Firewall-regels maken

Er zijn drie typen firewallregels die in dit voorbeeld wordt met verschillende pictogrammen worden gebruikt:

De omleidings-regel van toepassing: ![Omleidings-toepassingspictogram][7]

De doel-NAT-regel: ![Bestemming NAT-pictogram][8]

De pass-regel: ![Pass-pictogram][9]

Meer informatie over deze regels kan worden gevonden op de Barracuda-website.

De volgende regels maken of bestaande standaardregels controleren:

1. Vanuit het dashboard van de client Barracuda NG beheerder gaat u naar de **configuratie** tabblad.
1. In de **operationele configuratie** sectie, selecteer **Ruleset**.
1. De **Main regels** grid active ziet u de bestaande en de regels voor deze firewall gedeactiveerd. Selecteer het groene **+** in de rechterbovenhoek om een nieuwe regel te maken. Als uw firewall is vergrendeld voor wijzigingen, ziet u een knop gemarkeerd **vergrendeling** en kan niet maken of bewerken van regels. Selecteer de **vergrendeling** knop om te ontgrendelen van de set en toestaan. Met de rechtermuisknop op een regel die u wilt bewerken, en selecteer **regel bewerken**.

Nadat u maakt of een van de regels wijzigen, push ze naar de firewall en activeer deze. De wijzigingen in de regel worden anders pas van kracht. Het proces van push- en -activering wordt beschreven in [regel activering](#rule-activation).

Hier volgen de details van elke regel vereist voor het voltooien van dit voorbeeld:

* **Firewallregel management**: Deze app omleidings-regel staat verkeer moet worden doorgegeven aan de beheerpoorten van het virtuele netwerkapparaat, in dit voorbeeld een Barracuda NextGen Firewall. De poorten zijn 801, 807 en eventueel 22. De interne en externe poorten zijn hetzelfde, geen poortvertaling. Deze regel wordt de SETUP-MGMT-ACCESS genoemd. Het is een standaardregel en standaard ingeschakeld in de Barracuda NextGen Firewall, versie 6.1.
  
    ![Firewallregel voor beheer][10]

  > [!TIP]
  > De bron-adresruimte in deze regel is **eventuele**. Als het beheer van IP-adresbereiken bekend zijn, minder waardoor dit bereik ook gevoelig voor aanvallen op de beheerpoorten.

* **Regels voor RDP**:  Deze bestemming NAT-regels toestaan beheer van de afzonderlijke servers via RDP. De essentiële velden voor deze regels zijn:
  * De bron. Als u RDP vanaf elke locatie, gebruikt u de verwijzing **eventuele** in het veld voor de gegevensbron.
  * Service. Gebruik het RDP-serviceobject die u eerder hebt gemaakt: **AppVM01 RDP**. De externe poorten doorsturen naar het lokale IP-adres van de server en naar de standaard-RDP-poort 3386. Deze specifieke regel is voor de RDP-toegang tot AppVM01.
  * Bestemming. Gebruik de lokale poort op de firewall: **DCHP 1 lokale IP** of **eth0** als u statische IP-adressen. Het rangtelwoord (eth0, eth1, enzovoort) kan afwijken als uw netwerkapparaat meerdere lokale interfaces heeft. De firewall gebruikt deze poort voor het verzenden van, en deze gelijk zijn aan de ontvangende poort kan zijn. De werkelijke gerouteerde bestemming zich in de **doellijst** veld.
  * Omleiding. Configureren als u wilt zien van het virtuele apparaat waar u uiteindelijk dit verkeer omleiden. De eenvoudigste omleiding is het IP-adres in het veld doellijst plaatsen. U kunt ook de poort opgeven en zowel de poort en het IP-adres NAT wordt omgeleid. Als u een poort niet opgeeft, het virtuele apparaat maakt gebruik van de doelpoort voor de inkomende aanvraag.

    ![RDP-firewallregel][11]

    Vier RDP-regels maken:

    | Regelnaam | Server | Service | De lijst van doel |
    | --- | --- | --- | --- |
    | RDP-to-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
    | RDP-to-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
    | RDP-to-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
    | RDP-to-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

  > [!TIP]
  > Het bereik van de velden van de bron- en Service vermindert de kwetsbaarheid voor aanvallen. Gebruik de meest beperkte mogelijkheden waarmee de functionaliteit.

* **Regels voor netwerkverkeer toepassing**: Er zijn twee regels voor netwerkverkeer van toepassing. Eén meter is voor de front-end-webverkeer te genereren. De andere bevat informatie over back-end-verkeer, zoals webserver-gegevenslaag. Deze regels zijn afhankelijk van netwerkarchitectuur en verkeer stromen.
  
  * De front-end-regel voor het webverkeer te genereren:
  
    ![Web-firewallregel][12]
  
    Deze bestemming NAT-regel staat de feitelijke toepassingsverkeer om de toepassingsserver te bereiken. In tegenstelling tot de regels voor beveiliging, beheer en enz kunnen regels voor application externe gebruikers of -services tot de toepassingen. In dit voorbeeld heeft één webserver op poort 80, waardoor één toepassing firewallregel omleiden van verkeer dat bestemd is voor een extern IP-adres aan in plaats daarvan wordt doorgestuurd naar het interne IP-adres van de webserver. De sessie omgeleide verkeer opnieuw door NAT wordt toegewezen aan de interne server.

    > [!NOTE]
    > Er is geen poort toegewezen in de **doellijst** veld. De binnenkomende poort 80 (of 443 voor de geselecteerde Service) wordt dus gebruikt in de omleiding van de webserver. Als de webserver op een andere poort, zoals 8080 luistert, kunt u het veld doellijst bijwerken naar 10.0.1.4:8080 om toe te staan voor de poortomleiding.
  
  * De back-end-regel kan de webserver om te communiceren met de AppVM01-server, maar niet AppVM02 via **eventuele** service:
  
    ![Firewallregel AppVM01][13]
  
    Met deze regel pass kunnen elke IIS-server op het front-end-subnet te bereiken AppVM01 (10.0.2.5) op een willekeurige poort met behulp van elk protocol voor zodat gegevens kunnen worden geopend door de web-App.
  
    In deze schermafbeelding `<explicit-dest>` wordt gebruikt in de **bestemming** veld om aan te duiden 10.0.2.5 als doel. U kunt het IP-adres expliciet zoals wordt weergegeven in de schermafbeelding opgeven. U kunt ook een Object met de naam, zoals in de vereisten voor de DNS-server. De beheerder van de firewall kunt kiezen welke methode u wilt gebruiken. Als u wilt toevoegen 10.0.2.5 als een expliciete doel, dubbelklikt u op de eerste lege rij onder `<explicit-dest>` en voer het adres in het dialoogvenster dat wordt geopend.
  
    Met deze regel pass worden geen NAT is nodig omdat deze intern verkeer afhandelt. U kunt instellen dat de **verbindingsmethode** naar `No SNAT`.
  
    > [!NOTE]
    > Het Bronnetwerk in deze regel is een resource op het front-end-subnet als er slechts één. Als uw architectuur Hiermee geeft u een bekende aantal webservers, kunt u een resource-netwerkobject voor meer specifieke zodat deze exacte IP-adressen in plaats van het gehele front-end-subnet maken.

    > [!TIP]
    > Deze regel maakt gebruik van de service **eventuele** om de voorbeeld-App gemakkelijker instellen en gebruiken. Hierdoor kan ICMPv4 (ping) in een enkele regel. We raden echter om te beperken van de aanval surface voor deze grens aan de poorten en protocollen-Services beperken tot het minimum mogelijk waarmee de toepassing opnieuw.

    > [!TIP]
    > Hoewel deze voorbeeldregel maakt gebruik van `<explicit-dest>` -verwijzing, moet u een consistent benadering in de gehele configuratie van de firewall. Het is raadzaam om met behulp van een benoemde-netwerkobject voor eenvoudiger leesbaarheid en ondersteuningsmogelijkheden. De `<explicit-dest>` alleen om weer te geven van een methode alternatieve verwijzing als volgt weergegeven. Niet in het algemeen wordt aangeraden, met name voor complexe configuraties.

* **Regel voor uitgaand naar internet**: Deze pass-regel staat verkeer van een Bronnetwerk moeten worden doorgegeven aan de geselecteerde bestemming-netwerken. De Barracuda NextGen firewall heeft meestal deze regel 'aan' standaard, maar een uitgeschakelde status. Met de rechtermuisknop op deze regel voor toegang tot de **regel activeren** opdracht. Wijzig de regel wordt weergegeven in de schermopname de netwerkobjecten voor back-end- en front-end-subnetten toevoegen aan het bronkenmerk van deze regel. U hebt deze netwerkobjecten gemaakt in de sectie vereisten van dit artikel.
  
    ![Uitgaande-firewallregel][14]

* **Een DNS-regel**: Deze regel pass kunnen alleen DNS (poort 53)-verkeer moeten worden doorgegeven aan de DNS-server. Voor deze omgeving is de meeste verkeer van de front-end naar de back-end, zodat deze regel expliciet toestaan; DNS-verkeer geblokkeerd.
  
    ![DNS-firewallregel][15]
  
    > [!NOTE]
    > De **verbindingsmethode** is ingesteld op `No SNAT` omdat met deze regel voor interne IP-adres op het interne IP-adres verkeer is en geen omleidingen via NAT vereist is.

* **Subnet-naar-subnet regel**: Deze standaardregel pass is geactiveerd en gewijzigd, zodat elke server in de back-end-subnet verbinding maken met elke server in het front-end-subnet. Deze regel alleen intern verkeer coves zodat de **verbindingsmethode** kan worden ingesteld op `No SNAT`.

    ![Intra-VNet-firewallregel][16]
  
    > [!NOTE]
    > De **bidirectionele** selectievakje niet hier is geselecteerd, zodat deze regel van toepassing slechts in één richting is. Een verbinding kan worden gestart vanuit de back-end-subnet op de front-endnetwerk, maar niet andersom. Als dit selectievakje is ingeschakeld, zou deze regel bidirectioneel verkeer, die we hebben opgegeven als ongewenste in onze logisch diagram inschakelen.

* **Alle verkeersregel voor weigeren**: Deze regel moet altijd de laatste regel in termen van prioriteit. Als de verkeersstroom komt niet overeen met een van de vorige regels, met deze regel zorgt ervoor dat deze worden verwijderd. De regel is meestal standaard geactiveerd, zodat er geen wijzigingen nodig zijn.
  
    ![Regel voor weigeren van Firewall][17]

> [!IMPORTANT]
> Nadat u alle voorgaande regels hebt gemaakt, Controleer u de prioriteit van elke regel om te controleren of verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels weergegeven in de volgorde die ze moeten worden weergegeven in de Barracuda-Management-client hoofdraster van regels voor doorsturen.

## <a name="rule-activation"></a>Activering van de regel

Nadat u de regel die is ingesteld om te voldoen aan de specificaties van het logische-diagram hebt gewijzigd, moet u de regelset uploaden naar de firewall en activeer deze.

![Activering van Firewall-regel][18]

Zoek in de rechterbovenhoek van het venster van de management-client en selecteer **wijzigingen verzenden** de gewijzigde regels uploaden naar de firewall. Selecteer **activeren**.

Wanneer u de firewall regelset activeert, wordt deze Voorbeeldomgeving is voltooid.

## <a name="traffic-scenarios"></a>Scenario's voor verkeer

> [!IMPORTANT]
> Houd er rekening mee dat *alle* verkeer binnenkomt via de firewall. Met extern bureaublad op de server IIS01 moet u verbinding maken met de firewall op poort 8014 en laat de firewall aan de aanvraag RDP intern wordt doorgestuurd naar de IIS01 RDP-poort. De Azure-portal **Connect** knop werkt niet omdat er geen directe RDP-pad naar IIS01 zo lang de portal kunt zien. Alle verbindingen via internet zijn naar de service en een poort (bijvoorbeeld secscv001.cloudapp.net:xxxx). Verwijzen naar het bovenstaande diagram van de toewijzing van de poort van de externe en interne IP en poort.

Voor deze scenario's moeten de volgende firewallregels zijn voldaan:

1. Firewall Management (FW Mgmt)
2. RDP-verbinding IIS01
3. RDP-verbinding DNS01
4. RDP-verbinding AppVM01
5. RDP-verbinding AppVM02
6. Verkeer naar de Web-App
7. App-verkeer naar AppVM01
8. Uitgaand naar internet
9. Front end-to-DNS01
10. Intra-Subnet-verkeer (back-end aan de front-end alleen)
11. Alles negeren

De regelset van uw werkelijke firewall moet waarschijnlijk meer regels dan die in dit voorbeeld. Ze kunnen ook andere prioriteitsnummers hebben. U moet verwijzen naar deze lijst en de bijbehorende nummers voor hun relatieve prioriteit aan elkaar. Bijvoorbeeld, de regel 'RDP-IIS01' mogelijk aantal 5 op de werkelijke firewall, maar zo lang als de onderstaande 'Firewall Management"regel en regel boven de"RDP naar DNS01', de set wordt uitgelijnd met de bedoeling van deze lijst. Deze lijst helpt ook bij het vereenvoudigen van de instructies voor scenario's die volgen. Bijvoorbeeld, "Firewall-regel 9 (DNS)." Houd er rekening mee dat de vier regels voor RDP gezamenlijk 'de RDP-regels worden genoemd wanneer het verkeer scenario heeft geen betrekking op RDP.

Let erop dat netwerkbeveiligingsgroepen (nsg's) zijn ook aanwezig voor inkomend internetverkeer op de front-end en back-end-subnetten.

### <a name="allowed-internet-to-web-server"></a>(Toegestaan) Internet-webserver

1. Een internetgebruiker vraagt HTTP-pagina van SecSvc001.CloudApp.Net (internetgerichte cloudservice).
1. De cloudservice wordt verkeer via een eindpunt geopend op poort 80 op de firewall-interface op 10.0.0.4:80 doorgegeven.
1. Er is geen NSG is toegewezen aan Security subnet, zodat de NSG-regels van het systeem het verkeer naar de firewall toestaan.
1. Het verkeer komt binnen via een interne IP-adres van de firewall (10.0.1.4).
1. Regelverwerking wordt uitgevoerd door de firewall:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels (regels voor RDP) 2-5 zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 6 (App: Web) is van toepassing. Het verkeer is toegestaan. Firewall verkeer via NAT 10.0.1.4 (IIS01) wordt omgeleid.
1. Verwerking van inkomende regel wordt uitgevoerd door het front-end-subnet:
   1. NSG-regel 1 (internet blokkeren) niet van toepassing. De firewall omgeleid dit verkeer via NAT, zodat de bron-adres nu de firewall is. Omdat de firewall op het subnet van de beveiliging en wordt weergegeven als lokale verkeer naar het front-end-subnet-NSG, wordt het verkeer is toegestaan. Verplaatsen naar de volgende regel.
   1. Standaard-NSG-regels toestaan subnet-naar-subnet-verkeer, zodat dit verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. IIS01 luistert voor webverkeer te genereren. Het ontvangen van deze aanvraag en begint met de verwerking van de aanvraag.
1. IIS01 probeert het starten van een FTP-sessie naar AppVM01 op de back-end-subnet.
1. De UDR-route op front-end-subnet wordt de firewall van de volgende hop.
1. Er zijn geen regels voor uitgaand verkeer op de front-end-subnet, zodat het verkeer is toegestaan.
1. Firewall begint regelverwerking:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels (regels voor RDP) 2-5 zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 6 (App: Web) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 7 (App: back-end) is van toepassing. Het verkeer is toegestaan. De firewall verzendt het verkeer naar 10.0.2.5 (AppVM01).
1. Verwerking van inkomende regel wordt uitgevoerd door de back-end-subnet:
    1. NSG-regel 1 (internet blokkeren) niet van toepassing. Verplaatsen naar de volgende regel.
    1. Standaard-NSG-regels toestaan subnet-naar-subnet-verkeer. Het verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. AppVM01 ontvangt de aanvraag, initieert de sessie en reageert.
1. De UDR-route op back-end-subnet wordt de firewall van de volgende hop.
1. Er zijn geen uitgaande NSG-regels op de back-end-subnet, zodat het antwoord is toegestaan.
1. Omdat het verkeer voor een vastgestelde sessie worden geretourneerd, wordt het antwoord terug naar de webserver (IIS01) doorgegeven in de firewall.
1. Front-end-subnet voert de verwerking van inkomende regel:
    1. NSG-regel 1 (internet blokkeren) niet van toepassing. Verplaatsen naar de volgende regel.
    1. De standaard-NSG-regels toestaan subnet-naar-subnet-verkeer, zodat dit verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. De IIS-server-antwoord is ontvangen en de transactie met AppVM01 is voltooid. Vervolgens wordt de server is voltooid het bouwen van het HTTP-antwoord en verzendt dit naar de aanvrager.
1. Er zijn geen uitgaande NSG-regels op het front-end-subnet, zodat het antwoord is toegestaan.
1. Het HTTP-antwoord komt binnen via de firewall. Omdat het een reactie op een vastgestelde NAT-sessie is, wordt het door de firewall accepteert.
1. De firewall stuurt het antwoord terug naar de internet-gebruiker.
1. Er zijn geen uitgaande NSG-regels of UDR-hops op het front-end-subnet, zodat het antwoord is toegestaan. De gebruiker internet ontvangt de webpagina die is aangevraagd.

### <a name="allowed-internet-rdp-to-back-end"></a>(Toegestaan) Internet RDP naar de back-end

1. Een serverbeheerder op het internet vraagt een RDP-sessie naar AppVM01 via SecSvc001.CloudApp.Net:8025. 8025 is het poortnummer voor de firewallregel 4 (RDP AppVM01) door de gebruiker toegewezen.
1. De cloudservice wordt verkeer via het open-eindpunt op poort 8025 firewall-interface op 10.0.0.4:8025 doorgegeven.
1. Er is geen NSG is toegewezen aan Security subnet, zodat het systeem NSG-regels verkeer naar de firewall toestaan.
1. Regelverwerking wordt uitgevoerd door de firewall:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewallregel 2 (RDP IIS) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewallregel 3 (RDP DNS01) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 4 (RDP AppVM01) is van toepassing, zodat verkeer is toegestaan. De firewall omgeleid deze via NAT aan 10.0.2.5:3386 (RDP-poort op AppVM01).
1. Verwerking van inkomende regel wordt uitgevoerd door de back-end-subnet:
   1. NSG-regel 1 (internet blokkeren) niet van toepassing. De firewall omgeleid dit verkeer via NAT, zodat de bron-adres is nu de firewall die zich in het subnet van de beveiliging. Het wordt gezien door de back-end-subnet-NSG aan het lokale verkeer en is toegestaan. Verplaatsen naar de volgende regel.
   1. Standaard-NSG-regels toestaan subnet-naar-subnet-verkeer, zodat dit verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. AppVM01 luistert voor RDP-verkeer en reageert.
1. Er zijn geen uitgaande NSG-regels, zodat de standaardregels van toepassing. Retourverkeer is toegestaan.
1. UDR-routes uitgaand verkeer naar de firewall als de volgende hop.
1. Omdat het verkeer voor een vastgestelde sessie worden geretourneerd, wordt het antwoord terug naar de gebruiker internet doorgegeven in de firewall.
1. RDP-sessie is ingeschakeld.
1. AppVM01 vraagt om wachtwoord van de naam van gebruiker.

### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web server DNS-zoekactie op DNS-server

1. Webserver IIS01 een gegevensfeed HTTP-aanvragen\:\/\/www.data.gov, maar moet het adres omzetten.
1. De netwerkconfiguratie voor het virtuele netwerk van een lijst met DNS01 (10.0.2.4 op de back-end-subnet) als de primaire DNS-server. IIS01 verzendt de DNS-aanvraag naar DNS01.
1. UDR-routes uitgaand verkeer naar de firewall als de volgende hop.
1. Er is geen uitgaande NSG-regels zijn gebonden aan de front-end-subnet. Het verkeer is toegestaan.
1. Regelverwerking wordt uitgevoerd door Firewall:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 2-5 (regels voor RDP) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels 6 en 7 (App-regels) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 8 (met internet) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 9 (DNS) is van toepassing. Het verkeer is toegestaan. Firewall verzendt het verkeer naar 10.0.2.4 (DNS01).
1. Verwerking van inkomende regel wordt uitgevoerd door de back-end-subnet:
   1. NSG-regel 1 (internet blokkeren) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Standaard-NSG-regels toestaan subnet-naar-subnet-verkeer. Het verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. De DNS-server ontvangt de aanvraag.
1. De DNS-server beschikt niet over het adres in de cache opgeslagen en wordt u gevraagd een basis-DNS-server op het internet.
1. UDR stuurt het uitgaande verkeer naar de firewall als de volgende hop.
1. Er zijn geen uitgaande NSG-regels op back-end-subnet op het verkeer is toegestaan.
1. Regelverwerking wordt uitgevoerd door Firewall:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 2-5 (regels voor RDP) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels 6 en 7 (App-regels) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 8 (met het internet) is van toepassing. Het verkeer is toegestaan. De sessie wordt gerouteerd via SNAT naar de root DNS-server op het internet.
1. De internet-DNS-server reageert. Deze sessie is gestart vanaf de firewall, zodat het antwoord wordt geaccepteerd door de firewall.
1. Deze sessie is al tot stand gebracht, zodat de firewall het antwoord op de initiërende DNS01-server stuurt.
1. Verwerking van inkomende regel wordt uitgevoerd door de back-end-subnet:
    1. NSG-regel 1 (internet blokkeren) niet van toepassing. Verplaatsen naar de volgende regel.
    1. Standaard-NSG-regels toestaan subnet-naar-subnet-verkeer naar dit verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. De DNS-server ontvangt en plaatst het antwoord en klik vervolgens op de eerste aanvraag terug naar IIS01 reageert.
1. De UDR-route op back-end-subnet wordt de firewall van de volgende hop.
1. Er is geen uitgaande NSG-regels bestaan op de back-end-subnet, zodat het verkeer is toegestaan.
1. Deze sessie is al gemaakt op de firewall, zodat de firewall het antwoord terug naar de IIS-server wordt omgeleid.
1. Verwerking van inkomende regel wordt uitgevoerd door het front-end-subnet:
    1. Er is geen NSG-regel voor binnenkomend verkeer van de back-end-subnet aan de front-end-subnet, zodat geen van de NSG-regels van toepassing.
    1. De standaardregel voor systeem staat verkeer tussen subnetten. Het verkeer is toegestaan.
1. IIS01 ontvangt het antwoord van DNS01.

### <a name="allowed-back-end-server-to-front-end-server"></a>(Toegestaan) Back-end-server op de front-endwebserver

1. Een beheerder is aangemeld bij AppVM02 via RDP vraagt een bestand rechtstreeks vanaf de server IIS01 via windows Verkenner.
1. De UDR-route op back-end-subnet wordt de firewall van de volgende hop.
1. Er zijn geen uitgaande NSG-regels op de back-end-subnet, zodat het antwoord is toegestaan.
1. Regelverwerking wordt uitgevoerd door de firewall:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 2-5 (regels voor RDP) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels 6 en 7 (App-regels) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 8 (met internet) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 9 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 10 (Intra-Subnet) is van toepassing. Het verkeer is toegestaan. De firewall stuurt verkeer naar 10.0.1.4 (IIS01).
1. Het front-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (internet blokkeren) niet van toepassing, verplaatsen naar de volgende regel
   1. De standaard-NSG-regels toestaan subnet-naar-subnet-verkeer, zodat het verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. Ervan uitgaande dat het juiste verificatie en autorisatie, IIS01 worden aanvragen geaccepteerd en reageert.
1. De UDR-route op het front-end-subnet wordt de firewall van de volgende hop.
1. Er zijn geen uitgaande NSG-regels op het front-end-subnet, zodat het antwoord is toegestaan.
1. Deze sessie al bestaat op de firewall, zodat deze reactie is toegestaan. De firewall retourneert het antwoord op AppVM02.
1. Verwerking van inkomende regel wordt uitgevoerd door de back-end-subnet:
    1. NSG-regel 1 (internet blokkeren) niet van toepassing. Verplaatsen naar de volgende regel.
    2. De standaard-NSG-regels toestaan subnet-naar-subnet-verkeer, zodat het verkeer is toegestaan. Stop de verwerking van NSG-regel.
1. AppVM02-antwoord is ontvangen.

### <a name="denied-internet-direct-to-web-server"></a>(Geweigerd) Rechtstreeks naar de webserver van Internet

1. Een internet-gebruiker probeert te krijgen tot de webserver IIS01 via de service FrontEnd001.CloudApp.Net.
1. Er zijn geen eindpunten openen voor HTTP-verkeer, zodat dit verkeer niet via de cloudservice doorgeven. Het verkeer bereiken de server niet.
1. Als de eindpunten geopend voor een of andere reden zijn, de NSG (internet blokkeren) op het front-end-subnet deze verkeer wordt geblokkeerd.
1. Ten slotte verzendt de front-end-subnet UDR-route al het uitgaande verkeer van IIS01 naar de firewall als de volgende hop. De firewall ziet als asymmetrische verkeer en het uitgaande antwoord komt.

>Er zijn dus ten minste drie onafhankelijke defensieve tussen internet en IIS01. De cloudservice wordt voorkomen dat onbevoegde of ongepaste toegang.

### <a name="denied-internet-to-back-end-server"></a>(Geweigerd) Internet naar het back-end-server

1. Een internet-gebruiker probeert te krijgen tot een bestand op AppVM01 via de BackEnd001.CloudApp.Net-service.
2. Er zijn geen eindpunten openen voor het delen van bestanden, zodat deze aanvraag de cloudservice niet doorgeven. Het verkeer bereiken de server niet.
3. Als de eindpunten geopend voor een of andere reden zijn, de NSG (internet blokkeren) dit verkeer wordt geblokkeerd.
4. Ten slotte verzendt de UDR-route al het uitgaande verkeer van AppVM01 naar de firewall als de volgende hop. De firewall ziet als asymmetrische verkeer en het uitgaande antwoord komt.

> Er zijn dus ten minste drie onafhankelijke defensieve tussen internet en AppVM01. De cloudservice wordt voorkomen dat onbevoegde of ongepaste toegang.

### <a name="denied-front-end-server-to-back-end-server"></a>(Geweigerd) Front-endwebserver op back-end-server

1. IIS01 is geknoeid en schadelijke code het opvragen van de back-end-subnet-servers wordt uitgevoerd.
1. De front-end-subnet UDR-route verzendt al het uitgaande verkeer van IIS01 aan de firewall als de volgende hop. De aangetaste virtuele machine kan niet deze routering wijzigen.
1. De firewall verwerkt het verkeer. Als de aanvraag AppVM01 of de DNS-server voor DNS-zoekacties is, kan de firewall het verkeer toestaan vanwege Firewall-regels 7 en 9. Al het andere verkeer wordt geblokkeerd door Firewall-regel 11 (Alles weigeren).
1. Als u geavanceerde detectie van bedreigingen op de firewall inschakelt, kan het verkeer dat bevat handtekeningen van bekende of patronen die markering van een regel voor geavanceerde bedreigingen kan worden voorkomen. Deze meting kunt werken, zelfs als het verkeer is toegestaan volgens de regels voor doorsturen van basic die in dit artikel worden besproken. Geavanceerde detectie van bedreigingen is niet in dit document behandeld. Zie de documentatie van de leverancier voor uw specifieke netwerkapparaat geavanceerde mogelijkheden van de bedreiging.

### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Geweigerd) Internet-DNS-zoekactie op DNS-server

1. Een internet-gebruiker probeert om te controleren of een interne DNS-record op DNS01 via BackEnd001.CloudApp.Net-service.
1. Omdat er geen eindpunten openen voor DNS-verkeer zijn, wordt dit verkeer niet doorgegeven via de cloudservice. Deze bereiken de server niet.
1. Als de eindpunten geopend voor een of andere reden zijn, de NSG-regel (internet blokkeren) op het front-end-subnet deze verkeer wordt geblokkeerd.
1. Ten slotte verzendt de back-end-subnet UDR-route al het uitgaande verkeer van DNS01 naar de firewall als de volgende hop. De firewall ziet dit als asymmetrische verkeer en het uitgaande antwoord komt.

> Er zijn dus ten minste drie onafhankelijke defensieve tussen internet en DNS01. De cloudservice wordt voorkomen dat onbevoegde of ongepaste toegang.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Geweigerd) SQL-toegang via de firewall voor Internet

1. Een internetgebruiker vraagt SQL-gegevens van de SecSvc001.CloudApp.Net internetgerichte-cloudservice.
1. Er zijn geen eindpunten openen voor SQL, zodat dit verkeer de cloudservice niet doorgeven. Deze bereiken de firewall niet.
1. Als SQL-eindpunten geopend voor een of andere reden zijn, voert de firewall regelverwerking:
   1. Firewallregel 1 (FW Mgmt) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels (regels voor RDP) 2-5 zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regels 6 en 7 (regels van toepassing) zijn niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 8 (met internet) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 9 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 10 (Intra-Subnet) niet van toepassing. Verplaatsen naar de volgende regel.
   1. Firewall-regel 11 (Alles weigeren) is van toepassing. Verkeer wordt geblokkeerd. Regelverwerking stoppen.

## <a name="references"></a>Verwijzingen

Deze sectie bevat de volgende items:

* Volledige script. Opslaan in een PowerShell-scriptbestand.
* Netwerk-configuratie. Sla deze op in een bestand met de naam NetworkConf2.xml.

De gebruiker gedefinieerde variabelen in de bestanden zo nodig wijzigen. Voer het script uit en volgt u de Firewall-regel installatie-instructies die eerder in dit artikel worden vermeld.

### <a name="full-script"></a>Volledige script

Voer dit script voor na het instellen van de gebruiker gedefinieerde variabelen:

1. Verbinding maken met een Azure-abonnement
1. Een nieuw opslagaccount maken
1. Een nieuw virtueel netwerk en drie subnetten zoals gedefinieerd in het netwerk-configuratiebestand maken
1. Vijf virtuele machines maken: een firewall en vier Windows Server-VM's
1. Configureren van UDR:
   1. Twee nieuwe routetabellen maken
   1. Routes toevoegen aan de tabellen
   1. Tabellen aan de juiste subnetten binden
1. Doorsturen via IP op de NVA inschakelen
1. NSG configureren:
   1. Een NSG te maken
   1. Een regel toevoegen
   1. De NSG koppelen aan de juiste subnetten

Voer deze PowerShell verbonden-script lokaal op een internet PC of de server.

> [!IMPORTANT]
> Wanneer u dit script uitvoert, waarschuwingen of andere informatieve berichten kunnen worden weergegeven in PowerShell. Alleen rode foutberichten zijn oorzaak zouden kunnen gaan maken.

```powershell
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
       - IP Forwarding from the FireWall out to the internet
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

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
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
    # No User Defined Variables or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) {
            Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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

      # Associate the Route Tables with the Subnets
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
        # since the NSG bound to the FrontEnd and BackEnd subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install BackEnd resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

### <a name="network-config-file"></a>Netwerk-configuratiebestand

Dit XML-bestand opslaan met bijgewerkte locatie. Wijzig de `$NetworkConfigFile` variabele in het volledige script hierboven om de opgeslagen netwerk config-bestand te koppelen.

```xml
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
```

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeldtoepassing met dit voorbeeld perimeternetwerk installeren.

> [!div class="nextstepaction"]
> [Voorbeeldscript voor toepassing](./virtual-networks-sample-app.md)

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
