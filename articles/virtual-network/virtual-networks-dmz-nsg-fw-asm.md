---
title: "Perimeter network voorbeeld: Build een perimeternetwerk ter bescherming van toepassingen met een firewall en nsg's | Microsoft Docs"
description: Bouw een perimeternetwerk met een firewall en Netwerkbeveiligingsgroepen (nsg's)
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
ms.openlocfilehash: c1c64945aaa0bc4cd83cc769dab1c2a755896c01
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57442475"
---
# <a name="example-2-build-a-perimeter-network-to-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: Een perimeternetwerk ter bescherming van toepassingen met een firewall en nsg's maken
[Ga terug naar de Microsoft-cloudservices en beveiligingspagina netwerk][HOME]

Dit voorbeeld wordt het maken van een perimeternetwerk (ook wel bekend als *DMZ*, *gedemilitariseerde zone*, en *screened subnet genoemd*) met een firewall, vier Windows Server-computers en Netwerkbeveiligingsgroepen (nsg's). Het bevat details over elk van de relevante opdrachten voor een beter begrip van elke stap. De sectie 'Scenario's voor verkeer' biedt een stapsgewijze uitleg van hoe verkeer wordt uitgevoerd in de defensieve in het perimeternetwerk. Ten slotte biedt de sectie "Verwijzingen" de volledige code en instructies voor het bouwen van deze omgeving om te testen en te experimenteren met verschillende scenario's.

![Inkomende perimeternetwerk met NVA en nsg 's][1]

## <a name="environment"></a>Omgeving 
In dit voorbeeld is gebaseerd op een scenario met een Azure-abonnement dat bevat de volgende items:

* Twee cloudservices: FrontEnd001 and BackEnd001.
* Een virtueel netwerk met de naam CorpNetwork met twee subnetten: Front-end- en back-end.
* Een NSG die is één dat wordt toegepast op beide subnetten.
* Een virtueel netwerkapparaat: Barracuda NextGen Firewall verbonden aan het FrontEnd-subnet.
* Een Windows Server-computer die een web application server vertegenwoordigt: IIS01.
* Twee Windows Server-computers die staan voor back-end-toepassingsservers: AppVM01 en AppVM02.
* Een Windows Server-computer met een DNS-server: DNS01.

> [!NOTE]
> Hoewel dit voorbeeld wordt een Barracuda NextGen Firewall gebruikt, kunnen veel virtuele netwerkapparaten worden gebruikt.
> 
> 

Het PowerShell-script in de sectie "Verwijzingen" van dit artikel maakt de meeste van de omgeving die hier wordt beschreven. De virtuele machines en virtuele netwerken zijn gebouwd door het script, maar deze processen worden niet in dit document gedetailleerd beschreven.

De omgeving bouwen:

1. Het netwerk config XML-bestand zijn opgenomen in de sectie "Verwijzingen" (bijgewerkt met de namen, locaties en IP-adressen zodat deze overeenkomt met uw scenario).
2. Update de gebruiker gedefinieerde variabelen in het PowerShell-script, zodat deze overeenkomen met de omgeving van het script wordt uitgevoerd op (abonnementen, servicenamen, enzovoort).
3. Voer het script in PowerShell.

> [!NOTE]
> De regio die is opgegeven in het PowerShell-script moet overeenkomen met de regio die is opgegeven in het netwerk van het XML-configuratiebestand.
>
>

Nadat het script is uitgevoerd, kunt u deze stappen voltooien:

1. Instellen van de firewall-regels. Zie de sectie 'Firewall-regels' van dit artikel.
2. Als u wilt, kunt u de webserver en appserver met een eenvoudige webtoepassing waarmee de testen met de configuratie van het perimeternetwerk instellen. U kunt de twee toepassingsscripts vindt u in de sectie "Verwijzingen" gebruiken.

De volgende sectie wordt uitgelegd dat de meeste van de instructies van het script die gerelateerd aan de nsg's zijn.

## <a name="nsgs"></a>NSG's
In dit voorbeeld is een NSG-groep gemaakt en vervolgens geladen met zes regels.

> [!TIP]
> In het algemeen moet u eerst uw specifieke 'Toestaan'-regels maken en de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels worden eerst geëvalueerd. Nadat het verkeer wordt gevonden die van toepassing is op een specifieke regel, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in het inkomende of in de uitgaande richting (vanuit het perspectief van het subnet).
> 
> 

Deze regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan.
2. RDP-verkeer (poort 3389) van het internet op elke virtuele machine is toegestaan.
3. HTTP-verkeer (poort 80) van het internet naar de NVA (firewall) is toegestaan.
4. Alle verkeer (alle poorten) van IIS01 naar AppVM01 is toegestaan.
5. Al het verkeer (alle poorten) van het internet naar het hele virtuele netwerk (beide subnetten) is geweigerd.
6. Al het verkeer (alle poorten) vanaf het subnet FrontEnd naar de back-end-subnet wordt geweigerd.

Met deze regels die zijn gekoppeld aan elk subnet, als een HTTP-aanvraag binnenkomend verkeer van internet naar de webserver zijn beide regel 3 (toestaan) en regel 5 (weigeren) lijkt om toe te passen, maar omdat regel 3 een hogere prioriteit heeft, alleen wordt toegepast. Regel 5 wordt niet aan de orde komen. De HTTP-aanvraag wordt zo mag worden de firewall.

Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) is de eerste om toe te passen, zodat het verkeer wouldn't mag worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het subnet FrontEnd communiceert met de back-end-subnet (met uitzondering van verkeer dat is toegestaan in regels 1 en 4). Dit beveiligt de back-end-netwerk als een aanvaller de web-App op de front-end wordt aangetast. In dat geval zou de aanvaller hebben beperkte toegang tot de back-end 'beveiligd' netwerk. (De aanvaller zou kunnen alleen toegang tot de bronnen weergegeven op de server AppVM01.)

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. In dit voorbeeld zijn we uitgaand verkeer wordt toegestaan en alle regels voor uitgaand verkeer niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, moet u de gebruiker gedefinieerde routering. U kunt meer informatie over deze techniek in een ander voorbeeld in de [belangrijkste security grens document][HOME].

De NSG-regels die hier worden beschreven zijn vergelijkbaar met de NSG-regels in [voorbeeld 1: een eenvoudige DMZ met nsg's bouwen][Example1]. Lees de beschrijving van de NSG in dit artikel voor een gedetailleerde Kijk op elke NSG-regel en de bijbehorende kenmerken.

## <a name="firewall-rules"></a>Firewall-regels
U moet een management-client installeren op een computer voor het beheren van de firewall en het maken van de configuraties die nodig zijn. Zie de documentatie van uw firewall (of andere NVA) leverancier over het beheren van het apparaat. De rest van deze sectie beschrijft de configuratie van de firewall zelf, door middel van de leverancier management-client (niet in de Azure portal of PowerShell).

Zie [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin) voor instructies voor het downloaden van de client en verbinding maken met de Barracuda-firewall in dit voorbeeld gebruikt.

U moet maken van regels voor het doorsturen voor de firewall. Omdat het scenario in dit voorbeeld alleen internetverkeer routeert binnenkomende verbindingen naar de firewall en vervolgens naar de webserver, hoeft u alleen een doorsturen NAT-regel. Op de Barracuda-firewall gebruikt in dit voorbeeld, zou de regel een bestemming NAT-regel (zomertijd NAT) om door te geven dat het verkeer.

De volgende regel maken (of om te controleren of de bestaande standaardregels die), voert u deze stappen uit:
1. De Barracuda NG-client-Beheerdashboard op het tabblad configuratie in de **operationele configuratie** sectie, selecteer **Ruleset**. 

   Een raster met de naam **Main regels** active ziet u de bestaande en regels op de firewall gedeactiveerd.

2. Voor het maken van een nieuwe regel, selecteer de kleine groene **+** knop in de rechterbovenhoek van dit raster. (Uw firewall kan worden vergrendeld. Als u een knop gemarkeerd ziet **vergrendeling** en kan niet worden gemaakt of regels bewerken, selecteert u de knop om te ontgrendelen van het ruleset en bewerken.)
  
3. Als u wilt een bestaande regel wilt bewerken, selecteert u de regel, klik met de rechtermuisknop en selecteer vervolgens **regel bewerken**.

Maak een nieuwe regel met de naam er ongeveer als **WebTraffic.** 

Het pictogram van de bestemming NAT-regel ziet er als volgt: ![Bestemming NAT-pictogram][2]

De regel zelf ziet er ongeveer als volgt uit:

![Firewall-regel][3]

Alle inkomende adres dat komt binnen via de firewall probeert te bereiken HTTP (poort 80 en 443 voor HTTPS) wordt omgeleid naar de webserver met het IP-adres 10.0.1.5 zijn en worden verzonden buiten de firewall van DHCP1 lokale IP-interface. Omdat het verkeer op poort 80 is beschikbaar en gaat naar de webserver op poort 80, er is geen poortwijziging is vereist. Maar de lijst met doelservers kan zijn 10.0.1.5:8080 als de webserver geluisterd op poort 8080, de binnenkomende poort 80 op de firewall op de binnenkomende poort 8080 op de webserver vertaalt.

U moet ook een methode voor verbinding opgeven. Voor de doel-regel van het internet is dynamische SNAT het meest geschikte methode.

Hoewel u slechts één regel hebt gemaakt, is het belangrijk dat u de prioriteit correct ingesteld. In het raster met alle regels op de firewall, als deze nieuwe regel aan de onderkant (hieronder de regel BLOCKALL), is komen deze nooit aan de orde. Zorg ervoor dat de nieuwe regel voor webverkeer hoger is dan de BLOCKALL-regel.

Nadat de regel is gemaakt, moet u deze naar de firewall pushen en vervolgens te activeren. Als u deze stappen niet uitvoeren, worden de regel wijziging pas van kracht. De volgende sectie worden de push- en -activering beschreven.

## <a name="rule-activation"></a>Activering van de regel
Nu dat de regel is toegevoegd aan de regelset, moet u het ruleset uploaden naar de firewall en activeer deze.

![Activering van Firewall-regel][4]

In de rechterbovenhoek van de management-client ziet u een groep keuzerondjes. Selecteer **wijzigingen verzenden** voor het verzenden van de gewijzigde ruleset aan de firewall, en selecteer vervolgens **activeren**.

Nu dat u de firewall ruleset hebt geactiveerd, wordt de omgeving is voltooid. Als u wilt, kunt u de toepassing voorbeeldscripts kunt uitvoeren in de sectie "Verwijzingen" voor een toepassing toevoegen aan de omgeving. Als u een toepassing hebt toegevoegd, kunt u het verkeer-scenario's beschreven in de volgende sectie testen.

> [!IMPORTANT]
> U moet realiseren dat u de webserver rechtstreeks niet bereikt. Wanneer een browser een HTTP-pagina van FrontEnd001.CloudApp.Net aanvraagt, wordt het verkeer in het HTTP-eindpunt (poort 80) doorgegeven aan de firewall, niet op de webserver. De firewall wordt, vanwege de regel die u eerder hebt gemaakt, NAT om toe te wijzen van de aanvraag naar de webserver.
> 
> 

## <a name="traffic-scenarios"></a>Scenario's voor verkeer
#### <a name="allowed-web-to-web-server-through-the-firewall"></a>(Toegestaan) Web met webserver via de firewall
1. Een internetgebruiker vraagt een HTTP-pagina van FrontEnd001.CloudApp.Net (een internetgerichte cloudservice).
2. De cloudservice wordt verkeer via een eindpunt geopend op poort 80 doorgegeven aan de lokale interface van de firewall op 10.0.1.4:80.
3. Het subnet FrontEnd begint verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   2. NSG-regel 2 (RDP) niet van toepassing. Verplaatsen naar de volgende regel.
   3. NSG-regel 3 (internet aan firewall) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
4. Verkeer komt binnen via het interne IP-adres van de firewall (10.0.1.4).
5. Een regel voor doorsturen firewall bepaalt dat deze netwerkverkeer via de poort 80 en wordt omgeleid naar de webserver IIS01.
6. IIS01 luistert voor webverkeer te genereren, ontvangt de aanvraag en begint met de verwerking van de aanvraag.
7. IIS01 vraagt informatie op van de SQL Server-exemplaar op AppVM01.
8. Er zijn geen regels voor uitgaand verkeer op de front-end-subnet, zodat verkeer is toegestaan.
9. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   2. NSG-regel 2 (RDP) niet van toepassing. Verplaatsen naar de volgende regel.
   3. NSG-regel 3 (internet aan firewall) is niet van toepassing. Verplaatsen naar de volgende regel.
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
10. De SQL Server-exemplaar op AppVM01 ontvangt de aanvraag en reageert.
11. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet, wordt het antwoord is toegestaan.
12. Het subnet FrontEnd begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van de back-end-subnet aan het FrontEnd-subnet, zodat geen van de NSG-regels van toepassing.
    2. De standaardregel systeem voor het verkeer tussen subnetten kunt dit verkeer, zodat verkeer is toegestaan.
13. IIS01-antwoord is ontvangen van AppVM01, voltooit het HTTP-antwoord en verzendt dit naar de aanvrager.
14. Omdat dit een NAT-sessie van de firewall, is het doel van de reactie in eerste instantie voor de firewall.
15. De firewall-antwoord is ontvangen van de webserver en doorgestuurd naar de internet-gebruiker.
16. Omdat er geen regels voor uitgaand verkeer op de front-endsubnet, het antwoord is toegestaan en de internet-gebruiker ontvangt de webpagina wordt weergegeven.

#### <a name="allowed-rdp-to-backend"></a>(Toegestaan) RDP naar back-end
1. Een serverbeheerder op het internet vraagt een RDP-sessie naar AppVM01 op BackEnd001.CloudApp.Net:*xxxxx*, waarbij *xxxxx* is het aantal willekeurig toegewezen poort voor RDP-verbinding AppVM01. (U vindt de toegewezen poort op de Azure-portal of met behulp van PowerShell.)
2. Omdat de firewall alleen op het adres FrontEnd001.CloudApp.Net luistert, het niet betrokken bij dit netwerkverkeer.
3. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   2. NSG-regel 2 (RDP) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
4. Omdat er geen regels voor uitgaand verkeer, wordt de standaardregels toepassen en retourneren verkeer is toegestaan.
5. De RDP-sessie is ingeschakeld.
6. AppVM01 vraagt om een gebruikersnaam en wachtwoord.

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Toegestaan) Web server DNS-zoekactie op DNS-server
1. De web server, IIS01, moeten een feed op www.data.gov maar nodig heeft om het omzetten van het adres.
2. De netwerkconfiguratie voor het virtuele netwerk bevat DNS01 (10.0.2.4 op de back-end-subnet) als de primaire DNS-server. IIS01 verzendt de DNS-aanvraag naar DNS01.
3. Omdat er geen regels voor uitgaand verkeer op de front-endsubnet, is verkeer toegestaan.
4. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
5. De DNS-server ontvangt de aanvraag.
6. De DNS-server beschikt niet over het adres in de cache opgeslagen en vraagt een basis-DNS-server op het internet.
7. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet, wordt verkeer toegestaan.
8. De internet-DNS-server reageert. Omdat de sessie intern is gestart, wordt het antwoord is toegestaan.
9. De DNS-server plaatst het antwoord en reageert op de aanvraag van IIS01.
10. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet, wordt verkeer toegestaan.
11. Het subnet FrontEnd begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van de back-end-subnet aan het FrontEnd-subnet, zodat geen van de NSG-regels van toepassing.
    2. De standaardregel systeem voor het verkeer tussen subnetten kunt dit verkeer, zodat het verkeer is toegestaan.
12. IIS01 ontvangt het antwoord van DNS01.

#### <a name="allowed-web-server-file-access-on-appvm01"></a>(Toegestaan) Toegang tot bestand op AppVM01 webserver
1. IIS01 vraagt een bestand op AppVM01.
2. Omdat er geen regels voor uitgaand verkeer op de front-endsubnet, is verkeer toegestaan.
3. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   2. NSG-regel 2 (RDP) niet van toepassing. Verplaatsen naar de volgende regel.
   3. NSG-regel 3 (internet aan firewall) is niet van toepassing. Verplaatsen naar de volgende regel.
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
4. AppVM01 ontvangt de aanvraag en reageert met het bestand (ervan uitgaande dat de toegang geautoriseerd).
5. Omdat er geen regels voor uitgaand verkeer op de back-end-subnet, wordt het antwoord is toegestaan.
6. Het subnet FrontEnd begint verwerking van inkomende regel:
   1. Er is geen NSG-regel voor binnenkomend verkeer van de back-end-subnet aan het FrontEnd-subnet, zodat geen van de NSG-regels van toepassing.
   2. De standaardregel systeem voor het verkeer tussen subnetten kunt dit verkeer, zodat het verkeer is toegestaan.
7. IIS01 ontvangt het bestand.

#### <a name="denied-web-direct-to-web-server"></a>(Geweigerd) Web direct naar de webserver
Omdat de webserver IIS01 en de firewall zich in dezelfde cloudservice, delen ze hetzelfde openbare IP-adres. Dus wordt een HTTP-verkeer omgeleid naar de firewall. Wanneer een aanvraag zou met succes worden uitgevoerd, kan niet deze Ga rechtstreeks naar de webserver. Dit wordt doorgegeven, zoals ingesteld, via de firewall eerst. Zie het eerste scenario in deze sectie voor het netwerkverkeer.

#### <a name="denied-web-to-backend-server"></a>(Geweigerd) Web naar back-endserver
1. Een internet-gebruiker probeert te krijgen tot een bestand op AppVM01 via de BackEnd001.CloudApp.Net-service.
2. Omdat er geen eindpunten openen voor het delen van bestanden zijn, wordt dit de cloudservice wordt niet doorgegeven en wordt niet de server niet bereiken.
3. Als de eindpunten geopend voor een of andere reden zijn, worden het verkeer geblokkeerd door NSG-regel 5 (internet met virtual network).

#### <a name="denied-web-dns-lookup-on-the-dns-server"></a>(Geweigerd) Web DNS-zoekopdracht op de DNS-server
1. Een internet-gebruiker probeert om te controleren of een interne DNS-record op DNS01 via de BackEnd001.CloudApp.Net-service.
2. Omdat er geen eindpunten openen voor DNS zijn, moet dit de cloudservice wordt niet doorgegeven en wordt niet de server niet bereiken.
3. Als de eindpunten geopend voor een of andere reden zijn, worden het verkeer geblokkeerd door NSG-regel 5 (internet met virtual network). (Regel 1 [DNS] geldt voor twee redenen waarom niet. Eerst het bron-adres is het internet en deze regel geldt alleen als het lokale virtuele netwerk de bron is. Ten tweede wordt met deze regel een regel voor toestaan, zodat deze nooit verkeer weigert.)

#### <a name="denied-web-to-sql-access-through-the-firewall"></a>(Geweigerd) Web op SQL-toegang via de firewall
1. Een internetgebruiker vraagt SQL-gegevens van FrontEnd001.CloudApp.Net (een internetgerichte cloudservice).
2. Omdat er geen eindpunten openen voor SQL zijn, dit wordt niet doorgeven van de cloudservice en de firewall wordt niet bereiken.
3. Als de eindpunten geopend voor een of andere reden zijn, begint het subnet FrontEnd verwerking van inkomende regel:
   1. NSG-regel 1 (DNS) is niet van toepassing. Verplaatsen naar de volgende regel.
   2. NSG-regel 2 (RDP) niet van toepassing. Verplaatsen naar de volgende regel.
   3. NSG-regel 3 (internet aan firewall) is van toepassing. Verkeer toestaan. Regelverwerking stoppen.
4. Verkeer komt binnen via het interne IP-adres van de firewall (10.0.1.4).
5. De firewall heeft geen regels voor het doorsturen voor SQL en het verkeer komt.

## <a name="conclusion"></a>Conclusie
In dit voorbeeld ziet u een relatief eenvoudige manier voor het beveiligen van uw toepassing met een firewall en de back-end-subnet van het binnenkomende verkeer isoleren.

U vindt meer voorbeelden en een overzicht van network beveiligingsgrenzen [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="full-script-and-network-config"></a>Volledige script en netwerk-configuratie
Het volledige script opslaan in een PowerShell-scriptbestand. Sla het script van de configuratie van netwerk in een bestand met de naam NetworkConf2.xml.
De gebruiker gedefinieerd-variabelen desgewenst wijzigen. Voer het script uit en volg de instructies in de sectie 'Firewall-regels' van dit artikel.

#### <a name="full-script"></a>Volledige script
Met dit script, op basis van de gebruiker gedefinieerde variabelen, wordt de volgende stappen uitvoeren:

1. Verbinding maken met een Azure-abonnement.
2. Een opslagaccount maken.
3. Maak een virtueel netwerk en twee subnetten gemaakt, zoals gedefinieerd in het configuratiebestand van het netwerk.
4. Bouw vier Windows Server-VM's.
5. Configureer de NSG. Configuratie is voltooid als volgt:
   * Hiermee maakt u een NSG.
   * Vult de NSG met regels.
   * De Netwerkbeveiligingsgroep koppelt aan de juiste subnetten.

U moet dit PowerShell-script lokaal uitvoeren op een computer met internetverbinding of de server.

> [!IMPORTANT]
> Wanneer u dit script uitvoert, waarschuwingen en andere informatieve berichten kunnen worden weergegeven in PowerShell. U hoeft alleen te worden betrokken over foutberichten die worden weergegeven in het rood.
> 
> 

```powershell
    <# 
     .SYNOPSIS
      Example of a perimeter network and Network Security Groups in an isolated network. (Azure only. No hybrid connections.)

     .DESCRIPTION
      This script will build out a sample perimeter network setup containing:
       - A default storage account for VM disks.
       - Two new cloud services.
       - Two subnets (the FrontEnd and BackEnd subnets).
       - A network virtual appliance (NVA): a Barracuda NextGen Firewall.
       - One server on the FrontEnd subnet (plus the NVA on the FrontEnd subnet).
       - Three servers on the BackEnd subnet.
       - Network Security Groups to allow/deny traffic patterns as declared.

      Before running the script, ensure the network configuration file is created in
      the directory referenced by the $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in many ways. Be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that you can use, but it should not be used for all scenarios. You
      are responsible for assessing your security needs and the appropriate protections
      and then effectively implementing those protections.

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

    # User-Defined Global Variables
      # These should be changed to reflect your subscription and services.
      # Invalid options will fail in the validation section.

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

    # User-Defined VM-Specific Config
        # To ensure proper NSG rule creation later in this script:
        #       - The web server must be VM 1.
        #       - The AppVM1 server must be VM 2.
        #       - The DNS server must be VM 4.
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG rule IP addresses
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
    # No user-defined variables or   #
    # configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create storage account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update subscription pointer to new storage account
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

    # Create virtual network
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create services
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
                # Set up all the endpoints we'll need once we're up and running.
                # Note: Web traffic goes through the firewall, so we'll need to set up an HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: An SSH endpoint is automatically created on port 22 when the appliance is created.
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

      # Add NSG rules
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

        # Assign the NSG to the subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-Script Manual Configuration
      # Configure firewall
      # Install test web app (run post-build script on the IIS server)
      # Install back-end resources (run post-build script on AppVM01)
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
Dit XML-bestand opslaan met bijgewerkte locaties en voegt u een koppeling naar dit bestand in de variabele $NetworkConfigFile in het vorige script.

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
Als u installeren van een voorbeeldtoepassing voor deze en andere perimeter network-voorbeelden wilt, raadpleegt u de [toepassing voorbeeldscript][SampleApp].

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkomende DMZ met NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Bestemming NAT-pictogram"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall-regel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Activering van Firewall-regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
