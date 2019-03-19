---
title: "Voorbeeld van een Azure DMZ: een eenvoudige DMZ met nsg's bouwen | Microsoft Docs"
description: Bouw een DMZ met Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 68655ea03f53fe7100f67d111fcd3c8595bdf4c9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58109389"
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Voorbeeld 1: een eenvoudige DMZ bouwen met nsg's met een Azure Resource Manager-sjabloon
[Ga terug naar de grens Best Practices pagina][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-sjabloon](virtual-networks-dmz-nsg.md)
> * [Klassiek - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

In dit voorbeeld maakt een primitief DMZ met vier Windows-servers en Netwerkbeveiligingsgroepen. In dit voorbeeld wordt elk van de secties van de gewenste sjabloon voor een beter begrip van elke stap beschreven. Er is ook een sectie verkeer Scenario voor een gedetailleerde stapsgewijze kijken hoe verkeer wordt uitgevoerd in de defensieve in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de code van de volledige sjabloon en de instructies voor het bouwen van deze omgeving om te testen en te experimenteren met verschillende scenario's. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Inkomende DMZ met NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld bevat een abonnement in de volgende bronnen:

* Een enkele resourcegroep
* Een Virtueelnetwerk met twee subnetten; 'FrontEnd' en 'Back-end'
* Een Netwerkbeveiligingsgroep die is toegepast op beide subnetten
* Een Windows-Server die staat voor een toepassing webserver ("IIS01")
* Twee windows-servers die staan voor toepassingsservers van back-end ("AppVM01", "AppVM02")
* Een Windows-server die staat voor een DNS-server ("DNS01")
* Een openbaar IP-adres dat is gekoppeld aan de webserver van de toepassing

Er is een koppeling naar een Azure Resource Manager-sjabloon die de omgeving die worden beschreven in dit voorbeeld is gebaseerd in de sectie verwijzingen. Het bouwen van de virtuele machines en virtuele netwerken, hoewel uitgevoerd door de voorbeeldsjabloon, worden niet beschreven in dit document. 

**In deze omgeving te scheppen** (gedetailleerde instructies vindt u in de sectie Verwijzingen van dit document);

1. Op de Azure Resource Manager-sjabloon implementeren: [Azure Quickstart-sjablonen][Template]
2. Installeer de voorbeeldtoepassing op: [Voorbeeldscript voor toepassing][SampleApp]

>[!NOTE]
>Voor RDP voor back-end servers in dit geval wordt de IIS-server gebruikt als een 'jump-box." Eerste RDP naar de IIS-server en klik in het RDP-IIS-Server naar de back-endserver. Een openbaar IP-adres kan ook worden gekoppeld aan elke NIC voor eenvoudiger RDP-server.
> 
>

De volgende secties vindt u een gedetailleerde beschrijving van de Netwerkbeveiligingsgroep en hoe deze functies in dit voorbeeld door hek belangrijkste regels van de Azure Resource Manager-sjabloon.

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een NSG-groep gemaakt en vervolgens geladen met zes regels. 

>[!TIP]
>In het algemeen moet u eerst uw specifieke 'Toestaan'-regels maken en vervolgens de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels worden eerst geëvalueerd. Wanneer verkeer toe te passen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen in beide in de richting binnenkomend of uitgaand (vanuit het perspectief van het subnet).
>
>

De volgende regels zijn declaratief, gebouwd voor binnenkomend verkeer:

1. Interne DNS-verkeer (poort 53) is toegestaan
2. RDP-verkeer (poort 3389) van het Internet op elke virtuele machine is toegestaan
3. HTTP-verkeer (poort 80) van het Internet naar de webserver (IIS01) is toegestaan
4. Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan
5. Al het verkeer (alle poorten) van het Internet naar de volledige VNet (beide subnetten) is geweigerd
6. Al het verkeer (alle poorten) vanaf het subnet Frontend naar de back-end-subnet wordt geweigerd

Met deze regels gekoppeld aan elk subnet, als een HTTP-aanvraag binnenkomend verkeer van Internet naar de webserver, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen, maar aangezien regel 3 een hogere prioriteit heeft alleen zou worden toegepast en regel 5 niet aan de orde komt. De HTTP-aanvraag zou dus met de webserver worden toegestaan. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) is de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) Hiermee blokkeert u de front-endsubnet van de communicatie met de back-end-subnet (met uitzondering van toegestaan verkeer in regels 1 en 4), deze regelset worden de back-endnetwerk beveiligd als een aanvaller inbreuk de web-App op het front-end, de aanvaller zou beperkt toegang tot de back-end '-beveiliging gebruiken' netwerk (alleen voor resources weergegeven op de server AppVM01).

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. In dit voorbeeld zijn we uitgaand verkeer wordt toegestaan en alle regels voor uitgaand verkeer niet wordt gewijzigd. Als u wilt toepassen van beveiligingsbeleid voor het verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist en is verkend in "Voorbeeld 3" op de [Best Practices beveiligingspagina-grens][HOME].

Elke regel wordt als volgt in meer detail beschreven:

1. Een Netwerkbeveiligingsgroep resource moet worden gemaakt voor het opslaan van de regels:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. De eerste regel in dit voorbeeld kunt DNS-verkeer tussen alle interne netwerken voor de DNS-server op de back endsubnet. De regel heeft enkele belangrijke parameters:
   * "destinationAddressPrefix" - het voorvoegsel voor het doeladres is ingesteld op '10.0.2.4' zodat DNS-verkeer is toegestaan om de DNS-server te bereiken.
   * 'Richting' geeft aan in welke richting van netwerkverkeer met deze regel wordt van kracht. De richting is vanuit het perspectief van het subnet of de virtuele Machine (afhankelijk van waar deze NSG is gekoppeld). Dus als richting is 'Inkomend' en verkeer het subnet binnenkomt, de regel toepassen wilt en uitgaand verkeer van het subnet kan niet worden beïnvloed door deze regel.
   * 'Prioriteit' Hiermee stelt u de volgorde waarin netwerkverkeer wordt geëvalueerd. Des te lager het nummer hoe hoger de prioriteit. Wanneer een regel van toepassing aan een stroom specifiek verkeer is, worden er geen verdere regels worden verwerkt. Dus als een regel met prioriteit 1 verkeer staat, en een regel met prioriteit 2 verkeer weigert, en beide regels zijn van toepassing op het verkeer wordt het verkeer is toegestaan flow (omdat de regel 1 heeft een hogere prioriteit effect heeft geduurd en er geen verdere regels zijn toegepast).
   * 'Toegang' geeft aan of verkeer dat is beïnvloed door deze regel is geblokkeerd ("Deny") of toegestane ('toestaan').

     ```JSON
     "properties": {
     "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
     ```

3. Deze regel kunnen de RDP-verkeer van internet naar de RDP-poort op elke server in het subnet gebonden stromen. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Deze regel kunnen Binnenkomend internetverkeer naar de webserver bereikt. Deze regel wordt de werking van de routering niet gewijzigd. De regel kunt alleen verkeer dat is bestemd voor IIS01 om door te geven. Dus als verkeer vanaf Internet de webserver heeft als doel deze regel zou toelaat en stoppen van verdere regels worden verwerkt. (In de regel met prioriteit 140 alle andere Binnenkomend internetverkeer wordt geblokkeerd). Als u alleen HTTP-verkeer verwerken bent, kan deze regel verder worden beperkt om toe te staan alleen bestemming poort 80.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Deze regel staat verkeer om door te geven van de server IIS01 naar de AppVM01-server, een hoger regel blokkeert alle andere Frontend naar back-end-verkeer. Voor het verbeteren van deze regel als de poort bekend is dat moet worden toegevoegd. Bijvoorbeeld, als de IIS-server alleen SQL Server op AppVM01 bereikt is, het poortbereik van doel moet worden gewijzigd van ' * ' (willekeurig) op 1433 (de SQL-poort), zodat een kleinere inkomende kwetsbaarheid voor aanvallen op AppVM01 moet de web-App wordt aangetast.

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Regels voor een speciaal type adresvoorvoegsel met een 'standaard-Tag' de naam kunnen gebruiken, deze tags zijn systeem-id's waarmee een eenvoudige manier om een grotere categorie van adresvoorvoegsels. Deze regel maakt gebruik van de Tag standaard 'VirtualNetwork' voor het voorvoegsel voor het doeladres om elk adres binnen het VNet aan te geven. Andere labels voorvoegsel zijn Internet en AzureLoadBalancer. Deze regel weigert verkeer van internet voor servers op het netwerk. Het effect is aan de regels met prioriteit 110 en 120, om toe te staan alleen inkomend internetverkeer naar de firewall en RDP-poorten op servers en blokkeert al het andere. Deze regel is een "failsafe" regel voor het blokkeren van alle onverwachte stromen.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. De laatste regel weigert verkeer van het subnet Frontend naar de back-end-subnet. Omdat met deze regel een inkomende regel alleen wordt, is terugkerend verkeer is toegestaan (van de back-end aan de front-end).

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Scenario's voor verkeer
#### <a name="allowed-internet-to-web-server"></a>(*Toegestane*) Internet naar de webserver
1. Een internet-gebruiker aanvragen een pagina http-van het openbare IP-adres van de NIC die is gekoppeld aan de NIC IIS01
2. Het openbare IP-adres wordt verkeer naar het VNet naar IIS01 doorgegeven (de webserver)
3. Front-endsubnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan IIS01) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Verkeer komt binnen via het interne IP-adres van de webserver IIS01 (10.0.1.5)
5. IIS01 luistert voor webverkeer te genereren, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
6. IIS01 vraagt de SQL-Server op AppVM01 voor meer informatie
7. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
8. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan Firewall) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u regelverwerking
9. AppVM01 ontvangt van de SQL-Query en reageert
10. Aangezien er geen regels voor uitgaand verkeer op de back-end-subnet, is het antwoord toegestaan
11. Front-endsubnet begint verwerking van inkomende regel:
    1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
    2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
12. De IIS-server, SQL-antwoord is ontvangen en het HTTP-antwoord is voltooid en verzendt naar de aanvrager
13. Aangezien er geen regels voor uitgaand verkeer op de front-endsubnet, het antwoord is toegestaan en de Internet-gebruiker ontvangt de webpagina die is aangevraagd.

#### <a name="allowed-rdp-to-iis-server"></a>(*Toegestane*) RDP naar IIS-server
1. Een serverbeheerder op internet vraagt een RDP-sessie naar IIS01 op het openbare IP-adres van de NIC die is gekoppeld aan de IIS01 NIC (dit openbare IP-adres kan worden gevonden via de Portal of PowerShell)
2. Het openbare IP-adres wordt verkeer naar het VNet naar IIS01 doorgegeven (de webserver)
3. Front-endsubnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Er zijn geen regels voor uitgaand verkeer, standaardregels toepassen en retourverkeer is toegestaan
5. RDP-sessie is ingeschakeld
6. IIS01 wordt u gevraagd om de gebruikersnaam en wachtwoord

>[!NOTE]
>Voor RDP voor back-end servers in dit geval wordt de IIS-server gebruikt als een 'jump-box." Eerste RDP naar de IIS-server en klik in het RDP-IIS-Server naar de back-endserver.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Toegestane*) Web server DNS look up heeft op DNS-server
1. Web Server, IIS01, een gegevensfeed op www.data.gov behoeften, maar moet het adres omzetten.
2. De netwerkconfiguratie voor de VNet-lijsten DNS01 (10.0.2.4 op de back-end-subnet) als de primaire DNS-server, IIS01 verzendt de DNS-aanvraag naar DNS01
3. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
4. Back-end-subnet begint verwerking van inkomende regel:
   * NSG regel 1 (DNS) is van toepassing, verkeer is toegestaan, stop regelverwerking
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Toegestane*) Web server-databasebestand op AppVM01
1. IIS01 vraagt om een bestand op AppVM01
2. Er is geen uitgaande regels op de front-endsubnet, verkeer is toegestaan
3. De back-end-subnet begint verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan IIS01) niet van toepassing, verplaatsen naar de volgende regel
   4. NSG-regel 4 (IIS01 naar AppVM01) is van toepassing, verkeer is toegestaan, stopt u regelverwerking
4. AppVM01 ontvangt de aanvraag en reageert met een bestand (ervan uitgaande dat de toegang geautoriseerd)
5. Aangezien er geen regels voor uitgaand verkeer op de back-end-subnet, is het antwoord toegestaan
6. Front-endsubnet begint verwerking van inkomende regel:
   1. Er is geen NSG-regel voor binnenkomend verkeer van het back-end-subnet aan het Frontend-subnet, zodat geen van de NSG-regels toepassen
   2. De standaardregel systeem voor het verkeer tussen subnetten zou dit verkeer toestaan, zodat het verkeer is toegestaan.
7. De IIS-server ontvangt het bestand

#### <a name="denied-rdp-to-backend"></a>(*Geweigerd*) RDP naar back-end
1. Een internet-gebruiker probeert te RDP naar server AppVM01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC, dit verkeer zouden nooit invoeren voor het VNet en wouldn't de server niet bereiken
3. Maar als een openbaar IP-adres is ingeschakeld voor een of andere reden, NSG-regel 2 (RDP) zou dit verkeer toestaan

>[!NOTE]
>Voor RDP voor back-end servers in dit geval wordt de IIS-server gebruikt als een 'jump-box." Eerste RDP naar de IIS-server en klik in het RDP-IIS-Server naar de back-endserver.
>
>

#### <a name="denied-web-to-backend-server"></a>(*Geweigerd*) Web naar back-endserver
1. Een internetgebruiker probeert te krijgen tot een bestand op AppVM01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC, dit verkeer zouden nooit invoeren voor het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een of andere reden, blokkeren NSG-regel 5 (Internet naar VNet) dat het verkeer

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Geweigerd*) Web DNS resultaat op DNS-server
1. Een internet-gebruiker probeert om te controleren of een interne DNS-record op DNS01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC, dit verkeer zouden nooit invoeren voor het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een of andere reden, NSG-regel 5 (Internet naar VNet) dit verkeer wilt blokkeren (Opmerking: dat regel 1 (DNS) niet van toepassing omdat de aanvragen bronadres is het internet en regel 1 is alleen van toepassing op de lokale VNet als de bron)

#### <a name="denied-sql-access-on-the-web-server"></a>(*Geweigerd*) SQL-toegang op de webserver
1. Een internetgebruiker vraagt SQL-gegevens van IIS01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC, dit verkeer zouden nooit invoeren voor het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een of andere reden, begint het subnet Frontend verwerking van inkomende regel:
   1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
   2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
   3. NSG-regel 3 (Internet aan IIS01) is van toepassing, verkeer is toegestaan, stop regelverwerking
4. Verkeer komt binnen via het interne IP-adres van de IIS01 (10.0.1.5)
5. IIS01 is niet luisteren op poort 1433, dus geen antwoord op de aanvraag

## <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudig en duidelijk manier van het isoleren van de back-end-subnet van het binnenkomende verkeer.

Meer voorbeelden en een overzicht van beveiliging netwerkgrenzen vindt [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
In dit voorbeeld wordt een vooraf gedefinieerde Azure Resource Manager-sjabloon gebruikt in een door Microsoft beheerde GitHub-opslagplaats en toegankelijk voor de community. Deze sjabloon kan worden rechtstreeks uit de GitHub geïmplementeerd of gedownload en aangepast aan uw behoeften aan. 

De belangrijkste sjabloon is in het bestand met de naam "azuredeploy.json." Deze sjabloon kan worden verzonden via PowerShell of CLI (met het bestand gekoppeld 'azuredeploy.parameters.json') om deze sjabloon te implementeren. Hier is dat de eenvoudigste manier is met de knop 'Implementeren naar Azure' op de pagina README.md op GitHub.

Volg deze stappen voor het implementeren van de sjabloon die in dit voorbeeld uit GitHub en de Azure-portal:

1. Vanuit een browser, Ga naar de [sjabloon][Template]
2. Klik op de knop 'Implementeren naar Azure' (of de knop 'Visualiseren' om te zien van een grafische weergave van deze sjabloon)
3. Voer de Storage-Account, gebruikersnaam en wachtwoord in de Parameters-blade en klik op **OK**
5. Maak een resourcegroep voor deze implementatie (u kunt een bestaande resourcegroep gebruiken, maar ik u aan een nieuw wachtwoord voor de beste resultaten)
6. Indien nodig, wijzigt u de instellingen van het abonnement en de locatie voor uw VNet.
7. Klik op **juridische voorwaarden bekijken**, lees de voorwaarden en klik op **aankoop** om akkoord te gaan.
8. Klik op **maken** om te beginnen met de implementatie van deze sjabloon.
9. Nadat de implementatie voltooid is, gaat u naar de resourcegroep voor deze implementatie gemaakt om te zien van de resources die zijn geconfigureerd in.

>[!NOTE]
>Met deze sjabloon kunt RDP-verbinding alleen de IIS01-server (zoeken naar het openbare IP-adres voor IIS01 in de Portal). Voor RDP voor back-end servers in dit geval wordt de IIS-server gebruikt als een 'jump-box." Eerste RDP naar de IIS-server en klik in het RDP-IIS-Server naar de back-endserver.
>
>

Als u wilt verwijderen van deze implementatie, verwijdert u de resourcegroep en alle onderliggende resources worden ook verwijderd.

#### <a name="sample-application-scripts"></a>Voorbeeldscripts voor toepassing
Nadat de sjabloon is uitgevoerd, kunt u de webserver en appserver met een eenvoudige webtoepassing waarmee de testen met deze configuratie DMZ instellen. Als u wilt installeren een voorbeeldtoepassing voor deze en andere voorbeelden DMZ, heeft opgegeven op de volgende koppeling: [Voorbeeldscript voor toepassing][SampleApp]

## <a name="next-steps"></a>Volgende stappen

* In dit voorbeeld implementeren
* De voorbeeldtoepassing
* Ander verkeer stroomt via deze DMZ testen

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inkomende DMZ met NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md
