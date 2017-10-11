---
title: 'Zelfstudie: Azure Active Directory-integratie met Datahug | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Datahug.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: ec431dd5ccfa53e4b975e46da247704dd1e15c2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Zelfstudie: Azure Active Directory-integratie met Datahug

In deze zelfstudie leert u hoe Datahug integreren met Azure Active Directory (Azure AD).

Datahug integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Datahug heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Datahug (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, zien. [Wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Datahug, moet u de volgende items:

- Een Azure AD-abonnement
- Een Datahug eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Datahug uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-datahug-from-the-gallery"></a>Datahug uit de galerie toevoegen
Voor het configureren van de integratie van Datahug in Azure AD, moet u Datahug uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Datahug uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Datahug**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. Selecteer in het deelvenster resultaten **Datahug**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Datahug op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Datahug is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Datahug tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Datahug.

Om te configureren en testen van Azure AD eenmalige aanmelding met Datahug, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Datahug](#creating-a-datahug-test-user)**  - Datahug die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Datahug configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Datahug, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Datahug** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. Op de **Datahug domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://apps.datahug.com/identity/<uniqueID>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://apps.datahug.com/identity/<uniqueID>/acs`

4. Controleer **weergeven geavanceerde instellingen voor URL**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    In de **aanmeldings-URL** textbox, typ een URL als:`https://apps.datahug.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Hier raden we u voor het gebruik van de unieke waarde van een tekenreeks in de id en de antwoord-URL. Neem contact op met [Datahug Client ondersteuningsteam](http://datahug.com/about/contact-us/) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  Controleer **'Weergeven geavanceerde instellingen voor het ondertekenen van certificaat'** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    a. In **ondertekening optie**, selecteer **aanmelding SAML-verklaring**.
    
    b. In **ondertekening algoritme**, selecteer **SHA1**.
 
7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. Op de **Datahug configuratie** sectie, klikt u op **configureren Datahug** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** en **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. Eenmalige aanmelding configureren op **Datahug** zijde, moet u de gedownloade verzenden **Metadata XML**, **SAML entiteit-ID** en **SAML Single Sign-On Service-URL** naar [Datahug ondersteuning](http://datahug.com/about/contact-us/). Ze hebben de SAML SSO-verbinding juist ingesteld voor beide zijden van deze toepassing ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-datahug-test-user"></a>Een testgebruiker Datahug maken

Om Azure AD-gebruikers zich aanmelden bij Datahug, moeten ze worden ingericht in Datahug.  
Datahug, inrichting wanneer een handmatige taak is.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Datahug site als beheerder.

2. Beweeg de muisaanwijzer over de **tandwiel** in de rechterbovenhoek en klik op **instellingen**
   
   ![Werknemer toevoegen](./media/active-directory-saas-datahug-tutorial/1.png)

3. Kies **mensen** en klik op de **gebruikers toevoegen** tabblad

    ![Werknemer toevoegen](./media/active-directory-saas-datahug-tutorial/2.png)

4. Typ het e-mailadres van degene die u wilt maken voor een account en klik op **toevoegen**.

    ![Werknemer toevoegen](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **verzenden welkomstbericht** selectievakje.  
    > Als u een account voor Salesforce het welkomstbericht niet verzenden.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Datahug.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Datahug, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Datahug**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.
Als u op de tegel Datahug in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Datahug. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

