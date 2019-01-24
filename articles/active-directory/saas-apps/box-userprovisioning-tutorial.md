---
title: 'Zelfstudie: Vak voor het automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Box.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: jeedes
ms.openlocfilehash: 58ab80b9cdf6fbcfa8aebeeac837687ded41ae39
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821811"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Zelfstudie: Vak voor het automatisch inrichten van gebruikers configureren

Het doel van deze zelfstudie is om weer te geven van de stappen dat u moet uitvoeren in het vak en Azure AD aan automatisch inrichten en ongedaan maken inrichting gebruikersaccounts van Azure AD naar Box.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Box hebt u de volgende items nodig:

- Een Azure AD-tenant
- Een vak bedrijfsplan of hoger

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* een productie-omgeving gebruiken.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-box"></a>Gebruikers toewijzen aan het vak 

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot de Box-app. Als besloten, kunt u deze gebruikers toewijzen aan uw Box-app door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Gebruikers en groepen toewijzen
De **vak > gebruikers en groepen** tabblad in de Azure-portal kunt u opgeven welke gebruikers en groepen toegang moeten worden verleend in box. Toewijzing van een gebruiker of groep zorgt ervoor dat de volgende taken uitvoeren:

* Azure AD kan de toegewezen gebruiker (hetzij door directe toewijzing of groepslidmaatschap) om te verifiëren naar Box. Als een gebruiker niet is toegewezen, Azure AD staat niet toe dat ze zich aanmelden met Box en retourneert een fout op de aanmeldingspagina van Azure AD.
* Een app-tegel voor Box wordt toegevoegd aan van de gebruiker [startprogramma voor toepassingen van](../manage-apps/end-user-experiences.md).
* Als automatisch inrichten is ingeschakeld, worden klikt u vervolgens de toegewezen gebruikers en/of groepen toegevoegd aan de inrichting wachtrij automatisch worden ingericht.
  
  * Als alleen objecten zijn geconfigureerd om te worden ingericht, klikt u vervolgens alle rechtstreeks toegewezen gebruikers worden geplaatst in de wachtrij voor inrichting en alle gebruikers die lid van alle toegewezen groepen zijn in de inrichting wachtrij zijn geplaatst. 
  * Als groepsobjecten zijn geconfigureerd om te worden ingericht, worden alle objecten van de toegewezen groep ingericht voor Box, en voor alle gebruikers die lid zijn van die groepen. De groeps- en -lidmaatschappen worden bewaard na wordt geschreven naar Box.

Kunt u de **kenmerken > Single Sign-On** tabblad om te configureren welke gebruikerskenmerken (of claims), worden weergegeven in box tijdens de verificatie op basis van SAML en het **kenmerken > Provisioning** tab naar configureren hoe de gebruikers- en groepskenmerken tijdens het inrichten van bewerkingen van Azure AD wordt overgebracht naar Box.

### <a name="important-tips-for-assigning-users-to-box"></a>Belangrijke tips voor het toewijzen van gebruikers met Box 

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan het vak voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker in box, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

In deze sectie helpt bij het verbinden van uw Azure AD aan de gebruikersaccount van het vak Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van accounts toegewezen gebruiker in Box op basis van gebruikers en groepen in Azure AD.

Als automatisch inrichten is ingeschakeld, worden klikt u vervolgens de toegewezen gebruikers en/of groepen toegevoegd aan de inrichting wachtrij automatisch worden ingericht.
    
 * Als er slechts gebruikersobjecten worden geconfigureerd om te worden ingericht, en vervolgens rechtstreeks toegewezen gebruikers worden geplaatst in de wachtrij voor inrichting en alle gebruikers die lid van alle toegewezen groepen zijn worden geplaatst in de wachtrij voor inrichting. 
    
 * Als groepsobjecten zijn geconfigureerd om te worden ingericht, worden alle objecten van de toegewezen groep ingericht voor Box, en voor alle gebruikers die lid zijn van die groepen. De groeps- en -lidmaatschappen worden bewaard na wordt geschreven naar Box.

> [!TIP] 
> U kunt ook op SAML gebaseerde eenmalige aanmelding voor Box is ingeschakeld, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Het configureren van het inrichten van automatische gebruikersaccounts:

Het doel van deze sectie is om een overzicht van het inschakelen van de inrichting van Active Directory-gebruikersaccounts met Box te.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u vak al hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Box met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **vak** in de toepassingengalerie. Schakel in in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van het vak en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/box-userprovisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren** een dialoogvenster voor aanmelding in een nieuw browservenster geopend.

6. Op de **aanmelding om toegang te verlenen aan vak** pagina, geef de vereiste referenties op en klik vervolgens op **autoriseren**. 
   
    ![Automatische inrichting inschakelen](./media/box-userprovisioning-tutorial/IC769546.png "automatische inrichting inschakelen")

7. Klik op **toegang verlenen tot vak** te autoriseren met deze bewerking en om terug te keren naar de Azure-portal. 
   
    ![Automatische inrichting inschakelen](./media/box-userprovisioning-tutorial/IC769549.png "automatische inrichting inschakelen")

8. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw Box-app. Als de verbinding is mislukt, zorg ervoor dat uw Box-account Team beheerdersmachtigingen heeft en probeer de **"Autoriseren"** stap opnieuw uit.

9. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

10. Klik op **opslaan.**

11. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers naar Box.**

12. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Box. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in het vak voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

13. Om in te schakelen in de Azure AD-inrichtingsservice voor Box, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

14. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan het vak in de sectie gebruikers en groepen worden gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service op de Box-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

Gesynchroniseerde gebruikers worden vermeld in uw tenant vak onder **beheerde gebruikers** in de **-beheerconsole**.

![Integratiestatus van](./media/box-userprovisioning-tutorial/IC769556.png "integratie-status")


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](box-tutorial.md)
