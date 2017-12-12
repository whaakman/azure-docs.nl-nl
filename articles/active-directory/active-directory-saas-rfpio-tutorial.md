---
title: 'Zelfstudie: Azure Active Directory-integratie met RFPIO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RFPIO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 57bb448f1bb858d15a7ecaad5a9ba797f5d7d85e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Zelfstudie: Azure Active Directory-integratie met RFPIO

In deze zelfstudie leert u hoe RFPIO integreren met Azure Active Directory (Azure AD).

RFPIO integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen wie in Azure AD die toegang heeft tot RFPIO.
- U kunt uw gebruikers automatisch ophalen aangemeld bij RFPIO (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RFPIO, moet u de volgende items:

- Een Azure AD-abonnement.
- Een RFPIO eenmalige aanmelding op ingeschakeld abonnement.

> [!NOTE]
> Niet aanbevolen dat u een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. RFPIO uit de galerie toevoegen.
2. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-rfpio-from-the-gallery"></a>RFPIO uit de galerie toevoegen
Voor het configureren van de integratie van RFPIO in Azure AD, moet u RFPIO uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

### <a name="to-add-rfpio-from-the-gallery"></a>RFPIO uit de galerie toevoegen

1. In de  **[Azure-portal](https://portal.azure.com)**, Selecteer op het navigatiedeelvenster links de **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **RFPIO**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. Selecteer in het deelvenster resultaten **RFPIO**, en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met RFPIO op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat is de relatie tussen de gebruiker die equivalent in RFPIO en een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in RFPIO tot stand worden gebracht.

De waarde van toewijzen in RFPIO, **gebruikersnaam** in Azure AD als de waarde van **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configuring-azure-ad-single-sign-on)**--zodat uw gebruikers om deze functie te gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**--voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker RFPIO](#creating-a-rfpio-test-user)**  --RFPIO die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**--Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#testing-single-sign-on)**  --controleren als de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing RFPIO configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met RFPIO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **RFPIO** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. Op de **RFPIO domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    a. In de **id** textbox, typ de URL:`https://www.rfpio.com`

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Controleer **weergeven geavanceerde instellingen voor URL**.

    c. In de **Relay status** textbox een tekenreekswaarde invoeren. Neem contact op met [RFPIO ondersteuningsteam](https://www.rfpio.com/contact/) deze waarde op te halen. 

4. Controleer **weergeven geavanceerde instellingen voor URL**. Als u wilt configureren van de toepassing in **SP** modus gestart: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    In de **aanmelden URL** textbox, typ de URL:`https://www.app.rfpio.com`

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. In een ander browservenster, meld u aan bij de **RFPIO** website als beheerder.

8. Klik op de vervolgkeuzelijst van de linkerhoek onder.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. Klik op de **functies & integratie**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. In de **SAML SSO configuratie** klikt u op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. In deze sectie voert u volgende acties:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    a. Kopieer de inhoud van de **gedownload van metagegevens-XML** en plak deze in de **identiteit configuratie** veld.

    > [!NOTE]
    >Kopiëren van de inhoud van gedownload **Metadata XML** gebruik **Kladblok ++** of de juiste **XML-Editor**. 

    b. Klik op **valideren**.

    c. Wanneer u op **valideren**, spiegelen **SAML(Enabled)** op on.

    d. Klik op **indienen**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-rfpio-test-user"></a>Een testgebruiker RFPIO maken

Om Azure AD-gebruikers zich aanmelden bij RFPIO, moeten ze worden ingericht in RFPIO.  
In het geval van RFPIO is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf RFPIO site als beheerder.

2. Klik op de vervolgkeuzelijst van de linkerhoek onder.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. Klik op de **organisatie-instellingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. Klik op **TEAMLEDEN**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. Klik op **leden toevoegen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. In de **nieuwe leden toevoegen** sectie. Voer de volgende acties:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/app8.png)

    a. Voer **e-mailadres** in de **Voer één e-mailadres per regel** veld.

    b. Selecteer Plese **rol** volgens uw vereisten.

    c. Klik op **leden toevoegen**.
        
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan RFPIO.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen RFPIO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **RFPIO**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Als u op de tegel RFPIO in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing RFPIO.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

