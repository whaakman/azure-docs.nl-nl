---
title: 'Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a0e314aaf59a70107058829ea84dde362f097274
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Zelfstudie: Azure Active Directory-integratie met AirWatch

In deze zelfstudie leert u hoe AirWatch integreren met Azure Active Directory (Azure AD).

AirWatch integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AirWatch heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AirWatch (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AirWatch, moet u de volgende items:

- Een Azure AD-abonnement
- Een AirWatch eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AirWatch uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-airwatch-from-the-gallery"></a>AirWatch uit de galerie toevoegen
Voor het configureren van de integratie van AirWatch in Azure AD, moet u AirWatch uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AirWatch uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **AirWatch**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. Selecteer in het deelvenster resultaten **AirWatch**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met AirWatch op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in AirWatch is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in AirWatch tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in AirWatch.

Om te configureren en testen van Azure AD eenmalige aanmelding met AirWatch, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker AirWatch](#creating-a-airwatch-test-user)**  - AirWatch die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing AirWatch configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met AirWatch, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AirWatch** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Op de **AirWatch domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. In de **id** textbox, typ de waarde als`AirWatch`

    > [!NOTE] 
    > Deze waarde is niet de werkelijke. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [AirWatch Client ondersteuningsteam](http://www.air-watch.com/company/contact-us/) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Op de **AirWatch configuratie** sectie, klikt u op **configureren AirWatch** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. In een ander browservenster, meld u aan bij uw bedrijf AirWatch site als beheerder.

8. Klik in het navigatiedeelvenster links op **Accounts**, en klik vervolgens op **beheerders**.
   
   ![Beheerders](./media/active-directory-saas-airwatch-tutorial/ic791920.png "beheerders")

9. Vouw de **instellingen** menu en klik vervolgens op **Directory Services**.
   
   ![Instellingen](./media/active-directory-saas-airwatch-tutorial/ic791921.png "instellingen")

10. Klik op de **gebruiker** tabblad, in de **Base DN** textbox, typ de naam van uw domein en klik vervolgens op **opslaan**.
   
   ![Gebruiker](./media/active-directory-saas-airwatch-tutorial/ic791922.png "gebruiker")

11. Klik op de **Server** tabblad.
   
   ![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")

12. De volgende stappen uitvoeren:
    
    ![Uploaden](./media/active-directory-saas-airwatch-tutorial/ic791924.png "uploaden")   
    
    a. Als **maptype**, selecteer **geen**.

    b. Selecteer **SAML gebruiken voor verificatie**.

    c. De gedownloade om certificaat te uploaden, klikt u op **uploaden**.

13. In de **aanvragen** sectie, voert u de volgende stappen uit:
    
    ![Aanvraag](./media/active-directory-saas-airwatch-tutorial/ic791925.png "aanvragen")  

    a. Als **Binding aanvraagtype**, selecteer **POST**.

    b. In de Azure-portal op de **op Airwatch eenmalige aanmelding configureren** dialoogvenster pagina-, Kopieer de **SAML Single Sign-On Service-URL** waarde en plak deze in de **identiteit Provider eenmalige aanmelding op URL** textbox.

    c. Als **NameID indeling**, selecteer **e-mailadres**.

    d. Klik op **Opslaan**.

14. Klik op de **gebruiker** tabblad opnieuw.
    
    ![Gebruiker](./media/active-directory-saas-airwatch-tutorial/ic791926.png "gebruiker")

15. In de **kenmerk** sectie, voert u de volgende stappen uit:
    
    ![Kenmerk](./media/active-directory-saas-airwatch-tutorial/ic791927.png "kenmerk")

    a. In de **Object-id** textbox type **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. In de **gebruikersnaam** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. In de **weergavenaam** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. In de **voornaam** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. In de **achternaam** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. In de **e** textbox type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Klik op **Opslaan**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-airwatch-test-user"></a>Een testgebruiker AirWatch maken

Om Azure AD-gebruikers zich aanmelden bij AirWatch, moeten ze worden ingericht op AirWatch.

* AirWatch, inrichting wanneer een handmatige taak is.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **AirWatch** bedrijf site als administrator.
2. Klik in het navigatievenster aan de linkerkant op **Accounts**, en klik vervolgens op **gebruikers**.
   
   ![Gebruikers](./media/active-directory-saas-airwatch-tutorial/ic791929.png "gebruikers")
3. In de **gebruikers** menu, klikt u op **lijstweergave**, en klik vervolgens op **toevoegen \> gebruiker toevoegen**.
   
   ![Gebruiker toevoegen](./media/active-directory-saas-airwatch-tutorial/ic791930.png "gebruiker toevoegen")
4. Op de **toevoegen / bewerken van de gebruiker** dialoogvenster de volgende stappen uitvoeren:

   ![Gebruiker toevoegen](./media/active-directory-saas-airwatch-tutorial/ic791931.png "gebruiker toevoegen")   
   1. Typ de **gebruikersnaam**, **wachtwoord**, **wachtwoord bevestigen**, **voornaam**, **achternaam**, **e-mailadres** van een geldige Azure Active Directory-account dat u inrichten in de bijbehorende tekstvakken wilt.
   2. Klik op **Opslaan**.

>[!NOTE]
>U kunt andere AirWatch gebruiker account hulpmiddelen voor het maken of API's die is geleverd door AirWatch aan inrichten AAD-gebruikersaccounts.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan AirWatch.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen AirWatch, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **AirWatch**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

