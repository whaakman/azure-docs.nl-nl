---
title: Overzicht van poorten voor hoge beschikbaarheid in Azure | Microsoft Docs
description: Meer informatie over hoge beschikbaarheid poorten taakverdeling op een interne load balancer
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: e72fc0d4323f7a2d203fee66311c3fea10ad7a09
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="high-availability-ports-overview-preview"></a>Overzicht van poorten voor hoge beschikbaarheid (Preview)

Azure Load Balancer standaard introduceert een nieuwe mogelijkheid saldo TCP en UDP-overdrachten op alle poorten tegelijkertijd laden bij gebruik van een interne Load Balancer. 

>[!NOTE]
> Hoge beschikbaarheid poorten functie is beschikbaar met Load Balancer Standard en zijn momenteel in preview. Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. U hoeft aan te melden voor de Load Balancer standaard Preview HA-poorten te gebruiken met Load Balancer standaard resources. Volg de instructies voor aanmelding naast de Load Balancer [standaard Preview](https://aka.ms/lbpreview#preview-sign-up) ook.

Een regel voor HA poorten is een variant van een regel op een interne Load Balancer standaard geconfigureerd voor taakverdeling.  Scenario's zijn vereenvoudigd door één Load Balancer-regel om taken te verdelen alle TCP en UDP-stromen die op alle poorten van een interne Load Balancer standaard frontend aankomen. De load balancing wordt besloten per overdracht op basis van de 5-tuple IP-bronadres bronpoort, IP-doeladres, doelpoort en-Protocol.

HA poorten kunnen essentiële scenario's zoals hoge beschikbaarheid en schaal voor netwerk virtuele apparaten (NVA) binnen de virtuele netwerken, alsmede andere scenario's waarbij een groot aantal poorten moet zijn taakverdeling. 

HA wordt poorten geconfigureerd door het instellen van de front-end- en back-end-poorten op **0** en protocol **alle**.  De interne Load Balancer-resource servertaken nu alle TCP en UDP-stromen ongeacht poortnummer.

## <a name="why-use-ha-ports"></a>Waarom HA-poorten te gebruiken

### <a name="nva"></a>Virtuele netwerkapparaten

U kunt virtuele netwerkapparaten (NVA) gebruiken voor het beveiligen van uw Azure-workload uit meerdere typen beveiligingsrisico's. Wanneer NVA worden gebruikt in deze scenario's, moeten ze betrouwbaar, maximaal beschikbare en scale-out voor de aanvraag zijn.

U kunt deze doelstellingen kunt bereiken in uw scenario door gewoon NVA exemplaren toe te voegen aan de back-endpool van de Azure interne Load Balancer en het configureren van een HA-poorten Load Balancer-regel.

HA bieden poorten verschillende voordelen voor NVA HA-scenario's:
- snel een failover uitvoeren naar orde exemplaren met per exemplaar statuscontroles
- betere prestaties, met de scale-out op n-actieve exemplaren
- n-actief en actief / passief scenario 's
- hoeft u de complexe oplossingen zoals Zookeeper-knooppunten voor de bewaking van apparaten

Het volgende voorbeeld geeft een virtueel netwerk hub en spoke-implementatie met de spaken force tunneling hun verkeer van het virtuele netwerk hub en via de NVA vóór het verlaten van de vertrouwde ruimte. De NVAs zich achter een interne Load Balancer Standard met configuratie van de HA-poorten.  Al het verkeer kunnen worden verwerkt en dienovereenkomstig door te sturen. 

![ha poorten voorbeeld](./media/load-balancer-ha-ports-overview/nvaha.png)

Afbeelding 1 - Hub en spoke-virtueel netwerk met NVAs geïmplementeerd in de HA-modus

Als u virtuele netwerkapparaten gebruikt, Controleer of de met de respectieve provider voor optimaal gebruik HA poorten en welke scenario's worden ondersteund.

### <a name="load-balancing-large-numbers-of-ports"></a>Taakverdeling van een groot aantal poorten

U kunt ook HA poorten gebruiken voor toepassingsscenario's waarvoor balanicng laden van grote aantallen poorten vereist. Deze scenario's kunnen worden vereenvoudigd met een interne [Load Balancer standaard](https://aka.ms/lbpreview) met HA poorten waar één taakverdelingsregel meerdere afzonderlijke taakverdelingsregels, één voor elke poort vervangt.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

HA poorten is beschikbaar in de [dezelfde regio's als Load Balancer standaard](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Preview-registratie

Als u wilt deelnemen aan de evaluatieversie van de functie van de HA-poorten in Load Balancer standaard Registreer uw abonnement om met Azure CLI 2.0 of PowerShell toegang te krijgen.  Volg deze drie stappen:

>[!NOTE]
>Deze functie wilt gebruiken, moet u ook aanmelden voor de Load Balancer [standaard Preview](https://aka.ms/lbpreview#preview-sign-up) behalve HA poorten. Registratie van de voorbeelden HA poorten of Load Balancer standaard kan een uur duren.

### <a name="sign-up-using-azure-cli-20"></a>Aanmelden met Azure CLI 2.0

1. De functie met de provider registreren
    ```cli
    az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
2. De vorige bewerking kan maximaal 10 minuten duren.  U kunt de status van de bewerking met de volgende opdracht controleren:

    ```cli
    az feature show --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
    Ga verder met stap 3 wanneer de status van het onderdeel registratie geregistreerde retourneert zoals hieronder wordt weergegeven:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowILBAllPortsRule",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Voltooi de aanmelding preview door uw abonnement met de resourceprovider opnieuw te registreren:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-using-powershell"></a>Aanmelden met behulp van PowerShell

1. De functie met de provider registreren
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    
2. De vorige bewerking kan maximaal 10 minuten duren.  U kunt de status van de bewerking met de volgende opdracht controleren:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    Ga verder met stap 3 wanneer de status van het onderdeel registratie geregistreerde retourneert zoals hieronder wordt weergegeven:
   
    ```
    FeatureName          ProviderName      RegistrationState
    -----------          ------------      -----------------
    AllowILBAllPortsRule Microsoft.Network Registered
    ```
    
3. Voltooi de aanmelding preview door uw abonnement met de resourceprovider opnieuw te registreren:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```


## <a name="limitations"></a>Beperkingen

Hieronder volgen de ondersteunde configuraties of uitzonderingen voor HA poorten:

- Een enkele frontend-IP-configuratie kan een enkele DSR Load Balancer-regel met HA poorten, of deze een enkele niet DSR load balancer-regel met HA poorten kan hebben. Er kan niet beide.
- Een enkele Network Interface-IP-configuratie kan slechts één niet-DSR load balancer-regel met HA poorten hebben. Er zijn geen andere regels kunnen worden geconfigureerd voor deze ipconfig.
- Een enkele Network Interface-IP-configuratie kan een of meer regels die DSR load balancer met HA-poorten, alle hun respectieve frontend-IP-configuraties uniek zijn.
- Als alle van de taakverdeling regels zijn HA-poorten (alleen DSR) of, alle regels zijn niet - HA poorten (DSR & niet DSR), worden twee (of meer) Load Balancer-regels die verwijst naar de dezelfde back-end-adresgroep kunnen naast elkaar bestaan. Twee dergelijke regels voor taakverdeling kunnen niet naast elkaar bestaan als er een combinatie van regels voor HA en niet - HA poorten.
- HA poorten is niet beschikbaar voor IPv6.
- Stroom symmetrie voor NVA scenario's wordt met één NIC alleen ondersteund. Zie de beschrijving en diagram voor [virtuele netwerkapparaten](#nva). 



## <a name="next-steps"></a>Volgende stappen

- [HA poorten op een interne Load Balancer Standard configureren](load-balancer-configure-ha-ports.md)
- [Meer informatie over de Load Balancer standaard preview](https://aka.ms/lbpreview)

