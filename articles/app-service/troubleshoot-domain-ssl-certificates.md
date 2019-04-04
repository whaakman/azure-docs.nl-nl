---
title: Problemen oplossen met domein en SSL-certificaten - Azure App Service | Microsoft Docs
description: Problemen met domein en SSL-certificaat oplossen in Azure App Service
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 8ae6c9d5238f2853a12c20edfd3dba6d3f529b2c
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905814"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-app-service"></a>Problemen met domein en SSL-certificaat oplossen in Azure App Service

Dit artikel worden veelvoorkomende problemen die u tegenkomen kunt wanneer u een domein of een SSL-certificaat voor uw web-apps in Azure App Service configureren. Ook wordt beschreven mogelijke oorzaken en oplossingen voor deze problemen.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en selecteer **ontvang ondersteuning**.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="certificate-problems"></a>Certificaatproblemen

### <a name="you-cant-add-an-ssl-certificate-binding-to-an-app"></a>U kunt een SSL-certificaat-binding niet toevoegen aan een app 

#### <a name="symptom"></a>Symptoom

Wanneer u een SSL-binding toevoegt, wordt de volgende strekking weergegeven:

"Het SSL-binding toevoegen is mislukt. Kan geen certificaat instellen voor de bestaande VIP omdat een andere VIP wordt al gebruikt voor dat certificaat."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als u meerdere IP-gebaseerd SSL-bindingen voor hetzelfde IP-adres voor meerdere apps hebt. App A heeft bijvoorbeeld een op IP gebaseerde SSL met een oud certificaat. App B is een op IP gebaseerde SSL met een nieuw certificaat voor hetzelfde IP-adres. Wanneer u de app SSL-binding niet met het nieuwe certificaat bijwerken, is het vanwege de volgende fout mislukt omdat hetzelfde IP-adres wordt gebruikt voor een andere app. 

#### <a name="solution"></a>Oplossing 

U lost dit probleem, moet u een van de volgende methoden gebruiken:

- De IP-gebaseerd SSL-binding op de app die gebruikmaakt van het oude certificaat verwijderd. 
- Maak een nieuwe IP-gebaseerd SSL-binding die gebruikmaakt van het nieuwe certificaat.

### <a name="you-cant-delete-a-certificate"></a>U kunt een certificaat niet verwijderen 

#### <a name="symptom"></a>Symptoom

Wanneer u probeert om een certificaat te verwijderen, ontvangt u de volgende strekking weergegeven:

'Kan niet het certificaat niet verwijderen omdat deze momenteel wordt gebruikt in een SSL-binding. De SSL-binding moet worden verwijderd voordat u het certificaat kunt verwijderen."

#### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als het certificaat wordt gebruikt door een andere app.

#### <a name="solution"></a>Oplossing

De SSL-binding voor dat certificaat uit de apps verwijderen. Probeer te verwijderen van het certificaat. Als u het certificaat niet verwijderen, schakelt u de cache van de browser internet en opnieuw openen van de Azure portal in een nieuw browservenster. Probeer te verwijderen van het certificaat.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Kunt u aanschaffen een App Service certificate niet 

#### <a name="symptom"></a>Symptoom
U kunt geen koopt een [Azure App Service certificate](./web-sites-purchase-ssl-web-site.md) vanuit Azure portal.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing
Dit probleem kan optreden voor het gebruik van de volgende redenen:

- De App Service-plan is gratis of gedeeld. Deze Prijscategorieën ondersteuning geen voor SSL. 

    **Oplossing**: De App Service-plan voor app upgraden naar Standard.

- Het abonnement beschikt niet over een geldige creditcard.

    **Oplossing**: Een geldige creditcard aan uw abonnement toevoegt. 

- Het abonnement biedt geen ondersteuning voor een App Service-certificaat, zoals Microsoft Student aanschaffen.  

    **Oplossing**: Upgrade van uw abonnement. 

- Het abonnement bereikt de limiet van aankopen die zijn toegestaan op een abonnement.

    **Oplossing**: App Service-certificaten hebben een limiet van 10 certificaat aankopen voor het abonnement betalen per gebruik en EA. Voor andere abonnementstypen is de limiet van 3. Als u wilt de limiet te verhogen, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het App Service-certificaat is gemarkeerd als fraude. U hebt ontvangen het volgende foutbericht: 'Uw certificaat is gemarkeerd voor mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet gebruikt binnen 24 uur wordt, neem contact op met ondersteuning voor Azure."

    **Oplossing**: Als het certificaat is gemarkeerd als fraude en na 24 uur niet is opgelost, volgt u deze stappen:

    1. Meld u aan bij [Azure Portal](https://portal.azure.com).
    2. Ga naar **App Service-certificaten**, en selecteer het certificaat.
    3. Selecteer **configuratie van het certificaat** > **stap 2: Controleer of** > **domeinverificatie**. Deze stap verzendt een e-mailbericht naar de provider van het certificaat van Azure om het probleem te verhelpen.

## <a name="custom-domain-problems"></a>Problemen met aangepast domein

### <a name="a-custom-domain-returns-a-404-error"></a>Een aangepast domein retourneert een 404-fout 

#### <a name="symptom"></a>Symptoom

Wanneer u naar de site bladert met behulp van de aangepaste domeinnaam, ontvangt u de volgende strekking weergegeven:

"Fout 404-Web-app niet vinden."

#### <a name="cause-and-solution"></a>Oorzaak en oplossing

**Oorzaak 1** 

Het aangepaste domein die u hebt geconfigureerd, een CNAME- of A-record ontbreekt. 

**Oplossing voor oorzaak 1**

- Als u een A-record hebt toegevoegd, zorg ervoor dat er ook een TXT-record wordt toegevoegd. Zie voor meer informatie, [A-record maken](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Als u geen gebruik van het hoofddomein voor uw app, wordt u aangeraden dat u een CNAME-record in plaats van een A-record gebruiken.
- Een CNAME-record en een A-record niet gebruiken voor hetzelfde domein bevinden. Dit probleem kan een conflict veroorzaken en te voorkomen dat het domein worden omgezet. 

**Oorzaak 2** 

De browser internet mogelijk nog steeds de oude IP-adres voor uw domein caching. 

**Oplossing voor oorzaak 2**

Schakel de browser. Voor Windows-apparaten, kunt u de opdracht uitvoeren `ipconfig /flushdns`. Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de app verwijst. 

### <a name="you-cant-add-a-subdomain"></a>U kunt geen een subdomein toevoegen 

#### <a name="symptom"></a>Symptoom

U kunt een nieuwe hostnaam niet toevoegen aan een app een subdomein toewijzen.

#### <a name="solution"></a>Oplossing

- Neem contact op met de beheerder van abonnement om ervoor te zorgen dat u gemachtigd bent de naam van een host toevoegt aan de app.
- Als u meer subdomeinen nodig hebt, raden wij u wijzigt de domeinen te hosten op Azure Domain Name Service (DNS). U kunt met behulp van Azure DNS, 500 hostnamen toevoegen aan uw app. Zie voor meer informatie, [een subdomein toevoegen](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).

### <a name="dns-cant-be-resolved"></a>DNS kan niet worden omgezet

#### <a name="symptom"></a>Symptoom

U hebt ontvangen het volgende foutbericht:

"De DNS-record kan niet worden gevonden."

#### <a name="cause"></a>Oorzaak
Dit probleem doet zich voor een van de volgende redenen:

- Time to live (TTL) periode niet is verlopen. Controleer de DNS-configuratie voor uw domein om te bepalen van de TTL-waarde, en wacht totdat de periode is verlopen.
- De DNS-configuratie is onjuist.

#### <a name="solution"></a>Oplossing
- Wacht tot 48 uur voor dit probleem op te lossen zelf.
- Als u de TTL-instelling in de DNS-configuratie wijzigen kunt, wijzigt u de waarde in vijf minuten om te zien of hiermee het probleem is opgelost.
- Gebruik [WhatsmyDNS.net](https://www.whatsmydns.net/) om te controleren of uw domein naar het IP-adres van de app verwijst. Als dat niet het geval is, configureert u de A-record naar het juiste IP-adres van de app.

### <a name="you-need-to-restore-a-deleted-domain"></a>U moet een verwijderde domein herstellen 

#### <a name="symptom"></a>Symptoom
Uw domein is niet meer zichtbaar in de Azure portal.

#### <a name="cause"></a>Oorzaak 
De eigenaar van het abonnement heeft mogelijk per ongeluk het domein verwijderd.

#### <a name="solution"></a>Oplossing
Als uw domein is minder dan zeven dagen geleden hebt verwijderd, is de verwijdering niet nog gestart door het domein. In dit geval kunt u hetzelfde domein opnieuw op de Azure-portal onder hetzelfde abonnement kopen. (Zorg ervoor dat de exacte domeinnaam in het zoekvak typt.) U wordt niet in rekening gebracht opnieuw voor dit domein. Als het domein is meer dan zeven dagen geleden hebt verwijderd, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor hulp bij het herstellen van het domein.

## <a name="domain-problems"></a>Problemen met domein

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>U hebt aangeschaft met een SSL-certificaat voor het verkeerde domein

#### <a name="symptom"></a>Symptoom

U hebt aangeschaft voor een App Service-certificaat voor het verkeerde domein. U kunt het certificaat voor het gebruik van het juiste domein niet bijwerken.

#### <a name="solution"></a>Oplossing

Certificaat verwijderen en vervolgens een nieuw certificaat kopen.

Als het huidige certificaat die gebruikmaakt van het verkeerde domein zich in de status "Verleend", moet u ook worden gefactureerd voor dat certificaat. App Service-certificaten zijn niet worden terugbetaald, maar u kunt contact opnemen met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om te zien of er nog andere opties. 

### <a name="an-app-service-certificate-was-renewed-but-the-app-shows-the-old-certificate"></a>Een App Service-certificaat is vernieuwd, maar de app ziet u het oude certificaat 

#### <a name="symptom"></a>Symptoom

Het App Service-certificaat is vernieuwd, maar de app die gebruikmaakt van het App Service-certificaat is nog steeds met behulp van het oude certificaat. U hebt ontvangen ook een waarschuwing dat het HTTPS-protocol vereist is.

#### <a name="cause"></a>Oorzaak 
Azure App Service een achtergrondtaak elke acht uur wordt uitgevoerd en de certificaatresource wordt gesynchroniseerd als er wijzigingen zijn. Wanneer u draaien of bijwerken van een certificaat, soms de toepassing is nog steeds bezig met ophalen van het oude certificaat en niet het bijgewerkte certificaat. De reden is dat de taak voor het synchroniseren van de certificaatresource nog niet is uitgevoerd. 
 
#### <a name="solution"></a>Oplossing

U kunt afdwingen dat een synchronisatie van het certificaat:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer **App Service-certificaten**, en selecteer vervolgens het certificaat.
2. Selecteer **opnieuw versleutelen en synchroniseren**, en selecteer vervolgens **synchronisatie**. De synchronisatie duurt enige tijd om te voltooien. 
3. Als de synchronisatie is voltooid, ziet u de volgende melding: "Zijn bijgewerkt alle resources met het nieuwste certificaat."

### <a name="domain-verification-is-not-working"></a>Domeinverificatie werkt niet 

#### <a name="symptom"></a>Symptoom 
Het App Service-certificaat vereist domeinverificatie uit te voeren voordat het certificaat klaar is voor gebruik. Wanneer u selecteert **controleren**, het proces is mislukt.

#### <a name="solution"></a>Oplossing
Handmatig controleren of uw domein door een TXT-record toe te voegen:
 
1.  Ga naar de Domain Name Service (DNS)-provider die als host fungeert voor de domeinnaam van uw.
2.  Toevoegen van een TXT-record voor uw domein dat gebruikmaakt van de waarde van het domein-token dat wordt weergegeven in de Azure-portal. 

Wacht een paar minuten voor DNS-doorgifte uitvoeren en selecteer vervolgens de **vernieuwen** knop voor het activeren van de verificatie. 

Als alternatief kunt u de HTML-webpagina methode handmatig controleren of uw domein. Deze methode kunt de certificeringsinstantie te bevestigen dat het domeineigendom van het domein dat het certificaat is verleend.

1.  Maak een HTML-bestand met de naam {domeinverificatietoken} .html. De inhoud van dit bestand moet de waarde van het domeinverificatietoken zijn.
3.  Upload dit bestand in de hoofdmap van de webserver die als host voor uw domein fungeert.
4.  Selecteer **vernieuwen** om de certificaatstatus te controleren. Het duurt enkele minuten voor verificatie te voltooien.

Bijvoorbeeld, als u een standaard-certificaat voor azure.com met het domein verificatie token waarde 1234abcd, een webaanvraag gedaan bij https://azure.com/1234abcd.html moet de waarde 1234abcd retourneren. 

> [!IMPORTANT]
> De volgorde van een certificaat heeft slechts 15 dagen om het domein verificatie-bewerking te voltooien. Na 15 dagen, de certificeringsinstantie het certificaat geweigerd en u niet worden in rekening gebracht voor het certificaat. In dit geval verwijderen van dit certificaat en probeer het opnieuw.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Kunt u aanschaffen niet een domein

#### <a name="symptom"></a>Symptoom
U kunt een App Service-domein in Azure portal kan niet kopen.

#### <a name="cause-and-solution"></a>Oorzaak en oplossing

Dit probleem doet zich voor een van de volgende redenen:

- Er is geen creditcard is geregistreerd op het Azure-abonnement of de creditcard is ongeldig.

    **Oplossing**: Een geldige creditcard aan uw abonnement toevoegt.

- U bent niet de eigenaar van het abonnement, zodat u bent niet gemachtigd om aan te schaffen van een domein.

    **Oplossing**: [De rol van eigenaar toewijzen](../role-based-access-control/role-assignments-portal.md) aan uw account. Of neem contact op met de beheerder van het abonnement om machtiging voor het kopen van een domein.
- U hebt de limiet voor het aanschaffen van domeinen in uw abonnement bereikt. De huidige limiet is 20.

    **Oplossing**: Om aan te vragen een uitbreiding van de limiet, neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Het type Azure-abonnement biedt geen ondersteuning voor de aanschaf van een App Service-domein.

    **Oplossing**: Uw Azure-abonnement upgraden naar een ander abonnementstype, zoals een abonnement met betalen per gebruik.

### <a name="you-cant-add-a-host-name-to-an-app"></a>U kunt een hostnaam niet toevoegen aan een app 

#### <a name="symptom"></a>Symptoom

Wanneer u een hostnaam toevoegt, mislukt het proces om te valideren en controleer of het domein.

#### <a name="cause"></a>Oorzaak 

Dit probleem doet zich voor een van de volgende redenen:

- U bent niet gemachtigd om toe te voegen een hostnaam.

    **Oplossing**: Vraag de beheerder van het abonnement u om toestemming te geven de hostnaam van een toe te voegen.
- Uw domeineigendom kan niet worden geverifieerd.

    **Oplossing**: Controleren of uw CNAME- of een record correct is geconfigureerd. Als u wilt een aangepast domein toewijzen aan een app, een CNAME-record of een A-record te maken. Als u een hoofddomein gebruiken wilt, moet u een en TXT-records:

    |Recordtype|Host|Wijs|
    |------|------|-----|
    |A|@|IP-adres voor een app|
    |TXT|@|<app-name>.azurewebsites.net|
    |CNAME|www|<app-name>.azurewebsites.net|

## <a name="faq"></a>Veelgestelde vragen

**Heb ik mijn aangepaste domein configureren voor mijn website wanneer ik het kopen?**

Wanneer u een domein van de portal voor Azure koopt, wordt de App-servicetoepassing wordt automatisch geconfigureerd voor het gebruik van het aangepaste domein. U hebt geen eventuele extra stappen uitvoeren. Bekijk voor meer informatie, [Azure App Service zelf helpen: Een aangepaste domeinnaam toevoegen](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name) op Channel 9.

**Kan ik een domein hebt aangeschaft in de Azure-portal gebruiken om te verwijzen naar een Azure-VM in plaats daarvan?**

Ja, kunt u het domein verwijzen naar een virtuele machine. Zie [Use Azure DNS to provide custom domain settings for an Azure service](../dns/dns-custom-domain.md) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

**Wordt mijn domein gehost door GoDaddy of Azure DNS?**

App Service-domeinen worden GoDaddy voor domeinregistratie en Azure DNS gebruiken voor het hosten van de domeinen. 

**Ik heb voor automatisch verlengen ingeschakeld, maar nog steeds een verlenging kennisgeving voor mijn domein via e-mail te ontvangen. Wat moet ik doen?**

Als u hebt voor automatisch verlengen ingeschakeld, u niet hoeft geen actie te ondernemen. De melding wordt geboden om u te informeren dat het domein is bijna verloopt en handmatig vernieuwen als u automatisch verlengen niet is ingeschakeld.

**Moet ik betalen voor Azure DNS hosten van mijn domein?**

De initiële kosten van inkoop domein geldt voor domeinregistratie alleen. Naast de kosten voor registratie zijn er er kosten in rekening gebracht voor Azure DNS op basis van uw gebruik. Zie voor meer informatie, [prijzen voor Azure DNS](https://azure.microsoft.com/pricing/details/dns/) voor meer informatie.

**Kan ik mijn domein eerder vanuit Azure portal hebt aangeschaft en wilt overstappen van GoDaddy die als host fungeert voor het beheren van Azure DNS. Hoe kan ik dit doen?**

Het is niet verplicht om te migreren naar Azure DNS hosten. Als u migreren naar Azure DNS, de ervaring in Azure portal domein wilt over vindt informatie over de stappen die nodig zijn om te verplaatsen naar Azure DNS. Als het domein is gekocht via App Service, is de migratie van GoDaddy naar Azure DNS hosten relatief naadloze procedure.

**Ik wil mijn domein van App Service-domein kopen, maar kan ik mijn domein op GoDaddy in plaats van Azure DNS hosten?**

Vanaf 24 juli 2017, worden App Service-domeinen die zijn aangeschaft in de portal gehost op Azure DNS. Als u liever een andere hosting provider gebruikt, moet u gaan naar de website van verkrijgen van een oplossing voor hosting-domein.

**Heb ik betalen voor privacybescherming voor mijn domein?**

Wanneer u een domein via Azure portal hebt gekocht, kunt u kiezen privacy toevoegen zonder extra kosten. Dit is een van de voordelen van het aanschaffen van uw domein door middel van Azure App Service.

**Als ik besluit dat ik mijn domein niet meer wilt, vind ik mijn geld terug?**

Wanneer u een domein koopt, betaalt u geen gedurende een periode van vijf dagen, gedurende die tijd u bepalen kunt dat u niet dat het domein wilt. Als u dat u niet wilt dat het domein in die periode van vijf dagen besluit, u betaalt geen. (.uk domeinen vormen een uitzondering hierop. Als u een domein .uk koopt, in rekening worden gebracht direct en kan niet worden terugbetaald.)

**Kan ik het domein in een andere Azure App Service-app gebruiken in mijn abonnement?**

Ja. Wanneer u de blade voor aangepaste domeinen en SSL in Azure portal opent, ziet u de domeinen die u hebt aangeschaft. U kunt uw app voor het gebruik van een van deze domeinen configureren.

**Kan ik een domein in één abonnement overbrengen naar een ander abonnement?**

U kunt een domein verplaatsen naar een ander abonnement/resource-groep met de [verplaatsen AzResource](https://docs.microsoft.com/powershell/module/az.Resources/Move-azResource) PowerShell-cmdlet.

**Hoe kan ik mijn aangepaste domein beheren als ik een Azure App Service-app op dit moment geen?**

Zelfs als u een App Service Web-App niet hebt, kunt u uw domein beheren. Domein kan worden gebruikt voor Azure-services zoals virtuele machine, opslag enzovoort. Als u van plan bent met het domein voor App Service Web Apps, moet u een Web-App die zich niet in de gratis App Service-plan als u het domein koppelen aan uw web-app wilt opnemen.

**Kan ik een web-app met een aangepast domein naar een ander abonnement of verplaatsen van App Service Environment v1 in V2?**

Ja, kunt u uw web-app verplaatsen tussen abonnementen. Volg de instructies in [over het verplaatsen van resources in Azure](../azure-resource-manager/resource-group-move-resources.md). Er zijn enkele beperkingen bij het verplaatsen van de web-app. Zie voor meer informatie, [beperkingen voor het verplaatsen van resources van App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations
).

Na het verplaatsen van de web-app, moeten de host naam bindingen van de domeinen in de aangepaste domeinen instellen blijven hetzelfde. Geen extra stappen zijn vereist om de bindingen van de naam host configureren.
