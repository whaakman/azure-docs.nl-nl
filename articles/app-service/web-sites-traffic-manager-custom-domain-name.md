---
title: Een aangepaste domeinnaam voor een web-app in Azure App Service die gebruikmaakt van Traffic Manager voor de taakverdeling configureren.
description: Gebruik een aangepaste domeinnaam voor een een web-app in Azure App Service met Traffic Manager voor de taakverdeling.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: a83b5dada8afff455375ae1606ecaac4c9ebe8e7
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42054453"
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Een aangepaste domeinnaam voor een web-app configureren in Azure App Service met Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

In dit artikel vindt u algemene instructies voor het gebruik van een aangepaste domeinnaam met een [App Service](app-service-web-overview.md) app die is geïntegreerd met [Traffic Manager](../traffic-manager/traffic-manager-overview.md) voor de taakverdeling.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Informatie over DNS-records
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Configureren van uw web-apps voor de standaardmodus
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Een DNS-record voor uw aangepaste domein toevoegen
> [!NOTE]
> Als u hebt aangeschaft domein door middel van Azure App Service Web Apps en vervolgens overslaat stappen te volgen en naar de laatste stap van verwijzen [domein voor Web-Apps kopen](custom-dns-web-site-buydomains-web-app.md) artikel.
> 
> 

Als u wilt uw aangepaste domein koppelen aan een web-app in Azure App Service, moet u een nieuwe vermelding in de tabel DNS toevoegen voor uw aangepaste domein. U doen dit met behulp van de beheerhulpprogramma's van uw domeinprovider.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Terwijl de details van elke domeinprovider variëren, wijst u *van* uw aangepaste domeinnaam (zoals **contoso.com**) *naar* de naam van het Traffic Manager-domein ( **Contoso.trafficmanager.NET**) die is geïntegreerd met uw web-app.
   
> [!NOTE]
> Als een record al gebruikt wordt en u moet uw apps preventief te binden aan, kunt u een extra CNAME-record maken. Bijvoorbeeld, preventief te binden **www.contoso.com** voor uw web-app, het maken van een CNAME-record van **awverify.www** naar **contoso.trafficmanager.net**. U kunt vervolgens 'www.contoso.com' toevoegen aan uw Web-App zonder dat u wijzigt de CNAME-record 'www'. Zie voor meer informatie, [maken DNS-records voor een web-app in een aangepast domein][CREATEDNS].
> 
> 

Zodra u klaar bent met het toevoegen of wijzigen van de DNS-records bij uw domeinprovider, moet u de wijzigingen opslaan.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Traffic Manager inschakelen
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Volgende stappen
Zie het [Node.js Developer Center](http://azure.microsoft.com/develop/nodejs/) voor meer informatie.

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
