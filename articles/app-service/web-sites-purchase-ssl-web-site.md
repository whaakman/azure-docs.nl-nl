---
title: Een SSL-certificaat toevoegen aan uw app in Azure App Service | Microsoft Docs
description: Informatie over het toevoegen van een SSL-certificaat aan uw App Service-app.
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: 191dd7240ad15b4936a72bc27a2d0162350f3afb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Een SSL-certificaat kopen en configureren voor uw Azure App Service

In deze zelfstudie wordt u uw web-app beveiligen door het aanschaffen van een SSL-certificaat voor uw  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, veilig opslaan in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), en het koppelen van met een aangepast domein.

## <a name="step-1---log-in-to-azure"></a>Stap 1 - logboek in naar Azure

Aanmelden bij de Azure portal op http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Stap 2: een SSL-certificaat bestelling plaatsen

U kunt een SSL-certificaat-volgorde plaatsen door het maken van een nieuw [App Service-certificaat](https://portal.azure.com/#create/Microsoft.SSL) In de **Azure-portal**.

![Maken van het certificaat](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Voer een beschrijvende **naam** voor uw SSL-certificaat en voer de **domeinnaam**

> [!NOTE]
> Dit is een van de belangrijkste onderdelen van het aankoopproces. Zorg ervoor dat juiste host-naam (aangepaste domein) die u wilt beveiligen met dit certificaat op te geven. **GEEN** de hostnaam met WWW toevoegen. 
>

Selecteer uw **abonnement**, **resourcegroep**, en **SKU van het certificaat**

> [!WARNING]
> App Service-certificaten kunnen alleen worden gebruikt door andere Services App binnen hetzelfde abonnement.  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Stap 3: het certificaat wordt opgeslagen in Azure Sleutelkluis

> [!NOTE]
> [Sleutelkluis](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is een Azure-service die helpt bij het beschermen van de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services.
>

Zodra de aankoop van het SSL-certificaat voltooid is, moet u openen [App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource-blade.

![afbeelding van gereed is om op te slaan in KV invoegen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

U ziet dat certificaatstatus **"In behandeling zijnde uitgifte"** omdat er meer stappen u uitvoeren moet voordat u met dit certificaat kunt gaan.

Klik op **Certificaatconfiguratie** in eigenschappen voor certificaat-blade en klik op **stap 1: opslaan** dit certificaat wordt opgeslagen in Azure Sleutelkluis.

Van **Sleutelkluis Status** Blade, klikt u op **Sleutelkluis opslagplaats** kiezen een bestaande Sleutelkluis voor het opslaan van dit certificaat **of maak nieuwe Sleutelkluis** voor het maken van nieuwe Sleutelkluis binnen hetzelfde abonnement en de resource-groep.

> [!NOTE]
> Azure Sleutelkluis heeft minimale kosten voor het opslaan van dit certificaat.
> Zie voor meer informatie  **[prijsinformatie voor Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Nadat u de opslagplaats van de kluis sleutel voor het opslaan van dit certificaat, hebt geselecteerd de **opslaan** optie geslaagd moet worden weergegeven.

![afbeelding van store slagen van KV invoegen](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Stap 4: Controleer of het eigendom van het domein

> [!NOTE]
> Er zijn drie soorten verificatie van het domein wordt ondersteund door App service Certificate: domein, E-mail, handmatige verificatie. Deze worden beschreven in meer informatie in de [geavanceerde sectie](#advanced).

Uit dezelfde **Certificaatconfiguratie** blade die u in stap 3 gebruikt, klikt u op **stap 2: Controleer of**.

**Verificatie van domein** dit is het meest geschikte proces **alleen als** u  **[uw aangepaste domein via Azure App Service hebt aangeschaft.](custom-dns-web-site-buydomains-web-app.md)**
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

[!code-azurecli[belangrijkste](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "een aangepaste SSL-certificaat binden aan een web-app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[belangrijkste](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "een aangepaste SSL-certificaat binden aan een web-app")]

## <a name="advanced"></a>Geavanceerd

### <a name="verifying-domain-ownership"></a>Domein eigendom verifiëren

Er zijn twee soorten verificatie van het domein wordt ondersteund door App service Certificate: e-Mail en handmatige verificatie.

#### <a name="mail-verification"></a>Controle e-mail

Reeds is bevestigingsmail verzonden naar de e-adressen die zijn gekoppeld aan dit aangepast domein.
De e-verificatiestap voltooid, opent u het e-mailbericht en klik op de koppeling voor verificatie.

![afbeelding van e-mailverificatie invoegen](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Als u de verificatie-e-mail verzenden moet, klikt u op de **E-mail opnieuw verzenden** knop.

#### <a name="manual-verification"></a>Handmatige verificatie

> [!IMPORTANT]
> HTML-pagina verificatie (werkt alleen met standaard SKU van het certificaat.)
>

1. Maken van een HTML-bestand met de naam **'starfield.html'**

1. Inhoud van dit bestand moet de exacte naam van het domein verificatie-Token. (U kunt het token van de Blade domein verificatie Status kopiëren)

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

Houd er rekening mee dat dit IP-adres afwijkt van het virtuele IP-adres is eerder gebruikt voor het configureren van de A-record voor uw domein. Als u zijn geconfigureerd voor gebruik SNI op basis van SSL, of zijn niet geconfigureerd om SSL te gebruiken, geen adres wordt vermeld voor dit item.

Met de hulpprogramma's door uw domeinnaamregistrar, wijzig de A-record voor uw aangepaste domeinnaam om te verwijzen naar het IP-adres van de vorige stap.

## <a name="rekey-and-sync-the-certificate"></a>Sleutel opnieuw genereren en synchroniseren van het certificaat

Als u ooit sleutel opnieuw genereren van uw certificaat wilt, selecteert u **sleutel opnieuw genereren en synchroniseren** optie van **certificaateigenschappen** Blade.

Klik op **sleutel opnieuw genereren** om het proces te starten. Dit kan 1 tot 10 minuten duren.

![afbeelding van de sleutel opnieuw genereren SSL invoegen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Het certificaat opnieuw genereren van sleutels, wordt het certificaat met een nieuw certificaat is uitgegeven door de certificeringsinstantie.

## <a name="next-steps"></a>Volgende stappen

* [Een Content Delivery Network toevoegen](app-service-web-tutorial-content-delivery-network.md)