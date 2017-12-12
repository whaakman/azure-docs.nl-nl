---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dropbox voor bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c41760d60d53dee7be36b2af287cd6755605b708
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Zelfstudie: Dropbox voor bedrijven configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u wilt uitvoeren in Dropbox voor bedrijven en Azure AD automatisch inrichten en ongedaan inrichten gebruikersaccounts vanuit Azure AD naar Dropbox voor bedrijven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Dropbox voor bedrijven-eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Dropbox voor bedrijven met beheerdersmachtigingen Team.

## <a name="assigning-users-to-dropbox-for-business"></a>Gebruikers toewijzen aan Dropbox voor bedrijven

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw Dropbox voor bedrijven-app. Als besloten, kunt u deze gebruikers toewijzen aan uw Dropbox voor bedrijven-app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Belangrijke tips voor het toewijzen van gebruikers aan Dropbox voor bedrijven

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan Dropbox voor bedrijven voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan Dropbox voor bedrijven, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten van...

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinden van uw Azure AD met Dropbox voor bedrijven van gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Dropbox voor bedrijven op basis van gebruikers en groepen toewijzen in Azure AD.

>[!Tip]
>U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor Dropbox voor bedrijven, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Voor het configureren van automatische account gebruikersaanvragen:

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al Dropbox voor bedrijven voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van Dropbox voor bedrijven met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **Dropbox voor bedrijven** in de galerie met toepassingen. Selecteer Dropbox voor bedrijven in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Dropbox voor bedrijven en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![Inrichting](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Er wordt een Dropbox voor zakelijke aanmeldingsdialoogvenster geopend in een nieuw browservenster.

6. Op de **aanmelden bij Dropbox om te koppelen aan Azure AD** dialoogvenster Aanmelden bij uw Dropbox voor bedrijven-tenant.

     ![Gebruikers inrichten](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "gebruikers inrichten")

7. Bevestig dat u wilt machtigen voor Azure Active Directory wijzigingen aanbrengen in uw Dropbox voor bedrijven-tenant. Klik op **toestaan**.
    
      ![Gebruikers inrichten](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "gebruikers inrichten")

8. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw Dropbox voor bedrijven-app. Als de verbinding is mislukt, zorg ervoor dat uw Dropbox voor bedrijven-account Team beheerdersmachtigingen heeft en probeer de **'Autoriseren'** stap opnieuw.

9. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

10. Klik op **opslaan.**

11. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers Dropbox voor bedrijven.**

12. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker van Azure AD worden gesynchroniseerd met Dropbox voor bedrijven. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Dropbox voor bedrijven voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

13. Wijzigen om de Azure AD-service voor Dropbox voor bedrijven inricht, de **inrichting Status** naar **op** in de sectie instellingen

14. Klik op **opslaan.**

De initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan Dropbox voor bedrijven in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service op uw Dropbox voor bedrijven-app.

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd met Dropbox voor bedrijven.

Een is voltooid gebruikersaanvragen cyclus wordt aangegeven door de bijbehorende status.

![Gebruikers toewijzen](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "gebruikers toewijzen")


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-dropboxforbusiness-tutorial.md)