---
title: Voorbeelden van Azure Storage met .NET | Microsoft Docs
description: Weergeven, downloaden en uitvoeren van toepassingen voor Azure Storage en voorbeeldcode. Ontdek aan de slag-voorbeelden voor blobs, wachtrijen, tabellen en bestanden, met behulp van de opslagclientbibliotheken voor .NET.
services: storage
author: seguler
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2017
ms.author: seguler
ms.component: common
ms.openlocfilehash: 663df77a7fb574f05cfaa9378dff53ca5db21c49
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526106"
---
# <a name="azure-storage-samples-using-net"></a>Voorbeelden van Azure Storage met .NET

## <a name="net-sample-index"></a>Index van de .NET-voorbeeld

De volgende tabel bevat een overzicht van onze opslagplaats met voorbeelden en de scenario's in elk voorbeeld behandeld. Klik op de koppelingen om weer te geven van de bijbehorende voorbeeldcode in GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Toevoeg-Blob</td> 
<td><a href="https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">Voorbeeld van de methode CloudBlobContainer.GetAppendBlobReference</a></td> 
</tr> 
<tr> 
<td>Blok-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web-App</a></td>
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
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web-App</a></td>
</tr> 
<tr> 
<td>Blob verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web-App</a></td>
</tr> 
<tr> 
<td>Container verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>BLOB-metagegevens/eigenschappen/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>ACL/metagegevens/eigenschappen van de container</td>
<td><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web-App</a></td>
</tr> 
<tr> 
<td>Get Page-bereiken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Lease-Blob/Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Blob/Container vermelden</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Aan de slag met Blobs</a></td>
</tr> 
<tr> 
<td>Pagina-blobs</td>
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
<td>Momentopname maken van Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Back-up Azure VM-schijven met incrementele momentopnamen</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>File</b></td>
<td>Shares/mappen/bestanden maken</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr>
<tr> 
<td>Shares/mappen/bestanden verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Aan de slag met Azure File-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Directory eigenschappen/metagegevens</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr> 
<tr> 
<td>Bestanden downloaden</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr> 
<tr> 
<td>Bestand eigenschappen/metagegevens/metrische gegevens</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr> 
<tr> 
<td>File Service Properties</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr> 
<tr> 
<td>Lijst met mappen en bestanden</td> 
<td><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr>
<tr> 
<td>Lijst met Shares</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr>
<tr> 
<td>Delen van eigenschappen/metagegevens/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET-voorbeeldbestand voor opslag</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>wachtrij</b></td>
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
<td>Bericht/wachtrij verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht bekijken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Wachtrij ACL/metagegevens/Stats</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Eigenschappen van de Queue-Service</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td>Bericht bijwerken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Aan de slag met Azure Queue-Service in .NET</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Met behulp van Azure Storage - voorbeeldtoepassing gelijktijdigheid beheren</a></td> 
</tr> 
<tr> 
<td>Entiteit/tabel verwijderen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>De entiteit invoegen/Merge/vervangen</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Met behulp van Azure Storage - voorbeeldtoepassing gelijktijdigheid beheren</a></td> 
</tr> 
<tr> 
<td>Entiteiten opvragen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Querytabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Tabeleigenschappen ACL</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Aan de slag met Azure Table Storage in .NET</a></td> 
</tr> 
<tr> 
<td>Een entiteit bijgewerkt</td> 
<td><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Met behulp van Azure Storage - voorbeeldtoepassing gelijktijdigheid beheren</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure-codevoorbeelden-bibliotheek

Als u het volledige voorbeeld van een bibliotheek, gaat u naar de [Azure-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=storage) bibliotheek, waaronder voorbeelden voor Azure-opslag die u kunt downloaden en lokaal uitvoeren. De bibliotheek met voorbeeld wordt een voorbeeldcode in .zip-indeling. U kunt ook bladeren en kloon de GitHub-opslagplaats voor elk voorbeeld.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Introductiehandleidingen

Bekijk de volgende handleidingen in als u op zoek bent voor instructies over het installeren en aan de slag met de Azure Storage-clientbibliotheken.

* [Aan de slag met Azure Blob-Service in .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Aan de slag met Azure Queue-Service in .NET](../storage-dotnet-how-to-use-queues.md)
* [Aan de slag met Azure Table-Service in .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Aan de slag met Azure File-Service in .NET](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over voorbeelden voor andere talen:

* Java: [voorbeelden van Azure Storage met behulp van Java](storage-samples-java.md)
* Alle andere talen: [Azure Storage-voorbeelden](../storage-samples.md)
