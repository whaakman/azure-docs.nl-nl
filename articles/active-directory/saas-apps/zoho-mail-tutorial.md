---
title: 'Zelfstudie: Azure Active Directory-integratie met Zoho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zoho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: c44eef96bc40e2ccfac9e5dceb106ed2e809dea2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055896"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Zelfstudie: Azure Active Directory-integratie met Zoho

In deze zelfstudie leert u hoe u Zoho integreren met Azure Active Directory (Azure AD).

Zoho integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zoho heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zoho (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zoho, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zoho eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zoho uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zoho-from-the-gallery"></a>Zoho uit de galerie toe te voegen
Voor het configureren van de integratie van Zoho in Azure AD, moet u Zoho uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zoho uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Zoho**, selecteer **Zoho** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Zoho in de lijst met resultaten](./media/zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zoho op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Zoho is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zoho tot stand worden gebracht.

In Zoho, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zoho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Zoho](#create-a-zoho-test-user)**  : als u wilt een equivalent van Britta Simon in Zoho die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zoho.

**Voor het configureren van Azure AD eenmalige aanmelding met Zoho, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zoho** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. Op de **Zoho domein en URL's** sectie, voert u de volgende stappen uit:

    ![Zoho domein en URL's, eenmalige aanmelding informatie](./media/zoho-mail-tutorial/tutorial_zoho_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.zohomail.com`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Zoho Client ondersteuningsteam](https://www.zoho.com/mail/contact.html) deze waarde op te halen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/zoho-mail-tutorial/tutorial_general_400.png)

6. Op de **Zoho configuratie** sectie, klikt u op **configureren Zoho** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, de URL wachtwoord wijzigen en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Zoho configuratie](./media/zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. Meld u in een ander browservenster in uw bedrijf Zoho Mail site als beheerder.

8. Ga naar de **Configuratiescherm**.
   
    ![Het Configuratiescherm](./media/zoho-mail-tutorial/ic789607.png "Configuratiescherm")

9. Klik op de **SAML-verificatie** tabblad.
   
    ![SAML-verificatie](./media/zoho-mail-tutorial/ic789608.png "SAML-verificatie")

10. In de **SAML-verificatiegegevens** sectie, voert u de volgende stappen uit:
   
    ![SAML-verificatiegegevens](./media/zoho-mail-tutorial/ic789609.png "Details van SAML-verificatie")
   
    a. In de **aanmeldings-URL** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
    b. In de **afmeldings-URL van** tekstvak plakken **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
   
    c. In de **URL van wijzigen wachtwoord** tekstvak plakken **URL van wijzigen wachtwoord** die u hebt gekopieerd vanuit Azure portal.
       
    d. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **PublicKey** tekstvak.
   
    e. Als **algoritme**, selecteer **RSA**.
   
    f. Klik op **OK**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zoho-mail-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zoho-mail-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zoho-mail-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zoho-mail-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-zoho-test-user"></a>Maak een testgebruiker Zoho

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Zoho e-Mail, moeten ze worden ingericht voor Zoho e-Mail. In het geval van Zoho Mail is inrichten een handmatige taak.

> [!NOTE]
> Kunt u een andere Zoho Mail gebruiker account hulpmiddelen voor het maken of API's die per E-mail Zoho inrichten AAD-gebruikersaccounts.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zoho Mail** bedrijf site als beheerder.

2. Ga naar **Configuratiescherm \> e-Mail en documenten**.

3. Ga naar **Gebruikersdetails \> gebruiker toevoegen**.
   
    ![Gebruiker toevoegen](./media/zoho-mail-tutorial/ic789611.png "gebruiker toevoegen")

4. Op de **gebruikers toevoegen** dialoogvenster, voer de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/zoho-mail-tutorial/ic789612.png "gebruiker toevoegen")
   
    a. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **Julia**.

    b. In de **achternaam** tekstvak, type de achternaam van de gebruiker, zoals **Simon**.

    c. In de **E-mail-ID** tekstvak, type de e-mail-id van gebruiker, zoals **brittasimon@contoso.com**.

    d. In de **wachtwoord** tekstvak, voer het wachtwoord van gebruiker.
   
    e. Klik op **OK**.  
      
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht met een koppeling naar het account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zoho.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Zoho toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Zoho**.

    ![De koppeling Zoho in de lijst met toepassingen](./media/zoho-mail-tutorial/tutorial_zoho_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zoho in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Zoho.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/zoho-mail-tutorial/tutorial_general_203.png

