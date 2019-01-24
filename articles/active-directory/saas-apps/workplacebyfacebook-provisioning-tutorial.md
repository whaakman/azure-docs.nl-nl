---
title: 'Zelfstudie: Werkplek door Facebook configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Workplace by Facebook configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 63faea6dffa47aa7b0b9a1cc27052c0b0885aca9
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814350"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Zelfstudie: Werkplek door Facebook configureren voor het automatisch inrichten van gebruikers

Het doel van deze zelfstudie is om weer te geven u de stappen die u uitvoeren in de werkplek op Facebook en Azure AD wilt om automatisch inrichten en verwijdering van gebruikersaccounts uit Azure AD aan een werkplek op Facebook.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Workplace by Facebook:

- Een Azure AD-abonnement
- Een werkplek op Facebook eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="assigning-users-to-workplace-by-facebook"></a>Gebruikers toewijzen aan een werkplek op Facebook

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, wordt alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw werkplek op Facebook-app. Als besloten, kunt u deze gebruikers toewijzen aan uw werkplek met Facebook-app door de instructies hier:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a>Belangrijke tips voor het toewijzen van gebruikers aan de werkplek op Facebook

*   Het wordt aanbevolen dat één Azure AD-gebruiker is toegewezen aan een werkplek op Facebook voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan een werkplek op Facebook, moet u een geldige gebruikersrol selecteren. De rol 'standaardtoegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Inrichting van de gebruiker inschakelen

Deze sectie helpt u bij het verbinden van uw Azure AD met werkplek door de Facebook-gebruikersaccount Inrichtings-API, en configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van accounts in de werkplek op Facebook op basis van gebruikers en groepen toegewezen gebruiker toewijzing in Azure AD.

>[!Tip]
>U kunt ook op SAML gebaseerde eenmalige aanmelding inschakelen voor werkplek met Facebook, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Het configureren van het inrichten van gebruikersaccounts aan een werkplek op Facebook in Azure AD:

Het doel van deze sectie is om een overzicht van het inschakelen van de inrichting van Active Directory-gebruikersaccounts aan een werkplek op Facebook te.

Azure AD ondersteunt de mogelijkheid om automatisch te synchroniseren de accountdetails van de toegewezen gebruikers aan een werkplek op Facebook. Deze automatische synchronisatie kunt werkplek met Facebook om op te halen van de gegevens die nodig is om te autoriseren van gebruikers om toegang te krijgen, vóór u zich voor de eerste keer aanmeldt. Deze ook inricht ongedaan maken gebruikers vanuit werkplek door Facebook wanneer toegang is ingetrokken in Azure AD.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **Bedrijfsapps** > **alle toepassingen** sectie.

2. Als u werkplek door Facebook al hebt geconfigureerd voor eenmalige aanmelding, zoeken naar uw exemplaar van de werkplek door het zoekveld met Facebook. Selecteer anders **toevoegen** en zoek naar de **werkplek door Facebook** in de toepassingengalerie. Selecteer werkplek door Facebook in de resultaten voor zoeken en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van de werkplek op Facebook en selecteer vervolgens de **Provisioning** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, voer het toegangstoken van uw werkplek door Facebook-beheerder en stel de waarde van de Tenant-URL op `https://www.facebook.com/scim/v1/` . Deze [instructies](https://developers.facebook.com/docs/workplace/integrations/custom-integrations/apps) over het maken van een toegangstoken voor werkplek. 

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD kan verbinding maken met uw werkplek met Facebook-app. Als de verbinding is mislukt, controleert u of dat uw werkplek op Facebook-account Team beheerdersmachtigingen heeft.

7. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan een werkplek op Facebook.**

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die aan een werkplek door Facebook van Azure AD worden gesynchroniseerd. De kenmerken die zijn geselecteerd als **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in werkplek door Facebook voor update-bewerkingen. Selecteer de knop Opslaan om door te voeren van eventuele wijzigingen.

11. Om in te schakelen in de Azure AD-inrichtingsservice voor werkplek met Facebook, wijzigen de **Inrichtingsstatus** naar **op** in de **instellingen** sectie

12. Klik op **opslaan.**

Zie voor meer informatie over het configureren van automatische inrichting. [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

U kunt nu een testaccount maken. Wacht tot 20 minuten om te verifiëren dat het account is gesynchroniseerd met werkplek op Facebook.

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](workplacebyfacebook-tutorial.md)
