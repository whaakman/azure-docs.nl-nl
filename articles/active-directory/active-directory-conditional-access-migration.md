---
title: Wat is de migratie van een beleid voor voorwaardelijke toegang van Azure Active Directory? | Microsoft Docs
description: Meer informatie over wat u moet weten voor het migreren van klassieke beleidsregels in Azure portal.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
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
ms.date: 07/24/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6123e05d6e20d213a859836ddf55a3ca6f7255a5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224565"
---
# <a name="what-is-a-policy-migration-in-azure-active-directory-conditional-access"></a>Wat is de migratie van een beleid voor voorwaardelijke toegang van Azure Active Directory? 


[Voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) is een mogelijkheid van Azure Active directory (Azure AD) waarmee u om te bepalen hoe gemachtigde gebruikers toegang tot uw cloud-apps. Terwijl het doel nog steeds hetzelfde is, is de versie van de nieuwe Azure-portal aanzienlijke verbeteringen aan hoe voorwaardelijke toegang werkt geïntroduceerd.

U moet rekening houden met de beleidsregels die u hebt gemaakt in Azure portal omdat migreren:

- U kunt nu scenario's die u niet voordat verwerken kan adres.

- U kunt beperken het nummer van het beleid dat u hebt voor het beheren van door onder te brengen.   

- U kunt alle beleid voor voorwaardelijke toegang op één centrale locatie kunt beheren.

- De klassieke Azure portal wordt beëindigd.   

In dit artikel wordt uitgelegd wat u moet weten voor het migreren van uw bestaande beleidsregels voor voorwaardelijke toegang naar de nieuwe structuur.
 
## <a name="classic-policies"></a>Klassieke beleidsregels

In de [Azure-portal](https://portal.azure.com), wordt de [voorwaardelijke toegang - beleid](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) pagina is uw beginpunt voor uw voorwaardelijk beleid. Echter, in uw omgeving mogelijk ook hebt u niet hebt gemaakt met behulp van deze pagina beleid voor voorwaardelijke toegang. Deze beleidsregels worden aangeduid als *klassieke beleidsregels*. Klassieke beleidsregels zijn beleidsregels voor voorwaardelijke toegang, u hebt gemaakt in:

- De klassieke Azure portal
- De klassieke Intune-portal
- De Intune App Protection-portal


Op de **voorwaardelijke toegang** pagina, kunt u uw klassieke beleidsregels openen door te klikken [ **klassiek beleid (preview)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) in de **beheren** de sectie. 


![Azure Active Directory](./media/active-directory-conditional-access-migration/71.png)


De **klassiek beleid** weergave biedt u een optie voor het:

- Uw klassieke beleidsregels filteren.
 
    ![Azure Active Directory](./media/active-directory-conditional-access-migration/72.png)

- Klassiek beleid uitschakelen.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/73.png)
   
- Controleer de instellingen van een klassieke beleidsregels (en uitschakelen).

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/74.png)


Als u een klassiek beleid hebt uitgeschakeld, niet u deze stap niet meer ongedaan maken. Daarom kunt u het lidmaatschap in een klassiek beleid met de **Details** weergeven. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/75.png)

Door de geselecteerde groepen wijzigen of door specifieke groepen uitsluit, kunt u het effect van een uitgeschakelde klassiek beleid voor een paar testgebruikers testen voordat u het beleid voor alle opgenomen gebruikers en groepen uitschakelt. 



## <a name="azure-ad-conditional-access-policies"></a>Azure AD-beleidsregels voor voorwaardelijke toegang

U kunt alle beleidsregels op één centrale locatie kunt beheren met voorwaardelijke toegang in Azure portal. Omdat de implementatie van hoe voorwaardelijke toegang is aanzienlijk gewijzigd, moet u vertrouwd raken met de basisconcepten voordat u uw klassieke beleidsregels migreert.

Zie:

- [Wat is voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md) voor meer informatie over de basisconcepten en de terminologie.

- [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md) voor enkele richtlijnen over het implementeren van voorwaardelijke toegang in uw organisatie.

- [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md) om vertrouwd te raken met de gebruikersinterface in Azure portal.


 
## <a name="migration-considerations"></a>Overwegingen bij migraties

In dit artikel wordt Azure AD-beleidsregels voor voorwaardelijke toegang worden ook aangeduid als *nieuwe beleidsregels*.
Uw klassieke beleidsregels blijven werken samen met uw nieuwe beleid totdat u ze verwijderen of uitschakelen. 

De volgende aspecten zijn belangrijk in de context van een beleid voor consolidatie:

- Terwijl klassieke beleidsregels zijn gekoppeld aan een bepaalde cloud-app, kunt u zoveel cloudapps, zoals u in een nieuw beleid wilt selecteren.

- Besturingselementen van een klassiek beleid en een nieuw beleid voor een cloud-app is vereist voor alle besturingselementen (*en*) moet worden voldaan. 


- In een nieuw beleid kunt u het volgende doen:
 
    - Meerdere voorwaarden combineren als nodig is voor uw scenario. 

    - Selecteer verschillende vereisten als de toegang beheren en deze combineren met een logische verlenen *of* (een van de geselecteerde besturingselementen vereisen) of met een logische *en* (alle van de geselecteerde besturingselementen vereisen).

        ![Azure Active Directory](./media/active-directory-conditional-access-migration/25.png)




### <a name="office-365-exchange-online"></a>Office 365 Exchange online

Als u wilt migreren van klassieke beleidsregels voor **Office 365 Exchange online** die bevatten **Exchange Active Sync** als voorwaarde voor client-apps, u mogelijk niet te consolideren in een nieuw beleid. 

Dit is, bijvoorbeeld het geval als u wilt ondersteunen alle client-app-typen. In een nieuw beleid met **Exchange Active Sync** als voorwaarde voor client-apps, u kunt andere client-apps niet selecteren.

![Azure Active Directory](./media/active-directory-conditional-access-migration/64.png)

Een consolidatie in een nieuw beleid is ook niet mogelijk als uw klassieke beleidsregels verschillende voorwaarden zijn bevatten. Een nieuw beleid met **Exchange Active Sync** als client-apps voorwaarde geconfigureerd biedt geen ondersteuning voor andere voorwaarden:   

![Azure Active Directory](./media/active-directory-conditional-access-migration/08.png)

Hebt u een nieuw beleid met **Exchange Active Sync** als client-apps de voorwaarde is geconfigureerd, moet u om ervoor te zorgen dat alle andere voorwaarden niet zijn geconfigureerd. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/16.png)
 

[App-gebaseerde](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) klassieke beleidsregels voor Office 365 Exchange Online met **Exchange Active Sync** als voorwaarde voor client-apps toestaan **ondersteund** en **wordtnietondersteund** [apparaatplatformen](active-directory-conditional-access-technical-reference.md#device-platform-condition). Terwijl u afzonderlijke apparaatplatformen niet in een nieuw beleid voor gerelateerde configureren, kunt u de ondersteuning om te beperken [ondersteunde platforms voor apparaten](active-directory-conditional-access-technical-reference.md#device-platform-condition) alleen. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/65.png)

U kunt meerdere klassieke beleidsregels die zijn samenvoegen **Exchange Active Sync** als voorwaarde voor client-apps als ze beschikken over:

- Alleen **Exchange Active Sync** als voorwaarde 

- Verschillende vereisten voor het verlenen van toegang dat is geconfigureerd

Een veelvoorkomend scenario is de consolidatie van:

- Een apparaat gebaseerde klassiek beleid vanuit de klassieke Azure portal 
- Een app-beleid op basis van klassieke in de Intune app protection-beheerportal 
 
In dit geval kunt u uw klassieke beleidsregels samenvoegen in een nieuw beleid met beide vereisten hebt geselecteerd.

![Azure Active Directory](./media/active-directory-conditional-access-migration/62.png)



### <a name="device-platforms"></a>Apparaatplatformen

Klassieke beleidsregels met [besturingselementen op basis van app](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) zijn vooraf geconfigureerd met iOS en Android als de [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition). 

In een nieuw beleid, moet u selecteert de [apparaatplatformen](active-directory-conditional-access-technical-reference.md#device-platform-condition) u wilt ondersteunen afzonderlijk.

![Azure Active Directory](./media/active-directory-conditional-access-migration/41.png)



 
 


## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [GRequire MFA voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, raadpleegt u de [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
