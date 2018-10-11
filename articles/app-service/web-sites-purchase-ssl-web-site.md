---
title: Kopen en configureren van een SSL-certificaat voor uw Azure App Service | Microsoft Docs
description: Leer hoe u een App Service-certificaat kopen en te binden aan uw App Service-app
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
ms.openlocfilehash: 0c2adcfa4e11e444f66e1a9c04bea6e3d352f117
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077723"
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Een SSL-certificaat kopen en configureren voor uw Azure App Service

Deze zelfstudie leert u hoe u kunt uw web-app beveiligen door het aanschaffen van een SSL-certificaat voor uw  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, veilig op te slaan in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis), en koppelen aan een aangepast domein.

## <a name="step-1---sign-in-to-azure"></a>Stap 1: aanmelden bij naar Azure

Meld u aan bij Azure portal op http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Stap 2: een SSL-certificaat bestelling plaatsen

U kunt een SSL-certificaat bestelling plaatsen door het maken van een nieuw [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In de **Azure-portal**.

![Het maken van certificaten](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Voer een beschrijvende **naam** voor uw SSL-certificaat en voer de **domeinnaam**

> [!NOTE]
> Deze stap is een van de belangrijkste onderdelen van het aankoopproces. Zorg ervoor dat u de juiste hostnaam (aangepast domein) die u wilt beveiligen met dit certificaat opgeven. **GEEN** voegt u vóór de naam van de Host met WWW. 
>

Selecteer uw **abonnement**, **resourcegroep**, en **SKU van het certificaat**

> [!TIP]
> App Service-certificaten kunnen worden gebruikt voor alle Azure of niet - Azure-Services en is niet beperkt tot de App-Services. Om dit te doen, moet u een lokaal PFX-exemplaar van een App Service-certificaat dat u kunt gebruiken waar die u wilt maken. Lees voor meer informatie, [het maken van een lokale kopie van het pfx-van een App Service Certificate](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Stap 3: Store van het certificaat in Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) is een Azure-service waarmee de beveiliging van cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services.
>

Zodra de aankoop van het SSL-certificaat voltooid is, moet u opent de [App Service-certificaten](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) pagina.

![afbeelding van gereed is om op te slaan in KV invoegen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

De certificaatstatus van het is **'Uitgifte in behandeling'** als er meer stappen u voltooien moet voordat u kunt beginnen met het certificaat.

Klik op **Certificaatconfiguratie** pagina in de eigenschappen voor certificaat en klik op **stap 1: Store** dit certificaat wordt opgeslagen in Azure Key Vault.

Uit de **Key Vault-Status** pagina, klikt u op **Key Vault-opslagplaats** kiezen van een bestaande Sleutelkluis voor het opslaan van dit certificaat **of maak nieuwe Key Vault** nieuwe Key Vault maken binnen hetzelfde abonnement en de resourcegroep-groep.

> [!NOTE]
> Azure Key Vault heeft minimale kosten in rekening gebracht voor het opslaan van dit certificaat.
> Zie voor meer informatie,  **[prijsinformatie voor Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Nadat u hebt geselecteerd de Key Vault-opslagplaats voor het opslaan van dit certificaat in de **Store** optie succes moet worden weergegeven.

![afbeelding van store succes in KV invoegen](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Stap 4: het Domeineigendom controleren

Uit dezelfde **Certificaatconfiguratie** pagina die u hebt gebruikt in stap 3, klikt u op **stap 2: Controleer of**.

Kies de gewenste domein-verificatiemethode. 

Er zijn vier typen domeinverificatie wordt ondersteund door App Service-certificaten: App Service, het domein en handmatige verificatie. Deze verificatietypen worden uitgelegd in meer details in de [geavanceerde sectie](#advanced).

> [!NOTE]
> **App Service-verificatie** is de meest geschikte optie bij het domein dat u wilt controleren of al aan een App Service-app in hetzelfde abonnement toegewezen is. Het maakt gebruik van het feit dat de App Service-app al het domeineigendom is geverifieerd.
>

Klik op **controleren** knop om deze stap te voltooien.

![afbeelding van domeinverificatie invoegen](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Nadat u hebt geklikt **controleren**, gebruiken de **vernieuwen** knop totdat de **controleren** optie succes moet worden weergegeven.

![afbeelding invoegen van verifiëren in KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Stap 5 - certificaat toewijzen aan App Service-App

> [!NOTE]
> Voordat u de stappen in deze sectie, moet een aangepaste domeinnaam hebt gekoppeld aan uw app. Zie voor meer informatie,  **[configureren van een aangepaste domeinnaam voor een web-app.](app-service-web-tutorial-custom-domain.md)**
>

In de  **[Azure-portal](https://portal.azure.com/)**, klikt u op de **App Service** optie aan de linkerkant van de pagina.

Klik op de naam van uw app waaraan u dit certificaat wilt toewijzen.

In de **instellingen**, klikt u op **SSL-instellingen**.

Klik op **importeren van App Service Certificate** en selecteer het certificaat dat u zojuist hebt aangeschaft.

![afbeelding van het certificaat importeren invoegen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

In de **ssl-bindingen** sectie klikken op **bindingen toevoegen**, en selecteer de naam van het domein wilt beveiligen met SSL en het certificaat te gebruiken met de vervolgkeuzelijsten. U kunt ook kiezen of u wilt gebruiken **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** of op IP gebaseerd SSL.

![afbeelding van SSL-bindingen invoegen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klik op **toevoegen Binding** de wijzigingen opslaan en SSL in te schakelen.

> [!NOTE]
> Als u hebt geselecteerd **op IP gebaseerd SSL** en uw aangepaste domein is geconfigureerd met behulp van een A-record, moet u de volgende aanvullende stappen uitvoeren. Deze worden uitgelegd in meer details in de [geavanceerde sectie](#Advanced).

Op dit moment zou het mogelijk om naar uw app met te `HTTPS://` in plaats van `HTTP://` om te verifiëren dat het certificaat correct is geconfigureerd.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Stap 6 - beheertaken

### <a name="azure-cli"></a>Azure-CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Geavanceerd

### <a name="verifying-domain-ownership"></a>Domeineigendom controleren

Er zijn twee andere typen domeinverificatie wordt ondersteund door App service-certificaten: domeinverificatie uit te voeren en handmatige verificatie.

#### <a name="domain-verification"></a>Domeinverificatie

Kies deze optie alleen voor [een App Service-domein dat u hebt aangeschaft in Azure.](custom-dns-web-site-buydomains-web-app.md). Azure wordt automatisch de verificatie-TXT-record voor u wordt toegevoegd en het proces is voltooid.

#### <a name="manual-verification"></a>Handmatige verificatie

> [!IMPORTANT]
> HTML-pagina verificatie (werkt alleen met standaard certificaat-SKU.)
>

1. Maken van een HTML-bestand met de naam **"starfield.html"**

1. Inhoud van dit bestand moet de exacte naam van het Domeinverificatietoken. (U kunt het token van de pagina Domeinverificatiestatus kopiëren)

1. Dit bestand in de hoofdmap van de webserver die als host fungeert voor uw domein niet uploaden `/.well-known/pki-validation/starfield.html`

1. Klik op **vernieuwen** om bij te werken van de certificaatstatus nadat de verificatie is voltooid. Het duurt enkele minuten voor de verificatie is voltooid.

> [!TIP]
> Controleer of in een terminal met `curl -G http://<domain>/.well-known/pki-validation/starfield.html` het antwoord moet bevatten de `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Controle van DNS TXT-Record

1. Een TXT-record met behulp van uw DNS-beheer maken op de `@` subdomein met de waarde gelijk is aan het Domeinverificatietoken.
1. Klik op **'Vernieuwen'** om bij te werken van de certificaatstatus nadat de verificatie is voltooid.

> [!TIP]
> U moet een TXT-record maken in `@.<domain>` met waarde `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Certificaat toewijzen aan App Service-App

Als u hebt geselecteerd **op IP gebaseerd SSL** en uw aangepaste domein is geconfigureerd met behulp van een A-record, moet u de volgende aanvullende stappen uitvoeren:

Nadat u hebt geconfigureerd SSL-binding op basis van een IP-adres, een toegewezen IP-adres is toegewezen aan uw app. U vindt dit IP-adres op de **aangepast domein** pagina onder instellingen van uw app, rechts boven de **hostnamen** sectie. Deze wordt vermeld als **externe IP-adres**

![afbeelding van IP SSL invoegen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Dit IP-adres is anders dan het virtuele IP-adres dat eerder gebruikt voor het configureren van de A-record voor uw domein. Als u zijn geconfigureerd voor het gebruik van SNI SSL op basis van, of zijn niet geconfigureerd om SSL te gebruiken, geen adres voor deze vermelding wordt vermeld.

Met behulp van de hulpprogramma's van uw domeinnaamregistrar, wijzig de A-record voor uw aangepaste domeinnaam om te verwijzen naar het IP-adres uit de vorige stap.

## <a name="rekey-and-sync-the-certificate"></a>Opnieuw versleutelen en synchroniseren van het certificaat

Als u ooit moet u uw certificaat opnieuw versleutelen, selecteert u de **opnieuw versleutelen en synchroniseren** optie uit de **eigenschappen voor certificaat** pagina.

Klik op **opnieuw versleutelen** om het proces te starten. Dit proces kan 1-10 minuten duren.

![afbeelding van SSL opnieuw versleutelen invoegen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Uw certificaat opnieuw genereren van sleutels, wordt het certificaat met een nieuw certificaat is uitgegeven door de certificeringsinstantie getotaliseerd.

## <a name="renew-the-certificate"></a>Het certificaat vernieuwen

Als u wilt inschakelen voor de automatische vernieuwing van uw certificaat op elk gewenst moment, klikt u op **instellingen voor automatisch verlengen** in de certificate management-pagina. Selecteer **op** en klikt u op **opslaan**. Certificaten kunnen beginnen met 90 dagen voordat het verloopt automatisch vernieuwen als u ingeschakeld voor de automatische vernieuwing hebt.

![](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Als u wilt handmatig vernieuwen van het certificaat in plaats daarvan, klikt u op **handmatig verlengen** in plaats daarvan. U kunt aanvragen naar uw certificaat handmatig vernieuwen 60 dagen voordat het verloopt.

> [!NOTE]
> De vernieuwde certificaat is niet automatisch gebonden aan uw app, of u deze handmatig wordt verlengd of het automatisch vernieuwd. Als u wilt koppelen aan uw app, Zie [certificaten vernieuwen](./app-service-web-tutorial-custom-ssl.md#renew-certificates). 

## <a name="more-resources"></a>Meer bronnen

* [HTTPS afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 afdwingen](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions)
* [Een SSL-certificaat in de code van uw toepassing in Azure App Service gebruiken](app-service-web-ssl-cert-load.md)
* [Veelgestelde vragen: App Service-certificaten](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)
