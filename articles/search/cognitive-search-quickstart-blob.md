---
title: 'Quickstart: Bouw een AI-verrijkt index in Azure portal - Azure Search'
description: Ophalen van gegevens, natuurlijke taal en afbeeldingsverwerking vaardigheden in een Azure Search-indexering portal met behulp van de Azure portal en sample van gegevens.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 07/09/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8f3a1dadaddb423a83f4c3691a4b5747a5196d2a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795336"
---
# <a name="quickstart-create-an-ai-indexing-pipeline-using-cognitive-skills-in-azure-search"></a>Quickstart: Een AI-pijplijn indexeren met cognitieve vaardigheden in Azure Search

Azure Search kan worden geïntegreerd met [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/), uitpakken van inhoud, natuurlijke taalverwerking (NLP) en afbeelding verwerking vaardigheden toe te voegen aan een Azure Search indexeren pijplijn, waardoor unsearchable of ongestructureerde inhoud meer doorzoekbaar. 

Veel bronnen voor Cognitive Services - zoals [OCR](cognitive-search-skill-ocr.md), [taaldetectie](cognitive-search-skill-language-detection.md), [entiteit erkenning](cognitive-search-skill-entity-recognition.md) om de naam van een paar - kan worden gekoppeld aan een indexeringsproces worden geautomatiseerd. De AI-algoritmen van Cognitive Services worden gebruikt om patronen, functies en kenmerken in brongegevens te vinden en structuren en tekstuele inhoud te retourneren voor gebruik in oplossingen voor zoekopdrachten in volledige tekst op basis van Azure Search.

Maak in deze snelstart uw eerste verrijkingspijplijn in [Azure Portal](https://portal.azure.com) voordat u ook maar één regel code schrijft:

> [!div class="checklist"]
> * Begin met voorbeeldgegevens in Azure Blob-opslag
> * Configureer de wizard [**Gegevens importeren**](search-import-data-portal.md) voor cognitieve indexering en verrijking 
> * Voer de wizard uit (een entiteitsvaardigheid detecteert mensen, locatie en organisaties)
> * Gebruik [**Search Explorer**](search-explorer.md) om query's op de verrijkte gegevens uit te voeren

Deze snelstartgids wordt uitgevoerd op de gratis service, maar het aantal gratis transacties is beperkt tot 20 documenten per dag. Als u wilt uitvoeren in deze Quick Start van meer dan eenmaal per dag, gebruikt een kleiner bestand instellen, zodat u in meer uitvoeringen past.

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start.

[Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) biedt de AI. In deze quickstart bevat stappen voor het toevoegen van deze resources in de regel, bij het opgeven van de pijplijn. Het is niet nodig voor het vooraf instellen van accounts.

Azure-services zijn vereist voor de invoer voor de pijplijn voor indexering. U kunt elke willekeurige gegevensbron wordt ondersteund door [Azure Search-indexeerfuncties](search-indexer-overview.md) , met uitzondering van Azure Table Storage, die niet wordt ondersteund voor AI-indexering. Deze snelstartgids maakt gebruik van [Azure Blob-opslag](https://azure.microsoft.com/services/storage/blobs/) als een container voor de bronbestanden van gegevens. 

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Azure Blob service instellen en voorbeeldgegevens laden

1. [Download de voorbeeldgegevens](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) die bestaan uit een kleine set van verschillende typen bestanden. 

1. [Aanmelden voor Azure Blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal), een opslagaccount maken, opent u de services Blob's en een container maken.  De storage-account maken in dezelfde regio als de Azure Search.

1. Klik in de container die u hebt gemaakt op **Uploaden** om de voorbeeldbestanden te uploaden die u in een vorige stap hebt gedownload.

   ![Bronbestanden in Azure-blobopslag](./media/cognitive-search-quickstart-blob/sample-data.png)

## <a name="create-the-enrichment-pipeline"></a>De verrijkingspijplijn maken

Ga terug naar de dashboardpagina van de Azure Search-service en klik in de opdrachtbalk op **Gegevens importeren** om in vier stappen cognitieve verrijking in te stellen.

  ![Opdracht Gegevens importeren](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken

In **Verbinden met uw gegevens** kiest u **Azure Blob Storage** en selecteert u het account dat en de container die u hebt gemaakt. Geef een naam op voor de gegevensbron en gebruik standaardwaarden voor de rest. 

  ![Azure-blobconfiguratie](./media/cognitive-search-quickstart-blob/blob-datasource.png)

Ga door naar de volgende pagina.

  ![De knop volgende pagina voor Cognitive Search](media/cognitive-search-quickstart-blob/next-button-add-cog-search.png)

### <a name="step-2-add-cognitive-skills"></a>Stap 2: Cognitieve vaardigheden toevoegen

Voeg vervolgens verrijkingsstappen toevoegen aan de pijplijn voor indexering. Als u geen Cognitive Services-resource hebt, kunt u zich aanmelden voor een gratis versie met twintig transacties per dag. Omdat de voorbeeldgegevens uit veertien bestanden bestaan, verbruikt u met het uitvoeren van deze wizard een groot deel van uw dagelijkse toewijzing.

1. Vouw **Cognitive Services toevoegen** uit om opties voor brontoewijzing van de Cognitive Services-API's weer te geven. Voor deze zelfstudie kunt u de resource **Gratis** gebruiken.

   ![Cognitive Services toevoegen](media/cognitive-search-quickstart-blob/cog-search-attach.png)

2. Vouw **Verrijkingen toevoegen** uit en selecteer vaardigheden voor het verwerken van natuurlijke taal. Kies bij deze snelstart voor entiteitsherkenning voor personen, organisaties en locaties.

   ![Cognitive Services toevoegen](media/cognitive-search-quickstart-blob/skillset.png)

   De portal biedt ingebouwde vaardigheden voor OCR-analyse en tekstanalyse. In de portal werkt een set vaardigheden via één bronveld. Dat lijkt misschien een klein doel, maar voor Azure-blobs bevat het veld `content` het meeste van het blobdocument (bijvoorbeeld een Word-document of PowerPoint-presentatie). Dit veld is een ideale invoer omdat het alle inhoud van een blob bevat.

3. Ga door naar de volgende pagina.

   ![Volgende pagina Index aanpassen](media/cognitive-search-quickstart-blob/next-button-customize-index.png)

> [!NOTE]
> Vaardigheden voor verwerking van natuurlijke taal werken met behulp van tekstinhoud in de set voorbeeldgegevens. Omdat we geen OCR-optie hebben geselecteerd, worden de JPEG- en PNG-bestanden die worden gevonden in de set voorbeeldgegevens niet verwerkt in deze snelstart. 

### <a name="step-3-configure-the-index"></a>Stap 3: De index configureren

De wizard kan gewoonlijk een standaardindex afleiden. U kunt in deze stap het gegenereerde indexschema bekijken en desgewenst instellingen wijzigen. Hieronder ziet u de standaardindex die voor de demoset Blobgegevens is gemaakt.

De wizard biedt goede standaardinstellingen voor deze snelstart: 

+ De standaardnaam is *azureblob-index*, gebaseerd op het type gegevensbron. 

+ De standaardvelden worden gebaseerd op het oorspronkelijke brongegevensveld (`content`), plus de uitvoervelden (`people`, `organizations` en `locations`) die zijn gemaakt door de cognitieve pijplijn. De standaardgegevenstypen worden afgeleid van metagegevens en steekproeven van gegevens.

+ De standaardsleutel is *metadata_storage_path* (dit veld bevat unieke waarden).

+ **Ophaalbaar** en **Doorzoekbaar** zijn standaardkenmerken voor deze velden. **Doorzoekbaar** geeft aan dat een veld kan worden doorzocht. **Ophaalbaar** betekent dat het in resultaten kan worden geretourneerd. De wizard gaat ervan uit dat deze velden ophaalbaar en doorzoekbaar moeten zijn omdat u ze hebt gemaakt via een set vaardigheden.

  ![Indexvelden](media/cognitive-search-quickstart-blob/index-fields.png)

Let op het doorgehaalde vinkje en het vraagteken in de kolom **Ophaalbaar** voor het veld `content`. Voor blob-documenten met veel tekst bevat het veld `content` het grootste deel van het bestand, mogelijk wel duizenden regels. Als u de inhoud van een bestand wilt doorgeven aan clientcode, moet **Ophaalbaar** geselecteerd blijven. Anders kunt u dit kenmerk uitschakelen voor `content` als de uitgepakte elementen (`people`, `organizations` en `locations`) voldoende zijn voor uw doeleinden.

Als u een veld markeert als **Ophaalbaar**, betekent niet dat het veld aanwezig *moet* zijn in de lijst met zoekresultaten. U kunt de samenstelling van zoekresultaten nauwkeurig beheren met behulp van de queryparameter **$select** door op te geven welke velden u wilt opnemen. Voor velden met veel tekst zoals `content`, is de parameter **$select** de oplossing voor het aanbieden van beheersbare zoekresultaten aan de gebruikers van uw toepassing, terwijl clientcode via het kenmerk **Ophaalbaar** toegang heeft tot alle gegevens die nodig zijn.
  
Ga door naar de volgende pagina.

  ![Volgende pagina Indexeerfunctie maken](media/cognitive-search-quickstart-blob/next-button-create-indexer.png)

### <a name="step-4-configure-the-indexer"></a>Stap 4: De indexeerfunctie configureren

De indexeerfunctie is een belangrijke resource die het indexeerproces aandrijft. Hiermee specificeert u de naam van de gegevensbron, een doelindex en de uitvoerfrequentie. Het eindresultaat van de wizard **Gegevens importeren** is altijd een indexeerfunctie die u herhaaldelijk kunt uitvoeren.

Op de pagina **Indexeerfunctie** kunt u de standaardnaam accepteren en de schemaoptie **Eén keer uitvoeren** gebruiken om de indexeerfunctie direct uit te voeren. 

  ![Definitie van de indexeerfunctie](media/cognitive-search-quickstart-blob/indexer-def.png)

Klik op **Verzenden** om de indexeerfunctie te maken en tegelijkertijd uit te voeren.

## <a name="monitor-indexing"></a>Het indexeren bewaken

Verrijkingsstappen vergen meer tijd dan gebruikelijke indexering op basis van tekst. De lijst van de indexeerfunctie moet door de wizard worden geopend op de overzichtspagina, zodat u de voortgang kunt volgen. Voor zelfnavigatie gaat u naar de overzichtspagina en klikt u op **Indexeerfuncties**.

De waarschuwing wordt weergegeven omdat JPG- en PNG-bestanden afbeeldingsbestanden zijn en we de OCR-vaardigheid voor deze pijplijn hebben weggelaten. U krijgt ook afkappingsmeldingen. In Azure Search is de extractie beperkt tot 32.000 tekens in de laag Gratis.

  ![Azure Search-melding](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Indexeren en verrijken kunnen even duren. Daarom is het aan te raden om kleinere gegevenssets te gebruiken voor verkenning. 

## <a name="query-in-search-explorer"></a>Query uitvoeren in Search Explorer

Nadat er een index is gemaakt, kunt u query's uitvoeren om documenten te retourneren uit de index. Gebruik **Search Explorer** in de portal om query's uit te voeren en resultaten te bekijken. 

1. Klik op de dashboardpagina van de zoekservice op **Search Explorer** in de opdrachtbalk.

1. Selecteer bovenaan **Index wijzigen** om de index die u hebt gemaakt te selecteren.

1. Voer een zoekopdracht in om een query op de index toe te passen, zoals `search=Microsoft&searchFields=organizations`.

Resultaten worden in JSON geretourneerd, wat uitgebreid en moeilijk te lezen kan zijn, met name in grote documenten die afkomstig zijn van Azure-blobs. Als u niet eenvoudig resultaten kunt scannen, gebruikt u CTRL+F om in documenten te zoeken. Voor deze query kunt u in de JSON zoeken naar specifieke voorwaarden. 

Met CTRL+F kunt u ook bepalen hoeveel documenten een bepaalde resultatenset bevat. Voor Azure-blobs kiest de portal 'metadata_storage_path' als de sleutel omdat elke waarde uniek voor het document is. Zoek met CTRL+F naar 'metadata_storage_path' om het aantal documenten te tellen. 

  ![Voorbeeld Search Explorer](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Opgedane kennis

U hebt nu uw eerste oefening in indexeren met cognitieve verrijking voltooid. Het doel van deze snelstart is om belangrijke concepten te introduceren en u de wizard te laten doorlopen, zodat u snel een oplossing voor cognitief zoeken kunt maken met behulp van uw eigen gegevens.

Een van de belangrijke concepten die we wilden overbrengen, is de afhankelijkheid van Azure-gegevensbronnen. Verrijking van cognitief zoeken is gebonden aan indexeerfuncties, en indexeerfuncties zijn Azure- en bronspecifiek. Hoewel deze snelstart Azure Blob-opslag gebruikt, zijn andere Azure-gegevensbronnen ook mogelijk. Zie [Indexeerfuncties in Azure Search](search-indexer-overview.md) voor meer informatie.

Een ander belangrijk concept is dat vaardigheden via invoervelden werken. In de portal moet u één bronveld voor alle vaardigheden kiezen. Invoeren kunnen in code andere velden zijn, of de uitvoer van een upstream-vaardigheid.

 Invoeren voor een vaardigheid worden toegewezen aan een uitvoerveld in een index. Intern stelt de portal [aantekeningen](cognitive-search-concept-annotations-syntax.md) in en definieert een [set vaardigheden](cognitive-search-defining-skillset.md), waarmee de volgorde van bewerkingen en de algemene stroom wordt bepaald. Deze stappen zijn verborgen in de portal, maar wanneer u begint met het schrijven van code worden deze concepten belangrijk.

Tot slot hebt u geleerd dat u resultaten kunt bekijken door query's in de index uit te voeren. Azure Search biedt eigenlijk een doorzoekbare index, waarin u query's kunt uitvoeren met behulp van de [eenvoudige](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) of [volledig uitgebreide querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). Een index met verrijkte velden werkt zoals elke andere index. Als u standaard of [aangepaste analyse](search-analyzers.md), [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [synoniemen](search-synonyms.md), [meervoudige filters](search-filters-facets.md), geografisch zoeken of een andere functie van Azure Search wilt opnemen, kunt u dat gewoon doen.

## <a name="clean-up"></a>Opruimen

Wanneer u in uw eigen abonnement werkt, is het een goed idee aan het einde van een project om te bepalen of u moet nog steeds de resources die dat u hebt gemaakt. Resources naar links wordt uitgevoerd kan kosten u geld. U kunt afzonderlijke resources verwijderen of verwijder de resourcegroep als u wilt verwijderen van de volledige set van resources.

U kunt zoeken en beheren van resources in de portal, met behulp van de **alle resources** of **resourcegroepen** koppeling in het deelvenster navigatie aan de linkerkant.

Als u van een gratis service gebruikmaakt, houd er rekening mee dat u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen bent. U kunt afzonderlijke items in de portal om te blijven onder de limiet verwijderen. 

## <a name="next-steps"></a>Volgende stappen

Afhankelijk van hoe u de Cognitive Services-resource hebt ingericht, kunt u experimenteren met indexering en verrijking door de wizard opnieuw uit te voeren met verschillende vaardigheden en brongegevensvelden. Herhaal de stappen door de index en indexeerfunctie te verwijderen en vervolgens de indexeerfunctie opnieuw te maken met nieuwe selecties.

+ Selecteer in **Overzicht** > **Indexen** de index die u hebt gemaakt en klik vervolgens op **Verwijderen**.

+ Dubbelklik in **Overzicht** op de tegel **Indexeerfuncties**. Zoek de indexeerfunctie die u hebt gemaakt en verwijder deze.

U kunt ook de voorbeeldgegevens en de services die u hebt gemaakt opnieuw gebruiken in de volgende zelfstudie om meer te weten te komen over het programmatisch uitvoeren van dezelfde taken. 

> [!div class="nextstepaction"]
> [Zelfstudie: De REST API's voor cognitief zoeken gebruiken](cognitive-search-tutorial-blob.md)
