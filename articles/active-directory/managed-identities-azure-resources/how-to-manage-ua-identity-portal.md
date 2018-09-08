---
title: Over het beheren van een gebruiker toegewezen beheerde identiteit met Azure portal
description: Stapsgewijze beheerde instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen identiteit.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 6729bee9bfebd8e80ae3b791dc2a8a480ac8b525
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158719"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-portal"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen beheerde identiteit met Azure portal

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit met behulp van de Azure-Portal.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van een gebruiker toegewezen beheerde identiteit.

## <a name="create-a-user-assigned-managed-identity"></a>Maken van een gebruiker toegewezen beheerde identiteit

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement te maken van de gebruiker toegewezen beheerde identiteit.
2. Typ in het zoekvak *beheerde identiteiten*, en klikt u onder **Services**, klikt u op **beheerde identiteiten**.
3. Klik op **toevoegen** en voer waarden in de volgende velden onder **maken door gebruiker toegewezen beheerd** identiteit deelvenster:
   - **Resourcenaam**: dit is de naam voor de gebruiker toegewezen beheerde identiteit, bijvoorbeeld UAI1.
   - **Abonnement**: Kies het abonnement te maken van de gebruiker toegewezen beheerde identiteit onder
   - **Resourcegroep**: Maak een nieuwe resourcegroep voor uw gebruiker toegewezen beheerde identiteit bevatten of kies **gebruik bestaande** te maken van de gebruiker toegewezen beheerde identiteit in een bestaande resourcegroep.
   - **Locatie**: Kies een locatie voor het implementeren van de gebruiker toegewezen beheerde identiteit, bijvoorbeeld **VS-West**.
4. Klik op **Create**.

![Maken van een gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Lijst met door de gebruiker toegewezen identiteiten beheren

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de beheerde gebruiker toegewezen identiteiten weer te geven.
2. Typ in het zoekvak *beheerde identiteiten*, en klik onder Services op **beheerde identiteiten**.
3. Een lijst van de gebruiker toegewezen beheerde identiteit voor uw abonnement wordt geretourneerd.  Aan de details van een gebruiker toegewezen beheerde identiteit klikt u op de naam ervan bekijken.

![Lijst met door de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement te verwijderen van een gebruiker toegewezen beheerde identiteit.
2. Selecteer de gebruiker toegewezen beheerde identiteit en klik op **verwijderen**.
3. Kies in het bevestigingsvenster, **Ja**.

![Verwijderen van de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)