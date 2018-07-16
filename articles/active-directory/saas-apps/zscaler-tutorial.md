---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: b7a011baeb9b526e8de22ecaa34672327c6aab17
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050164"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler

In deze zelfstudie leert u hoe u Zscaler integreren met Azure Active Directory (Azure AD).

Zscaler integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zscaler heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zscaler (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zscaler eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-from-the-gallery"></a>Zscaler uit de galerie toe te voegen
Voor het configureren van de integratie van Zscaler in Azure AD, moet u Zscaler uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Zscaler**.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/tutorial_zscaler_search.png)

5. Selecteer in het deelvenster resultaten **Zscaler**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Zscaler is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zscaler tot stand worden gebracht.

In de Zscaler, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Proxy-instellingen configureren](#configuring-proxy-settings)**  : als u wilt de proxy-instellingen in Internet Explorer configureren
3. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Het maken van een testgebruiker Zscaler](#creating-a-zscaler-test-user)**  : als u wilt een equivalent van Britta Simon in Zscaler die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. Op de **Zscaler-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_zscaler_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.zsclaer.net`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Zscaler Client ondersteuningsteam](https://www.zscaler.com/company/contact) deze waarde op te halen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_general_400.png)

6. Op de **Zscaler configuratie** sectie, klikt u op **configureren Zscaler** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_zscaler_configure.png) 

7. In een ander browservenster aanmelden bij uw bedrijf ZScaler site als beheerder.

8. Klik in het menu aan de bovenkant op **beheer**.
   
    ![Beheer](./media/zscaler-tutorial/ic800206.png "beheer")

9. Onder **beheren beheerders en rollen**, klikt u op **gebruikers beheren & verificatie**.   
            
    ![Gebruikers & verificatie beheren](./media/zscaler-tutorial/ic800207.png "gebruikers & verificatie beheren")

10. In de **verificatie-opties kiezen voor uw organisatie** sectie, voert u de volgende stappen uit:   
                
    ![Verificatie](./media/zscaler-tutorial/ic800208.png "verificatie")
   
    a. Selecteer **verifiëren met behulp van SAML Single Sign-On**.

    b. Klik op **configureren van eenmalige SAML-aanmelding Parameters**.

11. Op de **configureren SAML Single Sign-On Parameters** dialoogvenster pagina, voer de volgende stappen uit en klik vervolgens op **gedaan**

    ![Single Sign-On](./media/zscaler-tutorial/ic800209.png "Single Sign-On")
    
    a. Plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit de Azure portal in de **URL van de SAML-Portal waarmee gebruikers worden verzonden voor verificatie** tekstvak.
    
    b. In de **aanmeldingsnaam die het kenmerk** tekstvak, type **NameID**.
    
    c. Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    
    d. Selecteer **SAML automatische inrichting inschakelen**.

12. Op de **gebruikersverificatie configureren** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Beheer](./media/zscaler-tutorial/ic800210.png "beheer")
    
    a. Klik op **Opslaan**.

    b. Klik op **nu activeren**.

## <a name="configuring-proxy-settings"></a>Proxy-instellingen configureren
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen in Internet Explorer configureren

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** uit de **extra** menu voor open de **Internetopties** dialoogvenster.   
    
     ![Internetopties](./media/zscaler-tutorial/ic769492.png "Internetopties")

3. Klik op de **verbindingen** tabblad.   
  
     ![Verbindingen](./media/zscaler-tutorial/ic769493.png "verbindingen")

4. Klik op **LAN-instellingen** openen de **LAN-instellingen** dialoogvenster.

5. In het gedeelte van de server Proxy de volgende stappen uitvoeren:   
   
    ![Proxyserver](./media/zscaler-tutorial/ic769494.png "proxyserver")

    a. Selecteer **een proxyserver gebruiken voor uw LAN**.

    b. Typ in het tekstvak adres **gateway.zscaler.net**.

    c. Typ in het tekstvak poort **80**.

    d. Selecteer **proxyserver niet gebruiken voor lokale adressen**.

    e. Klik op **OK** sluiten de **Local Area Network (LAN)-instellingen** dialoogvenster.

6. Klik op **OK** sluiten de **Internetopties** dialoogvenster.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/zscaler-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-zscaler-test-user"></a>Het maken van een testgebruiker Zscaler

Als u wilt dat Azure AD-gebruikers zich aanmelden bij ZScaler, moeten ze worden ingericht op ZScaler.  
In het geval van ZScaler is inrichten een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zscaler** tenant.

2. Klik op **beheer**.   
   
    ![Beheer](./media/zscaler-tutorial/ic781035.png "beheer")

3. Klik op **Gebruikersbeheer**.   
        
     ![Voeg](./media/zscaler-tutorial/ic781036.png "toevoegen")

4. In de **gebruikers** tabblad **toevoegen**.
      
    ![Voeg](./media/zscaler-tutorial/ic781037.png "toevoegen")

5. In de sectie toevoegen van gebruiker door de volgende stappen uitvoeren:
        
    ![Gebruiker toevoegen](./media/zscaler-tutorial/ic781038.png "gebruiker toevoegen")
   
    a. Type de **UserID**, **weergavenaam gebruiker**, **wachtwoord**, **wachtwoord bevestigen**, en selecteer vervolgens **groepen**en de **afdeling** van een geldige AAD-account dat u inrichten wilt.

    b. Klik op **Opslaan**.

> [!NOTE]
> U kunt elke andere ZScaler gebruiker account hulpmiddelen voor het maken of API's die door ZScaler inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Zscaler toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Zscaler**.

    ![Eenmalige aanmelding configureren](./media/zscaler-tutorial/tutorial_zscaler_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Zscaler-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zscaler-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-tutorial/tutorial_general_203.png

