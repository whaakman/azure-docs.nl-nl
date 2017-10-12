---
title: Aan de slag met Data Catalog | Microsoft Docs
description: End-to-end zelfstudie waarin de scenario's en mogelijkheden van Azure Data Catalog worden gepresenteerd.
documentationcenter: 
services: data-catalog
author: steelanddata
manager: jhubbard
editor: 
tags: 
ms.assetid: 03332872-8d84-44a0-8a78-04fd30e14b18
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 5a3445aee7722579405b67830ca49ef8c0b29d0e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-catalog"></a>Aan de slag met Azure Data Catalog
Azure Data Catalog is een volledig beheerde cloudservice die als een registratie- en detectiesysteem voor gegevensassets van ondernemingen fungeert. Zie [Wat is Azure Data Catalog?](data-catalog-what-is-data-catalog.md) voor een gedetailleerd overzicht.

Deze zelfstudie helpt u om aan de slag te gaan met Azure Data Catalog. In deze zelfstudie voert u de volgende procedures uit:

| Procedure | Beschrijving |
|:--- |:--- |
| [Gegevenscatalogus inrichten](#provision-data-catalog) |In deze procedure richt of stelt u een Azure Data Catalog in. U voert deze stap alleen uit als de catalogus nog niet is ingesteld. Zelfs als er aan uw Azure-account meerdere abonnementen zijn gekoppeld, kunt u slechts één gegevenscatalogus per organisatie (Microsoft Azure Active Directory-domein) hebben. |
| [Gegevensassets registreren](#register-data-assets) |In deze procedure registreert u gegevensassets uit de AdventureWorks2014-voorbeelddatabase in de gegevenscatalogus. Registratie is het proces waarbij belangrijke structurele metagegevens, zoals namen, typen en locaties, worden geëxtraheerd uit de gegevensbron en worden gekopieerd naar de catalogus. De gegevensbronnen en gegevensassets blijven waar ze zijn, maar de metagegevens worden door de catalogus gebruikt om de gegevensbronnen en gegevensassets eenvoudiger te detecteren en te begrijpen. |
| [Gegevensassets detecteren](#discover-data-assets) |In deze procedure gebruikt u de Azure Data Catalog-portal voor het detecteren van de gegevensassets die in de vorige stap zijn geregistreerd. Nadat een gegevensbron is geregistreerd in Azure Data Catalog, worden de metagegevens ervan door de service geïndexeerd, zodat gebruikers eenvoudig kunnen zoeken naar de gegevens die zij nodig hebben. |
| [Aantekeningen toevoegen aan gegevensassets](#annotate-data-assets) |In deze procedure voegt u aantekeningen (informatie zoals beschrijvingen, labels, documentatie of experts) toe aan de gegevensassets. Deze informatie is een aanvulling op de metagegevens die uit de gegevensbron zijn geëxtraheerd. Zo maakt u de gegevensbron voor meer mensen inzichtelijk. |
| [Verbinding maken met gegevensassets](#connect-to-data-assets) |In deze procedure opent u gegevensassets in geïntegreerde clienthulpprogramma's (zoals Excel en SQL Server Data Tools) en een niet-geïntegreerd hulpprogramma (SQL Server Management Studio). |
| [Gegevensassets beheren](#manage-data-assets) |In deze procedure stelt u beveiliging in voor uw gegevensassets. Data Catalog geeft gebruikers geen toegang tot de gegevens zelf. De eigenaar van de gegevensbron bepaalt de toegang tot gegevens. <br/><br/> Met Data Catalog kunt u gegevensbronnen detecteren en de **metagegevens** bekijken die gerelateerd zijn aan de bronnen die in de catalogus zijn geregistreerd. Mogelijk zijn er echter situaties waarin gegevensbronnen alleen zichtbaar moeten zijn voor specifieke gebruikers of specifieke groepen. Voor deze scenario's kunt u via Data Catalog eigenaar worden van geregistreerde gegevensassets in de catalogus en zelf de zichtbaarheid van de assets bepalen. |
| [Gegevensassets verwijderen](#remove-data-assets) |In deze procedure leert u hoe u gegevensassets uit de gegevenscatalogus verwijdert. |

## <a name="tutorial-prerequisites"></a>Vereisten voor de zelfstudie
### <a name="azure-subscription"></a>Azure-abonnement
Voor het instellen van Azure Data Catalog moet u eigenaar of mede-eigenaar van een Azure-abonnement zijn.

Azure-abonnementen helpen u om toegang tot cloudserviceresources als Azure Data Catalog te organiseren. Ze helpen u ook om te bepalen hoe resourcegebruik wordt gerapporteerd, gefactureerd en betaald. Elk abonnement kan een andere facturerings- en betalingsinstelling hebben, zodat u per afdeling, project, regiokantoor, enzovoort verschillende abonnementen en plannen kunt hebben. Elke cloudservice behoort tot een abonnement. U moet over een abonnement beschikken voordat u Azure Data Catalog instelt. Zie [Accounts, abonnementen en beheerdersrollen beheren](../active-directory/active-directory-how-subscriptions-associated-directory.md) voor meer informatie.

Als u geen abonnement hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="azure-active-directory"></a>Azure Active Directory
Voor het instellen van Azure Data Catalog moet u zijn aangemeld met een gebruikersaccount van Azure Active Directory (Azure AD). U moet de eigenaar of mede-eigenaar van een Azure-abonnement zijn.  

Azure AD biedt uw bedrijf een eenvoudige manier om ID’s en toegang, zowel in de cloud als on-premises, te beheren. U kunt één werk- of schoolaccount gebruiken om u bij elke cloud- of on-premises webtoepassing aan te melden. Azure Data Catalog maakt gebruik van Azure AD om de aanmelding te verifiëren. Zie [Wat is Azure Active Directory?](../active-directory/active-directory-whatis.md) voor meer informatie.

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-beleid configureren
Het kan voorkomen dat u zich wel kunt aanmelden bij de Azure Data Catalog-portal, maar niet bij het hulpprogramma voor gegevensbronregistratie. Er wordt dan een foutmelding weergegeven. Deze fout kan zich voordoen wanneer u zich op het bedrijfsnetwerk bevindt of wanneer u verbinding maakt als u zich buiten het bedrijfsnetwerk bevindt.

Het registratiehulpprogramma maakt gebruik van *formulierverificatie* om de gebruikersaanmeldingen te valideren bij Azure Active Directory. Als u zich wilt aanmelden, moet een Azure Active Directory-beheerder formulierverificatie inschakelen in het *algemene verificatiebeleid*.

Met het algemene verificatiebeleid kunt u afzonderlijke verificatie voor intranet- en extranetverbindingen inschakelen, zoals wordt weergegeven in de volgende afbeelding. Als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit u verbinding maakt, kan dit resulteren in aanmeldingsfouten.

 ![Het algemene verificatiebeleid van Azure Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Zie [Verificatiebeleid configureren](https://technet.microsoft.com/library/dn486781.aspx) voor meer informatie.

## <a name="provision-data-catalog"></a>Gegevenscatalogus inrichten
U kunt per organisatie (Azure Active Directory-domein) slechts één gegevenscatalogus inrichten. Als de eigenaar of mede-eigenaar van het Azure-abonnement waartoe dit Azure Active Directory-domein behoort, al een catalogus heeft gemaakt, kunt u daarom geen nieuwe catalogus maken, zelfs niet als u meerdere Azure-abonnementen hebt. Als u wilt testen of er al een gegevenscatalogus is gemaakt door een gebruiker in uw Azure Active Directory-domein, gaat u naar de [startpagina van Azure Data Catalog](http://azuredatacatalog.com) en controleert u of u de catalogus ziet. Als er al een catalogus voor u is gemaakt, slaat u de volgende procedure over en gaat u naar de volgende sectie.    

1. Ga naar de [Data Catalog-servicepagina](https://azure.microsoft.com/services/data-catalog) en klik op **Aan de slag**.
   
    ![Azure Data Catalog - marketingstartpagina](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Meld u aan met een gebruikersaccount dat de eigenaar of mede-eigenaar van een Azure-abonnement is. Na de aanmelding ziet u de volgende pagina.
   
    ![Azure Data Catalog - gegevenscatalogus inrichten](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Geef een **naam** op voor de gegevenscatalogus, het **abonnement** dat u wilt gebruiken en de **locatie** voor de catalogus.
4. Vouw **Prijzen** uit en selecteer een Azure Data Catalog-**editie** (gratis of Standard).
    ![Azure Data Catalog - editie selecteren](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Vouw **Catalogusgebruikers** uit en klik op **Toevoegen** om gebruikers toe te voegen aan de gegevenscatalogus. U wordt automatisch toegevoegd aan deze groep.
    ![Azure Data Catalog - gebruikers](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Vouw **Catalogusbeheerders** uit en klik op **Toevoegen** om meer beheerders toe te voegen aan de gegevenscatalogus. U wordt automatisch toegevoegd aan deze groep.
    ![Azure Data Catalog - beheerders](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klik op **Catalogus maken** om de gegevenscatalogus voor uw organisatie te maken. U ziet de startpagina van de gegevenscatalogus nadat deze is gemaakt.
    ![Azure Data Catalog - gemaakt](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Zoeken naar een gegevenscatalogus in Azure Portal
1. Ga op een afzonderlijk tabblad in de webbrowser of in een afzonderlijke geopend browservenster naar de [Azure Portal](https://portal.azure.com) en meld u aan met hetzelfde account dat u in de vorige stap hebt gebruikt voor het maken van de gegevenscatalogus.
2. Selecteer **Bladeren** en klik vervolgens op **Gegevenscatalogus**.
   
    ![Azure Data Catalog - bladeren door Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) U ziet de gegevenscatalogus die u hebt gemaakt.
   
    ![Azure Data Catalog - catalogus in lijst bekijken](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
3. Klik op de catalogus die u hebt gemaakt. U ziet de blade **Gegevenscatalogus** in de portal.
   
   ![Azure Data Catalog - blade in portal ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
4. U kunt de eigenschappen van de gegevenscatalogus bekijken en bijwerken. Klik bijvoorbeeld op **Prijscategorie** en wijzig de editie.
   
    ![Azure Data Catalog - prijscategorie](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-voorbeelddatabase
In deze zelfstudie registreert u gegevensassets (tabellen) uit de voorbeelddatabase van AdventureWorks2014 voor de SQL Server Database Engine, maar als u liever wilt werken met gegevens die vertrouwd en relevant zijn voor uw rol, kunt u ook een andere ondersteunde gegevensbron gebruiken. Zie voor een lijst met ondersteunde gegevensbronnen [Ondersteunde gegevensbronnen](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>De Adventure Works 2014 OLTP-database installeren
De database van Adventure Works ondersteunt standaardscenario’s voor het verwerken van online transacties van een fictieve fietsfabrikant (Adventure Works Cycles), met inbegrip van producten, verkoop en inkoop. In deze zelfstudie registreert u informatie over producten in Azure Data Catalog.

Ga als volgt te werk om de voorbeelddatabase van Adventure Works te installeren:

1. Download [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) in CodePlex.
2. Volg voor het herstellen van de database op uw machine de instructies in [Een back-up van een database herstellen met behulp van SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx) of via de volgende stappen:
   1. Open SQL Server Management Studio en maak verbinding met de SQL Server Database Engine.
   2. Klik met de rechtermuisknop op **Databases** en klik op **Database terugzetten**.
   3. Klik onder **Database terugzetten** op de optie **Apparaat** voor **Bron**. Klik vervolgens op **Bladeren**.
   4. Klik onder **Apparaat voor back-up selecteren** op **Toevoegen**.
   5. Ga naar de map waarin het bestand **AdventureWorks2014.bak** zich bevindt, selecteer het bestand en klik op **OK** om het dialoogvenster **Back-upbestand zoeken** te sluiten.
   6. Klik op **OK** om het dialoogvenster **Apparaat voor back-up selecteren** te sluiten.    
   7. Klik op **OK** om het dialoogvenster **Database terugzetten** te sluiten.

U kunt nu gegevensassets uit de Adventure Works-voorbeelddatabase registreren met behulp van Azure Data Catalog.

## <a name="register-data-assets"></a>Gegevensassets registreren
In deze oefening gebruikt u de het hulpprogramma voor registratie om gegevensassets uit de Adventure Works-database te registreren bij de catalogus. Registratie is het proces waarbij belangrijke structurele metagegevens, zoals namen, typen en locaties, worden geëxtraheerd uit de gegevensbron en bijbehorende assets, en worden gekopieerd naar de catalogus. De gegevensbronnen en gegevensassets blijven waar ze zijn, maar de metagegevens worden door de catalogus gebruikt om de gegevensbronnen en gegevensassets eenvoudiger te detecteren en te begrijpen.

### <a name="register-a-data-source"></a>Een gegevensbron registreren
1. Ga naar de [startpagina van Azure Data Catalog](http://azuredatacatalog.com) en klik op **Gegevens publiceren**.
   
   ![Azure Data Catalog - de knop Gegevens publiceren](media/data-catalog-get-started/data-catalog-publish-data.png)
2. Klik op **Toepassing starten** om het hulpprogramma voor registratie te downloaden, te installeren en uit te voeren op uw computer.
   
   ![Azure Data Catalog - de knop Starten](media/data-catalog-get-started/data-catalog-launch-application.png)
3. Klik op de **Welkomstpagina** op **Aanmelden** en voer uw referenties in.     
   
    ![Azure Data Catalog - Welkomstpagina](media/data-catalog-get-started/data-catalog-welcome-dialog.png)
4. Klik op de pagina **Microsoft Azure Data Catalog** op **SQL Server** en daarna op **Volgende**.
   
    ![Azure Data Catalog - gegevensbronnen](media/data-catalog-get-started/data-catalog-data-sources.png)
5. Voer de verbindingseigenschappen van SQL Server in voor **AdventureWorks2014** (zie het onderstaande voorbeeld) en klik op **VERBINDING MAKEN**.
   
   ![Azure Data Catalog - SQL Server-verbindingsinstellingen](media/data-catalog-get-started/data-catalog-sql-server-connection.png)
6. Registreer de metagegevens van uw gegevensasset. In dit voorbeeld registreert u objecten voor **Production/Product** vanuit de naamruimte AdventureWorks Production:
   
   1. Vouw in **Serverhiërarchie** het item **AdventureWorks2014** uit en klik op **Production**.
   2. Ctrl+klik op **Product**, **ProductCategory**, **ProductDescription** en **ProductPhoto**.
   3. Klik op de **pijl om de geselecteerde items te verplaatsen** (**>**). Met deze actie worden alle geselecteerde objecten verplaatst naar de lijst **Te registreren objecten**.
      
      ![Zelfstudie voor Azure Data Catalog - bladeren door objecten en objecten selecteren](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
   4. Selecteer **Preview opnemen** om een snapshotvoorbeeld van de gegevens op te nemen. De snapshot bevat maximaal 20 records uit elke tabel en wordt gekopieerd naar de catalogus.
   5. Selecteer **Gegevensprofiel opnemen** om een snapshot van de objectstatistieken op te nemen voor het gegevensprofiel (bijvoorbeeld: minimale, maximale en gemiddelde waarden voor een kolom, het aantal rijen).
   6. Voer in het veld **Tags toevoegen** **adventure works, cycles** in. Met deze actie worden zoektags voor deze gegevensassets toegevoegd. Tags zijn voor gebruikers een uitstekende manier om een geregistreerde gegevensbron te zoeken.
   7. Geef de naam op van een **expert** met betrekking tot deze gegevens (optioneel).
      
      ![Zelfstudie voor Azure Data Catalog - te registreren objecten](media/data-catalog-get-started/data-catalog-objects-register.png)
   8. Klik op **REGISTREREN**. Azure Data Catalog registreert uw geselecteerde objecten. In deze oefening worden de geselecteerde objecten van Adventure Works geregistreerd. Het hulpprogramma voor registratie extraheert metagegevens uit de gegevensasset en kopieert deze gegevens naar de Azure Data Catalog-catalogus. De gegevens blijven waar ze zich momenteel bevinden, onder het beheer van de groep beheerders en het beleid van het huidige systeem.
      
      ![Azure Data Catalog - geregistreerde objecten](media/data-catalog-get-started/data-catalog-registered-objects.png)
   9. Als u uw geregistreerde gegevensbronobjecten wilt zien, klikt u op **Portal weergeven**. Bevestig in de Azure Data Catalog-portal dat u alle vier tabellen en de database in de rasterweergave ziet.
      
      ![Objecten in de Azure Data Catalog-portal ](media/data-catalog-get-started/data-catalog-view-portal.png)

In deze oefening hebt u objecten uit de voorbeelddatabase van Adventure Works geregistreerd, zodat ze eenvoudig kunnen worden gevonden door gebruikers in uw organisatie. In de volgende oefening leert u hoe u geregistreerde gegevensassets kunt detecteren.

## <a name="discover-data-assets"></a>Gegevensassets detecteren
Voor detectie in Azure Data Catalog worden twee primaire mechanismen gebruikt: zoeken en filteren.

Zoeken is zowel intuïtief als krachtig. Standaard worden de zoektermen vergeleken met een eigenschap in de catalogus, met inbegrip van door de gebruiker gemaakte aantekeningen.

Filteren is bedoeld als aanvulling op het zoeken. U kunt specifieke kenmerken, zoals experts, gegevensbrontype, objecttype en tags, selecteren om overeenkomende gegevensassets weer te geven en om uw zoekresultaten te beperken tot overeenkomende assets.

Met behulp van een combinatie van zoeken en filteren kunt u snel navigeren door de gegevensbronnen die bij Azure Data Catalog zijn geregistreerd. Zo detecteert u eenvoudig de relevante gegevensassets.

In deze oefening gebruikt u de Azure Data Catalog-portal voor het detecteren van de gegevensassets die u in de vorige stap hebt geregistreerd. Zie [Verwijzing naar de zoeksyntaxis van Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor meer informatie over de zoeksyntaxis.

Hieronder volgen enkele voorbeelden van het detecteren van gegevensassets in de catalogus.  

### <a name="discover-data-assets-with-basic-search"></a>Gegevensassets met een basiszoekopdracht detecteren
Met een basiszoekopdracht kunt u in een catalogus zoeken met behulp van een of meer zoektermen. De resultaten die worden opgehaald, zijn assets die wat betreft een willekeurige eigenschap overeenkomen met een of meer van de opgegeven voorwaarden.

1. Klik in de Azure Data Catalog-portal op **Startpagina**. Als u de webbrowser hebt gesloten, gaat u naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com).
2. Typ `cycles` in het zoekvak en druk op **ENTER**.
   
    ![Azure Data Catalog - basiszoekopdracht naar tekst](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Bevestig dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten ziet. U kunt overschakelen tussen **rasterweergave** en **lijstweergave** door te klikken op de knoppen op de werkbalk zoals weergegeven in de volgende afbeelding. U ziet dat het zoekwoord wordt gemarkeerd in de zoekresultaten. Dit gebeurt omdat de optie **Markeren** is **ingeschakeld**. U kunt ook het aantal **resultaten per pagina** in zoekresultaten opgeven.
   
    ![Azure Data Catalog - resultaten van basiszoekopdracht naar tekst](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)
   
    Het deelvenster **Zoekopdrachten** bevindt zich aan de linkerkant. Het deelvenster **Eigenschappen** bevindt zich aan de rechterkant. In het deelvenster **Zoekopdrachten** kunt u zoekcriteria wijzigen en resultaten filteren. In het deelvenster **Eigenschappen** worden de eigenschappen van een geselecteerd object weergegeven in het raster of de lijst.
4. Klik op **Product** in de zoekresultaten. Klik op de tabbladen **Preview**, **Kolommen**, **Gegevensprofiel** en **Documentatie** of klik op de pijl om het onderste deelvenster uit te vouwen.  
   
    ![Azure Data Catalog - onderste deelvenster](media/data-catalog-get-started/data-catalog-data-asset-preview.png)
   
    Op het tabblad **Preview** ziet u een voorbeeld van de gegevens in de tabel **Product**.  
5. Klik op het tabblad **Kolommen** voor meer informatie over de kolommen (zoals **naam** en **gegevenstype**) in de gegevensasset.
6. Klik op het tabblad **Gegevensprofiel** om het profiel van gegevens te bekijken (bijvoorbeeld: het aantal rijen, de grootte van gegevens of de minimumwaarde in een kolom) in de gegevensasset.
7. Filter de resultaten met behulp van **Filters** (aan de linkerkant). Klik bijvoorbeeld op **Tabel** voor **Objecttype**. U ziet dan alleen de vier tabellen, niet de database.
   
    ![Azure Data Catalog - zoekresultaten filteren](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Gegevens detecteren door het bereik van eigenschappen te definiëren
Door het bereik van eigenschappen te definiëren, kunt u gegevensassets detecteren waarbij de zoekterm overeenkomt met de opgegeven eigenschap.

1. Schakel het filter **Tabel** onder **Objecttype** in **Filters** uit.  
2. Typ `tags:cycles` in het zoekvak en druk op **ENTER**. Zie [Verwijzing naar de zoeksyntaxis van Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor alle eigenschappen die u kunt gebruiken voor het zoeken in de gegevenscatalogus.
3. Bevestig dat u alle vier tabellen en de database (AdventureWorks2014) in de resultaten ziet.  
   
    ![Data Catalog - resultaten van het definiëren van het bereik van eigenschappen](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>De zoekopdracht opslaan
1. Voer in het deelvenster **Zoekopdrachten** in de sectie **Huidige zoekopdracht** een naam voor de zoekopdracht in en klik op **Opslaan**.
   
    ![Azure Data Catalog - zoekopdracht opslaan](media/data-catalog-get-started/data-catalog-save-search.png)
2. Bevestig dat de opgeslagen zoekopdracht wordt weergegeven onder **Opgeslagen zoekopdrachten**.
   
    ![Azure Data Catalog - opgeslagen zoekopdrachten](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Selecteer een van de acties die kunnen worden uitgevoerd op de opgeslagen zoekopdracht (**Naam wijzigen**, **Verwijderen**, **Opslaan als standaard** (zoekopdracht)).
   
    ![Azure Data Catalog - opties voor opgeslagen zoekopdrachten](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Booleaanse operators
U kunt uw zoekopdracht uitbreiden of beperken met Booleaanse operators.

1. Typ `tags:cycles AND objectType:table` in het zoekvak en druk op **ENTER**.
2. Bevestig dat u alleen tabellen (niet de database) in de resultaten ziet.  
   
    ![Azure Data Catalog - Booleaanse operator in zoekopdracht](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Groeperen met haakjes
Door te groeperen met behulp van haakjes, kunt u delen van de query groeperen voor logische isolatie, met name in combinatie met Booleaanse operators.

1. Typ `name:product AND (tags:cycles AND objectType:table)` in het zoekvak en druk op **ENTER**.
2. Bevestig dat u alleen de tabel **Product** in de zoekresultaten ziet.
   
    ![Azure Data Catalog - zoekopdracht groeperen](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vergelijkingsoperators
Met vergelijkingsoperators kunt u andere vergelijkingen dan gelijkheid gebruiken voor eigenschappen die de gegevenstypen numeriek en datum hebben.

1. Typ `lastRegisteredTime:>"06/09/2016"` in het zoekvak.
2. Schakel het filter **Tabel** onder **Objecttype** uit.
3. Druk op **ENTER**.
4. Bevestig dat u in de zoekresultaten de tabellen **Product**, **ProductCategory**, **ProductDescription** en **ProductPhoto** en de geregistreerde AdventureWorks2014-database ziet.
   
    ![Azure Data Catalog - vergelijking van zoekresultaten](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Zie [Gegevensbronnen detecteren](data-catalog-how-to-discover.md) voor gedetailleerde informatie over het detecteren van gegevens en [Verwijzing naar de zoeksyntaxis van Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) voor informatie over de zoeksyntaxis.

## <a name="annotate-data-assets"></a>Aantekeningen toevoegen aan gegevensassets
In deze oefening gebruikt u de portal van Azure Data Catalog voor het maken van aantekeningen (voeg informatie toe zoals beschrijvingen, tags of experts) bij gegevensassets die eerder zijn geregistreerd in de catalogus. De aantekeningen dienen ter aanvulling en verbetering van de structurele metagegevens die zijn geëxtraheerd uit de gegevensbron tijdens de registratie, en zorgen ervoor dat de gegevensassets veel eenvoudiger te detecteren en te begrijpen zijn.

In deze oefening maakt u aantekeningen voor een enkele gegevensasset (ProductPhoto). U voegt een beschrijvende naam en een beschrijving toe aan de gegevensasset ProductPhoto.  

1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com) en zoek met `tags:cycles` naar de gegevensassets die u hebt geregistreerd.  
2. Klik in de zoekresultaten op **ProductPhoto**.  
3. Voer **Productafbeeldingen** in voor **Beschrijvende naam** en **Productfoto’s voor marketingmateriaal** voor de **Beschrijving**.
   
    ![Azure Data Catalog - beschrijving van ProductPhoto](media/data-catalog-get-started/data-catalog-productphoto-description.png)
   
    De **Beschrijving** helpt anderen om de geselecteerde gegevensasset te detecteren en te begrijpen waarom en hoe ze deze kunnen gebruiken. U kunt ook meer tags toevoegen en kolommen weergeven. Nu kunt u met zoeken en filteren proberen om de gegevensassets te detecteren op basis van de beschrijvende metagegevens die u aan de catalogus hebt toegevoegd.

U kunt ook de volgende handelingen op deze pagina uitvoeren:

* Experts toevoegen voor de gegevensasset. Klik in het gebied **Experts** op **Toevoegen**.
* Tags toevoegen op het niveau van de gegevensset. Klik in het gebied **Tags** op **Toevoegen**. Een label kan een gebruikerstag of een woordenlijsttag zijn. De Standard-editie van Data Catalog bevat een zakelijke woordenlijst waarmee catalogusbeheerders een centrale zakelijke taxonomie kunnen definiëren. Catalogusgebruikers kunnen vervolgens aantekeningen toevoegen aan gegevensassets met behulp van termen in de woordenlijst. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md)
* Tags toevoegen op kolomniveau. Klik voor de kolom waaraan u aantekeningen wilt toevoegen, onder **Tags** op **Toevoegen**.
* Beschrijving toevoegen op kolomniveau. Voer een **Beschrijving** in voor een kolom. U kunt ook de metagegevens van de beschrijving bekijken die uit de gegevensbron zijn geëxtraheerd.
* Informatie over **Verzoeken tot toegang** toevoegen die gebruikers laat zien hoe ze toegang kunnen krijgen tot de gegevensasset.
  
    ![Azure Data Catalog - tags en beschrijvingen toevoegen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)
* Ga naar het tabblad **Documentatie** en verstrek hier documentatie over de gegevensasset. Met Azure Data Catalog -documentatie kunt u de gegevenscatalogus gebruiken als een opslagplaats voor inhoud om een volledig overzicht te maken van uw gegevensassets.
  
    ![Azure Data Catalog - het tabblad Documentatie](media/data-catalog-get-started/data-catalog-documentation.png)

U kunt ook een aantekening toevoegen aan meerdere gegevensassets. U kunt bijvoorbeeld alle geregistreerde gegevensassets selecteren en er een expert voor opgeven.

![Azure Data Catalog - aantekeningen toevoegen aan meerdere gegevensassets](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure Data Catalog ondersteunt een benadering voor aantekeningen via crowdsourcing. Elke Data Catalog-gebruiker kan tags (gebruikerstags of woordenlijsttags), beschrijvingen en andere metagegevens toevoegen. Elke gebruiker met kennis over een gegevensasset en het gebruik ervan kan deze kennis dus vastleggen en beschikbaar stellen aan andere gebruikers.

Zie [Aantekeningen toevoegen aan gegevensbronnen](data-catalog-how-to-annotate.md) voor gedetailleerde informatie over het toevoegen van aantekeningen aan gegevens.

## <a name="connect-to-data-assets"></a>Verbinding maken met gegevensassets
In deze oefening opent u gegevensassets in geïntegreerde clienthulpprogramma's en een niet-geïntegreerd hulpprogramma (SQL Server Management Studio) met behulp van verbindingsgegevens.

> [!NOTE]
> Het is belangrijk om te weten dat Azure Data Catalog u geen toegang geeft tot de werkelijke gegevensbron. Het wordt gewoon eenvoudiger voor u om ze te detecteren en te begrijpen. Wanneer u verbinding maakt met een gegevensbron, worden uw Windows-referenties gebruikt of wordt u indien nodig om referenties gevraagd. Als u niet eerder toegang hebt gekregen tot de gegevensbron, moet aan u toegang worden verleend voordat u verbinding kunt maken.
> 
> 

### <a name="connect-to-a-data-asset-from-excel"></a>Verbinding maken met een gegevensasset vanuit Excel
1. Selecteer in de zoekresultaten de optie **Product**. Klik op de werkbalk op **Openen in**. Klik vervolgens op **Excel**.
   
    ![Azure Data Catalog - verbinding maken met gegevensasset](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klik in het downloadpop-upvenster op **Openen**. Deze ervaring kan variëren afhankelijk van de browser.
   
    ![Azure Data Catalog - gedownload Excel-verbindingsbestand](media/data-catalog-get-started/data-catalog-download-open.png)
3. Klik in het venster **Beveiligingsbericht Microsoft Excel** op **Inschakelen**.
   
    ![Azure Data Catalog - beveiligingspop-upvenster van Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Behoud de standaardinstellingen in het dialoogvenster **Gegevens importeren** en klik op **OK**.
   
    ![Azure Data Catalog - Excel-gegevens importeren](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Bekijk de gegevensbron in Excel.
   
    ![Azure Data Catalog - producttabel in Excel](media/data-catalog-get-started/data-catalog-connect2.png)

In deze oefening hebt u verbinding gemaakt met gegevensasssets die zijn gedetecteerd via Azure Data Catalog. Via de portal van Azure Data Catalog kunt u direct verbinding maken via de clienttoepassingen die zijn geïntegreerd in het menu **Openen in**. U kunt ook verbinding maken met behulp van een andere toepassing naar keuze. Hierbij wordt gebruikgemaakt van de locatiegegevens van de verbinding in de metagegevens van de asset. U kunt bijvoorbeeld SQL Server Management Studio gebruiken om verbinding te maken met de AdventureWorks2014-database voor toegang tot de gegevens in de gegevensassets die in deze zelfstudie zijn geregistreerd.

1. Open **SQL Server Management Studio**.
2. Voer in het dialoogvenster **Verbinding maken met server** de naam in van de server uit het deelvenster **Eigenschappen** in de Azure Data Catalog-portal.
3. Gebruik de juiste verificatieprocedure en aanmeldingsgegevens voor toegang tot de gegevensasset. Gebruik de informatie in het veld **Verzoeken tot toegang** als u geen toegang hebt.
   
    ![Azure Data Catalog - verzoeken tot toegang](media/data-catalog-get-started/data-catalog-request-access.png)

Klik op **Verbindingsreeksen weergeven** om ADF.NET-, ODBC- en OLEDB-verbindingsreeksen te bekijken en te kopiëren naar het klembord voor gebruik in uw toepassing.

## <a name="manage-data-assets"></a>Gegevensassets beheren
In deze stap ziet u hoe u beveiliging kunt instellen voor uw gegevensassets. Data Catalog geeft gebruikers geen toegang tot de gegevens zelf. De eigenaar van de gegevensbron bepaalt de toegang tot gegevens.

Met Data Catalog kunt u gegevensbronnen detecteren en de metagegevens bekijken die gerelateerd zijn aan de bronnen die in de catalogus zijn geregistreerd. Mogelijk zijn er echter situaties echter waarin gegevensbronnen alleen zichtbaar moeten zijn voor specifieke gebruikers of specifieke groepen. Voor deze scenario's kunt u via Data Catalog het eigendom over geregistreerde gegevensassets in de catalogus opeisen en dan zelf de zichtbaarheid van de assets bepalen.

> [!NOTE]
> De beheermogelijkheden die in deze taak worden beschreven, zijn alleen beschikbaar in de Standard-editie van Azure Data Catalog, niet in de gratis editie.
> In Azure Data Catalog kunt u eigenaar worden van gegevensassets, mede-eigenaren aan gegevensassets toevoegen en de zichtbaarheid van gegevensassets instellen.
> 
> 

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Eigenaar worden van gegevensassets en de zichtbaarheid beperken
1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com). Typ `tags:cycles` in het tekstvak **Zoeken** en druk op **ENTER**.
2. Klik op een item in de lijst met resultaten. Klik vervolgens op **Eigenaar worden** in de werkbalk.
3. Klik in het venster **Eigenschappen** van de sectie **Beheer** op **Eigenaar worden**.
   
    ![Azure Data Catalog - eigenaar worden](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Als u de zichtbaarheid wilt beperken, kiest u **Eigenaars en deze gebruikers** in de sectie **Zichtbaarheid** en klikt u vervolgens op **Toevoegen**. Voer in het tekstvak de e-mailadressen van de betreffende gebruikers in en druk op **ENTER**.
   
    ![Azure Data Catalog - toegang beperken](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Gegevensassets verwijderen
In deze oefening gebruikt u de portal van Azure Data Catalog om voorbeeldgegevens te verwijderen uit geregistreerde gegevensassets en om gegevensassets te verwijderen uit de catalogus.

In Azure Data Catalog kunt u een individuele asset of meerdere assets verwijderen.

1. Ga naar de [startpagina van Azure Data Catalog](https://www.azuredatacatalog.com).
2. Typ `tags:cycles` in het tekstvak **Zoeken** en druk op **ENTER**.
3. Selecteer een item in de lijst met resultaten en klik in de werkbalk op **Verwijderen**, zoals weergegeven in de volgende afbeelding:
   
    ![Azure Data Catalog - rasteritem verwijderen](media/data-catalog-get-started/data-catalog-delete-grid-item.png)
   
    Als u de lijstweergave gebruikt, wordt het selectievakje aan de linkerkant van het item weergegeven. Zie de volgende afbeelding:
   
    ![Azure Data Catalog - lijstitem verwijderen](media/data-catalog-get-started/data-catalog-delete-list-item.png)
   
    U kunt ook meerdere gegevensassets selecteren en deze verwijderen zoals op de volgende afbeelding wordt aangegeven:
   
    ![Azure Data Catalog - meerdere gegevensassets verwijderen](media/data-catalog-get-started/data-catalog-delete-assets.png)

> [!NOTE]
> Het standaardgedrag van de catalogus is om elke gebruiker toe te staan een gegevensbron te registreren en om elke gebruiker toe te staan een geregistreerde gegevensasset te verwijderen. De beheermogelijkheden die in de Standard-editie van Azure Data Catalog zijn opgenomen, bieden aanvullende opties om eigenaar te worden van assets, te beperken wie er assets kunnen detecteren en te beperken wie er assets kunnen verwijderen.
> 
> 

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u essentiële mogelijkheden van Azure Data Catalog verkend, inclusief het registreren, annoteren, detecteren en beheren van zakelijke gegevensassets. Nu u de zelfstudie hebt voltooid, is het tijd om aan de slag te gaan. U kunt vandaag nog beginnen de gegevensbronnen waarop u en uw team vertrouwen te registreren en collega's uit te nodigen om de catalogus te gebruiken.

## <a name="references"></a>Verwijzingen
* [Gegevensassets registreren](data-catalog-how-to-register.md)
* [Gegevensassets detecteren](data-catalog-how-to-discover.md)
* [Aantekeningen toevoegen aan gegevensassets](data-catalog-how-to-annotate.md)
* [Gegevensassets documenteren](data-catalog-how-to-documentation.md)
* [Verbinding maken met gegevensassets](data-catalog-how-to-connect.md)
* [Gegevensassets beheren](data-catalog-how-to-manage.md)

