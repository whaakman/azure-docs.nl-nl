---
title: Hudson gebruiken met Blob Storage | Microsoft Docs
description: Hierin wordt beschreven hoe u Hudson gebruikt met Azure Blob Storage als opslag plaats voor het bouwen van artefacten.
services: storage
author: seguler
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: tarcher
ms.subservice: common
ms.openlocfilehash: 54e91d4df1109b9ece1150f8b44665789e4dfce1
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875890"
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Azure Storage gebruiken met een Hudson CI-oplossing
## <a name="overview"></a>Overzicht
In de volgende informatie ziet u hoe u Blob Storage kunt gebruiken als opslag plaats voor constructie artefacten die zijn gemaakt met een CI-oplossing (Hudson Continuous Integration) of als bron van Download bare bestanden die moeten worden gebruikt in een bouw proces. Een van de scenario's waarin dit nuttig is, is wanneer u codeert in een flexibele ontwikkel omgeving (met Java of andere talen), builds worden uitgevoerd op basis van doorlopende integratie en u hebt een opslag plaats voor uw bouw artefacten nodig, zodat u u kunt ze bijvoorbeeld delen met andere organisatie leden, uw klanten of een archief onderhouden.  Een ander scenario is wanneer uw bouw taak zelf andere bestanden vereist, zoals afhankelijkheden die moeten worden gedownload als onderdeel van de build-invoer.

In deze zelf studie gebruikt u de Azure Storage-invoeg toepassing voor Hudson CI die beschikbaar wordt gesteld door micro soft.

## <a name="introduction-to-hudson"></a>Inleiding tot Hudson
Hudson maakt continue integratie van een software project mogelijk door ontwikkel aars in staat te stellen hun code wijzigingen eenvoudig te integreren en gebouwde builds automatisch en regel matig te laten produceren, waardoor de productiviteit van de ontwikkel aars wordt verhoogd. Builds zijn versie en het bouwen van artefacten kan worden geüpload naar verschillende opslag plaatsen. In dit artikel wordt uitgelegd hoe u Azure Blob Storage kunt gebruiken als opslag plaats voor de build-artefacten. Er wordt ook weer gegeven hoe u afhankelijkheden downloadt van Azure Blob-opslag.

Meer informatie over Hudson vindt u op [Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-the-blob-service"></a>Voor delen van het gebruik van de Blob service
Voor delen van het gebruik van de Blob service om uw flexibele ontwikkel artefacten te hosten, zijn onder andere:

* Hoge Beschik baarheid van uw build-artefacten en/of download bare afhankelijkheden.
* Prestaties wanneer uw Hudson CI-oplossing uw build-artefacten uploadt.
* Prestaties wanneer uw klanten en partners uw build-artefacten downloaden.
* Controle over beleid voor gebruikers toegang, met een keuze tussen anonieme toegang, op verloop van gedeelde toegangs handtekeningen, persoonlijke toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om de Blob service te gebruiken met uw Hudson CI-oplossing:

* Een Hudson-oplossing voor continue integratie.
  
    Als u momenteel geen Hudson CI-oplossing hebt, kunt u een Hudson CI-oplossing uitvoeren met behulp van de volgende techniek:
  
  1. Down load de Hudson-WAR van <http://hudson-ci.org/>op een Java-computer.
  2. Voer de Hudson WAR uit vanaf een opdracht prompt die is geopend in de map met de Hudson WAR. Als u bijvoorbeeld versie 3.1.2 hebt gedownload:
     
      `java -jar hudson-3.1.2.war`

  3. Open `http://localhost:8080/`in uw browser. Hiermee wordt het Hudson-dash board geopend.
  4. Bij het eerste gebruik van Hudson voltooit u de eerste `http://localhost:8080/`installatie op.
  5. Nadat u de eerste Setup hebt voltooid, annuleert u het actieve exemplaar van de Hudson War, start u de Hudson oorlog opnieuw en opent u het dash `http://localhost:8080/`Board Hudson, dat u gebruikt voor het installeren en configureren van de Azure Storage-invoeg toepassing.
     
      Hoewel een typische Hudson CI-oplossing zo is ingesteld dat deze als een service wordt uitgevoerd, is het uitvoeren van de Hudson War op de opdracht regel voldoende voor deze zelf studie.
* Een Azure-account. U kunt zich registreren voor een Azure-account <https://www.azure.com>op.
* Een Azure Storage-account. Als u nog geen opslag account hebt, kunt u er een maken met behulp van de stappen in [een opslag account maken](../common/storage-quickstart-create-account.md).
* De vertrouwdheid van de Hudson CI-oplossing wordt aanbevolen, maar is niet vereist, omdat de volgende inhoud een basis voorbeeld gebruikt om u te laten zien welke stappen nodig zijn bij het gebruik van de Blob service als opslag plaats voor Hudson CI-build-artefacten.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Het Blob service gebruiken met Hudson CI
Als u de Blob service wilt gebruiken met Hudson, moet u de Azure Storage-invoeg toepassing installeren, de invoeg toepassing configureren voor het gebruik van uw opslag account en vervolgens een actie na het bouwen maken waarmee uw build-artefacten worden geüpload naar uw opslag account. Deze stappen worden beschreven in de volgende secties.

## <a name="how-to-install-the-azure-storage-plugin"></a>De Azure Storage-invoeg toepassing installeren
1. Klik in het Hudson-dash board op **Hudson beheren**.
2. Klik op de pagina **Hudson beheren** op **invoeg toepassingen beheren**.
3. Klik op het tabblad **beschikbaar** .
4. Klik op **anderen**.
5. Selecteer **Microsoft Azure Storage-invoeg toepassing**in de sectie **artefacten uploaden** .
6. Klik op **Install**.
7. Nadat de installatie is voltooid, start u Hudson opnieuw.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>De Azure Storage-invoeg toepassing configureren voor het gebruik van uw opslag account
1. Klik in het Hudson-dash board op **Hudson beheren**.
2. Klik op de pagina **Hudson beheren** op **systeem configureren**.
3. In het gedeelte **configuratie van Microsoft Azure Storage-account** :
   
    a. Voer de naam van uw opslag account in, die u kunt verkrijgen via [Azure Portal](https://portal.azure.com).
   
    b. Voer de sleutel voor het opslag account in, die ook kan worden verkregen via [Azure Portal](https://portal.azure.com).
   
    c. Gebruik de standaard waarde voor de URL van het **eind punt** van de BLOB-service als u gebruikmaakt van de open bare Azure-Cloud. Als u een andere Azure-Cloud gebruikt, gebruikt u het eind punt zoals opgegeven in [Azure Portal](https://portal.azure.com) voor uw opslag account.
   
    d. Klik op **opslag referenties valideren** om uw opslag account te valideren.
   
    e. Beschrijving Als u extra opslag accounts hebt die u beschikbaar wilt maken voor uw Hudson-CI, klikt u op **meer opslag accounts toevoegen**.
   
    f. Klik op **Opslaan** om uw instellingen op te slaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Het maken van een actie na het bouwen waarmee uw build-artefacten worden geüpload naar uw opslag account
Voor instructie doeleinden moet eerst een taak worden gemaakt waarmee meerdere bestanden worden gemaakt en vervolgens worden toegevoegd aan de actie na het bouwen om de bestanden te uploaden naar uw opslag account.

1. Klik in het Hudson-dash board op **nieuwe taak**.
2. Geef de taak de naam **MyJob**, klik op **een gratis software taak bouwen**en klik vervolgens op **OK**.
3. Klik in de sectie **Build** van de taak configuratie op **Build-stap toevoegen** en kies **Windows batch-opdracht uitvoeren**.
4. Gebruik de volgende opdrachten in de **opdracht**:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. Klik in de sectie **acties na het maken** van de taak configuratie op **artefacten uploaden om de Blob-opslag te Microsoft Azure**.
6. Selecteer bij **naam van opslag account**het opslag account dat u wilt gebruiken.
7. Geef voor **container name**de naam van de container op. (De container wordt gemaakt als deze nog niet bestaat wanneer de build-artefacten worden geüpload.) U kunt omgevings variabelen gebruiken. in dit voor beeld wordt **$ {JOB_NAME}** als container naam ingevoerd.
   
    **Tip**
   
    Onder de **opdracht** sectie waar u een script voor de **opdracht Windows Batch uitvoeren** hebt opgegeven, is een koppeling naar de omgevings variabelen die worden herkend door Hudson. Klik op deze koppeling voor meer informatie over de namen en beschrijvingen van omgevings variabelen. Houd er rekening mee dat omgevings variabelen die speciale tekens bevatten, zoals de **BUILD_URL** -omgevings variabele, niet zijn toegestaan als container naam of algemeen virtueel pad.
8. Klik op **nieuwe container standaard openbaar maken** voor dit voor beeld. (Als u een persoonlijke container wilt gebruiken, moet u een hand tekening voor gedeelde toegang maken om toegang toe te staan. Dit valt buiten het bereik van dit artikel. Meer informatie over gedeelde toegangs handtekeningen kunt u vinden [met behulp van Shared Access signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md).)
9. Beschrijving Klik op **container opschonen voordat** u deze uploadt als u wilt dat de container wordt gewist voordat er artefacten worden geüpload (laat dit selectie vakje uitgeschakeld als u de inhoud van de container niet wilt opschonen).
10. Voer **tekst/*. txt**in **Als u een lijst met artefacten wilt uploaden**.
11. Voor het **algemene virtuele pad voor geüploade artefacten**typt u **$\_{build id}/$\_{Build Number}** .
12. Klik op **Opslaan** om uw instellingen op te slaan.
13. Klik in het Hudson-dash board op **nu bouwen** om **MyJob**uit te voeren. Bekijk de console-uitvoer voor status. Status berichten voor Azure Storage worden opgenomen in de console-uitvoer wanneer de actie na het maken van het bouwen van build-artefacten wordt gestart.
14. Wanneer de taak is voltooid, kunt u de constructie artefacten bekijken door de open bare BLOB te openen.
    
    a. Meld u aan bij [Azure-portal](https://portal.azure.com).
    
    b. Klik op **opslag**.
    
    c. Klik op de naam van het opslag account dat u hebt gebruikt voor Hudson.
    
    d. Klik op **containers**.
    
    e. Klik op de container met de naam **MyJob**. Dit is de kleine versie van de naam van de taak die u hebt toegewezen tijdens het maken van de Hudson-taak. Namen van containers en blobs zijn kleine letters (en hoofdletter gevoelig) in Azure Storage. In de lijst met blobs voor de container met de naam **MyJob** moet **Hello. txt** en **date. txt**worden weer geven. Kopieer de URL voor een van deze items en open deze in uw browser. Het tekst bestand dat is geüpload als een build-artefact, wordt weer geven.

Er kan slechts één actie na het bouwen worden gemaakt waarmee artefacten naar Azure Blob-opslag kunnen worden geüpload per taak. Houd er rekening mee dat de enkelvoudige actie na het maken van het uploaden van artefacten naar Azure Blob-opslag verschillende bestanden (met inbegrip van joker tekens) en paden naar bestanden in de **lijst met artefacten kan opgeven om te uploaden** met een punt komma als scheidings teken. Als uw Hudson-Build bijvoorbeeld jar-bestanden en txt-bestanden in de map **Build** van uw werk ruimte produceert en u wilt uploaden naar Azure Blob Storage, gebruikt u het volgende voor de **lijst met artefacten voor het uploaden van** de waarde: **Build/\*. jar; Build/\*. txt**. U kunt ook de syntaxis met een dubbel punt gebruiken om een pad op te geven dat u wilt gebruiken in de naam van de blob. Als u bijvoorbeeld wilt dat de potten worden geüpload met behulp van **binaire bestanden** in het BLOB-pad en de txt-bestanden om te worden geüpload met behulp van **meldingen** in het BLOB-pad, gebruikt u het volgende voor de **lijst met artefacten voor het uploaden van** de waarde: **Build/\*. jar: : binaire bestanden; build\*/. txt:: kennisgevingen**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Een build-stap maken die wordt gedownload vanuit Azure Blob-opslag
De volgende stappen laten zien hoe u een build-stap configureert voor het downloaden van items uit Azure Blob-opslag. Dit is handig als u items wilt opnemen in uw build, bijvoorbeeld potten die u in Azure Blob-opslag wilt bewaren.

1. Klik in de sectie **Build** van de taak configuratie op **Build-stap toevoegen** en kies **downloaden vanuit Azure Blob-opslag**.
2. Selecteer bij **naam van opslag account**het opslag account dat u wilt gebruiken.
3. Geef bij **container naam**de naam op van de container met de blobs die u wilt downloaden. U kunt omgevings variabelen gebruiken.
4. Geef de naam van de BLOB op voor de blobnaam. U kunt omgevings variabelen gebruiken. U kunt ook een asterisk gebruiken als Joker teken nadat u de eerste letter (s) van de naam van de BLOB hebt opgegeven. **Project\\** * geeft bijvoorbeeld alle blobs aan waarvan de naam begint met **project**.
5. Beschrijving Voor het **downloadmap**geeft u het pad op de Hudson-computer op waar u bestanden wilt downloaden van Azure Blob-opslag. Omgevings variabelen kunnen ook worden gebruikt. (Als u geen waarde opgeeft voor het **Download traject**, worden de bestanden van Azure Blob-opslag gedownload naar de werk ruimte van de taak.)

Als er extra items zijn die u wilt downloaden vanuit Azure Blob-opslag, kunt u aanvullende build-stappen maken.

Nadat u een build hebt uitgevoerd, kunt u de uitvoer van de console build-geschiedenis controleren of de download locatie bekijken om te zien of de blobs die u verwacht, zijn gedownload.

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de Blob service
Hieronder vindt u een overzicht van de Blob service-onderdelen.

* **Opslagaccount**: Alle toegang tot Azure Storage wordt uitgevoerd via een opslag account. Dit is het hoogste niveau van de naam ruimte voor toegang tot blobs. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte minder is dan 100 TB.
* **Container**: Een container biedt een groepering van een set blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.
* **BLOB**: Een bestand van elk type en elke grootte. Er zijn twee typen blobs die kunnen worden opgeslagen in Azure Storage: blok-en pagina-blobs. De meeste bestanden zijn blok-blobs. Eén blok-Blob kan Maxi maal 200 GB groot zijn. In deze zelf studie wordt gebruikgemaakt van blok-blobs. Pagina-blobs, een ander BLOB-type, kunnen Maxi maal 1 TB groot zijn, en zijn efficiënter wanneer een bereik van de bytes in een bestand regel matig wordt gewijzigd. Zie voor meer informatie over blobs ' [blok-blobs, toevoeg-blobs en pagina](https://msdn.microsoft.com/library/azure/ee691964.aspx)-blobs '.
* **URL-indeling**: Blobs zijn adresseerbaar met behulp van de volgende URL-indeling:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (De bovenstaande notatie geldt voor de open bare Azure-Cloud. Als u een andere Azure-Cloud gebruikt, gebruikt u het eind punt binnen [Azure Portal](https://portal.azure.com) om het URL-eind punt te bepalen.)
  
    In de bovenstaande `storageaccount` notatie vertegenwoordigt de naam van uw opslag account, `container_name` de naam van uw container en `blob_name` de naam van uw blob. Binnen de naam van de container kunt u meerdere paden hebben, gescheiden door een slash **/** . De voor beeld-container naam in deze zelf studie is **MyJob**, en **\_$ {build-id}/$ {buildnummer\_}** is gebruikt voor het algemene virtuele pad, wat resulteert in de blob met een URL van de volgende vorm:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen
* [Voldoen aan Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Azure Storage SDK voor Java](https://github.com/azure/azure-storage-java)
* [Azure Storage Client SDK-referentie](http://dl.windowsazure.com/storage/javadoc/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](https://blogs.msdn.com/b/windowsazurestorage/)

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).
