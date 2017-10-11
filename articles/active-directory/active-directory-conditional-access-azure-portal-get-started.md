---
title: Aan de slag met voorwaardelijke toegang in Azure Active Directory | Microsoft Docs
description: Voorwaardelijke toegang met behulp van een locatie voorwaarde testen.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: cd53e8be32d1e98aaf9f72177895871dba69df86
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Aan de slag met voorwaardelijke toegang in Azure Active Directory

Voorwaardelijke toegang is een functie van Azure Active Directory waarmee u kunt de voorwaarden waaronder gemachtigde gebruikers toegang uw apps tot hebben definiëren. 

In dit onderwerp vindt u instructies voor het testen van een voorwaardelijke toegang op basis van de voorwaarde van een locatie in uw omgeving.  


## <a name="scenario-description"></a>Scenariobeschrijving

Een van de algemene vereisten in veel organisaties is het alleen meervoudige verificatie vereisen voor toegang tot apps die niet van het bedrijfsintranet wordt uitgevoerd. Met Azure Active Directory, kunt u eenvoudig deze doelstelling uitvoeren door het configureren van beleid voor voorwaardelijke toegang op basis van locatie. Dit onderwerp vindt u gedetailleerde instructies voor het configureren van een gerelateerd beleid. Het beleid maakt gebruik van [goedgekeurde IP-adressen](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) onderscheid maken tussen toegangspogingen tot van het bedrijf de intranet- en alle andere locaties.


## <a name="prerequisites"></a>Vereisten

Het scenario in dit onderwerp wordt ervan uitgegaan dat u bekend met de concepten die worden beschreven bent in [voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-azure-portal.md).

Als u wilt testen van dit scenario, moet u:

- Een testgebruiker maken 

- Een Azure AD Premium-licentie toewijzen aan de testgebruiker

- Configureren van een beheerde app en uw testgebruiker toekennen

- Goedgekeurde IP-adressen configureren

Als u meer informatie over de goedgekeurde IP-adressen, Zie [goedgekeurde IP-adressen](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Beleid configuratiestappen

**Ga voor het configureren van uw beleid voor voorwaardelijke toegang:**

1. In de Azure-portal op de navigatiebalk links, klikt u op **Azure Active Directory**. 

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. Op de **Azure Active Directory** blade in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. Op de **voorwaardelijke toegang** blade openen de **nieuw** blade in de werkbalk bovenaan, klikt u op **toevoegen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. Op de **nieuw** blade in de **naam** textbox, typ een naam voor uw beleid.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. Op de **gebruikers en groepen** blade, voer de volgende stappen uit:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Klik op **gebruikers en groepen selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** blade uw testgebruiker selecteren en klik vervolgens op **Selecteer**.

    d. Op de **gebruikers en groepen** blade, klikt u op **gedaan**.

7. Op de **nieuw** blade openen de **Cloud-apps** blade in de **toewijzing** sectie, klikt u op **Cloud-apps**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. Op de **Cloud-apps** blade, voer de volgende stappen uit:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Klik op **apps selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** blade uw cloud-app selecteren en klik vervolgens op **Selecteer**.

    d. Op de **Cloud-apps** blade, klikt u op **gedaan**.

9. Op de **nieuw** blade openen de **voorwaarden** blade in de **toewijzing** sectie, klikt u op **voorwaarden**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. Op de **voorwaarden** blade openen de **locaties** blade, klikt u op **locaties**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. Op de **locaties** blade, voer de volgende stappen uit:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. Onder **configureren**, klikt u op **Ja**.

    b. Onder **opnemen**, klikt u op **alle locaties**.

    c. Klik op **uitsluiten**, en klik vervolgens op **alle goedgekeurde IP-adressen**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Klik op **Gereed**.

12. Op de **voorwaarden** blade, klikt u op **gedaan**.

13. Op de **nieuw** blade openen de **Grant** blade in de **besturingselementen** sectie, klikt u op **Grant**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Op de **Grant** blade, voer de volgende stappen uit:

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Selecteer **meervoudige authenticatie**.

    b. Klik op **Selecteren**.

15. Op de **nieuw** blade onder **beleid inschakelen**, klikt u op **op**.

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Op de **nieuw** blade, klikt u op **maken**.


## <a name="testing-the-policy"></a>Het beleid testen

Als u wilt testen van uw beleid, moet u toegang tot uw app vanaf een apparaat dat: 

1. Heeft een IP-adres dat binnen de geconfigureerde goedgekeurde IP-adressen 

1. Heeft een IP-adres dat is niet binnen de geconfigureerde goedgekeurde IP-adressen

Multi-factor authentication-server zijn moet alleen tijdens een verbindingspoging die is gemaakt van een apparaat dat is niet binnen de geconfigureerde goedgekeurde IP-adressen vereist. 


## <a name="next-steps"></a>Volgende stappen

Als u meer informatie over voorwaardelijke toegang wilt, Zie [voorwaardelijke toegang van Azure Active Directory](active-directory-conditional-access-azure-portal.md).

