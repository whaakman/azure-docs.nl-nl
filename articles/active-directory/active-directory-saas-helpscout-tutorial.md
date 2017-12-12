---
title: 'Zelfstudie: Azure Active Directory-integratie met Help Scout | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Scout Help.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.openlocfilehash: 5608df300ad3b5d3fd42bd904a52d43d115ebf53
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Zelfstudie: Azure Active Directory-integratie met Scout Help

In deze zelfstudie leert u hoe Scout te integreren met Azure Active Directory (Azure AD).

Scout te integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Scout Help heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Scout Help (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met Scout te configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een helpen Scout eenmalige aanmelding ingeschakeld abonnement

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Help Scout uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-help-scout-from-the-gallery"></a>Help Scout uit de galerie toevoegen
Voor het configureren van de integratie van Help Scout in Azure AD, moet u helpen Scout uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Om u te helpen Scout uit de galerie toevoegt, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Scout Help**, selecteer **helpen Scout** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Help Scout in de lijst met resultaten](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Help Scout op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Help Scout is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de Help Scout tot stand worden gebracht.

Help Scout maakt gebruik van e-mailadressen voor aanmeldingen, dus om vast te stellen op de koppeling relatie, gebruiken dezelfde **e-mailadres** als **gebruikersnaam** in Azure AD.

Als u wilt configureren en Azure AD eenmalige aanmelding met Scout te testen, moet u voltooien van de volgende elementen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Scout Help](#create-a-help-scout-test-user)**  - Help Scout, dat is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Scout te configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Help Scout, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Help Scout** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Op de **helpen Scout domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Help-URL's en Scout domein één aanmelding informatie](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

    a. **Id** is de **'Doelgroep URI (serviceprovider entiteit-ID)'** van Help Scout begint met`urn:`

    b. **Antwoord-URL** is de **'na de back-URL (Assertion Consumer Service URL)** van Help Scout begint met`https://` 

    > [!NOTE] 
    > De waarden in deze URL's zijn voor de demonstratie alleen. U moet deze waarden uit de werkelijke antwoord-URL en de ID bijwerken. U deze waarden ophalen uit de **Single Sign-On** tabblad onder sectie verificatie, die verderop in de zelfstudie wordt beschreven.

4. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus selectievakje **weergeven geavanceerde instellingen voor URL** en voer de volgende stap:

    ![Help-URL's en Scout domein één aanmelding informatie](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL als:`https://secure.helpscout.net/members/login/`
     
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)


7. Op de **helpen Scout configuratie** sectie, klikt u op **helpen Scout configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-helpscout-tutorial/config.png) 

8. In een ander browservenster, meld u aan bij uw bedrijf helpen Scout site als beheerder.

9. Nadat u bent aangemeld in Klik op **'Beheren'** van het bovenste menu en selecteer vervolgens **'Bedrijf'** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-helpscout-tutorial/settings1.png) 
 
10. Selecteer **'Verificatie'** uit in het menu links. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-helpscout-tutorial/settings2.png) 

11. Dit gaat u naar de sectie SAML-instellingen en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-helpscout-tutorial/settings3.png) 
 
    a. Kopiëren de **terugposting URL (Assertion Consumer Service URL)** waarde en plak de waarde in de **antwoord-URL** vak in de Azure-portal onder Help Scout **domein en de URL's** sectie.
    
    b. Kopieer de **doelgroep-URI (Service Provider entiteit-ID)** waarde en plak de waarde in de **id** vak in de Azure-portal onder Help Scout **domein en de URL's** sectie.

12. Wisselknop **SAML inschakelen** op en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-helpscout-tutorial/settings4.png) 
 
    a. In **-URL met eenmalige aanmelding** textbox, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
    
    b. Klik op **certificaat uploaden** voor het uploaden van de **Certificate(Base64)** gedownload vanuit Azure-portal.

    c. Voer uw organisatie e domein(en) e.x. - `contoso.com` in de **e-maildomeinen** textbox. U kunt meerdere domeinen scheiden met komma's. Op elk gewenst moment een helpen Scout gebruiker of beheerder dat specifieke domein ingevoerd op de [helpen Scout aanmelden pagina](https://secure.helpscout.net/members/login/) worden doorgestuurd naar identiteitsprovider te verifiëren met hun referenties.

    d. Ten slotte kunt u schakelen **Force SAML aanmelding** als u wilt dat gebruikers alleen aan te melden bij Help Scout via via deze methode. Als u nog steeds laat de optie voor ze zich kunnen aanmelden met hun referenties helpen Scout wilt, kunt u deze uitschakelen uitgeschakeld laten. Zelfs als deze optie is ingeschakeld, zich de eigenaar van een Account altijd aan te melden bij Scout helpen met het wachtwoord van hun account.

    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-help-scout-test-user"></a>Een testgebruiker Scout te maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Scout Help. Help Scout ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker niet al in de Help Scout bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te openen Scout Help.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruikt helpen Scout toegang verlenen.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Help Scout, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **helpen Scout**.

    ![De Scout Help-koppeling in de lijst met toepassingen](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Help Scout in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Scout Help.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

