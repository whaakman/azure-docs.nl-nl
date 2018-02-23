---
title: Beheerders van de server in Azure Analysis Services beheren | Microsoft Docs
description: Informatie over het beheren van server-beheerders voor een Analysis Services-server in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: d90f1e3df8f5934d5c334ec72b5726f105842ca1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
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
[Op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-what-is.md)  

