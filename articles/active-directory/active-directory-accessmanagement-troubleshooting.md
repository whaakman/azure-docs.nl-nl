---
title: Het oplossen van problemen dynamisch lidmaatschap voor groepen | Microsoft Docs
description: Tips voor probleemoplossing voor dynamisch lidmaatschap voor groepen in Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 6d8d04273e9f29b2634c8b77b0268f3c7b77b1e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Oplossen van problemen met dynamische lidmaatschappen voor groepen
**Ik een regel voor een groep hebt geconfigureerd, maar geen lidmaatschappen worden bijgewerkt in de groep**<br/>Controleer de waarden voor kenmerken van de gebruiker op de regel: Er zijn van gebruikers die voldoen aan de regel? Als alles er goed uitziet, wacht u even totdat de groep te vullen. Afhankelijk van de grootte van uw tenant, kan het de eerste keer of na een regelwijziging tot 24 uur duren voordat de groep is ingevuld.

**Ik een regel hebt geconfigureerd, maar nu de bestaande leden van de regel zijn verwijderd**<br/>Dit is verwacht gedrag. Bestaande leden van de groep worden verwijderd wanneer een regel is ingeschakeld of gewijzigd. De gebruikers van de evaluatieversie van de regel wordt geretourneerd worden als leden toegevoegd aan de groep.     

**Ik zie niet met het lidmaatschap wordt gewijzigd direct als ik toevoegen of wijzigen van een regel, waarom niet?**<br/>Evaluatie toegewezen lidmaatschap wordt periodiek uitgevoerd in een asynchrone achtergrondproces. Hoe lang duurt het proces wordt bepaald door het aantal gebruikers in uw directory en de grootte van de groep gemaakt als gevolg van de regel. Mappen met een klein aantal gebruikers ziet normaal gesproken de wijzigingen voor een groepslidmaatschap in minder dan een paar minuten. Mappen met een groot aantal gebruikers kunnen duren voordat de 30 minuten of langer te vullen.

### <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [What is Azure Active Directory?](active-directory-whatis.md) (Wat is Azure Active Directory?)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
