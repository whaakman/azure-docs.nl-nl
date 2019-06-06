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
ms.date: 05/29/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ead1892062912840c9931ae60d11c90975ad26ac
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475120"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Een SSL-certificaat in de code van uw toepassing in Azure App Service gebruiken

In deze gebruiksaanwijzing laat zien hoe het gebruik van openbare of persoonlijke certificaten in de code van uw toepassing. Een voorbeeld van deze use-case is dat uw app toegang heeft tot een externe service die certificaatverificatie vereist.

Deze aanpak voor het gebruik van certificaten in uw code wordt gebruikgemaakt van het SSL-functionaliteit in App Service, die vereist uw app dat zich in **Basic** laag of hoger. U kunt ook [het certificaatbestand in de opslagplaats van uw app opnemen](#load-certificate-from-file), maar het is niet aanbevolen voor persoonlijke certificaten.

Als u App Service uw SSL-certificaten laat beheren, kunt u de certificaten afzonderlijk houden van de code van uw toepassing en zo uw gevoelige gegevens beschermen.

## <a name="upload-a-private-certificate"></a>Een persoonlijk certificaat uploaden

Voordat u een persoonlijk certificaat uploadt, zorg ervoor dat [deze voldoet aan alle vereisten](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate), behalve dat hoeft niet te worden geconfigureerd voor verificatie van de Server.

Wanneer u klaar om te uploaden bent, voert u de volgende opdracht uit in de <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Kopieer de certificaatvingerafdruk van het en Zie [toegankelijk maken van het certificaat](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Een openbaar certificaat uploaden

Openbare certificaten worden ondersteund in de *.cer* indeling. Het uploaden van een openbaar certificaat, de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, en Ga naar de app.

Klik op **SSL-instellingen** > **openbare certificaten (.cer)**  > **openbaar certificaat uploaden** in de linkernavigatiebalk van uw app.

In **naam**, typ een naam voor het certificaat. In **CER-certificaatbestand**, selecteert u het CER-bestand.

Klik op **Uploaden**.

![Openbaar certificaat uploaden](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Zodra het certificaat is geüpload, Kopieer de certificaatvingerafdruk van het en Zie [toegankelijk maken van het certificaat](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>Een App Service-certificaat importeren

Zie [kopen en configureren van een SSL-certificaat voor Azure App Service](web-sites-purchase-ssl-web-site.md).

Zodra het certificaat is geïmporteerd, Kopieer de certificaatvingerafdruk van het en Zie [toegankelijk maken van het certificaat](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Het certificaat toegankelijk maken

Voor het gebruik van een certificaat geüpload of geïmporteerd in uw app-code, moet u de vingerafdruk toegankelijk zijn met de `WEBSITE_LOAD_CERTIFICATES` app-instelling met de volgende opdracht de <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Als u al uw certificaten toegankelijk is, kunt u de waarde instelt op `*`.

> [!NOTE]
> Deze instelling wordt de opgegeven certificaten in de [huidige User\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) store voor de meeste Prijscategorieën, maar in de **geïsoleerd** laag (dat wil zeggen app wordt uitgevoerd in een [App Service Environment](environment/intro.md)), wordt de certificaten in de [lokale Machine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) opslaan.
>

![App-instelling configureren](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Wanneer u klaar bent, klikt u op **Opslaan**.

De geconfigureerde certificaten zijn nu gereed om te worden gebruikt door uw code.

## <a name="load-the-certificate-in-code"></a>Laden van het certificaat in de code

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
## <a name="load-certificate-from-file"></a>Belasting-certificaat uit bestand

Als u laden bestand van een certificaat uit de toepassingsdirectory van uw wilt, is het beter om te uploaden met behulp van [FTPS](deploy-ftp.md) in plaats van [Git](deploy-local-git.md), bijvoorbeeld. Houd rekening met gevoelige gegevens, zoals een persoonlijk certificaat buiten het besturingselement.

Hoewel u het bestand rechtstreeks in uw .NET-code laadt, verifieert de bibliotheek nog steeds als de huidige gebruikersprofiel wordt geladen. Voor het laden van het huidige profiel instellen de `WEBSITE_LOAD_USER_PROFILE` app-instelling met de volgende opdracht in de <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Zodra deze instelling is ingesteld, worden de volgende C# voorbeeld wordt een certificaat met de naam geladen `mycert.pfx` uit de `certs` map van de opslagplaats van uw app.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
