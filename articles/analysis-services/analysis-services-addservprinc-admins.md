---
title: Een service-principal toevoegen aan de beheerdersrol van Azure Analysis Services-server | Microsoft Docs
description: Leer hoe u een automation-service-principal toevoegen aan de beheerdersrol van server
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 779a202fccd3ff56c174ebc1ebbf3c4adfdd8c7b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441736"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Een service-principal toevoegen aan de rol van de server-beheerder 

 Zonder toezicht PowerShell om taken te automatiseren, een service-principal moet hebben **serverbeheerder** bevoegdheden op de Analysis Services-server wordt beheerd. In dit artikel wordt beschreven hoe u een service-principal toevoegen aan de serverfunctie voor beheerders op een Azure-AS-server.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u deze taak is voltooid, moet u een service-principal die is geregistreerd bij Azure Active Directory hebben.

[Service-principal - Azure portal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Service-principal maken - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Vereiste machtigingen
Als u wilt deze taak hebt voltooid, hebt u [serverbeheerder](analysis-services-server-admins.md) machtigingen op de AS Azure-server. 

## <a name="add-service-principal-to-server-administrators-role"></a>Service-principal toevoegen aan beheerders-serverfunctie

1. In SSMS verbinding maken met uw Azure-AS-server.
2. In **servereigenschappen** > **Security**, klikt u op **toevoegen**.
3. In **selecteert u een gebruiker of groep**, zoek naar uw geregistreerde app met de naam, selecteer en klik vervolgens op **toevoegen**.

    ![Zoek naar service-principalaccount](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Controleer of de service-principal-account-ID en klik vervolgens op **OK**.
    
    ![Zoek naar service-principalaccount](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Service principal actieve scheduler moet voor serverbewerkingen met behulp van de AzureRm-cmdlets, ook behoren tot de **eigenaar** rol voor de resource in [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


