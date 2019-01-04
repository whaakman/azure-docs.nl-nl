---
title: Regels voor het benoemen van Azure Data Factory-entiteiten | Microsoft Docs
description: Beschrijving van naamgevingsregels voor Data Factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: c02a9393de72b827b7e38b52d06589f042d581b0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024770"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - naamgevingsregels
De volgende tabel bevat de naamgevingsregels voor Data Factory-artefacten.

| Name | Uniekheid van naam | De validatie wordt gecontroleerd |
|:--- |:--- |:--- |
| Data Factory |Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig, dat wil zeggen, `MyDF` en `mydf` verwijzen naar de dezelfde data factory. |<ul><li>Elke data factory is gekoppeld aan één Azure-abonnement.</li><li>De naam moeten beginnen met een letter of cijfer en mag alleen letters, cijfers en streepjes (-) bevatten.</li><li>Elk streepje (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. Opeenvolgende streepjes zijn niet toegestaan in containernamen.</li><li>Naam mag 3 tot 63 tekens lang zijn.</li></ul> |
| Gekoppelde Services/gegevenssets/pijplijnen |Unieke met in een data factory. Namen zijn niet hoofdlettergevoelig. |<ul><li>De naam moeten beginnen met een letter, cijfer of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: ".", '+ ','?', '/', ' < ', ' > ',' * ', '%', '&', ":","\\"</li><li>Streepjes ('-') zijn niet toegestaan in de namen van gekoppelde services en gegevenssets alleen.</li></ul>  |
| Resourcegroep |Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig. | Zie voor meer informatie, [Azure naamgevingsregels en beperkingen](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het maken van data factory's door het volgen van stapsgewijze instructies in [Snelstartgids: een data Factory](quickstart-create-data-factory-powershell.md) artikel. 
