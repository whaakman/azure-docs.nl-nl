---
title: 'Zelfstudie: Azure Active Directory-integratie met Marketo | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Marketo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 10ce59668127ff918a85caf537e8ab495c542278
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Zelfstudie: Azure Active Directory-integratie met Marketo

In deze zelfstudie leert u hoe Marketo integreren met Azure Active Directory (Azure AD).

Marketo integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Marketo heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Marketo (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Marketo, moet u de volgende items:

- Een Azure AD-abonnement
- Een Marketo-eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Marketo uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-marketo-from-the-gallery"></a>Marketo uit de galerie toevoegen
Voor het configureren van de integratie van Marketo in Azure AD, moet u Marketo uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Marketo uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Marketo**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. Selecteer in het deelvenster resultaten **Marketo**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Marketo op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Marketo is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante Marketo-gebruiker worden gemaakt.

Wijs in Marketo, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Marketo, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Marketo](#creating-a-marketo-test-user)**  - Marketo die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Marketo.

**Voor het configureren van Azure AD eenmalige aanmelding met Marketo, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Marketo** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. Op de **Marketo-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://saml.marketo.com/sp`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [Marketo-ondersteuningsteam](http://investors.marketo.com/contactus.cfm) ophalen van deze waarden.
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. Op de **Marketo configuratie** sectie, klikt u op **configureren Marketo** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. Meld u aan met beheerdersreferenties Marketo bij om Munchkin-Id van uw toepassing, en volgende acties uitvoeren:
   
    a. Aanmelden bij Marketo-app met beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar het menu integratie en klik op de **Munchkin koppeling**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    d. Kopieer de weergegeven op het scherm Munchkin-Id en volledige uw antwoord-URL in de Azure AD-configuratiewizard.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. Volg de eenmalige aanmelding configureren in de toepassing de onderstaande stappen te volgen:
   
    a. Aanmelden bij Marketo-app met beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar het menu integratie en klik op **eenmalige aanmelding**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    d. Als de SAML-instellingen, klikt u op **bewerken** knop.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Ingeschakeld** Single Sign-On-instellingen.
   
    f. Plak de **SAML entiteit-ID**, in de **uitgever-ID** textbox.
   
    g. In de **entiteit-ID** textbox, voert u de URL als `http://saml.marketo.com/sp`.
   
    h. Selecteer de locatie van de gebruiker-ID als **naam-ID-element**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Als uw gebruikers-id geen UPN-naam en wijzig de waarde in het tabblad kenmerk is.
   
    ik. Upload het certificaat dat u hebt gedownload van Azure AD-configuratiewizard. **Sla** de instellingen.
   
    j. Bewerk de instellingen voor pagina's omleiden.
   
    k. Plak de **SAML Single Sign-On Service-URL** in de **aanmeldings-URL** textbox.
   
    l. Plak de **Sign-Out URL** in de **afmelding URL** textbox.
   
    m. In de **Error URL**, Kopieer uw **Marketo exemplaar-URL** en klik op **opslaan** knop instellingen op te slaan.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. Als u wilt inschakelen voor gebruikers met SSO, moet u de volgende acties uitvoeren:
   
    a. Aanmelden bij Marketo-app met beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar de **beveiliging** en klik op **aanmeldingsinstellingen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    d. Controleer de **vereisen SSO** optie en **opslaan** de instellingen.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-marketo-test-user"></a>Een testgebruiker Marketo maken

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in Marketo. Volg deze stappen voor het maken van een gebruiker in Marketo-platform.

1. Aanmelden bij Marketo-app met beheerdersreferenties.

2. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. Navigeer naar de **beveiliging** en klik op **gebruikers en rollen**
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. Klik op de **nieuwe gebruikers uitnodigen** koppeling op het tabblad gebruikers
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. Vul de volgende informatie in de wizard nieuwe gebruikers uitnodigen
   
    a. Voer de gebruiker **e** adres in het tekstvak
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    b. Voer de **voornaam** in het tekstvak
   
    c. Voer de **achternaam** in het tekstvak
   
    d. Klik op **Volgende**

6. In de **machtigingen** tabblad de **userRoles** en klik op **volgende**
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. Klik op de **verzenden** knop de uitnodiging gebruiker te verzenden
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Gebruiker e-mailmelding ontvangt en klik op de koppeling en wijzig het wachtwoord om het account te activeren. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Marketo.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Marketo, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Marketo**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Marketo-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Marketo.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

