---
title: Veelgestelde vragen over Azure Data Lake Store | Microsoft Docs
description: Hulp bij het oplossen en voorkomen van problemen met Azure Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: f426cf4b91941f19714c33abc21ef0c777f5b233
ms.openlocfilehash: 38f3ca24b7fa136bdb4b64b6cae77078b7ea3c97


---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Veelgestelde vragen over Azure Data Lake Store
In dit artikel komt u meer te weten over veelgestelde vragen met betrekking tot Azure Data Lake Store.

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>Hoe kan ik mijn gegevens beter beschermen tegen regiobrede noodgevallen of onbedoelde verwijderingen?
De gegevens op uw Azure Data Lake Store-account zijn beschermd tegen tijdelijke hardwarefouten in een regio door automatische replica's. Dit garandeert duurzaamheid en een hoge beschikbaarheid, waarmee wordt voldaan aan de SLA van Azure Data Lake Store. Hier volgen een aantal richtlijnen voor het verder beschermen van uw gegevens tegen zeldzame regiobrede storingen of onbedoelde verwijderingen.

### <a name="disaster-recovery-guidance"></a>Richtlijnen voor herstel na noodgevallen
Het is essentieel dat elke klant een eigen plan voor herstel na noodgevallen voorbereidt. Raadpleeg onderstaande Azure-documentatie voor het opstellen van uw eigen plan voor herstel na noodgevallen. Hier volgen enkele bronnen waarmee u uw eigen plan kunt maken.

* [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technische richtlijnen voor flexibiliteit van Azure](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>Aanbevolen procedures
We raden u aan uw kritieke gegevens naar een ander Data Lake Store-account in een andere regio te kopiëren volgens een frequentie in lijn met uw plan voor herstel na noodgevallen. Er zijn verschillende methoden om gegevens te kopiëren, waaronder [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) en [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md). Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

In het geval van een regionale storing kunt u uw gegevens openen vanuit de regio waarnaar u ze hebt gekopieerd. U kunt het [Azure Service Health Dashboard](https://azure.microsoft.com/status/) in de gaten houden voor de wereldwijde servicestatus van Azure.

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Herstelrichtlijnen voor gegevensbeschadiging en onbedoelde verwijdering
Hoewel Azure Data Lake Store gegevensflexibiliteit biedt door middel van automatische replica's, sluit dit niet uit dat de gegevens door uw toepassing (of door ontwikkelaars/gebruikers) worden beschadigd of onbedoeld worden verwijderd.

#### <a name="best-practices"></a>Aanbevolen procedures
Om onbedoelde verwijdering te voorkomen, is het raadzaam dat u eerst de juiste toegangsbeleiden instelt voor uw Data Lake Store-account met de beschikbare [Data Lake Store-beveiligingsfuncties](data-lake-store-security-overview.md). We raden u ook aan regelmatig kopieën van uw kritieke gegevens te maken met [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) of [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) naar een ander Data Lake Store-account, een andere map of een ander Azure-abonnement.  Dit kan worden gebruikt om gegevens te herstellen nadat ze beschadigd zijn geraakt of per ongeluk zijn verwijderd. Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

Bedrijven kunnen ook [Diagnostische gegevens vastleggen](data-lake-store-diagnostic-logs.md) inschakelen voor hun Azure Data Lake Store-account om audittrails voor gegevenstoegang te verzamelen. Deze bieden informatie over wie mogelijk een bestand heeft verwijderd of bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)




<!--HONumber=Nov16_HO5-->


