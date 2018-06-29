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
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: eb4579c2d94ae610d21b7221c0b410b07268d419
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051795"
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - naamgevingsregels
> [!NOTE]
> In dit artikel is van toepassing op versie 1 van de Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [naamgevingsregels in Gegevensfactory](../naming-rules.md).

De volgende tabel bevat de naamgevingsregels voor Data Factory-artefacten.

| Naam | Uniekheid van de naam | De validatie wordt gecontroleerd |
|:--- |:--- |:--- |
| Data Factory |Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig, dat wil zeggen, `MyDF` en `mydf` verwijzen naar de dezelfde gegevensfactory. |<ul><li>Elke gegevensfactory is gekoppeld aan één Azure-abonnement.</li><li>Object-namen moeten beginnen met een letter of cijfer en mag alleen letters, cijfers en het streepje (-) bevatten.</li><li>Elk streepje (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. Twee opeenvolgende streepjes zijn in containernamen niet toegestaan.</li><li>Naam mag 3-63 tekens lang zijn.</li></ul> |
| Gekoppelde Services/tabellen/pijplijnen |Unieke met in een gegevensfactory. De namen zijn niet hoofdlettergevoelig. |<ul><li>Maximum aantal tekens in een tabelnaam: 260.</li><li>Objectnamen moeten beginnen met een letter, cijfer of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', ' < ', ' > "," * ","%","&",": ","\\"</li></ul> |
| Resourcegroep |Uniek zijn voor Microsoft Azure. De namen zijn niet hoofdlettergevoelig. |<ul><li>Maximum aantal tekens: 1000.</li><li>Naam mag letters, cijfers en de volgende tekens bevatten: '-', ' _ ',', 'en'. '</li></ul> |

