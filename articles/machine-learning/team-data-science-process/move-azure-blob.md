---
title: Gegevens van en naar Azure Blob-opslag verplaatsen | Microsoft Docs
description: Gegevens verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev;sachouks
ms.openlocfilehash: f282705b6d5d1f6867ea87737f19b5550054789a
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Gegevens verplaatsen en naar Azure Blob-opslag
[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Welke methode voor u het beste is, is afhankelijk van uw scenario. De [scenario's voor geavanceerde analyses in Azure Machine Learning](plan-sample-scenarios.md) artikel kunt u bepalen welke resources u nodig hebt voor een verscheidenheid aan gegevens wetenschappelijke werkstromen gebruikt tijdens de geavanceerde analyses.

> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [basisbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Als alternatief kunt u [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) naar: 

* maken en een pijplijn die gegevens gedownload uit Azure blob-opslag plannen 
* Geef dit door aan een gepubliceerde Azure Machine Learning-webservice 
* ontvangen van de resultaten predictive analytics en 
* Upload de resultaten naar de opslag. 

Zie voor meer informatie [voorspellende pijplijnen met behulp van Azure Data Factory en Azure Machine Learning maken](../../data-factory/v1/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslagaccount en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gaat gegevens uploaden/downloaden, moet u weten Azure naam en sleutel van uw opslagaccount.

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een opslagaccount en voor het ophalen van de account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

