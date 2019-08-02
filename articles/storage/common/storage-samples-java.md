---
title: Voor beelden Azure Storage met Java | Microsoft Docs
description: Voorbeeld code en toepassingen voor Azure Storage weer geven, downloaden en uitvoeren. Ontdek aan de slag met voor beelden voor blobs, wacht rijen, tabellen en bestanden, met behulp van de client bibliotheken voor Java-opslag.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/03/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 4348400e66e4b975d1eeb899cab8bb5024260149
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721842"
---
# <a name="azure-storage-samples-using-java"></a>Azure Storage voor beelden met behulp van Java

## <a name="java-sample-index"></a>Java-voorbeeld index

De volgende tabel bevat een overzicht van onze voor beelden van opslag plaatsen en de scenario's die in elk voor beeld worden behandeld. Klik op de koppelingen om de bijbehorende voorbeeld code in GitHub weer te geven.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody>
<tr>
<td rowspan="16"><b>Blob</b></td>
<td>Blob toevoegen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Blok-blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Aan de slag met de Azure-versleuteling aan de client zijde in Java</a></td>
</tr>
<tr>
<td>Blob kopiëren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Container maken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Blob verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Container verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Meta gegevens/eigenschappen/statistieken voor blobs</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Container-ACL/meta gegevens/eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Paginabereiken ophalen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>BLOB/container lease</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>BLOB/container weer geven</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Pagina-blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>GEBASEERD</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">Voor beeld van SAS-tests</a></td>
</tr>   
<tr>
<td>Service-eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td>Moment opname-BLOB</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob service in Java</a></td>
</tr>
<tr>
<td rowspan="9"><b>File</b></td>
<td>Shares/Directory's/bestanden maken</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Shares/mappen/bestanden verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Mapeigenschappen/meta gegevens</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Bestanden downloaden</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Bestands eigenschappen/meta gegevens/statistieken</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Eigenschappen van bestands service</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Mappen en bestanden weer geven</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Shares weer geven</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td>Eigenschappen/meta gegevens/statistieken delen</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File Service in Java</a></td>
</tr>
<tr>
<td rowspan="8"><b>Wachtrij</b></td>
<td>Bericht toevoegen</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java">Aan de slag met de Azure-versleuteling aan de client zijde in Java</a></td>
</tr>
<tr>
<td>Wacht rijen maken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Bericht/wachtrij verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Bericht bekijken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Wachtrij-ACL/meta gegevens/statistieken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Eigenschappen van Queue-service</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td>Bericht bijwerken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue service in Java</a></td>
</tr>
<tr>
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteit/tabel verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteit invoegen/samen voegen/vervangen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteiten opvragen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Query tabellen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Tabel-ACL/eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteit bijwerken</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
</tbody>
</table>
<br/>

## <a name="azure-code-samples-library"></a>Bibliotheek voor Azure-code voorbeelden

Als u de volledige voorbeeld bibliotheek wilt weer geven, gaat u naar de [Azure code samples](https://azure.microsoft.com/resources/samples/?service=storage) -bibliotheek, die voor beelden bevat voor Azure Storage die u lokaal kunt downloaden en uitvoeren. De voorbeeld bibliotheek code bevat voorbeeld code in. zip-indeling. U kunt ook bladeren en de GitHub-opslag plaats voor elk voor beeld klonen.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Aan de slag-hand leidingen

Bekijk de volgende hand leidingen als u op zoek bent naar instructies voor het installeren en aan de slag met de Azure Storage-client bibliotheken.

* [Aan de slag met Azure Blob service in Java](../blobs/storage-quickstart-blobs-java.md)
* [Aan de slag met Azure Queue service in Java](../queues/storage-java-how-to-use-queue-storage.md)
* [Aan de slag met de Azure-tabelservice in Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Aan de slag met Azure File Service in Java](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over voor beelden voor andere talen:

* .NET: [Azure Storage-voorbeelden met behulp van .NET](storage-samples-dotnet.md)
* Alle andere talen: [Azure Storage-voor beelden](storage-samples.md)
