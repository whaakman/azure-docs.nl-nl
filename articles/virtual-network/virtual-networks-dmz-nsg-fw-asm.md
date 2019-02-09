---
title: Voorbeeld van DMZ – een DMZ toepassingen met een Firewall en nsg's te beschermen bouwen | Microsoft Docs
description: Bouw een DMZ met Firewall en Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 31d945f64cccd0c811d4dc45163583224102fb8a
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965236"
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: een DMZ toepassingen met een Firewall en nsg's te beschermen bouwen
[Ga terug naar de grens Best Practices pagina][HOME]

In dit voorbeeld wordt een DMZ met firewall, vier windows-servers en Netwerkbeveiligingsgroepen maken. Het helpt ook bij elk van de relevante opdrachten voor een beter begrip van elke stap. Er is ook een sectie verkeer Scenario voor een gedetailleerde stapsgewijze hoe verkeer wordt uitgevoerd in de defensieve in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige code en instructies voor het bouwen van deze omgeving om te testen en te experimenteren met verschillende scenario's. 

![DMZ met NVA en in de Netwerkbeveiligingsgroep inkomende][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat u het volgende bevat:

* Twee cloudservices: "FrontEnd001" en "BackEnd001"
* Een Virtueelnetwerk 'CorpNetwork', met twee subnetten: 'FrontEnd' en 'Back-end'
* Een één Netwerkbeveiligingsgroep die is toegepast op beide subnetten
* Een virtueel netwerkapparaat, in dit voorbeeld een Barracuda NextGen Firewall, die is verbonden met het subnet Frontend
* Een Windows-Server die staat voor een toepassing webserver ("IIS01")
* Twee windows-servers die staan voor toepassing back end servers ("AppVM01", "AppVM02")
* Een Windows-server die staat voor een DNS-server ("DNS01")

> [!NOTE]
> Hoewel dit voorbeeld wordt een Barracuda NextGen Firewall gebruikt, kunnen veel van de verschillende virtuele netwerkapparaten worden gebruikt voor dit voorbeeld.
> 
> 

In het gedeelte hieronder met verwijzingen wordt er een PowerShell-script dat wordt gemaakt van de meeste van de omgeving die hierboven worden beschreven. Het bouwen van de virtuele machines en virtuele netwerken, maar moeten worden uitgevoerd door het voorbeeldscript worden niet beschreven in dit document.

De omgeving bouwen:

1. Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario)
2. Bijwerken van de Gebruikersvariabelen voor de in het script moet overeenkomen met de omgeving die het script uitgevoerd op basis van wordt (abonnementen, servicenamen, enzovoort)
3. Voer het script uit in PowerShell

**Opmerking**: De regio aangegeven in het PowerShell-script moet overeenkomen met de regio aangegeven in het netwerk van het XML-configuratiebestand.

Nadat het script is uitgevoerd, de volgende stappen van script dat volgt uit kunnen worden genomen:

1. De firewall-regels hebt ingesteld, wordt dit in de onderstaande sectie met de titel behandeld: Firewall-regels.
2. (Optioneel) zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en appserver met een eenvoudige webtoepassing waarmee de testen met deze configuratie DMZ.

De volgende sectie wordt uitgelegd dat de meeste scripts instructies ten opzichte van Netwerkbeveiligingsgroepen.

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een NSG-groep gemaakt en vervolgens geladen met zes regels. 

> [!TIP]
> In het algemeen moet u eerst uw specifieke 'Toestaan'-regels maken en vervolgens de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels worden eerst geëvalueerd. Wanneer verkeer toe te passen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in beide in de richting binnenkomend of uitgaand (vanuit het perspectief van het subnet).
> 
> 

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan
2. RDP-verkeer (poort 3389) van het Internet op elke virtuele machine is toegestaan
3. HTTP-verkeer (poort 80) van het Internet naar de NVA (firewall) is toegestaan
4. Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan
5. Al het verkeer (alle poorten) van het Internet naar de volledige VNet (beide subnetten) is geweigerd
6. Al het verkeer (alle poorten) vanaf het subnet Frontend naar de back-end-subnet wordt geweigerd

Met deze regels gekoppeld aan elk subnet, als een HTTP-aanvraag binnenkomend verkeer van Internet naar de webserver, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen, maar aangezien regel 3 een hogere prioriteit heeft alleen zou worden toegepast en regel 5 niet aan de orde komt. De HTTP-aanvraag zou dus worden toegestaan aan de firewall. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) is de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) Hiermee blokkeert u de front-endsubnet van de communicatie met de back-end-subnet (met uitzondering van toegestaan verkeer in regels 1 en 4), dit beveiligt de back-end-netwerk als een aanvaller inbreuk de web-App op het front-end, de aanvaller zou hebben beperkte toegang tot de back-end '-beveiliging gebruiken' netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. In dit voorbeeld zijn we uitgaand verkeer wordt toegestaan en alle regels voor uitgaand verkeer niet wordt gewijzigd. Om verkeer in beide richtingen, de gebruiker gedefinieerde routering is vereist, dit is verkend in een ander voorbeeld die te vinden in de [belangrijkste security grens document][HOME].

De hierboven besproken NSG-regels zijn vergelijkbaar met de NSG-regels in [voorbeeld 1: een eenvoudige DMZ met nsg's bouwen][Example1]. Controleer de beschrijving van de NSG in dit document voor een gedetailleerde Kijk op elke NSG-regel en de kenmerken.

## <a name="firewall-rules"></a>Firewallregels
Een management-client moet worden geïnstalleerd op een PC voor het beheren van de firewall en het maken van de configuraties die nodig zijn. Zie de documentatie van uw firewall (of andere NVA)-leverancier over het beheren van het apparaat. De rest van deze sectie beschrijft de configuratie van de firewall zelf, via de leveranciers management-client (dat wil zeggen niet in de Azure portal of PowerShell).

Instructies voor de client downloaden en verbinding maken met de Barracuda gebruikt in dit voorbeeld kunt u hier vinden: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Op de firewall moet regels voor doorsturen worden gemaakt. Omdat in dit voorbeeld worden alleen verkeer van internet in-gebonden aan de firewall en vervolgens naar de webserver routes, wordt slechts één doorsturen NAT-regel die nodig zijn. Op de Barracuda NextGen Firewall gebruikt in dit voorbeeld zou de regel een bestemming NAT-regel ("Dst NAT') om door te geven dat het verkeer.

Als u wilt maken met de volgende regel (of Controleer of de bestaande standaardregels die), vanaf het dashboard van de client Barracuda NG beheerder gaat u naar het tabblad configuratie, in de configuratie van de operationele sectie klikt u op Ruleset. Een raster met de naam 'Main-regels worden de bestaande regels die actief is en gedeactiveerd weergegeven op de firewall. In de rechterbovenhoek van dit raster is een kleine groene '+' knop, klik hierop om een nieuwe regel te maken (Opmerking: uw firewall mogelijk "vergrendeld" wijzigingen zijn aangebracht, als er een knop gemarkeerd als 'Vergrendelen' en u zich niet maken of bewerken van regels, klik op deze knop om het ruleset 'ontgrendelen' en bewerken toestaan). Als u een bestaande regel bewerken wilt, selecteert u deze regel, met de rechtermuisknop op en selecteer regel bewerken.

Maak een nieuwe regel en geef een naam, zoals 'WebTraffic'. 

Het pictogram van de bestemming NAT-regel ziet er als volgt: ![Bestemming NAT-pictogram][2]

De regel zelf zou er ongeveer als volgt uit:

![Firewall-regel][3]

Hier een adres dat komt binnen via de Firewall inkomend probeert te bereiken HTTP (poort 80 en 443 voor HTTPS) worden verzonden van de Firewall 'DHCP1 lokale IP'-interface en omgeleid naar de webserver met het IP-adres van 10.0.1.5. Omdat het verkeer wordt binnenkort op poort 80 en Ga naar de webserver op poort 80 is geen poortwijziging nodig. Echter, de doellijst kan zijn 10.0.1.5:8080 als onze webserver geluisterd op poort 8080 dus vertalen van de binnenkomende poort 80 op de firewall op de binnenkomende poort 8080 op de webserver.

Een verbindingsmethode moet ook worden aangegeven, voor de doel-regel van het Internet "Dynamische SNAT" is het meest geschikt. 

Hoewel u slechts één regel is gemaakt is het belangrijk dat de prioriteit correct is ingesteld. Als in het raster van alle regels op de firewall voor dat deze nieuwe regel is op de onderste (hieronder de regel 'BLOCKALL') er nooit aan de orde komen. Zorg ervoor dat de nieuwe regel voor webverkeer hoger is dan de BLOCKALL-regel.

Als de regel is gemaakt, moet worden gepusht naar de firewall en wordt geactiveerd, als dit niet is gedaan de wijziging van de regel niet door te voeren. Het proces van push- en -activering wordt in de volgende sectie beschreven.

## <a name="rule-activation"></a>Activering van de regel
Met het ruleset om toe te voegen met deze regel is gewijzigd, moet de regelset worden geüpload naar de firewall en geactiveerd.

![Activering van Firewall-regel][4]

In de rechterbovenhoek van de management-client zijn een cluster van de knoppen. Klik op de knop 'Wijzigingen verzenden' voor het verzenden van de gewijzigde regels aan de firewall en klik vervolgens op de knop 'Activeren'.

Met de activering van de regelset van de firewall zijn in dit voorbeeld omgeving build is voltooid. (Optioneel) de post-build-scripts in de sectie Verwijzingen kunnen worden uitgevoerd voor een toepassing toevoegen aan deze omgeving voor het testen van de onderstaande scenario's voor verkeer.

> [!IMPORTANT]
> Het is essentieel om te profiteren van dat niet u de webserver rechtstreeks bereikt. Wanneer een browser een HTTP-pagina van FrontEnd001.CloudApp.Net aanvraagt, geeft het HTTP-eindpunt (poort 80) dit verkeer aan de firewall niet op de webserver. De firewall vervolgens, op basis van de regel hierboven gemaakte NAT's die naar de webserver aanvragen.
> 
> 

## <a name="traffic-scenarios"></a>Scenario's voor verkeer
#### <a name="allowed-web-to-web-server-through-firewall"></a>(Toegestaan) Web op het Web Server via Firewall
1. Internet gebruiker aanvragen HTTP-pagina van FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service-passen verkeer via de open-eindpunt op poort 80 op de lokale firewall-interface op 10.0.1.4:80
3. Front-endsubnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan Firewall) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Verkeer komt binnen via het interne IP-adres van de firewall (10.0.1.4)
5. Regel voor doorsturen via een firewall zien dit netwerkverkeer via poort 80 is, wordt omgeleid naar de webserver IIS01
6. IIS01 luistert voor webverkeer te genereren, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
7. IIS01 vraagt de SQL-Server op AppVM01 voor meer informatie
8. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
9. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u regelverwerking
10. AppVM01 ontvangt van de SQL-Query en reageert
11. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet is het antwoord toegestaan
12. Front-endsubnet begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
13. De IIS-server, SQL-antwoord is ontvangen en het HTTP-antwoord is voltooid en wordt verzonden naar de aanvrager
14. Omdat dit een NAT-sessie van de firewall, is het doel van het antwoord (in eerste instantie) voor de Firewall
15. De firewall-antwoord is ontvangen van de webserver en doorgestuurd naar de Internet-gebruiker
16. Omdat er geen regels voor uitgaand verkeer op het subnet Frontend het antwoord is toegestaan en de Internet-gebruiker ontvangt de webpagina die is aangevraagd.

#### <a name="allowed-rdp-to-backend"></a>(Toegestaan) RDP naar back-end
1. De serverbeheerder op internet vraagt RDP-sessie naar AppVM01 op BackEnd001.CloudApp.Net:xxxxx waarbij xxxxx het willekeurig toegewezen poort voor RDP-verbinding AppVM01 is (de toegewezen poort kan worden gevonden in de Azure Portal of via PowerShell)
2. Omdat de Firewall alleen op het adres FrontEnd001.CloudApp.Net luistert, is niet verbonden met deze verkeersstroom
3. Back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Er zijn geen regels voor uitgaand verkeer, standaardregels toepassen en retourverkeer is toegestaan
5. RDP-sessie is ingeschakeld
6. AppVM01 vraagt om wachtwoord van de naam van gebruiker

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web Server-DNS-zoekactie op DNS-server
1. Web Server, IIS01, een gegevensfeed op www.data.gov behoeften, maar moet het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op de back-end-subnet) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
4. Back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) is van toepassing, verkeer is toegestaan, stop regelverwerking
5. DNS-server ontvangt de aanvraag
6. DNS-server beschikt niet over het adres in de cache opgeslagen en wordt u gevraagd een basis-DNS-server op het internet
7. Er is geen uitgaande regels op back-end-subnet, verkeer is toegestaan
8. Internet-DNS-server reageert, omdat deze sessie intern is gestart, is het antwoord toegestaan
9. DNS-server plaatst het antwoord en reageert op de eerste aanvraag terug naar IIS01
10. Er is geen uitgaande regels op back-end-subnet, verkeer is toegestaan
11. Front-endsubnet begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan
12. IIS01-antwoord is ontvangen van DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Toegestaan) Web Server-databasebestand op AppVM01
1. IIS01 vraagt om een bestand op AppVM01
2. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
3. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u regelverwerking
4. AppVM01 ontvangt de aanvraag en reageert met een bestand (ervan uitgaande dat de toegang geautoriseerd)
5. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet is het antwoord toegestaan
6. Front-endsubnet begint verwerking van inkomende regel:
   1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
   2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7. De IIS-server ontvangt het bestand

#### <a name="denied-web-direct-to-web-server"></a>(Geweigerd) Web direct naar de webserver
Omdat de webserver, IIS01 en de Firewall in dezelfde Cloudservice zijn delen ze het hetzelfde openbare IP-adres. Zo zou een HTTP-verkeer worden omgeleid naar de firewall. Terwijl de aanvraag zou met succes worden uitgevoerd, het kan geen Ga rechtstreeks naar de webserver, wordt doorgegeven, zoals ingesteld, via de Firewall eerst. Zie het eerste Scenario in deze sectie voor het netwerkverkeer.

#### <a name="denied-web-to-backend-server"></a>(Geweigerd) Web naar back-endserver
1. Internet-gebruiker probeert te krijgen van een bestand op AppVM01 via de service BackEnd001.CloudApp.Net
2. Omdat er geen eindpunten openen voor bestandsshare zijn, dit zou de Cloudservice niet doorgeven en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een of andere reden zijn, blokkeren NSG-regel 5 (Internet naar VNet) dat het verkeer

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Geweigerd) DNS-zoekactie web op DNS-server
1. Internet-gebruiker probeert voor het opzoeken van een interne DNS-record op DNS01 via de service BackEnd001.CloudApp.Net
2. Omdat er geen eindpunten openen voor DNS zijn, dit zou de Cloudservice niet doorgeven en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een of andere reden zijn, NSG-regel 5 (Internet naar VNet) dit verkeer wilt blokkeren (Opmerking: deze regel 1 (DNS) is niet van toepassing voor twee doeleinden, eerst het bron-adres is het internet, met deze regel is alleen van toepassing op de lokale VNet als de bron Dit is ook een regel voor toestaan, zodat deze zouden nooit verkeer weigeren)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Geweigerd) Web toegang tot SQL via Firewall
1. Internetgebruiker opvraagt SQL gegevens uit FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Omdat er geen eindpunten openen voor SQL zijn, dit zou niet doorgeven voor de Cloudservice en de firewall wouldn't bereiken
3. Als u eindpunten zijn geopend voor een of andere reden, begint het subnet Frontend verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 2 (Internet aan Firewall) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Verkeer komt binnen via het interne IP-adres van de firewall (10.0.1.4)
5. Firewall heeft geen regels voor het doorsturen voor SQL en het verkeer komt

## <a name="conclusion"></a>Conclusie
Dit is een relatief eenvoudig voorwaarts manier van het beveiligen van uw toepassing met een firewall en het back-end-subnet van het binnenkomende verkeer isoleren.

Meer voorbeelden en een overzicht van beveiliging netwerkgrenzen vindt [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste Script en de netwerkconfiguratie
Het volledige Script opslaan in een PowerShell-scriptbestand. Sla de netwerk-configuratie in een bestand met de naam 'NetworkConf2.xml'.
De gebruiker gedefinieerde variabelen zo nodig wijzigen. Voer het script uit en volg de bovenstaande voor setup instructies van Firewall-regel.

#### <a name="full-script"></a>Volledige Script
Met dit script wordt op basis van de gebruiker gedefinieerde variabelen:

1. Verbinding maken met een Azure-abonnement
2. Een nieuw opslagaccount maken
3. Een nieuw VNet en twee subnetten, zoals gedefinieerd in het netwerk-configuratiebestand maken
4. 4 windows server-VM's bouwen
5. Configureren van NSG met inbegrip van:
   * Het maken van een NSG
   * Het vullen met regels
   * De NSG-binding aan de juiste subnetten

Dit PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of de server.

> [!IMPORTANT]
> Wanneer dit script wordt uitgevoerd, zijn er mogelijk andere informatieve berichten die de pop-in PowerShell of waarschuwingen worden gegenereerd. Alleen foutberichten worden weergegeven in het rood zijn oorzaak zouden kunnen gaan maken.
> 
> 

```powershell
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
```

#### <a name="network-config-file"></a>Netwerk-configuratiebestand
Dit xml-bestand opslaan met bijgewerkte locatie en de koppeling naar dit bestand naar de variabele $NetworkConfigFile in het bovenstaande script toevoegen.

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

#### <a name="sample-application-scripts"></a>Voorbeeldscripts voor toepassing
Als u een voorbeeldtoepassing voor deze en andere voorbeelden DMZ installeren wilt, is een opgegeven op de volgende koppeling: [Voorbeeldscript voor toepassing][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkomende DMZ met NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Bestemming NAT-pictogram"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall-regel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Activering van Firewall-regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
