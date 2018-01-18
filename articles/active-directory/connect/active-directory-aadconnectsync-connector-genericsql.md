---
title: Algemene SQL-Connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe algemene SQL-Connector van Microsoft te configureren.
services: active-directory
documentationcenter: 
author: billmath
manager: bhu
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: billmath
ms.openlocfilehash: 66e3559c244a76101be7b7d944a48cd6dd99bd4c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="generic-sql-connector-technical-reference"></a>Algemene technische documentatie van SQL-Connector
Dit artikel worden de algemene SQL-Connector. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Hotfix 4.1.3671.0 moet gebruiken of later [KB3092178](https://support.microsoft.com/kb/3092178).

Voor MIM2016 en FIM2010R2, de Connector is gedownload van de [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Zie voor deze Connector in actie de [algemene SQL-Connector stapsgewijze](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) artikel.

## <a name="overview-of-the-generic-sql-connector"></a>Overzicht van de algemene SQL-Connector
De algemene SQL-Connector kunt u de synchronization service integreren met een databasesysteem dat ODBC-connectiviteit biedt.  

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige release van de connector:

| Functie | Ondersteuning |
| --- | --- |
| Gekoppelde gegevensbron |De Connector wordt ondersteund met alle 64-bits ODBC-stuurprogramma's. Het is getest met de volgende opties: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 &11;g</li><li>MySQL 5.x</li> |
| Scenario's |<li>Object levenscyclusbeheer</li><li>Wachtwoordbeheer</li> |
| Bewerkingen |<li>Volledige Import- en Delta-Import, Export</li><li>Voor het exporteren: Toevoegen, verwijderen, bijwerken en vervangen</li><li>Wachtwoord instellen, wachtwoord wijzigen</li> |
| Schema |<li>Dynamische detectie van objecten en kenmerken</li> |

### <a name="prerequisites"></a>Vereisten
Zorg voordat u de Connector gebruikt, hebt u de volgende op de server voor de synchronisatie:

* Microsoft.NET 4.5.2 of hoger
* 64-bits ODBC-stuurprogramma's client

### <a name="permissions-in-connected-data-source"></a>Machtigingen in de gekoppelde gegevensbron
Als u wilt maken of een van de ondersteunde bewerkingen uitvoeren in algemene SQL-connector, moet u het volgende hebben:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Poorten en protocollen
Voor de vereiste poorten voor de ODBC-stuurprogramma werkt, Raadpleeg de documentatie van de databaseleverancier.

## <a name="create-a-new-connector"></a>Een nieuwe Connector maken
Maken van een algemene SQL-connector in **synchronisatieservice** Selecteer **beheeragent** en **maken**. Selecteer de **algemene SQL (Microsoft)** Connector.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
De Connector gebruikt een ODBC DSN-bestand voor connectiviteit. Maak de DSN-bestand met **ODBC-gegevensbronnen** gevonden in het startmenu onder **Systeembeheer**. In het beheerprogramma, maakt u een **DSN-bestand** zodat deze kan worden opgegeven met de Connector.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Het scherm connectiviteit is de eerste wanneer u een nieuwe algemene SQL-Connector maakt. U moet eerst de volgende informatie:

* Bestandspad DSN-naam
* Verificatie
  * Gebruikersnaam
  * Wachtwoord

De database moet een van deze verificatiemethoden kunnen ondersteunen:

* **Windows-verificatie**: de verifiërende database gebruikt de Windows-referenties om te controleren of de gebruiker. De gebruikersnaam en wachtwoord opgegeven wordt voor verificatie met de database gebruikt. Dit account moet machtigingen voor de database.
* **SQL-verificatie**: de verifiërende database maakt gebruik van de gebruikersnaam en wachtwoord gedefinieerd het scherm verbinding verbinding maken met de database. Als u de naam van gebruiker/wachtwoord in de DSN-bestand opslaat, hebben voorrang op de referenties die zijn opgegeven op het scherm verbinding.
* **Azure SQL Database-verificatie**: Zie voor meer informatie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](../../sql-database/sql-database-aad-authentication.md).

**DN-naam is anker**: als u deze optie selecteert, de DN-naam wordt ook gebruikt als het ankerkenmerk. Kan worden gebruikt voor een eenvoudige implementatie, maar heeft ook de volgende beperking:

* Connector ondersteunt slechts één objecttype. Verwijzingskenmerken kunnen daarom alleen verwijzen naar hetzelfde objecttype.

**Exporttype: Object-vervangen**: tijdens het exporteren wanneer u alleen bepaalde kenmerken zijn gewijzigd, het gehele object met alle kenmerken worden geëxporteerd en vervangt het bestaande object.

### <a name="schema-1-detect-object-types"></a>Schema 1 (objecttypen detecteren)
Op deze pagina gaat u configureren hoe de Connector gaat de verschillende objecttypen in de database niet vinden.

Elk objecttype wordt weergegeven als een partitie en geconfigureerd op verdere **configureren partities en hiërarchieën**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Object-Type detectiemethode**: de Connector deze detectiemethoden voor object-type wordt ondersteund.

* **Vaste waarde**: bieden van de lijst met objecttypen die met een door komma's gescheiden lijst. Bijvoorbeeld: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Weergave-tabel/opgeslagen Procedure**: Geef de naam van de tabel, weergave/opgeslagen procedure en vervolgens de naam van de kolom met de lijst met objecttypen. Als u een opgeslagen procedure gebruikt, ook geeft parameters voor de indeling **[Name]: [richting]: [waarde]**. Geef elke parameter in op een afzonderlijke regel (Gebruik Ctrl + Enter om op te halen van een nieuwe regel).  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **SQL-Query**: deze optie kunt u een SQL-query één kolom met objecttypen, bijvoorbeeld retourneert bieden `SELECT [Column Name] FROM TABLENAME`. De geretourneerde kolom moet van het type tekenreeks (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (kenmerktypen detecteren)
U gaat configureren hoe de kenmerknamen en typen gaat worden gedetecteerd op deze pagina. De configuratieopties worden weergegeven voor elke objecttype gedetecteerd op de vorige pagina.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Het kenmerk Type detectiemethode**: de Connector ondersteunt deze kenmerk type detectiemethoden met elke gedetecteerde objecttype in Schema-1-scherm.

* **Weergave-tabel/opgeslagen Procedure**: Geef de naam van de tabel, weergave/opgeslagen procedure die moet worden gebruikt om de kenmerknamen te vinden. Als u een opgeslagen procedure gebruikt, ook geeft parameters voor de indeling **[Name]: [richting]: [waarde]**. Geef elke parameter in op een afzonderlijke regel (Gebruik Ctrl + Enter om op te halen van een nieuwe regel). Geef een door komma's gescheiden lijst met tabellen of weergaven om te detecteren de kenmerknamen in een kenmerk met meerdere waarden. Met meerdere waarden worden niet ondersteund wanneer de bovenliggende en onderliggende tabel dezelfde kolomnamen hebt.
* **SQL-query**: deze optie kunt u een SQL-query één kolom met de namen van kenmerken, bijvoorbeeld retourneert bieden `SELECT [Column Name] FROM TABLENAME`. De geretourneerde kolom moet van het type tekenreeks (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (definiëren anker en DN-naam)
Deze pagina kunt u anker en DN-kenmerk voor elk gedetecteerd objecttype configureren. U kunt meerdere kenmerken uniek te maken het anker selecteren.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Kenmerken met meerdere waarden en Booleaanse worden niet weergegeven.
* Hetzelfde kenmerk voor DN-naam en anker, niet gebruiken tenzij **DN-naam is anker** is geselecteerd op de pagina Verbindingsmogelijkheden.
* Als **DN-naam is anker** is geselecteerd op de pagina Verbindingsmogelijkheden deze pagina alleen de DN-kenmerk vereist. Dit kenmerk moet ook worden gebruikt als het ankerkenmerk.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (definiëren kenmerktype verwijzing en richting)
Deze pagina kunt u het kenmerktype, zoals geheel getal, binair of Booleaanse waarde en richting voor elk kenmerk configureren. Alle kenmerken van pagina **schema 2** met inbegrip van kenmerken met meerdere waarden worden weergegeven.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **Gegevenstype**: het kenmerktype toewijzen aan deze bestandstypen die bekend is bij de synchronisatie-engine wordt gebruikt. De standaardwaarde is het gebruik van hetzelfde type gevonden in het SQL-schema, maar de datum/tijd- en verwijzingen niet gemakkelijk waarneembaar zijn. Voor gebruikers, moet u opgeven **DateTime** of **verwijzing**.
* **Richting**: U kunt de richting van het kenmerk Import, Export of ImportExport instellen. ImportExport is standaard.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Opmerkingen:

* Als een kenmerktype niet gedetecteerd door de Connector is, wordt het gegevenstype String.
* **Geneste tabellen** kunnen worden beschouwd als één kolom databasetabellen. Oracle slaat de rijen van een geneste tabel in willekeurige volgorde. Bij het ophalen van de geneste tabel in een variabele PL-SQL, worden de rijen echter opeenvolgende subscript beginnen bij 1 gegeven. Die geeft u matrixachtige toegang tot afzonderlijke rijen.
* **VARRYS** worden niet ondersteund in de connector.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (partitie definiëren voor verwijzingskenmerken)
Op deze pagina configureert u voor alle verwijzingskenmerken welke partitie (objecttype) een kenmerk naar verwijst.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Als u **DN-naam is anker**, moet u hetzelfde objecttype gebruiken als het account dat u van verwijst. U kan niet verwijzen naar een ander objecttype.

>[!NOTE]
Vanaf de update van maart 2017 is nu een optie voor ' * ' wanneer deze optie is gekozen en vervolgens alle mogelijke lidtypen moeten worden geïmporteerd.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Vanaf mei 2017 de ' * ' aka **elke optie** ondersteuning voor importeren en exporteren van de stroom is gewijzigd. Als u wilt gebruiken met deze optie hebt de meerdere waarden tabelweergave een kenmerk met het objecttype.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Als ' * ' is geselecteerd wordt de naam van de kolom met het objecttype moet ook worden opgegeven.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Na het importeren ziet u iets soortgelijks als in de onderstaande afbeelding:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globale Parameters
De pagina Algemene Parameters wordt gebruikt voor het configureren van Delta-Import, datum/tijd-indeling en wachtwoordmethode.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



De algemene SQL-Connector ondersteunt de volgende methoden voor Delta-Import:

* **Trigger**: Zie [genereren van Delta-weergaven met Triggers](https://technet.microsoft.com/library/cc708665.aspx).
* **Watermerk**: een algemene benadering die kan worden gebruikt voor elke database. De query watermerk is al ingevuld op basis van de leverancier van de database. Een watermerk-kolom moet aanwezig zijn op elke tabel of weergave die wordt gebruikt. In deze kolom moet voegt bijhouden en updates op de tabellen als en de afhankelijke (met meerdere waarden of onderliggende) tabellen. De klokken tussen synchronisatieservice en de databaseserver moeten worden gesynchroniseerd. Als dat niet het geval is, wordt een aantal invoergegevens in de delta-import kunnen worden weggelaten.  
  Beperking:
  * Watermerk strategie biedt geen verwijderde ondersteuningsobjecten.
* **Momentopname**: (werkt alleen met Microsoft SQL Server) [Delta weergaven met momentopnamen genereren](https://technet.microsoft.com/library/cc720640.aspx)
* **Het bijhouden van**: (werkt alleen met Microsoft SQL Server) [over bijhouden](https://msdn.microsoft.com/library/bb933875.aspx)  
  Beperkingen:
  * Anker & DN-kenmerk moet deel uitmaken van de primaire sleutel voor het geselecteerde object in de tabel.
  * SQL-query wordt niet ondersteund tijdens het importeren en exporteren met bijhouden.

**Extra Parameters**: Geef de tijdzone van de Database Server waarmee wordt aangegeven waar uw Database-server zich bevindt. Deze waarde wordt gebruikt ter ondersteuning van de verschillende indelingen van kenmerken van datum en tijd.

De Connector slaat altijd datum- en datum / tijd in UTC-notatie. Om te kunnen correct converteren van de datum en tijden, de tijdzone van de databaseserver en de gebruikte indeling moet worden opgegeven. De indeling moet worden uitgedrukt in .net-indeling.

Tijdens het exporteren moet de Connector in UTC-tijdnotatie elke datum-tijd-kenmerk worden opgegeven.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Configuratie van wachtwoord**: de connector biedt mogelijkheden voor synchronisatie van wachtwoord en ondersteunt het instellen en wachtwoord te wijzigen.

De Connector biedt twee methoden ter ondersteuning van synchronisatie van wachtwoorden:

* **Opgeslagen Procedure**: deze methode moet twee opgeslagen procedures voor de ondersteuning van Set & wijzigen wachtwoord. Typ alle parameters voor toevoegen en wijzigen van het wachtwoord opnieuw in **ingesteld wachtwoord SP** en **wijzigen wachtwoord SP** Parameters respectievelijk conform onderstaand voorbeeld.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Wachtwoord extensie**: deze methode moet een wachtwoord uitbreidings-DLL (moet u de naam van de DLL-bestand dat u implementeert de [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) interface). Uitbreidingsassembly wachtwoord moet worden geplaatst in de map de extensie, zodat de connector kan het DLL-bestand tijdens runtime worden geladen.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

U hebt ook de wachtwoord-beheer inschakelen op de **extensie configureren** pagina.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer op de pagina partities en hiërarchieën alle objecttypen. Elk objecttype is zijn eigen partitie.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

U kunt ook de waarden die zijn gedefinieerd op overschrijven de **connectiviteit** of **globale Parameters** pagina.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina is alleen-lezen omdat het anker is al gedefinieerd. De geselecteerde ankerkenmerk worden altijd toegevoegd met het objecttype om ervoor te zorgen, blijft dit wel unieke over objecttypen.

![vertrouwensankers](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Voer stap Parameter configureren
Deze stappen zijn geconfigureerd op de uitvoeringsprofielen op de Connector. Deze configuraties doet het eigenlijke werk van de gegevens importeren en exporteren.

### <a name="full-and-delta-import"></a>Volledige en Delta-Import
Algemene SQL-Connector ondersteuning voor volledige en Delta-Import gebruik van deze methoden:

* Tabel
* Weergave
* Opgeslagen procedure
* SQL Query

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabel of weergave**  
Voor het importeren van meerdere waarden kenmerken voor een object dat u hebt de naam van de tabel of weergave in te geven **naam van meerwaardige tabelweergaven** en respectieve join-voorwaarden in de **joinvoorwaarde** met de bovenliggende tabel . Als er meer dan één tabel voor meerdere waarden in de gegevensbron, kunt u samenvoeging tot één.

>[!IMPORTANT]
De algemene SQL management agent kunt werken alleen met één tabel voor meerdere waarden. Plaats niet in de naam van meerdere waarden tabelweergaven meer dan één naam van de tabel. Dit is de beperking van algemene SQL.


Voorbeeld: U wilt importeren van de werknemer-object en alle bijbehorende kenmerken met meerdere waarden. Er zijn twee tabellen, met de naam werknemer (hoofdtabel) en afdeling (met meerdere waarden).
Ga als volgt te werk:

* Type **werknemer** in **tabel/weergave/SP**.
* Type afdeling in **naam van meerwaardige tabelweergaven**.
* Typ de join-voorwaarde tussen werknemer & afdeling in **Join-voorwaarde**, bijvoorbeeld `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Opgeslagen procedures**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Als u veel gegevens hebt, wordt het aanbevolen voor het implementeren van paginering met uw opgeslagen Procedures.
* Voor de opgeslagen Procedure voor de ondersteuning van paginering, moet u Index Start en End Index opgeven. Zie: [efficiënt paginering van grote hoeveelheden gegevens](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexen @EndIndex tijdens uitvoeringstijd worden vervangen door geconfigureerd op de waarde van de desbetreffende pagina bestandsgrootte **stap configureren** pagina. Bijvoorbeeld, wanneer de connector haalt eerste pagina en de paginagrootte is ingesteld 500, in een dergelijke situatie @StartIndex zou dit 1 en @EndIndex 500. Deze waarden vergroten wanneer connector de volgende pagina's en wijzig haalt de @StartIndex & @EndIndex waarde.
* Geef de parameters in voor het uitvoeren van de opgeslagen Procedure met parameters `[Name]:[Direction]:[Value]` indeling. Voer elke parameter in op een afzonderlijke regel (Gebruik Ctrl + Enter om op te halen van een nieuwe regel).
* Algemene SQL-connector ondersteunt ook de importbewerking van gekoppelde Servers in Microsoft SQL Server. Gegevens moeten worden opgehaald uit een tabel in de gekoppelde server, moet tabel worden opgegeven in de indeling:`[ServerName].[Database].[Schema].[TableName]`
* Algemene SQL-Connector ondersteunt alleen objecten die vergelijkbaar structuur (zowel alias naam en het gegevenstype type) tussen de detectie van gegevens en schema's voor de stappen uitvoeren. Als het geselecteerde object van het schema en de opgegeven informatie in stappen van uitvoering verschilt, zijn SQL-Connector kan geen ondersteuning voor dit soort scenario's.

**SQL Query**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Meerdere resultatensets query's niet ondersteund.
* SQL-query ondersteunt de paginering en bieden start-Index en End Index als een variabele voor de ondersteuning van paginering.

### <a name="delta-import"></a>Delta-Import
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Configuratie van delta-Import is vergeleken met de volledige Import meer configuratie vereist.

* Als u de Trigger of momentopname van de aanpak voor het bijhouden van wijzigingen, en geeft vervolgens geschiedenistabel of momentopname van de database in **geschiedenistabel of momentopname van de databasenaam** vak.
* U moet ook bijvoorbeeld join-voorwaarde tussen geschiedenistabel en bovenliggende tabel bieden`Employee.ID=History.EmployeeID`
* Om bij te houden de transactie op de bovenliggende tabel uit de geschiedenistabel, moet u de naam van de kolom met de bewerkingsinformatie (toevoegen, bijwerken, verwijderen) opgeven.
* Als u watermerk kiest voor het bijhouden van wijzigingen, en geef vervolgens de naam van de kolom met de gegevens opnieuw in **Water Mark kolomnaam**.
* De **wijzigen typekenmerk** kolom is vereist voor het wijzigingstype. In deze kolom wijst een wijziging die in de primaire tabel of meerdere waarden naar een wijziging in de delta-weergave plaatsvindt. Deze kolom mag het wijzigingstype Modify_Attribute voor kenmerk niveau wijzigen of toevoegen, wijzigen of verwijderen Wijzig type voor een type op objectniveau wijzigen. Als deze iets anders dan de standaardwaarde toevoegen, wijzigen of verwijderen, moet u deze waarden met deze optie kunt definiëren.

### <a name="export"></a>Exporteren
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Ondersteuning voor algemene SQL-Connector exporteren met behulp van vier ondersteunde methoden, zoals:

* Tabel
* Weergave
* Opgeslagen procedure
* SQL Query

**Tabel of weergave**  
Als u de tabel of weergave-optie kiest, genereert de connector de respectieve query's voor het uitvoeren van de Export.

**Opgeslagen procedures**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Als u de opgeslagen Procedure-optie kiest, moet exporteren drie verschillende opgeslagen procedures Insert/Update/Delete-bewerkingen uit te voeren.

* **Naam van de Serviceprovider toevoegen**: deze SP wordt uitgevoerd als een object wordt geleverd met connector voor het invoegen in de bijbehorende tabel.
* **Werk de naam SP**: deze SP wordt uitgevoerd als een object wordt geleverd met connector voor update in de bijbehorende tabel.
* **De naam van de Serviceprovider verwijderen**: deze SP wordt uitgevoerd als een object wordt geleverd met connector in de bijbehorende tabel voor verwijdering.
* Het kenmerk is geselecteerd in het schema gebruikt als een parameterwaarde voor de opgeslagen procedure. Bijvoorbeeld: `@EmployeeName: INPUT: EmployeeName` (EmployeeName is geselecteerd in het schema van de connector en de connector vervangt de betreffende waarde tijdens de export)
* Geef voor uitvoering van opgeslagen procedure met parameters, parameters in `[Name]:[Direction]:[Value]` indeling. Voer elke parameter in op een afzonderlijke regel (Gebruik Ctrl + Enter om op te halen van een nieuwe regel).

**SQL-query**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Als u de SQL-query-optie kiest, moet exporteren drie verschillende query's Insert/Update/Delete-bewerkingen uit te voeren.

* **Query invoegen**: deze query wordt uitgevoerd als een object wordt geleverd met connector voor het invoegen in de bijbehorende tabel.
* **Query bijwerken**: deze query wordt uitgevoerd als een object wordt geleverd met connector voor update in de bijbehorende tabel.
* **Query verwijdert**: deze query wordt uitgevoerd als een object wordt geleverd met connector in de bijbehorende tabel voor verwijdering.
* Kenmerk geselecteerd in het schema gebruikt als een parameterwaarde aan de query, bijvoorbeeld`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector de [hoe ETW-tracering inschakelen voor Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).
