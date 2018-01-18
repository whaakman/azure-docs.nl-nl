---
title: 'Azure AD Connect-synchronisatie: inzicht in de architectuur | Microsoft Docs'
description: Dit onderwerp beschrijft de architectuur van Azure AD Connect-synchronisatie en verklaart de termen die worden gebruikt.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 465bcbe9-3bdd-4769-a8ca-f8905abf426d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 6a6150dca80a137e9787d943bfe9abb4224e6fe6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect-synchronisatie: inzicht in de architectuur
Dit onderwerp worden de basisarchitectuur voor Azure AD Connect-synchronisatie. In veel aspecten is vergelijkbaar met de voorafgaande MIIS 2003, ILM 2007 en FIM 2010. Azure AD Connect-synchronisatie is de ontwikkeling van deze technologieën. Als u bekend met een van deze eerdere technologieën bent, wordt de inhoud van dit onderwerp ook vertrouwd aan u zijn. Als u niet bekend met synchronisatie bent, wordt de in dit onderwerp voor u. Het is echter niet vereist voor de details kennen van dit onderwerp te laten slagen bij het maken van aanpassingen van Azure AD Connect-synchronisatie (synchronisatie-engine in dit onderwerp genoemd).

## <a name="architecture"></a>Architectuur
De synchronisatie-engine maakt van een geïntegreerde weergave van objecten die zijn opgeslagen in meerdere verbonden gegevensbronnen en gegevens van identiteit in die gegevensbronnen beheert. Deze geïntegreerde weergave wordt bepaald door de gegevens van identiteit opgehaald uit verbonden gegevensbronnen en een reeks regels die bepalen hoe u kunt deze informatie niet verwerken.

### <a name="connected-data-sources-and-connectors"></a>Verbonden gegevensbronnen en -Connectors
De synchronisatie-engine verwerkt identiteitsgegevens van opslagplaatsen voor verschillende gegevens, zoals Active Directory of een SQL Server-database. Elke gegevensopslagplaats die de gegevens in de indeling van een database-achtige ordent en biedt methoden standaard gegevenstoegang is een mogelijke data source kandidaat voor de synchronisatie-engine. De opslagplaatsen voor gegevens die worden gesynchroniseerd door de synchronisatie-engine heten **verbonden gegevensbronnen** of **mappen verbonden** (CD).

De synchronisatie-engine ingekapseld interactie met een verbonden gegevensbron binnen een module met de naam een **Connector**. Elk type van de gekoppelde gegevensbron heeft een specifieke Connector. De Connector kan het een vereiste bewerking in de indeling die de gekoppelde gegevensbron begrijpt.

Connectors maken voor een verbonden gegevensbron API-aanroepen voor het uitwisselen van gegevens van identiteit (zowel lezen en schrijven). Het kan ook een aangepaste Connector met behulp van het framework extensible connectivity toevoegen. De volgende afbeelding ziet hoe een verbonden gegevensbron in een Connector maakt verbinding met de synchronisatie-engine.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Kan gegevensstroom in beide richtingen, maar deze kan niet tegelijkertijd doorlopen in beide richtingen. Met andere woorden, een Connector om te kunnen gegevens van de gekoppelde gegevensbron om de synchronisatie-engine of van synchronisatie-engine op de gekoppelde gegevensbron kan worden geconfigureerd, maar slechts één van deze bewerkingen kan optreden op elk gewenst moment voor een object en kenmerk. De richting kan afwijken voor verschillende objecten en voor verschillende kenmerken.

Als u wilt een Connector configureert, moet u de objecttypen die u wilt synchroniseren opgeven. Geven de objecttypen definieert het bereik van objecten die zijn opgenomen in het synchronisatieproces. De volgende stap is het selecteren van de kenmerken worden gesynchroniseerd, die een kenmerk opnamelijst wordt genoemd. Deze instellingen kunnen elk gewenst moment in reactie op wijzigingen aan uw bedrijfsregels worden gewijzigd. Wanneer u de Azure AD Connect-installatiewizard gebruikt, worden deze instellingen voor u geconfigureerd.

Wanneer objecten wilt exporteren naar een verbonden gegevensbron, moet de opnamelijst kenmerk ten minste de kenmerken die zijn vereist voor het maken van een bepaald objecttype in een verbonden gegevensbron. Bijvoorbeeld, de **sAMAccountName** kenmerk moet worden opgenomen in de lijst met kenmerken opnemen voor het exporteren van een gebruikersobject naar Active Directory omdat alle gebruikersobjecten in Active Directory moeten hebben een **sAMAccountName** kenmerk gedefinieerd. De installatiewizard heeft opnieuw, deze configuratie.

Als de gekoppelde gegevensbron gebruikmaakt van structurele onderdelen, zoals partities of containers te organiseren objecten, kunt u de gebieden in de gekoppelde gegevensbron die worden gebruikt voor een bepaalde oplossing kunt beperken.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interne structuur van de synchronisatie-engine-naamruimte
De volledige synchronisatie-engine-naamruimte bestaat uit twee naamruimten die voor het opslaan van identiteitsgegevens. De twee naamruimten zijn:

* De connectorruimte (CS)
* De metaverse (MV)

De **connectorruimte** is een faseringsgebied dat weergaven van de aangewezen objecten van een verbonden gegevensbron en de kenmerken die zijn opgegeven in de lijst met kenmerken opnemen bevat. De synchronisatie-engine gebruikt het connectorgebied overgebracht om te bepalen wat er in de gekoppelde gegevensbron is gewijzigd en voor het Faseren van binnenkomende wijzigingen. De synchronisatie-engine gebruikt ook het connectorgebied overgebracht om voor te bereiden uitgaande wijzigingen voor het exporteren van de gekoppelde gegevensbron. De synchronisatie-engine heeft een afzonderlijke connectorruimte als een faseringsgebied voor elke Connector.

Met behulp van een faseringsgebied de synchronisatie-engine blijft onafhankelijk van de verbonden gegevensbronnen en wordt niet beïnvloed door de beschikbaarheid en toegankelijkheid. Als gevolg hiervan kunt u gegevens van identiteit op elk gewenst moment verwerken met behulp van de gegevens in het faseringsgebied. De synchronisatie-engine kan alleen de wijzigingen in de gekoppelde gegevensbron sinds de laatste communicatie sessie beëindigd of push om alleen de wijzigingen aan de identiteitsgegevens die de gekoppelde gegevensbron nog niet ontvangen, waardoor het netwerk aanvragen verkeer tussen de synchronisatie-engine en de gekoppelde gegevensbron.

Synchronisatie-engine slaat bovendien statusinformatie over alle objecten die deze in het connectorgebied overgebracht fasen. Wanneer nieuwe gegevens worden ontvangen, evalueert synchronisatie-engine altijd of de gegevens is gesynchroniseerd.

De **metaverse** is een opslagruimte met de samengevoegde identiteitsgegevens uit meerdere verbonden gegevensbronnen, zodat u één mondiale, geïntegreerde weergave van alle gecombineerde objecten. Metaverse-objecten worden gemaakt op basis van de gegevens van de identiteit die is opgehaald uit de verbonden gegevensbronnen en een reeks regels waarmee u het synchronisatieproces aanpassen.

De volgende afbeelding ziet u de connector ruimte naamruimte en de naamruimte metaverse binnen de synchronisatie-engine.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Synchronisatie-engine identity-objecten
De objecten in de synchronisatie-engine zijn weergaven van de objecten in de gekoppelde gegevensbron, of de geïntegreerde weergave die engine voor het synchroniseren van die objecten. Elk synchronisatie-engine-object moet een globally unique identifier (GUID) hebben. GUID's bieden de integriteit van gegevens en snelle relaties tussen objecten.

### <a name="connector-space-objects"></a>Connector ruimteobjecten
Wanneer synchronisatie-engine met een verbonden gegevensbron communiceert, leest de gegevens van identiteit in de gekoppelde gegevensbron en die gegevens worden gebruikt voor het maken van een weergave van het object identiteit in het connectorgebied overgebracht. U kunt maken of deze objecten afzonderlijk verwijderen. U kunt alle objecten in een connectorruimte echter handmatig verwijderen.

Alle objecten in het connectorgebied overgebracht hebben twee kenmerken:

* Een globally unique identifier (GUID)
* Een DN-naam (Distinguished Name)

Als de gekoppelde gegevensbron wijst een uniek kenmerk toe aan het object, kunnen klikt u vervolgens objecten in de connectorruimte ook hebben geen ankerkenmerk. Het ankerkenmerk identificatie unieke van een object in de gekoppelde gegevensbron. De synchronisatie-engine maakt gebruik van het anker naar de bijbehorende weergave van dit object niet vinden in de gekoppelde gegevensbron. Synchronisatie-engine wordt ervan uitgegaan dat het anker van een object nooit gedurende de levensduur van het object verandert.

Veel van de verbindingslijnen een bekende unieke id gebruiken voor het genereren van een anker automatisch voor elk object wanneer deze wordt geïmporteerd. De Active Directory-Connector gebruikt bijvoorbeeld de **objectGUID** kenmerk voor een anker. U kunt voor verbonden gegevensbronnen die niet in een duidelijk gedefinieerde unieke id voorzien, anker generatie opgeven als onderdeel van de Connector-configuratie.

In dat geval het anker wordt samengesteld uit een of meer unieke kenmerken van een object, noch van welke wijzigingen, en de unieke identificeert het object in de connectorruimte (bijvoorbeeld een nummer of een gebruikers-ID).

Een connector space-object kan een van de volgende zijn:

* Een gefaseerde installatie-object
* Een tijdelijke aanduiding

### <a name="staging-objects"></a>Tijdelijke objecten
Een gefaseerde installatie object vertegenwoordigt een exemplaar van de aangewezen objecttypen van de gekoppelde gegevensbron. Naast de GUID en de DN-naam heeft een gefaseerde installatie object altijd een waarde die welk objecttype aangeeft.

Tijdelijke objecten die zijn geïmporteerd altijd hebben een waarde voor het ankerkenmerk. Staging-objecten die nieuw zijn ingericht met synchronisatie-engine en zijn bezig met wordt gemaakt in de gekoppelde gegevensbron hebben niet een waarde voor het ankerkenmerk.

Tijdelijke objecten ook uitvoeren huidige waarden van de kenmerken van bedrijven en operationele informatie die nodig is voor synchronisatie-engine voor het uitvoeren van het synchronisatieproces. Operationele informatie bevat vlaggen die aangeven welke typen updates die zijn voorbereid voor de staging-object. Als een staging-object heeft ontvangen van nieuwe identiteitsgegevens van de gekoppelde gegevensbron die nog niet zijn verwerkt, het object is gemarkeerd als **in behandeling zijnde importeren**. Als een gefaseerde installatie object nieuwe identiteitsgegevens die nog niet is geëxporteerd naar de gekoppelde gegevensbron heeft, is gemarkeerd als **in behandeling zijnde uitvoer**.

Een gefaseerde installatie object kan ook een importeren of exporteren object. De synchronisatie-engine maakt een object importeren met behulp van objectgegevens ontvangen van de gekoppelde gegevensbron. Wanneer synchronisatie-engine informatie over het bestaan van een nieuw object die met de objecttypen die in de Connector is geselecteerd ontvangt overeenkomt, wordt een object importeren in het connectorgebied overgebracht als een representatie van het object in de gekoppelde gegevensbron.

De volgende afbeelding ziet een importobject dat een object in de gekoppelde gegevensbron vertegenwoordigt.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

De synchronisatie-engine maakt een object exporteren met behulp van de objectinformatie in de metaverse. Objecten exporteren worden geëxporteerd naar de gekoppelde gegevensbron tijdens de volgende communicatie. Vanuit het perspectief van de synchronisatie-engine, export objecten bestaan niet in de gekoppelde gegevensbron nog. Het ankerkenmerk voor een object exporteren is daarom niet beschikbaar. Nadat het het object van synchronisatie-engine ontvangt, maakt de gekoppelde gegevensbron een unieke waarde voor het ankerkenmerk van het object.

De volgende afbeelding ziet hoe een export-object is gemaakt met behulp van de gegevens van identiteit in de metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

De synchronisatie-engine wordt bevestigd dat het exporteren van het object door het object van de gekoppelde gegevensbron opnieuw importeren. Objecten exporteren worden objecten importeren wanneer synchronisatie-engine ze tijdens de volgende importeren uit die verbonden gegevensbron ontvangt.

### <a name="placeholders"></a>Tijdelijke aanduidingen
De synchronisatie-engine gebruikt een platte naamruimte voor het opslaan van objecten. Sommige verbonden gegevensbronnen zoals Active Directory gebruiken, een hiërarchische naamruimte. Voor het transformeren van gegevens uit een hiërarchische naamruimte in een platte naamruimte, gebruikt synchronisatie-engine tijdelijke aanduidingen voor het behouden van de hiërarchie.

Elke tijdelijke aanduiding vertegenwoordigt een onderdeel (bijvoorbeeld een organisatie-eenheid) van een hiërarchische objectnaam die niet zijn geïmporteerd in synchronisatie-engine, maar is vereist om de naam van de hiërarchische samen te stellen. Gemaakt door verwijzingen naar objecten die niet zijn staging-objecten in het connectorgebied overgebracht in de gekoppelde gegevensbron hiaten in te vullen.

De synchronisatie-engine gebruikt ook de tijdelijke aanduidingen voor het opslaan van objecten waarnaar wordt verwezen die nog niet is geïmporteerd. Bijvoorbeeld, als de synchronisatie is zodanig geconfigureerd dat deze het kenmerk manager voor de *Abbie Spencer* object en de ontvangen waarde is een object dat niet is geïmporteerd, zoals *CN Lee Sperry, CN = Users, DC = = fabrikam, DC = com* , de manager informatie wordt opgeslagen als tijdelijke aanduidingen in het connectorgebied overgebracht. Als het object manager later wordt geïmporteerd, wordt de tijdelijke aanduiding voor object overschreven door de staging-object dat de manager vertegenwoordigt.

### <a name="metaverse-objects"></a>Metaverse-objecten
Metaverse-object bevat de samengevoegde weergave die synchronisatie-engine heeft van de staging-objecten in het connectorgebied overgebracht. Synchronisatie-engine maakt metaverse-objecten met behulp van de informatie in objecten importeren. Verschillende connector ruimteobjecten kunnen worden gekoppeld aan een enkele metaverse-object, maar een connector space-object kan niet worden gekoppeld aan meer dan één metaverse-object.

Metaverse-objecten kunnen niet handmatig worden gemaakt of verwijderd. De synchronisatie-engine wordt automatisch verwijderd metaverse-objecten die een koppeling naar een object van de ruimte connector geen in het connectorgebied overgebracht hebben.

Synchronisatie-engine biedt om objecten in een verbonden gegevensbron toewijzen aan een corresponderende objecttype binnen de metaverse, een uitgebreid schema met een vooraf gedefinieerde set objecttypen en de bijbehorende kenmerken. U kunt nieuwe objecttypen en kenmerken voor metaverse-objecten maken. Kenmerken kunnen worden één waarde of meerdere waarden en de kenmerktypen kunnen tekenreeksen, verwijzingen, cijfers en Boole-waarden zijn.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Relaties tussen fasering en metaverse-objecten
In de naamruimte van synchronisatie-engine, wordt de gegevensstroom ingeschakeld door de relatie van de koppeling tussen fasering en metaverse-objecten. Een gefaseerde installatie-object dat is gekoppeld aan een metaverse-object heet een **gekoppelde object** (of **connectorobject**). Een gefaseerde installatie-object dat niet is gekoppeld aan een metaverse-object heet een **object niet langer lid** (of **disconnector object**). De voorwaarden die lid is en niet langer lid zijn van voorkeur niet gehaald met de Connectors die verantwoordelijk is voor het importeren en exporteren van gegevens vanuit een verbonden map.

Tijdelijke aanduidingen zijn nooit gekoppeld aan een metaverse-object

Een gekoppelde object bestaat uit een gefaseerde installatie object en de gekoppelde relatie met een enkele metaverse-object. Gekoppelde objecten worden gebruikt voor het synchroniseren van kenmerkwaarden tussen een object van de ruimte connector en metaverse-object.

Wanneer een gefaseerde installatie object verandert in een gekoppelde object tijdens de synchronisatie, kunnen kenmerken stromen tussen de staging-object en het metaverse-object. Kenmerkstroom bidirectionele en is geconfigureerd met behulp van kenmerk importeren en exporteren kenmerk regels.

Een object van de ruimte één connector kan worden gekoppeld aan slechts één metaverse-object. Elke metaverse-object kan echter worden gekoppeld aan meerdere connector ruimteobjecten in dezelfde of verschillende connectorspaces, zoals wordt weergegeven in de volgende afbeelding.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

De gekoppelde relatie tussen de staging-object en metaverse-object is permanent en kan alleen door regels die u opgeeft worden verwijderd.

Een object niet langer lid is een test-object dat niet is gekoppeld aan een metaverse-object. Het kenmerk waarden van een object niet langer lid niet zijn verwerkt alle verdere binnen de metaverse. De waarden van het kenmerk van het bijbehorende object in de gekoppelde gegevensbron wordt niet bijgewerkt met de synchronisatie-engine.

U kunt met behulp van afzonderlijke objecten opslaan van gegevens van identiteit in synchronisatie-engine en later verwerken. Een gefaseerde installatie object behouden als een afzonderlijke object in het connectorgebied overgebracht heeft veel voordelen. Omdat de vereiste informatie over dit object is al door het systeem worden opgezet, is het niet nodig voor het maken van een weergave van dit object opnieuw tijdens de volgende import van de gekoppelde gegevensbron. Op deze manier synchronisatie-engine heeft altijd een volledige momentopname van de gekoppelde gegevensbron, zelfs als er momenteel geen verbinding met de gekoppelde gegevensbron. Afzonderlijke objecten kunnen worden geconverteerd naar de gekoppelde objecten, en vice versa, afhankelijk van de regels die u opgeeft.

Een object importeren wordt als een object niet langer lid gemaakt. Een export-object moet een gekoppelde object. De systeemlogica met deze regel wordt afgedwongen en verwijdert u elke export-object dat is geen gekoppelde object.

## <a name="sync-engine-identity-management-process"></a>Synchronisatieproces engine identity management
Het proces van identiteit bepaalt hoe de gegevens van identiteit tussen verschillende verbonden gegevensbronnen wordt bijgewerkt. Identiteitsbeheer gebeurt in drie processen:

* Importeren
* Synchronisatie
* Exporteren

Tijdens het importproces evalueert synchronisatie-engine de binnenkomende gegevens van de identiteit van een verbonden gegevensbron. Wanneer wijzigingen worden gedetecteerd, deze nieuwe tijdelijke objecten maakt of bestaande staging objecten in de connectorruimte voor synchronisatie van updates.

Tijdens het synchronisatieproces synchronisatie-engine de metaverse zodat de wijzigingen die zijn opgetreden in de connectorruimte-updates en updates van het connectorgebied overgebracht naar aanleiding van wijzigingen die zijn opgetreden in de metaverse.

Tijdens het exportproces duwt synchronisatie-engine om wijzigingen die zijn voorbereid op tijdelijke objecten en die zijn gemarkeerd als in behandeling zijnde uitvoer.

De volgende afbeelding ziet u waar elk van de processen vindt plaats als identiteit informatiestromen van een verbonden gegevensbron naar een andere.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importeren
Tijdens het importproces evalueert synchronisatie-engine updates voor gegevens van identiteit. Synchronisatie-engine vergelijkt de identiteitsgegevens van de gekoppelde gegevensbron met de id-informatie over een gefaseerde installatie object ontvangen en Hiermee bepaalt u of updates voor de staging-object is vereist. Als het bijwerken van de staging-object met nieuwe gegevens noodzakelijk is, wordt de staging-object is gemarkeerd als in behandeling zijnde importeren.

Door objecten in het connectorgebied overgebracht voordat de synchronisatie voor fasering, kan synchronisatie-engine verwerken van alleen de gegevens van identiteit die is gewijzigd. Deze procedure biedt de volgende voordelen:

* **Efficiënte synchronisatie**. De hoeveelheid gegevens verwerkt tijdens de synchronisatie wordt geminimaliseerd.
* **Efficiënte hersynchronisatie**. U kunt wijzigen hoe de synchronisatie-engine identiteitsgegevens zonder opnieuw verbinding te maken met de gegevensbron in de synchronisatie-engine verwerkt.
* **Mogelijkheid om een voorbeeld van de synchronisatie te**. U kunt een voorbeeld van synchronisatie om te controleren of uw veronderstellingen over het beheerproces identiteit juist zijn.

Voor elk object dat is opgegeven in de Connector, probeert de synchronisatie-engine eerst naar een weergave van het object niet vinden in het connectorgebied overgebracht van de Connector. Synchronisatie-engine alle tijdelijke objecten in de connectorruimte moet worden gecontroleerd en wordt geprobeerd een bijbehorende test-object met een overeenkomende ankerkenmerk niet vinden. Als er geen bestaande staging-object een overeenkomende ankerkenmerk heeft, wordt de synchronisatie-engine gezocht naar een overeenkomende staging-object met dezelfde DN-naam.

Wanneer synchronisatie-engine vindt een test-object dat overeenkomt met de door de DN-naam, maar niet door anker, gebeurt het volgende speciale:

* Als het object zich in de connectorruimte geen anker heeft, wordt synchronisatie-engine wordt dit object verwijderd uit het connectorgebied overgebracht en markeert de metaverse-object is gekoppeld aan een als **opnieuw inrichten op de volgende synchronisatie uitvoeren**. Vervolgens wordt de nieuwe importobject gemaakt.
* Als het object zich in de connectorruimte met een anker heeft, klikt u vervolgens synchronisatie-engine wordt ervan uitgegaan dat dit object is gewijzigd of verwijderd uit de directory verbonden. Een tijdelijke, nieuwe DN-naam voor het object van de ruimte connector wilt toewijzen zodat deze het binnenkomende object kunt voorbereiden. Er wordt vervolgens het oude object **tijdelijke**, wachten tot de Connector voor het importeren van de hernoemen of verwijderen van de situatie oplossen.

Als de synchronisatie-engine wordt gezocht naar een staging-object dat overeenkomt met het opgegeven object in de Connector, bepaalt welke wijzigingen toe te passen. Bijvoorbeeld, synchronisatie-engine kan hernoemen of verwijderen van het object in de gekoppelde gegevensbron of deze kenmerkwaarden van het object kan alleen worden bijgewerkt.

Tijdelijke objecten met de bijgewerkte gegevens zijn gemarkeerd als in behandeling zijnde importeren. Er zijn verschillende typen wacht op invoer beschikbaar. Afhankelijk van het resultaat van het importproces heeft een gefaseerde installatie-object in het connectorgebied overgebracht een van de volgende opties in behandeling zijnde importeren typen:

* **Geen**. Er zijn geen wijzigingen aan een van de kenmerken van de staging-object beschikbaar. Synchronisatie-engine vlag dit type niet als in behandeling zijnde importeren.
* **Voeg**. De staging-object is een nieuw object importeren in het connectorgebied overgebracht. Dit type vlaggen synchronisatie-engine als in behandeling zijnde importeren voor verdere verwerking in de metaverse.
* **Update**. Synchronisatie-engine in het connectorgebied overgebracht gevonden met een bijbehorende test-object en het markeren van dit type als in behandeling zijnde importeren, zodat de updates voor de kenmerken kunnen worden verwerkt in de metaverse. Updates bevatten object wijzigen.
* **Verwijder**. Synchronisatie-engine in het connectorgebied overgebracht gevonden met een bijbehorende test-object en het markeren van dit type als in behandeling zijnde importeren, zodat het gekoppelde object kan worden verwijderd.
* **Verwijderen/toevoegen**. Synchronisatie-engine wordt een bijbehorende test-object gevonden in het connectorgebied overgebracht, maar de objecttypen komen niet overeen. In dit geval een verwijderen toevoegen wijziging tijdelijk worden opgeslagen. Een delete-toevoegen wijziging geeft aan dat de synchronisatie-engine dat een volledige hersynchronisatie van dit object omdat verschillende sets van regels op dit object toepassen wanneer de wijzigingen van het objecttype moet plaatsvinden.

Door de status in behandeling zijnde importeren van een gefaseerde installatie-object is ingesteld, is het mogelijk te verminderen aanzienlijk de hoeveelheid gegevens verwerkt tijdens de synchronisatie omdat hierdoor dus kan het systeem verwerkt alleen objecten die gegevens zijn bijgewerkt.

### <a name="synchronization-process"></a>Synchronisatieproces
Synchronisatie bestaat uit twee verwante processen:

* Inkomende synchronisatie, wanneer de inhoud van de metaverse wordt bijgewerkt met behulp van de gegevens in het connectorgebied overgebracht.
* Uitgaande synchronisatie, wanneer de inhoud van het connectorgebied overgebracht wordt bijgewerkt met gegevens in de metaverse.

Met behulp van de informatie in het connectorgebied overgebracht voorbereid maakt tijdens de inkomende synchronisatie in de metaverse de geïntegreerde weergave van de gegevens die zijn opgeslagen in de verbonden gegevensbronnen. Alle tijdelijke objecten of alleen de met een in behandeling zijnde importeren informatie worden samengevoegd, afhankelijk van hoe de regels zijn geconfigureerd.

De proces-updates voor uitgaande synchronisatie exporteren objecten als metaverse objecten wijzigen.

Inkomende synchronisatie maakt de geïntegreerde weergave in de metaverse van de gegevens van de identiteit die is ontvangen van de verbonden gegevensbronnen. Synchronisatie-engine kan op elk gewenst moment de identiteitsgegevens verwerken met behulp van de meest recente gegevens van identiteit die deze uit de gekoppelde gegevensbron.

**Inkomende synchronisatie**

Inkomende synchronisatie omvat de volgende processen:

* **Inrichten** (ook wel **projectie** als het is belangrijk om te onderscheiden van dit proces van het uitgaande synchronisatie inrichting). De synchronisatie-engine maakt een nieuw metaverse-object op basis van een gefaseerde installatie object en deze koppelingen. Inrichten is een bewerking op objectniveau.
* **Deelnemen aan**. De synchronisatie-engine is een staging-object gekoppeld aan een bestaand metaverse-object. Een join is een bewerking op objectniveau.
* **Importeren van de kenmerkstroom**. Synchronisatie-engine-updates de kenmerkwaarden aangeroepen kenmerkstroom van het object in de metaverse. Kenmerkstroom importeren is een kenmerk niveau bewerking uitgevoerd waarvoor een koppeling tussen een gefaseerde installatie object en metaverse-object.

Inrichten is het proces voor alleen die objecten worden gemaakt in de metaverse. Inrichten van invloed op import alleen objecten die niet langer lid objecten. Synchronisatie-engine maakt tijdens het inrichten, een metaverse-object dat overeenkomt met het objecttype van het object importeren en er wordt een koppeling tussen beide objecten, waardoor het maken van een gekoppelde object.

Het join-proces stelt ook een koppeling tussen objecten importeren en metaverse-object. Het verschil tussen join en in te richten is dat de join-proces vereist dat het importobject zijn gekoppeld aan een bestaand metaverse-object, waarbij het inrichten proces maakt een nieuw metaverse-object.

Synchronisatie-engine wordt geprobeerd een object importeren koppelen aan een metaverse-object op basis van criteria die is opgegeven in de configuratie van de Synchronisatieregel.

Synchronisatie-engine bevat een object niet langer lid tijdens het inrichten en join koppelingen naar een metaverse-object, waardoor ze lid zijn. Nadat deze op objectniveau bewerkingen zijn voltooid, kunt de synchronisatie-engine de kenmerkwaarden van de bijbehorende metaverse-object bijwerken. Dit proces heet kenmerkstroom importeren.

Kenmerkstroom importeren wordt uitgevoerd op alle objecten voor importeren die nieuwe gegevens bevatten en zijn gekoppeld aan een metaverse-object.

**Uitgaande synchronisatie**

Uitgaande synchronisatie updates exporteren objecten als een metaverse-object wijzigen, maar wordt niet verwijderd. Het doel van de uitgaande synchronisatie is om te beoordelen of wijzigingen in de metaverse-objecten nodig updates staging objecten in de connector spaties. In sommige gevallen kunnen de wijzigingen die fasering vereisen. objecten in alle connectorspaces worden bijgewerkt. Tijdelijke objecten die zijn gewijzigd, zijn gemarkeerd als in behandeling zijnde uitvoer, waardoor ze objecten exporteren. Deze objecten worden later gepusht naar de gekoppelde gegevensbron tijdens het exportproces exporteren.

Uitgaande synchronisatie heeft drie processen:

* **Inrichten**
* **Opheffen van inrichting**
* **Kenmerkstroom exporteren**

Inrichting en het opheffen van inrichting zijn beide bewerkingen op objectniveau. Opheffen van inrichting, is afhankelijk van de inrichting omdat alleen inrichting kunt starten. Opheffen van inrichting wordt geactiveerd wanneer de koppeling tussen een metaverse-object en een object exporteren inrichting verwijdert.

Inrichting wordt altijd geactiveerd wanneer wijzigingen worden toegepast op objecten in de metaverse. Wanneer wijzigingen worden aangebracht in de metaverse-objecten, kan synchronisatie-engine van de volgende taken uitvoeren als onderdeel van het inrichtingsproces:

* Gekoppelde objecten, waarbij een metaverse-object is gekoppeld aan een nieuwe export-object maken.
* Wijzig de naam van een gekoppelde object.
* Meld de koppelingen tussen een metaverse-object en staging-objecten, het maken van een object niet langer lid.

Als inrichting synchronisatie-engine voor het maken van een nieuw connectorobject is vereist, de fasering object waaraan het metaverse-object is gekoppeld is altijd een object exporteren, omdat het object nog niet in de gekoppelde gegevensbron bestaat.

Als de synchronisatie-engine een gekoppelde-object maken van een object niet langer lid, meld inrichten, vereist voor wordt opheffen van inrichting geactiveerd. Het proces van inrichting verwijdert het object.

Tijdens het opheffen van inrichting verwijdert verwijderen van een object exporteren fysiek niet het object. Het object is gemarkeerd als **verwijderd**, wat betekent dat de bewerking delete is voorbereid op het object.

Kenmerkstroom exporteren vindt ook plaats tijdens het uitgaande synchronisatie wordt op dezelfde manier dat importeren kenmerkstroom tijdens de inkomende synchronisatie plaatsvindt. Kenmerkstroom exporteren doet zich alleen tussen de metaverse en export-objecten die zijn gekoppeld.

### <a name="export-process"></a>Exportproces
Synchronisatie-engine controleert tijdens het exporteren alle export-objecten die zijn gemarkeerd als in behandeling zijnde uitvoer van het connectorgebied overgebracht en stuurt vervolgens updates naar de gekoppelde gegevensbron.

De synchronisatie-engine kunt bepalen het succes van het exporteren van een maar voldoende kan niet bepalen of het proces van identiteit voltooid is. Objecten in de gekoppelde gegevensbron kunnen altijd worden gewijzigd door andere processen. Omdat de synchronisatie-engine heeft geen een permanente verbinding met de gekoppelde gegevensbron, is het niet voldoende om veronderstellingen over de eigenschappen van een object in de gekoppelde gegevensbron op basis van een succesvolle export-melding alleen.

Een proces in de gekoppelde gegevensbron kan bijvoorbeeld de kenmerken van het object wijzigen terug naar de oorspronkelijke waarden (dat wil zeggen, de gekoppelde gegevensbron kan de waarden overschrijven onmiddellijk nadat de gegevens worden gepusht door synchronisatie-engine en toegepast de gekoppelde gegevensbron).

De synchronisatie-engine winkels exporteren en importeren van statusinformatie over elk staging-object. Als de waarden van de kenmerken die zijn opgegeven in de lijst met kenmerken insluiting zijn gewijzigd sinds de laatste export, wordt de opslag van importeren en exporteren van de status Hiermee synchronisatie-engine om te reageren op de juiste wijze. Het importproces synchronisatie-engine gebruikt om te bevestigen dat de waarden van de kenmerken die zijn geëxporteerd naar de gekoppelde gegevensbron. Een vergelijking tussen de geïmporteerde en geëxporteerde gegevens, kunt zoals wordt weergegeven in de volgende afbeelding synchronisatie-engine om te bepalen of het exporteren is geslaagd is of als moet worden herhaald.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Bijvoorbeeld, als de synchronisatie-engine exporteert kenmerk C, dat een waarde van 5, met een verbonden gegevensbron heeft, worden opgeslagen C = 5 in het geheugen van de status exporteren. Elke aanvullende exporteren op dit object resulteert in een poging tot opnieuw C = 5 worden geëxporteerd naar de gekoppelde gegevensbron omdat de synchronisatie-engine wordt ervan uitgegaan dat deze waarde niet permanent op het object toegepast is (dat wil zeggen, tenzij een andere waarde onlangs is geïmporteerd uit de gekoppelde gegevensbron). Het geheugen van de uitvoer wordt gewist wanneer C = 5 tijdens een importbewerking op het object is ontvangen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).

