---
title: Migreren van een klassieke beleid waarvoor u multi-factor authentication in de Azure portal | Microsoft Docs
description: Dit artikel laat zien hoe u migreert van een klassieke beleid waarvoor meervoudige verificatie in de Azure-portal.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 77484dc3773736ea15c39ede5f9d49b6b694d960
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migreren van een klassieke beleid waarvoor meervoudige verificatie in de Azure-portal 

Dit artikel laat zien hoe u migreert van een klassieke beleid waarvoor **multi-factorauthenticatie** voor een cloud-app. Hoewel het niet vereist, raden we aan dat u leest [migreren klassieke beleid in de Azure portal](active-directory-conditional-access-migration.md) voordat u begint met het beleid voor klassieke migreren.


 
## <a name="overview"></a>Overzicht 

Het scenario in dit artikel wordt beschreven hoe u voor het migreren van een klassieke beleid waarvoor **multi-factorauthenticatie** voor een cloud-app. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


Het migratieproces bestaat uit de volgende stappen uit:

1. [Open het klassieke beleid](#open-a-classic-policy) ophalen van de configuratie-instellingen.
2. Maak een nieuwe Azure AD-beleid voor voorwaardelijke toegang voor uw beleid voor klassieke vervangen. 
3. Schakel het klassieke beleid.



## <a name="open-a-classic-policy"></a>Een klassieke beleid openen

1. In de [Azure-portal](https://portal.azure.com), klik op de navigatiebalk links, **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration-mfa/01.png)

2. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration-mfa/02.png)

3. In de **beheren** sectie, klikt u op **klassieke beleid (preview)**.

    ![Klassieke beleidsregels](./media/active-directory-conditional-access-migration-mfa/12.png)

4. Klik op het beleid dat is vereist in de lijst met klassieke beleidsregels **multi-factorauthenticatie** voor een cloud-app.

    ![Klassieke beleidsregels](./media/active-directory-conditional-access-migration-mfa/13.png)


## <a name="create-a-new-conditional-access-policy"></a>Maak een nieuw beleid voor voorwaardelijke toegang


1. In de [Azure-portal](https://portal.azure.com), klik op de navigatiebalk links, **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/02.png)



3. Op de **voorwaardelijke toegang** pagina, open de **nieuw** pagina in de werkbalk bovenaan, klikt u op **toevoegen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/03.png)

4. Op de **nieuw** pagina in de **naam** textbox, typ een naam voor uw beleid.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/29.png)

5. In de **toewijzingen** sectie, klikt u op **gebruikers en groepen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/05.png)

    a. Als u alle gebruikers in uw beleid voor klassieke geselecteerd hebt, klikt u op **alle gebruikers**. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/35.png)

    b. Als u groepen in uw beleid voor klassieke geselecteerd hebt, klikt u op **gebruikers en groepen selecteren**, en selecteer vervolgens de vereiste gebruikers en groepen.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/36.png)

    c. Als u de uitgesloten groepen hebt, klikt u op de **uitsluiten** tabblad en selecteer vervolgens de vereiste gebruikers en groepen. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/37.png)

6. Op de **nieuw** pagina, open de **Cloud-apps** pagina in de **toewijzing** sectie, klikt u op **Cloud-apps**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Op de **Cloud-apps** pagina, voert u de volgende stappen uit:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/08.png)

    a. Klik op **apps selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina, selecteer uw cloud-app en klik vervolgens op **Selecteer**.

    d. Op de **Cloud-apps** pagina, klikt u op **gedaan**.



9. Als u hebt **meervoudige authenticatie** geselecteerde:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/26.png)

    a. In de **toegangscontroles** sectie, klikt u op **Grant**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/27.png)

    b. Op de **Grant** pagina, klikt u op **toegang verlenen**, en klik vervolgens op **meervoudige authenticatie**.

    c. Klik op **Selecteren**.


10. Klik op **op** om in te schakelen van uw beleid.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/30.png)



## <a name="disable-the-classic-policy"></a>Schakel het klassieke beleid

Als u wilt uw klassieke beleid uitschakelt, klikt u op **uitschakelen** in de **Details** weergeven.

![Klassieke beleidsregels](./media/active-directory-conditional-access-migration-mfa/14.png)



## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de migratie klassieke beleid [migreren klassieke beleid in de Azure portal](active-directory-conditional-access-migration.md).


- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
