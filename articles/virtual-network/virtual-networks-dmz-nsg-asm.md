---
title: "Voorbeeld van Azure DMZ – een eenvoudige DMZ met nsg's bouwen | Microsoft Docs"
description: Maken van een DMZ met Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ed172d552e1e4c9ee27c58abcd7ad2d98df21579
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>Voorbeeld 1: een eenvoudige DMZ met nsg's met klassieke PowerShell bouwen
[Ga terug naar de grens van Best Practices beveiligingspagina][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-sjabloon](virtual-networks-dmz-nsg.md)
> * [Klassieke - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

In dit voorbeeld maakt een primitieve DMZ met vier Windows-servers en Netwerkbeveiligingsgroepen. In dit voorbeeld ziet u de relevante PowerShell-opdrachten voor een beter begrip van elke stap. Er is ook een sectie verkeer Scenario voor een gedetailleerd stapsgewijs hoe verkeer wordt uitgevoerd door de lagen verdediging in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige code en de instructies voor het bouwen van deze omgeving om te testen en Experimenteer met verschillende scenario's. 

![Inkomende DMZ met NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld bevat een abonnement op de volgende bronnen:

* Twee cloudservices: 'FrontEnd001' en 'BackEnd001'
* Een virtueel netwerk, 'CorpNetwork' met twee subnetten; 'FrontEnd' en back-end'
* Een Netwerkbeveiligingsgroep dat wordt toegepast op beide subnetten
* Een Windows-Server waarmee een toepassing webserver ('IIS01')
* Twee windows-servers die vertegenwoordigen toepassingsservers van back-end ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')

In de sectie verwijzingen is er een PowerShell-script dat de meeste van de omgeving beschreven in dit voorbeeld is gebaseerd. Opbouwen van de virtuele machines en virtuele netwerken, hoewel worden uitgevoerd door het voorbeeldscript worden niet beschreven in dit document uitvoeriger. 

Voor het bouwen van de omgeving.

1. Het netwerk config XML-bestand zijn opgenomen in de sectie Verwijzingen (bijgewerkt met de naam, locatie en IP-adressen zodat deze overeenkomt met het gegeven scenario)
2. Bijwerken van de gebruikersvariabelen in het script moet overeenkomen met de omgeving die het script wordt uitgevoerd tegen (abonnementen, servicenamen, enz.)
3. Voer het script in PowerShell

>[!Note]
>De regio die wordt aangegeven in het PowerShell-script moet overeenkomen met de regio die wordt aangegeven in het netwerk van het XML-configuratiebestand.
>
>

Nadat het script wordt uitgevoerd is aanvullende optionele stappen kunnen worden genomen, zijn in de sectie Verwijzingen twee scripts voor het instellen van de webserver en de appserver met een eenvoudige webtoepassing om te testen met deze configuratie DMZ toestaan.

De volgende secties bieden een gedetailleerde beschrijving van Netwerkbeveiligingsgroepen en hoe ze werken in dit voorbeeld door de regels van het PowerShell-script doorlopen.

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een groep NSG gebouwd en vervolgens geladen met zes regels. 

> [!TIP]
> In het algemeen moet u eerst uw specifieke regels voor 'Toestaan' maken en vervolgens de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels eerst geëvalueerd. Als verkeer toepassen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen op een of meer in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
> 
> 

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan
2. RDP-verkeer (poort 3389) van het Internet met een virtuele machine is toegestaan
3. HTTP-verkeer (poort 80) van het Internet met webserver (IIS01) is toegestaan
4. Alle verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan
5. Verkeer (alle poorten) van het Internet op de gehele VNet (beide subnetten) is geweigerd.
6. Al het verkeer (alle poorten) van het subnet Frontend naar het back-end-subnet is geweigerd

Met deze regels gekoppeld aan elk subnet als een HTTP-aanvraag inkomende verkeer van Internet naar de webserver, beide regels 3 was (toestaan) en 5 (weigeren) toepassing zou zijn, maar omdat de regel 3 heeft een hogere prioriteit alleen zou worden toegepast en regel 5 niet in de play zou komen. De HTTP-aanvraag zou dus naar de webserver worden toegestaan. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) zijn de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het subnet Frontend uit het praten met het subnet van de back-end (met uitzondering van toegestane regels 1 en 4-verkeer), deze regelset beveiligt het netwerk Backend als een aanvaller inbreuk de webtoepassing op de front-end de aanvaller zou beperkt toegang tot de back-end '-beveiliging gebruiken' netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. Voor dit voorbeeld bent we uitgaand verkeer toestaat en alle regels voor uitgaande verbindingen niet wijzigen. Als u wilt vergrendelen verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist en wordt verkend in "Voorbeeld 3" op de [Best Practices beveiligingspagina-grens][HOME].

Elke regel als volgt in detail beschreven (**Opmerking**: een item in de volgende lijst, te beginnen met een dollarteken (bijvoorbeeld: $NSGName) is een door de gebruiker gedefinieerde variabele van het script in de sectie Verwijzingen van dit document):

1. Een Netwerkbeveiligingsgroep moet eerst worden gemaakt om de regels houdt:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. De eerste regel in dit voorbeeld kunt DNS-verkeer tussen alle interne netwerken voor de DNS-server op het subnet voor back-end. De regel heeft enkele belangrijke parameters:
   
   * 'Typ' geeft aan in welke richting van verkeer met deze regel wordt van kracht. De richting is vanuit het perspectief van de subnet- of virtuele Machine (afhankelijk van waar deze NSG is gekoppeld). Dus als het Type is 'Inkomend' en verkeer is het subnet invoeren, de regel van toepassing en het subnet uitgaand verkeer niet wordt beïnvloed door deze regel.
   * 'Prioriteit', bepaalt de volgorde waarin netwerkverkeer wordt geëvalueerd. Hoe lager het getal hoe hoger de prioriteit. Wanneer een regel van toepassing op een specifieke netwerkverkeer, worden er geen verdere regels verwerkt. Dus als een regel met prioriteit 1 gegevensverkeer, en een regel met prioriteit 2 verkeer weigert, en beide regels zijn van toepassing op het verkeer wordt het verkeer mogen stromen (omdat de regel 1 een hogere prioriteit heeft geduurd effect en geen verdere regels zijn toegepast).
   * "Action" betekent dat verkeer dat is beïnvloed door deze regel is geblokkeerd of toegestaan.

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. Deze regel kunnen de RDP-verkeer van internet naar de RDP-poort op elke server in het subnet gebonden stromen. Deze regel maakt gebruik van twee speciale soorten adresvoorvoegsels; 'VIRTUAL_NETWORK' en 'INTERNET'. Deze tags zijn een eenvoudige manier om een grotere categorie van adresvoorvoegsels.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Deze regel kunnen Binnenkomend internetverkeer om de webserver. Deze regel wordt het routeringsgedrag niet gewijzigd. De regel kunnen alleen verkeer dat is bestemd voor IIS01 om door te geven. Dus als verkeer van Internet had van de webserver als de bestemming voor deze regel zou toe te staan en stoppen met het verwerken van verdere regels. (In de regel met prioriteit 140 alle andere Binnenkomend internetverkeer wordt geblokkeerd). Als u alleen HTTP-verkeer wordt verwerkt, kan deze regel verder worden beperkt zodat alleen bestemming poort 80.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Deze regel kan verkeer door te geven van de server IIS01 aan de AppVM01-server, een hoger regel blokken alle Frontend voor verkeer van de back-end. Deze regel verbeteren als de poort bekend is dat moet worden toegevoegd. Bijvoorbeeld, als de IIS-server alleen SQL Server op AppVM01 roept is, het Doelpoortbereik moet worden gewijzigd van ' * ' (Any) op 1433 (de SQL-poort), waardoor een kleinere inkomende kwetsbaarheid voor aanvallen op AppVM01 of de webtoepassing ooit verdacht.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Deze regel weigert verkeer van het internet voor servers op het netwerk. Aan de regels met prioriteit 110 en 120 is het effect dat alleen binnenkomende internetverkeer naar de firewall en RDP-poorten op servers en -blokken rest. Deze regel is een 'foutveilige' regel voor het blokkeren van alle onverwachte stromen.
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $VNetName VNet from the Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. De laatste regel weigert verkeer van de front-end-subnet met het subnet voor back-end. Aangezien deze regel een inkomende regel alleen is, is (van de back-end voor de Frontend) omgekeerde verkeer toegestaan.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a>Verkeer scenario 's
#### <a name="allowed-internet-to-web-server"></a>(*Toegestane*) Internet naar webserver
1. Een internet-gebruiker aanvragen een pagina http-van FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service geeft verkeer via open eindpunten op poort 80 voor IIS01 (de webserver)
3. Subnet frontend begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet naar IIS01) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de webserver IIS01 (10.0.1.5)
5. IIS01 luistert voor internetverkeer, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
6. IIS01 vraagt de SQL-Server op AppVM01 voor meer informatie
7. Aangezien er geen uitgaande regels op subnet Frontend zijn, is verkeer toegestaan
8. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u de verwerking van regels
9. AppVM01 ontvangen van de SQL-Query en beantwoord
10. Aangezien er geen uitgaande regels op het subnet Backend zijn, is het antwoord toegestaan
11. Subnet frontend begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
12. De SQL-respons ontvangt de IIS-server en het HTTP-antwoord is voltooid en verzendt naar de aanvrager
13. Aangezien er geen uitgaande regels zijn op het subnet Frontend het antwoord is toegestaan en internet de gebruiker ontvangt de aangevraagde webpagina.

#### <a name="allowed-rdp-to-backend"></a>(*Toegestane*) RDP met back-end
1. De serverbeheerder op internet aanvragen RDP-sessie op AppVM01 op BackEnd001.CloudApp.Net:xxxxx waarbij xxxxx het willekeurig toegewezen poort voor RDP naar AppVM01 is (de toegewezen poort kan worden gevonden in de Azure portal of via PowerShell)
2. Subnet backend begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
3. Er is geen uitgaande regels standaardregels toepassen en terugkerend verkeer is toegestaan
4. RDP-sessie is ingeschakeld
5. AppVM01 wordt u gevraagd om de gebruikersnaam en wachtwoord

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Toegestane*) Web server opzoeken in DNS op DNS-server
1. Web Server, IIS01, moet een gegevensfeed op www.data.gov, maar moet u het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op het subnet voor back-end) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
4. Subnet backend begint verwerking van inkomende regel:
   * NSG regel 1 (DNS) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Toegestane*) Web server toegang tot bestand op AppVM01
1. IIS01 vraagt om een bestand op AppVM01
2. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
3. Het back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet naar IIS01) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u de verwerking van regels
4. AppVM01 ontvangt de aanvraag en reageert met bestand (ervan uitgaande dat toegang wordt geverifieerd)
5. Aangezien er geen uitgaande regels op het subnet Backend zijn, is het antwoord toegestaan
6. Subnet frontend begint verwerking van inkomende regel:
   1. Er is geen NSG-regel voor inkomend verkeer van de back-end-subnet met het subnet Frontend, zodat geen van de NSG regels toepassen
   2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7. De IIS-server ontvangt het bestand

#### <a name="denied-web-to-backend-server"></a>(*Geweigerd*) Web naar back-endserver
1. Een internet-gebruiker probeert te krijgen van een bestand op AppVM01 via de service BackEnd001.CloudApp.Net
2. Aangezien er geen eindpunten voor de bestandsshare is geopend zijn, dit verkeer zou niet doorgeven voor de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, wordt NSG regel 5 (Internet naar VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Geweigerd*) opzoeken in Web DNS op DNS-server
1. Een internet-gebruiker probeert een interne DNS-record op DNS01 via de service BackEnd001.CloudApp.Net opzoeken
2. Aangezien er geen eindpunten voor DNS is geopend zijn, dit verkeer zou niet doorgeven voor de Cloudservice en wouldn't de server niet bereiken
3. Als de eindpunten geopend voor een bepaalde reden zijn, NSG regel 5 (Internet naar VNet) dit verkeer wordt geblokkeerd (Opmerking: deze regel 1 (DNS) is niet van toepassing om twee redenen, eerst het bronadres is met het internet, met deze regel is alleen van toepassing op de lokale VNet als de bron Met deze regel is ook een regel voor toestaan, zodat deze zou nooit verkeer weigeren)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Geweigerd*) Web op SQL-toegang via de firewall
1. Een internetgebruiker opvraagt SQL-gegevens uit FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Aangezien er geen eindpunten geopend voor SQL zijn, dit verkeer zou de Cloudservice niet doorgeven en de firewall wouldn't bereiken
3. Als eindpunten geopend voor een bepaalde reden zijn, begint het subnet Frontend verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG regel 3 (Internet naar IIS01) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de IIS01 (10.0.1.5)
5. IIS01 wordt niet geluisterd op poort 1433, dus geen antwoord op de aanvraag

## <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudige en meteen verder manier van het subnet van de back-end van binnenkomend verkeer isoleren.

Meer voorbeelden en een overzicht van het netwerk beveiligingsgrenzen vindt [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="main-script-and-network-config"></a>Belangrijkste script en netwerk-configuratie
Sla het volledige Script in een PowerShell-scriptbestand. De netwerkconfiguratie opslaan in een bestand met de naam 'NetworkConf1.xml'.
De gebruiker gedefinieerde variabelen naar wens wijzigen en voer het script.

#### <a name="full-script"></a>Volledige script
Dit script wordt, op basis van de gebruiker gedefinieerde variabelen;

1. Verbinding maken met een Azure-abonnement
2. Een opslagaccount maken
3. Maak een VNet en twee subnets zoals gedefinieerd in het netwerk configuratiebestand.
4. Vier windows server-VM's maken
5. Configureer het NSG inclusief:
   * Maken van een NSG
   * Met regels in te vullen
   * Het NSG binden aan de juiste subnetten

Deze PowerShell-script moet lokaal op worden uitgevoerd dat een internet verbonden PC of de server.

> [!IMPORTANT]
> Wanneer dit script wordt uitgevoerd, kunnen er waarschuwingen of andere informatieve berichten die pop in PowerShell. Alleen foutberichten in rood zijn aanleiding.
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on the FrontEnd Subnet
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
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: To ensure proper NSG Rule creation later in this script:
    #       - The Web Server must be VM 0
    #       - The AppVM1 Server must be VM 1
    #       - The DNS server must be VM 3
    #
    #       Otherwise the NSG rules in the last section of this
    #       script will need to be changed to match the modified
    #       VM array numbers ($i) so the NSG Rule IP addresses
    #       are aligned to the associated VM IP addresses.

    # VM 0 - The Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - The First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - The Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - The DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
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
Else { Write-Host "The network configuration file was found" -ForegroundColor Green
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
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

  # Build the NSG
    Write-Host "Building the NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
  # Install Test Web App (Run Post-Build Script on the IIS Server)
  # Install Backend resource (Run Post-Build Script on the AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a>Netwerk-configuratiebestand
Dit xml-bestand opslaan met bijgewerkte locatie en de koppeling toevoegen aan dit bestand naar de variabele $NetworkConfigFile in het voorgaande script.

```XML
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

#### <a name="sample-application-scripts"></a>Voorbeeldscripts toepassing
Als u een voorbeeldtoepassing voor deze en andere voorbeelden DMZ installeren wilt, een is opgegeven op de volgende koppeling: [voorbeeldscript toepassing][SampleApp]

## <a name="next-steps"></a>Volgende stappen
* Bijwerken en XML-bestand opslaan
* Voer de PowerShell-script om samen te stellen van de omgeving
* De voorbeeldtoepassing installeren
* Ander verkeer stroomt via deze DMZ testen

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inkomende DMZ met NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

