---
title: Gekoppelde apparaten instellen van Azure Active Directory | Microsoft Docs
description: Meer informatie over het instellen van Azure Active Directory verbonden apparaten.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 511580b95646677a231a68862cae8a6aa57abcb0
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345528"
---
# <a name="set-up-azure-active-directory-joined-devices"></a>Gekoppelde apparaten instellen van Azure Active Directory

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen van apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie voor meer informatie, [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).

Als u wilt om eigendom van het werk Windows 10-apparaten onder het beheer van Azure AD, kunt u dit doen door Azure AD gekoppelde apparaten te configureren. In dit onderwerp biedt u de bijbehorende stappen. 


## <a name="prerequisites"></a>Vereisten

Als u wilt deelnemen aan een Windows 10-apparaat, moet de device registratieservice om te registreren van apparaten worden geconfigureerd. Naast de machtiging voor het toevoegen van apparaten in uw Azure AD-tenant, moet u minder apparaten die zijn geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie, [apparaatinstellingen configureren](../device-management-azure-portal.md#configure-device-settings).



## <a name="what-you-should-know"></a>Wat u moet weten


- Windows lid wordt van het apparaat in de map van de organisatie in Azure AD.

- Mogelijk moet u multi-factor authentication-uitdaging doorlopen. Dit probleem kan worden geconfigureerd door uw IT-beheerder.

- Azure AD controleert of het apparaat inschrijven voor beheer van mobiele apparaten moet en deze inschrijft, indien van toepassing.

- Als u een beheerde gebruiker gaat Windows u naar het bureaublad via de automatische aanmelding.

- Als u een federatieve gebruiker bent, hebt u zich aanmelden met uw referenties.

- Als u federatief zijn, moet uw id-provider eindpunt voor WS-Federation en WS-Trust gebruikersnaam en wachtwoord te ondersteunen. Dit is versie 1.3 of 2005. De ondersteuning van dit protocol is vereist voor zowel het apparaat toevoegen aan Azure AD en meld u aan bij het apparaat met een wachtwoord. 




## <a name="joining-a-device"></a>Een apparaat toevoegen

In deze sectie biedt u de stappen voor het toevoegen van uw Windows 10-apparaat aan uw Azure AD. Een neemt deel aan apparaat wordt weergegeven als **verbonden met \<uw Azure AD\>**.

![Verbonden](./media/device-management-azuread-joined-devices-setup/13.png)


**Lid worden van uw Windows 10-apparaat:**

1. In de **Start** menu, klikt u op **instellingen**.

    ![Instellingen](./media/device-management-azuread-joined-devices-setup/01.png)

2. Klik op **Accounts**.

    ![Accounts](./media/device-management-azuread-joined-devices-setup/02.png)


3. Klik op **toegang tot werk of school**.

    ![Toegang tot werk of school](./media/device-management-azuread-joined-devices-setup/03.png)

4. Op de **toegang tot werk of school** dialoogvenster, klikt u op **Connect**.

    ![Verbinding maken](./media/device-management-azuread-joined-devices-setup/04.png)


5. Op de **instellen van een account voor werk of school** dialoogvenster, klikt u op **dit apparaat toevoegen aan Azure Active Directory**.

    ![Verbinding maken](./media/device-management-azuread-joined-devices-setup/08.png)


6. Op de **je krijgt zo aangemeld** dialoogvenster, geef de accountnaam van uw (bijvoorbeeld someone@example.com), en klik vervolgens op **volgende**.

    ![Je krijgt zo aangemeld](./media/device-management-azuread-joined-devices-setup/10.png)


6. Op de **wachtwoord opgeven** dialoogvenster, Voer uw wachtwoord in en klik vervolgens op **aanmelden**.

    ![Wachtwoord invoeren](./media/device-management-azuread-joined-devices-setup/05.png)


7. Op de **Zorg ervoor dat dit is uw organisatie** dialoogvenster, klikt u op **Join**.

    ![Zorg ervoor dat dit is uw organisatie](./media/device-management-azuread-joined-devices-setup/11.png)


8. Op de **u bent klaar** dialoogvenster, klikt u op **gedaan**.

    ![U bent nu klaar](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Verificatie

Om te controleren of een apparaat is toegevoegd aan een Azure AD, kunt u bekijken de **toegang tot werk of school** dialoogvenster op uw apparaat.

![Verbonden](./media/device-management-azuread-joined-devices-setup/13.png)

U kunt ook de volgende opdracht uitvoeren: `dsregcmd /status`  
Op een apparaat is verbonden, **AzureAdJoined** is **Ja**.

![Verbonden](./media/device-management-azuread-joined-devices-setup/14.png)

U kunt ook de instellingen voor apparaten op de Azure AD-portal bekijken.

![Verbonden](./media/device-management-azuread-joined-devices-setup/15.png)

Zie voor meer informatie, [apparaten zoeken](../device-management-azure-portal.md#locate-devices).


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie: 

- De [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
- [Beheer van apparaten met behulp van de Azure portal](../device-management-azure-portal.md)
- 



