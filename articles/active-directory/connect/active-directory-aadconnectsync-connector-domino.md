---
title: Lotus Domino-Connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe Microsoft Lotus Domino-Connector configureren.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/119/2017
ms.author: barclayn
ms.openlocfilehash: 6c412be1c54e0378166791c61469c951bca3a583
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="lotus-domino-connector-technical-reference"></a>Technische naslaginformatie voor Lotus Domino-Connector
In dit artikel beschrijft de Lotus Domino-Connector. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Hotfix 4.1.3671.0 moet gebruiken of later [KB3092178](https://support.microsoft.com/kb/3092178).

Voor MIM2016 en FIM2010R2, de Connector is gedownload van de [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Overzicht van de Lotus Domino-Connector
De Lotus Domino-Connector kunt u de synchronization service integreren met IBM Lotus Domino-server.

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige release van de connector:

| Functie | Ondersteuning |
| --- | --- |
| Gekoppelde gegevensbron |Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Client:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Scenario's |<li>Object levenscyclusbeheer</li><li>Beheer van groepen</li><li>Wachtwoordbeheer</li> |
| Bewerkingen |<li>Volledige en Delta-Import</li><li>Exporteren</li><li>Ingesteld en het wachtwoord op http-wachtwoord wijzigen</li> |
| Schema |<li>Persoon (gebruiker Roaming, neem contact op met (personen met geen enkel certificaat))</li><li>Groep</li><li>Resource (Resource, kamer, Online vergadering)</li><li>Database-e-mail in</li><li>Dynamische detectie van kenmerken voor ondersteunde objecten</li><li>Ondersteuning voor maximaal 250 aangepaste certifiers met een organisatie en organisatie-eenheden (OE)</li> |

Lotus Notes-client de Lotus Domino-connector gebruikt om te communiceren met Lotus Domino-Server. Als gevolg van deze afhankelijkheid moet een ondersteunde Lotus Notes-Client worden geïnstalleerd op de server voor de synchronisatie. De communicatie tussen de client en de server wordt geïmplementeerd via de interface Lotus Notes .NET Interop (Interop.domino.dll). Deze interface vergemakkelijkt de communicatie tussen de Microsoft.NET-platform en Lotus Notes-client en biedt ondersteuning voor toegang voor Lotus Domino-documenten en weergaven. Voor de delta-import is het ook mogelijk dat de native C++-interface (afhankelijk van de methode van de geselecteerde delta-import) wordt gebruikt.

### <a name="prerequisites"></a>Vereisten
Voordat u de Connector gebruikt, zorg er dan voor dat u de volgende vereisten hebben op de server voor de synchronisatie:

* Microsoft.NET 4.5.2 of hoger
* Lotus Notes-client moet worden geïnstalleerd op uw server voor de synchronisatie
* De Lotus Domino-Connector vereist de standaard Lotus Domino LDAP-schema-database (schema.nsf) aanwezig zijn op de server Domino-Directory. Als dit niet aanwezig is, kunt u deze installeren door uitgevoerd of opnieuw starten van de LDAP-service op de Domino-server.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u wilt een van de ondersteunde bewerkingen uitvoeren in Lotus Domino-connector, moet u lid zijn van de volgende groepen:

* Beheerders met volledige toegang
* Beheerders
* Databasebeheerders

De volgende tabel bevat de machtigingen die vereist voor elke bewerking zijn:

| Bewerking | Toegangsrechten |
| --- | --- |
| Importeren |<li>Openbare documenten lezen</li><li> Volledige toegang beheerder (wanneer u lid van de groep administrators volledige toegang bent, automatisch hebt u de effectieve toegang tot in de ACL.)</li> |
| Exporteren en het wachtwoord instellen |Effectieve toegang: <li>Documenten maken</li><li>Documenten verwijderen</li><li>Openbare documenten lezen</li><li>Openbare documenten schrijven</li><li>Repliceren of een kopie van documenten</li>Voor bewerkingen exporteren moet u ook de volgende rollen: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Directe bewerkingen en AdminP
Bewerkingen gaat ofwel u rechtstreeks naar de map Domino of door het proces AdminP. De volgende tabellen lijst ondersteund objecten, bewerkingen en, indien van toepassing, de bijbehorende implementatie-methode:

**Primaire adresboek**

| Object | Maken | Update | Verwijderen |
| --- | --- | --- | --- |
| Person |AdminP |Direct |AdminP |
| Groep |AdminP |Direct |AdminP |
| MailInDB |Direct |Direct |Direct |
| Resource |AdminP |Direct |AdminP |

**Secundaire adresboek**

| Object | Maken | Update | Verwijderen |
| --- | --- | --- | --- |
| Person |N/A |Direct |Direct |
| Groep |Direct |Direct |Direct |
| MailInDB |Direct |Direct |Direct |
| Resource |N/A |N/A |N/A |

Wanneer een resource is gemaakt, wordt een Notes-document gemaakt. Wanneer een resource wordt verwijderd, wordt ook het document opmerkingen verwijderd.

### <a name="ports-and-protocols"></a>Poorten en protocollen
IBM Lotus Notes-client en Domino-servers communiceren met opmerkingen Remote Procedure Call (NRPC) waar NRPC TCP/IP moet gebruiken. Het standaardpoortnummer is 1352, maar kan worden gewijzigd door de beheerder Domino.

### <a name="not-supported"></a>Niet ondersteund
De volgende bewerkingen worden niet ondersteund door de huidige release van de Lotus Domino-connector:

* Postvak verplaatsen tussen servers.

## <a name="create-a-new-connector"></a>Een nieuwe Connector maken
### <a name="client-software-installation-and-configuration"></a>De installatie van clientsoftware en configuratie
Lotus Notes moeten worden geïnstalleerd op de server **voordat** de Connector is geïnstalleerd.

Wanneer u installeert, moet u een **installatie voor één gebruiker**. De standaardwaarde **Multi-User installeren** werkt niet.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Op de pagina onderdelen installeren alleen de vereiste onderdelen Lotus Notes en **één clientaanmelding**. Eenmalige aanmelding is vereist voor de connector kunnen aanmelden bij de Domino-server.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Opmerking:** Start Lotus Notes eenmaal aan een gebruiker die zich op dezelfde server als het account dat u als de connector gebruiken-serviceaccount. Controleer ook of de Lotus Notes-client op de server sluit. Deze kan niet worden uitgevoerd op hetzelfde moment die de Connector probeert verbinding maken met de Domino-server.

### <a name="create-connector"></a>Connector maken
Maken van een Lotus Domino-connector in **synchronisatieservice** Selecteer **beheeragent** en **maken**. Selecteer de **Lotus Domino (Microsoft)** Connector.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Als uw versie van de synchronisatieservice de mogelijkheid biedt voor het configureren van **architectuur**, zorg ervoor dat de connector op de standaardwaarde is ingesteld op uitvoeren in **proces**.

### <a name="connectivity"></a>Connectiviteit
Op de pagina Verbindingsmogelijkheden moet u de Lotus Domino-server opgeven en voer de referenties voor de aanmelding.  
![Connectiviteit](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

De eigenschap Domino-Server ondersteunt twee verschillende indelingen voor naam van de server:

* ServerName
* ServerName/DirectoryName

De **ServerName/DirectoryName** indeling is de gewenste indeling voor dit kenmerk omdat u sneller een antwoord biedt, wanneer de Domino-Server contact legt met de connector.

De opgegeven gebruikers-id-bestand wordt opgeslagen in de configuratiedatabase van de synchronisatieservice.

Voor **Delta-Import** hebt u deze opties:

* **Geen**. De Connector wordt niet een delta-invoer.
* **Toevoegen/bijwerken**. De Connector heeft delta-import toevoegen en bijwerken van bewerkingen. Om te verwijderen, een **volledige Import** bewerking is vereist. Deze bewerking maakt gebruik van de .net-interop.
* **Toevoegen, bijwerken, verwijderen**. De Connector heeft delta-import toevoegen, bijwerken en verwijderen van bewerkingen. Deze bewerking maakt gebruik van de native C++-interfaces.

In **schemaopties** hebt u de volgende opties:

* **Standaard Schema**. De Connector detecteert het schema van de Domino-server. Dit is de standaardoptie.
* **DSML-Schema**. Alleen gebruikt als de server Domino wordt het schema niet beschikbaar. U kunt vervolgens een DSML-bestand maken met het schema en importeer dit in plaats daarvan. Zie voor meer informatie over DSML [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Als u op Volgende klikt, wordt de configuratieparameters gebruikersnaam en wachtwoord zijn geverifieerd.

### <a name="global-parameters"></a>Globale Parameters
Op de pagina Algemene Parameters configureren van de tijdzone en het importeren en exporteren van de optie bij bewerking.  
![Globale Parameters](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

De **Domino servertijdzone** parameter definieert de locatie van uw Domino-Server.

Deze configuratieoptie is vereist voor ondersteuning **delta-import** operations omdat hij Hiermee de synchronisatieservice wijzigingen tussen de laatste twee invoer bepalen.

>[!Note]
Starten van het scherm globale parameters in de update van maart 2017 biedt de mogelijkheid van de gebruiker e-mail database verwijderen bij het verwijderen van de gebruiker.

![Verwijderen van het postvak van gebruiker](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Instellingen importeren, methode
De **volledige Import uitvoeren door** volgende opties:

* Search
* Weergave (aanbevolen)

**Search** is met behulp van indexeren in Domino, maar er is gebruikelijk dat de indexen niet in realtime bijgewerkt worden en de gegevens die wordt geretourneerd vanaf de server niet altijd correct is. Voor een systeem met veel wijzigingen deze optie meestal niet goed werkt en false worden verwijderd in sommige situaties biedt. Echter, **search** sneller dan **weergave**.

**Weergave** optie wordt aanbevolen omdat hiermee de juiste status van gegevens. Er is iets langer dan zoeken.

#### <a name="creation-of-virtual-contact-objects"></a>Maken van virtuele contact op met objecten
De **maken van een \_object contactpersonen** volgende opties:

* None
* Niet-verwijzingsvariant waarden
* Naslaggids voor niet waarden

In Domino bevatten verwijzingskenmerken veel verschillende bestandsindelingen om te verwijzen naar andere objecten. Kunnen andere variaties, de implements Connector \_contact op met objecten, ook wel bekend als **virtuele contactpersonen** (VC). Deze objecten zijn gemaakt, zodat ze aan bestaande MV-objecten toevoegen kunnen of geprojecteerd als nieuwe objecten. Op deze manier kunnen kenmerk verwijzingen worden bewaard.

Door deze instelling inschakelt en als de inhoud van een verwijzingskenmerk niet DN-indeling een \_Contact-object is gemaakt. Een Lidkenmerk van een groep kan bijvoorbeeld de SMTP-adressen bevatten. Het is ook mogelijk dat de korte naam en andere kenmerken in verwijzingskenmerken aanwezig. Selecteer voor dit scenario **niet referentiewaarden**. Deze configuratie is de meest voorkomende instelling voor Domino-implementaties.

Wanneer Lotus Domino is geconfigureerd om afzonderlijke adresboeken met verschillende DN-namen die hetzelfde object vertegenwoordigt, is het mogelijk te maken ook \_Neem contact op met de objecten voor alle verwijzing waarden die zijn gevonden in een adresboek. Selecteer voor dit scenario wordt de **referentie- en Non-referentiewaarden** optie.

Als er meerdere waarden in het kenmerk **FullName** Domino, vervolgens u ook wilt inschakelen, het maken van virtuele contactpersonen zodat verwijzingen omgezet worden kunnen. Dit kenmerk kan bijvoorbeeld meerdere waarden hebben na een huwelijk of echtscheiding. Schakel het selectievakje **inschakelen... Volledige naam heeft meerdere waarden** voor dit scenario.

Door op de juiste kenmerken de \_Neem contact op met objecten zou worden toegevoegd aan het MV-object.

Deze objecten hebben VC =\_contactpersoon toegevoegd aan hun DN-naam.

#### <a name="import-settings-conflict-object"></a>Instellingen importeren, object in conflict
**Conflict-Object uitsluiten**

In een grote Domino-implementatie is het mogelijk dat meerdere objecten de dezelfde DN vanwege problemen met replicatie hebben. In dergelijke gevallen ziet de connector twee objecten met verschillende UniversalIDs maar dezelfde DN-naam. Dit conflict zou leiden tot een tijdelijk object wordt gemaakt in het connectorgebied overgebracht. De Connector kunt de objecten die zijn geselecteerd in Domino als slachtoffer replicatie negeren. De aanbeveling is dit selectievakje geselecteerd houden.

#### <a name="export-settings"></a>Exportinstellingen
Als de optie **AdminP gebruiken voor het bijwerken van verwijzingen** niet is geselecteerd, en vervolgens de uitvoer van de referentie-kenmerken, zoals lid, is een directe aanroep en gebruikt het proces AdminP niet. Gebruik deze optie alleen als AdminP is niet geconfigureerd referentiële integriteit te handhaven.

#### <a name="routing-information"></a>Informatie over routering
In Domino is het mogelijk dat een verwijzingskenmerk routinggegevens ingesloten als achtervoegsel aan de DN-naam heeft. Bijvoorbeeld, het Lidkenmerk in een groep kan bevatten **CN =example/organization@ABC**. Het achtervoegsel @ABC de routinggegevens is. De routinggegevens wordt gebruikt door Domino e-mailberichten verzenden naar de juiste Domino-systeem, die een systeem in een andere organisatie kan worden. U kunt de routering achtervoegsels gebruikt binnen de organisatie in het bereik van de Connector opgeven in het veld gegevens routering. Als een van deze waarden in een verwijzingskenmerk als achtervoegsel wordt gevonden, wordt de routinggegevens van de verwijzing verwijderd. Als het achtervoegsel voor de routering op de waarde van een verwijzing komt niet met een van deze waarden wordt opgegeven overeen, een \_Contact-object is gemaakt. Deze \_Neem contact op met objecten zijn gemaakt met **RO = @<RoutingSuffix>**  ingevoegd in de DN-naam. Voor deze \_Neem contact op met objecten met de volgende kenmerken worden ook toegevoegd aan het toestaan dat u toevoegt aan een echte object indien nodig: \_routingName, \_contactpersoon, \_displayName, en UniversalID.

#### <a name="additional-address-books"></a>Extra adresboeken
Als u nog geen **telefoonboek** geïnstalleerd, waarmee u de naam van de secundaire adresboeken en vervolgens kunt u deze adresboeken handmatig invoeren.

#### <a name="multivalued-transformation"></a>Met meerdere waarden transformatie
Er zijn veel kenmerken in Lotus Domino met meerdere waarden. De bijbehorende metaverse-kenmerken zijn doorgaans één waarden. Het importeren en de bewerking exportoptie configureert, moet u de connector om te helpen bij de vereiste omzetting van de betrokken kenmerken inschakelen.

**Exporteren**  
De bewerking exportoptie ondersteunt twee modi:

* Item toevoegen
* Item vervangen

**Item vervangen** – wanneer u deze optie selecteert, de connector altijd de huidige waarden van het kenmerk in Domino verwijderen en ze vervangen door de opgegeven waarden. De opgegeven waarden mag één waarde of meerdere waarden.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als een nieuwe assistent met de naam **David Alexander** is toegewezen aan dit object persoon het resultaat is:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Item toevoegen** – wanneer u deze optie selecteert, de connector behoudt de bestaande waarden van het kenmerk in Domino en insert nieuwe waarden boven aan de gegevenslijst.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Als een nieuwe assistent met de naam **David Alexander** is toegewezen aan dit object persoon het resultaat is:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importeren**  
De optie importeren bewerking ondersteunt twee modi:

* Standaard
* Enkele waarde met meerdere waarden

**Standaard** – wanneer u de standaardoptie alle waarden van alle kenmerken worden geïmporteerd.

**Enkele waarde met meerdere waarden** – wanneer u deze optie selecteert, een kenmerk met meerdere waarden wordt omgezet in een kenmerk met één waarde. Als meer dan één waarde bestaat, wordt de waarde in de rechterbovenhoek (deze waarde is meestal ook de meest recente) gebruikt.

Voorbeeld: Het kenmerk assistent van een persoon-object heeft de volgende waarden:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

De meest recente update aan dit kenmerk is **David Alexander**. Omdat de optie importeren opnieuw is ingesteld op Multivalued enkele waarde, connector alleen geïmporteerd **David Alexander** naar het connectorgebied overgebracht.

De logica voor kenmerken met meerdere waarden converteren naar kenmerken met één waarde is niet van toepassing voor het kenmerk group lid en het kenmerk van de volledige naam persoon.

Het is ook mogelijk om te importeren configureren en transformatie van regels voor kenmerken met meerdere waarden per kenmerk exporteren als een uitzondering op de algemene regel. Voer voor het configureren van deze optie [objecttype]. [attributename] in de **importeren kenmerk uitsluitingslijst** en **exporteren kenmerk uitsluitingslijst** tekstvakken. Als u Person.Assistant invoeren en de globale vlag is ingesteld voor het importeren van alle waarden, is bijvoorbeeld alleen de eerste waarde van de assistent geïmporteerd.

#### <a name="certifiers"></a>Certifiers
Alle organisatie/organisatie-eenheden worden weergegeven door de connector. Als u persoon om objecten te exporteren naar de primaire adresboek, is een certifier met het wachtwoord vereist.

Als alle certifiers hetzelfde wachtwoord hebben, de **wachtwoord voor alle Certifers** kan worden gebruikt. Vervolgens kunt u hier het wachtwoord invoert en alleen het bestand certifier opgeven.

Als u alleen importeert, vervolgens u beschikt niet over een certifiers opgeven.

### <a name="configure-provisioning-hierarchy"></a>Hiërarchie inrichten configureren
Wanneer u de Lotus Domino-connector configureert, moet u deze pagina dialoogvenster overslaan. De connector voor Lotus Domino ondersteunt geen hiërarchie inrichten.  
![Hiërarchie inrichten](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Wanneer u partities en hiërarchieën configureren, moet u de primaire adresboek NAB=names.nsf aangeroepen. U kunt naast het primaire adresboek secundaire adresboeken selecteren als deze bestaan.  
![Partities](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Kenmerken selecteren
Wanneer u de kenmerken configureert, moet u alle kenmerken die worden voorafgegaan door  **\_MMS\_**. Deze kenmerken zijn vereist wanneer u nieuwe objecten voor Lotus Domino inrichten

![Kenmerken](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Object levenscyclusbeheer
Deze sectie biedt een overzicht van de verschillende objecten in Domino.

### <a name="person-objects"></a>Objecten van de persoon
Het object persoon vertegenwoordigt gebruikers in de organisatie en organisatie-eenheden. Naast de standaardkenmerken, kunt de beheerder Domino aangepaste kenmerken toevoegen aan een persoon-object. Ten minste moet een object persoon alle verplichte kenmerken bevatten. Zie voor een volledige lijst van verplichte kenmerken [Lotus Notes-eigenschappen](#lotus-notes-properties). Voor het registreren van een persoon-object, moeten de volgende vereisten worden voldaan:

* Het adresboek (names.nsf) moet zijn gedefinieerd en deze moet de primaire adresboek.
* U moet de certifier O/organisatie-eenheid Id en het wachtwoord voor het registreren van een bepaalde gebruiker in de organisatie hebben / organisatie-eenheid.
* U moet een specifieke set met Lotus Notes eigenschappen voor een persoon-object instellen. Deze eigenschappen worden gebruikt voor het inrichten van het object persoon. Zie voor meer informatie de sectie [Lotus Notes-eigenschappen](#lotus-notes-properties) verderop in dit document.
* De eerste HTTP-wachtwoord voor een persoon is een kenmerk en een set tijdens het inrichten.
* Het object persoon moet een van de volgende drie ondersteunde typen:
  1. Normaal-gebruiker met een e-mail en een gebruiker-id-bestand
  2. Zwervende gebruiker (een normale gebruiker waarin alle zwervende databasebestanden)
  3. Contactpersonen (gebruiker met geen id-bestand)

Personen (met uitzondering van contactpersonen) kunnen verder worden gegroepeerd in ons gebruikers en internationale gebruikers, zoals gedefinieerd door de waarde van de \_MMS\_IDRegType-eigenschap. Deze mensen gebruik de opmerkingen bij de Client toegang tot Lotus Domino-servers hebben een opmerkingen-Id en een persoon-document. Als ze met opmerkingen bij de e-mail, vervolgens hebben ze ook een e-mailbestand. De gebruiker moet worden geregistreerd voor actief. Zie voor meer informatie:

* [Opmerkingen bij de gebruikers instellen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Gebruikersregistratie](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Het beheren van gebruikers](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Naam van gebruikers](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Alle deze bewerkingen worden uitgevoerd in Lotus Domino en vervolgens geïmporteerd in de synchronisatieservice.

### <a name="resources-and-rooms"></a>Ruimten en resources
Een bron is een ander type van een database in Lotus Domino. Resources kunnen worden vergaderruimten met verschillende soorten apparatuur, zoals projectors. Er zijn subtypen van resources die worden ondersteund door de connector voor Lotus Domino die zijn gedefinieerd door het kenmerk brontype:

| Het type Resource | Kenmerk van het Type resource |
| --- | --- |
| Ruimte |1 |
| Resource (Other) |2 |
| Online vergaderingen |3 |

Het volgende is vereist voor het type Resource-object om te werken:

* Reservering resourcedatabase moet al bestaan in de verbonden Domino-server
* De site is al gedefinieerd voor de Resource

De reservering van de Resource-database bevat drie typen documenten:

* Profiel van uw site
* Resource
* Reservering

Zie voor meer informatie over het instellen van de database van de Resource-reservering [instellen van de database van de Resource-reserveringen](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Maken, bijwerken, en Resources verwijderen**  
De Create, Update en Delete-bewerkingen worden uitgevoerd door de connector voor Lotus Domino in de database van de Resource-reservering. Resources worden gemaakt als documenten in Names.nsf (dat wil zeggen, de primaire adresboek). Zie voor meer informatie over het bewerken en verwijderen van Resources [bewerken en verwijderen van de Resource-documenten](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importeren en exporteren van de bewerking voor bronnen**  
De bronnen worden geïmporteerd en geëxporteerd uit de synchronisatieservice, zoals een ander objecttype. Selecteer het objecttype als Resource tijdens de configuratie. Voor de exportbewerking is geslaagd, moet u de details voor resourcetype, conferentie Database en de naam van de Site hebben.

### <a name="mail-in-databases"></a>E-mail In Databases
Een Database Mail-In is een database die is ontworpen voor het ontvangen van e-mailberichten. Het is een Lotus Domino-postvak is niet gekoppeld aan een specifieke Lotus Domino-gebruikersaccount (dat wil zeggen, het heeft geen eigen bestand ID en wachtwoord). Een e-mail in database heeft een unieke gebruikers-id ('korte naam') die zijn gekoppeld aan het bestand en een eigen e-mailadres.

Als er is een nodig voor een afzonderlijke Postvak met een eigen e-mailadres dat kan worden gedeeld met andere gebruikers (bijvoorbeeld group@contoso.com), een database mail in wordt gemaakt. De toegang tot dit postvak wordt geregeld via de lijst met ACL (Access Control), waarin de namen van de opmerkingen bij de gebruikers die zijn toegestaan om het postvak te openen.

Zie de sectie die is aangeroepen voor een lijst met de vereiste kenmerken, [verplichte kenmerken](#mandatory-attributes) verderop in dit artikel.

Wanneer een database is ontworpen om een e-mail te ontvangen, wordt een e-Mail In Database-document gemaakt in Lotus Domino. Dit document moet bestaan in de Directory Domino van elke server die een kopie van de database opgeslagen. Zie voor een gedetailleerde beschrijving over het maken van een e-mail in databasedocument [maken van een e-Mail In Database-document](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Voordat u een e-Mail In Database maakt, moet de database al bestaan (moet zijn gemaakt door de beheerder Lotus) op de Domino-server.

### <a name="group-management"></a>Beheer van groepen
U kunt een gedetailleerd overzicht van het beheer van de groep Lotus Domino krijgen van de volgende bronnen:

* [Met behulp van groepen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Maken van een groep](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Maken en wijzigen van groepen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Groepen beheren](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Naam van een groep](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Wachtwoordbeheer
Er zijn twee typen van wachtwoorden voor een geregistreerde Lotus Domino-gebruiker:

1. Wachtwoord van gebruiker (opgeslagen in User.id-bestand)
2. Internet / HTTP-wachtwoord

De connector voor Lotus Domino ondersteunt alleen bewerkingen met HTTP-wachtwoord.

Als u wilt uitvoeren wachtwoordbeheer, moet u wachtwoordbeheer voor de connector in de ontwerpfunctie voor Management Agent inschakelen. Als u wachtwoordbeheer selecteert **inschakelen wachtwoordbeheer** op de **uitbreidingen configureren** dialoogvenster pagina.  
![Extensies configureren](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

De connector Lotus Domino ondersteuning bewerkingen op internetwachtwoord te volgen:

* Instellen van het wachtwoord:-Wachtwoord instellen Hiermee stelt u een nieuw wachtwoord voor de HTTP-Internet op de gebruiker in Domino. Standaard is ook het account ontgrendeld. De vlag unlock wordt weergegeven op de WMI-interface van de synchronisatie-Engine.
* Wachtwoord wijzigen: In dit scenario wordt een gebruiker mogelijk wilt wijzigen van het wachtwoord of is gevraagd wachtwoord te wijzigen na een opgegeven periode. Voor deze bewerking te plaatsen, zowel (de oude en het nieuwe wachtwoord) zijn verplicht. Nadat u gewijzigd, wordt het nieuwe wachtwoord in Lotus Domino bijgewerkt.

Zie voor meer informatie:

* [Met behulp van Internet accountvergrendeling](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Wachtwoorden voor het beheer van Internet](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Referentie-informatie
Deze sectie vindt u zoals beschrijvingen van kenmerken en vereisten voor de connector voor Lotus Domino kenmerk.

### <a name="lotus-notes-properties"></a>Lotus Notes-eigenschappen
Wanneer u objecten van de persoon aan uw directory Lotus Domino inricht, moeten uw objecten een specifieke set eigenschappen hebben met specifieke waarden ingevuld. Deze waarden zijn alleen vereist voor bewerkingen maakt.

De volgende tabel geeft een lijst van deze eigenschappen en bevat een beschrijving van deze.

| Eigenschap | Beschrijving |
| --- | --- |
| \_MMS_AltFullName |De alternatieve volledige naam van gebruiker. |
| \_MMS_AltFullNameLanguage |De taal die moet worden gebruikt voor het opgeven van de alternatieve volledige naam van gebruiker. |
| \_MMS_CertDaysToExpire |Het aantal dagen vanaf de huidige datum voordat het certificaat verloopt. Als niet wordt opgegeven, wordt de standaard twee jaar na de huidige datum. |
| \_MMS_Certifier |Eigenschap met de naam van de organisatie-hiërarchie van de certifier. Bijvoorbeeld: OU = OrganizationUnit, O = Org, C = land. |
| \_MMS_IDPath |Als de eigenschap leeg is, wordt er geen gebruiker identificatie-bestand lokaal op de synchronisatieserver gemaakt. Als de eigenschap een bestandsnaam bevat, wordt een gebruikers-ID-bestand gemaakt in de map madata. De eigenschap kan ook een volledig pad bevatten. |
| \_MMS_IDRegType |Personen kunnen worden geclassificeerd als contactpersonen, ons en internationale gebruikers. De volgende tabel bevat de mogelijke waarden: <li>0 - Neem contact op met</li><li>1 - VS-gebruiker</li><li>2 - internationale gebruikers</li> |
| \_MMS_IDStoreType |Vereiste eigenschap voor de Verenigde Staten en internationale gebruikers. De eigenschap bevat een geheel getal-waarde die aangeeft of de gebruikers-id wordt opgeslagen als een bijlage in het adresboek opmerkingen of in e-mailbestand van de persoon. Als de gebruikers-ID-bestand een bijlage in het adresboek is, het eventueel kan worden gemaakt als een bestand met \_MMS_IDPath. <li>Lege - opslag-ID-bestand in de kluis-ID, geen identificatiebestand (gebruikt voor contactpersonen).</li><li> 1 - bijlage in het adresboek opmerkingen. De \_MMS_Password-eigenschap moet worden ingesteld voor gebruikers-id-bestanden die bijlagen</li><li>2 - ID in van de persoon mailbestand opslaan. De \_MMS_UseAdminP moet worden ingesteld op onwaar om het e-mailbestand worden gemaakt tijdens de registratie van de persoon. De \_MMS_Password-eigenschap moet worden ingesteld voor identificatie van gebruikersbestanden.</li> |
| \_MMS_MailQuotaSizeLimit |Het aantal megabytes die zijn toegestaan voor de database van e-bestand. |
| \_MMS_MailQuotaWarningThreshold |Het aantal megabytes die zijn toegestaan voor de e-bestand-database voordat er een waarschuwing weergegeven. |
| \_MMS_MailTemplateName |Het bestand van de e-sjabloon die wordt gebruikt voor het maken van de gebruiker e-mail-bestand. Als een sjabloon wordt opgegeven, wordt het e-mailbestand gemaakt met de opgegeven sjabloon. Als er geen sjabloon is opgegeven, wordt het standaardsjabloonbestand gebruikt het bestand te maken. |
| \_MMS_OU |Optionele eigenschap met de naam van de organisatie-eenheid onder de certifier. Deze eigenschap mag niet leeg zijn voor contactpersonen. |
| \_MMS_Password |Vereiste eigenschap voor gebruikers. De eigenschap bevat het wachtwoord voor de identificatiebestand van het object. |
| \_MMS_UseAdminP |De eigenschap moet worden ingesteld op true als het e-mailbestand moet worden gemaakt door het proces AdminP op de server Domino (asynchrone naar het exportproces start). Als de eigenschap is ingesteld op false, is het e-mailbestand gemaakt met de gebruiker Domino (synchrone tijdens het exportproces). |

Voor een gebruiker met een bijbehorende identificatiebestand, de \_MMS_Password-eigenschap moet een waarde bevatten. Voor toegang tot e-mail via de Lotus Notes-client, moeten de MailServer en MailFile eigenschappen van een gebruiker een waarde bevatten.

Voor toegang tot e-mail via een webbrowser, moeten de volgende eigenschappen waarden bevatten:

* MailFile - vereiste eigenschap met het pad op de Lotus Domino-server waarop het e-mailbestand is opgeslagen.
* MailServer - vereiste eigenschap met de naam van de Lotus Domino-server. Deze waarde is de naam moet worden gebruikt wanneer u het e-mailbestand Lotus op de server Domino gemaakt.
* HTTPPassword - optionele eigenschap die het Web toegang wachtwoord voor het object bevat.

Voor toegang tot de Server Domino zonder mail-toepassing, moet de eigenschap HTTPPassword een waarde bevatten. De eigenschap MailFile en de eigenschap MailServer kunnen niet leeg zijn.

Met \_MMS_ IDStoreType = 2 (store-id in e-mailbestand), de eigenschap MailSystem van NotesRegistrationclass is ingesteld op REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Verplichte kenmerken
De connector voor Lotus Domino ondersteunt hoofdzakelijk de volgende typen objecten (documenttypen):

* Groep
* Database-e-mail In
* Person
* Neem contact op met (iemand die geen certifier)
* Resource

Deze sectie vindt de kenmerken die zijn verplicht voor elk ondersteund object om te exporteren naar een Domino-server.

| Objecttype | Verplichte kenmerken |
| --- | --- |
| Groep |<li>ListName</li> |
| Main In Database |<li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Person |<li>LastName</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Neem contact op met (iemand die geen certifier) |<li>\_MMS_IDRegType</li> |
| Resource |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>Resourcecapaciteit</li><li>Site</li><li>Weergavenaam</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Veelvoorkomende problemen en vragen
### <a name="schema-detection-does-not-work"></a>Schema-detectie werkt niet
Als u het schema te detecteren, is het nodig dat het bestand schema.nsf op de server Domino aanwezig is. Dit bestand wordt alleen weergegeven als LDAP is geïnstalleerd op de server. Als het schema niet gedetecteerd is, Controleer het volgende:

* Het bestand schema.nsf is aanwezig op de hoofdmap van de Server Domino
* De gebruiker is gemachtigd om te zien van het bestand schema.nsf.
* Forceert een herstart van de LDAP-server. Open **Lotus Domino Console** en gebruik **vertellen LDAP ReloadSchema** opdracht het schema opnieuw laden.

### <a name="not-all-secondary-address-books-are-visible"></a>Niet alle secundaire adresboeken zijn zichtbaar.
De Domino-Connector is afhankelijk van de functie **telefoonboek** kunnen vinden van de secundaire adresboeken. Als de secundaire adresboeken ontbreken, controleert u of als [telefoonboek](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) is ingeschakeld en geconfigureerd op de Domino-Server.

### <a name="custom-attributes-in-domino"></a>Aangepaste kenmerken in Domino
Er zijn verschillende manieren in Domino het schema uitbreiden zodat deze wordt weergegeven als een aangepast kenmerk worden gebruikt door de Connector.

**Methode 1: Lotus Domino-schema uitbreiden**

1. Maak een kopie van sjabloon van de Directory Domino {PUBNAMES. NTF} door [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (u moet niet de standaardmap voor IBM Lotus Domino sjabloon aanpassen):
2. Open de kopie van Domino directory sjabloon {CONTOSO. De sjabloon NTF} die is gemaakt in de ontwerpfunctie Domino en als volgt te werk:
   * Klik op gedeelde elementen en vouw subformulieren
   * Dubbelklik op ${ObjectName} InheritableSchema subformulier (waar is {ObjectName} de naam van de standaard structurele object-klasse, bijvoorbeeld: persoon).
   * De naam van het kenmerk dat u wilt toevoegen in het schema {MyPersonAtrribute} en de bijbehorende aan dit kenmerk. Selecteer een veld maken de **maken** Menu en selecteer vervolgens **veld** in het menu van.
   * In het veld toegevoegd eigenschappen ervan te instellen door het Type, stijl, grootte, lettertype en andere gerelateerde parameters voor veld van het venster Eigenschappen te selecteren.
   * De standaardwaarde is dezelfde als de naam opgegeven voor dit kenmerk kenmerk houden (bijvoorbeeld: als kenmerknaam MyPersonAttribute, houd de standaardwaarde met dezelfde naam).
   * Sla het ${ObjectName} InheritableSchema subformulier met de bijgewerkte waarden.
3. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Domino Admin sluit en open Domino-Console de LDAP-service opnieuw starten en het LDAP-Schema opnieuw laden:
   * Invoegen in Domino-Console, de opdracht onder **Domino opdracht** tekst ingediend om opnieuw te starten van de LDAP-service - [starten taak LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * Om opnieuw te laden LDAP schema gebruiken vertellen LDAP-command - LDAP-ReloadSchema vertellen
5. Open Domino Admin en selecteer tabblad voor personen en groepen zien toegevoegde kenmerk wordt weergegeven in domino persoon toevoegen.
6. Open Schema.nsf van **bestanden** tabblad en Zie toegevoegde kenmerk wordt weergegeven in de klasse dominoPerson LDAP-object.

**Methode 2: Een auxClass met het aangepaste kenmerk maken en koppelen aan de object-klasse**

1. Maak een kopie van sjabloon van de Directory Domino {PUBNAMES. NTF} door [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nooit de standaardmap voor IBM Lotus Domino sjabloon aanpassen):
2. Open de kopie van Domino directory sjabloon {CONTOSO. De sjabloon NTF} die is gemaakt in de ontwerpfunctie Domino.
3. Selecteer in het linkerdeelvenster Code gedeeld en subformulieren.
4. Klik op nieuwe subformulier
5. Doe het volgende als u wilt de eigenschappen voor het nieuwe subformulier opgeven:
   * Met het nieuwe subformulier open, ontwerp - subformulier eigenschappen kiezen
   * Voer een naam voor de ondersteunende objectklasse--bijvoorbeeld TestSubform naast de eigenschap Name.
   * Behouden van de eigenschap Options 'Opnemen in subformulier Insert... dialoogvenster' geselecteerd
   * Schakel de opties voor eigenschap 'Render pass through-HTML-code in de notities. "
   * Laat de overige eigenschappen dezelfde en sluit het dialoogvenster subformulier.
   * Opslaan en sluiten van het nieuwe subformulier.
6. Doe het volgende als u wilt toevoegen om te definiëren van de klasse reserve object:
   * Open het subformulier die u hebt gemaakt.
   * Kies maken - veld.
   * Naast de naam op het tabblad basisbeginselen van het dialoogvenster veld, geeft u een naam, bijvoorbeeld: {MyPersonTestAttribute}.
   * In het veld is toegevoegd, stelt u de eigenschappen in selecteren van het Type, stijl, grootte, lettertype en bijbehorende eigenschappen.
   * De standaardwaarde is dezelfde als de naam opgegeven voor dit kenmerk kenmerk houden (bijvoorbeeld: als kenmerknaam MyPersonTestAttribute, houd de standaardwaarde met dezelfde naam).
   * Het subformulier worden opgeslagen met de bijgewerkte waarden en het volgende doen:
     * Selecteer in het linkerdeelvenster Code gedeeld en vervolgens subformulieren
     * Selecteer het nieuwe subformulier en kies ontwerp - ontwerpeigenschappen.
     * Klik op het tabblad derde vanaf de linkerkant en selecteer **doorgegeven deze verbieden van ontwerpwijziging**.
7. Open ${ObjectName} ExtensibleSchema subformulier ({ObjectName} is waar de naam van de standaard structurele objectklasse, bijvoorbeeld – persoon).
8. Resource invoegen, selecteert u het subformulier (die u hebt gemaakt, bijvoorbeeld – TestSubform) en sla het ${ObjectName} ExtensibleSchema subformulier.
9. Vervang de Domino Directory sjabloon {PUBNAMES. NTF} met de nieuwe aangepaste sjabloon {CONTOSO. NTF} door [deze stappen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Domino Admin sluit en open Domino-Console de LDAP-service opnieuw starten en het LDAP-Schema opnieuw laden:
    * Invoegen in Domino-Console, de opdracht onder **Domino opdracht** tekst ingediend om opnieuw te starten van de LDAP-service - [starten taak LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * Opnieuw laden LDAP schema vertellen LDAP-opdracht gebruiken **vertellen LDAP ReloadSchema**.
11. Open Domino-beheerder en selecteer personen en groepen tabblad om te zien toegevoegde kenmerk wordt weergegeven in domino persoon toevoegen (onder andere tab).
12. Open Schema.nsf van **bestanden** tabblad en Zie toegevoegde kenmerk wordt weergegeven onder TestSubform LDAP reserve objectklasse.

**Methode 3: Het aangepaste kenmerk toevoegen aan de klasse ExtensibleObject**

1. Open {Schema.nsf}-bestand geplaatst in de hoofdmap
2. Selecteer LDAP-objectklassen in het menu links onder **alle Schema-documenten** en klik op **toevoegen objectklasse** knop:
3. Geef LDAP-naam in de vorm van {zzzExtensibleSchema} (waarbij zzz de naam van de standaard structurele objectklasse, bijvoorbeeld persoon is). Bijvoorbeeld, als u wilt het schema uitbreidt voor persoon objectklasse, LDAP-naam {PersonExtensibleSchema} opgeven.
4. Geef de klassenaam van het bovenliggende Object waarvoor u wilt het schema uitbreiden. Bijvoorbeeld, als u wilt het schema voor de objectklasse persoon uitbreidt, bieden klassenaam van bovenliggend Object {dominoPerson}:
5. Geef een geldige OID die overeenkomt met de object-klasse.
6. Selecteer uitgebreide/aangepaste kenmerken bij verplichte of optionele kenmerktypen velden aan de hand van de vereiste:
7. Nadat de vereiste kenmerken aan de ExtensibleObjectClass toevoegt, klikt u op **opslaan en sluiten**.
8. Een ExtensibleObjectClass wordt gemaakt voor de objectklasse respectieve standaard met uitgebreide kenmerken.

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector de [hoe ETW-tracering inschakelen voor Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).
