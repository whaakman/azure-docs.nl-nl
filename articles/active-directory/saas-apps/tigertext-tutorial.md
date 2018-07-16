---
title: 'Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TigerText Secure Messenger.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 09f04ab4d289eb7fb6664d192404f719f8604e1e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041344"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger

In deze zelfstudie leert u hoe u TigerText beveiligen Messenger integreren met Azure Active Directory (Azure AD).

TigerText Secure Messenger integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TigerText Secure Messenger heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TigerText Secure Messenger (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TigerText Secure Messenger, moet u de volgende items:

- Een Azure AD-abonnement
- Een TigerText Secure Messenger eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TigerText Secure Messenger uit de galerie toevoegen
2. Configureren en Azure AD eenmalige aanmelding testen

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a>TigerText Secure Messenger uit de galerie toevoegen
Voor het configureren van de integratie van TigerText Secure Messenger in Azure AD, moet u TigerText beveiligen Messenger uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TigerText Secure Messenger uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TigerText Secure Messenger**, selecteer **TigerText Secure Messenger** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In deze sectie maakt u configureren en testen Azure AD eenmalige aanmelding met TigerText Secure Messenger op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TigerText Secure Messenger is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker TigerText Secure Messenger tot stand worden gebracht.

In TigerText Secure Messenger, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TigerText Secure Messenger, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker TigerText Secure Messenger](#create-a-tigertext-secure-messenger-test-user)**  : als u wilt een equivalent van Britta Simon in TigerText Secure Messenger die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TigerText Secure Messenger.

**Voor het configureren van Azure AD eenmalige aanmelding met TigerText Secure Messenger, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TigerText Secure Messenger** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Op SAML gebaseerde aanmelding](./media/tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. Op de **TigerText Secure Messenger domein en URL's** sectie, voert u de volgende stappen uit:

    ![Sectie TigerText Secure Messenger domein en URL 's](./media/tigertext-tutorial/tutorial_tigertext_url.png)

    a. In de **aanmeldings-URL** tekstvak, typ de URL als: `https://home.tigertext.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke-id. Neem contact op met [TigerText Secure Messenger-Client-ondersteuningsteam](mailTo:prosupport@tigertext.com) deze waarde op te halen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Sectie voor SAML-handtekeningcertificaat](./media/tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop Opslaan](./media/tigertext-tutorial/tutorial_general_400.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met [TigerText Secure Messenger-ondersteuningsteam](mailTo:prosupport@tigertext.com) en geeft u de **gedownloade metagegevens**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/tigertext-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers](./media/tigertext-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Knop toevoegen](./media/tigertext-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster voor de gebruiker](./media/tigertext-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>Maak een testgebruiker TigerText Secure Messenger

In deze sectie maakt u een gebruiker met de naam van Britta Simon in TigerText. Neem contact op [TigerText Secure Messenger-Client-ondersteuningsteam](mailTo:prosupport@tigertext.com) om toe te voegen de gebruikers in het TigerText-platform.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot TigerText Secure Messenger.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij TigerText Secure Messenger, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **TigerText Secure Messenger**.

    ![TigerText Secure Messenger in de lijst met Apps](./media/tigertext-tutorial/tutorial_tigertext_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TigerText in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TigerText. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tigertext-tutorial/tutorial_general_01.png
[2]: ./media/tigertext-tutorial/tutorial_general_02.png
[3]: ./media/tigertext-tutorial/tutorial_general_03.png
[4]: ./media/tigertext-tutorial/tutorial_general_04.png

[100]: ./media/tigertext-tutorial/tutorial_general_100.png

[200]: ./media/tigertext-tutorial/tutorial_general_200.png
[201]: ./media/tigertext-tutorial/tutorial_general_201.png
[202]: ./media/tigertext-tutorial/tutorial_general_202.png
[203]: ./media/tigertext-tutorial/tutorial_general_203.png

