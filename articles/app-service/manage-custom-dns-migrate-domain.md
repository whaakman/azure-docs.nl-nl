---
title: Migreren van een actieve DNS-naam - Azure App Service | Microsoft Docs
description: Informatie over het migreren van een aangepaste DNS-domeinnaam al aan een live site naar Azure App Service zonder uitvaltijd toegewezen is.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: e7449b5c36d9a1c3df3692f80aed8ccc05a98ade
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731142"
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Een actieve DNS-naam migreren naar Azure App Service

Dit artikel leest u hoe u migreert van een actieve DNS-naam voor [Azure App Service](../app-service/overview.md) zonder uitvaltijd.

Wanneer u een live site en de DNS-domeinnaam naar App Service migreert, dient die DNS-naam al live-verkeer. Tijdens de migratie kunt u downtime in DNS-omzetting voorkomen door de actieve DNS-naam te binden aan uw App Service-app.

Als u niet bang zijn niet beschikbaar is wegens in DNS-omzetting bent, Zie [een bestaande aangepaste DNS-naam toewijzen in Azure App Service](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze instructies:

- [Zorg ervoor dat uw App Service-app zich niet in de laag gratis](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-the-domain-name-preemptively"></a>Naam van het domein te koppelen

Wanneer u een aangepast domein te koppelen, uitvoeren van de volgende voordat u wijzigingen aanbrengt in uw DNS-records:

- Domeineigendom controleren
- De domeinnaam voor uw app inschakelen

Wanneer u ten slotte uw aangepaste DNS-naam van de oude site naar de App Service-app migreert, worden er geen uitvaltijd in DNS-omzetting.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Domein-verificatie-record maken

Als u wilt verifiëren dat dit domein, een TXT-record toevoegen De TXT-record wordt toegewezen vanuit _awverify.&lt; subdomein >_ naar  _&lt;appname >. azurewebsites.net_. 

De TXT-record die u nodig hebt, is afhankelijk van de DNS-record die u wilt migreren. Zie voor voorbeelden van de volgende tabel (`@` vertegenwoordigt doorgaans het hoofddomein):

| Voorbeeld van de DNS-record | TXT-Host | TXT-waarde |
| - | - | - |
| \@ (root) | _awverify_ | _&lt;AppName >. azurewebsites.net_ |
| www (sub) | _awverify.www_ | _&lt;AppName >. azurewebsites.net_ |
| \* (jokertekens) | _awverify.\*_ | _&lt;AppName >. azurewebsites.net_ |

Houd er rekening mee het recordtype van de DNS-naam die u wilt migreren in de pagina van uw DNS-records. App Service biedt ondersteuning voor toewijzingen van CNAME- en A-records.

> [!NOTE]
> Voor bepaalde providers, zoals CloudFlare, `awverify.*` is geen geldige record. Gebruik `*` alleen in plaats daarvan.

> [!NOTE]
> Jokertekens `*` records subdomeinen met een bestaande CNAME-record niet gevalideerd. Mogelijk moet u expliciet een TXT-record voor elk subdomein te maken.


### <a name="enable-the-domain-for-your-app"></a>Het domein voor uw app inschakelen

In de [Azure-portal](https://portal.azure.com), in het linkernavigatievenster van de app-pagina, selecteer **aangepaste domeinen**. 

![Menu voor aangepaste domeinen](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

In de **aangepaste domeinen** weergeeft, schakelt de **+** pictogram naast **hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ de volledig gekwalificeerde domeinnaam dat u de TXT-record, zoals toegevoegd `www.contoso.com`. Voor een wildcard-domein (zoals \*. contoso.com), kunt u een DNS-naam die overeenkomt met het wildcard-domein. 

Selecteer **Valideren**.

De knop **Hostnaam toevoegen** wordt geactiveerd. 

Zorg ervoor dat **hostnaam recordtype** is ingesteld op de DNS-recordtype dat u wilt migreren.

Selecteer **Hostnaam toevoegen**.

![DNS-naam toevoegen aan de app](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Het kan even duren voor de nieuwe hostnaam wordt weergegeven op de pagina **Aangepaste domeinen** van de app. Vernieuw de browser voor om de gegevens bij te werken.

![CNAME-record toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Uw aangepaste DNS-naam is nu ingeschakeld in uw Azure-app. 

## <a name="remap-the-active-dns-name"></a>Opnieuw toewijzen van de actieve DNS-naam

De enige nog te doen is opnieuw toewijzen van uw actieve DNS-record om te verwijzen naar App Service. Rechts nu nog steeds verwijst naar de oude site.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Kopieert u het IP-adres van de app (alleen een record)

Als u opnieuw van een CNAME-record toewijzen, moet u deze sectie overslaan. 

Als u wilt een A-record toewijzen, moet u de App Service-app externe IP-adres, dat wordt weergegeven in de **aangepaste domeinen** pagina.

Sluit de **hostnaam toevoegen** pagina door te selecteren **X** in de rechterbovenhoek. 

Op de pagina **Aangepaste domeinen** kopieert u het IP-adres van de app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>De DNS-record bijwerken

Selecteer de DNS-record opnieuw toewijzen terug in de pagina DNS-records van uw domeinprovider.

Voor de `contoso.com` domeinvoorbeeld hoofdmap, opnieuw toewijzen van de A of CNAME-record, zoals de voorbeelden in de volgende tabel: 

| FQDN-voorbeeld | Recordtype | Host | Waarde |
| - | - | - | - |
| Contoso.com (root) | A | `@` | IP-adres uit [Het IP-adres van de app kopiëren](#info) |
| www.contoso.com (sub) | CNAME | `www` | _&lt;AppName >. azurewebsites.net_ |
| \*. contoso.com (jokertekens) | CNAME | _\*_ | _&lt;AppName >. azurewebsites.net_ |

Uw instellingen opslaan.

DNS-query's moeten beginnen omgezet naar uw app in App Service onmiddellijk nadat de DNS-doorgifte plaatsvindt.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u een aangepast SSL-certificaat binden aan App Service.

> [!div class="nextstepaction"]
> [Een bestaand aangepast SSL-certificaat binden aan Azure App Service](app-service-web-tutorial-custom-ssl.md)
