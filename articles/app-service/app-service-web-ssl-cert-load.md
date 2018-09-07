---
title: Gebruik een geüpload SSL-certificaat in de code van uw toepassing in Azure App Service | Microsoft Docs
description: ''
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
ms.openlocfilehash: 87c9cd5955dda1a379733e5ad48d58f8361f0e6b
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051473"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Een SSL-certificaat in de code van uw toepassing in Azure App Service gebruiken

In deze gebruiksaanwijzing ziet u hoe u een van de SSL-certificaten, u hebt geüpload of geïmporteerd in uw App Service-app in de code van uw toepassing. Een voorbeeld van de use-case is dat uw app toegang heeft tot een externe service die certificaatverificatie vereist. 

Deze aanpak voor het gebruik van SSL-certificaten in uw code wordt gebruikgemaakt van het SSL-functionaliteit in App Service, die vereist uw app dat zich in **Basic** laag of hoger. Een alternatief is het opnemen van het certificaatbestand in de toepassingsmap en rechtstreeks laden (Zie [alternatieve: load-certificaat als een bestand](#file)). Maar kunt dit alternatief u niet de persoonlijke sleutel in het certificaat van de toepassingscode of de ontwikkelaar verbergen. Bovendien, als de code van uw toepassing zich in een open-source-opslagplaats, blijven van een certificaat met een persoonlijke sleutel in de opslagplaats kan niet worden gebruikt.

Als u App Service uw SSL-certificaten te beheren, kunt u afzonderlijk behouden van de certificaten en code van uw toepassing en uw gevoelige gegevens beveiligen.

## <a name="prerequisites"></a>Vereisten

In deze gebruiksaanwijzing voltooien:

- [Een App Service-app maken](/azure/app-service/)
- [Een aangepaste DNS-naam aan uw web-app toewijzen](app-service-web-tutorial-custom-domain.md)
- [Een SSL-certificaat uploaden](app-service-web-tutorial-custom-ssl.md) of [importeren van een App Service Certificate](web-sites-purchase-ssl-web-site.md) aan uw web-app


## <a name="load-your-certificates"></a>Laden van uw certificaten

Voor het gebruik van een certificaat dat is geüpload naar of geïmporteerd in App Service, eerst deze toegankelijk maken voor uw toepassingscode. U doet dit met de `WEBSITE_LOAD_CERTIFICATES` app-instelling.

In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u de web-app-pagina.

Klik in het linkernavigatievenster op **SSL-certificaten**.

![Het certificaat is geüpload](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Alle uw geüploade en geïmporteerde SSL-certificaten voor deze web-app worden hier weergegeven met de vingerafdrukken. Kopieer de vingerafdruk van het certificaat dat u wilt gebruiken.

Klik in het linkernavigatievenster op **toepassingsinstellingen**.

Toevoegen van een app-instelling met de naam `WEBSITE_LOAD_CERTIFICATES` en stel de waarde om de vingerafdruk van het certificaat. Als u meerdere certificaten toegankelijk is, vingerafdruk door komma's gescheiden waarden te gebruiken. Als u alle certificaten die toegankelijk is, kunt u de waarde instelt op `*`. Let op dat Hiermee plaatst u het certificaat in de `CurrentUser\My` opslaan.

![App-instelling configureren](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Wanneer u klaar bent, klikt u op **opslaan**.

Het certificaat is geconfigureerd, is nu klaar om te worden gebruikt door uw code.

## <a name="use-certificate-in-c-code"></a>Certificaat gebruiken in C#-code

Als uw certificaat toegankelijk is, u toegang tot het in C#-code door de vingerafdruk van het certificaat. De volgende code wordt een certificaat met de vingerafdruk van het geladen `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

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

Deze sectie wordt beschreven hoe u en een certificaatbestand dat zich in de toepassingsmap te laden. 

De volgende C#-voorbeeld wordt een certificaat met de naam geladen `mycert.pfx` uit de `certs` map van de opslagplaats van uw app.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

