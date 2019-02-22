---
title: Software-updates van Microsoft in Azure Stack-validatie als een Service valideren | Microsoft Docs
description: Informatie over het valideren van software-updates van Microsoft met validatie als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/14/2019
ROBOTS: NOINDEX
ms.openlocfilehash: b7fa03cdf52fc3218e9556c9664daafdc60243f3
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593214"
---
# <a name="validate-software-updates-from-microsoft"></a>Valideren van software-updates van Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft brengt regelmatig updates voor de Azure Stack-software. Deze updates zijn bedoeld om Azure Stack coengineering partners. De updates zijn opgegeven in vooraf van openbaar beschikbaar. U kunt de updates op basis van uw oplossing controleren en feedback naar Microsoft.

Software-updates van Microsoft met Azure Stack worden aangeduid met behulp van een naamconventie geldt, bijvoorbeeld voor maart 2018 1803 die wijzen op de update is. Voor meer informatie over het beleid voor Azure Stack update uitgebracht en release-opmerkingen beschikbaar zijn, Zie [Azure Stack servicebeleid](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

## <a name="prerequisites"></a>Vereisten

Voordat u zich op de maandelijkse updateproces in VaaS, moet u bekend bent met de volgende items zijn:

- [Validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md)
- [Functie voor interactieve verificatie testen](azure-stack-vaas-interactive-feature-verification.md)

## <a name="required-tests"></a>Vereiste tests

De volgende tests moeten worden uitgevoerd in de volgende volgorde voor maandelijkse software-validatie:

1. Maandelijks Azure Stack-Update-verificatie
2. Engine voor cloud-simulatie

## <a name="validating-software-updates"></a>Het valideren van software-updates

1. Maak een nieuwe **validatie van het pakket** werkstroom.
1. Volg de instructies uit voor de bovenstaande vereiste tests [test van het pakket niet valideren uitvoeren](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests). Zie de sectie hieronder voor meer informatie over de **maandelijkse Update verificatie van de Azure Stack** testen.

### <a name="apply-the-monthly-update"></a>De maandelijkse update van toepassing

1. Selecteer een agent voor het uitvoeren van tests tegen.
1. Planning **maandelijkse Azure Stack Update verificatie**.
1. Geef de locatie voor het OEM-uitbreidingspakket die momenteel zijn geïmplementeerd op het stempel en de locatie voor het pakket met de uitbreiding OEM die worden toegepast tijdens het bijwerken. Zie configureren van de URL's voor deze pakketten [beheren van pakketten voor validatie](azure-stack-vaas-validate-oem-package.md#managing-packages-for-validation).
1. Volg de stappen in de gebruikersinterface van de geselecteerde agent.

Als u vragen of opmerkingen hebt, kunt u contact op met [VaaS Help](mailto:vaashelp@microsoft.com).

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)