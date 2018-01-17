---
title: Instellen van de Azure Active Directory die lid zijn van de apparaten | Microsoft Docs
description: Informatie over het instellen van Azure Active Directory die lid zijn van apparaten.
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
ms.openlocfilehash: ac6c9224925e5bfd3cb056c6c8d9cf2a96b0eb2b
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="set-up-azure-active-directory-joined-devices"></a>Instellen van de Azure Active Directory die lid zijn van apparaten

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. Zie voor meer informatie [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md).

Als u brengen van Windows 10-apparaten die eigendom zijn van werk onder het beheer van Azure AD wilt, kunt u dit doen door Azure AD die lid zijn van apparaten te configureren. Dit onderwerp vindt u met de bijbehorende stappen. 


## <a name="prerequisites"></a>Vereisten

Als u wilt deelnemen aan een Windows 10-apparaat, moet de device registratieservice worden geconfigureerd zodat u apparaten registreren. Naast de machtiging voor het koppelen van apparaten in uw Azure AD-tenant hebt, moet u minder apparaten geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie [apparaatinstellingen configureren](device-management-azure-portal.md#configure-device-settings).



## <a name="what-you-should-know"></a>Wat u moet weten


- Windows koppelt het apparaat in de map van de organisatie in Azure AD.

- Mogelijk moet u multi-factor authentication-server uitdaging doorlopen. Deze uitdaging kan worden geconfigureerd door uw IT-beheerder.

- Azure AD gecontroleerd of het apparaat registratie voor beheer van mobiele apparaten is vereist en wordt deze indien van toepassing.

- Als u een beheerde gebruiker bent, gaat Windows u naar het bureaublad via het automatisch aanmelden.

- Als u een federatieve gebruiker bent, hebt u aanmelden met uw referenties.


## <a name="joining-a-device"></a>Lid worden van een apparaat

Deze sectie biedt u de stappen voor uw Windows 10-apparaat koppelt aan uw Azure AD. Als u hebt uw apparaat is toegevoegd aan Azure AD, uw **toegang werk of school** dialoogvenster wordt aangegeven dat dit met een **verbonden met \<uw Azure AD\>**  vermelding.

![Verbonden](./media/device-management-azuread-joined-devices-setup/13.png)


**Lid worden van Windows 10-apparaat:**

1. In de **Start** menu, klikt u op **instellingen**.

    ![Instellingen](./media/device-management-azuread-joined-devices-setup/01.png)

2. Klik op **Accounts**.

    ![Accounts](./media/device-management-azuread-joined-devices-setup/02.png)


3. Klik op **toegang werk of school**.

    ![Toegang tot werk of school](./media/device-management-azuread-joined-devices-setup/03.png)

4. Op de **toegang werk of school** dialoogvenster, klikt u op **Connect**.

    ![Verbinding maken](./media/device-management-azuread-joined-devices-setup/04.png)


5. Op de **een werk- of schoolaccount-account instellen** dialoogvenster, klikt u op **dit apparaat toevoegen aan Azure Active Directory**.

    ![Verbinding maken](./media/device-management-azuread-joined-devices-setup/08.png)


6. Op de **je krijgt aangemeld** dialoogvenster, Voer uw accountnaam (bijvoorbeeld someone@example.com), en klik vervolgens op **volgende**.

    ![Je krijgt aangemeld](./media/device-management-azuread-joined-devices-setup/10.png)


6. Op de **wachtwoord opgeven** dialoogvenster, Voer uw wachtwoord in en klik vervolgens op **aanmelden**.

    ![Wachtwoord invoeren](./media/device-management-azuread-joined-devices-setup/05.png)


7. Op de **Zorg ervoor dat dit is uw organisatie** dialoogvenster, klikt u op **Join**.

    ![Zorg ervoor dat dit is uw organisatie](./media/device-management-azuread-joined-devices-setup/11.png)


8. Op de **alles is voorbereid** dialoogvenster, klikt u op **gedaan**.

    ![U bent klaar](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Verificatie

U kunt bekijken om te controleren of een apparaat is verbonden met een Azure AD, de **toegang werk of school** dialoogvenster op uw apparaat.

![Verbonden](./media/device-management-azuread-joined-devices-setup/13.png)

U kunt ook de volgende opdracht uitvoeren:`dsregcmd /status`  
Op een apparaat met succes aan **AzureAdJoined** is **Ja**.

![Verbonden](./media/device-management-azuread-joined-devices-setup/14.png)

U kunt ook instellingen op de Azure AD-portal bekijken.

![Verbonden](./media/device-management-azuread-joined-devices-setup/15.png)

Zie voor meer informatie [apparaten vinden](device-management-azure-portal.md#locate-devices).


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie: 

- De [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)
- [Het beheer van apparaten met de Azure portal](device-management-azure-portal.md)
- 



