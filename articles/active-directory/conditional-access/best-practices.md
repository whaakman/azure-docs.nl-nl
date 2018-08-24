---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over dingen die u moet weten en wat dat inhoudt dat dient u dit te vermijden bij het configureren van beleid voor voorwaardelijke toegang.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4e9f5a9318db813b1a0f16d3599f74fd98e53ffc
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818054"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), u kunt bepalen hoe gemachtigde gebruikers toegang tot uw cloud-apps. In dit artikel vindt u informatie over:

- Dingen die u moet weten 
- Wat is Vermijd bij het configureren van beleid voor voorwaardelijke toegang. 

In dit artikel wordt ervan uitgegaan dat u bekend de concepten en termen die worden beschreven in [wat is voorwaardelijke toegang in Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>Wat is vereist voor het maken van een beleid werkt?

Wanneer u een nieuw beleid maakt, zijn er geen gebruikers, groepen, apps of besturingselementen voor toegang geselecteerd.

![Cloud-apps](./media/best-practices/02.png)


Als u uw beleid wilt werken, moet u het volgende configureren:


|Wat           | Hoe                                  | Waarom|
|:--            | :--                                  | :-- |
|**Cloud-apps** |U moet een of meer apps selecteren.  | Het doel van een beleid voor voorwaardelijke toegang is om te bepalen hoe gemachtigde gebruikers hebben toegang tot cloudapps.|
| **Gebruikers en groepen** | U moet ten minste één gebruiker of groep die is gemachtigd voor toegang tot uw geselecteerde cloud-apps selecteren. | Beleid voor voorwaardelijke toegang dat er geen gebruikers en groepen die zijn toegewezen, wordt nooit geactiveerd. |
| **Besturingselementen voor toegang** | U moet ten minste één toegangsbeheer selecteren. | Als uw voorwaarden wordt voldaan, moet de processor van uw beleid weten wat te doen.|




## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="how-are-assignments-evaluated"></a>Hoe worden toewijzingen geëvalueerd?

Alle toewijzingen zijn logisch **and**. Als u meer dan één toewijzing geconfigureerd hebt, moeten alle toewijzingen voor het activeren van een beleid worden voldaan.  

Als u nodig hebt om de locatievoorwaarde van een die van toepassing op alle verbindingen van buiten het netwerk van uw organisatie te configureren:

- Opnemen **alle locaties**
- Uitsluiten **alle vertrouwde IP-adressen**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Wat te doen als u toegang tot de Azure AD-beheerportal worden geblokkeerd?

Als u de Azure AD-portal vanwege een onjuiste instelling in een beleid voor voorwaardelijke toegang zijn vergrendeld:

- Controleer of er nog andere beheerders in uw organisatie die nog niet zijn geblokkeerd. Een beheerder met toegang tot de Azure portal kunt uitschakelen van het beleid dat is van invloed op uw aanmelding. 

- Als geen van de beheerders in uw organisatie het beleid bijwerken kan, moet u een ondersteuningsaanvraag indienen. Ondersteuning van Microsoft kunt controleren en bijwerken van beleid voor voorwaardelijke toegang waardoor toegang.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Wat gebeurt er als u beleidsregels in de klassieke Azure-portal en Azure-portal geconfigureerd hebt?  

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker toegang krijgt alleen als aan alle vereisten wordt voldaan.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Wat gebeurt er als u beleidsregels in de Intune Silverlight-portal en Azure portal hebt?

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker toegang krijgt alleen als aan alle vereisten wordt voldaan.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Wat gebeurt er als ik meerdere beleidsregels voor dezelfde gebruiker geconfigureerd?  

Voor elke aanmelding, Azure Active Directory evalueert alle beleidsregels en zorgt ervoor dat aan alle vereisten wordt voldaan voordat toegang verleend aan de gebruiker. Toegang blokkeren trumps alle andere configuratie-instellingen. 


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Werkt voorwaardelijke toegang met Exchange ActiveSync?

Ja, kunt u Exchange ActiveSync in een beleid voor voorwaardelijke toegang.






## <a name="what-you-should-avoid-doing"></a>Wat u hiermee moet voorkomen

Het framework voor voorwaardelijke toegang biedt u de flexibiliteit van een geweldige configuratie. Hoge mate van flexibiliteit ook betekent echter dat u zorgvuldig elke configuratiebeleid voordat u publiceert, krijgt om te voorkomen dat ongewenste resultaten. In deze context, u moet speciale aandacht besteden aan toewijzingen, zoals die betrekking hebben op voltooid ingesteld **alle gebruikers / groepen / cloud-apps**.

In uw omgeving, moet u voorkomen dat de volgende configuraties:


**Voor alle gebruikers, alle cloud-apps:**

- **Toegang blokkeren** -uw hele organisatie, zeker niet verstandig is deze configuratie wordt geblokkeerd.

- **Compatibel apparaat vereisen** - voor gebruikers die hun apparaten niet hebben ingeschreven, maar dit beleid blokkeert alle toegang, met inbegrip van toegang tot de Intune-portal. Als u een beheerder een geregistreerd apparaat, wordt u door dit beleid blokkeert terug om bij de Azure-portal om het beleid te wijzigen.

- **Lid van domein vereisen** - dit blok beleid toegang heeft ook de mogelijkheid om toegang te blokkeren voor alle gebruikers in uw organisatie als u een apparaat domein nog niet hebt.


**Voor alle gebruikers, alle cloud-apps, alle apparaatplatformen:**

- **Toegang blokkeren** -uw hele organisatie, zeker niet verstandig is deze configuratie wordt geblokkeerd.


## <a name="how-should-you-deploy-a-new-policy"></a>Hoe moet u een nieuw beleid implementeren?

Als eerste stap, moet u evalueren uw beleid met de [hulpprogramma what-if](what-if-tool.md).

Wanneer u klaar bent voor een nieuw beleid implementeren in uw omgeving, moet u dit doen in fasen:

1. Een beleid toepassen op een klein aantal gebruikers en controleer of de dat verwachte manier werkt. 

2.  Wanneer u een beleid om op te nemen meer gebruikers wilt uitbreiden, blijven alle beheerders uitsluiten van het beleid. Dit zorgt ervoor dat beheerders nog steeds toegang hebt en een beleid kunnen worden bijgewerkt als er een wijziging is vereist.

3. Alleen als dit echt nodig is, moet u een beleid toepassen op alle gebruikers. 

Als een best practice, maakt u een gebruikersaccount dat is:

- Toegewezen aan het beheer van beleid 
- Uitgesloten van alle beleidsregels voor uw


## <a name="policy-migration"></a>Beleid voor migratie

Houd rekening met de beleidsregels die u hebt gemaakt in Azure portal omdat migreren:

- U kunt nu scenario's die u niet voordat verwerken kan adres.

- U kunt beperken het nummer van het beleid dat u hebt voor het beheren van door onder te brengen.   

- U kunt alle beleid voor voorwaardelijke toegang op één centrale locatie kunt beheren.

- De klassieke Azure portal is buiten gebruik gesteld.   


Zie voor meer informatie, [klassiek beleid migreren in Azure portal](policy-migration.md).


## <a name="next-steps"></a>Volgende stappen

Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).
