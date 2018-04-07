---
title: Het uitvoeren van een onderzoek toegang in Privileged Identity Management voor Azure-Resources | Microsoft Docs
description: Dit document wordt beschreven hoe u en toegang tot controleren in PIM voor Azure-Resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4afb923058143dd1771043db8433aa3a65541bf7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---perform-access-review"></a>Uitvoeren van Privileged Identity Management - Resource role - toegang controleren
Privileged Identity Management voor Azure-Resources vereenvoudigt de manier waarop bedrijven bevoorrechte toegang tot bronnen in Azure beheert. 

Als u zijn toegewezen aan een administratieve rol, kan u regelmatig bevestigen dat u nog die rol bij uw werk moet door beheerder met bevoorrechte rol van uw organisatie vragen. Krijgt u mogelijk een e-mailbericht een koppeling bevat of u kunt dan direct door naar de [Azure-portal](https://portal.azure.com). Volg de stappen in dit artikel om uit te voeren met een zelf controleren van de toegewezen rollen.

Als u een beheerder met bevoorrechte rol geïnteresseerd in toegang revisies, vraag meer details op [starten een onderzoek toegang](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
U kunt de toepassing Azure AD Privileged Identity Management (PIM) in de [Azure-portal](https://portal.azure.com/) om uit te voeren van uw beoordeling.  Als u niet de Azure AD Privileged Identity Management-toepassing op uw portal hebt, als volgt te werk om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikersnaam in de rechterbovenhoek van het Azure-portal en selecteer de map waar u wilt dat u werkt.
3. Selecteer **Alle services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="approve-or-deny-access"></a>Goedkeuren of weigeren van toegang
Wanneer u goedkeuren of weigeren van toegang, u hebt zojuist zorgt ervoor dat de revisor of u deze rol of niet. Kies **goedkeuren** als u wilt blijven in de rol of **weigeren** als u de toegang niet meer nodig. De status niet meteen gewijzigd totdat de revisor van toepassing de resultaten is.
Volg deze stappen om te zoeken en voltooien van de controle van toegang:
1. Navigeer naar de Azure AD Privileged Identity Management-toepassing.
2. Selecteer de blade van de toegang controleren

![](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Selecteer de controle wilt voltooien. 
4. Kies een **goedkeuren** of **weigeren**. Mogelijk moet u een reden voor uw beslissing in omvatten de **een reden opgeven** in het tekstvak.

![](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
