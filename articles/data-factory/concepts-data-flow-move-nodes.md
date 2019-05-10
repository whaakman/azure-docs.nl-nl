---
title: Azure Data Factory-stroom verplaatsen gegevensknooppunten
description: Het verplaatsen van knooppunten in een Azure Data Factory gegevensstroom-Diagram
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: c41ab1c0c8a26488c476d187fbc1bcac2e624ac8
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472015"
---
# <a name="mapping-data-flow-move-nodes"></a>Toewijzing van gegevensknooppunten stroom verplaatsen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Opties voor transformatie aggregeren](media/data-flow/agghead.png "aggregator koptekst")

Het ontwerpoppervlak gegevensstroom van Azure Data Factory is een oppervlak "bouwen" waar u gegevensstromen van boven naar beneden, links-naar-rechts maken. Er is een werkset die is gekoppeld aan elke transformatie met een plusteken (+) symbool. Zich concentreren op uw bedrijfslogica in plaats van de knooppunten via randen in de omgeving van een vrije DAG verbinding te maken.

Dus zonder een paradigma slepen en neerzetten, de manier om een knooppunt transformatie 'verplaatsen' is te wijzigen van de stroom inkomende. In plaats daarvan, verplaatst u transformaties om door te wijzigen van de 'binnenkomende stream'.

In Azure Data Factory-gegevensstroom vertegenwoordigen stromen de stroom van gegevens. De instellingen voor transformatie in het deelvenster ziet u een veld 'Binnenkomende Stream'. Hiermee wordt aangegeven welke binnenkomende gegevensstroom is deze transformatie. U kunt de fysieke locatie van de transformatie-knooppunt in de grafiek wijzigen door te klikken op de naam van de binnenkomende Stream en selecteren van een andere stroom van gegevens. De huidige transformatie samen met alle volgende transformaties voor deze stroom wordt vervolgens naar de nieuwe locatie verplaatsen.

Als u een transformatie met een of meer transformaties nadat deze zijn verplaatst, wordt de nieuwe locatie in de gegevensstroom worden gekoppeld via een nieuwe vertakking.

Hebt u geen verdere transformaties na het knooppunt dat u hebt geselecteerd, wordt alleen deze transformatie verplaatsen naar de nieuwe locatie.
