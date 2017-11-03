---
title: Azure Storage-voorbeelden met Java | Microsoft Docs
description: Weergeven, downloaden en uitvoeren van toepassingen voor Azure Storage en voorbeeldcode. Ontdek aan de slag voorbeelden voor blobs, wachtrijen, tabellen en bestanden, met behulp van de clientbibliotheken van Java-opslag.
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-samples-using-java"></a>Azure Storage-voorbeelden u Java gebruikt

## <a name="java-sample-index"></a>Index van Java-voorbeeld

De volgende tabel bevat een overzicht van onze voorbeelden-opslagplaats en de scenario's worden behandeld in elk voorbeeld. Klik op de koppelingen naar de bijbehorende voorbeeldcode weergeven in GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>BLOB</b></td>
<td>Toevoeg-Blob</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td> 
</tr> 
<tr> 
<td>Blok-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Aan de slag met Azure versleuteling voor aan de clientzijde in Java</a></td>
</tr> 
<tr> 
<td>Blob kopiëren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Container maken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Verwijderen van Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Verwijderen van Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>BLOB eigenschappen-metagegevens/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>ACL, metagegevens/eigenschappen van container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Get-paginabereiken</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Pagina-Blob test voorbeeld</a></td>
</tr> 
<tr> 
<td>De lease van Blob-Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Lijst Blobcontainer</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td>Pagina-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Voorbeeld van SAS-Tests</a></td>
</tr>   
<tr> 
<td>Service-eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>           
<tr> 
<td>Momentopname Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>File</b></td>
<td>Mappen-Shares-bestanden maken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr>
<tr> 
<td>Mappen-Shares-bestanden verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr> 
<tr> 
<td>Directory eigenschappen/Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr> 
<tr> 
<td>Bestanden downloaden</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr> 
<tr> 
<td>Bestand eigenschappen/metagegevens/metrische gegevens</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr> 
<tr> 
<td>File Service Properties</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr> 
<tr> 
<td>Lijst met mappen en bestanden</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr>
<tr> 
<td>Lijst met Shares</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr>
<tr> 
<td>Delen van metagegevens-eigenschappen-statistieken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Wachtrij</b></td>
<td>Bericht toevoegen</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Opslag Java Client Library-voorbeelden</a></td> 
</tr> 
<tr> 
<td>Clientversleuteling</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Opslag Java Client Library-voorbeelden</a></td> 
</tr> 
<tr> 
<td>Wachtrijen maken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td>Berichtenwachtrij/verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td>Bericht</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td>Wachtrij metagegevens-ACL/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td>Eigenschappen van de Queue-Service</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td>Updatebericht</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Aan de slag met Azure Table-Service in Java</a></td> 
</tr> 
<tr> 
<td>Entiteit/tabel verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Aan de slag met Azure Table-Service in Java</a></td> 
</tr> 
<tr> 
<td>De entiteit invoegen of Merge/verplaatsen</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Opslag Java Client Library-voorbeelden</a></td> 
</tr> 
<tr> 
<td>Query-entiteiten</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Aan de slag met Azure Table-Service in Java</a></td> 
</tr> 
<tr> 
<td>Querytabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Aan de slag met Azure Table-Service in Java</a></td> 
</tr> 
<tr> 
<td>ACL/eigenschappen van de tabel</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Aan de slag met Azure Table-Service in Java</a></td> 
</tr> 
<tr> 
<td>Bijwerken van entiteit</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Opslag Java Client Library-voorbeelden</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure-codevoorbeelden-bibliotheek

Als u wilt de compleet codevoorbeeld bibliotheek bekijken, gaat u naar de [Azure codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=storage) bibliotheek, waaronder voorbeelden voor Azure-opslag die u kunt downloaden en lokaal uitvoeren. De bibliotheek met voorbeeld wordt een voorbeeldcode in het ZIP-indeling. U kunt ook bladeren en kloon de GitHub-opslagplaats voor elk voorbeeld.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Ophalen van slag-handleidingen

Bekijk de volgende handleidingen in als u op zoek bent voor instructies over het installeren en aan de slag met de clientbibliotheken van Azure Storage.

* [Aan de slag met Azure Blob-Service in Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Aan de slag met Azure Queue-Service in Java](../storage-java-how-to-use-queue-storage.md)
* [Aan de slag met Azure Table-Service in Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Aan de slag met Azure File-Service in Java](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over steekproeven voor de andere talen:

* .NET: [voorbeelden van azure Storage met .NET](../storage-samples-dotnet.md)
* Alle andere talen: [Azure Storage-voorbeelden](../storage-samples.md)
