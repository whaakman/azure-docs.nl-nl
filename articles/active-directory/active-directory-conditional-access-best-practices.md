---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over wat die u moet weten en wat het is raadzaam doen bij het configureren van beleidsregels voor voorwaardelijke toegang.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory

In dit onderwerp vindt u informatie over wat die u moet weten en wat het is raadzaam doen bij het configureren van beleidsregels voor voorwaardelijke toegang. Voordat u dit onderwerp leest, moet u vertrouwd raken met de concepten en termen die worden beschreven in [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="whats-required-to-make-a-policy-work"></a>Wat is vereist voor het maken van een beleid werken?

Wanneer u een nieuw beleid maakt, zijn er geen gebruikers, groepen, apps of toegangsbeheer geselecteerd.

![Cloud-apps](./media/active-directory-conditional-access-best-practices/02.png)


Als u uw beleid wilt werken, moet u het volgende configureren:


|Wat           | Hoe                                  | Waarom|
|:--            | :--                                  | :-- |
|**Cloud-apps** |U moet een of meer apps selecteren.  | Het doel van een beleid voor voorwaardelijke toegang is zodat u kunt aanpassen hoe gemachtigde gebruikers toegang uw apps tot hebben.|
| **Gebruikers en groepen** | U moet ten minste één gebruiker of groep die is gemachtigd voor toegang tot de cloud-apps die u hebt geselecteerd selecteren. | Beleid voor voorwaardelijke toegang dat er geen gebruikers en groepen die zijn toegewezen, wordt nooit geactiveerd. |
| **Toegangsbeheer** | U moet ten minste één toegangsbeheer selecteren. | De processor van uw beleid moet weten wat te doen als uw voorwaarden is voldaan.|


Naast deze basisvereisten moet in veel gevallen u ook configureren een voorwaarde. Een beleid zou ook werken zonder een geconfigureerde voorwaarde, zijn voorwaarden de aangedreven factor voor toegang tot uw apps aan te passen.


![Cloud-apps](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Hoe worden toewijzingen geëvalueerd

Alle toewijzingen zijn logische **and**. Als u meer dan één toewijzing geconfigureerd hebt, om te activeren van een beleid moeten alle toewijzingen worden voldaan.  

Als u nodig hebt voor het configureren van de voorwaarde van een locatie die van toepassing op alle verbindingen van buiten het netwerk van uw organisatie, kunt u dit doen door:

- Inclusief **alle locaties**
- Met uitzondering van **alle goedgekeurde IP-adressen**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Wat gebeurt er als het beleid in de klassieke Azure-portal en de Azure portal geconfigureerd?  
Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker krijgt toegang alleen wanneer alle vereisten wordt voldaan.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Wat gebeurt er als u een beleid in de Intune Silverlight-portal en de Azure-Portal hebt?
Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker krijgt toegang alleen wanneer alle vereisten wordt voldaan.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Wat gebeurt er als ik heb meerdere beleidsregels voor dezelfde gebruiker geconfigureerd?  
Voor elke aanmelding, Azure Active Directory evalueert alle beleidsregels en zorgt ervoor dat alle vereisten wordt voldaan voordat toegang verleend aan de gebruiker.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Werkt voorwaardelijke toegang met Exchange ActiveSync?

Ja, kunt u Exchange ActiveSync in een beleid voor voorwaardelijke toegang.


## <a name="what-you-should-avoid-doing"></a>Wat moet u niet doen

Het framework voor voorwaardelijke toegang biedt u de flexibiliteit van een geweldige configuratie. Hoge mate van flexibiliteit ook betekent echter dat u zorgvuldig elke configuratiebeleid vóór vrijgeven om ongewenste resultaten voorkomen. In deze context moet u speciale aandacht schenken aan toewijzingen zoals die invloed hebben op volledige set betalen **alle gebruikers / groepen / cloud-apps**.

In uw omgeving, moet u de volgende configuraties voorkomen:


**Voor alle gebruikers alle cloud-apps:**

- **Toegang blokkeren** -uw hele organisatie, absoluut niet verstandig is deze configuratie wordt geblokkeerd.

- **Vereisen dat apparaat compatibel** - voor gebruikers die niet hun apparaten hebben ingeschreven, maar dit beleid blokkeert alle toegang, waaronder toegang tot de Intune-portal. Als u een beheerder met een geregistreerd apparaat, blokkeert dit beleid u uit om terug te zetten in de Azure-portal om het beleid te wijzigen.

- **Aan domein toevoegen vereisen** : dit beleid blok toegang heeft ook de mogelijkheid om toegang te blokkeren voor alle gebruikers in uw organisatie als u een apparaat domein nog geen hebt.


**Voor alle gebruikers, alle cloud-apps, alle platforms:**

- **Toegang blokkeren** -uw hele organisatie, absoluut niet verstandig is deze configuratie wordt geblokkeerd.



## <a name="policy-migration"></a>Beleid voor migratie

U moet rekening houden met de beleidsregels die u hebt gemaakt in de Azure portal omdat migreren:

- U kunt nu scenario's die u niet voordat verwerken kan oplossen.

- U kunt het nummer van de beleidsregels die u hebt voor het beheren van deze samen te verkleinen.   

- U kunt het voorwaardelijke toegangsbeleid op één centrale locatie kunt beheren.

- De klassieke Azure portal wordt buiten gebruik worden gesteld.   


Zie voor meer informatie [migreren klassieke beleid in de Azure portal](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
