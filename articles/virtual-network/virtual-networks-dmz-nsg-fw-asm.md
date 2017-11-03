---
title: "Voorbeeld DMZ – bouwen een DMZ toepassingen met een Firewall en nsg's beveiligen | Microsoft Docs"
description: Maken van een DMZ met een Firewall en Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc0e8a3fa749eb2e6f65ef92c2d3cb404cfc8bc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: een DMZ ter bescherming van toepassingen met een Firewall en nsg's maken
[Ga terug naar de grens van Best Practices beveiligingspagina][HOME]

In dit voorbeeld wordt een DMZ maken met een firewall, vier windows-servers en Netwerkbeveiligingsgroepen. Deze begeleidt ook bij elk van de relevante opdrachten voor het bieden van een beter begrip van elke stap. Er is ook een sectie verkeer Scenario voor een gedetailleerd stapsgewijs hoe verkeer wordt uitgevoerd door de lagen verdediging in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige code en de instructies voor het bouwen van deze omgeving om te testen en Experimenteer met verschillende scenario's. 

![Inkomende DMZ met NVA en NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

* Twee cloudservices: 'FrontEnd001' en 'BackEnd001'
* Een virtueel netwerk 'CorpNetwork' met twee subnetten: 'FrontEnd' en back-end'
* Een enkele Netwerkbeveiligingsgroep dat wordt toegepast op beide subnetten
* Een virtueel netwerkapparaat, in dit voorbeeld een Barracuda NextGen Firewall, die is verbonden met het subnet Frontend
* Een Windows-Server waarmee een toepassing webserver ('IIS01')
* Twee windows-servers die toepassing terug vertegenwoordigen end servers ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')

> [!NOTE]
> Hoewel dit voorbeeld wordt een Barracuda NextGen Firewall gebruikt, kunnen veel van de verschillende virtuele netwerkapparaten worden gebruikt voor dit voorbeeld.
> 
> 

In de sectie Verwijzingen hieronder is er een PowerShell-script dat de meeste van de hierboven beschreven omgeving bouwt. Opbouwen van de virtuele machines en virtuele netwerken, hoewel worden uitgevoerd door het voorbeeldscript worden niet beschreven in dit document uitvoeriger.

De omgeving maken:

1. Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario)
2. Bijwerken van de gebruikersvariabelen in het script moet overeenkomen met de omgeving die het script wordt uitgevoerd tegen (abonnementen, servicenamen, enzovoort)
3. Voer het script in PowerShell

**Opmerking**: de regio die wordt aangegeven in het PowerShell-script moet overeenkomen met de regio die wordt aangegeven in het netwerk van het XML-configuratiebestand.

Nadat het script wordt uitgevoerd kan de volgende stappen voor na script kunnen worden genomen:

1. Stel de firewallregels, deze procedure wordt besproken in de sectie met de titel: Firewall-regels.
2. Optioneel zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en de appserver met een eenvoudige webtoepassing waarmee testen met deze configuratie DMZ.

De volgende sectie wordt uitgelegd dat de meeste scripts instructies ten opzichte van Netwerkbeveiligingsgroepen.

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een groep NSG gebouwd en vervolgens geladen met zes regels. 

> [!TIP]
> In het algemeen moet u eerst uw specifieke regels voor 'Toestaan' maken en vervolgens de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels eerst geëvalueerd. Als verkeer toepassen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen op een of meer in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
> 
> 

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan
2. RDP-verkeer (poort 3389) van het Internet met een virtuele machine is toegestaan
3. HTTP-verkeer (poort 80) van het Internet naar de NVA (firewall) is toegestaan
4. Alle verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan
5. Verkeer (alle poorten) van het Internet op de gehele VNet (beide subnetten) is geweigerd.
6. Al het verkeer (alle poorten) van het subnet Frontend naar het back-end-subnet is geweigerd

Met deze regels gekoppeld aan elk subnet als een HTTP-aanvraag inkomende verkeer van Internet naar de webserver, beide regels 3 was (toestaan) en 5 (weigeren) toepassing zou zijn, maar omdat de regel 3 heeft een hogere prioriteit alleen zou worden toegepast en regel 5 niet in de play zou komen. De HTTP-aanvraag zou dus mag worden de firewall. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) zijn de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het subnet Frontend uit het praten met het subnet van de back-end (met uitzondering van toegestane regels 1 en 4-verkeer), beveiligt u hiermee het netwerk Backend in geval een aanvaller inbreuk de webtoepassing op de front-end de aanvaller zou hebben beperkte toegang tot de back-end 'beveiligde' netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. Voor dit voorbeeld bent we uitgaand verkeer toestaat en alle regels voor uitgaande verbindingen niet wijzigen. Vergrendelen van verkeer in beide richtingen, de gebruiker gedefinieerde routering is vereist, is dit verkend in een ander voorbeeld die u kunt vinden in de [belangrijkste beveiliging grens document][HOME].

De bovenstaande NSG-regels besproken zijn vergelijkbaar met het NSG-regels in [voorbeeld 1: een eenvoudige DMZ met nsg's bouwen][Example1]. Controleer de beschrijving van het NSG in dat document voor een gedetailleerde blik op elke regel van het NSG en de kenmerken.

## <a name="firewall-rules"></a>Firewallregels
Een management-client moet worden geïnstalleerd op een computer voor het beheren van de firewall en maken van de configuraties die nodig zijn. Zie de documentatie van uw firewall (of andere NVA)-leverancier voor het beheren van het apparaat. De rest van deze sectie beschrijft de configuratie van de firewall zelf, via de leveranciers management-client (d.w.z. niet in de Azure-portal of PowerShell).

Instructies voor het downloaden van de client en verbinding maken met de in dit voorbeeld gebruikt Barracuda vindt u hier: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Op de firewall moet doorsturen regels worden gemaakt. Omdat hierbij alleen routes internetverkeer in is gebonden aan de firewall en vervolgens naar de webserver, wordt slechts één doorsturen NAT-regel vereist. Op de Barracuda NextGen Firewall gebruikt in dit voorbeeld is de regel een bestemming NAT-regel ('zomertijd NAT') om door te geven van dit verkeer.

De volgende regel maken (of Controleer of de bestaande standaardregels) starten vanuit het dashboard van de client Barracuda NG Admin gaat u naar het tabblad configuratie, in de operationele configuratie sectie Ruleset op. Een raster aangeroepen, 'Main regels' wordt de bestaande regels voor de actieve als gedeactiveerde weergegeven op de firewall. In de rechterbovenhoek van dit raster is een klein, groene '+' knop, klik hierop om een nieuwe regel te maken (Opmerking: uw firewall mogelijk 'vergrendeld' om wijzigingen, als er een knop gemarkeerd als 'Vergrendelen' en niet maken of regels bewerken, klikt u op deze knop om het ruleset ' ontgrendelen' en bewerken). Als u bewerken van een bestaande regel wilt, selecteert u deze regel, met de rechtermuisknop op en selecteer regel bewerken.

Maak een nieuwe regel en geef een naam op, zoals 'WebTraffic'. 

Het pictogram bestemming NAT-regel uitziet: ![bestemming NAT-pictogram][2]

De regel zelf zou als volgt uitzien:

![Firewallregel][3]

Hier een adres dat treffers in de Firewall inkomend probeert te bereiken HTTP (poort 80 of 443 voor HTTPS) worden verstuurd buiten de Firewall 'DHCP1 lokale-IP-interface en omgeleid naar de webserver met het IP-adres van 10.0.1.5. Omdat het verkeer wordt binnenkort in op poort 80 en gaat naar de webserver op poort 80 is geen poortwijziging is vereist. Echter, de lijst met doelservers kan zijn 10.0.1.5:8080 als uw webserver worden geluisterd op poort 8080, dus het vertalen van de binnenkomende poort 80 op de firewall binnenkomende poort 8080 op de webserver.

Een verbindingsmethode moet ook worden aangegeven, voor de regel van het doel van het Internet 'Dynamische snat omzetten' is het meest geschikt. 

Hoewel u slechts één regel is gemaakt is het belangrijk dat de prioriteit correct is ingesteld. Als in het raster van alle regels op de firewall voor dat deze nieuwe regel wordt aan de onderkant (onder de regel 'BLOCKALL') dit wordt nooit een rol spelen. Zorg ervoor dat de nieuwe regel voor internetverkeer hoger is dan de BLOCKALL-regel.

Als de regel is gemaakt, moet aan de firewall gepusht en wordt geactiveerd, als u dit niet doet de regel wijziging wordt pas van kracht. Het proces van push- en -activering wordt in de volgende sectie beschreven.

## <a name="rule-activation"></a>Activering van de regel
Met de ruleset gewijzigd als deze regel wilt toevoegen, moet de ruleset worden geüpload naar de firewall en geactiveerd.

![Activering van de firewall-regel][4]

In de rechterbovenhoek van de management-client zijn een cluster van knoppen. Klik op de knop 'Wijzigingen verzenden' voor het verzenden van de gewijzigde regels op de firewall en klik vervolgens op de knop 'Activeren'.

Deze versie van de omgeving voorbeeld is voor de activering van de firewall ruleset voltooid. Eventueel de post-build-scripts in de sectie Verwijzingen kunnen worden uitgevoerd om een toepassing toevoegen aan deze omgeving voor het testen van de onderstaande verkeer scenario's.

> [!IMPORTANT]
> Het is essentieel dat niet u de webserver rechtstreeks bereikt mee. Wanneer een browser een HTTP-pagina van FrontEnd001.CloudApp.Net aanvraagt, geeft het HTTP-eindpunt (poort 80) dit verkeer aan de firewall niet op de webserver. De firewall vervolgens volgens de regel die eerder is gemaakt, NAT's die naar de webserver aanvragen.
> 
> 

## <a name="traffic-scenarios"></a>Verkeer scenario 's
#### <a name="allowed-web-to-web-server-through-firewall"></a>(Toegestaan) Web-Web-Server via de Firewall
1. Internet aanvragen HTTP-gebruikerspagina van FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud-service geeft verkeer via open eindpunten op poort 80 voor lokale firewall-interface op 10.0.1.4:80
3. Subnet frontend begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet aan Firewall) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de firewall (10.0.1.4)
5. Doorsturen via firewallregel Zie dit netwerkverkeer via de poort 80 is, stuurt het naar de webserver IIS01
6. IIS01 luistert voor internetverkeer, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
7. IIS01 vraagt de SQL-Server op AppVM01 voor meer informatie
8. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
9. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u de verwerking van regels
10. AppVM01 ontvangen van de SQL-Query en beantwoord
11. Aangezien er geen uitgaande regels op het subnet voor back-end van zijn is de reactie toegestaan
12. Subnet frontend begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
13. De SQL-respons ontvangt de IIS-server en het HTTP-antwoord is voltooid en verzendt naar de aanvrager
14. Aangezien dit een NAT-sessie van de firewall, is doellocatie van het antwoord (in eerste instantie) voor de Firewall
15. De firewall ontvangt van het antwoord van de webserver en stuurt terug naar de Internet-gebruiker
16. Omdat er geen uitgaande regels op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker ontvangt de aangevraagde webpagina.

#### <a name="allowed-rdp-to-backend"></a>(Toegestaan) RDP met back-end
1. De serverbeheerder op internet aanvragen RDP-sessie op AppVM01 op BackEnd001.CloudApp.Net:xxxxx waarbij xxxxx het willekeurig toegewezen poort voor RDP naar AppVM01 is (de toegewezen poort kan worden gevonden in de Azure Portal of via PowerShell)
2. Aangezien de Firewall alleen op het adres FrontEnd001.CloudApp.Net luistert, is niet verbonden met dit netwerkverkeer
3. Subnet backend begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Er is geen uitgaande regels standaardregels toepassen en terugkerend verkeer is toegestaan
5. RDP-sessie is ingeschakeld
6. AppVM01 gebruikersnaam wachtwoord gevraagd

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server DNS-lookup op DNS-server
1. Web Server, IIS01, moet een gegevensfeed op www.data.gov, maar moet u het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op het subnet voor back-end) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
4. Subnet backend begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
5. DNS-server ontvangt de aanvraag
6. DNS-server beschikt niet over het adres in de cache opgeslagen en wordt u gevraagd een basis-DNS-server op het internet
7. Er is geen uitgaande regels op subnet Backend verkeer wordt toegestaan.
8. Internet-DNS-server reageert, omdat deze sessie intern is gestart, is het antwoord toegestaan
9. DNS-server het antwoord-cache en het reageert op de eerste aanvraag terug naar IIS01
10. Er is geen uitgaande regels op subnet Backend verkeer wordt toegestaan.
11. Subnet frontend begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan
12. IIS01 ontvangt het antwoord van DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Toegestaan) Web Server toegang tot bestand op AppVM01
1. IIS01 vraagt om een bestand op AppVM01
2. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
3. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u de verwerking van regels
4. AppVM01 ontvangt de aanvraag en reageert met bestand (ervan uitgaande dat toegang wordt geverifieerd)
5. Aangezien er geen uitgaande regels op het subnet voor back-end van zijn is de reactie toegestaan
6. Subnet frontend begint verwerking van inkomende regel:
   1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
   2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7. De IIS-server ontvangt het bestand

#### <a name="denied-web-direct-to-web-server"></a>(Geweigerd) Web rechtstreeks naar de webserver
Aangezien de webserver, IIS01 en de Firewall in de dezelfde Cloudservice zijn delen ze hetzelfde openbare internetgerichte IP-adres. Zo zou HTTP-verkeer worden omgeleid naar de firewall. Terwijl de aanvraag zou met succes worden geleverd, deze kan niet gaat u rechtstreeks naar de webserver, deze doorgegeven, die is ontworpen, via de Firewall eerst. Zie het eerste Scenario in dit gedeelte voor het netwerkverkeer.

#### <a name="denied-web-to-backend-server"></a>(Geweigerd) Web naar back-endserver
1. Internet-gebruiker probeert te krijgen van een bestand op AppVM01 via de service BackEnd001.CloudApp.Net
2. Aangezien er geen eindpunten voor de bestandsshare is geopend zijn, dit zou niet doorgeven voor de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, wordt NSG regel 5 (Internet naar VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Geweigerd) Web DNS-zoekopdracht op DNS-server
1. Internet-gebruiker probeert een interne DNS-record op DNS01 via de service BackEnd001.CloudApp.Net opzoeken
2. Aangezien er geen eindpunten voor DNS is geopend zijn, dit zou niet doorgeven voor de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, NSG regel 5 (Internet naar VNet) dit verkeer wordt geblokkeerd (Opmerking: of regel 1 (DNS) is niet van toepassing om twee redenen, eerst het bronadres is met het internet, met deze regel is alleen van toepassing op de lokale VNet als de bron, dit is tevens een regel voor toestaan, zodat deze zou nooit verkeer weigeren)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Geweigerd) Web op SQL-toegang via de Firewall
1. Internet-gebruiker opvraagt SQL-gegevens uit FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Omdat er geen eindpunten geopend voor SQL zijn, dit zou niet doorgeven voor de Cloudservice en de firewall wouldn't bereiken
3. Als eindpunten geopend voor een bepaalde reden zijn, begint het subnet Frontend verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 2 (Internet aan Firewall) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de firewall (10.0.1.4)
5. Firewall heeft geen regels voor doorsturen voor SQL en wordt het verkeer

## <a name="conclusion"></a>Conclusie
Dit is een relatief eenvoudig en duidelijk van het beveiligen van uw toepassing met een firewall en het subnet van de back-end van binnenkomend verkeer isoleren.

Meer voorbeelden en een overzicht van het netwerk beveiligingsgrenzen vindt [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en netwerkconfiguratie
Sla het volledige Script in een PowerShell-scriptbestand. De netwerkconfiguratie opslaan in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen zo nodig wijzigen. Voer het script uit en volg de bovenstaande van Firewall regel setup instructies.

#### <a name="full-script"></a>Volledige Script
Dit script wordt, op basis van de gebruiker gedefinieerde variabelen:

1. Verbinding maken met een Azure-abonnement
2. Een nieuw opslagaccount maken
3. Maak een nieuw VNet en twee subnets zoals gedefinieerd in het netwerk-configuratiebestand
4. 4 windows server-VM's maken
5. Configureer het NSG inclusief:
   * Maken van een NSG
   * Met regels in te vullen
   * Het NSG binden aan de juiste subnetten

Deze PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of de server.

> [!IMPORTANT]
> Wanneer dit script wordt uitgevoerd, kunnen er waarschuwingen of andere informatieve berichten die pop in PowerShell. Alleen foutberichten in rood zijn aanleiding.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

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

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

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
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkomende DMZ met NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Bestemming NAT-pictogram"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewallregel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Activering van de firewall-regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
