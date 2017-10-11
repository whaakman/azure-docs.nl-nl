---
title: 'Zelfstudie: Azure Active Directory-integratie met Freshservice | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Freshservice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d32775fa91d3a49da1ef55e57d1d38990fa09346
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Zelfstudie: Azure Active Directory-integratie met Freshservice

In deze zelfstudie leert u hoe Freshservice integreren met Azure Active Directory (Azure AD).

Freshservice integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Freshservice heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Freshservice (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Freshservice, moet u de volgende items:

- Een Azure AD-abonnement
- Een Freshservice eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Freshservice uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-freshservice-from-the-gallery"></a>Freshservice uit de galerie toevoegen
Voor het configureren van de integratie van Freshservice in Azure AD, moet u Freshservice uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Freshservice uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Freshservice**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. Selecteer in het deelvenster resultaten **Freshservice**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Freshservice op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Freshservice is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Freshservice tot stand worden gebracht.

Wijs in Freshservice, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Freshservice, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Freshservice](#creating-a-freshservice-test-user)**  - Freshservice die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Freshservice configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Freshservice, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Freshservice** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. Op de **Freshservice domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<democompany>.freshservice.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Freshservice Client ondersteuningsteam](https://support.freshservice.com/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. Op de **Freshservice configuratie** sectie, klikt u op **configureren Freshservice** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf Freshservice site als beheerder.

8. Klik in het menu bovenaan op **Admin**.
   
    ![Beheerder](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")

9. In de **Customer Portal**, klikt u op **beveiliging**.
   
    ![Beveiliging](./media/active-directory-saas-freshservice-tutorial/ic790815.png "beveiliging")

10. In de **beveiliging** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/active-directory-saas-freshservice-tutorial/ic790816.png "eenmalige aanmelding")
   
    a. Switch **eenmalige aanmelding**.

    b. Selecteer **SAML SSO**.

    c. In de **aanmeldings-URL van SAML** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    d. In de **afmelding URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.

    e. In **beveiliging certificaat vingerafdruk** textbox, plak de **VINGERAFDRUK** waarde van het certificaat dat u hebt gekopieerd vanuit Azure-portal.

    f. Klik op **opslaan**
   
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-freshservice-test-user"></a>Een testgebruiker Freshservice maken

Om Azure AD-gebruikers zich aanmelden bij FreshService, moeten ze worden ingericht in FreshService. In het geval van FreshService is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **FreshService** bedrijf site als beheerder.

2. Klik in het menu bovenaan op **Admin**.
   
    ![Beheerder](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")

3. In de **Gebruikersbeheer** sectie, klikt u op **aanvragers**.
   
    ![Aanvragers](./media/active-directory-saas-freshservice-tutorial/ic790818.png "aanvragers")

4. Klik op **nieuwe aanvrager**.
   
    ![Nieuwe aanvragers](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nieuwe aanvragers")

5. In de **nieuwe aanvrager** sectie, voert u de volgende stappen uit:
   
    ![Nieuwe aanvrager](./media/active-directory-saas-freshservice-tutorial/ic790820.png "nieuwe aanvrager")   

    a. Voer de **voornaam** en **e** kenmerken van een geldig Azure Active Directory-account dat u inrichten in de bijbehorende tekstvakken wilt.

    b. Klik op **Opslaan**.
   
    >[!NOTE]
    >De accounthouder Azure Active Directory wordt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt
    >  

>[!NOTE]
>U kunt andere FreshService gebruiker account hulpmiddelen voor het maken of API's die is geleverd door FreshService aan inrichten AAD-gebruikersaccounts.
>  

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Freshservice, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Freshservice**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel Freshservice in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Freshservice.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

