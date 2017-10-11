---
title: Het uitvoeren van een onderzoek toegang | Microsoft Docs
description: Informatie over het uitvoeren van een overzicht met de Azure Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: a98ed60221eeba1d9c92df846aeae2deafb8ae60
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>Het uitvoeren van een onderzoek toegang in Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management vereenvoudigt de manier waarop bedrijven bevoorrechte toegang tot resources in Azure AD en andere Microsoft online services zoals Office 365 of Microsoft Intune beheert.  

Als u zijn toegewezen aan een administratieve rol, kan u regelmatig bevestigen dat u nog die rol bij uw werk moet door beheerder met bevoorrechte rol van uw organisatie vragen. Krijgt u mogelijk een e-mailbericht een koppeling bevat of u kunt dan direct door naar de [Azure-portal](https://portal.azure.com). Volg de stappen in dit artikel om uit te voeren met een zelf controleren van de toegewezen rollen.

Als u een beheerder met bevoorrechte rol geïnteresseerd in toegang revisies, vraag meer details op [starten een onderzoek toegang](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
U kunt de toepassing Azure AD Privileged Identity Management (PIM) in de [Azure-portal](https://portal.azure.com/) om uit te voeren van uw beoordeling.  Als u niet de Azure AD Privileged Identity Management-toepassing op uw portal hebt, als volgt te werk om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikersnaam in de rechterbovenhoek van het Azure-portal en selecteer de map waar u wilt dat u werkt.
3. Selecteer **Meer services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="approve-or-deny-access"></a>Goedkeuren of weigeren van toegang
Wanneer u goedkeuren of weigeren van toegang, u hebt zojuist zorgt ervoor dat de revisor of u deze rol of niet. Kies **goedkeuren** als u wilt blijven in de rol of **weigeren** als u de toegang niet meer nodig. De status niet meteen gewijzigd totdat de revisor van toepassing de resultaten is.
Volg deze stappen om te zoeken en voltooien van de controle van toegang:

1. Selecteer in de toepassing PIM **revisie bevoegde toegang**. Als u geen beoordelingen die in behandeling toegang hebt, wordt deze weergegeven in de blade van Azure AD Access beoordelingen.
2. Selecteer de revisie die u wilt voltooien.
3. Tenzij u de controle hebt gemaakt, wordt u worden weergegeven als de enige gebruiker in de evaluatie. Selecteer het selectievakje naast uw naam.
4. Kies een **goedkeuren** of **weigeren**. Mogelijk moet u een reden voor uw beslissing in omvatten de **een reden opgeven** in het tekstvak.  
5. Sluit de **revisie Azure AD-rollen** blade.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
