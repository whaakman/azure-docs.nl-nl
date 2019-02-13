---
title: 'Zelfstudie: Azure Active Directory-integratie met Marketo | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en Marketo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: abab9f6e38fcf69dcb04bfea0f84d883dc5267b7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199660"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Zelfstudie: Azure Active Directory-integratie met Marketo

In deze zelfstudie leert u hoe u Marketo integreren met Azure Active Directory (Azure AD).

Integratie van Marketo in Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Marketo heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Marketo (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Marketo, moet u de volgende items:

- Een Azure AD-abonnement
- Een Marketo eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Marketo uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-marketo-from-the-gallery"></a>Marketo uit de galerie toe te voegen
Voor het configureren van de integratie van Marketo in Azure AD, moet u Marketo uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Marketo uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Marketo**.

    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/tutorial_marketo_search.png)

1. Selecteer in het deelvenster resultaten **Marketo**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Marketo op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Marketo is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Marketo tot stand worden gebracht.

In Marketo, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Marketo, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Marketo](#creating-a-marketo-test-user)**  : als u wilt een equivalent van Britta Simon in Marketo die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Marketo-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Marketo, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Marketo** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_samlbase.png)

1. Op de **Marketo-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://saml.marketo.com/sp`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [Marketo-ondersteuningsteam](http://investors.marketo.com/contactus.cfm) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_general_400.png)

1. Op de **Marketo configuratie** sectie, klikt u op **configureren Marketo** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_configure.png) 

1. Meld u aan bij Marketo met behulp van beheerdersreferenties als Munchkin-Id van uw toepassing, en de volgende acties uitvoeren:
   
    a. Meld u aan bij Marketo-app met behulp van beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar het integratie-menu en klik op de **Munchkin koppeling**.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Kopieer de Munchkin-Id weergegeven op het scherm en uw antwoord-URL in de wizard Azure AD-configuratie te voltooien.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_12.png) 

1. Voor het configureren van de eenmalige aanmelding in de toepassing, volg de onderstaande stappen te volgen:
   
    a. Meld u aan bij Marketo-app met behulp van beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar het integratie-menu en klik op **Single Sign On**.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. Als de SAML-instellingen, klikt u op **bewerken** knop.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Ingeschakeld** Single Sign-On-instellingen.
   
    f. Plak de **SAML entiteit-ID**, in de **uitgever-ID** tekstvak.
   
    g. In de **entiteit-ID** tekstvak, voert u de URL als `http://saml.marketo.com/sp`.
   
    h. Selecteer de locatie van de gebruiker-ID als **naam-ID-element**.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Als uw gebruikers-id geen UPN-waarde en wijzig de waarde in het tabblad kenmerk is.
   
    i. Upload het certificaat dat u hebt gedownload van Azure AD-configuratiewizard. **Sla** de instellingen.
   
    j. Bewerk de instellingen omleidingspagina's.
   
    k. Plak de **Single Sign-On Service URL voor SAML** in de **aanmeldings-URL** tekstvak.
   
    l. Plak de **afmelding URL** in de **afmeldings-URL van** tekstvak.
   
    m. In de **fout URL**, kopie uw **Marketo exemplaar URL** en klikt u op **opslaan** knop instellingen op te slaan.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_10.png)

1. Als u wilt inschakelen voor gebruikers met SSO, moet u de volgende acties uitvoeren:
   
    a. Meld u aan bij Marketo-app met behulp van beheerdersreferenties.
   
    b. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigeer naar de **Security** en klik op **Login Settings**.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. Controleer de **vereisen SSO** optie en **opslaan** de instellingen.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/marketo-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-marketo-test-user"></a>Het maken van een testgebruiker Marketo

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Marketo. Volg deze stappen voor het maken van een gebruiker in de Marketo-platform.

1. Meld u aan bij Marketo-app met behulp van beheerdersreferenties.

1. Klik op de **Admin** knop op het bovenste navigatiedeelvenster.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_06.png) 

1. Navigeer naar de **Security** en klik op **gebruikers en rollen**
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_19.png)  

1. Klik op de **nieuwe gebruiker uitnodigen** koppeling op het tabblad gebruikers
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_15.png) 

1. Vul de volgende informatie in de wizard nieuwe gebruiker uitnodigen
   
    a. Voer de gebruiker **e** adres in het tekstvak.
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Voer de **voornaam** in het tekstvak.
   
    c. Voer de **achternaam** in het tekstvak.
   
    d. Klik op **Volgende**

1. In de **machtigingen** tabblad de **userRoles** en klikt u op **volgende**
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_17.png)
1. Klik op de **verzenden** knop om de uitnodiging van de gebruiker te verzenden
   
    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_18.png)

1. Gebruiker de e-mailmelding ontvangt en klik op de koppeling en het wachtwoord voor het activeren van het account wijzigen. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Marketo.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon met Marketo, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Marketo**.

    ![Eenmalige aanmelding configureren](./media/marketo-tutorial/tutorial_marketo_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de Marketo-tegel in het toegangsvenster klikt, u moet u automatisch aangemeld bij uw Marketo-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/marketo-tutorial/tutorial_general_01.png
[2]: ./media/marketo-tutorial/tutorial_general_02.png
[3]: ./media/marketo-tutorial/tutorial_general_03.png
[4]: ./media/marketo-tutorial/tutorial_general_04.png

[100]: ./media/marketo-tutorial/tutorial_general_100.png

[200]: ./media/marketo-tutorial/tutorial_general_200.png
[201]: ./media/marketo-tutorial/tutorial_general_201.png
[202]: ./media/marketo-tutorial/tutorial_general_202.png
[203]: ./media/marketo-tutorial/tutorial_general_203.png

