---
title: 'Zelfstudie: Dropbox voor het automatisch gebruikers inrichten met Azure Active Directory configureren | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en Dropbox voor Bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 31b7d64e98c67cab8fea92a709a1452c1798a749
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203739"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>Zelfstudie: Dropbox voor bedrijven configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in Dropbox voor bedrijven en Azure AD wilt voor het automatisch inrichten en verwijdering van gebruikersaccounts vanuit Azure AD naar Dropbox voor bedrijven.

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een Dropbox voor bedrijven eenmalige aanmelding ingeschakeld abonnement.
*   Een gebruikersaccount in Dropbox voor bedrijven met Team beheerdersmachtigingen.

## <a name="assigning-users-to-dropbox-for-business"></a>Gebruikers toewijzen aan Dropbox voor bedrijven

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw Dropbox voor bedrijven-app. Als besloten, kunt u deze gebruikers in uw Dropbox voor bedrijven-app toewijzen door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>Belangrijke tips voor het toewijzen van gebruikers aan Dropbox voor bedrijven

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan Dropbox voor bedrijven voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker in Dropbox voor bedrijven, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten van...

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde Gebruikersinrichting inschakelen

Deze sectie helpt u bij het verbinden van uw Azure AD met Dropbox voor de gebruikersaccount van bedrijf Inrichtings-API, en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van accounts toegewezen gebruiker in Dropbox voor bedrijven op basis van gebruiker en groep toewijzing in Azure AD.

>[!Tip]
>U kunt ook op SAML gebaseerde eenmalige aanmelding ingeschakeld voor Dropbox voor bedrijven, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Het configureren van het inrichten van automatische gebruikersaccounts:

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > Bedrijfsapps > alle toepassingen** sectie.

2. Als u al Dropbox voor bedrijven hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van Dropbox voor bedrijven met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar de **Dropbox voor bedrijven** in de toepassingengalerie. Selecteer Dropbox voor bedrijven in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van Dropbox voor bedrijven, en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Een Dropbox voor bedrijven-aanmeldvenster wordt het geopend in een nieuw browservenster.

6. Op de **aanmelden met Dropbox om te koppelen aan Azure AD** dialoogvenster Aanmelden bij uw Dropbox voor bedrijven-tenant.

     ![Inrichten van gebruikers](./media/dropboxforbusiness-provisioning-tutorial/ic769518.png "inrichten van gebruikers")

7. Bevestig dat u wilt machtigen voor Azure Active Directory wijzigingen aanbrengen in uw Dropbox voor bedrijven-tenant. Klik op **toestaan**.
    
      ![Inrichten van gebruikers](./media/dropboxforbusiness-provisioning-tutorial/ic769519.png "inrichten van gebruikers")

8. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kunt verbinden met uw Dropbox voor bedrijven-app. Als de verbinding is mislukt, zorg ervoor dat uw Dropbox voor bedrijven-account Team beheerdersmachtigingen heeft en probeer de **"Autoriseren"** stap opnieuw uit.

9. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

10. Klik op **opslaan.**

11. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers naar Dropbox voor bedrijven.**

12. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD naar Dropbox voor bedrijven. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in Dropbox voor bedrijven voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

13. Als wilt inschakelen in de Azure AD-inrichtingsservice voor Dropbox voor bedrijven, wijzigt de **Inrichtingsstatus** naar **op** in de sectie instellingen

14. Klik op **opslaan.**

De initiële synchronisatie van alle gebruikers en/of groepen die zijn toegewezen aan Dropbox voor bedrijven in de sectie gebruikers en groepen wordt gestart. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken, waarin alle acties die worden uitgevoerd door de provisioning-service in uw Dropbox voor bedrijven-app worden beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](dropboxforbusiness-tutorial.md)
