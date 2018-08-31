---
title: Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Store | Microsoft Docs
description: Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: b51f0c1e0c6ef713bf8d3ff0a124300f446a9373
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306805"
---
# <a name="disaster-recovery-guidance-for-data-in-data-lake-store"></a>Richtlijnen voor herstel na noodgevallen voor gegevens in Data Lake Store

Azure Data Lake Store biedt lokaal redundante opslag (LRS). De gegevens in uw Azure Data Lake Store-account is daarom tegen tijdelijke hardwarefouten in een datacenter door automatische replica's. Dit garandeert duurzaamheid en een hoge beschikbaarheid, waarmee wordt voldaan aan de SLA van Azure Data Lake Store. In dit artikel bevat richtlijnen voor het verder beschermen van uw gegevens tegen zeldzame regiobrede storingen of onbedoelde verwijderingen.

## <a name="disaster-recovery-guidance"></a>Richtlijnen voor herstel na noodgevallen
Het is essentieel dat elke klant een eigen plan voor herstel na noodgevallen voorbereidt. Lees de informatie in dit artikel voor het bouwen van uw plan voor herstel na noodgevallen. Hier volgen enkele bronnen waarmee u uw eigen plan kunt maken.

* [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technische richtlijnen voor flexibiliteit van Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Aanbevolen procedures
We raden u aan uw kritieke gegevens naar een ander Data Lake Store-account in een andere regio te kopiëren volgens een frequentie in lijn met uw plan voor herstel na noodgevallen. Er zijn verschillende methoden om gegevens te kopiëren, waaronder [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) en [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

In het geval van een regionale storing kunt u uw gegevens openen vanuit de regio waarnaar u ze hebt gekopieerd. U kunt het [Azure Service Health Dashboard](https://azure.microsoft.com/status/) in de gaten houden voor de wereldwijde servicestatus van Azure.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Herstelrichtlijnen voor gegevensbeschadiging en onbedoelde verwijdering
Hoewel Azure Data Lake Store gegevensflexibiliteit biedt door middel van automatische replica's, sluit dit niet uit dat de gegevens door uw toepassing (of door ontwikkelaars/gebruikers) worden beschadigd of onbedoeld worden verwijderd.

### <a name="best-practices"></a>Aanbevolen procedures
Om onbedoelde verwijdering te voorkomen, is het raadzaam dat u eerst het juiste toegangsbeleid instelt voor uw Data Lake Store-account.  Dit omvat het vergrendelen van belangrijke resources met [Azure-resourcevergrendelingen](../azure-resource-manager/resource-group-lock-resources.md) en het instellen van toegangsbeheer op account- en bestandsniveau met behulp van de beschikbare [beveiligingsfuncties van Data Lake Store](data-lake-store-security-overview.md). We raden u ook aan regelmatig kopieën van uw kritieke gegevens te maken met [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) of [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) naar een ander Data Lake Store-account, een andere map of een ander Azure-abonnement.  Dit kan worden gebruikt om gegevens te herstellen nadat ze beschadigd zijn geraakt of per ongeluk zijn verwijderd. Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

Bedrijven kunnen ook [Diagnostische gegevens vastleggen](data-lake-store-diagnostic-logs.md) inschakelen voor hun Azure Data Lake Store-account om audittrails voor gegevenstoegang te verzamelen. Deze bieden informatie over wie mogelijk een bestand heeft verwijderd of bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)

