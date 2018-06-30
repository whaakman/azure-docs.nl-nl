---
title: De Azure Data Lake Storage Gen2 Preview URI gebruiken
description: De Azure Data Lake Storage Gen2 Preview URI gebruiken
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
manager: jahogg
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a6130d8440b16e5a72c939fc07f6bf32c0946418
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114289"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Gebruik de Azure Data Lake Storage Gen2 URI

De [Hadoop Filesystem](http://www.aosabook.org/en/hdfs.html) stuurprogramma dat compatibel is met de Preview van Azure Data Lake Storage Gen2 bekend is onder de schema-id `abfs` (Azure Blob File System). Consistent met andere Hadoop Filesystem-stuurprogramma's, het stuurprogramma ABFS maakt gebruik van een URI-indeling voor bestanden en mappen in een geschikt Gen2 van Data Lake Storage-account.

## <a name="uri-syntax"></a>URI-syntaxis

De URI-syntaxis voor het Data Lake Storage Gen2 is afhankelijk van of uw storage-account is ingesteld om Data Lake Storage Gen2 als het standaardbestandssysteem.

Als het Data Lake Storage Gen2 geschikt account u adres wilt **is geen** ingesteld als het standaardbestandssysteem tijdens het maken van de account, wordt het steno URI-syntaxis is:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-identificatie**: de `abfs` -protocol wordt gebruikt als de schema-id. U hebt de optie om te verbinden met of zonder een secure socket layer (SSL)-verbinding. Gebruik `abfss` om te verbinden met een secure socket layer-verbinding.

2. **Bestandssysteem**: de locatie van de bovenliggende die de bestanden en mappen bevat. Dit is hetzelfde als Containers in de service Azure Storage-Blobs.

3. **Accountnaam**: de naam van uw storage-account tijdens het maken van.

4. **Paden**: een slash gescheiden (`/`) weergave van de mapstructuur.

5. **Bestandsnaam**: de naam van de afzonderlijke bestand. Deze parameter is optioneel als u een map adresseert.

Echter, als het account dat u adres wilt is ingesteld als het standaardbestandssysteem tijdens het maken van de account, het steno URI-syntaxis is:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: een slash gescheiden (`/`) weergave van de mapstructuur.

2. **Bestandsnaam**: de naam van de afzonderlijke bestand.


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](use-hdi-cluster.md)