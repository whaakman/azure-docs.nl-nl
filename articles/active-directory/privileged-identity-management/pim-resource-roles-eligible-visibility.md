---
title: In aanmerking komende toewijzingen en de zichtbaarheid van resources in PIM - Azure | Microsoft Docs
description: Beschrijft hoe leden als in aanmerking voor Azure-resource-rollen in Azure AD Privileged Identity Management (PIM) toewijzen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3551c3231a94f8a844d26a713cbf171ca7653815
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189211"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>In aanmerking komende toewijzingen en de zichtbaarheid van resources in PIM

Privileged Identity Management (PIM) voor Azure-resourcerollen biedt verbeterde beveiliging voor organisaties die essentiële Azure-resources hebben. Resource-beheerders kunnen PIM gebruiken voor het toewijzen van leden als in aanmerking komen voor de resource-rollen. Meer informatie over de van verschillende toewijzingstypen en de toewijzing voor de Azure-resource in de volgende secties. 

## <a name="assignment-types"></a>Toewijzingstypen

PIM voor Azure-resources biedt twee afzonderlijke toewijzingstypen:

- In aanmerking komend
- Actief

In aanmerking komende toewijzingen moeten het lid van de rol van een actie voor het gebruik van de rol uit te voeren. Acties advies inwinnen bij een controle van de multi-factor authentication slaagt, een zakelijke rechtvaardiging te geven of aanvragen van goedkeuring van fiatteurs.

Directe toewijzingen nodig geen voor het lid aan een actie voor het gebruik van de rol wordt uitgevoerd. Leden toegewezen als actief hebben de bevoegdheden die zijn toegewezen aan de rol te allen tijde.

## <a name="assignment-duration"></a>Duur van de toewijzing

Resource-beheerders kunnen kiezen uit twee opties voor elk toewijzingstype wanneer ze PIM-instellingen voor een functie configureert. Deze opties worden de standaard maximale duur voor wanneer een lid is toegewezen aan de rollen in PIM. 

Een beheerder kunt kiezen uit een van deze toewijzingstypen:

- Permanent in aanmerking komende toewijzing toestaan
- Permanente actieve toewijzing toestaan

Of een beheerder kan een van deze toewijzingstypen kiezen:

- In aanmerking komende toewijzingen na verlopen
- Actieve toewijzingen laten verlopen na

Als de resourcebeheerder van een kiest **permanent in aanmerking komende toewijzing toestaan** of **permanente actieve toewijzing toestaan**, alle beheerders die leden aan de resource toewijzen permanent kunnen toewijzen te beheren.

Als de resourcebeheerder van een kiest **verlopen van in aanmerking komende toewijzingen na** of **actieve toewijzingen na verlopen**, beheerder van de resource beheert de levenscyclus van de toewijzing door te vereisen dat alle toewijzingen van hebben een opgegeven begin- en datum.

> [!NOTE] 
> Alle toewijzingen van met een opgegeven datum kunnen worden vernieuwd door beheerders van de resource. Bovendien leden Self-service aanvragen om te kunnen initiëren [verlengen of vernieuwen toewijzingen](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Toewijzing van statussen

PIM voor Azure-resources heeft twee verschillende toewijzing statussen die worden weergegeven op de **actieve rollen** tabblad de **mijn rollen**, **rollen**, en **leden**weergaven van PIM. Deze statussen zijn:

- Toegewezen
- Geactiveerd

Wanneer u een lidmaatschap dat wordt vermeld in bekijkt **actieve rollen**, kunt u de waarde in de **status** kolom onderscheid maken tussen de gebruikers die zijn **toegewezen** als actief en gebruikers die **geactiveerd** een in aanmerking komende toewijzing en zijn nu actief.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM toewijzen](pim-resource-roles-assign-roles.md)
