---
title: BizTalk Server EDI-oplossingen migreren naar BizTalk Services | Microsoft Docs
description: Leer hoe u kunt uw Microsoft BizTalk Server EDI-oplossingen migreren naar Microsoft Azure BizTalk Services (MABS)
services: biztalk-services
ms.service: biztalk-services
author: jonfancey
ms.author: jonfan
manager: jeconnoc
ms.topic: article
ms.date: 07/31/2018
ms.reviewer: jonfan, LADocs
ms.suite: integration
ms.openlocfilehash: 38fe0e4901977637be22670d09dccffdfa274304
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253819"
---
# <a name="migrate-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server EDI-oplossingen migreren naar BizTalk Services: technische handleiding

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Auteur: Tim Wieman en Mirchandani Mehrotra

Revisoren: Karthik Bharthy

Met behulp van geschreven: Microsoft Azure BizTalk Services – februari 2014-release.

## <a name="introduction"></a>Inleiding
(EDI, Electronic Data Interchange) is een van de meest voorkomende manier waarop bedrijven exchange de gegevens elektronisch, ook als Business-to-Business- of B2B-transacties worden genoemd. BizTalk Server heeft ondersteuning voor meer dan een decennium sinds de release van de eerste BizTalk Server EDI. Microsoft blijft de ondersteuning voor EDI-oplossingen met BizTalk Services, de op het Microsoft Azure-platform. B2B-transacties worden meestal buiten een organisatie en daarom is het gemakkelijker te implementeren als deze is geïmplementeerd op een cloudplatform. Microsoft Azure biedt deze mogelijkheid tot en met BizTalk Services.

Hoewel sommige klanten naar BizTalk Services als een 'groen veld'-platform voor het nieuwe EDI-oplossingen kijkt, hebben veel klanten huidige BizTalk Server EDI-oplossingen die ze wilt migreren naar Azure. Omdat EDI van BizTalk Services is ontworpen is op basis van de dezelfde voornaamste entiteiten als BizTalk Server EDI-architectuur (trading partners, entiteiten, overeenkomsten), het mogelijk om te migreren van BizTalk Server EDI-artefacten naar BizTalk Services.

Dit document komen enkele van de verschillen die betrokken zijn bij het migreren van BizTalk Server EDI-artefacten naar BizTalk Services. Dit document wordt ervan uitgegaan dat een praktische kennis van BizTalk Server EDI-verwerking en Trading Partner overeenkomsten. Zie voor meer informatie over BizTalk Server EDI [Trading Partner met behulp van BizTalk-beheerserver](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>Welke versie van BizTalk Server EDI-artefacten kan worden gemigreerd naar BizTalk Services?
De BizTalk Server EDI-module is aanzienlijk verbeterd voor BizTalk Server 2010, wanneer deze opnieuw om op te nemen van partners, profielen, en overeenkomsten is gemodelleerd. BizTalk Services maakt gebruik van hetzelfde model om de handelspartners en de business-divisies binnen deze handelspartners te organiseren. Als gevolg hiervan is EDI-artefacten van BizTalk Server 2010 en latere versies naar BizTalk Services migreert, een nog veel meer eenvoudig proces. Als u wilt migreren EDI-artefacten die zijn gekoppeld aan versies voorafgaand aan BizTalk Server 2010, moet u eerst een upgrade naar BizTalk Server 2010 en vervolgens uw EDI-artefacten te migreren naar BizTalk Services.

## <a name="scenariosmessage-flow"></a>Scenario's / stroom
Als met BizTalk Server is EDI-verwerking van BizTalk Services gebaseerd op een oplossing Trading Partner Management (TPM). De TPM-oplossing heeft de volgende belangrijke onderdelen:

* Handelspartners, welke organisatie in een B2B-transacties vertegenwoordigen.
* Profielen die afdelingen binnen een trading partner vertegenwoordigen.
* Trading partner overeenkomsten (of overeenkomsten), die staan voor de zakelijke overeenkomst tussen de twee partners/profielen.

De volgende afbeelding ziet u de overeenkomsten, evenals de verschillen tussen een BizTalk Server EDI-oplossing en de BizTalk Services EDI-oplossing:

![][EDImessageflow]

De belangrijkste verschillen en overeenkomsten tussen een EDI-oplossingsstroom in BizTalk Server en BizTalk Services zijn:

* Net zoals een pijplijn EDIReceive BizTalk Server gebruikt voor het ontvangen van een EDI-bericht en een pijplijn EDISend een EDI-bericht te verzenden, gebruikt de BizTalk Services een brug EDI ontvangen voor het ontvangen en verzenden EDI-brug EDI-berichten verzenden. De pijplijnen zijn gekoppeld aan een overeenkomst met behulp van verzenden of ontvangen van poorten in BizTalk Server. De overeenkomst zelf geeft aan het verzenden of ontvangen van brug in BizTalk Services.
* Nadat de pijplijn EDIReceive de EDI-bericht verwerkt, wordt het bericht in BizTalk Server gedumpt met een SQL Server-database. De pijplijn EdiSend vervolgens neemt het bericht uit de SQL Server-database, verwerkt en verzendt die gegevens vervolgens uit naar de trading partner.
  
    In BizTalk Services, nadat de EDI ontvangen bridge verwerkt de EDI-bericht, wordt het bericht naar een extern proces gerouteerd. De extern proces kan worden uitgevoerd op Microsoft Azure of on-premises. De extern proces moet het bericht routeren naar de EDI verzenden brug; het bericht niet inherent wordt opgehaald door de brug verzenden. Na het verwerken van het bericht, stuurt de brug EDI-verzenden het bericht naar de trading partner.

BizTalk Services biedt een eenvoudig te gebruiken configuratie-ervaring voor het snel maken en implementeren van een B2B-overeenkomst tussen handelspartners zonder een Microsoft Azure Compute-exemplaren (Web- of Worker-rollen), een Microsoft Azure SQL-database of een configuratie Microsoft Azure storage-accounts. Complexere scenario's verbinden in werkstromen of andere service-verwerking is vereist 'rond de randen' van een Trading Partner-overeenkomst, dat wil zeggen, vóór of na de verwerking van Trading Partner Agreement EDI-brug. In de details, wordt de volgende reeks gebeurtenissen optreden tijdens een EDI-berichtverwerking in BizTalk Services.

1. Een EDI-bericht wordt ontvangen van trading partner, Fabrikam.  BizTalk Services biedt voor EDI-berichten ontvangen van handelspartners, ondersteuning van transportprotocollen, zoals FTP-, SFTP-, AS2- en HTTP/S.
2. De trading partner agreement schalen aan de verwerking worden ontleed de EDI-bericht naar XML-indeling.  U kunt het EDI-bericht (in XML-indeling) versturen naar Service Bus-eindpunten, zoals een Service Bus Relay-eindpunt, Service Bus-onderwerp, Service Bus-wachtrij of een brug BizTalk Services.
3. De-XML-berichten kunnen vervolgens worden ontvangen van het eindpunt voor verdere aangepaste verwerking.  Deze eindpunten kunnen bijvoorbeeld worden verwerkt door een on-premises-onderdeel of een Microsoft Azure Compute-instantie verdere verwerking van het bericht in een Windows-werkstroom (WF) of Windows Communication Foundation (WCF)-service.
4. De 'verzendkant verwerking' van de trading partner agreement vervolgens ophaalprotocol van het XML-bericht naar de EDI-indeling en verzendt dit naar trading partner, Contoso.  BizTalk Services biedt voor het EDI-berichten verzenden naar handelspartners, ondersteuning van dezelfde protocollen als die worden gebruikt voor het ontvangen van EDI-berichten.

Dit document verder biedt wel theoretische richtlijnen enkele van de verschillende BizTalk Server EDI-artefacten op migreren naar BizTalk Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Verzenden/ontvangen poorten voor handelspartners
In BizTalk Server u instellen-ontvangstlocaties weergegeven ontvangen en poorten en EDI/XML-berichten ontvangen van handelspartners en EDI/XML-berichten te verzenden naar trading partner instellen van poorten verzenden. U bezighouden deze poorten op een handelspartnerovereenkomst met behulp van de beheerconsole van BizTalk Server vervolgens. In de BizTalk Services, de locaties waar u berichten ontvangt van handelspartners en waar u verzendt berichten naar handelspartners zijn geconfigureerd als onderdeel van de handelspartnerovereenkomst zelf (als onderdeel van de instellingen voor Transport) in de BizTalk Services-Portal.  Zodat u echt geen het concept van 'poorten verzenden' en 'ontvangen locaties', per se in BizTalk Services. Zie voor meer informatie, [overeenkomsten voor het maken van](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pijplijnen (bruggen)
Pijplijnen zijn in BizTalk Server EDI, bericht verwerking entiteiten die u kunnen ook aangepaste logica voor de van de specifieke verwerkingsmogelijkheden, zoals vereist door de toepassing. Voor BizTalk Services zou het equivalent een brug EDI. Maar in BizTalk Services, nu de EDI-bruggen 'gesloten'.  U kunt uw eigen aangepaste activiteiten dat wil zeggen, niet toevoegen aan een bridge EDI. Een aangepaste verwerking moet worden uitgevoerd buiten de EDI-brug in uw toepassing voordat of nadat het bericht de brug geconfigureerd als onderdeel van de Trading Partner agreement krijgt. EAI-bruggen hebt de optie aangepaste verwerken. Als u wilt dat aangepaste verwerking, kunt u EAI-bruggen vóór of na het bericht is verwerkt door de EDI-brug is ingesteld. Zie voor meer informatie, [hoe u aangepaste Code opnemen in bruggen](https://msdn.microsoft.com/library/azure/dn232389.aspx).

U kunt een stroom voor publiceren/abonneren met aangepaste code en/of met behulp van Service Bus messaging-wachtrijen en onderwerpen voordat de handelspartnerovereenkomst ontvangt het bericht of nadat de overeenkomst verwerkt het bericht en doorgestuurd naar een Service Bus-eindpunt kunt invoegen.

Zie **scenario's / berichtenstroom** in dit artikel voor het patroon van de stroom weergegeven.

## <a name="agreements"></a>Overeenkomsten
Als u bekend met de BizTalk Server 2010 handel Partner overeenkomsten voor EDI-verwerking gebruikt bent, zijn BizTalk Services handelspartnerovereenkomsten er waarschijnlijk bekend. De meeste van de instellingen van de overeenkomst zijn hetzelfde, en gebruik de dezelfde terminologie. In sommige gevallen kan de instellingen van de overeenkomst zijn veel eenvoudiger vergeleken met dezelfde instellingen in BizTalk Server. Microsoft Azure BizTalk Services ondersteunt X12 en EDIFACT, AS2-transport.

Microsoft Azure BizTalk Services biedt ook een **TPM gegevensmigratie** hulpprogramma handelspartners en -overeenkomsten van BizTalk Server Trading Partner module migreren naar BizTalk Services-Portal. Het hulpprogramma voor gegevensmigratie TPM is beschikbaar als onderdeel van een pakket hulpprogramma's, die kan worden gedownload vanaf de [MABS SDK](https://go.microsoft.com/fwlink/p/?LinkId=235057). Het pakket bevat ook een Leesmij-bestand met instructies over het gebruik van het hulpprogramma en elementaire informatie over probleemoplossing voor het hulpprogramma.

## <a name="schemas"></a>Schema 's
BizTalk Services biedt EDI-schema's die kunnen worden gebruikt in oplossingen voor BizTalk Services.  BizTalk Server EDI-schema's kunnen bovendien ook worden gebruikt met BizTalk Services omdat het hoofdknooppunt van het EDI-schema hetzelfde voor BizTalk Server, evenals de BizTalk Services is. Zo kunt u zich rechtstreeks uw BizTalk Server EDI-schema's en deze gebruiken in de EDI-oplossingen die u hebt ontwikkeld met behulp van BizTalk Services. U kunt ook downloaden de schema's uit de [MABS SDK](https://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Maps (transformaties)
Kaarten in BizTalk Server heten transformaties in BizTalk Services. Toewijzingen van BizTalk Server migreren naar BizTalk Services wordt mogelijk een van de complexere taken om te realiseren (afhankelijk van de complexiteit van de kaart). De toewijzingsfunctie voor BizTalk Services gebruikt, wijkt af van het toewijzen van de BizTalk. Hoewel het toewijzen van de ziet er meestal hetzelfde, wordt de onderliggende toewijzingsindeling verschilt. De functoids (met de naam **kaart Operations** in BizTalk Services) beschikbaar voor de gebruikers ook verschillend zijn.  U kunt een BizTalk-kaart van kracht, rechtstreeks in BizTalk Services gebruiken. Niet alle functoids die beschikbaar zijn in BizTalk Server zijn ook beschikbaar als kaart bewerkingen in BizTalk Services.

### <a name="new-transform-operations"></a>Nieuwe transformatiebewerkingen
Terwijl de lijst met kaart transformatiebewerkingen beschikbaar heel anders dan het toewijzen van de BizTalk Server lijkt, BizTalk Services transformeert beschikken over nieuwe manieren van dezelfde taken uitvoeren. BizTalk Services transformaties bijvoorbeeld **Opvraagbewerkingen** beschikbaar. Dit is niet beschikbaar in de BizTalk-toewijzing.  De **Opvraagbewerkingen** kunt u maken en uitvoeren op een 'List', waarbij een lijst met een reeks items (ook wel bekend als ' rijen') en waarbij elk item kan meerdere leden (ook wel bekend als ' kolommen') hebben.  U kunt de lijst sorteren, selecteert u items op basis van een voorwaarde, enzovoort.

Een ander voorbeeld van nieuwe functionaliteit van BizTalk Services transformeert zijn de **lus Operations**.  Het is moeilijk om te maken van geneste lussen in het toewijzen van de BizTalk Server.  Zo worden de bewerkingen van de kaart lus toegevoegd voor de BizTalk-Services worden getransformeerd.

Nog een ander voorbeeld is de **als-dan-anders** expressie kaart bewerking.  Een bewerking als-dan-anders doen is mogelijk in het toewijzen van de BizTalk, maar deze meerdere functoids om uit te voeren een ogenschijnlijk eenvoudige taak vereist.

### <a name="migrating-biztalk-server-maps"></a>BizTalk-Server migreren wordt toegewezen
Microsoft Azure BizTalk Services biedt een hulpprogramma voor het migreren van BizTalk Server toegewezen aan de transformaties BizTalk Services. De **BTMMigrationTool** is beschikbaar als onderdeel van de **extra** pakket voorzien van de [BizTalk Services SDK downloaden](https://go.microsoft.com/fwlink/p/?LinkId=235057). Zie voor meer informatie over het hulpprogramma [omzetten in een BizTalk-kaart een transformatie van BizTalk Services](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

U kunt ook een voorbeeld van Sandro Pereira, BizTalk MVP, voor het kijken [maps voor BizTalk Server migreren naar BizTalk Services transformaties](https://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Indelingen
Als u nodig hebt voor het migreren van BizTalk Server-orchestration verwerking naar Microsoft Azure, moet de indelingen worden herschreven omdat Microsoft Azure biedt geen ondersteuning voor actieve BizTalk Server-indelingen.  U kunt de orchestration-functionaliteit in een Windows Workflow Foundation 4.0 (WF4)-service kan herschrijven.  Dit moet een volledig herschreven omdat er momenteel geen migratie van BizTalk Server-indelingen naar WF4. Hier volgen enkele resources voor Windows-werkstroom:

* [*Over het integreren van een werkstroom WCF-Service met Service Bus-wachtrijen en onderwerpen* ](https://blogs.msdn.microsoft.com/paolos/2013/04/09/how-to-integrate-a-wcf-workflow-service-with-service-bus-queues-and-topics/) door Paolo Salvatori. 
* [*Het bouwen van apps met Windows Workflow Foundation en Azure* sessie](https://go.microsoft.com/fwlink/p/?LinkId=237314) van de Conferentie Build 2011.
* [*Windows Workflow Foundation Developer Center* ](https://go.microsoft.com/fwlink/p/?LinkId=237315) op MSDN.
* [*Documentatie van Windows Workflow Foundation 4 (WF4)* ](https://msdn.microsoft.com/library/dd489441.aspx) op MSDN.

## <a name="other-considerations"></a>Andere overwegingen
Hier volgen enkele overwegingen die u aanbrengen moet tijdens het gebruik van BizTalk Services.

### <a name="fallback-agreements"></a>Alternatieve overeenkomsten
BizTalk Server EDI-verwerking heeft het concept van 'Terugval overeenkomsten'.  BizTalk Services biedt **niet** een concept terugval overeenkomst tot nu toe hebt.  Zie BizTalk documentatie onderwerpen [de rol van de overeenkomsten in de EDI-verwerking](https://go.microsoft.com/fwlink/p/?LinkId=237317) en [terugval overeenkomst eigenschappen of configureren van algemene](https://msdn.microsoft.com/library/bb245981.aspx) voor meer informatie over hoe alternatieve overeenkomsten worden gebruikt in BizTalk De server.

### <a name="routing-to-multiple-destinations"></a>Routering naar meerdere bestemmingen
BizTalk Services-bruggen, in de huidige status biedt geen ondersteuning voor routeren van berichten naar meerdere bestemmingen van een publish-subscribe-model. In plaats daarvan kunt u berichten van een brug BizTalk Services naar een Service Bus-onderwerp, die vervolgens uit meerdere abonnementen voor het bericht op meer dan één eindpunt versturen.

## <a name="see-also"></a>Zie ook
[LOB-oplossingen in Azure](https://azure.microsoft.com/solutions/lob-applications)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
