---
title: 'Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TigerText Secure Messenger.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: efbdf55f425d8e559b741f41b09527e9ce93f999
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger

In deze zelfstudie leert u hoe TigerText Secure Messenger integreren met Azure Active Directory (Azure AD).

TigerText Secure Messenger integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TigerText Secure Messenger heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TigerText Secure Messenger (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TigerText Secure Messenger, moet u de volgende items:

- Een Azure AD-abonnement
- Een TigerText Secure Messenger eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TigerText Secure Messenger uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a>TigerText Secure Messenger uit de galerie toevoegen
Voor het configureren van de integratie van TigerText Secure Messenger in Azure AD, moet u TigerText Secure Messenger uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TigerText Secure Messenger uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TigerText Secure Messenger**, selecteer **TigerText Secure Messenger** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met TigerText Secure Messenger op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TigerText Secure Messenger is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante TigerText Secure Messenger-gebruiker worden gemaakt.

Wijs in TigerText Secure Messenger, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TigerText Secure Messenger, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker TigerText Secure Messenger](#create-a-tigertext-secure-messenger-test-user)**  - TigerText Secure Messenger die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing TigerText Secure Messenger.

**Voor het configureren van Azure AD eenmalige aanmelding met TigerText Secure Messenger, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TigerText Secure Messenger** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. Op de **TigerText Secure Messenger domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Sectie TigerText Secure Messenger domein en URL 's](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    a. In de **aanmeldings-URL** textbox, typ de URL als:`https://home.tigertext.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde door de werkelijke ID bijwerken. Neem contact op met [TigerText Secure Messenger-Client-ondersteuningsteam](mailTo:prosupport@tigertext.com) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Certificaat voor ondertekening van SAML-sectie](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop Opslaan](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met [TigerText Secure Messenger ondersteuningsteam](mailTo:prosupport@tigertext.com) en geeft u de **gedownloade metagegevens**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Knop toevoegen](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Gebruikersdialoogvenster](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>Een testgebruiker TigerText Secure Messenger maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in TigerText maken. Neem bereiken [TigerText Secure Messenger-Client-ondersteuningsteam](mailTo:prosupport@tigertext.com) de gebruikers van het platform TigerText toevoegen.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TigerText Secure Messenger.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen TigerText Secure Messenger, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TigerText Secure Messenger**.

    ![TigerText Secure Messenger in lijst met Apps](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel TigerText in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing TigerText. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

