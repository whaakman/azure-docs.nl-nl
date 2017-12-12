---
title: 'Zelfstudie: Azure Active Directory-integratie met Netsuite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Netsuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: jeedes
ms.openlocfilehash: 23e39f20f5b3faa4f6cfba57508d1649e2bbdaa3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Zelfstudie: Azure Active Directory-integratie met Netsuite

In deze zelfstudie leert u hoe Netsuite integreren met Azure Active Directory (Azure AD).

Netsuite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Netsuite heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Netsuite (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Netsuite, moet u de volgende items:

- Een Azure AD-abonnement
- Een Netsuite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Netsuite uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-netsuite-from-the-gallery"></a>Netsuite uit de galerie toevoegen
Voor het configureren van de integratie van Netsuite in Azure AD, moet u Netsuite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Netsuite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Netsuite**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. Selecteer in het deelvenster resultaten **Netsuite**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Netsuite op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Netsuite is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Netsuite tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Netsuite.

Om te configureren en testen van Azure AD eenmalige aanmelding met Netsuite, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Netsuite](#creating-a-netsuite-test-user)**  - Netsuite die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Netsuite configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Netsuite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Netsuite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. Op de **Netsuite domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Deze zijn niet echte waarden. Deze waarden bijwerken met de werkelijke antwoord-URL. Neem contact op met [Netsuite ondersteuningsteam](http://www.netsuite.com/portal/services/support.shtml) ophalen van deze waarden.
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. Op de **Netsuite configuratie** sectie, klikt u op **configureren Netsuite** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. Een nieuw tabblad openen in uw browser en meld u aan bij uw site van het bedrijf Netsuite als beheerder.

8. Klik in de werkbalk boven aan de pagina op **Setup**, klikt u vervolgens op **Installatiebeheer**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. Van de **instellingstaken** selecteert **integratie**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. In de **verificatie beheren** sectie, klikt u op **SAML Single Sign-on**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. Op de **SAML Setup** pagina, voert u de volgende stappen uit:
   
    a. Kopiëren de **SAML Single Sign-On Service-URL** waarde uit de **Naslaggids** sectie van **eenmalige aanmelding configureren** en plak deze in de **identiteit Provider aanmelding Pagina** veld Netsuite.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    b. Selecteer in de Netsuite, **primaire methode voor netwerkverificatie**.

    c. Voor het veld met het label **SAMLV2 identiteit Provider metagegevens**, selecteer **IDP metagegevens-bestand uploaden**. Klik vervolgens op **Bladeren** voor het uploaden van het bestand met metagegevens dat u hebt gedownload vanuit Azure-portal.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    d. Klik op **indienen**.

12. In Azure AD, klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje en het kenmerk toevoegen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. Voor de **kenmerknaam** veld, typt u in `account`. Voor de **kenmerkwaarde** veld, typt u in uw Netsuite account-ID. Deze waarde is constante en wijzigen met account. Instructies voor het vinden van je account-ID vindt u hieronder:

      ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    a. Klik in de Netsuite, **Setup** in het menu van de bovenste navigatiebalk.

    b. Klik onder de **instellingstaken** gedeelte van het linkernavigatiegedeelte menu, selecteer de **integratie** sectie en op **Web Services-voorkeuren**.

    c. Uw Account-ID Netsuite Kopieer en plak deze in de **kenmerkwaarde** veld in Azure AD.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. Voordat gebruikers eenmalige aanmelding in Netsuite uitvoeren kunnen, moeten worden ze eerst de juiste machtigingen in Netsuite toegewezen. Volg de onderstaande instructies om deze machtigingen te wijzen.

    a. Klik op de bovenste navigatiebalk menu **Setup**, klikt u vervolgens op **Installatiebeheer**.
      
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    b. Selecteer in het menu linkernavigatievenster **gebruikers/rollen**, klikt u vervolgens op **rollen beheren**.
      
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    c. Klik op **nieuwe rol**.

    d. Typ in een **naam** voor uw nieuwe rol en selecteer de **eenmalige aanmelding alleen** selectievakje.
      
      ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    e. Klik op **Opslaan**.

    f. Klik in het menu bovenaan op **machtigingen**. Klik vervolgens op **Setup**.
      
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    g. Selecteer **ingesteld Up SAM Single Sign-on**, en klik vervolgens op **toevoegen**.

    h. Klik op **Opslaan**.

    ik. Klik op de bovenste navigatiebalk menu **Setup**, klikt u vervolgens op **Installatiebeheer**.
      
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    j. Selecteer in het menu linkernavigatievenster **gebruikers/rollen**, klikt u vervolgens op **gebruikers beheren**.
      
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    k. Selecteer een testgebruiker. Klik vervolgens op **bewerken**.
      
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    l. Selecteer de rol die u hebt gemaakt en klik in het dialoogvenster rollen **toevoegen**.
      
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    m. Klik op **Opslaan**.
    
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 

### <a name="creating-a-netsuite-test-user"></a>Een testgebruiker Netsuite maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Netsuite. Netsuite ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker in Netsuite nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot Netsuite.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Netsuite.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Netsuite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Netsuite**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u wilt testen van uw instellingen voor eenmalige aanmelding, opent u het toegangsvenster op [https://myapps.microsoft.com](https://myapps.microsoft.com/), meld u aan bij de testaccount en klikt u op **Netsuite**.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

