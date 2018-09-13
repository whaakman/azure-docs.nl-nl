---
title: Gegevens verplaatsen naar en van Azure Blob-opslag | Microsoft Docs
description: Gegevens verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 854c671d4db6cdca2b019ed9adb0475e588281b1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35622548"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Gegevens verplaatsen van en naar Azure Blob-opslag
[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Methode die voor u geschikt is, is afhankelijk van uw scenario. De [scenario's voor geavanceerde analyses in Azure Machine Learning](plan-sample-scenarios.md) artikel helpt u bepalen welke resources u nodig hebt voor verschillende data science-werkstromen in de geavanceerde analytics-proces gebruikt.

> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [grondbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Als alternatief kunt u [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) aan: 

* maken en plannen van een pijplijn die gegevens vanuit Azure blob-opslag 
* deze doorgeven aan een gepubliceerde Azure Machine Learning-webservice 
* de resultaten voorspellende analyses en 
* de resultaten uploaden naar storage. 

Zie voor meer informatie, [voorspellende pijplijnen maken met Azure Data Factory en Azure Machine Learning](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een storage-account en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gegevens uploadt/downloadt, moet u uw Azure-opslag account naam en de accountsleutel weten.

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een storage-account en voor het ophalen van account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

