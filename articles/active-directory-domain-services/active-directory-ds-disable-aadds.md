---
title: Azure Active Directory Domain Services uitschakelen | Microsoft Docs
description: Azure Active Directory Domain Services met Azure portal uitschakelen
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: maheshu
ms.openlocfilehash: d2db86dbd65e64af2e8643dc02e27701bfe61717
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34586421"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory Domain Services met Azure portal uitschakelen
In dit artikel leest u hoe de Azure portal gebruiken om te schakelen van Azure Active Directory (AD) voor uw Azure AD-directory-domeinservices.

> [!WARNING]
> **Verwijdering is permanent en kan niet ongedaan worden gemaakt.**
> Wees voorzichtig! Wanneer u het beheerde domein verwijderen:
  * Domeincontrollers voor het beheerde domein zijn ingericht ongedaan en verwijderd uit het virtuele netwerk.
  * Gegevens op het beheerde domein wordt permanent verwijderd. Dit omvat aangepaste organisatie-eenheden, groepsbeleidsobjecten, aangepaste DNS-records, service-principals, gmsa's enzovoort die u op het beheerde domein hebt gemaakt.
  * Computers die zijn toegevoegd aan het beheerde domein verliezen hun vertrouwensrelatie heeft met het domein en moeten worden verwijderd uit het domein.
  * U kan niet aanmelden bij deze machines met zakelijke AD-referenties. Gebruik in plaats daarvan de lokale beheerdersreferenties voor de machine.
Verwijderen van het beheerde domein niet verwijderen van uw Azure AD-directory of anders negatieve invloed hebben op de map.
>

Voer de volgende stappen uit om uw beheerde domein van Azure AD Domain Services verwijderen:
1. Navigeer naar de [Azure AD Domain Services-extensie](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in de Azure portal.
2. Klik op de naam van uw beheerde domein.

    ![Selecteer het domein verwijderen](./media/getting-started/domain-services-delete-select-domain.png)

3. Op de **overzicht** pagina, klikt u op de **verwijderen** knop.

    ![Domein verwijderen](./media/getting-started/domain-services-delete-domain.png)

4. Het verwijderen te bevestigen, typt u de DNS-domeinnaam van het beheerde domein. Klik op de **verwijderen** wanneer u klaar bent.

    ![Bevestiging van het domein verwijderen](./media/getting-started/domain-services-delete-domain-confirm.png)

Het beheerde domein wordt verwijderd in ongeveer 15-20 minuten.

Houd rekening met [feedback delen](active-directory-ds-contact-us.md) om u te helpen ons inzicht in hoe functies kunt u Azure AD Domain Services in de toekomst hebt gekozen. Deze feedback kunnen we de service beter aanpassen aan uw implementatievereisten en use cases ontwikkelen.
