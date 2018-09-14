---
title: Het verwijderen van een groep met behulp van Azure Active Directory | Microsoft Docs
description: Informatie over het verwijderen van een groep met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 014fe487d23a6c75e94ca2708ed15044bd6cf53b
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574323"
---
# <a name="how-to-delete-a-group-using-azure-active-directory"></a>Hoe: een groep met behulp van Azure Active Directory verwijderen
U kunt een groep voor een willekeurig aantal redenen verwijderen, maar meestal dit is omdat u:

- Onjuist ingestelde de **groepstype** aan de verkeerde optie

- De verkeerde of een dubbele groep per ongeluk hebt gemaakt 

- De groep niet langer nodig hebt

## <a name="to-delete-a-group"></a>Een groep verwijderen
1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount voor de map.

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **groepen**.

3. Uit de **groepen - alle groepen** pagina, zoek en selecteer de groep die u wilt verwijderen. Voor deze stappen, gebruiken we **MDM-beleid - Oost**.

    ![Pagina groepen-alle groepen in de naam van de groep is gemarkeerd](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Op de **MDM-beleid - Oost-overzicht** pagina en selecteer vervolgens **verwijderen**.

    De groep wordt verwijderd uit uw Azure Active Directory-tenant.

    ![Beleid voor MDM - Oost-overzichtspagina, optie voor het verwijderen is gemarkeerd](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Volgende stappen

- Als u een groep per ongeluk hebt verwijderd, kunt u deze opnieuw maken. Zie voor meer informatie, [hoe u een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md).

- Als u een Office 365-groep per ongeluk verwijdert, is het mogelijk dat u deze kunt herstellen. Zie voor meer informatie, [herstellen van een verwijderde Office 365-groep](../users-groups-roles/groups-restore-deleted.md).
