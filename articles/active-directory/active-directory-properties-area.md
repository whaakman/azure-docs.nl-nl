---
title: Privacy-informatie van uw organisatie toevoegen in Azure AD | Microsoft Docs
description: Legt uit hoe privacy-informatie van uw organisatie toevoegen aan het gedeelte Eigenschappen van Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: 8cdf30ed09601a31529073eaedd4ab53780157d5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Instructies: De privacy-informatie van uw organisatie in Azure Active Directory toevoegen
Dit artikel wordt uitgelegd hoe een tenantbeheerder informatie met betrekking tot privacy kunt toevoegen aan een organisatie Azure Active Directory (Azure AD) tenant via de Azure portal.

Wij raden dat u toevoegen, zowel uw contactpersoon globale privacy en de privacyverklaring van uw organisatie, zodat uw interne werknemers en externe gasten voor uw beleid bekijken kunnen. Omdat privacyverklaringen uniek zijn gemaakt en is aangepast voor elk bedrijf, wordt aangeraden u contact opneemt met een advocaat voor hulp.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Toegang tot het gedeelte Eigenschappen van de gegevens van uw privacy toevoegen

1.  Meld u aan bij de Azure portal als tenantbeheerder.

2.  Selecteer op de navigatiebalk links, **Azure Active Directory**, en selecteer vervolgens **eigenschappen**.

    De **eigenschappen** gebied wordt weergegeven.

    ![Azure AD-eigenschappengebied markering van het gebied van privacy-informatie](./media/active-directory-properties-area/properties-area.png)

3.  Uw privacy-informatie voor uw werknemers toevoegen:

    - **Technische contactpersoon.** Typ de e-mailadres voor de contactpersoon voor voor technische ondersteuning binnen uw organisatie.
    
    - **Neem contact op met globale privacy.** Typ de e-mailadres voor de contactpersoon voor voor vragen over privacy van persoonlijke gegevens. Deze persoon is ook die contact op met Microsoft als er een schending van de gegevens. Als er geen persoon die hier worden vermeld, contact op met Microsoft uw globale beheerders.

    - **URL van privacyverklaring.** Typ de koppeling naar uw organisatie-document dat wordt beschreven hoe uw organisatie omgaat met zowel interne en externe van Gast gegevensprivacy.

        >[!Important]
        >Als u niet uw eigen privacyverklaring of uw privacy contact opneemt, ziet uw externe gasten tekst in de **Controleer machtigingen** vak waarin wordt gemeld,  **< _de naam van uw organisatie_> is niet opgegeven voor koppelingen naar hun voorwaarden voor u deze kunt bekijken**. Bijvoorbeeld, ziet een gastgebruiker dit bericht wanneer ze een uitnodiging ontvangen voor een organisatie toegang via B2B-samenwerking.

        ![B2B-samenwerking controleren machtigingen in met het bericht](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen
- [Azure Active Directory B2B-samenwerking uitnodiging inwisseling](https://aka.ms/b2bredemption)
- [Toevoegen of wijzigen van de informatie over het profiel voor een gebruiker in Azure Active Directory](/active-directory-users-profile-azure-portal.md)