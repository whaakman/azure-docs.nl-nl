---
title: Azure Data Factory gegevensstroom toewijzing maken
description: Azure Data Factory gegevensstroom toewijzing maken
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: bb6ae9f97d681625218118b8adca116de1c0fb21
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728230"
---
# <a name="create-azure-data-factory-data-flow"></a>Gegevensstroom van Azure Data Factory maken

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Toewijzing van gegevens stromen in ADF bieden een manier om gegevens te transformeren op schaal zonder codering vereist. U kunt een taak voor gegevenstransformatie in de ontwerpfunctie van de gegevens door een reeks transformaties ontwerpen. Beginnen met een willekeurig aantal bron transformaties gevolgd door de stappen voor gegevenstransformatie. Voltooi uw gegevensstroom met sink op grond van de resultaten in een doel.

Aan de slag door eerst een nieuwe V2-Gegevensfactory maken vanuit Azure Portal. Nadat uw nieuwe gegevensfactory is gemaakt, klikt u op de tegel 'Maken en controleren' om te starten van de gebruikersinterface van Data Factory.

![Opties voor Flow](media/data-flow/v2dataflowportal.png "gegevensstroom maken")

Zodra u zich in de gebruikersinterface van Data Factory, kunt u de voorbeeld-gegevens stromen kunt gebruiken. De voorbeelden zijn beschikbaar in de galerie met ADF. In ADF, maken 'Pijplijn van sjabloon' en selecteer de categorie gegevensstroom in de galerie met sjablonen.

![Opties voor Flow](media/data-flow/template.png "gegevensstroom maken")

U wordt gevraagd om in te voeren van de gegevens van uw Azure Blob Storage-account.

![Opties voor Flow](media/data-flow/template2.png "gegevensstroom maken 2")

[De gegevens die worden gebruikt voor deze voorbeelden vindt u hier](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). De voorbeeldgegevens downloaden en opslaan van de bestanden in uw Azure Blob storage-accounts, zodat u kunt de voorbeelden uitvoeren.

## <a name="create-new-data-flow"></a>Nieuwe gegevensstroom te maken

Gebruik de Resource maken "plusteken" knop in de UI ADF gegevens stromen maken

![Opties voor Flow](media/data-flow/newresource.png "nieuwe Resource")

## <a name="next-steps"></a>Volgende stappen

Begin met het maken van de gegevenstransformatie van uw met een [bron transformatie](data-flow-source.md).
