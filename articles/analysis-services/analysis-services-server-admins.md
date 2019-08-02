---
title: Server beheerders beheren in Azure Analysis Services | Microsoft Docs
description: Meer informatie over het beheren van Server beheerders voor een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: eb5248d374fa6212398ad652b4db1496833473ae
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68696310"
---
# <a name="manage-server-administrators"></a>Serverbeheerders beheren

Server beheerders moeten een geldige gebruiker of beveiligings groep zijn in de Azure Active Directory (Azure AD) voor de Tenant waarin de server zich bevindt. U kunt **Analysis Services beheerders** voor uw server gebruiken in azure Portal, Server eigenschappen in SSMS, Power shell of rest API voor het beheren van Server beheerders. 

Voor **beveiligings groepen** moet een [e-mail bericht worden ingeschakeld](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-enabled-security-groups) waarvoor de `MailEnabled` eigenschap is ingesteld op. `True` Wanneer u `obj:groupid@tenantid`een groep op basis van een e-mail adres opgeeft.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Server beheerders toevoegen met behulp van Azure Portal

1. Klik in de portal, voor uw server, op **Analysis Services beheerders**.
2. Klik in  **\<server naam >-Analysis Services beheerders**op **toevoegen**.
3. Selecteer in **Server beheerders toevoegen**de optie gebruikers accounts uit uw Azure AD of externe gebruikers uitnodigen op e-mail adres.

    ![Server beheerders in Azure Portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Server beheerders toevoegen met behulp van SSMS

1. Klik met de rechter muisknop op de > **Eigenschappen**van de server.
2. Klik in **Analyseserver eigenschappen**op **beveiliging**.
3. Klik op **toevoegen**en voer het e-mail adres voor een gebruiker of groep in uw Azure AD in.
   
    ![Server beheerders toevoegen in SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Gebruik de cmdlet [New-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) om de Administrator-para meter op te geven bij het maken van een nieuwe server. <br>
Gebruik de cmdlet [set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) om de Administrator-para meter voor een bestaande server te wijzigen.

## <a name="rest-api"></a>REST-API

Gebruik [Create](https://docs.microsoft.com/rest/api/analysisservices/servers/create) om de eigenschap asAdministrator op te geven bij het maken van een nieuwe server. <br>
Gebruik [Update](https://docs.microsoft.com/rest/api/analysisservices/servers/update) om de eigenschap asAdministrator op te geven bij het wijzigen van een bestaande server. <br>



## <a name="next-steps"></a>Volgende stappen 

[Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md)  
[Database rollen en-gebruikers beheren](analysis-services-database-users.md)  
[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)  

