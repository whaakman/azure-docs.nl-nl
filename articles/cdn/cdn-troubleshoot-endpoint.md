---
title: Het oplossen van Azure CDN-eindpunten die 404 status retourneren | Microsoft Docs
description: Problemen met 404-responscodes met Azure CDN-eindpunten.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f59fbd18413fb44026d8c92b7f6940ed2f8a00a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Problemen oplossen voor CDN-eindpunten die 404-statusberichten retourneren
Dit artikel helpt u problemen oplossen met [CDN-eindpunten](cdn-create-new-endpoint.md) 404-fouten retourneren.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**.

## <a name="symptom"></a>Symptoom
U kunt een CDN-profiel en een eindpunt hebt gemaakt, maar uw inhoud lijkt niet beschikbaar op de CDN.  Gebruikers die proberen te krijgen tot uw inhoud via de URL CDN ontvangen HTTP 404-statuscodes. 

## <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken, inclusief:

* Oorsprong van het bestand is niet zichtbaar voor de CDN
* Het eindpunt is onjuist geconfigureerd, waardoor de CDN om te zoeken op de verkeerde plaats
* De host is de host-header vanaf de CDN weigeren
* Het eindpunt nog niet heeft doorgegeven in de CDN

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
> [!IMPORTANT]
> Nadat een CDN-eindpunt is gemaakt, zich het onmiddellijk niet beschikbaar voor gebruik, zoals het duurt om de registratie te geven in CDN.  Profielen van <b>Azure CDN van Akamai</b> worden doorgaans binnen één minuut doorgegeven.  Profielen van <b>Azure CDN van Verizon</b> worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren.  Als u de stappen in dit document en u nog steeds 404-responsberichten ontvangt, kunt u nog enkele uren om te controleren opnieuw voordat u een ondersteuningsticket opent.
> 
> 

### <a name="check-the-origin-file"></a>Controleer het bronbestand
Eerst we moet verifiëren dat het bestand we in de cache willen is beschikbaar op onze oorsprong en openbaar toegankelijk is.  De snelste manier om dit is een browser openen in een privé-In- of Incognito-sessie en rechtstreeks naar het bestand bladeren.  NET Typ of plak de URL in het adresvak en Zie als die resulteert in het bestand dat u verwacht.  Bijvoorbeeld, ga ik gebruik een bestand hebben een Azure Storage-account, toegankelijk is op `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Zoals u ziet, wordt de test is doorgegeven.

![Success!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Dit is de snelste en gemakkelijkste manier om te controleren of dat het bestand openbaar beschikbaar is, kunnen sommige netwerkconfiguraties in uw organisatie bieden u de illusie dat dit bestand openbaar beschikbaar is wanneer het is in feite alleen zichtbaar voor gebruikers van uw netwerk (zelfs als Deze wordt gehost in Azure).  Als u een externe browser van waaruit u testen kunt, zoals een mobiel apparaat dat niet is verbonden met het netwerk van uw organisatie of een virtuele machine in Azure, zou die worden aanbevolen.
> 
> 

### <a name="check-the-origin-settings"></a>Controleer de instellingen van de oorsprong
Nu dat we hebt geverifieerd dat het bestand is algemeen beschikbaar op het internet, controleren we onze instellingen oorsprong.  In de [Azure Portal](https://portal.azure.com), blader naar uw CDN-profiel en klik op het eindpunt dat u wilt oplossen.  In de resulterende **eindpunt** blade, klikt u op de oorsprong.  

![De blade een eindpunt met oorsprong gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

De **oorsprong** blade wordt weergegeven. 

![Oorsprong-blade](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Oorsprongtype en hostnaam
Controleer of de **oorsprongtype** juist is, en controleer of de **de hostnaam van oorsprong**.  In mijn voorbeeld `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, het gedeelte hostname van de URL is `cdndocdemo.blob.core.windows.net`.  Zoals u in de schermafbeelding dit klopt ziet.  Voor Azure Storage en Web-App Service in de Cloud oorsprongen, de **de hostnaam van oorsprong** veld is een vervolgkeuzelijst zodat we niet hoeven te hoeven maken over het correct spelling.  Als u een aangepaste oorsprong, het is echter *absoluut essentieel* uw hostnaam juist is gespeld.

#### <a name="http-and-https-ports"></a>HTTP en HTTPS-poorten
De andere ding om te controleren is hier uw **HTTP** en **HTTPS-poorten**.  In de meeste gevallen 80 en 443 juist zijn en u geen wijzigingen nodig hebt.  Echter, als de oorspronkelijke server op een andere poort luistert, die moet worden hier weergegeven.  Als u niet zeker weet, kijk eens naar de URL voor uw bronbestand.  Poorten 80 en 443 opgeven de specificaties voor HTTP en HTTPS als de standaardwaarden. In mijn URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, een poort niet is opgegeven, dus aangenomen de standaardwaarde 443 dat wordt en Mijn instellingen correct zijn.  

De URL voor uw oorsprong-bestand dat u eerder hebt getest, is echter aannemen dat `http://www.contoso.com:8080/file.txt`.  Opmerking de `:8080` aan het einde van het segment hostnaam.  Die vertelt de browser voor gebruik van poort `8080` verbinding maken met de webserver op `www.contoso.com`, dus u moet invoeren 8080 in de **HTTP-poort** veld.  Het is belangrijk te weten dat deze poortinstellingen alleen van invloed op welke poort het eindpunt wordt gebruikt voor het ophalen van informatie van de oorsprong.

> [!NOTE]
> Eindpunten van **Azure CDN van Akamai** staan niet het volledige TCP-poortbereik voor oorsprongen toe.  Zie [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Door Azure CDN van Akamai toegestane poorten van oorsprong) voor een lijst met poorten van oorsprong die niet zijn toegestaan.  
> 
> 

### <a name="check-the-endpoint-settings"></a>Controleer de eindpuntinstellingen
Terug op de **eindpunt** blade, klikt u op de **configureren** knop.

![De blade een eindpunt met de knop configureren gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Het eindpunt **configureren** blade wordt weergegeven.

![Blade configureren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocollen
Voor **protocollen**, Controleer of het protocol dat wordt gebruikt door de clients is geselecteerd.  Hetzelfde protocol gebruikt door de client zal worden gebruikt voor toegang tot de oorsprong, dus is het belangrijk om de oorsprongspoorten juist geconfigureerd in de vorige sectie.  Het eindpunt luistert alleen op de standaard HTTP en HTTPS-poorten (80 en 443), ongeacht de oorsprongspoorten.

Laten we terug naar het hypothetische voorbeeld met `http://www.contoso.com:8080/file.txt`.  Als u onthouden wilt, Contoso opgegeven `8080` als hun HTTP-poort, maar we ook wordt ervan uitgegaan dat ze opgegeven `44300` als hun HTTPS-poort.  Als ze een eindpunt met de naam gemaakt `contoso`, de hostnaam van het CDN-eindpunt zou worden `contoso.azureedge.net`.  Een aanvraag voor `http://contoso.azureedge.net/file.txt` een HTTP-aanvraag is dus het eindpunt HTTP via poort 8080 gebruikt te halen vanuit de oorsprong.  Beveiligde aanvragen via HTTPS, `https://contoso.azureedge.net/file.txt`, ervoor zou zorgen dat het eindpunt voor het gebruik van HTTPS op poort 44300 wanneer het bestand vanuit de oorsprong op te halen.

#### <a name="origin-host-header"></a>Host-header van oorsprong
De **host-header van oorsprong** is de waarde van de host-header verzonden naar de oorsprong met elke aanvraag.  In de meeste gevallen dit moet hetzelfde zijn als de **de hostnaam van oorsprong** we eerder gecontroleerd.  Een onjuiste waarde in dit veld niet in het algemeen toe leiden dat 404-statusberichten, maar is waarschijnlijk tot andere statussen 4xx, afhankelijk van wat de oorsprong verwacht.

#### <a name="origin-path"></a>Pad voor de oorsprong
Ten slotte moet controleren we onze **oorsprongpad**.  Dit is standaard leeg.  Gebruik dit veld alleen als u wilt beperken van het bereik van de oorsprong gehoste bronnen die u beschikbaar wilt maken op de CDN.  

Bijvoorbeeld in mijn eindpunt gewenste alle bronnen op mijn storage-account worden beschikbaar is, zodat ik links **oorsprongpad** leeg.  Dit betekent dat een aanvraag voor `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` resulteert in een verbinding van mijn eindpunt `cdndocdemo.core.windows.net` die aanvragen `/publicblob/lorem.txt`.  Ook kan een aanvraag voor `https://cdndocdemo.azureedge.net/donotcache/status.png` resulteert in het aanvragen van het eindpunt `/donotcache/status.png` vanuit de oorsprong.

Maar wat gebeurt er als ik wil niet de CDN voor elk pad op mijn oorsprong gebruiken?  Zeg I wilde slechts om weer te geven de `publicblob` pad.  Als ik invoert */publicblob* in mijn **oorsprongpad** veld, waardoor het eindpunt invoegen */publicblob* voor elke aanvraag naar de oorsprong wordt gemaakt.  Dit betekent dat de aanvraag voor `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nu daadwerkelijk duurt het gedeelte van de aanvraag van de URL `/publicblob/lorem.txt`, en toevoeg- `/publicblob` aan het begin. Dit resulteert in een aanvraag voor `/publicblob/publicblob/lorem.txt` vanuit de oorsprong.  Als dat het pad kan niet worden omgezet naar een bestand, worden de oorsprong 404 status geretourneerd.  De juiste URL voor het ophalen van lorem.txt in dit voorbeeld zou daadwerkelijk worden `https://cdndocdemo.azureedge.net/lorem.txt`.  Let op: Er zijn niet opgenomen in de */publicblob* pad helemaal, omdat het gedeelte van de aanvraag van de URL is `/lorem.txt` en het eindpunt wordt toegevoegd `/publicblob`, wat resulteert in `/publicblob/lorem.txt` de aanvraag wordt doorgegeven naar de oorsprong.

