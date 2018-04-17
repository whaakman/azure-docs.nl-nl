---
title: Beheerders van de server in Azure Analysis Services beheren | Microsoft Docs
description: Informatie over het beheren van server-beheerders voor een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: def09f2853f761f3fefca80f341e6cc0557bac86
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="manage-server-administrators"></a>Serverbeheerders beheren
Serverbeheerders moet een geldige gebruiker of groep in de Azure Active Directory (Azure AD) voor de tenant waarin de server zich bevindt. U kunt **Analysis Services-beheerders** voor uw server in Azure portal of servereigenschappen in SSMS serverbeheerders beheren. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Serverbeheerders toevoegen met behulp van Azure-portal
1. In de de portal voor uw server, klikt u op **Analysis Services-beheerders**.
2. In  **\<servername >-Analysis Services-beheerders**, klikt u op **toevoegen**.
3. In **serverbeheerders toevoegen**, selecteert u gebruikersaccounts in uw Azure AD of externe gebruikers uitnodigen door e-mailadres.

    ![Beheerders van de server in Azure-portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Serverbeheerders toevoegen met behulp van SSMS
1. Met de rechtermuisknop op de server > **eigenschappen**.
2. In **eigenschappen van Analysis Server**, klikt u op **beveiliging**.
3. Klik op **toevoegen**, en voer het e-mailadres voor een gebruiker of groep in uw Azure AD.
   
    ![Serverbeheerders in SSMS toevoegen](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Volgende stappen 
[Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md)  
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)  

