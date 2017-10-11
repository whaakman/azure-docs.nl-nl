---
title: 'Zelfstudie: Azure Active Directory-integratie met PolicyStat | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PolicyStat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 704afd5515b02ce2a4fbf35da65fad74dc506271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Zelfstudie: Azure Active Directory-integratie met PolicyStat

In deze zelfstudie leert u hoe PolicyStat integreren met Azure Active Directory (Azure AD).

PolicyStat integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PolicyStat heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij PolicyStat (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PolicyStat, moet u de volgende items:

- Een Azure AD-abonnement
- Een PolicyStat eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PolicyStat uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-policystat-from-the-gallery"></a>PolicyStat uit de galerie toevoegen
Voor het configureren van de integratie van PolicyStat in Azure AD, moet u PolicyStat uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PolicyStat uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **PolicyStat**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. Selecteer in het deelvenster resultaten **PolicyStat**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met PolicyStat op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in PolicyStat is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in PolicyStat tot stand worden gebracht.

Wijs in PolicyStat, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met PolicyStat, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker PolicyStat](#creating-a-policystat-test-user)**  - PolicyStat die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing PolicyStat configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met PolicyStat, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PolicyStat** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. Op de **PolicyStat domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.policystat.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [PolicyStat Client ondersteuningsteam](http://www.policystat.com/support/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren PolicyStat aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.

    De toepassing PolicyStat verwacht de SAML-asserties in een specifieke indeling waarvoor u het toevoegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie.  

     De volgende Schermafbeelding toont een voorbeeld hiervan.

     ![Kenmerken](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "kenmerken")

6. Als u wilt de vereiste kenmerktoewijzingen toevoegen, moet u de volgende stappen uitvoeren:

    | Naam van kenmerk    |   De waarde van kenmerk |
    |------------------- | -------------------- |
    | UID | ExtractMailPrefix([mail]) |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. In de **kenmerknaam** textbox type **uid**.

    c. In de **kenmerkwaarde** textbox, selecteer **ExtractMailPrefix()**.    
   
    d. Van de **Mail** selecteert **User.mail**.
    
    e. Klik op **Ok**

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. In een ander browservenster, meld u bij uw bedrijf PolicyStat site als beheerder.

9. Klik op de **Admin** tabblad en klik vervolgens op **configuratie voor één aanmelding** in het navigatiedeelvenster links.
   
    ![Menu beheerder](./media/active-directory-saas-policystat-tutorial/ic808633.png "Menu beheerder")

10. In de **Setup** sectie **inschakelen eenmalige aanmelding integratie**.
   
    ![Eenmalige aanmelding configuratie](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On-configuratie")

11. Klik op **kenmerken configureren**, en klikt u op de **kenmerken configureren** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configuratie](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On-configuratie")
   
    a. In de **kenmerk Username** textbox type **uid**.

    b. In de **voornaam kenmerk** textbox type **firstname** van gebruiker **Britta**.

    c. In de **laatste naamkenmerk** textbox type **lastname** van gebruiker **Simon**.

    d. In de **e kenmerk** textbox type **emailaddress** van gebruiker  **BrittaSimon@contoso.com** .

    e. Klik op **wijzigingen opslaan**.

12. Klik op **uw IDP metagegevens**, en klikt u op de **uw IDP metagegevens** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configuratie](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On-configuratie")
   
    a. Uw gedownloade metagegevensbestand openen, de inhoud kopieert en plakt u deze in de **uw identiteit Provider metagegevens** textbox.

    b. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-policystat-test-user"></a>Een testgebruiker PolicyStat maken

Om in te schakelen gebruikers van Azure AD aan te melden bij PolicyStat, moeten ze worden ingericht in PolicyStat.  

PolicyStat ondersteunt alleen in de tijd van gebruikersinrichting. Dit houdt in dat u niet hoeft de gebruikers handmatig toevoegen aan PolicyStat. De gebruikers wordt automatisch ophalen toegevoegd aan de eerste aanmelding via eenmalige aanmelding.

>[!NOTE]
>U kunt andere PolicyStat gebruiker account hulpmiddelen voor het maken of API's die is geleverd door PolicyStat voor het inrichten van Azure AD-gebruikersaccounts.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan PolicyStat.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen PolicyStat, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **PolicyStat**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel PolicyStat in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing PolicyStat.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

