---
title: 'Overzicht: Het uitvoeren van een onderzoek toegang in Privileged Identity Management voor Azure-resources | Microsoft Docs'
description: Dit document wordt beschreven hoe u een revisie toegang uitvoert in PIM voor Azure-resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 61be9873cac462c096599680a6e071e104f3a54c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Een resource-dashboard gebruiken om uit te voeren een controle toegang

U kunt een resource-dashboard gebruiken om uit te voeren een controle toegang in Privileged Identity Management (PIM) voor Azure-resources. Het dashboard Admin-weergave bevat drie primaire onderdelen:

- Een grafische weergave van de resource rol activeringen.
- Twee diagrammen die de distributie van roltoewijzingen door toewijzingstype weergeven.
- Een gegevensgebied met betrekking tot de toewijzing van nieuwe functies.

![Schermopname van het dashboard Admin weergave van grafieken en diagrammen](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Schermopname van het dashboard Admin weergeven die van een lijst met gegevens](media/azure-pim-resource-rbac/role-settings.png)

De grafische weergave van de resource rol activeringen bevat informatie over de afgelopen zeven dagen. Deze gegevens is afgestemd op de geselecteerde resource en geeft weer activeringen voor de meest voorkomende rollen (eigenaar, bijdrager, beheerder voor gebruikerstoegang), en voor alle functies die worden gecombineerd.

Aan de rechterkant van de grafiek activeringen weergeven twee grafieken de distributie van roltoewijzingen toewijzingstype voor gebruikers en groepen. U kunt de waarde in een percentage (of andersom) wijzigen door het selecteren van een segment van de grafiek.

U ziet het aantal gebruikers en groepen met nieuwe roltoewijzingen gedurende de afgelopen 30 dagen en een lijst met rollen gesorteerd op basis van totale toewijzingen (in aflopende volgorde) onder de grafieken.


