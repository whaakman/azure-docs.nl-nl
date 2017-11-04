---
title: Azure App Service hybride verbindingen | Microsoft Docs
description: Het maken en hybride verbindingen gebruiken voor toegang tot bronnen in verschillende netwerken
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: f0b148cb9c304c54b47be9601740e7634462d59b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service hybride verbindingen #

## <a name="overview"></a>Overzicht ##

Hybride verbindingen is zowel een service in Azure als een functie in de Azure App Service.  Als een service heeft gebruikt en de mogelijkheden die worden gebruikt in Azure App Service.  Voor meer informatie over hybride verbindingen en hun gebruik buiten de Azure App Service kunt u hier starten [Azure Relay hybride verbindingen][HCService]

In Azure App Service kan hybride verbindingen worden gebruikt voor toegang tot toepassingsbronnen in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt.  Het is niet ingeschakeld op een alternatieve mogelijkheden voor toegang tot uw toepassing.  Als in App Service gebruikt, wordt elke hybride verbinding heeft betrekking op een enkele TCP-host en poort combinatie.  Dit betekent dat het eindpunt voor hybride verbinding kan op elk besturingssysteem en elke toepassing, mits u zijn kunt u door een TCP-luisterpoort. Hybride verbindingen niet kent of wat het toepassingsprotocol is of als u opent het belangrijkst.  Ze bieden gewoon toegang tot het netwerk.  


## <a name="how-it-works"></a>Hoe werkt het? ##
De functie hybride verbindingen bestaat uit twee uitgaande aanroepen naar Service Bus Relay.  Er is een verbinding van een bibliotheek op de host waarop uw app in App Service wordt uitgevoerd en er is een verbinding van de hybride verbinding Manager (HCM) met Service Bus Relay.  De HCM is een relayservice die u in het netwerk die als host fungeert voor de resource die u probeert implementeert te openen. 

Met behulp van de twee gekoppelde verbindingen heeft uw app in een TCP-tunnel naar een combinatie van vaste hostnaam: poort op de andere kant van de HCM.  De verbinding maakt gebruik van TLS 1.2 op beveiligings- en SAS-sleutels voor verificatie/autorisatie.    

![Hybride verbinding hoog niveau stroom][1]

Wanneer uw app een DNS-aanvraag die overeenkomt met een geconfigureerde eindpunt voor hybride verbinding maakt, wordt het uitgaande TCP-verkeer wordt omgeleid via de hybride verbinding.  

> [!NOTE]
> Dit betekent dat u proberen moet te gebruiken altijd een DNS-naam voor uw hybride-verbinding.  Sommige clientsoftware doet een DNS-zoekopdracht als het eindpunt gebruikmaakt van een IP-adres in plaats daarvan.
>
>

Er zijn twee soorten hybride verbindingen: de nieuwe hybride verbindingen die worden aangeboden als een service onder Azure Relay en de oudere BizTalk hybride verbindingen.  De oudere BizTalk hybride verbindingen worden aangeduid als klassieke hybride verbindingen in de portal.  Er is meer informatie verderop in dit document informatie hierover.

### <a name="app-service-hybrid-connection-benefits"></a>Voordelen van App Service hybride verbinding ###

Er zijn een aantal voordelen voor de functie hybride verbindingen, met inbegrip van:

- Apps veilig toegang tot on-premises systemen en services veilig.
- De functie is niet vereist voor een eindpunt toegankelijk is via het Internet.
- Het is snel en eenvoudig te installeren. 
- Elke hybride verbinding komt overeen met naar een combinatie van één hostnaam: poort, die een uitstekende beveiliging aspect is.
- Normaal gesproken hoeft niet firewall gaten als de verbindingen alle uitgaande via standaard webpoorten zijn.
- Omdat de functie netwerkniveau, is het agnostisch ten opzichte van de taal die wordt gebruikt door uw app en de technologie die wordt gebruikt door het eindpunt.
- Het kan worden gebruikt om toegang in meerdere netwerken uit een enkele app te bieden. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Wat u niet kunt doen met hybride verbindingen ###

Er zijn enkele dingen die u niet kunt met hybride verbindingen doen, met inbegrip van:

- koppelen van een station
- met behulp van UDP
- Toegang tot TCP op basis van services die gebruikmaken van dynamische poorten, zoals de passieve modus van FTP of uitgebreide passieve modus
- LDAP-ondersteuning, zoals deze soms UDP is vereist
- Active Directory-ondersteuning

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Toe te voegen en een hybride verbinding te maken in uw App ##

Hybride verbindingen kunnen worden gemaakt via uw App Service-app in de Azure portal of vanuit de Azure-Relay in de Azure portal.  Het is raadzaam dat u hybride verbindingen via de App Service-app die u wilt gebruiken met de hybride verbinding maken.  Een hybride verbinding wilt maken, gaat u naar de [Azure-portal] [ portal] en selecteert u uw app.  Selecteer **Networking > uw hybride-verbindingseindpunten configureren**.  Hier ziet u de hybride verbindingen die zijn geconfigureerd voor uw app.  

![Lijst met hybride verbindingen][2]

Klik op de hybride verbinding toevoegen als u een nieuwe hybride verbinding.  De gebruikersinterface die wordt geopend, worden de hybride verbindingen die u al hebt gemaakt.  Om een of meer van deze toevoegen aan uw app, klik op de computer die u wilt gebruiken en klik op **toevoegen geselecteerd hybride verbinding**.  

![Portal voor hybride verbinding][3]

Als u een nieuwe hybride verbinding maken wilt, klikt u op **nieuwe hybride verbinding maken**.  Hier kunt u opgeven de: 

- naam van het eindpunt
- hostnaam van het eindpunt
- Eindpuntpoort
- Service Bus-naamruimte die u wilt gebruiken

![Een hybride verbinding maken][4]

Elke hybride verbinding is gekoppeld aan een Service Bus-naamruimte en elke Service Bus-naamruimte is in een Azure-regio.  Het is belangrijk om te gebruiken van een Service Bus-naamruimte in dezelfde regio als uw app om te voorkomen dat wordt veroorzaakt netwerklatentie.

Als u wilt uw hybride-verbinding verwijderen uit uw app, klik hierop en selecteer **Disconnect**.  

Wanneer een hybride verbinding is toegevoegd aan uw app, ziet u details op dit door gewoon erop te klikken. 

![Details voor hybride verbinding][5]

### <a name="creating-a-hybrid-connection-in-the-azure-relay-portal"></a>Een hybride verbinding maken in de Relay Azure-portal ###

Naast de portal ervaring van binnen uw app is er ook een mogelijkheid voor het maken van hybride verbindingen van binnen de Relay Azure-portal. Om een hybride verbinding moet worden gebruikt door de Azure App Service aan deze twee criteria voldoet. Dit moet doen:

* Clientautorisatie vereisen
* Een metagegevensitem naam hebt gegeven eindpunt dat een combinatie van hostnaam: poort als de waarde bevat

## <a name="hybrid-connections-and-app-service-plans"></a>Hybride verbindingen en App Service-abonnementen ##

Hybride verbindingen zijn alleen beschikbaar in Basic, Standard, Premium en geïsoleerd prijzen SKU's.  Er gelden beperkingen die zijn gekoppeld aan de prijsstelling.  

> [!NOTE] 
> U kunt alleen nieuwe hybride verbindingen op basis van Azure Relay maken. U kunt geen nieuwe BizTalk hybride verbindingen maken.
>

| Plan prijzen | Aantal hybride verbindingen kan worden gebruikt in het plan |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Aangezien er beperkingen App Service-Plan, is er ook gebruikersinterface in de App Service-Plan dat toont hoe veel hybride verbindingen worden gebruikt en door welke apps.  

![Eigenschappen van App gevolgd plannen][6]

U kunt details op uw hybride verbinding zien door erop te klikken.  In de eigenschappen die hier worden weergegeven, ziet u alle informatie die u hebt gezien in de weergave van de app en u ziet ook hoe veel apps in de dezelfde App Service-Plan dat hybride verbinding gebruikt.

Er is een limiet van het aantal hybride-verbindingseindpunten die kunnen worden gebruikt in een App Service-Plan, kan elke hybride verbinding die wordt gebruikt over een aantal apps in deze App Service-Plan worden gebruikt.  Met andere woorden, telt een enkele hybride verbinding die wordt gebruikt in 5 afzonderlijke apps in een App Service-Plan als 1 hybride verbinding.

Er is een extra kosten voor het gebruik van hybride verbindingen.  Voor meer informatie over prijzen voor hybride verbinding gaat hier: [Service Bus prijzen][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybride Verbindingsbeheer ##

Voor hybride verbindingen werken moet, u een relay-agent in het netwerk die als host fungeert voor uw eindpunt hybride verbinding.  De hybride verbinding Manager (HCM), die relay-agent wordt genoemd.  Dit hulpprogramma kan worden gedownload vanaf de **Networking > uw hybride-verbindingseindpunten configureren** gebruikersinterface die beschikbaar zijn vanuit uw app in de [Azure-portal][portal].  

Dit hulpprogramma wordt uitgevoerd op WindowsServer 2012 en latere versies van Windows.  De HCM wordt uitgevoerd nadat deze is geïnstalleerd als een service.  Deze service verbindt met Azure Service Bus Relay op basis van de geconfigureerde eindpunten.  De verbindingen van de HCM zijn uitgaand naar Azure via poort 443.    

De HCM heeft een gebruikersinterface te configureren.  Nadat de HCM is geïnstalleerd, kunt u door het uitvoeren van de HybridConnectionManagerUi.exe die zich in de installatiemap hybride Verbindingsbeheer online zetten van de gebruikersinterface.  Deze ook eenvoudig op Windows 10 wordt bereikt door op te geven *hybride Verbindingsbeheer UI* in het zoekvak.  

![Portal voor hybride verbinding][7]

Wanneer de HCM UI wordt gestart, is het eerste wat dat u ziet een tabel met een lijst met alle hybride verbindingen die zijn geconfigureerd met dit exemplaar van de HCM.  Als u wijzigen wilt, moet u te verifiëren bij Azure. 

Een of meer hybride verbindingen toevoegen aan uw HCM:

1. Start de gebruikersinterface HCM
1. Klik op een andere hybride verbinding configureren ![een ch toevoegen in de HCM][8]

1. Meld u aan met uw Azure-account
1. Een abonnement kiezen
1. Klik op de hybride verbindingen die u wilt de HCM naar relay ![een CH selecteren][9]

1. Op Opslaan klikken

U ziet nu de hybride verbindingen die u hebt toegevoegd.  U kunt ook klikt u op de geconfigureerde hybride verbinding en meer details over it.

![CH-details][10]

Het moet voor uw HCM kunnen ter ondersteuning van de hybride verbindingen wordt deze geconfigureerd met:

- TCP-toegang tot Azure poorten 80 en 443
- TCP-toegang tot het eindpunt voor hybride verbinding
- Kan doen DNS te zoeken op de host eindpunt en de Azure Service Bus-naamruimte

De HCM ondersteunt zowel nieuwe hybride verbindingen, evenals de oudere BizTalk hybride verbindingen.

> [!NOTE]
> Azure Relay is afhankelijk van de Web-Sockets voor connectiviteit. Deze functie is alleen beschikbaar op Windows Server 2012 of nieuwer. Vanwege die het hybride Verbindingsbeheer wordt niet ondersteund voor andere ouder dan Windows Server 2012.
>

### <a name="redundancy"></a>Redundantie ###

Elke HCM kan meerdere hybride verbindingen ondersteunen.  Opgegeven hybride verbindingen kan ook worden ondersteund door meerdere HCMs.  Het standaardgedrag is round robin-verkeer via de geconfigureerde HCMs voor een willekeurig eindpunt opgegeven.  Desgewenst kunt u hoge beschikbaarheid voor uw hybride verbindingen vanaf het netwerk eenvoudig exemplaar maken van meerdere HCMs op afzonderlijke computers. 

### <a name="manually-adding-a-hybrid-connection"></a>Handmatig toevoegen van een hybride verbinding ###

Als u iemand buiten uw abonnement voor het hosten van een HCM-exemplaar voor een gegeven hybride verbinding wenst, kunt u de verbindingsreeks voor de gateway voor de hybride verbinding met hen delen.  U kunt dit zien in de eigenschappen voor een hybride verbinding in de [Azure-portal][portal]. Met deze reeks, klikt u op de **handmatig invoeren** knop in de HCM en plak in de verbindingsreeks voor de gateway.


## <a name="troubleshooting"></a>Problemen oplossen ##

De status van de verbinding voor een hybride verbinding betekent dat ten minste één HCM is geconfigureerd met deze hybride verbinding en kunnen bereiken van Azure.  Als de status voor uw hybride verbinding niet de tekst **verbonden**, en vervolgens uw hybride verbinding is niet geconfigureerd op een HCM die toegang tot Azure heeft.

De primaire reden dat clients geen verbinding met hun eindpunt maken is omdat het eindpunt met een IP-adres in plaats van een DNS-naam is opgegeven.  Als uw app het gewenste eindpunt niet bereiken kan en u een IP-adres gebruikt, overschakelen op een DNS-naam die geldig is op de host waarop de HCM wordt uitgevoerd.  Andere wat u moet controleren zijn dat de DNS-naam correct wordt omgezet door op de host waarop de HCM wordt uitgevoerd en dat er verbinding van de host waarop de HCM wordt uitgevoerd met het eindpunt voor hybride verbinding is.  

Er is een hulpprogramma in App Service, die kan worden aangeroepen vanuit de console van de geavanceerde's (Kudu) tcpping aangeroepen.  Dit hulpprogramma kunt u zien als u toegang tot een TCP-eindpunt hebt, maar het geeft niet aan als u toegang tot een hybride verbinding-eindpunt hebt.  Bij gebruik in de console voor het eindpunt van een hybride verbinding is een geslaagde ping alleen laat u weten dat er een hybride verbinding geconfigureerd voor uw app die gebruikmaakt van deze combinatie van hostnaam: poort.  

## <a name="biztalk-hybrid-connections"></a>Hybrid Connections van BizTalk ##

De oudere BizTalk hybride verbindingen mogelijkheid is gesloten uit naar de nieuwe BizTalk hybride verbindingen.  U kunt doorgaan met het gebruik van uw bestaande BizTalk hybride verbindingen met uw apps, maar u moet migreren naar de nieuwe hybride verbindingen die gebruikmaken van Azure Relay.  Een van de voordelen in de nieuwe service via de BizTalk-versie zijn:

- Er is geen aanvullende BizTalk-account is vereist.
- TLS is 1.2 in plaats van 1.0 zoals in BizTalk hybride verbindingen.
- Er is een communicatie via poorten 80 en 443 met behulp van een DNS-naam te bereiken Azure in plaats van IP-adressen en een aantal extra andere poorten.  

Als u wilt een bestaande BizTalk hybride verbinding toevoegen aan uw app, gaat u naar uw app in de [Azure-portal] [ portal] en klik op **Networking > uw hybride-verbindingseindpunten configureren**.  Klik in de tabel klassieke hybride verbindingen **klassieke hybride verbinding toevoegen**.  Hier ziet u een lijst met uw BizTalk hybride verbindingen.  


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

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/
