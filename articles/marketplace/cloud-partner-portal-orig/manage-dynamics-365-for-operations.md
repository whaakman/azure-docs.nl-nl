---
title: Over het maken van Dynamics 365 voor bewerkingen via Cloud Partner-portal bieden | Microsoft Docs
description: Over het maken van Dynamics 365 voor bewerkingen via Cloud Partner-portal bieden
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 93c70e038589667ae97acb86663a6179dcc81637
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57877323"
---
# <a name="how-to-create-dynamics-365-for-operations-offer-via-cloud-partner-portal"></a>Over het maken van Dynamics 365 voor bewerkingen via Cloud Partner-portal bieden

Portal voor publiceren biedt op basis van rollen toegang tot de portal zodat meerdere personen kunnen samenwerken voor het publiceren van een aanbieding. Zie [Cloud Portal beheren-gebruikers](./cloud-partner-portal-manage-users.md) voor meer informatie.

Voordat een aanbieding kan worden gepubliceerd namens een publisher-account, een van de personen met \"eigenaar\" rol moet instemmen met de [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/), [privacyverklaring van Microsoft](https://www.microsoft.com/privacystatement/default.aspx), en [Microsoft Azure Certified-Programmaovereenkomst](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="how-to-create-a-new-dynamics-365-for-operations-offer"></a>Over het maken van een nieuwe Dynamics 365 voor bewerkingen bieden

Als alle vereisten wordt voldaan, bent u klaar om te beginnen met het ontwerpen van uw Dynamics 365 for Operations-aanbieding.

1. Aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Klik in de linker navigatiebalk op \"+ nieuwe aanbieding\" en selecteer \"Dynamics 365 voor bewerkingen\".
3. Een nieuwe aanbieding \"Editor\" weergave wordt nu voor u geopend en we zijn klaar om door te gaan ontwerpen.
4. De \"formulieren\" die moeten worden ingevuld zijn zichtbaar aan de linkerkant binnen de \"Editor\" weergeven. Elke \"formulier\" bestaat uit een set velden die moeten worden ingevuld. Vereiste velden zijn gemarkeerd met een rood sterretje (\*).

Er zijn vier belangrijkste formulieren voor het ontwerpen van een Dynamics 365 for Operations-aanbieding:

- Aanbiedingsinstellingen
- Technische informatie
- StoreFront Details
- Contactpersonen

## <a name="how-to-fill-out-the-offer-settings-form"></a>Het invullen van het formulier instellingen bieden

Het formulier van de instellingen voor aanbieding is een eenvoudige methode om de instellingen van de aanbieding te geven. De verschillende velden worden hieronder beschreven.

### <a name="offer-id"></a>Aanbiedings-id

Dit is een unieke id voor de aanbieding binnen een publisher-profiel. Deze ID zijn in URL's voor product zichtbaar. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. Dit veld is vergrendeld zodra een aanbieding live meteen.

bijvoorbeeld, als een publisher contoso publisher een aanbieding met maakt aanbiedings-ID *voorbeeld-dynamics365 voor bewerkingen*, wordt het weergegeven in AppSource als `https://appsource.microsoft.com/marketplace/apps/**contoso**.*sample-dynamics365 for operations*?tab=Overview\`.

### <a name="publisher-id"></a>Uitgevers-ID

Deze vervolgkeuzelijst kunt u de uitgeverprofiel dat u wilt publiceren, deze aanbieding onder kiezen. Dit veld is vergrendeld zodra een aanbieding live meteen.

Name

Dit is de weergavenaam voor uw aanbieding. Dit is de naam die wordt weergegeven in de [AppSource](https://appsource.microsoft.com). De naam mag maximaal 50 tekens bevatten.

Klik op \"opslaan\" uw voortgang opslaan. Volgende stap is om technische informatie voor uw aanbod in te vullen.

## <a name="how-to-fill-out-the-technical-info-form"></a>Vul het formulier technische informatie over hoe u

Het formulier technische info bevat informatie die wordt weergegeven in de pagina van uw aanbieding. Instructies voor de verschillende velden worden hieronder beschreven.

![Venster voor de nieuwe aanbieding](./media/publish_d365_new_offer/Technical_info.png)

### <a name="solution-identifier"></a>Oplossings-id

Eerst wordt de id van uw oplossing.

1. Deze id vinden, gaat u naar de levenscyclus van Services en selecteer beheer van de oplossing.
2. Nadat u de juiste oplossing, ziet u de id van de oplossing in het overzicht van het pakket. \*\*Als de id leeg is, selecteert u bewerken en opnieuw publiceren van uw pakket voor de id van de oplossing worden weergegeven.

### <a name="validation-assets"></a>Validatie van Assets

Upload het hier uw auto (aanpassing van rapporten).

### <a name="does-solution-enable-translations"></a>Oplossing translation(s) inschakelen?

Selecteer \'Ja\' of \'Nee\'.

### <a name="does-solution-include-localizations"></a>Omvat oplossing Localization(s)?

Selecteer \'Ja\' of \'Nee\'.

### <a name="product-version"></a>Productversie

Selecteer nieuwe AX. Ten slotte klikt u op opslaan.

## <a name="how-to-fill-out-storefront-details-form"></a>Klik hier voor meer informatie over het invullen van formulier voor gegevens van webwinkel.

Eerst wordt de Details van de aanbieding.

1. **Overzicht van aanbieding**

    \- Geef een korte samenvatting van uw oplossing (maximaal 100 tekens).

2. **Beschrijving van aanbieding**

    \- Geef een korte beschrijving van uw oplossing. De beschrijving moet het functionele footprint van uw oplossing en rechtstreeks uit te lijnen met uw BPM-bibliotheek. bijvoorbeeld, als u bijvoorbeeld hebt functies x, y, z in uw marketinginhoud, tijdens de laatste revisie wij zorgen ervoor dat deze worden beschreven in de bibliotheek BPM binnen LCS.

![StoreFront detailscherm](./media/publish_d365_new_offer/offer_details.png)

### <a name="listing-details"></a>De Details van aanbieding

![StoreFront detailscherm](./media/publish_d365_new_offer/storefront_details.png)

1. **Branches** -maximaal twee branches in de opgegeven opties controleren.
2. **Categorieën** -maximaal drie categorieën uit de opgegeven opties controleren.
3. **App-Type** -Selecteer in de opgegeven opties.
4. **Help-koppeling voor uw App** -voert u de help-koppeling voor uw oplossing.
5. **Ondersteunde landen/regio's** -uit de opgegeven opties controleren.
6. **Ondersteunde talen** -uit de opgegeven opties controleren.
7. **App-versie** -versie van uw oplossing die wordt uitgegeven in te voeren. (bijvoorbeeld 1.0.0.0)
8. **App Release datum** -release Voer datum van uw solution(mm/dd/yyyy).
9. **Uw app met werkt producten** -lijst-specifieke producten die geschikt is voor uw app. U kunt maximaal drie producten weergeven. Als u een product, klikt u op het plusteken (naast Nieuw) en een nieuw open tekstveld wordt gemaakt voor u de naam van een product dat uw app met werkt invoeren.
10. **Trefwoorden zoeken** -Voer algemene voorwaarden die gebruikers gebruiken kunnen om te vinden van uw oplossing tijdens een zoekopdracht. \*\*Deze trefwoorden worden niet weergegeven in de marketplace.
11. **Sleutel verbergen** -dit is de sleutel die kan worden gecombineerd met de voorbeeld-URL van de aanbieding te verbergen in openbaar weer te geven. Het is niet een wachtwoord. U kunt een willekeurige tekenreeks hier invoeren.

### <a name="marketing-artifacts"></a>Marketing artefacten

1. Vervolgens wordt geüpload uw **logo's**, **schermafbeeldingen**. \*\*Houd er rekening mee de grootten voor het uploaden van elke en alle afbeeldingen moeten zich op de PNG-indeling.
2. **Demo van video's** -Klik op \"+ nieuw\". Upload een demovideo van uw oplossing (alleen YouTube of Vimeo-koppelingen). \*\*. Houd er rekening mee dat u een miniatuur van de opgegeven grootte voor het maken van uw video worden weergegeven in fasering moet uploaden.
3. **Documenten** : alle documenten met betrekking tot uw oplossing uploaden en voer een naam op voor het document.

### <a name="legal"></a>Juridische informatie

Deze informatie koppelt aan uw privacybeleid en de gebruiksvoorwaarden. Voer de oplossing URL privacybeleid en de gebruiksvoorwaarden. \*\*De klant zich voor dit beleid op de portal.

### <a name="customer-support"></a>Klantondersteuning

De URL voor ondersteuning is alleen zichtbaar in de portal voor uw gebruikers.

### <a name="leads-management"></a>Beheer van leads

Selecteer een CRM-systeem indien u leiden worden, opgeslagen. Selecteer \"Azure Table\" hier hebt u een van de volgende CRM-systemen: Salesforce, Marketo, Microsoft Dynamics CRM. De CRM-systeem die u hier selecteert, wordt waar we de details van eindgebruikers die uw Apps weer op AppSource (leads proberen) worden geschreven. Afhankelijk van de CRM-systeem dat u selecteert, klikt u op de bijbehorende URL hieronder voor meer informatie over het uitvoeren van de volgende set velden.

![Management leaddetails](./media/publish_d365_new_offer/leads.png)

1. Raadpleeg voor Azure Table [hier](https://aka.ms/leadsettingforazuretable)
2. Raadpleeg voor Dynamics CRM online, [hier](https://aka.ms/leadsettingfordynamicscrm)
3. Voor Marketo [hier](https://aka.ms/leadsettingformarketo)
4. Raadpleeg voor Salesforce [hier](https://aka.ms/leadsettingforsalesforce)

## <a name="how-to-fill-out-the-contacts-form"></a>Klik hier voor meer informatie over het invullen van het formulier contactpersonen.

Deze informatie wordt gebruikt voor klanten van Microsoft en ondersteuning. Voer de Engineering Contact en klantondersteuning voor uw bedrijf en de URL voor ondersteuning voor uw oplossing. Deze gegevens zullen worden gebruikt, als een single point of neem contact op met, als Microsoft een vraag over uw oplossing en ook voor klantondersteuning heeft.

![Aanbieding contactpersonen scherm](./media/publish_d365_new_offer/Contacts.png)
