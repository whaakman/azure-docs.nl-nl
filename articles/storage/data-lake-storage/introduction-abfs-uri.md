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
ms.openlocfilehash: 75edf6dc7382a8a2ece7c25edd09aeacfe1c5189
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060056"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Gebruik de Azure Data Lake Storage Gen2 URI

De [Hadoop Filesystem](http://www.aosabook.org/en/hdfs.html) stuurprogramma dat compatibel is met de Preview van Azure Data Lake Storage Gen2 bekend is onder de schema-id `abfs` (Azure Blob File System). Consistent met andere Hadoop Filesystem-stuurprogramma's, het stuurprogramma ABFS maakt gebruik van een URI-indeling voor bestanden en mappen in een geschikt Gen2 van Data Lake Storage-account.

## <a name="uri-syntax"></a>URI-syntaxis

De URI-syntaxis voor het Data Lake Storage Gen2 is afhankelijk van of uw storage-account is ingesteld om Data Lake Storage Gen2 als het standaardbestandssysteem.

Als het Data Lake Storage Gen2 geschikt account dat u adres wilt is ingesteld als het standaardbestandssysteem tijdens het maken van de account, is het steno URI-syntaxis:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: een slash gescheiden (`/`) weergave van de mapstructuur.

2. **Bestandsnaam**: de naam van de afzonderlijke bestand.

Als het Data Lake Storage Gen2 geschikt account u adres wilt *niet* het standaardbestandssysteem de URI-syntaxis is:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-identificatie**: de `abfs` -protocol wordt gebruikt als de schema-id. U hebt de optie om te verbinden met of zonder een secure socket layer (SSL)-verbinding. Gebruik `abfss` om te verbinden met een secure socket layer-verbinding.

2. **Bestandssysteem**: de locatie van de bovenliggende die de bestanden en mappen bevat. Dit is hetzelfde als Containers in de service Azure Storage-Blobs.

3. **Accountnaam**: de naam van uw storage-account tijdens het maken van.

4. **Paden**: een slash gescheiden (`/`) weergave van de mapstructuur.

5. **Bestandsnaam**: de naam van de afzonderlijke bestand. Deze parameter is optioneel als u een map adresseert.

## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](use-hdi-cluster.md)