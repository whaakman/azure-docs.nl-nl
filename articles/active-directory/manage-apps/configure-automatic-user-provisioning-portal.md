---
title: Gebruikers inrichten voor zakelijke apps in de Azure Active Directory | Microsoft Docs
description: Informatie over het inrichten van gebruikersaccounts voor bedrijfs-apps met behulp van de Azure Active Directory beheren
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/01/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: b994078350aec5657659f8835d228eb907606bb8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807624"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Beheer van gebruikersaccount inrichten voor enterprise-apps in Azure portal

In dit artikel wordt beschreven hoe u de [Azure-portal](https://portal.azure.com) om automatisch gebruikers inrichten en ongedaan maken inrichting voor toepassingen die ondersteuning bieden voor deze te beheren. Zie voor meer informatie over het inrichten van automatische gebruikersaccounts en hoe het werkt, [automatiseren van Gebruikersinrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory](user-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Uw apps zoeken in de portal

Gebruik de Azure Active Directory-portal weergeven en beheren van alle toepassingen die zijn geconfigureerd voor eenmalige aanmelding in een map. Zakelijke apps zijn apps die zijn geïmplementeerd en gebruikt binnen uw organisatie. Volg deze stappen voor het weergeven en beheren van uw zakelijke toepassingen:

1. Open de [Azure Active Directory-portal](https://aad.portal.azure.com).
1. Selecteer **bedrijfstoepassingen** in het linkerdeelvenster. Een lijst met alle geconfigureerde apps wordt weergegeven, met inbegrip van apps die zijn toegevoegd vanuit de galerie.
1. Selecteer de gewenste app te laden van de resource in het deelvenster waar u kunt rapporten weergeven en beheren van app-instellingen.
1. Selecteer **Provisioning** voor het beheren van instellingen voor de geselecteerde app voor het inrichten gebruikersaccount.

   ![Scherm voor het beheren van instellingen voor het inrichten gebruikersaccount inrichten](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Modi inrichten

De **Provisioning** deelvenster begint met een **modus** menu, die de inrichting modi voor een zakelijke toepassing bevat, en kunt u ze configureren. De beschikbare opties zijn onder andere:

* **Automatische** -deze optie wordt weergegeven als Azure AD ondersteunt automatische op API gebaseerde inrichting ongedaan maken inrichting van gebruikersaccounts aan deze toepassing. Selecteer dit om een interface waarmee beheerders weer te geven:

  * Azure AD verbinding maken met de API voor gebruikersbeheer van de toepassing configureren
  * Accounttoewijzingen en werkstromen die bepalen hoe de gegevens van gebruikersaccounts moeten stromen tussen Azure AD maken en de app.
  * De Azure AD-inrichtingsservice beheren

* **Handmatige** -deze optie wordt weergegeven als Azure AD biedt geen ondersteuning voor automatische inrichting van gebruikersaccounts aan deze toepassing. Gebruikersaccount records die zijn opgeslagen in de toepassing moet in dit geval worden beheerd met behulp van een extern proces, op basis van de gebruiker-beheer en inrichting mogelijkheden zijn opgegeven door de toepassing (waaronder kunt inrichten van SAML JIT).

## <a name="configuring-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

Selecteer de **automatische** optie voor het opgeven van instellingen voor beheerdersreferenties, toewijzingen, starten en stoppen, en synchronisatie.

### <a name="admin-credentials"></a>Beheerdersreferenties

Vouw **beheerdersreferenties** in te voeren van de referenties die nodig zijn voor Azure AD verbinding maken met de API voor gebruikersbeheer van de toepassing. De invoer is vereist, is afhankelijk van de toepassing. Zie voor meer informatie over de referenties en de vereisten voor specifieke toepassingen, de [zelfstudie voor de configuratie voor de specifieke toepassing](user-provisioning.md).

Selecteer **verbinding testen** voor het testen van de referenties door Azure AD-poging verbinding maken met de app app met behulp van de opgegeven referenties de inrichting.

### <a name="mappings"></a>Toewijzingen

Vouw **toewijzingen** bekijken en bewerken van de kenmerken van de gebruiker die tussen Azure AD stromen en de doeltoepassing wanneer gebruikersaccounts worden ingericht of bijgewerkt.

Er is een vooraf geconfigureerde set toewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen of contactpersonen. Selecteer een toewijzing in de tabel om te openen van de editor voor kolomtoewijzing aan de rechterkant, waarin u kunt bekijken en pas ze aan.

![De toewijzing van het kenmerk wordt weergegeven](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Ondersteunde aanpassingen zijn onder andere:

* In- en uitschakelen van toewijzingen voor specifieke objecten, zoals de Azure AD-gebruiker-object aan van de SaaS-app-gebruikersobject.
* De kenmerken die worden overgebracht van de Azure AD-gebruiker-object naar het gebruikersobject van de app bewerken. Zie voor meer informatie over kenmerktoewijzing [kenmerk toewijzing typen](customize-application-attributes.md#understanding-attribute-mapping-types).
* Filteren van de inrichting acties die door Azure AD wordt uitgevoerd op de betreffende toepassing. In plaats van Azure AD volledig objecten synchroniseren, kunt u de acties uit te voeren beperken.

  Selecteer bijvoorbeeld alleen **Update** en Azure AD-alleen updates bestaande accounts in een toepassing, maar de gebruiker die de nieuwe niet maken. Selecteer alleen **maken** en Azure alleen nieuwe gebruikersaccounts maakt, maar bestaande klanten niet bijwerken. Deze functie kunnen beheerders verschillende toewijzingen voor het maken van accounts maken en bijwerken van werkstromen.

* Toevoegen van een nieuw kenmerk wordt toegewezen. Selecteer **nieuwe toewijzing toevoegen** aan de onderkant van de **kenmerk toewijzing** deelvenster. Vul de **kenmerk bewerken** vormen en selecteer **Ok** om toe te voegen van de nieuwe toewijzing aan de lijst.

### <a name="settings"></a>Instellingen

U kunt starten en stoppen van de Azure AD-inrichtingsservice voor de geselecteerde toepassing in de **instellingen** gebied van de **Provisioning** scherm. U kunt ook de inrichting cache wissen en de service opnieuw starten.

Als inrichting wordt ingeschakeld voor de eerste keer gebruikt voor een toepassing, schakelt u op de service door het veranderen van de **Inrichtingsstatus** naar **op**. Deze wijziging zorgt ervoor dat de Azure AD-inrichtingsservice om uit te voeren van een eerste synchronisatie. Het ingesteld op de gebruikers die zijn toegewezen de **gebruikers en groepen** sectie de doeltoepassing voor deze query en voert vervolgens de inrichting acties die zijn gedefinieerd in de Azure AD **toewijzingen** sectie. Tijdens dit proces slaat de provisioning-service gegevens over welke gebruikersaccounts die worden beheerd, in de cache zodat niet-beheerde accounts in de doeltoepassingen die nooit binnen het bereik van de toewijzing zijn, worden niet beïnvloed door het ongedaan maken inrichting bewerkingen. Na de initiële synchronisatie, worden de provisioning-service automatisch gebruikers- als groepsobjecten gesynchroniseerd op een interval van tien minuten.

Wijziging de **Inrichtingsstatus** naar **uit** onderbreken van de inrichtingsservice. In deze status niet Azure maken, bijwerken of verwijderen van een gebruiker of groepsobjecten worden weergegeven in de app. Wijzig de status weer **op** en de service neemt waar het afgebroken.

Selecteer de **de huidige status wissen en opnieuw starten van synchronisatie** selectievakje in en selecteer **opslaan** aan:

* De provisioning-service stoppen
* Dump van de gegevens in de cache over wat is het beheren van Azure AD-accounts
* Start de services opnieuw en voer de initiële synchronisatie opnieuw uit

Deze optie kunnen beheerders het inrichtingsproces voor de implementatie opnieuw starten.

### <a name="synchronization-details"></a>Synchronisatiedetails

Deze sectie bevat aanvullende informatie over de werking van de provisioning-service, met inbegrip van de eerste en laatste keer dat de provisioning-service wordt uitgevoerd op de toepassing en het aantal gebruikers en groepsobjecten die deze beheert.

Er wordt een koppeling gegeven naar de **inrichting activiteitenrapport**, dat biedt een logboek van alle gebruikers en groepen gemaakte, bijgewerkte en verwijderde tussen Azure AD en de doeltoepassing. Een koppeling is ook beschikbaar voor de **inrichting foutenrapport**, waarmee u meer gedetailleerde foutberichten worden weergegeven voor gebruiker en groepsobjecten die niet worden gelezen konden, gemaakt, bijgewerkt of verwijderd.
