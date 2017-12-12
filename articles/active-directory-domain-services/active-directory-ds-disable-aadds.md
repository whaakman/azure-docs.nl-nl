---
title: Azure Active Directory Domain Services uitschakelen | Microsoft Docs
description: Azure Active Directory Domain Services met Azure portal uitschakelen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: maheshu
ms.openlocfilehash: f61f6df85e47bec32e147990d956a4409429a60c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
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
