---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler persoonlijke toegang (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang (ZPA)

In deze zelfstudie leert u hoe Zscaler persoonlijke toegang (ZPA) integreren met Azure Active Directory (Azure AD).

Zscaler persoonlijke toegang (ZPA) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD met toegang op Zscaler persoonlijke toegang (ZPA)
- U kunt uw gebruikers automatisch ophalen aangemelde naar Zscaler persoonlijke toegang (ZPA) (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler persoonlijke toegang (ZPA), moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler persoonlijke toegang (ZPA) eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.


Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen
Voor het configureren van de integratie van Zscaler persoonlijke toegang (ZPA) in Azure AD, moet u Zscaler persoonlijke toegang (ZPA) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler persoonlijke toegang (ZPA) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Zscaler persoonlijke toegang (ZPA)**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. Selecteer in het deelvenster resultaten **Zscaler persoonlijke toegang (ZPA)**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Zscaler persoonlijke toegang (ZPA) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Zscaler persoonlijke toegang (ZPA) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Zscaler persoonlijke toegang (ZPA) tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Zscaler persoonlijke toegang (ZPA).

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Zscaler persoonlijke toegang (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang (ZPA) die is gekoppeld aan de Azure AD-representatie van haar.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding configureren in uw toepassing Zscaler persoonlijke toegang (ZPA).

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler persoonlijke toegang (ZPA), moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **Zscaler persoonlijke toegang (ZPA)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. Op de **Zscaler persoonlijke toegang (ZPA)-domein en de URL's** sectie, voert u de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. In de **aanmelding op URL** textbox, typ een URL met het volgende patroon volgen:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. In de **id** textbox, type:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met het werkelijke aanmelding op de URL en de id. Hier raden we u voor het gebruik van de unieke waarde van de URL in de id. Neem contact op met [Zscaler persoonlijke toegang (ZPA) ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **nieuw certificaat maken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. Op de **nieuw certificaat maken** dialoogvenster, klikt u op het pictogram van de kalender en selecteer een **vervaldatum**. Klik vervolgens op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. Op de **SAML-certificaat voor ondertekening van** sectie **nieuwe certificaat activeren** en klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. In het pop-upvenster **rollovercertificaat** venster, klikt u op **OK**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. In een ander browservenster, meld u bij uw bedrijf Zscaler persoonlijke toegang (ZPA) site als beheerder.

10. Navigeer naar **beheerder** en klik vervolgens op **Idp configuratie**.

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. In de **Idp configuratie** sectie, klikt u op **nieuwe IDP-configuratie toevoegen**.

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. In de **nieuwe IDP configuratie** sectie, voert u de volgende stappen uit:

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Klik op **bestand selecteren** en uw van het gedownloade metagegevensbestand te uploaden.

    b. Klik op **opslaan** knop.
    


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Maken van een testgebruiker Zscaler persoonlijke toegang (ZPA)

In deze sectie maakt u een gebruiker met de naam Britta Simon in Zscaler persoonlijke toegang (ZPA). Neem contact op met [Zscaler persoonlijke toegang (ZPA) ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om toe te voegen de gebruikers van het platform Zscaler persoonlijke toegang (ZPA).


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Zscaler persoonlijke toegang (ZPA).

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Zscaler persoonlijke toegang (ZPA), moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zscaler persoonlijke toegang (ZPA)**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Zscaler persoonlijke toegang (ZPA) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Zscaler persoonlijke toegang (ZPA).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png