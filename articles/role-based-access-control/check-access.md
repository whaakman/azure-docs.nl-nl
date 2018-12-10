---
title: 'Quickstart: Rollen weergeven die met behulp van de Azure-portal zijn toegewezen aan een gebruiker | Microsoft Docs'
description: Meer informatie over het weergeven van RBAC-machtigingen (op rollen gebaseerd toegangsbeheer) die met behulp van de Azure-portal zijn toegewezen aan een gebruiker, groep, service-principal of beheerde identiteit.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641864"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Quickstart: Rollen weergeven die met behulp van de Azure-portal zijn toegewezen aan een gebruiker

U kunt de blade **Toegangsbeheer (IAM)** in [Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) gebruiken om de roltoewijzingen weer te geven voor meerdere gebruikers, groepen, service-principals en beheerde identiteiten, maar soms wilt u alleen snel de roltoewijzingen voor één gebruiker, groep, service-principal of beheerde identiteit bekijken. De eenvoudigste manier om dit te doen, is met de functie **Toegang controleren** in de Azure-portal.

## <a name="view-role-assignments"></a>Roltoewijzingen weergeven

Volg deze stappen om de roltoewijzingen weer te geven voor één gebruiker, groep, service-principal of beheerde identiteit in het bereik van het abonnement.

1. Klik in de Azure-portal op de optie **Alle services** en vervolgens op **Abonnementen**.

1. Klik op uw abonnement.

1. Klik op **Toegangsbeheer (IAM)**.

1. Klik op het tabblad **Toegang controleren**.

    ![Toegangsbeheer - Tabblad Toegang controleren](./media/check-access/access-control-check-access.png)

1. Selecteer in de lijst **Zoeken** het type beveiligings-principal waarvoor u de toegang wilt controleren.

1. Voer in het zoekvak een tekenreeks in om de map te doorzoeken op weergavenamen, e-mailadressen of object-id's.

    ![De toegangsselectielijst controleren](./media/check-access/check-access-select.png)

1. Klik op de beveiligings-principal om het deelvenster **Toewijzingen** te openen.

    ![Deelvenster Toewijzingen](./media/check-access/check-access-assignments.png)

    In dit deelvenster ziet u de rollen die zijn toegewezen aan de geselecteerde beveiligings-principal en het bereik. Als er een in dit bereik toewijzingen zijn geweigerd of zijn overgenomen, worden deze weergegeven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Toegang verlenen aan een gebruiker met behulp van RBAC en de Azure-portal](quickstart-assign-role-user-portal.md)
