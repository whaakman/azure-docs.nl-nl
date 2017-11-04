---
title: Azure Blob Storage-resources beheren met Opslagverkenner (Preview) | Microsoft Docs
description: Azure Blob-Containers en Blobs met Opslagverkenner (Preview) beheren
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 2f09e545-ec94-4d89-b96c-14783cc9d7a9
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: c23b87cca66df0834a31494be7d8657ff9f2a865
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Azure Blob Storage-resources beheren met Opslagverkenner (Preview)
## <a name="overview"></a>Overzicht
[Azure Blob Storage](storage/blobs/storage-dotnet-how-to-use-blobs.md) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk zijn vanuit overal ter wereld via HTTP of HTTPS.
U kunt Blob Storage gebruiken om gegevens openbaar te maken of om toepassingsgegevens privé op te slaan. In dit artikel leert u hoe u Opslagverkenner (Preview) werkt met blob-containers en blobs.

## <a name="prerequisites"></a>Vereisten
U moet het volgende doen om de stappen in dit artikel uit te voeren:

* [Opslagverkenner (preview) downloaden en installeren](http://www.storageexplorer.com)
* [Verbinding maken met een opslagaccount of -service van Azure Storage](vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Een blob-container maken
Alle blobs moeten zich bevinden in een blob-container gewoon een logische groepering van blobs is. Een account kan een onbeperkt aantal containers bevatten en elke container kan een onbeperkt aantal blobs opslaan.

De volgende stappen laten zien hoe u van een blobcontainer in Opslagverkenner (Preview).

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account waarin u wilt maken van de blob-container.
3. Met de rechtermuisknop op **Blob-Containers**, en selecteer in het contextmenu - **Blob-Container maken**.

   ![Contextmenu van blob-containers maken][0]
4. Een tekstvak wordt weergegeven onder de **Blob-Containers** map. Voer de naam voor uw blob-container. Zie de [Container naamgevingsregels](storage/blobs/storage-dotnet-how-to-use-blobs.md#create-a-container) sectie voor een lijst van regels en beperkingen voor de naamgeving van blob-containers.

   ![Tekstvak voor Blob-Containers maken][1]
5. Druk op **Enter** wanneer u klaar bent voor het maken van de blob-container of **Esc** te annuleren. Zodra de blob-container is gemaakt, die wordt weergegeven onder de **Blob-Containers** map voor het geselecteerde opslagaccount.

   ![BLOB-Container gemaakt][2]

## <a name="view-a-blob-containers-contents"></a>Een blob-container inhoud weergeven
BLOB-containers bevatten blobs en -mappen (die ook blobs kunnen bevatten).

De volgende stappen laten zien hoe de inhoud van een blobcontainer in Opslagverkenner (Preview):

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account de blob-container die u wilt weergeven.
3. Vouw het opslagaccount **Blob-Containers**.
4. Met de rechtermuisknop op de blob-container die u wilt weergeven, en selecteer in het contextmenu - **Open Editor voor Blob-Container**.
   U kunt ook dubbelklikken op de blob-container die u wilt weergeven.

   ![Open blob-container editor contextmenu][19]
5. Het hoofdvenster wordt de blob-container inhoud weergegeven.

   ![Editor voor BLOB-container][3]

## <a name="delete-a-blob-container"></a>Verwijderen van een blob-container
BLOB-containers kunnen eenvoudig worden gemaakt en verwijderd indien nodig. (Voor informatie over het verwijderen van afzonderlijke blobs, Zie de sectie [beheren blobs in een blobcontainer](#managing-blobs-in-a-blob-container).)

De volgende stappen uit te laten zien hoe u een blobcontainer in Opslagverkenner (Preview) verwijderen:

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account de blob-container die u wilt weergeven.
3. Vouw het opslagaccount **Blob-Containers**.
4. Met de rechtermuisknop op de blob-container die u wilt verwijderen, en selecteer in het contextmenu - **verwijderen**.
   U kunt ook op drukken **verwijderen** verwijderen van de momenteel geselecteerde blob-container.

   ![Contextmenu van blob-container verwijderen][4]
5. Selecteer **Ja** in het bevestigingsvenster.

   ![Verwijderen van blob-Container bevestigen][5]

## <a name="copy-a-blob-container"></a>Kopiëren van een blob-container
Opslagverkenner (Preview) kunt u een blob-container kopiëren naar het Klembord en plak deze blob-container in een ander opslagaccount. (Voor informatie over het kopiëren van afzonderlijke blobs, Zie de sectie [beheren blobs in een blobcontainer](#managing-blobs-in-a-blob-container).)

De volgende stappen laten zien hoe u een blob-container van één opslagaccount kopieert naar een andere.

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account de blob-container die u wilt kopiëren.
3. Vouw het opslagaccount **Blob-Containers**.
4. Met de rechtermuisknop op de blob-container die u wilt kopiëren, en selecteer in het contextmenu - **kopie Blob-Container**.

   ![Kopiëren van blob-container contextmenu][6]
5. Met de rechtermuisknop op het gewenste 'target' storage-account waarin u wilt de blob-container plakken, en selecteer in het contextmenu - **Blob-Container plakken**.

   ![Menu context blob-container plakken][7]

## <a name="get-the-sas-for-a-blob-container"></a>De SAS ophalen voor een blobcontainer
Een [SAS (Shared Access Signature; handtekening voor gedeelde toegang)](storage/common/storage-dotnet-shared-access-signature-part-1.md) biedt gedelegeerde toegang tot bronnen in uw opslagaccount.
Dit betekent dat u een client gedurende de opgegeven periode een beperkte set machtigingen kunt verlenen voor objecten in uw opslagaccount, zonder dat u hiervoor de toegangssleutels voor het account hoeft te delen.

De volgende stappen laten zien hoe u van een SAS voor een blob-container:

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account met de blob-container die u wilt een SAS ophalen.
3. Vouw het opslagaccount **Blob-Containers**.
4. Met de rechtermuisknop op de gewenste blob-container, en selecteer in het contextmenu - **Shared Access Signature ophalen**.

   ![Contextmenu SAS ophalen][8]
5. Geef in het dialoogvenster **Handtekening voor gedeelde toegang** waarden op voor het beleid, de begin- en eindtijd, de tijdzone en de toegangsniveaus die u wilt gebruiken voor de resource.

   ![Ophalen van SAS-opties][9]
6. Als u alle SAS-opties hebt opgegeven, selecteert u **Maken**.
7. Een tweede **Shared Access Signature** dialoogvenster vervolgens met de blob-container samen met de URL en de QueryStrings voor toegang tot de opslagbronnen kunt u een lijst met weergegeven.
   Selecteer **Kopiëren** naast de URL die u naar het klembord wilt kopiëren.

   ![SAS-URL's kopiëren][10]
8. Als u klaar bent, selecteert u **Sluiten**.

## <a name="manage-access-policies-for-a-blob-container"></a>-Beleid beheren voor een blob-container
De volgende stappen laten zien hoe beheren (toevoegen en verwijderen) toegangsbeleid voor een blob-container:

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account met de blob-container waarvan toegangsbeleid dat u wilt beheren.
3. Vouw het opslagaccount **Blob-Containers**.
4. Selecteer de gewenste blob-container, en selecteer in het contextmenu - **toegangsbeleid beheren**.

   ![Contextmenu Toegangsbeleid beheren][11]
5. De **toegangsbeleid** dialoogvenster vermeldt alle beleidsregels voor toegang al is gemaakt voor de geselecteerde blob-container.

   ![Opties voor toegangsbeleid][12]        
6. Volg hieronder de stappen voor de beleidsbeheertaak die u wilt uitvoeren:

   * **Een nieuw toegangsbeleid toevoegen**: selecteer **Toevoegen**. Als de bewerking is voltooid, ziet u het dialoogvenster **Toegangsbeleid** met het zojuist toegevoegde toegangsbeleid (met de standaardinstellingen).
   * **Een toegangsbeleid bewerken** - eventueel wijzigingen doorvoeren en selecteer **opslaan**.
   * **Een toegangsbeleid verwijderen**: selecteer **Verwijderen** naast het toegangsbeleid dat u wilt verwijderen.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Het niveau van de openbare toegang instellen voor een blob-container
Elke blob-container is standaard ingesteld op 'Geen openbare toegang'.

De volgende stappen laten zien hoe het niveau van een openbare toegang voor een blob-container opgeven.

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account met de blob-container waarvan toegangsbeleid dat u wilt beheren.
3. Vouw het opslagaccount **Blob-Containers**.
4. Selecteer de gewenste blob-container, en selecteer in het contextmenu - **ingesteld openbare toegangsniveau**.

   ![Openbare toegang niveau contextmenu instellen][13]
5. In de **ingesteld Container openbaar toegangsniveau** dialoogvenster u de gewenste toegangsniveau kunt opgeven.

   ![Opties voor openbare toegang niveau instellen][14]
6. Selecteer **Toepassen**.

## <a name="managing-blobs-in-a-blob-container"></a>Blobs in een blobcontainer beheren
Als u een blob-container hebt gemaakt, kunt u een blob uploadt naar deze blob-container, een blob gedownload naar uw lokale computer, opent u een blob op uw lokale computer en nog veel meer.

De volgende stappen uit te laten zien hoe de BLOB's (en mappen) beheren binnen een blob-container.

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account de blob-container die u wilt beheren.
3. Vouw het opslagaccount **Blob-Containers**.
4. Dubbelklik op de blob-container die u wilt weergeven.
5. Het hoofdvenster wordt de blob-container inhoud weergegeven.

   ![Weergave blob-container][3]
6. Het hoofdvenster wordt de blob-container inhoud weergegeven.
7. Volg hieronder de stappen voor de bewerking die u wilt uitvoeren:

   * **Bestanden uploaden naar een blob-container**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Bestanden uploaden** in de vervolgkeuzelijst.

        ![Menu bestanden uploaden][15]
     2. Selecteer in het dialoogvenster **Bestanden uploaden** de knop met de weglatingstekens (**...** ) rechts van het tekstvak **Bestanden** om de bestanden te selecteren die u wilt uploaden.

        ![Opties voor bestanden uploaden][16]
     3. Geef het type **Blob-type**. Het artikel [aan de slag met Azure Blob storage met .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) worden de verschillen tussen de verschillende typen van de blob.
     4. Geef desgewenst een pad van de map waarin de geselecteerde bestanden worden geüpload. Als de doelmap niet bestaat, wordt deze gemaakt.
     5. Selecteer **Uploaden**.
   * **Uploaden van een map naar een blob-container**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Map uploaden** in de vervolgkeuzelijst.

        ![Menu voor uploaden van map][17]
     2. Selecteer in het dialoogvenster **Map uploaden** de knop met de weglatingstekens (**...** ) rechts van het tekstvak **Map** om de map te selecteren waarvan u de inhoud wilt uploaden.

        ![Mapopties uploaden][18]
     3. Geef het type **Blob-type**. Het artikel [aan de slag met Azure Blob storage met .NET](storage/blobs/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) worden de verschillen tussen de verschillende typen van de blob.
     4. Geef desgewenst een doelmap op waarin de inhoud van de geselecteerde map moet worden geüpload. Als de doelmap niet bestaat, wordt deze gemaakt.
     5. Selecteer **Uploaden**.
   * **Een blob naar de lokale computer downloaden**

     1. Selecteer de blob die u wilt downloaden.
     2. Selecteer **Downloaden** op de werkbalk van het hoofdvenster.
     3. In de **opgeven waar de gedownloade blob opslaan** dialoogvenster, geef de locatie waar u de blob gedownload en de naam die u wilt geven.  
     4. Selecteer **Opslaan**.
   * **Open een blob op uw lokale computer**

     1. Selecteer de blob die u wilt openen.
     2. Selecteer **Openen** op de werkbalk van het hoofdvenster.
     3. De blob wordt gedownload en geopend met behulp van de toepassing die is gekoppeld aan de blob onderliggende bestandstype.
   * **Een blob naar het Klembord kopiëren**

     1. Selecteer de blob die u wilt kopiëren.
     2. Selecteer **Kopiëren** op de werkbalk van het hoofdvenster.
     3. Klik in het linkerdeelvenster, gaat u naar een andere blob-container en dubbelklik erop om het te bekijken in het hoofdvenster.
     4. Selecteer op de werkbalk van het hoofdvenster **plakken** om een kopie van de blob te maken.
   * **Verwijderen van een blob**

     1. Selecteer de blob die u wilt verwijderen.
     2. Selecteer **Verwijderen** op de werkbalk van het hoofdvenster.
     3. Selecteer **Ja** in het bevestigingsvenster.

## <a name="next-steps"></a>Volgende stappen
* De [meest recente releaseopmerkingen en video's van Opslagverkenner (preview)](http://www.storageexplorer.com) bekijken.
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
