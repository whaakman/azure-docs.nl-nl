---
title: 'Zelfstudie: Azure Active Directory-integratie met SumoLogic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 9b737ce50c1b52c771af275f5722bfd92b2f1491
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Zelfstudie: Azure Active Directory-integratie met SumoLogic

In deze zelfstudie leert u hoe SumoLogic integreren met Azure Active Directory (Azure AD).

SumoLogic integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SumoLogic heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SumoLogic (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SumoLogic, moet u de volgende items:

- Een Azure AD-abonnement
- Een SumoLogic eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SumoLogic uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sumologic-from-the-gallery"></a>SumoLogic uit de galerie toevoegen
Voor het configureren van de integratie van SumoLogic in Azure AD, moet u SumoLogic uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SumoLogic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **SumoLogic**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. Selecteer in het deelvenster resultaten **SumoLogic**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met SumoLogic op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SumoLogic is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SumoLogic tot stand worden gebracht.

Wijs in SumoLogic, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SumoLogic, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SumoLogic](#creating-a-sumologic-test-user)**  - SumoLogic die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing SumoLogic configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SumoLogic, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SumoLogic** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. Op de **SumoLogic domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.SumoLogic.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [SumoLogic Client ondersteuningsteam](https://www.sumologic.com/contact-us/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. Op de **SumoLogic configuratie** sectie, klikt u op **configureren SumoLogic** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf SumoLogic site als beheerder.

8. Ga naar **beheren \> beveiliging**.
   
    ![Beheren](./media/active-directory-saas-sumologic-tutorial/ic778556.png "beheren")

9. Klik op **SAML**.
   
    ![Globale beveiligingsinstellingen](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globale beveiligingsinstellingen")

10. Van de **selecteert u een configuratie of maak een nieuwe** Selecteer **Azure AD**, en klik vervolgens op **configureren**.
   
    ![Configureren van SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "SAML 2.0 configureren")

11. Op de **configureren SAML 2.0** dialoogvenster de volgende stappen uitvoeren:
   
    ![Configureren van SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "SAML 2.0 configureren")
   
    a. In de **configuratienaam** textbox type **Azure AD**. 

    b. Selecteer **foutopsporingsmodus**.

    c. In de **verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 

    d. In de **Authn aanvraag-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    e. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plak het gehele certificaat in **X.509-certificaat** textbox.

    f. Als **e kenmerk**, selecteer **gebruik SAML onderwerp**.  

    g. Selecteer **SP geïnitieerd aanmelding configuratie**.

    h. In de **aanmelding pad** textbox type **Azure** en klik op **opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sumologic-test-user"></a>Een testgebruiker SumoLogic maken

Om Azure AD-gebruikers zich aanmelden bij SumoLogic inschakelt, moeten ze worden ingericht op SumoLogic.  

* In het geval van SumoLogic is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **SumoLogic** tenant.

2. Ga naar **beheren \> gebruikers**.
   
    ![Gebruikers](./media/active-directory-saas-sumologic-tutorial/ic778561.png "gebruikers")

3. Klik op **Add**.
   
    ![Gebruikers](./media/active-directory-saas-sumologic-tutorial/ic778562.png "gebruikers")

4. Op de **nieuwe gebruiker** dialoogvenster de volgende stappen uitvoeren:
   
    ![Nieuwe gebruiker](./media/active-directory-saas-sumologic-tutorial/ic778563.png "nieuwe gebruiker") 
 
    a. Typ de bijbehorende gegevens van de Azure AD-account dat u wilt inrichten in de **voornaam**, **achternaam**, en **e** tekstvakken.
  
    b. Selecteer een rol.
  
    c. Als **Status**, selecteer **Active**.
  
    d. Klik op **Opslaan**.

>[!NOTE]
>U kunt andere SumoLogic gebruiker account hulpmiddelen voor het maken of API's die is geleverd door SumoLogic aan inrichten AAD-gebruikersaccounts. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SumoLogic.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen SumoLogic, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SumoLogic**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel SumoLogic in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SumoLogic.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

