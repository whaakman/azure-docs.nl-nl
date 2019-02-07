---
title: Azure Data Explorer-connector voor Apache Spark
description: In dit artikel wordt beschreven hoe u met de Azure Data Explorer-connector voor Apache Spark.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: 6f115df97d0b790c94d6dc07f3f40feeceb1a7cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824113"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Azure Data Explorer-connector voor Apache Spark

Met Azure Data Explorer en Apache Spark, kunt u ontwikkelen voor snelle en schaalbare toepassingen die zijn gericht op scenario's, zoals machine learning-(ML), Extract-Transform-Load (ETL) en Log Analytics-gegevens.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de connector, moet uw toepassing:

* 1.8 Java SDK
* Maven 3.x
* Spark versie 2.3.2 of hoger

## <a name="link-to-data-explorer"></a>Koppeling naar Data Explorer

Definities voor Scala en Java-toepassingen met behulp van Maven-project, Koppel uw App met de volgende artefacten:

```java
groupId = com.microsoft.azure
artifactId = spark-kusto-connector
version = 1.0.0-Beta-01 
```

In Maven, koppelt u het volgende:

```Maven
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-kusto-connector</artifactId>
    <version>1.0.0-Beta-01</version>
  </dependency>
```

## <a name="build-commands"></a>Bouwen-opdrachten

Jar bouwen en uitvoeren van alle tests:

```
mvn clean package
```

Voor het bouwen van jar, voert u alle test en jar installeren op uw lokale opslagplaats met Maven:

```
mvn clean install
```