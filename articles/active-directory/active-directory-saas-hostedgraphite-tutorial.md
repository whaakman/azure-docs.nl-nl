---
title: 'Zelfstudie: Azure Active Directory-integratie met gehost grafiek | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de grafiek die worden gehost.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 57ee7770193641d0e13da2c7f8aaa6cfc6aefe73
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Zelfstudie: Azure Active Directory-integratie met gehoste-grafiek

In deze zelfstudie leert u hoe gehost grafiek integreren met Azure Active Directory (Azure AD).

Grafiek gehost integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de grafiek die worden gehost heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij gehost grafiek (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met grafiek gehost, moet u de volgende items:

- Een Azure AD-abonnement
- Een grafiek gehost eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Grafiek gehost vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hosted-graphite-from-the-gallery"></a>Grafiek gehost vanuit de galerie toevoegen
Voor het configureren van de integratie van grafiek gehost in Azure AD, moet u de grafiek gehost vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen die worden gehost grafiek uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **grafiek gehost**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. Selecteer in het deelvenster resultaten **grafiek gehost**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met gehost grafiek op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de grafiek die worden gehost in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de grafiek gehost tot stand worden gebracht.

In de grafiek die worden gehost, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met grafiek gehost, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gehost grafiek testgebruiker](#creating-a-hosted-graphite-test-user)**  - gehost grafiek die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing grafiek gehost.

**Voor het configureren van Azure AD eenmalige aanmelding met grafiek gehost, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **grafiek gehost** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. Op de **grafiek domein gehost en URL's** sectie als u wilt configureren van de toepassing in **IDP geïnitieerd modus**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://www.hostedgraphite.com/metadata/<user id>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://www.hostedgraphite.com/complete/saml/<user id>`

4. Op de **grafiek domein gehost en URL's** sectie als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    a. Klik op de **weergeven geavanceerde instellingen voor URL** optie

    b. In de **aanmelding op URL** textbox, typ een URL met het volgende patroon volgen:`https://www.hostedgraphite.com/login/saml/<user id>/`   

    > [!NOTE] 
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U moet deze waarden bijwerken met de werkelijke id, antwoord-URL en meld u op de URL. Als u deze waarden, gaat u naar toegang setup op uw toepassing aan clientzijde of neem contact op met SAML -> [grafiek gehost ondersteuningsteam](mailto:help@hostedgraphite.com).
    >
 
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. Op de **gehost grafiek configuratie** sectie, klikt u op **gehost grafiek configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. Eenmalige aanmelding voor uw tenant grafiek gehost als beheerder.

9. Ga naar de **SAML installatiepagina** in de zijbalk (**toegang SAML-instellingen ->**).
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. Bevestig deze URL's overeenkomen met uw configuratie uitgevoerd op de **grafiek domein gehost en URL's** sectie van de Azure-portal.
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. In **entiteit of ID van de verlener** en **aanmeldings-URL voor eenmalige aanmelding** tekstvakken, plak de waarde van **SAML entiteit-ID** en **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal. 
   
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. Selecteer "**alleen-lezen**' als **standaard gebruikersrol**.
    
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. De base-64 gecodeerde certificaat openen in Kladblok van Azure portal hebt gedownload, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **X.509-certificaat** textbox.
    
    ![Eenmalige aanmelding op App aan clientzijde configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. Klik op **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-hosted-graphite-test-user"></a>Een grafiek gehost testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in de grafiek die worden gehost. Gehoste grafiek ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot gehoste grafiek als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig maken moet, moet u contact op met het ondersteuningsteam grafiek gehost via < mailto:help@hostedgraphite.com >. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang aan de grafiek die worden gehost.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen grafiek gehost, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **grafiek gehost**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel gehost grafiek in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing grafiek gehost.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

