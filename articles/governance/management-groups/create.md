---
title: Beheergroepen voor het ordenen van Azure-resources maken
description: Informatie over het maken van Azure-beheergroepen voor het beheren van meerdere resources.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 699a9b7a371a004213419567d0672f56b5365598
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620111"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Beheergroepen voor resource organisatie en beheer maken

Beheergroepen zijn containers waarmee u toegang, beleid en naleving beheren voor meerdere abonnementen. Maken van deze containers voor het bouwen van een effectief en efficiënt-hiërarchie die kan worden gebruikt met [Azure Policy](../policy/overview.md) en [Azure rol-gebaseerd toegangsbeheer](../../role-based-access-control/overview.md). Zie voor meer informatie over beheergroepen [organiseren van uw resources met Azure-beheergroepen](overview.md).

De eerste beheergroep die u hebt gemaakt in de map kan tot 15 minuten duren. Er zijn processen die worden uitgevoerd van de eerste keer voor het instellen van de beheerservice van groepen in Azure voor uw directory. U ontvangt een melding wanneer het proces voltooid is.

## <a name="create-a-management-group"></a>Een beheergroep maken

U kunt de beheergroep maken met behulp van de portal, PowerShell of Azure CLI. Op dit moment niet u Resource Manager-sjablonen gebruiken voor het maken van beheergroepen.

### <a name="create-in-portal"></a>In de portal maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **alle services** > **beheergroepen**.

1. Selecteer op de hoofdpagina van **nieuwe beheergroep**.

   ![Main-groep](./media/main.png)

1. Vul in het veld management groep-ID.

   - De **beheergroep-ID** is de unieke id van map die wordt gebruikt voor het verzenden van opdrachten in deze beheergroep. Deze id kan niet worden bewerkt nadat is gemaakt, omdat deze in het hele systeem Azure gebruikt om te identificeren van deze groep.
   - Het weergavenaamveld is de naam die wordt weergegeven in Azure portal. Een afzonderlijke weergavenaam is een optioneel veld bij het maken van het beheer te groeperen en op elk gewenst moment kan worden gewijzigd.  

   ![Maken](./media/create_context_menu.png)  

1. Selecteer **Opslaan**.

### <a name="create-in-powershell"></a>Maken in PowerShell

In PowerShell, moet u de cmdlet New-AzureRmManagementGroup gebruiken:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso'
```

De **GroupName** is een unieke id wordt gemaakt. Deze ID wordt gebruikt door andere opdrachten om te verwijzen naar deze groep en kan later worden gewijzigd.

Als u wilt dat de beheergroep die u wilt weergeven van een andere naam in Azure portal, voegt u de **DisplayName** parameter met de tekenreeks. Als u wilt maken van een beheergroep met de groepsnaam van Contoso en de weergavenaam van 'Contoso groeperen', gebruikt u bijvoorbeeld de volgende cmdlet:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId 'ContosoTenant'
```

Gebruik de **ParentId** parameter voor deze beheergroep hebt gemaakt onder een ander management.

### <a name="create-in-azure-cli"></a>Maken in Azure CLI

Op de Azure CLI, gebruikt u de az-opdracht account management-groep maken.

```azurecli-interactive
az account management-group create --name 'Contoso'
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over beheergroepen gaat u naar:

- [Beheergroepen maken om Azure-resources te ordenen](create.md)
- [Uw beheergroepen wijzigen, verwijderen of beheren](manage.md)
- [Beheergroepen bekijken via de module voor Azure PowerShell-resources](https://aka.ms/mgPSdocs)
- [Beheergroepen bekijken via de REST-API](https://aka.ms/mgAPIdocs)
- [Beheergroepen bekijken via de Azure-CLI](https://aka.ms/mgclidoc)
