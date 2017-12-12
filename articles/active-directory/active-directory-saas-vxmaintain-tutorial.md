---
title: 'Zelfstudie: Azure Active Directory integreren met vxMaintain | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en vxMaintain.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: a987247d2c8a76161f9c8a5a027e34b3a8e25b30
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a>Zelfstudie: Azure Active Directory integreren met vxMaintain

In deze zelfstudie leert u hoe vxMaintain integreren met Azure Active Directory (Azure AD).

Deze integratie biedt een aantal belangrijke voordelen. U kunt:

- Beheren in Azure AD die toegang tot vxMaintain heeft.
- Kunnen uw gebruikers automatisch aanmelden bij vxMaintain met eenmalige aanmelding (SSO) via hun Azure AD-accounts.
- Uw accounts op één locatie beheren: de Azure-portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met vxMaintain, moet u de volgende items:

- Een Azure AD-abonnement
- Een vxMaintain abonnement SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie test, wordt u aangeraden een productie-omgeving niet te gebruiken.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat geeft een overzicht van deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* VxMaintain uit de galerie toevoegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-vxmaintain-from-the-gallery"></a>VxMaintain uit de galerie toevoegen
Voor het configureren van de integratie van vxMaintain met Azure AD, moet u vxMaintain uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

Als u wilt toevoegen vxMaintain uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** knop. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster 'Bedrijfstoepassingen'][2]
    
3. Toevoegen van een toepassing in de **alle toepassingen** dialoogvenster, **nieuwe toepassing**.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **vxMaintain**.

    ![De vervolgkeuzelijst 'Single Sign-on modus'](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Selecteer in de lijst met resultaten **vxMaintain**, en selecteer vervolgens **toevoegen**.

    ![De koppeling vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie u configureren en testen van Azure AD-SSO met behulp van vxMaintain, op basis van een testgebruiker genaamd "Britta Simon."

Azure AD moet weten de vxMaintain equivalent aan de Azure AD-gebruiker voor eenmalige aanmelding werkt. Dat wil zeggen, moet u een koppeling relatie tussen de Azure AD-gebruiker en de bijbehorende vxMaintain gebruiker instellen.

Toewijzen om vast te stellen op de koppeling relatie, de vxMaintain **gebruikersnaam** waarde als de Azure AD **gebruikersnaam** waarde.

Als u wilt configureren en testen van Azure AD-SSO met behulp van vxMaintain, voltooi de volgende elementen.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

In deze sectie maakt kunt u zowel Azure AD-eenmalige aanmelding inschakelen in de Azure portal en SSO in uw toepassing vxMaintain als volgt configureren:

1. In de Azure-portal op de **vxMaintain** toepassing Integratiepagina **eenmalige aanmelding**.

    ![De opdracht "Single sign-on"][4]

2. Voor het inschakelen van eenmalige aanmelding, in de **modus voor één aanmelding** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![De opdracht 'op basis van SAML aanmelding'](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. Onder **vxMaintain domein en de URL's**, het volgende doen:

    ![De vxMaintain sectie domein en URL 's](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. In de **id** typt u een URL die heeft de volgende syntaxis:`https://<company name>.verisae.com`

    b. In de **antwoord-URL** typt u een URL die heeft de volgende syntaxis:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id en antwoord-URL. De waarden, neem contact op met de [vxMaintain ondersteuningsteam](http://www.verisae.com/contact-us).
 
4. Onder **SAML-certificaat voor ondertekening van**, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De sectie 'SAML handtekeningcertificaat'](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. Voor het configureren van **vxMaintain** SSO, verzendt de gedownloade **Metadata XML** van het bestand in de [vxMaintain ondersteuningsteam](http://www.verisae.com/contact-us).

> [!TIP]
> Bij het instellen van de app, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Na het toevoegen van de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de ingesloten documentatie van de **configuratie** sectie. 
>
>Zie voor meer informatie over de functie embedded-documentatie, [het beheren van eenmalige aanmelding voor zakelijke apps](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

![De Azure AD-testgebruiker][100]

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop 'Azure Active Directory'](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. Lijst van gebruikers wilt weergeven, gaat u naar **gebruikers en groepen** > **alle gebruikers**.
    
    ![De koppeling 'Alle gebruikers'](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)  
    De **alle gebruikers** dialoogvenster wordt geopend. 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen**.
 
    ![De knop toevoegen](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van de testgebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** uit en noteer de waarde die is gegenereerd in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-vxmaintain-test-user"></a>Een testgebruiker vxMaintain maken

In deze sectie maakt u testgebruiker Britta Simon in vxMaintain. Als gebruikers wilt toevoegen in het platform vxMaintain, werken met de [vxMaintain ondersteuningsteam](http://www.verisae.com/contact-us). Voordat u eenmalige aanmelding gebruiken, maken en activeren van de gebruikers.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u testgebruiker Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot vxMaintain. Ga als volgt te werk om dit te doen:

![Testgebruiker in de lijst weergavenaam][200] 

1. In de Azure portal **toepassingen** weergeven, gaat u naar **Directory** weergave > **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle toepassingen'][201] 

2. In de **toepassingen** selecteert **vxMaintain**.

    ![De koppeling vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][203]

5. In de **gebruikers en groepen** het dialoogvenster de **gebruikers** selecteert **Britta Simon**, en selecteer vervolgens de **Selecteer** knop.

7. In de **toevoegen toewijzing** dialoogvenster, **toewijzen**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Test uw Azure AD eenmalige aanmelding

In deze sectie kunt u de configuratie van uw Azure AD SSO testen met behulp van het toegangsvenster.

Als u de **vxMaintain** tegel in het deelvenster toegang moet aanmelden bij uw toepassing vxMaintain automatisch.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Volgende stappen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

