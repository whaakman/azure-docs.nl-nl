---
title: 'Zelfstudie: Azure Active Directory-integratie met M-bestanden | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en M-bestanden.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 73d4ca68588e216f6434ad4a1adfa92f66074d7a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>Zelfstudie: Azure Active Directory-integratie met M-bestanden

In deze zelfstudie leert u hoe M bestanden integreren met Azure Active Directory (Azure AD).

M-bestanden worden geïntegreerd met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot M-bestanden heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij M-bestanden (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met M-bestanden, moet u de volgende items:

- Een Azure AD-abonnement
- Een M bestanden eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. M-bestanden uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-m-files-from-the-gallery"></a>M-bestanden uit de galerie toevoegen
Voor het configureren van de integratie van M-bestanden in Azure AD, moet u M-bestanden uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen M-bestanden uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **M bestanden**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. Selecteer in het deelvenster resultaten **M bestanden**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met M-bestanden op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de M-bestanden is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in M bestanden worden gemaakt.

In de M-bestanden, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met M-bestanden, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker M bestanden](#creating-a-m-files-test-user)**  - hebben een equivalent van Britta Simon in M-bestanden dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing M-bestanden.

**Voor het configureren van Azure AD eenmalige aanmelding met M-bestanden, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **M bestanden** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. Op de **M bestanden domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.cloudvault.m-files.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [M bestanden Client ondersteuningsteam](mailto:support@m-files.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. Als u eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, neem contact op met [M bestanden ondersteuningsteam](mailto:support@m-files.com) en geeft u de gedownloade metagegevens.
   
    >[!NOTE]
    >Voer de volgende stappen uit als u wilt configureren van eenmalige aanmelding voor u bureaubladtoepassing M-bestand. Er zijn geen extra stappen vereist als u alleen wilt eenmalige aanmelding configureren voor de webversie M-bestanden.  

7. Volg de volgende stappen om de M-bestand bureaublad toepassing configureren voor eenmalige aanmelding met Azure AD inschakelen. M-om bestanden te downloaden, gaat u naar [M-bestanden downloaden](https://www.m-files.com/en/download-latest-version) pagina.

8. Open de **M bestanden bureaublad instellingen** venster. Klik vervolgens op **toevoegen**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. Op de **Document kluis verbindingseigenschappen** venster de volgende stappen uitvoeren:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    Sectie type, de waarden als volgt onder de Server:  

    a. Voor **naam**, type `<tenant-name>.cloudvault.m-files.com`. 
 
    b. Voor **poortnummer**, type **4466**. 

    c. Voor **Protocol**, selecteer **HTTPS**. 

    d. In de **verificatie** optie **specifieke Windows-gebruiker**. Vervolgens wordt u gevraagd met een pagina ondertekenen. Voeg in uw Azure AD-referenties. 

    e. Voor de **kluis op Server**, selecteert u de betreffende kluis op de server.
 
    f. Klik op **OK**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-m-files-test-user"></a>Een testgebruiker M-bestanden maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in M-bestanden. Werken met [M bestanden ondersteuningsteam](mailto:support@m-files.com) om toe te voegen de gebruikers in de M-bestanden.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding toegang verleent tot M-bestanden gebruiken.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen M-bestanden, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **M bestanden**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel M-bestanden in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing M-bestanden.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

