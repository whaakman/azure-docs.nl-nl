---
title: Problemen oplossen voor Azure CDN-eindpunten die resulteren in een 404-statuscode | Microsoft Docs
description: Problemen met 404-responscodes met Azure CDN-eindpunten.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 66ee211856bb451caad7af02103aa306d76e8f97
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916271"
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>Oplossen van problemen met Azure CDN-eindpunten die 404-statuscode retourneren
In dit artikel kunt u problemen oplossen met Azure Content Delivery Network (CDN)-eindpunten die 404 statuscodes voor HTTP-antwoord retourneren.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en selecteer **ontvang ondersteuning**.

## <a name="symptom"></a>Symptoom
U kunt een CDN-profiel en een eindpunt hebt gemaakt, maar de inhoud van uw lijkt niet beschikbaar op het CDN. Gebruikers die proberen te krijgen tot uw inhoud via de URL van het CDN ontvangen een HTTP 404-statuscode. 

## <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken, met inbegrip van:

* Oorsprong van het bestand is niet zichtbaar is voor het CDN.
* Het eindpunt is onjuist geconfigureerd, waardoor het CDN om te zoeken in de verkeerde plaats.
* De host wordt afgewezen met de host-header van het CDN.
* Het eindpunt nog niet worden doorgegeven tijdens het CDN heeft.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
> [!IMPORTANT]
> Na het maken van een CDN-eindpunt, wordt deze niet onmiddellijk beschikbaar zijn voor gebruik, aangezien het enige tijd voor de registratie door te geven via het CDN:
> - Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen tien minuten voltooid. 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Profielen van **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren. 
> 
> Als u de stappen in dit document en u nog steeds 404-responsberichten ontvangt, kunt u nog een paar uur om te controleren of het opnieuw voordat u een ondersteuningsticket te openen.
> 
> 

### <a name="check-the-origin-file"></a>Controleer het bronbestand
Controleer eerst of dat het bestand naar de cache beschikbaar op de oorspronkelijke server is en openbaar toegankelijk op het internet is. De snelste manier om dat te doen is een browser openen in een privé- of incognito-sessie en Ga rechtstreeks naar het bestand. Typ of plak de URL in het adresvak en controleer of dat deze resulteert in het bestand dat u verwacht. Stel bijvoorbeeld dat u hebt een bestand in een Azure Storage-account toegankelijk zijn op https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt. Als u kunt de inhoud van dit bestand is geladen, wordt de test doorgegeven.

![Gelukt!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Dit is de snelste en eenvoudigste manier om te controleren of het bestand is openbaar beschikbaar, sommige netwerkconfiguraties in uw organisatie kunnen gemakkelijk worden weergegeven die is een bestand algemeen beschikbaar wanneer deze is, in feite alleen zichtbaar voor gebruikers van uw netwerk (zelfs als deze wordt gehost in Azure). Om ervoor te zorgen dat dit niet het geval is, moet u het bestand met een externe browser, zoals een mobiel apparaat dat niet is verbonden met het netwerk van uw organisatie, of een virtuele machine in Azure testen.
> 
> 

### <a name="check-the-origin-settings"></a>Controleer de instellingen van de oorsprong
Nadat u hebt geverifieerd dat het bestand is openbaar beschikbaar is op het internet, controleert u of uw instellingen van oorsprong. In de [Azure Portal](https://portal.azure.com), blader naar uw CDN-profiel en selecteert u het eindpunt dat u problemen met. In de resulterende **eindpunt** pagina, selecteert u de oorsprong.  

![Pagina met endpoint met oorsprong gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

De **oorsprong** pagina wordt weergegeven. 

![Oorsprong-pagina](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Oorsprongtype en hostnaam
Controleer de waarden van de **oorsprongtype** en **hostnaam van oorsprong** juist zijn. In dit voorbeeld https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, het gedeelte van de hostnaam van de URL is *cdndocdemo.blob.core.windows.net*, dat klopt. Omdat Azure Storage, Web-App en Cloud Service oorsprongen gebruikmaken van een vervolgkeuzelijst met waarde voor de **hostnaam van oorsprong** veld, onjuist gespeld niet een probleem zijn. Echter, als u een aangepaste oorsprong gebruikt, zorg ervoor dat de hostnaam van uw juist is gespeld.

#### <a name="http-and-https-ports"></a>HTTP en HTTPS-poorten
Controleer uw **HTTP** en **HTTPS-poorten**. In de meeste gevallen 80 en 443 juist zijn en u geen wijzigingen nodig hebt.  Echter, als de oorspronkelijke server op een andere poort luistert, die moet worden hier weergegeven. Als u niet zeker weet, bekijkt u de URL voor uw bronbestand. De specificaties voor HTTP en HTTPS gebruiken poorten 80 en 443 als de standaardwaarden. In de voorbeeld-URL, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, een poort niet is opgegeven, wordt aangenomen dat de standaardwaarde 443 en de instellingen juist zijn.  

Stel echter dat de URL voor de oorsprong-bestand dat u eerder hebt getest, http wordt:\//www.contoso.com:8080/file.txt. Houd er rekening mee de *: 8080* deel aan het einde van het segment hostnaam. Dat nummer Hiermee geeft u de browser gebruikmaakt van poort 8080 verbinding maken met de webserver op www\.contoso.com, dus u moet invoeren *8080* in de **HTTP-poort** veld. Het is belangrijk te weten dat deze poortinstellingen gelden alleen welke poort het eindpunt wordt gebruikt voor het ophalen van gegevens uit de oorsprong.

> [!NOTE]
> **Azure CDN Standard van Akamai** eindpunten staan niet het volledige TCP-poortbereik voor oorsprongen toe.  Zie [Azure CDN from Akamai Allowed Origin Ports](/previous-versions/azure/mt757337(v=azure.100)) (Door Azure CDN van Akamai toegestane poorten van oorsprong) voor een lijst met poorten van oorsprong die niet zijn toegestaan.  
> 
> 

### <a name="check-the-endpoint-settings"></a>Controleer de eindpuntinstellingen van het
Op de **eindpunt** weergeeft, schakelt de **configureren** knop.

![Eindpunt-pagina met de knop configureren gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Het CDN-eindpunt **configureren** pagina wordt weergegeven.

![De pagina configureren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocollen
Voor **protocollen**, controleert u of het protocol dat wordt gebruikt door de clients is geselecteerd. Omdat hetzelfde protocol wordt gebruikt door de client gebruikt wordt voor toegang tot de oorsprong, is het belangrijk dat u hebt de oorsprongpoorten juist geconfigureerd in de vorige sectie. Het CDN-eindpunt luistert alleen op de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de oorsprongpoorten.

Gaan we terug naar ons voorbeeld hypothetische met http:\//www.contoso.com:8080/file.txt.  Als u meer weet u, Contoso opgegeven *8080* als de HTTP-poort, maar we gaan ook wordt ervan uitgegaan dat ze opgegeven *44300* als hun HTTPS-poort.  Als ze een eindpunt met de naam gemaakt *contoso*, de hostnaam van de CDN-eindpunt zou worden *contoso.azureedge.net*.  Een aanvraag voor http:\//contoso.azureedge.net/file.txt is een HTTP-aanvraag, zodat het eindpunt HTTP via poort 8080 gebruikt te halen uit de oorsprong.  Een beveiligde aanvragen via HTTPS, https: \/ /contoso.azureedge.net/file.txt, ertoe kan leiden dat het eindpunt voor gebruik van HTTPS op poort 44300 bij het ophalen van het bestand van de oorsprong.

#### <a name="origin-host-header"></a>Host-header van oorsprong
De **host-header van oorsprong** is de waarde van de host-header verzonden naar de oorsprong elke aanvraag.  In de meeste gevallen dit moet hetzelfde zijn als de **hostnaam van oorsprong** we eerder hebt geverifieerd.  Een onjuiste waarde in dit veld wordt niet in het algemeen 404-statusberichten veroorzaken, maar andere 4xx-statussen, afhankelijk van wat de oorsprong wordt verwacht dat leidt waarschijnlijk tot.

#### <a name="origin-path"></a>Oorsprongpad
Ten slotte moet controleren we onze **oorsprongpad**.  Dit is standaard leeg.  U moet dit veld alleen gebruiken als u wilt beperken tot het bereik van de oorsprong gehoste bronnen die u beschikbaar wilt maken op het CDN.  

In het voorbeeld-eindpunt, wilden we alle resources in de storage-account beschikbaar, zodat **oorsprongpad** is leeg.  Dit betekent dat een aanvraag naar https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt resulteert in een verbinding van het eindpunt met cdndocdemo.core.windows.net die worden aangevraagd */publicblob/lorem.txt*.  Ook kan een aanvraag voor https:\//cdndocdemo.azureedge.net/donotcache/status.png resulteert in het aanvragen van het eindpunt */donotcache/status.png* bij de oorsprong.

Maar wat gebeurt er als u het CDN gebruiken voor elk pad op de oorsprong niet wilt?  Stel dat u wilt alleen blootstellen de *publicblob* pad.  Als we invoeren */publicblob* in de **oorsprongpad** veld, waardoor het eindpunt om in te voegen */publicblob* vóór elk verzoek naar de oorsprong.  Dit betekent dat de aanvraag voor https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt duurt het gedeelte van de aanvraag van de URL, nu eigenlijk */publicblob/lorem.txt*, en toevoeg- */publicblob* aan het begin. Dit resulteert in een aanvraag voor */publicblob/publicblob/lorem.txt* bij de oorsprong.  Als het opgegeven pad niet worden omgezet in een bestand, wordt de oorsprong een 404-status geretourneerd.  De juiste URL ophalen van lorem.txt in dit voorbeeld zou daadwerkelijk https:\//cdndocdemo.azureedge.net/lorem.txt.  Let op: we zijn niet opgenomen de */publicblob* pad, omdat het deel van de aanvraag van de URL is */lorem.txt* en het eindpunt wordt toegevoegd */publicblob*, wat resulteert in */publicblob/lorem.txt* de aanvraag wordt doorgegeven aan de oorsprong.

