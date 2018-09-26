---
title: Azure-portal dashboards delen met behulp van RBAC | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een dashboard in Azure portal met behulp van Role-Based Access Control.
services: azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: c07a9d92cac13d6325e66f44426f1a64e8ac53cb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096197"
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Azure-dashboards delen met behulp van Role-Based Access Control
Na het configureren van een dashboard, kunt u het publiceren en deze delen met andere gebruikers in uw organisatie. U toestaan dat anderen uw dashboard weergeven met behulp van Azure [Role-Based Access Control](../role-based-access-control/role-assignments-portal.md). U een gebruiker of groep van gebruikers toewijzen aan een rol en die rol definieert of deze gebruikers kunnen weergeven of wijzigen van het gepubliceerde dashboard. 

Alle gepubliceerde dashboards worden geïmplementeerd als Azure-resources, wat betekent dat ze bestaan als beheerbare objecten binnen uw abonnement en zijn opgenomen in een resourcegroep.  Dashboards zijn vanuit een access control-perspectief niet anders dan andere resources, zoals een virtuele machine of een storage-account.

> [!TIP]
> Afzonderlijke tegels op het dashboard afdwingen hun eigen vereisten voor toegangsbeheer op basis van de resources die ze worden weergegeven.  Daarom kunt u een dashboard dat globaal wordt gedeeld terwijl u nog steeds beveiligt de gegevens op afzonderlijke tegels ontwerpen.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Wat is toegangsbeheer voor dashboards
Met rollen gebaseerd toegangsbeheer (RBAC), kunt u gebruikers toewijzen aan rollen op drie verschillende niveaus van bereik:

* abonnement
* resourcegroep
* Bron

De machtigingen die u toewijst, worden overgenomen van abonnement omlaag naar de resource. De gepubliceerde dashboard is een resource. Daarom mogelijk hebt u al gebruikers toegewezen aan rollen voor het abonnement dat ook voor de gepubliceerde dashboard werken. 

Hier volgt een voorbeeld.  Stel dat u hebt een Azure-abonnement en de verschillende leden van uw team zijn toegewezen de rollen van **eigenaar**, **Inzender**, of **lezer** voor het abonnement. Gebruikers die eigenaren of bijdragers zijn kunnen weergeven, weergeven, maken, wijzigen of verwijderen in het abonnement.  Gebruikers die lezers kunnen aan dashboards, vermelden en weergeven, maar kunnen niet worden gewijzigd of verwijderd.  Gebruikers met leestoegang hebben mogen lokale wijzigingen aanbrengen in een gepubliceerde dashboard (zoals bij het oplossen van een probleem), maar zijn niet in staat zijn om deze wijzigingen publiceren naar de server.  Ze hebben de mogelijkheid om te maken van een persoonlijke kopie van het dashboard zelf

U kunt echter ook machtigingen toewijzen aan de resourcegroep met verschillende dashboards of aan een afzonderlijke dashboard. U kunt bijvoorbeeld besluiten dat een groep gebruikers moet hebben beperkte machtigingen voor het abonnement, maar hoger toegang tot een bepaald dashboard. U toewijzen gebruikers aan een rol voor dat dashboard. 

## <a name="publish-dashboard"></a>Dashboard publiceren
Stel dat u klaar bent met het configureren van een dashboard dat u wilt delen met een groep gebruikers in uw abonnement. De onderstaande stappen weer een aangepaste groep met de naam van de opslag-Managers, maar u kunt uw groep naam wat u wilt. Zie voor meer informatie over het maken van een Active Directory-groep en gebruikers toevoegen aan die groep [groepen beheren in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

1. Selecteer in het dashboard, **Share**.
   
     ![Selecteer delen](./media/azure-portal-dashboard-share-access/select-share.png)
2. Alvorens toegang toe te wijzen, moet u het dashboard publiceren. Standaard wordt het dashboard worden gepubliceerd naar een resourcegroep met de naam **dashboards**. Selecteer **Publiceren**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Uw dashboard is nu gepubliceerd. Als de machtigingen die zijn overgenomen uit het abonnement geschikt zijn, hoeft u niet verder niets te doen meer. Andere gebruikers in uw organisatie zich voor toegang tot en het dashboard op basis van hun abonnement op de rol wijzigen. Echter voor deze zelfstudie gaan we een groep toewijzen van gebruikers aan een rol voor dat dashboard.

## <a name="assign-access-to-a-dashboard"></a>Toegang toewijzen aan een dashboard
1. Na het publiceren van het dashboard, selecteert u **gebruikers beheren**.
   
     ![gebruikers beheren](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Hier ziet u een lijst met bestaande gebruikers die al een rol voor dit dashboard zijn toegewezen. De lijst met bestaande gebruikers is anders dan de onderstaande afbeelding. Zeer waarschijnlijk worden de toewijzingen overgenomen van het abonnement. Een nieuwe gebruiker of groep wilt toevoegen, selecteert u **toevoegen**.
   
     ![Gebruiker toevoegen](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Selecteer de rol die getallen vertegenwoordigt de machtigingen die u wilt verlenen. Selecteer voor dit voorbeeld **Inzender**.
   
     ![rol selecteren](./media/azure-portal-dashboard-share-access/select-role.png)
4. Selecteer de gebruiker of groep die u wilt toewijzen aan de rol. Als u de gebruiker of groep die u in de lijst zoekt niet ziet, gebruikt u het zoekvak. De lijst met beschikbare groepen is afhankelijk van de groepen die u hebt gemaakt in uw Active Directory.
   
     ![gebruiker selecteren](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Wanneer u klaar bent met het toevoegen van gebruikers of groepen selecteren **OK**. 
6. De nieuwe toewijzing wordt toegevoegd aan de lijst met gebruikers. U ziet dat de **toegang** wordt vermeld als **toegewezen** in plaats van **overgenomen**.
   
     ![toegewezen rollen](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een lijst met rollen, [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md).
* Zie voor meer informatie over het beheren van resources, [Azure-resources beheren via portal](resource-group-portal.md).

