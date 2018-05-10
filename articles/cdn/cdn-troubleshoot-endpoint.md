---
title: Het oplossen van Azure CDN-eindpunten die 404 statuscode retourneren | Microsoft Docs
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
ms.openlocfilehash: 1cffef5bbda475032ee7ff07188ab0d9d52846ea
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>Het oplossen van Azure CDN-eindpunten die 404 statuscode retourneren
In dit artikel kunt u problemen oplossen met Azure Content Delivery Network (CDN)-eindpunten die 404 HTTP-statuscodes voor antwoorden retourneren.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **Get Support**.

## <a name="symptom"></a>Symptoom
U kunt een CDN-profiel en een eindpunt hebt gemaakt, maar uw inhoud lijkt niet beschikbaar op de CDN. Gebruikers die proberen te krijgen tot uw inhoud via de URL CDN ontvangen een HTTP 404-statuscode. 

## <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken, inclusief:

* Oorsprong van het bestand is niet zichtbaar is voor de CDN.
* Het eindpunt is onjuist geconfigureerd, waardoor de CDN om te zoeken op de verkeerde plaats.
* De host weigert de host-header vanaf de CDN.
* Het eindpunt nog niet heeft doorgegeven in de CDN.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
> [!IMPORTANT]
> Nadat een CDN-eindpunt is gemaakt, wordt deze niet onmiddellijk beschikbaar zijn voor gebruik, zoals het duurt om de registratie te geven in CDN:
> - Voor **Azure CDN Standard van Microsoft** profielen, doorgeven voltooid gewoonlijk in tien minuten. 
> - Voor **Azure CDN Standard van Akamai** profielen, doorgeven voltooit meestal binnen één minuut. 
> - Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, doorgeven voltooit meestal binnen 90 minuten. 
> 
> Als u de stappen in dit document en u nog steeds 404-responsberichten ontvangt, kunt u nog enkele uren om te controleren opnieuw voordat u een ondersteuningsticket opent.
> 
> 

### <a name="check-the-origin-file"></a>Controleer het bronbestand
Controleer eerst of dat het bestand aan het cachegeheugen beschikbaar is op de bronserver en openbaar toegankelijk is op het internet. De snelste manier om dit is een browser openen in een persoonlijke of incognito-sessie en blader naar het bestand rechtstreeks. Typ of plak de URL in het adresvak en controleer of dat deze resulteert in het bestand dat u verwacht. Stel bijvoorbeeld dat u hebt een bestand in een Azure Storage-account, toegankelijk is op https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt. Als u kunt de inhoud van dit bestand is geladen, is de test geslaagd.

![Gelukt!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Dit is de snelste en gemakkelijkste manier om te controleren of uw bestand openbaar beschikbaar is, is enkele netwerkconfiguraties in uw organisatie kunnen eenvoudiger worden weergegeven die is een bestand algemeen beschikbaar wanneer deze is, in feite alleen zichtbaar voor gebruikers van uw netwerk (zelfs als deze wordt gehost in Azure). Om ervoor te zorgen dat dit niet het geval is, het bestand met een externe browser, zoals een mobiel apparaat dat niet is verbonden met het netwerk van uw organisatie of een virtuele machine in Azure te testen.
> 
> 

### <a name="check-the-origin-settings"></a>Controleer de instellingen van de oorsprong
Nadat u hebt geverifieerd dat het bestand is algemeen beschikbaar op het internet, controleert u of uw instellingen voor de oorsprong. In de [Azure Portal](https://portal.azure.com), blader naar uw CDN-profiel en selecteer het eindpunt dat u wilt oplossen. In de resulterende **eindpunt** pagina, selecteert u de oorsprong.  

![Pagina endpoint met oorsprong gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

De **oorsprong** pagina wordt weergegeven. 

![Pagina van de oorsprong](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Oorsprongtype en hostnaam
Controleer de waarden van de **oorsprongtype** en **de hostnaam van oorsprong** juist zijn. In dit voorbeeld https:\/-cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, het gedeelte hostname van de URL is *cdndocdemo.blob.core.windows.net*, waarmee juist is. Omdat Azure Storage en Web-App Service in de Cloud oorsprongen gebruiken een vervolgkeuzelijst waarde voor de **de hostnaam van oorsprong** veld, onjuist gespeld zijn een probleem niet. Echter, als u een aangepaste oorsprong gebruikt, zorg ervoor dat uw hostnaam juist is gespeld.

#### <a name="http-and-https-ports"></a>HTTP en HTTPS-poorten
Controleer uw **HTTP** en **HTTPS-poorten**. In de meeste gevallen 80 en 443 juist zijn en u geen wijzigingen nodig hebt.  Echter, als de oorspronkelijke server op een andere poort luistert, die moet worden hier weergegeven. Als u niet zeker weet, de URL voor uw oorsprong-bestand weergeven De specificaties voor HTTP en HTTPS gebruiken poorten 80 en 443 als de standaardwaarden. In de voorbeeld-URL, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, een poort niet is opgegeven, zodat aangenomen de standaardwaarde 443 dat wordt en de instellingen juist zijn.  

Stel de URL voor het bronbestand die u eerder hebt getest http is:\//www.contoso.com:8080/file.txt. Opmerking de *: 8080* deel aan het einde van het segment hostnaam. Dat nummer Hiermee geeft u de browser via poort 8080 verbinding maken met de webserver op www.contoso.com, moet u daarom invoeren *8080* in de **HTTP-poort** veld. Het is belangrijk te weten dat deze poortinstellingen alleen welke poort het eindpunt wordt gebruikt gelden voor het ophalen van informatie van de oorsprong.

> [!NOTE]
> **Azure CDN Standard van Akamai** eindpunten niet toestaan dat het volledige TCP-poortbereik voor oorsprongen.  Zie [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Door Azure CDN van Akamai toegestane poorten van oorsprong) voor een lijst met poorten van oorsprong die niet zijn toegestaan.  
> 
> 

### <a name="check-the-endpoint-settings"></a>Controleer de eindpuntinstellingen
Op de **eindpunt** pagina de **configureren** knop.

![Pagina met de knop configureren gemarkeerd endpoint](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Het CDN-eindpunt **configureren** pagina wordt weergegeven.

![Pagina configureren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocollen
Voor **protocollen**, Controleer of het protocol dat wordt gebruikt door de clients is geselecteerd. Omdat deze door de client gebruikt hetzelfde protocol gebruikt voor toegang tot de oorsprong, is het belangrijk dat de oorsprongspoorten juist geconfigureerd in de vorige sectie. Het CDN-eindpunt luistert alleen op de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de oorsprongspoorten.

Laten we terug naar het hypothetische voorbeeld met http:\//www.contoso.com:8080/file.txt.  Als u onthouden wilt, Contoso opgegeven *8080* als hun HTTP-poort, maar we ook wordt ervan uitgegaan dat ze opgegeven *44300* als hun HTTPS-poort.  Als ze een eindpunt met de naam gemaakt *contoso*, de hostnaam van het CDN-eindpunt zou worden *contoso.azureedge.net*.  Een aanvraag voor http:\//contoso.azureedge.net/file.txt is een HTTP-aanvraag, zodat het eindpunt HTTP via poort 8080 gebruikt te halen vanuit de oorsprong.  Beveiligde aanvragen via HTTPS, https: \/ /contoso.azureedge.net/file.txt, ervoor zou zorgen dat het eindpunt voor gebruik van HTTPS op poort 44300 bij het ophalen van het bestand vanuit de oorsprong.

#### <a name="origin-host-header"></a>Host-header van oorsprong
De **host-header van oorsprong** is de waarde van de host-header verzonden naar de oorsprong met elke aanvraag.  In de meeste gevallen dit moet hetzelfde zijn als de **de hostnaam van oorsprong** we eerder gecontroleerd.  Een onjuiste waarde in dit veld niet in het algemeen toe leiden dat 404-statusberichten, maar is waarschijnlijk tot andere statussen 4xx, afhankelijk van wat de oorsprong verwacht.

#### <a name="origin-path"></a>Oorsprongpad
Ten slotte moet controleren we onze **oorsprongpad**.  Dit is standaard leeg.  Gebruik dit veld alleen als u wilt beperken van het bereik van de oorsprong gehoste bronnen die u beschikbaar wilt maken op de CDN.  

In het voorbeeld-eindpunt wilden we alle bronnen op het opslagaccount beschikbaar, dus **oorsprongpad** is leeg.  Dit betekent dat een aanvraag naar https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt resulteert in een verbinding van het eindpunt met cdndocdemo.core.windows.net waarin */publicblob/lorem.txt*.  Ook kan een aanvraag voor https:\//cdndocdemo.azureedge.net/donotcache/status.png resulteert in het aanvragen van het eindpunt */donotcache/status.png* vanuit de oorsprong.

Maar wat gebeurt er als u niet wilt dat met de CDN voor elk pad op uw oorsprong?  Stel u alleen wilt weergeven de *publicblob* pad.  Als we invoert */publicblob* in de **oorsprongpad** veld, waardoor het eindpunt invoegen */publicblob* voor elke aanvraag naar de oorsprong wordt gemaakt.  Dit betekent dat de aanvraag voor https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt duurt het gedeelte van de aanvraag van de URL nu daadwerkelijk */publicblob/lorem.txt*, en toevoeg- */publicblob* aan het begin. Dit resulteert in een aanvraag voor */publicblob/publicblob/lorem.txt* vanuit de oorsprong.  Als dat het pad kan niet worden omgezet naar een bestand, worden de oorsprong 404 status geretourneerd.  De juiste URL voor het ophalen van lorem.txt in dit voorbeeld zou daadwerkelijk https:\//cdndocdemo.azureedge.net/lorem.txt.  Let op: Er zijn niet opgenomen in de */publicblob* pad helemaal, omdat het gedeelte van de aanvraag van de URL is */lorem.txt* en het eindpunt wordt toegevoegd */publicblob*, wat resulteert in */publicblob/lorem.txt* de aanvraag wordt doorgegeven naar de oorsprong.

