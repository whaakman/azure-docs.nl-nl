---
title: Veelgestelde vragen over netwerken in Azure Functions
description: Antwoorden op enkele van de meest voorkomende vragen en scenario's voor netwerken met Azure Functions.
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 3cf6a0d080e2d8cafcab8e69a614b59a470c7aba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637043"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Veelgestelde vragen over netwerken in Azure Functions

In dit artikel geeft een lijst met veelgestelde vragen over netwerken in Azure Functions. Zie voor een uitgebreider overzicht [functies netwerkopties](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Hoe stel ik een statisch IP-adres in de functies?

Implementatie van een functie in een App Service Environment is momenteel de enige manier om een statische binnenkomende en uitgaande IP-adres voor uw functie. Beginnen met het artikel voor meer informatie over het gebruik van een App Service Environment [maken en gebruiken van een interne load balancer met een App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>Hoe beperk ik de toegang tot internet voor mijn functie?

U kunt toegang tot internet op een aantal manieren beperken:

* [IP-beperkingen](../app-service/app-service-ip-restrictions.md): Binnenkomend verkeer naar uw functie-app beperken door IP-bereik.
* Het verwijderen van alle HTTP-triggers. Voor sommige toepassingen is voldoende om de HTTP-triggers te voorkomen dat een andere bron gebruiken om uw functie te activeren.

Houd er rekening mee dat de Azure portal-editor is vereist voor directe toegang tot uw functie wordt uitgevoerd. Geen codewijzigingen via de Azure-portal moet het apparaat dat u de portal om de IP-endexemplaren bladeren. Maar u kunt op het tabblad van de functies platform nog steeds gebruiken met netwerkbeperkingen op locatie.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>Hoe beperk ik mijn functie-app met een virtueel netwerk?

De enige manier om een functie volledig te beperken, zodat alle verkeer via een virtueel netwerk stroomt is het gebruik van een intern gelijke App Service Environment. Deze optie de site op een speciale infrastructuur binnen een virtueel netwerk implementeert en alle triggers en het verkeer via het virtuele netwerk worden verzonden. 

Beginnen met het artikel voor meer informatie over het gebruik van een App Service Environment [maken en gebruiken van een interne load balancer met een App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>Hoe krijg ik toegang tot resources in een virtueel netwerk van een functie-app?

U kunt toegang tot resources in een virtueel netwerk van een functie wordt uitgevoerd met behulp van de integratie van virtuele netwerken. Zie voor meer informatie, [integratie van virtuele netwerken](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hoe krijg ik toegang tot bronnen worden beveiligd door de service-eindpunten?

Met behulp van integratie van virtuele netwerken (momenteel in preview), kunt u toegang tot de service-eindpunt-beveiligde resources van een actieve functie. Zie voor meer informatie, [Preview van de integratie van virtueel netwerk](functions-networking-options.md#preview-version-of-virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>Hoe kan ik een functie van een resource in een virtueel netwerk activeren?

U kunt een functie van een resource in een virtueel netwerk activeren alleen door uw functie-app implementeren in een App Service Environment. Zie voor meer informatie over het gebruik van een App Service Environment [maken en gebruiken van een interne load balancer met een App Service Environment](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>Hoe kan ik mijn functie-app in een virtueel netwerk implementeren?

Implementeren naar een App Service Environment is de enige manier om een functie-app die is volledig binnen een virtueel netwerk maken. Beginnen met het artikel voor meer informatie over het gebruik van een interne load balancer met een App Service Environment [maken en gebruiken van een interne load balancer met een App Service Environment](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Zie voor scenario's waarbij u slechts één richting toegang tot virtuele-netwerkbronnen of minder uitgebreide netwerkisolatie moet de [netwerken overzicht van Functions](functions-networking-options.md).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over netwerken en -functies: 

* [Volg de zelfstudie over aan de slag met integratie van virtuele netwerken](./functions-create-vnet.md)
* [Meer informatie over de netwerkopties van Azure Functions](./functions-networking-options.md)
* [Meer informatie over de integratie van virtueel netwerk met App Service en Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Meer informatie over virtuele netwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Inschakelen van meer netwerken functies en controle met App Service-omgevingen](../app-service/environment/intro.md)
