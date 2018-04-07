---
title: Privileged Identity Management voor Azure-Resources - toewijzingen van in aanmerking komende en zichtbaarheid | Microsoft Docs
description: Beschrijft hoe leden naar in aanmerking komende aan resourcerollen toewijzen.
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
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 273b06c91d68a764fe814374c0eca6ed1698cc2e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="eligible-assignments-and-resource-visibility"></a>In aanmerking komende toewijzingen en de zichtbaarheid van de resource

PIM voor Azure-resource functies biedt betere beveiliging voor organisaties met essentiële Azure resoruces. PIM biedt resource beheerders de mogelijkheid leden als in aanmerking komende toewijzen aan resourcerollen. Lees meer over de toewijzing van verschillende typen en statussen voor Azure-resourcerollen hieronder. 

## <a name="assignment-types"></a>Toewijzing van typen

PIM voor Azure-resources biedt twee afzonderlijke toewijzing typen:

- In aanmerking komend
- Actief

In aanmerking komende toewijzingen vereisen het lid van de rol van een actie voor het gebruik van de rol uit te voeren. Deze acties bevatten een controle van de multi-factor Authentication slaagt, bieden een reden en vragen goedkeuring van aangewezen fiatteurs.

Actieve toewijzingen vereisen niet het lid een actie voor het gebruik van de rol uit te voeren. Leden als actief hebben de bevoegdheden die zijn opgegeven door de rol te allen tijde.

## <a name="assignment-duration"></a>Duur van de toewijzing

Resource-beheerders kunnen een van twee opties voor elk Type toewijzing kiezen bij het configureren van de PIM-instellingen voor een rol. De maximale standaardduur wanneer een lid aan de PIM-rol wordt toegewezen, worden deze opties.

- Permanente in aanmerking komende toewijzing toestaan
- Permanente actieve toewijzing toestaan

of

- Verlopen van in aanmerking komende toewijzingen na
- Actieve toewijzingen na het verlopen

Als de resourcebeheerder van een kiest voor 'Toestaan permanente in aanmerking komende toewijzing' en/of 'Toestaan permanente actieve toewijzing', alle beheerders die leden aan de resource toewijzen hebt de mogelijkheid om toe te wijzen permanente lidmaatschappen.

Kiezen 'Verloopt in aanmerking komende toewijzingen na' en/of 'Active toewijzingen voor verloopt na' controle over de levenscyclus van de toewijzing kunt door te vereisen alle toewijzingen van een opgegeven begin- en datum.

>[!NOTE] 
>Alle toewijzingen met een opgegeven einddatum kunnen worden vernieuwd door resource beheerders en leden Self-service aanvragen om te kunnen initiëren [verlengen of vernieuwen toewijzingen](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Toewijzing van statussen

PIM voor Azure-resources heeft twee verschillende toewijzing statussen die worden weergegeven op het tabblad 'Active rollen' in mijn rollen, functies en leden weergaven van PIM. Deze statussen zijn:

- Toegewezen
- Geactiveerd

Wanneer een lidmaatschap weer te geven die worden vermeld in 'Active rollen' wordt de kolom 'Status' kunt u onderscheid maken tussen de gebruikers die 'Toegewezen' als actief ten opzichte van de gebruikers die een in aanmerking komende toewijzing 'geactiveerd zijn' en zijn nu actief.

## <a name="next-steps"></a>Volgende stappen

[Toewijzen van rollen in PIM](pim-resource-roles-assign-roles.md)

