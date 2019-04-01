---
title: Configureren van instellingen voor Toegangsbeoordeling hulpprogramma - Content Moderator
titlesuffix: Azure Cognitive Services
description: Gebruik het beoordelingsprogramma configureren of het ophalen van uw team, labels, connectors, werkstromen en referenties voor Content Moderator.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: f88ccbabc925b651abbc06f571a9d4220ed8aeb2
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756519"
---
# <a name="configure-the-review-tool"></a>Het beoordelingsprogramma configureren

De [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com) bevat verschillende belangrijke functies die u via openen kunt de **instellingen** menu op het dashboard.

![Content Moderator revisie te instellingenmenu](images/settings-1.png)

## <a name="manage-team-and-subteams"></a>Team en subteams beheren

De **Team** tabblad kunt u bij het beheren van uw team en subteams&mdash;groepen van gebruikers die kunnen worden gewaarschuwd wanneer bepaalde [onlinebeoordelingen door mensen](../review-api.md#reviews) worden gestart. U kunt slechts één team (die u maakt wanneer u zich aanmeldt met het beoordelingsprogramma) hebben, maar u kunt meerdere subteams maken. De beheerder team kunt uitnodigen leden, hun machtigingen instellen en deze toewijzen aan verschillende subteams.

![Hulpprogramma voor team-instellingen controleren](images/settings-2-team.png)

Subteams zijn handig voor het maken van escalatie teams of teams die zijn toegewezen aan het controleren van bepaalde categorieën van inhoud. U kunt bijvoorbeeld inhoud voor volwassenen verzenden naar een afzonderlijk team voor verder onderzoek.

In deze sectie wordt uitgelegd hoe u subteams maken en snel beoordelingen op elk gewenst moment toe te wijzen. U kunt echter [werkstromen](workflows.md) om toe te wijzen van beoordelingen op basis van specifieke criteria.

### <a name="create-a-subteam"></a>Maken van een subteams bepalen

Ga naar de **Subteams** sectie en klikt u op **toevoegen subteams bepalen**. Voer de naam van uw subteams bepalen in het dialoogvenster en klik op **opslaan**.

![De naam van de subteams bepalen](images/1-Teams-2.PNG)

#### <a name="invite-teammates"></a>Collega uitnodigen

U toewijzen niet iemand aan een subteams bepalen als ze niet al een lid van de standaardteam, dus moet u eerst revisoren toevoegen aan de standaardteam. Klik op **uitnodigen** op de **Team** tabblad.

![Gebruikers uitnodigen](images/invite-users.png)

#### <a name="assign-teammates-to-subteam"></a>Teamleden op subteam toewijzen

Klik op de **Add Member** knop leden van uw standaardteam toewijzen aan een of meer subteams. U kunt alleen bestaande gebruikers toevoegen aan een subteams bepalen. Voor het toevoegen van nieuwe gebruikers die zich niet in het beoordelingsprogramma, nodigen u ze met behulp van de knop 'Uitnodigen' op de pagina Team-instellingen.

![Subteams bepalen leden toewijzen](images/1-Teams-3.PNG)

### <a name="assign-reviews-to-subteams"></a>Beoordelingen toewijzen aan subteams

Nadat u uw subteams hebt gemaakt en toegewezen leden, kunt u start toewijzen van inhoud [beoordeelt](../review-api.md#reviews) naar deze subteams. Dit wordt gedaan via de **revisie** tabblad van de site.
Klik op het weglatingsteken in de rechterbovenhoek, selecteert u de inhoud toewijzen aan een subteams bepalen, **verplaatsen naar**, en selecteer een subteams bepalen.

![Afbeelding beoordeling subteam toewijzen](images/3-review-image-subteam-1.png)

### <a name="switch-between-subteams"></a>Schakelen tussen subteams

Als u een lid van meer dan één subteams bepalen, kunt u schakelen tussen deze subteams te wijzigen welke inhoud beoordelingen worden weergegeven. In de **revisie** tabblad, selecteert u de vervolgkeuzelijst met het label **standaard** en selecteer **Kies subteams bepalen**. Vindt u de inhoud beoordelingen voor verschillende subteams, maar alleen de waarvan je lid bent.

![Schakelen tussen subteams](images/3-review-image-subteam-2.png)

## <a name="tags"></a>Tags

De **Tags** tabblad kunt u beheer van aangepaste labels naast de twee standaard toezicht op labels definiëren&mdash;**isadult** (**een**) en **isracy**  (**r**). Wanneer u een aangepaste tag maakt, is deze beschikbaar in beoordelingen naast de standaardtags. U kunt wijzigen welke labels worden weergegeven op beoordelingen door over te schakelen van de instellingen voor de zichtbaarheid.

![Labels weergeven, met inbegrip van de selectievakjes in 'Zichtbaar Is'](images/tags-4-disable.png)

### <a name="create-custom-tags"></a>Aangepaste labels maken

Als u wilt een nieuw label maken, moet u een korte code, naam en beschrijving invoeren in de desbetreffende velden.

- **Korte code**: Voer een code twee letters voor uw code. Voorbeeld: **cb**
- **Naam**: Voer een tagnaam in de kort en beschrijvend in kleine letters zonder spaties. Voorbeeld: **isbullying**.
- **Beschrijving**: (optioneel) Voer een beschrijving van het type inhoud dat de doelen van uw code. Voorbeeld: **Voorstellingen of verschillende exemplaren van cyber bullying**.

Klik op **toevoegen** naar een label toevoegen en klik op **opslaan** wanneer u bent klaar labels maken.

![Hulpprogramma voor beoordeling dialoogvenster voor een nieuwe tag maken](images/settings-3-tags.png)

### <a name="delete-tags"></a>Labels verwijderen

U kunt aangepaste labels verwijderen door het prullenbakpictogram naast de items in de lijst met labels selecteren, maar u kunt de standaardtags niet verwijderen.

## <a name="connectors"></a>Connectors

De **Connectors** tabblad kunt u uw connectors die zijn servicespecifieke invoegtoepassingen waarmee kunnen inhoud op verschillende manieren worden verwerkt als onderdeel van de inhoud beheren [werkstromen](../review-api.md#workflows).

De standaard-connector wanneer u een werkstroom maakt, wordt de Content Moderator-connector, die inhoud als kunt markeren **volwassenen** of **ongepaste**, grof taalgebruik zoeken, enzovoort. Echter, kunt u andere connectors, hier worden vermeld, zolang u referenties voor hun respectieve services hebt (voor het gebruik van de Face-API-connector, bijvoorbeeld, u moet aan een [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/overview) abonnementssleutel).

De [beoordelingsprogramma](./human-in-the-loop.md) bevat de volgende connectors:

- Emotion-API
- Face-API
- PhotoDNA Cloud Service
- Tekstanalyse-API

### <a name="add-a-connector"></a>Een connector toevoegen

Een connector toevoegen (en maakt deze beschikbaar voor gebruik in de inhoud [werkstromen](../review-api.md#workflows)), selecteer de juiste **Connect** knop. Geef uw abonnementssleutel voor die service in het volgende dialoogvenster. Wanneer u klaar bent, kan de nieuwe connector moet worden weergegeven aan de bovenkant van de pagina.

![Content Moderator Connectors instellingen](images/settings-4-connectors.png)

## <a name="workflows"></a>Workflows

De **werkstromen** tabblad kunt u beheren uw [werkstromen](../review-api.md#workflows). Werkstromen zijn cloud-gebaseerde filters voor inhoud en ze werken met connectors voor het sorteren van inhoud op verschillende manieren en gepaste actie ondernemen. Hier kunt u definiëren, bewerken en testen van uw werkstromen. Zie [definiëren en gebruiken werkstromen](Workflows.md) voor informatie over hoe u dit doet.

![Content Moderator Werkstroominstellingen](images/settings-5-workflows.png)

## <a name="credentials"></a>Referenties

De **referenties** tabblad biedt snel toegang tot uw abonnementssleutel Content Moderator, hebt u nodig voor toegang tot alle van de services voor beheer van een REST-aanroep of client-SDK.

![Content Moderator-referenties](images/settings-6-credentials.png)

### <a name="use-external-credentials-for-workflows"></a>Externe referenties gebruiken voor werkstromen

De [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com) genereert een gratis proefversie sleutel voor Azure Content Moderator services wanneer u zich aanmeldt, maar u kunt ook configureren om het gebruik van een bestaande sleutel van uw Azure-account. Dit wordt aanbevolen voor grootschalige scenario's, zoals sleutels voor een gratis proefversie strikte gebruikslimieten hebt ([prijzen en beperkingen](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/)).

Als u hebt gemaakt een [Content Moderator resource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) in Azure, gaat u naar het in de Azure portal en selecteer de **sleutels** blade. Kopieer een van uw sleutels.

![Content Moderator-sleutels in Azure portal](images/credentials-azure-portal-keys.PNG)

In de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com)van **referenties** tabblad, Ga naar de **Werkstroominstellingen** venster **bewerken**, en plak uw sleutel in de **Ocp-Apim-Subscription-Key** veld. Werkstromen die de afbeeldingstoezicht-API's aanroepen gebruik nu uw Azure-referenties.

> [!NOTE]
> De twee andere velden in de **Werkstroominstellingen** deelvenster zijn voor de aangepaste periode en image-lijsten. Zie de [aangepaste voorwaarden](../try-terms-list-api.md) of [aangepaste installatiekopieën](../try-image-list-api.md) handleidingen voor meer informatie over deze.

### <a name="use-your-azure-account-with-the-review-apis"></a>Uw Azure-account gebruiken met de beoordelings-API 's

Voor het gebruik van uw Azure-sleutel met de beoordelings-API's, dat u wilt ophalen van de Resource-ID. Ga naar uw Content Moderator-resource in Azure portal en selecteer de **eigenschappen** blade. Kopieer de Resource-ID-waarde en plak deze in de **Resource (s) in de whitelist opgenomen** veld van het beoordelingsprogramma **referenties** tabblad.

![Content Moderator-Resource-ID in Azure portal](images/credentials-azure-portal-resourceid.PNG)

Als u uw abonnementssleutel hebt ingevoerd op beide plaatsen, de proefversie sleutel die wordt geleverd met uw account lees-hulpprogramma wordt niet gebruikt, maar blijven beschikbaar.

## <a name="next-steps"></a>Volgende stappen

Ga als volgt de [revisie hulpprogramma snelstartgids](../quick-start.md) om te beginnen met het beoordelingsprogramma in scenario's voor inhoudstoezicht.