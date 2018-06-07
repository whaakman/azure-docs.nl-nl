---
title: Een service-principal toevoegen aan Azure Analysis Services-serverfunctie admin | Microsoft Docs
description: Informatie over het toevoegen van een automation-service-principal voor de serverrol admin
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8840dd9a8a8e8ba1f78d461824ac0cdce32b2ac9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596767"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Een service-principal toevoegen aan de rol van server 

 Zonder toezicht PowerShell om taken te automatiseren, een service-principal moet hebben **serverbeheerder** machtigingen op de Analysis Services-server wordt beheerd. Dit artikel wordt beschreven hoe u een service-principal toevoegt aan de serverfunctie voor beheerders op een AS Azure-server.

## <a name="before-you-begin"></a>Voordat u begint
Voordat deze taak is voltooid, moet u een service-principal die is geregistreerd in Azure Active Directory hebben.

[Maken van de service-principal - Azure-portal](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Service-principal maken - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Vereiste machtigingen
U moet hebben om deze taak [serverbeheerder](analysis-services-server-admins.md) machtigingen op de AS Azure-server. 

## <a name="add-service-principal-to-server-administrators-role"></a>Service-principal voor beheerders serverrol toevoegen

1. In SSMS, verbinding maken met uw Azure-AS-server.
2. In **servereigenschappen** > **beveiliging**, klikt u op **toevoegen**.
3. In **gebruiker of groep selecteren**, zoek naar uw app geregistreerd op naam, selecteren en klik vervolgens op **toevoegen**.

    ![Zoeken naar service-principalaccount](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Controleer of de service-principal account-ID en klik vervolgens op **OK**.
    
    ![Zoeken naar service-principalaccount](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Serverbewerkingen met behulp van cmdlets AzureRm, service principal actieve scheduler moet ook deel uitmaken van de **eigenaar** rol voor de resource in [gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>Gerelateerde informatie

* [SQL Server PowerShell-Module downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Downloaden van SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


