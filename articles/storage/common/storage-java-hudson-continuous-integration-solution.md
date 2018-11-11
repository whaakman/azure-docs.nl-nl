---
title: Hudson gebruiken met Blob-opslag | Microsoft Docs
description: Beschrijft hoe u het gebruik van Hudson met Azure Blob-opslag als een opslagplaats voor gebouwde artefacten.
services: storage
author: seguler
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.component: common
ms.openlocfilehash: a62af881efd4f6f3422db19850a0ce9987c6ae7b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245117"
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Azure Storage gebruiken met een Hudson CI-oplossing
## <a name="overview"></a>Overzicht
De volgende informatie laat zien hoe Blob storage gebruiken als opslagplaats voor gebouwde artefacten die zijn gemaakt door een continue integratie (CI) Hudson-oplossing, of als een bron van downloadbare bestanden moet worden gebruikt in een buildproces. Een van de scenario's waarin u zijn dit interessant is wanneer u in een flexibele ontwikkeling-omgeving coderen bent (met behulp van Java of andere talen), builds worden uitgevoerd op basis van continue integratie en u een opslagplaats voor uw build-artefacten moet, zodat u kunt bijvoorbeeld, delen met andere leden van de organisatie, uw klanten, of houd een archief.  Een ander scenario is wanneer uw build-taak zelf andere bestanden, bijvoorbeeld afhankelijkheden te downloaden als onderdeel van de invoer-build vereist.

In deze zelfstudie wordt u de Azure Storage-invoegtoepassing voor Hudson CI beschikbaar gesteld door Microsoft.

## <a name="introduction-to-hudson"></a>Inleiding tot Hudson
Hudson kunt continue integratie van een softwareproject door ontwikkelaars kunnen hun wijzigingen in de code eenvoudig kunt integreren en builds geproduceerde automatisch en regelmatig, waardoor de productiviteit van de ontwikkelaars wordt verhoogd. Builds worden samengesteld en build-artefacten naar diverse opslagplaatsen kunnen worden geüpload. Dit artikel wordt beschreven hoe u Azure Blob-opslag gebruikt als de opslagplaats van de build-artefacten. U ziet ook hoe u kunt downloaden van afhankelijkheden van Azure Blob-opslag.

Meer informatie over Hudson kan worden gevonden op [Hudson voldoen aan](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service
Voordelen van het gebruik van de Blob-service voor het hosten van uw build-artefacten voor flexibele ontwikkeling zijn:

* Hoge beschikbaarheid van uw build-artefacten en/of de downloadbare afhankelijkheden.
* Prestaties wanneer uw Hudson CI-oplossing uw build-artefacten uploadt.
* Prestaties wanneer uw klanten en partners downloadt uw build-artefacten.
* De controle over het toegangsbeleid van de gebruiker met de keuze tussen anonieme toegang, gedeelde toegang op basis van een verlopen handtekening toegang, persoonlijke toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten
U moet de volgende voor het gebruik van de Blob-service met uw Hudson CI-oplossing:

* Een Hudson CI-oplossing.
  
    Als u een Hudson CI-oplossing op dit moment geen hebt, kunt u een Hudson CI-oplossing met behulp van de volgende techniek uitvoeren:
  
  1. Op een machine Java-functionaliteit downloaden via het WAR Hudson van <http://hudson-ci.org/>.
  2. Bij een opdrachtprompt naar de map met de Hudson WAR wordt geopend, het WAR Hudson worden uitgevoerd. Bijvoorbeeld, als u versie 3.1.2 hebt gedownload:
     
      `java -jar hudson-3.1.2.war`

  3. Open in uw browser `http://localhost:8080/`. Hiermee opent u het dashboard Hudson.
  4. Bij het eerste gebruik van Hudson voltooit u de initiële installatie op `http://localhost:8080/`.
  5. Nadat u de initiële installatie is voltooid, het actieve exemplaar van de WAR Hudson annuleren, opnieuw starten van de WAR Hudson en opnieuw het dashboard Hudson opent `http://localhost:8080/`, die u wilt gebruiken voor het installeren en configureren van de Azure Storage-invoegtoepassing.
     
      Tijdens een typische Hudson CI-oplossing kan worden ingesteld voor het uitvoeren als een service, de war Hudson uitgevoerd op de opdrachtregel is voldoende voor deze zelfstudie.
* Een Azure-account. U kunt zich aanmelden voor een Azure-account op <http://www.azure.com>.
* Een Azure Storage-account. Als u nog een storage-account hebt, kunt u maken met de stappen in [een Opslagaccount maken](../common/storage-quickstart-create-account.md).
* Vertrouwd zijn met de Hudson CI-oplossing wordt aanbevolen maar niet vereist, zoals de volgende inhoud maakt gebruik van een eenvoudige voorbeeld om weer te geven de stappen die nodig zijn bij het gebruik van de Blob-service als een opslagplaats voor Hudson CI build-artefacten.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>De Blob-service gebruiken met Hudson CI
Voor het gebruik van de Blob-service met Hudson, moet u de Azure Storage-invoegtoepassing installeren, configureren van de invoegtoepassing voor het gebruik van uw storage-account en maak vervolgens een actie na bouwen die uw build-artefacten geüpload naar uw opslagaccount. Deze stappen worden beschreven in de volgende secties.

## <a name="how-to-install-the-azure-storage-plugin"></a>De Azure Storage-invoegtoepassing installeren
1. Klik in het dashboard van Hudson **Hudson beheren**.
2. Op de **beheren Hudson** pagina, klikt u op **Plug-ins beheren**.
3. Klik op de **beschikbaar** tabblad.
4. Klik op **anderen**.
5. In de **artefact opleggen aan Uploaders** sectie, selecteer **Microsoft Azure Storage-invoegtoepassing**.
6. Klik op **Install**.
7. Nadat de installatie voltooid is, opnieuw opstarten Hudson.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Het configureren van de Azure Storage-invoegtoepassing voor het gebruik van uw storage-account
1. Klik in het dashboard van Hudson **Hudson beheren**.
2. Op de **beheren Hudson** pagina, klikt u op **Configure System**.
3. In de **configuratie van Microsoft Azure Storage-Account** sectie:
   
    a. Voer de naam van uw opslagaccount, die u van verkrijgen kunt de [Azure Portal](https://portal.azure.com).
   
    b. Voer de sleutel van uw opslagaccount, ook kan worden verkregen uit de [Azure Portal](https://portal.azure.com).
   
    c. Gebruik de standaardwaarde voor **eindpunt-URL van Blob-Service** als u de openbare Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruik het eindpunt zoals opgegeven in de [Azure Portal](https://portal.azure.com) voor uw opslagaccount.
   
    d. Klik op **storage-referenties te valideren** om uw storage-account te valideren.
   
    e. [Optioneel] Als u meer storage-accounts die u beschikbaar gesteld aan uw Hudson CI, klikt u op **meer opslagaccounts toevoegen**.
   
    f. Klik op **opslaan** uw instellingen op te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Een actie na bouwen die uw build-artefacten naar uw storage-account uploadt maken
Voor instructie-doeleinden, eerst moet we een taak voor het maken van meerdere bestanden en voeg deze in de actie na bouwen de bestanden te uploaden naar uw storage-account maken.

1. Klik in het dashboard van Hudson **nieuwe taak**.
2. Naam van de taak **MyJob**, klikt u op **bouwen van een taak free-style software**, en klik vervolgens op **OK**.
3. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **build-stap toevoegen** en kies **Windows uitvoeren batchopdracht**.
4. In **opdracht**, gebruikt u de volgende opdrachten:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. In de **acties na bouwen** sectie van de taakconfiguratie van de, klikt u op **artefacten uploaden naar Microsoft Azure-blobopslag**.
6. Voor **Opslagaccountnaam**, selecteer het opslagaccount te gebruiken.
7. Voor **containernaam**, geef de containernaam van de. (De container wordt gemaakt als deze nog niet bestaat wanneer de build-artefacten worden geüpload.) U kunt omgevingsvariabelen gebruiken, dus voor dit voorbeeld voert **${JOB_NAME}** als de containernaam van de.
   
    **Tip**
   
    Onder de **opdracht** sectie waarin u een script voor het opgegeven **Windows uitvoeren batchopdracht** is een koppeling naar de omgevingsvariabelen die wordt herkend door Hudson. Klik op de koppeling voor meer informatie over de namen van omgevingsvariabelen en beschrijvingen. Houd er rekening mee dat omgevingsvariabelen die bevat speciale tekens, zoals de **BUILD_URL** omgevingsvariabele, zijn niet toegestaan als een containernaam of algemene virtueel pad.
8. Klik op **nieuwe container openbaar maken standaard** voor dit voorbeeld. (Als u een privé-container gebruiken wilt, moet u het maken van een shared access signature om toegang te verlenen. Dat is buiten het bereik van dit artikel. U kunt meer informatie over handtekeningen voor gedeelde toegang op [met behulp van Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [Optioneel] Klik op **schone container voordat u uploadt** als u wilt dat de container van de inhoud van worden gewist voordat build-artefacten worden geüpload (laat het vakje uitgeschakeld als u niet wilt opschonen van de inhoud van de container).
10. Voor **lijst van artefacten uploaden**, voer **tekst/*.txt**.
11. Voor **algemene virtuele pad voor geüploade artefacten**, voer **${bouwen\_ID} / ${bouwen\_getal}**.
12. Klik op **opslaan** uw instellingen op te slaan.
13. Klik in het dashboard Hudson **Build Now** om uit te voeren **MyJob**. Bekijk de uitvoer van de console voor status. Statusberichten voor Azure Storage worden opgenomen in de console-uitvoer wanneer de na build-actie start voor het uploaden van de build-artefacten.
14. Na voltooiing van de taak, kunt u de build-artefacten controleren door het openen van de openbare blob.
    
    a. Meld u aan bij [Azure-portal](https://portal.azure.com).
    
    b. Klik op **opslag**.
    
    c. Klik op de naam van het opslagaccount dat u voor Hudson gebruikt.
    
    d. Klik op **Containers**.
    
    e. Klik op de container met de naam **myjob**, die de kleine versie van de naam van de taak die u hebt toegewezen tijdens het maken van de taak Hudson is. Namen van containers en blob-namen zijn kleine letters (hoofdlettergevoelig) in Azure Storage. In de lijst met blobs voor de container met de naam **myjob** ziet u **hello.txt** en **date.txt**. Kopieer de URL voor een van deze items en opent u het in uw browser. Als een build-artefact ziet u het tekstbestand dat is geüpload.

Slechts één na build-actie die wordt artefacten geüpload naar Azure Blob-opslag kan per taak worden gemaakt. Houd er rekening mee dat de actie na bouwen artefacten uploaden naar Azure Blob-opslag opgeven kunt verschillende bestanden (inclusief jokertekens) en paden naar bestanden in **lijst van artefacten uploaden** met een puntkomma als scheidingsteken. Bijvoorbeeld, als uw Hudson bouwen produceert JAR-bestanden en een TXT-bestanden in van uw werkruimte **bouwen** map en u wilt uploaden beide naar Azure Blob storage, gebruikt u de volgende voor de **lijst van artefacten uploaden** waarde: **bouwen /\*JAR; build /\*.txt**. U kunt ook de syntaxis van de dubbele gebruiken om een pad om te gebruiken binnen de blob-naam te geven. Bijvoorbeeld, als u wilt dat de JAR-bestanden ophalen geüpload met behulp van **binaire bestanden** in de blobpad en de TXT-bestanden ophalen geüpload met behulp van **kennisgevingen** in het blobpad, gebruikt u de volgende voor de **lijst van artefacten het uploaden van** waarde: **bouwen /\*. jar::binaries; build /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Over het maken van een build-stap die kan worden gedownload uit Azure Blob storage
De volgende stappen laten zien hoe het configureren van een build-stap om items te downloaden uit Azure Blob storage. Dit is handig als u artikelen wilt opnemen in uw build, bijvoorbeeld JAR-bestanden die u in Azure Blob-opslag wilt behouden.

1. In de **bouwen** sectie van de taakconfiguratie van de, klikt u op **build-stap toevoegen** en kies **downloaden uit Azure Blob storage**.
2. Voor **opslagaccountnaam**, selecteer het opslagaccount te gebruiken.
3. Voor **containernaam**, geef de naam van de container met de blobs die u wilt downloaden. U kunt omgevingsvariabelen gebruiken.
4. Voor **blobnaam**, geef de naam van de blob. U kunt omgevingsvariabelen gebruiken. Bovendien kunt u een sterretje als jokerteken nadat u de eerste letter (s) van de blob-naam opgeven. Bijvoorbeeld, **project\***  geeft alle blobs waarvan de namen met beginnen **project**.
5. [Optioneel] Voor **downloadpad**, geef het pad op de Hudson machine waar u bestanden wilt downloaden uit Azure Blob storage. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u een waarde op voor geen opgeeft **downloadpad**, de bestanden van Azure Blob-opslag naar de werkruimte van de taak zal worden gedownload.)

Als u extra items die u wilt downloaden uit Azure Blob storage hebt, kunt u extra build-stappen kunt maken.

Nadat u een build uitvoert, kunt u het console-uitvoer van de build-geschiedenis te controleren of kijken naar de downloadlocatie, om te zien of de blobs die u wilde zijn gedownload.

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de Blob-service
Het volgende biedt een overzicht van de onderdelen van de Blob-service.

* **Storage-account**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Dit is het hoogste niveau van de naamruimte voor toegang tot blobs. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte onder 100 TB is.
* **Container**: een container is een groepering van een reeks blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.
* **BLOB**: een bestand van willekeurig type en grootte. Er zijn twee typen die kunnen worden opgeslagen in Azure Storage-blobs: blok- en pagina-blobs. De meeste bestanden zijn blok-blobs. Eén blok-blob kan maximaal 200 GB groot zijn. In deze zelfstudie maakt gebruik van blok-blobs. Pagina-blobs, een ander blobtype, kunnen tot 1 TB in grootte, en zijn efficiënter zijn bij het bereiken van de bytes in een bestand regelmatig worden gewijzigd. Zie voor meer informatie over blobs [blok-Blobs, toevoeg-Blobs en pagina-Blobs](https://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL-indeling**: Blobs kunnen worden opgevraagd met de volgende URL-indeling:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (De bovenstaande indeling van toepassing op de openbare Azure-cloud. Als u van een andere Azure-cloud gebruikmaakt, gebruikt u het eindpunt in de [Azure Portal](https://portal.azure.com) om te bepalen van uw URL-eindpunt.)
  
    In de bovenstaande indeling `storageaccount` vertegenwoordigt de naam van uw opslagaccount `container_name` vertegenwoordigt de naam van uw container en `blob_name` vertegenwoordigt de naam van de blob, respectievelijk. In de containernaam van de, kunt u meerdere paden, gescheiden door een slash hebben **/**. De naam van de voorbeeld-container in deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_getal}** is gebruikt voor het algemene virtueel pad, wat resulteert in de blob met een URL van de de volgende vorm:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen
* [Voldoen aan de Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Azure Storage SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage Client SDK-referentie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](https://blogs.msdn.com/b/windowsazurestorage/)

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).