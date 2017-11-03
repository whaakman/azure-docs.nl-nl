---
title: Het gebruik van Hudson met Blob storage | Microsoft Docs
description: Beschrijft hoe Hudson met Azure Blob storage gebruiken als een opslagplaats voor build-artefacten.
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 119becdd-72c4-4ade-a439-070233c1e1ac
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: e54bedff5f744004288e132efbed8c3e7981f8a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Azure Storage gebruiken met een Hudson CI-oplossing
## <a name="overview"></a>Overzicht
De volgende informatie laat zien hoe Blob storage gebruiken als een opslagplaats van build-artefacten die zijn gemaakt door een oplossing Hudson continue integratie (CI) of als een bron van downloadbare bestanden moet worden gebruikt in een buildproces. Een van de scenario's waarbij u dit nuttig vinden zou is wanneer u in een flexibele-ontwikkelomgeving coderen bent (met Java of andere talen) en moet u een opslagplaats voor uw build-artefacten, zodat u kunt, bijvoorbeeld met andere leden van de organisatie, uw klanten delen of houd een archief builds worden uitgevoerd op basis van continue integratie.  Een ander scenario is als uw build-taak zelf andere bestanden, bijvoorbeeld afhankelijkheden te downloaden als onderdeel van de build invoer vereist.

In deze zelfstudie wordt u de Azure Storage-invoegtoepassing voor Hudson CI beschikbaar gesteld door Microsoft.

## <a name="introduction-to-hudson"></a>Inleiding tot Hudson
Hudson continue integratie van een software-project kunt doordat ontwikkelaars hun codewijzigingen eenvoudig kunt integreren en builds geproduceerde automatisch en vaak, waardoor de productiviteit van de ontwikkelaars verhogen. Builds zijn samengestelde en build artefacten kunnen worden geüpload naar verschillende opslagplaatsen. In dit artikel wordt beschreven hoe Azure Blob storage gebruiken als de opslagplaats van de build-artefacten. Dit wordt ook beschreven hoe downloaden van afhankelijkheden van Azure Blob-opslag.

Meer informatie over Hudson kan worden gevonden op [Hudson voldoen aan](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service
Voordelen van het gebruik van de Blob-service voor het hosten van uw flexibele ontwikkeling build artefacten zijn:

* Hoge beschikbaarheid van uw artefacten build en/of downloadbare afhankelijkheden.
* Prestaties wanneer uw oplossing Hudson CI uw build-artefacten uploadt.
* Prestaties wanneer uw klanten en partners downloadt uw build-artefacten.
* De controle over het toegangsbeleid van de gebruiker de keuze tussen anonieme toegang, gedeelde toegang op basis van een verlopen handtekening toegang, persoonlijke toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten
U moet het volgende wanneer u het gebruik van de Blob-service met uw Hudson CI-oplossing:

* Een oplossing Hudson continue integratie.
  
    Als u een oplossing Hudson CI momenteel geen hebt, kunt u een Hudson CI-oplossing met behulp van de volgende techniek uitvoeren:
  
  1. Op een machine Java ingeschakeld downloaden via het WAR Hudson van <http://hudson-ci.org/>.
  2. Bij een opdrachtprompt die naar de map waarin het WAR Hudson wordt geopend, het WAR Hudson worden uitgevoerd. Bijvoorbeeld, als u versie 3.1.2 hebt gedownload:
     
      `java -jar hudson-3.1.2.war`

  3. Open in uw browser `http://localhost:8080/`. Hiermee opent u het dashboard Hudson.
  4. Voltooi de eerste installatie op bij het eerste gebruik van Hudson `http://localhost:8080/`.
  5. Nadat u de eerste installatie, het exemplaar van het WAR Hudson annuleren, start het WAR Hudson opnieuw, en opnieuw openen van het dashboard Hudson `http://localhost:8080/`, gaat u voor het installeren en configureren van de invoegtoepassing van Azure Storage.
     
      Terwijl een standaardoplossing Hudson CI zou worden ingesteld om te worden uitgevoerd als een service, het war Hudson uitgevoerd op de opdrachtregel is voldoende voor deze zelfstudie.
* Een Azure-account. U kunt zich aanmelden voor een Azure-account op <http://www.azure.com>.
* Een Azure Storage-account. Als u nog een opslagaccount hebt, kunt u een met de stappen in [een Opslagaccount maken](../common/storage-create-storage-account.md#create-a-storage-account).
* Bekend bent met de oplossing Hudson CI wordt aanbevolen, maar niet vereist, zoals de volgende inhoud wordt met een eenvoudige voorbeeld ziet u de stappen die nodig zijn bij gebruik van de Blob-service als een opslagplaats voor Hudson CI artefacten bouwen.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Het gebruik van de Blob-service met Hudson CI
Als u wilt de Blob-service met Hudson gebruiken, moet u de Azure Storage-invoegtoepassing installeren, configureren van de invoegtoepassing voor het gebruik van uw opslagaccount en maak vervolgens een na build-actie die uw artefacten build naar uw opslagaccount uploadt. Deze stappen worden beschreven in de volgende secties.

## <a name="how-to-install-the-azure-storage-plugin"></a>Het installeren van de Azure Storage-invoegtoepassing
1. Klik in het dashboard Hudson op **Hudson beheren**.
2. Op de **beheren Hudson** pagina, klikt u op **invoegtoepassingen beheren**.
3. Klik op de **beschikbaar** tabblad.
4. Klik op **anderen**.
5. In de **artefact Uploaders** sectie **invoegtoepassing voor Microsoft Azure Storage**.
6. Klik op **Install**.
7. Nadat de installatie voltooid is, start u Hudson opnieuw.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Het configureren van de Azure Storage-invoegtoepassing voor het gebruik van uw storage-account
1. Klik in het dashboard Hudson op **Hudson beheren**.
2. Op de **beheren Hudson** pagina, klikt u op **System configureren**.
3. In de **configuratie van Microsoft Azure Storage-Account** sectie:
   
    a. Voer de naam van uw opslagaccount, die u van verkrijgen kunt de [Azure Portal](https://portal.azure.com).
   
    b. Voer de sleutel van uw opslagaccount, ook kan worden verkregen uit de [Azure Portal](https://portal.azure.com).
   
    c. Gebruik de standaardwaarde voor **eindpunt-URL van Blob-Service** als u de openbare Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruikt u het eindpunt zoals opgegeven in de [Azure Portal](https://portal.azure.com) voor uw opslagaccount.
   
    d. Klik op **storage-referenties gevalideerd** uw storage-account te valideren.
   
    e. [Optioneel] Als u meer storage-accounts die u wilt die beschikbaar zijn gesteld uw CI Hudson, klikt u op **Voeg meer opslagaccounts voor**.
   
    f. Klik op **opslaan** uw instellingen op te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Het maken van een na build-actie die uw artefacten build naar uw storage-account uploadt
Instructie oog wordt eerst moet we een taak die verschillende bestanden maken en voegt u de actie na build de bestanden uploaden naar uw storage-account te maken.

1. Klik in het dashboard Hudson op **nieuwe taak**.
2. Naam van de taak **MyJob**, klikt u op **bouwen van een taak vrije-stijl software**, en klik vervolgens op **OK**.
3. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **toevoegen build stap** en kies **Windows uitvoeren van batch-opdracht**.
4. In **opdracht**, gebruik de volgende opdrachten:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. In de **na build acties** sectie van de taakconfiguratie van de, klikt u op **artefacten uploaden naar Microsoft Azure-blobopslag**.
6. Voor **Opslagaccountnaam**, selecteer het opslagaccount moet worden gebruikt.
7. Voor **containernaam**, geef de containernaam. (De container gemaakt als deze niet bestaat nog wanneer de build-artefacten worden geüpload.) U kunt gebruik maken van omgevingsvariabelen, dus voor dit voorbeeld geeft **${JOB_NAME}** als de containernaam van de.
   
    **Tip**
   
    Onder de **opdracht** sectie waarin u hebt opgegeven een script voor **Windows uitvoeren van batch-opdracht** is een koppeling naar de omgevingsvariabelen die wordt herkend door Hudson. Klik op de koppeling voor meer informatie over de namen van omgevingsvariabelen en beschrijvingen. Houd er rekening mee dat omgevingsvariabelen die bevat speciale tekens, zoals de **BUILD_URL** omgevingsvariabele, zijn niet toegestaan als een containernaam of algemene virtueel pad.
8. Klik op **nieuwe container openbaar maken standaard** voor dit voorbeeld. (Als u een persoonlijke container gebruiken wilt, hebt u nodig voor het maken van een shared access signature om toegang te verlenen. Dat is buiten het bereik van dit artikel. U kunt meer informatie over handtekeningen voor gedeelde toegang op [met behulp van Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [Optioneel] Klik op **schone container voordat u uploadt** als u wilt dat de container van inhoud worden gewist voordat build artefacten worden geüpload (laat het vakje uitgeschakeld als u niet wilt opschonen van de inhoud van de container).
10. Voor **lijst van artefacten uploaden**, voer  **tekst /*.txt**.
11. Voor **algemene virtuele pad voor de geüploade artefacten**, voer **${bouwen\_ID} / ${bouwen\_getal}**.
12. Klik op **opslaan** uw instellingen op te slaan.
13. Klik in het dashboard Hudson **nu bouwen** om uit te voeren **MyJob**. Bekijk de uitvoer van de console voor status. Statusberichten voor Azure Storage worden opgenomen in de console-uitvoer wanneer de actie na build build artefacten uploaden wordt gestart.
14. De taak is voltooid, kunt u de build-artefacten controleren door het openen van de openbare blob.
    
    a. Meld u aan bij [Azure-portal](https://portal.azure.com).
    
    b. Klik op **opslag**.
    
    c. Klik op de opslagaccountnaam die u voor Hudson gebruikt.
    
    d. Klik op **Containers**.
    
    e. Klik op de container met de naam **myjob**, die de kleine versie van de naam van de taak die u tijdens het maken van de taak Hudson toegewezen is. Namen van containers en blobs namen zijn kleine letters (en hoofdlettergevoelig) in Azure Storage. In de lijst met BLOB's voor de container met de naam **myjob** ziet u **hello.txt** en **date.txt**. Kopieer de URL voor een van deze items en open het in uw browser. Als een artefact build ziet u het tekstbestand dat is geüpload.

Slechts één na build-actie die artefacten naar Azure Blob-opslag uploadt kan per taak worden gemaakt. De actie na build artefacten uploaden naar Azure Blob-opslag kunt opgeven verschillende bestanden (inclusief jokertekens) en paden naar bestanden binnen **lijst van artefacten uploaden** met een puntkomma als scheidingsteken. Bijvoorbeeld, als uw Hudson bouwen produceert JAR-bestanden en TXT-bestanden in uw werkruimte **bouwen** map en u wilt uploaden zowel voor Azure Blob-opslag, gebruikt u de volgende voor de **lijst van artefacten uploaden** waarde: **bouwen /\*JAR; build /\*.txt**. U kunt de syntaxis van de dubbele ook gebruiken om een pad in de blob-naam gebruiken. Bijvoorbeeld, als u wilt dat de potten ophalen geüpload met behulp van **binaire bestanden** in de blobpad en de TXT-bestanden ophalen geüpload met behulp van **mededelingen** in het blobpad, gebruikt u de volgende voor de **lijst van artefacten uploaden** waarde: **bouwen /\*. jar::binaries; build /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Het maken van een build-stap die u van Azure Blob-opslag downloadt
De volgende stappen laten zien hoe de stap in een build voor het downloaden van items uit Azure Blob-opslag configureren. Dit is handig als u items wilt opnemen in uw build, bijvoorbeeld potten die u in Azure Blob-opslag wilt behouden.

1. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **toevoegen build stap** en kies **downloaden uit Azure Blob storage**.
2. Voor **opslagaccountnaam**, selecteer het opslagaccount moet worden gebruikt.
3. Voor **containernaam**, geef de naam van de container met de blobs die u wilt downloaden. U kunt de omgevingsvariabelen.
4. Voor **Blob-naam**, geef de blobnaam. U kunt de omgevingsvariabelen. U kunt ook een sterretje als jokerteken nadat u de eerste letter (s) van de blob-naam opgeven. Bijvoorbeeld: **project\***  geeft alle blobs die met beginnen **project**.
5. [Optioneel] Voor **downloadpad**, geef het pad op de Hudson machine waar u bestanden wilt downloaden uit Azure Blob-opslag. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u geen waarde voor opgeeft **downloadpad**, de bestanden uit Azure Blob storage worden gedownload naar de werkruimte van de taak.)

Als er extra items die u wilt downloaden uit Azure Blob storage, kunt u aanvullende build stappen.

Nadat u een build uitgevoerd, kunt u de uitvoer van de build geschiedenis-console controleren of kijken naar de downloadlocatie, om te zien of de blobs die u verwacht zijn gedownload.

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de Blob-service
Het volgende gedeelte bevat een overzicht van de onderdelen van de Blob-service.

* **Storage-account**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Dit is het hoogste niveau van de naamruimte voor toegang tot blobs. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte minder dan 100 TB is.
* **Container**: een container is een groepering van een reeks blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.
* **BLOB**: een bestand van willekeurig type en de grootte. Er zijn twee typen kunnen worden opgeslagen in Azure Storage blobs: blok en pagina-blobs. De meeste bestanden zijn blok-blobs. Een enkel blok-blob kan maximaal 200 GB groot zijn. Deze zelfstudie maakt gebruik van blok-blobs. Pagina-blobs, een ander blobtype, mag maximaal 1 TB in grootte en zijn efficiënter als bereiken in bytes in een bestand vaak worden gewijzigd. Zie voor meer informatie over blobs [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL-indeling**: Blobs worden opgevraagd met de volgende URL-indeling:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (De indeling hierboven van toepassing op de openbare Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruikt u het eindpunt in de [Azure Portal](https://portal.azure.com) om te bepalen van de URL-eindpunt.)
  
    In de bovenstaande indeling `storageaccount` vertegenwoordigt de naam van uw opslagaccount `container_name` vertegenwoordigt de naam van de container en `blob_name` vertegenwoordigt de naam van uw blob, respectievelijk. U kunt meerdere paden, gescheiden door een slash hebben binnen de containernaam  **/** . De naam van de voorbeeld-container in deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_getal}** is gebruikt voor het algemene virtueel pad, wat resulteert in de blob met een URL van de volgende notatie:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen
* [Voldoen aan Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Azure-opslag-SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage Client SDK-naslaginformatie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage/)

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).