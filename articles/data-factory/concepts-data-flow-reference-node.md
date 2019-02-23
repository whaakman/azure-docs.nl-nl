---
title: Azure Data Factory gegevensknooppunt stroom verwijzing toewijzen
description: Gegevensstroom van Data Factory, wordt een verwijzingsknooppunt voor joins, zoekacties, samenvoegingen toevoegen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 626943143e8fa193f143e66d856d9b00e3589fb5
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56732795"
---
# <a name="mapping-data-flow-reference-node"></a>Stroom toewijzing-verwijzing gegevensknooppunt

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Verwijzen naar knooppunt](media/data-flow/referencenode.png "verwijzingsknooppunt")

Een referentie-knooppunt wordt automatisch toegevoegd aan het canvas om aan te duiden dat het knooppunt dat is gekoppeld aan de verwijst naar een ander bestaand knooppunt op het canvas. Een verwijzingsknooppunt weer als een verwijzing of een verwijzing naar een andere stroom gegevenstransformatie beschouwen.

Bijvoorbeeld: Wanneer u deelnemen aan of Union van meer dan één stroom van gegevens, het canvas gegevensstroom kan worden gebruikt voor het toevoegen van een verwijzingsknooppunt die overeenkomt met de naam en de instellingen van de niet-primaire binnenkomende stream.

Het verwijzingsknooppunt kan niet worden verplaatst of verwijderd. U kunt echter klikken in het knooppunt om de oorspronkelijke transformatie-instellingen te wijzigen.

De UI-regels die bepalen wanneer het verwijzingsknooppunt gegevensstroom toevoegt worden op basis van de beschikbare ruimte en verticale afstand tussen de rijen.
