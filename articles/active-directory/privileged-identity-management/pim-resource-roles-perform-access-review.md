---
title: Uitvoeren van een onderzoek toegang in Privileged Identity Management voor Azure-resources | Microsoft Docs
description: Dit document wordt beschreven hoe u een revisie toegang uitvoert in PIM voor Azure-resources, volgens de resourcerol.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 10da40cf83dd39251033403a6a6913eb2e3b8b1d
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260848"
---
# <a name="perform-an-access-review-in-pim-according-to-resource-role"></a>Een onderzoek toegang in PIM, volgens de resourcerol uitvoeren
Privileged Identity Management (PIM) voor Azure-resources vereenvoudigt de manier waarop bedrijven bevoorrechte toegang tot bronnen in Azure beheert. 

Als u zijn toegewezen aan een beheerderrol, kan beheerder met bevoorrechte rol van uw organisatie u vragen om regelmatig bevestigen dat u nog steeds die rol voor uw project moet. Krijgt u mogelijk een e-mailbericht een koppeling bevat of u kunt dan direct door naar de [Azure-portal](https://portal.azure.com). Volg de stappen in dit artikel om uit te voeren met een zelf controleren van de toegewezen rollen.

Als u een beheerder met bevoorrechte rol geïnteresseerd in toegang revisies, vraag meer details op [starten een onderzoek toegang](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
U kunt de Azure Active Directory (Azure AD) PIM-toepassing in de [Azure-portal](https://portal.azure.com/) om uit te voeren van uw beoordeling. Als u de toepassing geen in uw portal hebt, als volgt te werk om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruiker naam in de rechterbovenhoek van de Azure portal en selecteer de map waar u wilt dat u werkt.
3. Selecteer **alle services**, en gebruik de **Filter** vak om te zoeken naar *Azure AD Privileged Identity Management*.
4. Controleer **vastmaken aan dashboard**, en selecteer vervolgens **maken**. De PIM-toepassing wordt geopend.

## <a name="approve-or-deny-access"></a>Goedkeuren of weigeren van toegang
Wanneer u goedkeuren of weigeren van toegang, u hebt zojuist zorgt ervoor dat de revisor of u deze rol of niet. Kies **goedkeuren** als u wilt blijven in de rol of **weigeren** als u de toegang niet meer nodig. De status verandert alleen wanneer de revisor van toepassing de resultaten is.

Volg deze stappen om te zoeken en voltooien van de controle van toegang:
1. Blader naar de Azure AD PIM-toepassing.
2. Selecteer de **toegang controleren** blade.

   ![Schermafbeelding van de PIM-toepassing, met controle toegang blade geselecteerd](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Selecteer de revisie die u wilt voltooien. 
4. Kies een **goedkeuren** of **weigeren**. In de **een vak reden**, moet u mogelijk een reden voor uw beslissing nemen.

   ![Schermopname van details voorbeeldpagina](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
