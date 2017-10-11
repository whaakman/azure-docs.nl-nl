---
title: Veelgestelde vragen over Azure Data Catalog | Microsoft Docs
description: Veelgestelde vragen over Azure Data Catalog, met inbegrip van de mogelijkheden voor detectie van gegevensbron, aantekening en beheer.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 11e4bc7e71b4a94c3a0eda4275745b1beb44974d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Veelgestelde vragen over Azure Data Catalog
In dit artikel vindt u antwoorden op veelgestelde vragen met betrekking tot de service Azure Data Catalog.

## <a name="what-is-azure-data-catalog"></a>Wat is Azure Data Catalog?
Data Catalog is een volledig beheerde service wordt gehost in Microsoft Azure, die als een systeem van registratie en detectie van zakelijke gegevensbronnen fungeert. Met Data Catalog, kan een willekeurige gebruiker, van analisten en gegevenswetenschappers en ontwikkelaars registreren, detecteren, begrijpen en gebruiken gegevensbronnen.

## <a name="what-customer-challenges-does-it-solve"></a>Welke klant uitdagingen biedt deze oplossen?
Data Catalog adressen de uitdagingen van de gegevensbron detectie en 'donkere gegevens', zodat gebruikers kunnen detecteren en begrijpen van zakelijke gegevensbronnen.

## <a name="what-are-its-target-audiences"></a>Wat zijn de doelgroepen?
Data Catalog is ontworpen voor technische en niet-technische gebruikers, waaronder:

* Ontwikkelaars van gegevens en BI en analytics-professionals: mensen die verantwoordelijk zijn voor het maken van gegevens en analyse inhoud zodat anderen deze kunnen gebruiken.
* Gegevens stewards: personen die de kennis over de gegevens, wat het betekent en hoe het is bedoeld om te worden gebruikt.
* Gegevensgebruikers: mensen die moeten kunnen gemakkelijk detecteren, begrijpen en verbinding maken met de gegevens die ze hun taak uitvoeren moeten met het hulpprogramma van hun keuze.
* Centrale IT: mensen die honderden gegevensbronnen detecteerbaar maken met zakelijke gebruikers nodig hebben en die zich moet houden toezicht over hoe gegevens wordt gebruikt en door wie.

## <a name="what-is-its-availability-by-region"></a>Wat is de beschikbaarheid per regio?
Data catalogusservices zijn momenteel beschikbaar in de volgende datacenters:

* VS - west
* VS - oost
* West-Europa
* Noord-Europa
* Australië - oost
* Zuidoost-Azië

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Wat zijn de beperkingen van het aantal gegevensassets?
De gratis editie van Data Catalog is beperkt tot 5.000 geregistreerde gegevensassets.

De Standard-editie van Data Catalog biedt ondersteuning voor maximaal 100.000 geregistreerde gegevensassets.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Wat zijn de ondersteunde bron- en asset gegevenstypen?
Zie voor een lijst met ondersteunde gegevensbronnen, [Data Catalog DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Hoe ik ondersteuning voor een andere gegevensbron aanvragen?
Als u functie-aanvragen en andere feedback, gaat u naar de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Hoe ga ik aan de slag met Data Catalog?
De beste manier aan de slag is door te gaan naar [aan de slag met Data Catalog](data-catalog-get-started.md). Dit artikel is een end-to-end-overzicht van de functies in de service.

## <a name="how-do-i-register-my-data"></a>Hoe registreer ik mijn gegevens?
Registreren van uw gegevens in Data Catalog:
1. In de Azure Data Catalog-portal in de **publiceren** gebied, start u het hulpprogramma Azure Data Catalog registratie. 
2. In de toepassing publiceren op Data Catalog, meld u aan met dezelfde referenties dat u toegang tot de Data Catalog-portal.
3. Selecteer de gegevensbron en de specifieke assets die u wilt registreren.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Welke eigenschappen wordt deze uitgepakt voor bedrijfsmiddelen die met gegevens die zijn geregistreerd
De specifieke eigenschappen afwijken van de gegevensbron met gegevensbron, maar in het algemeen de gegevenscatalogus publishing-service extraheert de volgende informatie:

* De Assetnaam van de
* Activatype
* Beschrijving Asset
* Kenmerk-/ kolomnamen
* Kenmerk-/ kolomgegevenstypen
* Beschrijving van het kenmerk/kolom

> [!IMPORTANT]
> Registreren van gegevensassets met Data Catalog niet verplaatsen of kopieer uw gegevens naar de cloud. De activa metagegevens activa van een gegevensbron registreren gekopieerd naar Azure, maar blijven de gegevens in de locatie van de bestaande gegevensbronnen. De uitzondering op deze regel is als u uploaden preview records of een profiel voor gegevens wilt tijdens de registratie van de activa. Wanneer u een voorbeeld van maximaal 20 records opneemt zijn gekopieerd vanaf elke asset en opgeslagen als een momentopname in Data Catalog. Als u een profiel gegevens opneemt, worden de verzamelde gegevens berekend en opgenomen in de metagegevens die zijn opgeslagen in de catalogus. Statistische informatie kan bestaan uit de grootte van tabellen, het percentage van null-waarden per kolom of de minimum, maximum en gemiddelde waarden voor kolommen. 
>
>

> [!NOTE]
> Voor gegevensbronnen zoals SQL Server Analysis Services die u een eersteklas hebt **beschrijving** eigenschap, de gegevenscatalogus toepassing publiceren die eigenschapwaarde haalt. Voor relationele databases van SQL Server, die een uitstekende gebrek **beschrijving** eigenschap, de catalogus met gegevens publiceren toepassing haalt de waarde van de **ms_description** uitgebreide eigenschap voor objecten en kolommen. Zie voor meer informatie [met behulp van uitgebreide eigenschappen voor databaseobjecten](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Hoe lang moet het duren zojuist geregistreerde assets worden weergegeven in de catalogus?
Nadat u activa met Data Catalog registreert, kan er een periode van 5 tot 10 seconden voordat ze worden weergegeven in de portal voor Data Catalog.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Hoe ik aantekeningen toevoegen aan en de metagegevens voor mijn geregistreerde gegevensassets verrijken?
De eenvoudigste manier metagegevens op te geven voor de geregistreerde assets is de asset selecteren in de Data Catalog-portal en voer vervolgens de waarden in het eigenschappendeelvenster of schema voor het geselecteerde object.

U kunt ook bepaalde metagegevens, zoals tags en experts opgeven tijdens het registratieproces. De waarden die u in de catalogus met gegevens publishing-service opgeeft van toepassing op alle activa op dat moment wordt geregistreerd. Als u wilt de onlangs geregistreerde objecten weergeven in de portal voor aanvullende opmerkingen, selecteer de **Portal weergeven** knop op het laatste scherm van de catalogus met gegevens publiceren van toepassing.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Hoe verwijder ik mijn objecten geregistreerde gegevens?
U kunt een object verwijderen uit Data Catalog door het object selecteren in de portal en vervolgens te klikken op de **verwijderen** knop. Verwijderen van het object, verwijdert u de metagegevens van Data Catalog, maar heeft geen invloed op de onderliggende gegevensbron.

## <a name="what-is-an-expert"></a>Wat is er een expert?
Een expert is een persoon die een perspectief op de hoogte over een gegevensobject heeft. Een object kan meerdere experts hebben. Een expert hoeft niet te worden van de 'eigenaar' voor een object, maar is gewoon iemand die weet hoe de gegevens kan en moet worden gebruikt.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Hoe moet ik informatie delen met het team van Data Catalog als ik problemen?
Problemen melden, gegevens, delen en vragen hebt, gaat u naar de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Werkt de catalogus met een andere gegevensbron die ik ben?
We proberen actief over het toevoegen van meer gegevensbronnen naar de gegevenscatalogus. Als u wilt zien van een specifieke gegevensbron ondersteund, raden deze (of stem de ondersteuning als deze al is voorgesteld) door te gaan naar de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Hoe Azure Data Catalog verbonden is met de catalogus met gegevens in Power BI voor Office 365?
U kunt Azure Data Catalog beschouwen als een evolutie van de catalogus met gegevens in Power BI. Vanaf versie die voorjaar 2017, wordt Azure Data Catalog gebruikt voor het inschakelen van het delen en de detectie van query's in Excel 2016 en Power Query voor Excel. Mogelijkheden van Data Catalog in Excel zijn beschikbaar voor gebruikers met Power BI Pro licenties.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Welke machtigingen heb ik nodig om te registreren van activa met Data Catalog?
Voor het uitvoeren van het hulpprogramma voor registratie van Data Catalog, moet u machtigingen voor de gegevensbron waarmee u de metagegevens van de bron lezen. Als u wilt ook een voorbeeld opnemen, moet u machtigingen voor lezen in de gegevens van de objecten die wordt geregistreerd hebben.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Data Catalog beschikbaar gesteld voor on-premises implementatie ook?
Data Catalog is een cloudservice waarmee zowel cloud als on-premises gegevensbronnen voor het leveren van een oplossing voor hybride gegevensbron detectie kunt werken. Er zijn momenteel geen plannen voor een versie van de Data Catalog-service die lokaal wordt uitgevoerd.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Kan ik meer of uitgebreidere metagegevens extraheren uit de gegevensbronnen die ik registreren?
We proberen actief als u de mogelijkheden van Data Catalog uitbreiden. Als u aanvullende metagegevens die zijn geëxtraheerd uit de gegevensbron tijdens de registratie hebt wilt, raden deze (of stem voor, als deze al is voorgesteld) in de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). In de toekomst kunnen we externe partijen nieuwe gegevensbrontypen via een API voor uitbreidbaarheid toevoegen.

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Hoe beperk ik de zichtbaarheid van geregistreerde gegevensassets, zodat alleen bepaalde personen kan ze detecteren?
Selecteer de gegevensassets die in de catalogus met gegevens en klik vervolgens op de **eigenaar** knop. Eigenaars van gegevensassets in Data Catalog kunnen de instellingen voor zichtbaarheid ofwel dat alle gebruikers kunnen het eigendom activa te detecteren of zichtbaarheid beperken tot specifieke gebruikers wijzigen.

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Hoe kan ik de registratie voor een gegevensasset bijwerken zodat de wijzigingen in de gegevensbron worden weergegeven in de catalogus?
Voor het bijwerken van de metagegevens voor de gegevensassets die al zijn geregistreerd in de catalogus gewoon Registreer opnieuw de gegevensbron met de activa. Eventuele wijzigingen in de gegevensbron, zoals kolommen worden toegevoegd of verwijderd uit de tabellen of weergaven, worden bijgewerkt in de catalogus, maar alle aantekeningen die door gebruikers worden bewaard.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Mijn vraag niet wordt hier beantwoord. Waar kan ik voor antwoorden gaan?
Ga naar de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Er vragen vindt u hier hun manier.
