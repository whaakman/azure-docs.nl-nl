---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over wat die u moet weten en wat het is raadzaam doen bij het configureren van beleidsregels voor voorwaardelijke toegang.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: c155e18ddc4f1713aa9c62a991f4e849e024c5da
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723657"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt beheren hoe gemachtigde gebruikers toegang tot uw cloud-apps. In dit artikel vindt u informatie over:

- Dingen die u moet weten 
- Wat is Vermijd bij het configureren van beleidsregels voor voorwaardelijke toegang. 

In dit artikel wordt ervan uitgegaan dat u bekend de concepten en termen die worden beschreven in [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>Wat is vereist voor het maken van een beleid werken?

Wanneer u een nieuw beleid maakt, zijn er geen gebruikers, groepen, apps of toegangsbeheer geselecteerd.

![Cloud-apps](./media/active-directory-conditional-access-best-practices/02.png)


Als u uw beleid wilt werken, moet u het volgende configureren:


|Wat           | Hoe                                  | Waarom|
|:--            | :--                                  | :-- |
|**Cloud-apps** |U moet een of meer apps selecteren.  | Het doel van een beleid voor voorwaardelijke toegang is om te bepalen hoe geautoriseerde gebruikers hebben toegang tot cloud-apps.|
| **Gebruikers en groepen** | U moet ten minste één gebruiker of groep die is gemachtigd voor toegang tot uw geselecteerde cloud-apps selecteren. | Beleid voor voorwaardelijke toegang dat er geen gebruikers en groepen die zijn toegewezen, wordt nooit geactiveerd. |
| **Toegangsbeheer** | U moet ten minste één toegangsbeheer selecteren. | Als uw voorwaarden is voldaan, moet uw beleid processor weten wat te doen.|




## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="how-are-assignments-evaluated"></a>Hoe worden toewijzingen geëvalueerd

Alle toewijzingen zijn logische **and**. Als u meer dan één toewijzing geconfigureerd hebt, moeten u alle toewijzingen voldaan voor het activeren van een beleid.  

Als u nodig hebt om de voorwaarde van een locatie die van toepassing op alle verbindingen van buiten het netwerk van uw organisatie te configureren:

- Omvatten **alle locaties**
- Uitsluiten **alle goedgekeurde IP-adressen**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Wat te doen als u de Azure AD-beheerportal zijn vergrendeld?

Als u de Azure AD-beheerportal vanwege een onjuiste instelling in beleid voor voorwaardelijke toegang zijn vergrendeld:

- Controleer of er andere beheerders in uw organisatie die nog niet zijn geblokkeerd. Een beheerder met toegang tot de Azure portal kunt het beleid dat is van invloed op uw aanmelding uitschakelen. 

- Als geen van de beheerders in uw organisatie het beleid bijwerken kunt, moet u een ondersteuningsaanvraag te verzenden. Microsoft-ondersteuning kunt controleren en bijwerken van beleid voor voorwaardelijke toegang die van toegang verhinderen.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Wat gebeurt er als het beleid in de klassieke Azure-portal en de Azure portal geconfigureerd?  

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker krijgt toegang alleen wanneer alle vereisten wordt voldaan.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Wat gebeurt er als u een beleid in de Intune Silverlight-portal en de Azure-portal hebt?

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker krijgt toegang alleen wanneer alle vereisten wordt voldaan.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Wat gebeurt er als ik heb meerdere beleidsregels voor dezelfde gebruiker geconfigureerd?  

Voor elke aanmelding, Azure Active Directory evalueert alle beleidsregels en zorgt ervoor dat alle vereisten wordt voldaan voordat toegang verleend aan de gebruiker.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Werkt voorwaardelijke toegang met Exchange ActiveSync?

Ja, kunt u Exchange ActiveSync in een beleid voor voorwaardelijke toegang.






## <a name="what-you-should-avoid-doing"></a>Wat moet u niet doen

Het framework voor voorwaardelijke toegang biedt u de flexibiliteit van een geweldige configuratie. Hoge mate van flexibiliteit ook betekent echter dat u zorgvuldig elke configuratiebeleid voordat het beschikbaar wordt om ongewenste resultaten te voorkomen. In deze context moet u speciale aandacht schenken aan toewijzingen zoals die invloed hebben op volledige set betalen **alle gebruikers / groepen / cloud-apps**.

In uw omgeving, moet u de volgende configuraties voorkomen:


**Voor alle gebruikers alle cloud-apps:**

- **Toegang blokkeren** -uw hele organisatie, absoluut niet verstandig is deze configuratie wordt geblokkeerd.

- **Vereisen dat apparaat compatibel** - voor gebruikers die hun apparaten niet zijn geregistreerd, maar dit beleid blokkeert alle toegang, waaronder toegang tot de Intune-portal. Als u een beheerder met een geregistreerd apparaat, blokkeert dit beleid u uit om terug te zetten in de Azure-portal om het beleid te wijzigen.

- **Aan domein toevoegen vereisen** : dit beleid blok toegang heeft ook de mogelijkheid om toegang te blokkeren voor alle gebruikers in uw organisatie als u een apparaat domein nog geen hebt.


**Voor alle gebruikers, alle cloud-apps, alle platforms:**

- **Toegang blokkeren** -uw hele organisatie, absoluut niet verstandig is deze configuratie wordt geblokkeerd.


## <a name="how-should-you-deploy-a-new-policy"></a>Hoe moet u een nieuw beleid implementeren?

Als eerste stap, moet u evalueren uw beleid met de [wat gebeurt er als hulpprogramma](active-directory-conditional-access-whatif.md).

Wanneer u klaar bent voor een nieuw beleid implementeren in uw omgeving, moet u dit doen in fasen:

1. Een beleid toepassen op een klein aantal gebruikers en controleer of dat het naar verwachting werkt. 

2.  Wanneer u een beleid voor het opnemen van meer gebruikers wilt uitbreiden, blijven alle beheerders uitsluiten van het beleid. Dit zorgt ervoor dat beheerders nog steeds toegang hebben en een beleid kunnen worden bijgewerkt als er een wijziging is vereist.

3. Alleen als dit echt nodig is, moet u een beleid toepassen op alle gebruikers. 

Maak een gebruikersaccount dat als een best practice:

- Toegewezen aan het beheer van beleid 
- Uitgesloten van alle beleidsregels voor uw


## <a name="policy-migration"></a>Beleid voor migratie

Houd rekening met de beleidsregels die u hebt gemaakt in de Azure portal omdat migreren:

- U kunt nu scenario's die u niet voordat verwerken kan oplossen.

- U kunt het nummer van de beleidsregels die u hebt voor het beheren van deze samen te verkleinen.   

- U kunt het voorwaardelijke toegangsbeleid op één centrale locatie kunt beheren.

- De klassieke Azure portal is buiten gebruik gesteld.   


Zie voor meer informatie [migreren klassieke beleid in de Azure portal](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
