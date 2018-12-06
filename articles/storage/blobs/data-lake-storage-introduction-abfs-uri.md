---
title: Gebruik de Azure Data Lake Storage Gen2 Preview URI
description: Gebruik de Azure Data Lake Storage Gen2 Preview URI
services: storage
author: jamesbak
ms.topic: conceptual
ms.author: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e596123cb218a542166d80b53916a73034f71760
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975254"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Gebruik de Azure Data Lake Storage Gen2 URI

De [Hadoop Filesystem](http://www.aosabook.org/en/hdfs.html) stuurprogramma die compatibel is met de Preview van Azure Data Lake Storage Gen2 bekend is onder de schema-id `abfs` (Azure Blob-bestandssysteem). Consistent zijn met andere Hadoop Filesystem stuurprogramma's, het stuurprogramma ABFS maakt gebruik van een URI-notatie op adresbestanden en mappen in een Data Lake Storage Gen2 geschikt-account.

## <a name="uri-syntax"></a>URI-syntaxis

De syntaxis van de URI voor de Data Lake Storage Gen2 is afhankelijk van of uw storage-account is ingesteld dat Data Lake Storage Gen2 als het standaardbestandssysteem.

Als het Data Lake Storage Gen2 geschikt account u adres wilt **is niet** instellen als het standaardbestandssysteem tijdens het maken van account en vervolgens de verkorte versie van URI-syntaxis is:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-id**: de `abfs` -protocol wordt gebruikt als de schema-id. U hebt de optie verbinding maken met of zonder een secure socket layer (SSL)-verbinding. Gebruik `abfss` om te verbinden met een secure socket layer-verbinding.

2. **Bestandssysteem**: de bovenliggende locatie waarin de bestanden en mappen. Dit is hetzelfde als Containers in de service Azure Storage-Blobs.

3. **Accountnaam**: de naam van uw storage-account tijdens het maken van.

4. **Paden**: een slash als scheidingsteken (`/`) weergave van de mapstructuur.

5. **Bestandsnaam**: de naam van de afzonderlijke bestanden. Deze parameter is optioneel als u een map hebben betrekking op.

Maar als het account dat u adres wilt is ingesteld als het standaardbestandssysteem tijdens het maken van accounts, is klikt u vervolgens de verkorte versie van URI-syntaxis:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: een slash als scheidingsteken (`/`) weergave van de mapstructuur.

2. **De naam van het bestand**: de naam van de afzonderlijke bestanden.


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](data-lake-storage-use-hdi-cluster.md)
