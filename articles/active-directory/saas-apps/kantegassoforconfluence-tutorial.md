---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor samenloop.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 5468b278d16cbc0373cd268f45820fbc2a207370
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046784"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor samenloop

In deze zelfstudie leert u hoe u Kantega SSO voor samenloop integreren met Azure Active Directory (Azure AD).

Kantega SSO voor samenloop integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kantega SSO voor samenloop heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kantega SSO voor samenloop (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor samenloop, moet u de volgende items:

- Een Azure AD-abonnement
- Een SSO Kantega voor eenmalige aanmelding samenloop ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kantega SSO voor samenloop uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Kantega SSO voor samenloop uit de galerie toe te voegen
Voor het configureren van de integratie van Kantega SSO voor samenloop in Azure AD, moet u Kantega SSO voor samenloop uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor samenloop uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Kantega SSO voor samenloop**.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. Selecteer in het deelvenster resultaten **Kantega SSO voor samenloop**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Kantega SSO voor samenloop op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Kantega SSO voor samenloop is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Kantega SSO voor samenloop tot stand worden gebracht.

In Kantega SSO voor samenloop, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een SSO Kantega voor de testgebruiker samenloop](#creating-a-kantega-sso-for-confluence-test-user)**  : als u wilt een equivalent van Britta Simon in Kantega SSO voor samenloop die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw Kantega SSO voor samenloop toepassing configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor samenloop, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kantega SSO voor samenloop** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. In **IDP** modus gestart op de **Kantega SSO voor samenloop domein en URL's** sectie de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. In **SP** gestart modus selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Deze waarden zijn ontvangen tijdens de configuratie van samenloop-invoegtoepassing, die later in de zelfstudie wordt uitgelegd.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster aanmelden bij uw **samenloop-beheerportal** als beheerder.

8. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon1.png)

9. Onder **ATLASSIAN MARKETPLACE** tabblad **nieuwe invoegtoepassingen zoeken**. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon.png)

10. Search **Kantega SSO voor samenloop SAML Kerberos** en klikt u op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon2.png)

11. De installatie van de invoegtoepassing wordt gestart.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon3.png)

12. Nadat de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon33.png)

13. Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon34.png)
    
14. Klik op **configureren** het configureren van de nieuwe-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon35.png)

15. Deze nieuwe invoegtoepassing kunt u vinden onder **gebruikers en beveiliging** tabblad.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon36.png)
    
16. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** uit de **id-provider toevoegen** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon4.png)

17. Selecteer het abonnementsniveau van als **Basic**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon5.png)     

18. Op de **eigenschappen van de App** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Kopieer de **App ID URI** waarde en deze gebruiken als **-id, de antwoord-URL en de aanmeldings-URL** op de **Kantega SSO voor samenloop domein en URL's** sectie in Azure portal.

    b. Klik op **Volgende**.

19. Op de **metagegevens importeren** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en het bestand met metagegevens uploaden, die u hebt gedownload vanuit Azure portal.

    b. Klik op **Volgende**.

20. Op de **naam en de SSO-locatie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon8.png)
    
    a. Naam van de id-Provider in **identiteit providernaam** tekstvak (bijvoorbeeld Azure AD).

    b. Klik op **Volgende**.

21. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon9.png)

22. Op de **samenloop gebruikersaccounts** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Selecteer **gebruikers in samenloop van interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen met door komma's gescheiden).

    b. Klik op **Volgende**.

23. Klik op **Voltooien**.   

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon11.png)

24. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen uit: 

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Selecteer **bekend domeinen** in het linkerdeelvenster van de pagina.

    b. Voer de domeinnaam van het in de **bekend domeinen** tekstvak.

    c. Klik op **Opslaan**. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Het maken van een SSO Kantega voor samenloop testgebruiker

Als u wilt dat Azure AD-gebruikers zich aanmelden bij samenloop, moeten ze worden ingericht voor samenloop. In het geval van Kantega SSO voor samenloop is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw Kantega SSO voor samenloop bedrijf site als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/kantegassoforconfluence-tutorial/user1.png) 

3. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Op de **'Een gebruiker toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/kantegassoforconfluence-tutorial/user2.png) 

    a. In de **gebruikersnaam** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **volledige naam** tekstvak typt u de volledige naam van gebruiker, zoals Britta Simon.

    c. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** tekstvak typt u het wachtwoord voor de gebruiker.

    e. Klik op **wachtwoord bevestigen** het wachtwoord opnieuw invoeren.
    
    f. Klik op **toevoegen** knop.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Kantega SSO voor samenloop.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Kantega SSO voor samenloop toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Kantega SSO voor samenloop**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Kantega SSO voor samenloop tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Kantega SSO voor samenloop toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforconfluence-tutorial/tutorial_general_203.png

