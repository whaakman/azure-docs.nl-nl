---
title: Met behulp van een groep voor het beheren van toegang tot SaaS-toepassingen - Azure Active Directory | Microsoft Docs
description: Het gebruik van groepen in Azure Active Directory toegang toewijzen tot SaaS-toepassingen die kunnen worden geïntegreerd met Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 873863016d6dc54c7439ec1f46180b3c063bda19
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173623"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Een groep gebruiken om SaaS-toepassingen te beheren

Met behulp van Azure Active Directory (Azure AD) met een Azure AD Premium of Azure AD Basic-licentie, kunt u groepen gebruiken voor toegang toewijzen tot een SaaS-toepassing die geïntegreerd met Azure AD. Bijvoorbeeld, als u toegang toewijzen voor de marketingafdeling vijf verschillende SaaS-toepassingen gebruiken wilt, kunt u een groep met de gebruikers van de marketingafdeling maken, en vervolgens die groep toewijzen aan deze vijf SaaS-toepassingen die nodig zijn voor de marketingafdeling. Op deze manier kunt u tijd besparen door het lidmaatschap van de marketingafdeling op één locatie beheren. Gebruikers worden vervolgens toegewezen aan de toepassing wanneer ze worden toegevoegd als leden van de groep marketing en hun toewijzingen verwijderd uit de toepassing wanneer ze worden verwijderd uit de groep marketing. Deze mogelijkheid kan worden gebruikt met honderden toepassingen die u uit binnen de Azure AD-Toepassingsgalerie kunt toevoegen.

> [!IMPORTANT]
> U kunt deze functie alleen gebruiken nadat u een proefversie van Azure AD Premium start of Azure AD Premium of Azure AD Basic-licenties koopt. Toewijzing op basis van een groep wordt alleen ondersteund voor beveiligingsgroepen. Op dit moment wordt genest groepslidmaatschap niet ondersteund voor toewijzing aan een toepassing op basis van een groep.

**Toegang voor een gebruiker of groep toewijzen aan een SaaS-toepassing**

1. In de [Azure AD-beheercentrum](https://aad.portal.azure.com), selecteer **bedrijfstoepassingen**.
2. Selecteer een toepassing die u hebt toegevoegd uit de galerie om het te openen.
3. Selecteer **gebruikers en groepen**, en selecteer vervolgens **gebruiker toevoegen**.
4. Op **toevoegen toewijzing**, selecteer **gebruikers en groepen** openen de **gebruikers en groepen** selectielijst.
6. Selecteer zo veel groepen of gebruikers als u wilt, klikt u op of tik vervolgens op **Selecteer** naar deze toevoegen aan de **toewijzing toevoegen** lijst. U kunt ook een rol toewijzen aan een gebruiker in deze fase.
7. Selecteer **toewijzen** de gebruikers of groepen toewijzen aan de geselecteerde enterprise-toepassing.

### <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Azure Active Directory cmdlets for configuring group settings](groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
