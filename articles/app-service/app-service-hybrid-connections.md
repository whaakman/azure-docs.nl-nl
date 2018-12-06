---
title: Hybride verbindingen van Azure App Service | Microsoft Docs
description: Over het maken en hybride verbindingen gebruiken voor toegang tot resources in afzonderlijke netwerken
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: ccompy
ms.openlocfilehash: cdfe1a35dbbf8fa048e0b0ca24f06bb0e73bde61
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968307"
---
# <a name="azure-app-service-hybrid-connections"></a>Hybride verbindingen van Azure App Service #

Hybride verbindingen is zowel een service in Azure als een functie in Azure App Service. Als een service heeft het gebruik en mogelijkheden die worden gebruikt in App Service. Zie voor meer informatie over hybride verbindingen en hun gebruik buiten de App Service, [hybride Azure Relay-verbindingen][HCService].

In App Service, kan de hybride verbindingen worden gebruikt voor toegang tot toepassingsresources in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt. Er is een alternatieve mogelijkheid toegang tot uw toepassing niet ingeschakeld. Als in App Service gebruikt, wordt elke hybride verbinding overeenkomt met een enkele TCP-host en poort combinatie. Dit betekent dat het eindpunt van de hybride verbinding kan zich op elk besturingssysteem en elke toepassing, mits u toegang hebben tot een TCP-poort voor luisteren. De functie Hybrid Connections niet weet of zorgt dat er wat het toepassingsprotocol is, of wat u wilt openen. Het gewoon levert toegang tot het netwerk.  


## <a name="how-it-works"></a>Hoe werkt het? ##
De functie Hybrid Connections bestaat uit twee uitgaande oproepen naar Azure Service Bus Relay. Er is een verbinding van een bibliotheek op de host waarop uw app in App Service wordt uitgevoerd. Er is ook een verbinding van de Hybrid Connection Manager (HCM) met Service Bus Relay. De HCM is een relayservice die u in het netwerk die als host fungeert voor de resource die u probeert implementeert te openen. 

Met behulp van de twee gekoppelde verbindingen heeft uw app in een TCP-tunnel naar een combinatie van vaste host: poort op de andere kant van de HCM. De verbinding maakt gebruik van TLS 1.2 voor beveiliging en shared access signature (SAS) sleutels voor verificatie en autorisatie.    

![Diagram van hybride verbinding op hoog niveau stroom][1]

Als uw app in een DNS-aanvraag die overeenkomt met een geconfigureerde hybride verbinding-eindpunt, worden de uitgaande TCP-verkeer omgeleid via de hybride verbinding.  

> [!NOTE]
> Dit betekent dat u proberen moet te gebruiken altijd een DNS-naam voor de hybride verbinding. Sommige clientsoftware doet een DNS-zoekopdracht als het eindpunt maakt gebruik van een IP-adres in plaats daarvan.
>


### <a name="app-service-hybrid-connection-benefits"></a>Voordelen van App-Service hybride verbinding ###

Er zijn een aantal voordelen de functie hybride verbindingen, met inbegrip van:

- Apps kunnen veilig toegang tot on-premises systemen en services.
- De functie is niet vereist voor een via internet toegankelijke eindpunt.
- Het is snel en eenvoudig om in te stellen. 
- Elke hybride verbinding komt overeen met een één host: poort combinatie, nuttig voor beveiliging.
- Normaal gesproken hoeft niet firewall gaten. De verbindingen zijn al het uitgaande via de standaard webpoorten.
- Omdat de functie het niveau van netwerk is, is het agnostisch ten opzichte van de taal die wordt gebruikt door uw app en de technologie die wordt gebruikt door het eindpunt.
- Het kan worden gebruikt om meerdere netwerken vanuit een één-app toegang te verlenen. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Dingen die u kunt geen met hybride verbindingen doen ###

Dingen die u kunt geen met hybride verbindingen doen zijn onder andere:

- Een station koppelen.
- UDP gebruiken.
- Toegang op basis van TCP-services die gebruikmaken van dynamische poorten, zoals de passieve modus van FTP of uitgebreide passieve modus.
- Ondersteuning voor LDAP, omdat er kunt UDP is vereist.
- Ondersteuning voor Active Directory, omdat u kunt geen lid van domein een werknemer van App Service.

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Toevoegen en hybride verbindingen in uw app maken ##

Een hybride verbinding wilt maken, gaat u naar de [Azure-portal] [ portal] en selecteer uw app. Selecteer **netwerken** > **uw hybride-verbindingseindpunten configureren**. Hier ziet u de hybride verbindingen die zijn geconfigureerd voor uw app.  

![Schermafbeelding van Hybrid Connection list][2]

Als u wilt een nieuwe hybride verbinding toevoegen, selecteert u **[+] hybride verbinding toevoegen**.  U ziet een lijst van de hybride verbindingen die u al hebt gemaakt. Een of meer van deze toevoegen aan uw app, selecteer de documentsjablonen die u wilt en selecteer vervolgens **toevoegen geselecteerde hybride verbinding**.  

![Schermafbeelding van de hybride verbinding-portal][3]

Als u een nieuwe hybride verbinding maken wilt, selecteert u **nieuwe hybride verbinding maken**. Geef de: 

- Naam van hybride verbinding.
- De hostnaam van eindpunt.
- Poort van het eindpunt.
- Service Bus-naamruimte die u wilt gebruiken.

![Schermopname maken van nieuwe hybride verbinding in het dialoogvenster][4]

Elke hybride verbinding is gekoppeld aan een Service Bus-naamruimte, en elke Service Bus-naamruimte is in een Azure-regio. Het is belangrijk om te proberen te gebruiken van een Service Bus-naamruimte in dezelfde regio als uw app om te voorkomen dat veroorzaakte netwerklatentie.

Als u uw hybride verbinding verwijderen uit uw app wilt, met de rechtermuisknop en selecteer **verbinding verbreken**.  

Wanneer een hybride verbinding is toegevoegd aan uw app, kunt u details weergeven op het gewoon door deze te selecteren. 

![Schermafbeelding van hybride verbindingen details][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Een hybride verbinding maken in de portal voor Azure Relay ###

Naast de portal-ervaring van binnen uw app, kunt u hybride verbindingen van maken in de Azure Relay-portal. Voor een hybride verbinding moet worden gebruikt door App Service, moet deze:

* Clientautorisatie nodig is.
* Een metagegevensitem bevatten, de naam van eindpunt, die een combinatie van de host: poort als de waarde bevat.

## <a name="hybrid-connections-and-app-service-plans"></a>Hybride verbindingen en App Service-plannen ##

App Service Hybrid Connections zijn alleen beschikbaar in Basic, Standard, Premium en geïsoleerde prijs-SKU's. Er zijn beperkingen die zijn gekoppeld aan de prijsstelling.  

| Het prijsplan | Het aantal hybride verbindingen worden gebruikt in het plan |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

De gebruikersinterface van de App Service-plan ziet u het aantal hybride verbindingen worden gebruikt en door welke apps.  

![Schermopname van App Service-plan-eigenschappen][6]

Selecteer de hybride verbinding om details te bekijken. Hier ziet u alle informatie die u hebt gezien in de weergave van de app. U kunt ook zien hoe veel andere apps in hetzelfde abonnement met behulp van de hybride verbinding.

Er geldt een limiet voor het aantal hybride-verbindingseindpunten die kunnen worden gebruikt in een App Service-plan. Elke hybride verbinding gebruikt, maar kan worden gebruikt in alle apps in het plan. Bijvoorbeeld, telt een enkel hybride verbinding die wordt gebruikt in vijf afzonderlijke apps in een App Service-plan als een hybride verbinding.

### <a name="pricing"></a>Prijzen ###

Naast er wordt een App Service-plan SKU vereiste, is er een extra kosten voor het gebruik van hybride verbindingen. Er is een post voor elke listener die worden gebruikt door een hybride verbinding. De listener is hybride Verbindingsbeheer. Als u vijf hybride verbindingen ondersteund door twee Hybrid Connection Managers had, zou dat 10 listeners zijn. Zie voor meer informatie, [prijzen van Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

De functie hybride verbindingen vereist een relay-agent in het netwerk die als host fungeert voor uw eindpunt voor de hybride verbinding. De Hybrid Connection Manager (HCM), die relay-agent wordt genoemd. Voor het downloaden van HCM, vanuit uw app in de [Azure-portal][portal], selecteer **netwerken** > **uw hybride-verbindingseindpuntenconfigureren**.  

Dit hulpprogramma wordt uitgevoerd op Windows Server 2012 en hoger. De HCM wordt uitgevoerd als een service en verbindt u uitgaand naar Azure Relay op poort 443.  

U kunt na de installatie van HCM, HybridConnectionManagerUi.exe voor het gebruik van de gebruikersinterface voor het hulpprogramma uitvoeren. Dit bestand is in de installatiemap Hybrid Connection Manager. In Windows 10, u kunt ook zoeken naar *Hybrid Connection Manager UI* in het zoekvak.  

![Schermafbeelding van de Hybrid Connection Manager][7]

Wanneer u de UI HCM start, is het eerste wat dat u ziet een tabel met een lijst met alle hybride verbindingen die zijn geconfigureerd met dit exemplaar van de HCM. Als u wijzigen wilt, eerst worden geverifieerd met Azure. 

Een of meer hybride verbindingen toevoegen aan uw HCM:

1. Start de HCM-gebruikersinterface.
1. Selecteer **configureren van een andere hybride verbinding**.
![Schermafbeelding van nieuwe hybride verbindingen configureren][8]

1. Meld u aan met uw Azure-account.
1. Kies een abonnement.
1. Selecteer de hybride verbindingen die u wilt dat de HCM om door te sturen.
![Schermafbeelding van hybride verbindingen][9]

1. Selecteer **Opslaan**.

U ziet nu de hybride verbindingen die u hebt toegevoegd. U kunt ook selecteren op de geconfigureerde hybride verbinding om details te bekijken.

![Schermafbeelding van de gegevens van hybride verbinding][10]

Ter ondersteuning van de hybride verbindingen wordt deze geconfigureerd met HCM zijn vereist:

- TCP-toegang tot Azure via poort 443.
- TCP-toegang tot het eindpunt van de hybride verbinding.
- De mogelijkheid om DNS te zoeken op de host van het eindpunt en de Service Bus-naamruimte te doen.

> [!NOTE]
> Azure Relay is afhankelijk van Websockets voor connectiviteit. Deze mogelijkheid is alleen beschikbaar op Windows Server 2012 of hoger. Vanwege dat wordt HCM niet ondersteund op Alles ouder dan Windows Server 2012.
>

### <a name="redundancy"></a>Redundantie ###

Elke HCM kan meerdere hybride verbindingen ondersteunen. Alle opgegeven hybride verbinding kan ook worden ondersteund door meerdere HCMs. Het standaardgedrag is het routeren van verkeer via de geconfigureerde HCMs voor een bepaald eindpunt. Als u hoge beschikbaarheid voor uw hybride verbindingen van uw netwerk wilt, moet u meerdere HCMs uitgevoerd op afzonderlijke computers. De distributie load algoritme gebruikt door de Relay-service voor het distribueren van verkeer naar de HCMs is willekeurige toewijzing. 

### <a name="manually-add-a-hybrid-connection"></a>Handmatig een hybride verbinding toevoegen ###

Deel de verbindingsreeks van de gateway voor de hybride verbinding mee zodat iemand buiten uw abonnement voor het hosten van een HCM-exemplaar voor een bepaalde hybride verbinding. U kunt zien dat de gateway-verbindingsreeks in de eigenschappen van de hybride verbinding in de [Azure-portal][portal]. Selecteer voor het gebruik van die tekenreeks **handmatig invoeren** in de HCM en plak in de verbindingsreeks van de gateway.

![Handmatig een hybride verbinding toevoegen][11]

### <a name="upgrade"></a>Upgraden ###

Er zijn periodieke updates naar de Hybrid Connection Manager oplossen van problemen of verbeteringen bieden. Als de upgrades worden vrijgegeven, wordt een pop-upvenster in de UI HCM weergegeven. Toepassen van de upgrade, wordt de wijzigingen toepassen en opnieuw opstarten van de HCM. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Een hybride verbinding toevoegen via een programma aan uw app ##

De API's zoals hieronder vermeld kan rechtstreeks naar het beheren van de hybride verbindingen die zijn verbonden met uw web-apps worden gebruikt. 

    /subscriptions/[subscription name]/resourceGroups/[resource group name]/providers/Microsoft.Web/sites/[app name]/hybridConnectionNamespaces/[relay namespace name]/relays/[hybrid connection name]?api-version=2016-08-01

Het JSON-object dat is gekoppeld aan een hybride verbinding ziet eruit zoals:

    {
      "name": "[hybrid connection name]",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "[location]",
      "properties": {
        "serviceBusNamespace": "[namespace name]",
        "relayName": "[hybrid connection name]",
        "relayArmUri": "/subscriptions/[subscription id]/resourceGroups/[resource group name]/providers/Microsoft.Relay/namespaces/[namespace name]/hybridconnections/[hybrid connection name]",
        "hostName": "[endpoint host name]",
        "port": [port],
        "sendKeyName": "defaultSender",
        "sendKeyValue": "[send key]"
      }
    }

Een manier om deze informatie te gebruiken, is met de armclient, die u kunt ophalen uit de [ARMClient] [ armclient] github-project. Hier volgt een voorbeeld van een bestaande hybride verbinding toevoegen aan uw web-app. Maak een JSON-bestand per het bovenstaande schema, zoals:

    {
      "name": "relay-demo-hc",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "North Central US",
      "properties": {
        "serviceBusNamespace": "demo-relay",
        "relayName": "relay-demo-hc",
        "relayArmUri": "/subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myrelay-rg/providers/Microsoft.Relay/namespaces/demo-relay/hybridconnections/relay-demo-hc",
        "hostName": "my-wkstn.home",
        "port": 1433,
        "sendKeyName": "defaultSender",
        "sendKeyValue": "Th9is3is8a82lot93of3774stu887ff122235="
      }
    }

Voor het gebruik van deze API, moet u het verzenden en omleiding van sleutel resource-ID. Als u uw gegevens met de bestandsnaam hctest.json opgeslagen, voert u deze opdracht uit uw hybride verbinding toevoegen aan uw app: 

    armclient login
    armclient put /subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myapp-rg/providers/Microsoft.Web/sites/myhcdemoapp/hybridConnectionNamespaces/demo-relay/relays/relay-demo-hc?api-version=2016-08-01 @hctest.json

## <a name="troubleshooting"></a>Problemen oplossen ##

De status van de 'Verbonden' betekent dat ten minste één HCM is geconfigureerd met de hybride verbinding, en kan bereiken van Azure. Als de status voor de hybride verbinding niet de tekst **verbonden**, uw hybride verbinding is niet geconfigureerd op een HCM dat toegang tot Azure heeft.

De primaire reden dat clients geen verbinding met het eindpunt maken is omdat het eindpunt is opgegeven met behulp van een IP-adres in plaats van een DNS-naam. Als uw app het eindpunt van de gewenste niet bereiken kan en u een IP-adres gebruikt, kunt u overstappen naar een DNS-naam die geldig is op de host waarop de HCM wordt uitgevoerd. Controleer ook of de DNS-naam wordt omgezet correct op de host waarop de HCM wordt uitgevoerd. Controleer of er een verbinding van de host waarop de HCM wordt uitgevoerd naar het eindpunt van de hybride verbinding is.  

In App Service, kan het hulpprogramma tcpping worden aangeroepen vanuit de console geavanceerde hulpmiddelen (Kudu). Dit hulpprogramma kunt u zien als u toegang tot een TCP-eindpunt hebt, maar het geeft niet aan als u toegang tot een eindpunt voor de hybride verbinding hebt. Wanneer u het hulpprogramma in de console voor het eindpunt van een hybride verbinding gebruikt, alleen bevestigt u dat deze gebruikmaakt van een combinatie van host: poort.  

## <a name="biztalk-hybrid-connections"></a>Hybrid Connections van BizTalk ##

De vroege vorm van deze functie is aangeroepen BizTalk Hybrid Connections. Deze mogelijkheid einde van de levenscyclus is een fout op 31 mei 2018 en beëindiging van bewerkingen. Hybrid connections van BizTalk is verwijderd uit alle web-apps en zijn niet toegankelijk zijn via de portal of de API. Als u nog steeds deze oudere verbindingen die zijn geconfigureerd in de Hybrid Connection Manager hebt, wordt u ziet een status van stopgezet en weergeven van een instructie einde levenscyclus onder.

![BizTalk hybride verbindingen in de HCM][12]


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: https://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: https://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
