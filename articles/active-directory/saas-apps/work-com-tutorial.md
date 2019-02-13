---
title: 'Zelfstudie: Azure Active Directory-integratie met Work.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Work.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e5d097afa3af111a7de8341b90d8c6c05ac0347
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205746"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Zelfstudie: Azure Active Directory-integratie met Work.com

In deze zelfstudie leert u hoe u Work.com integreren met Azure Active Directory (Azure AD).

Work.com integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Work.com heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Work.com (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Work.com, moet u de volgende items:

- Een Azure AD-abonnement
- Een Work.com eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Work.com uit de galerie toevoegen
1. Azure AD-eenmalige aanmelding configureren en testen

## <a name="add-workcom-from-the-gallery"></a>Work.com uit de galerie toevoegen
Voor het configureren van de integratie van Work.com in Azure AD, moet u Work.com uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Work.com uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Work.com**, selecteer **Work.com** Klik vanuit het deelvenster resultaten **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Work.com op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Work.com is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Work.com tot stand worden gebracht.

In Work.com, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Work.com, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Work.com](#create-a-workcom-test-user)**  : als u wilt een equivalent van Britta Simon in Work.com die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Work.com.

>[!NOTE]
>Voor het configureren van eenmalige aanmelding, moet u een aangepaste domeinnaam van Work.com nog instellen. U moet ten minste een domeinnaam te definiëren, de domeinnaam van uw testen en deze implementeren in uw hele organisatie.

**Voor het configureren van Azure AD eenmalige aanmelding met Work.com, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Work.com** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/work-com-tutorial/tutorial_work-com_samlbase.png)

1. Op de **Work.com domein en URL's** sectie, voer de volgende stappen uit:

    ![Sectie Work.com domein en URL 's](./media/work-com-tutorial/tutorial_work-com_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Work.com Client ondersteuningsteam](https://help.salesforce.com/articleView?id=000159855&type=3) deze waarde op te halen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Sectie voor SAML-handtekeningcertificaat](./media/work-com-tutorial/tutorial_work-com_certificate.png) 

1. Klik op de knop **Save**.

    ![De knop Opslaan](./media/work-com-tutorial/tutorial_general_400.png)

1. Op de **Work.com configuratie** sectie, klikt u op **configureren Work.com** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![De configuratiesectie Work.com](./media/work-com-tutorial/tutorial_work-com_configure.png) 
1. Meld u aan bij uw tenant Work.com als administrator.

1. Ga naar **Setup**.
   
    ![Setup](./media/work-com-tutorial/ic794108.png "Setup")

1. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **Mijn domein** pagina. 
   
    ![My Domain](./media/work-com-tutorial/ic767825.png "My Domain")

1. Om te bevestigen dat uw domein correct is ingesteld, zorg ervoor dat deze zich op "**stap 4 geïmplementeerd naar gebruikers**' en bekijk uw"**mijn domeininstellingen**'.
   
    ![Domein geïmplementeerd voor gebruiker](./media/work-com-tutorial/ic784377.png "domein geïmplementeerd voor gebruiker")

1. Meld u aan bij uw tenant Work.com.

1. Ga naar **Setup**.
    
    ![Setup](./media/work-com-tutorial/ic794108.png "Setup")

1. Vouw de **beveiligingsmechanismen** menu en klik vervolgens op **instellingen voor eenmalige aanmelding**.
    
    ![Single Sign-On instellingen](./media/work-com-tutorial/ic794113.png "Single Sign-On-instellingen")

1. Op de **instellingen voor eenmalige aanmelding** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML ingeschakeld](./media/work-com-tutorial/ic781026.png "SAML ingeschakeld")
    
    a. Selecteer **SAML ingeschakeld**.
    
    b. Klik op **Nieuw**.

1. In de **eenmalige SAML-aanmelding instellingen** sectie, voert u de volgende stappen uit:
    
    ![Eenmalige SAML-aanmelding instelling](./media/work-com-tutorial/ic794114.png "eenmalige SAML-aanmelding instellen")
    
    a. Typ in het tekstvak **Name** een naam voor de configuratie.  
       
    > [!NOTE]
    > Een waarde voor **naam** automatisch ingevuld de **API-naam** tekstvak.
    
    b. In **verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
    
    c. Als u wilt uploaden van het gedownloade certificaat vanuit Azure portal, klikt u op **Bladeren**.
    
    d. In de **entiteit-Id** tekstvak, type `https://salesforce-work.com`.
    
    e. Als **SAML identiteitstype**, selecteer **verklaring bevat de Federation-ID van het gebruikersobject**.
    
    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentfier van het onderwerp overzicht**.
    
    g. In **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    h. In **afmeldings-URL van id-Provider** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
    
    i. Als **Service Provider gestart aanvragen Binding**, selecteer **HTTP Post**.
    
    j. Klik op **Opslaan**.

1. Klik in de klassieke portal van Work.com in het linkernavigatiedeelvenster op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **mijn domein** de pagina. 
    
    ![My Domain](./media/work-com-tutorial/ic794115.png "My Domain")

1. Op de **mijn domein** pagina, in de **aanmelden pagina huisstijl** sectie, klikt u op **bewerken**.
    
    ![Login Page Branding](./media/work-com-tutorial/ic767826.png "Login Page Branding")

1. Op de **aanmelden pagina huisstijl** pagina, in de **verificatieservice** sectie, de naam van uw **SAML SSO-instellingen** wordt weergegeven. Selecteer deze en klik vervolgens op **opslaan**.
    
    ![Login Page Branding](./media/work-com-tutorial/ic784366.png "Login Page Branding")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/work-com-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers](./media/work-com-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Toevoegen](./media/work-com-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster op de gebruikerspagina](./media/work-com-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-workcom-test-user"></a>Maak een testgebruiker Work.com
Voor Azure Active Directory-gebruikers kunnen zich aanmelden, moeten ze worden ingericht op Work.com. In het geval van Work.com is inrichten een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voer de volgende stappen uit om de inrichting van gebruikers te configureren:
1. Meld u aan bij uw bedrijf Work.com site als beheerder.

1. Ga naar **Setup**.
   
    ![Setup](./media/work-com-tutorial/IC794108.png "Setup")
1. Ga naar **gebruikers beheren \> gebruikers**.
   
    ![Gebruikers beheren](./media/work-com-tutorial/IC784369.png "gebruikers beheren")

1. Klik op **New User**.
   
    ![Alle gebruikers](./media/work-com-tutorial/IC794117.png "alle gebruikers")

1. Uitvoeren in de sectie gebruikers bewerken in de volgende stappen in de kenmerken van een geldige Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt:
   
    ![Gebruiker bewerken](./media/work-com-tutorial/ic794118.png "gebruiker bewerken")
   
    a. In de **voornaam** tekstvak, type de **voornaam** van de gebruiker **Julia**.
    
    b. In de **achternaam** tekstvak, type de **achternaam** van de gebruiker **Simon**.
    
    c. In de **Alias** tekstvak, type de **naam** van de gebruiker **BrittaS**.
    
    d. In de **e** tekstvak, type de **e-mailadres** van gebruiker **Brittasimon@contoso.com**.
    
    e. In de **gebruikersnaam** tekstvak, typ een naam van gebruiker, zoals **Brittasimon@contoso.com**.
    
    f. In de **bijnaam** tekstvak, typ een **bijnaam** van gebruiker **Simon**.
    
    g. Selecteer **rol**, **gebruikerslicentie**, en **profiel**.
    
    h. Klik op **Opslaan**.  
      
    > [!NOTE]
    > De houder van Azure AD-account ontvangt een e-mailbericht ook een koppeling voor het account te bevestigen voordat deze geactiveerd wordt.
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Work.com.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Work.com toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Work.com**.

    ![Work.com in de applijst](./media/work-com-tutorial/tutorial_work-com_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Work.com in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Work.com.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/work-com-tutorial/tutorial_general_01.png
[2]: ./media/work-com-tutorial/tutorial_general_02.png
[3]: ./media/work-com-tutorial/tutorial_general_03.png
[4]: ./media/work-com-tutorial/tutorial_general_04.png

[100]: ./media/work-com-tutorial/tutorial_general_100.png

[200]: ./media/work-com-tutorial/tutorial_general_200.png
[201]: ./media/work-com-tutorial/tutorial_general_201.png
[202]: ./media/work-com-tutorial/tutorial_general_202.png
[203]: ./media/work-com-tutorial/tutorial_general_203.png

