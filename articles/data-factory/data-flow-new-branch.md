---
title: Azure Data Factory Mapping Data Flow New Branch Transformation
description: Azure Data Factory Mapping Data Flow New Branch Transformation
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: c48c1500a31ce8f22bcef7207c0ea67caca24231
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271864"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Azure Data Factory Mapping Data Flow New Branch Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Opties voor vertakking](media/data-flow/menu.png "menu")

Vertakkingen nemen van de huidige gegevensstroom in uw gegevensstroom en gerepliceerd naar een andere stroom. De nieuwe vertakking gebruiken om uit te voeren meerdere sets bewerkingen en transformaties voor dezelfde gegevensstroom.

Voorbeeld: Uw gegevensstroom heeft een bron transformeren met een bepaalde verzameling van kolommen en de conversie van het gegevenstype. U plaatst vervolgens een afgeleide kolom direct na deze bron. In de kolom afgeleid, hebt u een nieuw veld maken die combineert de voornaam en achternaam in om een nieuwe "volledige naam"-veld te maken.

U kunt behandelen die nieuwe stream met een set transformaties en een sink op één rij en nieuwe vertakking gebruiken voor het maken van een kopie van deze stream waar u deze gegevens met een andere set transformaties kunt transformeren. Door de gekopieerde gegevens in een afzonderlijke vertakking transformeert, kunt u die gegevens vervolgens sink naar een afzonderlijke locatie.

> [!NOTE]
> 'Nieuwe vertakking' wordt alleen weergegeven als een actie op de + transformatie menu wanneer er een latere transformatie na de huidige locatie waar u toegang probeert te vertakken. dat wil zeggen ziet niet u een optie 'Nieuwe vertakking' aan het einde hier totdat u een andere transformatie na het selecteren toevoegen

![Vertakking](media/data-flow/branch2.png "vertakking 2")
