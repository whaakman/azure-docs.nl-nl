---
title: Veelgestelde vragen over Azure traffic analytics | Microsoft Docs
description: Vind antwoorden op enkele veelgestelde vragen over traffic analytics.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: 3ab06b624d1e433641d190d9621592ef83df3344
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Verkeer analytics Veelgestelde vragen

1.  Wat zijn de vereisten voor traffic analytics gebruiken?

    Traffic Analytics moet de volgende vereisten:

    - Een abonnement van de netwerk-Watcher ingeschakeld
    - NSG-stroom logboeken ingeschakeld voor het nsg's die u wilt bewaken
    - Een Azure Storage-account voor het opslaan van onbewerkte flog Logboeken
    - Een werkruimte voor logboekanalyse (OMS), met lees- en schrijftoegang
    - Uw account moet worden toegewezen aan de volgende acties op de Microsoft.COMPUTE-provider:

        - Microsoft.Network/applicationGateways/read
        - Microsoft.Network/connections/read
        - Microsoft.Network/loadBalancers/read 
        - Microsoft.Network/localNetworkGateways/read 
        - Microsoft.Network/networkInterfaces/read 
        - Microsoft.Network/networkSecurityGroups/read 
        - Microsoft.Network/publicIPAddresses/read
        - Microsoft.Network/routeTables/read
        - Microsoft.Network/virtualNetworkGateways/read 
        - Microsoft.Network/virtualNetworks/read

2.  Welke Azure-regio's traffic analytics beschikbaar zijn in?

    In de preview-versie, kunt u verkeer analytics voor nsg's in een van de volgende **ondersteunde regio's**: West-Centraal VS, VS-Oost, VS-Oost 2, Noordelijk Centraal, VS, Zuid-centraal VS, VS-midden, VS-West, VS-West-2, West-Europa, Noord-Europa , West VK, Zuid VK, Australië-Oost en Australië-Zuidoost. De werkruimte voor logboekanalyse moet bestaan in de West-Centraal VS, VS-Oost, West-Europa, Australië-Zuidoost of de regio Zuid VK.

3.  Kan het nsg's ik stroom inschakelen Logboeken voor zich in verschillende regio's dan mijn OMS-werkruimte?

    Ja

4.  Kunnen meerdere nsg's worden geconfigureerd in een enkel werkruimte?

    Ja

5.  Kan ik een bestaande OMS-werkruimte gebruiken?

    Ja, als u een bestaande werkruimte selecteert, zorg dat deze is gemigreerd naar de nieuwe querytaal. Als u niet wilt bijwerken van de werkruimte; u moet een nieuwe maken. Zie voor meer informatie over de nieuwe querytaal [Azure-logboekanalyse bijwerken naar de nieuwe logboek zoekopdracht](../log-analytics/log-analytics-log-search-upgrade.md).

6.  Kan mijn Azure Storage-Account in een abonnement en Mijn OMS-werkruimte in een ander abonnement?

    Ja

7.  Kan ik onbewerkte Logboeken opslaan in verschillende Storage-Account in een ander abonnement

    Nee. U kunt onbewerkte logboeken worden opgeslagen in een opslagaccount waarvoor een NSG voor logboeken van de stroom is ingeschakeld, zowel de storage-account en de logboeken van de onbewerkte moet echter in hetzelfde abonnement en dezelfde regio.

8.  Als ik een 'Niet gevonden'-Fout tijdens het configureren van een NSG voor analyses verkeer ontvangt, hoe kan ik oplossen?

    Selecteer een ondersteunde regio in vraag 2 weergegeven. Als u een niet-ondersteunde regio selecteert, ontvangt u een 'Niet gevonden'-fout.

9.  Onder het NSG-flow-logboeken ik krijg NSG status als 'Mislukt laden', wat te doen?

    De provider Microsoft.Insights moet worden geregistreerd voor logboekregistratie werkt alleen goed als stroom. Als u niet zeker weet of de provider Microsoft.Insights is geregistreerd of voor uw abonnement niet vervangen door de *xxxxx-xxxxx-xxxxxx-xxxx* in de volgende opdracht uit en voer de volgende opdrachten vanuit PowerShell:

    ```powershell-interactive
    **Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
    **Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
    ```

10. Ik hebt de oplossing geconfigureerd. Waarom krijg ik niet zien alles op het dashboard?

    Het dashboard kan maximaal 30 minuten duren voor het eerst wordt weergegeven. De oplossing moet eerst cumulatieve voldoende gegevens voor het afleiden van betekenisvolle inzichten voordat rapporten worden gegenereerd. 

11.  Als ik het volgende bericht: "Er is niet gevonden geen gegevens in deze werkruimte voor het geselecteerde tijdsinterval. Wijzig het tijdsinterval of Selecteer een andere werkruimte', hoe los ik dit?

        Voer de volgende opties:
        - Wijzig de tijdsinterval in de bovenste balk.
        - Selecteer een andere Log Analytics-werkruimte in de bovenste balk
        - Probeer toegang tot Traffic Analytics na 30 minuten als deze onlangs is ingeschakeld
    
        Als de problemen zich blijven voordoen, verhoogt u problemen in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

12.  Als ik het volgende bericht: '1) analyse van uw NSG logboeken voor de eerste keer stromen. Dit proces kan 20-30 minuten duren. Controleer na enige tijd opnieuw. 2) als de vorige stap niet werkt en uw werkruimte onder de gratis SKU is, wordt uw werkruimte-gebruik controleren om te valideren overschreden, anders verwijzen naar Veelgestelde vragen voor meer informatie', hoe los ik dit?

        Het kan foutbericht voor de volgende redenen:
        - Verkeer analyses mogelijk zijn onlangs ingeschakeld en kan niet nog zijn geaggregeerd voldoende gegevens voor het afleiden van betekenisvolle inzichten.
        - De OMS-werkruimte is onder de gratis SKU en deze de quotalimieten geschonden. In dit geval moet u wellicht een werkruimte in een SKU gebruiken met grotere capaciteit.
    
        Als de problemen zich blijven voordoen, verhoogt u problemen in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
13.  Als ik het volgende bericht: 'lijkt te hebben we resources (topologie) en er zijn geen gegevens stromen. Ondertussen Klik hier om te zien van bronnen gegevens en Raadpleeg de veelgestelde vragen voor meer informatie', hoe kan ik deze oplossen?

        U ziet de bronneninformatie op het dashboard; Er zijn echter geen stroom-gegevens aanwezig. Vanwege geen overdrachten van communicatie tussen de bronnen kunnen geen gegevens aanwezig zijn. Wacht tot 60 minuten en status controleren. Als u zeker weet dat overdrachten van communicatie tussen bronnen bestaat en vraagtekens in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

14. Kan ik traffic analytics met PowerShell of een Azure Resource Manager-sjabloon configureren?

    Nee, traffic analytics kan alleen worden geconfigureerd met de Azure portal.

15.  Hoe wordt verkeer analytics prijs?

        Verkeer analytics datalimiet geldt voor het verbeteren van verminderde logboeken en de uitgebreide logboeken op te slaan in een werkruimte voor logboekanalyse. In de preview, traffic analytics wordt niet in rekening gebracht voor het verbeteren van de verminderde logboeken maar bewaren van gegevens in een werkruimte onderworpen aan facturering volgens de tarieven voor gepubliceerde is. Dit antwoord wordt bijgewerkt zodra de prijzen voor traffic analytics is beschikbaar.

16.  Hoe kan ik navigeren met behulp van toetsenbord in Geo overzichtsweergave?

        De pagina geo-kaart bevat twee hoofdsecties:
    
        - **Banner**: de banner geplaatst in de rechterbovenhoek van de kaart Geo biedt de mogelijkheid verkeer Distributiefilters via knoppen zoals Deployment/No implementatie/actief/inactief/verkeer Analytics ingeschakeld/verkeer Analytics niet ingeschakeld/verkeer selecteren uit andere landen/Benign/kwaadaardig/toegestane schadelijk verkeer en informatie van de legenda. Het respectieve filter wordt toegepast op de selectie van gedefinieerde knoppen op de kaart, zoals als een gebruiker de knop met 'Active' filter onder de banner selecteert en vervolgens de kaart de 'Active' datacentra in uw implementatie markeert.
        - **Kaart**: sectie van de Map geplaatst onder de banner toont de distributie van verkeer tussen Azure-datacenters en andere landen.
    
        **Toetsenbordnavigatie op koptekst**
    
        - Standaard is de selectie op de pagina geo-toewijzing voor het logo van het filter 'Azure DC's ' knop.
        - Ga naar een andere filters knop, kunt u ofwel de `Tab` of `Right arrow` sleutel naar de volgende verplaatsen. Gebruik om te navigeren achteruit, ofwel `Shift+Tab` of de `Left arrow` sleutel. Navigatie richting prioriteit is links naar rechts, gevolgd door boven naar beneden.
        - Druk op de `Enter` of `Down` pijltoets het geselecteerde filter wilt toepassen. Op basis van filterselectie en implementatie, zijn een of meer knooppunten onder de sectie Map gemarkeerd.
            - Schakelen tussen **Banner** en **kaart**, drukt u op `Ctrl+F6`.
        
        **Toetsenbordnavigatie op kaart**
    
        - Zodra u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de gemarkeerde knooppunten (**Azure-datacenter** of **land/regio**) in de overzichtsweergave.
        - Om te navigeren naar andere gemarkeerde knooppunten in de kaart kunt u ofwel de `Tab` of `Right arrow` sleutels voor voorwaartse beweging en `Shift+Tab` of de `Left arrow` sleutel voor neerwaartse verkeer.
        - U selecteert een gemarkeerde knooppunt in de kaart, met de `Enter` of `Down arrow` sleutel.
        - Op de selectie van eventuele dergelijke knooppunten, focus naar de **informatie werkset** voor het knooppunt. Standaard focus naar de gesloten knop op de **informatie werkset**. Om verder te bladeren in **vak** geven, gebruikt u `Right` en `Left arrow` sleutels verplaatsen voorwaarts en achterwaarts, respectievelijk. Drukken `Enter` heeft hetzelfde effect als de knop gerichte in de **informatie werkset**.
        - Drukken `Tab` terwijl de focus op de **informatie werkset**, de focus naar de eindpunten in het hetzelfde continent als het geselecteerde knooppunt. U kunt de `Right` en `Left arrow` sleutels om te navigeren door deze eindpunten.
        - Gebruik om te navigeren naar andere stroom eindpunten/continenten cluster, `Tab` voor voorwaartse beweging en `Shift+Tab` voor neerwaartse verkeer.
        - Zodra de focus heeft `Continent clusters`, gebruiken de `Enter` of `Down` pijltoetsen om te markeren de eindpunten in het continent cluster. Gebruik om te navigeren door middel van eindpunten en de knop sluiten op het informatievak van het cluster continent, ofwel de `Right` of `Left arrow` sleutel voor voorwaarts en achterwaarts verkeer, respectievelijk. U kunt gebruiken op een willekeurig eindpunt `Shift+L` overschakelen naar de regel voor verbinding van het geselecteerde knooppunt naar het eindpunt. Drukken `Shift+L` navigeert u opnieuw voor het eindpunt.
        
        In elk stadium:
    
        - `ESC` de uitgebreide selectie wordt samengevouwen.
        - De `UP Arrow` sleutel voert dezelfde actie als `ESC`. De `Down arrow` sleutel voert dezelfde actie als `Enter`.
        - Gebruik `Shift+Plus` inzoomen, en `Shift+Minus` om uit te zoomen.
