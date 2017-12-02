---
title: Een bestaande aangepaste SSL-certificaat binden aan Azure Web Apps | Microsoft Docs
description: Hoe u kunt een aangepaste SSL-certificaat binden aan uw web-app, back-end voor mobiele app of API-app in Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f69bc731b2858c338d7f7b4d347e7107a0f4eeed
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Een bestaande aangepaste SSL-certificaat binden aan Azure-Web-Apps

Azure Web Apps biedt een zeer schaalbaar, zelf patch webhosting-service. Deze zelfstudie leert u hoe u kunt een aangepaste SSL-certificaat dat u hebt aangeschaft via een vertrouwde certificeringsinstantie te binden [Azure Web Apps](app-service-web-overview.md). Wanneer u klaar bent, kunt u zult toegang kunnen krijgen tot uw web-app op het HTTPS-eindpunt van uw aangepaste DNS-domein.

![Web-app met aangepaste SSL-certificaat](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Upgrade van uw app-prijscategorie
> * Uw aangepaste SSL-certificaat binden aan de App Service
> * Afdwingen van HTTPS voor uw app
> * SSL-certificaat-binding met scripts automatiseren

> [!NOTE]
> Als u nodig hebt om een aangepaste SSL-certificaat te verkrijgen, kunt u een in de Azure portal rechtstreeks ophalen en bindt dit aan uw web-app. Ga als volgt de [App Service Certificate zelfstudie](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- [Een App Service-app maken](/azure/app-service/)
- [Een aangepaste DNS-naam toegewezen aan uw web-app](app-service-web-tutorial-custom-domain.md)
- Een SSL-certificaat van een vertrouwde certificeringsinstantie verkrijgen
- De persoonlijke sleutel die u gebruikt voor het ondertekenen van de certificaataanvraag SSL

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Vereisten voor SSL-certificaat

Als u een certificaat in App Service, kan het certificaat moet voldoen aan de volgende vereisten:

* Ondertekend door een vertrouwde certificeringsinstantie
* Geëxporteerd als een PFX-wachtwoord is beveiligd bestand
* Persoonlijke sleutel van minstens 2048 bits bevat lang
* Bevat alle tussenliggende certificaten in de certificaatketen

> [!NOTE]
> **Elliptic Curve Cryptography (ECC)-certificaten** kunt werken met App Service, maar worden niet behandeld in dit artikel wordt beschreven. Werken met uw certificeringsinstantie op de exacte stappen voor het maken van ECC-certificaten.

## <a name="prepare-your-web-app"></a>Voorbereiden van uw web-app

Een aangepaste SSL-certificaat binden aan uw web-app uw [App Service-abonnement](https://azure.microsoft.com/pricing/details/app-service/) moet zich in de **Basic**, **standaard**, of **Premium** laag. In deze stap maakt ervoor u zorgen dat uw web-app is in de ondersteunde prijscategorie.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Open de [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navigeer naar uw web-app

Klik in het menu links op **App Services**, en klik vervolgens op de naam van uw web-app.

![Web-app selecteren](./media/app-service-web-tutorial-custom-ssl/select-app.png)

U bevindt zich op de beheerpagina van uw web-app.  

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Schuif in de linkernavigatiebalk van uw web-app-pagina naar de **instellingen** sectie en selecteer **opschalen (App Service-abonnement)**.

![Omhoog schalen-menu](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Controleer of uw web-app is niet in de **vrije** of **gedeelde** laag. De huidige tier uw web-app is gemarkeerd met een donker blauw vak.

![Controleer de prijscategorie](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Aangepaste SSL wordt niet ondersteund in de **vrije** of **gedeelde** laag. Als u moet worden uitgebreid, volgt u de stappen in de volgende sectie. Anders sluit de **Kies uw prijscategorie** pagina en doorgaan met [uploaden en de SSL-certificaat binden](#upload).

### <a name="scale-up-your-app-service-plan"></a>Uw App Service-abonnement opschalen

Selecteer een van de **Basic**, **standaard**, of **Premium** lagen.

Klik op **Selecteren**.

![Kies de prijscategorie](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking is voltooid.

![Melding opschalen](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>SSL-certificaat binden

U bent klaar om uw SSL-certificaat uploaden naar uw web-app.

### <a name="merge-intermediate-certificates"></a>Tussenliggende certificaten samenvoegen

Als uw certificeringsinstantie meerdere certificaten in de certificaatketen geeft, moet u de certificaten in de volgorde samenvoegen. 

U doet dit door elk certificaat dat u hebt ontvangen in een teksteditor te openen. 

Maak een bestand voor het samengevoegde certificaat, aangeroepen _mergedcertificate.crt_. Kopieer de inhoud van elk certificaat in dit bestand in een teksteditor. De volgorde van uw certificaten moet de volgorde in de certificaatketen met uw certificaat begint en eindigt met het basiscertificaat volgen. Het lijkt erop dat het volgende voorbeeld:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>PFX-certificaat exporteren

Exporteer uw samengevoegde SSL-certificaat met de persoonlijke sleutel die uw certificaataanvraag is gegenereerd met.

Als u de certificaataanvraag met het OpenSSL gegenereerd, kunt u een bestand met een persoonlijke sleutel hebt gemaakt. Voer de volgende opdracht om uw certificaat exporteren naar PFX. Vervang de tijdelijke aanduidingen  _&lt;persoonlijke sleutelbestand >_ en  _&lt;samengevoegd--certificaatbestand >_ met de paden naar uw persoonlijke sleutel en het samengevoegde certificaatbestand.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Wanneer u wordt gevraagd, moet u een wachtwoord voor export definiëren. U hebt dit wachtwoord gebruiken wanneer u later uw SSL-certificaat uploadt naar App Service.

Als u IIS gebruikt of _Certreq.exe_ bij het genereren van uw certificaataanvraag, installeer het certificaat in uw lokale computer en vervolgens [Exporteer het certificaat naar PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Uw SSL-certificaat uploaden

Als u wilt uw SSL-certificaat uploaden, klikt u op **SSL-certificaten** in het linkernavigatievenster van uw web-app.

Klik op **-certificaat uploaden**. 

In **PFX-certificaatbestand**, selecteer uw PFX-bestand. In **certificaatwachtwoord**, typ het wachtwoord dat u hebt gemaakt toen u het PFX-bestand hebt geëxporteerd.

Klik op **Uploaden**.

![Certificaat uploaden](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Wanneer de App Service klaar is met uw certificaat uploaden, wordt deze weergegeven de **SSL-certificaten** pagina.

![Certificaat geüpload](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>SSL-certificaat binden

In de **SSL-bindingen** sectie, klikt u op **binding toevoegen**.

In de **SSL-Binding toevoegen** pagina, gebruikt u de vervolgkeuzelijsten selecteren voor het beveiligen van de naam van het domein en het certificaat te gebruiken.

> [!NOTE]
> Als u uw certificaat hebt geüpload, maar niet ziet de domein-/-namen in de **hostnaam** dropdown, probeer de browserpagina te vernieuwen.
>
>

In **SSL Type**, opgeven of  **[indicatie voor Server-naam (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  of SSL op basis van IP.

- **Op basis van SNI SSL** -op basis van meerdere SNI SSL-bindingen kunnen worden toegevoegd. Deze optie kunt meerdere SSL-certificaten voor het beveiligen van meerdere domeinen op hetzelfde IP-adres. De meeste moderne browsers (met inbegrip van Internet Explorer, Chrome, Firefox en Opera) ondersteuning voor SNI (vinden uitgebreidere browser ondersteuningsinformatie op [Servernaamindicatie](http://wikipedia.org/wiki/Server_Name_Indication)).
- **IP-gebaseerde SSL** -slechts één IP-gebaseerde SSL-binding kan worden toegevoegd. Deze optie kan slechts één SSL-certificaat voor het beveiligen van een specifieke openbare IP-adres. Als u wilt beveiligen in meerdere domeinen, moet u beveiligen ze allemaal zijn gebaseerd op het SSL-certificaat. Dit is de traditionele optie voor SSL-binding.

Klik op **Binding toevoegen**.

![SSL-certificaat binden](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Wanneer de App Service klaar is met uw certificaat uploaden, wordt deze weergegeven de **SSL-bindingen** secties.

![Certificaat dat is gebonden aan web-app](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Opnieuw toewijzen van een record voor IP-SSL

Als u geen IP-gebaseerde SSL in uw web-app gebruikt, gaat u naar [Test HTTPS voor uw aangepaste domein](#test).

Uw web-app maakt standaard gebruik van een gedeelde openbare IP-adres. Wanneer u een certificaat met SSL op basis van IP verbonden, wordt een nieuwe, toegewezen IP-adres voor uw web-app in App Service gemaakt.

Als u een A-record hebt toegewezen aan uw web-app, bijwerken van uw domein register met dit nieuwe, toegewezen IP-adres.

Uw web-app **aangepaste domeinen** pagina wordt bijgewerkt met de nieuwe, toegewezen IP-adres. [Kopieer dit IP-adres](app-service-web-tutorial-custom-domain.md#info), klikt u vervolgens [opnieuw toewijzen van de A-record](app-service-web-tutorial-custom-domain.md#map-an-a-record) naar deze nieuwe IP-adres.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Alle die nog moet doen nu om ervoor te zorgen dat HTTPS voor uw aangepaste domein werkt is. Blader in verschillende browsers naar `https://<your.custom.domain>` om te zien dat het fungeert van uw web-app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Als uw web-app biedt u validatiefouten van het certificaat, gebruikt u waarschijnlijk een zelfondertekend certificaat.
>
> Als dit niet het geval is, mogelijk hebt u weggelaten tussenliggende certificaten als u uw certificaat naar het PFX-bestand exporteert.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>HTTPS afdwingen

Iedereen nog steeds standaard toegang tot uw web-app met behulp van HTTP. U kunt alle HTTP-aanvragen omleiden naar de HTTPS-poort.

Selecteer in de app webpagina in de navigatiebalk links **aangepaste domeinen**. Klik op **alleen HTTPS**, selecteer **op**.

![HTTPS afdwingen](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Wanneer de bewerking voltooid is, gaat u naar een van de HTTP-URL's die naar uw app verwijzen. Bijvoorbeeld:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="automate-with-scripts"></a>Automatiseren met behulp van scripts

U kunt SSL-bindingen automatiseren voor uw web-app met behulp van scripts, met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

De volgende opdracht een geëxporteerde PFX-bestand uploadt en de vingerafdruk van het opgehaald.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

De volgende opdracht voegt een SNI op basis van een SSL-binding met de vingerafdruk van de vorige opdracht.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

De volgende opdracht een geëxporteerde PFX-bestand uploadt en voegt een SNI op basis van een SSL-binding.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Certificaten voor openbare (optioneel)
U kunt uploaden [certificaten voor openbare](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) aan uw web-app. U kunt ook certificaten voor openbare voor apps in App Service-omgevingen. Als u het certificaat wordt opgeslagen in het certificaatarchief van de hoofdmap van LocalMachine moet, moet u een web-app op App Service-omgeving gebruiken. Zie voor meer informatie [openbare certificaten aan uw Web-App configureren](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Openbaar certificaat uploaden](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Upgrade van uw app-prijscategorie
> * Uw aangepaste SSL-certificaat binden aan de App Service
> * Afdwingen van HTTPS voor uw app
> * SSL-certificaat-binding met scripts automatiseren

Ga naar de volgende zelfstudie voor meer informatie over het gebruik van Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Een Content Delivery Network (CDN) toevoegen aan een Azure App Service](app-service-web-tutorial-content-delivery-network.md)

Zie voor meer informatie [gebruiken een SSL-certificaat in uw toepassingscode in Azure App Service](app-service-web-ssl-cert-load.md).