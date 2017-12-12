---
title: 'Zelfstudie: Werkplek door Facebook configureren voor gebruikers inrichten | Microsoft Docs'
description: Informatie over het automatisch inrichten en de gebruikersaccounts van Azure AD aan werkplek door Facebook ongedaan in te richten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 77f5f33044b1915fbda7b86c6b07882c0e1e7554
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Zelfstudie: Werkplek door Facebook configureren voor gebruikers inrichten

Deze zelfstudie ziet u de stappen nodig zijn voor automatisch leveren en intrekken gebruikersaccounts van Azure Active Directory (Azure AD) aan een werkplek met Facebook.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkplek door Facebook, moet u het volgende:

- Een Azure AD-abonnement
- Een werkplek door Facebook eenmalige aanmelding (SSO) abonnement ingeschakeld

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een [één maand proefabonnement](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-workplace-by-facebook"></a>Gebruikers toewijzen aan de werkplek door Facebook

Azure AD gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die zijn toegewezen aan een toepassing in Azure AD gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, bepalen welke gebruikers en groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw werkplek door Facebook-app. U kunt vervolgens deze gebruikers toewijzen aan uw werkplek door Facebook-app door de instructies in [een gebruiker of groep toewijzen aan een app enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   De configuratie van de inrichting testen door het toewijzen van één Azure AD-gebruiker aan een werkplek met Facebook. Later extra gebruikers en groepen toewijzen.
>*   Wanneer u een gebruiker aan een werkplek door Facebook toewijzen, moet u een geldige gebruikersrol selecteren. De functie standaardtoegang werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie helpt u bij uw Azure AD verbinden met het gebruikersaccount inrichten API werkplek met Facebook. U leert ook hoe u de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in werkplek door Facebook configureren. Dit is gebaseerd op de gebruiker en groepstoewijzing in Azure AD.

>[!Tip]
>U kunt ook ingeschakeld SAML gebaseerde SSO voor werkplek door Facebook, door de instructies die is opgegeven in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies elkaar aanvullen.

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Gebruikersaccount inrichten werkplek door Facebook in Azure AD configureren

Azure AD biedt ondersteuning voor de mogelijkheid om automatisch te synchroniseren de accountdetails van toegewezen gebruikers aan een werkplek met Facebook. Deze automatische synchronisatie kunt werkplek met Facebook om op te halen van de vereiste gegevens voor het autoriseren van gebruikers om toegang te krijgen, vóór u wilt zich aanmelden voor de eerste keer. Het ook inricht ongedaan gebruikers van werkplek door Facebook wanneer toegang in Azure AD is ingetrokken.

1. In de [Azure-portal](https://portal.azure.com), selecteer **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen**.

2. Als u werkplek door Facebook al hebt geconfigureerd voor eenmalige aanmelding, kunt u zoeken naar uw werkplek door Facebook-exemplaar met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **werkplek door Facebook** in de galerie met toepassingen. Selecteer **werkplek door Facebook** uit de lijst met zoekresultaten en toe te voegen aan de lijst met toepassingen.

3. Selecteer uw exemplaar van de werkplek door Facebook, en selecteer vervolgens de **inrichten** tabblad.

4. Stel **Inrichtingsmodus** naar **automatische**. 

    ![Schermopname van werkplek door Facebook opties voor apparaatinrichting](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, voert u de **geheim Token** en de **Tenant-URL** van uw werkplek door Facebook-beheerder.

6. Selecteer in de Azure-portal **testverbinding** om te controleren of Azure AD verbinding kunt maken met uw werkplek met Facebook-app. Als de verbinding is mislukt, zorg ervoor dat uw werkplek door Facebook-account Team beheerdersmachtigingen.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

8. Selecteer **Opslaan**.

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan een werkplek door Facebook**.

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD aan werkplek Facebook. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen worden gebruikt voor het overeenkomen met de gebruikersaccounts in werkplek door Facebook voor update-bewerkingen. Selecteer om eventuele wijzigingen doorvoert, **opslaan**.

11. Inschakelen van de Azure AD-service voor werkplek door Facebook, inrichten in de **instellingen** sectie, wijzigt u de **inrichting Status** naar **op**.

12. Selecteer **Opslaan**.

Zie voor meer informatie over het configureren van automatische inrichting [de Facebook-documentatie](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd met werkplek met Facebook.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-facebook-at-work-tutorial.md)

