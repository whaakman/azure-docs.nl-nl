---
title: Een actieve DNS-naam migreren naar Azure App Service | Microsoft Docs
description: Informatie over het migreren van een aangepaste DNS-domeinnaam al aan een actieve site naar Azure App Service zonder uitvaltijd toegewezen is.
services: app-service
documentationcenter: 
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
ms.openlocfilehash: cd04be2046a23901471cb7bd0da9e0ed2d514d0d
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="migrate-an-active-dns-name-to-azure-app-service"></a>Een actieve DNS-naam migreren naar Azure App Service

Dit artikel laat zien hoe u migreert van een actieve DNS-naam voor [Azure App Service](../app-service/app-service-web-overview.md) zonder uitvaltijd.

Wanneer u een live site en de DNS-domeinnaam in App Service migreert, is al live verkeer voor de DNS-naam. U kunt uitvaltijd in DNS-omzetting tijdens de migratie vermijden door de naam van de actieve DNS-optie preventief binding aan uw App Service-app.

Als u niet over uitvaltijd in DNS-omzetting vastgesteld, Zie [aangepaste DNS-naam van een bestaande toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Vereisten

Voltooi deze instructies volgt:

- [Zorg ervoor dat uw app in App Service zich niet in de laag gratis](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-the-domain-name-preemptively"></a>De domeinnaam optie preventief binden

Wanneer u een aangepast domein optie preventief bindt, uitvoeren van de volgende voordat u wijzigingen aanbrengt in de DNS-records:

- Domeineigendom controleren
- De domeinnaam voor uw app inschakelen

Wanneer u ten slotte uw aangepaste DNS-naam van de oude site naar de App Service-app migreert, zal er geen uitvaltijd in DNS-omzetting.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Domein verificatie-record maken

Als u wilt verifiëren dat dit domein, een TXT-record toevoegen De TXT-record wordt toegewezen uit _awverify.&lt; subdomein >_ naar  _&lt;appname >. azurewebsites.net_. 

De TXT-record dat u nodig hebt, is afhankelijk van de DNS-record dat u wilt migreren. Zie de volgende tabel voor voorbeelden (`@` vertegenwoordigt doorgaans het hoofddomein):

| Voorbeeld van DNS-record | TXT-Host | TXT-waarde |
| - | - | - |
| @ (root) | _awverify_ | _&lt;AppName >. azurewebsites.net_ |
| www (sub) | _awverify.www_ | _&lt;AppName >. azurewebsites.net_ |
| \*(jokertekens) | _awverify.\*_ | _&lt;AppName >. azurewebsites.net_ |

Noteer het recordtype van de DNS-naam die u wilt migreren in uw pagina DNS-records. App Service biedt ondersteuning voor toewijzingen van CNAME- en A-records.

### <a name="enable-the-domain-for-your-app"></a>Het domein voor uw app inschakelen

In de [Azure-portal](https://portal.azure.com), selecteert u in het linkernavigatievenster van de app-pagina **aangepaste domeinen**. 

![Aangepast domein menu](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

In de **aangepaste domeinen** pagina de  **+**  pictogram naast **hostnaam toevoegen**.

![Hostnaam toevoegen](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ de volledig gekwalificeerde domeinnaam dat u de TXT-record, zoals toegevoegd `www.contoso.com`. Voor een jokertekendomein (zoals \*. contoso.com), kunt u een DNS-naam die overeenkomt met het jokertekendomein. 

Selecteer **valideren**.

De **hostnaam toevoegen** knop wordt geactiveerd. 

Zorg ervoor dat **hostnaam recordtype** is ingesteld op het DNS-recordtype dat u wilt migreren.

Selecteer **hostnaam toevoegen**.

![DNS-naam toevoegen aan de app.](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Het kan even duren voor de nieuwe hostnaam worden weergegeven in de app **aangepaste domeinen** pagina. Vernieuw de browser voor het bijwerken van de gegevens.

![CNAME-record is toegevoegd](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Uw aangepaste DNS-naam is nu ingeschakeld in uw app in Azure. 

## <a name="remap-the-active-dns-name"></a>Opnieuw toewijzen van de actieve DNS-naam

De enige nog te doen is opnieuw toewijzen van uw actieve DNS-record om te verwijzen naar App Service. Rechts nu nog steeds verwijst naar de oude site.

<a name="info"></a>

### <a name="copy-the-apps-ip-address-a-record-only"></a>Kopiëren van de app IP-adres (alleen een record)

Als u opnieuw van een CNAME-record toewijzen, moet u deze sectie overslaan. 

Opnieuw toewijzen van een A-record, moet u de App Service-app externe IP-adres, die wordt weergegeven in de **aangepaste domeinen** pagina.

Sluit de **hostnaam toevoegen** pagina door te selecteren **X** in de rechterbovenhoek. 

In de **aangepaste domeinen** pagina, kopieert u de app IP-adres.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-the-dns-record"></a>De DNS-record bijwerken

Selecteer op de pagina DNS-records van uw domeinprovider de DNS-record opnieuw toewijzen.

Voor de `contoso.com` voorbeeld van domein hoofdmap, wijst u de A of CNAME-record zoals de voorbeelden in de volgende tabel: 

| FQDN-voorbeeld | Recordtype | Host | Waarde |
| - | - | - | - |
| Contoso.com (root) | A | `@` | IP-adres uit [kopiëren van de app IP-adres](#info) |
| www.contoso.com (sub) | CNAME | `www` | _&lt;AppName >. azurewebsites.net_ |
| \*. contoso.com (jokertekens) | CNAME | _\*_ | _&lt;AppName >. azurewebsites.net_ |

Uw instellingen opslaan.

DNS-query's moeten beginnen met het omzetten van naar uw app in App Service onmiddellijk nadat de DNS-servers gebeurt.

## <a name="next-steps"></a>Volgende stappen

Ontdek hoe u een aangepaste SSL-certificaat binden aan de App Service.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste SSL-certificaat binden aan Azure-Web-Apps](app-service-web-tutorial-custom-ssl.md)
