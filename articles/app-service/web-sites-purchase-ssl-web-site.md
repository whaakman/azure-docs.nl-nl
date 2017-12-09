---
title: Kopen en configureren van een SSL-certificaat voor uw Azure App Service | Microsoft Docs
description: Informatie over het aanschaffen van een App Service-certificaat en bindt dit aan uw app in App Service
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: apurvajo;cephalin
ms.openlocfilehash: 6c0125bf0bd22912a21372b5a7da6846e924e6cd
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Een SSL-certificaat kopen en configureren voor uw Azure App Service

Deze zelfstudie leert u hoe u uw web-app beveiligen door het aanschaffen van een SSL-certificaat voor uw  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, veilig opslaan in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis), en koppelen aan een aangepast domein.

## <a name="step-1---log-in-to-azure"></a>Stap 1 - logboek in naar Azure

Aanmelden bij de Azure portal op http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Stap 2: een SSL-certificaat bestelling plaatsen

U kunt een SSL-certificaat-volgorde plaatsen door het maken van een nieuw [App Service-certificaat](https://portal.azure.com/#create/Microsoft.SSL) In de **Azure-portal**.

![Maken van het certificaat](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Voer een beschrijvende **naam** voor uw SSL-certificaat en voer de **domeinnaam**

> [!NOTE]
> Deze stap is een van de belangrijkste onderdelen van het aankoopproces. Zorg ervoor dat juiste host-naam (aangepaste domein) die u wilt beveiligen met dit certificaat op te geven. **GEEN** de hostnaam met WWW toevoegen. 
>

Selecteer uw **abonnement**, **resourcegroep**, en **SKU van het certificaat**

> [!WARNING]
> App Service-certificaten kunnen alleen worden gebruikt door andere Services App binnen hetzelfde abonnement.  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Stap 3: het certificaat wordt opgeslagen in Azure Sleutelkluis

> [!NOTE]
> [Sleutelkluis](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) is een Azure-service die helpt bij het beschermen van de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services.
>

Zodra de aankoop van het SSL-certificaat voltooid is, moet u opent de [App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) pagina.

![afbeelding van gereed is om op te slaan in KV invoegen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

De status van het certificaat **"In behandeling zijnde uitgifte"** omdat er meer stappen u uitvoeren moet voordat u met dit certificaat kunt gaan.

Klik op **Certificaatconfiguratie** pagina in de eigenschappen voor certificaat en klik op **stap 1: opslaan** dit certificaat wordt opgeslagen in Azure Sleutelkluis.

Van de **Sleutelkluis Status** pagina, klikt u op **Sleutelkluis opslagplaats** kiezen een bestaande Sleutelkluis voor het opslaan van dit certificaat **of maak nieuwe Sleutelkluis** nieuwe Sleutelkluis maken binnen hetzelfde abonnement en de resource-groep.

> [!NOTE]
> Azure Sleutelkluis heeft minimale kosten voor het opslaan van dit certificaat.
> Zie voor meer informatie  **[prijsinformatie voor Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Nadat u de opslagplaats van de kluis sleutel voor het opslaan van dit certificaat, hebt geselecteerd de **opslaan** optie geslaagd moet worden weergegeven.

![afbeelding van store slagen van KV invoegen](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Stap 4: Controleer of het eigendom van het domein

Uit dezelfde **Certificaatconfiguratie** pagina die u in stap 3 wordt gebruikt, klikt u op **stap 2: Controleer of**.

Kies de verificatiemethode voorkeur domein. 

Er zijn vier typen domeinverificatie ondersteund door App Service Certificate: App Service, domein, E-mail en handmatige verificatie. Meer informatie in deze typen verificatie worden beschreven de [geavanceerde sectie](#advanced).

> [!NOTE]
> **App Service-verificatie** is de meest geschikte optie bij het domein dat u wilt controleren al aan een App Service-app in hetzelfde abonnement toegewezen is. Dit maakt gebruik van het feit dat de App Service-app al het eigendom van het domein is geverifieerd.
>

Klik op **controleren** knop om deze stap te voltooien.

![afbeelding van domeinverificatie invoegen](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Wanneer u op **controleren**, gebruiken de **vernieuwen** knop totdat de **controleren** optie geslaagd moet worden weergegeven.

![afbeelding van INSERT verifiëren in KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Stap 5 - certificaat toewijzen aan de App Service-App

> [!NOTE]
> Voordat u de stappen in deze sectie, moet een aangepaste domeinnaam hebt gekoppeld aan uw app. Zie voor meer informatie  **[configureren van een aangepaste domeinnaam voor een web-app.](app-service-web-tutorial-custom-domain.md)**
>

In de  **[Azure-portal](https://portal.azure.com/)**, klikt u op de **App Service** optie aan de linkerkant van de pagina.

Klik op de naam van uw app waaraan u dit certificaat wilt toewijzen.

In de **instellingen**, klikt u op **SSL-certificaten**.

Klik op **App Service-certificaat importeren** en selecteer het certificaat dat u zojuist hebt aangeschaft.

![afbeelding van het certificaat importeren invoegen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

In de **ssl-bindingen** sectie Klik op **bindingen toevoegen**, en selecteer de naam van het domein te beveiligen met SSL en het certificaat te gebruiken met de vervolgkeuzelijsten. U kunt ook selecteren of u wilt gebruiken  **[indicatie voor Server-naam (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  of IP op basis van SSL.

![afbeelding van SSL-bindingen worden ingevoegd](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klik op **toevoegen Binding** de wijzigingen opslaan en SSL in te schakelen.

> [!NOTE]
> Als u hebt geselecteerd **IP SSL gebaseerde** en uw aangepaste domein is geconfigureerd met een A-record, moet u de volgende aanvullende stappen uitvoeren. Deze worden beschreven in meer informatie in de [geavanceerde sectie](#Advanced).

Op dit moment moet u kunnen bezoeken uw app met `HTTPS://` in plaats van `HTTP://` om te controleren of het certificaat correct is geconfigureerd.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Stap 6: beheertaken

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Geavanceerd

### <a name="verifying-domain-ownership"></a>Domein eigendom verifiëren

Er zijn twee soorten verificatie van het domein wordt ondersteund door App service Certificate: e-Mail en handmatige verificatie.

#### <a name="mail-verification"></a>E-mailverificatie

Reeds is bevestigingsmail verzonden naar de e-adressen die zijn gekoppeld aan dit aangepast domein.
De e-verificatiestap voltooid, opent u het e-mailbericht en klik op de koppeling voor verificatie.

![afbeelding van e-mailverificatie invoegen](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Als u de verificatie-e-mail verzenden moet, klikt u op de **E-mail opnieuw verzenden** knop.

#### <a name="domain-verification"></a>Domeinverificatie

Selecteer deze optie alleen voor [een App Service-domein dat u hebt aangeschaft van Azure.](custom-dns-web-site-buydomains-web-app.md). Azure automatisch de verificatie TXT-record voor u worden toegevoegd en het proces is voltooid.

#### <a name="manual-verification"></a>Handmatige verificatie

> [!IMPORTANT]
> HTML-pagina verificatie (werkt alleen met standaard SKU van het certificaat.)
>

1. Maken van een HTML-bestand met de naam **'starfield.html'**

1. Inhoud van dit bestand moet de exacte naam van het domein verificatie-Token. (U kunt het token van de pagina domein verificatiestatus kopiëren)

1. Dit bestand in de hoofdmap van de webserver die als host fungeert voor uw domein niet uploaden`/.well-known/pki-validation/starfield.html`

1. Klik op **vernieuwen** wilt bijwerken van de certificaatstatus nadat de verificatie is voltooid. Het duurt enkele minuten voor verificatie te voltooien.

> [!TIP]
> Controleer of in een terminal met `curl -G http://<domain>/.well-known/pki-validation/starfield.html` het antwoord moet bevatten de `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Controle van DNS TXT-Record

1. Maak een TXT-record met uw DNS-beheer op de `@` subdomein met waarde gelijk is aan het domein verificatie-Token.
1. Klik op **'Vernieuwen'** de certificaatstatus bijwerken nadat de verificatie is voltooid.

> [!TIP]
> U moet een TXT-record maken op `@.<domain>` met waarde `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Certificaat toewijzen aan App Service-App

Als u hebt geselecteerd **IP SSL gebaseerde** en uw aangepaste domein is geconfigureerd met een A-record, moet u de volgende aanvullende stappen uitvoeren:

Nadat u hebt geconfigureerd SSL-binding op basis van een IP-adres, een vast IP-adres is toegewezen aan uw app. U kunt dit IP-adres vinden op de **aangepaste domeinen** pagina onder de instellingen van uw app, direct boven de **hostnamen** sectie. Deze wordt vermeld als **externe IP-adres**

![afbeelding van IP-SSL invoegen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Dit IP-adres is anders dan het virtuele IP-adres is eerder gebruikt voor het configureren van de A-record voor uw domein. Als u zijn geconfigureerd voor gebruik SNI op basis van SSL, of zijn niet geconfigureerd om SSL te gebruiken, geen adres wordt vermeld voor dit item.

Met de hulpprogramma's door uw domeinnaamregistrar, wijzig de A-record voor uw aangepaste domeinnaam om te verwijzen naar het IP-adres van de vorige stap.

## <a name="rekey-and-sync-the-certificate"></a>Sleutel opnieuw genereren en synchroniseren van het certificaat

Als u ooit sleutel opnieuw genereren van uw certificaat wilt, selecteert u de **opnieuw genereren van sleutels en Sync** optie van de **certificaateigenschappen** pagina.

Klik op **sleutel opnieuw genereren** om het proces te starten. Dit kan 1 tot 10 minuten duren.

![afbeelding van de sleutel opnieuw genereren SSL invoegen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Het certificaat opnieuw genereren van sleutels, wordt het certificaat met een nieuw certificaat is uitgegeven door de certificeringsinstantie.

<a name="notrenewed"></a>
## <a name="why-is-my-ssl-certificate-not-auto-renewed"></a>Waarom wordt mijn SSL-certificaat niet automatisch-verlengd?

Als uw SSL-certificaat is geconfigureerd voor automatisch verlengen, maar deze niet automatisch wordt vernieuwd, mogelijk hebt u de verificatie van een in behandeling domein. Houd rekening met het volgende: 

- GoDaddy dit App Service-certificaten genereert, vereist verificatie van het domein om de drie jaar. De domeinbeheerder ontvangt een e-mailbericht om de drie jaar om te controleren of het domein. Controleer het e-mailbericht of verifieer uw domein voorkomt u het App Service-certificaat dat automatisch wordt vernieuwd. 
- Alle App Service-certificaten uitgegeven vóór maart 31 2017 moeten Herverificatie van domein op het moment van de volgende vernieuwing (zelfs als de automatisch verlengen is ingeschakeld voor het certificaat). Dit is een resultaat van de wijziging in GoDaddy-beleid. Controleer je e-mail en voltooit deze domeinverificatie eenmalige om door te gaan automatisch verlengen van het App Service-certificaat. 

## <a name="more-resources"></a>Meer bronnen

* [Een SSL-certificaat gebruiken in uw toepassingscode in Azure App Service](app-service-web-ssl-cert-load.md)
* [Veelgestelde vragen over: App Service-certificaten](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)