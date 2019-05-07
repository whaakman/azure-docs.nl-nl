---
title: Een toegangsbeoordeling van Azure AD-rollen in PIM - Azure Active Directory maken | Microsoft Docs
description: Informatie over het maken van een toegangscontrole van Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/27/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a0680ddf2c9e654455933bf09699ab81e8ab65d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141838"
---
# <a name="create-an-access-review-of-azure-ad-roles-in-pim"></a>Een toegangsbeoordeling van Azure AD-rollen in PIM maken

Toegang tot Azure AD-rollen voor werknemers wijzigingen na verloop van tijd in beschermde modus. Als u wilt verminderen het risico dat samenhangt met verouderde roltoewijzingen, moet u regelmatig toegang controleren. U kunt Azure Active Directory (Azure AD) Privileged Identity Management (PIM) gebruiken om te maken van toegangsbeoordelingen voor Azure AD-rollen in beschermde modus. U kunt ook configureren terugkerende toegangsbeoordelingen die automatisch worden uitgevoerd.

In dit artikel wordt beschreven hoe u een of meer toegangsbeoordelingen maken voor Azure AD-rollen in beschermde modus.

## <a name="prerequisites"></a>Vereisten

- [Beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Open toegangsbeoordelingen

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) aan een gebruiker die deel uitmaakt van de rol beheerder met bevoorrechte rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik in het menu links op **Azure AD-rollen** en klik vervolgens op **Toegangsbeoordelingen**.

1. Klik onder beheren, op **Toegangsbeoordelingen**.

    ![Functies van Azure AD - toegangsbeoordelingen](./media/pim-how-to-start-security-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Start de toegangsbeoordeling

Nadat u de instellingen voor een overzicht van access hebt opgegeven, klikt u op **Start**. De toegangsbeoordeling wordt weergegeven in de lijst met een indicatie van de status ervan.

![Lijst met toegangsbeoordelingen](./media/pim-how-to-start-security-review/access-reviews-list.png)

Standaard verzendt Azure AD een e-mailbericht naar de revisors kort nadat de evaluatie wordt gestart. Als u geen Azure AD-tenant het e-mailbericht verzenden, zorg er dan voor dat laten weten de revisoren een toegangsbeoordeling wordt gewacht om uit te voeren. U ze kunt weergeven, kunt u de instructies voor het [toegang aan Azure AD-rollen beoordelen](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Beheren van de toegangsbeoordeling

U kunt de voortgang volgen de revisoren hun beoordelingen zijn voltooid op de **overzicht** pagina van de toegangsbeoordeling. Er is geen toegangsrechten worden gewijzigd in de map totdat de [beoordeling is voltooid](pim-how-to-complete-review.md).

![Toegangsbeoordelingen wordt uitgevoerd](./media/pim-how-to-start-security-review/access-review-overview.png)

Als dit een eenmalige beoordeling, klikt u vervolgens of de beheerder van de toegangsbeoordeling, stopt de periode van de toegangsbeoordeling is via de stappen in [een toegangscontrole van Azure AD-rollen voltooien](pim-how-to-complete-review.md) om te zien en de resultaten van toepassing.  

Voor het beheren van een reeks beoordelingen, gaat u naar de toegangsbeoordeling en u toekomstige voorvallen niet vinden in de geplande beoordelingen, en de einddatum bewerken of toevoegen/verwijderen revisoren dienovereenkomstig.

Op basis van uw selecties in **na voltooiing van de instellingen**, wordt automatisch toepassen worden uitgevoerd na de einddatum van de beoordeling of wanneer u de beoordeling handmatig stoppen. De status van de beoordeling wordt gewijzigd van **voltooid** via tussenliggende Staten zoals **toepassen** en tot slot naar status **toegepast**. U kunt verwachten om te zien van geweigerde gebruikers, indien van toepassing, wordt verwijderd van rollen in een paar minuten.

## <a name="next-steps"></a>Volgende stappen

- [Controleer de toegang tot Azure AD-rollen](pim-how-to-perform-security-review.md)
- [Een toegangscontrole van Azure AD-rollen voltooien](pim-how-to-complete-review.md)
- [Een toegangsbeoordeling van Azure-resource-rollen maken](pim-resource-roles-start-access-review.md)
