---
title: 'Problemen oplossen: ''Active Directory'' item is ontbreekt of is niet beschikbaar | Microsoft Docs'
description: Wat te doen wanneer de menuopdracht Active Directory niet wordt weergegeven in de Azure-beheerportal.
services: active-directory
documentationcenter: na
author: bryanla
manager: mtillman
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: 27a884b89224ed5765e9fcef134e711e3952ff72
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Problemen oplossen: 'Active Directory' item is ontbreekt of is niet beschikbaar
Veel van de instructies voor het gebruik van Azure Active Directory-functies en services beginnen met ' Ga naar de Azure-beheerportal en klik **Active Directory**. " Wat doet u als het Active Directory-extensie of menu-item niet wordt weergegeven of als deze is gemarkeerd, maar **niet beschikbaar**? Dit onderwerp is bedoeld om u te helpen. Beschrijft de omstandigheden waaronder **Active Directory** niet wordt weergegeven of is niet beschikbaar en wordt uitgelegd hoe u om door te gaan.

## <a name="active-directory-is-missing"></a>Active Directory ontbreekt
Normaal gesproken een **Active Directory** item wordt weergegeven in het navigatiemenu links. De instructies in Azure Active Directory-procedures wordt ervan uitgegaan dat dit item in de weergave.

![Schermopname: Active Directory in Azure](./media/active-directory-troubleshooting/typical-view.png)

Het Active Directory-item wordt weergegeven in het navigatiemenu links wanneer elk van de volgende voorwaarden is waar. Anders wordt weergegeven het item niet.

* De huidige gebruiker aangemeld met een Microsoft-account (voorheen bekend als een Windows Live ID).
  
    OF
* De Azure-tenant heeft een map en het huidige account is een directory-beheerder.
  
    OF
* De Azure-tenant heeft ten minste één Azure AD Access Control (ACS)-naamruimte. Zie voor meer informatie [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    OF
* De Azure-tenant heeft ten minste één Azure multi-factor Authentication-provider. Zie voor meer informatie [Azure multi-factor Authentication-Providers beheren](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Klik op om een Access Control-naamruimte of een multi-Factor Authentication-provider **+ nieuw** > **App Services** > **Active Directory**.

Als u beheerdersrechten voor een map, hebt u een beheerder een beheerdersrol toewijzen aan uw account. Zie voor meer informatie [beheerdersrollen toewijzen](active-directory-assign-admin-roles-azure-portal.md).

## <a name="active-directory-is-not-available"></a>Active Directory is niet beschikbaar
Wanneer u klikt op **+ nieuw** > **App Services**, een **Active Directory** item wordt weergegeven. De Active Directory-item wordt weergegeven in het bijzonder voor wanneer een van de Active Directory-functies, zoals de map, Access Control of multi-factor Authentication-Provider, beschikbaar voor de huidige gebruiker zijn.

Echter tijdens het laden van de pagina is het item wordt grijs weergegeven en is gemarkeerd als **niet beschikbaar**. Dit is een tijdelijke status. Als u een paar seconden wacht, wordt het item beschikbaar. Als de vertraging wordt verlengd, vernieuwen van de webpagina vaak het probleem is opgelost.

![Schermopname: Active Directory is niet beschikbaar](./media/active-directory-troubleshooting/not-available.png)

