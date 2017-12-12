---
title: Het starten van een onderzoek toegang | Microsoft Docs
description: Informatie over het maken van een revisie toegang voor bevoegde identiteiten met de Azure Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 3e52b731-55f4-4c8a-ba87-9fd34033f52f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: f57a32ca1914d18540289ebb05421a7ae9618094
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Het starten van een onderzoek toegang in Azure AD Privileged Identity Management
Roltoewijzingen worden 'verouderde' wanneer gebruikers uitgebreide toegang dat ze niet meer nodig hebt. Om te reduceren het risico dat samenhangt met deze verouderde roltoewijzingen, bevoorrechte rol beheerders regelmatig wordt aangeraden de functies die gebruikers hebben gekregen. Dit document worden de stappen voor het starten van een onderzoek toegang in Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Een revisie toegang starten
> [!NOTE]
> Als u de PIM-toepassing aan uw dashboard hebt toegevoegd in de Azure portal, Zie de stappen in [aan de slag met Azure Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)
> 
> 

Er zijn drie manieren kunt u een revisie toegang starten van de PIM-toepassing hoofdpagina van:

* **Toegang tot beoordelingen** > **toevoegen**
* **Rollen** > **revisie** knop
* Selecteer de specifieke rol moet worden geëvalueerd in de lijst met functies > **revisie** knop

Wanneer u klikt op in de **bekijken** knop, de **een revisie toegang starten** blade wordt weergegeven. Op deze blade en gaat u naar de revisie configureren met een naam en een tijdslimiet, kiest u een rol om te controleren en beslissen wie verantwoordelijk is voor de controle.

![Starten van de evaluatie van een access - schermafbeelding][1]

### <a name="configure-the-review"></a>De controle configureren
Voor het maken van een onderzoek toegang, moet u deze de naam en stel een begin- en datum.

![Configureren van de evaluatie - schermafbeelding][2]

Controleer de lengte van de evaluatie lang genoeg is voor gebruikers om deze te voltooien. Als u klaar voor de einddatum vallen bent, kunt u de controle vroeg altijd stoppen.

### <a name="choose-a-role-to-review"></a>Kies een rol te bekijken
Iedere evaluatie is gericht op slechts één rol. Tenzij u de controle van toegang vanuit een specifieke rolblade hebt gestart, moet u nu een rol kiezen.

1. Navigeer naar **lidmaatschap van de gebruikersrol controleren**
   
    ![Bekijk rollidmaatschap - schermafbeelding][3]
2. Kies één rol in de lijst.

### <a name="decide-who-will-perform-the-review"></a>Bepalen wie verantwoordelijk is voor de controle
Er zijn drie opties voor het uitvoeren van een beoordeling. Kunt u de controle toewijzen aan iemand anders is voltooid, kunt u dit zelf doen of u kunt elke gebruiker bekijken van hun eigen toegang hebben.

1. Navigeer naar **revisoren selecteren**
   
    ![Selecteer revisoren - schermafbeelding][4]
2. Kies een van de opties:
   
   * **Selecteer revisor**: Gebruik deze optie als u niet weet die toegang nodig heeft. Met deze optie kunt u de controle toewijzen aan een resource-eigenaar of de groepmanager te voltooien.
   * **Mij**: handig als u wilt zien hoe de toegang tot werk controleert of u wilt bekijken namens mensen die niet kan.
   * **Leden lees zelf**: Gebruik deze optie om gebruikers hun eigen roltoewijzingen bekijken.

### <a name="start-the-review"></a>De revisie starten
Tot slot hebt u de optie om te vereisen dat gebruikers een reden opgeven als ze hun toegang wordt goedgekeurd. Indien gewenst een beschrijving van de evaluatie toevoegen en selecteer **Start**.

Zorg ervoor dat u uw gebruikers weten dat er een wachten tot ze toegang-controleren en te laten laten [het uitvoeren van een onderzoek toegang](active-directory-privileged-identity-management-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>De controle van toegang beheren
U kunt de voortgang volgen als de revisoren hun beoordelingen in het Azure AD PIM-dashboard in de sectie toegang beoordelingen voltooid. Er is geen toegangsrechten worden gewijzigd in de map tot [de controle is voltooid](active-directory-privileged-identity-management-how-to-complete-review.md).

Totdat de controleperiode afgelopen is, kunt u gebruikers kunnen hun beoordeling voltooien herinneren of stoppen van de controle vroeg in het gedeelte van de beoordelingen toegang.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>PIM inhoudsopgave
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_start_review.png
[2]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_configure.png
[3]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_role.png
[4]: ./media/active-directory-privileged-identity-management-how-to-start-security-review/PIM_review_reviewers.png
