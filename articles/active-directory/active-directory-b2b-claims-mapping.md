---
title: B2B-samenwerking gebruikersclaims toewijzen in Azure Active Directory | Microsoft Docs
description: De claims van de gebruiker die zijn uitgegeven in het SAML-token voor Azure Active Directory (Azure AD) B2B gebruikers aanpassen.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/06/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 8f5e471d4e7102300cd5581976b45c9fa8cc57bc
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>B2B-samenwerking gebruikersclaims toewijzen in Azure Active Directory

Azure Active Directory (Azure AD) ondersteunt het aanpassen van de claims die zijn uitgegeven in het SAML-token voor B2B-samenwerking gebruikers. Wanneer een gebruiker zich bij de toepassing verifieert, verstrekt Azure AD een SAML-token aan de app die informatie (of claims) bevat over de gebruiker die uniek wordt geïdentificeerd. Dit omvat standaard gebruikersnaam, e-mailadres, de voornaam en achternaam van de gebruiker.

In de [Azure-portal](https://portal.azure.com), kunt u weergeven of bewerken van de claims die worden verzonden in het SAML-token naar de toepassing. Voor toegang tot de instellingen, selecteer **Azure Active Directory** > **bedrijfstoepassingen** > de toepassing die geconfigureerd voor eenmalige aanmelding > **eenmalige aanmelding** . Zie de SAML-token-instellingen in de **gebruikerskenmerken** sectie.

![Geeft de kenmerken van SAML-token in de gebruikersinterface](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Er zijn twee mogelijke redenen waarom moet u mogelijk de claims die zijn uitgegeven in het SAML-token bewerken:

1. De toepassing vereist een andere set claimen van URI's of claimwaarden.

2. De toepassing vereist dat de NameIdentifier claim iets anders dan de UPN (user Principal name) die opgeslagen in Azure AD.

Zie voor meer informatie over het toevoegen en bewerken van claims [uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure Active Directory aanpassen](develop/active-directory-saml-claims-customization.md).

Voor B2B-samenwerking, gebruikers, toewijzing NameID en UPN cross-tenant worden uit veiligheidsoverwegingen voorkomen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over B2B-samenwerking gebruikerseigenschappen [eigenschappen van een gebruiker met Azure Active Directory B2B-samenwerking](active-directory-b2b-user-properties.md).
- Zie voor meer informatie over de gebruikerstokens voor B2B-samenwerking gebruikers [begrijpen gebruikerstokens in Azure AD B2B-samenwerking](active-directory-b2b-user-token.md).

