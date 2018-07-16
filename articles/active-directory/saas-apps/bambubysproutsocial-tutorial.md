---
title: 'Zelfstudie: Azure Active Directory-integratie met Bambu door sociale Sprout | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Bambu door sociale Sprout.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: eba6f2a22de42ce65996dbe0f9b75533d2a4dacc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047889"
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a>Zelfstudie: Azure Active Directory-integratie met Bambu door sociale Sprout

In deze zelfstudie leert u hoe u Bambu door sociale Sprout integreren met Azure Active Directory (Azure AD).

Bambu door sociale Sprout integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Bambu door sociale Sprout heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Bambu door sociale Sprout (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Bambu door sociale Sprout, moet u de volgende items:

- Een Azure AD-abonnement
- Een Bambu door sociale Sprout eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Bambu door sociale Sprout uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bambu-by-sprout-social-from-the-gallery"></a>Bambu door sociale Sprout uit de galerie toe te voegen
Voor het configureren van de integratie van Bambu door sociale Sprout in Azure AD, moet u Bambu door sociale Sprout uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Bambu door sociale Sprout uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster nieuwe toepassing toevoegen.

    ![Toepassingen][3]

4. Typ in het zoekvak **Bambu door sociale Sprout**.

    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. Selecteer in het deelvenster resultaten **Bambu door sociale Sprout**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Bambu door sociale Sprout op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Bambu door sociale Sprout is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Bambu door sociale Sprout tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Bambu door sociale Sprout.

Om te configureren en testen van Azure AD eenmalige aanmelding met Bambu door sociale Sprout, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een Bambu door sociale Sprout testgebruiker](#creating-a-bambu-by-sprout-social-test-user)**  : als u wilt een equivalent van Britta Simon in Bambu door Sprout sociale die is gekoppeld aan de Azure AD-weergave van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Bambu door sociale Sprout toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Bambu door sociale Sprout, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Bambu door sociale Sprout** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. Op de **Bambu door Sprout sociale domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure. 

    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. Op de **Bambu door sociale configuratie Sprout** sectie, klikt u op **Bambu configureren door sociale Sprout** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. Het configureren van eenmalige aanmelding op **Bambu door sociale Sprout** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** en **Single Sign-On Service URL voor SAML** naar [ Bambu door het ondersteuningsteam van sociale Sprout](mailto:support@getbambu.com). Ze stelt dit om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

<!--### Next steps

To ensure users can sign-in to Bambu by Sprout Social after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Bambu by Sprout Social in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/bambubysproutsocial-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **Britta Simon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a>Het maken van een Bambu door sociale Sprout testgebruiker

Toepassing biedt ondersteuning voor Just in time gebruikersinrichting en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Bambu door sociale Sprout.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Bambu door sociale Sprout, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Bambu door sociale Sprout**.

    ![Eenmalige aanmelding configureren](./media/bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Bambu door sociale Sprout tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Bambu door sociale Sprout toepassing. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/bambubysproutsocial-tutorial/tutorial_general_203.png
