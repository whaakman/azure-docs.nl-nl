---
title: 'Zelfstudie: Azure Active Directory-integratie met Learning op het werk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en daarvan te leren op het werk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: b7a097107f68c692c960efc465d00b05e81f7c81
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046852"
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a>Zelfstudie: Azure Active Directory-integratie met Learning op het werk

In deze zelfstudie leert u hoe u om te leren op het werk integreren met Azure Active Directory (Azure AD).

Training op het werk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Learning op het werk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Learning op het werk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Learning op het werk, moet u de volgende items:

- Een Azure AD-abonnement
- Een Learning op werk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Learning toe te voegen op het werk uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-learning-at-work-from-the-gallery"></a>Learning toe te voegen op het werk uit de galerie
Voor het configureren van de integratie van de training op het werk in Azure AD, moet u Learning op het werk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt leren toevoegen op het werk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Learning op het werk**.

    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. Selecteer in het deelvenster resultaten **Learning op het werk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Learning op het werk op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Learning op het werk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker weten op het werk tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Learning op het werk.

Om te configureren en testen van Azure AD eenmalige aanmelding met Learning op het werk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een Learning op werk testgebruiker](#creating-a-learning-at-work-test-user)**  : als u wilt een equivalent van Britta Simon in Learning op het werk dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw studie op Work-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Learning op het werk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Learning op het werk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. Op de **Learning op Werkdomein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_learningatwork_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Learning op werk Client ondersteuningsteam](https://www.learninga-z.com/site/contact/support) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_general_400.png)

6. Op de **Learning op werk configuratie** sectie, klikt u op **Learning configureren op het werk** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. Het configureren van eenmalige aanmelding op **Learning op het werk** zijde, moet u voor het verzenden van de gedownloade **Metadata XML**, **SAML entiteit-ID**, **SAML Single Sign-On-Service URL**, en **afmelding URL** naar [Learning op ondersteuning voor werk](https://www.learninga-z.com/site/contact/support).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/learning-at-work-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-learning-at-work-test-user"></a>Het maken van een Learning op werk testgebruiker

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Learning op het werk. Werken met [Learning op ondersteuning voor werk](https://www.learninga-z.com/site/contact/support) om toe te voegen de gebruikers in het leren op Work-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te leren op het werk.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon om te leren op het werk, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Learning op het werk**.

    ![Eenmalige aanmelding configureren](./media/learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de Learning op de tegel werk in het toegangsvenster klikt, u moet u automatisch aangemeld bij uw studie op Work-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/learning-at-work-tutorial/tutorial_general_203.png

