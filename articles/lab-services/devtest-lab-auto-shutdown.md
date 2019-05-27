---
title: Beheren van autoshutdown beleid in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u autoshutdown beleid voor een testomgeving instellen zodat virtuele machines automatisch worden afgesloten wanneer ze zich niet in gebruik.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: spelluru
ms.openlocfilehash: 9adf8dd4a5a3c469ed130b29308a0d828aee40bf
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873988"
---
# <a name="manage-autoshutdown-policies-for-a-lab-in-azure-devtest-labs"></a>Autoshutdown beleidsregels beheren voor een lab in Azure DevTest Labs
Azure DevTest Labs kunt u kosten beheren en minimale verspilling in uw labs door het beheer van beleid (instellingen) voor elk lab. Dit artikel leest u hoe autoshutdown beleid voor een lab-account te configureren en autoshutdown-instellingen voor een lab configureren in het lab-account. Als u wilt bekijken over het instellen van elk lab-beleid, Zie [definiëren van beleid voor lab maken in Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="set-auto-shutdown-policy-for-a-lab"></a>Beleid voor automatisch afsluiten voor een testomgeving instellen
Als de eigenaar van een lab, kunt u een planning afsluiten configureren voor alle virtuele machines in uw testomgeving. Op deze manier kunt u kosten kunt besparen van het uitvoeren van virtuele machines die niet worden gebruikt (niet-actief). U kunt afdwingen dat een beleid voor afsluiten op al uw lab-virtuele machines centraal, maar ook opslaan uw labgebruikers de moeite van het instellen van een schema voor de afzonderlijke computers. Deze functie kunt u het beleid op uw lab-schema biedt geen controle op volledig beheer voor uw labgebruikers vanaf instellen. Als de eigenaar van een lab, kunt u dit beleid configureren door de volgende stappen uit:

1. Selecteer op de startpagina van uw lab, **configuratie en het beleid**.
2. Selecteer **automatisch afsluiten beleid** in de **planningen** gedeelte van het menu links.
3. Selecteer een van de opties. De volgende secties bevatten meer informatie over deze opties: De set-beleid van toepassing alleen op nieuwe virtuele machines die zijn gemaakt in de testomgeving en niet op de al bestaande VM's. 

    ![Automatisch opties voor afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-auto-shutdown-settings"></a>Configureren van instellingen voor automatisch afsluiten
Het beleid autoshutdown helpt bij het lab verspilling minimaliseren met de mogelijkheid om op te geven van de tijd die VM's van deze testomgeving afgesloten.

Als u wilt weergeven (en wijzigen) op het beleid voor een testomgeving, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.   
4. Selecteer **configuratie en het beleid**.

    ![Deelvenster instellingen met beleid](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. Op van het lab **configuratie en het beleid** venster **Auto-shutdown** onder **planningen**.
   
    ![Autoshutdown](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.
7. Als u dit beleid inschakelt, geeft u de tijd (en tijdzone) om alle virtuele machines in de huidige testomgeving af te sluiten.
8. Geef **Ja** of **geen** voor de optie voor het verzenden van een melding voorafgaand aan het moment van de opgegeven autoshutdown de 15 minuten. Als u ervoor kiest **Ja**voert u een webhook-URL-eindpunt of een e-mailadres op te geven waar u de melding om te worden geplaatst of worden verzonden. De gebruiker melding ontvangt en de optie voor het uitstellen van het afsluiten is opgegeven. Zie voor meer informatie de [meldingen](#notifications) sectie. 
9. Selecteer **Opslaan**.

    Standaard eenmaal is ingeschakeld, dit beleid is van toepassing op alle virtuele machines in de huidige testomgeving. Als deze instelling uit een specifieke virtuele machine verwijderen, open deelvenster van de VM-management en wijzig de **Autoshutdown** instelling.

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Gebruikers-Hiermee stelt u een planning en zich kunt afmelden
Als u uw testomgeving aan dit beleid instelt, wordt het labgebruikers kunnen overschrijven, of afmelden voor het lab-schema. Deze optie geeft labgebruikers volledige controle over de planning voor automatische afsluiting van hun virtuele machines. Labgebruikers zien geen wijziging in hun planningspagina VM automatisch afsluiten.

![Automatisch afsluiten beleidsoptie - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Gebruikers-Hiermee stelt u een planning en kan niet opt-out
Als u uw testomgeving aan dit beleid instelt, kunnen labgebruikers het lab-schema negeren. Echter afnemen ze naar beleid voor automatisch afsluiten. Deze optie zorgt ervoor dat elke computer in uw testomgeving onder een planning voor automatische afsluiting is. Labgebruikers kunnen planning voor automatische afsluiting van hun virtuele machines bijwerken en afsluiten meldingen instellen.

![Automatisch afsluiten beleidsoptie - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Gebruiker heeft geen controle over de planning die door de beheerder lab instellen
Als u uw testomgeving aan dit beleid instelt, kunnen de labgebruikers overschrijven of afmelden voor het lab-schema. Deze optie biedt lab-beheerder de volledige controle over de planning voor elke computer in het lab. Labgebruikers kunnen alleen automatisch afsluiten meldingen instellen voor hun VM's.

![Automatisch afsluiten beleidsoptie - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Meldingen
Zodra autoshutdown instellen door de eigenaar van het lab, meldingen worden verzonden naar de labgebruikers 15 minuten voordat de autoshutdown geactiveerd als een van hun VM's wordt beïnvloed. Met deze optie geeft labgebruikers een kans om op te slaan hun werk op voordat u het afsluiten. De melding bevat ook koppelingen voor elke virtuele machine voor de volgende acties:

- Deze tijd de autoshutdown overslaan
- Uitstellen gedurende een uur of twee uur, de autoshutdown zodat ze op de virtuele machine kunnen blijven werken.

Melding wordt verzonden via het geconfigureerde web hook-eindpunt of een e-mailadres opgegeven door de eigenaren van een lab in de instellingen voor autoshutdown. Webhooks kunt u bouwen of integraties gebruikt die zich op bepaalde gebeurtenissen abonneren instellen. Wanneer een van de gebeurtenissen die wordt geactiveerd, wordt een HTTP POST-nettolading in DevTest Labs verzenden naar geconfigureerde van de webhook-URL. Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

We raden u aan het gebruik van webhooks, omdat ze zijn uitgebreid ondersteund door verschillende apps (bijvoorbeeld, Slack, Azure Logic Apps en enz.) en u kunnen voor het implementeren van uw eigen manier om meldingen te verzenden. Bijvoorbeeld: in dit artikel leert u hoe u notification autoshutdown ophalen uit de e-mailberichten met behulp van Azure Logic Apps. Eerst gaan we snel gaan door de basisstappen om in te schakelen autoshutdown melding in uw testomgeving.   

## <a name="create-a-logic-app-that-receives-email-notifications"></a>Een logische app die u e-mailmeldingen ontvangt maken
[Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) biedt vele out-of-the-box-connectors kunt u eenvoudig een service integreren met andere clients, zoals Office 365 en twitter. Op het hoogste niveau, kunnen de stappen voor het instellen van een logische App voor e-mailmeldingen worden onderverdeeld in vier fasen: 

- Een logische app maken. 
- De ingebouwde sjabloon configureren.
- Integreren met uw e-mailclient
- De Webhook-URL ophalen.

### <a name="create-a-logic-app"></a>Een logische app maken
Als u wilt beginnen, moet u een logische app maken in uw Azure-abonnement met behulp van de volgende stappen uit:

1. Selecteer **+ een resource maken** in het menu links, selecteer **integratie**, en selecteer **logische App**. 

    ![Nieuw menu van de logische app](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. Op de **logische App - maken** pagina, als volgt te werk: 
    1. Voer een **naam** voor de logische app.
    2. Selecteer uw Azure-**abonnement**.
    3. Maak een nieuwe **resourcegroep** of Selecteer een bestaande resourcegroep. 
    4. Selecteer een **locatie** voor de logische app. 

        ![Nieuwe logische app - instellingen](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. In de **meldingen**, selecteer **naar de resource gaan** op de melding. 

    ![Ga naar resource](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. Selecteer **Logic app designer** onder **implementatiehulpprogramma's** categorie.

    ![Selecteer HTTP-aanvraag/antwoord](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. Op de **HTTP-Aanvraagantwoord** weergeeft, schakelt **Gebruik deze sjabloon**. 

    ![Selecteer deze optie sjabloon gebruiken](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. Kopieer de volgende JSON naar de **hoofdtekst van de JSON-Schema aanvragen** sectie: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![Hoofdtekst van aanvraag voor JSON-schema](./media/devtest-lab-auto-shutdown/request-json.png)
7. Selecteer **+ nieuwe stap** in de ontwerpfunctie voor, en als volgt te werk:
    1. Zoeken naar **Office 365 Outlook - een e-mail verzenden**. 
    2. Selecteer **een e-mailbericht verzenden** van **acties**. 
    
        ![Optie voor e-mailbericht verzenden](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. Selecteer **aanmelden** aan te melden bij uw e-mailaccount. 
    4. Selecteer **aan** veld en kiest u de eigenaar.
    5. Selecteer **onderwerp**, en voert u een onderwerp van het e-mailbericht. Bijvoorbeeld: "Afsluiten van de machine vmName voor testomgeving: labName."
    6. Selecteer **hoofdtekst**, en de hoofdtekst voor e-mailmelding te definiëren. Bijvoorbeeld: 'vmName is gepland om binnen 15 minuten af te sluiten. Dit afsluiten door te klikken op Overslaan: URL. Afsluiten een uur vertraging: delayUrl60. Afsluiten gedurende 2 uur vertraging: delayUrl120. "

        ![Hoofdtekst van aanvraag voor JSON-schema](./media/devtest-lab-auto-shutdown/email-options.png)
1. Selecteer **Opslaan** op de werkbalk. Nu kunt u de **URL voor HTTP POST**. Selecteer de knop kopiëren aan de URL naar het Klembord kopiëren. 

    ![WebHook URL](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het instellen van alle beleidsregels, [definiëren van beleid voor lab maken in Azure DevTest Labs](devtest-lab-set-lab-policy.md).
