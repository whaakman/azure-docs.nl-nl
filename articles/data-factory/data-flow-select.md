---
title: Trans formatie Azure Data Factory toewijzing van gegevens stroom selecteren
description: Trans formatie Azure Data Factory toewijzing van gegevens stroom selecteren
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 15c74637a2dc42ec44f582878b5505d94637cd7b
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314188"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Trans formatie Azure Data Factory toewijzing van gegevens stroom selecteren
[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik deze trans formatie voor kolom selectiviteit (kleiner aantal kolommen), alias kolommen en stroom namen en volg orde van kolommen.

## <a name="how-to-use-select-transformation"></a>Trans formatie selecteren gebruiken
Met de Select Transform kunt u een volledige stroom of kolommen in die stroom aliassen, verschillende namen toewijzen (aliassen) en vervolgens naar deze nieuwe namen verwijzen verderop in uw gegevens stroom. Deze trans formatie is handig voor Self-koppelings scenario's. De manier waarop u een self-samen voeging in de ADF-gegevens stroom implementeert, is door een stroom te maken, deze te vertakking met ' nieuwe vertakking ' en vervolgens onmiddellijk daarna een ' Select '-trans formatie toe te voegen. Deze stroom heeft nu een nieuwe naam die u kunt gebruiken om terug te gaan naar de oorspronkelijke stream, waarbij u een self-deelname maakt:

![Self-deelname](media/data-flow/selfjoin.png "Self-deelname")

In het bovenstaande diagram bevindt de geselecteerde trans formatie zich bovenaan. Dit is een alias voor de oorspronkelijke stroom naar ' OrigSourceBatting '. In de onderstaande gemarkeerde join-trans formatie ziet u dat we deze Select alias Stream gebruiken als de rechter join, zodat we naar dezelfde sleutel in de linker & rechter kant van de inner join kunnen verwijzen.

Select kan ook worden gebruikt als een manier om de kolommen uit uw gegevens stroom te deselecteren. Als u bijvoorbeeld 6 kolommen hebt gedefinieerd in uw Sink, maar u alleen een specifieke 3 wilt transformeren en vervolgens naar de Sink wilt gaan, kunt u alleen die drie selecteren met behulp van de trans formatie selecteren.

> [!NOTE]
> U moet de optie Alles selecteren uitschakelen om alleen specifieke kolommen te kiezen

![Trans formatie selecteren](media/data-flow/select001.png "Alias selecteren")

## <a name="options"></a>Opties
* De standaard instelling voor ' Select ' is om alle binnenkomende kolommen op te slaan en de oorspronkelijke namen te blijven gebruiken. U kunt de stroom alias door de naam van de Select trans formatie in te stellen.
* Als u afzonderlijke kolommen als alias wilt gebruiken, schakelt u ' Alles selecteren ' uit en gebruikt u onderaan de kolom toewijzing.
* Kies dubbele items overs Laan om dubbele kolommen van invoer-of uitvoer meta gegevens te verwijderen.

![Dubbele items overs Laan](media/data-flow/select-skip-dup.png "Dubbele items overs Laan")

## <a name="next-steps"></a>Volgende stappen
* Gebruik de [sink-trans formatie](data-flow-sink.md) om uw gegevens in een gegevens archief in te voeren nadat u selecteren voor naam wijzigen, volg orde en alias kolommen hebt gebruikt.
