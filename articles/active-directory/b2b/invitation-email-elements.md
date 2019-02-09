---
title: De elementen van de B2B-samenwerking uitnodigingse-mail - Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B-samenwerking uitnodiging e-mailsjabloon
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: a119e2797c4771551b6087e148d609c68ffd36db
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977505"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>De elementen van de B2B-samenwerking uitnodigingse-mail - Azure Active Directory

Uitnodigingen per e-mail zijn een essentieel onderdeel om partners aan boord als gebruikers van B2B-samenwerking in Azure AD. U kunt ze gebruiken te verhogen van de vertrouwensrelatie van de ontvanger. u kunt geldigheid toevoegen en het e-mailbericht, om te controleren of de ontvanger het sociale bewijs lijkt vertrouwd bent met het selecteren van de **aan de slag** knop de uitnodiging te accepteren. Deze vertrouwensrelatie is dat een sleutel betekent dat u wilt delen wrijving verminderen. En u ook wilt maken van het e-mailbericht er fantastisch uitzien.

![Azure AD B2b uitnodigingse-mail](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>Het e-mailbericht waarin wordt uitgelegd
We kijken naar enkele elementen van het e-mailbericht, zodat u hoe het beste weet om hun mogelijkheden te gebruiken.

### <a name="subject"></a>Onderwerp
Het onderwerp van het e-mailbericht heeft het volgende patroon: U bent uitgenodigd voor de &lt;tenantname&gt; organisatie

### <a name="from-address"></a>Van adres
We gebruiken een LinkedIn-patroon voor het adres van de afzender.  U moet wissen die de afzender van de uitnodiging is en waaruit de bedrijfsportal, en ook verduidelijken dat de e-mail afkomstig is van een Microsoft e-mailadres. De indeling is: &lt;Weergavenaam van de afzender van de uitnodiging&gt; van &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com>

### <a name="reply-to"></a>Antwoorden op
Het antwoordadres e-mailbericht is ingesteld op van de afzender van de uitnodiging e indien beschikbaar, zodat u het e-mailbericht beantwoordt een e-mailbericht naar de afzender van de uitnodiging verzendt.

### <a name="branding"></a>Huisstijl
De uitnodigingen per e-mail van uw gebruik van de tenant de huisstijl die u mogelijk hebt ingesteld voor uw tenant. Als u wilt profiteren van deze mogelijkheid [hier](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) vindt u de details over het configureren van deze. Het bannerlogo wordt weergegeven in het e-mailbericht. Voer de grootte van de installatiekopie en kwaliteit instructies [hier](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) voor de beste resultaten. Bovendien de bedrijfsnaam wordt ook weergegeven in de aanbieding.

### <a name="call-to-action"></a>Aanzetten tot actie
De aanroep van actie bestaat uit twee delen: waarin wordt uitgelegd waarom de ontvanger het e-mailbericht heeft ontvangen en wat de ontvanger is verzocht te doen.
- Zie de sectie 'Waarom' kan worden aangepakt met behulp van het volgende patroon: U bent uitgenodigd voor toegang tot toepassingen in de &lt;tenantname&gt; organisatie

- En de 'wat u wordt gevraagd om te doen"sectie wordt aangegeven door de aanwezigheid van de **aan de slag** knop. Als de ontvanger is toegevoegd zonder de noodzaak voor uitnodigingen, wordt niet deze knop weergegeven.

### <a name="inviters-information"></a>Afzender van de uitnodiging van informatie
De weergavenaam van de afzender van de uitnodiging is opgenomen in het e-mailbericht. En verder als u een profielfoto voor uw Azure AD-account hebt ingesteld, nodigen e-mailbericht bevat ook afbeelding. Beide zijn bedoeld om vergroot het vertrouwen van de ontvanger in het e-mailbericht.

Als u uw eigen profielfoto nog hebt ingesteld, ziet u een pictogram met de initialen van de afzender van de uitnodiging in plaats van de afbeelding:

  ![Initialen van de afzender van de uitnodiging weergeven](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Hoofdtekst
De hoofdtekst bevat het bericht dat de afzender van de uitnodiging stelt wanneer bericht [uitnodigen van een gastgebruiker naar de directory, groep of app](add-users-administrator.md) of [met behulp van de uitnodiging API](customize-invitation-api.md). Dit is een tekstgebied zodat HTML-codes uit veiligheidsoverwegingen worden niet verwerkt.

### <a name="footer-section"></a>Voettekst
De voettekst bevat de huisstijl van het Microsoft-bedrijf en kan de geadresseerde weten als het e-mailbericht is verzonden vanaf een niet-gecontroleerde alias. Speciale gevallen:

- De afzender van de uitnodiging geen een e-mailadres in de uitnodigende tenancy

  ![afbeelding van afzender van de uitnodiging geen een e-mailadres in de uitnodigende tenancy](media/invitation-email-elements/inviter-no-email.png)


- De ontvanger nodig niet om in te wisselen van de uitnodiging

  ![Wanneer de ontvanger hoeft niet te uitnodiging inwisselen](media/invitation-email-elements/when-recipient-doesnt-redeem.png)

## <a name="how-the-language-is-determined"></a>Hoe de taal die wordt bepaald
De taal die wordt gepresenteerd aan de gastgebruiker in de uitnodiging per e-mail wordt bepaald door de volgende instellingen. Deze instellingen worden weergegeven in de volgorde van prioriteit. Als een instelling niet is geconfigureerd, bepaalt de volgende instelling in de lijst de taal. 
- De **messageLanguage** eigenschap van de [invitedUserMessageInfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0) object als de uitnodiging maken API wordt gebruikt
-   De **preferredLanguage** eigenschap die is opgegeven in van de Gast [gebruikersobject](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0)
-   De **taal van meldingen** instellen in de eigenschappen van de gastgebruiker de starttenant (voor alleen voor Azure AD-tenants)
-   De **taal van meldingen** instellen in de eigenschappen van de resource-tenant

Als geen van deze instellingen zijn geconfigureerd, standaard de taal Engels (V.S.).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen op Azure AD B2B-samenwerking:

- [Wat is Azure AD B2B-samenwerking](what-is-b2b.md)
- [Hoe voeg beheerders van Azure Active Directory gebruikers van B2B-samenwerking toe?](add-users-administrator.md)
- [Hoe voeg informatiemedewerkers gebruikers van B2B-samenwerking toe?](add-users-information-worker.md)
- [B2B-samenwerking uitnodiging inwisselen](redemption-experience.md)
- [Gebruikers van B2B-samenwerking zonder uitnodiging toevoegen](add-user-without-invite.md)
