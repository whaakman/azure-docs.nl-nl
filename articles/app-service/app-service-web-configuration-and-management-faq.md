---
title: Configuratie Veelgestelde vragen over Azure-web-apps | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over het configureren en beheren van problemen voor de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 92cbc36ac2a566cf5dfbb2f7b3347973bab5ee8c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Configuratie- en veelgestelde vragen voor Web-Apps in Azure

In dit artikel bevat antwoorden op veelgestelde vragen (FAQ's) over configuratie en beheer van problemen voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-to-move-app-service-resources"></a>Zijn er beperkingen die ik houden moet rekening als ik wil verplaatsen van resources in App Service?

Als u van plan bent een App Service-resources verplaatsen naar een nieuwe resourcegroep of abonnement, zijn er enkele beperkingen moet houden. Zie voor meer informatie [App Service-beperkingen](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Hoe gebruik ik een aangepaste domeinnaam voor mijn web-app

Voor antwoorden op veelgestelde vragen over het gebruik van een aangepaste domeinnaam met uw Azure-web-app raadpleegt u onze zeven minuten durende video [toevoegen van een aangepaste domeinnaam](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). De video biedt een overzicht van het toevoegen van een aangepaste domeinnaam. Dit wordt beschreven hoe u uw eigen URL in plaats van de *. azurewebsites.net URL met uw App Service-web-app. Ook ziet u een gedetailleerd overzicht van [het toewijzen van een aangepaste domeinnaam](app-service-web-tutorial-custom-domain.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Hoe ik een nieuw aangepast domein aanschaffen voor mijn web-app?

Zie voor meer informatie over het aanschaffen en instellen van een aangepast domein voor uw App Service-web-app, [kopen en configureren van een aangepaste domeinnaam in App Service](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Hoe ik uploaden en configureren van een bestaand SSL-certificaat voor mijn web-app?

Zie voor informatie over het uploaden en het instellen van een bestaande aangepaste SSL-certificaat, [een bestaande aangepaste SSL-certificaat binden aan een Azure-web-app](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Hoe ik kopen en configureren van een nieuw SSL-certificaat in Azure voor mijn web-app?

Zie voor meer informatie over het aanschaffen en instellen van een SSL-certificaat voor uw App Service-web-app, [een SSL-certificaat toevoegen aan uw App Service-app](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Hoe kan ik Application Insights-resources verplaatsen?

Op dit moment ondersteunt Azure Application Insights niet de move-bewerking. Als uw oorspronkelijke resourcegroep een Application Insights-resource bevat, kunt u deze bron niet verplaatsen. Als u de Application Insights-resource opneemt wanneer u probeert te verplaatsen van een App Service-app, verplaatst de hele bewerking mislukt. Application Insights en de App Service-abonnement hoeft echter niet in dezelfde resourcegroep als de app voor de app te laten functioneren.

Zie voor meer informatie [App Service-beperkingen](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Waar kan ik een controlelijst richtlijnen en meer informatie over resource bewerkingen verplaatsen?

[App Service-beperkingen](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) ziet u hoe u resources verplaatsen naar een nieuw abonnement of een nieuwe resourcegroep in hetzelfde abonnement. U kunt informatie ophalen over de controlelijst voor het verplaatsen van resource, informatie over welke services ondersteunt de verplaatsing en meer informatie over App Service-beperkingen en andere onderwerpen.

## <a name="how-do-i-set-the-server-time-zone-for-my-web-app"></a>Hoe stel de tijdzone van de server voor mijn web-app?

De tijdzone van de server voor uw web-app instellen:

1. In de Azure-portal in uw App Service-abonnement, gaat u naar de **toepassingsinstellingen** menu.
2. Onder **appinstellingen**, deze instelling toevoegen:
    * Sleutel = WEBSITE_TIME_ZONE
    * Waarde = *de tijdzone die u wilt*
3. Selecteer **Opslaan**.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Waarom mijn doorlopende webtaken soms mislukt?

Standaard worden web-apps uit het geheugen verwijderd als ze inactief gedurende een bepaalde tijd zijn. Hiermee wordt het systeem te besparen. In het basis en standaard-plan, kunt u op de **altijd op** voortdurend instellen voor het behouden van de web-app geladen. Als u uw web-app doorlopende webtaken uitvoert, moet u inschakelen **altijd op**, of de WebJobs mogelijk niet betrouwbaar uitgevoerd. Zie voor meer informatie [maken van een continu actief webtaak](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-the-outbound-ip-address-for-my-web-app"></a>Hoe krijg ik het uitgaande IP-adres voor mijn web-app

Voor de lijst met uitgaande IP-adressen voor uw web-app:

1. In de Azure-portal op de blade van uw web-app, gaat u naar de **eigenschappen** menu.
2. Zoeken naar **uitgaande IP-adressen**.

De lijst van uitgaande IP-adressen weergegeven.

Als uw website wordt gehost in App Service-omgeving voor PowerApps, voor informatie over het ophalen van uw uitgaande IP-adres, Zie [uitgaande netwerkadressen](environment/app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Hoe krijg ik een gereserveerde of toegewijd binnenkomende IP-adres voor mijn web-app

Als u een speciale of gereserveerd IP-adres voor binnenkomende oproepen naar de website van uw Azure-app instelt, installeren en configureren van een IP-gebaseerde SSL-certificaat.

Houd er rekening mee dat voor het gebruik van een toegewezen of gereserveerd IP-adres voor binnenkomende oproepen, uw App Service-abonnement moet zich in een Basic of hoger service-abonnement.

## <a name="can-i-export-my-app-service-certificate-to-use-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Kan ik mijn App Service-certificaat moet worden gebruikt buiten Azure, zoals voor een website elders gehost exporteren? 

App Service-certificaten worden beschouwd als Azure-resources. Ze zijn niet bedoeld voor gebruik buiten uw Azure-services. U kunt deze voor gebruik buiten Azure kan niet exporteren. Zie voor meer informatie [Veelgestelde vragen over App Service-certificaten en aangepaste domeinen](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-to-use-with-other-azure-cloud-services"></a>Kan ik mijn App Service-certificaat moet worden gebruikt met andere Azure-cloud-services exporteren?

De portal biedt een uitstekende ervaring voor het implementeren van een App Service-certificaat via Azure Key Vault voor App Service-apps. Echter, we hebben zijn ontvangen aanvragen van klanten voor het gebruik van deze certificaten buiten de App Service-platform, bijvoorbeeld met Azure Virtual Machines. Zie voor meer informatie over het maken van een lokale PFX-kopie van uw App Service-certificaat, zodat u het certificaat met andere Azure-resources gebruiken kunt, [maken van een lokale PFX-kopie van een App Service-certificaat](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Zie voor meer informatie [Veelgestelde vragen over App Service-certificaten en aangepaste domeinen](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-the-message-partially-succeeded-when-i-try-to-back-up-my-web-app"></a>Waarom zie ik het bericht 'Gedeeltelijk geslaagd' als ik wil back-up van mijn web-app?

Een veelvoorkomende oorzaak van de back-fout is dat bepaalde bestanden gebruikt door de toepassing zijn. Bestanden die in gebruik zijn vergrendeld terwijl u de back-up uitvoert. Dit voorkomt dat deze bestanden back-up wordt gemaakt en kan leiden tot een status 'Gedeeltelijk is voltooid'. U kunt mogelijk te voorkomen dat dit zich voordoet door bestanden uitsluiten van het back-upproces. U kunt back-up die nodig is. Zie voor meer informatie [back-up alleen de belangrijke onderdelen van uw site met Azure-web-apps](http://www.zainrizvi.io/2015/06/05/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-the-http-response"></a>Hoe verwijder ik een koptekst van het HTTP-antwoord

Bijwerken van uw site web.config-bestand voor het verwijderen van de headers van het HTTP-antwoord. Zie voor meer informatie [Standard van SQL server-headers van uw Azure websites verwijderd](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>App Service voldoet aan de standaard PCI-3.0 en 3.1 is?

De functie Web Apps van Azure App Service is momenteel in overeenstemming met PCI Data Security Standard (DSS) versie 3.0-niveau 1. PCI DSS versie 3.1 is op onze roadmap. Planning wordt al uitgevoerd voor hoe de toepassing van de meest recente standaard wordt voortgezet.

PCI DSS vereist versie 3.1 Transport Layer Security (TLS) 1.0 uitschakelen. TLS 1.0 uitschakelen is momenteel geen een optie voor de meeste App Service-abonnementen. Als u App Service-omgeving gebruiken of u bereid bent te migreren van uw werkbelasting naar App Service-omgeving, kunt u meer controle over uw omgeving ophalen. Dit omvat het TLS 1.0 uitschakelen contact opnemen met de ondersteuning van Azure. In de nabije toekomst zullen we deze instellingen om toegankelijk te maken voor gebruikers.

Zie voor meer informatie [compatibiliteit van Microsoft Azure App Service web-app met PCI standaard 3.0 en 3.1](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-the-staging-environment-and-deployment-slots"></a>Hoe gebruik ik de staging-omgeving en implementatie sleuven

In een Standard en Premium-App Service-plan als u uw web-app in App Service implementeert kunt u implementeren op een afzonderlijke implementatiesleuf in plaats van naar de productiesite standaard. Implementatiesites zijn live web-apps die hun eigen hostnamen hebben. Web-app inhoud en configuratie-elementen worden ingewisseld tussen twee implementatiesites, met inbegrip van de productiesite.

Zie voor meer informatie over het gebruik van implementatiesites [instellen van een testomgeving in App Service](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Hoe ik toegang tot logboeken en controleren webtaak?

Webtaak logboeken bekijken:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer de webtaak.
3. Selecteer de **wisselknop uitvoer** knop.
4. Voor het downloaden van het uitvoerbestand, selecteer de **downloaden** koppeling.
5. Selecteer voor afzonderlijke wordt uitgevoerd, **afzonderlijke aanroepen**.
6. Selecteer de **wisselknop uitvoer** knop.
7. Selecteer de koppeling.

## <a name="im-trying-to-use-hybrid-connections-with-sql-server-why-do-i-see-the-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>Ik wil hybride verbindingen met SQL Server gebruiken. Waarom zie ik het bericht ' System.OverflowException: rekenkundige bewerking resulteerde in een overloop '?

Als u hybride verbindingen gebruiken voor toegang tot SQL Server, kan een Microsoft .NET-update op 10 mei 2016-verbindingen niet veroorzaken. U ziet dit bericht:

```
Exception: System.Data.Entity.Core.EntityException: The underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Oplossing

De uitzondering is veroorzaakt door een probleem met hybride Verbindingsbeheer die sindsdien zijn opgelost. Zorg ervoor dat [bijwerken van uw hybride Verbindingsbeheer](https://go.microsoft.com/fwlink/?LinkID=841308) om dit probleem te verhelpen.

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Hoe ik toevoegen of bewerken van een regel voor het herschrijven van URL?

Toevoegen of bewerken van een regel voor het herschrijven van URL:

1. Internet Information Services (IIS) Manager zo instellen dat deze verbinding met uw App Service-web-app maakt. Zie voor meer informatie over het aansluiten van IIS-beheer op App Service, [extern beheer van Azure websites met behulp van IIS-beheer](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. In de IIS-beheer toevoegen of bewerken van een regel voor het herschrijven van URL. Zie voor informatie over het toevoegen of bewerken van een regel voor het herschrijven van URL, [herschrijven-regels maken voor de URL herschrijven module](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-to-app-service"></a>Hoe bepaal ik binnenkomend verkeer op App Service

Op het siteniveau van de hebt u twee opties voor het beheren van binnenkomend verkeer op App Service:

* Schakel dynamisch IP-beperkingen. Zie voor meer informatie over het inschakelen van dynamische IP-beperkingen, [IP- en domeinbeperkingen voor Azure websites](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Schakel beveiliging van de Module. Zie voor meer informatie over het inschakelen van beveiliging van de Module, [ModSecurity web application firewall op Azure websites](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

Als u App Service-omgeving gebruikt, kunt u [Barracuda firewall](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Hoe ik poorten in een App Service-web-app blokkeren?

In de omgeving van App Service gedeelde tenant is het niet mogelijk om te blokkeren van bepaalde poorten vanwege de aard van de infrastructuur. TCP-poorten 4016 4018 en 4020 ook mogelijk voor foutopsporing op afstand Visual Studio is geopend.

In App Service-omgeving hebt u volledige controle over binnenkomend en uitgaand verkeer. U kunt Netwerkbeveiligingsgroepen gebruiken om te beperken of bepaalde poorten blokkeren. Zie voor meer informatie over App Service-omgeving, [introductie van App Service-omgeving](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Hoe ik een tracering F12 vastleggen?

U hebt twee opties voor het vastleggen van een tracering F12:

* F12 HTTP-tracering
* F12 console-uitvoer

### <a name="f12-http-trace"></a>F12 HTTP-tracering

1. Ga naar uw website in Internet Explorer. Het is belangrijk aan te melden voordat u de volgende stappen. Anders worden de F12-tracering gevoelige gegevens aanmelden.
2. Druk op F12.
3. Controleer de **netwerk** tabblad is geselecteerd en selecteer vervolgens de groene **afspelen** knop.
4. Voer de stappen die het probleem te reproduceren.
5. Selecteer de rode **stoppen** knop.
6. Selecteer de **opslaan** knop (schijfpictogram) en sla het bestand HAR (in Internet Explorer en Edge) *of* met de rechtermuisknop op het bestand HAR en selecteer vervolgens **opslaan als HAR met inhoud** (in Chrome).

### <a name="f12-console-output"></a>F12 console-uitvoer

1. Selecteer de **Console** tabblad.
2. Selecteer het tabblad voor elk tabblad die groter zijn dan nul items bevat (**fout**, **waarschuwing**, of **informatie**). Als het tabblad niet is geselecteerd, is het tabblad pictogram grijs of zwart wanneer de cursor niet op deze.
3. Met de rechtermuisknop in het berichtgedeelte aan het deelvenster en selecteer vervolgens **Kopieer alle**.
4. De gekopieerde tekst plakken in een bestand en sla het bestand.

Als u wilt een bestand HAR weergeven, kunt u de [HAR viewer](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-to-connect-an-app-service-web-app-to-a-virtual-network-that-is-connected-to-expressroute"></a>Waarom krijg ik een fout als ik wil verbinding maken met een App Service web-app met een virtueel netwerk dat is verbonden met ExpressRoute?

Als u probeert een Azure-web-app kunt koppelen aan een virtueel netwerk dat is verbonden met Azure ExpressRoute, mislukt dit. Het volgende bericht weergegeven: 'Gateway is niet een VPN-gateway'.

U kunt op dit moment geen punt-naar-site VPN-verbindingen met een virtueel netwerk dat is verbonden met ExpressRoute. Een punt-naar-site-VPN en ExpressRoute kunnen niet worden gecombineerd voor hetzelfde virtuele netwerk. Zie voor meer informatie [ExpressRoute en site-naar-site VPN-verbindingen limieten en beperkingen](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-to-a-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Hoe kan ik een App Service-web-app verbinden met een virtueel netwerk met een statische routering (op beleid gebaseerd)-gateway?

Op dit moment wordt een App Service-web-app verbinden met een virtueel netwerk met een statische routering (op beleid gebaseerd)-gateway niet ondersteund. Als het virtuele doelnetwerk al bestaat, moet de punt-naar-site VPN is ingeschakeld, met een gateway voor dynamische routering voordat deze kan worden verbonden met een app hebben. Als uw gateway is ingesteld op statische routering, kunt u een punt-naar-site VPN niet inschakelen. 

Zie voor meer informatie [een app integreren met een Azure-netwerk](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>Mijn App-serviceomgeving waarom kan ik maken slechts één App Service-abonnement, zelfs als er twee werknemers beschikbaar?

Bieden fouttolerantie, vereist App Service-omgeving dat elke worker-groep ten minste één extra Reken-bron moet. De extra Reken-resource kan niet worden toegewezen als een werkbelasting.

Zie voor meer informatie [het maken van een App-serviceomgeving](environment/app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-to-create-an-app-service-environment"></a>Waarom zie ik time-outs wanneer ik probeer te maken van een App Service-omgeving?

Soms mislukt het maken van een App Service-omgeving. In dat geval ziet u de volgende fout in de logboeken van de activiteit:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"The resource provision operation did not complete within the allowed timeout period.”}}
```

U lost dit op, zorg dat geen van de volgende voorwaarden voldaan wordt:
* Het subnet is te klein.
* Het subnet is niet leeg.
* ExpressRoute wordt voorkomen dat de netwerkvereisten voor de verbinding van een App Service-omgeving.
* Een onjuiste Netwerkbeveiligingsgroep voorkomt u dat de netwerkvereisten voor de verbinding van een App Service-omgeving.
* Geforceerde tunneling is ingeschakeld.

Zie voor meer informatie [regelmatige problemen bij het implementeren van (maken) een nieuwe Azure App Service-omgeving](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Waarom kan ik mijn App Service-abonnement niet verwijderen

U kunt een App Service-abonnement niet verwijderen als een App Service-apps gekoppeld aan de App Service-abonnement zijn. Voordat u een App Service-abonnement verwijdert, verwijdert u alle gekoppelde App Service-apps uit de App Service-abonnement.

## <a name="how-do-i-schedule-a-webjob"></a>Hoe kan ik een webtaak plannen?

U kunt een geplande webtaak maken met behulp van Cron expressies:

1. Maak een bestand settings.job.
2. Neem in dit JSON-bestand, een schema-eigenschap met behulp van een Cron-expressie: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of the week}" }
    ```

Zie voor meer informatie over de geplande webtaken [een geplande webtaak maken met behulp van een expressie Cron](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Hoe voer ik binnendringen testen voor mijn App Service-app uit?

Testen binnendringen, [een aanvraag indienen](https://security-forms.azure.com/penetration-testing/terms).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Hoe kan ik een aangepaste domeinnaam voor een App Service-web-app die gebruikmaakt van Traffic Manager configureren?

Zie voor meer informatie over het gebruik van een aangepaste domeinnaam met een App Service-app die gebruikmaakt van Azure Traffic Manager load balancing, [een aangepaste domeinnaam configureren voor een Azure-web-app met Traffic Manager](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>Mijn App Service-certificaat is gemarkeerd voor fraude. Hoe kan ik dit oplossen?

Tijdens de verificatie van het domein van de aankoop van een App Service-certificaat ziet u het volgende bericht:

'Uw certificaat is gemarkeerd om mogelijke fraude. De aanvraag wordt momenteel onderzocht. Als het certificaat niet gebruikt binnen 24 uur wordt, neem contact op met ondersteuning van Azure."

Als het bericht aangeeft, kan dit verificatieproces fraude tot 24 uur duren. Gedurende deze tijd, gaat u verder met het bericht wordt weergegeven.

Als uw App Service-certificaat, dit bericht weergeven na 24 uur blijft, voert u de volgende PowerShell-script. De script-contactpersonen de [certificaatprovider](https://www.godaddy.com/) rechtstreeks aan het probleem is opgelost.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Hoe verificatie en autorisatie werken in App Service?

Zie voor gedetailleerde documentatie voor verificatie en autorisatie in App Service docs voor verschillende provider aanmeldingen identificeren:
* [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Microsoft-account](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-the-default-azurewebsitesnet-domain-to-my-azure-web-apps-custom-domain"></a>Hoe leiden de standaard *. azurewebsites.net domein aan aangepast domein mijn Azure-web-app?

Wanneer u een nieuwe website maken met behulp van Web-Apps in Azure, een standaard *sitename*. azurewebsites.net domein is toegewezen aan uw site. Als u een aangepaste hostnaam aan uw site toevoegen en niet wilt dat gebruikers kunnen toegang krijgen tot uw standaard *. azurewebsites.net domein, kunt u de standaard-URL omleiden. Zie voor meer informatie over het omleiden van alle verkeer van uw website standaarddomein naar uw aangepaste domein, [het standaarddomein omleiden naar uw aangepaste domein in Azure-web-apps](http://www.zainrizvi.io/2016/04/07/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hoe bepalen welke versie van .NET versie is geïnstalleerd in App Service?

Er is de snelste manier om de versie van Microsoft .NET die geïnstalleerd in App Service vinden met behulp van de Kudu-console. U kunt de Kudu-console kunt openen vanuit de portal of met behulp van de URL van uw App Service-app. Zie voor gedetailleerde instructies [bepalen van de geïnstalleerde versie van .NET in App Service](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Waarom werkt niet voor automatisch schalen zoals verwacht?

Als Azure voor automatisch schalen die nog niet geschaald in of uit de web-app-exemplaar wordt geschaald zoals u verwacht, kunt u uitvoeren in een scenario waarin we opzettelijk ervoor kiest geen te schalen om te voorkomen dat een oneindige lus vanwege "op en neer." Dit gebeurt meestal wanneer er geen een voldoende marge tussen de drempelwaarden voor scale-out en de schaal. Zie voor meer informatie over het voorkomen 'op en neer' en voor meer informatie over andere aanbevelingen voor automatisch schalen, [aanbevolen procedures voor automatisch schalen](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Waarom automatisch schalen soms schaalt slechts gedeeltelijk?

Automatisch schalen wordt geactiveerd wanneer de metrische gegevens overschrijden vooraf geconfigureerde grenzen. Soms moet wellicht u opgevallen dat de capaciteit slechts gedeeltelijk is gevuld in vergelijking met wat u verwacht. Dit kan gebeuren wanneer het aantal exemplaren dat u wilt dat niet beschikbaar zijn. In dit scenario vult automatisch schalen gedeeltelijk met het aantal beschikbare exemplaren. Automatisch schalen voert de logica deel opnieuw als u meer capaciteit. De resterende exemplaren toegewezen. Houd er rekening mee dat dit kan enkele minuten duren.

Als u niet het verwachte aantal exemplaren van na een paar minuten ziet, is dit mogelijk doordat de gedeeltelijke aanvulling voldoende is om de metrische gegevens binnen de grenzen. Of automatisch schalen kan omlaag geschaald omdat de ondergrens van de metrische gegevens is bereikt.

Als geen van deze voorwaarden van toepassing en het probleem zich blijft voordoen, moet u een ondersteuningsaanvraag indienen.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>Hoe schakel ik op HTTP-compressie voor mijn inhoud?

Als u compressie voor statische en dynamische inhoudstypen, voeg de volgende code in het bestand web.config op toepassingsniveau:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

U kunt ook de specifieke dynamische en statische MIME-typen die u wilt comprimeren opgeven. Zie voor meer informatie onze reactie op een forumvraag in [httpCompression-instellingen op een eenvoudige Azure-website](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-to-app-service"></a>Hoe Migreer ik van een on-premises-omgeving in App Service?

Voor het migreren van sites van Windows en Linux-webservers in App Service, kunt u assistent Azure App Service-migratie. Het hulpprogramma voor migratie van web-apps en databases maakt in Azure indien nodig en vervolgens de inhoud wordt gepubliceerd. Zie voor meer informatie [assistent Azure App Service-migratie](https://www.migratetoazure.net/).
