---
title: BizTalk Server EDI-oplossingen migreren naar technische handleiding voor BizTalk Services | Microsoft Docs
description: EDI migreren naar MABS; Microsoft Azure BizTalk Services
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 1b70fc3d199d7f1521acb534dafec8fb3e69500e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server EDI-oplossingen migreren naar BizTalk Services: technische handleiding

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Auteur: Tim Wieman en Mirchandani Mehrotra

Revisoren: Karthik Bharthy

Geschreven met behulp van: Microsoft Azure BizTalk Services – februari 2014-release.

## <a name="introduction"></a>Inleiding
Elektronische Data Interchange (EDI) is een van de meest gangbare manier waarop bedrijven exchange de gegevens elektronisch, ook als Business-to-Business- of B2B genoemd. BizTalk Server heeft EDI-ondersteuning voor via een tien jaar na de initiële release van BizTalk Server. Met BizTalk Services blijft Microsoft de ondersteuning voor EDI-oplossingen op de Microsoft Azure-platform. B2B-transacties zijn voornamelijk buiten een organisatie en daarom is het gemakkelijker te implementeren als deze is geïmplementeerd op een cloudplatform. Microsoft Azure biedt deze mogelijkheid via BizTalk Services.

Terwijl sommige klanten BizTalk Services als een platform 'greenfield' voor nieuwe EDI-oplossingen bekijkt, hebben veel klanten huidige BizTalk Server EDI-oplossingen die willen migreren naar Azure. Omdat BizTalk Services EDI is ontworpen is op basis van de entiteiten voor dezelfde sleutel als BizTalk Server EDI-architectuur (handel partners, entiteiten, overeenkomsten), het mogelijk om te migreren van BizTalk Server EDI-artefacten naar BizTalk Services.

Dit document worden enkele van de verschillen zijn betrokken bij migratie BizTalk Server EDI-artefacten BizTalk Services beschreven. Dit document wordt ervan uitgegaan dat een praktische kennis van de BizTalk Server EDI-verwerking en Trading Partner overeenkomsten. Zie voor meer informatie over BizTalk Server EDI [Trading Partner met behulp van BizTalk beheerserver](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welke versie van BizTalk Server EDI-artefacten kan worden gemigreerd naar BizTalk Services?
De BizTalk Server EDI-module is aanzienlijk verbeterd voor BizTalk Server 2010, wanneer deze opnieuw is gemodelleerd naar partners, profielen en overeenkomsten bevatten. BizTalk Services maakt gebruik van hetzelfde model om de handelspartners en de zakelijke afdelingen binnen deze handelspartners te organiseren. Als gevolg hiervan is EDI-artefacten van BizTalk Server 2010 en nieuwere versies voor BizTalk Services migreert, een veel meer eenvoudig proces. Als u wilt migreren die zijn gekoppeld aan versies voorafgaand aan de BizTalk Server 2010 EDI-artefacten, moet u eerst een upgrade naar BizTalk Server 2010 en vervolgens uw EDI-artefacten te migreren naar BizTalk Services.

## <a name="scenariosmessage-flow"></a>Scenario's / berichtenstroom
Als met BizTalk Server is EDI verwerking van BizTalk Services gebaseerd op een oplossing Trading Partner Management (TPM). De TPM-oplossing heeft de volgende hoofdonderdelen:

* Handelspartners, die de organisatie in een transactie B2B vertegenwoordigen.
* Profielen die afdelingen binnen een trading partner vertegenwoordigen.
* Trading partner overeenkomsten (of overeenkomsten) die staan voor de zakelijke overeenkomst tussen de twee partners/profielen.

De volgende afbeelding ziet de overeenkomsten, evenals de verschillen tussen een BizTalk Server EDI-oplossing en de BizTalk Services EDI-oplossing:

![][EDImessageflow]

De belangrijkste verschillen en overeenkomsten tussen een stroom EDI-oplossing in BizTalk Server en BizTalk Services zijn:

* Net zoals BizTalk Server gebruikmaakt van een pijplijn EDIReceive voor het ontvangen van een EDI-bericht en een pijplijn EDISend EDI-berichten wilt verzenden, gebruikt de BizTalk Services een brug EDI ontvangen voor het ontvangen en verzenden EDI-brug EDI-berichten verzenden. BizTalk Server pijplijnen zijn gekoppeld aan een overeenkomst met behulp van verzenden of ontvangen van poorten. De overeenkomst zelf geeft de verzenden of ontvangen over de sitekoppelingsbrug in BizTalk Services.
* Nadat de pijplijn EDIReceive het EDI-bericht verwerkt, wordt het bericht in BizTalk Server gedumpt met een SQL Server-database. De pijplijn EdiSend vervolgens haalt het bericht uit de SQL Server-database, verwerkt en verstuurt het vervolgens aan de trading partner.
  
    In BizTalk Services nadat de EDI ontvangen bridge verwerkt het EDI-bericht, wordt het bericht gerouteerd naar een extern proces. Het externe proces kan worden uitgevoerd op Microsoft Azure of on-premises. Het externe proces moet het bericht doorsturen naar de EDI verzenden brug; het bericht heeft geen inherent pull-de brug verzenden. Na het verwerken van het bericht, stuurt de brug EDI-verzenden het bericht naar de trading partner.

BizTalk Services biedt een ervaring configuratie eenvoudig te gebruiken om snel te maken en implementeren van een B2B-overeenkomst tussen handelspartners zonder configureren van een Microsoft Azure Compute-exemplaren (Web- of Worker rollen), een Microsoft Azure SQL-Databases of een Microsoft Azure storage-accounts. Complexere scenario's verbinden in werkstromen of andere service-verwerking is vereist 'rond de randen' van een Trading Partner-overeenkomst, dat wil zeggen vóór of na de verwerking van Trading Partner Agreement EDI-bridge. In de details over optreden de volgende reeks gebeurtenissen tijdens een EDI-berichtverwerking in BizTalk Services.

1. Een EDI-bericht is ontvangen van trading partner Fabrikam.  Voor EDI-berichten ontvangen van handelspartners, ondersteunt BizTalk Services transportprotocollen zoals FTP-, SFTP AS2 en HTTP-/ S.
2. De trading partner agreement ontvangstzijde verwerking worden ontleed EDI-bericht naar XML-indeling.  U kunt het EDI-bericht (in XML-indeling) versturen naar Service Bus-eindpunten zoals een Service Bus Relay-eindpunt, Service Bus-onderwerp, Service Bus-wachtrij of een brug BizTalk Services.
3. De XML-berichten kunnen vervolgens worden ontvangen van het eindpunt voor verdere aangepaste verwerking.  Deze eindpunten kunnen bijvoorbeeld worden verwerkt door het onderdeel van een lokale of een exemplaar van Microsoft Azure Compute verdere verwerking van het bericht in een Windows-werkstroom (WF) of Windows Communication Foundation (WCF)-service.
4. De 'verzendkant verwerking' van de trading partner agreement vervolgens het XML-bericht ophaalprotocol naar EDI-indeling en verzendt het naar trading partner, Contoso.  BizTalk Services ondersteuning EDI-berichten verzonden naar handelspartners, dezelfde protocollen als die worden gebruikt voor het ontvangen van EDI-berichten.

Dit document verder bevat algemene richtlijnen op sommige andere BizTalk Server EDI-artefacten naar BizTalk Services migreert.

## <a name="sendreceive-ports-to-trading-partners"></a>Verzenden/ontvangen poorten voor handelspartners
BizTalk Server u locaties ontvangen en instellen ontvangen poorten EDI/XML om berichten te ontvangen van handelspartners, en u Send poorten EDI/XML om berichten te verzenden naar trading partner instellen. U bezighouden deze poorten op een handelspartnerovereenkomst met behulp van de beheerconsole van BizTalk Server vervolgens. In de locaties waar u berichten ontvangt van handelspartners van BizTalk Services en waarop u verzendt berichten naar handelspartners zijn geconfigureerd als onderdeel van de handelspartnerovereenkomst zelf (als onderdeel van Transport-instellingen) in de Portal van BizTalk Services.  Zodat u echt geen het concept van 'send poorten' en 'ontvangen locaties', per definitie in BizTalk Services. Zie voor meer informatie [overeenkomsten maken](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pijplijnen (bruggen)
In de BizTalk Server EDI zijn pijplijnen bericht verwerking entiteiten die kunnen ook aangepaste regels voor de van de specifieke verwerkingsmogelijkheden, zoals vereist door de toepassing bevatten. Voor BizTalk Services zou hetzelfde als een brug EDI. Maar in BizTalk Services op dit moment de EDI-bruggen 'gesloten'.  U kunt uw eigen aangepaste activiteiten dat wil zeggen, niet toevoegen aan een EDI-bridge. De aangepaste verwerking moet worden uitgevoerd buiten de EDI-brug in uw toepassing voor of na het bericht de brug geconfigureerd als onderdeel van de overeenkomst Trading Partner krijgt. EAI-bruggen hebt de mogelijkheid aangepaste verwerking. Als u aangepaste verwerking wilt, kunt u EAI-bruggen vóór of na het bericht is verwerkt door de EDI-brug is ingesteld. Zie voor meer informatie [hoe aangepaste Code opnemen in bruggen](https://msdn.microsoft.com/library/azure/dn232389.aspx).

U kunt een stroom publiceren/abonneren met aangepaste code en/of met behulp van Service Bus-wachtrijen en onderwerpen messaging voordat de handelspartnerovereenkomst ontvangt het bericht of nadat de overeenkomst verwerkt het bericht en doorgestuurd naar een Service Bus-eindpunt kunt invoegen.

Zie **scenario's / berichtenstroom** in dit onderwerp voor het bericht stroom-patroon.

## <a name="agreements"></a>Overeenkomsten
Als u bekend met de BizTalk Server 2010 handel Partner overeenkomsten voor EDI-bewerking gebruikt bent, zijn BizTalk Services handelspartnerovereenkomsten zoekt bekend. De meeste van de instellingen van de overeenkomst zijn hetzelfde en de dezelfde terminologie gebruiken. In sommige gevallen kan de instellingen van de overeenkomst zijn veel eenvoudiger vergeleken met dezelfde instellingen in BizTalk Server. Microsoft Azure BizTalk Services ondersteunt X12, EDIFACT en AS2-transport.

Microsoft Azure BizTalk Services biedt ook een **TPM gegevensmigratie** hulpprogramma voor het migreren van handelspartners partners en overeenkomsten uit module BizTalk Server Trading Partner bij BizTalk Services-Portal. Het hulpprogramma voor gegevensmigratie TPM is beschikbaar als onderdeel van een pakket hulpprogramma's die kunnen worden gedownload vanuit de [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Het pakket bevat ook een Leesmij-bestand met instructies over het gebruik van het hulpprogramma en elementaire informatie over probleemoplossing voor het hulpprogramma.

## <a name="schemas"></a>Schema 's
BizTalk Services biedt EDI-schema's die kunnen worden gebruikt in BizTalk Services-oplossingen.  BizTalk Server EDI-schema's kunnen bovendien ook worden gebruikt met BizTalk Services omdat het hoofdknooppunt van het EDI-schema hetzelfde voor BizTalk Server, evenals BizTalk Services is. Zo kunt u zich rechtstreeks duren voordat uw BizTalk Server EDI-schema's en deze gebruiken in de EDI-oplossingen die u ontwikkelt met behulp van BizTalk Services. U kunt ook downloaden de schema's van de [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Maps (transformaties)
Toewijzingen in BizTalk Server heten transformaties in BizTalk Services. Toewijzingen van de BizTalk Server migreren naar BizTalk Services mogelijk een van de complexere taken te bereiken (afhankelijk van de complexiteit van de kaart). De toewijzingsfunctie voor BizTalk Services gebruikt, wijkt af van de BizTalk-toewijzing. Hoewel de mapper ziet er meestal hetzelfde uit, worden de onderliggende toewijzingsindeling verschilt. De functoids (aangeroepen **kaart Operations** in BizTalk Services) beschikbaar zijn voor de gebruikers zijn ook verschillend.  In feite kan niet rechtstreeks u een BizTalk-kaart in BizTalk Services. Niet alle functoids in BizTalk Server beschikbaar zijn ook beschikbaar als de bewerkingen van de kaart in BizTalk Services.

### <a name="new-transform-operations"></a>Nieuwe transformatiebewerkingen
Terwijl de lijst met kaart transformatiebewerkingen beschikbaar heel anders dan de mapper BizTalk Server lijkt, hebben transformeert van BizTalk Services nieuwe manieren voor het uitvoeren van de dezelfde taken. BizTalk Services transformeert bijvoorbeeld **bewerkingen na opvragen** beschikbaar. Dit is niet beschikbaar in de BizTalk-toewijzing.  De **bewerkingen na opvragen** zodat u kunt maken en gebruiken op een 'List', waarbij een lijst met een reeks items (ook wel bekend als ' rijen') en waarbij elk item kan meerdere leden (ook wel bekend als ' kolommen') hebben.  U kunt de lijst sorteren, selecteert u items op basis van een voorwaarde, enzovoort.

Een ander voorbeeld van de nieuwe functionaliteit in BizTalk Services transformeert zijn de **lus Operations**.  Het is moeilijk geneste lussen maken in de BizTalk Server-toewijzing.  Zo worden de bewerkingen van de kaart lus toegevoegd voor de BizTalk Services transformaties.

Nog een ander voorbeeld is de **If vervolgens Else** expressie kaart bewerking.  Met een if-then-else bewerking is mogelijk in BizTalk mapper, maar het vereist dat meerdere functoids een schijnbaar eenvoudige taak uit te voeren.

### <a name="migrating-biztalk-server-maps"></a>Toegewezen BizTalk-Server migreren
Microsoft Azure BizTalk Services biedt een hulpprogramma voor het migreren van BizTalk Server toegewezen aan BizTalk Services transformaties. De **BTMMigrationTool** is beschikbaar als onderdeel van de **extra** pakket voorzien van de [BizTalk Services SDK downloaden](http://go.microsoft.com/fwlink/p/?LinkId=235057). Zie voor meer informatie over het hulpprogramma [omzetten in een kaart BizTalk een transformatie van BizTalk Services](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

U kunt ook zoeken op een codevoorbeeld door Sandro Pereira, MVP BizTalk, voor het [maps BizTalk Server migreren naar BizTalk Services transformaties](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Integraties
Als u migreren van BizTalk Server orchestration verwerking naar Microsoft Azure wilt, zou de integraties moet opnieuw worden geschreven omdat er actieve BizTalk Server integraties biedt geen ondersteuning voor Microsoft Azure.  U kunt de orchestration-functionaliteit in een Windows Workflow Foundation 4.0 (WF4)-service kan herschrijven.  Dit is een volledig herschreven omdat er momenteel geen migratie van BizTalk Server integraties naar WF4. Hier volgen enkele resources voor Windows-werkstroom:

* [*Het integreren van een werkstroom WCF-Service met Service Bus-wachtrijen en onderwerpen* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) door Paolo Salvatori. 
* [*Het bouwen van apps met Windows Workflow Foundation en Azure* sessie](http://go.microsoft.com/fwlink/p/?LinkId=237314) van de Conferentie Build 2011.
* [*Windows Workflow Foundation Developer Center* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) op MSDN.
* [*Documentatie van Windows Workflow Foundation 4 (WF4)* ](https://msdn.microsoft.com/library/dd489441.aspx) op MSDN.

## <a name="other-considerations"></a>Andere overwegingen
Hieronder volgen enkele overwegingen die u aanbrengen moet tijdens het gebruik van BizTalk Services.

### <a name="fallback-agreements"></a>Alternatieve overeenkomsten
BizTalk Server EDI-bewerking heeft het concept van 'Terugval overeenkomsten'.  BizTalk Services biedt **niet** een concept terugval overeenkomst tot nu toe hebben.  Zie BizTalk documentatie onderwerpen [de rol van de overeenkomsten bij het verwerken van EDI](http://go.microsoft.com/fwlink/p/?LinkId=237317) en [terugval overeenkomst eigenschappen of configureren van algemene](https://msdn.microsoft.com/library/bb245981.aspx) voor meer informatie over hoe terugval overeenkomsten worden gebruikt in BizTalk -Server.

### <a name="routing-to-multiple-destinations"></a>Routering naar meerdere bestemmingen
BizTalk Services bruggen, in de huidige status biedt geen ondersteuning voor routeren van berichten naar meerdere bestemmingen met behulp van een publiceren-abonneren model. In plaats daarvan kunt u berichten van een brug BizTalk Services aan een Service Bus-onderwerp dat meerdere abonnementen het bericht op meer dan één eindpunt hebben versturen.

## <a name="conclusion"></a>Conclusie
Microsoft Azure BizTalk Services is bijgewerkt op reguliere mijlpalen toevoegen van meer functies en mogelijkheden. Bij elke update graag we ondersteunende verbeterde functionaliteit vergemakkelijkt end-to-end-oplossingen met behulp van BizTalk Services en andere technologieën voor Azure maken.

## <a name="see-also"></a>Zie ook
[Ontwikkelen van toepassingen met Azure Enterprise](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
