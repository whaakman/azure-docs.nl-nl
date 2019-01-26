---
title: B2B-samenwerking gebruiker claims toewijzen in Azure Active Directory | Microsoft Docs
description: De gebruikersclaims die zijn uitgegeven in het SAML-token voor Azure Active Directory (Azure AD) B2B-gebruikers aanpassen.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: dd6be92bcd519dac369c5c21bb57b0b64ef6bfd1
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075949"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>B2B-samenwerking gebruiker claims toewijzen in Azure Active Directory

Azure Active Directory (Azure AD) biedt ondersteuning voor aanpassen van de claims die zijn uitgegeven in het SAML-token voor gebruikers van B2B-samenwerking. Wanneer een gebruiker wordt geverifieerd op de toepassing, geeft Azure AD een SAML-token aan de app die gegevens (of claims) bevat over de gebruiker die een unieke identificatie ze. Dit omvat standaard, gebruikersnaam, e-mailadres, voornaam en achternaam van de gebruiker.

In de [Azure-portal](https://portal.azure.com), u kunt weergeven of bewerken van de claims die in het SAML-token naar de toepassing worden verzonden. Voor toegang tot de instellingen, selecteer **Azure Active Directory** > **bedrijfstoepassingen** > de toepassing die geconfigureerd voor eenmalige aanmelding > **eenmalige aanmelding** . Zie de instellingen voor de SAML-token in de **gebruikerskenmerken** sectie.

![Bevat de kenmerken van SAML-token in de gebruikersinterface](media/claims-mapping/view-claims-in-saml-token.png)

Er zijn twee mogelijke redenen waarom u mogelijk de claims die zijn uitgegeven in het SAML-token bewerken:

1. De toepassing vereist een andere set claimen van URI's of claimwaarden.

2. De toepassing moet de claim NameIdentifier dan de UPN (user Principal name) die zijn opgeslagen in Azure AD.

Zie voor meer informatie over het toevoegen en bewerken van claims [in het SAML-token voor bedrijfstoepassingen in Azure Active Directory wordt uitgegeven claims aanpassen](../develop/active-directory-saml-claims-customization.md).

B2B-samenwerking mogelijk gebruikers, cross-tenant-NameID en UPN-toewijzing niet uit veiligheidsoverwegingen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de eigenschappen van gebruikers van B2B-samenwerking [eigenschappen van de gebruiker van een Azure Active Directory B2B-samenwerking](user-properties.md).
- Zie voor meer informatie over gebruikerstokens voor gebruikers van B2B-samenwerking [begrijpen gebruikerstokens in Azure AD B2B-samenwerking](user-token.md).

