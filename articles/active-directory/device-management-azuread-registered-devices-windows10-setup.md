---
title: Azure Active Directory geregistreerd apparaten instellen | Microsoft Docs
description: Informatie over het instellen van Azure Active Directory geregistreerd apparaten.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2560e51d61506389e84288bf983b0ebcb5776ff2
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Instellen van de Azure Active Directory geregistreerd Windows 10-apparaten

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. Zie voor meer informatie [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md).

Als u wilt inschakelen de **Bring Your Own Device (BYOD)** scenario, u kunt dit doen door Azure AD geregistreerde apparaten te configureren. U kunt in Azure AD, Azure AD geregistreerde apparaten configureren voor Windows 10-, iOS-, Android- en Mac OS. Dit onderwerp vindt u de bijbehorende stappen voor Windows 10-apparaten. 


## <a name="before-you-begin"></a>Voordat u begint

Voor het registreren van een Windows 10-apparaat moet de device registratieservice worden geconfigureerd zodat u apparaten registreren. Naast de machtiging voor het registreren van apparaten in uw Azure AD-tenant hebt, moet u minder apparaten geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie [apparaatinstellingen configureren](device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>Wat u moet weten

Wanneer u een apparaat registreert, moet u het volgende rekening houden:

- Windows registreert het apparaat in-map van de organisatie in Azure AD

- Mogelijk moet u multi-factor authentication-server uitdaging doorlopen. Deze uitdaging kan worden geconfigureerd door uw IT-beheerder.

- Azure AD gecontroleerd of het apparaat registratie voor beheer van mobiele apparaten is vereist en wordt deze indien van toepassing.

- Als u een beheerde gebruiker bent, gaat Windows u naar het bureaublad via het automatisch aanmelden.

- Als u een federatieve gebruiker bent, gaat u naar een Windows-aanmeldingsscherm uw referenties in te voeren.


## <a name="registering-a-device"></a>Een apparaat wordt geregistreerd

Deze sectie biedt u de stappen voor het registreren van Windows 10-apparaat aan uw Azure AD. Als u uw apparaat bij Azure AD hebt geregistreerd uw **toegang werk of school** dialoogvenster wordt aangegeven dat dit met een **werk- of schoolaccount** vermelding.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Windows 10-apparaat registreren:**

1. In de **Start** menu, klikt u op **instellingen**.

    ![Instellingen](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Klik op **Accounts**.

    ![Accounts](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Klik op **toegang werk of school**.

    ![Toegang tot werk of school](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Op de **toegang werk of school** dialoogvenster, klikt u op **Connect**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Op de **instellen van een account voor werk of school** dialoogvenster, Voer uw accountnaam (bijvoorbeeld: someone@example.com), en klik vervolgens op **volgende**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Op de **wachtwoord opgeven** dialoogvenster, Voer uw wachtwoord in en klik vervolgens op **volgende**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Op de **alles is voorbereid** dialoogvenster, klikt u op **gedaan**.

    ![Verbinding maken](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Verificatie

U kunt bekijken om te controleren of een apparaat is verbonden met een Azure AD, de **toegang werk of school** dialoogvenster op uw apparaat.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

U kunt ook kunt u ook instellingen op de Azure AD-portal bekijken.

![Registreren](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie de [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)

- Zie voor meer informatie over het beheren van apparaten in de Azure AD-portal, de [apparaten beheert met de Azure-portal ](device-management-azure-portal.md).




