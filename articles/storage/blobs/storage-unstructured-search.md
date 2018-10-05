---
title: Niet-gestructureerde gegevens in Azure-cloudopslag doorzoeken
description: Niet-gestructureerde gegevens doorzoeken met behulp van Azure Search.
author: roygara
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogarana
ms.custom: mvc
ms.openlocfilehash: 902009d7807b1ce340000c271350af1c37231d77
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181189"
---
# <a name="tutorial-search-unstructured-data-in-cloud-storage"></a>Zelfstudie: Niet-gestructureerde gegevens in cloudopslag doorzoeken

In deze zelfstudie leert u hoe u niet-gestructureerde gegevens kunt doorzoeken met [Azure Search](../../search/search-what-is-azure-search.md), waarbij gegevens worden gebruikt die zijn opgeslagen in Azure-blobs. Niet-gestructureerde gegevens zijn gegevens die niet zijn geordend op een vooraf gedefinieerde manier of waarvoor geen gegevensmodel bestaat. Een voorbeeld hiervan is een txt-bestand.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep maken
> * Create a storage account
> * Een container maken
> * Gegevens uploaden naar de container
> * Een zoekservice via de portal maken
> * De zoekservice gebruiken om uw container te doorzoeken

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Er is een voorbeeldgegevensset voor u voorbereid. **Download [clinical-trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)** en pak het uit in een eigen map.

Het voorbeeld bestaat uit tekstbestanden, verkregen via [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). U kunt ze als voorbeelden van tekstbestanden gebruiken die u met behulp van Azure gaat doorzoeken.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de [Azure-portal](http://portal.azure.com).

## <a name="create-a-storage-account"></a>Create a storage account

Een opslagaccount biedt een unieke locatie voor het opslaan en openen van uw Azure Storage-gegevensobjecten.

Er bestaan momenteel twee typen opslagaccounts, **blob** en **voor algemeen gebruik**. Voor deze zelfstudie gaat u een opslagaccount **voor algemeen gebruik** maken.

Als u niet bekend bent met het proces voor het maken van een algemeen opslagaccount, ga dan als volg te werk:

1. In het linkermenu selecteert u **Opslagaccounts** en vervolgens **Toevoegen**.

2. Voer een unieke naam in voor het opslagaccount. 

3. Selecteer **Resource Manager** als uw **implementatiemodel** en selecteer **Voor algemeen gebruik** in de vervolgkeuzelijst **Soort Account**.

4. Selecteer **Lokaal redundante opslag (LRS)** in de vervolgkeuzelijst **Replicatie**.

5. Selecteer bij **Resourcegroep** de optie **Nieuwe maken** en voer een unieke naam in.

6. Selecteer een geschikt abonnement.

7. Kies een locatie en selecteer **Maken**.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Een container maken

Containers zijn vergelijkbaar met mappen en worden gebruikt voor het opslaan van blobs.

Voor deze zelfstudie gebruikt u een enkele container om de tekstbestanden in op te slaan die zijn opgehaald uit clinicaltrials.gov.

1. Ga in de Azure-portal naar uw opslagaccount.

2. Selecteer **Door blobs bladeren** onder **Blob-service**.

3. Voeg een nieuwe container toe.

4. Noem de container 'gegevens' en selecteer **Container** als het openbare toegangsniveau.

5. Selecteer **OK** om de container te maken. 

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>De voorbeeldgegevens uploaden

Nu u een container hebt, kunt u uw voorbeeldgegevens erin uploaden.

1. Selecteer de container en selecteer **Uploaden**.

2. Selecteer het pictogram met de blauwe map die naast het veld Bestanden wordt weergegeven en blader naar de lokale map waarin u de voorbeeldgegevens hebt uitgepakt.

3. Selecteer alle uitgepakte bestanden en selecteer **Openen**

4. Selecteer **Uploaden** om te beginnen met uploaden.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/upload.png)

Het uploadproces kan even duren.

Nadat dit is voltooid, gaat u terug naar de gegevenscontainer om te controleren of de tekstbestanden zijn geüpload.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Een zoekservice maken

Azure Search is een SaaS-cloudoplossing (Search-as-a-Service) die ontwikkelaars API's en hulpprogramma's biedt waarmee ze de manier waarop u zoekt naar gegevens op het web, op mobiele apparaten en in bedrijfstoepassingen kunnen verrijken.

Als u niet bekend bent met het proces voor het maken van een zoekservice, ga dan als volg te werk:

1. Ga in de Azure-portal naar uw opslagaccount.

2. Schuif naar beneden en klik op **Azure Search toevoegen** onder **BLOB-SERVICE**.

3. Onder **Gegevens importeren** selecteert u **Kies een service**.

4. Selecteer **Klik hier om een nieuwe zoekservice te maken**.

5. Voer in **Nieuwe zoekservice** een unieke naam in voor uw zoekservice in het **URL**-veld.

6. Selecteer bij **Resourcegroep** **Bestaande gebruiken** en kies de resourcegroep die u eerder hebt gemaakt.

7. Selecteer bij **Prijscategorie** de **Gratis**-laag en klik op **Selecteren**.

8. Selecteer **Maken** om de zoekservice te maken.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Uw zoekservice verbinden met de container

Nu u over een zoekservice beschikt, kunt u deze koppelen aan Blob Storage. In deze sectie wordt stapsgewijs uitgelegd hoe u een gegevensbron kunt kiezen en een index en een indexeerfunctie kunt maken.

1. Ga naar uw opslagaccount.

2. Selecteer **Azure Search toevoegen** onder **BLOB-SERVICE**.

3. Selecteer **Zoekservice** in **Gegevens importeren** en klik vervolgens op de zoekservice die u in de vorige sectie hebt gemaakt. Hiermee wordt **Nieuwe gegevensbron** geopend.

### <a name="new-data-source"></a>Nieuwe gegevensbron

  Een gegevensbron geeft aan welke gegevens moeten worden geïndexeerd en hoe u toegang tot de gegevens kunt krijgen. Een gegevensbron kan meerdere keren worden gebruikt door dezelfde searchservice.

1. Voer een naam in voor de gegevensbron. Bij **Gegevens die moeten worden geëxtraheerd** selecteert u **Inhoud en metagegevens**. De gegevensbron geeft aan welke onderdelen van de blob worden geïndexeerd.
    
    a. In uw eigen scenario's in de toekomst kunt u ook **Alleen opslagmetagegevens** selecteren. Deze mogelijkheid kunt u selecteren als u de gegevens die u wilt indexeren wilt beperken tot standaard blobeigenschappen of door de gebruiker gedefinieerde eigenschappen.
    
    b. U kunt ook **Alle metagegevens** kiezen om zowel standaard blobeigenschappen als *alle* metagegevens te verkrijgen die specifiek zijn voor het type inhoud. 

2. Aangezien de blobs die u gebruikt tekstbestanden zijn, moet u **Parseermodus** op **Tekst** instellen.
    
    a. In uw eigen scenario's in de toekomst kunt u desgewenst [Overige parseermodi](../../search/search-howto-indexing-azure-blob-storage.md) selecteren, afhankelijk van de inhoud van uw blobs.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/datasources.png)

3. Selecteer **Opslagcontainer** om de beschikbare opslagaccounts weer te geven.

4. Selecteer uw opslagaccount en selecteer vervolgens de container die u eerder hebt gemaakt.

5. Klik op **Selecteren** om terug te keren naar **Nieuwe gegevensbron** en selecteer **OK** om door te gaan.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>De index configureren

  Een index is een verzameling velden afkomstig uit uw gegevensbron die kan worden doorzocht. De index is datgene wat er voor zorgt dat uw zoekservice weet op welke manieren uw gegevens moeten worden doorzocht.

1. Bij **Gegevens importeren** selecteert u **Doelindex aanpassen**.
 
2. Typ een naam voor de index in het veld **Indexnaam**.

3. Schakel het selectievakje **Ophalen mogelijk** van het kenmerk bij **metadata_storage_name** in.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/valuestoselect.png)

4. Klik op **OK**. Vervolgens wordt **Een indexeerfunctie maken** weergegeven.

De parameters van de index en de kenmerken die u aan deze parameters toewijst zijn belangrijk. De parameters geven aan *welke* gegevens moeten worden opgeslagen en de kenmerken geven aan *hoe* die gegevens moeten worden opgeslagen.

De kolom **VELDNAAM** bevat de parameters. De volgende tabel bevat een overzicht van de beschikbare kenmerken en de bijbehorende beschrijvingen.

### <a name="field-attributes"></a>Veldkenmerken
| Kenmerk | Beschrijving |
| --- | --- |
| *Sleutel* |Een tekenreeks met de unieke id van elk document. Deze reeks wordt gebruikt om op te zoeken. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld gebruiken in een meervoudige navigatiestructuur om op de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om meerdere documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |


### <a name="create-an-indexer"></a>Een indexeerfunctie maken
    
  Een indexeerfunctie verbindt een gegevensbron met een zoekindex en biedt een schema waarmee uw gegevens opnieuw kunnen worden geïndexeerd.

1. Typ een naam in het veld **Naam** en selecteer **OK**.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/exindexer.png)

2. U gaat terug naar **Gegevens importeren**, waar u **OK** selecteert zodat het verbindingsproces kan worden voltooid.

U hebt uw blob nu met uw zoekservice verbonden. Het duurt een paar minuten voordat op de portal zichtbaar is dat de index is ingevuld. De zoekservice begint echter onmiddellijk met indexeren, zodat u meteen met zoeken kunt beginnen.

## <a name="search-your-text-files"></a>Uw tekstbestanden doorzoeken

Als u uw bestanden wilt doorzoeken, opent u Search Explorer in de index van de zoekservice die zojuist is gemaakt.

De volgende stappen laten zien u waar u Search Explorer en een aantal query's kunt vinden:

1. Ga naar alle resources en zoek naar de zoekservice die zojuist is gemaakt.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/exampleurl.png)

3. Selecteer de index en open deze. 

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/overview.png)

4. Selecteer **Search Explorer** om deze te openen. Hier kunt u livequery's op uw gegevens uitvoeren.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/indexespane.png)

5. Selecteer **Zoeken** terwijl het veld voor de queryreeks leeg is. Bij een lege query worden *alle* gegevens van uw blobs geretourneerd.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Zoekopdracht in volledige tekst 

Typ 'myopie' in het veld **Queryreeks** en selecteer **Zoeken**. Hiermee wordt een opdracht gestart waarmee de inhoud van de bestanden wordt doorzocht, en wordt een subset geretourneerd waarin het woord 'myopie' voorkomt.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Systeemeigenschappen doorzoeken

U kunt ook query's maken waarmee met behulp van de parameter `$select` per systeemeigenschap wordt gezocht. Voer `$select=metadata_storage_name` in als de queryreeks en druk op Enter zodat alleen dat specifieke veld wordt geretourneerd.
    
De queryreeks zorgt ervoor dat de URL rechtstreeks wordt gewijzigd, dus spaties zijn niet toegestaan. Als u meerdere velden wilt doorzoeken, gebruikt u een komma zoals in `$select=metadata_storage_name,metadata_storage_path`.
    
De parameter `$select` kan alleen worden gebruikt met velden die als ophaalbaar zijn gemarkeerd op het moment dat u uw index definieerde.

  ![Niet-gestructureerde zoekopdracht](media/storage-unstructured-search/metadatasearchunstructured.png) 

U bent nu klaar met deze zelfstudie en beschikt nu over een doorzoekbare set niet-gestructureerde gegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het doorzoeken van niet-gestructureerde gegevens met behulp van Azure Search. U hebt onder andere het volgende geleerd:

> [!div class="checklist"]
> * Een resourcegroep maken
> * Create a storage account
> * Een container maken
> * Gegevens uploaden naar de container
> * Een zoekservice maken
> * De zoekservice gebruiken om uw container te doorzoeken

Volg deze koppeling voor meer informatie over het indexeren van documenten met Azure Search.

> [!div class="nextstepaction"]
> [Met Azure Search documenten in Azure Blob Storage indexeren](../../search/search-howto-indexing-azure-blob-storage.md)