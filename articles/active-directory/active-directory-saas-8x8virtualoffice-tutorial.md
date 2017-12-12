---
title: 'Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 8 x 8 virtuele Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 71362243bf56e7a8b6e013ab7a91ecac89a32117
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office

In deze zelfstudie leert u hoe 8 x 8 virtuele Office integreren met Azure Active Directory (Azure AD).

Integratie van 8 x 8 virtuele Office met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot 8 x 8 virtuele Office heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij 8 x 8 virtuele Office (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 8 x 8 virtuele Office, moet u de volgende items:

- Een Azure AD-abonnement
- Een 8 x 8 virtuele Office eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 8 x 8 virtuele Office toevoegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuele Office toevoegen uit de galerie
Voor het configureren van de integratie van 8 x 8 virtuele Office in Azure AD, moet u 8 x 8 virtuele Office uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 8 x 8 virtuele Office uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **8 x 8 virtuele Office**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. Selecteer in het deelvenster resultaten **8 x 8 virtuele Office**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met 8 x 8 die virtuele Office op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in 8 x 8 virtuele Office aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in 8 x 8 virtuele Office moet tot stand worden gebracht.

Wijs in 8 x 8 virtuele kantoor, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker van 8 x 8 virtuele Office](#creating-a-8x8-virtual-office-test-user)**  - 8 x 8 virtuele Office die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw 8 x 8 virtuele Office-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **8 x 8 virtuele Office** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Op de **8 x 8 virtuele Office domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [8 x 8 virtuele Office ondersteuningsteam](https://www.8x8.com/about-us/contact-us) ophalen van deze waarden.
 


4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Op de **8 x 8 virtuele Office-configuratie** sectie, klikt u op **configureren 8 x 8 virtuele Office** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. Eenmalige aanmelding in uw tenant, 8 x 8 virtuele Office als beheerder.

8. Selecteer **virtuele Office Account Mgr** in deelvenster van de toepassing.
   
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Selecteer **Business** account te beheren en klikt u op **aanmelden** knop.
   
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Klik op **Accounts** tabblad in de menulijst.
   
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Klik op **eenmalige aanmelding** in de lijst van Accounts.
   
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Selecteer **eenmalige aanmelding** onder verificatiemethode en op **SAML**.
    
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Kopiëren **SAML SSO URL**, **Kana uit de URL van één** en **URL-verlener** van Azure AD **URL aanmelden**, **URL afmelden**  en **URL-verlener** in 8 x 8 virtuele Office. 
    
    ![Zijde van de App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Klik op **Browser** om het certificaat dat u hebt gedownload van Azure AD te uploaden en klik op de **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>Maken van een 8 x 8 virtuele Office-testgebruiker

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in 8 x 8 virtuele Office. 8 x 8 virtuele Office ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot 8 x 8 virtuele Office als deze nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de [8 x 8 virtuele Office ondersteuningsteam](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan 8 x 8 virtuele Office.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **8 x 8 virtuele Office**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel 8 x 8 virtuele Office in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw 8 x 8 virtuele Office-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

