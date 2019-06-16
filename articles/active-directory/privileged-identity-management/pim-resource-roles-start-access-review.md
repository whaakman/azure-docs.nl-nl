---
title: Een toegangsbeoordeling van Azure-resource-rollen in PIM - Azure Active Directory maken | Microsoft Docs
description: Informatie over het maken van een toegangscontrole van Azure-resource-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 78e4de5bbc56f95c0e903b1dac4e8481373716f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65143517"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-pim"></a>Een toegangsbeoordeling van Azure-resource-rollen in PIM maken

Toegang tot Azure-resourcerollen bevoegde werknemers gewijzigd na verloop van tijd. Als u wilt verminderen het risico dat samenhangt met verouderde roltoewijzingen, moet u regelmatig toegang controleren. U kunt Azure Active Directory (Azure AD) Privileged Identity Management (PIM) gebruiken om te maken van toegangsbeoordelingen voor Azure-resourcerollen in beschermde modus. U kunt ook configureren terugkerende toegangsbeoordelingen die automatisch worden uitgevoerd.

In dit artikel wordt beschreven hoe u een of meer beoordelingen voor bevoegde Azure resource-rollen maken.

## <a name="prerequisites"></a>Vereisten

- [Beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Open toegangsbeoordelingen

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) aan een gebruiker die deel uitmaakt van de rol beheerder met bevoorrechte rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik in het menu links op **Azure-resources**.

1. Klik op de resource die u beheren wilt, zoals een abonnement of beheergroep-groep.

1. Klik onder beheren, op **Toegangsbeoordelingen**.

    ![Azure-resources - toegangsbeoordelingen](./media/pim-resource-roles-start-access-review/access-reviews.png)


[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]


## <a name="start-the-access-review"></a>Start de toegangsbeoordeling

Nadat u de instellingen voor een overzicht van access hebt opgegeven, klikt u op **Start**. De toegangsbeoordeling wordt weergegeven in de lijst met een indicatie van de status ervan.

![Lijst met toegangsbeoordelingen](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

Standaard verzendt Azure AD een e-mailbericht naar de revisors kort nadat de evaluatie wordt gestart. Als u geen Azure AD-tenant het e-mailbericht verzenden, zorg er dan voor dat laten weten de revisoren een toegangsbeoordeling wordt gewacht om uit te voeren. U ze kunt weergeven, kunt u de instructies voor het [toegang tot Azure-resource-rollen beoordelen](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Beheren van de toegangsbeoordeling

U kunt de voortgang volgen de revisoren hun beoordelingen zijn voltooid op de **overzicht** pagina van de toegangsbeoordeling. Er is geen toegangsrechten worden gewijzigd in de map totdat de [beoordeling is voltooid](pim-resource-roles-complete-access-review.md).

![Toegangsbeoordelingen wordt uitgevoerd](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Als dit een eenmalige beoordeling, klikt u vervolgens of de beheerder van de toegangsbeoordeling, stopt de periode van de toegangsbeoordeling is via de stappen in [een toegangscontrole van Azure-resourcerollen voltooien](pim-resource-roles-complete-access-review.md) om te zien en de resultaten van toepassing.  

Voor het beheren van een reeks beoordelingen, gaat u naar de toegangsbeoordeling en u toekomstige voorvallen niet vinden in de geplande beoordelingen, en de einddatum bewerken of toevoegen/verwijderen revisoren dienovereenkomstig.

Op basis van uw selecties in **na voltooiing van de instellingen**, wordt automatisch toepassen worden uitgevoerd na de einddatum van de beoordeling of wanneer u de beoordeling handmatig stoppen. De status van de beoordeling wordt gewijzigd van **voltooid** via tussenliggende Staten zoals **toepassen** en tot slot naar status **toegepast**. U kunt verwachten om te zien van geweigerde gebruikers, indien van toepassing, wordt verwijderd van rollen in een paar minuten.

## <a name="next-steps"></a>Volgende stappen

- [Controleer de toegang tot Azure-resourcerollen](pim-resource-roles-perform-access-review.md)
- [Een toegangscontrole van Azure-resourcerollen voltooien](pim-resource-roles-complete-access-review.md)
- [Een toegangsbeoordeling van Azure AD-rollen maken](pim-how-to-start-security-review.md)
