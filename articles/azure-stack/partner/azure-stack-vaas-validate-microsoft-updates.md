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
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 7fcc7d5a1d87fe93d32772dbbb84f1d3c91d5631
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648779"
---
# <a name="validate-software-updates-from-microsoft"></a>Valideren van software-updates van Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft brengt regelmatig updates voor de Azure Stack-software. Deze updates worden aan Azure Stack-partners CO engineering voorafgaand aan openbaar beschikbaar wordt gesteld zodat ze kunnen de updates op basis van hun oplossingen te valideren en feedback naar Microsoft verstrekt.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Maandelijkse update van toepassing

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Een werkstroom maken

Update validaties gebruiken dezelfde werkstroom als **validatie van het pakket**. Volg de instructies op [maken van een werkstroom voor validatie van het pakket](azure-stack-vaas-validate-oem-package.md#create-a-package-validation-workflow).

## <a name="run-tests"></a>Tests uitvoeren

Update validaties gebruiken dezelfde werkstroom als **validatie van het pakket**. Volg de instructies op [uitvoeren van het pakket niet valideren tests](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests).

U hoeft niet om aan te vragen van pakket zich voor validaties van de update.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)
