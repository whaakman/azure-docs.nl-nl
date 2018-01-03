---
title: "Voorbeeld van Azure DMZ – een eenvoudige DMZ met nsg's bouwen | Microsoft Docs"
description: Maken van een DMZ met Netwerkbeveiligingsgroepen (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Voorbeeld 1: een eenvoudige DMZ met nsg's met een Azure Resource Manager-sjabloon maken
[Ga terug naar de grens van Best Practices beveiligingspagina][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-sjabloon](virtual-networks-dmz-nsg.md)
> * [Klassieke - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

In dit voorbeeld maakt een primitieve DMZ met vier Windows-servers en Netwerkbeveiligingsgroepen. In dit voorbeeld ziet u de gewenste sjabloonsecties voor een beter begrip van elke stap. Er is ook een sectie verkeer Scenario met een diepgaande stapsgewijze blik op hoe verkeer wordt uitgevoerd door de lagen verdediging in het Perimeternetwerk. Ten slotte in de verwijzingen is sectie de volledige sjablooncode en instructies voor het bouwen van deze omgeving om te testen en Experimenteer met verschillende scenario's. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Inkomende DMZ met NSG][1]

## <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld bevat een abonnement op de volgende bronnen:

* Één resourcegroep
* Een virtueel netwerk met twee subnetten; 'FrontEnd' en back-end'
* Een Netwerkbeveiligingsgroep dat wordt toegepast op beide subnetten
* Een Windows-Server waarmee een toepassing webserver ('IIS01')
* Twee windows-servers die vertegenwoordigen toepassingsservers van back-end ('AppVM01', 'AppVM02')
* Een Windows-server met een DNS-server ('DNS01')
* Een openbaar IP-adres die zijn gekoppeld aan de webserver van de toepassing

In de sectie verwijzingen is er een koppeling naar een Azure Resource Manager-sjabloon die de omgeving beschreven in dit voorbeeld is gebaseerd. Opbouwen van de virtuele machines en virtuele netwerken, hoewel uitgevoerd door de voorbeeldsjabloon worden niet beschreven in dit document uitvoeriger. 

**In deze omgeving te scheppen** (gedetailleerde instructies vindt u in de sectie Verwijzingen van dit document);

1. Implementeren van de Azure Resource Manager-sjabloon op: [Azure-Snelstartsjablonen][Template]
2. De voorbeeldtoepassing op installeren: [voorbeeldscript toepassing][SampleApp]

>[!NOTE]
>Voor RDP voor back-end servers in dit exemplaar van wordt de IIS-server gebruikt als een 'jump vak'. Eerste RDP de IIS-server en vervolgens naar het RDP-IIS-Server naar de back-endserver. Een openbaar IP-adres kan ook worden gekoppeld aan elke server NIC voor RDP eenvoudiger.
> 
>

De volgende secties bieden een gedetailleerde beschrijving van de Netwerkbeveiligingsgroep en hoe werkt dit voorbeeld door de stap voor stap sleutel regels van de Azure Resource Manager-sjabloon.

## <a name="network-security-groups-nsg"></a>Netwerkbeveiligingsgroepen (NSG's)
In dit voorbeeld is een groep NSG gebouwd en vervolgens geladen met zes regels. 

>[!TIP]
>In het algemeen moet u eerst uw specifieke regels voor 'Toestaan' maken en vervolgens de algemene regels voor "Deny" laatste. De toegewezen prioriteit bepaalt welke regels eerst geëvalueerd. Als verkeer toepassen op een specifieke regel wordt gevonden, wordt er geen verdere regels worden geëvalueerd. NSG-regels kunnen toepassen op een of meer in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).
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

Er is een uitgaande standaardregel waarmee uitgaand verkeer naar internet. Voor dit voorbeeld bent we uitgaand verkeer toestaat en alle regels voor uitgaande verbindingen niet wijzigen. Als beveiligingsbeleid wilt toepassen om verkeer in beide richtingen, gebruiker gedefinieerde routering is vereist en wordt verkend in "Voorbeeld 3" op de [Best Practices beveiligingspagina-grens][HOME].

Elke regel is als volgt in meer detail besproken:

1. Een Netwerkbeveiligingsgroep resource moet worden geïnstantieerd voor het opslaan van de regels:

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

2. De eerste regel in dit voorbeeld kunt DNS-verkeer tussen alle interne netwerken voor de DNS-server op het subnet voor back-end. De regel heeft enkele belangrijke parameters:
  * 'destinationAddressPrefix' - regels kunnen gebruiken een speciaal type adresvoorvoegsel een "standaard code' genoemd, deze tags zijn systeem-id's die een eenvoudige manier om een grotere categorie van adresvoorvoegsels toestaan. Deze regel maakt gebruik van de Tag standaard 'Internet' om een adres buiten het VNet aan te geven. Andere labels voorvoegsel zijn VirtualNetwork en AzureLoadBalancer.
  * 'Richting' geeft aan in welke richting van verkeer met deze regel wordt van kracht. De richting is vanuit het perspectief van de subnet- of virtuele Machine (afhankelijk van waar deze NSG is gekoppeld). Dus als richting is 'Inkomend' en verkeer is het subnet invoeren, de regel van toepassing en het subnet uitgaand verkeer niet wordt beïnvloed door deze regel.
  * 'Prioriteit', bepaalt de volgorde waarin netwerkverkeer wordt geëvalueerd. Hoe lager het getal hoe hoger de prioriteit. Wanneer een regel van toepassing op een specifieke netwerkverkeer, worden er geen verdere regels verwerkt. Dus als een regel met prioriteit 1 gegevensverkeer, en een regel met prioriteit 2 verkeer weigert, en beide regels zijn van toepassing op het verkeer wordt het verkeer mogen stromen (omdat de regel 1 een hogere prioriteit heeft geduurd effect en geen verdere regels zijn toegepast).
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

4. Deze regel kunnen Binnenkomend internetverkeer om de webserver. Deze regel wordt het routeringsgedrag niet gewijzigd. De regel kunnen alleen verkeer dat is bestemd voor IIS01 om door te geven. Dus als verkeer van Internet had van de webserver als de bestemming voor deze regel zou toe te staan en stoppen met het verwerken van verdere regels. (In de regel met prioriteit 140 alle andere Binnenkomend internetverkeer wordt geblokkeerd). Als u alleen HTTP-verkeer wordt verwerkt, kan deze regel verder worden beperkt zodat alleen bestemming poort 80.

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

5. Deze regel kan verkeer door te geven van de server IIS01 aan de AppVM01-server, een hoger regel blokken alle Frontend voor verkeer van de back-end. Deze regel verbeteren als de poort bekend is dat moet worden toegevoegd. Bijvoorbeeld, als de IIS-server alleen SQL Server op AppVM01 roept is, het Doelpoortbereik moet worden gewijzigd van ' * ' (Any) op 1433 (de SQL-poort), waardoor een kleinere inkomende kwetsbaarheid voor aanvallen op AppVM01 of de webtoepassing ooit verdacht.

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

6. Deze regel weigert verkeer van het internet voor servers op het netwerk. Aan de regels met prioriteit 110 en 120 is het effect dat alleen binnenkomende internetverkeer naar de firewall en RDP-poorten op servers en -blokken rest. Deze regel is een 'foutveilige' regel voor het blokkeren van alle onverwachte stromen.

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

7. De laatste regel weigert verkeer van de front-end-subnet met het subnet voor back-end. Aangezien deze regel een inkomende regel alleen is, is (van de back-end voor de Frontend) omgekeerde verkeer toegestaan.

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

## <a name="traffic-scenarios"></a>Verkeer scenario 's
#### <a name="allowed-internet-to-web-server"></a>(*Toegestane*) Internet naar webserver
1. Een internet-gebruiker aanvragen een pagina http-van het openbare IP-adres van de NIC die is gekoppeld aan de IIS01-NIC
2. Het openbare IP-adres wordt doorgegeven verkeer naar het VNet naar IIS01 (de webserver)
3. Subnet frontend begint verwerking van inkomende regel:
  1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3. NSG regel 3 (Internet naar IIS01) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de webserver IIS01 (10.0.1.5)
5. IIS01 luistert voor internetverkeer, ontvangt deze aanvraag en begint met de verwerking van de aanvraag
6. IIS01 vraagt de SQL-Server op AppVM01 voor meer informatie
7. Er is geen uitgaande regels op subnet Frontend verkeer wordt toegestaan.
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
12. De IIS-server de SQL-respons ontvangt en de HTTP-antwoord is voltooid en verzendt naar de aanvrager
13. Aangezien er geen uitgaande regels op het subnet Frontend zijn, het antwoord is toegestaan en de Internet-gebruiker ontvangt de aangevraagde webpagina.

#### <a name="allowed-rdp-to-iis-server"></a>(*Toegestane*) RDP naar IIS-server
1. Een serverbeheerder op internet vraagt een RDP-sessie op IIS01 voor het openbare IP-adres van de NIC die is gekoppeld aan de IIS01 NIC (dit openbare IP-adres kan worden gevonden via de Portal of PowerShell)
2. Het openbare IP-adres wordt doorgegeven verkeer naar het VNet naar IIS01 (de webserver)
3. Subnet frontend begint verwerking van inkomende regel:
  1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2. NSG regel 2 (RDP) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Er is geen uitgaande regels standaardregels toepassen en terugkerend verkeer is toegestaan
5. RDP-sessie is ingeschakeld
6. IIS01 wordt u gevraagd om de gebruikersnaam en wachtwoord

>[!NOTE]
>Voor RDP voor back-end servers in dit exemplaar van wordt de IIS-server gebruikt als een 'jump vak'. Eerste RDP de IIS-server en vervolgens naar het RDP-IIS-Server naar de back-endserver.
>
>

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

#### <a name="denied-rdp-to-backend"></a>(*Geweigerd*) RDP met back-end
1. Een internet-gebruiker probeert te RDP met server AppVM01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC zijn, dit verkeer nooit voert het VNet en wouldn't de server niet bereiken
3. Maar als een openbaar IP-adres is ingeschakeld voor een bepaalde reden, NSG-regel 2 (RDP) zou dit verkeer toestaan

>[!NOTE]
>Voor RDP voor back-end servers in dit exemplaar van wordt de IIS-server gebruikt als een 'jump vak'. Eerste RDP de IIS-server en vervolgens naar het RDP-IIS-Server naar de back-endserver.
>
>

#### <a name="denied-web-to-backend-server"></a>(*Geweigerd*) Web naar back-endserver
1. Een internet-gebruiker probeert te krijgen tot een bestand op AppVM01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC zijn, dit verkeer nooit voert het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een bepaalde reden, wordt NSG regel 5 (Internet naar VNet) dit verkeer geblokkeerd

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Geweigerd*) opzoeken in Web DNS op DNS-server
1. Een internet-gebruiker probeert een interne DNS-record op DNS01 opzoeken
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC zijn, dit verkeer nooit voert het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een bepaalde reden, NSG regel 5 (Internet naar VNet) dit verkeer wordt geblokkeerd (Opmerking: of regel 1 (DNS) niet van toepassing omdat de aanvragen bronadres is het internet en regel 1 is alleen van toepassing op de lokale VNet als de bron)

#### <a name="denied-sql-access-on-the-web-server"></a>(*Geweigerd*) SQL-toegang op de webserver
1. Een internetgebruiker opvraagt SQL-gegevens uit IIS01
2. Aangezien er geen openbare IP-adressen die zijn gekoppeld aan deze servers NIC zijn, dit verkeer nooit voert het VNet en wouldn't de server niet bereiken
3. Als een openbaar IP-adres is ingeschakeld voor een bepaalde reden, begint het subnet Frontend verwerking van inkomende regel:
  1. NSG regel 1 (DNS) niet van toepassing, verplaatsen naar de volgende regel
  2. NSG regel 2 (RDP) niet van toepassing, verplaatsen naar de volgende regel
  3. NSG regel 3 (Internet naar IIS01) is van toepassing, -verkeer is toegestaan, zelfs niet meer regelverwerking
4. Verkeer komt binnen via interne IP-adres van de IIS01 (10.0.1.5)
5. IIS01 wordt niet geluisterd op poort 1433, dus geen antwoord op de aanvraag

## <a name="conclusion"></a>Conclusie
In dit voorbeeld is een relatief eenvoudige en meteen verder manier van het subnet van de back-end van binnenkomend verkeer isoleren.

Meer voorbeelden en een overzicht van het netwerk beveiligingsgrenzen vindt [hier][HOME].

## <a name="references"></a>Verwijzingen
### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Dit voorbeeld wordt een vooraf gedefinieerde Azure Resource Manager-sjabloon gebruikt in een door Microsoft beheerde GitHub-opslagplaats en toegankelijk voor de community. Deze sjabloon kan rechtstreeks uit de GitHub geïmplementeerd of gedownload en gewijzigd naar wens aanpassen. 

De belangrijkste sjabloon is in het bestand met de naam 'azuredeploy.json'. Deze sjabloon kan worden verzonden via PowerShell of CLI (met het bestand gekoppeld 'azuredeploy.parameters.json') om deze sjabloon te implementeren. Ik vinden in dat de eenvoudigste manier is de knop 'Implementeren naar Azure' gebruiken op de pagina README.md op GitHub.

Volg deze stappen voor het implementeren van de sjabloon die in dit voorbeeld GitHub en de Azure-portal voortbouwt:

1. Navigeer via een browser naar de [sjabloon][Template]
2. Klik op de knop 'Implementeren naar Azure' (of de knop 'Visualiseren' om te zien van een grafische weergave van deze sjabloon)
3. Voer de Storage-Account, gebruikersnaam en wachtwoord in de blade Parameters en klik vervolgens op **OK**
5. Maak een resourcegroep voor deze implementatie (u kunt een bestaande, maar ik het beste een nieuw wachtwoord voor de beste resultaten)
6. Indien nodig, moet u de instellingen van het abonnement en de locatie voor uw VNet wijzigen.
7. Klik op **juridische voorwaarden bekijken**, lees de voorwaarden en klik op **aankoop** accepteren.
8. Klik op **maken** om te beginnen met de implementatie van deze sjabloon.
9. Zodra de implementatie voltooid wordt, gaat u naar de resourcegroep voor deze implementatie gemaakt voor een overzicht van de resources die zijn geconfigureerd in.

>[!NOTE]
>Deze sjabloon kunt RDP met alleen de IIS01-server (zoek het openbare IP-adres voor IIS01 op de Portal). Voor RDP voor back-end servers in dit exemplaar van wordt de IIS-server gebruikt als een 'jump vak'. Eerste RDP de IIS-server en vervolgens naar het RDP-IIS-Server naar de back-endserver.
>
>

U verwijdert deze implementatie, verwijderen van de resourcegroep en alle onderliggende resources worden eveneens verwijderd.

#### <a name="sample-application-scripts"></a>Voorbeeldscripts toepassing
Nadat de sjabloon voltooid wordt, kunt u de webserver en de appserver met een eenvoudige webtoepassing waarmee testen met deze configuratie DMZ instellen. Als u wilt installeren een voorbeeldtoepassing voor deze en andere voorbeelden van het Perimeternetwerk, een is opgegeven op de volgende koppeling: [voorbeeldscript toepassing][SampleApp]

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