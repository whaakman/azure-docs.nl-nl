---
title: Toegewezen groepen in Azure Active Directory | Microsoft Docs
description: Overzicht van hoe toegewezen groepen werken in Azure Active Directory en hoe ze worden gemaakt.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;oldportal
ms.openlocfilehash: 992f4563064d7a292cf4fdd90a9a3c84cdec91c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Toegewezen groepen in Azure Active Directory
In Azure Active Directory (Azure AD), de functie toegewezen groepen maakt en automatisch gevuld lidmaatschap voor groepen van Azure AD zijn vooraf gedefinieerd. Leden van specifieke groepen kan niet worden toegevoegd of verwijderd met behulp van de klassieke Azure-portal, de Windows PowerShell-cmdlets of via een programma.

> [!NOTE]
> Toegewezen groepen moeten een Azure AD Premium-licentie is toegewezen aan
>
> * De beheerder van de regel voor een groep
> * alle gebruikers die zijn geselecteerd door de regel moet lid zijn van de groep
>
>

**Toegewezen groepen inschakelen**

1. In de [Azure-portal](https://portal.azure.com), selecteer **Active Directory**, en open vervolgens de directory van uw organisatie.
2. Selecteer de **groepen** tabblad en open vervolgens de groep die u wilt bewerken.
3. Selecteer de **configureren** tabblad en stel vervolgens **toegewezen groepen inschakelen** naar **Ja**.

Zodra de switch toegewezen groepen inschakelen is ingesteld op **Ja**, verder kunt u de map voor het automatisch maken van de speciale groep alle gebruikers door in te stellen de **inschakelen 'Alle gebruikers' groep** overschakelen naar  **Ja**. U kunt vervolgens ook bewerken de naam van deze groep toegewezen door te typen in de **weergavenaam voor 'Alle gebruikers' groep** veld.

De groep alle gebruikers kan worden gebruikt dezelfde machtigingen toewijzen aan alle gebruikers in uw directory. Bijvoorbeeld, kunt u alle gebruikers in uw directory toegang tot een SaaS-toepassing verlenen toegang voor de speciale groep alle gebruikers toewijzen aan deze toepassing.

De speciale groep alle gebruikers bevat alle gebruikers in de directory, inclusief gasten en externe gebruikers. Als u een groep moet op die externe gebruikers worden uitgesloten en u kunt dit doen door het maken van een groep met een kenmerk op basis van een dynamische regel zoals de volgende:

                (user.userPrincipalName -notContains "#EXT#@")

Voor een groep die omvat niet alle gasten, gebruikt u een regel, zoals de volgende:

                (user.userType -ne "Guest")

Zie [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md) (Engelstalig) voor meer informatie over het maken van *geavanceerde* regels (regels met meerdere vergelijkingen).

### <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [What is Azure Active Directory?](active-directory-whatis.md) (Wat is Azure Active Directory?)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
