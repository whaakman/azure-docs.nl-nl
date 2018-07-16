---
title: 'Zelfstudie: Azure Active Directory-integratie met Evidence.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Evidence.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 5859a48034339fafa59acb003c117dd8873850c2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052866"
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Zelfstudie: Azure Active Directory-integratie met Evidence.com

In deze zelfstudie leert u hoe u Evidence.com integreren met Azure Active Directory (Azure AD).

Evidence.com integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Evidence.com heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Evidence.com (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Evidence.com, moet u de volgende items:

- Een Azure AD-abonnement
- Een Evidence.com eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Evidence.com uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-evidencecom-from-the-gallery"></a>Evidence.com uit de galerie toe te voegen
Voor het configureren van de integratie van Evidence.com in Azure AD, moet u Evidence.com uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Evidence.com uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Evidence.com**, selecteer **Evidence.com** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Evidence.com in de lijst met resultaten](./media/evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Evidence.com op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Evidence.com is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Evidence.com tot stand worden gebracht.

In Evidence.com, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Evidence.com, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Evidence.com](#create-a-evidencecom-test-user)**  : als u wilt een equivalent van Britta Simon in Evidence.com die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Evidence.com.

**Voor het configureren van Azure AD eenmalige aanmelding met Evidence.com, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Evidence.com** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. Op de **Evidence.com domein en URL's** sectie, voert u de volgende stappen uit:

    ![Evidence.com domein en URL's, eenmalige aanmelding informatie](./media/evidence-tutorial/tutorial_evidence.com_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<yourtenant>.evidence.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<yourtenant>.evidence.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Evidence.com Client ondersteuningsteam](https://communities.taser.com/support/SupportContactUs?typ=LE) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/evidence-tutorial/tutorial_general_400.png)

6. Op de **Evidence.com configuratie** sectie, klikt u op **configureren Evidence.com** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Evidence.com configuratie](./media/evidence-tutorial/tutorial_evidence.com_configure.png) 

7. In een afzonderlijke geopend browservenster, meld u aan bij uw Evidence.com tenant als beheerder en navigeer naar **Admin** tabblad

8. Klik op **Bureau voor eenmalige aanmelding**

9. Selecteer **SAML eenmalige aanmelding op basis van**

10. Kopiëren de **SAML entiteit-ID**, **Single Sign-On Service URL voor SAML** en **afmelding URL** waarden die worden weergegeven in de Azure-portal en de bijbehorende velden op Evidence.com.

11. Open het gedownloade Certificate(Base64)-bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **beveiligingscertificaat** vak. 

12. Sla de configuratie in Evidence.com.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/evidence-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/evidence-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/evidence-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/evidence-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-evidencecom-test-user"></a>Maak een testgebruiker Evidence.com

Voor Azure AD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht voor toegang in de toepassing Evidence.com. In deze sectie wordt beschreven hoe u Azure AD-gebruikersaccounts in Evidence.com maken

**Voor het inrichten van een gebruikersaccount in Evidence.com:**

1. Meld u in een browservenster in uw bedrijf Evidence.com site als beheerder.

2. Navigeer naar **Admin** tabblad.

3. Klik op **gebruiker toevoegen**.

4. Klik op de knop **Toevoegen**.

5. De **e-mailadres** van de toegevoegde gebruiker moet overeenkomen met de gebruikersnaam van de gebruikers in Azure AD die u wilt om toegang te geven. Als de gebruikersnaam en e-mailadres niet dezelfde waarde in uw organisatie, kunt u de **Evidence.com > kenmerken > Single Sign-On** sectie van de Azure portal om te wijzigen van de nameidenitifer verzonden naar Evidence.com om te worden de e-mailadres.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Evidence.com.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Evidence.com toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Evidence.com**.

    ![De koppeling Evidence.com in de lijst met toepassingen](./media/evidence-tutorial/tutorial_evidence.com_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Evidence.com in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Evidence.com.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/evidence-tutorial/tutorial_general_01.png
[2]: ./media/evidence-tutorial/tutorial_general_02.png
[3]: ./media/evidence-tutorial/tutorial_general_03.png
[4]: ./media/evidence-tutorial/tutorial_general_04.png

[100]: ./media/evidence-tutorial/tutorial_general_100.png

[200]: ./media/evidence-tutorial/tutorial_general_200.png
[201]: ./media/evidence-tutorial/tutorial_general_201.png
[202]: ./media/evidence-tutorial/tutorial_general_202.png
[203]: ./media/evidence-tutorial/tutorial_general_203.png

