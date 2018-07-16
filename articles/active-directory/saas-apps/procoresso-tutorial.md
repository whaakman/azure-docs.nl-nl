---
title: 'Zelfstudie: Azure Active Directory-integratie met Procore SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Procore eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bd84224f4c3a8a498a296ff50190713111895472
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051612"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Zelfstudie: Azure Active Directory-integratie met Procore eenmalige aanmelding

In deze zelfstudie leert u hoe u Procore SSO integreren met Azure Active Directory (Azure AD).

Procore SSO integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Procore eenmalige aanmelding heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Procore SSO (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Procore eenmalige aanmelding, moet u de volgende items:

- Een Azure AD-abonnement
- Een Procore SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Procore SSO uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-procore-sso-from-the-gallery"></a>Procore SSO uit de galerie toe te voegen
Voor het configureren van de integratie van Procore SSO in Azure AD, moet u Procore SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Procore SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Procore SSO**.

    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/tutorial_procoresso_search.png)

5. Selecteer in het deelvenster resultaten **Procore SSO**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Procore eenmalige aanmelding op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Procore SSO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Procore eenmalige aanmelding tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Procore eenmalige aanmelding.

Als u wilt configureren en Azure AD eenmalige aanmelding met Procore eenmalige aanmelding testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Procore SSO](#creating-a-procore-sso-test-user)**  : als u wilt een equivalent van Britta Simon in Procore eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure Management portal en configureren van eenmalige aanmelding in uw toepassing Procore eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met Procore eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **Procore SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Op de **Procore SSO-domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_url.png)

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_general_400.png)

6. Op de **Procore SSO-configuratie** sectie, klikt u op **Procore eenmalige aanmelding configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Het configureren van eenmalige aanmelding op **Procore SSO** side, meld u aan bij uw site procore bedrijf als beheerder.

8. In de vervolgkeuzelijst werkset omlaag, klik op **Admin** de SSO-instellingen voor pagina te openen.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/procore_tool_admin.png)

9. Plak de waarden in de vakken zoals beschreven onder-

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. In de **eenmalige aanmelding op URL-verlener** vak, plak de SAML-entiteit-ID opgehaald uit de Azure-portal.

    b. In de **SAML Meld u op de doel-URL** vak, plak de SAML Single Sign-On Service-URL hebt gekopieerd vanuit Azure portal.

    c. Open nu het **Metadata XML** hierboven gedownload vanuit Azure portal en het certificaat in de tag met de naam kopiëren **X509Certificate**. Plak de gekopieerde waarde in de **Single Sign On x509 certificaat** vak.

10. Klik op **wijzigingen opslaan**.

11. Na deze instellingen zijn, moet u voor het verzenden van de **domeinnaam** (bijvoorbeeld **contoso.com**) via waarmee u zich bij Procore naar de [Procore ondersteuningsteam](https://support.procore.com/) en deze zullen activeren van federatieve SSO voor dat domein.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/procoresso-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-procore-sso-test-user"></a>Het maken van een testgebruiker Procore eenmalige aanmelding

Volg de onderstaande stappen voor het maken van een Procore testgebruiker op hun kant.

1. Meld u aan met uw site procore bedrijf als beheerder.  

2. In de vervolgkeuzelijst werkset omlaag, klik op **Directory** om het bedrijf directory-pagina te openen.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Klik op **toevoegen van een persoon** optie voor het openen van het formulier en voer de volgende mogelijkheden: uitvoeren

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_add.png)

    a. In de **voornaam** tekstvak de voornaam van de gebruiker van het type, zoals **Julia**.

    b. In de **achternaam** tekstvak, de achternaam van de gebruiker van het type, zoals **Simon**.

    c. In de **e-mailadres** tekstvak van de gebruiker van het type e-mailadres zoals **BrittaSimon@contoso.com**.

    d. Selecteer **machtigingssjabloon** als **machtigingssjabloon Later toepassen**.

    e. Klik op **Create**.

4. Controleer en de details voor de toegevoegde contactpersoon bijwerken.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_check.png)

5. Klik op **opslaan en verzenden Invitiation** (als een uitnodiging via e-mail vereist is) of **opslaan** (opslaan rechtstreeks) om de Gebruikersregistratie te voltooien.
    
    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/Procore_user_save.png)  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Procore eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Procore SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Procore SSO**.

    ![Eenmalige aanmelding configureren](./media/procoresso-tutorial/tutorial_procoresso_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). Wanneer u op de tegel Procore eenmalige aanmelding in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Procore eenmalige aanmelding.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/procoresso-tutorial/tutorial_general_01.png
[2]: ./media/procoresso-tutorial/tutorial_general_02.png
[3]: ./media/procoresso-tutorial/tutorial_general_03.png
[4]: ./media/procoresso-tutorial/tutorial_general_04.png

[100]: ./media/procoresso-tutorial/tutorial_general_100.png

[200]: ./media/procoresso-tutorial/tutorial_general_200.png
[201]: ./media/procoresso-tutorial/tutorial_general_201.png
[202]: ./media/procoresso-tutorial/tutorial_general_202.png
[203]: ./media/procoresso-tutorial/tutorial_general_203.png

