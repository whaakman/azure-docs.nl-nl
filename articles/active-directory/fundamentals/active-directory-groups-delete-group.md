---
title: 'Verwijderen van een groep: Azure Active Directory | Microsoft Docs'
description: Instructies over het verwijderen van een groep met behulp van Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdba55e0655a13e65e403f5da73fcb69db5dbca5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561921"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Een groep met behulp van Azure Active Directory verwijderen
U kunt een groep Azure Active Directory (Azure AD) voor een aantal redenen verwijderen, maar meestal dit is omdat u:

- Het groeps **type** is onjuist ingesteld op de verkeerde optie.

- Er is per ongeluk een onjuiste of dubbele groep gemaakt. 

- De groep is niet meer nodig.

## <a name="to-delete-a-group"></a>Een groep verwijderen
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met het account van een globale administrator voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

3. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt verwijderen. Voor deze stappen, gebruiken we **MDM-beleid - Oost**.

    ![Pagina groepen-alle groepen in de naam van de groep is gemarkeerd](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Op de **MDM-beleid - Oost-overzicht** pagina en selecteer vervolgens **verwijderen**.

    De groep wordt verwijderd uit uw Azure Active Directory-tenant.

    ![Beleid voor MDM - Oost-overzichtspagina, optie voor het verwijderen is gemarkeerd](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Volgende stappen

- Als u een groep per ongeluk hebt verwijderd, kunt u deze opnieuw maken. Zie voor meer informatie, [hoe u een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md).

- Als u een Office 365-groep per ongeluk verwijdert, is het mogelijk dat u deze kunt herstellen. Zie voor meer informatie, [herstellen van een verwijderde Office 365-groep](../users-groups-roles/groups-restore-deleted.md).
