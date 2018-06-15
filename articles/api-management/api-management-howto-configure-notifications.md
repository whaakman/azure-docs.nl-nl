---
title: Meldingen configureren en e-mailsjablonen in Azure API Management | Microsoft Docs
description: Informatie over het configureren van meldingen en e-mailsjablonen in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: apimpm
ms.openlocfilehash: 60788f76dac58ead10e43e892d587a86bdd3fcad
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33934281"
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Meldingen en e-mailsjablonen configureren in Azure API Management
API Management biedt de mogelijkheid voor het configureren van meldingen voor specifieke gebeurtenissen en de e-mailsjablonen die worden gebruikt om te communiceren met de beheerders en ontwikkelaars van exemplaar van API Management configureren. Dit artikel laat zien hoe u meldingen configureren voor de gebeurtenissen beschikbaar en biedt een overzicht van de configuratie van de e-mailsjablonen gebruikt deze gebeurtenissen.

## <a name="prerequisites"></a>Vereisten

Als u geen exemplaar van API Management-service, voert u de volgende Quick Start: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

## <a name="publisher-notifications"> </a>Meldingen configureren

1. Selecteer uw **API MANAGEMENT** exemplaar.
2. Klik op **meldingen** om de beschikbare meldingen weer te geven.

    ![Meldingen van de uitgever][api-management-publisher-notifications]

    De volgende lijst met gebeurtenissen kan worden geconfigureerd voor meldingen.

    * **Verzoeken om abonnementen (goedkeuring wordt vereist)** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailmeldingen over verzoeken om abonnementen voor API-producten goedkeuring wordt vereist.
    * **Nieuwe abonnementen** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailmeldingen over nieuwe abonnementen voor API-product.
    * **Galerie toepassingsaanvragen** -de opgegeven e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer nieuwe toepassingen worden verzonden naar de galerie met toepassingen.
    * **BCC** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailbericht blind kopie van alle e-mails worden verzonden voor ontwikkelaars.
    * **Nieuwe probleem of opmerking** : het opgegeven e-mailontvangers en gebruikers e-mailmeldingen wanneer een nieuw actie-item ontvangen of opmerking wordt ingediend op de portal voor ontwikkelaars.
    * **Account sluiten bericht** -de opgegeven e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer een account wordt gesloten.
    * **Bijna abonnement quotumlimiet** -de volgende e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer abonnement gebruik bijna gebruiksquotum opgehaald.

    Voor elke gebeurtenis, kunt u e-mailontvangers in het tekstvak voor e-mailadres opgeven of u gebruikers kunt kiezen uit een lijst.

3. Als u de e-mailadressen moeten ontvangen, moet u deze in het tekstvak e-mailadres invoeren. Als er meerdere e-mailadressen, scheiden met komma's.

    ![Geadresseerden voor meldingen][api-management-email-addresses]
4. Klik op **Toevoegen**.

## <a name="email-templates"> </a>Meldingssjablonen configureren
API Management biedt meldingssjablonen voor de e-mailberichten die worden verzonden in de loop van beheer en het gebruik van de service. De volgende e-mailsjablonen worden geleverd.

* Toepassing galerie inzending is goedgekeurd
* Ontwikkelaars afscheidstekst letter
* Ontwikkelaars quotalimiet bijna melding
* Gebruiker uitnodigen
* Nieuwe opmerking toegevoegd aan een probleem
* Nieuwe probleem ontvangen
* Nieuw abonnement geactiveerd
* Abonnement wordt verlengd bevestigen
* Abonnementaanvraag heeft geweigerd
* Abonnementaanvraag ontvangen

Deze sjablonen kunnen worden gewijzigd naar wens.

Als u wilt weergeven en configureren van de e-mailsjablonen voor uw API Management-exemplaar, klikt u op **meldingen sjablonen**.

![E-mailsjablonen][api-management-email-templates]

Elk e-mailsjabloon heeft een onderwerp als tekst zonder opmaak en de definitie van een instantie in HTML-indeling. Elk item kan worden aangepast naar wens.

![Editor voor e-sjabloon][api-management-email-template]

De **Parameters** lijst bevat een lijst met parameters, die tijdens ingevoegd in het onderwerp of de hoofdtekst, worden de opgegeven waarde wordt vervangen als het e-mailbericht wordt verzonden. Als u wilt invoegen een parameter, plaats de cursor waar u wilt dat de parameter om te gaan en klik op de pijl naar links van de parameternaam van de.

> [!NOTE] 
> De parameters worden niet vervangen door feitelijke waarden wanneer een voorbeeldweergave of verzenden van een test.

Sla de wijzigingen in het e-mailsjabloon, klikt u op **opslaan**, of het wijzigingen klikt u op Annuleren **negeren**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
