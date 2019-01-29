---
title: 'Zelfstudie: Azure Active Directory-integratie met Gigya | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Gigya.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: a4141dc5a4d7be663e382e5aadb80d42a653f807
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179256"
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Zelfstudie: Azure Active Directory-integratie met Gigya

In deze zelfstudie leert u hoe u Gigya integreren met Azure Active Directory (Azure AD).

Gigya integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Gigya heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Gigya (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Gigya, moet u de volgende items:

- Een Azure AD-abonnement
- Een Gigya eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Gigya uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-gigya-from-the-gallery"></a>Gigya uit de galerie toe te voegen
Voor het configureren van de integratie van Gigya in Azure AD, moet u Gigya uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Gigya uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Gigya**.

    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/tutorial_gigya_search.png)

1. Selecteer in het deelvenster resultaten **Gigya**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Gigya op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Gigya is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Gigya tot stand worden gebracht.

In Gigya, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Gigya, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Gigya](#creating-a-gigya-test-user)**  : als u wilt een equivalent van Britta Simon in Gigya die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Gigya.

**Voor het configureren van Azure AD eenmalige aanmelding met Gigya, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Gigya** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_gigya_samlbase.png)

1. Op de **Gigya domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_gigya_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `http://<companyname>.gigya.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://fidm.gigya.com/saml/v2.0/<companyname>`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Gigya Client ondersteuningsteam](https://www.gigya.com/support-policy/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_gigya_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_general_400.png)

1. Op de **Gigya configuratie** sectie, klikt u op **configureren Gigya** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_gigya_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Gigya site als beheerder.

1. Ga naar **instellingen \> SAML-aanmelding**, en klik vervolgens op de **toevoegen** knop.
   
    ![SAML-aanmelding](./media/gigya-tutorial/ic789532.png "SAML-aanmelding")

1. In de **SAML-aanmelding** sectie, voert u de volgende stappen uit:
   
    ![SAML-configuratie](./media/gigya-tutorial/ic789533.png "SAML-configuratie")
   
    a. In de **naam** tekstvak, typ een naam voor uw configuratie.
   
    b. In **verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure Portal. 
   
    c. In **Single Sign-On Service URL** tekstvak, plak de waarde van **Single Sign-On Service URL** die u hebt gekopieerd vanuit Azure Portal.
   
    d. In **indeling van de ID** tekstvak, plak de waarde van **indeling van de id** die u hebt gekopieerd vanuit Azure Portal.
   
    e. Open in Kladblok het met Base 64 gecodeerde certificaat dat u hebt gedownload uit de Azure-portal, kopieer de inhoud ervan naar het Klembord en plak deze vervolgens in het tekstvak **X.509 Certificate**.
   
    f. Klik op **instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/gigya-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-gigya-test-user"></a>Het maken van een testgebruiker Gigya

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Gigya, moeten ze worden ingericht voor Gigya.  
In het geval van Gigya is inrichten een handmatige taak.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Ga als volgt te werk om een gebruikersaccount in te richten:

1. Meld u aan bij uw **Gigya** bedrijf site als beheerder.

1. Ga naar **Admin \> gebruikers beheren**, en klik vervolgens op **gebruikers uitnodigen**.
   
    ![Gebruikers beheren](./media/gigya-tutorial/ic789535.png "gebruikers beheren")

1. In het dialoogvenster gebruikers uitnodigen, moet u de volgende stappen uitvoeren:
   
    ![Invite Users](./media/gigya-tutorial/ic789536.png "Invite Users")
   
    a. In de **e** tekstvak typt u de e-mailalias van een geldige Azure Active Directory-account dat u inrichten wilt.
    
    b. Klik op **gebruiker uitnodigen**.
      
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze geactiveerd wordt.
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Gigya.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Gigya toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Gigya**.

    ![Eenmalige aanmelding configureren](./media/gigya-tutorial/tutorial_gigya_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel Gigya in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Gigya.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/gigya-tutorial/tutorial_general_01.png
[2]: ./media/gigya-tutorial/tutorial_general_02.png
[3]: ./media/gigya-tutorial/tutorial_general_03.png
[4]: ./media/gigya-tutorial/tutorial_general_04.png

[100]: ./media/gigya-tutorial/tutorial_general_100.png

[200]: ./media/gigya-tutorial/tutorial_general_200.png
[201]: ./media/gigya-tutorial/tutorial_general_201.png
[202]: ./media/gigya-tutorial/tutorial_general_202.png
[203]: ./media/gigya-tutorial/tutorial_general_203.png

