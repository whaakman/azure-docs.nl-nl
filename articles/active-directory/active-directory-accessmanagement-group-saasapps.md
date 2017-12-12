---
title: Toegang tot SaaS-toepassingen beheren met behulp van een groep | Microsoft Docs
description: "Het gebruik van groepen in Azure Active Directory Premium of Basic toewijzen van toegang tot SaaS-toepassingen die zijn geïntegreerd met Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 49a2c86516f0882f341597876d2a44ea3a312ea8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Een groep gebruiken om SaaS-toepassingen te beheren
Met Azure Active Directory (Azure AD) met een licentie voor Azure AD Premium of Azure AD Basic, kunt u groepen gebruiken voor het toewijzen van toegang tot een SaaS-toepassing die geïntegreerd met Azure AD. Bijvoorbeeld, als u toegang toewijzen voor de marketingafdeling vijf verschillende SaaS-toepassingen gebruiken wilt, kunt u een groep met de gebruikers van de marketingafdeling maken en vervolgens die groep toewijzen aan deze vijf SaaS-toepassingen die nodig zijn voor de marketingafdeling. Op deze manier kunt u tijd besparen door het lidmaatschap van de marketingafdeling op één plek beheren. Gebruikers zijn vervolgens toegewezen aan de toepassing wanneer ze zijn toegevoegd als leden van de groep marketing en hun toewijzingen verwijderd uit de toepassing wanneer ze zijn verwijderd uit de groep marketing. Deze mogelijkheid kan worden gebruikt met honderden toepassingen die u uit binnen de Azure AD-Toepassingsgalerie kunt toevoegen.

> [!IMPORTANT]
> U kunt deze functie alleen gebruiken nadat u een proefversie van Azure AD Premium start of Azure AD Premium of Azure AD Basic licenties koopt.
> Op dit moment wordt genest groepslidmaatschap niet ondersteund voor toewijzing aan een toepassing op basis van een groep.

**Toegang voor een gebruiker of groep toewijzen aan een SaaS-toepassing**

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **bedrijfstoepassingen**.
2. Selecteer een toepassing die u hebt toegevoegd uit de galerie met toepassingen om deze te openen.
3. Selecteer **gebruikers en groepen**, en selecteer vervolgens **gebruiker toevoegen**.
4. Op **toevoegen toewijzing**, selecteer **gebruikers en groepen** openen de **gebruikers en groepen** selectielijst.
6. Selecteer zo veel groepen of gebruikers als u wilt, klikt u vervolgens klik of tik **Selecteer** om toe te voegen aan de **toevoegen toewijzing** lijst. U kunt ook een rol toewijzen aan een gebruiker in deze fase.
7. Selecteer **toewijzen** de gebruikers of groepen toewijzen aan de geselecteerde bedrijfstoepassing.

### <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
