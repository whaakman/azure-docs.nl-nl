---
title: 'Zelfstudie: Azure Active Directory-integratie met kudo | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en kudo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 5807ab0090f401c02599db40f110b7a9b1504060
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047049"
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a>Zelfstudie: Azure Active Directory-integratie met kudo

In deze zelfstudie leert u hoe u kudo integreren met Azure Active Directory (Azure AD).

Kudo integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot kudo heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij kudo (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met kudo, moet u de volgende items:

- Een Azure AD-abonnement
- Een kudo eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kudo uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kudos-from-the-gallery"></a>Kudo uit de galerie toe te voegen
Voor het configureren van de integratie van kudo in Azure AD, moet u kudo uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen kudo uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **kudo**.

    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/tutorial_kudos_search.png)

5. Selecteer in het deelvenster resultaten **kudo**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met kudo op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in kudo is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kudo tot stand worden gebracht.

In kudo, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met kudo, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker kudo](#creating-a-kudos-test-user)**  : als u wilt een equivalent van Britta Simon in kudo die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing kudo.

**Voor het configureren van Azure AD eenmalige aanmelding met kudo, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **kudo** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_kudos_samlbase.png)

3. Op de **kudo domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_kudos_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company>.kudosnow.com`
    
    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [kudo Client ondersteuningsteam](http://success.kudosnow.com/home) deze waarde op te halen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_kudos_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_general_400.png)

6. Op de **kudo configuratie** sectie, klikt u op **configureren kudo** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_kudos_configure.png) 

7. Meld u in een ander browservenster in uw bedrijf kudo site als beheerder.

8. Klik in het menu aan de bovenkant op **instellingen**.
   
    ![Instellingen voor](./media/kudos-tutorial/ic787806.png "instellingen")

9. Klik op **integraties \> SSO**.

10. In de **SSO** sectie, voert u de volgende stappen uit:
   
    ![EENMALIGE AANMELDING](./media/kudos-tutorial/ic787807.png "EENMALIGE AANMELDING")
   
    a. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal. 

    b. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak
   
    c. In **afmeldings-URL**, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
   
    d. In de **uw kudo URL** tekstvak, typ de naam van uw bedrijf.
   
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/kudos-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-kudos-test-user"></a>Het maken van een testgebruiker kudo

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij kudo, moeten ze worden ingericht voor kudo. 

In het geval van kudo is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **kudo** bedrijf site als administrator.

2. Klik in het menu aan de bovenkant op **instellingen**.
   
   ![Instellingen voor](./media/kudos-tutorial/ic787806.png "instellingen")

3. Klik op **Gebruikerbeheerder**.

4. Klik op de **gebruikers** tabblad en klik vervolgens op **toevoegen van een gebruiker**.
   
   ![Gebruikerbeheerder](./media/kudos-tutorial/ic787809.png "Gebruikerbeheerder")

5. In de **toevoegen van een gebruiker** sectie, voert u de volgende stappen uit:
   
    ![Een gebruiker toevoegen](./media/kudos-tutorial/ic787810.png "een gebruiker toevoegen")
   
    a. Type de **voornaam**, **achternaam**, **e** en andere gegevens van een geldige Azure Active Directory-account dat u inrichten in de bijbehorende tekstvakken wilt.
   
    b. Klik op **gebruiker maken**.

>[!NOTE]
>U kunt alle andere kudo gebruiker-account maken van hulpprogramma's of API's geleverd door kudo aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan kudo.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan kudo toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **kudo**.

    ![Eenmalige aanmelding configureren](./media/kudos-tutorial/tutorial_kudos_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel kudo in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing kudo. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kudos-tutorial/tutorial_general_01.png
[2]: ./media/kudos-tutorial/tutorial_general_02.png
[3]: ./media/kudos-tutorial/tutorial_general_03.png
[4]: ./media/kudos-tutorial/tutorial_general_04.png

[100]: ./media/kudos-tutorial/tutorial_general_100.png

[200]: ./media/kudos-tutorial/tutorial_general_200.png
[201]: ./media/kudos-tutorial/tutorial_general_201.png
[202]: ./media/kudos-tutorial/tutorial_general_202.png
[203]: ./media/kudos-tutorial/tutorial_general_203.png

