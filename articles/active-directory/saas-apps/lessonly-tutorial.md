---
title: 'Zelfstudie: Azure Active Directory-integratie met Lessonly.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lessonly.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c911a53f16e07b67a1ec22a9e0592df06c192325
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188181"
---
# <a name="tutorial-azure-active-directory-integration-with-lessonlycom"></a>Zelfstudie: Azure Active Directory-integratie met Lessonly.com

In deze zelfstudie leert u hoe u Lessonly.com integreren met Azure Active Directory (Azure AD).

Lessonly.com integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Lessonly.com heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Lessonly.com (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Lessonly.com, moet u de volgende items:

- Een Azure AD-abonnement
- Een Lessonly.com eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Lessonly.com uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lessonlycom-from-the-gallery"></a>Lessonly.com uit de galerie toe te voegen
Voor het configureren van de integratie van Lessonly.com in Azure AD, moet u Lessonly.com uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Lessonly.com uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Lessonly.com**.

    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/tutorial_lessonly.com_search.png)

1. Selecteer in het deelvenster resultaten **Lessonly.com**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/tutorial_lessonly.com_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Lessonly.com op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Lessonly.com is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Lessonly.com tot stand worden gebracht.

In Lessonly.com, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Lessonly.com, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Lessonly.com](#creating-a-lessonly-test-user)**  : als u wilt een equivalent van Britta Simon in Lessonly.com die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Lessonly.com.

**Voor het configureren van Azure AD eenmalige aanmelding met Lessonly.com, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Lessonly.com** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly.com_samlbase.png)

1. Op de **Lessonly.com domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly.com_url.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    >Wanneer die verwijst naar een algemene naam die **companyname** moet worden vervangen door een daadwerkelijke naam.
    
    b. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Lessonly.com Client ondersteuningsteam](mailto:support@lessonly.com) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly.com_certificate.png)

1. De toepassing Lessonly.com wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, die vereist dat u om toe te voegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly_06.png)
           
1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk   | Waarde kenmerk |
    | ---------------  | ----------------|
    | urn: oid:2.5.4.42 |user.givenname |
    | urn: oid:2.5.4.4  |user.surname |
    | urn:oid:0.9.2342.19200300.100.1.3 |user.mail |
    | urn:oid:1.3.6.1.4.1.5923.1.1.1.10 |User.objectid |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.     

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_general_400.png)

1. Op de **Lessonly.com configuratie** sectie, klikt u op **configureren Lessonly.com** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly.com_configure.png)

1. Het configureren van eenmalige aanmelding op **Lessonly.com** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)** en **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Lessonly.com ondersteuningsteam](mailto:support@lessonly.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/lessonly-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lessonlycom-test-user"></a>Het maken van een testgebruiker Lessonly.com

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Lessonly.com. Lessonly.com biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot Lessonly.com als deze nog niet bestaat.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [Lessonly.com ondersteuningsteam](mailto:support@lessonly.com).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Lessonly.com.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Lessonly.com toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Lessonly.com**.

    ![Eenmalige aanmelding configureren](./media/lessonly-tutorial/tutorial_lessonly.com_app.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel Lessonly.com in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Lessonly.com.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/lessonly-tutorial/tutorial_general_01.png
[2]: ./media/lessonly-tutorial/tutorial_general_02.png
[3]: ./media/lessonly-tutorial/tutorial_general_03.png
[4]: ./media/lessonly-tutorial/tutorial_general_04.png

[100]: ./media/lessonly-tutorial/tutorial_general_100.png

[200]: ./media/lessonly-tutorial/tutorial_general_200.png
[201]: ./media/lessonly-tutorial/tutorial_general_201.png
[202]: ./media/lessonly-tutorial/tutorial_general_202.png
[203]: ./media/lessonly-tutorial/tutorial_general_203.png

