---
title: De Azure Data Lake Storage Gen2-URI gebruiken
description: De Azure Data Lake Storage Gen2-URI gebruiken
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: 04df30c2a97e865d23999df26768b38cb38be607
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855565"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>De Azure Data Lake Storage Gen2-URI gebruiken

Het stuur programma [Hadoop-bestands](https://www.aosabook.org/en/hdfs.html) systeem dat compatibel is met Azure data Lake Storage Gen2, wordt aangeduid `abfs` met de schema-ID (Azure Blob File System). Het ABFS-stuur programma maakt consistent met andere Hadoop-bestandssysteem Stuur Programma's en maakt gebruik van een URI-indeling voor het adresseren van bestanden en mappen in een Data Lake Storage Gen2-account.

## <a name="uri-syntax"></a>URI-syntaxis

De syntaxis van de URI voor Data Lake Storage Gen2 is afhankelijk van het feit of uw opslag account is ingesteld op Data Lake Storage Gen2 als het standaard bestandssysteem.

Als het Data Lake Storage Gen2 geschikte account dat u wilt adresseren, **niet is** ingesteld als het standaard bestandssysteem tijdens het maken van het account, is de syntaxis van de steno-URI:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-ID**: Het `abfs` protocol wordt als de schema-ID gebruikt. U hebt de optie om verbinding te maken met of zonder een Secure Socket Layer (SSL)-verbinding. Gebruiken `abfss` om verbinding te maken met een Secure Socket Layer-verbinding.

2. **Bestands systeem**: De bovenliggende locatie van de bestanden en mappen. Dit is hetzelfde als containers in de Azure Storage blobs-service.

3. **Accountnaam**: De naam die aan uw opslag account is gegeven tijdens het maken.

4. **Paden**: Een door slash gescheiden (`/`) weer gave van de mapstructuur.

5. **Bestands naam**: De naam van het afzonderlijke bestand. Deze para meter is optioneel als u een directory adresseert.

Als het account dat u wilt adresseren echter is ingesteld als het standaard bestandssysteem tijdens het maken van het account, is de syntaxis van de steno-URI:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: Een door slash gescheiden (`/`) weer gave van de mapstructuur.

2. **Bestands naam**: De naam van het afzonderlijke bestand.


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
