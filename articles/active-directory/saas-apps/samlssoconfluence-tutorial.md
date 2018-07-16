---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor samenloop resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor samenloop resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe1960fb07a7cb62246f0eefe9563d6ee2b13f5c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045893"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor samenloop resolutie GmbH

In deze zelfstudie leert u over het integreren van SAML SSO voor samenloop resolutie GmbH met Azure Active Directory (Azure AD).

Integratie van SAML SSO voor samenloop resolutie GmbH met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML SSO voor samenloop resolutie GmbH heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAML SSO voor samenloop door resolutie GmbH (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor samenloop resolutie GmbH, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML SSO voor samenloop resolutie GmbH eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van SAML SSO voor samenloop resolutie GmbH uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>Toevoegen van SAML SSO voor samenloop resolutie GmbH uit de galerie

Voor het configureren van de integratie van SAML SSO voor samenloop resolutie GmbH in Azure AD, moet u de SAML SSO voor samenloop resolutie GmbH uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**SAML SSO voor samenloop met resolutie GmbH uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SAML SSO voor samenloop resolutie GmbH**.

    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. Selecteer in het deelvenster resultaten **SAML SSO voor samenloop resolutie GmbH**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAML SSO voor samenloop door resolutie die GmbH op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente gebruiker in de SAML SSO voor samenloop resolutie GmbH aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAML SSO voor samenloop resolutie GmbH moet tot stand worden gebracht.

In de SAML SSO voor samenloop resolutie GmbH, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor samenloop resolutie GmbH, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een SAML SSO voor samenloop door resolutie GmbH testgebruiker](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de SAML SSO voor samenloop resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in de SAML SSO voor samenloop configureren door de resolutie GmbH toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor samenloop resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAML SSO voor samenloop resolutie GmbH** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. Op de **SAML SSO voor samenloop resolutie GmbH domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

4. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [SAML SSO voor samenloop resolutie GmbH Client ondersteuningsteam](https://www.resolution.de/go/support) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_general_400.png)    
    
7. In een ander browservenster aanmelden bij uw **SAML SSO voor samenloop door resolutie GmbH-beheerportal** als beheerder.

8. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **invoegtoepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon1.png)

9. U bent omgeleid naar beheerderstoegang pagina. Voer het wachtwoord en klikt u op **bevestigen** knop.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon2.png)

10. Onder **ATLASSIAN MARKETPLACE** tabblad **nieuwe invoegtoepassingen zoeken**. 

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon.png)

11. Search **SAML eenmalige aanmelding (SSO) voor samenloop** en klikt u op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon7.png)

12. De installatie van de invoegtoepassing wordt gestart. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon8.png)

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon9.png)

13. Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon10.png)
    
14. Klik op **configureren** het configureren van de nieuwe-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon11.png)

15. Deze nieuwe invoegtoepassing kunt u vinden onder **gebruikers en beveiliging** tabblad.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon3.png)
    
16. Op **SAML-configuratie van invoegtoepassing SingleSignOn** pagina, klikt u op **toevoegen van nieuwe IdP** knop om de instellingen van id-Provider te configureren.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon4.png)

17. Op **kiest u de SAML-identiteitsprovider** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. Stel **Azure AD** als het type id-provider.
    
    b. Voeg **naam** van de id-Provider (bijvoorbeeld Azure AD).
    
    c. Voeg **beschrijving** van de id-Provider (bijvoorbeeld Azure AD).
    
    d. Klik op **Volgende**.
    
18. Op **id-providerconfiguratie** pagina, klikt u op **volgende** knop.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon5b.png)

19. Op **metagegevens importeren SAML-id-provider** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon5c.png)

    a. Klik op **bestand laden** knop en kies Metadata XML-bestand die u in stap 5 hebt gedownload.

    b. Klik op **importeren** knop.
    
    c. Wacht even totdat importeren is geslaagd.
    
    d. Klik op **volgende** knop.
    
20. Op **gebruikers-ID-kenmerk en transformatie** pagina, klikt u op **volgende** knop.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon5d.png)
    
21. Op **gebruiker maken en bijwerken** pagina, klikt u op **opslaan & volgende** instellingen op te slaan.   
    
    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon6a.png)
    
22. Op **uw instellingen testen** pagina, klikt u op **overslaan test & handmatig configureren** test van de gebruiker nu over te slaan. Dit wordt uitgevoerd in de volgende sectie en bepaalde instellingen in Azure portal vereist. 
    
    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon6b.png)
    
23. In het dialoogvenster apprearing lezen **overslaan van de test-middelen...** , klikt u op **OK**.
    
    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/addon6c.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/samlssoconfluence-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>Het maken van een SAML SSO voor samenloop door de gebruiker van het probleem zou moeten GmbH testen

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij de SAML SSO voor samenloop resolutie GmbH, moeten deze in de SAML SSO voor samenloop worden ingericht door resolutie GmbH.  
In de SAML SSO voor samenloop resolutie GmbH is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw SAML SSO voor samenloop door resolutie GmbH bedrijf site als beheerder.

2. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **Gebruikersbeheer**.

    ![Werknemer toevoegen](./media/samlssoconfluence-tutorial/user1.png) 

3. Klik onder de sectie gebruikers op **gebruikers toevoegen** tabblad. Op de **'Een gebruiker toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/samlssoconfluence-tutorial/user2.png) 

    a. In de **gebruikersnaam** tekstvak typt u het e-mailadres van gebruiker, zoals Britta Simon.

    b. In de **volledige naam** tekstvak typt u de volledige naam van gebruiker, zoals Britta Simon.

    c. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. In de **wachtwoord** tekstvak typt u het wachtwoord voor Britta Simon.

    e. Klik op **wachtwoord bevestigen** het wachtwoord opnieuw invoeren.
    
    f. Klik op **toevoegen** knop.    

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SAML SSO voor samenloop resolutie GmbH.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon toewijzen aan SAML SSO voor samenloop met resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SAML SSO voor samenloop resolutie GmbH**.

    ![Eenmalige aanmelding configureren](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u de SAML SSO voor samenloop door resolutie GmbH tegel in het toegangsvenster, u moet u automatisch aangemeld bij de SAML SSO voor samenloop resolutie GmbH toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/samlssoconfluence-tutorial/tutorial_general_203.png

