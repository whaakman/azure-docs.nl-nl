---
title: Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Meer informatie over wat die u moet weten en wat het is raadzaam doen bij het configureren van beleidsregels voor voorwaardelijke toegang.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 74b97ac263dcc45f7a8dd7461cbdb23d9fd5e6fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

Als u beleid in de klassieke Azure portal geconfigureerd hebt, moet u deze migreert naar de Azure portal omdat:


- Een gebruiker die zich in een Azure classic portal beleid en een Azure-portal beleid moet voldoen aan de vereisten in beide soorten beleid 

- Als u uw bestaande beleidsregels niet migreert, kunt u zich geen beleid implementeren dat het verlenen van toegang


### <a name="migration-from-the-azure-classic-portal"></a>Migratie van de klassieke Azure portal

In dit scenario: 

- In uw [klassieke Azure-portal](https://manage.windowsazure.com), u hebt geconfigureerd:

    - SharePoint Online

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-best-practices/14.png)

    - Beleid voor voorwaardelijke toegang op basis van apparaten

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-best-practices/15.png)

- U wilt een mam-beleid voor voorwaardelijke toegang configureren in de Azure portal 
 

#### <a name="configuration"></a>Configuratie 

- Bekijk uw beleid voor voorwaardelijke toegang op basis van apparaten

- Migreert naar de Azure-portal 

- Beleidsregels voor mobile application management-voorwaardelijke toegang toevoegen


### <a name="migrating-from-intune"></a>Migreren van Intune 

In dit scenario:

- In [Intune](https://portal.azure.com/#blade/Microsoft_Intune/SummaryBlade ), hebt u een mam-beleid voor voorwaardelijke toegang voor beide Exchange Online of SharePoint Online geconfigureerd

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-best-practices/15.png)

- U wilt migreren met voorwaardelijke toegang voor mobile application management in Azure portal


#### <a name="configuration"></a>Configuratie 
 
- Bekijk uw beleid voor voorwaardelijke toegang op basis van apparaten

- Migreert naar de Azure-portal 

- Bekijk u beleidsregels voor voorwaardelijke toegang van mobile application management geconfigureerd voor Exchange Online of SharePoint Online in Intune

- Toevoegen van het besturingselement voor **vereisen goedgekeurde toepassingen** naast het besturingselement op basis van apparaten 
 

### <a name="migrating-from-the-azure-classic-portal-and-intune"></a>Migreren van de klassieke Azure-portal en Intune

In dit scenario:

- U hebt de volgende geconfigureerd:

    - **Klassieke Azure-portal:** voorwaardelijke op basis van apparaten 

    - **Intune:** Mobile application management-beleid voor voorwaardelijke toegang 
    
- U wilt migreren van beide beleidsregels voor het gebruik van beleidsregels voor voorwaardelijke toegang van mobile application management in Azure portal


#### <a name="configuration"></a>Configuratie

- Bekijk uw beleid voor voorwaardelijke toegang op basis van apparaten

- Migreert naar de Azure-portal 

- Bekijk u beleid voor voorwaardelijke toegang van mobiele toepassing management geconfigureerd voor Exchange Online of SharePoint Online in Intune

- Toevoegen van het besturingselement voor **vereisen goedgekeurde toepassingen** naast de op basis van apparaten 




## <a name="next-steps"></a>Volgende stappen

Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
