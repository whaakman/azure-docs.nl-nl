---
title: 'Zelfstudie: Azure Active Directory-integratie met UNIFI | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UNIFI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a93e4863a8466ad6599b11e6fe6e53d8d4d971a4
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049912"
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Zelfstudie: Azure Active Directory-integratie met UNIFI

In deze zelfstudie leert u hoe u UNIFI integreren met Azure Active Directory (Azure AD).

UNIFI integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot UNIFI heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij UNIFI (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UNIFI, moet u de volgende items:

- Een Azure AD-abonnement
- Een UNIFI eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. UNIFI uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-unifi-from-the-gallery"></a>UNIFI uit de galerie toe te voegen
Voor het configureren van de integratie van UNIFI in Azure AD, moet u UNIFI uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UNIFI uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **UNIFI**.

    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/tutorial_unifi_search.png)

5. Selecteer in het deelvenster resultaten **UNIFI**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met UNIFI op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in UNIFI is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UNIFI tot stand worden gebracht.

In UNIFI, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met UNIFI, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een UNIFI testgebruiker](#creating-a-unifi-test-user)**  : als u wilt een equivalent van Britta Simon in UNIFI die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing UNIFI.

**Voor het configureren van Azure AD eenmalige aanmelding met UNIFI, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **UNIFI** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_samlbase.png)

3. Op de **UNIFI domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_url1.png)

    In de **id** tekstvak typt u de waarde: `INVIEWlabs` 

4. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://app.discoverunifi.com/login`

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_general_400.png)
    
7. Op de **UNIFI configuratie** sectie, klikt u op **configureren UNIFI** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_configure.png)

8. In een ander browservenster aanmelden bij uw **UNIFI** bedrijf site als administrator.

9. Klik op de **gebruikers**.

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/app1.png) 

10. Klik op de **nieuwe id-Provider toevoegen**.

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/app2.png)

11. In de **id-Provider toevoegen** sectie, voert u de volgende stappen uit:   

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/app3.png) 

    a. In de **providernaam** tekstvak, typ de naam van de id-Provider...

    b. In de **URL van de Provider** tekstvak plakken de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Open het certificaat dat u hebt gedownload vanuit Azure portal in Kladblok en verwijder de **---BEGIN CERTIFICATE---** en **---END CERTIFICATE---** code en plak de resterende inhoud in de **Certificaat** tekstvak.

    d. Selecteer de **-provider is standaard** selectievakje.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/unifi-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-unifi-test-user"></a>Het maken van een testgebruiker UNIFI

In deze sectie maakt u een gebruiker met de naam Britta Simon. **UNIFI** biedt ondersteuning voor automatisch gebruikers inrichten, zodat er geen handmatige stappen vereist zijn. Gebruikers worden automatisch gemaakt na een geslaagde authenticatie van de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UNIFI.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan UNIFI toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **UNIFI**.

    ![Eenmalige aanmelding configureren](./media/unifi-tutorial/tutorial_unifi_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel UNIFI in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing UNIFI.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/unifi-tutorial/tutorial_general_01.png
[2]: ./media/unifi-tutorial/tutorial_general_02.png
[3]: ./media/unifi-tutorial/tutorial_general_03.png
[4]: ./media/unifi-tutorial/tutorial_general_04.png

[100]: ./media/unifi-tutorial/tutorial_general_100.png

[200]: ./media/unifi-tutorial/tutorial_general_200.png
[201]: ./media/unifi-tutorial/tutorial_general_201.png
[202]: ./media/unifi-tutorial/tutorial_general_202.png
[203]: ./media/unifi-tutorial/tutorial_general_203.png

