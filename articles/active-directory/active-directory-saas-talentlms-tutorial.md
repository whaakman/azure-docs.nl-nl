---
title: 'Zelfstudie: Azure Active Directory-integratie met TalentLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TalentLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: f28d6fbfad9dae578a20db7218b7e3b174ed859c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Zelfstudie: Azure Active Directory-integratie met TalentLMS

In deze zelfstudie leert u hoe TalentLMS integreren met Azure Active Directory (Azure AD).

TalentLMS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TalentLMS heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TalentLMS (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TalentLMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een TalentLMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TalentLMS uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-talentlms-from-the-gallery"></a>TalentLMS uit de galerie toevoegen
Voor het configureren van de integratie van TalentLMS in Azure AD, moet u TalentLMS uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TalentLMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TalentLMS**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. Selecteer in het deelvenster resultaten **TalentLMS**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met TalentLMS op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TalentLMS is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in TalentLMS tot stand worden gebracht.

Wijs in TalentLMS, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TalentLMS, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker TalentLMS](#creating-a-talentlms-test-user)**  - TalentLMS die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing TalentLMS configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met TalentLMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TalentLMS** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. Op de **TalentLMS domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenant-name>.TalentLMSapp.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`http://<tenant-name>.talentlms.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [TalentLMS Client ondersteuningsteam](https://www.talentlms.com/contact) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. Op de **TalentLMS configuratie** sectie, klikt u op **configureren TalentLMS** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. In een ander browservenster, meld u aan bij uw bedrijf TalentLMS site als beheerder.

8. In de **& Accountinstellingen** sectie, klikt u op de **gebruikers** tabblad.
   
    ![& Accountinstellingen](./media/active-directory-saas-talentlms-tutorial/IC777296.png "& Accountinstellingen")

9. Klik op **eenmalige aanmelding (SSO)**,

10. Voer de volgende stappen uit in de sectie eenmalige aanmelding:
   
    ![Eenmalige aanmelding](./media/active-directory-saas-talentlms-tutorial/IC777297.png "eenmalige aanmelding")   

    a. Van de **SSO-Integratietype** selecteert **SAML 2.0**.

    b. In de **identiteitsprovider (IDP)** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.
 
    c. Plak de **vingerafdruk** waarde van de Azure-portal in de **vingerafdruk van certificaat** textbox.    

    d.  In de **extern aanmelden URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
 
    e. In de **externe URL voor afmelden** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    f. Vul in het volgende: 

    * In de **TargetedID** textbox type`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
     
    * In de **voornaam** textbox type`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    
    * In de **achternaam** textbox type`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    
    * In de **e** textbox type`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
    
11. Klik op **Opslaan**.
 
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-talentlms-test-user"></a>Een testgebruiker TalentLMS maken

Om Azure AD-gebruikers zich aanmelden bij TalentLMS, moeten ze worden ingericht in TalentLMS. In het geval van TalentLMS is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **TalentLMS** tenant.

2. Klik op **gebruikers**, en klik vervolgens op **gebruiker toevoegen**.

3. Op de **gebruiker toevoegen** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Gebruiker toevoegen](./media/active-directory-saas-talentlms-tutorial/IC777299.png "gebruiker toevoegen")  

    a. In de **voornaam** textbox, voer de voornaam van de gebruiker zoals **Britta**.

    b. In de **achternaam** textbox, geef de achternaam van de gebruiker zoals **Simon**.
 
    c. In de **e-mailadres** textbox, voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

    d. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt andere TalentLMS gebruiker account hulpmiddelen voor het maken of API's die is geleverd door TalentLMS aan inrichten AAD-gebruikersaccounts.
 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TalentLMS.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen TalentLMS, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TalentLMS**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel TalentLMS in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing TalentLMS

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

