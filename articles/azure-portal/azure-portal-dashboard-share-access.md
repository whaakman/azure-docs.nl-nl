---
title: Azure portal dashboards delen met behulp van RBAC | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een dashboard in de Azure portal deelt met behulp van toegangsbeheer op basis van rollen.
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: azure-portal
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: 33ddfef1e23a6ff0c2cb8e359d408d2c42cbcf3a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Azure dashboards delen met toegangsbeheer op basis van rollen
U kunt na het configureren van een dashboard, publiceren en delen met andere gebruikers in uw organisatie. Anderen uw dashboard weergeven met behulp van Azure [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-configure.md). U een gebruiker of groep gebruikers toewijzen aan een rol en die rol definieert die gebruikers kunnen weergeven of wijzigen van het gepubliceerde-dashboard. 

Alle gepubliceerde dashboards worden geïmplementeerd als Azure-resources, wat betekent dat ze bestaan als beheerbare objecten binnen uw abonnement en zijn opgenomen in een resourcegroep.  Dashboards zijn vanuit een access control-perspectief niet anders dan andere resources, zoals een virtuele machine of een opslagaccount.

> [!TIP]
> Afzonderlijke tegels op het dashboard afdwingen hun eigen vereisten voor toegangsbeheer op basis van de resources die ze weergeven.  Daarom kunt u een dashboard dat globaal wordt gedeeld terwijl beveiligd blijven de gegevens op afzonderlijke tegels ontwerpen.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Wat is toegangsbeheer voor dashboards
Met op rollen gebaseerde toegangsbeheer (RBAC), kunt u gebruikers toewijzen aan rollen op drie verschillende niveaus van bereik:

* abonnement
* resourcegroep
* Bron

De machtigingen die u toewijst, worden overgenomen van abonnement omlaag naar de resource. Het gepubliceerde dashboard is een resource. Daarom mogelijk hebt u al gebruikers toegewezen aan rollen voor het abonnement dat ook voor het gepubliceerde dashboard werken. 

Hier volgt een voorbeeld.  Stel dat u hebt een Azure-abonnement en verschillende leden van uw team zijn toegewezen de functies van **eigenaar**, **Inzender**, of **lezer** voor het abonnement. Gebruikers die eigenaar of inzenders kunnen weergeven, bekijken, maken, wijzigen of verwijderen in het abonnement.  Gebruikers die lezers zijn kunnen lijst en weergave dashboards, maar kunnen niet worden gewijzigd of verwijderd.  Gebruikers met leestoegang hebben, kunnen lokale wijzigingen aanbrengen in een gepubliceerde dashboard (zoals wanneer een probleem moet oplossen), maar niet kunnen deze wijzigingen publiceren naar de server.  De optie om een persoonlijke kopie van het dashboard voor zichzelf hebben

U kan echter ook machtigingen toewijzen aan de resourcegroep met de verschillende dashboards of naar een afzonderlijke dashboard. U kunt bijvoorbeeld besluiten dat een groep gebruikers moet hebben beperkte machtigingen voor het abonnement, maar groter toegang tot een bepaalde dashboard. U toewijzen gebruikers aan een rol voor dit dashboard. 

## <a name="publish-dashboard"></a>dashboard publiceren
Stel dat u klaar bent met het configureren van een dashboard dat u wilt delen met een groep gebruikers in uw abonnement. De volgende stappen uit een aangepaste groep met de naam van de opslag Managers weer, maar u kunt uw groep naam wat u wilt. Zie voor meer informatie over het maken van een Active Directory-groep en gebruikers toevoegen aan die groep [groepen beheren in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

1. Selecteer in het dashboard **Share**.
   
     ![Selecteer share](./media/azure-portal-dashboard-share-access/select-share.png)
2. Alvorens toegang toe te wijzen, moet u het dashboard te publiceren. Standaard wordt het dashboard worden gepubliceerd naar een resourcegroep met de naam **dashboards**. Selecteer **publiceren**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Uw dashboard is nu gepubliceerd. Als de machtigingen die zijn overgenomen uit het abonnement geschikt zijn, hoeft u niet verder te doen. Andere gebruikers in uw organisatie zich voor toegang tot en het dashboard op basis van hun abonnement niveau rol wijzigen. Echter, voor deze zelfstudie gaan we een groep gebruikers aan een rol toewijzen voor dit dashboard.

## <a name="assign-access-to-a-dashboard"></a>Toegang tot een dashboard toewijzen
1. Na het publiceren van het dashboard, selecteert u **gebruikers beheren**.
   
     ![gebruikers beheren](./media/azure-portal-dashboard-share-access/manage-users.png)
2. U ziet een lijst met bestaande gebruikers die een rol voor dit dashboard al zijn toegewezen. De lijst met bestaande gebruikers is anders dan in de onderstaande afbeelding. Zeer waarschijnlijk worden de toewijzingen overgenomen van het abonnement. Selecteer een nieuwe gebruiker of groep wilt toevoegen, **toevoegen**.
   
     ![gebruiker toevoegen](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Selecteer de rol met de machtigingen die u wilt verlenen. Selecteer voor dit voorbeeld **Inzender**.
   
     ![rol selecteren](./media/azure-portal-dashboard-share-access/select-role.png)
4. Selecteer de gebruiker of groep die u wilt toewijzen aan de rol. Als u de gebruiker of groep die u in de lijst zoekt niet ziet, gebruik het zoekvak. Uw lijst met beschikbare groepen zijn afhankelijk van de groepen die u hebt gemaakt in uw Active Directory.
   
     ![gebruiker selecteren](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Wanneer u klaar bent met het toevoegen van gebruikers of groepen selecteren **OK**. 
6. De nieuwe toewijzing wordt toegevoegd aan de lijst met gebruikers. U ziet dat de **toegang** wordt vermeld als **toegewezen** plaats **overgenomen**.
   
     ![toegewezen rollen](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een lijst met rollen [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md).
* Zie voor meer informatie over het beheren van resources, [beheren Azure-resources via de portal](resource-group-portal.md).

