---
title: 'Zelfstudie: Azure Active Directory-integratie met Teamphoria | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Zelfstudie: Azure Active Directory-integratie met Teamphoria

In deze zelfstudie leert u hoe Teamphoria integreren met Azure Active Directory (Azure AD).

Teamphoria integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Teamphoria heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Teamphoria (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Teamphoria, moet u de volgende items:

- Een Azure AD-abonnement
- Een Teamphoria eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Teamphoria uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-teamphoria-from-the-gallery"></a>Teamphoria uit de galerie toevoegen
Voor het configureren van de integratie van Teamphoria in Azure AD, moet u Teamphoria uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Teamphoria uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Teamphoria**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Selecteer in het deelvenster resultaten **Teamphoria**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Teamphoria op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Teamphoria is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Teamphoria tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Teamphoria.

Om te configureren en testen van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Teamphoria](#creating-a-teamphoria-test-user)**  - Teamphoria die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding in uw toepassing Teamphoria configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **Teamphoria** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Op de **Teamphoria domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. In de **aanmeldings-URL** textbox, typ de URL die met het volgende patroon volgen:`https://<sub-domain>.teamphoria.com/login`    

    > [!NOTE] 
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Teamphoria Client ondersteuningsteam](https://www.teamphoria.com/) ophalen van de aanmeldings-URL. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaat op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Op de **Teamphoria configuratie** sectie, klikt u op **configureren Teamphoria** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. Eenmalige aanmelding configureren op **Teamphoria** aan clientzijde, meld u aan bij uw toepassing Teamphoria als beheerder.

8. Ga naar **BEHEERDERSINSTELLINGEN** optie in de werkbalk links en klikt u onder de het tabblad configureren, klikt u op **SINGLE SIGN-ON** om de configuratie van SSO-venster te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Klik op **IDENTITEITSPROVIDER toevoegen** optie in de rechterbovenhoek om het formulier voor het toevoegen van de instellingen voor eenmalige aanmelding te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Voer de gegevens in de velden in zoals beschreven onderstaande-

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **WEERGAVENAAM** : Geef de weergavenaam van de invoegtoepassing op de beheerpagina.

    b. **NAAM van de knop** : de naam van het tabblad dat wordt weergegeven op de aanmeldingspagina voor logboekregistratie in via eenmalige aanmelding.

    c. **CERTIFICAAT** : Open het certificaat eerder hebt gedownload vanuit de Azure-portal in Kladblok, Kopieer de inhoud van dezelfde en plakt u deze hier in het vak.

    d. **TOEGANGSPUNT** : plak de **SAML Single Sign-On Service-URL** gekopieerd eerdere formulier de Azure-portal.

    e. De optie voor het overschakelen **ON** en klik op **opslaan**.   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-teamphoria-test-user"></a>Een testgebruiker Teamphoria maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Teamphoria, moeten ze worden ingericht in Teamphoria. In het geval van Teamphoria is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Teamphoria site als beheerder.

2. Klik op **ADMIN** instellingen op de werkbalk links en onder de **beheren** Klik op tabblad **gebruikers** openen de beheerpagina voor gebruikers.

    ![Werknemer toevoegen](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Klik op de **handmatig uit te NODIGEN** optie.

    ![Personen uitnodigen](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. Uitvoeren na de actie op deze pagina. 
    
    ![Personen uitnodigen](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    a. In de **e-MAILADRES** tekstvak de **e-mailadres** van BrittaSimon.

    b. In de **VOORNAAM** textbox type **Britta**.

    c. In de **ACHTERNAAM** textbox type **Simon**.

    d. Klik op **uitnodiging 1 gebruiker**. Gebruiker moet de uitnodiging accepteren farmherstelpunten gemaakt in het systeem.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Teamphoria.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Teamphoria, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Teamphoria**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

