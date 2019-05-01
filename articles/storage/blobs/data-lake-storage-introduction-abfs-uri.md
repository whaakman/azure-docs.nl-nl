---
title: Gebruik de Azure Data Lake Storage Gen2 URI
description: Gebruik de Azure Data Lake Storage Gen2 URI
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: 3f486da121927be23a6bd86e8567574cd95c541e
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939273"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Gebruik de Azure Data Lake Storage Gen2 URI

De [Hadoop Filesystem](https://www.aosabook.org/en/hdfs.html) stuurprogramma die compatibel is met Azure Data Lake Storage Gen2 bekend is onder de schema-id `abfs` (Azure Blob-bestandssysteem). Consistent zijn met andere Hadoop Filesystem stuurprogramma's, het stuurprogramma ABFS maakt gebruik van een URI-notatie op adresbestanden en mappen in een Data Lake Storage Gen2 geschikt-account.

## <a name="uri-syntax"></a>URI-syntaxis

De syntaxis van de URI voor de Data Lake Storage Gen2 is afhankelijk van of uw storage-account is ingesteld dat Data Lake Storage Gen2 als het standaardbestandssysteem.

Als het Data Lake Storage Gen2 geschikt account u adres wilt **is niet** instellen als het standaardbestandssysteem tijdens het maken van account en vervolgens de verkorte versie van URI-syntaxis is:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-id**: De `abfs` -protocol wordt gebruikt als de schema-id. U hebt de optie verbinding maken met of zonder een secure socket layer (SSL)-verbinding. Gebruik `abfss` om te verbinden met een secure socket layer-verbinding.

2. **Bestandssysteem**: De bovenliggende locatie waarin de bestanden en mappen. Dit is hetzelfde als Containers in de service Azure Storage-Blobs.

3. **Accountnaam**: De naam die aan uw storage-account tijdens het maken van.

4. **Paden**: Een slash als scheidingsteken (`/`) weergave van de mapstructuur.

5. **Bestandsnaam**: De naam van de afzonderlijke bestanden. Deze parameter is optioneel als u een map hebben betrekking op.

Maar als het account dat u adres wilt is ingesteld als het standaardbestandssysteem tijdens het maken van accounts, is klikt u vervolgens de verkorte versie van URI-syntaxis:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: Een slash als scheidingsteken (`/`) weergave van de mapstructuur.

2. **De naam van het bestand**: De naam van de afzonderlijke bestanden.


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
