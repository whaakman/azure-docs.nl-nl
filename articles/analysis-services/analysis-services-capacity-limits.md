---
title: Azure Analysis Services-resource en object limieten | Microsoft Docs
description: Beschrijving van Azure Analysis Services-resource en object-limieten.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c2cebe2225e475ccd40460e7b10a6ba3ed428d5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724005"
---
# <a name="analysis-services-resource-and-object-limits"></a>Analysis Services-resource en object limieten

Dit artikel wordt beschreven bron en het model object limieten.

## <a name="tier-limits"></a>Laag-limieten

### <a name="developer-tier"></a>Developer-laag

Deze laag wordt aanbevolen voor evaluatie-, ontwikkelings- en testscenario's. Eén abonnement bevat alle functionaliteit van de Standard-laag, maar heeft beperkte verwerkingskracht, QPU's en geheugengrootte. Uitschalen van queryreplica’s *is niet beschikbaar* voor deze laag. Voor deze laag is geen SLA beschikbaar.

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Basislaag

Deze-laag wordt aanbevolen voor productieoplossingen met kleinere tabellarische modellen, een beperkt aantal gelijktijdige gebruikers en eenvoudige vereisten voor gegevensvernieuwing. Uitschalen van queryreplica’s *is niet beschikbaar* voor deze laag. Perspectieven, meerdere partities en functies van het tabellarische DirectQuery-model *worden niet ondersteund* in deze laag.  

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Standaardlaag

Deze laag is de meest geschikte voor essentiële productietoepassingen waarvoor elastische gebruikersgelijktijdigheid is vereist en waarvan de gegevensmodellen snel groter worden. Deze laag ondersteunt geavanceerde gegevensvernieuwing voor bijna real-time gegevensmodelupdates en ondersteunt alle tabellarische modelleringsfuncties.

|Plannen  |QPU's  |Geheugen (GB)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Niet beschikbaar in alle regio’s.  

## <a name="object-limits"></a>Object-limieten

Dit zijn theoretische limieten. Prestaties wordt op lagere getallen worden verminderd.

|Object|Maximale grootte/getallen|  
|------------|----------------------------|  
|Databases in een exemplaar|16,000|  
|Gecombineerde aantal tabellen en kolommen in een database|16,000|  
|Rijen in een tabel|Onbeperkt<br /><br /> **Waarschuwing:** met de beperking dat geen enkele kolom in de tabel meer dan 1.999.999.997 afzonderlijke waarden kan hebben.|  
|Hiërarchieën in een tabel|15,999|  
|Niveaus in een hiërarchie|15,999|  
|Relaties|8,000|  
|Sleutelkolommen in alle tabel|15,999|  
|Metingen in een tabellen|2 ^ 31-1 = 2.147.483.647|  
|Cellen die zijn geretourneerd door een query|2 ^ 31-1 = 2.147.483.647|  
|Grootte van de record van de bronquery|64 KB|  
|Lengte van de namen van objecten|512 tekens|  


