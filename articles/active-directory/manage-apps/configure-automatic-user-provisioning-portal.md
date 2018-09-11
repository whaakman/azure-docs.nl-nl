---
title: Gebruikers inrichten voor zakelijke apps in de Azure Active Directory | Microsoft Docs
description: Informatie over het inrichten van gebruikersaccounts voor bedrijfs-apps met behulp van de Azure Active Directory beheren
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 95d2f65e99b37b38f99ec5a750d74828661fe7ee
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44349140"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Beheer van gebruikersaccount inrichten voor enterprise-apps in Azure portal
In dit artikel wordt beschreven hoe u de [Azure-portal](https://portal.azure.com) om automatisch gebruikers inrichten en ongedaan maken inrichting voor toepassingen die ondersteuning bieden voor deze, met name zijn die zijn toegevoegd in de categorie 'aanbevolen' van de tebeheren[ Azure Active Directory-toepassingsgalerie](what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery). Zie voor meer informatie over het inrichten van automatische gebruikersaccounts en hoe het werkt, [automatiseren van Gebruikersinrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory](user-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Uw apps zoeken in de portal
Alle toepassingen die zijn geconfigureerd voor eenmalige aanmelding in een map door de beheerder van een directory met behulp van de [Azure Active Directory-toepassingsgalerie](what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery), kan worden bekeken en beheerd in de [Azure-portal](https://portal.azure.com). De toepassingen kunnen u vinden in de **alle Services** &gt; **bedrijfstoepassingen** sectie van de portal. Zakelijke apps zijn apps die zijn geïmplementeerd en gebruikt binnen uw organisatie.

![Deelvenster voor zakelijke toepassingen](./media/configure-automatic-user-provisioning-portal/enterprise-apps-pane.png)

Selecteren van de **alle toepassingen** koppelen aan de linkerkant ziet u een lijst met alle apps die zijn geconfigureerd, met inbegrip van apps die in de galerie is toegevoegd. Selecteren van een app, laadt de resource-deelvenster voor die app, waar rapporten kunnen worden weergegeven voor die app en een aantal instellingen kan worden beheerd.

Instellingen voor het inrichten gebruikersaccount kan worden beheerd door te selecteren **Provisioning** aan de linkerkant.

![Deelvenster van de resource Application](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Modi inrichten
De **Provisioning** deelvenster begint met een **modus** in het menu dat laat zien welke inrichting modi worden ondersteund voor een bedrijfstoepassing, en kan ze worden geconfigureerd. De beschikbare opties zijn onder andere:

* **Automatische** -deze optie wordt weergegeven als Azure AD biedt ondersteuning voor automatische inrichting op basis van API en/of ongedaan maken inrichting van gebruikersaccounts aan deze toepassing. In deze modus te selecteren wordt weergegeven een interface waarmee beheerders via Azure AD verbinding maken met de API voor gebruikersbeheer van de toepassing configureren, het maken van Accounttoewijzingen en werkstromen die bepalen hoe de gegevens van gebruikersaccounts moeten stromen tussen Azure AD en de App en de Azure AD-inrichtingsservice beheren.
* **Handmatige** -deze optie wordt weergegeven als Azure AD biedt geen ondersteuning voor automatische inrichting van gebruikersaccounts aan deze toepassing. Deze optie betekent dat de gebruiker accountrecords die zijn opgeslagen in de toepassing moeten worden beheerd met behulp van een extern proces, op basis van de gebruiker-beheer en inrichting mogelijkheden zijn opgegeven door de toepassing (waaronder kunt inrichten van SAML JIT).

## <a name="configuring-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren
Selecteren van de **automatische** optie wordt een scherm dat is onderverdeeld in vier secties:

### <a name="admin-credentials"></a>Referenties voor beheerder
In dit gedeelte is waar de referenties vereist zijn voor Azure AD verbinding maken met het beheer van gebruikers van de toepassing API zijn ingevoerd. De invoer is vereist, is afhankelijk van de toepassing. Zie voor meer informatie over de referenties en de vereisten voor specifieke toepassingen, de [zelfstudie voor de configuratie voor de specifieke toepassing](user-provisioning.md).

Selecteren van de **verbinding testen** knop kunt u voor het testen van de referenties door Azure AD-poging verbinding maken met de app de app met behulp van de opgegeven referenties inrichten.

### <a name="mappings"></a>Toewijzingen
In dit gedeelte is waar beheerders kunt weergeven en bewerken welke kenmerken gebruikersstroom tussen Azure AD en de doeltoepassing wanneer gebruikersaccounts worden ingericht of bijgewerkt.

Er is een vooraf geconfigureerde set toewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen of contactpersonen. Selecteer een van deze toewijzingen in de tabel ziet u de editor voor kolomtoewijzing aan de rechterkant, waar ze kunnen worden bekeken en aangepast.

![Deelvenster van de resource Application](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Ondersteunde aanpassingen zijn onder andere:

* In- en uitschakelen van toewijzingen voor specifieke objecten, zoals de Azure AD-gebruiker-object aan van de SaaS-app-gebruikersobject.
* De kenmerken die worden overgebracht van de Azure AD-gebruiker-object naar het gebruikersobject van de app bewerken. Zie voor meer informatie over kenmerktoewijzing [kenmerk toewijzing typen](customize-application-attributes.md#understanding-attribute-mapping-types).
* Filter de inrichting acties die door Azure AD wordt uitgevoerd op de betreffende toepassing. In plaats van Azure AD volledig objecten synchroniseren, kunt u de acties die worden uitgevoerd te beperken. Bijvoorbeeld, door het selecteren van **Update**, Azure AD-alleen updates bestaande gebruikers-accounts in een toepassing en maakt geen nieuwe labels. Door het selecteren van **maken**, Azure alleen nieuwe gebruikersaccounts maakt, maar wordt de bestaande apparaten niet bijgewerkt. Deze functie kan beheerders verschillende toewijzingen voor het maken van accounts maken en bijwerken van werkstromen.

### <a name="settings"></a>Instellingen
In deze sectie kan beheerders starten en stoppen van de Azure AD-inrichtingsservice voor de geselecteerde toepassing, evenals eventueel Wis de cache van de inrichting en start de service opnieuw.

Als inrichting wordt ingeschakeld voor de eerste keer gebruikt voor een toepassing, schakelt u op de service door het veranderen van de **Inrichtingsstatus** naar **op**. Deze wijziging zorgt ervoor dat de Azure AD-inrichtingsservice om uit te voeren van een eerste synchronisatie, waarbij het ingesteld op de gebruikers die zijn toegewezen de **gebruikers en groepen** sectie, de doeltoepassing voor deze query en voert vervolgens de inrichting acties gedefinieerd in de Azure AD **toewijzingen** sectie. Tijdens dit proces slaat de provisioning-service gegevens over welke gebruikersaccounts die worden beheerd, in de cache zodat niet-beheerde accounts in de doeltoepassingen die nooit binnen het bereik van de toewijzing zijn, worden niet beïnvloed door het ongedaan maken inrichting bewerkingen. Na de initiële synchronisatie, worden de provisioning-service automatisch gebruikers- als groepsobjecten gesynchroniseerd op een interval van tien minuten.

Wijzigen van de **Inrichtingsstatus** naar **uit** gewoon pauzeert de provisioning-service. In deze status kunnen Azure niet maken, bijwerken of verwijderen van een gebruiker of groepsobjecten worden weergegeven in de app. Wijzigen van de status aan op zorgt ervoor dat de service om op te halen waar het afgebroken.

Selecteren van de **de huidige status wissen en opnieuw starten van synchronisatie** selectievakje en slaan stopt de provisioning-service, dumpbestanden voor foutopsporing gegevens over welke Azure AD-accounts in de cache wordt beheerd, de services opnieuw gestart en voert de eerste synchronisatie opnieuw uit. Deze optie kan beheerders het inrichtingsproces voor de implementatie opnieuw starten.

### <a name="synchronization-details"></a>Synchronisatiedetails
Deze sectie bevat aanvullende informatie over de werking van de provisioning-service, met inbegrip van de eerste en laatste keer dat de provisioning-service wordt uitgevoerd op de toepassing en het aantal gebruikers en groepsobjecten worden beheerd.

Vindt u koppelingen naar de **activiteitenrapport inrichting** die zorgt voor een logboek van alle gebruikers en groepen die zijn gemaakt, bijgewerkt en verwijderd tussen Azure AD en de doeltoepassing en zo de **foutenrapportinrichten** die voorziet in gedetailleerde foutberichten worden weergegeven voor gebruikers en groepen objecten die niet konden worden gelezen, gemaakt, bijgewerkt of verwijderd. 

## <a name="feedback"></a>Feedback

Houd uw feedback! Plaats uw feedback en ideeën voor verbetering van de **-beheerportal** sectie van onze [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Het technische team is enthousiast over het bouwen van coole nieuwe dingen elke dag, en ze gebruiken de informatie voor de shape en definiëren wat moet worden hierna bouwen.

