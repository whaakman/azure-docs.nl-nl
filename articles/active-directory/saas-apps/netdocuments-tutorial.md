---
title: 'Zelfstudie: Azure Active Directory-integratie met NetDocuments | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en NetDocuments.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 279a5700b7d9934afa1f772a282f276580a168ec
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817899"
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Zelfstudie: Azure Active Directory-integratie met NetDocuments

In deze zelfstudie leert u hoe u NetDocuments integreren met Azure Active Directory (Azure AD).

NetDocuments integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot NetDocuments heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij NetDocuments (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met NetDocuments, moet u de volgende items:

- Een Azure AD-abonnement
- Een NetDocuments eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. NetDocuments uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-netdocuments-from-the-gallery"></a>NetDocuments uit de galerie toe te voegen
Voor het configureren van de integratie van NetDocuments in Azure AD, moet u NetDocuments uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen NetDocuments uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **NetDocuments**.

    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/tutorial_netdocuments_search.png)

1. Selecteer in het deelvenster resultaten **NetDocuments**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met NetDocuments op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in NetDocuments is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in NetDocuments tot stand worden gebracht.

In NetDocuments, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met NetDocuments, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker NetDocuments](#creating-a-netdocuments-test-user)**  : als u wilt een equivalent van Britta Simon in NetDocuments die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing NetDocuments.

**Voor het configureren van Azure AD eenmalige aanmelding met NetDocuments, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **NetDocuments** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

1. Op de **NetDocuments domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/netdocuments-tutorial/tutorial_netdocuments_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en de antwoord-URL. Neem contact op met [NetDocuments ondersteuningsteam](https://support.netdocuments.com/hc/) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/netdocuments-tutorial/tutorial_general_400.png)

1. Meld u in een ander browservenster in uw bedrijf NetDocuments site als beheerder.

1. Ga naar **Admin**.

1. Klik op **toevoegen en verwijderen van gebruikers en groepen**.
   
    ![Opslagplaats](./media/netdocuments-tutorial/ic795047.png "opslagplaats")

1. Klik op **geavanceerde verificatieopties configureren**.
    
    ![Configureer geavanceerde verificatieopties](./media/netdocuments-tutorial/ic795048.png "geavanceerde verificatieopties configureren")

1. Op de **federatieve identiteit** dialoogvenster, voer de volgende stappen uit:
   
    ![Federatieve Identitty](./media/netdocuments-tutorial/ic795049.png "federatieve Identitty")
   
    a. Als **federatieve identiteit servertype**, selecteer **Active Directory Federation Services**.
   
    b. Klik op **bestand kiezen**, het uploaden van het gedownloade metagegevensbestand dat u hebt gedownload vanuit Azure portal.
   
    c. Klik op **OK**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/netdocuments-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-netdocuments-test-user"></a>Het maken van een testgebruiker NetDocuments

Als u wilt dat Azure AD-gebruikers zich aanmelden bij NetDocuments, moeten ze worden ingericht voor NetDocuments.  
In het geval van NetDocuments is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Sing aan bij uw **NetDocuments** bedrijf site als administrator.

1. Klik in het menu bovenaan op **Admin**.
   
    ![Admin](./media/netdocuments-tutorial/ic795051.png "Admin")

1. Klik op **toevoegen en verwijderen van gebruikers en groepen**.
   
    ![Opslagplaats](./media/netdocuments-tutorial/ic795047.png "opslagplaats")

1. In de **e-mailadres** tekstvak typt u het e-mailadres van een geldige Azure Active Directory-account dat u wilt inrichten, en klik vervolgens op **gebruiker toevoegen**.
   
    ![E-mailadres](./media/netdocuments-tutorial/ic795053.png "e-mailadres")
   
   >[!NOTE]
   >De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze geactiveerd wordt. U kunt elke andere NetDocuments gebruiker account hulpmiddelen voor het maken of API's geleverd door NetDocuments voor het inrichten van Azure Active Directory-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan NetDocuments.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan NetDocuments toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **NetDocuments**.

    ![Eenmalige aanmelding configureren](./media/netdocuments-tutorial/tutorial_netdocuments_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel NetDocuments in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing NetDocuments.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/netdocuments-tutorial/tutorial_general_203.png

