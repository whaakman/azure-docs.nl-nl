---
title: Gebruik client SSL-certificaat in de code van de toepassing - Azure App Service | Microsoft Docs
description: Informatie over het gebruik van clientcertificaten verbinding maken met externe bronnen die ze nodig hebben.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 763aadc50a8760b4265dbfc21e9278f909b68433
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342014"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Een SSL-certificaat in de code van uw toepassing in Azure App Service gebruiken

In deze gebruiksaanwijzing ziet u hoe u SSL-certificaten kan gebruiken, die u hebt geüpload of geïmporteerd in uw App Service-app in de code van uw toepassing. Een voorbeeld van deze use-case is dat uw app toegang heeft tot een externe service die certificaatverificatie vereist. 

Deze aanpak voor het gebruik van SSL-certificaten in uw code maakt gebruik van de SSL-functionaliteit in App Service, die vereist dat uw app in de **Basic** laag of hoger zit. Een alternatief is het opnemen van het certificaatbestand in de toepassingsmap en rechtstreeks laden (Zie [alternatieve: laad certificaat als een bestand](#file)). Maar, dit alternatief laat u niet toe om de persoonlijke sleutel in het certificaat te verbergen van de toepassingscode of de ontwikkelaar. Bovendien, als de code van uw toepassing zich in een open-source-opslagplaats bevindt, is een certificaat met een persoonlijke sleutel in de opslagplaats geen optie.

Als u App Service uw SSL-certificaten laat beheren, kunt u de certificaten afzonderlijk houden van de code van uw toepassing en zo uw gevoelige gegevens beschermen.

## <a name="prerequisites"></a>Vereisten

Om deze gebruiksaanwijzing te voltooien:

- [Een App Service-app maken](/azure/app-service/)
- [Een aangepaste DNS-naam aan uw web-app toewijzen](app-service-web-tutorial-custom-domain.md)
- [Een SSL-certificaat uploaden](app-service-web-tutorial-custom-ssl.md) of [importeren van een App Service Certificate](web-sites-purchase-ssl-web-site.md) aan uw web-app


## <a name="load-your-certificates"></a>Laden van uw certificaten

Voor het gebruik van een certificaat dat is geüpload naar of geïmporteerd in App Service, maakt u deze eerst toegankelijk voor uw toepassingscode. U doet dit met de `WEBSITE_LOAD_CERTIFICATES` app-instelling.

In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u de web-app-pagina.

Klik in het linkernavigatievenster op **SSL-certificaten**.

![Het certificaat is geüpload](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Alle geüploade en geïmporteerde SSL-certificaten voor deze web-app worden hier weergegeven met de vingerafdrukken. Kopieer de vingerafdruk van het certificaat dat u wilt gebruiken.

Klik in het linkernavigatievenster op **Toepassingsinstellingen**.

Voeg een app-instelling toe met de naam `WEBSITE_LOAD_CERTIFICATES` en stel de waarde in van de vingerafdruk van het certificaat. Om meerdere certificaten toegankelijk te maken kan u de vingerafdrukken door komma's scheiden. Om alle certificaten toegankelijk te maken, kunt u de waarde instellen op `*`. Let op, hiermee plaatst u het certificaat in de `CurrentUser\My`.

![App-instelling configureren](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Wanneer u klaar bent, klikt u op **Opslaan**.

Het geconfigureerde certificaat is nu klaar om gebruikt te worden door uw code.

## <a name="use-certificate-in-c-code"></a>Certificaat gebruiken in C#-code

Wanneer uw certificaat toegankelijk is, kunt u het benaderen in C#-code door de vingerafdruk van het certificaat. De volgende code laadt een certificaat met de vingerafdruk `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="alternative-load-certificate-as-a-file"></a>Alternatief: certificaat als een bestand laden

In deze sectie wordt beschreven hoe u een certificaatbestand inlaad dat zich in de toepassingsmap bevindt. 

Het volgende C#-voorbeeld laadt een certificaat in met de naam `mycert.pfx` uit de `certs` map van de opslagplaats van uw app.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

