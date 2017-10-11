---
title: 'Zelfstudie: Azure Active Directory-integratie met Procore SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Procore eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 042a41eaa6bb21670b4c12068f1b017222f64b99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Zelfstudie: Azure Active Directory-integratie met Procore SSO

In deze zelfstudie leert u hoe Procore SSO integreren met Azure Active Directory (Azure AD).

Procore SSO integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Procore SSO heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Procore SSO (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Procore SSO, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Procore SSO, moet u de volgende items:

- Een Azure AD-abonnement
- Een Procore SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Procore SSO uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-procore-sso-from-the-gallery"></a>Procore SSO uit de galerie toevoegen
Voor het configureren van de integratie van Procore SSO in Azure AD, moet u Procore SSO uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Procore SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Procore SSO**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. Selecteer in het deelvenster resultaten **Procore SSO**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Procore eenmalige aanmelding op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Procore eenmalige aanmelding is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Procore eenmalige aanmelding tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Procore eenmalige aanmelding.

Om te configureren en testen van Azure AD eenmalige aanmelding met Procore SSO, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Procore SSO](#creating-a-procore-sso-test-user)**  - Procore eenmalige aanmelding die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding configureren in uw toepassing Procore eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met Procore SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **Procore SSO** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Op de **Procore SSO-domein en de URL's** sectie, de gebruiker heeft geen alle stappen uitvoeren als de app al vooraf is geïntegreerd met Azure.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. Op de **Procore SSO configuratie** sectie, klikt u op **Procore eenmalige aanmelding configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Eenmalige aanmelding configureren op **Procore SSO** side, meld u aan bij uw site procore bedrijf als beheerder.

8. In de vervolgkeuzelijst werkset omlaag, klikt u op **Admin** naar de instellingenpagina SSO openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. De waarden in de vakken plakken, zoals beschreven onderstaande-

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. In de **eenmalige aanmelding op URL-verlener** vak, plak de SAML-entiteit-ID opgehaald uit de Azure-portal.

    b. In de **SAML aanmelding op de doel-URL** vak, plak de SAML Single Sign-On Service-URL van de Azure-portal gekopieerd.

    c. Open nu het **Metadata XML** hierboven gedownload vanuit de Azure portal en kopieert u het certificaat in de tag met de naam **X509Certificate**. Plak de gekopieerde waarde in de **certificaat voor eenmalige aanmelding x509** vak.

10. Klik op **wijzigingen opslaan**.

11. Na deze instellingen moet u verzenden de **domeinnaam** (bijvoorbeeld **contoso.com**) via die u aan te bij Procore naar melden de [Procore ondersteuningsteam](https://support.procore.com/) en deze zullen activeren van federatieve SSO voor dat domein.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-procore-sso-test-user"></a>Maken van een testgebruiker Procore SSO

Volg de onderstaande stappen voor het maken van een Procore testgebruiker op hun kant.

1. Meld u aan bij uw site procore bedrijf als beheerder.  

2. In de vervolgkeuzelijst werkset omlaag, klikt u op **Directory** openen van de directory-pagina van het bedrijf.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Klik op **toevoegen van een persoon** optie voor het openen van het formulier en voer uitvoeren na de opties -

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. In de **voornaam** tekstvak de voornaam van de gebruiker van het type zoals **Britta**.

    b. In de **achternaam** tekstvak de achternaam van de gebruiker van het type zoals **Simon**.

    c. In de **e-mailadres** textbox e-mailadres van de gebruiker van het type, zoals  **BrittaSimon@contoso.com** .

    d. Selecteer **machtigingssjabloon** als **machtigingssjabloon Later toepassen**.

    e. Klik op **Create**.

4. Controleer en de details voor de zojuist toegevoegde contactpersoon bijwerken.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Klik op **opslaan en verzenden Invitiation** (als een uitnodiging via e-mail vereist is) of **opslaan** (opslaan rechtstreeks) om de inschrijving van de gebruiker te voltooien.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Procore eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Procore SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Procore SSO**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586). Als u op de tegel Procore SSO in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Procore SSO-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

