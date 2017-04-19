---
title: Op rollen gebaseerd toegangsbeheer in de Azure Portal | Microsoft Docs
description: Ga aan de slag met toegangsbeheer met op rollen gebaseerd toegangsbeheer in de Azure Portal. Gebruik roltoewijzingen om machtigingen aan uw resources toe te wijzen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 9fb0fdf1a2a48c5c9daf7d0ec22e85221bfb6b28
ms.lasthandoff: 04/07/2017


---
# <a name="use-role-based-access-control-to-manage-access-to-your-azure-subscription-resources"></a>Op rollen gebaseerd toegangsbeheer gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement
> [!div class="op_single_selector"]
> * [Toegang beheren per gebruiker of groep](role-based-access-control-manage-assignments.md)
> * [Toegang beheren per resource](role-based-access-control-configure.md)

Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren. In dit artikel leest u hoe u met RBAC kunt werken in de Azure Portal. Zie [Wat is op rollen gebaseerd toegangsbeheer](role-based-access-control-what-is.md) als u meer informatie wilt over het beheren van toegang met RBAC.

Binnen elk abonnement geldt een limiet van 2000 roltoewijzingen. 

## <a name="view-access"></a>Toegang voor weergeven
U kunt zien wie toegang heeft tot een resource, resourcegroep of abonnement op de hoofdblade in de [Azure Portal](https://portal.azure.com). We willen bijvoorbeeld zien wie toegang heeft tot een van onze resourcegroepen:

1. Selecteer **Resourcegroepen** op de linkernavigatiebalk.  
    ![Resourcegroepen - pictogram](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Selecteer de naam van de resourcegroep op de blade **Resourcegroepen**.
3. Selecteer **Toegangsbeheer (IAM)** in het menu links.  
4. Op de blade Toegangsbeheer worden alle gebruikers, groepen en toepassingen weergegeven aan wie/waaraan toegang is verleend tot de resourcegroep.  
   
    ![Schermafbeelding van de blade Gebruikers - overgenomen en toegewezen toegang](./media/role-based-access-control-configure/view-access.png)

U ziet dat aan bepaalde gebruikers de toegang is **Toegewezen** en dat voor andere gebruikers de toegang is **Overgenomen**. De toegang is specifiek toegewezen aan de resourcegroep of is overgenomen van een toewijzing aan het bovenliggende abonnement.

> [!NOTE]
> Klassieke abonnementsbeheerders en co-beheerders worden beschouwd als eigenaar van het abonnement in het nieuwe RBAC-model.

## <a name="add-access"></a>Toegang voor toevoegen
U verleent toegang vanuit de resource en resourcegroep die of het abonnement dat het bereik van de roltoewijzing is.

1. Selecteer **Toevoegen** op de blade Toegangsbeheer.  
2. Selecteer de rol die u wilt toewijzen op de blade **Rol selecteren**.
3. Selecteer de gebruiker, groep of toepassing in de directory aan wie/waaraan u toegang wilt verlenen. U kunt zoeken in de directory met weergavenamen, e-mailadressen en object-id's.  
   
    ![Schermafbeelding van de blade Gebruikers toevoegen - zoeken](./media/role-based-access-control-configure/grant-access2.png)
4. Selecteer **OK** om de toewijzing te maken. Met de pop-up **Gebruiker toevoegen** wordt de voortgang bijgehouden.  
    ![Schermafbeelding van de voortgangsbalk Gebruiker toevoegen](./media/role-based-access-control-configure/addinguser_popup.png)

Nadat u een roltoewijzing hebt toegevoegd, wordt deze weergegeven op de blade **Gebruikers**.

## <a name="remove-access"></a>Toegang verwijderen
1. Gebruik de selectievakjes op de blade Toegangsbeheer om een of meer roltoewijzingen te selecteren.
2. Selecteer **Verwijderen**.  
3. Er verschijnt een venster waarin u wordt gevraagd de actie te bevestigen. Selecteer **Ja** om de roltoewijzingen te verwijderen.

Overgenomen toewijzingen kunnen niet worden verwijderd. Als u een overgenomen toewijzing wilt verwijderen, dient u dit te doen in het bereik waarin de roltoewijzing is gemaakt. In de kolom **Bereik**, naast **Overgenomen**, vindt u een koppeling waarmee u naar de resources gaat waaraan deze rol is toegewezen. Ga naar de resource die hier wordt weergegeven om de roltoewijzing te verwijderen.

![Schermafbeelding van de blade Gebruikers - bij overgenomen toegang is knop Verwijderen uitgeschakeld](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere hulpprogramma's om toegang te beheren
U kunt rollen toewijzen en toegang beheren met Azure RBAC-opdrachten met andere hulpprogramma's dan de Azure Portal.  Volg de koppelingen voor meer informatie over de vereisten en om aan de slag te gaan met de Azure RBAC-opdrachten.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Azure-opdrachtregelinterface](role-based-access-control-manage-access-azure-cli.md)
* [REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Volgende stappen
* [Een geschiedenisrapport voor gewijzigde toegang maken](role-based-access-control-access-change-history-report.md)
* Zie de [Ingebouwde RBAC-rollen](role-based-access-built-in-roles.md)
* Definieer uw eigen [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)


