---
title: Beheren van server-beheerders in Azure Analysis Services | Microsoft Docs
description: Informatie over het beheren van server-beheerders voor een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 25eb111871bbe3b18f59b0d6c123c72f3e55c859
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737107"
---
# <a name="manage-server-administrators"></a>Serverbeheerders beheren

Serverbeheerders moet een geldige gebruiker of beveiligingsgroep groep in de Azure Active Directory (Azure AD) voor de tenant waarin de server zich bevindt. U kunt **Analysis Services-beheerder** voor uw server in Azure portal, de eigenschappen van de Server in SSMS, PowerShell of REST-API voor het beheren van server-beheerders. 

> [!NOTE]
> Beveiligingsgroepen moeten hebben de `MailEnabled` eigenschap ingesteld op `True`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Serverbeheerders toevoegen met behulp van Azure portal

1. Klik in de portal voor uw server op **Analysis Services-beheerder**.
2. In  **\<servername >-Analysis Services-beheerder**, klikt u op **toevoegen**.
3. In **serverbeheerders toevoegen**, selecteert u gebruikersaccounts uit uw Azure AD of externe gebruikers uitnodigen door e-mailadres.

    ![Server-beheerders in Azure portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Serverbeheerders toevoegen met behulp van SSMS

1. Met de rechtermuisknop op de server > **eigenschappen**.
2. In **eigenschappen van analyseserver**, klikt u op **Security**.
3. Klik op **toevoegen**, en voer het e-mailadres voor een gebruiker of groep in uw Azure AD.
   
    ![Serverbeheerders toevoegen in SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet om op te geven van de parameter beheerder bij het maken van een nieuwe server. <br>
Gebruik [Set AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet om te wijzigen van de beheerder de parameter voor een bestaande server.

## <a name="rest-api"></a>REST-API

Gebruik [maken](https://docs.microsoft.com/rest/api/analysisservices/servers/create) om op te geven van de eigenschap asAdministrator bij het maken van een nieuwe server. <br>
Gebruik [Update](https://docs.microsoft.com/rest/api/analysisservices/servers/update) om op te geven van de eigenschap asAdministrator bij het wijzigen van een bestaande server. <br>



## <a name="next-steps"></a>Volgende stappen 

[Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md)  
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)  

