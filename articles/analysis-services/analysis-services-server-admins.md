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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 6a42baf9fd880264e4130c009f27935a4743de21
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="manage-server-administrators"></a>Serverbeheerders beheren
Serverbeheerders moet een geldige gebruiker of groep in de Azure Active Directory (Azure AD) voor de tenant waarin de server zich bevindt. U kunt **Analysis Services-beheerders** in de besturingselementblade voor uw server in Azure-portal of servereigenschappen in SSMS serverbeheerders beheren. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Serverbeheerders toevoegen met behulp van Azure-portal
1. Klik op de besturingselementblade voor uw server **Analysis Services-beheerders**.
2. In de  **\<servername >-Analysis Services-beheerders** blade, klikt u op **toevoegen**.
3. In de **serverbeheerders toevoegen** blade, selecteert u gebruikersaccounts in uw Azure AD of externe gebruikers uitnodigen door e-mailadres.

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

