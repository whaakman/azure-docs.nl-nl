---
title: Configureren van meldingen en e-mailsjablonen in Azure API Management | Microsoft Docs
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
ms.openlocfilehash: 2a959c9d131c6aa0bdc99450cf2b6f09a5d8bfa7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58102346"
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Meldingen en e-mailsjablonen configureren in Azure API Management
API Management biedt de mogelijkheid om meldingen voor specifieke gebeurtenissen te configureren en het configureren van het e-mailsjablonen die worden gebruikt om te communiceren met de beheerders en ontwikkelaars van exemplaar van API Management. Dit artikel laat zien hoe u meldingen voor de beschikbare gebeurtenissen configureren en biedt een overzicht van het configureren van het e-mailsjablonen gebruikt deze gebeurtenissen.

## <a name="prerequisites"></a>Vereisten

Als u geen exemplaar van API Management-service, voert u de volgende Quick Start: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="publisher-notifications"> </a>Meldingen configureren

1. Selecteer uw **API MANAGEMENT** exemplaar.
2. Klik op **meldingen** om de beschikbare meldingen weer te geven.

    ![Meldingen van de uitgever][api-management-publisher-notifications]

    De volgende lijst met gebeurtenissen kan worden geconfigureerd voor meldingen.

   * **Verzoeken om abonnementen (moeten worden goedgekeurd)** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailmeldingen over verzoeken om abonnementen voor API-producten die moeten worden goedgekeurd.
   * **Nieuwe abonnementen** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailmeldingen over nieuwe abonnementen voor API-product.
   * **Galerie toepassingsaanvragen** -de opgegeven e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer nieuwe toepassingen worden verzonden naar de galerie.
   * **BCC** -ontvangt de opgegeven e-mailontvangers en gebruikers e-mailbericht blind kopie van alle e-mailberichten verzonden voor ontwikkelaars.
   * **Nieuw probleem of opmerking** : de opgegeven e-mailontvangers en gebruikers e-mailmeldingen wanneer een nieuw probleem wordt ontvangen of opmerking wordt ingediend op de portal voor ontwikkelaars.
   * **Account sluiten bericht** -de opgegeven e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer een account is gesloten.
   * **Bijna quotumlimiet voor uw abonnement** -de volgende e-mailontvangers en gebruikers worden e-mailmeldingen ontvangen wanneer abonnementsgebruik dicht bij gebruiksquotum opgehaald.

     Voor elke gebeurtenis, kunt u e-mailontvangers in het tekstvak voor e-mailadres of kunt u gebruikers in een lijst selecteren.

3. Als u de e-mailadressen op de hoogte worden gebracht, moet u deze in het tekstvak e-mailadres invoeren. Hebt u meerdere e-mailadressen scheiden met komma's.

    ![Geadresseerden voor meldingen][api-management-email-addresses]
4. Klik op **Toevoegen**.

## <a name="email-templates"> </a>Meldingssjablonen configureren
API Management biedt meldingssjablonen voor de e-mailberichten die worden verzonden bij het beheren van en het gebruik van de service. De volgende e-mailsjablonen worden geleverd.

* De galerie verzending is goedgekeurd
* Developer afscheidstekst letter
* Developer-quotumlimiet melding nadert
* Gebruiker uitnodigen
* Nieuwe opmerking toegevoegd aan een probleem
* Nieuw probleem ontvangen
* Nieuw abonnement zijn geactiveerd
* Bevestiging van het abonnement vernieuwd
* Abonnementsaanvraag afwijst
* Abonnementsaanvraag ontvangen

Deze sjablonen kunnen worden gewijzigd naar wens.

Als u wilt weergeven en configureren van het e-mailsjablonen voor uw exemplaar van API Management, klikt u op **meldingen sjablonen**.

![E-mailsjablonen][api-management-email-templates]

Elk e-mailsjabloon heeft een onderwerp in tekst zonder opmaak, en de definitie van een instantie in HTML-indeling. Elk item kan worden aangepast naar wens.

![Editor voor e-sjabloon][api-management-email-template]

De **Parameters** lijst bevat een lijst met parameters, die bij in het onderwerp of de hoofdtekst ingevoegd, is de opgegeven waarde vervangen als het e-mailbericht wordt verzonden. Plaats de cursor waar u de parameter aan de slag wilt voor het invoegen van een parameter, en klik op de pijl links van de naam van de parameter.

> [!NOTE] 
> De parameters worden niet vervangen door de werkelijke waarden wanneer op voorhand of het verzenden van een test.

Als u wilt de wijzigingen in de e-mailsjabloon opslaan, klikt u op **opslaan**, of klik op wijzigingen annuleren **negeren**.
 

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
