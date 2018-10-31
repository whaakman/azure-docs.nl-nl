---
title: Veelgestelde vragen over de configuratie voor Azure WebApps | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over problemen met configuratie en beheer voor de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 8cc47fc7ec60027851525b98cdf95e7d534ea556
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241851"
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Configuratie en beheer Veelgestelde vragen voor Web-Apps in Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over problemen met configuratie en beheer voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-to-move-app-service-resources"></a>Zijn er beperkingen waarop die ik letten moet als u wilt verplaatsen van resources van App Service?

Als u van plan bent een App Service-resources verplaatsen naar een nieuwe resourcegroep of abonnement, zijn er enkele beperkingen voor het op de hoogte zijn. Zie voor meer informatie, [beperkingen voor App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Hoe gebruik ik een aangepaste domeinnaam voor mijn web-app?

Voor antwoorden op veelgestelde vragen over het gebruik van een aangepaste domeinnaam met uw Azure-web-app, Zie onze 7 minuten durende video [een aangepaste domeinnaam toevoegen](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). De video biedt een overzicht van hoe u een aangepaste domeinnaam toevoegen. Dit wordt beschreven hoe u uw eigen URL in plaats van de *. azurewebsites.net-URL met uw App Service-web-app. Ook ziet u een gedetailleerd overzicht van [hoe u een aangepaste domeinnaam toewijzen](app-service-web-tutorial-custom-domain.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Hoe koop ik een nieuw aangepast domein voor mijn web-app?

Zie voor informatie over het aanschaffen en het instellen van een aangepast domein voor uw App Service-web-app, [kopen en configureren van een aangepaste domeinnaam in App Service](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Hoe kan ik uploaden en een bestaand SSL-certificaat configureren voor mijn web-app?

Zie voor meer informatie over het uploaden en het instellen van een bestaand aangepast SSL-certificaat, [een bestaand aangepast SSL-certificaat koppelen aan een Azure-web-app](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Hoe kan ik kopen en configureren van een nieuwe SSL-certificaat in Azure voor mijn web-app?

Zie voor informatie over het aanschaffen en het instellen van een SSL-certificaat voor uw App Service-web-app, [een SSL-certificaat toevoegen aan uw App Service-app](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Hoe verplaats ik Application Insights-resources

Op dit moment ondersteunt Azure Application Insights niet de verplaatsing. Als de oorspronkelijke resourcegroep een Application Insights-resource bevat, kunt u deze resource niet verplaatsen. Als u de Application Insights-resource opgeeft wanneer u probeert te verplaatsen van een App Service-app, verplaats de hele bewerking mislukt. Application Insights en de App Service-plan hoeft echter niet te worden in dezelfde resourcegroep bevinden als de app voor de app te laten functioneren.

Zie voor meer informatie, [beperkingen voor App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Waar kan ik een controlelijst die richtlijnen en meer informatie over resource bewerkingen verplaatsen?

[Beperkingen voor App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) ziet u hoe u resources wilt verplaatsen naar een nieuw abonnement of naar een nieuwe resourcegroep in hetzelfde abonnement. U kunt informatie over de controlelijst voor het verplaatsen van de resource, meer informatie over welke services ondersteunt de verplaatsing en meer informatie over de beperkingen voor App Service en andere onderwerpen.

## <a name="how-do-i-set-the-server-time-zone-for-my-web-app"></a>Hoe stel ik de tijdzone van de server voor mijn web-app?

De tijdzone van de server voor uw web-app instellen:

1. In de Azure-portal in uw App Service-abonnement, gaat u naar de **toepassingsinstellingen** menu.
2. Onder **App-instellingen**, voegt u deze instelling:
    * Sleutel = WEBSITE_TIME_ZONE
    * Waarde = *de tijdzone die u wilt*
3. Selecteer **Opslaan**.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Waarom mijn doorlopende webtaken soms mislukt?

Standaard worden web-apps uit het geheugen verwijderd als ze gedurende een bepaalde tijd niet actief zijn. Hiermee wordt het systeem te besparen. In de Basic en Standard-abonnement, kunt u inschakelen de **Always On** instellen om te voorkomen dat de WebApp steeds geladen. Als uw web-app wordt uitgevoerd doorlopende webtaken, schakelt u **Always On**, of de webtaken mogelijk niet betrouwbaar uit te voeren. Zie voor meer informatie, [maken van een doorlopend uitgevoerde webtaak](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-the-outbound-ip-address-for-my-web-app"></a>Hoe krijg ik het uitgaande IP-adres voor mijn web-app?

De lijst met uitgaande IP-adressen voor uw web-app ophalen:

1. In de Azure-portal op de blade van uw web-app, gaat u naar de **eigenschappen** menu.
2. Zoeken naar **uitgaande ip-adressen**.

De lijst met uitgaande IP-adressen wordt weergegeven.

Zie voor meer informatie over het verkrijgen van de uitgaande IP-adres als uw website wordt gehost in een App Service Environment, [uitgaande netwerkadressen](environment/app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Hoe krijg ik een gereserveerd of toegewezen inkomende IP-adres voor mijn web-app?

Als u een toegewezen of gereserveerd IP-adres voor binnenkomende oproepen naar de website van uw Azure-app instelt, installeren en configureren van een IP-gebaseerd SSL-certificaat.

Houd er rekening mee dat voor het gebruik van een toegewezen of gereserveerd IP-adres voor binnenkomende oproepen, uw App Service-plan moet zich in een standaard of hoger service-plan.

## <a name="can-i-export-my-app-service-certificate-to-use-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Kan ik mijn App Service-certificaat voor gebruik buiten Azure, zoals voor een website die wordt gehost elders exporteren? 

App Service-certificaten worden beschouwd als Azure-resources. Ze zijn niet bedoeld voor gebruik buiten uw Azure-services. U kunt deze voor gebruik buiten Azure kan niet exporteren. Zie voor meer informatie, [Veelgestelde vragen over App Service-certificaten en aangepaste domeinen](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-to-use-with-other-azure-cloud-services"></a>Kan ik mijn App Service-certificaat moet worden gebruikt met andere Azure-cloud-services exporteren?

De portal biedt een eersteklas ervaring voor het implementeren van een App Service certificate via Azure Key Vault in App Service-apps. Echter, we hebben ontvangen aanvragen van klanten voor het gebruik van deze certificaten buiten de App Service-platform, bijvoorbeeld met Azure Virtual Machines. Zie voor meer informatie over het maken van een lokaal PFX-kopie van uw App Service-certificaat zodat u het certificaat met andere Azure-resources gebruiken kunt, [maken van een lokale kopie van het pfx-van een App Service certificate](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Zie voor meer informatie, [Veelgestelde vragen over App Service-certificaten en aangepaste domeinen](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-the-message-partially-succeeded-when-i-try-to-back-up-my-web-app"></a>Waarom zie ik het bericht 'Gedeeltelijk voltooid' wanneer ik wil back-up van mijn web-app?

Een veelvoorkomende oorzaak van de back-fout is dat bepaalde bestanden gebruikt door de toepassing worden. Bestanden die gebruikt worden zijn vergrendeld terwijl u de back-up uitvoert. Dit voorkomt dat deze bestanden back-up wordt gemaakt en kan leiden tot een status 'Gedeeltelijk voltooid'. U kunt mogelijk te voorkomen dat dit voorkomen door bestanden uitsluiten van het back-upproces. U kunt back-up alleen wat u nodig hebt. Zie voor meer informatie, [back-up van alleen de belangrijke onderdelen van uw site met Azure WebApps](http://zainrizvi.io/blog/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-the-http-response"></a>Hoe verwijder ik een koptekst van het HTTP-antwoord?

Als u wilt de headers van het HTTP-antwoord verwijderen, bijwerken van uw site web.config-bestand. Zie voor meer informatie, [Standard van SQL server-headers van uw Azure-websites verwijderen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>App Service compatibel is met PCI Standard 3.0 en 3.1?

De functie Web Apps van Azure App Service is momenteel in overeenstemming met PCI Data Security Standard (DSS) versie 3.0 Level 1. PCI DSS versie 3.1 is in de planning. Planning wordt al uitgevoerd voor hoe de toepassing van de meest recente standaard wordt voortgezet.

PCI DSS-certificering voor versie 3.1 vereist Transport Layer Security (TLS) 1.0 uitschakelen. TLS 1.0 uitschakelen is op dit moment geen een optie voor de meeste App Service-plannen. Als u App Service-omgeving of bereid bent te migreren van uw werkbelasting naar App Service-omgeving, kunt u echter meer controle over uw omgeving krijgen. Dit omvat het TLS 1.0 uitschakelen contact opnemen met de ondersteuning van Azure. In de nabije toekomst wij van plan bent om deze instellingen toegankelijk voor gebruikers.

Zie voor meer informatie, [Microsoft Azure App Service web app-naleving van PCI Standard 3.0 en 3.1](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-the-staging-environment-and-deployment-slots"></a>Hoe gebruik ik de staging-omgeving en implementatiesleuven?

In de Standard- en Premium App Service-abonnementen, wanneer u uw web-app in App Service implementeren, kunt u implementeren op een afzonderlijke implementatiesite in plaats van naar de standaard-productiesite. Implementatiesleuven zijn live web-apps waarvoor hun eigen hostnamen. Web-app-inhoud en configuratie-elementen kunnen worden gewisseld tussen twee implementatiesites, inclusief de productiesite.

Zie voor meer informatie over het gebruik van implementatiesites [instellen van een faseringsomgeving in App Service](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Hoe kan ik toegang tot en WebJob-logboeken bekijken?

WebJob-logboeken bekijken:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer de webtaak.
3. Selecteer de **in-/ uitschakelen uitvoer** knop.
4. Voor het downloaden van het uitvoerbestand, selecteer de **downloaden** koppeling.
5. Selecteer voor afzonderlijke wordt uitgevoerd, **afzonderlijke aanroepen**.
6. Selecteer de **in-/ uitschakelen uitvoer** knop.
7. Selecteer de downloadkoppeling.

## <a name="im-trying-to-use-hybrid-connections-with-sql-server-why-do-i-see-the-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>Ik wil hybride verbindingen met SQL Server gebruiken. Waarom zie ik het bericht ' System.OverflowException: rekenkundige bewerking heeft geresulteerd in een overloop '?

Als u hybride verbindingen gebruiken voor toegang tot SQL Server, een Microsoft .NET-update op 10 mei 2016, kan leiden tot verbindingen mislukken. U kunt dit bericht ziet:

```
Exception: System.Data.Entity.Core.EntityException: The underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Oplossing

De uitzondering is veroorzaakt door een probleem met de Hybrid Connection Manager die sindsdien zijn opgelost. Zorg ervoor dat u [bijwerken van uw hybride Verbindingsbeheer](https://go.microsoft.com/fwlink/?LinkID=841308) om dit probleem te verhelpen.

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Hoe ik toevoegen of bewerken van een herschrijvingsregel voor URL?

Toevoegen of bewerken van een herschrijvingsregel voor URL:

1. Instellen van Internet Information Services (IIS) Manager zodat deze verbinding met uw App Service-web-app maakt. Zie voor informatie over het IIS-beheer verbinding met App Service, [extern beheer van Azure websites met behulp van IIS-beheer](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. In de IIS-beheer toevoegen of bewerken van een herschrijvingsregel voor URL. Zie voor informatie over het toevoegen of bewerken van een herschrijvingsregel voor URL, [herschrijvingsregels maken voor de URL van de module herschrijven](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-to-app-service"></a>Hoe beheers ik het binnenkomende verkeer naar App Service?

Op het siteniveau van de hebt u twee opties voor het beheren van inkomend verkeer naar App Service:

* Dynamische IP-beperkingen inschakelen. Zie voor meer het inschakelen van dynamische IP-adresbeperkingen [IP- en domeinbeperkingen voor Azure websites](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Schakel beveiliging van de Module. Zie voor meer het inschakelen van Module beveiliging [ModSecurity web application firewall op Azure websites](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

Als u App Service-omgeving gebruikt, kunt u [Barracuda firewall](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Hoe ik de poorten in een App Service-web-app blokkeren?

In de omgeving van App Service gedeelde tenant is het niet mogelijk om te blokkeren van bepaalde poorten vanwege de aard van de infrastructuur. TCP-poorten 4016 4018 en 4020 mogelijk ook openen voor externe foutopsporing van Visual Studio.

In App Service-omgeving hebt u volledige controle over binnenkomend en uitgaand verkeer. Netwerkbeveiligingsgroepen kunt u beperken of specifieke poorten blokkeren. Zie voor meer informatie over App Service-omgeving, [Kennismaking met App Service Environment](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Hoe ik een F12-tracering vastleggen?

U hebt twee opties voor het vastleggen van een F12-tracering:

* F12 HTTP-tracering
* F12 console-uitvoer

### <a name="f12-http-trace"></a>F12 HTTP-tracering

1. Ga naar uw website in Internet Explorer. Het is belangrijk dat u zich aanmelden voordat u de volgende stappen uitvoert. Anders bevat de F12-tracering gevoelige gegevens voor aanmelden.
2. Druk op F12.
3. Controleer de **netwerk** tabblad is geselecteerd en selecteer vervolgens de groene **afspelen** knop.
4. Voer de stappen uit die het probleem te reproduceren.
5. Selecteer de rode **stoppen** knop.
6. Selecteer de **opslaan** knop (schijfpictogram van de) en sla het HAR-bestand (in Internet Explorer en Microsoft Edge) *of* met de rechtermuisknop op het HAR-bestand en selecteer vervolgens **opslaan als HAR met inhoud** (in Chrome).

### <a name="f12-console-output"></a>F12 console-uitvoer

1. Selecteer de **Console** tabblad.
2. Voor elk tabblad die groter dan nul items bevat, selecteer het tabblad (**fout**, **waarschuwing**, of **informatie**). Als het tabblad niet is geselecteerd, is het pictogram tabblad grijs of zwart wanneer u de cursor vandaan lopen.
3. Klik in het berichtgedeelte aan het deelvenster met de rechtermuisknop en selecteer vervolgens **Kopieer alle**.
4. Plak de gekopieerde tekst in een bestand en sla het bestand.

Als u wilt een HAR-bestand bekijkt, kunt u de [HAR viewer](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-to-connect-an-app-service-web-app-to-a-virtual-network-that-is-connected-to-expressroute"></a>Waarom krijg ik een foutbericht wanneer ik wil verbinding maken met een App Service web-app voor een virtueel netwerk dat is verbonden met ExpressRoute?

Als u probeert een Azure-web-app kunt koppelen aan een virtueel netwerk dat is verbonden met Azure ExpressRoute, mislukt de bewerking. Het volgende bericht weergegeven: 'Gateway is niet een VPN-gateway'.

U kunt op dit moment geen punt-naar-site VPN-verbindingen met een virtueel netwerk dat is verbonden met ExpressRoute. Een punt-naar-site VPN en ExpressRoute kunnen niet worden gecombineerd voor hetzelfde virtuele netwerk. Zie voor meer informatie, [ExpressRoute en site-naar-site VPN-verbindingen limieten en beperkingen](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-to-a-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Hoe maak ik een App Service-web-app verbinding met een virtueel netwerk met een statische routering (op beleid gebaseerd)-gateway?

Op dit moment wordt een App Service-web-app verbinden met een virtueel netwerk met een statische routering (op beleid gebaseerd)-gateway niet ondersteund. Als uw virtuele doelnetwerk al bestaat, moet de punt-naar-site VPN is ingeschakeld, met een gateway voor dynamische routering, voordat deze kan worden verbonden met een app hebben. Als uw gateway is ingesteld op statische routering, kunt u een punt-naar-site-VPN-verbinding niet inschakelen. 

Zie voor meer informatie, [een app integreren met een Azure virtual network](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>In mijn App Service Environment, waarom kan ik maken slechts één App Service-plan, hoewel ik over twee workers beschik?

Als u wilt fouttolerantie, vereist App Service-omgeving dat elke workergroep ten minste één extra rekenresource zijn moet. De aanvullende compute-resource kan niet worden toegewezen als een werkbelasting.

Zie voor meer informatie, [over het maken van een App Service Environment](environment/app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-to-create-an-app-service-environment"></a>Waarom zie ik time-outs wanneer ik probeer te maken van een App Service-omgeving?

Soms mislukt het maken van een App Service Environment. In dat geval ziet u de volgende fout in de activiteitenlogboeken:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"The resource provision operation did not complete within the allowed timeout period.”}}
```

U kunt dit oplossen, zorg dat geen van de volgende voorwaarden voldaan wordt:
* Het subnet is te klein.
* Het subnet is niet leeg zijn.
* ExpressRoute wordt voorkomen dat de netwerkverbindingsvereisten van een App Service Environment.
* Een onjuiste Netwerkbeveiligingsgroep wordt voorkomen dat de netwerkverbindingsvereisten van een App Service Environment.
* Geforceerde tunneling is ingeschakeld.

Zie voor meer informatie, [frequente problemen bij het implementeren (maken) een nieuwe Azure App Service Environment](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Waarom kan ik mijn App Service-plan niet verwijderen?

U kunt een App Service-plan niet verwijderen als er App Service-apps gekoppeld aan het App Service-plan zijn. Voordat u een App Service-plan verwijderen, verwijdert u alle gekoppelde App Service-apps uit de App Service-plan.

## <a name="how-do-i-schedule-a-webjob"></a>Hoe kan ik een webtaak plannen?

U kunt een geplande webtaak maken met behulp van de Cron-expressies:

1. Maak een settings.job-bestand.
2. In dit JSON-bestand, zijn onder andere een schema-eigenschap met behulp van een Cron-expressie: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of the week}" }
    ```

Zie voor meer informatie over geplande webtaken [een geplande webtaak maken met behulp van een Cron-expressie](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Hoe voer ik indringingstests voor mijn App Service-app?

Om uit te voeren indringingstests, [een aanvraag indienen](https://portal.msrc.microsoft.com/en-us/engage/pentest).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Hoe configureer ik een aangepaste domeinnaam voor een App Service-web-app die gebruikmaakt van Traffic Manager?

Zie voor meer informatie over het gebruik van een aangepaste domeinnaam met een App Service-app die gebruikmaakt van Azure Traffic Manager voor taakverdeling, [een aangepaste domeinnaam voor een Azure-web-app configureren met Traffic Manager](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>Mijn App Service-certificaat is gemarkeerd voor fraude. Hoe kan ik dit oplossen?

Tijdens de domeinverificatie na de aankoop van een App Service-certificaat ziet u mogelijk het volgende bericht:

'Uw certificaat is gemarkeerd voor mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet gebruikt binnen 24 uur wordt, neem contact op met ondersteuning voor Azure."

Als het bericht aangeeft, kan dit verificatieproces fraude tot 24 uur duren. Gedurende deze tijd, gaat u verder met het bericht wordt weergegeven.

Als uw App Service-certificaat, dit bericht weergeven na 24 uur blijft, voert u de volgende PowerShell-script. De script-contactpersonen de [certificaatprovider](https://www.godaddy.com/) rechtstreeks naar het probleem is opgelost.

```
Connect-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Hoe verificatie en autorisatie werken in App Service?

Zie voor gedetailleerde informatie voor verificatie en autorisatie in App Service-documenten voor verschillende provider-aanmeldingen identificeren:
* [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Microsoft-account](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-the-default-azurewebsitesnet-domain-to-my-azure-web-apps-custom-domain"></a>Hoe omleiden standaard *. azurewebsites.net domein aan mijn Azure-web-app aangepaste domein?

Wanneer u een nieuwe website maken met behulp van Web-Apps in Azure, een standaard *sitename*. azurewebsites.net domein is toegewezen aan uw site. Als u een aangepaste hostnaam aan uw site toevoegen en niet wilt dat gebruikers kunnen toegang krijgen tot uw standaard *. azurewebsites.net domein, kunt u de standaard-URL omleiden. Zie voor informatie over het omleiden van al het verkeer van het standaarddomein van uw website naar uw aangepaste domein, [het standaarddomein een omleiding naar uw aangepaste domein in Azure-web-apps](http://zainrizvi.io/blog/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hoe bepaal ik welke versie van .NET versie is geïnstalleerd in App Service?

Er is de snelste manier om te zien welke versie van Microsoft .NET die in App Service geïnstalleerd met behulp van de Kudu-console. U kunt de Kudu-console openen vanuit de portal of met behulp van de URL van uw App Service-app. Zie voor gedetailleerde instructies [bepalen van de geïnstalleerde versie van .NET in App Service](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Waarom werkt de functie voor automatisch schalen niet zoals verwacht?

Als voor automatisch schalen van Azure nog niet geschaald in of de web-app-exemplaar uitgeschaalde zoals u verwacht, kunt u uitvoeren in een scenario waarin kiezen we opzettelijk niet om te schalen om te voorkomen dat een oneindige lus vanwege "op en neer." Dit gebeurt meestal wanneer er geen een voldoende marge tussen de drempelwaarden voor scale-out en schaal. Als u wilt weten hoe om te voorkomen dat 'op en neer' en voor meer informatie over andere aanbevolen procedures voor automatisch schalen, Zie [aanbevolen procedures voor automatisch schalen](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Waarom voor automatisch schalen soms schaalt slechts gedeeltelijk?

Automatisch schalen wordt geactiveerd wanneer de metrische gegevens over vooraf geconfigureerde grenzen overschrijden. Soms zult u merken dat de capaciteit wordt slechts gedeeltelijk gevuld in vergelijking met wat u verwacht. Dit kan gebeuren wanneer het aantal exemplaren dat u wilt dat niet beschikbaar zijn. In dit scenario is ingevuld voor automatisch schalen gedeeltelijk aan met het beschikbare aantal exemplaren. Automatisch schalen wordt vervolgens uitgevoerd voor de logica voor opnieuw verdelen om op te halen van meer capaciteit. De resterende exemplaren toegewezen. Houd er rekening mee dat dit kan enkele minuten duren.

Als u niet het verwachte aantal exemplaren na een paar minuten ziet, is dit mogelijk doordat de gedeeltelijke aanvulling voldoende is om de metrische gegevens binnen de grenzen. Of, voor automatisch schalen kan omlaag geschaald omdat de onderste grens van de metrische gegevens is bereikt.

Als geen van deze voorwaarden van toepassing en het probleem zich blijft voordoen, moet u een ondersteuningsaanvraag indienen.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>Hoe kan ik HTTP-compressie inschakelen voor mijn inhoud?

Als u compressie voor statische en dynamische inhoudstypen, voeg de volgende code in het bestand web.config op toepassingsniveau:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

U kunt ook de specifieke dynamische en statische MIME-typen die u wilt comprimeren opgeven. Zie voor meer informatie onze reactie op een forumvraag in [httpCompression instellingen op een eenvoudige Azure-website](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-to-app-service"></a>Hoe Migreer ik van een on-premises-omgeving in App Service?

Voor het migreren van sites van Windows en Linux-webservers in App Service, kunt u Azure App Service Migration Assistant. Het hulpprogramma voor migratie web-apps en databases maakt in Azure zo nodig en vervolgens de inhoud wordt gepubliceerd. Zie voor meer informatie, [Azure App Service Migration Assistant](https://www.migratetoazure.net/).
