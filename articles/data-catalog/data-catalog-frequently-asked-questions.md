---
title: Veelgestelde vragen over Azure Data Catalog
description: Veelgestelde vragen over Azure Data Catalog, met inbegrip van mogelijkheden voor detectie van gegevensbronnen, aantekening en beheren.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 9aaba38b7dc0cd2bf659c9d91c9d7660bd17a6dd
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409181"
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Veelgestelde vragen over Azure Data Catalog
In dit artikel vindt u antwoorden op veelgestelde vragen met betrekking tot de service Azure Data Catalog.

## <a name="what-is-azure-data-catalog"></a>Wat is Azure Data Catalog?
Data Catalog is een volledig beheerde service, die wordt gehost in Microsoft Azure, die als een systeem van registratie en detectie van bedrijfsgegevensassets fungeert. Met Data Catalog, kunt een willekeurige gebruiker, van analisten tot gegevenswetenschappers en ontwikkelaars, registreren, ontdekken, begrijpen, gegevensbronnen en gebruiken.

## <a name="what-customer-challenges-does-it-solve"></a>Welke klant uitdagingen doet het allemaal oplossen?
Data Catalog uitdagingen het hoofd van gegevensbron detectie en de 'donkere gegevens' zodat gebruikers kunnen detecteren en te begrijpen van zakelijke gegevensbronnen.

## <a name="what-are-its-target-audiences"></a>Wat zijn de doelgroepen?
Data Catalog is ontworpen voor technische en niet-technische gebruikers, met inbegrip van:

* Ontwikkelaars kunnen en BI- en analyse-professionals: personen die verantwoordelijk zijn voor het maken van gegevens en analyses inhoud zodat anderen deze kunnen gebruiken.
* Gegevens stewards: personen die de kennis op over de gegevens, wat het betekent en hoe deze is bedoeld om te worden gebruikt.
* Gegevensgebruikers: mensen die eraan moeten kunnen eenvoudig ontdekken, begrijpen en verbinding maken met de gegevens die ze nodig hebben voor hun werk doen met behulp van het hulpprogramma van hun keuze.
* Centrale IT: mensen die nodig hebben om honderden gegevensbronnen kunnen worden gedetecteerd door zakelijke gebruikers en die moeten toezicht over hoe gegevens worden gebruikt en door wie onderhouden.

## <a name="what-is-its-availability-by-region"></a>Wat is de beschikbaarheid per regio?
Data Catalog-services zijn momenteel beschikbaar in de volgende datacenters:

* US - west
* US - oost
* Europa -west
* Europa - noord
* Australië - oost
* Azië - zuidoost

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Wat zijn de limieten voor het aantal gegevensassets?
De gratis editie van Data Catalog is beperkt tot 5000 geregistreerde gegevensassets.

De Standard-editie van Data Catalog biedt ondersteuning voor maximaal 100.000 geregistreerde gegevensassets.

Een object geregistreerd in Data Catalog, zoals tabellen, weergaven, bestanden en rapporten, telt als een gegevensasset.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Wat zijn de ondersteunde bron- en asset gegevenstypen?
Zie voor een lijst met ondersteunde gegevensbronnen, [Data Catalog DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Hoe ik vragen om ondersteuning voor een andere gegevensbron?
Als u wilt functieverzoeken en andere feedback indienen, gaat u naar de [gegevenscatalogus op de Azure-Forums voor Feedback](https://feedback.azure.com/forums/906052-data-catalog/category/320788-data-sources).

## <a name="how-do-i-get-started-with-data-catalog"></a>Hoe ga ik aan de slag met Data Catalog?
De beste manier om te beginnen is door te gaan naar [aan de slag met Data Catalog](data-catalog-get-started.md). Dit artikel is een end-to-end-overzicht van de mogelijkheden van de service.

## <a name="how-do-i-register-my-data"></a>Hoe kan ik mijn gegevens registreren?
Uw gegevens in Data Catalog registreren:
1. In de Azure Data Catalog-portal in de **publiceren** gebied, start het hulpprogramma voor registratie van Azure Data Catalog. 
2. In de Data Catalog registratiehulpprogramma voor gegevensbronnen, meld u aan met dezelfde referenties die u gebruiken voor toegang tot de Data Catalog-portal.
3. Selecteer de gegevensbron en de specifieke activa die u wilt registreren.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Welke eigenschappen wordt deze uitgepakt voor gegevens die zijn geregistreerd?
De specifieke eigenschappen van gegevensbron tot gegevensbron verschillen, maar in het algemeen de service voor het publiceren van Data Catalog haalt de volgende informatie:

* Assetnaam
* Activatype
* Beschrijving van de Asset
* Kenmerk-/ kolomnamen
* Kenmerk/kolom-gegevenstypen
* Kenmerk-/ kolombeschrijving

> [!IMPORTANT]
> Gegevensassets registreren met Data Catalog niet verplaatsen of kopiëren van uw gegevens naar de cloud. Elementen van een gegevensbron registreren de activa metagegevens worden gekopieerd naar Azure, maar blijven de gegevens in de bestaande locatie van de gegevensbron. De uitzondering op deze regel is als u ervoor kiezen om te uploaden preview records of een gegevensprofiel voor tijdens de registratie van de activa. Wanneer het opnemen van een Preview-versie, maximaal 20 records worden opgehaald uit elke asset en opgeslagen als een momentopname in Data Catalog. Als u een gegevensprofiel worden weergegeven, is de verzamelde gegevens berekend en de opgenomen in de metagegevens die zijn opgeslagen in de catalogus. Statistische informatie kan bestaan uit de grootte van tabellen, het percentage van de null-waarden per kolom of de minimale, maximale en gemiddelde waarden voor kolommen. 
>
>

> [!NOTE]
> Voor gegevensbronnen, zoals SQL Server Analysis Services die u een eersteklas hebt **beschrijving** eigenschap, de Data Catalog hulpprogramma voor registratie extraheert waarde van die eigenschap. Voor relationele databases van SQL Server, die een eersteklas ontbreken **beschrijving** eigenschap, de Data Catalog hulpprogramma voor registratie extraheert de waarde van de **ms_description** uitgebreide eigenschap voor objecten en kolommen. Zie voor meer informatie, [met behulp van uitgebreide eigenschappen voor databaseobjecten](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Hoe lang het duurt voor de zojuist geregistreerde assets om te worden weergegeven in de catalogus?
Nadat u activa met Data Catalog registreert, is het mogelijk dat er een periode van 5 tot 10 seconden voordat ze worden weergegeven in de Data Catalog-portal.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Hoe kan ik van aantekeningen voorzien en verrijken van de metagegevens voor mijn geregistreerde gegevensassets?
De eenvoudigste manier voor metagegevens voor geregistreerde assets is voor het selecteren van de asset in de Data Catalog-portal en voer vervolgens de waarden in het eigenschappendeelvenster of schema voor het geselecteerde object.

U kunt ook een aantal metagegevens, zoals experts en tags, opgeven tijdens het registratieproces. De waarden die u in de Data Catalog-publicatieservice opgeeft zijn van toepassing op alle activa op dat moment wordt geregistreerd. Als u wilt de onlangs geregistreerde objecten in de portal voor aanvullende opmerkingen bekijken, selecteert u de **Portal weergeven** knop op het laatste scherm van het registratiehulpprogramma voor Data Catalog gegevensbronnen.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Hoe kan ik mijn geregistreerde objecten verwijderen?
U kunt een object verwijderen uit de gegevenscatalogus door het object selecteren in de portal en vervolgens te klikken op de **verwijderen** knop. Het object verwijderen, verwijdert u de metagegevens van de van Data Catalog, maar heeft geen invloed op de onderliggende gegevensbron.

## <a name="what-is-an-expert"></a>Wat is er een expert?
Een expert is een persoon die een perspectief op de hoogte over een gegevensobject. Een object kan meerdere experts hebben. Een expert hoeft niet te worden van de 'eigenaar' voor een object, maar is gewoon iemand die weet hoe de gegevens kan en moet worden gebruikt.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Hoe kan ik delen informatie met de Data Catalog-team als ik problemen ondervindt?
Problemen melden, informatie delen, en stel vragen, gaat u naar de [forum van Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Werkt de catalogus met een andere gegevensbron die ik ben geïnteresseerd in?
We werken er hard aan meer gegevensbronnen toe te voegen aan Data Catalog. Als u wilt weergeven van een specifieke gegevensbron wordt ondersteund, suggestie (of het ondersteuningsteam van uw stem als deze al is voorgesteld) door te gaan naar de [gegevenscatalogus op de Azure-Forums voor Feedback](https://feedback.azure.com/forums/906052-data-catalog).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Hoe wordt Azure Data Catalog met betrekking tot de catalogus met gegevens in Power BI voor Office 365?
U kunt Azure Data Catalog beschouwen als een verbetering van de catalogus met gegevens in Power BI. Vanaf het voorjaar van 2017, wordt Azure Data Catalog gebruikt voor het delen en detectie van query's in Excel 2016 en Power Query voor Excel. Mogelijkheden van Data Catalog in Excel zijn beschikbaar voor gebruikers met Power BI Pro-licenties.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Welke machtigingen heb ik nodig voor het registreren van activa met Data Catalog?
Als u wilt uitvoeren van het hulpprogramma voor registratie van Data Catalog, moet u machtigingen voor de gegevensbron waarmee u kunt de metagegevens van de bron lezen. Inclusief een Preview-versie, moet u machtigingen kunt u lezen in de gegevens van de objecten die wordt geregistreerd hebben.

Data Catalog kan ook catalogusbeheerders om te beperken welke gebruikers en groepen metagegevens aan de catalogus toevoegen kunnen. Zie voor meer informatie, [over het beveiligen van toegang tot de gegevenscatalogus en gegevensassets](data-catalog-how-to-secure-catalog.md).

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Data Catalog beschikbaar gesteld voor on-premises implementatie ook?
Data Catalog is een cloudservice waarmee zowel cloud en on-premises gegevensbronnen te bieden een oplossing voor hybride gegevensbron detectie kunt werken. Er zijn momenteel geen plannen voor een versie van de Data Catalog-service die wordt uitgevoerd on-premises.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Kan ik meer of uitgebreide metagegevens te extraheren uit de gegevensbronnen die kan ik registreren?
We werken er hard om uit te breiden, de mogelijkheden van Data Catalog. Als u wilt dat aanvullende metagegevens geëxtraheerd uit de gegevensbron tijdens de registratie, voorstellen dat (of stem voor deze, als deze al is voorgesteld) in de [gegevenscatalogus op de Azure-Forums voor Feedback](https://feedback.azure.com/forums/906052-data-catalog). 

Als u wilt opnemen kolom/schema metagegevens, voorlopige versies of gegevens-profielen voor gegevensbronnen waar deze metagegevens niet wordt opgehaald door het registratiehulpprogramma voor gegevensbronnen, kunt u de API van Data Catalog om toe te voegen deze metagegevens. Zie voor meer informatie, [REST-API van Azure Data Catalog](https://docs.microsoft.com/rest/api/datacatalog/).

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Hoe beperk ik de zichtbaarheid van geregistreerde gegevensassets, zodat alleen bepaalde personen kunnen ze detecteren?
Selecteer de gegevensassets die in de catalogus met gegevens en klik vervolgens op de **eigenaar** knop. Eigenaars van gegevensassets in Data Catalog, kunnen de instellingen van de zichtbaarheid ofwel zodat alle gebruikers kunnen het eigendom assets detecteren of zichtbaarheid beperken tot specifieke gebruikers wijzigen. Zie voor meer informatie, [gegevensassets in Azure Data Catalog beheren](data-catalog-how-to-manage.md).

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Hoe kan ik de registratie voor een gegevensasset bijwerken zodat wijzigingen in de gegevensbron worden weergegeven in de catalogus?
Voor het bijwerken van de metagegevens voor de gegevensassets die al zijn geregistreerd in de catalogus, gewoon Registreer opnieuw de gegevensbron met de activa. Alle wijzigingen die in de gegevensbron, zoals kolommen worden toegevoegd of verwijderd uit de tabellen of weergaven, zijn bijgewerkt in de catalogus, maar aantekeningen die door gebruikers worden bewaard.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Mijn vraag niet wordt hier beantwoord. Waar vind ik antwoorden?
Ga naar de [forum van Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Er vragen vindt hier aankomen.
