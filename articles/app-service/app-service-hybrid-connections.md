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
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service hybride verbindingen #

## <a name="overview"></a>Overzicht ##

Hybride verbindingen is zowel een service in Azure als een functie in de Azure App Service.  Als een service heeft het gebruik en mogelijkheden die worden gebruikt in de Azure App Service.  Voor meer informatie over hybride verbindingen en hun gebruik buiten de Azure App Service kunt u hier starten [Azure Relay hybride verbindingen][HCService]

In de Azure App Service kunnen hybride verbindingen worden gebruikt voor toegang tot toepassingsbronnen in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt.  Het is niet ingeschakeld op een alternatieve mogelijkheden voor toegang tot uw toepassing.  Als in de App-Service gebruikt, wordt elke hybride verbinding heeft betrekking op een enkele TCP-host en poort combinatie.  Dit betekent dat het eindpunt van hybride verbinding kan op elk besturingssysteem en alle toepassingen die u kunt u een TCP-luisterpoort zijn door. Hybride verbindingen niet weet of het belangrijkst wat het toepassingsprotocol is of als u opent.  Het levert gewoon toegang tot het netwerk.  


## <a name="how-it-works"></a>Hoe werkt het? ##
De functie voor hybride verbindingen bestaat uit twee uitgaande aanroepen naar Service Bus Relay.  Er is een verbinding van een bibliotheek op de host waarop uw app in app service wordt uitgevoerd en wordt er een verbinding tussen de hybride verbinding Manager(HCM) en Service Bus relay.  De HCM is een relayservice die u in het netwerk die als host fungeert implementeert 

Met behulp van de twee gekoppelde verbindingen heeft uw app in een TCP-tunnel naar een combinatie van vaste hostnaam: poort op de andere kant van de HCM.  De verbinding maakt gebruik van TLS 1.2 op beveiligings- en SAS-sleutels voor verificatie/autorisatie.    

![][1]

Wanneer uw app een DNS-aanvraag die overeenkomt met een eindpunt van de hybride verbinding configureren maakt, wordt het uitgaande TCP-verkeer worden omgeleid naar beneden de hybride verbinding.  

> [!NOTE]
> Dit betekent dat u proberen moet te gebruiken altijd een DNS-naam voor uw hybride-verbinding.  Sommige clientsoftware doet een DNS-zoekopdracht als het eindpunt gebruikmaakt van een IP-adres in plaats daarvan.
>
>

Er zijn twee soorten hybride verbindingen, de nieuwe hybride verbindingen die worden aangeboden als een Azure-Relay-service en de oudere BizTalk hybride verbindingen.  De oudere BizTalk hybride verbindingen worden aangeduid als klassieke hybride verbindingen in de portal.  Er is meer informatie verderop in dit document informatie hierover.

### <a name="app-service-hybrid-connection-benefits"></a>Voordelen van App Service hybride verbinding ###

Er zijn een aantal voordelen voor het hybride verbindingen mogelijkheden, zoals

- Apps veilig toegang tot op premises systemen en services veilig
- de functie vereist niet dat een internet toegankelijk eindpunt
- het is snel en eenvoudig te installeren  
- elke hybride verbinding met een combinatie van één hostnaam: poort die een uitstekende beveiliging aspect is komt overeen met
- Normaal gesproken hoeft niet firewall gaten als de verbindingen alle uitgaande via standaard webpoorten zijn
- omdat de functie netwerkniveau die ook dat betekent is agnostisch ten opzichte van de taal die wordt gebruikt door uw app en de technologie die wordt gebruikt door het eindpunt
- Dit kan worden gebruikt voor toegang in meerdere netwerken uit een enkele app 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Wat u niet kunt doen met hybride verbindingen ###

Er zijn enkele dingen die u niet kunt met hybride verbindingen doen en ze omvatten:

- koppelen van een station
- met behulp van UDP
- toegang tot TCP op basis van services die gebruikmaken van dynamische poorten zoals passieve modus van FTP of uitgebreide passieve modus
- LDAP-ondersteuning, zoals deze soms UDP is vereist
- Active Directory-ondersteuning

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Toe te voegen en een hybride verbinding te maken in uw App ##

Hybride verbindingen kunnen worden gemaakt via de portal van de app of via de serviceportal voor hybride verbinding.  Het is raadzaam dat u de app-portal gebruiken voor het maken van de hybride verbindingen die u wilt gebruiken met uw app.  Maken van een hybride verbinding gaat u naar de [Azure-portal] [ portal] en Ga in de gebruikersinterface voor uw app.  Selecteer **Networking > uw hybride-verbindingseindpunten configureren**.  Hier ziet u de hybride verbindingen die zijn geconfigureerd met uw app  

![][2]

Klik op de hybride verbinding toevoegen als u een nieuwe hybride verbinding.  De gebruikersinterface die wordt geopend, worden de hybride verbindingen die u al hebt gemaakt.  Om een of meer van deze toevoegen aan uw app, klik op de computer die u wilt gebruiken en klik op **toevoegen geselecteerd hybride verbinding**.  

![][3]

Als u een nieuwe hybride verbinding maken wilt, klikt u op **nieuwe hybride verbinding maken**.  Vanaf hier geeft u de: 

- naam van het eindpunt
- hostnaam van het eindpunt
- Eindpuntpoort
- Service Bus-naamruimte die u wilt gebruiken

![][4]

Elke hybride verbinding is gekoppeld aan een service bus-naamruimte en elke service bus-naamruimte is in een Azure-regio.  Het is belangrijk om te gebruiken van een service bus-naamruimte in dezelfde regio als uw app om te voorkomen dat wordt veroorzaakt netwerklatentie.

Als u wilt uw hybride-verbinding verwijderen uit uw app, klik hierop en selecteer **Disconnect**.  

Nadat een hybride verbinding is toegevoegd aan uw web-app, ziet u details op dit door gewoon erop te klikken.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hybride verbindingen en App Service-abonnementen ##

De enige hybride verbindingen die u kunt nu zijn de nieuwe hybride verbindingen.  Ze zijn alleen beschikbaar in Basic, Standard, Premium en geïsoleerd prijzen SKU's.  Er gelden beperkingen die zijn gekoppeld aan de prijsstelling.  

| Plan prijzen | Aantal hybride verbindingen kan worden gebruikt in het plan |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Omdat er beperkingen App Service-Plan is er ook gebruikersinterface in de App Service-Plan dat toont hoe veel hybride verbindingen worden gebruikt en door welke apps.  

![][6]

Net als met de app-weergave ziet u details op uw hybride-verbinding door erop te klikken.  In de eigenschappen die hier worden weergegeven u vindt alle informatie in de weergave van de app maar ziet ook hoe veel apps in de dezelfde App Service-Plan dat hybride verbinding gebruikt.

![][7]

Er is een limiet van het aantal hybride-verbindingseindpunten die kunnen worden gebruikt in een App Service-Plan, kan elke hybride verbinding die wordt gebruikt over een aantal apps in deze App Service-Plan worden gebruikt.  Dat wil zeggen dat als ik had 1 hybride verbinding die ik in 5 afzonderlijke apps in mijn App Service-Plan gebruikt, die is zich nog steeds 1 hybride verbinding is.

Er is een extra kosten voor hybride verbindingen, dan wordt alleen gebruikt in een Basic, Standard, Premium of geïsoleerde SKU.  Voor meer informatie over prijzen voor hybride verbinding hier gaat: [Service Bus prijzen][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybride Verbindingsbeheer ##

Om hybride verbindingen werken, moet u een relay-agent in het netwerk die als host fungeert voor uw eindpunt van hybride verbinding.  De hybride verbinding Manager (HCM), die relay-agent wordt genoemd.  Dit hulpprogramma kan worden gedownload vanaf de **Networking > uw hybride-verbindingseindpunten configureren** gebruikersinterface die beschikbaar zijn vanuit uw app in de [Azure-portal][portal].  

Dit hulpprogramma wordt uitgevoerd op WindowsServer 2008 R2 en latere versies van Windows.  De HCM wordt uitgevoerd nadat deze is geïnstalleerd als een service.  Deze service verbindt met Azure Service Bus relay op basis van de geconfigureerde eindpunten.  De verbindingen van de HCM zijn uitgaand naar poorten 80 en 443.    

De HCM heeft een gebruikersinterface te configureren.  Nadat de HCM is geïnstalleerd kunt u door het uitvoeren van de HybridConnectionManagerUi.exe die zich in de installatiemap hybride Verbindingsbeheer online zetten van de gebruikersinterface.  Deze ook eenvoudig op Windows 10 wordt bereikt door op te geven *hybride Verbindingsbeheer UI* in het zoekvak.  

Wanneer de HCM UI wordt gestart, is het eerste wat dat u ziet een tabel met een lijst met alle hybride verbindingen die zijn geconfigureerd met dit exemplaar van de HCM.  Als u aanbrengen van wijzigingen die u moet verifiëren met Azure wilt. 

Een of meer hybride verbindingen toevoegen aan uw HCM:

1. Start de gebruikersinterface HCM
1. Klik op een andere hybride verbinding configureren![][8]

1. Meld u aan met uw Azure-account
1. Een abonnement kiezen
1. Klik op de hybride verbindingen die u wilt dat deze HCM om door te sturen![][9]

1. Op Opslaan klikken

U ziet nu de hybride verbindingen die u hebt toegevoegd.  U kunt ook klikt u op de geconfigureerde hybride verbinding en informatie over de verbinding.

![][10]

Het moet voor uw HCM kunnen ter ondersteuning van de hybride verbindingen die wordt deze geconfigureerd met:

- TCP-toegang tot Azure poorten 80 en 443
- TCP-toegang tot het eindpunt van hybride verbinding
- kan doen DNS uiterlijk-ups op de host eindpunt en de azure service bus-naamruimte

De HCM ondersteunt zowel nieuwe hybride verbindingen als de oudere BizTalk hybride verbindingen.

### <a name="redundancy"></a>Redundantie ###

Elke HCM kan meerdere hybride verbindingen ondersteunen.  Opgegeven hybride verbindingen kan ook worden ondersteund door meerdere HCMs.  Het standaardgedrag is round robin-verkeer via de geconfigureerde HCMs voor een willekeurig eindpunt opgegeven.  Desgewenst kunt u hoge beschikbaarheid voor uw hybride verbindingen vanaf het netwerk eenvoudig exemplaar maken van meerdere HCMs op afzonderlijke computers. 

### <a name="manually-adding-a-hybrid-connection"></a>Handmatig toevoegen van een hybride verbinding ###

Als u iemand buiten uw abonnement voor het hosten van een HCM-exemplaar voor een bepaalde hybride verbinding wenst, kunt u delen met hen de gateway-verbindingsreeks voor de hybride verbinding.  U kunt dit zien in de eigenschappen voor een hybride verbinding in de [Azure-portal][portal]. Met deze reeks, klikt u op de **handmatig configureren** knop in de HCM en plak in de verbindingsreeks voor de gateway.


## <a name="troubleshooting"></a>Problemen oplossen ##

Wanneer uw hybride verbinding is ingesteld met een actieve toepassing en er ten minste één HCM die hybride verbinding geconfigureerd heeft, wordt de status dicteert **verbonden** in de portal.  Als u niet de tekst **verbonden** betekent dat dat uw app niet actief is of uw HCM out kan geen verbinding met Azure op poort 80 of 443 maken.  

De primaire reden dat clients geen verbinding met hun eindpunt maken is omdat het eindpunt met een IP-adres in plaats van een DNS-naam is opgegeven.  Als uw app het gewenste eindpunt niet bereiken kan en u een IP-adres gebruikt, overschakelen op een DNS-naam die geldig is op de host waarop de HCM wordt uitgevoerd.  Andere wat u moet controleren zijn dat de DNS-naam correct wordt omgezet door op de host waarop de HCM wordt uitgevoerd en dat er verbinding van de host waarop de HCM wordt uitgevoerd met het eindpunt van hybride verbinding is.  

Er is een hulpprogramma in de App-Service kan worden aangeroepen vanuit de console tcpping genaamd.  Dit hulpprogramma kunt u zien als u toegang tot een TCP-eindpunt hebt maar geeft niet aan als u toegang tot een hybride verbinding-eindpunt hebt.  Bij gebruik in de console voor een eindpunt van hybride verbinding is een geslaagde ping alleen laat u weten dat er een hybride verbinding geconfigureerd voor uw app die gebruikmaakt van deze combinatie van hostnaam: poort.  

## <a name="biztalk-hybrid-connections"></a>Hybrid Connections van BizTalk ##

De oudere BizTalk hybride verbindingen mogelijkheid is gesloten uitschakelen voor verdere BizTalk hybride verbinding maken.  U kunt doorgaan met het gebruik van uw bestaande BizTalk hybride verbindingen met uw apps, maar moet migreren naar de nieuwe service.  Een van de voordelen in de nieuwe service via de BizTalk-versie zijn:

- Er is geen aanvullende BizTalk-account is vereist
- TLS is 1.2 in plaats van 1.0 zoals in BizTalk hybride verbindingen
- Er is een communicatie via poorten 80 en 443 met behulp van een DNS-naam te bereiken Azure in plaats van IP-adressen en een aantal extra andere poorten.  

Als u wilt een hybride BizTalk-verbinding toevoegen aan uw app, gaat u naar uw app in de [Azure-portal] [ portal] en klik op **Networking > uw hybride-verbindingseindpunten configureren**.  Klik in de klassieke hybride verbindingen tabel **klassieke hybride verbinding toevoegen**.  Hier ziet u een lijst met uw BizTalk hybride verbindingen.  


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

