---
title: Instellen van failover tussen meerdere Azure CDN-eindpunten met Azure Traffic Manager | Microsoft Docs
description: Meer informatie over het instellen van Azure Traffic Manager met Azure CDN-eindpunten.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: b52cad1f32cc3d16cf70bb81640dcb1d9f8614bf
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132940"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Instellen van failover tussen meerdere Azure CDN-eindpunten met Azure Traffic Manager

Wanneer u Azure Content Delivery Network (CDN) configureert, kunt u de optimale provider en de prijscategorie voor uw behoeften. Azure CDN maakt met de globaal gedistribueerde infrastructuur standaard lokale en geografische redundantie en globale taakverdeling ter verbetering van de servicebeschikbaarheid en prestaties. Als u een locatie is niet beschikbaar voor het leveren van inhoud, aanvragen automatisch worden doorgestuurd naar een andere locatie en de optimale pop-server (gebaseerd op factoren als werklast locatie en server) wordt gebruikt voor elke clientaanvraag. 
 
Als u meerdere CDN-profielen hebt, kunt u de beschikbaarheid en prestaties met Azure Traffic Manager verder verbeteren. U kunt Azure Traffic Manager met Azure CDN gebruiken om taken te verdelen over meerdere CDN-eindpunten voor failover, taakverdeling en andere geo load-scenario's. In een typische failover-scenario worden eerst alle aanvragen van clients omgeleid naar de primaire CDN-profiel. Als het profiel niet beschikbaar is, worden aanvragen vervolgens doorgegeven aan de secundaire CDN-profiel tot uw primaire CDN-profiel weer online is. Met behulp van Azure Traffic Manager op deze manier zorgt ervoor dat uw webtoepassing is altijd beschikbaar. 

Dit artikel vindt u richtlijnen en een voorbeeld van het instellen van de failover met **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen.

## <a name="set-up-azure-cdn"></a>Instellen van Azure CDN 
Twee of meer Azure CDN-profielen en eindpunten maken met verschillende providers.

1. Maak een **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profiel door de stappen in [Maak een nieuw CDN-profiel](cdn-create-new-endpoint.md#create-a-new-cdn-profile).
 
   ![CDN meerdere profielen](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. In elk van de nieuwe profielen door ten minste één eindpunt te maken door de stappen in [een nieuw CDN-eindpunt maken](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint).

## <a name="set-up-azure-traffic-manager"></a>Stel Azure Traffic Manager
Een Azure Traffic Manager-profiel maken en taakverdeling over uw CDN-eindpunten instellen. 

1. Een Azure Traffic Manager-profiel maken door de stappen in [een Traffic Manager-profiel maken](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#create-a-traffic-manager-profile-1). 

    Voor **routeringsmethode**, selecteer **prioriteit**.

2. Uw CDN-eindpunten in uw Traffic Manager-profiel toevoegen door de stappen in [toevoegen Traffic Manager-eindpunten](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints)

    Voor **Type**, selecteer **externe eindpunten**. Voor **prioriteit**, voer een getal.

    Maak bijvoorbeeld *cdndemo101akamai.azureedge.net* met een prioriteit van *1* en *cdndemo101verizon.azureedge.net* met een prioriteit van *2*.

   ![CDN-eindpunten voor traffic manager](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Aangepast domein op Azure CDN en Azure Traffic Manager instellen
Na het instellen van uw CDN en Traffic Manager-profielen, volg deze stappen voor het toevoegen van DNS-toewijzing en registreren van aangepast domein naar de CDN-eindpunten. Voor dit voorbeeld wordt de aangepaste domeinnaam is *cdndemo101.dustydogpetcare.online*.

1. Ga naar de website voor de domeinprovider van uw aangepaste domein, zoals GoDaddy, en twee DNS CNAME-vermeldingen te maken. 

    a. De eerste CNAME-vermelding voor uw aangepaste domein met het subdomein cdnverify worden toegewezen aan uw CDN-eindpunt. Dit item is een vereiste stap registreren van het aangepaste domein naar de CDN-eindpunt dat u aan Traffic Manager toegevoegd in stap 2.

      Bijvoorbeeld: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. De tweede CNAME-vermelding voor uw aangepaste domein, zonder het subdomein cdnverify worden toegewezen aan uw CDN-eindpunt. Dit item wordt het aangepaste domein toegewezen aan Traffic Manager. 

      Bijvoorbeeld: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Als uw domein momenteel live is en kan niet worden onderbroken, voert u deze stap laatst in. Controleren of de CDN-eindpunten en traffic manager domeinen live zijn voordat u uw aangepaste DNS-domein aan Traffic Manager bijwerken.
    >


2.  Selecteer de eerste CDN-eindpunt (Akamai) in uw Azure CDN-profiel. Selecteer **aangepaste domein toevoegen** en invoer *cdndemo101akamai.azureedge.net*. Controleer of het selectievakje voor het valideren van het aangepaste domein groen is. 

    Maakt gebruik van Azure CDN de *cdnverify* subdomein valideren van de DNS-toewijzing voor het voltooien van het registratieproces. Zie voor meer informatie [maakt u een DNS CNAME-record](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record). Deze stap kunt Azure CDN voor het herkennen van het aangepaste domein zodat deze op de aanvragen reageren kan.

3.  Terug naar de website voor de domeinprovider van uw aangepaste domein en bijwerken van de eerste DNS-toewijzing die u hebt gemaakt in, zodat het aangepaste domein is toegewezen aan uw tweede CDN-eindpunt.
                             
    Bijvoorbeeld: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. Selecteer het tweede CDN-eindpunt (Verizon) en herhaalt u stap 2 van uw Azure CDN-profiel. Selecteer **aangepaste domein toevoegen**, en invoer *cdndemo101akamai.azureedge.net*.
 
Nadat u deze stappen hebt voltooid, is uw multi-CDN-service met failover-mogelijkheden ingesteld met Azure Traffic Manager. Voor toegang tot de test kunt URL's van uw aangepaste domein. Uitschakelen van de primaire CDN-eindpunt waarmee de functionaliteit, en controleer of dat de aanvraag correct naar de secundaire CDN-eindpunt verplaatst is. 

## <a name="next-steps"></a>Volgende stappen
U kunt ook instellen andere methoden voor het doorsturen, zoals geografische voor de taakverdeling tussen verschillende CDN-eindpunten. Zie voor meer informatie [configureren van de geografische verkeersrouteringsmethode met Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).



