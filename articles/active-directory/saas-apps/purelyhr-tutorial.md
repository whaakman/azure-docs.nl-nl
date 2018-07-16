---
title: 'Zelfstudie: Azure Active Directory-integratie met PurelyHR | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PurelyHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 3e3546a5161c62c137af0a29daf5140133f91eb6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046104"
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a>Zelfstudie: Azure Active Directory-integratie met PurelyHR

In deze zelfstudie leert u hoe u PurelyHR integreren met Azure Active Directory (Azure AD).

PurelyHR integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PurelyHR heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij PurelyHR (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PurelyHR, moet u de volgende items:

- Een Azure AD-abonnement
- Een PurelyHR eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PurelyHR uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-purelyhr-from-the-gallery"></a>PurelyHR uit de galerie toe te voegen
Voor het configureren van de integratie van PurelyHR in Azure AD, moet u PurelyHR uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PurelyHR uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **PurelyHR**.

    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/tutorial_purelyhr_search.png)

5. Selecteer in het deelvenster resultaten **PurelyHR**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met PurelyHR op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in PurelyHR is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in PurelyHR tot stand worden gebracht.

In PurelyHR, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met PurelyHR, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker PurelyHR](#creating-a-purelyhr-test-user)**  : als u wilt een equivalent van Britta Simon in PurelyHR die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing PurelyHR.

**Voor het configureren van Azure AD eenmalige aanmelding met PurelyHR, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PurelyHR** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. Op de **PurelyHR domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyID>.purelyhr.com/sso-consume`

4. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<companyID>.purelyhr.com/sso-initiate`
     
    > [!NOTE]
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [PurelyHR Client ondersteuningsteam](http://support.purelyhr.com/) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_general_400.png)
    
7. Op de **PurelyHR configuratie** sectie, klikt u op **configureren PurelyHR** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. Het configureren van eenmalige aanmelding op **PurelyHR** side, meld u aan bij de website als beheerder.

9. Open de **Dashboard** van de opties in de werkbalk en klikt u op **SSO-instellingen**.

10. Plak de waarden in de vakken zoals beschreven onder-

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)  

    a. Open de **Certificate(Bas64)** gedownload vanuit Azure portal in Kladblok en kopieer de waarde van het certificaat. Plak de gekopieerde waarde in de **X.509-certificaat** vak.

    b. In de **URL-verlener Idp** vak, plak de **SAML entiteit-ID** gekopieerd vanuit Azure portal.

    c. In de **Idp eindpunt-URL** vak, plak de **Single Sign-On Service URL voor SAML** gekopieerd vanuit Azure portal. 

    d. Controleer de **gebruikers automatisch maakt** selectievakje in om in te schakelen automatisch gebruikers inrichten in PurelyHR.

    e. Klik op **wijzigingen opslaan** de instellingen op te slaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/purelyhr-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-purelyhr-test-user"></a>Het maken van een testgebruiker PurelyHR

Als u wilt dat Azure AD-gebruikers zich aanmelden bij PurelyHR, moeten ze worden ingericht voor PurelyHR. Inrichting is een automatische taak in PurelyHR, en er geen handmatige stappen zijn vereist bij het automatisch inrichten van gebruikers is ingeschakeld.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan PurelyHR.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan PurelyHR toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **PurelyHR**.

    ![Eenmalige aanmelding configureren](./media/purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Klik op de absorberen LMS-tegel in het toegangsvenster, u u automatisch aangemeld bij uw absorberen LMS-toepassing.

Zie voor meer informatie over het toegangsvenster. [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/purelyhr-tutorial/tutorial_general_203.png

