---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor FishEye/filterkroes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: cbed5303a605579c595a2230c2e18193f93d4366
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes

In deze zelfstudie leert u hoe Kantega SSO voor FishEye/filterkroes integreren met Azure Active Directory (Azure AD).

Kantega SSO voor FishEye/filterkroes integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kantega SSO voor FishEye/filterkroes heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kantega SSO voor FishEye/filterkroes (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor FishEye/filterkroes, moet u de volgende items:

- Een Azure AD-abonnement
- Een Kantega SSO voor eenmalige aanmelding FishEye/filterkroes abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kantega SSO voor FishEye/filterkroes uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Kantega SSO voor FishEye/filterkroes uit de galerie toevoegen
Voor het configureren van de integratie van Kantega SSO voor FishEye/filterkroes in Azure AD, moet u Kantega SSO voor FishEye/filterkroes uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor FishEye/filterkroes uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Kantega SSO voor FishEye/filterkroes**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_search.png)

5. Selecteer in het deelvenster resultaten **Kantega SSO voor FishEye/filterkroes**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Kantega SSO voor FishEye/filterkroes is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Kantega SSO voor FishEye/filterkroes tot stand worden gebracht.

Wijs in Kantega SSO voor FishEye/filterkroes, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SSO Kantega voor FishEye/filterkroes testgebruiker](#creating-a-kantega-sso-for-fisheyecrucible-test-user)**  - Kantega SSO voor FishEye/filterkroes die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw Kantega SSO voor FishEye/filterkroes toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kantega SSO voor FishEye/filterkroes** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_samlbase.png)

3. In **IDP** modus gestart op de **Kantega SSO voor FishEye/filterkroes domein en URL's** sectie de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url1.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. In **SP** geïnitieerd modus selectievakje **weergeven geavanceerde instellingen voor URL** en voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_url2.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Deze waarden worden ontvangen tijdens de configuratie van FishEye/filterkroes invoegtoepassing die verderop in de zelfstudie wordt beschreven.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster, meld u aan bij uw FishEye/filterkroes op de lokale server als beheerder.

8. Beweeg de muisaanwijzer op het tandwiel en klik op de **invoegtoepassingen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon1.png)

9. Klik bij systeeminstellingen op **vinden van nieuwe invoegtoepassingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/add-on2.png)

10. Search **Kantega SSO voor filterkroes** en klik op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon2.png)

11. De installatie van de invoegtoepassing wordt gestart. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon33.png)

12. Zodra de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon34.png)

13. Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon35.png)

14. Klik op **configureren** voor het configureren van de nieuwe invoegtoepassing.    

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon3.png)

15. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** van de **toevoegen identiteitsprovider** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon4.png)

16. Abonnement als selecteren **Basic**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon5.png)

17. Op de **App-eigenschappen** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Kopieer de **App ID URI** waarde en deze gebruiken als **id, de antwoord-URL en de aanmeldings-URL** op de **Kantega SSO voor FishEye/filterkroes domein en URL's** sectie in Azure-portal.

    b. Klik op **Volgende**.

18. Op de **metagegevens importeren** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en de metagegevens-uploadbestand, die u hebt gedownload vanuit Azure-portal.

    b. Klik op **Volgende**.

19. Op de **en SSO** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Naam van de identiteitsprovider in toevoegen **identiteit providernaam** textbox (bijvoorbeeld Azure AD).

    b. Klik op **Volgende**.

20. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.  

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon9.png)

21. Op de **vissenoog gebruikersaccounts** sectie, voert u de volgende stappen:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Selecteer **gebruikers in de FishEye interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen van elkaar gescheiden door komma's).

    b. Klik op **Volgende**.

22. Klik op **Voltooien**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon11.png)

23. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Selecteer **domeinen bekend** in het linkerpaneel van de pagina.

    b. Geef de domeinnaam in de **domeinen bekend** textbox.

    c. Klik op **Opslaan**.  

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kantega-sso-for-fisheyecrucible-test-user"></a>Maken van een SSO Kantega voor FishEye/filterkroes testgebruiker

Om Azure AD-gebruikers zich aanmelden bij FishEye/filterkroes, moeten ze worden ingericht in FishEye/filterkroes. In Kantega SSO voor FishEye/filterkroes is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw filterkroes op de lokale server als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klik op de **gebruikers**.

    ![Werknemer toevoegen](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user1.png) 

3. Onder **gebruikers** tabblad gedeelte, klikt u op **gebruiker toevoegen**.

    ![Werknemer toevoegen](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user2.png)

4. Op de **nieuwe gebruiker toevoegen** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/user3.png) 

    a. In de **gebruikersnaam** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.
    
    b. In de **weergavenaam** textbox weergavenaam van de gebruiker zoals Britta Simon.
    
    c. In de **e-mailadres** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** textbox, typt u het wachtwoord van gebruiker.  

    e. In de **wachtwoord bevestigen** textbox, voer het wachtwoord van gebruiker opnieuw in.

    f. Klik op **Add**.   

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Kantega SSO voor FishEye/filterkroes.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Kantega SSO voor FishEye/filterkroes, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Kantega SSO voor FishEye/filterkroes**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_kantegassoforfisheyecrucible_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Kantega SSO FishEye/filterkroes tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Kantega SSO voor FishEye/filterkroes toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforfisheyecrucible-tutorial/tutorial_general_203.png

