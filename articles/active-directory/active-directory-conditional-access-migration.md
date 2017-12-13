---
title: Migreren van klassieke beleid in de Azure portal | Microsoft Docs
description: Meer informatie over wat u moet weten voor het migreren van klassieke beleid in de Azure portal.
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
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 16628bd4fa41d2e7697e1c2501f2ccd31dbd0496
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migreren van klassieke beleid in de Azure portal 


[Voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) is een functie van Azure Active directory (Azure AD) dat u bepalen hoe kunt gemachtigde gebruikers toegang hebben uw cloud-apps. Het doel is nog steeds hetzelfde is, heeft de release van de nieuwe Azure portal aanzienlijke verbeteringen in hoe voorwaardelijke toegang werkt geïntroduceerd.

U moet rekening houden met de beleidsregels die u hebt gemaakt in de Azure portal omdat migreren:

- U kunt nu scenario's die u niet voordat verwerken kan oplossen.

- U kunt het nummer van de beleidsregels die u hebt voor het beheren van deze samen te verkleinen.   

- U kunt het voorwaardelijke toegangsbeleid op één centrale locatie kunt beheren.

- De klassieke Azure portal wordt buiten gebruik worden gesteld.   

Dit artikel wordt uitgelegd wat u moet weten om te migreren van uw bestaande beleidsregels voor voorwaardelijke toegang naar de nieuwe structuur.
 
## <a name="classic-policies"></a>Klassieke beleidsregels

In de [Azure-portal](https://portal.azure.com), wordt de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) pagina is het beleid van uw toegangspunt voor de voorwaardelijke toegang. Echter in uw omgeving, mogelijk ook hebt u geen hebt gemaakt met behulp van deze pagina beleid voor voorwaardelijke toegang. Deze beleidsregels worden aangeduid als *klassieke beleid*. Klassieke beleidsregels zijn beleidsregels voor voorwaardelijke toegang, u hebt gemaakt in:

- de klassieke Azure portal
- De klassieke portal Intune
- De Intune App Protection-portal


Op de **voorwaardelijke toegang** pagina kunt u uw klassieke beleid openen door te klikken op [ **klassieke beleid (preview)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) in de **beheren** sectie. 


![Azure Active Directory](./media/active-directory-conditional-access-migration/71.png)


De **klassieke beleid** weergave biedt u een optie voor:

- Het beleid voor klassieke filteren.
 
    ![Azure Active Directory](./media/active-directory-conditional-access-migration/72.png)

- Beleidsregels voor klassieke uitschakelen.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/73.png)
   
- Controleer de instellingen van een klassieke beleidsregels (en dat u deze uitschakelt).

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/74.png)


Als u een beleid voor klassieke hebt uitgeschakeld, is niet mogelijk deze stap niet meer. Daarom kunt u het lidmaatschap in een klassieke beleid met de **Details** weergeven. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/75.png)

Beide wijzigen van de geselecteerde groepen of uitsluiten van specifieke groepen, kunt u het effect van een uitgeschakelde klassieke beleid voor enkele testgebruikers testen voordat u het beleid voor alle inbegrepen gebruikers en groepen uitschakelt. 



## <a name="azure-ad-conditional-access-policies"></a>Azure AD-beleid voor voorwaardelijke toegang

U kunt alle beleidsregels op één centrale locatie kunt beheren met voorwaardelijke toegang in de Azure portal. Omdat de implementatie van de werking van voorwaardelijke toegang is aanzienlijk gewijzigd, moet u vertrouwd raken met de basisconcepten vóór de migratie van uw klassieke beleid.

Zie:

- [Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md) voor meer informatie over de basisconcepten en de terminologie.

- [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md) ophalen van sommige hulp bij het implementeren van voorwaardelijke toegang in uw organisatie.

- [Aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) om vertrouwd te raken met de gebruikersinterface in de Azure portal.


 
## <a name="migration-considerations"></a>Overwegingen bij migratie

In dit artikel, beleidsregels voor voorwaardelijke toegang van Azure AD worden ook aangeduid als *nieuw beleid*.
Het beleid voor klassieke blijven werken naast het nieuwe beleid totdat u ze verwijdert of uitschakelt. 

De volgende aspecten zijn belangrijk in de context van een beleid voor consolidatie:

- Tijdens het klassieke beleidsregels zijn gekoppeld aan een specifieke cloud-app, kunt u zoveel cloud-apps zoals u in een nieuw beleid nodig hebt om te selecteren.

- Besturingselementen van een klassieke beleid en een nieuw beleid voor een cloud-app is vereist voor alle besturingselementen (*en*) moet worden voldaan. 


- In een nieuw beleid kunt u het volgende doen:
 
    - Meerdere voorwaarden combineren indien vereist door uw scenario. 

    - Selecteer verschillende vereisten als toegang beheren en deze te combineren met een logische verlenen *OR* (vereist een van de geselecteerde besturingselementen) of met een logische *en* (alle geselecteerde besturingselementen vereist).

        ![Azure Active Directory](./media/active-directory-conditional-access-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange online

Als u wilt migreren van klassieke beleidsregels voor **Office 365 Exchange online** die **Exchange Active Sync** als voorwaarde voor client-apps, u niet mogelijk ze naar een nieuw beleid te consolideren. 

Dit is, bijvoorbeeld het geval als u wilt ondersteunen alle client-app-typen. In een nieuw beleid dat is **Exchange Active Sync** als voorwaarde voor client-apps, u kunt geen andere ClientApps te selecteren.

![Azure Active Directory](./media/active-directory-conditional-access-migration/64.png)

Een consolidatie in een nieuw beleid is ook niet mogelijk als het beleid voor klassieke verschillende voorwaarden bevatten. Een nieuw beleid met **Exchange Active Sync** als client-apps voorwaarde geconfigureerd biedt geen ondersteuning voor andere voorwaarden:   

![Azure Active Directory](./media/active-directory-conditional-access-migration/08.png)

Als er een nieuw beleid met **Exchange Active Sync** als client-apps voorwaarde geconfigureerd, moet u ervoor zorgen dat alle andere voorwaarden niet zijn geconfigureerd. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/16.png)
 

[Op basis van een App](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) klassieke beleidsregels voor Office 365 Exchange Online met **Exchange Active Sync** als voorwaarde voor client-apps toestaan **ondersteund** en **wordtnietondersteund** [apparaatplatforms](active-directory-conditional-access-technical-reference.md#device-platform-condition). Terwijl u afzonderlijke apparaatplatforms in een nieuw beleid voor verwante configureren kunt, kunt u de ondersteuning om te beperken [ondersteunde platforms voor apparaten](active-directory-conditional-access-technical-reference.md#device-platform-condition) alleen. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/65.png)

U kunt meerdere beleidsregels voor het klassieke met consolideren **Exchange Active Sync** als voorwaarde voor client-apps als ze hebben:

- Alleen **Exchange Active Sync** als voorwaarde 

- Verschillende vereisten voor het verlenen van toegang dat is geconfigureerd

Een gebruikelijk scenario is de consolidatie van:

- Op basis van apparaten en beleid klassieke vanuit de klassieke Azure portal 
- Een app gebaseerde klassieke beleid in de Intune-portal voor app-beveiliging 
 
In dit geval kunt u uw klassieke beleid samenvoegen in een nieuw beleid met beide vereisten die zijn geselecteerd.

![Azure Active Directory](./media/active-directory-conditional-access-migration/62.png)



### <a name="device-platforms"></a>Apparaatplatforms

Klassieke beleidsregels met [besturingselementen op basis van een app](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) vooraf geconfigureerd zijn met iOS en Android als de [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition). 

In een nieuw beleid, moet u selecteert de [apparaatplatforms](active-directory-conditional-access-technical-reference.md#device-platform-condition) u wilt ondersteunen afzonderlijk.

![Azure Active Directory](./media/active-directory-conditional-access-migration/41.png)



 
 


## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
