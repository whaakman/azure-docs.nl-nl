---
title: Voor beelden Azure Storage met .NET | Microsoft Docs
description: Voorbeeld code en toepassingen voor Azure Storage weer geven, downloaden en uitvoeren. Ontdek aan de slag met voor beelden voor blobs, wacht rijen, tabellen en bestanden, met behulp van de .NET Storage-client bibliotheken.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/03/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: cec6b0498adf55428fade7ae00db6550496a20bd
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721857"
---
# <a name="azure-storage-samples-using-net"></a>Voor beelden Azure Storage met .NET

## <a name="net-sample-index"></a>.NET-voorbeeld index

De volgende tabel bevat een overzicht van onze voor beelden van opslag plaatsen en de scenario's die in elk voor beeld worden behandeld. Klik op de koppelingen om de bijbehorende voorbeeld code in GitHub weer te geven.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Blob toevoegen</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs#L1144">Aan de slag met blobs</a></td> 
</tr> 
<tr> 
<td>Blok-blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Webtoepassing voor Azure Blob Storage Photo Gallery</a></td>
</tr> 
<tr> 
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Voor beelden van BLOB-versleuteling</a></td>
</tr> 
<tr> 
<td>Blob kopiëren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>Container maken</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Webtoepassing voor Azure Blob Storage Photo Gallery</a></td>
</tr> 
<tr> 
<td>Blob verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Webtoepassing voor Azure Blob Storage Photo Gallery</a></td>
</tr> 
<tr> 
<td>Container verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>Meta gegevens/eigenschappen/statistieken voor blobs</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>Container-ACL/meta gegevens/eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Webtoepassing voor Azure Blob Storage Photo Gallery</a></td>
</tr> 
<tr> 
<td>Paginabereiken ophalen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>BLOB/container lease</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>BLOB/container weer geven</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Aan de slag met blobs</a></td>
</tr> 
<tr> 
<td>Pagina-blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Aan de slag met blobs</a></td>
</tr>
<tr> 
<td>GEBASEERD</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr>   
<tr> 
<td>Service-eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met blobs</a></td>
</tr>           
<tr> 
<td>Moment opname-BLOB</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Back-ups maken van Azure virtual machine-schijven met incrementele moment opnamen</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>File</b></td>
<td>Shares/Directory's/bestanden maken</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr>
<tr> 
<td>Shares/mappen/bestanden verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Aan de slag met Azure File Service in .NET</a></td> 
</tr> 
<tr> 
<td>Mapeigenschappen/meta gegevens</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr> 
<tr> 
<td>Bestanden downloaden</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr> 
<tr> 
<td>Bestands eigenschappen/meta gegevens/statistieken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr> 
<tr> 
<td>Eigenschappen van bestands service</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr> 
<tr> 
<td>Mappen en bestanden weer geven</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr>
<tr> 
<td>Shares weer geven</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr>
<tr> 
<td>Eigenschappen/meta gegevens/statistieken delen</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage-voor beeld</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Wachtrij</b></td>
<td>Bericht toevoegen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Clientversleuteling</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Versleuteling van de .NET-wachtrij aan client zijde Azure Storage</a></td> 
</tr> 
<tr> 
<td>Wacht rijen maken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht/wachtrij verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht bekijken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Wachtrij-ACL/meta gegevens/statistieken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Eigenschappen van Queue-service</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht bijwerken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue service in .NET</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid beheren met Azure Storage-voorbeeld toepassing</a></td> 
</tr> 
<tr> 
<td>Entiteit/tabel verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Entiteit invoegen/samen voegen/vervangen</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid beheren met Azure Storage-voorbeeld toepassing</a></td> 
</tr> 
<tr> 
<td>Entiteiten opvragen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Query tabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Tabel-ACL/eigenschappen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Entiteit bijwerken</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Gelijktijdigheid beheren met Azure Storage-voorbeeld toepassing</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Bibliotheek voor Azure-code voorbeelden

Als u de volledige voorbeeld bibliotheek wilt weer geven, gaat u naar de [Azure code samples](https://azure.microsoft.com/resources/samples/?service=storage) -bibliotheek, die voor beelden bevat voor Azure Storage die u lokaal kunt downloaden en uitvoeren. De voorbeeld bibliotheek code bevat voorbeeld code in. zip-indeling. U kunt ook bladeren en de GitHub-opslag plaats voor elk voor beeld klonen.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Aan de slag-hand leidingen

Bekijk de volgende hand leidingen als u op zoek bent naar instructies voor het installeren en aan de slag met de Azure Storage-client bibliotheken.

* [Aan de slag met Azure Blob service in .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Aan de slag met Azure Queue service in .NET](../storage-dotnet-how-to-use-queues.md)
* [Aan de slag met Azure Table service in .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Aan de slag met Azure File Service in .NET](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over voor beelden voor andere talen:

* Java: [Azure Storage-voorbeelden met behulp van Java](storage-samples-java.md)
* Alle andere talen: [Azure Storage-voor beelden](../storage-samples.md)
