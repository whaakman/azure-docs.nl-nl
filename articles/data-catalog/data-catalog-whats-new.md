---
title: Wat is er nieuw in Azure Data Catalog
description: Dit artikel bevat een overzicht van nieuwe mogelijkheden toegevoegd aan Azure Data Catalog.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 86c8e8c10811b1478ae2c853f1efef5b6b5caa83
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406325"
---
# <a name="whats-new-in-azure-data-catalog"></a>Wat is er nieuw in Azure Data Catalog
Updates voor **Azure Data Catalog** regelmatig worden vrijgegeven. Niet elke versie bevat nieuwe functies van de gebruiker gerichte, omdat sommige versies van het gericht zijn op de mogelijkheden van de back-end-service. Deze pagina ziet u nieuwe gebruikersgerichte mogelijkheden toegevoegd aan de service Azure Data Catalog.

## <a name="whats-new-for-november-2017"></a>Wat is er nieuw voor November 2017 
Vanaf November 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het koppelen van rechtstreeks naar specifieke termen voor een bedrijfswoordenlijst in de Data Catalog-portal. Gebruikers kunnen koppelingen van de zakelijke woordenlijst kopiëren en deze insluiten in documenten, e-mailberichten, rapporten of andere locaties om rechtstreeks aan de definitie van de termijn te koppelen.
* Ondersteuning voor Azure Active Directory service-principals. Data Catalog-beheerders kunnen machtigen clienttoepassingen met behulp van service-principals toegang hebben tot de catalogus en kunnen deze toepassingen specifieke machtigingen verlenen zoals ze machtigingen aan gebruikers en beveiligingsgroepen verlenen kunnen. Zie voor meer informatie [toepassing en service-principalobjecten in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md).
* Ondersteuning voor Azure Active Directory-verificatie wanneer u verbinding maakt met gegevensbronnen in de Azure SQL Database en Azure SQL Data Warehouse met behulp van het registratiehulpprogramma voor Data Catalog gegevensbronnen. Zie voor meer informatie [gebruik Azure Active Directory-verificatie voor verificatie bij SQL-Database of SQL Data Warehouse](../sql-database/sql-database-aad-authentication.md).


## <a name="whats-new-for-september-2017"></a>Wat is er nieuw voor September 2017 
Vanaf September 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het extraheren van lid worden van metagegevens van entiteitsrelaties uit gegevensbronnen DB2 bij het registreren van gerelateerde tabellen met behulp van het hulpprogramma voor registratie.
* Ondersteuning voor het registreren van de MongoDB-versie 3.4-gegevensbronnen met behulp van het hulpprogramma voor registratie.
* Ondersteuning voor het verwijderen van alle metagegevens voor objecten in één bewerking wanneer u een database of een andere container uit de gegevenscatalogus verwijdert.
* Ondersteuning voor de weer te geven tot 1000 tags, termen voor een bedrijfswoordenlijst of andere zoeken facetten, wanneer een zoekopdracht in de Data Catalog-portal te verfijnen.


## <a name="whats-new-for-august-2017"></a>Wat is er nieuw voor augustus 2017 
Vanaf augustus 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

*   Een nieuwe steekproef van de ontwikkelaar is beschikbaar voor het maken en beheren van metagegevens van entiteitsrelaties via de REST-API van Data Catalog. De *relatiegegevens importeren in Data Catalog* voorbeeld is beschikbaar op de [pagina met codevoorbeelden Data Catalog](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Ondersteuning voor het extraheren van lid van de metagegevens van entiteitsrelaties uit Teradata-gegevensbronnen bij het registreren van gerelateerde tabellen met behulp van het hulpprogramma voor registratie.
* Ondersteuning voor SQL Server tabelwaardefunctie (TVF) objecten bij het registreren van SQL Server-gegevensbronnen met behulp van het hulpprogramma voor registratie.
* Meerdere updates en verbeteringen om de prestaties en bruikbaarheid van de Data Catalog-portal.

## <a name="whats-new-for-july-2017"></a>Wat is er nieuw voor juli 2017 
Vanaf juli 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor gedetailleerdere controle over dit bewerkingen voor metagegevens met inbegrip van:
    - Catalogusbeheerders kunnen gebruikers beperken om bij te dragen van tags en verwante metagegevens om de catalogus, zodat alleen-lezen toegang tot de catalogus.
    - Catalogusbeheerders kunnen gebruikers bedrijfsdocumenten kunnen nieuwe gegevensbronnen registreren in de catalogus beperken.
    - Catalogusbeheerders kunnen gebruikers bedrijfsdocumenten kunnen eigenaar worden van de metagegevens van de asset gegevens in de catalogus beperken.
    - Er kunnen machtigingen worden verleend aan Azure Active Directory-beveiligingsgroepen en gebruikers gemakkelijker te maken van het beheer van machtigingen.
* Ondersteuning voor relaties tussen geregistreerde gegevensassets en detecterende gerelateerde gegevensassets in de portal voor Data Catalog, met inbegrip van:
    - Extractie van metagegevens van entiteitsrelaties van SQL Server (met inbegrip van Azure SQL Database), Oracle en MySQL gegevensbronnen wanneer u het registratiehulpprogramma voor Data Catalog gegevensbronnen.
    - Detectie van gerelateerde gegevensassets wanneer ze metagegevens van de asset bekijken in de Data Catalog-portal.
    - Bewerkingen voor het definiëren van, detecteren en beheren van relaties tussen gegevensassets met behulp van de REST-API van Data Catalog.

Zie voor meer informatie over het beheren van machtigingen in Data Catalog [over het beveiligen van toegang tot de gegevenscatalogus en gegevensassets](data-catalog-how-to-secure-catalog.md).
Zie voor meer informatie over relaties in Data Catalog [gerelateerde gegevensassets weergeven in Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Wat is er nieuw voor juni 2017 
Vanaf juni 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor Sybase-, Apache Cassandra- en MongoDB-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van Cassandra en MongoDB-databases en tabellen en Sybase-databases, tabellen en weergaven.

> [!NOTE]
> Bij het registreren van MongoDB en Cassandra-tabellen die kolommen met complexe gegevenstypen zoals records en -matrices, worden deze kolommen worden niet opgenomen in de metagegevens toegevoegd aan Data Catalog.

## <a name="whats-new-for-may-2017"></a>Wat is er nieuw voor mei 2017 
Vanaf mei 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Een nieuwe steekproef van de ontwikkelaar is beschikbaar voor het bulksgewijs importeren van termen voor een bedrijfswoordenlijst. De verklarende woordenlijst bulkimport-voorbeeld is beschikbaar op de [Data Catalog developer-pagina met codevoorbeelden](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Ondersteuning voor het bewerken van ODBC-verbindingsgegevens in de Azure Data Catalog-portal. Gegevens asset eigenaars en beheerders van de Data Catalog kunnen u nu de verbindingsgegevens voor de geregistreerde ODBC-gegevensbronnen bewerken zonder dat u hoeft de gegevensbronnen opnieuw te registreren.
*   Ondersteuning voor geklikt URL's in de zakelijke woordenlijst definities en beschrijvingen. Als URL's zijn opgenomen in de eigenschappen van de beschrijving en de definitie voor termen voor een bedrijfswoordenlijst, wordt de URL's worden weergegeven als hyperlinks in de Data Catalog-portal.
*   Ondersteuning voor het weergeven van deskundige namen naast deskundige e-mailadressen. Wanneer u experts in de eigenschappen voor een gegevensasset bekijkt in de Data Catalog-portal, wordt de volledige naam van de expert uit Azure Active Directory worden weergegeven in de knopinfo.

## <a name="whats-new-for-april-2017"></a>Wat is er nieuw voor April 2017 
Vanaf April 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor ODBC-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van ODBC-databases, tabellen en weergaven met behulp van het registratiehulpprogramma voor Data Catalog gegevensbronnen.

## <a name="whats-new-for-march-2017"></a>Wat is er nieuw voor maart 2017 
Vanaf maart 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor het gebruik van AAD-beveiligingsgroepen voor Data Catalog-beheerders.
*   Azure Data Catalog is nu beschikbaar in een nieuwe Azure-regio. Klanten kunnen inrichten van de Azure Data Catalog in West-Centraal VS regio, naast VS-Oost, VS-West, West-Europa en Australië-Oost, Noord-Europa en Zuidoost-Azië. Zie voor meer informatie, [Azure-regio's](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Wat is er nieuw voor februari 2017 
Vanaf februari 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor geavanceerde instellingen in het registratiehulpprogramma voor Data Catalog gegevensbronnen. Bij het registreren van grote gegevensbronnen, kunnen gebruikers de time-outwaarden opdracht opgeven.
*   Ondersteuning voor FTP- en PostgreSQL-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van FTP-bestanden en mappen, en PostgreSQL-databases, tabellen en weergaven.

## <a name="whats-new-for-january-2017"></a>Wat is er nieuw voor januari 2017 
Vanaf januari 2017, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [CSA STAR](https://www.microsoft.com/en-us/trustcenter/compliance/csa-star-certification) voldoen aan het beleid.
*   Integratie met [ophalen en transformeren in Excel 2016 en Power Query voor Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel-gebruikers kunnen delen van query's en query's met behulp van Azure Data Catalog uit detecteren in Excel. Deze functionaliteit is beschikbaar voor gebruikers met Power BI Pro-licenties.

## <a name="whats-new-for-december-2016"></a>Wat is er nieuw voor December 2016
Vanaf December 2016 zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa) en [EU-Modelclausules](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses) voldoen aan het beleid.
*   Ondersteuning voor het bewerken van informatie van de gegevensbronverbinding. Gegevens asset eigenaars en beheerders van de Data Catalog kunnen u nu de verbindingsgegevens voor geregistreerde gegevensbronnen bewerken zonder dat u hoeft de gegevensbronnen opnieuw te registreren.
*   Ondersteuning voor Salesforce.com-gegevensbronnen. Gebruikers kunnen nu registreren en Salesforce-objecten te detecteren.


## <a name="whats-new-for-november-2016"></a>Wat is er nieuw voor November 2016
Vanaf November 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) en [ISO/IEC 27018](https://www.microsoft.com/en-us/TrustCenter/Compliance/iso-iec-27018) voldoen aan het beleid.
*   Ondersteuning voor de handmatige registratie van ODBC-gegevensbronnen met behulp van de Data Catalog-portal en de REST-API.

## <a name="whats-new-for-september-2016"></a>Wat is er nieuw voor September 2016
Vanaf September 2016 zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor IBM DB2-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van de DB2-databases, tabellen en weergaven.
* Ondersteuning voor Azure Cosmos DB-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van Cosmos DB-databases en verzamelingen.
* Ondersteuning voor het aanpassen van de catalogusnaam van de in de Data Catalog-portal. Catalogusbeheerders kunnen nu opgeven voor tekst die wordt weergegeven in de titel van de portal, zoals de naam van de organisatie.

## <a name="whats-new-for-august-2016"></a>Wat is er nieuw voor augustus 2016
Vanaf augustus 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Verbeteringen voor de registratie van gegevensbronnen van SQL Server Master Data Services (MDS). Gebruikers kunnen nu profielen voorbeelden en gegevens opnemen tijdens het registreren van de MDS-entiteiten met behulp van het registratiehulpprogramma voor Data Catalog gegevensbronnen.
* Ondersteuning voor organisatie-opgeslagen zoekopdrachten door beheerder gedefinieerde. Bij het opslaan van een zoekopdracht in de Data Catalog-portal, kiest u kunnen nu Data Catalog-beheerders om op te slaan zoekopdrachten voor persoonlijk gebruik of voor alle gebruikers van de catalogus. Opgeslagen zoekopdrachten organisatie worden gedeeld met alle gebruikers van de catalogus en gestandaardiseerde beginpunten kunnen opgeven voor detectie van gegevensbronnen.
* Updates voor de eigenschappenweergave in de Data Catalog-portal. Alle eigenschappen worden nu weergegeven en beheerd in een deelvenster voor één, vergroten/verkleinen voor een consistentere en ervaring kunnen worden gedetecteerd.

## <a name="whats-new-for-july-2016"></a>Wat is er nieuw voor juli 2016
Vanaf juli 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor SQL Server Master Data Services (MDS)-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren MDS-modellen en entiteiten.
* Ondersteuning voor SQL Server opgeslagen procedures. Gebruikers kunnen nu registreren en detecteren van objecten van de opgeslagen procedure in SQL Server-gegevensbronnen.
* Ondersteuning voor extra talen in de Azure Data Catalog-portal en het registratiehulpprogramma voor gegevensbronnen, voor een totaal van 18 ondersteunde talen. De gebruikerservaring van Azure Data Catalog is gelokaliseerd op basis van de taalvoorkeuren instellen in Windows of in de webbrowser.
* Updates en verfijning voor de Data Catalog portal introductiepagina, met inbegrip van verbeterde prestaties en een gestroomlijnde gebruikerservaring.

## <a name="whats-new-for-june-2016"></a>Wat is er nieuw voor juni 2016
Vanaf juni 2016 worden zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het formaat van kolommen in de lijstweergave wijzigen bij het detecteren van gegevensassets in de Data Catalog-portal. Gebruikers kunnen nu het formaat van afzonderlijke kolommen om te lezen eenvoudiger langdurige asset metagegevens, zoals labels en beschrijvingen.
* Power Query voor Excel is toegevoegd aan het menu 'Open in' in de Data Catalog-portal. Gebruikers kunnen nu ondersteunde gegevensbronnen openen in Excel 2016 of in Excel 2010 of Excel 2013 met de [Power Query voor Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) invoegtoepassing geïnstalleerd.
* Ondersteuning voor Azure Table Storage-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van objecten van de tabel in Azure Storage-gegevensbronnen.

## <a name="whats-new-for-may-2016"></a>Wat is er nieuw voor mei 2016
Vanaf mei 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Een zakelijke woordenlijst waarmee catalogusbeheerders bedrijfstermen en hiërarchieën maken van een algemeen bedrijfsjargon definiëren. Gebruikers, geregistreerde gegevensassets met termen in de woordenlijst zodat het eenvoudiger te detecteren en begrijpen van de inhoud van de catalogus kunnen labelen. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md)  
* Verbeteringen in de Data Catalog zakelijke woordenlijst waarmee gebruikers om bij te werken van meerdere termen in de woordenlijst in één bewerking. Gebruikers kunnen meerdere voorwaarden bewerken van de volgende velden selecteren:
  * Bovenliggende Term: De gebruiker kan een nieuwe bovenliggende term selecteren en alle geselecteerde voorwaarden worden bijgewerkt om te worden van onderliggende items van de geselecteerde bovenliggende term. Als de geselecteerde alle voorwaarden dezelfde bovenliggende hebt, wordt dat de bovenliggende wordt weergegeven in het tekstvak, anders wordt de bovenliggende term veld is ingesteld op leeg.   
  * Tags en belanghebbenden: gebruikers kunnen toevoegen en verwijderen van tags en belanghebbenden voor meerdere termen in de woordenlijst met de dezelfde ervaring als meerdere gegevensassets labelen.

> [!NOTE]
> De zakelijke woordenlijst is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De Free Edition biedt mogelijkheden voor beheerde-tagging of een zakelijke woordenlijst.

## <a name="whats-new-for-march-2016"></a>Wat is er nieuw voor maart 2016
Vanaf maart 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog: g:

* Een geconsolideerde REST-API-eindpunt voor het programmatisch toegang tot de zoekmogelijkheden en catalogus beheer van middelen van de service Azure Data Catalog. Dit eindpunt zoeken-API-eindpunt en catalogus-API is afgeschaft en buiten gebruik gesteld op 21 maart 2016. Er zijn geen wijzigingen in de semantiek van de API. Alleen de eindpunt-URI gewijzigd. Zie voor meer informatie de [Azure Data Catalog REST API-verwijzing](https://msdn.microsoft.com/library/azure/mt267595.aspx). Zie voor voorbeelden van de API, [voorbeelden voor ontwikkelaars van Azure Data Catalog](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Wat is er nieuw voor februari 2016
Vanaf februari 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Een nieuw vernieuwde gegevens voor de geselecteerde bron-ervaring in het registratiehulpprogramma voor Azure Data Catalog gegevensbronnen. Het registratiehulpprogramma voor gegevensbronnen is bijgewerkt om het gemakkelijker om te zoeken en selecteren van de gegevensbronnen ondersteund door Azure Data Catalog.
* Ondersteuning voor 10 extra talen in de Azure Data Catalog-portal en het hulpprogramma voor registratie. Naast Engels, zijn de Azure Data Catalog-ervaring is nu beschikbaar in het Duits, Spaans, Frans, Italiaans, Japans, Koreaans, Portugees (Brazilië), Russisch, Chinees vereenvoudigd en traditioneel Chinees. De gebruikerservaring van Azure Data Catalog is gelokaliseerd op basis van de taalvoorkeuren instellen in Windows of in de webbrowser van de gebruiker.
* Ondersteuning voor geo-replicatie van gegevens van Azure Data Catalog voor zakelijke continuïteit en herstel na noodgevallen. Alle inhoud van de Azure Data Catalog, met inbegrip van data source-metagegevens en via crowdsourcing aantekeningen, worden nu gerepliceerd tussen twee Azure-regio's zonder extra kosten aan klanten. De Azure-regio's zijn vooraf geplaatst ten minste 500 mijl uit elkaar liggen, en volgt u de toewijzing zoals beschreven in [zakelijke continuïteit en herstel na noodgevallen (BCDR): gekoppelde regio's Azure](../best-practices-availability-paired-regions.md).
* Ondersteuning voor het wijzigen van het Azure-abonnement gebruikt door Azure Data Catalog. Azure Data Catalog-beheerders kunnen de pagina instellingen in de Azure Data Catalog-portal gebruiken om te selecteren van een ander Azure-abonnement voor factureringsdoeleinden.

## <a name="whats-new-for-january-2016"></a>Wat is er nieuw voor januari 2016
Vanaf januari 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het handmatig extra gegevensbronnen te registreren. U kunt nu 'Handmatige vermelding maken' in de Azure Data Catalog-portal gebruiken, of de REST-API van Azure Data Catalog gebruiken voor het registreren van de volgende gegevensbronnen:
  * OData - functie, entiteitsset en Entiteitscontainer
  * HTTP - bestand, eindpunt, rapport en -Site
  * File System - bestand
  * SharePoint - lijst
  * FTP - bestanden en mappen
  * SalesForce.com - Object
  * DB2 - tabel, weergave en -Database
  * PostgreSQL - tabel, weergave en -Database
* Ondersteuning voor 'Openen in SQL Server Data Tools' voor gegevensbronnen van SQL Server (met inbegrip van Azure SQL DB en Azure SQL Data Warehouse).  
* Ondersteuning voor het registreren en detecteren van SAP HANA-weergaven en pakketten. SAP HANA-gegevensbronnen met behulp van de Azure Data Catalog gegevensbron hulpprogramma voor registratie en kunt annoteren en detecteren van geregistreerde gegevensbronnen van SAP HANA met behulp van de Azure Data Catalog-portal, kunt u registreren.
* De mogelijkheid om te koppelen en loskoppelen van gegevensassets in de Azure Data Catalog-portal. U kunt vastmaken gegevensassets zodat ze gemakkelijker te opnieuw detecteren en opnieuw te gebruiken.
* Een nieuw vernieuwde startpagina in de Azure Data Catalog-portal. De nieuwe startpagina bevat inzicht in de huidige activiteit van gebruikers - met inbegrip van onlangs gepubliceerde activa, vastgemaakte assets en opgeslagen zoekopdrachten- en meer inzicht in de activiteit in de catalogus in zijn geheel.
* Ondersteuning voor permanente gebruikersinstellingen in de Azure Data Catalog-portal. Instellingen voor gebruikerservaring gebruiker - met inbegrip van raster of tegel weergeven, het aantal resultaten per pagina en in- of uitschakelen markeren van treffers - worden gehandhaafd blijft tussen sessies van gebruiker.
* Azure Data Catalog is nu beschikbaar in twee nieuwe Azure-regio's. Klanten kunnen de Azure Data Catalog in de regio Noord-Europa en Zuidoost-Azië, naast VS-Oost, VS-West, West-Europa en Australië-Oost inrichten. Zie voor meer informatie, [Azure-regio's](https://azure.microsoft.com/regions/).

> [!NOTE]
> 'Open in SQL Server Data Tools' vereist dat Visual Studio 2013 Update 4 en hulpprogramma's SQL Server worden geïnstalleerd. Als u wilt installeren de nieuwste versie van SQL Server Data Tools, gaat u naar [Download SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Wat is er nieuw voor December 2015
Vanaf December 2015 zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor gegevens van profielen voor Azure SQL Data Warehouse-gegevensbronnen. Bij het registreren van Azure SQL Data Warehouse-tabellen en weergaven, kunnen gebruikers kiezen om op te nemen van gegevens profiel metrische gegevens met de metagegevens die zijn geëxtraheerd uit de gegevensbron.
* Ondersteuning voor het registreren en detecteren van MySQL-objecten en -databases. Gebruikers kunnen zich registreren voor MySQL-gegevensbronnen met behulp van de Azure Data Catalog gegevensbron hulpprogramma voor registratie en kunt annoteren en detecteren van geregistreerde gegevensbronnen van MySQL met behulp van de Azure Data Catalog-portal.
* Ondersteuning voor het SPNEGO- en Windows-verificatie voor Teradata-gegevensbronnen. Bij het registreren van Teradata-tabellen en weergaven, kunnen gebruikers kiezen verbinding maken met Teradata SPNEGO- en Windows, en LDAP en TD2 verificatie gebruikt.
* Ondersteuning voor Azure Data Lake Store-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van gegevensbronnen voor Azure Data Lake Store met behulp van Azure Data Catalog.
* Ondersteuning voor het netwerk proxy-instellingen handmatig opgeven in de Azure Data Catalog hulpprogramma voor registratie. Gebruikers 'Proxy-instellingen wijzigen' kunnen kiezen uit de welkomstpagina van het hulpprogramma en de proxy-adres en poort moet worden gebruikt door het hulpprogramma kunnen opgeven.

## <a name="whats-new-for-november-2015"></a>Wat is er nieuw voor November 2015
Vanaf November 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* De mogelijkheid om te bekijken en kopiëren van verbindingsreeksen uit binnen de Azure Data Catalog-portal voor SQL-Server (met inbegrip van Azure SQL Database) en Oracle-gegevensbronnen. Gebruikers kunnen klikken op 'Verbindingsreeksen weergeven' koppeling in de verbindingsgegevens voor een SQL Server of Oracle-tabel, weergave of database, om te zien van de verbindingsreeksen waarmee verbinding wordt gemaakt met de gegevensbron. ADO.NET, ODBC, OLEDB en JDBC verbindingsreeksen worden opgegeven voor de SQL Server-gegevensbronnen. ODBC- en OLEDB-verbindingsreeksen zijn beschikbaar voor Oracle-gegevensbronnen.
* Ondersteuning voor het opnemen van gegevens profielen bij het registreren van Teradata-tabellen en weergaven.
* Ondersteuning voor 'Openen in Power BI Desktop' voor SQL Server (met inbegrip van Azure SQL DB en Azure SQL Data Warehouse), SQL Server Analysis Services en Azure Storage, HDFS-bronnen.  
* Ondersteuning voor LDAP-verificatie voor Teradata-gegevensbronnen. Bij het registreren van Teradata-tabellen en weergaven, gebruikers kunnen kiezen om te verbinden met Teradata met behulp van LDAP, en TD2 verificatie.
* Ondersteuning voor 'Openen in Excel' voor Teradata-gegevensbronnen.
* Ondersteuning voor recente zoektermen op in de Azure Data Catalog-portal. Wanneer u zoekt in de portal, kunnen gebruikers selecteren van recent gebruikte zoektermen om de detectie-ervaring te versnellen.
* Ondersteuning voor Preview-versie voor Teradata-gegevensbronnen. Bij het registreren van Teradata-tabellen en weergaven, kunnen gebruikers kiezen momentopname om records te nemen met de metagegevens die zijn geëxtraheerd uit de gegevensbron.
* Ondersteuning voor 'Openen in Excel' voor Azure SQL Data Warehouse-gegevensbronnen.
* Ondersteuning voor het definiëren en bewerken op kolomniveau schema's voor handmatig geregistreerde gegevensassets. Nadat een gegevensasset met behulp van de Azure Data Catalog-portal handmatig is gemaakt, kunnen gebruikers de kolomdefinities toevoegen in de eigenschappen.
* Ondersteuning voor 'is'-query's bij het zoeken naar Azure Data Catalog, zodat de detectie van geregistreerde gegevensassets die beschikken over specifieke metagegevens. Azure Data Catalog-querysyntaxis bevat nu:

| Querysyntaxis | Doel |
| --- | --- |
| `has:previews` |Zoeken naar gegevensassets die een Preview-versie |
| `has:documentation` |Zoeken naar gegevensassets waarvoor documentatie is opgegeven |
| `has:tableDataProfiles` |Zoeken naar gegevensassets met profielgegevens van gegevens op een tabel-niveau |
| `has:columnsDataProfiles` |Zoeken naar gegevensassets met profielgegevens op kolomniveau gegevens |


> [!NOTE]
> 'Open in Power BI Desktop' moet een actuele versie van de Power BI Desktop-toepassing wordt geïnstalleerd. Als u problemen ondervindt of fouten met deze functie, zorgt u ervoor dat u hebt de nieuwste versie van Power BI Desktop uit [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Wat is er nieuw voor oktober 2015
Vanaf oktober 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor versleuteling van inactieve gegevens voor profielen voor voorbeelden en gegevens voor geregistreerde gegevensbronnen. Azure Data Catalog transparant versleutelt alle records die Preview-versie en -gegevens profielen gegevensbronnen die zijn geregistreerd bij de service, hoeft u geen voor sleutelbeheer door beheerders van de catalogus.
* Ondersteuning voor Teradata-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van Teradata-tabellen en weergaven.
* Ondersteuning voor on-premises Hive-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van Hive-tabellen voor Apache Hive in Hadoop on-premises gegevensbronnen.
* Ondersteuning voor opgeslagen zoekopdrachten in de Azure Data Catalog-portal. Gebruikers kunnen opslaan zoektermen en filterselecties eenvoudig Herhaal de vorige zoekopdrachten en nuttige weergaven van de inhoud van de catalogus definiëren. Gebruiker kan ook een opgeslagen zoekopdracht markeren als hun Standaardzoekopdracht. Wanneer een gebruiker op het zoekpictogram 'Vergrootglas' vanaf de startpagina voor Azure Data Catalog-portal of via de pagina 'aan de slag', wordt de gebruiker die rechtstreeks naar de opgeslagen zoekopdracht is gemarkeerd als standaard.
* Ondersteuning voor RTF-documentatie voor geregistreerde gegevensassets en containers in de Azure Data Catalog-portal. Gebruikers kunnen nu bieden de documentatie voor gegevensassets zoals tabellen, weergaven en rapporten, en voor containers, zoals databases en -modellen voor scenario's waarin labels en beschrijvingen niet voldoende zijn.
* Ondersteuning voor het handmatig registreren van bekende typen gegevensbronnen. Gebruikers kunnen de informatie van de gegevensbron met behulp van de Azure Data Catalog-portal voor alle typen gegevensbronnen ondersteund door Azure Data Catalog handmatig invoeren.
* Ondersteuning voor het verlenen van Azure Active Directory-beveiligingsgroepen. Catalogusbeheerders kunnen catalogus toegang tot beveiligingsgroepen, gebruikersaccounts, waardoor het gemakkelijker voor het beheren van toegang tot Azure Data Catalog inschakelen.
* Ondersteuning voor Hive-gegevensbronnen in Excel opent vanuit de Azure Data Catalog-portal.

> [!NOTE]
> Voor de huidige versie, wordt alleen Teradata TD2 verificatie ondersteund. Aanvullende verificatie mechanismen worden ondersteund in toekomstige releases.

> [!NOTE]
> Voor het gebruik van de functie voor 'Openen in Excel' voor Hive-gegevensbronnen, moeten op gebruikers het ODBC-stuurprogramma voor Hive hebben geïnstalleerd.

## <a name="whats-new-for-september-2015"></a>Wat is er nieuw voor September 2015
Vanaf September 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het opnemen van gegevens profielen bij het registreren van Hive-gegevensbronnen.
* Ondersteuning voor het detecteren van via een programma met de API-catalogus, waardoor het gemakkelijker wordt om toepassingen te integreren met Azure Data Catalog.
* Een nieuwe 'aan de slag' gegevensbron discovery-ervaring in de Azure Data Catalog-portal. Wanneer gebruikers de pagina "ontdekken" van de Azure Data Catalog-portal zonder een zoekterm invoert, worden ze weergegeven met een overzicht van de inhoud van de catalogus met inbegrip van de meest gebruikte tags, experts, typen gegevensbronnen en objecttypen.
* Ondersteuning voor het registreren en detecteren van Azure SQL Data Warehouse-objecten en -databases. Zie voor meer informatie over Azure SQL Data Warehouse, [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Ondersteuning voor het registreren en detecteren van SQL Server Analysis Services-modellen en SQL Server Reporting Services-servers als containers. Bij het registreren van SSAS- en SQL Server Reporting Services-objecten, maakt Azure Data Catalog u een vermelding voor de SSAS-model en de SSRS-server, en voor de rapporten en andere objecten. De containers kunnen worden gedetecteerd en van aantekeningen voorzien met behulp van de Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een model of de server naast zoeken en filteren van de inhoud van de catalogus.
* Ondersteuning voor het registreren en detecteren van SQL Server Analysis Services-objecten via HTTP/HTTPS. Gebruikers kunnen nu verbinding maken met de SSAS-servers via een URL (zoals https://servername/olap/msmdpump.dll) in plaats van een server een naam en basisverificatie en anonieme verbindingen naast Windows-verificatie kunt gebruiken. Zie voor meer informatie over HTTP/HTTPS-verbindingen met SSAS, [HTTP-toegang configureren met Analysis Services](https://msdn.microsoft.com/library/gg492140.aspx).
* Ondersteuning voor gegevensbronnen van Hive in HDInsight. Gebruikers kunnen nu registreren en detecteren van Hive-tabellen voor Apache Hive in Hadoop op HDInsight-gegevensbronnen. Zie voor meer informatie over Hive in HDInsight, de [HDInsight-documentatiecentrum](../hdinsight/hadoop/hdinsight-use-hive.md).
* Ondersteuning voor het registreren en detecteren van Oracle-databases en clusters van HDFS als containers. Bij het registreren van Oracle-tabellen en weergaven of HDFS, maakt Azure Data Catalog u een vermelding voor de database, tabellen en weergaven. De database kan worden gedetecteerd en van aantekeningen voorzien met behulp van de Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een database of het cluster naast zoeken en filteren van de inhoud van de catalogus.
* Ondersteuning voor het handmatig registreren onbekende typen gegevensbronnen. Gebruikers kunnen de informatie van de gegevensbron met behulp van de Azure Data Catalog-portal, zodat de gegevensbronnen niet expliciet worden ondersteund door het registratiehulpprogramma voor gegevensbronnen kunnen worden van aantekeningen voorzien en ontdekt handmatig invoeren.
* Ondersteuning voor het registreren en detecteren van SQL Server-databases als containers. Bij het registreren van SQL Server-tabellen en weergaven, wordt een vermelding voor de database, tabellen en weergaven gemaakt in Azure Data Catalog. De database kan worden gedetecteerd en van aantekeningen voorzien met behulp van de Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een database naast zoeken en filteren van de inhoud van de catalogus.

> [!NOTE]
> SSAS- en SQL Server Reporting Services-objecten die geregistreerd vóór de release van September 18 zijn moeten opnieuw worden geregistreerd met behulp van het hulpprogramma voor registratie voordat de vermelding model of de server is toegevoegd aan de catalogus. Een gegevensbron opnieuw te registreren, heeft dit geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de Azure Data Catalog-portal.

> [!NOTE]
> Oracle-tabellen en weergaven en HDFS-bestanden en mappen die geregistreerd vóór de release van 11 September zijn moeten opnieuw worden geregistreerd met behulp van het hulpprogramma voor registratie voordat de post-database of het cluster is toegevoegd aan de catalogus. Een gegevensbron opnieuw te registreren, heeft dit geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de Azure Data Catalog-portal.

> [!NOTE]
> SQL Server-tabellen en weergaven die geregistreerd vóór de release van September 4 zijn moeten opnieuw worden geregistreerd met behulp van het hulpprogramma voor registratie voordat de databasevermelding is toegevoegd aan de catalogus. Een gegevensbron opnieuw te registreren, heeft dit geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de Azure Data Catalog-portal.

## <a name="whats-new-for-august-2015"></a>Wat is er nieuw voor augustus 2015
Vanaf augustus 2015 geldt zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het profileren van gegevens van SQL Server en Oracle-gegevensbronnen. Bij het registreren van SQL Server en Oracle-tabellen en weergaven, kunnen gebruikers kiezen om op te nemen van de profielgegevens van gegevens voor de objecten die worden geregistreerd. Het gegevensprofiel bevat statistieken op objectniveau en op kolomniveau.
* Ondersteuning voor Hadoop HDFS-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van HDFS-bestanden en mappen.
* Ondersteuning voor het bieden van toegang tot aanvraag informatie voor geregistreerde gegevensbronnen. Voor alle geregistreerde gegevensasset kunnen gebruikers nu instructies voor het aanvragen van toegang, met inbegrip van koppelingen in e-mailadres of URL's, eenvoudig kunt integreren met bestaande hulpmiddelen en processen opgeven.
* Knopinfo voor labels en experts, zodat u gemakkelijk om te ontdekken welke gebruikers de metagegevens voor geregistreerde gegevensassets hebt opgegeven.
* Er is een nieuwe 'Gebruiker'-knop en het menu aan de bovenste navigatiebalk toegevoegd. Dit menu krijgt de gebruiker het account dat wordt gebruikt voor aanmelding bij Azure Data Catalog en u Meld u af als gewenst. Dit menu ook weergegeven de naam van de gegevenscatalogus, wat nuttig is voor ontwikkelaars met behulp van de REST-API van Azure Data Catalog is.
* Standard-editie alleen: Bij het toevoegen van eigenaren aan gegevensassets, Azure Data Catalog ondersteunt nu de gebruikersaccounts en beveiligingsgroepen als eigenaren. Als u wilt een beveiligingsgroep toevoegen als eigenaar van geselecteerde gegevensassets, kunt u de weergavenaam van de groep of van de groep UPN-e-mailadres invoeren als er een.
* Ondersteuning voor Azure Blob Storage-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren van Azure Storage-blobs en mappen.

