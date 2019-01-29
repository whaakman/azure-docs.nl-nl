---
title: 'Zelfstudie: Azure Active Directory-integratie met AppBlade | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AppBlade.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2efe0cd50e894ad76ab3b40b17970091870f9711
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178049"
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a>Zelfstudie: Azure Active Directory-integratie met AppBlade

In deze zelfstudie leert u hoe u AppBlade integreren met Azure Active Directory (Azure AD).

AppBlade integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AppBlade heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AppBlade (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AppBlade, moet u de volgende items:

- Een Azure AD-abonnement
- Een AppBlade eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AppBlade uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-appblade-from-the-gallery"></a>AppBlade uit de galerie toe te voegen
Voor het configureren van de integratie van AppBlade in Azure AD, moet u AppBlade uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AppBlade uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **AppBlade**.

    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/tutorial_appblade_search.png)

5. Selecteer in het deelvenster resultaten **AppBlade**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met AppBlade op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in AppBlade is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AppBlade tot stand worden gebracht.

In AppBlade, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met AppBlade, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker AppBlade](#creating-an-appblade-test-user)**  : als u wilt een equivalent van Britta Simon in AppBlade die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing AppBlade.

**Voor het configureren van Azure AD eenmalige aanmelding met AppBlade, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AppBlade** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/appblade-tutorial/tutorial_appblade_samlbase.png)

3. Op de **AppBlade domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/appblade-tutorial/tutorial_appblade_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.appblade.com/saml/<tenantid>`

    > [!NOTE] 
    > Deze waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [AppBlade Client ondersteuningsteam](mailto:support@appblade.com) om de waarde. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/appblade-tutorial/tutorial_appblade_certificate.png) 

5. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/appblade-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **AppBlade** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [AppBlade ondersteuningsteam](mailto:support@appblade.com). Bovendien vraag deze om te configureren de **URL SSO-verlener** als `https://appblade.com/saml`. Deze instelling is vereist voor eenmalige aanmelding in om te werken.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/appblade-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-appblade-test-user"></a>Het maken van een testgebruiker AppBlade

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in AppBlade. AppBlade biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. **Zorg ervoor dat de domeinnaam van uw is geconfigureerd met AppBlade voor het inrichten van gebruikers. Na die alleen de gebruiker just-in-time inrichting werkt.**

Als de gebruiker een e-mailadres dat eindigt op het domein dat is geconfigureerd door AppBlade voor uw account en vervolgens de gebruiker wordt dan automatisch lid van het account als een lid met het machtigingsniveau die u opgeeft heeft, dit is een van de 'Basic' (een basic gebruiker die u kunt toepassingen alleen installeren) , 'Teamlid' (een gebruiker die u kunt nieuwe app-versies uploaden en beheren van projecten) of 'Beheerder' (volledige beheerdersbevoegdheden aan het account). Normaal gesproken zou een Basic kiezen en vervolgens het niveau verhogen gebruikers handmatig via een beheerdersaccount (AppBlade moet een op basis van een e-beheerderaanmelding vooraf configureren of een gebruiker bevorderen namens de klant na aanmelding).

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot AppBlade als deze nog niet bestaat. 

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [AppBlade ondersteuningsteam](mailto:support@appblade.com).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan AppBlade.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan AppBlade toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **AppBlade**.

    ![Eenmalige aanmelding configureren](./media/appblade-tutorial/tutorial_appblade_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.  
Wanneer u op de tegel AppBlade in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing AppBlade. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/appblade-tutorial/tutorial_general_01.png
[2]: ./media/appblade-tutorial/tutorial_general_02.png
[3]: ./media/appblade-tutorial/tutorial_general_03.png
[4]: ./media/appblade-tutorial/tutorial_general_04.png

[100]: ./media/appblade-tutorial/tutorial_general_100.png

[200]: ./media/appblade-tutorial/tutorial_general_200.png
[201]: ./media/appblade-tutorial/tutorial_general_201.png
[202]: ./media/appblade-tutorial/tutorial_general_202.png
[203]: ./media/appblade-tutorial/tutorial_general_203.png

