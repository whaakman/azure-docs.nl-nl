---
title: Azure Storage gebruiken met een Jenkins CI-oplossing
description: In deze zelfstudie laten zien hoe de Azure blob-service gebruiken als een opslagplaats voor artefacten die zijn gemaakt door een Jenkins CI-oplossing bouwen.
ms.topic: article
ms.author: tarcher
author: tarcher
services: devops
custom: jenkins
ms.date: 07/31/2018
ms.component: common
ms.openlocfilehash: b7ac5c277b45ae5c005a284d5c38e55099c11e33
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735559"
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Azure Storage gebruiken met een Jenkins CI-oplossing

In dit artikel ziet u hoe u Blob storage gebruiken als opslagplaats voor gebouwde artefacten die zijn gemaakt door een Jenkins-oplossing voor continue integratie (CI) of als een bron van downloadbare bestanden moet worden gebruikt in een buildproces. Een van de scenario's waarin u zijn deze oplossing interessant is wanneer u in een flexibele ontwikkeling-omgeving coderen bent (met behulp van Java of andere talen), builds worden uitgevoerd op basis van continue integratie en u een opslagplaats voor uw build-artefacten moet, zodat u kan bijvoorbeeld delen met andere leden van de organisatie, uw klanten, of houd een archief. Een ander scenario is wanneer uw build-taak zelf andere bestanden, bijvoorbeeld afhankelijkheden te downloaden als onderdeel van de invoer-build vereist.

In deze zelfstudie hebt zal u gebruikmaken van de Azure Storage-invoegtoepassing voor Jenkins CI beschikbaar gesteld door Microsoft.

## <a name="jenkins-overview"></a>Jenkins-overzicht
Jenkins kunt continue integratie van een softwareproject door ontwikkelaars kunnen hun wijzigingen in de code eenvoudig kunt integreren en builds geproduceerde automatisch en regelmatig, waardoor de productiviteit van de ontwikkelaars wordt verhoogd. Builds worden samengesteld en build-artefacten naar diverse opslagplaatsen kunnen worden geüpload. Dit artikel wordt beschreven hoe u Azure blob-opslag gebruikt als de opslagplaats van de build-artefacten. U ziet ook hoe u kunt downloaden van afhankelijkheden van Azure blob-opslag.

Meer informatie over Jenkins kan worden gevonden op [voldoen aan Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service
Voordelen van het gebruik van de Blob-service voor het hosten van uw build-artefacten voor flexibele ontwikkeling zijn:

* Hoge beschikbaarheid van uw build-artefacten en/of de downloadbare afhankelijkheden.
* Prestaties wanneer uw Jenkins CI-oplossing uw build-artefacten uploadt.
* Prestaties wanneer uw klanten en partners downloadt uw build-artefacten.
* De controle over het toegangsbeleid van de gebruiker met de keuze tussen anonieme toegang, gedeelde toegang op basis van een verlopen handtekening toegang, persoonlijke toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten
* Een Jenkins CI-oplossing.
  
    Als u een Jenkins CI-oplossing op dit moment geen hebt, kunt u een Jenkins CI-oplossing met behulp van de volgende techniek uitvoeren:
  
  1. Op een machine Java-functionaliteit downloaden jenkins.war van <http://jenkins-ci.org>.
  2. Bij een opdrachtprompt naar de map met jenkins.war wordt geopend, voert u de volgende uit:
     
      `java -jar jenkins.war`

  3. Open in uw browser `http://localhost:8080/` openen van het dashboard van Jenkins, die u kunt installeren en configureren van de Azure Storage-invoegtoepassing.
     
      Tijdens een typische Jenkins CI-oplossing kan worden ingesteld voor het uitvoeren als een service, die de Jenkins-war wordt uitgevoerd op de opdrachtregel is voldoende voor deze zelfstudie.
* Een Azure-account. U kunt zich aanmelden voor een Azure-account op <http://www.azure.com>.
* Een Azure Storage-account. Als u nog een storage-account hebt, kunt u maken met de stappen in [een Opslagaccount maken](../common/storage-quickstart-create-account.md).
* Vertrouwd zijn met de Jenkins CI-oplossing wordt aanbevolen maar niet vereist, zoals de volgende inhoud maakt gebruik van een eenvoudige voorbeeld om weer te geven de stappen die nodig zijn bij het gebruik van de Blob-service als een opslagplaats voor Jenkins CI build-artefacten.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>De Blob-service gebruiken met Jenkins CI
Voor het gebruik van de Blob-service met Jenkins, moet u de Azure Storage-invoegtoepassing installeren, configureren van de invoegtoepassing voor het gebruik van uw storage-account en maak vervolgens een actie na bouwen die uw build-artefacten geüpload naar uw opslagaccount. Deze stappen worden beschreven in de volgende secties.

## <a name="how-to-install-the-azure-storage-plugin"></a>De Azure Storage-invoegtoepassing installeren
1. Selecteer in het dashboard van Jenkins **Manage Jenkins**.
2. In de **Manage Jenkins** weergeeft, schakelt **Plug-ins beheren**.
3. Selecteer het tabblad **Beschikbaar**.
4. In de **artefact opleggen aan Uploaders** sectie selectievakje **Microsoft Azure Storage-invoegtoepassing**.
5. Selecteer een **installeren zonder opnieuw opstarten** of **nu downloaden en installeren na opnieuw opstarten**.
6. Start opnieuw op Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Het configureren van de Azure Storage-invoegtoepassing voor het gebruik van uw storage-account
1. Selecteer in het dashboard van Jenkins **Manage Jenkins**.
2. In de **Manage Jenkins** weergeeft, schakelt **Configure System**.
3. In de **configuratie van Microsoft Azure Storage-Account** sectie:
   1. Voer de naam van uw opslagaccount, die u van verkrijgen kunt de [Azure Portal](https://portal.azure.com).
   2. Voer de sleutel van uw opslagaccount, ook kan worden verkregen uit de [Azure Portal](https://portal.azure.com).
   3. Gebruik de standaardwaarde voor **eindpunt-URL van Blob-Service** als u de globale Azure-cloud. Als u een andere Azure-cloud gebruikt, gebruik het eindpunt zoals opgegeven in de [Azure Portal](https://portal.azure.com) voor uw opslagaccount. 
   4. Selecteer **storage-referenties te valideren** om uw storage-account te valideren. 
   5. [Optioneel] Hebt u extra opslagaccounts die u beschikbaar gesteld aan uw Jenkins CI, selecteer **meer Opslagaccounts toevoegen**.
   6. Selecteer **opslaan** uw instellingen op te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Een actie na bouwen die uw build-artefacten naar uw storage-account uploadt maken
Voor educatieve doeleinden moet u eerst een taak voor het maken van meerdere bestanden en voeg deze in de actie na bouwen de bestanden te uploaden naar uw storage-account maken.

1. Selecteer in het dashboard van Jenkins **Nieuw Item**.
2. Naam van de taak **MyJob**, selecteer **bouwen van een softwareproject free-style**, en selecteer vervolgens **OK**.
3. In de **bouwen** sectie van de taakconfiguratie, selecteert u **build-stap toevoegen** en selecteer **Windows uitvoeren batchopdracht**.
4. In **opdracht**, gebruikt u de volgende opdrachten:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. In de **acties na bouwen** sectie van de taakconfiguratie, selecteert u **toevoegen na build-actie** en selecteer **artefacten uploaden naar Azure Blob-opslag**.
6. Voor **opslagaccountnaam**, selecteer het opslagaccount te gebruiken.
7. Voor **containernaam**, geef de containernaam van de. (De container wordt gemaakt als deze nog niet bestaat wanneer de build-artefacten worden geüpload.) U kunt omgevingsvariabelen gebruiken, dus voor dit voorbeeld voert `${JOB_NAME}` als de containernaam van de.
   
    **Tip**
   
    Onder de **opdracht** sectie waarin u een script voor het opgegeven **Windows uitvoeren batchopdracht** is een koppeling naar de omgevingsvariabelen die wordt herkend door Jenkins. Selecteer de koppeling voor meer informatie over de namen van omgevingsvariabelen en beschrijvingen. Omgevingsvariabelen met speciale tekens, zoals de **BUILD_URL** omgevingsvariabele, zijn niet toegestaan als een containernaam of algemene virtueel pad.
8. Selecteer **nieuwe container openbaar maken standaard** voor dit voorbeeld. (Als u een privé-container gebruiken wilt, moet u het maken van een shared access signature voor toegang, ligt buiten het bereik van dit artikel. U kunt meer informatie over handtekeningen voor gedeelde toegang op [met behulp van Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. [Optioneel] Selecteer **schone container voordat u uploadt** als u wilt dat de container van de inhoud van worden gewist voordat build-artefacten worden geüpload (laat het vakje uitgeschakeld als u niet wilt opschonen van de inhoud van de container).
10. Voor **lijst van artefacten uploaden**, voer `text/*.txt`.
11. Voor **algemene virtuele pad voor geüploade artefacten**, voor deze zelfstudie voert `${BUILD\_ID}/${BUILD\_NUMBER}`.
12. Selecteer **opslaan** uw instellingen op te slaan.
13. Selecteer in het dashboard van Jenkins **Build Now** om uit te voeren **MyJob**. Bekijk de uitvoer van de console voor status. Statusberichten voor Azure storage worden opgenomen in de console-uitvoer wanneer de na build-actie start voor het uploaden van de build-artefacten.
14. Na voltooiing van de taak, kunt u de build-artefacten controleren door het openen van de openbare blob.
    1. Meld u aan bij [Azure-portal](https://portal.azure.com).
    2. Selecteer **Opslag**.
    3. Selecteer de naam van het opslagaccount die u hebt gebruikt voor Jenkins.
    4. Selecteer **Containers**.
    5. Selecteer de container met de naam **myjob**, die de kleine versie van de naam van de taak die u hebt toegewezen tijdens het maken van de Jenkins-taak is. Namen van containers en blob-namen zijn kleine letters (hoofdlettergevoelig) in Azure storage. In de lijst met blobs voor de container met de naam **myjob**, ziet u **hello.txt** en **date.txt**. Kopieer de URL voor een van deze items en opent u het in uw browser. Als een build-artefact ziet u het tekstbestand dat is geüpload.

Slechts één na build-actie die wordt artefacten geüpload naar Azure blob-opslag kan per taak worden gemaakt. De actie na bouwen artefacten uploaden naar Azure blob-opslag kunt opgeven voor verschillende bestanden (inclusief jokertekens) en paden naar bestanden in **lijst van artefacten uploaden** met een puntkomma als scheidingsteken. Bijvoorbeeld, als uw Jenkins-build produceert JAR-bestanden en een TXT-bestanden in van uw werkruimte **bouwen** map en u wilt uploaden beide naar Azure blob-opslag, gebruikt u de volgende waarde voor de **lijst van artefactenuploaden** optie: `build/\*.jar;build/\*.txt`. U kunt ook de syntaxis van de dubbele gebruiken om een pad om te gebruiken binnen de blob-naam te geven. Bijvoorbeeld, als u wilt dat de JAR-bestanden ophalen geüpload met behulp van **binaire bestanden** in de blobpad en de TXT-bestanden ophalen geüpload met behulp van **kennisgevingen** in het blobpad, gebruik de volgende waarde voor de **lijst Artefacten uploaden** optie: `build/\*.jar::binaries;build/\*.txt::notices`.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Over het maken van een build-stap die kan worden gedownload uit Azure blob-opslag
De volgende stappen laten zien voor het configureren van een build-stap om items te downloaden vanuit Azure blob storage, dit handig is als u artikelen wilt opnemen in uw build. Een voorbeeld van het gebruik van dit patroon is JAR-bestanden die u wilt behouden in Azure blob-opslag.

1. In de **bouwen** sectie van de taakconfiguratie, selecteert u **build-stap toevoegen** en selecteer **downloaden uit Azure Blob storage**.
2. Voor **opslagaccountnaam**, selecteer het opslagaccount te gebruiken.
3. Voor **containernaam**, geef de naam van de container met de blobs die u wilt downloaden. U kunt omgevingsvariabelen gebruiken.
4. Voor **blobnaam**, geef de naam van de blob. U kunt omgevingsvariabelen gebruiken. Bovendien kunt u een sterretje als jokerteken nadat u de eerste letter (s) van de blob-naam opgeven. Bijvoorbeeld, **project\***  geeft alle blobs waarvan de namen met beginnen **project**.
5. [Optioneel] Voor **downloadpad**, geef het pad op de Jenkins-machine waar u bestanden wilt downloaden uit Azure blob-opslag. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u een waarde op voor geen opgeeft **downloadpad**, de bestanden vanuit Azure blob storage naar de werkruimte van de taak zal worden gedownload.)

Als u extra items die u wilt downloaden uit Azure blob-opslag hebt, kunt u extra build-stappen kunt maken.

Nadat u een build uitvoert, kunt u het console-uitvoer van de build-geschiedenis te controleren of kijken naar de downloadlocatie, om te zien of de blobs die u wilde zijn gedownload.  

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de Blob-service
Deze sectie bevat een overzicht van de onderdelen van de Blob-service.

* **Opslagaccount**: alle toegang tot Azure Storage vindt plaats via een opslagaccount. Een storage-account is het hoogste niveau van de naamruimte voor toegang tot blobs. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte onder 100 TB is.
* **Container**: een container is een groepering van een reeks blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.
* **BLOB**: een bestand van willekeurig type en grootte. Er zijn twee typen die kunnen worden opgeslagen in Azure Storage-blobs: blok- en pagina-blobs. De meeste bestanden zijn blok-blobs. Eén blok-blob kan maximaal 200 GB groot zijn. In deze zelfstudie maakt gebruik van blok-blobs. Pagina-blobs, een ander blobtype, kunnen tot 1 TB in grootte, en zijn efficiënter zijn bij het bereiken van de bytes in een bestand regelmatig worden gewijzigd. Zie voor meer informatie over blobs [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL-indeling**: Blobs kunnen worden opgevraagd met de volgende URL-indeling:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (De bovenstaande indeling van toepassing op de wereldwijde Azure-cloud. Als u van een andere Azure-cloud gebruikmaakt, gebruikt u het eindpunt in de [Azure Portal](https://portal.azure.com) om te bepalen van uw URL-eindpunt.)
  
    In de bovenstaande indeling `storageaccount` vertegenwoordigt de naam van uw opslagaccount `container_name` vertegenwoordigt de naam van uw container en `blob_name` vertegenwoordigt de naam van de blob, respectievelijk. In de containernaam van de, kunt u meerdere paden, gescheiden door een slash hebben **/**. De naam van het voorbeeld gebruikt voor deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_getal}** is gebruikt voor het algemene virtueel pad, wat resulteert in de blob met een URL de volgende vorm:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="troubleshooting-the-jenkins-plugin"></a>Problemen met de Jenkins-invoegtoepassing oplossen

Als u problemen ondervindt met de Jenkins-invoegtoepassingen, kunt u in [Jenkins JIRA](https://issues.jenkins-ci.org/) een ticket openen voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen
* [Voldoen aan Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
* [Azure Storage SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage Client SDK-referentie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage/)

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).