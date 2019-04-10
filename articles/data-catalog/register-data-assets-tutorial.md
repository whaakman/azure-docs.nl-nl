---
title: Gegevensassets registreren in Azure Data Catalog
description: Gegevensassets registreren in uw Azure Data Catalog
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: tutorial
ms.date: 04/08/2019
ms.openlocfilehash: e89bd4806e2bb2369c100e948be51dcf32f2e123
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59362373"
---
# <a name="tutorial-register-data-assets-in-azure-data-catalog"></a>Zelfstudie: Gegevensassets registreren in Azure Data Catalog

In deze zelfstudie kunt u het hulpprogramma voor registratie registreert gegevensassets uit de Azure SQL database-voorbeeld met de catalogus. Registratie is het proces waarbij belangrijke structurele metagegevens, zoals namen, typen en locaties, worden geëxtraheerd uit de gegevensbron en bijbehorende assets, en worden gekopieerd naar de catalogus. De gegevensbronnen en gegevensassets blijven waar ze zijn, maar de metagegevens worden door de catalogus gebruikt om de gegevensbronnen en gegevensassets eenvoudiger te detecteren en te begrijpen.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Gegevensassets registreren 
> * Gegevensassets zoeken
> * Aantekeningen toevoegen aan gegevensassets
> * Verbinding maken met gegevensassets
> * Gegevensassets beheren
> * Gegevensassets verwijderen

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen, moet u uitvoeren de [snelstartgids](register-data-assets-tutorial.md).

* Een [Microsoft Azure](https://azure.microsoft.com/) abonnement.
* U moet beschikken over uw eigen [Azure Active Directory-tenant](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

Als u Data Catalog instelt, moet u de eigenaar of mede-eigenaar van een Azure-abonnement zijn.

## <a name="register-data-assets"></a>Gegevensassets registreren

### <a name="register-a-data-source"></a>Een gegevensbron registreren

Registreert u gegevensassets (tabellen) uit een [Azure SQL-database voor klantorders](../sql-database/sql-database-single-database-get-started.md), maar u kunt een andere ondersteunde gegevensbron gebruiken als u liever wilt werken met gegevens die vertrouwd en relevant zijn voor uw rol. Zie voor een lijst met ondersteunde gegevensbronnen [Ondersteunde gegevensbronnen](data-catalog-dsr.md).

De naam van de Azure SQL-database we in deze zelfstudie gebruiken is *RLSTest*.

U kunt nu gegevensassets uit de Azure SQL-database voor klantorders registreren met behulp van Azure Data Catalog.

1. Ga naar de [startpagina van Azure Data Catalog](http://azuredatacatalog.com) en selecteer **gegevens publiceren**.

   ![Azure Data Catalog - de knop Gegevens publiceren](media/register-data-assets-tutorial/data-catalog-publish-data.png)

2. Selecteer **toepassing starten** als u wilt downloaden, installeren en uitvoeren van het hulpprogramma voor registratie op uw computer.

   ![Azure Data Catalog - de knop Starten](media/register-data-assets-tutorial/data-catalog-launch-application.png)

3. Op de **Welkom** weergeeft, schakelt **aanmelden** en voer uw referenties.

    ![Azure Data Catalog - Welkomstpagina](media/register-data-assets-tutorial/data-catalog-welcome-dialog.png)

4. Op de **Microsoft Azure Data Catalog** weergeeft, schakelt **SQL Server** en **volgende**.

    ![Azure Data Catalog - gegevensbronnen](media/register-data-assets-tutorial/data-catalog-data-sources.png)

5. Voer de verbindingseigenschappen van SQL Server voor uw Azure SQL-database voor klantorders en selecteer **CONNECT**.

   ![Azure Data Catalog - SQL Server-verbindingsinstellingen](media/register-data-assets-tutorial/data-catalog-sql-server-connection.png)

6. Registreer de metagegevens van uw gegevensasset. In dit voorbeeld registreert u **Product** objecten uit de naamruimte van Azure SQL database-voorbeeld:

    1. In de **serverhiërarchie** structuur, vouw uw Azure SQL-database voor klantorders en selecteert u **SalesLT**.

    2. Selecteer **Product**, **ProductCategory**, **ProductDescription**, en **productmodelbestand** met behulp van Ctrl + selecteren.

    3. Selecteer de **pijl verplaatsen geselecteerd** (**>**). Met deze actie worden alle geselecteerde objecten verplaatst naar de lijst **Te registreren objecten**.

          ![Zelfstudie voor Azure Data Catalog - bladeren door objecten en objecten selecteren](media/register-data-assets-tutorial/data-catalog-server-hierarchy.png)

    4. Selecteer **Preview opnemen** om een snapshotvoorbeeld van de gegevens op te nemen. De snapshot bevat maximaal 20 records uit elke tabel en wordt gekopieerd naar de catalogus.

    5. Selecteer **Gegevensprofiel opnemen** om een snapshot van de objectstatistieken op te nemen voor het gegevensprofiel (bijvoorbeeld: minimale, maximale en gemiddelde waarden voor een kolom, het aantal rijen).

    6. In de **tags toevoegen** veld **verkoop, product, azure sql**. Met deze actie worden zoektags voor deze gegevensassets toegevoegd. Tags zijn voor gebruikers een uitstekende manier om een geregistreerde gegevensbron te zoeken.

    7. Geef de naam op van een **expert** met betrekking tot deze gegevens (optioneel).

          ![Zelfstudie voor Azure Data Catalog - te registreren objecten](media/register-data-assets-tutorial/data-catalog-objects-register.png)

    8. Selecteer **REGISTREREN**. Azure Data Catalog registreert uw geselecteerde objecten. In deze oefening worden de geselecteerde objecten uit uw Azure SQL-database voor klantorders geregistreerd. Het hulpprogramma voor registratie extraheert metagegevens uit de gegevensasset en kopieert deze gegevens naar de Azure Data Catalog-catalogus. De gegevens blijven waar deze momenteel blijft. Gegevens blijven onder het besturingselement van de beheerders en het beleid van de herkomst van systeem.

          ![Azure Data Catalog - geregistreerde objecten](media/register-data-assets-tutorial/data-catalog-registered-objects.png)

    9. Uw geregistreerde gegevensbronobjecten Selecteer **Portal weergeven**. In de portal voor Azure Data Catalog, bevestig dat u alle vier de tabellen en de database in de rasterweergave ziet (Controleer of de zoekbalk wissen is).

        ![Objecten in de Azure Data Catalog-portal](media/register-data-assets-tutorial/data-catalog-view-portal.png)

In deze oefening hebt u objecten uit de Azure SQL-database voor klantorders geregistreerd, zodat ze kunnen eenvoudig worden gevonden door gebruikers in uw organisatie.

In de volgende oefening leert u hoe u geregistreerde gegevensassets kunt detecteren.

## <a name="discover-data-assets"></a>Gegevensassets detecteren

Voor detectie in Azure Data Catalog worden twee primaire mechanismen gebruikt: zoeken en filteren.

Zoeken is zowel intuïtief als krachtig. Standaard worden de zoektermen vergeleken met een eigenschap in de catalogus, met inbegrip van door de gebruiker gemaakte aantekeningen.

Filteren is bedoeld als aanvulling op het zoeken. U kunt specifieke kenmerken, zoals experts, gegevensbrontype, objecttype en tags, selecteren om overeenkomende gegevensassets weer te geven en om uw zoekresultaten te beperken tot overeenkomende assets.

Met behulp van een combinatie van zoeken en filteren, kunt u snel de gegevensbronnen die zijn geregistreerd bij Azure Data Catalog navigeren.

In deze oefening gebruikt u de Azure Data Catalog-portal voor het detecteren van de gegevensassets die u in de vorige stap hebt geregistreerd. Zie [Verwijzing naar de zoeksyntaxis van Data Catalog](/rest/api/datacatalog/#search-syntax-reference) voor meer informatie over de zoeksyntaxis.

Hieronder volgen enkele voorbeelden van het detecteren van gegevensassets in de catalogus.  

### <a name="discover-data-assets-with-basic-search"></a>Gegevensassets met een basiszoekopdracht detecteren

Met een basiszoekopdracht kunt u in een catalogus zoeken met behulp van een of meer zoektermen. De resultaten die worden opgehaald, zijn assets die wat betreft een willekeurige eigenschap overeenkomen met een of meer van de opgegeven voorwaarden.

1. Selecteer **Start** in de Azure Data Catalog-portal. Als u de webbrowser hebt gesloten, gaat u naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com).

2. Typ `product` in het zoekvak en druk op **ENTER**.

    ![Azure Data Catalog - basiszoekopdracht naar tekst](media/register-data-assets-tutorial/data-catalog-basic-text-search.png)

3. Bevestig dat u alle vier de tabellen en de database in de resultaten ziet. U kunt schakelen tussen **rasterweergave** en **lijstweergave** door knoppen te selecteren op de werkbalk zoals weergegeven in de volgende afbeelding. U ziet dat het zoekwoord wordt gemarkeerd in de zoekresultaten. Dit gebeurt omdat de optie **Markeren** is **ingeschakeld**. U kunt ook het aantal **resultaten per pagina** in zoekresultaten opgeven.

    ![Azure Data Catalog - resultaten van basiszoekopdracht naar tekst](media/register-data-assets-tutorial/data-catalog-basic-text-search-results.png)

    Het deelvenster **Zoekopdrachten** bevindt zich aan de linkerkant. Het deelvenster **Eigenschappen** bevindt zich aan de rechterkant. In het deelvenster **Zoekopdrachten** kunt u zoekcriteria wijzigen en resultaten filteren. In het deelvenster **Eigenschappen** worden de eigenschappen van een geselecteerd object weergegeven in het raster of de lijst.

4. Selecteer **Product** in de lijst met zoekresultaten. Selecteer de **Preview**, **kolommen**, **Gegevensprofiel**, en **documentatie** tabbladen, of Selecteer de pijl om uit te breiden het onderste deelvenster.  

    ![Azure Data Catalog - onderste deelvenster](media/register-data-assets-tutorial/data-catalog-data-asset-preview.png)

    Op het tabblad **Preview** ziet u een voorbeeld van de gegevens in de tabel **Product**.  
5. Selecteer de **kolommen** tabblad voor informatie over de kolommen (zoals **naam** en **gegevenstype**) in de gegevensasset.

6. Selecteer de **Gegevensprofiel** tabblad om te bekijken van de profilering van gegevens (bijvoorbeeld: aantal rijen, de grootte van gegevens of de minimale waarde in een kolom) in de gegevensasset.

### <a name="discover-data-assets-with-property-scoping"></a>Gegevens detecteren door het bereik van eigenschappen te definiëren

Door het bereik van eigenschappen te definiëren, kunt u gegevensassets detecteren waarbij de zoekterm overeenkomt met de opgegeven eigenschap.

1. Schakel het filter **Tabel** onder **Objecttype** in **Filters** uit.  

2. Typ `tags:product` in het zoekvak en druk op **ENTER**. Zie [Verwijzing naar de zoeksyntaxis van Data Catalog](/rest/api/datacatalog/#search-syntax-reference) voor alle eigenschappen die u kunt gebruiken voor het zoeken in de gegevenscatalogus.

3. Bevestig dat u de tabellen en de database in de resultaten ziet.  

    ![Data Catalog - resultaten van het definiëren van het bereik van eigenschappen](media/register-data-assets-tutorial/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>De zoekopdracht opslaan

1. In de **zoekopdrachten** deelvenster in de **huidige zoekopdracht** sectie, voer een naam voor de zoekopdracht en selecteert u **opslaan**.

    ![Azure Data Catalog - zoekopdracht opslaan](media/register-data-assets-tutorial/data-catalog-save-search.png)

2. Bevestig dat de opgeslagen zoekopdracht wordt weergegeven onder **Opgeslagen zoekopdrachten**.

3. Selecteer een van de acties die kunnen worden uitgevoerd op de opgeslagen zoekopdracht (**Naam wijzigen**, **Verwijderen**, **Opslaan als standaard** (zoekopdracht)).

### <a name="grouping-with-parentheses"></a>Groeperen met haakjes

Door te groeperen met behulp van haakjes, kunt u delen van de query groeperen voor logische isolatie, met name in combinatie met Booleaanse operators.

1. Typ `name:product AND (tags:product AND objectType:table)` in het zoekvak en druk op **ENTER**.

2. Bevestig dat u alleen de tabel **Product** in de zoekresultaten ziet.

    ![Azure Data Catalog - zoekopdracht groeperen](media/register-data-assets-tutorial/data-catalog-grouping-search.png)

### <a name="comparison-operators"></a>Vergelijkingsoperators

Met vergelijkingsoperators kunt u andere vergelijkingen dan gelijkheid gebruiken voor eigenschappen die de gegevenstypen numeriek en datum hebben.

1. Typ `lastRegisteredTime:>"06/09/2016"` in het zoekvak.

2. Schakel het filter **Tabel** onder **Objecttype** uit.

3. Druk op **ENTER**.

4. Controleer of de **Product**, **ProductCategory**, en **ProductDescription** tabellen en de Azure SQL-database die u hebt geregistreerd in de zoekresultaten.

    ![Azure Data Catalog - vergelijking van zoekresultaten](media/register-data-assets-tutorial/data-catalog-comparison-operator-results.png)

Zie [gegevensassets detecteren](data-catalog-how-to-discover.md) voor gedetailleerde informatie over het detecteren van gegevensassets. Zie voor meer informatie over zoeksyntaxis [verwijzing naar Data Catalog syntaxis](/rest/api/datacatalog/#search-syntax-reference).

## <a name="annotate-data-assets"></a>Aantekeningen toevoegen aan gegevensassets

In deze oefening gebruikt u de Azure Data Catalog-portal voor aantekeningen toevoegen aan (toevoegen van informatie zoals beschrijvingen, tags of experts) bestaande gegevensassets in de catalogus. De aantekeningen vormen een aanvulling op de structurele metagegevens die zijn geëxtraheerd uit de gegevensbron tijdens de registratie. Aantekening maakt de gegevensassets veel eenvoudiger te detecteren en te begrijpen.

In deze oefening maakt u aantekeningen voor een enkele gegevensasset (ProductPhoto). U voegt een beschrijvende naam en een beschrijving toe aan de gegevensasset ProductPhoto.  

1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com) en zoek met `tags:product` te vinden van de gegevensassets die u hebt geregistreerd.

2. Selecteer **productmodelbestand** in de zoekresultaten.  

3. Voer **Productafbeeldingen** in voor **Beschrijvende naam** en **Productfoto’s voor marketingmateriaal** voor de **Beschrijving**.

    ![Azure Data Catalog - beschrijving van ProductPhoto](media/register-data-assets-tutorial/data-catalog-productmodel-description.png)

    De **Beschrijving** helpt anderen om de geselecteerde gegevensasset te detecteren en te begrijpen waarom en hoe ze deze kunnen gebruiken. U kunt ook meer tags toevoegen en kolommen weergeven. U kunt zoeken en filteren van gegevensbronnen met behulp van de beschrijvende metagegevens die u hebt toegevoegd aan de catalogus.

U kunt ook de volgende stappen uit op deze pagina doen:

* Experts toevoegen voor de gegevensasset. Selecteer **toevoegen** in de **Experts** gebied.

* Tags toevoegen op het niveau van de gegevensset. Selecteer **toevoegen** in de **Tags** gebied. Een label kan een gebruikerstag of een woordenlijsttag zijn. De Standard-editie van Data Catalog bevat een zakelijke woordenlijst waarmee catalogusbeheerders een centrale zakelijke taxonomie kunnen definiëren. Catalogusgebruikers kunnen vervolgens aantekeningen toevoegen aan gegevensassets met behulp van termen in de woordenlijst. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md)

* Tags toevoegen op kolomniveau. Selecteer **toevoegen** onder **Tags** voor de kolom die u aantekeningen wilt toevoegen.

* Beschrijving toevoegen op kolomniveau. Voer een **Beschrijving** in voor een kolom. U kunt ook de metagegevens van de beschrijving bekijken die uit de gegevensbron zijn geëxtraheerd.

* Informatie over **Verzoeken tot toegang** toevoegen die gebruikers laat zien hoe ze toegang kunnen krijgen tot de gegevensasset.
  
* Ga naar het tabblad **Documentatie** en verstrek hier documentatie over de gegevensasset. Met Azure Data Catalog -documentatie kunt u de gegevenscatalogus gebruiken als een opslagplaats voor inhoud om een volledig overzicht te maken van uw gegevensassets.
  
U kunt ook een aantekening toevoegen aan meerdere gegevensassets. U kunt bijvoorbeeld alle geregistreerde gegevensassets selecteren en er een expert voor opgeven.

![Azure Data Catalog - aantekeningen toevoegen aan meerdere gegevensassets](media/register-data-assets-tutorial/data-catalog-multi-select-annotate.png)

Azure Data Catalog ondersteunt een benadering voor aantekeningen via crowdsourcing. Elke Data Catalog-gebruiker kan tags (of woordenlijsttags), beschrijvingen en andere metagegevens toevoegen. Op deze manier gebruikers perspectief toevoegen op een gegevensasset en het gebruik ervan, en dit perspectief delen met andere gebruikers.

Zie [Aantekeningen toevoegen aan gegevensbronnen](data-catalog-how-to-annotate.md) voor gedetailleerde informatie over het toevoegen van aantekeningen aan gegevens.

## <a name="connect-to-data-assets"></a>Verbinding maken met gegevensassets

In deze oefening opent u gegevensassets in geïntegreerde clienthulpprogramma's en een niet-geïntegreerd hulpprogramma (SQL Server Management Studio) met behulp van verbindingsgegevens.

> [!NOTE]
> Het is belangrijk om te weten dat Azure Data Catalog u geen toegang geeft tot de werkelijke gegevensbron. Het wordt gewoon eenvoudiger voor u om ze te detecteren en te begrijpen. Wanneer u verbinding maakt met een gegevensbron, worden uw Windows-referenties gebruikt of wordt u indien nodig om referenties gevraagd. Als u niet eerder toegang hebt gekregen tot de gegevensbron, moet aan u toegang worden verleend voordat u verbinding kunt maken.

### <a name="connect-to-a-data-asset-from-excel"></a>Verbinding maken met een gegevensasset vanuit Excel

1. Selecteer in de zoekresultaten de optie **Product**. Selecteer **openen In** op de werkbalk en selecteer **Excel**.

    ![Azure Data Catalog - verbinding maken met gegevensasset](media/register-data-assets-tutorial/data-catalog-connect1.png)

2. Selecteer **Open** in het downloadpop-upvenster. Deze ervaring kan variëren afhankelijk van de browser.

3. In de **beveiligingsbericht Microsoft Excel** venster **inschakelen**.

    ![Azure Data Catalog - beveiligingspop-upvenster van Excel](media/register-data-assets-tutorial/data-catalog-excel-security-popup.png)

4. Behoud de standaardinstellingen in de **importgegevens** in het dialoogvenster en selecteer **OK**.

    ![Azure Data Catalog - Excel-gegevens importeren](media/register-data-assets-tutorial/data-catalog-excel-import-data.png)

5. Bekijk de gegevensbron in Excel.

    ![Azure Data Catalog - producttabel in Excel](media/register-data-assets-tutorial/data-catalog-connect2.png)

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

In deze oefening hebt u verbinding gemaakt met gegevensasssets die zijn gedetecteerd via Azure Data Catalog. Via de portal van Azure Data Catalog kunt u direct verbinding maken via de clienttoepassingen die zijn geïntegreerd in het menu **Openen in**. U kunt ook verbinding maken met behulp van een andere toepassing naar keuze. Hierbij wordt gebruikgemaakt van de locatiegegevens van de verbinding in de metagegevens van de asset. Bijvoorbeeld, kunt u SQL Server Management Studio verbinding maken met de Azure SQL-database voor toegang tot de gegevens in de gegevensassets die in deze zelfstudie zijn geregistreerd.

1. Open **SQL Server Management Studio**.

2. Voer in het dialoogvenster **Verbinding maken met server** de naam in van de server uit het deelvenster **Eigenschappen** in de Azure Data Catalog-portal.

3. Gebruik de juiste verificatieprocedure en aanmeldingsgegevens voor toegang tot de gegevensasset. Gebruik de informatie in het veld **Verzoeken tot toegang** als u geen toegang hebt.

    ![Azure Data Catalog - verzoeken tot toegang](media/register-data-assets-tutorial/data-catalog-request-access.png)

Selecteer **verbindingsreeksen weergeven** te bekijken en kopiëren van ADO.NET, ODBC- en OLEDB-verbindingsreeksen naar het Klembord voor gebruik in uw toepassing.

## <a name="manage-data-assets"></a>Gegevensassets beheren

In deze stap ziet u hoe u beveiliging kunt instellen voor uw gegevensassets. Data Catalog geeft geen gebruikers toegang tot de gegevens zelf. De eigenaar van de gegevensbron bepaalt de toegang tot gegevens.

Met Data Catalog kunt u gegevensbronnen detecteren en de metagegevens bekijken die gerelateerd zijn aan de bronnen die in de catalogus zijn geregistreerd. Mogelijk zijn er echter situaties echter waarin gegevensbronnen alleen zichtbaar moeten zijn voor specifieke gebruikers of specifieke groepen. Voor deze scenario's kunt u via Data Catalog eigenaar worden van geregistreerde gegevensassets en de zichtbaarheid van de assets bepalen.

> [!NOTE]
> De beheermogelijkheden die in deze taak worden beschreven, zijn alleen beschikbaar in de Standard-editie van Azure Data Catalog, niet in de gratis editie.
> In Azure Data Catalog kunt u eigenaar worden van gegevensassets, mede-eigenaren aan gegevensassets toevoegen en de zichtbaarheid van gegevensassets instellen.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Eigenaar worden van gegevensassets en de zichtbaarheid beperken

1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com). Typ `tags:cycles` in het tekstvak **Zoeken** en druk op **ENTER**.

2. Selecteer een item in de lijst met resultaten en selecteer **eigenaar** op de werkbalk.

3. In de **Management** sectie van de **eigenschappen** Configuratiescherm, selecteer **eigenaar**.

    ![Azure Data Catalog - eigenaar worden](media/register-data-assets-tutorial/data-catalog-take-ownership.png)

4. Als u wilt zichtbaarheid beperken, kiest u **eigenaars en deze gebruikers** in de **zichtbaarheid** sectie en selecteer **toevoegen**. Voer in het tekstvak de e-mailadressen van de betreffende gebruikers in en druk op **ENTER**.

    ![Azure Data Catalog - toegang beperken](media/register-data-assets-tutorial/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Gegevensassets verwijderen

In deze oefening gebruikt u de portal van Azure Data Catalog om voorbeeldgegevens te verwijderen uit geregistreerde gegevensassets en om gegevensassets te verwijderen uit de catalogus.

In Azure Data Catalog kunt u een individuele asset of meerdere assets verwijderen.

1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com).

2. In de **zoeken** tekst Voer `tags:cycles` en selecteer **ENTER**.

3. Selecteer een item in de lijst met resultaten en selecteer **verwijderen** op de werkbalk zoals weergegeven in de volgende afbeelding:

    ![Azure Data Catalog - rasteritem verwijderen](media/register-data-assets-tutorial/data-catalog-delete-grid-item.png)

    Als u de lijstweergave gebruikt, is het selectievakje aan de linkerkant van het item, zoals wordt weergegeven in de volgende afbeelding:

    ![Azure Data Catalog - lijstitem verwijderen](media/register-data-assets-tutorial/data-catalog-delete-list-item.png)

    U kunt ook meerdere gegevensassets selecteren en deze verwijderen zoals op de volgende afbeelding wordt aangegeven:

    ![Azure Data Catalog - meerdere gegevensassets verwijderen](media/register-data-assets-tutorial/data-catalog-delete-assets.png)

> [!NOTE]
> Het standaardgedrag van de catalogus is om elke gebruiker toe te staan een gegevensbron te registreren en om elke gebruiker toe te staan een geregistreerde gegevensasset te verwijderen. De beheermogelijkheden die in de Standard-editie van Azure Data Catalog zijn opgenomen, bieden aanvullende opties om eigenaar te worden van assets, te beperken wie er assets kunnen detecteren en te beperken wie er assets kunnen verwijderen.

## <a name="summary"></a>Samenvatting

In deze zelfstudie hebt u essentiële mogelijkheden van Azure Data Catalog verkend, inclusief het registreren, annoteren, detecteren en beheren van zakelijke gegevensassets. Nu u de zelfstudie hebt voltooid, is het tijd om aan de slag te gaan. U kunt vandaag nog beginnen de gegevensbronnen waarop u en uw team vertrouwen te registreren en collega's uit te nodigen om de catalogus te gebruiken.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ondersteunde gegevensbronnen](data-catalog-dsr.md)