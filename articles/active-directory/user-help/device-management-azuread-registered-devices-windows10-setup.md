---
title: Azure Active Directory geregistreerde apparaten instellen | Microsoft Docs
description: Meer informatie over het instellen van Azure Active Directory geregistreerde apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 7ce632c76a86fb00101db6664e9e79615484f9a1
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059793"
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Instellen van Azure Active Directory ingeschreven Windows 10-apparaten

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen van apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie voor meer informatie, [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).

Als u wilt inschakelen de **Bring Your Own Device (BYOD)** scenario, kunt u dit doen door Azure AD ingeschreven apparaten te configureren. U kunt in Azure AD wordt geregistreerd bij Azure AD-apparaten configureren voor Windows 10, iOS, Android en macOS. In dit artikel biedt u de bijbehorende stappen voor Windows 10-apparaten. 


## <a name="before-you-begin"></a>Voordat u begint

Voor het registreren van een Windows 10-apparaat, moet de device registratieservice om te registreren van apparaten worden geconfigureerd. Bovendien moet u minder apparaten die zijn geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie, [apparaatinstellingen configureren](../device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>Wat u moet weten

Bij het registreren van een apparaat, moet u het volgende waarmee u rekening moet houden:

- Windows wordt het apparaat geregistreerd bij de adreslijst van de organisatie in Azure AD

- Mogelijk moet u multi-factor authentication-uitdaging doorlopen. Dit probleem kan worden geconfigureerd door uw IT-beheerder.

- Azure AD controleert of het apparaat inschrijven voor beheer van mobiele apparaten moet en deze inschrijft, indien van toepassing.

- Als u een beheerde gebruiker gaat Windows u naar het bureaublad via de automatische aanmelding.

- Als u een federatieve gebruiker bent, gaat u naar een Windows-aanmeldingsscherm uw referenties in te voeren.


## <a name="registering-a-device"></a>Een apparaat wordt geregistreerd

In deze sectie biedt u de stappen voor het registreren van uw Windows 10-apparaat aan uw Azure AD. Een geregistreerd apparaat wordt weergegeven met een **werk- of schoolaccount** vermelding.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Uw Windows 10-apparaat registreren:**

1. In de **Start** menu, klikt u op **instellingen**.

    ![Instellingen](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Klik op **Accounts**.

    ![Accounts](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Klik op **toegang tot werk of school**.

    ![Toegang tot werk of school](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Op de **toegang tot werk of school** dialoogvenster, klikt u op **Connect**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Op de **instellen van een account voor werk of school** dialoogvenster, geef de accountnaam van uw (bijvoorbeeld someone@example.com), en klik vervolgens op **volgende**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Op de **wachtwoord opgeven** dialoogvenster, Voer uw wachtwoord in en klik vervolgens op **volgende**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Op de **u bent klaar** dialoogvenster, klikt u op **gedaan**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Verificatie

Om te controleren of een apparaat is toegevoegd aan een Azure AD, kunt u bekijken de **toegang tot werk of school** dialoogvenster op uw apparaat.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

U kunt ook kunt u instellingen voor apparaten op de Azure AD-portal bekijken.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie de [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)

- Zie voor meer informatie over het beheren van apparaten in de Azure AD-portal, de [apparaten beheert met de Azure-portal ](../device-management-azure-portal.md).




