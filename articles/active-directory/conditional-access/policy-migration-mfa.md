---
title: Migreren van een klassiek beleid waarvoor multi-factor authentication in Azure portal
description: In dit artikel laat zien hoe voor het migreren van een klassiek beleid waarvoor multi-factor authentication in Azure portal.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: tutorial
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6661cee8ba6176bd706d31a10a8f20549e29e4d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894420"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migreren van een klassiek beleid waarvoor multi-factor authentication in Azure portal

Deze zelfstudie laat zien hoe u migreert van een klassiek beleid waarvoor **multi-factor authentication** voor een cloud-app. Hoewel dit niet een vereiste is, raden we aan dat u leest [klassiek beleid migreren in Azure portal](policy-migration.md) voordat u begint met uw klassieke beleidsregels migreren.

## <a name="overview"></a>Overzicht

Het scenario in dit artikel laat zien hoe u voor het migreren van een klassiek beleid waarvoor **multi-factor authentication** voor een cloud-app.

![Azure Active Directory](./media/policy-migration/33.png)

Het migratieproces bestaat uit de volgende stappen uit:

1. [Open het klassieke beleid](#open-a-classic-policy) om op te halen van de configuratie-instellingen.
1. Maak een nieuwe Azure AD-beleid voor voorwaardelijke toegang ter vervanging van uw klassiek beleid. 
1. Klassiek beleid uitschakelen.

## <a name="open-a-classic-policy"></a>Een klassiek beleid openen

1. In de [Azure-portal](https://portal.azure.com), klik op de navigatiebalk links **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration-mfa/01.png)

1. Op de **Azure Active Directory** pagina, in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

   ![Voorwaardelijke toegang](./media/policy-migration-mfa/02.png)

1. In de **beheren** sectie, klikt u op **klassiek beleid (preview)**.

   ![Klassieke beleidsregels](./media/policy-migration-mfa/12.png)

1. Klik op het beleid dat is vereist in de lijst met klassieke beleidsregels **multi-factor authentication** voor een cloud-app.

   ![Klassieke beleidsregels](./media/policy-migration-mfa/13.png)

## <a name="create-a-new-conditional-access-policy"></a>Maak een nieuw beleid voor voorwaardelijke toegang

1. In de [Azure-portal](https://portal.azure.com), klik op de navigatiebalk links **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration/01.png)

1. Op de **Azure Active Directory** pagina, in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

   ![Voorwaardelijke toegang](./media/policy-migration/02.png)

1. Op de **voorwaardelijke toegang** pagina, open de **nieuw** in de werkbalk bovenaan op de pagina, klikt u op **toevoegen**.

   ![Voorwaardelijke toegang](./media/policy-migration/03.png)

1. Op de **nieuw** pagina, in de **naam** tekstvak, typ een naam voor uw beleid.

   ![Voorwaardelijke toegang](./media/policy-migration/29.png)

1. In de **toewijzingen** sectie, klikt u op **gebruikers en groepen**.

   ![Voorwaardelijke toegang](./media/policy-migration/05.png)

   1. Als u alle gebruikers in uw klassiek beleid geselecteerd hebt, klikt u op **alle gebruikers**. 

   ![Voorwaardelijke toegang](./media/policy-migration/35.png)

   1. Als u groepen geselecteerd in de klassieke beleid hebt, klikt u op **gebruikers en groepen selecteren**, en selecteer vervolgens de vereiste gebruikers en groepen.

   ![Voorwaardelijke toegang](./media/policy-migration/36.png)

   1. Als u de uitgesloten groepen hebt, klikt u op de **uitsluiten** tabblad, en selecteer vervolgens de vereiste gebruikers en groepen. 

   ![Voorwaardelijke toegang](./media/policy-migration/37.png)

1. Op de **nieuw** pagina, open de **Cloud-apps** pagina, in de **toewijzing** sectie, klikt u op **Cloud-apps**.

1. Op de **Cloud-apps** pagina, voert u de volgende stappen uit:

   ![Voorwaardelijke toegang](./media/policy-migration/08.png)

   1. Klik op **apps selecteren**.

   1. Klik op **Selecteren**.

   1. Op de **Selecteer** pagina, selecteert u uw cloud-app en klik vervolgens op **Selecteer**.

   1. Op de **Cloud-apps** pagina, klikt u op **gedaan**.

1. Als u hebt **meervoudige verificatie vereisen** geselecteerde:

   ![Voorwaardelijke toegang](./media/policy-migration/26.png)

   1. In de **besturingselementen voor toegang** sectie, klikt u op **verlenen**.

   ![Voorwaardelijke toegang](./media/policy-migration/27.png)

   1. Op de **verlenen** pagina, klikt u op **toegang verlenen**, en klik vervolgens op **meervoudige verificatie vereisen**.

   1. Klik op **Selecteren**.

1. Klik op **op** om in te schakelen van uw beleid.

   ![Voorwaardelijke toegang](./media/policy-migration/30.png)

## <a name="disable-the-classic-policy"></a>Klassiek beleid uitschakelen

Als u wilt uw klassiek beleid uitschakelen, klikt u op **uitschakelen** in de **Details** weergeven.

![Klassieke beleidsregels](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de migratie van klassiek beleid [klassiek beleid migreren in Azure portal](policy-migration.md).
- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).
- Zie [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md) als u klaar bent om beleid voor voorwaardelijke toegang in Azure Active Directory te configureren.
