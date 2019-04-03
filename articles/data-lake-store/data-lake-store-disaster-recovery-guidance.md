---
title: Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Storage Gen1 | Microsoft Docs
description: Richtlijnen voor herstel na noodgevallen voor Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: b3f1888a73baf2b7f9efa9f5e7cdb3305aa9f90d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878108"
---
# <a name="disaster-recovery-guidance-for-data-in-azure-data-lake-storage-gen1"></a>Richtlijnen voor herstel na noodgevallen voor gegevens in Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 biedt lokaal redundante opslag (LRS). De gegevens in uw Data Lake Storage Gen1-account is daarom tegen tijdelijke hardwarefouten in een datacenter door automatische replica's. Dit garandeert duurzaamheid en hoge beschikbaarheid, voldoen aan de Data Lake Storage Gen1 SLA. In dit artikel bevat richtlijnen voor het verder beschermen van uw gegevens tegen zeldzame regiobrede storingen of onbedoelde verwijderingen.

## <a name="disaster-recovery-guidance"></a>Richtlijnen voor herstel na noodgevallen
Het is essentieel dat elke klant een eigen plan voor herstel na noodgevallen voorbereidt. Lees de informatie in dit artikel voor het bouwen van uw plan voor herstel na noodgevallen. Hier volgen enkele bronnen waarmee u uw eigen plan kunt maken.

* [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technische richtlijnen voor flexibiliteit van Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practices"></a>Aanbevolen procedures
Het is raadzaam dat u uw kritieke gegevens naar een ander Gen1 van Data Lake Storage-account in een andere regio met een frequentie die is afgestemd op de behoeften van uw plan voor herstel na noodgevallen kopiëren. Er zijn verschillende methoden om gegevens te kopiëren, waaronder [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) en [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

Als er een regionale storing optreedt, kunt u vervolgens toegang tot uw gegevens in de regio waar de gegevens zijn gekopieerd. U kunt controleren de [Azure Service Health Dashboard](https://azure.microsoft.com/status/) om te bepalen van de Azure-servicestatus over de hele wereld.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Herstelrichtlijnen voor gegevensbeschadiging en onbedoelde verwijdering
Hoewel Data Lake Storage Gen1 tolerantie van gegevens door automatische replica's biedt, voorkomt dit niet dat uw toepassing (of het ontwikkelaars/gebruikers) van de gegevens te beschadigen of onbedoeld worden verwijderd.

### <a name="best-practices"></a>Aanbevolen procedures
U wordt aangeraden dat u eerst de juiste toegangsbeleiden voor uw account voor Data Lake Storage Gen1 instelt ter voorkoming van onbedoeld verwijderen.  Dit omvat het toepassen van [Azure-resourcevergrendelingen](../azure-resource-manager/resource-group-lock-resources.md) vergrendelen van belangrijke resources als toepassen-account en de bestandsnaam niveau van toegangsbeheer met behulp van de beschikbare [beveiligingsfuncties van Data Lake Storage Gen1](data-lake-store-security-overview.md). We raden u ook aan regelmatig kopieën van het gebruik van uw kritieke gegevens te maken [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) of [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) in een ander Data Lake Storage Gen1 account, een map of een Azure-abonnement.  Dit kan worden gebruikt om gegevens te herstellen nadat ze beschadigd zijn geraakt of per ongeluk zijn verwijderd. Azure Data Factory is een handige service voor het regelmatig maken en implementeren van pijplijnen voor gegevensverplaatsing.

Organisaties kunnen ook inschakelen [registratie in diagnoselogboek](data-lake-store-diagnostic-logs.md) voor hun Gen1 van Data Lake Storage-account voor het verzamelen van audittrails voor gegevenstoegang die informatie biedt over wie mogelijk verwijderd of een bestand bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* [Bescherm uw gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md)

