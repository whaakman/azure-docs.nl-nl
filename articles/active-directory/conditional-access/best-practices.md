---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over dingen die u moet weten en wat dat inhoudt dat dient u dit te vermijden bij het configureren van beleid voor voorwaardelijke toegang.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/25/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: d30fe326ef677ca4543534d57dd306ed2a660300
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895559"
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


| Wat           | Hoe                                  | Waarom |
| :--            | :--                                  | :-- |
| **Cloud-apps** |Selecteer een of meer apps.  | Het doel van een beleid voor voorwaardelijke toegang is om te bepalen hoe gemachtigde gebruikers hebben toegang tot cloudapps.|
| **Gebruikers en groepen** | Selecteer ten minste één gebruiker of groep die is gemachtigd voor toegang tot uw geselecteerde cloud-apps. | Beleid voor voorwaardelijke toegang dat er geen gebruikers en groepen die zijn toegewezen, wordt nooit geactiveerd. |
| **Besturingselementen voor toegang** | Selecteer ten minste één toegangsbeheer. | Als uw voorwaarden wordt voldaan, moet de processor van uw beleid weten wat te doen. |




## <a name="what-you-should-know"></a>Wat u moet weten



### <a name="how-are-conditional-access-policies-applied"></a>Hoe worden de beleidsregels voor voorwaardelijke toegang toegepast?

Meer dan één beleid voor voorwaardelijke toegang mogelijk van toepassing wanneer u een cloud-app. In dit geval moeten alle beleidsregels die van toepassing worden voldaan. Bijvoorbeeld, als één beleid MFA vereist en de tweede een compatibel apparaat vereist, moet u doorlopen MFA en een compatibel apparaat gebruiken. 

Alle beleidsregels worden afgedwongen in twee fasen:

- In de **eerste** fase, alle beleidsregels worden geëvalueerd en alle besturingselementen voor toegang die niet worden voldaan worden verzameld. 

- In de **tweede** fase, wordt u gevraagd om te voldoen aan de vereisten die u dit nog niet hebt voldaan. Als een van de beleidsregels voor de toegang blokkeert, kunt u bent geblokkeerd en wordt niet gevraagd te voldoen aan andere besturingselementen voor beleid. Als geen van de beleidsregels voor u wordt geblokkeerd, wordt u gevraagd om te voldoen aan andere besturingselementen voor beleid in de volgende volgorde:

    ![Bestellen](./media/best-practices/06.png)
    
    Externe MFA-providers en de gebruiksvoorwaarden van komen volgende.



### <a name="how-are-assignments-evaluated"></a>Hoe worden toewijzingen geëvalueerd?

Alle toewijzingen zijn logisch **and**. Als u meer dan één toewijzing geconfigureerd hebt, moeten alle toewijzingen voor het activeren van een beleid worden voldaan.  

Als u nodig hebt om de locatievoorwaarde van een die van toepassing op alle verbindingen van buiten het netwerk van uw organisatie te configureren:

- Opnemen **alle locaties**
- Uitsluiten **alle vertrouwde IP-adressen**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Wat te doen als u toegang tot de Azure AD-beheerportal worden geblokkeerd?

Als u de Azure AD-portal vanwege een onjuiste instelling in een beleid voor voorwaardelijke toegang zijn vergrendeld:

- Controle is er zijn andere beheerders in uw organisatie die nog niet zijn geblokkeerd. Een beheerder met toegang tot de Azure portal kunt uitschakelen van het beleid dat is van invloed op uw aanmelding. 

- Als geen van de beheerders in uw organisatie het beleid bijwerken kan, moet u een ondersteuningsaanvraag indienen. Ondersteuning van Microsoft kunt controleren en bijwerken van beleid voor voorwaardelijke toegang waardoor toegang.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Wat gebeurt er als u beleidsregels in de klassieke Azure-portal en Azure-portal geconfigureerd hebt?  

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker toegang krijgt alleen als aan alle vereisten wordt voldaan.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Wat gebeurt er als u beleidsregels in de Intune Silverlight-portal en Azure portal hebt?

Beide beleidsregels worden afgedwongen door Azure Active Directory en de gebruiker toegang krijgt alleen als aan alle vereisten wordt voldaan.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Wat gebeurt er als ik meerdere beleidsregels voor dezelfde gebruiker geconfigureerd?  

Voor elke aanmelding, Azure Active Directory evalueert alle beleidsregels en zorgt ervoor dat aan alle vereisten wordt voldaan voordat toegang verleend aan de gebruiker. Toegang blokkeren trumps alle andere configuratie-instellingen. 


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Werkt voorwaardelijke toegang met Exchange ActiveSync?

Ja, kunt u Exchange ActiveSync in een beleid voor voorwaardelijke toegang.

### <a name="how-should-you-configure-conditional-access-with-office-365-apps"></a>Hoe moet u voorwaardelijke toegang configureren met Office 365-apps?

Omdat Office 365-apps met elkaar zijn verbonden, wordt u aangeraden vaak toewijzen apps samen gebruikt bij het maken van beleid.

Algemene onderling verbonden toepassingen bevatten Microsoft Flow, Microsoft Planner Microsoft Teams, Office 365 Exchange Online, Office 365 SharePoint Online en Office 365 Yammer.

Het is belangrijk voor beleidsregels die interactie van gebruikers, zoals meervoudige verificatie, vereisen wanneer toegang wordt geregeld aan het begin van een sessie of de taak. Als u dit niet doet, kunnen gebruikers om sommige taken in een app te voltooien niet mogelijk. Bijvoorbeeld, als u multi-factor authentication op niet-beheerde apparaten toegang tot SharePoint, maar niet naar e-mailadres nodig hebt, gebruikers die werken in hun e-mailbericht niet mogelijk SharePoint-bestanden koppelen aan een bericht. Meer informatie vindt u in het artikel [wat serviceafhankelijkheden zijn in Azure Active Directory voor voorwaardelijke toegang?](service-dependencies.md).





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

Wanneer er nieuwe beleidsregels zijn gereed voor uw omgeving, kunt u deze in fasen implementeren:

1. Een beleid toepassen op een klein aantal gebruikers en controleer of de dat verwachte manier werkt. 

2.  Wanneer u een beleid om op te nemen meer gebruikers uitvouwt. Doorgaan met het uitsluiten van alle beheerders van het beleid om ervoor te zorgen dat ze nog steeds toegang hebt en een beleid kunnen worden bijgewerkt als er een wijziging is vereist.

3. Alleen indien nodig, kunt u een beleid toepassen op alle gebruikers. 

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

Als u weten wilt:

- Het beleid voor voorwaardelijke toegang configureren, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).
- Over het plannen van uw beleid voor voorwaardelijke toegang, Zie [over het plannen van uw implementatie van voorwaardelijke toegang in Azure Active Directory](plan-conditional-access.md).
