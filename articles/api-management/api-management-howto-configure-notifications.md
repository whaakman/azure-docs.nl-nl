---
title: Meldingen configureren en e-mailsjablonen in Azure API Management | Microsoft Docs
description: Informatie over het configureren van meldingen en e-mailsjablonen in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: ee25f26d-4752-433b-af9c-3817db38aed5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 3d8b74e32059cfc1a4c3a8fc7d3bd04676ee80c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Meldingen en e-mailsjablonen configureren in Azure API Management
API Management biedt de mogelijkheid voor het configureren van meldingen voor specifieke gebeurtenissen en de e-mailsjablonen die worden gebruikt om te communiceren met de beheerders en ontwikkelaars van exemplaar van API Management configureren. Dit onderwerp wordt beschreven hoe u meldingen configureren voor de gebeurtenissen beschikbaar en biedt een overzicht van de configuratie van de e-mailsjablonen gebruikt deze gebeurtenissen.

## <a name="publisher-notifications"></a>Publisher meldingen configureren
Meldingen configureren, klikt u op **publicatieportal** in de Azure-Portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management.

![Publicatieportal][api-management-management-console]

> [!NOTE] 
> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].

Klik op **meldingen** van de **API Management** menu aan de linkerkant om de beschikbare meldingen weer te geven.

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

Als u de e-mailadressen moeten ontvangen, moet u deze in het tekstvak e-mailadres invoeren. Als er meerdere e-mailadressen, scheiden met komma's.

![Geadresseerden voor meldingen][api-management-email-addresses]

Geef de gebruikers om te worden geïnformeerd, klikt u op **ontvanger toevoegen**, schakel het selectievakje in naast de gebruikers een melding en klikt u op **OK**.

> [!NOTE] 
> Alleen beheerders worden weergegeven in de lijst.


Klik na het configureren van de ontvangers van meldingen op **opslaan** om toe te passen de bijgewerkte melding geadresseerden.

> [!NOTE] 
> Als u de navigatiefunctie weg van de **Publisher meldingen** tabblad de publicatieportal waarschuwt u als er niet-opgeslagen wijzigingen.


## <a name="email-templates"></a>E-mailsjablonen configureren
E-mailsjablonen biedt API Management voor de e-mailberichten die worden verzonden in de loop van beheer en het gebruik van de service. De volgende e-mailsjablonen worden geleverd.

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

Als u wilt weergeven en configureren van de e-mailsjablonen voor uw API Management-exemplaar, klikt u op **meldingen** van de **API Management** menu aan de linkerkant en selecteer de **e-mailsjablonen** tabblad.

![E-mailsjablonen][api-management-email-templates]

Als u wilt bekijken of wijzigen van een specifieke sjabloon, selecteert u dit in de **sjablonen** vervolgkeuzelijst.

![E-Sjabloonlijst][api-management-email-templates-list]

Elk e-mailsjabloon heeft een onderwerp als tekst zonder opmaak en de definitie van een instantie in HTML-indeling. Elk item kan worden aangepast naar wens.

![Editor voor e-sjabloon][api-management-email-template]

De **Parameters** lijst bevat een lijst met parameters, die tijdens ingevoegd in het onderwerp of de hoofdtekst, worden de opgegeven waarde wordt vervangen als het e-mailbericht wordt verzonden. Als u wilt invoegen een parameter, plaats de cursor waar u wilt dat de parameter om te gaan en klik op de pijl naar links van de parameternaam van de.

Klik op **Preview** of **een test verzenden** om te zien hoe het e-mailbericht wordt zoeken of een testbericht verzenden.

> [!NOTE] 
> De parameters worden niet vervangen door feitelijke waarden wanneer een voorbeeldweergave of verzenden van een test.

Sla de wijzigingen in het e-mailsjabloon, klikt u op **opslaan**, of het wijzigingen klikt u op Annuleren **annuleren**.
 

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

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
