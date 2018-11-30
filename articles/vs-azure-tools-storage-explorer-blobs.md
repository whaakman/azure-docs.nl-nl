---
title: Azure Blob Storage-resources beheren met Storage Explorer | Microsoft Docs
description: Beheren van Azure Blob-Containers en Blobs met Storage Explorer
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: fd13cb25fbe3696bf67615ef956f478b7c355547
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422049"
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer"></a>Azure Blob Storage-resources beheren met Opslagverkenner
## <a name="overview"></a>Overzicht
[Azure Blob-opslag](storage/blobs/storage-dotnet-how-to-use-blobs.md) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS.
U kunt Blob Storage gebruiken om gegevens openbaar te maken of om toepassingsgegevens privé op te slaan. In dit artikel leert u hoe u Storage Explorer gebruiken om te werken met blob-containers en blobs.

## <a name="prerequisites"></a>Vereisten
U moet het volgende doen om de stappen in dit artikel uit te voeren:

* [Storage Explorer downloaden en installeren](https://www.storageexplorer.com)
* [Verbinding maken met een opslagaccount of -service van Azure Storage](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Een blob-container maken
Alle blobs moeten zich bevinden in een blobcontainer, dit gewoon een logische groepering van blobs is. Een account kan een onbeperkt aantal containers bevatten en elke container kan een onbeperkt aantal blobs.

De volgende stappen laten zien hoe u een blob-container binnen Storage Explorer maken.

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount waarin u wilt maken van de blob-container.
3. Met de rechtermuisknop op **Blobcontainers**, en selecteer in het contextmenu - **Blob-Container maken**.

   ![Contextmenu voor blob-containers maken][0]
4. Een tekstvak wordt weergegeven onder de **Blobcontainers** map. Voer een naam in voor de blobcontainer. Zie de [de container maken en machtigingen instellen](storage/blobs/storage-quickstart-blobs-dotnet.md#create-the-container-and-set-permissions) voor meer informatie over regels en beperkingen voor namen van blobcontainers.

   ![Tekstvak voor Blob-Containers maken][1]
5. Druk op **Enter** wanneer u klaar bent voor het maken van de blob-container of **Esc** om te annuleren. Als de blob-container is gemaakt, worden deze weergegeven onder de **Blobcontainers** map voor het geselecteerde opslagaccount.

   ![BLOB-Container gemaakt][2]

## <a name="view-a-blob-containers-contents"></a>Inhoud van een blob-container weergeven
BLOB-containers bevatten blobs en -mappen (die ook blobs kunnen bevatten).

De volgende stappen laten zien hoe u de inhoud van een blob-container binnen Storage Explorer weergeven:

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount dat de blob-container die u wilt weergeven.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Met de rechtermuisknop op de blob-container die u wilt weergeven, en selecteer in het contextmenu - **Open Editor voor Blob-Container**.
   U kunt ook dubbelklikken op de blob-container die u wilt weergeven.

   ![Contextmenu van Open blob-container-editor][19]
5. In het hoofdvenster wordt de inhoud van de blobcontainer weergegeven.

   ![BLOB-container-editor][3]

## <a name="delete-a-blob-container"></a>Een blob-container verwijderen
BLOB-containers kunnen eenvoudig worden gemaakt en verwijderd, omdat die nodig zijn. (Voor informatie over het verwijderen van afzonderlijke blobs, Raadpleeg het gedeelte [beheren van blobs in een blobcontainer](#managing-blobs-in-a-blob-container).)

De volgende stappen laten zien hoe u een blob-container binnen Storage Explorer:

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount dat de blob-container die u wilt weergeven.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Met de rechtermuisknop op de blob-container die u wilt verwijderen, en selecteer in het contextmenu - **verwijderen**.
   U kunt ook op drukken **verwijderen** verwijderen van de geselecteerde blob-container.

   ![Contextmenu voor blob-container verwijderen][4]
5. Selecteer **Ja** in het bevestigingsvenster.

   ![Blob-Container bevestiging verwijderen][5]

## <a name="copy-a-blob-container"></a>Kopiëren van een blob-container
Storage Explorer kunt u een blob-container naar het Klembord kopieert en plakt u deze blob-container in een ander opslagaccount. (Voor informatie over het kopiëren van afzonderlijke blobs, Raadpleeg het gedeelte [beheren van blobs in een blobcontainer](#managing-blobs-in-a-blob-container).)

De volgende stappen laten zien hoe u een blob-container van één opslagaccount kopieert naar een andere.

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount dat de blob-container die u wilt kopiëren.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Met de rechtermuisknop op de blob-container die u wilt kopiëren, en selecteer in het contextmenu - **kopie Blob-Container**.

   ![Kopiëren van blob-container-contextmenu][6]
5. Met de rechtermuisknop op het gewenste 'target' storage-account waarnaar u wilt de blob-container plakken, en selecteer in het contextmenu - **Blobcontainer plakken**.

   ![Contextmenu voor plakken blob-container][7]

## <a name="get-the-sas-for-a-blob-container"></a>De SAS ophalen voor een blobcontainer
Een [SAS (Shared Access Signature; handtekening voor gedeelde toegang)](storage/common/storage-dotnet-shared-access-signature-part-1.md) biedt gedelegeerde toegang tot bronnen in uw opslagaccount.
Dit betekent dat u een client gedurende de opgegeven periode een beperkte set machtigingen kunt verlenen voor objecten in uw opslagaccount, zonder dat u hiervoor de toegangssleutels voor het account hoeft te delen.

De volgende stappen laten zien hoe u een SAS maken voor een blob-container:

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount met de blob-container die u wilt ophalen van een SAS.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Met de rechtermuisknop op de gewenste blob-container, en selecteer in het contextmenu - **Shared Access Signature ophalen**.

   ![SAS-contextmenu ophalen][8]
5. Geef in het dialoogvenster **Handtekening voor gedeelde toegang** waarden op voor het beleid, de begin- en eindtijd, de tijdzone en de toegangsniveaus die u wilt gebruiken voor de resource.

   ![SAS-opties ophalen][9]
6. Als u alle SAS-opties hebt opgegeven, selecteert u **Maken**.
7. Een tweede **Shared Access Signature** dialoogvenster wordt vervolgens weergegeven met een lijst met de blob-container en de URL en queryreeksen waarmee u gebruiken kunt voor toegang tot de storage-resource.
   Selecteer **Kopiëren** naast de URL die u naar het klembord wilt kopiëren.

   ![SAS-URL's kopiëren][10]
8. Als u klaar bent, selecteert u **Sluiten**.

## <a name="manage-access-policies-for-a-blob-container"></a>Toegangsbeleid beheren voor een blob-container
De volgende stappen laten zien hoe u voor het beheren van (toevoegen en verwijderen) toegangsbeleid voor een blob-container:

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount met de blob-container waarvan toegangsbeleid dat u wilt beheren.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Selecteer de gewenste blob-container, en selecteer in het contextmenu - **toegangsbeleid beheren**.

   ![Contextmenu Toegangsbeleid beheren][11]
5. De **toegangsbeleid** dialoogvenster wordt alle toegangsregels die al zijn gemaakt voor de geselecteerde blob-container.

   ![Opties voor toegangsbeleid][12]        
6. Volg hieronder de stappen voor de beleidsbeheertaak die u wilt uitvoeren:

   * **Een nieuw toegangsbeleid toevoegen**: selecteer **Toevoegen**. Als de bewerking is voltooid, ziet u het dialoogvenster **Toegangsbeleid** met het zojuist toegevoegde toegangsbeleid (met de standaardinstellingen).
   * **Een toegangsbeleid bewerken** : Breng de gewenste wijzigingen aan en selecteer **opslaan**.
   * **Een toegangsbeleid verwijderen**: selecteer **Verwijderen** naast het toegangsbeleid dat u wilt verwijderen.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Het niveau openbare toegang instellen voor een blob-container
Elke blob-container is standaard ingesteld op 'Geen openbare toegang'.

De volgende stappen laten zien hoe u een openbaar toegangsniveau voor een blob-container opgeven.

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount met de blob-container waarvan toegangsbeleid dat u wilt beheren.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Selecteer de gewenste blob-container, en selecteer in het contextmenu - **niveau openbare toegang instelt**.

   ![Niveau openbare toegang-contextmenu instellen][13]
5. In de **ingesteld Container openbaar toegangsniveau** dialoogvenster, geef de gewenste toegangsniveau.

   ![Opties voor openbare toegang tot het niveau instellen][14]
6. Selecteer **Toepassen**.

## <a name="managing-blobs-in-a-blob-container"></a>Blobs in een blobcontainer beheren
Als u een blob-container hebt gemaakt, kunt u een blob geüpload naar deze blob-container, een blob te downloaden naar uw lokale computer, opent u een blob op uw lokale computer, en nog veel meer.

De volgende stappen ziet u hoe u voor het beheren van de blobs (en mappen) binnen een blob-container.

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount dat de blob-container die u wilt beheren.
3. Vouw het opslagaccount uit **Blobcontainers**.
4. Dubbelklik op de blob-container die u wilt weergeven.
5. In het hoofdvenster wordt de inhoud van de blobcontainer weergegeven.

   ![Blob-container weergeven][3]
6. In het hoofdvenster wordt de inhoud van de blobcontainer weergegeven.
7. Volg hieronder de stappen voor de bewerking die u wilt uitvoeren:

   * **Bestanden uploaden naar een blob-container**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Bestanden uploaden** in de vervolgkeuzelijst.

        ![Menu voor Bestanden uploaden][15]
     2. Selecteer in het dialoogvenster **Bestanden uploaden** de knop met de weglatingstekens (**...** ) rechts van het tekstvak **Bestanden** om de bestanden te selecteren die u wilt uploaden.

        ![Opties voor bestanden uploaden][16]
     3. Geef het type **Blobtype**. Zie [de container maken en machtigingen instellen](storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-the-container) voor meer informatie.
     4. Geef desgewenst een doelmap op waarin de geselecteerde bestanden worden geüpload. Als de doelmap niet bestaat, wordt deze gemaakt.
     5. Selecteer **Uploaden**.
   * **Een bestand uploaden naar een blob-container**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Map uploaden** in de vervolgkeuzelijst.

        ![Menu voor uploaden van map][17]
     2. Selecteer in het dialoogvenster **Map uploaden** de knop met de weglatingstekens (**...** ) rechts van het tekstvak **Map** om de map te selecteren waarvan u de inhoud wilt uploaden.

        ![Opties voor map uploaden][18]
     3. Geef het type **Blobtype**. Zie [de container maken en machtigingen instellen](storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-the-container) voor meer informatie.
     4. Geef desgewenst een doelmap op waarin de inhoud van de geselecteerde map moet worden geüpload. Als de doelmap niet bestaat, wordt deze gemaakt.
     5. Selecteer **Uploaden**.
   * **Een blob te downloaden naar uw lokale computer**

     1. Selecteer de blob die u wilt downloaden.
     2. Selecteer **Downloaden** op de werkbalk van het hoofdvenster.
     3. In de **Geef op waar om op te slaan van de gedownloade blob** dialoogvenster, geef de locatie waar u de blob gedownload en de naam die u wilt geven.  
     4. Selecteer **Opslaan**.
   * **Een blob op uw lokale computer openen**

     1. Selecteer de blob die u wilt openen.
     2. Selecteer **Openen** op de werkbalk van het hoofdvenster.
     3. De blob wordt gedownload en geopend met de toepassing die is gekoppeld aan het onderliggende bestandstype van de blob.
   * **Een blob naar het Klembord kopiëren**

     1. Selecteer de blob die u wilt kopiëren.
     2. Selecteer **Kopiëren** op de werkbalk van het hoofdvenster.
     3. In het linkerdeelvenster, gaat u naar een andere blob-container en dubbelklik erop om deze te bekijken in het hoofdvenster.
     4. Selecteer op de werkbalk van het hoofdvenster **plakken** om een kopie van de blob te maken.
   * **Een blob verwijderen**

     1. Selecteer de blob die u wilt verwijderen.
     2. Selecteer **Verwijderen** op de werkbalk van het hoofdvenster.
     3. Selecteer **Ja** in het bevestigingsvenster.

## <a name="next-steps"></a>Volgende stappen
* De [meest recente releaseopmerkingen en video's van Storage Explorer](https://www.storageexplorer.com) bekijken.
* Meer informatie lezen over [het maken van toepassingen met behulp van blobs, tabellen, wachtrijen en bestanden van Azure](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png
