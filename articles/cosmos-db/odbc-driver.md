---
title: Verbinding maken met Azure Cosmos DB met BI-hulpprogramma's voor webanalyse | Microsoft Docs
description: Informatie over het gebruik van het Azure Cosmos DB ODBC-stuurprogramma voor het maken van tabellen en weergaven zodat genormaliseerde gegevens kunnen worden weergegeven in de BI- en gegevens analytics-software.
keywords: ODBC, odbc-stuurprogramma
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 01/16/2018
ms.author: mimig
ms.openlocfilehash: 3892f698ec2b0b45f71dc38491687897559821ba
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>Verbinding maken met Azure Cosmos DB met BI analytics hulpprogramma van het ODBC-stuurprogramma

Het Azure Cosmos DB ODBC-stuurprogramma kunt u verbinding maken met Azure Cosmos DB met behulp van hulpprogramma's voor webanalyse BI zoals SQL Server Integration Services-, Power BI Desktop- en Tableau, zodat u kunt analyseren en visualisaties van uw Azure DB die Cosmos-gegevens in deze oplossingen maken.

Het Azure Cosmos DB ODBC-stuurprogramma ODBC 3.8 compatibel is en de syntaxis van de ANSI SQL-92 ondersteunt. Het stuurprogramma biedt uitgebreide functies waarmee u gegevens in Azure Cosmos DB renormalize. Met het stuurprogramma, kunt u gegevens in Azure Cosmos DB als tabellen en weergaven weergeven. Het stuurprogramma kunt u SQL-bewerkingen op basis van de tabellen en weergaven inclusief group by-query's, plaatst, updates, en wordt verwijderd uit te voeren.

## <a name="why-do-i-need-to-normalize-my-data"></a>Waarom moet ik mijn gegevens normaliseren?
Azure Cosmos-database is een database zonder schema zodat snelle ontwikkeling van apps kunnen door het inschakelen van toepassingen op hun gegevensmodel op elk gewenst moment herhalen en ze niet aan een strikte schema te beperken. Eén Azure DB die Cosmos-database kan de JSON-documenten van verschillende structuren bevatten. Dit is ideaal voor snelle ontwikkeling van toepassingen, maar als u wilt analyseren en rapporten van uw gegevens met behulp van gegevensanalyse en BI-hulpprogramma's maken, moet de gegevens vaak worden "afgevlakt" en voldoen aan een specifiek schema.

Dit is waar het ODBC-stuurprogramma wordt geleverd. U kunt nu renormalized gegevens in Azure Cosmos DB in tabellen en weergaven aanpassen aan uw data-analyse- en behoeften met behulp van het ODBC-stuurprogramma. De renormalized schema's hebben geen invloed op de onderliggende gegevens en ontwikkelaars om te voldoen aan deze niet wilt beperken, ze gewoon Hiermee kunt u gebruikmaken van ODBC-hulpprogramma's voor toegang tot de gegevens. Nu wordt uw Azure DB die Cosmos-database niet alleen een favoriet voor uw ontwikkelteam, maar uw gegevensanalisten wordt de evaluatieversie te.

Nu kunt aan de slag met het ODBC-stuurprogramma.

## <a id="install"></a>Stap 1: Installeer het Azure Cosmos DB ODBC-stuurprogramma

1. Download de stuurprogramma's voor uw omgeving:

    | Installatieprogramma | Ondersteunde besturingssystemen| 
    |---|---| 
    |[Microsoft Azure DB Cosmos ODBC-64-bit.msi](https://aka.ms/documentdb-odbc-64x64) voor 64-bits Windows| 64-bits versies van Windows 8.1 of hoger, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 en Windows Server 2008 R2.| 
    |[Microsoft Azure DB Cosmos ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) voor 32-bits op 64-bits Windows| 64-bits versies van Windows 8.1 of hoger, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 en Windows Server 2003.| 
    |[Microsoft Azure DB Cosmos ODBC 32 bit.msi](https://aka.ms/documentdb-odbc-32x32) voor 32-bits Windows|32-bits versies van Windows 8.1 of hoger, Windows 8, Windows 7, Windows XP en Windows Vista.|

    Voer het msi-bestand lokaal op te starten de **Azure Cosmos DB ODBC-stuurprogramma Wizard installatie van Microsoft**. 
2. Voltooi de wizard installeren met behulp van de invoer voor het installeren van het ODBC-stuurprogramma.
3. Open de **ODBC-gegevensbron beheerder** app op uw computer, u kunt dit doen door te typen **ODBC-gegevensbronnen** in het zoekvak. 
    Kunt u bevestigen dat het stuurprogramma is geïnstalleerd door te klikken op de **stuurprogramma's** tabblad en ervoor te zorgen **Microsoft Azure Cosmos DB ODBC-stuurprogramma** wordt vermeld.

    ![Azure Cosmos DB-gegevensbron](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>Stap 2: Verbinding maken met uw Azure DB die Cosmos-database

1. Na [installeert het Azure Cosmos DB ODBC-stuurprogramma](#install), in de **ODBC Data Source Administrator** venster, klikt u op **toevoegen**. U kunt een gebruiker of systeem-DSN maken. In dit voorbeeld maakt u een gebruikers-DSN.
2. In de **nieuwe gegevensbron maken** Selecteer **DB ODBC-stuurprogramma van Microsoft Azure Cosmos**, en klik vervolgens op **voltooien**.
3. In de **Azure Cosmos DB ODBC-stuurprogramma SDN Setup** venster vullen in het volgende: 

    ![Azure Cosmos DB ODBC-stuurprogramma DSN instellingen venster](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Naam van de gegevensbron**: uw eigen beschrijvende naam voor de ODBC-DSN. Deze naam uniek is voor uw Azure DB die Cosmos-account, dus op de juiste wijze name als u meerdere accounts hebt.
    - **Beschrijving**: een korte beschrijving van de gegevensbron.
    - **Host**: URI voor uw Azure DB die Cosmos-account. U kunt dit ophalen via de pagina Azure Cosmos DB sleutels in de Azure-portal, zoals wordt weergegeven in de volgende schermafbeelding. 
    - **Toegangstoets**: de primaire of secundaire, alleen-lezen of alleen-lezen-sleutel van de sleutels van Azure Cosmos DB pagina in de Azure portal, zoals wordt weergegeven in de volgende schermafbeelding. Het is raadzaam om dat de sleutel alleen-lezen als u de DSN-naam wordt gebruikt voor alleen-lezen gegevensverwerking en rapportage.
    ![Azure DB-sleutels Cosmos-pagina](./media/odbc-driver/odbc-driver-keys.png)
    - **Versleutelen van de toegangssleutel voor**: Selecteer de beste keuze op basis van de gebruikers van deze machine. 
4. Klik op de **Test** knop om te controleren of u kunt verbinding maken met uw Azure DB die Cosmos-account. 
5. Klik op **geavanceerde opties** en stel de volgende waarden:
    - **Query uitvoeren op consistentie**: Selecteer de [consistentieniveau](consistency-levels.md) voor uw doeleinden. De standaardwaarde is de sessie.
    - **Aantal nieuwe pogingen**: Voer het aantal keer opnieuw proberen aan een als de eerste aanvraag niet wordt voltooid vanwege een beperking van de service.
    - **Schemabestand**: U hebt een aantal opties hier.
        - Het stuurprogramma scant standaard als u deze vermelding (leeg) is de eerste paginagegevens voor alle verzamelingen om te bepalen van het schema van elke verzameling. Dit staat bekend als de toewijzing van de verzameling. Zonder een schemabestand dat is gedefinieerd, wordt het stuurprogramma heeft de scan uitvoeren voor elke sessie stuurprogramma en kan leiden tot een hogere opstarten van een toepassing met behulp van de DSN-naam. Het is raadzaam dat u altijd een schemabestand voor een DSN koppelen.
        - Als u al een schemabestand (mogelijk één die u hebt gemaakt met de [Schema-Editor](#schema-editor)), klikt u op **Bladeren**, gaat u naar het bestand, klik op **opslaan**, en klik vervolgens op **OK**.
        - Als u een nieuw schema maken wilt, klikt u op **OK**, en klik vervolgens op **Schema-Editor** in het hoofdvenster. Gaat u verder met de [Schema-Editor](#schema-editor) informatie. Bij het maken van het nieuwe schemabestand, houd er rekening mee wilt teruggaan naar de **geavanceerde opties** venster de zojuist gemaakte schemabestand opnemen.

6. Zodra u voltooien en sluit de **Azure Cosmos DB ODBC-stuurprogramma DSN Setup** venster de nieuwe gebruiker DSN-naam is toegevoegd aan de gebruikers-DSN-tabblad.

    ![Nieuwe Azure Cosmos DB ODBC DSN-naam op het tabblad gebruiker DSN](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>Stap 3: Een schemadefinitie waarin de toewijzingsmethode verzameling maken

Er zijn twee soorten steekproeven methoden die u kunt gebruiken: **verzameling toewijzing** of **tabel scheidingstekens**. Een sessie steekproeven kan gebruikmaken van beide methoden steekproeven, maar elke verzameling kunt alleen een specifieke methode gebruiken. De volgende stappen uit maken een schema voor de gegevens in een of meer verzamelingen met behulp van de verzameling toewijzingsmethode. Deze methode haalt de gegevens op de pagina van een verzameling om te bepalen van de structuur van de gegevens. Deze omgezet een verzameling aan een tabel aan de kant van de ODBC. Deze methode is snel en efficiënt wanneer de gegevens in een verzameling homogene. Als een verzameling heterogene type gegevens bevat, raden wij aan u de [tabel scheidingstekens toewijzingsmethode](#table-mapping) aangezien deze biedt een meer robuuste methode om te bepalen van de gegevensstructuren in de verzameling. 

1. Na het voltooien van stap 1 en 4 in [verbinding maken met de database van uw Azure Cosmos DB](#connect), klikt u op **Schema-Editor** in de **Azure Cosmos DB ODBC-stuurprogramma DSN Setup** venster.

    ![Knop voor schema-editor in het venster Azure Cosmos DB ODBC-stuurprogramma DSN Setup](./media/odbc-driver/odbc-driver-schema-editor.png)
2. In de **Schema-Editor** venster, klikt u op **nieuw**.
    De **Schema genereren** venster geeft alle verzamelingen in de Azure DB die Cosmos-account. 
3. Selecteer een of meer verzamelingen een steekproef nemen uit en klik vervolgens op **voorbeeld**. 
4. In de **ontwerpweergave** tabblad, de database, schema en tabel worden weergegeven. De scan weergegeven in de tabelweergave de set eigenschappen die zijn gekoppeld aan de kolomnamen (SQL-naam, de naam van gegevensbron, enzovoort).
    Voor elke kolom, kunt u de naam van de kolom SQL, de SQL-type, de lengte van de SQL (indien van toepassing), schalen (indien van toepassing), Precision (indien van toepassing) en null-waarden bevatten.
    - U kunt instellen **kolom verbergen** naar **true** als u wilt uitsluiten van die kolom van de queryresultaten. Kolommen kolom verbergen gemarkeerd = true niet worden geretourneerd voor de selectie en projectie, hoewel ze nog steeds deel van het schema uitmaken. U kunt bijvoorbeeld alle Azure Cosmos DB vereiste eigenschappen beginnen met '_' verbergen.
    - De **id** kolom is het enige veld dat als deze wordt gebruikt als de primaire sleutel in het genormaliseerde schema kan niet worden verborgen. 
5. Wanneer u klaar bent met het definiëren van het schema, klikt u op **bestand** | **opslaan**, navigeer naar de map op te slaan van het schema en klik vervolgens op **opslaan**.

    Als in de toekomst u dit schema gebruiken met een DSN wilt, opent u het venster Azure Cosmos DB ODBC-stuurprogramma DSN Setup (via de ODBC-gegevensbronbeheer), klikt u op Geavanceerde opties en navigeert u vervolgens in het vak schemabestand aan het schema opgeslagen. Opslaan van een schemabestand naar een bestaande DSN-naam, wijzigt de DSN-verbinding met bereik tot de gegevens en de structuur die is gedefinieerd door het schema.

## <a id="table-mapping"></a>Stap 4: Maken met behulp van de tabel-scheidingstekens schemadefinitie toewijzingsmethode

Er zijn twee soorten steekproeven methoden die u kunt gebruiken: **verzameling toewijzing** of **tabel scheidingstekens**. Een sessie steekproeven kan gebruikmaken van beide methoden steekproeven, maar elke verzameling kunt alleen een specifieke methode gebruiken. 

De volgende stappen maakt u een schema voor de gegevens in een of meer verzamelingen met behulp van de **tabel scheidingstekens** toewijzingsmethode. Het is raadzaam dat u deze methode gebruiken wanneer uw verzamelingen heterogene type gegevens bevatten. U kunt deze methode gebruiken als bereik voor de meting aan een set kenmerken en de bijbehorende waarden. Bijvoorbeeld, als een document bevat een eigenschap 'Type', u kunt het bereik van de meting met de waarden van deze eigenschap. Het eindresultaat van de steekproeven is een set met tabellen voor elk van de waarden voor het Type dat u hebt opgegeven. Typ bijvoorbeeld = auto produceert een auto-tabel bij het Type = vlak geeft als resultaat een tabel vlak.

1. Na het voltooien van stap 1 en 4 in [verbinding maken met de database van uw Azure Cosmos DB](#connect), klikt u op **Schema-Editor** in het venster Azure Cosmos DB ODBC-stuurprogramma DSN Setup.
2. In de **Schema-Editor** venster, klikt u op **nieuw**.
    De **Schema genereren** venster geeft alle verzamelingen in de Azure DB die Cosmos-account. 
3. Selecteer een verzameling op de **voorbeeldweergave** tabblad, in de **toewijzing definitie** kolom voor de verzameling, klikt u op **bewerken**. Klik in de **toewijzing definitie** Selecteer **tabel scheidingstekens** methode. Ga daarna als volgt te werk:

    a. In de **kenmerken** typt u de naam van een eigenschap van het scheidingsteken. Dit is een eigenschap in het document dat u wilt de steekproeven naar bijvoorbeeld stad bereik en druk op enter. 

    b. Als u wilt dat alleen als bereik voor de steekproeven aan bepaalde waarden voor het kenmerk dat u zojuist hebt ingevoerd, selecteert u het kenmerk in het selectievak en voer een waarde in de **waarde** vak bijvoorbeeld Seattle en druk op enter. U kunt meerdere waarden toevoegen voor kenmerken blijven. NET Controleer of het juiste kenmerk is geselecteerd wanneer u waarden invoert.

    Als u bijvoorbeeld een **kenmerken** waarde van plaats en u wilt beperken van de tabel zodanig dat alleen rijen met een waarde van de plaats van New York en Dubai, zou u plaats in het vak kenmerken en New York en vervolgens Dubai in de **Waarden** vak.
4. Klik op **OK**. 
5. Na het voltooien van de definities van de toewijzing voor de verzamelingen die u wilt kopiëren, in de **Schema-Editor** venster, klikt u op **voorbeeld**.
     Voor elke kolom, kunt u de naam van de kolom SQL, de SQL-type, de lengte van de SQL (indien van toepassing), schalen (indien van toepassing), Precision (indien van toepassing) en null-waarden bevatten.
    - U kunt instellen **kolom verbergen** naar **true** als u wilt uitsluiten van die kolom van de queryresultaten. Kolommen kolom verbergen gemarkeerd = true niet worden geretourneerd voor de selectie en projectie, hoewel ze nog steeds deel van het schema uitmaken. U kunt bijvoorbeeld alle Azure Cosmos DB vereist Systeemeigenschappen beginnen met '_' verbergen.
    - De **id** kolom is het enige veld dat als deze wordt gebruikt als de primaire sleutel in het genormaliseerde schema kan niet worden verborgen. 
6. Wanneer u klaar bent met het definiëren van het schema, klikt u op **bestand** | **opslaan**, navigeer naar de map op te slaan van het schema en klik vervolgens op **opslaan**.
7. Terug in de **Azure Cosmos DB ODBC-stuurprogramma DSN Setup** venster, klikt u op ** Geavanceerde opties **. Klik in de **schemabestand** vak, gaat u naar het schemabestand opgeslagen en klikt u op **OK**. Klik op **OK** om opnieuw op te slaan de DSN-naam. Dit bespaart het schema dat u hebt gemaakt naar de DSN. 

## <a name="optional-creating-views"></a>(Optioneel) Maken van weergaven
U kunt definiëren en weergaven maken als onderdeel van het proces steekproeven. Deze weergaven zijn equivalent aan SQL-weergaven. Ze zijn alleen-lezen en bereik zijn de selecties en projecties van de SQL Azure Cosmos DB gedefinieerd. 

Een weergave te maken voor uw gegevens de **Schema-Editor** venster in de **definities** kolom, klikt u op **toevoegen** op de rij van de verzameling met het volgende voorbeeld. Klik in de **definities** venster de volgende handelingen uit:
1. Klik op **nieuw**, voer een naam voor de weergave, bijvoorbeeld EmployeesfromSeattleView en klik vervolgens op **OK**.
2. In de **weergave bewerken** venster een Azure Cosmos DB-query invoeren. Dit moet een Azure Cosmos DB SQL-query, bijvoorbeeld`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`, en klik vervolgens op **OK**.

Als u wilt, kunt u een groot aantal weergaven maken. Wanneer u klaar bent voor het definiëren van de weergaven die u kunt vervolgens steekproef nemen de gegevens. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>Stap 5: Uw gegevens weergeven in de BI-tools zoals Power BI Desktop

U kunt uw nieuwe DSN DocumentADB verbinden met een ODBC-hulpprogramma's: deze stap gewoon laat zien hoe u verbinding maken met Power BI Desktop- en maken van een visualisatie in Power BI.

1. Power BI Desktop openen.
2. Klik op **gegevens ophalen**.
3. In de **gegevens ophalen** venster, klikt u op **andere** | **ODBC** | **Connect**.
4. In de **van ODBC** venster, selecteer de gegevensbron waarmee u hebt gemaakt en klik op **OK**. U kunt laten de **geavanceerde opties** vermeldingen leeg.
5. In de **toegang krijgen tot een gegevensbron met behulp van een ODBC-stuurprogramma** Selecteer **standaard of aangepast** en klik vervolgens op **Connect**. U hoeft niet te nemen de **referentie-eigenschappen van een verbindingsreeks**.
6. In de **Navigator** venster in het linkerdeelvenster Vouw de database, het schema uit en selecteer vervolgens de tabel. Het deelvenster met resultaten bevat de gegevens met het schema dat u hebt gemaakt.
7. Schakel het selectievakje in voor de tabelnaam om de gegevens in Power BI desktop, en klik vervolgens op **Load**.
8. In Power BI Desktop uiterst links, selecteer het tabblad gegevens ![Tabblad gegevens in Power BI Desktop](./media/odbc-driver/odbc-driver-data-tab.png) om te bevestigen dat de gegevens zijn geïmporteerd.
9. U kunt nu maken gebruik van Power BI door te klikken op het tabblad rapport visuele elementen ![tabblad van het rapport in Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png), te klikken op **nieuwe Visual**, en vervolgens aanpassen van uw tegel. Zie voor meer informatie over het maken van visualisaties in Power BI Desktop [visualisatietypen in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Problemen oplossen

Als u de volgende fout ontvangt, controleert de **Host** en **toegangssleutel** waarden die u hebt gekopieerd in de Azure portal [stap 2](#connect) juist zijn en probeer het vervolgens opnieuw. Gebruik de knoppen Kopieer rechts van de **Host** en **toegangssleutel** waarden in de Azure-portal voor het kopiëren van de beschikbare waarden-fout.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Cosmos DB, [Welkom bij Azure Cosmos DB](introduction.md).
