---
title: Migreren van klassieke beleid in de Azure portal | Microsoft Docs
description: Migreer klassieke beleid in de Azure portal.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c584eddb5542c2c49d08d35bcaf8e7acb5c5b83a
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="migrate-classic-policies-in-the-azure-portal"></a>Migreren van klassieke beleid in de Azure portal 


[Voorwaardelijke toegang](active-directory-conditional-access-azure-portal.md) is een functie van Azure Active directory (Azure AD) dat u bepalen hoe kunt gemachtigde gebruikers toegang hebben uw cloud-apps. Terwijl het doel nog steeds hetzelfde is is, heeft aanzienlijke verbeteringen in hoe voorwaardelijke toegang werkt ook in de release van de nieuwe Azure portal geïntroduceerd. U hebt geconfigureerd buiten de Azure portal beleid voor voorwaardelijke toegang kunnen worden gecombineerd met het nieuwe beleid dat u in de Azure portal maakt. Als u niet uit te schakelen of te verwijderen, worden ze nog steeds toegepast in uw omgeving. We raden u echter aan het beleid voor klassieke te migreren naar de nieuwe Azure AD voorwaardelijke toegangsbeleid omdat:

- Het nieuwe beleid kunnen u soort scenario's die u niet met beleid voor klassieke kan verwerken.

- U kunt het nummer van de beleidsregels die u hebt voor het beheren van deze samen te verkleinen.   

In dit onderwerp helpt u bij de migratie van uw bestaande beleidsregels voor klassieke naar het nieuwe de nieuwe Azure AD-beleid voor voorwaardelijke toegang.


## <a name="classic-policies"></a>Klassieke beleid

Beleid voor voorwaardelijke toegang voor Azure AD en Intune u niet in de Azure-portal hebt gemaakt, worden ook wel bekend als **klassieke beleid**. Voor het migreren van uw klassieke beleid, moet u geen toegang hebben tot de klassieke Azure-portal. De Azure-portal beschikt u over een [ **klassieke beleid (preview)** weergave](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) waarmee u kunt uw klassieke beleid.

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


### <a name="open-a-classic-policy"></a>Een klassieke beleid openen

**Een klassieke beleid openen:**

1. In de [Azure-portal](https://portal.azure.com), klik op de navigatiebalk links, **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/02.png)
 
2. Op de **voorwaardelijke toegang - beleid** pagina in de **beheren** sectie, klikt u op **klassieke beleid (preview)**.

3. Selecteer in de lijst met klassieke beleidsregels, het beleid voor dat u belangrijk vindt.   

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/34.png)



## <a name="azure-ad-conditional-access-policies"></a>Azure AD-beleid voor voorwaardelijke toegang

In dit onderwerp vindt u gedetailleerde stappen waarmee u kunt uw klassieke beleid migreren zonder dat u bekend bent met voorwaardelijke toegang van Azure AD hoeft het beleid. Echter, wordt nagegaan wat de basisbegrippen en terminologie van Azure AD voorwaardelijke toegang helpt om uw migratie-ervaring te verbeteren.

Zie:

- [Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md) voor meer informatie over de basisconcepten en de terminologie

- [Aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md) om vertrouwd te raken met de gebruikersinterface in de Azure portal


 





## <a name="multi-factor-authentication-policy"></a>Beleid voor meervoudige verificatie 

In dit voorbeeld laat zien hoe voor het migreren van een klassieke beleid waarvoor multi-factor authentication ** voor een cloud-app. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


**Een klassieke beleid migreren:**

1. [Open het klassieke beleid](#open-a-classic-policy) ophalen van de configuratie-instellingen.
2. Maak een nieuwe Azure AD-beleid voor voorwaardelijke toegang voor uw beleid voor klassieke vervangen. 


### <a name="create-a-new-conditional-access-policy"></a>Maak een nieuw beleid voor voorwaardelijke toegang


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

11. Schakel het klassieke beleid. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-migration/38.png)



 


## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
