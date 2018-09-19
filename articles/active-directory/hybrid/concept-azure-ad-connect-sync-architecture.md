---
title: 'Azure AD Connect-synchronisatie: inzicht in de architectuur | Microsoft Docs'
description: Dit onderwerp beschrijft de architectuur van Azure AD Connect-synchronisatie en verklaart de termen die worden gebruikt.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 958ec6b32434bd9e0228255c0edbe9312225586c
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312779"
---
# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect-synchronisatie: inzicht in de architectuur
Dit onderwerp worden de basisarchitectuur voor Azure AD Connect-synchronisatie. In vele opzichten is het vergelijkbaar met de voorafgaande MIIS 2003, ILM 2007 en FIM 2010. Azure AD Connect-synchronisatie is de ontwikkeling van deze technologieën. Als u bekend met een van deze eerdere technologieën bent, worden de inhoud van dit onderwerp ook vertrouwd mee bent. Als u niet bekend bent met synchronisatie, is de in dit onderwerp voor u. Het is echter niet te weten de details van dit onderwerp om te worden voltooid bij het maken van aanpassingen van Azure AD Connect-synchronisatie (synchronisatie-engine in dit onderwerp genoemd).

## <a name="architecture"></a>Architectuur
De synchronisatie-engine een geïntegreerde weergave van objecten die zijn opgeslagen in meerdere verbonden gegevensbronnen maakt en beheert identiteitsgegevens in deze gegevensbronnen. Deze geïntegreerde weergave wordt bepaald door de gegevens van identiteit opgehaald uit de verbonden gegevensbronnen en een set regels die bepalen hoe u deze informatie niet verwerken.

### <a name="connected-data-sources-and-connectors"></a>Verbonden gegevensbronnen en Connectors
De synchronisatie-engine verwerkt identiteitsgegevens uit verschillende gegevensopslagplaatsen, zoals Active Directory of een SQL Server-database. Elke gegevensopslagplaats die de gegevens in een indeling databaseachtige organiseert en waarmee de standaard gegevenstoegang methoden is een mogelijke data source kandidaat voor de synchronisatie-engine. De gegevensopslagplaatsen die worden gesynchroniseerd door de synchronisatie-engine heten **verbonden gegevensbronnen** of **mappen verbonden** (CD).

De synchronisatie-engine interactie met een verbonden gegevensbron in een module met de naam bevat een **Connector**. Elk type verbonden gegevensbron heeft een specifieke Connector. De Connector wordt omgezet in een vereiste bewerking in de indeling die werkt met de verbonden gegevensbron.

Connectors API-aanroepen voor het uitwisselen van gegevens van identiteit (lezen en schrijven) met een verbonden gegevensbron. Het is ook mogelijk om toe te voegen een aangepaste Connector met behulp van de uitbreidbare connectiviteit-framework. De volgende afbeelding ziet u hoe een verbonden gegevensbron in een Connector verbinding maakt met de synchronisatie-engine.

![Arch1](./media/concept-azure-ad-connect-sync-architecture/arch1.png)

Gegevens in beide richtingen kunnen stromen, maar deze kan niet tegelijkertijd flow in beide richtingen. Met andere woorden, een Connector kan worden geconfigureerd als u wilt toestaan dat gegevens op die moeten stromen van de gekoppelde gegevensbron om de synchronisatie-engine of van synchronisatie-engine met de verbonden gegevensbron, maar slechts één van deze bewerkingen kan plaatsvinden op elk gewenst moment voor een object en kenmerk. De richting kan afwijken voor verschillende objecten en andere kenmerken.

Als u wilt een Connector configureert, moet u de objecttypen die u wilt synchroniseren opgeven. Het bereik van objecten die zijn opgenomen in het proces van synchronisatie op te geven de objecttypen die worden gedefinieerd. De volgende stap is om te selecteren van de kenmerken worden gesynchroniseerd, die staat bekend als een lijst met opgenomen. Deze instellingen kunnen elk gewenst moment in reactie op wijzigingen aan uw bedrijfsregels worden gewijzigd. Wanneer u de Azure AD Connect-installatiewizard gebruikt, worden deze instellingen zijn geconfigureerd voor u.

Als u wilt exporteren objecten met een verbonden gegevensbron, moet de lijst met opgenomen ten minste de kenmerken die vereist voor het maken van een bepaald objecttype in een verbonden gegevensbron. Bijvoorbeeld, de **sAMAccountName** kenmerk moet zijn opgenomen in de lijst met opgenomen voor het exporteren van een gebruikersobject in Active Directory omdat alle gebruikersobjecten in Active Directory hebben een **sAMAccountName** kenmerk gedefinieerd. Nogmaals, doet de installatiewizard van deze configuratie voor u.

Als de verbonden gegevensbron structurele onderdelen, zoals partities en containers gebruikt indelen van objecten, kunt u de gebieden in de gekoppelde gegevensbron die worden gebruikt voor een bepaalde oplossing beperken.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interne structuur van de synchronisatie-engine van de naamruimte
De volledige synchronisatie-engine van de naamruimte bestaat uit twee naamruimten waarin de gegevens van identiteit. De twee naamruimten zijn:

* Het connectorgebied (CS)
* De metaverse (MV)

De **connectorgebied** is een faseringsgebied met weergaven van de aangewezen objecten van een verbonden gegevensbron en de kenmerken die zijn opgegeven in de lijst met opgenomen. De synchronisatie-engine maakt gebruik van het connectorgebied overgebracht om te bepalen wat er in de verbonden gegevensbron is gewijzigd en om te zetten van binnenkomende wijzigingen. De synchronisatie-engine gebruikt ook het connectorgebied overgebracht om te zetten uitgaande wijzigingen voor het exporteren naar de verbonden gegevensbron. De synchronisatie-engine heeft een afzonderlijke connectorgebied als een werkgebied voor elke Connector.

Met behulp van een faseringsgebied, wordt de synchronisatie-engine blijft onafhankelijk van de verbonden gegevensbronnen en wordt niet beïnvloed door hun beschikbaarheid en toegankelijkheid. Als gevolg hiervan kunt u gegevens van identiteit op elk gewenst moment verwerken met behulp van de gegevens in de tijdelijke opslag. De synchronisatie-engine kan alleen de wijzigingen die zijn aangebracht in de gekoppelde gegevensbron sinds de laatste communicatie sessie beëindigd of push alleen de wijzigingen in de identiteitsgegevens die door de gekoppelde gegevensbron nog niet ontvangen, waardoor het netwerk aanvragen verkeer tussen de synchronisatie-engine en de gekoppelde gegevensbron.

Synchronisatie-engine slaat bovendien statusinformatie over alle objecten die deze in het connectorgebied fasen. Wanneer er nieuwe gegevens wordt ontvangen, evalueert synchronisatie-engine altijd of de gegevens al is gesynchroniseerd.

De **metaverse** is een opslagruimte met de samengevoegde identiteitsgegevens uit meerdere bronnen met elkaar verbonden gegevens, zodat u één globaal en geïntegreerde weergave van alle gecombineerde objecten. Metaverse-objecten worden gemaakt op basis van de gegevens van de identiteit die is opgehaald uit de verbonden gegevensbronnen en een set regels waarmee u kunt het proces van synchronisatie aanpassen.

De volgende afbeelding ziet u de connector space-naamruimte en de metaverse-naamruimte in de synchronisatie-engine.

![Arch2](./media/concept-azure-ad-connect-sync-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Synchronisatie-engine identiteitsobjecten
De objecten in de synchronisatie-engine zijn representaties van de objecten in de verbonden gegevensbron of de synchronisatie-engine geïntegreerde weergave van die objecten heeft. Elk synchronisatie-engine-object moet een globally unique identifier (GUID) hebben. GUID's bieden de integriteit van gegevens en snelle relaties tussen objecten.

### <a name="connector-space-objects"></a>Connector space objecten
Wanneer synchronisatie-engine met een verbonden gegevensbron communiceert, leest de gegevens van identiteit in de verbonden gegevensbron en die gegevens worden gebruikt om te maken van een weergave van het object identiteit in het connectorgebied. U kunt maken of deze objecten afzonderlijk verwijderen. U kunt alle objecten in een connectorgebied echter handmatig verwijderen.

Alle objecten in het connectorgebied hebben twee kenmerken:

* Een globally unique identifier (GUID)
* Een DN-naam (Distinguished Name)

Als de gekoppelde gegevensbron wijst een uniek kenmerk toe aan het object, kunnen klikt u vervolgens objecten in het connectorgebied hebben ook een kenmerk van bronanker. Het kenmerk van bronanker identificatie unieke van een object in de verbonden gegevensbron. De synchronisatie-engine maakt gebruik van het anker naar de bijbehorende weergave van dit object niet vinden in de verbonden gegevensbron. Synchronisatie-engine wordt ervan uitgegaan dat het anker van een object wordt nooit gewijzigd gedurende de levensduur van het object.

Veel van de Connectors een bekende unieke id gebruiken voor het genereren van een anker automatisch voor elk object wanneer deze wordt geïmporteerd. De Active Directory-Connector gebruikt bijvoorbeeld de **objectGUID** kenmerk voor een anker. Voor verbonden gegevensbronnen die geen een duidelijk gedefinieerde unieke id leveren, kunt u anker generatie opgeven als onderdeel van de configuratie van de Connector.

In dat geval het anker wordt samengesteld uit een of meer unieke kenmerken van een object hebt getypt, noch van welke wijzigingen en unieke identificeert het object in het connectorgebied (bijvoorbeeld het nummer van een werknemer of een gebruikers-ID).

Een connector space-object kan een van de volgende zijn:

* Een staging-object
* Een tijdelijke aanduiding

### <a name="staging-objects"></a>Staging-objecten
Een staging-object vertegenwoordigt een instantie van de aangewezen objecttypen die uit de verbonden gegevensbron. Naast de GUID en de DN-naam heeft een staging-object altijd een waarde die aangeeft van het objecttype.

Staging-objecten die zijn geïmporteerd altijd hebben een waarde voor het kenmerk van bronanker. Staging-objecten die onlangs zijn ingericht met synchronisatie-engine en momenteel wordt gemaakt in de verbonden gegevensbron hebben niet een waarde voor het kenmerk van bronanker.

Fasering objecten hebben ook de huidige waarden van kenmerken van de zakelijke en operationele informatie die nodig is door de synchronisatie-engine om uit te voeren van het proces van synchronisatie. Operationele informatie omvat vlaggen die aangeven welke typen updates die zijn voorbereid op het staging-object. Als een staging-object is ontvangen van nieuwe identiteitsgegevens van de gekoppelde gegevensbron die nog niet zijn verwerkt, het object is gemarkeerd als **in behandeling zijnde importeren**. Als een staging-object nieuwe identiteitsgegevens die nog niet zijn geëxporteerd naar de gekoppelde gegevensbron heeft, is gemarkeerd als **exportbewerking**.

Een gefaseerde installatie object kan ook een import of export object. De synchronisatie-engine maakt een object importeren met behulp van de objectgegevens ontvangen van de verbonden gegevensbron. Wanneer synchronisatie-engine informatie over het bestaan van een nieuw object die overeenkomt met een van de objecttypen die in de Connector hebt geselecteerd ontvangt, maakt een object importeren in het connectorgebied als een weergave van het object in de verbonden gegevensbron.

De volgende afbeelding ziet u een importobject dat staat voor een object in de verbonden gegevensbron.

![Arch3](./media/concept-azure-ad-connect-sync-architecture/arch3.png)

De synchronisatie-engine maakt een object exporteren met behulp van gegevens over objecten in de metaverse. Objecten exporteren worden geëxporteerd naar de gekoppelde gegevensbron tijdens de volgende communicatie-sessie. Vanuit het perspectief van de synchronisatie-engine, exporteren objecten bestaan niet in de verbonden gegevensbron nog. Het kenmerk van bronanker voor een object exporteren is daarom niet beschikbaar. Nadat het object van synchronisatie-engine ontvangen, maakt de verbonden gegevensbron een unieke waarde voor het kenmerk van bronanker van het object.

De volgende afbeelding ziet u hoe een export-object wordt gemaakt met behulp van gegevens van identiteit in de metaverse.

![Arch4](./media/concept-azure-ad-connect-sync-architecture/arch4.png)

De synchronisatie-engine wordt bevestigd dat het exporteren van het object door het object uit de verbonden gegevensbron opnieuw importeren. Objecten exporteren worden objecten importeren wanneer synchronisatie-engine ze tijdens de volgende importeren uit verbonden gegevensbron ontvangt.

### <a name="placeholders"></a>Tijdelijke aanduidingen
De synchronisatie-engine maakt gebruik van een platte-naamruimte voor het opslaan van objecten. Sommige verbonden gegevensbronnen zoals Active Directory wordt echter een hiërarchische naamruimte gebruiken. Voor het transformeren van gegevens uit een hiërarchische naamruimte in een platte-naamruimte, synchronisatie-engine tijdelijke aanduidingen gebruikt voor het behouden van de hiërarchie.

De tijdelijke aanduiding vertegenwoordigt een onderdeel (bijvoorbeeld een organisatie-eenheid) van hiërarchische naam van een object dat is niet geïmporteerd in synchronisatie-engine, maar is vereist om de naam van de hiërarchische samen te stellen. Gemaakt door een verwijzing in de verbonden gegevensbron naar objecten die niet zijn staging-objecten in het connectorgebied hiaten in te vullen.

De synchronisatie-engine gebruikt ook de tijdelijke aanduidingen om op te slaan waarnaar wordt verwezen, objecten die nog niet is geïmporteerd. Bijvoorbeeld, als de synchronisatie is geconfigureerd om op te nemen van het kenmerk manager voor de *Abbie Spencer* object en de ontvangen waarde is een object dat niet is geïmporteerd, zoals *CN Lee Sperry, CN = Users, DC = = fabrikam, DC = com* , de manager informatie wordt opgeslagen als tijdelijke aanduidingen in het connectorgebied. Als het object manager later wordt geïmporteerd, wordt de tijdelijke aanduiding voor object overschreven door het staging-object dat de manager vertegenwoordigt.

### <a name="metaverse-objects"></a>Metaverse-objecten
Een metaverse-object bevat de geaggregeerde weergave die synchronisatie-engine heeft de staging-objecten in het connectorgebied. Synchronisatie-engine maakt metaverse-objecten met behulp van de informatie in objecten importeren. Verschillende connector space objecten kunnen worden gekoppeld aan een enkele metaverse-object, maar een connector space-object kan niet worden gekoppeld aan meer dan één metaverse-object.

Metaverse-objecten kunnen niet handmatig worden gemaakt of verwijderd. De synchronisatie-engine wordt automatisch verwijderd metaverse-objecten die nog geen een koppeling naar een connector space-object in het connectorgebied.

Synchronisatie-engine biedt voor objecten in een verbonden gegevensbron worden toegewezen aan een bijbehorende objecttype in de metaverse, een uitbreidbaar schema met een vooraf gedefinieerde set objecttypen en de bijbehorende kenmerken. U kunt nieuwe objecttypen en kenmerken voor metaverse-objecten maken. Kenmerken kunnen worden één waarde of meerdere waarden en de kenmerktypen zo kunnen tekenreeksen, verwijzingen, cijfers en Booleaanse waarden zijn.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relaties tussen fasering en metaverse-objecten
In de naamruimte van synchronisatie-engine, worden de gegevensstroom wordt ingeschakeld door de relatie van de koppeling tussen fasering en metaverse-objecten. Een staging-object dat is gekoppeld aan een metaverse-object heet een **gekoppelde object** (of **connectorobject**). Een staging-object dat niet is gekoppeld aan een metaverse-object heet een **geen lid meer object** (of **disconnector object**). De voorwaarden die lid is en geen lid meer zijn voorkeur niet worden verward met de Connectors die verantwoordelijk is voor het importeren en exporteren van gegevens uit een verbonden adreslijst.

Tijdelijke aanduidingen zijn nooit gekoppeld aan een metaverse-object

Een gekoppelde object bestaat uit een staging-object en de gekoppelde relatie met een één metaverse-object. Gekoppelde objecten worden gebruikt voor het synchroniseren van kenmerkwaarden tussen een object van connector space en metaverse-object.

Wanneer een object is toegevoegd aan een staging-object tijdens de synchronisatie wordt, kunnen kenmerken stromen tussen de staging-object en de metaverse-object. Kenmerkstroom in twee richtingen werkt en is geconfigureerd met behulp van regels voor het kenmerk van importeren en exporteren kenmerk.

Een object van de ruimte één connector kan worden gekoppeld aan slechts één metaverse-object. Elke metaverse-object kan echter worden gekoppeld aan meerdere connector space objecten in dezelfde of verschillende connectorspaces, zoals wordt weergegeven in de volgende afbeelding.

![Arch5](./media/concept-azure-ad-connect-sync-architecture/arch5.png)

De gekoppelde relatie tussen de staging-object en een metaverse-object is permanent en kan worden verwijderd door regels die u opgeeft.

Een object niet langer lid is een staging-object dat niet is gekoppeld aan een metaverse-object. Het kenmerk dat de waarden van een object niet langer lid niet zijn verwerkt alle verder in de metaverse. De waarden van het kenmerk van het bijbehorende object in de gekoppelde gegevensbron wordt niet bijgewerkt met de synchronisatie-engine.

Met behulp van afzonderlijke objecten, kunt u het opslaan van identiteitsgegevens in synchronisatie-engine en verwerk deze later opnieuw. Bewaren van een staging-object als een object niet langer lid in het connectorgebied heeft veel voordelen. Omdat de vereiste informatie over dit object is al door het systeem worden opgezet, is het niet nodig om te maken van een weergave van dit object tijdens het volgende importeren uit de verbonden gegevensbron. Op deze manier synchronisatie-engine heeft altijd een volledige momentopname van de gekoppelde gegevensbron, zelfs als er momenteel geen verbinding met de verbonden gegevensbron. Afzonderlijke objecten kunnen worden geconverteerd naar de gekoppelde objecten, en vice versa, afhankelijk van de regels die u opgeeft.

Een object importeren wordt als een object niet langer lid gemaakt. Een export-object moet een gekoppelde object. De systeemlogica met deze regel wordt afgedwongen en verwijdert u elke export-object dat is geen gekoppelde object.

## <a name="sync-engine-identity-management-process"></a>Synchronisatieproces engine identity management
Het proces identiteit bepaalt hoe de gegevens van identiteit tussen verschillende verbonden gegevensbronnen wordt bijgewerkt. Identiteitsbeheer vindt plaats in drie processen:

* Importeren
* Synchronisatie
* Exporteren

Synchronisatie-engine tijdens het importproces wordt geëvalueerd als de inkomende gegevens van de identiteit van een verbonden gegevensbron. Wanneer er wijzigingen worden gedetecteerd, deze nieuwe tijdelijke objecten maakt of bestaande tijdelijke objecten in het connectorgebied voor synchronisatie bijgewerkt.

Tijdens het synchronisatieproces synchronisatie-engine de metaverse als gevolg van wijzigingen die zijn opgetreden in het connectorgebied-updates en updates van het connectorgebied overgebracht als gevolg van wijzigingen die zijn opgetreden in de metaverse.

Tijdens het exportproces pushes synchronisatie-engine wijzigingen die zijn voorbereid op het staging-objecten en die zijn gemarkeerd als in behandeling zijnde uitvoer.

De volgende afbeelding ziet u waar elk van de processen als identiteit informatiestromen vanaf een verbonden gegevensbron naar een andere optreedt.

![Arch6](./media/concept-azure-ad-connect-sync-architecture/arch6.png)

### <a name="import-process"></a>Importeren
Synchronisatie-engine geëvalueerd tijdens het importproces, updates van gegevens van identiteit. Synchronisatie-engine vergelijkt de identiteitsgegevens van de gekoppelde gegevensbron met de id-informatie over een staging-object hebt ontvangen en Hiermee bepaalt u of updates voor de staging-object is vereist. Als het bijwerken van de staging-object met nieuwe gegevens nodig is, wordt de staging-object is gemarkeerd als in behandeling zijnde importeren.

Door de faserings-objecten in het connectorgebied vóór de synchronisatie, kan synchronisatie-engine verwerken van alleen de gegevens van identiteit die is gewijzigd. Deze procedure biedt de volgende voordelen:

* **Efficiënte synchronisatie**. De hoeveelheid gegevens die worden verwerkt tijdens de synchronisatie wordt geminimaliseerd.
* **Efficiënte hersynchronisatie**. U kunt wijzigen hoe synchronisatie-engine gegevens van identiteit zonder opnieuw verbinding maken met de gegevensbron de synchronisatie-engine verwerkt.
* **Mogelijkheid om een voorbeeld van synchronisatie**. U kunt een voorbeeld van synchronisatie om te controleren of uw veronderstellingen over het proces identiteit juist zijn.

Voor elk object dat is opgegeven in de Connector, is de synchronisatie-engine eerst wordt gezocht naar een weergave van het object in het connectorgebied van de Connector. Synchronisatie-engine onderzoekt alle tijdelijke objecten in het connectorgebied en probeert om een bijbehorende staging-object met een overeenkomende ankerkenmerk te vinden. Als er geen bestaande staging-object een overeenkomende ankerkenmerk heeft, wordt synchronisatie-engine probeert om een bijbehorende staging object met de dezelfde DN-naam te vinden.

Wanneer synchronisatie-engine een staging-object dat overeenkomt met de door de DN-naam, maar niet door de anker vindt, gebeurt het volgende speciale:

* Als het object zich bevindt in het connectorgebied geen anker heeft, wordt synchronisatie-engine wordt dit object verwijderd uit het connectorgebied en markeert het metaverse-object is gekoppeld aan een als **opnieuw inrichten op de volgende synchronisatie uitvoeren**. Het maakt vervolgens het nieuwe object importeren.
* Als het object zich bevindt in het connectorgebied een anker heeft, klikt u vervolgens synchronisatie-engine wordt ervan uitgegaan dat dit object is gewijzigd of verwijderd uit de verbonden adreslijst. Een tijdelijke, nieuwe DN-naam voor de connector space-object wilt toewijzen zodat deze het binnenkomende object kunt voorbereiden. Er wordt vervolgens het bestaande object **tijdelijke**, in afwachting van de Connector voor het importeren van de hernoemen of verwijderen om op te lossen de situatie.

Als de synchronisatie-engine zoekt een staging-object dat overeenkomt met het opgegeven object in de Connector, bepaalt wat voor soort wijzigingen om toe te passen. Bijvoorbeeld: synchronisatie-engine kan hernoemen of verwijderen van het object in de gekoppelde gegevensbron of er mogelijk alleen bijgewerkt met kenmerkwaarden van het object.

Staging-objecten met bijgewerkte gegevens zijn gemarkeerd als in behandeling zijnde importeren. Er zijn verschillende typen in afwachting van invoer beschikbaar. Afhankelijk van het resultaat van het importproces heeft een staging-object in het connectorgebied een van de volgende in behandeling zijnde importeren typen:

* **Geen**. Er zijn geen wijzigingen aangebracht in een van de kenmerken van de staging-object beschikbaar. Synchronisatie-engine heeft dit type niet markeren als in behandeling zijnde importeren.
* **Voeg**. De staging-object is een nieuw object importeren in het connectorgebied. Synchronisatie-engine wordt dit type gemarkeerd als in behandeling zijnde importeren voor verdere verwerking in de metaverse.
* **Update**. Synchronisatie-engine zoekt naar een bijbehorende staging-object in het connectorgebied en dit type als in behandeling zijnde importeren worden gemarkeerd zodat updates voor de kenmerken kunnen worden verwerkt in de metaverse. Updates bevatten de naam van object wijzigen.
* **Verwijder**. Synchronisatie-engine zoekt naar een bijbehorende staging-object in het connectorgebied en markeren van dit type als in behandeling zijnde importeren, zodat het gekoppelde object kan worden verwijderd.
* **Verwijderen/toevoegen**. Synchronisatie-engine wordt een bijbehorende staging-object gevonden in het connectorgebied, maar de objecttypen die komen niet overeen. In dit geval een verwijderen toevoegen wijziging tijdelijk worden opgeslagen. Een delete-toevoegen wijziging geeft aan dat de synchronisatie-engine dat er een volledige hersynchronisatie van dit object moet plaatsvinden omdat verschillende sets van regels voor dit object gelden wanneer het objecttype wijzigingen.

Door de status in behandeling zijnde importeren van een staging-object is ingesteld, is de hoeveelheid gegevens die tijdens de synchronisatie is verwerkt, omdat dit dus kan het systeem voor het verwerken van alleen de objecten die u gegevens bijgewerkte hebt worden aanzienlijk beperkt.

### <a name="synchronization-process"></a>Synchronisatieproces
Synchronisatie bestaat uit twee verwante processen:

* Inkomende synchronisatie, wanneer de inhoud van de metaverse wordt bijgewerkt met behulp van de gegevens in het connectorgebied.
* Uitgaande synchronisatie, wanneer de inhoud van het connectorgebied wordt bijgewerkt met behulp van gegevens in de metaverse.

Met behulp van de gegevens klaargezet in het connectorgebied, het proces van inkomende synchronisatie wordt gemaakt in de metaverse de geïntegreerde weergave van de gegevens die zijn opgeslagen in de verbonden gegevensbronnen. Alle tijdelijke objecten zijn of alleen gebruikers met een in behandeling zijnde importeren gegevens worden samengevoegd, afhankelijk van hoe de regels zijn geconfigureerd.

De proces-updates voor uitgaande synchronisatie exporteren objecten als metaverse objecten wijzigen.

Inkomende synchronisatie maakt de geïntegreerde weergave in de metaverse van de gegevens van de identiteit die wordt ontvangen van de verbonden gegevensbronnen. Synchronisatie-engine kunt identiteitsgegevens op elk gewenst moment verwerken met behulp van de meest recente gegevens van identiteit die het uit de verbonden gegevensbron.

**Inkomende synchronisatie**

Inkomende synchronisatie bevat de volgende processen:

* **Inrichten** (ook wel genoemd **projectie** als is het belangrijk om te onderscheiden van dit proces van inrichting van uitgaande synchronisatie). De synchronisatie-engine een nieuw metaverse-object op basis van een staging-object gemaakt en ze worden gekoppeld. Inrichten is een bewerking op objectniveau.
* **Deelnemen aan**. De synchronisatie-engine koppelt een staging-object aan een bestaand metaverse-object. Een join is een bewerking op objectniveau.
* **Importeren van de kenmerkstroom**. Synchronisatie-engine-updates de waarden van het kenmerk met de naam kenmerkstroom, van het object in de metaverse. Kenmerkstroom importeren is een kenmerk op serverniveau bewerking waarvoor een koppeling tussen een staging-object en een metaverse-object.

Inrichten is de enige proces dat wordt gemaakt van objecten in de metaverse. Inrichten is van invloed op alleen importeren van objecten die niet langer lid objecten zijn. Synchronisatie-engine maakt tijdens het inrichten, een metaverse-object dat overeenkomt met het objecttype van het object importeren en maakt een koppeling tussen beide objecten, waardoor het maken van een gekoppelde object.

De join-proces wordt ook een koppeling tussen objecten importeren en een metaverse-object. Het verschil tussen join en bepaling is dat de join-proces vereist dat de importobject zijn gekoppeld aan een bestaand metaverse-object, waarbij het inrichten-proces maakt een nieuw metaverse-object.

Synchronisatie-engine probeert lid worden van een object importeren naar een metaverse-object op basis van criteria die is opgegeven in de configuratie van de regel voor synchronisatie.

Tijdens het inrichten en join koppelt synchronisatie-engine een object niet langer lid aan een metaverse-object, waardoor ze lid is. Nadat deze op objectniveau bewerkingen zijn voltooid, kan de synchronisatie-engine de kenmerkwaarden van de bijbehorende metaverse-object bijwerken. Dit proces heet kenmerkstroom importeren.

Kenmerkstroom importeren wordt uitgevoerd op alle objecten voor importeren die nieuwe gegevens bevatten en die zijn gekoppeld aan een metaverse-object.

**Uitgaande synchronisatie**

Updates voor uitgaande synchronisatie exporteren objecten als een metaverse-object wijzigen, maar wordt niet verwijderd. Het doel van uitgaande synchronisatie is om te beoordelen of wijzigingen in objecten metaverse-updates op tijdelijke objecten in de connectorspaces vereist. In sommige gevallen kunnen de wijzigingen die fasering vereisen. objecten in alle connector spaties worden bijgewerkt. Staging-objecten die zijn gewijzigd zijn gemarkeerd als in behandeling zijnde uitvoer, waardoor ze objecten exporteren. Deze objecten worden later doorgegeven aan de gekoppelde gegevensbron tijdens het exportproces exporteren.

Uitgaande synchronisatie heeft drie processen:

* **Inrichten**
* **Opheffen van inrichting**
* **Kenmerkstroom exporteren**

Inrichting en ongedaan maken van inrichting zijn beide bewerkingen op objectniveau. Opheffen van inrichting, is afhankelijk van de inrichting omdat alleen inrichting kunt starten. Opheffen van inrichting wordt geactiveerd wanneer inrichting wordt de koppeling tussen een metaverse-object en een export-object verwijderd.

Inrichting wordt altijd geactiveerd wanneer wijzigingen worden toegepast op objecten in de metaverse. Wanneer wijzigingen worden aangebracht in de metaverse-objecten, kan synchronisatie-engine uitvoeren op elk van de volgende taken uitvoeren als onderdeel van het inrichtingsproces:

* Gekoppelde objecten, waarbij een metaverse-object is gekoppeld aan een nieuwe export-object maken.
* Wijzig de naam van een gekoppelde object.
* Meld de koppelingen tussen een metaverse-object en de staging-objecten, het maken van een object niet langer lid.

Als inrichting, moet synchronisatie-engine een nieuw connectorobject te maken, is de staging-object waaraan de metaverse-object is gekoppeld altijd een object exporteren, omdat het object nog niet in de verbonden gegevensbron bestaat.

Als inrichting is vereist voor synchronisatie-engine naar een gekoppelde object, het maken van een object niet langer lid, meld wordt ongedaan maken van inrichting geactiveerd. Het opheffen van inrichtingen proces wordt het object verwijderd.

Tijdens het opheffen van inrichting verwijdert verwijderen van een object exporteren fysiek niet de object. Het object is gemarkeerd als **verwijderd**, wat betekent dat de verwijderbewerking is voorbereid op het object.

Kenmerkstroom exporteren vindt ook plaats tijdens de uitgaande synchronisatie die vergelijkbaar is met de manier waarop dat kenmerkstroom importeren wordt uitgevoerd tijdens de inkomende synchronisatie. Exportkenmerken doet zich alleen tussen metaverse en exporteren van objecten die zijn gekoppeld.

### <a name="export-process"></a>Exportproces
Tijdens het exportproces onderzoekt synchronisatie-engine alle objecten voor exporteren die zijn gemarkeerd als in behandeling zijnde uitvoer in het connectorgebied en verzendt vervolgens updates naar de verbonden gegevensbron.

De synchronisatie-engine het succes van een export kunt bepalen, maar deze kan niet voldoende bepalen dat het proces identiteit voltooid is. Objecten in de verbonden gegevensbron kunnen altijd worden gewijzigd door andere processen. Synchronisatie-engine heeft geen een permanente verbinding met de verbonden gegevensbron, is het niet voldoende zijn voor veronderstellingen over de eigenschappen van een object in de verbonden gegevensbron alleen op basis van een succesvolle export-melding.

Bijvoorbeeld, een proces in de verbonden gegevensbron kan wijzigen van de kenmerken van het object terug naar de oorspronkelijke waarden (dat wil zeggen, de verbonden gegevensbron kan overschrijven de waarden onmiddellijk nadat de gegevens worden gepusht door synchronisatie-engine en met succes toegepast de gekoppelde gegevensbron).

De synchronisatie-engine winkels exporteren en importeren van statusinformatie over elk staging-object. Als de waarden van de kenmerken die zijn opgegeven in de lijst met opgenomen zijn gewijzigd sinds de laatste export, wordt de opslag van importeren en exporteren van de status Hiermee synchronisatie-engine om te reageren op de juiste wijze. Synchronisatie-engine maakt gebruik van het importproces te bevestigen van kenmerkwaarden die zijn geëxporteerd naar de verbonden gegevensbron. Een vergelijking tussen de geïmporteerde en geëxporteerde gegevens, kunt zoals wordt weergegeven in de volgende afbeelding, u synchronisatie-engine om te bepalen of het exporteren is voltooid of als moet worden herhaald.

![Arch7](./media/concept-azure-ad-connect-sync-architecture/arch7.png)

Bijvoorbeeld, als synchronisatie-engine kenmerk C, dat een waarde van 5, met een verbonden gegevensbron heeft exporteert, slaat deze C = 5 in het geheugen van de status uitvoer. Elke extra uitvoer voor dit object resulteert in een poging tot opnieuw C = 5 worden geëxporteerd naar de gekoppelde gegevensbron omdat de synchronisatie-engine wordt ervan uitgegaan dat deze waarde niet permanent is toegepast op het object (dat wil zeggen, tenzij een andere waarde onlangs is geïmporteerd uit de verbonden gegevensbron). Het geheugen van de export is uitgeschakeld wanneer C = 5 tijdens een importbewerking op het object is ontvangen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).

