---
title: Een aangepaste domeinnaam voor een web-app in Azure App Service die gebruikmaakt van Traffic Manager voor taakverdeling configureren.
description: Gebruik een aangepaste domeinnaam voor een een web-app in Azure App Service met Traffic Manager voor taakverdeling.
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: c78fb7883559e46ebaa1d8dab59a15c55fb76fdf
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met behulp van Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dit artikel vindt u algemene instructies voor het gebruik van een aangepaste domeinnaam met een [App Service](app-service-web-overview.md) app die is geïntegreerd met [Traffic Manager](../traffic-manager/traffic-manager-overview.md) voor taakverdeling.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Informatie over DNS-records
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Configureren van uw web-apps voor standaardmodus
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Een DNS-record voor uw aangepaste domein toevoegen
> [!NOTE]
> Als u domein via Azure App Service Web Apps hebt aangeschaft en overslaan van de volgende stappen uit en naar de laatste stap van verwijzen [domein kopen voor Web-Apps](custom-dns-web-site-buydomains-web-app.md) artikel.
> 
> 

Uw aangepaste domein koppelt u een web-app in Azure App Service, moet u een nieuwe vermelding in de DNS-tabel toevoegen voor uw aangepaste domein. U doen dit met behulp van de beheerhulpprogramma's van uw domeinprovider.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Terwijl de details van elke domeinprovider variëren, wijst u *van* uw aangepaste domeinnaam (zoals **contoso.com**) *naar* de naam van het Traffic Manager-domein ( **Contoso.trafficmanager.NET**) die is geïntegreerd met uw web-app.
   
> [!NOTE]
> Als een record al in gebruik is en u moet uw apps optie preventief binden aan, kunt u een extra CNAME-record maken. Bijvoorbeeld, optie preventief binden **www.contoso.com** aan uw web-app maakt u een CNAME-record van **awverify.www** naar **contoso.trafficmanager.net**. U kunt vervolgens 'www.contoso.com' toevoegen aan uw Web-App zonder te wijzigen van de 'www' CNAME-record. Zie voor meer informatie [maken DNS-records voor een web-app in een aangepast domein][CREATEDNS].
> 
> 

Zodra u klaar bent met het toevoegen of wijzigen van DNS-records bij uw domeinprovider, moet u de wijzigingen opslaan.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Traffic Manager inschakelen
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Volgende stappen
Zie het [Node.js Developer Center](/develop/nodejs/) voor meer informatie.

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
