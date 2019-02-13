---
title: 'Zelfstudie: Concur configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Concur configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 441aa9805f2a453e22f207238315125d2a281838
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205678"
---
# <a name="tutorial-configure-concur-for-automatic-user-provisioning"></a>Zelfstudie: Concur configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in Concur en Azure AD wilt voor het automatisch inrichten en inrichting van gebruikersaccounts vanuit Azure AD naar Concur ongedaan maken.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Concur eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Concur met Team beheerdersmachtigingen.

## <a name="assigning-users-to-concur"></a>Gebruikers toewijzen aan Concur

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD de gebruikers die toegang nodig tot uw app Concur vertegenwoordigen. Als besloten, kunt u deze gebruikers toewijzen aan uw app Concur door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a>Belangrijke tips voor het toewijzen van gebruikers aan Concur

*   Het wordt aanbevolen dat één Azure AD-gebruiker worden toegewezen aan Concur voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan Concur toewijzen, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Gebruikersinrichting inschakelen

In deze sectie helpt u bij het verbinden van uw Azure AD aan de gebruikersaccount van Concur Inrichtings-API en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Concur op basis van gebruikers en groepen in Azure AD.

> [!Tip] 
> U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Concur, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-user-account-provisioning"></a>Het configureren van het inrichten van gebruikersaccounts:

Het doel van deze sectie is om te beschrijven hoe u inrichting van Active Directory-gebruikersaccounts met Concur inschakelen.

Heeft om in te schakelen van apps in de Service onkosten, er moet een juiste installatie en het gebruik van een Web-servicebeheerder-profiel. Niet de rol Admin WS niet toevoegen aan uw bestaande administrator-profiel dat u voor T & E beheerfuncties gebruikt.

Concur consultants van Microsoft of de clientbeheerder van de moet een afzonderlijke Web Service Administrator-profiel maken en de beheerder van de Client moet dit profiel gebruiken voor de beheerder van Web Services-functies (bijvoorbeeld om apps). Deze profielen moeten gescheiden worden gehouden van van de clientbeheerder dagelijkse T & E admin-profiel (het T & E admin-profiel moet niet over de WSAdmin rol).

Wanneer u het profiel moet worden gebruikt voor het inschakelen van de app maakt, voert u de naam van de clientbeheerder in de velden van gebruiker-profiel. Dit wijst eigendom toe aan het profiel. Als een of meer profielen is gemaakt, de client moet zich aanmelden met dit profiel in klikt u op de '*inschakelen*"knop voor een App-Partner in het menu Web Services.

De volgende oorzaken hebben, moet deze actie niet worden uitgevoerd met het profiel dat ze voor het beheer van normale T & E gebruiken.

* De client moet het account klikt op '*Ja*' in het dialoogvenster-upvenster dat wordt weergegeven nadat een app is ingeschakeld. Hier klikken erkent dat de client wil voor de Partner-toepassing voor toegang tot hun gegevens, zodat u of de Partner kan niet klikt u op die knop Ja.

* Als de clientbeheerder van een die een app is ingeschakeld met behulp van de beheerder T & E profiel het bedrijf verlaat (wat resulteert in het profiel wordt gedeactiveerd), alle apps die zijn ingeschakeld met behulp van profiel niet werken totdat de app is ingeschakeld met een andere actieve WS-Admin-profiel. Dit is de reden waarom u moet afzonderlijke WS-Admin-profielen maken.

* Als een beheerder van het bedrijf verlaat, kan de naam die is gekoppeld aan het profiel WS-beheerder worden gewijzigd in de beheerder van de vervangende indien gewenst zonder gevolgen voor dat de ingeschakelde app omdat dit profiel niet hoeft uitgeschakeld.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw **Concur** tenant.

2. Uit de **beheer** in het menu **webservices**.
   
    ![Concur tenant](./media/concur-provisioning-tutorial/IC721729.png "Concur tenant")

3. Aan de linkerkant van de **webservices** venster **partnertoepassing inschakelen**.
   
    ![Partnertoepassing inschakelen](./media/concur-provisioning-tutorial/ic721730.png "partnertoepassing inschakelen")

4. Uit de **toepassing inschakelen** in de lijst met **Azure Active Directory**, en klik vervolgens op **inschakelen**.
   
    ![Microsoft Azure Active Directory](./media/concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Klik op **Ja** sluiten de **actie bevestigen** dialoogvenster.
   
    ![Bevestig de actie](./media/concur-provisioning-tutorial/ic721732.png "Bevestig de actie")

6. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

7. Als u al Concur hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Concur met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Concur** in de toepassingengalerie. Selecteer Concur in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

8. Selecteer uw exemplaar van Concur en selecteer vervolgens de **Provisioning** tabblad.

9. Stel de **Inrichtingsmodus** naar **automatische**. 
 
    ![inrichten](./media/concur-provisioning-tutorial/provisioning.png)

10. Onder de **beheerdersreferenties** sectie, voer de **gebruikersnaam** en de **wachtwoord** van uw beheerder Concur.

11. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw app Concur. Als de verbinding is mislukt, controleert u of dat uw account Concur Teambeheerder machtigingen heeft.

12. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

13. Klik op **opslaan.**

14. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan Concur.**

15. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die van Azure AD worden gesynchroniseerd naar Concur. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Concur voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

16. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Concur, wijzigt de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

17. Klik op **opslaan.**

U kunt nu een testaccount maken. Wacht tot 20 minuten om te verifiëren dat het account is gesynchroniseerd met Concur.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](concur-tutorial.md)

