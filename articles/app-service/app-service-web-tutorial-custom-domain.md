---
title: Een bestaande aangepaste DNS-naam toegewezen aan Azure Web Apps | Microsoft Docs
description: Informatie over het toevoegen van een bestaande aangepaste DNS-domeinnaam (vanity domein) aan een web-app, back-end voor mobiele app of API-app in Azure App Service.
keywords: App service, azure app service, domein toewijzing, domeinnaam, bestaand domein, hostnaam
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 9b35572b3275b5a2c5e89adf4890a2659d09626e
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Een bestaande aangepaste DNS-naam toegewezen aan Azure-Web-Apps

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze zelfstudie laat zien hoe u een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een subdomein toewijzen (bijvoorbeeld `www.contoso.com`) met behulp van een CNAME-record
> * Toewijzen van een hoofddomein (bijvoorbeeld `contoso.com`) met behulp van een A-record
> * Toewijzen van een jokertekendomein (bijvoorbeeld `*.contoso.com`) met behulp van een CNAME-record
> * Domein-toewijzing met scripts automatiseren

U kunt ofwel een **CNAME-record** of een **een record** toewijzen van een aangepaste DNS-naam in App Service. 

> [!NOTE]
> Het is raadzaam dat u een CNAME voor alle aangepaste DNS-namen, behalve een hoofddomein (bijvoorbeeld `contoso.com`).

Zie voor het migreren van een live site en de DNS-domeinnaam in App Service, [een actieve DNS-naam migreren naar Azure App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Een App Service-app maken](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.
* Een domeinnaam kopen en controleer of dat u toegang hebt tot het register DNS voor uw domeinprovider (zoals GoDaddy).

  Bijvoorbeeld, om toe te voegen DNS-vermeldingen voor `contoso.com` en `www.contoso.com`, moet u kunnen de DNS-instellingen configureren voor de `contoso.com` hoofddomein.

  > [!NOTE]
  > Als u een bestaand domein, Geef een naam, kunt u geen [aanschaffen van een domein met de Azure portal](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>De app voorbereiden

Een aangepaste DNS-naam toewijzen aan een web-app, de web-app van [App Service-abonnement](https://azure.microsoft.com/pricing/details/app-service/) moet een betaald laag (**gedeelde**, **Basic**, **standaard**, of  **Premium**). In deze stap maakt ervoor u zorgen dat de App Service-app is in de ondersteunde prijscategorie.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigeer naar de app in de Azure portal

Selecteer in het menu links **App Services**, en selecteer vervolgens de naam van de app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/select-app.png)

U ziet de beheerpagina van de App Service-app.  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Schuif in de navigatiebalk links van de app-pagina naar de **instellingen** sectie en selecteer **opschalen (App Service-abonnement)**.

![Omhoog schalen-menu](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

De huidige tier van de app wordt gemarkeerd door een rand. Controleer of de app is niet in de **vrije** laag. Aangepaste DNS wordt niet ondersteund in de **vrije** laag. 

![Controleer de prijscategorie](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Als de App Service-abonnement niet **vrije**, sluit de **Kies uw prijscategorie** pagina en doorgaan met [toewijzen van een CNAME-record](#cname).

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a>De App Service-abonnement opschalen

Selecteer een van de lagen niet vrij (**gedeelde**, **Basic**, **standaard**, of **Premium**). 

Klik op **Selecteren**.

![Controleer de prijscategorie](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking is voltooid.

![Schaal bewerking bevestigen](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Toewijzen van een CNAME-record

In de zelfstudie voorbeeld voegt u toe een CNAME-record voor de `www` subdomein (bijvoorbeeld `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>De CNAME-record maken

Voeg een CNAME-record voor een subdomein toewijzen aan de app standaard hostnaam (`<app_name>.azurewebsites.net`, waarbij `<app_name>` is de naam van uw app).

Voor de `www.contoso.com` domein voorbeeld, Voeg een CNAME-record dat is toegewezen, de naam van de `www` naar `<app_name>.azurewebsites.net`.

Nadat u de CNAME toevoegt, wordt de pagina DNS-records lijkt op het volgende voorbeeld:

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>De toewijzing van de CNAME-record in Azure inschakelen

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **aangepaste domeinen**. 

![Aangepast domein menu](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

In de **aangepaste domeinen** pagina van de app toevoegen de volledig gekwalificeerde aangepaste DNS-naam (`www.contoso.com`) aan de lijst.

Selecteer de  **+**  pictogram naast **hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ de volledig gekwalificeerde domeinnaam dat u een CNAME-record, zoals toegevoegd `www.contoso.com`. 

Selecteer **valideren**.

De **hostnaam toevoegen** knop wordt geactiveerd. 

Zorg ervoor dat **hostnaam recordtype** is ingesteld op **CNAME (www.example.com of elk subdomein)**.

Selecteer **hostnaam toevoegen**.

![DNS-naam toevoegen aan de app.](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Het kan even duren voor de nieuwe hostnaam worden weergegeven in de app **aangepaste domeinen** pagina. Vernieuw de browser voor het bijwerken van de gegevens.

![CNAME-record is toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Als u een stap gemist of hebt u een typefout gemaakt ergens eerder, ziet u een fout bij verificatie van aan de onderkant van de pagina.

![Fout bij verificatie](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Toewijzen van een A-record

In de zelfstudie voorbeeld voegt u toe een A-record voor het hoofddomein (bijvoorbeeld `contoso.com`). 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Kopiëren van de app IP-adres

Als u wilt een A-record toewijzen, moet u het externe IP-adres van de app. U vindt dit IP-adres in van de app **aangepaste domeinen** pagina in de Azure-portal.

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **aangepaste domeinen**. 

![Aangepast domein menu](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

In de **aangepaste domeinen** pagina, kopieert u de app IP-adres.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a>De A-record maken

Als u wilt een A-record toewijzen aan een app, App-Service vereist **twee** DNS-records:

- Een **A** record toewijzen aan IP-adres van de app.
- Een **TXT** record toewijzen aan de app standaard hostnaam `<app_name>.azurewebsites.net`. App Service gebruikt deze record alleen tijdens de configuratie, om te controleren of de eigenaar van het aangepaste domein. Nadat u uw aangepaste domein is gevalideerd en geconfigureerd in App Service, kunt u deze TXT-record verwijderen. 

Voor de `contoso.com` domein bijvoorbeeld de A- en TXT-records overeenkomstig de volgende tabel maken (`@` vertegenwoordigt doorgaans het hoofddomein). 

| Recordtype | Host | Waarde |
| - | - | - |
| A | `@` | IP-adres uit [kopiëren van de app IP-adres](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Wanneer de records worden toegevoegd, wordt de pagina DNS-records lijkt op het volgende voorbeeld:

![Pagina DNS-records](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>De toewijzing van een record in de app inschakelen

Terug in de app **aangepaste domeinen** pagina in de Azure portal, het toevoegen van de volledig gekwalificeerde aangepaste DNS-naam (bijvoorbeeld `contoso.com`) aan de lijst.

Selecteer de  **+**  pictogram naast **hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Typ de volledig gekwalificeerde domeinnaam dat u de A-record, zoals geconfigureerd `contoso.com`.

Selecteer **valideren**.

De **hostnaam toevoegen** knop wordt geactiveerd. 

Zorg ervoor dat **hostnaam recordtype** is ingesteld op **een record (example.com)**.

Selecteer **hostnaam toevoegen**.

![DNS-naam toevoegen aan de app.](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Het kan even duren voor de nieuwe hostnaam worden weergegeven in de app **aangepaste domeinen** pagina. Vernieuw de browser voor het bijwerken van de gegevens.

![Een record die is toegevoegd](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Als u een stap gemist of hebt u een typefout gemaakt ergens eerder, ziet u een fout bij verificatie van aan de onderkant van de pagina.

![Fout bij verificatie](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Een jokertekendomein toewijzen

In de zelfstudie voorbeeld, wijst u een [jokertekens DNS-naam](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (bijvoorbeeld `*.contoso.com`) naar de App Service-app door een CNAME-record toe te voegen. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>De CNAME-record maken

Voeg een CNAME-record de jokertekennaam van een om toe te wijzen hostnaam van de app-standaard (`<app_name>.azurewebsites.net`).

Voor de `*.contoso.com` domein bijvoorbeeld de CNAME-record wordt de naam toewijzen `*` naar `<app_name>.azurewebsites.net`.

Wanneer de CNAME wordt toegevoegd, ziet de pagina DNS-records in het volgende voorbeeld:

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>De toewijzing van de CNAME-record in de app inschakelen

U kunt nu elk subdomein dat overeenkomt met de jokertekennaam naar de app toevoegen (bijvoorbeeld `sub1.contoso.com` en `sub2.contoso.com` overeen met `*.contoso.com`). 

Selecteer in het linkernavigatievenster van de app-pagina in de Azure portal **aangepaste domeinen**. 

![Aangepast domein menu](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Selecteer de  **+**  pictogram naast **hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ een FQDN-naam die overeenkomt met het jokertekendomein (bijvoorbeeld `sub1.contoso.com`), en selecteer vervolgens **valideren**.

De **hostnaam toevoegen** knop wordt geactiveerd. 

Zorg ervoor dat **hostnaam recordtype** is ingesteld op **CNAME-record (www.example.com of elk subdomein)**.

Selecteer **hostnaam toevoegen**.

![DNS-naam toevoegen aan de app.](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Het kan even duren voor de nieuwe hostnaam worden weergegeven in de app **aangepaste domeinen** pagina. Vernieuw de browser voor het bijwerken van de gegevens.

Selecteer de  **+**  pictogram opnieuw naar een andere hostnaam die overeenkomt met het jokertekendomein toevoegen. Bijvoorbeeld, Voeg `sub2.contoso.com`.

![CNAME-record is toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>In de browser testen

Blader naar de DNS-namen die u eerder hebt geconfigureerd (bijvoorbeeld `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, en `sub2.contoso.com`).

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="resolve-404-error-web-site-not-found"></a>404-fout 'Website niet gevonden' oplossen

Als er een HTTP 404 (niet gevonden)-fout bij het bladeren naar de URL van uw aangepaste domein, moet u controleren of uw domein wordt omgezet naar uw app IP-adres met <a href="https://www.whatsmydns.net/" target="_blank">WhatsmyDNS.net</a>. Als dat niet het geval is, dit wordt mogelijk veroorzaakt door een van de volgende redenen:

- Het aangepaste domein geconfigureerd ontbreekt een A-record en/of een CNAME-record.
- De browserclient heeft het oude IP-adres van uw domein in de cache. Wis de cache en test DNS-omzetting opnieuw. Op een Windows-machine, schakelt u in de cache `ipconfig /flushdns`.

<a name="virtualdir"></a>

## <a name="direct-default-url-to-a-custom-directory"></a>Standaard-URL naar een aangepaste map omleiden

Standaard stuurt App Service webaanvragen naar de hoofdmap van uw app-code. Echter starten bepaalde frameworks web niet in de hoofdmap. Bijvoorbeeld: [Laravel](https://laravel.com/) start in de `public` submap. Om door te gaan de `contoso.com` DNS-voorbeeld, zoals een app is toegankelijk op `http://contoso.com/public`, maar u zeker wilt sturen `http://contoso.com` naar de `public` directory in plaats daarvan. Deze stap niet hebben betrekking op DNS-omzetting, maar de virtuele map aanpassen.

Om dit te doen, selecteert u **toepassingsinstellingen** in de linkernavigatiebalk van uw web-app-pagina. 

Aan de onderkant van de pagina, de virtuele hoofdmap `/` verwijst naar `site\wwwroot` standaard is dit de hoofdmap van uw app-code. Wijzig deze verwijzen naar de `site\wwwroot\public` in plaats daarvan bijvoorbeeld en sla de wijzigingen. 

![Virtuele map aanpassen](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

Nadat de bewerking is voltooid, moet u de app worden de juiste pagina op het hoofdpad (bijvoorbeeld http://contoso.com) geretourneerd.

## <a name="automate-with-scripts"></a>Automatiseren met behulp van scripts

U kunt beheer van aangepaste domeinen met behulp van scripts, automatiseren met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure-CLI 

De volgende opdracht voegt een geconfigureerde aangepaste DNS-naam naar een App Service-app. 

```bash 
az webapp config hostname add \
    --webapp-name <app_name> \
    --resource-group <resource_group_name> \ 
    --hostname <fully_qualified_domain_name> 
``` 

Zie voor meer informatie [een aangepast domein toewijzen aan een web-app](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

De volgende opdracht voegt een geconfigureerde aangepaste DNS-naam naar een App Service-app. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Zie voor meer informatie [een aangepast domein toewijzen aan een web-app](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een subdomein toewijzen met behulp van een CNAME-record
> * Een hoofddomein toewijzen met behulp van een A-record
> * Een jokertekendomein toewijzen met behulp van een CNAME-record
> * Domein-toewijzing met scripts automatiseren

Ga naar de volgende zelfstudie voor meer informatie over hoe u een aangepaste SSL-certificaat binden aan een web-app.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste SSL-certificaat binden aan Azure-Web-Apps](app-service-web-tutorial-custom-ssl.md)
