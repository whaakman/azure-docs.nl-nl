---
title: In aanmerking komende toewijzingen en de zichtbaarheid van de resource voor Azure in Privileged Identity Management | Microsoft Docs
description: Beschrijft hoe leden naar in aanmerking komen voor de resourcerollen toewijzen wanneer u PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 205b4f3113f369279dbe18e75b5945a0498e7bbd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260376"
---
# <a name="eligible-assignments-and-resource-visibility-with-privileged-identity-management"></a>In aanmerking komende toewijzingen en de zichtbaarheid van de resource met Privileged Identity Management

Privileged Identity Management (PIM) voor Azure-resource functies biedt betere beveiliging voor organisaties met essentiële Azure-resources. Resource-beheerders kunnen PIM gebruiken voor het toewijzen van leden als in aanmerking komen voor de resourcerollen. Meer informatie over de toewijzing van verschillende typen en toewijzing van statussen voor Azure-resource-functies in de volgende secties. 

## <a name="assignment-types"></a>Toewijzing van typen

PIM voor Azure-resources biedt twee afzonderlijke toewijzing typen:

- In aanmerking komend
- Actief

In aanmerking komende toewijzingen vereisen het lid van de rol van een actie voor het gebruik van de rol uit te voeren. Acties mogelijk zijn een controle meerledige verificatie slaagt, kan een zakelijke rechtvaardiging bieden of aanvragen van goedkeuring van aangewezen fiatteurs.

Actieve toewijzingen vereisen niet het lid een actie voor het gebruik van de rol uit te voeren. Leden toegewezen als actief hebben de bevoegdheden die zijn toegewezen aan de rol te allen tijde.

## <a name="assignment-duration"></a>Duur van de toewijzing

Resource-beheerders kunnen kiezen uit twee opties voor elk toewijzingstype wanneer ze PIM-instellingen voor een functie configureert. De maximale standaardduur wanneer een lid aan de PIM-rol wordt toegewezen, worden deze opties. 

Een beheerder kunt kiezen uit een van deze typen toewijzing:

- Permanent in aanmerking komende toewijzing toestaan
- Permanente actieve toewijzing toestaan

Of een beheerder kan een van deze typen toewijzing kiezen:

- Verlopen van in aanmerking komende toewijzingen na
- Actieve toewijzingen laten verlopen na

Als een resourcebeheerder ervoor kiest **toestaan permanente toewijzing van in aanmerking komende** of **permanente actieve toewijzing toestaan**, alle beheerders die leden aan de bron toewijzen permanent kunnen toewijzen te beheren.

Als een resourcebeheerder ervoor kiest **verlopen in aanmerking komende toewijzingen na** of **verlopen active toewijzingen na**, de resourcebeheerder bepaalt de levenscyclus van de toewijzing door te vereisen dat alle toewijzingen hebben een opgegeven begin- en datum.

> [!NOTE] 
> Alle toewijzingen waarvoor een opgegeven einddatum kunnen worden vernieuwd door beheerders van de resource. Bovendien leden Self-service aanvragen om te kunnen initiëren [verlengen of vernieuwen toewijzingen](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Toewijzing van statussen

PIM voor Azure-resources heeft twee verschillende toewijzing statussen die worden weergegeven op de **Active rollen** tabblad de **mijn rollen**, **rollen**, en **leden**weergaven van PIM. Deze statussen zijn:

- Toegewezen
- Geactiveerd

Wanneer u een lidmaatschap dat wordt vermeld in bekijkt **Active rollen**, kunt u de waarde in de **status** kolom onderscheid maken tussen gebruikers die zijn **toegewezen** als actief en gebruikers die **geactiveerd** een in aanmerking komende toewijzing en er wordt nu actief.

## <a name="next-steps"></a>Volgende stappen

[Toewijzen van rollen in Privileged Identity Manager](pim-resource-roles-assign-roles.md)
