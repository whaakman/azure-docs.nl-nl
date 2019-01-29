---
title: Azure Active Directory Domain Services uitschakelen | Microsoft Docs
description: Azure Active Directory Domain Services met behulp van de Azure-portal uitschakelen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/27/2017
ms.author: ergreenl
ms.openlocfilehash: d72fa2551eb4db3cb68a52cd8a76dd7b9d27b6f0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179001"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory Domain Services met behulp van de Azure-portal uitschakelen
Dit artikel leest u hoe de Azure-portal gebruiken om te schakelen van Azure Active Directory (AD) Domain Services voor uw Azure AD-directory.

> [!WARNING]
> **Verwijdering is permanent en kan niet ongedaan worden gemaakt.**
> Ga zorgvuldig te werk! Wanneer u het beheerde domein verwijdert:
  * Domeincontrollers voor het beheerde domein zijn niet ingericht en verwijderd uit het virtuele netwerk.
  * Gegevens in het beheerde domein wordt permanent verwijderd. Dit omvat aangepaste organisatie-eenheden, GPO's, aangepaste DNS-records, service-principals, gmsa's enzovoort die u hebt gemaakt in het beheerde domein.
  * Machines toegevoegd aan het beheerde domein verloren gaan van de vertrouwensrelatie met het domein en moeten worden verwijderd uit het domein.
  * U kunt zich niet op deze machines met behulp van zakelijke AD-referenties. Gebruik in plaats daarvan de lokale administrator-referenties voor de machine.
Verwijderen van het beheerde domein niet verwijderen van uw Azure AD-directory of anders negatieve invloed hebben op de map.
>

Voer de volgende stappen uit als u wilt verwijderen van uw Azure AD Domain Services beheerde domein:
1. Navigeer naar de [Azure AD Domain Services-extensie](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal.
2. Klik op de naam van uw beheerde domein.

    ![Selecteer domein verwijderen](./media/getting-started/domain-services-delete-select-domain.png)

3. Op de **overzicht** pagina, klikt u op de **verwijderen** knop.

    ![Domein verwijderen](./media/getting-started/domain-services-delete-domain.png)

4. Als u wilt bevestigen, typt u de DNS-domeinnaam van het beheerde domein. Klik op de **verwijderen** wanneer u klaar bent.

    ![Domein-bevestiging van verwijderen](./media/getting-started/domain-services-delete-domain-confirm.png)

Het beheerde domein wordt verwijderd in ongeveer 15-20 minuten.

Houd rekening met [feedback delen](active-directory-ds-contact-us.md) aan ons beter te begrijpen wat functies wilt helpen u Azure AD Domain Services in de toekomst hebt gekozen. Deze feedback helpt ons de service beter aan de behoeften van uw implementatiebehoeften en use cases ontwikkelen.
