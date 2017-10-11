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
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Configureren van een aangepaste domeinnaam voor een web-app in Azure App Service met behulp van Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dit artikel bevat algemene instructies voor het gebruik van een aangepaste domeinnaam met Azure App Service die Traffic Manager voor taakverdeling gebruiken.

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

Uw aangepaste domein koppelt u een web-app in Azure App Service, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van hulpprogramma's van de domeinregistrar die u hebt aangeschaft met de domeinnaam van uw uit. Gebruik de volgende stappen uit om te zoeken en de DNS-hulpprogramma's gebruiken.

1. Aanmelden bij uw account bij uw domeinregistrar en zoekt u naar een pagina voor het beheren van DNS-records. Zoek naar koppelingen of gebieden van de site met het label **domeinnaam**, **DNS**, of **naam Serverbeheer**. Vaak een koppeling naar deze pagina vindt u gegevens over uw account weer te geven en te zoeken naar een koppeling zoals **mijn domeinen**.
2. Wanneer u de beheerpagina hebt gevonden voor de domeinnaam, zoekt u een koppeling waarmee u de DNS-records bewerken. Dit worden vermeld als een **zonebestand**, **DNS-Records**, of als een **Geavanceerd** configuratiekoppeling.
   
   * De pagina heeft hoogstwaarschijnlijk een aantal records dat al is gemaakt, zoals het koppelen van een vermelding '**@**'of'\*' met een pagina 'domein parkeerplaatsen'. Records voor algemene subdomeinen kunnen ook zoals bevatten **www**.
   * De pagina wordt vermeld **CNAME-records**, of geef een vervolgkeuzelijst om te selecteren van een recordtype. Het kan ook andere records, zoals vermeld **A-records** en **MX-records**. In sommige gevallen CNAME-records wordt aangeroepen door andere namen, zoals een **Alias-Record**.
   * De pagina heeft de velden waarmee u kunt ook **kaart** van een **hostnaam** of **domeinnaam** naar een andere domeinnaam.
3. Terwijl de details van elke registrar variëren, in het algemeen u toewijzen *van* uw aangepaste domeinnaam (zoals **contoso.com**,) *naar* de naam van het Traffic Manager-domein (**contoso.trafficmanager.net**) die voor uw web-app wordt gebruikt.
   
   > [!NOTE]
   > Als een record al in gebruik is en u moet uw apps optie preventief binden aan, kunt u ook een extra CNAME-record maken. Bijvoorbeeld, optie preventief binden **www.contoso.com** aan uw web-app maakt u een CNAME-record van **awverify.www** naar **contoso.trafficmanager.net**. U kunt vervolgens 'www.contoso.com' toevoegen aan uw Web-App zonder te wijzigen van de 'www' CNAME-record. Zie voor meer informatie [maken DNS-records voor een web-app in een aangepast domein][CREATEDNS].
   > 
   > 
4. Zodra u klaar bent met het toevoegen of wijzigen van DNS-records bij uw registrar, moet u de wijzigingen opslaan.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Traffic Manager inschakelen
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Volgende stappen
Zie het [Node.js Developer Center](/develop/nodejs/) voor meer informatie.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
