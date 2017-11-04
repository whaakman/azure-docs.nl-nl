---
title: Niet-gestructureerde gegevens zoeken in de Azure-cloud-opslag
description: Zoeken naar niet-gestructureerde gegevens met behulp van Azure search.
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: rogara
ms.custom: mvc
ms.openlocfilehash: 930b735eb03aea6ce701b694ca527049b4c3f24d
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="search-unstructured-data-in-cloud-storage"></a>Niet-gestructureerde gegevens in cloudopslag doorzoeken

In deze zelfstudie leert u hoe niet-gestructureerde gegevens zoeken [Azure Search](../../search/search-what-is-azure-search.md) met behulp van gegevens die zijn opgeslagen in Azure blobs. Niet-gestructureerde gegevens zijn gegevens die niet zijn georganiseerd in een vooraf gedefinieerde manier of beschikt niet over een gegevensmodel. Een voorbeeld hiervan is een txt-bestand.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep maken
> * Een opslagaccount maken
> * Een container maken
> * Gegevens uploaden naar de container
> * Maak een zoekservice via de portal
> * De search-service gebruiken om te zoeken van de container

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Een verzameling voorbeeldgegevens is bedoeld voor u. **Download [klinische trials.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials.zip)**  en pak deze naar de eigen map.

Het voorbeeld bestaat uit tekstbestanden die zijn verkregen via [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). U kunt ze als voorbeeld tekstbestanden gebruiken om te zoeken met behulp van Azure.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](http://portal.azure.com).

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Een opslagaccount biedt een unieke locatie voor het opslaan en toegang tot uw Azure Storage-gegevensobjecten.

Er zijn momenteel twee typen opslagaccounts **Blob** en **algemeen**. Voor deze zelfstudie maakt u een **algemeen** storage-account.

Als u niet bekend met het proces bent van het maken van een algemeen opslagaccount, wordt hier het maken van een:

1. Selecteer in het menu links **Opslagaccounts**, selecteer daarna **toevoegen**.

2. Geef een unieke naam voor uw opslagaccount. 

3. Selecteer **Resource Manager** voor uw **implementatiemodel** en selecteer **algemeen** van de **Account kind** vervolgkeuzelijst.

4. Selecteer **lokaal redundante opslag (LRS)** van de **replicatie** vervolgkeuzelijst.

5. Onder **resourcegroep**, selecteer **nieuw** en voer een unieke naam.

6. Selecteer een geschikte abonnement.

7. Kies een locatie en selecteer **maken.**

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/storagepanes2.png)

## <a name="create-a-container"></a>Een container maken

Containers zijn vergelijkbaar met mappen en worden gebruikt voor het opslaan van blobs.

Voor deze zelfstudie maakt u een enkele container gebruiken voor het opslaan van de tekstbestanden die zijn verkregen via clinicaltrials.gov.

1. Navigeer naar uw opslagaccount in de Azure-portal.

2. Selecteer **bladeren blobs** onder **Blob-Service.**

3. Een nieuwe container toevoegen.

4. De naam van de container 'data' en selecteer **Container** voor het niveau van de openbare toegang.

5. Selecteer **OK** de container maken. 

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/storageactinfo.png)

## <a name="upload-the-example-data"></a>De bijvoorbeeld gegevens uploaden

Nu dat u een container hebt, kunt u uw voorbeeldgegevens te uploaden.

1. Selecteer de container en selecteer **uploaden**.

2. Selecteer de blauw mappictogram weergegeven naast het veld van bestanden en blader naar de lokale map waarin u de voorbeeldgegevens hebt uitgepakt.

3. De uitgepakte bestanden te selecteren en selecteer **openen**

4. Selecteer **uploaden** om te beginnen met het uploadproces.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/upload.png)

Het uploadproces kan even duren.

Nadat deze is voltooid, gaat u terug naar de gegevenscontainer van uw om te bevestigen dat de tekstbestanden geüpload.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/clinicalfolder.png)

## <a name="create-a-search-service"></a>Maak een zoekservice

Azure Search is een search-as-a-service cloudoplossing die biedt ontwikkelaars API's en hulpprogramma's voor het toevoegen van een uitgebreide zoekervaring via uw gegevens in toepassingen met web, mobiel en enterprise.

Als u niet bekend met het proces bent van het maken van een service voor zoeken, volgt hier een te maken:

1. Navigeer naar uw opslagaccount in de Azure-portal.

2. Schuif naar beneden en klik op **toevoegen Azure Search** onder **BLOB-SERVICE**.

3. Onder **importgegevens**, selecteer **Kies uw service**.

4. Selecteer **Klik hier om u te maken van een nieuwe zoekservice**.

5. Binnen **nieuwe zoekservice** Voer een unieke naam voor uw zoekservice in de **URL** veld.

6. Onder **resourcegroep**, selecteer **gebruik bestaande** en kiest u de resourcegroep die u eerder hebt gemaakt.

7. Voor de **prijscategorie**, selecteer de **vrije** servicetier en klik op **Selecteer**.

8. Selecteer **maken** de search-service te maken.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/createsearch2.png)

## <a name="connect-your-search-service-to-your-container"></a>Verbinding maken met uw search-service naar de container

Nu dat u een zoekservice hebt, kunt u het koppelen aan de blob-opslag. Deze sectie wordt uitgelegd hoe u van een gegevensbron kiezen, het maken van een index en het maken van een indexeerfunctie.

1. Navigeer naar uw storage-account.

2. Selecteer **toevoegen van Azure Search** onder **BLOB-SERVICE.**

3. Selecteer **zoekservice** binnen **importgegevens** en klik vervolgens op de zoekservice die u in de voorgaande sectie hebt gemaakt. Hiermee opent u **nieuwe gegevensbron**.

### <a name="new-data-source"></a>Nieuwe gegevensbron

  Een gegevensbron geeft aan welke gegevens worden index en hoe u toegang tot de gegevens. Een gegevensbron kan meerdere keren worden gebruikt door de dezelfde search-service.

1. Voer een naam voor de gegevensbron. Onder **gegevens uitpakken**, selecteer **inhoud en metagegevens**. De gegevensbron geeft aan welke onderdelen van de blob zijn geïndexeerd.
    
    a. In uw eigen toekomstige scenario's, kunt u ook selecteren **opslag bestaan alleen uit metagegevens**. U zou deze selectie als u wilt beperken van de gegevens die aan de gebruiker gedefinieerde eigenschappen of eigenschappen van de standaard blob is geïndexeerd.
    
    b. U kunt ook **alle metagegevens** om beide standaard blob-eigenschappen te verkrijgen en *alle* inhoudstype specifieke metagegevens. 

2. Aangezien de blobs u tekstbestanden, stel **bij het parseren van de modus** naar **tekst**.
    
    a. In uw eigen toekomstige scenario's mogelijk wilt u Selecteer [andere parseren modi](../../search/search-howto-indexing-azure-blob-storage.md) , afhankelijk van de inhoud van uw blobs.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/datasources.png)

3. Selecteer **Opslagcontainer** voor een lijst met beschikbare opslagaccounts.

4. Selecteer uw storage-account en selecteer vervolgens de container die u eerder hebt gemaakt.

5. Klik op **Selecteer** om terug te keren naar **nieuwe gegevensbron** en selecteer **OK** om door te gaan.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/datacontainer.png)

### <a name="configure-the-index"></a>De index configureren

  Een index is een verzameling van velden uit de gegevensbron dat kan worden doorzocht. De index is hoe uw zoekservice weet op welke manieren u uw gegevens worden doorzocht.

1. In **gegevens importeren** Selecteer **doelindex aanpassen**.
 
2. Voer een naam voor uw index in de **indexnaam** veld.

3. Selecteer de **ophalen mogelijk** van kenmerk selectievakje onder **metadata_storage_name**.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/valuestoselect.png)

4. Klik op **OK**, die wordt **Maak een indexeerfunctie**.

De parameters van de index en de kenmerken geeft u de parameters zijn belangrijk. Geef de parameters *wat* gegevens om op te slaan, geef de kenmerken *hoe* voor het opslaan van die gegevens.

De **veldnaam** kolom bevat de parameters. De volgende tabel bevat een overzicht van de beschikbare kenmerken en de bijbehorende beschrijvingen.

### <a name="field-attributes"></a>Veldkenmerken
| Kenmerk | Beschrijving |
| --- | --- |
| *Sleutel* |Een tekenreeks die de unieke id van elk document die wordt gebruikt voor het opzoeken van het document. Elke index moet een sleutel hebben. Slechts één veld kan de sleutel zijn en het type moet zijn ingesteld op Edm.String. |
| *Ophalen mogelijk* |Hiermee geeft u op of een veld in een zoekresultaat kan worden geretourneerd. |
| *Filterbaar* |Hiermee kan het veld in een filterquery's worden gebruikt. |
| *Sorteerbaar* |Hiermee kan een query de zoekresultaten sorteren op basis van dit veld. |
| *Facetten mogelijk* |Hiermee kunt u een veld moet worden gebruikt in een meervoudige navigatie-structuur voor de gebruiker te filteren. Doorgaans werken velden met terugkerende waarden die u kunt gebruiken om meerdere documenten te groeperen (bijvoorbeeld meerdere documenten die in een bepaalde merk- of servicecategorie vallen) het beste als facetten. |
| *Doorzoekbaar* |Hiermee kunt u in dit veld in de volledige tekst zoeken. |


### <a name="create-an-indexer"></a>Maak een indexeerfunctie
    
  Een indexeerfunctie een gegevensbron is verbonden met een search-index en vindt u een planning aan uw gegevens te indexeren.

1. Voer een naam in de **naam** veld en selecteert u **OK**.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/exindexer.png)

2. U worden teruggebracht naar **importgegevens**, selecteer **OK** om de verbindingsproces te voltooien.

U hebt uw blob nu is verbonden met uw search-service. Het duurt enkele minuten voor de portal om weer te geven dat de index is gevuld. De search-service begint echter indexeren onmiddellijk zodat u kunt ook zoeken meteen beginnen.

## <a name="search-your-text-files"></a>Uw tekstbestanden zoeken

Om te zoeken naar bestanden, opent u de search explorer in de index van de zojuist gemaakte search-service.

De volgende stappen laten zien waar u de search explorer vindt u en biedt u enkele voorbeelden van query's:

1. Ga naar alle resources en zoek de zojuist gemaakte search-service.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/exampleurl.png)

3. Selecteer uw index wilt kunnen openen. 

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/overview.png)

4. Selecteer **Search Explorer** om de search explorer, waar u live query's voor uw gegevens maken kunt te openen.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/indexespane.png)

5. Selecteer **Search** terwijl de query string-veld leeg is. Retourneert een lege query *alle* de gegevens van uw blobs.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/emptySearch.png)

### <a name="full-text-search"></a>Zoekopdracht in volledige tekst 

Voer 'Myopia' in de **querytekenreeks** veld en selecteert u **Search**. Starten van een zoekopdracht van de inhoud van de bestanden en retourneert een subset van deze, waarin het woord 'Myopia'.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/secondMyopia.png) 

### <a name="system-properties-search"></a>Systeem zoeken

U kunt ook query's die door Systeemeigenschappen met behulp van een zoekopdracht maken de `$select` parameter. Voer `$select=metadata_storage_name` in de queryreeks en druk op invoert, alleen dat veld retourneren.
    
De query-tekenreeks is de URL rechtstreeks wijzigen zodat spaties zijn niet toegestaan. Gebruiken om te zoeken naar meerdere velden, komma's, zoals:`$select=metadata_storage_name,metadata_storage_path`
    
De `$select` parameter kan alleen worden gebruikt met velden die zijn gemarkeerd worden opgehaald bij het definiëren van uw index.

  ![Niet-gestructureerde zoeken](media/storage-unstructured-search/metadatasearchunstructured.png) 

U nu in deze zelfstudie hebt voltooid en een doorzoekbare set niet-gestructureerde gegevens.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over niet-gestructureerde gegevens met behulp van Azure Search, zoals het zoeken:

> [!div class="checklist"]
> * Een resourcegroep maken
> * Een opslagaccount maken
> * Een container maken
> * Uploaden van gegevens naar de container
> * Maak een zoekservice
> * De Search-Service gebruiken om te zoeken van de container

Volg deze koppeling voor meer informatie over documenten met Azure Search indexeren.

> [!div class="nextstepaction"]
> [Documenten in Azure Blob Storage met Azure Search indexeren](../../search/search-howto-indexing-azure-blob-storage.md)