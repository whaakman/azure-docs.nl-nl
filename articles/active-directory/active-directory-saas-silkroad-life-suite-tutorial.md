---
title: 'Zelfstudie: Azure Active Directory-integratie met SilkRoad levensduur Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SilkRoad levensduur Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 0d6af7af7d6b28ff3ea9d518a65b8267a83b71b4
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Zelfstudie: Azure Active Directory-integratie met SilkRoad levensduur Suite

In deze zelfstudie leert u hoe SilkRoad levensduur Suite integreren met Azure Active Directory (Azure AD).

SilkRoad levensduur Suite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SilkRoad levensduur Suite heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SilkRoad levensduur Suite (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SilkRoad levensduur Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een SilkRoad levensduur Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SilkRoad levensduur Suite uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>SilkRoad levensduur Suite uit de galerie toevoegen
Voor het configureren van de integratie van SilkRoad levensduur Suite in Azure AD, moet u SilkRoad levensduur Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SilkRoad levensduur Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SilkRoad levensduur Suite**, selecteer **SilkRoad levensduur Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SilkRoad levensduur Suite in de lijst met resultaten](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SilkRoad levensduur Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SilkRoad leven Suite is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SilkRoad leven Suite tot stand worden gebracht.

Wijs in SilkRoad leven Suite, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SilkRoad levensduur Suite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SilkRoad levensduur Suite](#create-a-silkroad-life-suite-test-user)**  - SilkRoad levensduur Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing SilkRoad levensduur Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met SilkRoad levensduur Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SilkRoad levensduur Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_samlbase.png)

3. Op de **SilkRoad levensduur Suite domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en SilkRoad levensduur Suite domein één aanmelding informatie](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_url1.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.silkroad-eng.com/Authentication/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP` |
    | `https://<subdomain>.silkroad.com/Authentication/SP` |

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/` |
    | `https://<subdomain>.silkroad.com/Authentication/` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [SilkRoad levensduur Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_400.png)
    
6. Op de **SilkRoad levensduur Suite configuratie** sectie, klikt u op **configureren SilkRoad levensduur Suite** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van de Suite SilkRoad levensduur](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_configure.png) 

7. Aanmelding bij uw bedrijf SilkRoad site als administrator. 
 
    >[!NOTE] 
    > Toegang tot de toepassing SilkRoad levensduur Suite verificatie voor federatie configureren met Microsoft Azure AD, neem contact op met ondersteuning voor SilkRoad of uw vertegenwoordiger SilkRoad Services.

8. Ga naar **serviceprovider**, en klik vervolgens op **Federation Details**. 
   
    ![Azure AD voor eenmalige aanmelding][10]

9. Klik op **Federatiemetagegevens downloaden**, en sla het bestand met metagegevens op uw computer.
   
    ![Azure AD voor eenmalige aanmelding][11] 

10. In uw **SilkRoad** toepassing, klikt u op **verificatie bronnen**.
   
    ![Azure AD voor eenmalige aanmelding][12] 

11. Klik op **verificatiebron toevoegen**. 
   
    ![Azure AD voor eenmalige aanmelding][13] 

12. In de **verificatie-bron toevoegen** sectie, voert u de volgende stappen uit: 
   
    ![Azure AD voor eenmalige aanmelding][14]
  
    a. Onder **optie 2 - bestand met metagegevens**, klikt u op **Bladeren** voor het uploaden van het metagegevensbestand van de gedownloade vanuit Azure-portal.
  
    b. Klik op **maken id-Provider met behulp van bestandsgegevens**.

13. In de **verificatie bronnen** sectie, klikt u op **bewerken**. 
    
     ![Azure AD voor eenmalige aanmelding][15] 

14. Op de **verificatiebron bewerken** dialoogvenster de volgende stappen uitvoeren: 
    
     ![Azure AD voor eenmalige aanmelding][16] 

    a. Als **ingeschakeld**, selecteer **Ja**.

    b. In de **id van de entiteit** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
   
    c. In de **IdP beschrijving** textbox, typ een beschrijving voor uw configuratie (bijvoorbeeld: *Azure AD SSO*).

    d. In de **metagegevensbestand** textbox, het uploaden van de **metagegevens** -bestand dat u hebt gedownload vanuit Azure-portal.
  
    e. In de **IdP naam** textbox, typ een naam die specifiek is voor uw configuratie (bijvoorbeeld: *Azure SP*).
  
    f. In de **Service-URL voor afmelden** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.

    g. In de **aanmelding service-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    h. Klik op **Opslaan**.

15. Uitschakelen voor alle andere verificatie-bronnen. 
    
     ![Azure AD voor eenmalige aanmelding][17]

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-silkroad-life-suite-test-user"></a>Maak een testgebruiker SilkRoad levensduur Suite

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in SilkRoad leven Suite maken. Werken met [SilkRoad levensduur Suite Client ondersteuningsteam](https://www.silkroad.com/locations/) om toe te voegen de gebruikers van het platform SilkRoad levensduur Suite. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon toegang verlenen aan SilkRoad levensduur Suite gebruikt Azure eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon SilkRoad levensduur Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SilkRoad levensduur Suite**.

    ![De koppeling SilkRoad levensduur Suite in de lijst met toepassingen](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel SilkRoad levensduur Suite in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SilkRoad levensduur Suite.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
