---
title: 'Zelfstudie: Azure Active Directory-integratie met AnswerHub | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3bced675ad074de7050aee08d2a63b4831ba3606
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810343"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Zelfstudie: Azure Active Directory-integratie met AnswerHub

In deze zelfstudie leert u hoe u AnswerHub integreren met Azure Active Directory (Azure AD).

AnswerHub integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AnswerHub heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AnswerHub (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AnswerHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een AnswerHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AnswerHub uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-answerhub-from-the-gallery"></a>AnswerHub uit de galerie toe te voegen
Voor het configureren van de integratie van AnswerHub in Azure AD, moet u AnswerHub uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AnswerHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **AnswerHub**.

    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/tutorial_answerhub_search.png)

5. Selecteer in het deelvenster resultaten **AnswerHub**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met AnswerHub op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in AnswerHub is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AnswerHub tot stand worden gebracht.

In AnswerHub, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met AnswerHub, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker AnswerHub](#creating-an-answerhub-test-user)**  : als u wilt een equivalent van Britta Simon in AnswerHub die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing AnswerHub.

**Voor het configureren van Azure AD eenmalige aanmelding met AnswerHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AnswerHub** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. Op de **AnswerHub domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_answerhub_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<company>.answerhub.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [AnswerHub Client ondersteuningsteam](mailto:success@answerhub.com) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_general_400.png)

6. Op de **AnswerHub configuratie** sectie, klikt u op **configureren AnswerHub** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_answerhub_configure.png) 

7. Meld u in een ander browservenster in uw bedrijf AnswerHub site als beheerder.
   
    >[!NOTE]
    >Als u nodig bij het configureren van AnswerHub hebt, neem dan contact op met [AnswerHub van ondersteuningsteam](mailto:success@answerhub.com.).
   
8. Ga naar **beheer**.

9. Klik op de **gebruikers en groepen** tabblad.

10. In het navigatiedeelvenster aan de linkerkant in de **sociale instellingen** sectie, klikt u op **SAML Setup**.

11. Klik op **IDP Config** tabblad.

12. Op de **IDP Config** tabblad, voert u de volgende stappen uit:

     ![SAML-Setup](./media/answerhub-tutorial/ic785172.png "SAML-instellingen")  
  
     a. In **IDP aanmeldings-URL** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
  
     b. In **afmeldings-URL van id-provider** tekstvak plakken **afmelding URL** waarde die u hebt gekopieerd vanuit Azure portal.
     
     c. In **indeling van de IDP-id** tekstvak, voert u de id-waarde hetzelfde als de geselecteerde in Azure portal in gebruiker **gebruikerskenmerken** sectie.
  
     d. Klik op **sleutels en certificaten**.

13. Voer de volgende stappen uit op het tabblad sleutels en certificaten:
    
     ![Sleutels en certificaten](./media/answerhub-tutorial/ic785173.png "sleutels en certificaten")  
 
     a. Open uw base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **openbare sleutel van de id-provider (x 509-indeling)** tekstvak.
  
     b. Klik op **Opslaan**.

14. Op de **IDP Config** tabblad **opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/answerhub-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-answerhub-test-user"></a>Het maken van een testgebruiker AnswerHub

Als u wilt dat Azure AD-gebruikers zich aanmelden bij AnswerHub, moeten ze worden ingericht voor AnswerHub.  
In het geval van AnswerHub is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **AnswerHub** bedrijf site als administrator.

2. Ga naar **beheer**.

3. Klik op de **gebruikers en groepen** tabblad.

4. In het navigatiedeelvenster aan de linkerkant in de **gebruikers beheren** sectie, klikt u op **maken of importeren van gebruikers**.
   
   ![Gebruikers en groepen](./media/answerhub-tutorial/ic785175.png "gebruikers en groepen")

5. Type de **e-mailadres**, **gebruikersnaam** en **wachtwoord** van een geldige Azure Active Directory-account die u wilt inrichten in de bijbehorende tekstvakken en klik vervolgens op  **Sla**.

>[!NOTE]
>U kunt alle andere AnswerHub gebruiker-account maken van hulpprogramma's of API's geleverd door AnswerHub aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan AnswerHub.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan AnswerHub toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **AnswerHub**.

    ![Eenmalige aanmelding configureren](./media/answerhub-tutorial/tutorial_answerhub_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel AnswerHub in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing AnswerHub.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/answerhub-tutorial/tutorial_general_01.png
[2]: ./media/answerhub-tutorial/tutorial_general_02.png
[3]: ./media/answerhub-tutorial/tutorial_general_03.png
[4]: ./media/answerhub-tutorial/tutorial_general_04.png

[100]: ./media/answerhub-tutorial/tutorial_general_100.png

[200]: ./media/answerhub-tutorial/tutorial_general_200.png
[201]: ./media/answerhub-tutorial/tutorial_general_201.png
[202]: ./media/answerhub-tutorial/tutorial_general_202.png
[203]: ./media/answerhub-tutorial/tutorial_general_203.png

