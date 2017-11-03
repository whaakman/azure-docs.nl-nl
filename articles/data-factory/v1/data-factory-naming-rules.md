---
title: Regels voor het benoemen van Azure Data Factory-entiteiten | Microsoft Docs
description: Beschrijving van naamgevingsregels voor Data Factory-entiteiten.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2f4979a07db794f012ba602ab65ac54872003027
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - naamgevingsregels
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [naamgevingsregels in Gegevensfactory versie 2](../naming-rules.md).

De volgende tabel bevat de naamgevingsregels voor Data Factory-artefacten.

| Naam | Uniekheid van de naam | De validatie wordt gecontroleerd |
|:--- |:--- |:--- |
| Data Factory |Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig, dat wil zeggen, `MyDF` en `mydf` verwijzen naar de dezelfde gegevensfactory. |<ul><li>Elke gegevensfactory is gekoppeld aan één Azure-abonnement.</li><li>Object-namen moeten beginnen met een letter of cijfer en mag alleen letters, cijfers en het streepje (-) bevatten.</li><li>Elk streepje (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. Twee opeenvolgende streepjes zijn in containernamen niet toegestaan.</li><li>Naam mag 3-63 tekens lang zijn.</li></ul> |
| Gekoppelde Services/tabellen/pijplijnen |Unieke met in een gegevensfactory. De namen zijn niet hoofdlettergevoelig. |<ul><li>Maximum aantal tekens in een tabelnaam: 260.</li><li>Objectnamen moeten beginnen met een letter, cijfer of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', ' < ', ' > "," * ","%","&",": ","\\"</li></ul> |
| Resourcegroep |Uniek zijn voor Microsoft Azure. De namen zijn niet hoofdlettergevoelig. |<ul><li>Maximum aantal tekens: 1000.</li><li>Naam mag letters, cijfers en de volgende tekens bevatten: '-', ' _ ',', 'en'. '</li></ul> |

