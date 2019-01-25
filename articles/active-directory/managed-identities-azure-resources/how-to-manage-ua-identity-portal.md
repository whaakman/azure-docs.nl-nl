---
title: Over het beheren van een gebruiker toegewezen beheerde identiteit met Azure portal
description: Stapsgewijze instructies over het maken, weergeven, verwijderen en een rol toewijzen aan een gebruiker toegewezen beheerde identiteit.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: priyamo
ms.openlocfilehash: ea7a495fc7e39afeaab4e27c97b950704995e2db
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54886625"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Maken, weergeven, verwijderen of een rol toewijzen aan een gebruiker toegewezen beheerde identiteit met Azure portal

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u te maken, weergeven, verwijderen of een rol toewijzen aan een gebruiker toegewezen beheerde identiteit met behulp van de Azure-Portal.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken

Voor het maken van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement te maken van de gebruiker toegewezen beheerde identiteit.
2. Typ in het zoekvak *beheerde identiteiten*, en klikt u onder **Services**, klikt u op **beheerde identiteiten**.
3. Klik op **toevoegen** en voer waarden in de volgende velden onder **maken door gebruiker toegewezen beheerd** identiteit deelvenster:
   - **Resourcenaam**: Dit is de naam voor de gebruiker toegewezen beheerde identiteit, bijvoorbeeld UAI1.
   - **Abonnement**: Kies het abonnement te maken van de gebruiker toegewezen beheerde identiteit onder
   - **Resourcegroep**: Maak een nieuwe resourcegroep voor uw gebruiker toegewezen beheerde identiteit bevatten of kies **gebruik bestaande** te maken van de gebruiker toegewezen beheerde identiteit in een bestaande resourcegroep.
   - **Locatie**: Kies een locatie voor het implementeren van de gebruiker toegewezen beheerde identiteit, bijvoorbeeld **VS-West**.
4. Klik op **Create**.

![Een door de gebruiker toegewezen beheerde identiteit maken](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

Als u wilt lezen/lijst met een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) of [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de beheerde gebruiker toegewezen identiteiten weer te geven.
2. Typ in het zoekvak *beheerde identiteiten*, en klik onder Services op **beheerde identiteiten**.
3. Een lijst van de gebruiker toegewezen beheerde identiteit voor uw abonnement wordt geretourneerd.  Aan de details van een gebruiker toegewezen beheerde identiteit klikt u op de naam ervan bekijken.

![Lijst met door de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

Als u wilt verwijderen van een gebruiker toegewezen beheerde identiteit, uw account moet de [beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) roltoewijzing.

Verwijderen van een gebruiker toegewezen identiteit wordt niet verwijderd uit de virtuele machine of deze is toegewezen aan resource.  De gebruiker toegewezen identiteit verwijderen uit een VM-raadpleegt [verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#remove-a-user-assigned-managed-identity-from-a-vm).

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement te verwijderen van een gebruiker toegewezen beheerde identiteit.
2. Selecteer de gebruiker toegewezen beheerde identiteit en klik op **verwijderen**.
3. Kies in het bevestigingsvenster, **Ja**.

![Verwijderen van de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Een rol toewijzen aan een gebruiker toegewezen beheerde identiteit 

Als u wilt een rol toewijzen aan een gebruiker toegewezen beheerde identiteit, uw account moet de [Administrator voor gebruikerstoegang](/azure/role-based-access-control/built-in-roles#user-access-administrator) roltoewijzing.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de beheerde gebruiker toegewezen identiteiten weer te geven.
2. Typ in het zoekvak *beheerde identiteiten*, en klik onder Services op **beheerde identiteiten**.
3. Een lijst van de gebruiker toegewezen beheerde identiteit voor uw abonnement wordt geretourneerd.  Selecteer de op de gebruiker toegewezen beheerde identiteit die u wilt een rol toe te wijzen.
4. Selecteer **toegangsbeheer (IAM)** en selecteer vervolgens **roltoewijzing toevoegen**.

   ![Begin van de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Configureer de volgende waarden in het tabblad toevoegen rol toewijzing en klik vervolgens op **opslaan**:
   - **Rol** -de rol toewijzen
   - **Toegang toewijzen aan** -beheerde identiteit van de resource toe aan de gebruiker toegewezen toewijzen
   - **Selecteer** -het lid om toegang te verlenen
   
   ![De gebruiker toegewezen beheerde identiteit IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  