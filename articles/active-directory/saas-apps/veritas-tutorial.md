---
title: 'Zelfstudie: Azure Active Directory-integratie met eenmalige aanmelding Veritas Enterprise Vault.cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en eenmalige aanmelding Veritas Enterprise Vault.cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70adbcd8c25b3acb4408447070d3b0397d258847
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167453"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Zelfstudie: Azure Active Directory-integratie met Veritas Enterprise Vault.cloud eenmalige aanmelding

In deze zelfstudie leert u hoe u eenmalige aanmelding Veritas Enterprise Vault.cloud integreren met Azure Active Directory (Azure AD).

Eenmalige aanmelding Veritas Enterprise Vault.cloud integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Veritas Enterprise Vault.cloud SSO heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Veritas Enterprise Vault.cloud SSO (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Veritas Enterprise Vault.cloud eenmalige aanmelding, moet u de volgende items:

- Een Azure AD-abonnement
- Een Veritas Enterprise Vault.cloud SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Veritas Enterprise Vault.cloud SSO uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Veritas Enterprise Vault.cloud SSO uit de galerie toe te voegen
Voor het configureren van de integratie van Veritas Enterprise Vault.cloud SSO in Azure AD, moet u Veritas Enterprise Vault.cloud SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Veritas Enterprise Vault.cloud SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Veritas Enterprise Vault.cloud SSO**.

    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/tutorial_veritas_search.png)

1. Selecteer in het deelvenster resultaten **Veritas Enterprise Vault.cloud SSO**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Veritas Enterprise Vault.cloud eenmalige aanmelding op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Veritas Enterprise Vault.cloud SSO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Veritas Enterprise Vault.cloud eenmalige aanmelding tot stand worden gebracht.

In Veritas Enterprise Vault.cloud SSO, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Veritas Enterprise Vault.cloud eenmalige aanmelding testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Veritas Enterprise Vault.cloud SSO](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  : als u wilt een equivalent van Britta Simon in Veritas Enterprise Vault.cloud eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Veritas Enterprise Vault.cloud eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met Veritas Enterprise Vault.cloud eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Veritas Enterprise Vault.cloud SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_veritas_samlbase.png)

1. Op de **Veritas Enterprise Vault.cloud SSO-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_veritas_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`

    b. In de **id** tekstvak, gebruikt u de URL aan de hand van het Datacenter

    | Datacenter| URL |
    |----------|----|
    | Noord-Amerika| `https://auth.lax.archivecloud.net` |
    | Europa | `https://auth.ams.archivecloud.net` |
    | Azië en Stille Oceaan| `https://auth.syd.archivecloud.net`|

    c. In de **antwoord-URL** tekstvak, gebruikt u de URL aan de hand van het Datacenter

    | Datacenter| URL |
    |----------|----|
    | Noord-Amerika| `https://auth.lax.archivecloud.net` |
    | Europa | `https://auth.ams.archivecloud.net` |
    | Azië en Stille Oceaan| `https://auth.syd.archivecloud.net`|
    
    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Veritas Enterprise Vault.cloud SSO-Client-ondersteuningsteam](https://www.veritas.com/support/.html) deze waarde op te halen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_veritas_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_general_400.png)

1. Op de **Veritas Enterprise Vault.cloud SSO-configuratie** sectie, klikt u op **configureren Veritas Enterprise Vault.cloud SSO** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_veritas_configure.png) 

1. Het configureren van eenmalige aanmelding op **Veritas Enterprise Vault.cloud SSO** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)** en **Single Sign-On Service URL voor SAML** naar [Veritas Enterprise Vault.cloud SSO-ondersteuningsteam](https://www.veritas.com/support/.html).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/veritas-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a>Het maken van een testgebruiker Veritas Enterprise Vault.cloud SSO

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Enterprise Vault.cloud eenmalige aanmelding. Werken met [Veritas Enterprise Vault.cloud SSO-ondersteuningsteam](https://www.veritas.com/support/.html) om toe te voegen de gebruikers in de onderneming Vault.cloud SSO-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Veritas Enterprise Vault.cloud eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Veritas Enterprise Vault.cloud SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Veritas Enterprise Vault.cloud SSO**.

    ![Eenmalige aanmelding configureren](./media/veritas-tutorial/tutorial_veritas_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Veritas Enterprise Vault.cloud eenmalige aanmelding in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Veritas Enterprise Vault.cloud eenmalige aanmelding.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/veritas-tutorial/tutorial_general_01.png
[2]: ./media/veritas-tutorial/tutorial_general_02.png
[3]: ./media/veritas-tutorial/tutorial_general_03.png
[4]: ./media/veritas-tutorial/tutorial_general_04.png

[100]: ./media/veritas-tutorial/tutorial_general_100.png

[200]: ./media/veritas-tutorial/tutorial_general_200.png
[201]: ./media/veritas-tutorial/tutorial_general_201.png
[202]: ./media/veritas-tutorial/tutorial_general_202.png
[203]: ./media/veritas-tutorial/tutorial_general_203.png

