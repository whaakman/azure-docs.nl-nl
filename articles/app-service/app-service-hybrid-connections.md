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
ms.openlocfilehash: 677642e4e97523ed71ff5857ae27263743dca535
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service hybride verbindingen #

Hybride verbindingen is zowel een service in Azure en een functie in Azure App Service. Als een service heeft gebruikt en de mogelijkheden die worden gebruikt in App Service. Zie voor meer informatie over hybride verbindingen en hun gebruik buiten de App Service, [Azure Relay hybride verbindingen][HCService].

In App Service kan hybride verbindingen worden gebruikt voor toegang tot toepassingsbronnen in andere netwerken. Deze kunt openen vanuit uw app naar een toepassingseindpunt. Het is niet ingeschakeld op een alternatieve mogelijkheden voor toegang tot uw toepassing. Als in App Service gebruikt, wordt elke hybride verbinding heeft betrekking op een enkele TCP-host en poort combinatie. Dit betekent dat het eindpunt voor hybride verbinding kan op elk besturingssysteem en elke toepassing, mits u toegang hebben tot een TCP-luisterpoort. De functie hybride verbindingen niet weet of het belangrijkst wat het toepassingsprotocol is, of als u opent. Het levert gewoon toegang tot het netwerk.  


## <a name="how-it-works"></a>Hoe werkt het? ##
De functie hybride verbindingen bestaat uit twee uitgaande oproepen naar Azure Service Bus Relay. Er is een verbinding van een bibliotheek op de host waarop uw app in App Service wordt uitgevoerd. Er is ook een verbinding van de hybride verbinding Manager (HCM) met Service Bus Relay. De HCM is een relayservice die u in het netwerk die als host fungeert voor de resource die u probeert implementeert te openen. 

Met behulp van de twee gekoppelde verbindingen heeft uw app in een TCP-tunnel naar een combinatie van vaste hostnaam: poort op de andere kant van de HCM. De verbinding maakt gebruik van TLS 1.2 voor beveiliging en shared access signature (SAS)-sleutels voor verificatie en autorisatie.    

![Diagram van hoog niveau stroom van hybride verbinding][1]

Wanneer uw app een DNS-aanvraag die overeenkomt met een geconfigureerde eindpunt voor hybride verbinding maakt, wordt het uitgaande TCP-verkeer wordt omgeleid via de hybride verbinding.  

> [!NOTE]
> Dit betekent dat u proberen moet te gebruiken altijd een DNS-naam voor uw hybride-verbinding. Sommige clientsoftware doet een DNS-zoekopdracht als het eindpunt gebruikmaakt van een IP-adres in plaats daarvan.
>
>

De functie hybride verbindingen heeft twee typen: de hybride verbindingen die worden aangeboden als een Service Bus Relay-service en de oudere Azure BizTalk Services hybride verbindingen. De laatste worden aangeduid als klassieke hybride verbindingen in de portal. Er is meer informatie over deze verderop in dit artikel.

### <a name="app-service-hybrid-connection-benefits"></a>Voordelen van App Service hybride verbinding ###

Er zijn een aantal voordelen voor de functie hybride verbindingen, met inbegrip van:

- Apps hebben toegang tot on-premises systemen en services veilig.
- De functie is niet vereist voor een eindpunt toegankelijk is via het internet.
- Het is snel en eenvoudig te installeren. 
- Elke hybride verbinding komt overeen met naar een combinatie één hostnaam: poort handig voor beveiliging.
- Normaal gesproken hoeft niet firewall gaten. De verbindingen zijn alle uitgaande via standaard webpoorten.
- Omdat de functie netwerkniveau, is het agnostisch ten opzichte van de taal die wordt gebruikt door uw app en de technologie die wordt gebruikt door het eindpunt.
- Het kan worden gebruikt om toegang in meerdere netwerken uit een enkele app te bieden. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Wat u niet kunt doen met hybride verbindingen ###

Er zijn enkele dingen die u niet kunt met hybride verbindingen doen, met inbegrip van:

- Koppeling van een station.
- Met behulp van UDP.
- Toegang tot TCP-services die gebruikmaken van dynamische poorten, zoals de passieve modus van FTP of uitgebreide passieve modus.
- Ondersteuning voor LDAP, omdat soms UDP vereist.
- Ondersteuning voor Active Directory.

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Toevoegen en hybride verbindingen maken in uw app ##

U kunt een hybride verbindingen maken via uw App Service-app in de Azure portal of vanuit de Azure-Relay in de Azure portal. U wordt aangeraden dat u hybride verbindingen via de App Service-app die u wilt gebruiken met de hybride verbinding maken. Een hybride verbinding wilt maken, gaat u naar de [Azure-portal] [ portal] en selecteert u uw app. Selecteer **Networking** > **uw hybride-verbindingseindpunten configureren**. Hier ziet u de hybride verbindingen die zijn geconfigureerd voor uw app.  

![Schermopname van hybride verbinding lijst][2]

Selecteer om een nieuwe hybride verbinding toe **hybride verbinding toevoegen**.  Hier ziet u een lijst met de hybride verbindingen die u al hebt gemaakt. Als u wilt een of meer van deze toevoegen aan uw app, selecteer de documentsjablonen die u wilt en selecteer vervolgens **toevoegen geselecteerd hybride verbinding**.  

![Schermopname van hybride verbinding portal][3]

Als u een nieuwe hybride verbinding maken wilt, selecteert u **nieuwe hybride verbinding maken**. Geef de: 

- De naam van het eindpunt.
- Hostnaam van het eindpunt.
- Poort van het eindpunt.
- Service Bus-naamruimte die u wilt gebruiken.

![Schermopname maken van nieuwe hybride verbinding in het dialoogvenster][4]

Elke hybride verbinding is gekoppeld aan een Service Bus-naamruimte en elke Service Bus-naamruimte is in een Azure-regio. Het is belangrijk dat u probeert te gebruiken van een Service Bus-naamruimte in dezelfde regio bevinden als uw app om te voorkomen dat wordt veroorzaakt netwerklatentie.

Als u wilt uw hybride-verbinding verwijderen uit uw app, met de rechtermuisknop en selecteer **Disconnect**.  

Wanneer een hybride verbinding is toegevoegd aan uw app, ziet u informatie op deze gewoon door deze te selecteren. 

![Schermopname van hybride verbindingen details][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Een hybride verbinding maken in de Relay Azure-portal ###

Naast de portal-ervaring van binnen uw app kunt u hybride verbindingen van binnen de Relay Azure-portal. Voor een hybride verbinding moet worden gebruikt door App Service, moet deze:

* Clientautorisatie vereisen.
* Een metagegevensitem eindpunt, die een combinatie van hostnaam: poort als de waarde bevat met de naam hebben.

## <a name="hybrid-connections-and-app-service-plans"></a>Hybride verbindingen en App Service-plannen ##

De functie hybride verbindingen is alleen beschikbaar in Basic, Standard, Premium en geïsoleerd prijzen SKU's. Er gelden beperkingen die zijn gekoppeld aan de prijsstelling.  

> [!NOTE] 
> U kunt alleen nieuwe hybride verbindingen op basis van Azure Relay maken. U kunt geen nieuwe BizTalk hybride verbindingen maken.
>

| plan prijzen | Aantal hybride verbindingen kan worden gebruikt in het plan |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Houd er rekening mee dat de App Service-abonnement ziet u hoeveel hybride verbindingen worden gebruikt en door welke apps.  

![Schermopname van App Service plan eigenschappen][6]

Selecteer de hybride verbinding om details te bekijken. Hier ziet u de informatie die u hebt gezien in de weergave van de app. U ziet ook hoe veel apps in hetzelfde abonnement dat hybride verbinding gebruikt.

Er is een limiet van het aantal hybride-verbindingseindpunten die kunnen worden gebruikt in een App Service-plan. Elke hybride verbinding gebruikt, echter kunnen worden gebruikt in een willekeurig aantal apps in dat plan. Bijvoorbeeld, telt een enkele hybride verbinding die wordt gebruikt in vijf afzonderlijke apps in App Service-abonnement als een hybride verbinding.

Er is een extra kosten voor het gebruik van hybride verbindingen. Zie voor meer informatie [Service Bus prijzen][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybride Verbindingsbeheer ##

De functie hybride verbindingen vereist een relay-agent in het netwerk die als host fungeert voor uw eindpunt hybride verbinding. De hybride verbinding Manager (HCM), die relay-agent wordt genoemd. HCM, downloaden van uw app in de [Azure-portal][portal], selecteer **Networking** > **uw hybride-verbindingseindpuntenconfigureren**.  

Dit hulpprogramma wordt uitgevoerd op Windows Server 2012 en hoger. Wanneer geïnstalleerd, wordt als een service die is verbonden met Service Bus Relay, op basis van de geconfigureerde eindpunten HCM uitgevoerd. De verbindingen van HCM zijn uitgaand naar Azure via poort 443.    

U kunt na de installatie van HCM HybridConnectionManagerUi.exe voor het gebruik van de gebruikersinterface voor het hulpprogramma uitvoeren. Dit bestand is in de installatiemap hybride Verbindingsbeheer. In Windows 10, u kunt ook zoeken naar *hybride Verbindingsbeheer UI* in het zoekvak.  

![Schermopname van hybride Verbindingsbeheer][7]

Wanneer u de UI HCM start, is het eerste wat dat u ziet een tabel met een lijst met alle hybride verbindingen die zijn geconfigureerd met dit exemplaar van de HCM. Als u wijzigen wilt, eerst worden geverifieerd bij Azure. 

Een of meer hybride verbindingen toevoegen aan uw HCM:

1. Start de gebruikersinterface HCM.
1. Selecteer **configureren van een andere hybride verbinding**.
![Schermopname van nieuwe hybride verbindingen configureren][8]

1. Aanmelden met uw Azure-account.
1. Kies een abonnement.
1. Selecteer de hybride verbindingen die u wilt dat de HCM om door te sturen.
![Schermopname van hybride verbindingen][9]

1. Selecteer **Opslaan**.

U ziet nu de hybride verbindingen die u hebt toegevoegd. Ook kunt u de geconfigureerde hybride verbinding om details te bekijken.

![Schermafbeelding van de Details voor hybride verbinding][10]

Ter ondersteuning van de hybride verbindingen wordt deze geconfigureerd met HCM zijn vereist:

- TCP-toegang tot Azure poorten 80 en 443.
- TCP-toegang tot het eindpunt voor hybride verbinding.
- De mogelijkheid om DNS te zoeken op de host eindpunt en de Service Bus-naamruimte doen.

HCM ondersteunt zowel nieuwe hybride verbindingen en BizTalk hybride verbindingen.

> [!NOTE]
> Azure Relay is afhankelijk van de Web-Sockets voor connectiviteit. Deze functie is alleen beschikbaar op Windows Server 2012 of hoger. Vanwege die wordt HCM niet ondersteund voor andere ouder dan Windows Server 2012.
>

### <a name="redundancy"></a>Redundantie ###

Elke HCM kan meerdere hybride verbindingen ondersteunen. Opgegeven hybride verbindingen kan ook worden ondersteund door meerdere HCMs. Het standaardgedrag is voor het routeren van verkeer tussen de geconfigureerde HCMs voor een willekeurig eindpunt opgegeven. Als u hoge beschikbaarheid voor uw hybride verbindingen vanaf uw netwerk wilt, voert u meerdere HCMs op afzonderlijke computers. 

### <a name="manually-add-a-hybrid-connection"></a>Handmatig toevoegen van een hybride verbinding ###

Delen de gateway-verbindingsreeks voor de hybride verbinding ermee zodat iemand buiten uw abonnement voor het hosten van een HCM-exemplaar voor een gegeven hybride verbinding. U kunt dit zien in de eigenschappen voor een hybride verbinding in de [Azure-portal][portal]. Selecteer voor het gebruik van deze reeks **handmatig invoeren** in de HCM en plakken in de verbindingsreeks voor de gateway.


## <a name="troubleshooting"></a>Problemen oplossen ##

De status van de 'Verbonden' betekent dat ten minste één HCM is geconfigureerd met deze hybride verbinding, en kunnen bereiken van Azure. Als de status voor uw hybride verbinding niet de tekst **verbonden**, uw hybride verbinding is niet geconfigureerd op een HCM die toegang tot Azure heeft.

De primaire reden dat clients geen verbinding met hun eindpunt maken is omdat het eindpunt met behulp van een IP-adres in plaats van een DNS-naam is opgegeven. Als uw app het gewenste eindpunt niet bereiken kan en u een IP-adres gebruikt, overschakelen op een DNS-naam die geldig is op de host waarop de HCM wordt uitgevoerd. Controleer ook of de DNS-naam goed wordt omgezet op de host waarop de HCM wordt uitgevoerd. Bevestig dat er verbinding van de host waarop de HCM wordt uitgevoerd met het eindpunt voor hybride verbinding is.  

In App Service kan het hulpprogramma tcpping worden aangeroepen vanuit de console geavanceerde's (Kudu). Dit hulpprogramma kunt u zien als u toegang tot een TCP-eindpunt hebt, maar het geeft niet aan als u toegang tot een hybride verbinding-eindpunt hebt. Wanneer u het hulpprogramma in de console voor het eindpunt van een hybride verbinding gebruiken, alleen bevestigt u dat deze gebruikmaakt van een combinatie van hostnaam: poort.  

## <a name="biztalk-hybrid-connections"></a>Hybrid Connections van BizTalk ##

De mogelijkheid van de oudere BizTalk hybride verbindingen is naar de nieuwe BizTalk hybride verbindingen gesloten. U kunt doorgaan met het gebruik van uw bestaande BizTalk hybride verbindingen met uw apps, maar u moet migreren naar de nieuwe hybride verbindingen die gebruikmaken van Azure Relay. Een van de voordelen in de nieuwe service via de BizTalk-versie zijn:

- Er is geen aanvullende BizTalk-account is vereist.
- TLS is versie 1.2 in plaats van versie 1.0.
- Communicatie over poorten 80 en 443 is en een DNS-naam gebruikt voor het bereiken van Azure, in plaats van IP-adressen en een aantal extra poorten.  

Als u wilt een bestaande BizTalk hybride verbinding toevoegen aan uw app, gaat u naar uw app in de [Azure-portal][portal], en selecteer **Networking** > **configureren uw hybride-verbindingseindpunten**. Selecteer in de tabel klassieke hybride verbindingen **klassieke hybride verbinding toevoegen**. Vervolgens ziet u een lijst met uw BizTalk hybride verbindingen.  


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
