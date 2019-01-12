---
title: Aanbevolen procedures voor de Azure Stack-validatie. | Microsoft Docs
description: In dit artikel vindt aanbevolen procedures voor validatie als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: 8b7d2b6d2aef4567197fe18607cb50d1e9a4206b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247235"
---
# <a name="best-practices-for-validation-as-a-service"></a>Aanbevolen procedures voor validatie als een Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

In dit artikel bevat informatie over aanbevolen procedures voor het beheren van resources in de validatie als een Service (VaaS). Zie voor een overzicht van VaaS resources [validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Voor oplossingsbeheer

### <a name="naming-convention-for-vaas-solutions"></a>Naamconventie voor VaaS oplossingen

Gebruik een consistente naamgeving voor alle oplossingen die zijn geregistreerd in VaaS. Bijvoorbeeld: naam van de oplossing kan worden samengesteld uit de hardware-eigenschappen hieronder als volgt:

|Productnaam | Unieke Hardware-Element 1 | Unieke Hardware-Element 2 | De naam van oplossing
|---|---|---|---|
Mijn XYZ-oplossing |  Alle Flash | Mijn Switch X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Wanneer u een nieuwe VaaS-oplossing maken

Gebruik de dezelfde VaaS-oplossing met het uitvoeren van werkstromen in dezelfde hardware SKU. Een nieuwe VaaS-oplossing moet worden gemaakt, alleen wanneer er een wijziging in de hardware-SKU.

## <a name="workflow-management"></a>Werkstroombeheer

### <a name="naming-convention-for-vaas-workflows"></a>Naamconventie voor VaaS werkstromen

Gebruik een consistente naamgeving voor alle VaaS werkstroom wordt uitgevoerd. Bijvoorbeeld, om voor te bereiden een werkstroom een naam van de onderstaande build-eigenschappen als volgt:

|Build-nummer (ernstig) | Date | Grootte van oplossing | Werkstroomnaam
|---|---|---| ---|
1808 | 081518 | 4NODE | 1808_081518_4NODE

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [validatie uit als een Service-belangrijkste concepten](azure-stack-vaas-key-concepts.md)
