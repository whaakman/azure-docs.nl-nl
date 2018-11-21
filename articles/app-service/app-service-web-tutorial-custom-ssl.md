---
title: Een bestaand aangepast SSL-certificaat met Azure Web Apps verbinden | Microsoft Docs
description: Meer informatie over het verbinden van een aangepast SSL-certificaat met een web-app, de back-end voor een mobiele app of een API-app in Azure App Service.
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: ''
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a543561658d593398ca74f8ae68dd6d0d27bcdaa
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636441"
---
# <a name="tutorial-bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Zelfstudie: een bestaand aangepast SSL-certificaat met Azure Web Apps verbinden

Azure Web Apps biedt een uiterst schaalbare webhostingservice met self-patchfunctie. In deze zelfstudie wordt uitgelegd hoe u een aangepast SSL-certificaat dat u hebt aangeschaft via een vertrouwde certificeringsinstantie met [Azure Web Apps](app-service-web-overview.md) kunt verbinden. Wanneer u klaar bent, kunt u uw web-app openen op het HTTPS-eindpunt van uw aangepaste DNS-domein.

![Web-app met aangepast SSL-certificaat](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De prijscategorie van uw app upgraden
> * Uw aangepaste certificaat met App Service binden
> * Certificaten vernieuwen
> * HTTPS afdwingen
> * TLS 1.1/1.2 afdwingen
> * TLS-beheer automatiseren met scripts

> [!NOTE]
> Als u een aangepast SSL-certificaat nodig hebt, kunt u er een rechtstreeks in Azure Portal ophalen en met uw web-app binden. Volg de [zelfstudie voor App Service Certificates](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- [Een App Service-app maken](/azure/app-service/)
- [Een aangepaste DNS-naam aan uw web-app toewijzen](app-service-web-tutorial-custom-domain.md)
- Een SSL-certificaat van een vertrouwde certificeringsinstantie verkrijgen
- De persoonlijke sleutel die u hebt gebruikt voor het ondertekenen van de aanvraag voor het SSL-certificaat

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Vereisten voor uw SSL-certificaat

Als u een certificaat in App Service wilt gebruiken, moet het certificaat aan de volgende vereisten voldoen:

* Ondertekend door een vertrouwde certificeringsinstantie
* Geëxporteerd als een PFX-bestand met wachtwoordbeveiliging
* Bevat een persoonlijke sleutel van minstens 2048 bits
* Bevat alle tussenliggende certificaten in de certificaatketen

> [!NOTE]
> **ECC-certificaten (cryptografie met behulp van elliptische krommen)** kunnen met App Service worden gebruikt, maar worden niet in dit artikel beschreven. Werk samen met uw certificeringsinstantie aan de exacte stappen voor het maken van ECC-certificaten.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Uw SSL-certificaat binden

U bent klaar om uw SSL-certificaat naar uw web-app te uploaden.

### <a name="merge-intermediate-certificates"></a>Tussenliggende certificaten samenvoegen

Als uw certificeringsinstantie meerdere certificaten in de certificaatketen geeft, moet u de certificaten in de juiste volgorde samenvoegen. 

U doet dit door elk certificaat dat u hebt ontvangen in een teksteditor te openen. 

Maak een bestand voor het samengevoegde certificaat met de naam _mergedcertificate.crt_. Kopieer de inhoud van elk certificaat in dit bestand in een teksteditor. De volgorde van uw certificaten moet de volgorde in de certificaatketen volgen, beginnend met uw certificaat en eindigend met het hoofdcertificaat. Het lijkt op het volgende voorbeeld:

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

### <a name="export-certificate-to-pfx"></a>Certificaat naar PFX exporteren

Exporteer uw samengevoegde SSL-certificaat met de persoonlijke sleutel die met uw certificaataanvraag is gegenereerd.

Als u de certificaataanvraag met OpenSSL hebt gegenereerd, hebt u een bestand met een persoonlijke sleutel gemaakt. Voer de volgende opdracht uit om uw certificaat naar PFX te exporteren. Vervang de tijdelijke aanduidingen _&lt;private-key-file>_ en _&lt;merged-certificate-file>_ door de paden naar uw persoonlijke sleutel en uw bestand met samengevoegde certificaten.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Wanneer u daarom wordt gevraagd, geeft u een wachtwoord voor export op. U gebruikt dit wachtwoord later wanneer u uw SSL-certificaat naar App Service uploadt.

Als u IIS of _Certreq.exe_ hebt gebruikt voor het genereren van uw certificaataanvraag, installeert u het certificaat op uw lokale computer en [exporteert u het certificaat naar PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Uw SSL-certificaat uploaden

Als u het SSL-certificaat wilt uploaden, klikt u in het linkernavigatievenster van de web-app op **SSL-instellingen**.

Klik op **Certificaat uploaden**. 

In **PFX-certificaatbestand** selecteert u uw PFX-bestand. Typ in **Certificaatwachtwoord** het wachtwoord dat u hebt gemaakt toen u het PFX-bestand exporteerde.

Klik op **Uploaden**.

![Certificaat uploaden](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Wanneer App Service klaar is met het uploaden van uw certificaat, wordt het op de pagina **SSL-instellingen** weergegeven.

![Het certificaat is geüpload](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Uw SSL-certificaat binden

Klik in het gedeelte **SSL-bindingen** op **Binding toevoegen**.

Op de pagina **SSL-binding toevoegen** gebruikt u de vervolgkeuzelijsten om de domeinnaam die moet worden beveiligd en het certificaat dat moet worden gebruikt te selecteren.

> [!NOTE]
> Als u uw certificaat hebt geüpload, maar de domeinnaam/-namen niet wordt/worden weergegeven in de vervolgkeuzelijst **Hostnaam**, probeert u de browserpagina te vernieuwen.
>
>

Selecteer in **SSL-type** of u **[Servernaamindicatie (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** wilt gebruiken of op IP gebaseerde SSL.

- **SSL op basis van SNI**: meerdere SSL-bindingen op basis van SNI kunnen worden toegevoegd. Met deze optie kunnen meerdere SSL-certificaten verschillende domeinen beveiligen op hetzelfde IP-adres. De meeste moderne browsers (waaronder Internet Explorer, Chrome, Firefox en Opera) ondersteunen SNI. Ga voor uitgebreidere informatie over browserondersteuning naar [Servernaamindicatie](http://wikipedia.org/wiki/Server_Name_Indication).
- **Op IP gebaseerde SSL**: er kan slechts één op IP gebaseerde SSL-binding worden toegevoegd. Met deze optie kan slechts één SSL-certificaat een specifiek openbaar IP-adres beveiligen. Als u meerdere domeinen wilt beveiligen, moet u ze allemaal met hetzelfde SSL-certificaat beveiligen. Dit is de traditionele optie voor SSL-binding.

Klik op **Binding toevoegen**.

![SSL-certificaat binden](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Wanneer App Service klaar is met het uploaden van uw certificaat, wordt het in het gedeelte **SSL-bindingen** weergegeven.

![Certificaat dat aan web-app is gebonden](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Een record voor IP SSL opnieuw toewijzen

Als u geen op IP gebaseerde SSL in uw web-app gebruikt, gaat u naar [Test HTTPS for your custom domain](#test) (HTTPS voor uw aangepast domein testen).

Uw web-app maakt standaard gebruik van een gedeeld openbaar IP-adres. Wanneer u een certificaat met op IP gebaseerde SSL bindt, maakt App Service een nieuw, specifiek IP-adres voor uw web-app.

Als u een A-record aan uw web-app hebt toegewezen, werkt u uw domeinregister bij met dit nieuwe, specifieke IP-adres.

De pagina **Aangepaste domeinen** van uw web-app wordt bijgewerkt met het nieuwe, specifieke IP-adres. [Kopieer dit IP-adres](app-service-web-tutorial-custom-domain.md#info) en [wijs de A-record opnieuw toe](app-service-web-tutorial-custom-domain.md#map-an-a-record) aan dit nieuwe IP-adres.

<a name="test"></a>

## <a name="test-https"></a>HTTPS testen

Nu hoeft u alleen nog maar te controleren of HTTPS werkt voor uw aangepaste domein. Browse in verschillende browsers naar `https://<your.custom.domain>` om te controleren of het naar uw web-app leidt.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Als uw web-app certificaatvalidatiefouten geeft, gebruikt u waarschijnlijk een zelfondertekend certificaat.
>
> Als dit niet het geval is, hebt u mogelijk tussenliggende certificaten weggelaten toen u uw certificaat naar het PFX-bestand exporteerde.

<a name="bkmk_enforce"></a>

## <a name="renew-certificates"></a>Certificaten vernieuwen

Uw inkomende IP-adres kan wijzigen wanneer u een binding verwijdert, zelfs als die binding IP-gebaseerd is. Dit is vooral belangrijk wanneer u een certificaat vernieuwt dat zich al in een IP-gebaseerde binding bevindt. Als u een wijziging in het IP-adres van uw app wilt voorkomen, volgt u in volgorde de volgende stappen:

1. Upload het nieuwe certificaat.
2. Verbind het nieuwe certificaat aan het aangepaste domein dat u wilt, zonder het oude certificaat te verwijderen. Met deze actie wordt de oude binding vervangen en niet verwijderd.
3. Verwijder het oude certificaat. 

## <a name="enforce-https"></a>HTTPS afdwingen

Standaard heeft iedereen nog steeds toegang tot uw web-app via HTTP. U kunt alle HTTP-aanvragen omleiden naar de HTTPS-poort.

Selecteer in het linkernavigatievenster van de web-app-pagina **SSL-instellingen**. Klik op **Alleen HTTPS** en selecteer **Aan**.

![HTTPS afdwingen](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Wanneer de bewerking is voltooid, gaat u naar een van de HTTP-URL's die naar uw app verwijzen. Bijvoorbeeld:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>TLS-versies afdwingen

Voor uw app is standaard [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 toegestaan, wat het aanbevolen TLS-niveau is volgens industrienormen zoals [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Als u andere TLS-versies wilt afdwingen, volgt u deze stappen:

Selecteer in het linkernavigatievenster van de web-app-pagina **SSL-instellingen**. Selecteer vervolgens bij **TLS-versie** de gewenste minimale TLS-versie. Met deze instelling controleert u alleen de inkomende oproepen. 

![TLS 1.1 of 1.2 afdwingen](./media/app-service-web-tutorial-custom-ssl/enforce-tls1.2.png)

Als de bewerking is voltooid, worden in de app alle verbindingen met lagere TLS-versies geweigerd.

## <a name="automate-with-scripts"></a>Automatiseren met scripts

U kunt SSL-bindingen voor uw web-app met scripts automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure-CLI

Met de volgende opdracht wordt een geëxporteerd PFX-bestand geüpload en de vingerafdruk opgehaald.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Met de volgende opdracht wordt een op SNI gebaseerde SSL-binding toegevoegd met behulp van de vingerafdruk van de vorige opdracht.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

Met de volgende opdracht wordt een minimale TLS-versie van TLS 1.2 afgedwongen.

```bash
az webapp config set \
    --name <app_name> \
    --resource-group <resource_group_name>
    --min-tls-version 1.2
```

### <a name="azure-powershell"></a>Azure PowerShell

Met de volgende opdracht wordt een geëxporteerd PFX-bestand geüpload en een op SNI gebaseerde SSL-binding toegevoegd.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Openbare certificaten (optioneel)
Als uw app clienttoegang nodig heeft tot externe resources en er voor de externe resource verificatie via een certificaat is vereist, kunt u [openbare certificaten](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) naar uw web-app uploaden. Er zijn geen openbare certificaten vereist voor SSL-bindingen van uw app.

Voor meer details over een openbaar certificaat in uw app laden en gebruiken, raadpleegt u [Use an SSL certificate in your application code in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-ssl-cert-load) (Een SSL-certificaat in uw toepassingscode gebruiken in Azure App Service). U kunt openbare certificaten ook gebruiken voor apps in App Service-omgevingen. Als u het certificaat in het LocalMachine-certificaatarchief wilt opslaan, moet u een web-app in een App Service-omgeving gebruiken. Zie [How to configure public certificates to your Web App](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer) (Openbare certificaten naar uw web-app configureren) voor meer informatie.

![Openbaar certificaat uploaden](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De prijscategorie van uw app upgraden
> * Uw aangepaste certificaat met App Service binden
> * Certificaten vernieuwen
> * HTTPS afdwingen
> * TLS 1.1/1.2 afdwingen
> * TLS-beheer automatiseren met scripts

Ga naar de volgende zelfstudie voor meer informatie over het gebruik van Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Een netwerk voor contentlevering toevoegen aan een Azure App Service](../cdn/cdn-add-to-web-app.md)

Zie [Use an SSL certificate in your application code in Azure App Service](app-service-web-ssl-cert-load.md) (Een SSL-certificaat gebruiken in uw toepassingscode in Azure App Service) voor meer informatie.
