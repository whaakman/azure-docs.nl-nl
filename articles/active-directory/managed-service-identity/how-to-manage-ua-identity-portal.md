---
title: Over het beheren van een gebruiker toegewezen beheerde identiteit met Azure portal
description: Stapsgewijze instructies over het maken, weergeven en verwijderen van een gebruiker toegewezen beheerde identiteit.
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
ms.openlocfilehash: bd6327aa5c16d57c550025667659f9245a5b0b65
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058072"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-portal"></a>Maken, weergeven of verwijderen van een gebruiker toegewezen identiteit met Azure portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder de referenties in uw code. 

In dit artikel leert u hoe u kunt maken, weergeven en verwijderen van een gebruiker toegewezen identiteit met behulp van de Azure-Portal.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) (lijst) rol te maken, lezen, bijwerken en verwijderen van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol te lezen (lijst) de eigenschappen van de identiteit van een gebruiker toegewezen.

## <a name="create-a-user-assigned-managed-identity"></a>Maak een beheerde identiteit toegewezen gebruiker

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) beheerde identiteit met een account dat is gekoppeld aan het Azure-abonnement om te maken van de gebruiker toegewezen.
2. Typ in het zoekvak *beheerde identiteiten*, en klikt u onder **Services**, klikt u op **beheerde identiteiten**.
3. Klik op **toevoegen** en voer waarden in de volgende velden onder **maken door gebruiker toegewezen beheerd** identiteit deelvenster:
   - **Resourcenaam**: dit is de naam voor de gebruiker toegewezen identiteit, bijvoorbeeld UAI1 die worden beheerd.
   - **Abonnement**: Kies het abonnement te maken van de gebruiker beheerde identiteit onder toegewezen
   - **Resourcegroep**: Maak een nieuwe resourcegroep voor uw gebruiker toegewezen identiteit bevatten of kies **gebruik bestaande** toegewezen om te maken van de gebruiker beheerde identiteit in een bestaande resourcegroep.
   - **Locatie**: Kies een locatie voor het implementeren van de gebruiker beheerde identiteit, bijvoorbeeld toegewezen **VS-West**.
4. Klik op **Create**.

![Maak een beheerde identiteit toegewezen gebruiker](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-identities"></a>Lijst met door gebruiker toegewezen identiteiten

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) beheerde identiteiten met behulp van een account dat is gekoppeld aan het Azure-abonnement om een lijst van de gebruiker te worden toegewezen.
2. Typ in het zoekvak *beheerde identiteiten*, en klik onder Services op **beheerde identiteiten**.
3. Een lijst van de gebruiker toegewezen beheerde identiteiten voor uw abonnement wordt geretourneerd.  Klik op de naam voor de details van een gebruiker toegewezen.

![Lijst met door gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-identity"></a>Verwijderen van een gebruiker toegewezen identiteit

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) beheerde identiteit met behulp van een account dat is gekoppeld aan het Azure-abonnement te verwijderen van een gebruiker toegewezen.
2. Selecteer de gebruiker toegewezen identiteit en klik op **verwijderen**.
3. Kies in het bevestigingsvenster, **Ja**.

![Door de gebruiker toegewezen beheerde identiteit verwijderen](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)