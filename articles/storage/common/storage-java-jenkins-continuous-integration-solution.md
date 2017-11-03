---
title: Gebruik van Azure Storage met een oplossing voor Jenkins continue integratie | Microsoft Docs
description: Deze zelfstudie laat zien hoe de Azure blob-service gebruiken als een opslagplaats voor artefacten die zijn gemaakt door een continue integratie Jenkins oplossing bouwen.
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: f4e5ca75-f6cb-4f74-981b-2aa06bb8de45
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: 174ac449e803ed5327468a38ea7264cb9923a877
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Azure Storage gebruiken met een Jenkins CI-oplossing
## <a name="overview"></a>Overzicht
De volgende informatie laat zien hoe Blob storage gebruiken als een opslagplaats van build-artefacten die zijn gemaakt door een oplossing Jenkins continue integratie (CI) of als een bron van downloadbare bestanden moet worden gebruikt in een buildproces. Een van de scenario's waarbij u dit nuttig vinden zou is wanneer u in een flexibele-ontwikkelomgeving coderen bent (met Java of andere talen) en moet u een opslagplaats voor uw build-artefacten, zodat u kunt, bijvoorbeeld met andere leden van de organisatie, uw klanten delen of houd een archief builds worden uitgevoerd op basis van continue integratie. Een ander scenario is als uw build-taak zelf andere bestanden, bijvoorbeeld afhankelijkheden te downloaden als onderdeel van de build invoer vereist.

In deze zelfstudie wordt u de Azure Storage-invoegtoepassing voor Jenkins CI beschikbaar gesteld door Microsoft.

## <a name="overview-of-jenkins"></a>Overzicht van Jenkins
Jenkins continue integratie van een software-project kunt doordat ontwikkelaars hun codewijzigingen eenvoudig kunt integreren en builds geproduceerde automatisch en vaak, waardoor de productiviteit van de ontwikkelaars verhogen. Builds zijn samengestelde en build artefacten kunnen worden geüpload naar verschillende opslagplaatsen. In dit onderwerp wordt beschreven hoe Azure blob storage gebruiken als de opslagplaats van de build-artefacten. Dit wordt ook beschreven hoe downloaden van afhankelijkheden van Azure blob-opslag.

Meer informatie over Jenkins kan worden gevonden op [Jenkins voldoen aan](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service
Voordelen van het gebruik van de Blob-service voor het hosten van uw flexibele ontwikkeling build artefacten zijn:

* Hoge beschikbaarheid van uw artefacten build en/of downloadbare afhankelijkheden.
* Prestaties wanneer uw oplossing Jenkins CI uw build-artefacten uploadt.
* Prestaties wanneer uw klanten en partners downloadt uw build-artefacten.
* De controle over het toegangsbeleid van de gebruiker de keuze tussen anonieme toegang, gedeelde toegang op basis van een verlopen handtekening toegang, persoonlijke toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten
U moet het volgende wanneer u het gebruik van de Blob-service met uw Jenkins CI-oplossing:

* Een oplossing Jenkins continue integratie.
  
    Als u een oplossing Jenkins CI momenteel geen hebt, kunt u een Jenkins CI-oplossing met behulp van de volgende techniek uitvoeren:
  
  1. Downloaden op een machine Java ingeschakeld jenkins.war van <http://jenkins-ci.org>.
  2. Voer bij een opdrachtprompt die naar de map waarin jenkins.war wordt geopend:
     
      `java -jar jenkins.war`

  3. Open in uw browser `http://localhost:8080/`. Hiermee opent u het dashboard Jenkins, gaat u voor het installeren en configureren van de invoegtoepassing van Azure Storage.
     
      Terwijl een standaardoplossing Jenkins CI zou worden ingesteld om te worden uitgevoerd als een service, het war Jenkins uitgevoerd op de opdrachtregel is voldoende voor deze zelfstudie.
* Een Azure-account. U kunt zich aanmelden voor een Azure-account op <http://www.azure.com>.
* Een Azure Storage-account. Als u nog een opslagaccount hebt, kunt u een met de stappen in [een Opslagaccount maken](../common/storage-create-storage-account.md#create-a-storage-account).
* Bekend bent met de oplossing Jenkins CI wordt aanbevolen, maar niet vereist, zoals de volgende inhoud wordt met een eenvoudige voorbeeld ziet u de stappen die nodig zijn bij gebruik van de Blob-service als een opslagplaats voor Jenkins CI artefacten bouwen.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Het gebruik van de Blob-service met Jenkins CI
Als u wilt de Blob-service met Jenkins gebruiken, moet u de Azure Storage-invoegtoepassing installeren, configureren van de invoegtoepassing voor het gebruik van uw opslagaccount en maak vervolgens een na build-actie die uw artefacten build naar uw opslagaccount uploadt. Deze stappen worden beschreven in de volgende secties.

## <a name="how-to-install-the-azure-storage-plugin"></a>Het installeren van de Azure Storage-invoegtoepassing
1. Klik in het dashboard Jenkins op **Jenkins beheren**.
2. In de **beheren Jenkins** pagina, klikt u op **invoegtoepassingen beheren**.
3. Klik op de **beschikbaar** tabblad.
4. In de **artefact Uploaders** sectie selectievakje **invoegtoepassing voor Microsoft Azure Storage**.
5. Klik op **installeren zonder opnieuw opstarten** of **nu downloaden en installeren na opnieuw opstarten**.
6. Opnieuw opstarten Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Het configureren van de Azure Storage-invoegtoepassing voor het gebruik van uw storage-account
1. Klik in het dashboard Jenkins op **Jenkins beheren**.
2. In de **beheren Jenkins** pagina, klikt u op **System configureren**.
3. In de **configuratie van Microsoft Azure Storage-Account** sectie:
   1. Voer de naam van uw opslagaccount, die u van verkrijgen kunt de [Azure Portal](https://portal.azure.com).
   2. Voer de sleutel van uw opslagaccount, ook kan worden verkregen uit de [Azure Portal](https://portal.azure.com).
   3. Gebruik de standaardwaarde voor **eindpunt-URL van Blob-Service** als u de openbare Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruikt u het eindpunt zoals opgegeven in de [Azure Portal](https://portal.azure.com) voor uw opslagaccount. 
   4. Klik op **storage-referenties gevalideerd** uw storage-account te valideren. 
   5. [Optioneel] Als u meer storage-accounts die u wilt die beschikbaar zijn gesteld uw CI Jenkins, klikt u op **Voeg meer Accounts voor opslag**.
   6. Klik op **opslaan** uw instellingen op te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Het maken van een na build-actie die uw artefacten build naar uw storage-account uploadt
Instructie oog wordt eerst moet we een taak die verschillende bestanden maken en voegt u de actie na build de bestanden uploaden naar uw storage-account te maken.

1. Klik in het dashboard van Jenkins op **New Item**.
2. Naam van de taak **MyJob**, klikt u op **bouwen van een project vrije-stijl software**, en klik vervolgens op **OK**.
3. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **toevoegen build stap** en kies **Windows uitvoeren van batch-opdracht**.
4. In **opdracht**, gebruik de volgende opdrachten:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. In de **na build acties** sectie van de taakconfiguratie van de, klikt u op **na build actie toevoegen** en kies **artefacten uploaden naar Azure Blob storage**.
6. Voor **opslagaccountnaam**, selecteer het opslagaccount moet worden gebruikt.
7. Voor **containernaam**, geef de containernaam. (De container gemaakt als deze niet bestaat nog wanneer de build-artefacten worden geüpload.) U kunt gebruik maken van omgevingsvariabelen, dus voor dit voorbeeld geeft **${JOB_NAME}** als de containernaam van de.
   
    **Tip**
   
    Onder de **opdracht** sectie waarin u hebt opgegeven een script voor **Windows uitvoeren van batch-opdracht** is een koppeling naar de omgevingsvariabelen die wordt herkend door Jenkins. Klik op de koppeling voor meer informatie over de namen van omgevingsvariabelen en beschrijvingen. Houd er rekening mee dat omgevingsvariabelen die bevat speciale tekens, zoals de **BUILD_URL** omgevingsvariabele, zijn niet toegestaan als een containernaam of algemene virtueel pad.
8. Klik op **nieuwe container openbaar maken standaard** voor dit voorbeeld. (Als u een persoonlijke container gebruiken wilt, hebt u nodig voor het maken van een shared access signature om toegang te verlenen. Dat is buiten het bereik van dit onderwerp. U kunt meer informatie over handtekeningen voor gedeelde toegang op [met behulp van Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [Optioneel] Klik op **schone container voordat u uploadt** als u wilt dat de container van inhoud worden gewist voordat build artefacten worden geüpload (laat het vakje uitgeschakeld als u niet wilt opschonen van de inhoud van de container).
10. Voor **lijst van artefacten uploaden**, voer  **tekst /*.txt**.
11. Voor **algemene virtuele pad voor de geüploade artefacten**, voert u deze zelfstudie **${bouwen\_ID} / ${bouwen\_getal}**.
12. Klik op **opslaan** uw instellingen op te slaan.
13. Klik in het dashboard Jenkins **nu bouwen** om uit te voeren **MyJob**. Bekijk de uitvoer van de console voor status. Statusberichten voor Azure-opslag worden opgenomen in de console-uitvoer wanneer de actie na build build artefacten uploaden wordt gestart.
14. De taak is voltooid, kunt u de build-artefacten controleren door het openen van de openbare blob.
    1. Meld u aan bij de [Azure-Portal](https://portal.azure.com).
    2. Klik op **opslag**.
    3. Klik op de opslagaccountnaam die u voor Jenkins gebruikt.
    4. Klik op **Containers**.
    5. Klik op de container met de naam **myjob**, die de kleine versie van de naam van de taak die u tijdens het maken van de taak Jenkins toegewezen is. Namen van containers en blobs namen zijn kleine letters (en hoofdlettergevoelig) in Azure-opslag. In de lijst met BLOB's voor de container met de naam **myjob** ziet u **hello.txt** en **date.txt**. Kopieer de URL voor een van deze items en open het in uw browser. Als een artefact build ziet u het tekstbestand dat is geüpload.

Slechts één na build-actie die artefacten naar Azure blob storage uploadt kan per taak worden gemaakt. De actie na build artefacten uploaden naar Azure blob-opslag kunt opgeven verschillende bestanden (inclusief jokertekens) en paden naar bestanden binnen **lijst van artefacten uploaden** met een puntkomma als scheidingsteken. Bijvoorbeeld, als uw Jenkins bouwen produceert JAR-bestanden en TXT-bestanden in uw werkruimte **bouwen** map en u wilt uploaden beide naar Azure blob storage, gebruikt u de volgende voor de **lijst van artefacten uploaden** waarde: **bouwen /\*JAR; build /\*.txt**. U kunt de syntaxis van de dubbele ook gebruiken om een pad in de blob-naam gebruiken. Bijvoorbeeld, als u wilt dat de potten ophalen geüpload met behulp van **binaire bestanden** in de blobpad en de TXT-bestanden ophalen geüpload met behulp van **mededelingen** in het blobpad, gebruikt u de volgende voor de **lijst van artefacten uploaden** waarde: **bouwen /\*. jar::binaries; build /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Het maken van een build-stap die u van Azure blob-opslag downloadt
De volgende stappen laten zien hoe de stap in een build voor het downloaden van items uit Azure blob-opslag configureren. Dit is handig als u wilt de items in de build bevatten, bijvoorbeeld potten die u in Azure blob-opslag.

1. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **toevoegen build stap** en kies **downloaden uit Azure Blob storage**.
2. Voor **opslagaccountnaam**, selecteer het opslagaccount moet worden gebruikt.
3. Voor **containernaam**, geef de naam van de container met de blobs die u wilt downloaden. U kunt de omgevingsvariabelen.
4. Voor **Blob-naam**, geef de blobnaam. U kunt de omgevingsvariabelen. U kunt ook een sterretje als jokerteken nadat u de eerste letter (s) van de blob-naam opgeven. Bijvoorbeeld: **project\***  geeft alle blobs die met beginnen **project**.
5. [Optioneel] Voor **downloadpad**, geef het pad op de Jenkins machine waar u bestanden wilt downloaden uit Azure blob-opslag. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u geen waarde voor opgeeft **downloadpad**, de bestanden uit Azure blob storage worden gedownload naar de werkruimte van de taak.)

Als er extra items die u wilt downloaden uit Azure blob storage, kunt u aanvullende build stappen.

Nadat u een build uitgevoerd, kunt u de uitvoer van de build geschiedenis-console controleren of kijken naar de downloadlocatie, om te zien of de blobs die u verwacht zijn gedownload.  

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de Blob-service
Het volgende gedeelte bevat een overzicht van de onderdelen van de Blob-service.

* **Opslagaccount**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Dit is het hoogste niveau van de naamruimte voor toegang tot blobs. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte minder dan 100TB is.
* **Container**: een container is een groepering van een reeks blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.
* **BLOB**: een bestand van willekeurig type en de grootte. Er zijn twee typen kunnen worden opgeslagen in Azure Storage blobs: blok en pagina-blobs. De meeste bestanden zijn blok-blobs. Een enkel blok-blob kan maximaal 200GB groot zijn. Deze zelfstudie maakt gebruik van blok-blobs. Pagina-blobs, een ander blobtype, mag maximaal 1TB in grootte en zijn efficiënter als bereiken in bytes in een bestand vaak worden gewijzigd. Zie voor meer informatie over blobs [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL-indeling**: Blobs worden opgevraagd met de volgende URL-indeling:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (De indeling hierboven van toepassing op de openbare Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruikt u het eindpunt in de [Azure Portal](https://portal.azure.com) om te bepalen van de URL-eindpunt.)
  
    In de bovenstaande indeling `storageaccount` vertegenwoordigt de naam van uw opslagaccount `container_name` vertegenwoordigt de naam van de container en `blob_name` vertegenwoordigt de naam van uw blob, respectievelijk. U kunt meerdere paden, gescheiden door een slash hebben binnen de containernaam  **/** . De naam van de voorbeeld-container in deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_getal}** is gebruikt voor het algemene virtueel pad, wat resulteert in de blob met een URL van de volgende notatie:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen
* [Voldoen aan Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
* [Azure-opslag-SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage Client SDK-naslaginformatie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage/)

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).