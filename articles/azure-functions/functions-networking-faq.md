---
title: Veelgestelde vragen over netwerken in Azure Functions
description: Antwoorden op enkele van de meest voorkomende vragen en scenario's voor netwerken met Azure Functions.
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 2/26/2019
ms.author: alkarche
ms.openlocfilehash: 7946b7f45ff3df9225a27b70ccfbdb895bfd03c4
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896286"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Veelgestelde vragen over netwerken in Azure Functions

Hieronder volgt een lijst met veelgestelde vragen voor netwerken. Lees voor een uitgebreider overzicht de [netwerken opties voor document-functies](functions-networking-options.md)

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Hoe stel ik een statisch IP-adres in de functies?

Implementatie van een functie in een App Service-omgeving (ASE) is momenteel de enige manier om een statische binnenkomende en uitgaande IP-adres voor uw functie. Beginnen met het artikel hier voor meer informatie over het gebruik van een as-omgeving: [Het maken en gebruiken van een ILB as-omgeving](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>Hoe beperk ik de toegang tot Internet voor mijn functie?

U kunt toegang tot internet op een aantal manieren, hieronder vermelde beperken.

* [IP-beperkingen](../app-service/app-service-ip-restrictions.md): binnenkomend verkeer naar uw functie-app beperken door IP-bereik.
* Verwijder alle HTTP-triggers. Voor sommige toepassingen is het voldoende om de HTTP-triggers te voorkomen dat een andere bron gebruiken om uw functie te activeren.

De belangrijkste overweging bij het in dat geval is dat er rekening mee dat de Azure portal-editor is vereist voor directe toegang tot uw actieve functie te gebruiken. Geen codewijzigingen via de Azure-portal moet het apparaat dat u de portal om de IP-endexemplaren bladeren. Nog steeds, maar kunt u alles op het tabblad van de functies platform met netwerkbeperkingen in plaats.

## <a name="how-do-i-restrict-my-function-app-to-a-vnet"></a>Hoe beperk ik mijn functie-app met een VNET?

De enige manier om een functie volledig te beperken, zodat alle verkeer via een VNET stroomt is met een intern gelijke taakverdeling (ILB) App Service Environment (ASE). Deze optie wordt uw site op speciale infrastructuur binnen een VNET wordt geïmplementeerd en alle triggers en het verkeer via de VNET worden verzonden. Beginnen met het artikel hier voor meer informatie over het gebruik van een as-omgeving: [Het maken en gebruiken van een ILB as-omgeving](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-vnet-from-a-function-app"></a>Hoe krijg ik toegang tot resources in een VNET van een functie-app?

U kunt toegang tot resources in een VNET via een actieve functie met behulp van VNET-integratie. Zie voor meer informatie, [VNET-integratie](functions-networking-options.md#vnet-integration)

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hoe krijg ik toegang tot bronnen worden beveiligd door de Service-eindpunten?

Met behulp van de nieuwe VNET-integratie (momenteel in preview), kunt u toegang tot beveiligde bronnen van een actieve functie het Service-eindpunt. Zie voor meer informatie, [preview van de VNET-integratie](functions-networking-options.md#preview-vnet-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-vnet"></a>Hoe kan ik een functie van een resource in een VNET activeren?

U kunt alleen een functie van een resource in een VNET activeren door uw functie-app implementeren in een App Service Environment. Zie voor meer informatie over het gebruik van een as-omgeving [maken en gebruiken van een ILB as-omgeving](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-vnet"></a>Hoe kan ik mijn functie-app in een VNET implementeren?

Implementeren naar een App Service Environment is de enige manier om een functie-app die is volledig binnen een VNET voor meer informatie over het gebruik van een ILB as-omgeving maken, te beginnen met het artikel hier: [Het maken en gebruiken van een ILB as-omgeving](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Voor scenario's waarin u alleen één richting toegang tot VNET-resources of minder uitgebreide netwerkisolatie hoeft, Zie de [netwerken overzicht van Functions](functions-networking-options.md).
