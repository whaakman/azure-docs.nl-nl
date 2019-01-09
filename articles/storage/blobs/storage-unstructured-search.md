---
title: 'Zelfstudie: Niet-gestructureerde gegevens doorzoeken in Azure Blob Storage'
description: 'Zelfstudie: Niet-gestructureerde gegevens doorzoeken in Blob Storage met behulp van Azure Search.'
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 12/13/2018
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: 42c67d73ee776488fbe932676f61cb7166c2984b
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599829"
---
# <a name="tutorial-search-unstructured-data-in-cloud-storage"></a>Zelfstudie: Niet-gestructureerde gegevens in cloudopslag doorzoeken

In deze zelfstudie leert u hoe u niet-gestructureerde gegevens kunt doorzoeken met [Azure Search](../../search/search-what-is-azure-search.md), waarbij gegevens worden gebruikt die zijn opgeslagen in Azure Blob Storage. Niet-gestructureerde gegevens zijn gegevens die niet zijn geordend op een vooraf gedefinieerde manier of waarvoor geen gegevensmodel bestaat. Een voorbeeld is een .txt-bestand.

Voor deze zelfstudie hebt u een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep maken
> * Create a storage account
> * Een container maken
> * Gegevens uploaden naar de container
> * Een zoekservice via de portal maken
> * Een zoekservice verbinden met een opslagaccount
> * Een gegevensbron maken
> * De index configureren
> * Een indexeerfunctie maken
> * De zoekservice gebruiken om uw container te doorzoeken

## <a name="prerequisites"></a>Vereisten

Elk opslagaccount moet behoren tot een Azure-resourcegroep. Een resourcegroep is een logische container voor het groeperen van uw Azure-services. Wanneer u een opslagaccount maakt, kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken. In deze zelfstudie maakt u een nieuwe resourcegroep.

Meld u aan bij [Azure Portal](http://portal.azure.com).

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

Er is een voorbeeldgegevensset voor u voorbereid, zodat u er in deze zelfstudie gebruik van kunt maken. Download [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip) en pak het uit in een eigen map.

Het voorbeeld bestaat uit tekstbestanden, verkregen via [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). In deze zelfstudie worden ze gebruikt als voorbeeldtekstbestanden die worden doorzocht met behulp van Azure Search-services.

## <a name="create-a-container"></a>Een container maken

Containers zijn vergelijkbaar met mappen en worden gebruikt voor het opslaan van blobs.

Voor deze zelfstudie gebruikt u een enkele container om de tekstbestanden in op te slaan die zijn opgehaald uit clinicaltrials.gov.

1. Ga in de Azure-portal naar uw opslagaccount.

2. Selecteer **Door blobs bladeren** onder **Blob-service**.

3. Voeg een nieuwe container toe.

4. Noem de container **gegevens** en selecteer **Container** als het openbare toegangsniveau.

5. Selecteer **OK** om de container te maken.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>De voorbeeldgegevens uploaden

Nu u een container hebt, kunt u uw voorbeeldgegevens erin uploaden.

1. Selecteer de container en selecteer **Uploaden**.

2. Selecteer het pictogram met de blauwe map naast het veld **Bestanden** en blader naar de lokale map waarin u de voorbeeldgegevens hebt uitgepakt.

3. Selecteer alle uitgepakte bestanden en selecteer **Openen**.

4. Selecteer **Uploaden** om te beginnen met uploaden.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/upload.png)

Het uploadproces kan even duren.

Nadat dit is voltooid, gaat u terug naar de gegevenscontainer om te controleren of de tekstbestanden zijn geüpload.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Een zoekservice maken

Azure Search is een SaaS-cloudoplossing (Search-as-a-Service) die ontwikkelaars API's en hulpprogramma's biedt waarmee ze een zoekervaring kunnen toevoegen over uw gegevens.

Voor deze zelfstudie gebruikt u een zoekservice om te zoeken in de tekstbestanden die zijn opgehaald van clinicaltrials.gov.

1. Ga in de Azure-portal naar uw opslagaccount.

2. Schuif naar beneden en selecteer **Azure Search toevoegen** onder **BLOB-SERVICE**.

3. Onder **Gegevens importeren** selecteert u **Kies een service**.

4. Selecteer **Klik hier om een nieuwe zoekservice te maken**.

5. Voer in **Nieuwe zoekservice** een unieke naam in voor uw zoekservice in het **URL**-veld.

6. Selecteer bij **Resourcegroep** **Bestaande gebruiken** en kies de resourcegroep die u eerder hebt gemaakt.

7. Selecteer bij **Prijscategorie** de **Gratis** laag en klik op **Selecteren**.

8. Selecteer **Maken** om de zoekservice te maken.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Uw zoekservice verbinden met de container

Nu u over een zoekservice beschikt, kunt u deze koppelen aan Blob Storage. In deze sectie wordt stapsgewijs uitgelegd hoe u een gegevensbron kunt kiezen en een index en een indexeerfunctie kunt maken.

1. Ga naar uw opslagaccount.

2. Selecteer **Azure Search toevoegen** onder **BLOB-SERVICE**.

3. Selecteer **Search Service** in **Gegevens importeren** en klik vervolgens op de zoekservice die u in het vorige gedeelte hebt gemaakt. Hiermee wordt **Nieuwe gegevensbron** geopend.

### <a name="create-a-data-source"></a>Een gegevensbron maken

  Een gegevensbron geeft aan welke gegevens moeten worden geïndexeerd en hoe u toegang tot de gegevens kunt krijgen. Een gegevensbron kan meerdere keren worden gebruikt door dezelfde searchservice.

1. Voer een naam in voor de gegevensbron. Bij **Gegevens die moeten worden geëxtraheerd** selecteert u **Inhoud en metagegevens**. De gegevensbron geeft aan welke onderdelen van de blob worden geïndexeerd.

2. Omdat de blobs die u gebruikt tekstbestanden zijn, moet u **Parseermodus** instellen op **Tekst**.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/datasources.png)

3. Selecteer **Opslagcontainer** om de beschikbare opslagaccounts weer te geven.

4. Selecteer uw opslagaccount en selecteer vervolgens de container die u eerder hebt gemaakt.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/datacontainer.png)

5. Klik op **Selecteren** om terug te keren naar **Nieuwe gegevensbron** en selecteer **OK** om door te gaan.

### <a name="configure-the-index"></a>De index configureren

  Een index is een verzameling velden afkomstig uit uw gegevensbron die kan worden doorzocht. U stelt parameters voor deze velden in en configureert ze om aan te geven op welke manieren uw zoekservice de gegevens moet doorzoeken.

1. Bij **Gegevens importeren** selecteert u **Doelindex aanpassen**.

2. Typ een naam voor de index in het veld **Indexnaam**.

3. Schakel het selectievakje van het kenmerk **Ophalen mogelijk** bij **metadata_storage_name** in.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/valuestoselect.png)

4. Selecteer **OK**. **Een indexeerfunctie maken** wordt weergegeven.

De parameters van de index en de kenmerken die u aan deze parameters toewijst zijn belangrijk. De parameters geven aan *welke* gegevens moeten worden opgeslagen en de kenmerken geven aan *hoe* die gegevens moeten worden opgeslagen.

De kolom **VELDNAAM** bevat de parameters. De volgende tabel bevat een overzicht van de beschikbare kenmerken en de bijbehorende beschrijvingen.

#### <a name="field-attributes"></a>Veldkenmerken

| Kenmerk | Beschrijving |
| --- | --- |
| *Sleutel* |Een tekenreeks met de unieke id van elk document. Deze reeks wordt gebruikt om op te zoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld gebruiken in een meervoudige navigatiestructuur om op de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |

### <a name="create-an-indexer"></a>Een indexeerfunctie maken

  Een indexeerfunctie verbindt een gegevensbron met een zoekindex en biedt een schema waarmee uw gegevens opnieuw kunnen worden geïndexeerd.

1. Typ een naam in het veld **Naam** en selecteer **OK**.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/exindexer.png)

2. U komt terug bij **Gegevens importeren**. Selecteer **OK** om het verbindingsproces te voltooien.

U hebt uw blob nu met uw zoekservice verbonden. Het duurt een paar minuten voordat op de portal zichtbaar is dat de index is ingevuld. De zoekservice begint echter onmiddellijk met indexeren, zodat u meteen met zoeken kunt beginnen.

## <a name="search-your-text-files"></a>Uw tekstbestanden doorzoeken

Als u uw bestanden wilt doorzoeken, opent u Search Explorer in de index van de zoekservice die zojuist is gemaakt.

De volgende stappen laten zien u waar u Search Explorer en een aantal query's kunt vinden:

1. Ga naar alle resources en zoek naar de zoekservice die zojuist is gemaakt.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/exampleurl.png)

2. Selecteer de index en open deze.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/overview.png)

3. Selecteer **Search Explorer** om deze te openen. Hier kunt u livequery's op uw gegevens uitvoeren.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/indexespane.png)

4. Selecteer **Zoeken** terwijl het veld voor de queryreeks leeg is. Bij een lege query worden *alle* gegevens van uw blobs geretourneerd.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/emptySearch.png)

### <a name="perform-a-full-text-search"></a>Zoekopdracht in volledige tekst uitvoeren

Typ **myopia** in het veld **Queryreeks** en selecteer **Zoeken**. Met deze stap wordt de inhoud van de bestanden doorzocht en wordt een subset geretourneerd waarin het woord 'myopia' (bijziendheid) voorkomt.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/secondMyopia.png)

### <a name="perform-a-system-properties-search"></a>Systeemeigenschappen doorzoeken

Behalve zoeken in de volledige tekst, kunt u met behulp van de parameter `$select` ook query's maken waarmee per systeemeigenschap wordt gezocht.

Geef `$select=metadata_storage_name` op in de queryreeks en druk op **Enter**. Hierdoor wordt alleen dat bepaalde veld geretourneerd.

De queryreeks zorgt ervoor dat de URL rechtstreeks wordt gewijzigd, dus spaties zijn niet toegestaan. Als u meerdere velden wilt doorzoeken, gebruikt u een komma zoals in `$select=metadata_storage_name,metadata_storage_path`.

De parameter `$select` kan alleen worden gebruikt met velden die als ophaalbaar zijn gemarkeerd op het moment dat u uw index definieerde.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/metadatasearchunstructured.png)

U bent nu klaar met deze zelfstudie en beschikt nu over een doorzoekbare set niet-gestructureerde gegevens.

## <a name="clean-up-resources"></a>Resources opschonen

De gemakkelijkste manier om de resources die u in deze zelfstudie hebt gemaakt, te verwijderen, is door de resourcegroep te verwijderen. Als u de resourcegroep verwijdert, worden alle resources verwijderd die deze groep bevat. In het volgende voorbeeld worden samen met de resourcegroep het opslagaccount en de resourcegroep zelf verwijderd.

1. Ga in de Azure-portal naar de lijst met resourcegroepen in uw abonnement.
2. Selecteer de resourcegroep die u wilt verwijderen.
3. Selecteer de knop **Resourcegroep verwijderen** en geef de naam van de resourcegroep op in het veld voor verwijderen.
4. Selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppeling voor meer informatie over het indexeren van documenten met Azure Search:

> [!div class="nextstepaction"]
> [Met Azure Search documenten in Azure Blob Storage indexeren](../../search/search-howto-indexing-azure-blob-storage.md)
