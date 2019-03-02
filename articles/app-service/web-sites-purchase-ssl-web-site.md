---
title: Kopen en configureren van een SSL-certificaat van Azure - App Service | Microsoft Docs
description: Leer hoe u een App Service-certificaat kopen en te binden aan uw App Service-app
services: app-service
documentationcenter: .net
author: cephalin
manager: jpconnoc
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: apurvajo;cephalin
ms.custom: seodec18
ms.openlocfilehash: 3e113639dbe4220b943d49dc610ee22b6416e12a
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216574"
---
# <a name="buy-and-configure-an-ssl-certificate-for-azure-app-service"></a>Kopen en configureren van een SSL-certificaat voor Azure App Service

Deze zelfstudie leert u over het beveiligen van uw [App Service-app](https://docs.microsoft.com/azure/app-service/) of [functie-app](https://docs.microsoft.com/azure/azure-functions/) door te maken (schaffen) een App Service-certificaat in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) en vervolgens te binden aan een App Service-app.

> [!TIP]
> App Service-certificaten kunnen worden gebruikt voor alle Azure of niet - Azure-Services en is niet beperkt tot de App-Services. Om dit te doen, moet u een lokaal PFX-exemplaar van een App Service-certificaat dat u kunt gebruiken waar die u wilt maken. Zie voor meer informatie, [het maken van een lokale kopie van het pfx-van een App Service Certificate](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).
>

## <a name="prerequisites"></a>Vereisten

In deze gebruiksaanwijzing volgen:

- [Een App Service-app maken](/azure/app-service/)
- [Een domeinnaam toewijzen aan uw app](app-service-web-tutorial-custom-domain.md) of [kopen en configureren in Azure](manage-custom-dns-buy-domain.md)

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="start-certificate-order"></a>Certificaatbestelling starten

De volgorde van een App Service-certificaat te starten in de <a href="https://portal.azure.com/#create/Microsoft.SSL" target="_blank">App Service Certificate-pagina maken</a>.

![Het maken van certificaten](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Gebruik de volgende tabel om u te helpen u bij het configureren van het certificaat. Klik op **Create** als u klaar bent.

| Instelling | Description |
|-|-|
| Name | Een beschrijvende naam voor uw App Service-certificaat. |
| Hostnaam van domein zonder voorvoegsel | Als u het hoofddomein hier opgeeft, krijgt u een certificaat dat wordt beveiligd *beide* het hoofddomein en de `www` subdomein. Aan beveiligde elk subdomein alleen, geef de volledig gekwalificeerde domeinnaam van het subdomein hier (bijvoorbeeld `mysubdomain.contoso.com`). |
| Abonnement | Het datacenter waar de web-app wordt gehost. |
| Resourcegroep | De resourcegroep die het certificaat bevat. U kunt een nieuwe resourcegroep gebruiken of u kunt bijvoorbeeld dezelfde resourcegroep bevinden als uw App Service-app selecteren. |
| Certificaat-SKU | Bepaalt het type certificaat te maken, of een standaard-certificaat of een [jokertekencertificaat](https://wikipedia.org/wiki/Wildcard_certificate). |
| Juridische voorwaarden | Klik om te bevestigen dat u akkoord met de juridische bepalingen gaat. De certificaten worden verkregen via godaddy is gekocht. |

## <a name="store-in-azure-key-vault"></a>Store in Azure Key Vault

Als het aankoopproces certificaat voltooid is, bent er meer stappen die u voltooien moet voordat u kunt beginnen met het certificaat. 

Selecteer het certificaat in de [App Service-certificaten](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) pagina en klik vervolgens op **Certificaatconfiguratie** > **stap 1: Store**.

![afbeelding van gereed is om op te slaan in KV invoegen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) is een Azure-service waarmee de beveiliging van cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. Dit is de opslag van keuze voor App Service-certificaten.

In de **Key Vault-Status** pagina, klikt u op **Key Vault-opslagplaats** te maken van een nieuwe kluis of kies een bestaande kluis. Als u ervoor kiest om een nieuwe kluis te maken, gebruikt u de volgende tabel om u te helpen u bij het configureren van de kluis en klik op maken. Zie nieuwe Key Vault binnen hetzelfde abonnement en de resourcegroep-groep maken.

| Instelling | Description |
|-|-|
| Name | Een unieke naam die bestaat uit voor alfanumerieke tekens en streepjes bevatten. |
| Resourcegroep | Selecteer dezelfde resourcegroep bevinden als uw App Service-certificaat als een aanbeveling. |
| Locatie | Selecteer de dezelfde locatie als uw App Service-app. |
| Prijscategorie | Zie voor meer informatie, [Azure Key Vault prijsinformatie](https://azure.microsoft.com/pricing/details/key-vault/). |
| Toegangsbeleid| Definieert de toepassingen en de toegang hebben tot de resources van de kluis. U kunt deze later configureren de stappen in [verschillende toepassingen toegang verlenen tot een key vault](../key-vault/key-vault-group-permissions-for-apps.md). |
| Toegang tot virtueel netwerk | Kluis-toegang beperken tot bepaalde virtuele netwerken van Azure. U kunt deze later configureren de stappen in [configureren van Azure Key Vault Firewalls en virtuele netwerken](../key-vault/key-vault-network-security.md) |

Nadat u de kluis hebt geselecteerd, sluit u de **Key Vault-opslagplaats** pagina. De **Store** optie moet een groen vinkje voor succes worden weergegeven. Houd de pagina geopend zodat de volgende stap.

## <a name="verify-domain-ownership"></a>Domeineigendom controleren

Uit dezelfde **Certificaatconfiguratie** pagina die u hebt gebruikt in de vorige stap, klikt u op **stap 2: Controleer of**.

![](./media/app-service-web-purchase-ssl-web-site/verify-domain.png)

Selecteer **App Service-verificatie**. Nadat u het domein al aan uw web-app toegewezen (Zie [vereisten](#prerequisites)), het al geverifieerd. Klik op **controleren** voltooien van deze stap. Klik op de **vernieuwen** knop totdat het bericht **certificaat is geverifieerd domein** wordt weergegeven.

> [!NOTE]
> Vier typen verificatiemethoden domein worden ondersteund: 
> 
> - **App Service** -de meest geschikte optie bij het domein al aan een App Service-app in hetzelfde abonnement toegewezen is. Het maakt gebruik van het feit dat de App Service-app al het domeineigendom is geverifieerd.
> - **Domein** -Controleer of een [App Service-domein dat u hebt aangeschaft via Azure](manage-custom-dns-buy-domain.md). Azure wordt automatisch de verificatie-TXT-record voor u wordt toegevoegd en het proces is voltooid.
> - **E-mail** -Controleer of het domein door een e-mailbericht verzenden naar de beheerder van het domein. Instructies vindt u wanneer u de optie selecteert.
> - **Handmatige** -Controleer of het domein met een HTML-pagina (**Standard** alleen het certificaat) of een DNS TXT-record. Instructies vindt u wanneer u de optie selecteert.

## <a name="bind-certificate-to-app"></a>Certificaat binden aan de app

In de  **[Azure-portal](https://portal.azure.com/)**, selecteert u in het menu links **App Services** > **\<your_ app >**.

Selecteer in het linkernavigatievenster van uw app, **SSL-instellingen** > **persoonlijke certificaten (.pfx)** > **importeren van App Service Certificate**.

![afbeelding van het certificaat importeren invoegen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Selecteer het certificaat dat u zojuist hebt aangeschaft.

Nu het certificaat is geïmporteerd, moet u om dit te binden aan een toegewezen domein in uw app. Selecteer **bindingen** > **SSL-Binding toevoegen**. 

![afbeelding van het certificaat importeren invoegen](./media/app-service-web-purchase-ssl-web-site/AddBinding.png)

Gebruik de volgende tabel om u te helpen u bij het configureren van de binding in de **SSL-bindingen** dialoogvenster, klikt u vervolgens op **Binding toevoegen**.

| Instelling | Description |
|-|-|
| Hostnaam | De domeinnaam om toe te voegen voor SSL-binding. |
| Vingerafdruk van het persoonlijke certificaat | Het certificaat te binden. |
| SSL-type | <ul><li>**SNI SSL** -op basis van meerdere SNI SSL-bindingen kunnen worden toegevoegd. Met deze optie kunnen meerdere SSL-certificaten verschillende domeinen beveiligen op hetzelfde IP-adres. De meeste moderne browsers (waaronder Internet Explorer, Chrome, Firefox en Opera) ondersteunen SNI. Ga voor uitgebreidere informatie over browserondersteuning naar [Servernaamindicatie](https://wikipedia.org/wiki/Server_Name_Indication).</li><li>**Op IP gebaseerde SSL**: er kan slechts één op IP gebaseerde SSL-binding worden toegevoegd. Met deze optie kan slechts één SSL-certificaat een specifiek openbaar IP-adres beveiligen. Nadat u de binding geconfigureerd, volg de stappen in [opnieuw toewijzen van een record voor IP SSL](app-service-web-tutorial-custom-ssl.md#remap-a-record-for-ip-ssl). </li></ul> |

## <a name="verify-https-access"></a>Controleren of de HTTPS-toegang

Ga naar uw app met `HTTPS://<domain_name>` in plaats van `HTTP://<domain_name>` om te verifiëren dat het certificaat correct is geconfigureerd.

## <a name="rekey-certificate"></a>Certificaat opnieuw versleutelen

Als u denkt dat uw certificaat's persoonlijke sleutel is aangetast, kunt u uw certificaat opnieuw versleutelen. Selecteer het certificaat in de [App Service-certificaten](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) pagina en selecteer vervolgens **opnieuw versleutelen en synchroniseren** in de linkernavigatiebalk.

Klik op **opnieuw versleutelen** om het proces te starten. Dit proces kan 1-10 minuten duren.

![afbeelding van SSL opnieuw versleutelen invoegen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Uw certificaat opnieuw genereren van sleutels, wordt het certificaat met een nieuw certificaat is uitgegeven door de certificeringsinstantie getotaliseerd.

Nadat de bewerking opnieuw versleutelen voltooid is, klikt u op **synchronisatie**. De synchronisatiebewerking automatisch bijgewerkt met de hostnaambindingen voor het certificaat in App Service zonder eventuele uitvaltijd voor uw apps.

> [!NOTE]
> Als u niet op **synchronisatie**, App Service automatisch worden gesynchroniseerd met uw certificaat binnen 48 uur.

## <a name="renew-certificate"></a>Certificaat vernieuwen

Als u wilt inschakelen voor de automatische vernieuwing van uw certificaat op elk gewenst moment, selecteert u het certificaat in de [App Service-certificaten](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) pagina en klik vervolgens op **instellingen voor automatisch verlengen** in het linkernavigatievenster.

Selecteer **op** en klikt u op **opslaan**. Certificaten kunnen beginnen met 60 dagen voordat het verloopt automatisch vernieuwen als u ingeschakeld voor de automatische vernieuwing hebt.

![certificaat automatisch vernieuwen](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Als u wilt handmatig vernieuwen van het certificaat in plaats daarvan, klikt u op **handmatig verlengen**. U kunt aanvragen naar uw certificaat handmatig vernieuwen 60 dagen voordat het verloopt.

Nadat de bewerking voor verlengen voltooid is, klikt u op **synchronisatie**. De synchronisatiebewerking automatisch bijgewerkt met de hostnaambindingen voor het certificaat in App Service zonder eventuele uitvaltijd voor uw apps.

> [!NOTE]
> Als u niet op **synchronisatie**, App Service automatisch worden gesynchroniseerd met uw certificaat binnen 48 uur.

## <a name="automate-with-scripts"></a>Automatiseren met scripts

### <a name="azure-cli"></a>Azure-CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="more-resources"></a>Meer bronnen

* [HTTPS afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions)
* [Een SSL-certificaat in de code van uw toepassing in Azure App Service gebruiken](app-service-web-ssl-cert-load.md)
* [FAQ: App Service-certificaten](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)
