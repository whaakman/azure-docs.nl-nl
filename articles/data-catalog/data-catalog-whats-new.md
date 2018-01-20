---
title: Wat is er nieuw in Azure Data Catalog | Microsoft Docs
description: Dit artikel bevat een overzicht van nieuwe mogelijkheden toegevoegd aan Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 9fb7814a8412200f6d31cfb9dcaee4663d7cea97
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="whats-new-in-azure-data-catalog"></a>Wat is er nieuw in Azure Data Catalog
Updates voor de **Azure Data Catalog** regelmatig worden vrijgegeven. Niet elke versie bevat nieuwe gebruikersgerichte functies, zoals sommige versies zijn gericht op de mogelijkheden van de back-end-service. Deze pagina licht nieuwe gebruikersgerichte mogelijkheden toegevoegd aan de service Azure Data Catalog.

## <a name="whats-new-for-november-2017"></a>Wat is er nieuw voor November 2017 
De volgende mogelijkheden hebt vanaf November 2017 toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het koppelen van rechtstreeks naar specifieke termen in de in de portal voor Data Catalog. Gebruikers kunnen koppelingen van de zakelijke woordenlijst kopiëren en deze opnemen in documenten, e-mailberichten, rapporten of andere locaties directe koppeling naar de definitie van de termijn.
* Ondersteuning voor Azure Active Directory-service-principals. Data Catalog beheerders kunnen machtigen clienttoepassingen met service-principals toegang krijgen tot de catalogus en kunnen deze toepassingen specifieke machtigingen verlenen zoals ze machtigingen aan gebruikers en beveiligingsgroepen verlenen kunnen. Zie voor meer informatie [toepassing en service-principal objecten in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).
* Ondersteuning voor Azure Active Directory-verificatie bij het verbinden met gegevensbronnen in de Azure SQL Database en Azure SQL Data Warehouse met behulp van het hulpprogramma registratie Data Catalog voor gegevensbronnen. Zie voor meer informatie [Azure Active Directory-verificatie gebruiken voor verificatie bij SQL-Database of SQL Data Warehouse](../sql-database/sql-database-aad-authentication.md).


## <a name="whats-new-for-september-2017"></a>Wat is er nieuw voor September 2017 
Vanaf September 2017, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het uitpakken van lid worden van de metagegevens van de relatie van gegevensbronnen DB2 bij het registreren van de gerelateerde tabellen met het registratiehulpprogramma van gegevensbronnen.
* Ondersteuning voor het registreren van MongoDB versie 3.4 gegevensbronnen met behulp van het registratiehulpprogramma van gegevensbronnen.
* Ondersteuning voor het verwijderen van alle metagegevens voor objecten in één bewerking bij het verwijderen van een database of een andere container van Data Catalog.
* Ondersteuning voor weer te geven tot 1.000 labels, termen in de zakelijke of andere zoeken facetten, wanneer een zoekopdracht in de portal voor Data Catalog verfijnen.


## <a name="whats-new-for-august-2017"></a>Wat is er nieuw voor augustus 2017 
Vanaf augustus 2017, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

*   Een nieuwe developer-voorbeeld is beschikbaar voor het maken en beheren van metagegevens van de relatie met behulp van de REST-API van Data Catalog. De *relatiegegevens importeren in Data Catalog* voorbeeld is beschikbaar op de [voorbeelden van Data Catalog codetabel](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Ondersteuning voor het uitpakken van lid worden van de metagegevens van de relatie van gegevensbronnen Teradata bij het registreren van de gerelateerde tabellen met het registratiehulpprogramma van gegevensbronnen.
* Ondersteuning voor SQL Server tabelgewaardeerde functie (TVF)-objecten bij het registreren van gegevensbronnen van SQL Server met behulp van het registratiehulpprogramma van gegevensbronnen.
* Meerdere updates en verbeteringen om de prestaties en bruikbaarheid van de Data Catalog-portal.

## <a name="whats-new-for-july-2017"></a>Wat is er nieuw voor juli 2017 
De volgende mogelijkheden hebt vanaf juli 2017 toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor gedetailleerdere controle over de bewerkingen voor metagegevens dit is toegestaan met inbegrip van:
    - Catalogus beheerders kunnen gebruikers beperken om bij te dragen tags en verwante metagegevens om de catalogus, zodat alleen-lezen toegang tot de catalogus.
    - Catalogus-beheerders kunnen gebruikers beperken nieuwe gegevensbronnen registreren in de catalogus.
    - Catalogus-beheerders kunnen gebruikers beperken eigenaar van de metagegevens van de asset in de catalogus.
    - Machtigingen kunnen worden toegekend aan Azure Active Directory-beveiligingsgroepen en gebruikers voor eenvoudig beheer van machtigingen.
* Ondersteuning voor relaties tussen geregistreerde gegevensassets en detecterende gerelateerde gegevensassets in de portal voor Data Catalog, met inbegrip van:
    - Uitpakken van de metagegevens van de relatie van SQL Server (met inbegrip van Azure SQL Database), Oracle en MySQL gegevensbronnen wanneer u het hulpprogramma registratie Data Catalog voor gegevensbronnen.
    - Detectie van activa gerelateerde gegevens bij het bekijken van asset-metagegevens in de portal voor Data Catalog.
    - Bewerkingen om te definiëren, detecteren en beheren van relaties tussen gegevensassets met behulp van de REST-API van Data Catalog.

Zie voor meer informatie over het beheren van machtigingen in Data Catalog [het beveiligen van toegang tot de catalogus met gegevens en gegevensassets](data-catalog-how-to-secure-catalog.md).
Zie voor meer informatie over de relaties in Data Catalog [activa gerelateerde gegevens weergeven in Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Wat is er nieuw voor juni 2017 
Vanaf juni 2017, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor Sybase Apache Cassandra en MongoDB-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren Cassandra en MongoDB-databases en tabellen en Sybase-databases en tabellen en weergaven.

> [!NOTE]
> Bij het registreren van MongoDB en Cassandra tabellen die kolommen met complexe gegevenstypen zoals records en matrices bevatten, worden deze kolommen worden niet opgenomen in de metagegevens toegevoegd aan Data Catalog.

## <a name="whats-new-for-may-2017"></a>Wat is er nieuw voor mei 2017 
De volgende mogelijkheden hebt vanaf mei 2017 toegevoegd aan Azure Data Catalog:
*   Een nieuwe developer-voorbeeld is beschikbaar voor het bulksgewijs importeren termen in de zakelijke. De verklarende woordenlijst bulkimport voorbeeld is beschikbaar op de [pagina met Data Catalog developer-voorbeelden](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Ondersteuning voor het bewerken van ODBC-verbindingsgegevens in de portal voor Azure Data Catalog. Eigenaren van asset en beheerders van Data Catalog kunnen u nu de verbindingsgegevens voor de geregistreerde ODBC-gegevensbronnen bewerken zonder de gegevensbronnen opnieuw te registreren.
*   Ondersteuning voor klikbaar URL's in de zakelijke woordenlijst definities en beschrijvingen. Wanneer de URL's zijn opgenomen in de eigenschappen van de beschrijving en de definitie voor termen in de bedrijven, wordt de URL's weergegeven als hyperlinks in de portal voor Data Catalog.
*   Ondersteuning voor het weergeven van deskundige namen naast deskundige e-mailadressen. Wanneer u experts in de eigenschappen voor een gegevensasset bekijkt in de portal voor Data Catalog, wordt de volledige naam van de expert van Azure Active Directory weergegeven in de knopinfo.

## <a name="whats-new-for-april-2017"></a>Wat is er nieuw voor April 2017 
De volgende mogelijkheden hebt vanaf April 2017 toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor ODBC-gegevensbronnen. Gebruikers kunnen nu registreren en ODBC-databases, tabellen en weergaven met het hulpprogramma registratie Data Catalog voor gegevensbronnen detecteren.

## <a name="whats-new-for-march-2017"></a>Wat is er nieuw voor maart 2017 
De volgende mogelijkheden hebt vanaf maart 2017 toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor het gebruik van AAD-beveiligingsgroepen voor beheerders van Data Catalog.
*   Azure Data Catalog is nu beschikbaar in een nieuwe Azure-regio. Klanten kunnen inrichten van de Azure Data Catalog in West-Centraal VS regio, naast het VS-Oost, VS-West, West-Europa en Australië-Oost, Noord-Europa en Zuidoost-Azië. Zie voor meer informatie [Azure-gebieden](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Wat is er nieuw voor februari 2017 
Vanaf februari 2017, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Ondersteuning voor geavanceerde instellingen in het hulpprogramma registratie Data Catalog voor gegevensbronnen. Gebruikers kunnen opdracht time-outwaarden opgeven bij het registreren van gegevensbronnen van grote hoeveelheden gegevens.
*   Ondersteuning voor FTP- en PostgreSQL-gegevensbronnen. Gebruikers kunnen nu registreren en FTP-bestanden en mappen, en PostgreSQL databases, tabellen en weergaven te detecteren.

## <a name="whats-new-for-january-2017"></a>Wat is er nieuw voor januari 2017 
De volgende mogelijkheden hebt vanaf januari 2017 toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [CSA STER](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) compatibel.
*   Integratie met [ophalen en transformeren in Excel 2016 en Power Query voor Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel-gebruikers kunnen delen van query's en query's met Azure Data Catalog uit detecteren in Excel. Deze functionaliteit is beschikbaar voor gebruikers met Power BI Pro licenties.

## <a name="whats-new-for-december-2016"></a>Wat is er nieuw voor December 2016
Vanaf December 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) en [Modelbepalingen EU](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) compatibel.
*   Ondersteuning voor het bewerken van informatie van de gegevensbronverbinding. Eigenaren van asset en beheerders van Data Catalog kunnen u nu de verbindingsgegevens voor de geregistreerde gegevensbronnen bewerken zonder de gegevensbronnen opnieuw te registreren.
*   Ondersteuning voor Salesforce.com-gegevensbronnen. Gebruikers kunnen nu registreren en Salesforce-objecten te detecteren.


## <a name="whats-new-for-november-2016"></a>Wat is er nieuw voor November 2016
Vanaf November 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:
*   Azure Data Catalog is nu [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) en [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) compatibel.
*   Ondersteuning voor de handmatige registratie van ODBC-gegevensbronnen met de Data Catalog-portal en de REST-API.

## <a name="whats-new-for-september-2016"></a>Wat is er nieuw voor September 2016
Vanaf September 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor IBM DB2-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren DB2-databases, tabellen en weergaven.
* Ondersteuning voor Azure Cosmos DB-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren Cosmos-DB-databases en verzamelingen.
* Ondersteuning voor het aanpassen van de catalogusnaam in de portal voor Data Catalog. Catalogus-beheerders kunnen nu een tekst die wordt weergegeven in de titel van de portal, zoals de naam van de organisatie.

## <a name="whats-new-for-august-2016"></a>Wat is er nieuw voor augustus 2016
Vanaf augustus 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Verbeteringen voor de registratie van gegevensbronnen van SQL Server Master Data Services (MDS). Gebruikers kunnen nu opnemen voor profielen voor voorbeelden en gegevens bij het registreren van het hulpprogramma registratie Data Catalog voor gegevensbronnen met MDS-entiteiten.
* Ondersteuning voor de beheerder gedefinieerde organisatie opgeslagen zoekopdrachten. Als u een zoekopdracht opslaat in de portal voor Data Catalog, kunnen Data Catalog nu beheerders Sla zoekopdrachten voor persoonlijk gebruik of voor alle gebruikers van de catalogus. Opgeslagen zoekopdrachten organisatie worden gedeeld met alle gebruikers van de catalogus en gestandaardiseerde beginpunten kunnen opgeven voor detectie van gegevensbronnen.
* Updates voor de eigenschappenweergave in de portal voor Data Catalog. Eigenschappen van de asset alle gegevens worden nu weergegeven en beheerd in een enkele, formaat deelvenster voor een consistente en ervaring kunnen worden gedetecteerd.

## <a name="whats-new-for-july-2016"></a>Wat is er nieuw voor juli 2016
Vanaf juli 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor SQL Server Master Data Services (MDS)-gegevensbronnen. Gebruikers kunnen nu registreren en MDS-modellen en entiteiten te detecteren.
* Ondersteuning voor SQL Server opgeslagen procedures. Gebruikers kunnen nu worden geregistreerd en opgeslagen procedure-objecten in SQL Server-gegevensbronnen te detecteren.
* Ondersteuning voor extra talen in de Azure Data Catalog-portal en het hulpprogramma voor registratie, voor een totaal van 18 ondersteunde talen. De gebruikerservaring van Azure Data Catalog is gelokaliseerd op basis van het taalvoorkeuren instellen in Windows of in de webbrowser.
* Updates en verfijning voor de Data Catalog portal startpagina, met inbegrip van betere prestaties en een gestroomlijnde gebruikerservaring.

## <a name="whats-new-for-june-2016"></a>Wat is er nieuw voor juni 2016
Vanaf juni 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het formaat van kolommen in de lijstweergave wijzigen bij het detecteren van gegevensassets in de portal voor Data Catalog. Gebruikers kunnen nu afzonderlijke kolombreedtes langdurige asset metagegevens zoals labels en beschrijvingen gemakkelijker te lezen.
* Power Query voor Excel is toegevoegd aan het menu "Openen in" in de portal voor Data Catalog. Gebruikers kunnen nu ondersteunde gegevensbronnen openen in Excel 2016 of in Excel 2010 en Excel 2013 met de [Power Query voor Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) invoegtoepassing geïnstalleerd.
* Ondersteuning voor Azure Table Storage-gegevensbronnen. Gebruikers kunnen nu registreren en tabelobjecten in Azure Storage-gegevensbronnen te detecteren.

## <a name="whats-new-for-may-2016"></a>Wat is er nieuw voor mei 2016
Vanaf mei 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Een zakelijke woordenlijst waarmee beheerders van de catalogus voor het definiëren van zakelijke voorwaarden en hiërarchieën voor het maken van een algemene zakelijke woordenlijst. Gebruikers kunnen labelen geregistreerde gegevensassets met termen om te detecteren en begrijpen van de inhoud van de catalogus te vereenvoudigen. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md)  
* Verbeteringen in de Data Catalog zakelijke woordenlijst waarmee gebruikers kunnen meerdere woordenlijst van termen in één bewerking bijwerken. Gebruikers kunnen meerdere voorwaarden bewerken van de volgende velden selecteren:
  * Bovenliggende Term: Een nieuwe bovenliggende term voor de gebruiker kunt selecteren en alle geselecteerde voorwaarden zijn bijgewerkt voor onderliggende elementen van de geselecteerde bovenliggende term. Als de geselecteerde alle voorwaarden dezelfde bovenliggende activiteit hebben, wordt dat de bovenliggende wordt weergegeven in het tekstvak anders de termijn van de bovenliggende veld is ingesteld op leeg.   
  * Tags en belanghebbenden: gebruikers kunnen toevoegen en verwijderen van tags en belanghebbenden voor meerdere termen in met behulp van dezelfde ervaring als u meerdere gegevensassets labels.

> [!NOTE]
> De zakelijke woordenlijst is alleen beschikbaar in de Standard-editie van Azure Data Catalog. De editie Free biedt geen mogelijkheden voor geregeld tagging of een zakelijke woordenlijst.

## <a name="whats-new-for-march-2016"></a>Wat is er nieuw voor maart 2016
Vanaf maart 2016, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog: g:

* Een geconsolideerde REST-API-eindpunt voor programmatisch toegang tot de zoekfuncties en beheermogelijkheden van catalogus asset van de service Azure Data Catalog. Deze zoekopdracht API-eindpunt en catalogus API-eindpunt is afgeschaft en op 21 maart 2016 afgeschaft. Er zijn geen wijzigingen in de semantiek van de API. Alleen het eindpunt URI gewijzigd. Zie voor meer informatie de [Azure Data Catalog REST API Reference](https://msdn.microsoft.com/library/azure/mt267595.aspx). Zie voor voorbeelden van de API, [voorbeelden van Azure Data Catalog developer](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Wat is er nieuw voor februari 2016
Vanaf februari 2016 hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Een nieuw vernieuwde bronselectie optreden in de Azure Data Catalog hulpprogramma voor registratie. Het registratiehulpprogramma van gegevensbronnen is eenvoudiger kunt vinden en selecteren van de gegevensbronnen wordt ondersteund door Azure Data Catalog bijgewerkt.
* Ondersteuning voor 10 extra talen in de Azure Data Catalog-portal en het registratiehulpprogramma van gegevensbronnen. De ervaring voor de Azure Data Catalog is nu beschikbaar in het Duits, Spaans, Frans, Italiaans, Japans, Koreaans, Portugees (Brazilië), Russische, vereenvoudigd Chinees en traditioneel Chinees naast Engels. De gebruikerservaring van Azure Data Catalog is gelokaliseerd op basis van het taalvoorkeuren instellen in Windows of in de webbrowser van de gebruiker.
* Ondersteuning voor de geo-replicatie van gegevens van Azure Data Catalog voor zakelijke continuïteit en herstel na noodgevallen. Alle inhoud van de Azure Data Catalog, met inbegrip van gegevensbron metagegevens en crowdsourcing aantekeningen worden nu gerepliceerd tussen twee Azure-regio's zonder extra kosten aan klanten. Azure-regio's zijn vooraf gekoppeld ten minste 500 mijl uit elkaar liggen, en volgt u de toewijzing zoals beschreven in [zakelijke continuïteit en herstel na noodgevallen (BCDR): Azure-gebieden gekoppeld](../best-practices-availability-paired-regions.md).
* Ondersteuning voor het wijzigen van het Azure-abonnement gebruikt door Azure Data Catalog. Azure Data Catalog-beheerders kunnen de pagina instellingen in de portal voor Azure Data Catalog gebruiken om te selecteren van een ander Azure-abonnement ten behoeve van facturering.

## <a name="whats-new-for-january-2016"></a>Wat is er nieuw voor januari 2016
Vanaf januari 2016, hebben de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het handmatig extra gegevensbronnen te registreren. U kunt nu 'Handmatig bericht maken' in de portal voor Azure Data Catalog gebruiken of de REST-API van Azure Data Catalog gebruiken om te registreren van de volgende gegevensbronnen:
  * OData - functie, entiteitset en Entiteitscontainer
  * HTTP - bestand, eindpunt, rapport en de Site
  * Bestandssysteem - bestand
  * SharePoint - lijst
  * FTP - bestanden en mappen
  * SalesForce.com - Object
  * DB2 - tabel, weergave en Database
  * PostgreSQL - tabel, weergave en Database
* Ondersteuning voor 'Openen in SQL Server Data Tools' voor SQL Server (inclusief Azure SQL DB en Azure SQL Data Warehouse)-gegevensbronnen.  
* Ondersteuning voor het registreren en SAP HANA-weergaven en pakketten detecteren. U kunt registreren SAP HANA-gegevensbronnen met behulp van Azure Data Catalog gegevensbron hulpprogramma voor registratie en kan aantekeningen toevoegen aan en geregistreerde SAP HANA-gegevensbronnen met de Azure Data Catalog-portal detecteren.
* De mogelijkheid om vastmaken aan en losmaken van gegevensassets in de portal voor Azure Data Catalog. U kunt vastmaken gegevensassets zodat ze gemakkelijker te detecteren en te hergebruiken.
* Een nieuw vernieuwde startpagina in de portal voor Azure Data Catalog. De nieuwe introductiepagina bevat inzicht in de huidige activiteit van gebruikers - inclusief onlangs gepubliceerde activa, vastgemaakt activa is en opgeslagen zoekopdrachten- en inzicht in de activiteit in de catalogus als geheel.
* Ondersteuning voor permanente gebruikersinstellingen in de portal voor Azure Data Catalog. -Met inbegrip van raster of tegel weergave, het aantal resultaten per pagina en hits markeren in- of uitschakelen - instellingen voor gebruikerservaring blijven bestaan tussen gebruikerssessies.
* Azure Data Catalog is nu beschikbaar in twee nieuwe Azure-regio's. Klanten kunnen inrichten van de Azure Data Catalog in de regio Noord-Europa en Zuidoost-Azië's, naast het VS-Oost, VS-West, West-Europa en Australië-Oost. Zie voor meer informatie [Azure-gebieden](https://azure.microsoft.com/regions/).

> [!NOTE]
> 'Openen in SQL Server Data Tools' is vereist dat Visual Studio 2013 met Update 4 en SQL Server Tooling moet worden geïnstalleerd. Als u wilt installeren de nieuwste versie van SQL Server Data Tools, gaat u naar [Download SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Wat is er nieuw voor December 2015
De volgende mogelijkheden hebt vanaf December 2015 zijn toegevoegd aan Azure Data Catalog:

* Ondersteuning voor gegevens profielen voor gegevensbronnen voor Azure SQL Data Warehouse. Bij het registreren van Azure SQL Data Warehouse-tabellen en weergaven, kunnen gebruikers gegevens profiel metrische gegevens opnemen met de metagegevens die zijn opgehaald uit de gegevensbron kiezen.
* Ondersteuning voor het registreren en detectie van MySQL-objecten en -databases. Gebruikers kunnen zich registreren met behulp van Azure Data Catalog gegevensbron hulpprogramma voor registratie en kan aantekeningen toevoegen aan en geregistreerde MySQL gegevensbronnen met de Azure Data Catalog-portal detecteren van MySQL-gegevensbronnen.
* Ondersteuning voor het SPNEGO- en Windows-verificatie voor Teradata-gegevensbronnen. Wanneer u registreert Teradata-tabellen en weergaven, kunnen gebruikers kiezen verbinding maken met de Teradata SPNEGO en Windows, en LDAP en TD2 verificatie gebruikt.
* Ondersteuning voor Azure Data Lake Store-gegevensbronnen. Gebruikers kunnen nu registreren en gegevensbronnen voor Azure Data Lake Store met Azure Data Catalog detecteren.
* Ondersteuning voor het netwerk proxy-instellingen handmatig opgeven in de Azure Data Catalog hulpprogramma voor registratie. Gebruikers 'Proxy-instellingen wijzigen' van het hulpprogramma welkomstpagina kunnen selecteren en de proxy-adres en poort moet worden gebruikt door het hulpprogramma kunnen opgeven.

## <a name="whats-new-for-november-2015"></a>Wat is er nieuw voor November 2015
Vanaf November 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* De mogelijkheid om te bekijken en kopiëren van de verbindingsreeksen van binnen het Azure Data Catalog-portal voor SQL-Server (met inbegrip van Azure SQL Database) en Oracle-gegevensbronnen. Gebruikers kunnen de koppeling 'Verbindingsreeksen weergeven' in de verbindingsgegevens voor een SQL Server of Oracle-tabel, weergave of database voor een overzicht van de verbindingsreeksen gebruikt om verbinding met de gegevensbron te klikken. ADO.NET, ODBC, OLEDB en JDBC verbindingsreeksen zijn beschikbaar voor SQL Server-gegevensbronnen. ODBC en OLEDB-verbindingsreeksen zijn beschikbaar voor Oracle-gegevensbronnen.
* Ondersteuning voor het opnemen van gegevens profielen bij het registreren van Teradata-tabellen en weergaven.
* Ondersteuning voor 'Openen in Power BI Desktop' voor SQL Server (inclusief Azure SQL DB en Azure SQL Data Warehouse), SQL Server Analysis Services, Azure Storage en HDFS-bronnen.  
* Ondersteuning voor LDAP-verificatie voor Teradata-gegevensbronnen. Bij het registreren van de Teradata-tabellen en weergaven die gebruikers kunnen kiezen verbinding maken met de Teradata met behulp van LDAP, en TD2 verificatie.
* Ondersteuning voor 'Openen in Excel' voor Teradata-gegevensbronnen.
* Ondersteuning voor recente zoektermen op in de Azure Data Catalog-portal. Wanneer u zoekt in de portal, kunnen gebruikers van onlangs gebruikte zoektermen kiezen om te versnellen van de ervaring voor de detectie.
* Ondersteuning voor het voorbeeld voor Teradata-gegevensbronnen. Wanneer u registreert Teradata-tabellen en weergaven, kunnen gebruikers kiezen momentopname om records te nemen met de metagegevens die zijn opgehaald uit de gegevensbron.
* Ondersteuning voor 'Openen in Excel' voor gegevensbronnen voor Azure SQL Data Warehouse.
* Ondersteuning voor het definiëren en te bewerken op kolomniveau schema's voor handmatig geregistreerde gegevensassets. Na het handmatig maken van een gegevensasset via de portal voor Azure Data Catalog, kunnen gebruikers de kolomdefinities toevoegen in de eigenschappen voor asset van gegevens.
* Ondersteuning voor 'is'-query's bij het zoeken van Azure Data Catalog, zodat de detectie van geregistreerde gegevensassets die beschikken over specifieke metagegevens. Azure Data Catalog-querysyntaxis bevat nu:

| Querysyntaxis | Doel |
| --- | --- |
| `has:previews` |Zoeken naar gegevensassets die een voorbeeld |
| `has:documentation` |Zoeken naar gegevensassets waarvoor documentatie is opgegeven |
| `has:tableDataProfiles` |Zoeken naar gegevensassets met profielgegevens op tabelniveau gegevens |
| `has:columnsDataProfiles` |Zoeken naar gegevensassets met informatie over het profiel van de gegevens op kolomniveau |


> [!NOTE]
> 'Openen in Power BI Desktop' vereist een actuele versie van de Power BI Desktop-toepassing wordt geïnstalleerd. Als u problemen ondervindt of fouten bij het gebruik van deze functie, zorg ervoor dat u de nieuwste versie van Power BI Desktop van [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Wat is er nieuw voor oktober 2015
Vanaf oktober 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor versleuteling in rust van gegevens previews en gegevens profielen voor geregistreerde gegevensbronnen. Azure Data Catalog versleutelt transparant preview records en -gegevens profielen gegevensbronnen die zijn geregistreerd bij de service, zonder dat u hoeft voor sleutelbeheer door beheerders van de catalogus.
* Ondersteuning voor Teradata-gegevensbronnen. Gebruikers kunnen nu registreren en Teradata-tabellen en weergaven te detecteren.
* Ondersteuning voor on-premises Hive-gegevensbronnen. Gebruikers kunnen nu registreren en Hive-tabellen voor Apache Hive in Hadoop on-premises gegevensbronnen detecteren.
* Ondersteuning voor opgeslagen zoekopdrachten in de portal voor Azure Data Catalog. Gebruikers kunnen opslaan zoektermen en selecties eenvoudig Herhaal de vorige zoekopdrachten en definiëren van nuttige weergaven van de inhoud van de catalogus filteren. Gebruiker kan ook een opgeslagen zoekopdracht markeren als hun standaard-zoekactie. Wanneer een gebruiker op het zoekpictogram 'Vergrootglas' van de startpagina van Azure Data Catalog portal of van de pagina 'aan de slag', wordt de gebruiker die rechtstreeks naar de opgeslagen zoekopdracht die is gemarkeerd als standaard.
* Ondersteuning voor RTF-documentatie voor de geregistreerde gegevensassets en containers in de portal voor Azure Data Catalog. Gebruikers kunnen nu documentatie te verschaffen voor gegevensassets zoals tabellen, weergaven en rapporten en -containers zoals databases en -modellen voor scenario's waarbij labels en beschrijvingen niet voldoende zijn.
* Ondersteuning voor het handmatig registreren van bekende typen gegevensbronnen. Gebruikers kunnen de informatie van gegevensbron met de Azure Data Catalog-portal voor alle typen gegevensbronnen ondersteund door Azure Data Catalog handmatig invoeren.
* Ondersteuning voor het machtigen van Azure Active Directory-beveiligingsgroepen. Beheerders van de catalogus kunnen catalogus toegang tot beveiligingsgroepen, gebruikersaccounts, waardoor het gemakkelijker voor het beheren van toegang tot Azure Data Catalog inschakelen.
* Ondersteuning voor Hive-gegevensbronnen in Excel opent vanuit de Azure Data Catalog-portal.

> [!NOTE]
> Voor de huidige release wordt alleen Teradata TD2 verificatie ondersteund. Extra authenticatie mechanismen worden ondersteund in toekomstige versies.

> [!NOTE]
> Voor het gebruik van de functie 'Openen in Excel' voor Hive-gegevensbronnen, moeten op gebruikers het ODBC-stuurprogramma voor Hive hebben geïnstalleerd.

## <a name="whats-new-for-september-2015"></a>Wat is er nieuw voor September 2015
Vanaf September 2015, zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor het opnemen van gegevens profielen bij het registreren van gegevensbronnen Hive.
* Ondersteuning voor het detecteren van de catalogus-API, waardoor het gemakkelijker om toepassingen te integreren met Azure Data Catalog programmatisch.
* Een nieuwe 'aan de slag' gegevensbron detectie ervaring in de portal voor Azure Data Catalog. Wanneer gebruikers op de pagina 'detecteren' van de portal voor Azure Data Catalog invoeren zonder een zoekterm invoert, worden ze weergegeven met een overzicht van de inhoud van de catalogus met inbegrip van de meest gebruikte labels, deskundigen, gegevensbrontypen en objecttypen.
* Ondersteuning voor het registreren en detectie van Azure SQL Data Warehouse-objecten en -databases. Zie voor meer informatie over Azure SQL Data Warehouse [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Ondersteuning voor het registreren en detectie van SQL Server Analysis Services-modellen en SQL Server Reporting Services-servers als containers. Bij het registreren van SSAS en SSRS objecten maakt Azure Data Catalog een vermelding voor de SSAS-model en de SSRS-server, en voor de rapporten en andere objecten. De containers kunnen worden gedetecteerd en aangetekend met de Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een model of de server naast zoeken en filteren van de inhoud van de catalogus.
* Ondersteuning voor het registreren en het detecteren van SQL Server Analysis Services-objecten via HTTP/HTTPS. Gebruikers kunnen nu verbinding maken met de SSAS-servers met een URL (zoals https://servername/olap/msmdpump.dll) in plaats van een servernaam en basisverificatie en anonieme verbindingen naast Windows-verificatie kunnen gebruiken. Zie voor meer informatie over HTTP/HTTPS-verbindingen met SSAS [HTTP-toegang tot Analysis Services configureren](https://msdn.microsoft.com/library/gg492140.aspx).
* Ondersteuning voor de gegevensbronnen Hive in HDInsight. Gebruikers kunnen nu worden geregistreerd en het detecteren van Hive-tabellen voor Apache Hive in Hadoop op HDInsight-gegevensbronnen. Zie voor meer informatie over Hive in HDInsight, het [HDInsight-documentatiecentrum](../hdinsight/hadoop/hdinsight-use-hive.md).
* Ondersteuning voor het registreren en Oracle-databases en clusters van HDFS als containers detecteren. Bij het registreren van Oracle-tabellen en weergaven of HDFS, wordt een vermelding voor de database, tabellen en weergaven gemaakt in Azure Data Catalog. De database kan worden gedetecteerd en van aantekeningen voorzien met het Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een database of het cluster naast zoeken en filteren van de inhoud van de catalogus.
* Ondersteuning voor onbekende typen gegevensbronnen handmatig registreren. Gebruikers kunnen de informatie van gegevensbron met behulp van de portal voor Azure Data Catalog, zodat de gegevensbronnen niet expliciet worden ondersteund door het registratiehulpprogramma van gegevensbronnen kunnen worden aangetekend en gedetecteerd handmatig invoeren.
* Ondersteuning voor het registreren en detectie van SQL Server-databases als containers. Bij het registreren van SQL Server-tabellen en weergaven, wordt een vermelding voor de database, tabellen en weergaven gemaakt in Azure Data Catalog. De database kan worden gedetecteerd en van aantekeningen voorzien met het Azure Data Catalog-portal. Gebruikers kunnen ook zoeken en filteren van de inhoud van een database naast zoeken en filteren van de inhoud van de catalogus.

> [!NOTE]
> SSAS en SSRS-objecten die geregistreerd vóór de release van September 18 zijn moeten opnieuw worden geregistreerd met het registratiehulpprogramma van gegevensbronnen voordat de vermelding model of de server is toegevoegd aan de catalogus. Opnieuw registreren van een gegevensbron heeft geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de portal voor Azure Data Catalog.

> [!NOTE]
> Oracle-tabellen en weergaven en HDFS-bestanden en mappen die geregistreerd vóór de release van September 11 zijn moeten opnieuw worden geregistreerd met behulp van het registratiehulpprogramma van gegevensbronnen voordat de vermelding van de database of het cluster wordt toegevoegd aan de catalogus. Opnieuw registreren van een gegevensbron heeft geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de portal voor Azure Data Catalog.

> [!NOTE]
> SQL Server-tabellen en weergaven die geregistreerd vóór de release van September 4 zijn moeten worden opnieuw geregistreerd met het registratiehulpprogramma van gegevensbronnen voordat u de databasevermelding is toegevoegd aan de catalogus. Opnieuw registreren van een gegevensbron heeft geen invloed op alle aantekeningen die zijn toegevoegd door gebruikers in de portal voor Azure Data Catalog.

## <a name="whats-new-for-august-2015"></a>Wat is er nieuw voor augustus 2015
Vanaf augustus 2015 geldt zijn de volgende mogelijkheden toegevoegd aan Azure Data Catalog:

* Ondersteuning voor profileren van gegevens van SQL Server en Oracle-gegevensbronnen. Bij het registreren van SQL Server en Oracle-tabellen en weergaven, kunnen gebruikers kiezen om op te nemen informatie over het profiel van gegevens voor de objecten die wordt geregistreerd. Het profiel van de gegevens bevat op objectniveau en op kolomniveau statistische gegevens.
* Ondersteuning voor Hadoop HDFS-gegevensbronnen. Gebruikers kunnen nu registreren en detecteren HDFS-bestanden en mappen.
* Ondersteuning voor het bieden van informatie van het toegangsverzoek voor geregistreerde gegevensbronnen. Voor een geregistreerde gegevensasset, kunnen gebruikers nu instructies voor het aanvragen van toegang, waaronder e-koppelingen of URL's, eenvoudig kunt integreren met bestaande hulpprogramma's en processen opgeven.
* De knopinfo voor labels en experts om eenvoudiger te ontdekken wat gebruikers welke metagegevens voor geregistreerde gegevensassets hebt opgegeven.
* We hebt een nieuwe knop voor 'Gebruiker' en het menu aan de bovenste navigatiebalk toegevoegd. Dit menu krijgt de gebruiker het account dat wordt gebruikt voor aanmelding bij Azure Data Catalog en als u wilt afmelden, indien gewenst. Dit menu worden ook de catalogusnaam, wat nuttig voor ontwikkelaars met behulp van de REST-API van Azure Data Catalog is weergegeven.
* Standard-editie alleen: Bij het toevoegen van eigenaars aan gegevensassets, Azure Data Catalog ondersteunt nu de gebruikersaccounts en beveiligingsgroepen als eigenaars. Om een beveiligingsgroep als eigenaar van de geselecteerde gegevensassets toevoegt, kunt u de weergavenaam van de groep of van de groep UPN-e-mailadres, indien van toepassing.
* Ondersteuning voor Azure Blob Storage-gegevensbronnen. Gebruikers kunnen nu registreren en Azure Storage-blobs en mappen te detecteren.

