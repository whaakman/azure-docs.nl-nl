---
title: Voorbeelden van Azure Storage met .NET | Microsoft Docs
description: Weergeven, downloaden en uitvoeren van toepassingen voor Azure Storage en voorbeeldcode. Ontdek aan de slag voorbeelden voor blobs, wachtrijen, tabellen en bestanden, met behulp van de opslagclientbibliotheken voor .NET.
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: 74777ed14ebb41ad31657f814e86724ff1e5e62e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-samples-using-net"></a>Voorbeelden van Azure Storage met .NET

## <a name="net-sample-index"></a>Index van .NET-voorbeeld

De volgende tabel bevat een overzicht van onze voorbeelden-opslagplaats en de scenario's worden behandeld in elk voorbeeld. Klik op de koppelingen naar de bijbehorende voorbeeldcode weergeven in GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>BLOB</b></td>
<td>Toevoeg-Blob</td> 
<td><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Voorbeeld van de methode CloudBlobContainer.GetAppendBlobReference</a></td> 
</tr> 
<tr> 
<td>Blok-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie-webtoepassing</a></td>
</tr> 
<tr> 
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Voorbeelden van BLOB-versleuteling</a></td>
</tr> 
<tr> 
<td>Blob kopiëren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Container maken</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie-webtoepassing</a></td>
</tr> 
<tr> 
<td>Verwijderen van Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie-webtoepassing</a></td>
</tr> 
<tr> 
<td>Verwijderen van Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>BLOB eigenschappen-metagegevens/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>ACL, metagegevens/eigenschappen van container</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Fotogalerie-webtoepassing</a></td>
</tr> 
<tr> 
<td>Get-paginabereiken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>De lease van Blob-Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Lijst Blobcontainer</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Pagina-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Aan de slag met Blobs</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr>   
<tr> 
<td>Service-eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr>           
<tr> 
<td>Momentopname Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Back-virtuele Machine van Azure-schijven met incrementele momentopnamen</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>File</b></td>
<td>Mappen-Shares-bestanden maken</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr>
<tr> 
<td>Mappen-Shares-bestanden verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Aan de slag met Azure File-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Directory eigenschappen/Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr> 
<tr> 
<td>Bestanden downloaden</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr> 
<tr> 
<td>Bestand eigenschappen/metagegevens/metrische gegevens</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr> 
<tr> 
<td>File Service Properties</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr> 
<tr> 
<td>Lijst met mappen en bestanden</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr>
<tr> 
<td>Lijst met Shares</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr>
<tr> 
<td>Delen van metagegevens-eigenschappen-statistieken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand opslag</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Wachtrij</b></td>
<td>Bericht toevoegen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Clientversleuteling</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET wachtrij Client-Side-versleuteling</a></td> 
</tr> 
<tr> 
<td>Wachtrijen maken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Berichtenwachtrij/verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Wachtrij metagegevens-ACL/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Eigenschappen van de Queue-Service</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Updatebericht</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid van taken met behulp van Azure Storage - voorbeeldtoepassing beheren</a></td> 
</tr> 
<tr> 
<td>Entiteit/tabel verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>De entiteit invoegen of Merge/verplaatsen</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid van taken met behulp van Azure Storage - voorbeeldtoepassing beheren</a></td> 
</tr> 
<tr> 
<td>Query-entiteiten</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Querytabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>ACL/eigenschappen van de tabel</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Bijwerken van entiteit</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid van taken met behulp van Azure Storage - voorbeeldtoepassing beheren</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure-codevoorbeelden-bibliotheek

Als u wilt de compleet codevoorbeeld bibliotheek bekijken, gaat u naar de [Azure codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=storage) bibliotheek, waaronder voorbeelden voor Azure-opslag die u kunt downloaden en lokaal uitvoeren. De bibliotheek met voorbeeld wordt een voorbeeldcode in het ZIP-indeling. U kunt ook bladeren en kloon de GitHub-opslagplaats voor elk voorbeeld.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Ophalen van slag-handleidingen

Bekijk de volgende handleidingen in als u op zoek bent voor instructies over het installeren en aan de slag met de clientbibliotheken van Azure Storage.

* [Aan de slag met Azure Blob-Service in .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Aan de slag met Azure Queue-Service in .NET](../storage-dotnet-how-to-use-queues.md)
* [Aan de slag met Azure Table-Service in .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Aan de slag met Azure File-Service in .NET](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over steekproeven voor de andere talen:

* Java: [voorbeelden van Azure Storage met Java](storage-samples-java.md)
* Alle andere talen: [Azure Storage-voorbeelden](../storage-samples.md)
