---
title: 'Zelfstudie: Azure Active Directory-integratie met Onit | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Onit.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b856227733fcc65e12eb43ea1f6b704502a7b29a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169340"
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a>Zelfstudie: Azure Active Directory-integratie met Onit

In deze zelfstudie leert u hoe u Onit integreren met Azure Active Directory (Azure AD).

Onit integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Onit heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Onit (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Onit, moet u de volgende items:

- Een Azure AD-abonnement
- Een Onit eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Onit uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-onit-from-the-gallery"></a>Onit uit de galerie toe te voegen
Voor het configureren van de integratie van Onit in Azure AD, moet u Onit uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Onit uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Onit**, selecteer **Onit** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Onit in de lijst met resultaten](./media/onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Onit op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Onit is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Onit tot stand worden gebracht.

In Onit, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Onit, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Onit](#create-an-onit-test-user)**  : als u wilt een equivalent van Britta Simon in Onit die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Onit.

**Voor het configureren van Azure AD eenmalige aanmelding met Onit, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Onit** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/onit-tutorial/tutorial_onit_samlbase.png)

1. Op de **Onit domein en URL's** sectie, voert u de volgende stappen uit:

    ![Onit domein en URL's, eenmalige aanmelding informatie](./media/onit-tutorial/tutorial_onit_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<sub-domain>.onit.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<sub-domain>.onit.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Onit Client ondersteuningsteam](https://www.onit.com/support) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De link om het certificaat te downloaden](./media/onit-tutorial/tutorial_onit_certificate.png) 

1. Onit toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **"Atrribute"** tabblad van de toepassing. In de volgende schermopname ziet u een voorbeeld hiervan. 

    ![Eenmalige aanmelding configureren](./media/onit-tutorial/tutorial_onit_attribute.png) 

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |
    | e-mail | user.mail |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/onit-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/onit-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat **Naamruimte** leeg.
    
    e. Klik op **OK**.

1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/onit-tutorial/tutorial_general_400.png)

1. Op de **Onit configuratie** sectie, klikt u op **configureren Onit** openen **aanmelding configureren** venster. Kopiëren de **URL van de afmelding, Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Onit configuratie](./media/onit-tutorial/tutorial_onit_configure.png)

1. Meld u in een ander browservenster in uw bedrijf Onit site als beheerder.

1. Klik in het menu aan de bovenkant op **beheer**.
   
   ![Beheer](./media/onit-tutorial/IC791174.png "Beheer")
1. Klik op **bewerken Corporation**.
   
   ![Bewerken Corporation](./media/onit-tutorial/IC791175.png "Corporation bewerken")
   
1. Klik op de **Security** tabblad.
    
    ![Gegevens bewerken bedrijf](./media/onit-tutorial/IC791176.png "bedrijfsgegevens bewerken")

1. Op de **Security** tabblad, voert u de volgende stappen uit:

    ![Single Sign-On](./media/onit-tutorial/IC791177.png "Single Sign-On")

    a. Als **Verificatiestrategie**, selecteer **Single Sign On en het wachtwoord**.
    
    b. In **Idp doel-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In **afmeldings-URL van id-provider** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In **Idp Cert vingerafdruk (SHA1)** tekstvak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure portal.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/onit-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/onit-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/onit-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/onit-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-onit-test-user"></a>Maak een testgebruiker Onit

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Onit, moeten ze worden ingericht voor Onit.  

In het geval van Onit is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **Onit** bedrijf site als beheerder.
1. Klik op **Add User**.
   
   ![Beheer](./media/onit-tutorial/IC791180.png "Beheer")
1. Op de **gebruiker toevoegen** dialoogvenster pagina, voert u de volgende stappen uit:
   
   ![Gebruiker toevoegen](./media/onit-tutorial/IC791181.png "Gebruiker toevoegen")
   
  1. Type de **naam** en de **e-mailadres** van een geldige Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt.
  1. Klik op **Create**.    
   
 > [!NOTE]
 > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Onit.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Onit toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Onit**.

    ![De koppeling Onit in de lijst met toepassingen](./media/onit-tutorial/tutorial_onit_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Onit in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Onit.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/onit-tutorial/tutorial_general_01.png
[2]: ./media/onit-tutorial/tutorial_general_02.png
[3]: ./media/onit-tutorial/tutorial_general_03.png
[4]: ./media/onit-tutorial/tutorial_general_04.png

[100]: ./media/onit-tutorial/tutorial_general_100.png

[200]: ./media/onit-tutorial/tutorial_general_200.png
[201]: ./media/onit-tutorial/tutorial_general_201.png
[202]: ./media/onit-tutorial/tutorial_general_202.png
[203]: ./media/onit-tutorial/tutorial_general_203.png

