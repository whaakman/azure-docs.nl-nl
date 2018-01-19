---
title: 'Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werkplek met Facebook.
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
ms.date: 01/17/2018
ms.author: jeedes
ms.openlocfilehash: 5f1ca40b5ca490644a3af2c9d727026c1a18a2eb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a>Zelfstudie: Werkplek door Facebook configureren voor gebruikers inrichten

Het doel van deze zelfstudie is zodat u de stappen die u moet uitvoeren op de werkplek door Facebook en Azure AD automatisch inrichten en inrichten van gebruikersaccounts vanuit Azure AD aan werkplek door Facebook ongedaan.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkplek door Facebook, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkplek door Facebook-eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-workplace-by-facebook"></a>Gebruikers toewijzen aan een werkplek door Facebook

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw werkplek door Facebook-app. Als besloten, kunt u deze gebruikers toewijzen aan uw werkplek met Facebook-app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a>Belangrijke tips voor het toewijzen van gebruikers aan de werkplek door Facebook

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan een werkplek door Facebook voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker toewijzen aan een werkplek door Facebook, moet u een geldige gebruikersrol selecteren. De rol 'Default toegang' werkt niet voor het inrichten.

## <a name="enable-user-provisioning"></a>Gebruikersinrichting inschakelen

Deze sectie helpt u bij uw Azure AD verbinden met werkplek door Facebook van gebruikersaccount API-inrichting, en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in werkplek door Facebook op basis van gebruikers en groepen toewijzing in Azure AD.

>[!Tip]
>U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor werkplek door Facebook, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Voor het configureren van account gebruikersaanvragen werkplek door Facebook in Azure AD:

Het doel van deze sectie is het overzicht van Active Directory-gebruikersaccounts aan werkplek door Facebook-inrichting inschakelen.

Azure AD biedt ondersteuning voor de mogelijkheid om automatisch te synchroniseren de accountdetails van toegewezen gebruikers aan een werkplek met Facebook. Deze automatische synchronisatie kunt werkplek met Facebook om op te halen van de vereiste gegevens voor het autoriseren van gebruikers om toegang te krijgen, vóór u wilt zich aanmelden voor de eerste keer. Het ook inricht ongedaan gebruikers van werkplek door Facebook wanneer toegang in Azure AD is ingetrokken.

1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen** sectie.

2. Als u werkplek door Facebook al hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van de werkplek door het zoekveld met Facebook. Selecteer anders **toevoegen** en zoek naar **werkplek door Facebook** in de galerie met toepassingen. Werkplek door Facebook selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw werkplek door Facebook-exemplaar en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**. 

    ![inrichten](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Onder de **beheerdersreferenties** sectie, voer het geheim-Token van uw werkplek door Facebook-beheerder en stel de waarde van de Tenant-URL op `https://www.facebook.com/scim/v1/` .

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD verbinding kunt maken met uw werkplek met Facebook-app. Als de verbinding is mislukt, zorg ervoor dat uw werkplek door Facebook-account Team beheerdersmachtigingen heeft.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje in.

8. Klik op **opslaan.**

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory: gebruikers aan een werkplek met Facebook.**

10. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD aan werkplek Facebook. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen worden gebruikt voor het overeenkomen met de gebruikersaccounts in werkplek door Facebook voor update-bewerkingen. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD-service voor werkplek door Facebook inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12. Klik op **opslaan.**

Zie voor meer informatie over het configureren van automatische inrichting [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

U kunt nu een testaccount maken. Wacht 20 minuten duren om te verifiëren dat het account is gesynchroniseerd met werkplek met Facebook.

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Eenmalige aanmelding configureren](active-directory-saas-workplacebyfacebook-tutorial.md)
