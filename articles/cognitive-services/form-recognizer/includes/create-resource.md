---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: a37087c90b6c9b3629402c7a8c2fa5861e46ae9a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592500"
---
Wanneer u toegang tot het gebruik van formulier herkenning zijn verleend, ontvangt u een Welkom e-mailbericht met meerdere koppelingen en bronnen. Gebruik de koppeling 'Azure-portal' in dit bericht naar de Azure-portal openen en maak een formulier herkenning-resource. In de **maken** in het deelvenster de volgende informatie:

|    |    |
|--|--|
| **Name** | Een beschrijvende naam voor uw resource. Wordt u aangeraden een beschrijvende naam, bijvoorbeeld *MyNameFormRecognizer*. |
| **Abonnement** | Selecteer het Azure-abonnement dat toegang heeft gekregen. |
| **Location** | De locatie van uw cognitive service-exemplaar. Verschillende locaties kunnen introduceren latentie, maar hebben geen invloed op de runtime-beschikbaarheid van uw resource. |
| **Prijscategorie** | De kosten van uw resource is afhankelijk van de prijscategorie die u kiest en uw gebruik. Zie voor meer informatie, de API [prijsinformatie](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Resourcegroep** | De [Azure-resourcegroep](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) die uw resource bevat. U kunt een nieuwe groep maken of toe te voegen aan een bestaande groep. |

> [!IMPORTANT]
> Normaal gesproken wanneer u een resource voor Cognitive Service in Azure portal maakt, hebt u de optie voor het maken van een op meerdere services-abonnementssleutel (gebruikt op meerdere cognitive services) of de abonnementssleutel van een enkele service-(alleen gebruikt met een specifieke cognitive service). Echter, omdat formulier herkenning een preview-versie is, het is niet opgenomen in het abonnement op meerdere services en het één service-abonnement kan niet worden gemaakt, tenzij u de koppeling in de verwelkomingse-mail.

Als uw formulier herkenning-resource is voltooid implementeren, zoeken en selecteert u deze in de **alle resources** lijst in de portal. Selecteer vervolgens de **sleutels** tabblad om de abonnementssleutels van uw weer te geven. Beide sleutels, uw apptoegang krijgt tot de resource. Kopieer de waarde van **sleutel 1**. U gebruikt deze in de volgende sectie.