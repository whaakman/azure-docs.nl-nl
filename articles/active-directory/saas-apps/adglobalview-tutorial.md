---
title: 'Zelfstudie: Azure Active Directory-integratie met ADP Globalview | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ADP Globalview.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 8ac0856dcadb92b79090a6520ef29707fd7a3bae
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813675"
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a>Zelfstudie: Azure Active Directory-integratie met ADP Globalview

In deze zelfstudie leert u hoe u ADP Globalview integreren met Azure Active Directory (Azure AD).

ADP Globalview integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ADP Globalview heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ADP Globalview (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP Globalview, moet u de volgende items:

- Een Azure AD-abonnement
- Een ADP Globalview eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP Globalview uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adp-globalview-from-the-gallery"></a>ADP Globalview uit de galerie toe te voegen
Voor het configureren van de integratie van ADP Globalview in Azure AD, moet u ADP Globalview uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP Globalview uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **ADP Globalview**.

    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. Selecteer in het deelvenster resultaten **ADP Globalview**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ADP Globalview op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ADP Globalview is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ADP Globalview tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ADP Globalview.

Om te configureren en testen van Azure AD eenmalige aanmelding met ADP Globalview, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker ADP Globalview](#creating-an-adp-globalview-test-user)**  : als u wilt een equivalent van Britta Simon in ADP Globalview die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ADP Globalview.

**Voor het configureren van Azure AD eenmalige aanmelding met ADP Globalview, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ADP Globalview** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. Op de **ADP Globalview domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_url.png)

     In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.globalview.adp.com/federate` of `https://<subdomain>.globalview.adp.com/federate2`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke id. Neem contact op met [ADP Globalview ondersteuning](https://www.adp.com/contact-us/overview.aspx) om de waarde.
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. De toepassing ADP GlobalView wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. 

6. De volgende Schermafbeelding toont een voorbeeld voor deze. De namen van de claim worden altijd **"PersonImmutableID"** en de waarde van die we hebben toegewezen aan ExtensionAttribute2, waarin de werknemer-id van de gebruiker. Hier de Gebruikerstoewijzing van Azure AD naar ADP GlobalView is uitgevoerd op de id van de werknemer, maar kunt u deze toewijzen aan een andere waarde ook op basis van de toepassingsinstellingen van uw. U kunt werken met het team ADP GlobalView eerst het gebruik van de juiste id van een gebruiker en wijs die waarde met de **"PersonImmutableID"** claim. U kunt ook de e-mail en gebruikers-id claim toewijzen zoals weergegeven in de afbeelding.

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | personalimmutableid | User.extensionattribute2 |
    | e-mail               | user.mail |
    | gebruikers-id              | user.userprincipalname|
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-verklaring configureren kunt, moet u contact op met uw [ADP Globalview ondersteuning](https://www.adp.com/contact-us/overview.aspx) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U hebt deze waarde nodig voor het configureren van de aangepaste claim voor uw toepassing. 

8. Op de **ADP Globalview configuratie** sectie, klikt u op **configureren ADP Globalview** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_general_400.png)

10. Het configureren van eenmalige aanmelding op **ADP Globalview** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)**, **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [ADP Globalview ondersteuning](https://www.adp.com/contact-us/overview.aspx).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/create_aaduser_01.png) 

2.  Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/adglobalview-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-adp-globalview-test-user"></a>Het maken van een testgebruiker ADP Globalview

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ADP GlobalView. Werken met [ADP Globalview ondersteuning](https://www.adp.com/contact-us/overview.aspx) om toe te voegen de gebruikers in de ADP GlobalView-account. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ADP Globalview.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan ADP Globalview toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **ADP Globalview**.

    ![Eenmalige aanmelding configureren](./media/adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.  

Wanneer u op de tegel ADP GlobalView in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ADP GlobalView.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/adglobalview-tutorial/tutorial_general_203.png

