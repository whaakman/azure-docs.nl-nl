---
title: 'Zelfstudie: Azure Active Directory-integratie met RingCentral | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 35033e52fb54177428f8869ebcc462bd9465ad4c
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48871537"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Zelfstudie: Azure Active Directory-integratie met RingCentral

In deze zelfstudie leert u hoe u RingCentral integreren met Azure Active Directory (Azure AD).

RingCentral integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot RingCentral heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij RingCentral (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RingCentral, moet u de volgende items:

- Een Azure AD-abonnement
- Een RingCentral eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. RingCentral uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ringcentral-from-the-gallery"></a>RingCentral uit de galerie toe te voegen
Voor het configureren van de integratie van RingCentral in Azure AD, moet u RingCentral uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RingCentral uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/ringcentral-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/ringcentral-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/ringcentral-tutorial/a_new_app.png)

4. Typ in het zoekvak **RingCentral**, selecteer **RingCentral** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/ringcentral-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met RingCentral op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in RingCentral is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RingCentral tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met RingCentral, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker RingCentral](#create-a-ringcentral-test-user)**  : als u wilt een equivalent van Britta Simon in RingCentral die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing RingCentral.

**Voor het configureren van Azure AD eenmalige aanmelding met RingCentral, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **RingCentral** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/ringcentral-tutorial/b1_b2_select_sso.png)

2. Klik op **modus wijzigen in één aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/ringcentral-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/ringcentral-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/ringcentral-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, hebt u **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    a. Klik op **metagegevensbestand uploaden**.

    ![image](./media/ringcentral-tutorial/b9_saml.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![image](./media/ringcentral-tutorial/b9(1)_saml.png)

    c. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie tekstvak, zoals hieronder wordt weergegeven :

    ![image](./media/ringcentral-tutorial/b21-domains_and_urls.png)

    d. In de **aanmeldings-URL** tekstvak, een URL typen:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    > [!NOTE]
    > U krijgt de **Service Provider-bestand met metagegevens** op de pagina voor RingCentral SSO-configuratie die later in de zelfstudie wordt uitgelegd.

6. Als u geen **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak, een URL typen:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. In de **id** tekstvak, een URL typen:
    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. In de **antwoord-URL** tekstvak, een URL typen:
    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![image](./media/ringcentral-tutorial/b2-domains_and_urls.png)

7. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van het certificaat van de opgegeven opties volgens uw vereiste en sla deze op uw computer.

    ![image](./media/ringcentral-tutorial/certificatebase64.png)
    
8. In een ander browservenster, meld u aan bij RingCentral als een beveiligingsbeheerder.

9. Klik op de bovenkant op **extra**.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

10. Navigeer naar **Single Sign-on**.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

11. Op de **Single Sign-on** pagina onder **SSO-configuratie** sectie van **stap 1** klikt u op **bewerken** en voer de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

12. Op de **instellen Single Sign-on** pagina, voert u de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Klik op **Bladeren** voor het uploaden van het bestand met metagegevens die u hebt gedownload vanuit Azure portal.

    b. Na het uploaden van de metagegevens van de waarden ophalen automatisch ingevuld in **SSO algemene informatie** sectie.

    c. Onder **kenmerk toewijzing** sectie, selecteer **kaart e-kenmerk voor het** als `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Klik op **Opslaan**.

    e. Van **stap 2** klikt u op **downloaden** voor het downloaden van de **Service Provider-bestand met metagegevens** en upload dit in **SAML-basisconfiguratie** sectie polulate automatisch de **id** en **antwoord-URL** waarden in Azure portal.

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. Op dezelfde pagina bevinden, gaat u naar **SSO inschakelen** sectie en voer de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    a. Selecteer **SSO-Service inschakelen**.
    
    b. Selecteer **toestaan dat gebruikers zich kunnen aanmelden met referenties voor eenmalige aanmelding of RingCentral**.

    c. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/ringcentral-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/ringcentral-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/ringcentral-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-ringcentral-test-user"></a>Maak een testgebruiker RingCentral

In deze sectie maakt u een gebruiker met de naam van Britta Simon in RingCentral. Werken met [RingCentral Client ondersteuningsteam](https://success.ringcentral.com/RCContactSupp) om toe te voegen de gebruikers in het RingCentral-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan RingCentral.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/ringcentral-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **RingCentral**.

    ![image](./media/ringcentral-tutorial/d_all_proapplications.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/ringcentral-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/ringcentral-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel RingCentral in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing RingCentral.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/ringcentral-tutorial/tutorial_general_01.png
[2]: ./media/ringcentral-tutorial/tutorial_general_02.png
[3]: ./media/ringcentral-tutorial/tutorial_general_03.png
[4]: ./media/ringcentral-tutorial/tutorial_general_04.png

[100]: ./media/ringcentral-tutorial/tutorial_general_100.png

[200]: ./media/ringcentral-tutorial/tutorial_general_200.png
[201]: ./media/ringcentral-tutorial/tutorial_general_201.png
[202]: ./media/ringcentral-tutorial/tutorial_general_202.png
[203]: ./media/ringcentral-tutorial/tutorial_general_203.png

