---
title: Aangepaste instellingen voor App Service-omgevingen
description: Aangepaste configuratie-instellingen voor App Service-omgevingen
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 687475fae0c90713c15e8abbb92b71059eae81c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Aangepaste configuratie-instellingen voor App Service-omgevingen
## <a name="overview"></a>Overzicht
Omdat App Service-omgevingen beperkt tot één klant zijn, zijn er bepaalde configuratie-instellingen die uitsluitend op App Service-omgevingen kunnen worden toegepast. In dit artikel worden de verschillende specifieke aanpassingen die beschikbaar voor App Service-omgevingen zijn.

Als u geen App Service-omgeving, Zie [het maken van een App-serviceomgeving](app-service-web-how-to-create-an-app-service-environment.md).

U kunt de App Service-omgeving aanpassingen opslaan met behulp van een matrix in de nieuwe **clusterSettings** kenmerk. Dit kenmerk is gevonden in de woordenlijst 'Eigenschappen' van de *hostingEnvironments* Azure Resource Manager-entiteit.

De volgende afgekort Resource Manager-sjabloon codefragment bevat de **clusterSettings** kenmerk:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

De **clusterSettings** kenmerk kan worden opgenomen in een Resource Manager-sjabloon voor het bijwerken van de App Service-omgeving.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Azure Resource Explorer gebruiken voor het bijwerken van een App-serviceomgeving
U kunt ook de App Service-omgeving bijwerken met behulp van [Azure Resource Explorer](https://resources.azure.com).  

1. In Resource Explorer, gaat u naar het knooppunt voor de App Service-omgeving (**abonnementen** > **resourceGroups** > **providers**  >  **Microsoft.Web** > **hostingEnvironments**). Klik vervolgens op de specifieke App Service-omgeving, die u wilt bijwerken.
2. Klik in het rechterdeelvenster **lezen/schrijven** in de bovenste werkbalk om toe te staan interactieve bewerken in Resource Explorer.  
3. Klik op de blauwe **bewerken** knop om te zorgen dat de Resource Manager-sjabloon worden bewerkt.
4. Ga naar de onderkant van het rechter deelvenster. De **clusterSettings** kenmerk helemaal onder, kunt u opgeven of werk de waarde is.
5. Typ (of kopieer en plak) de matrix van configuratiewaarden die u wilt dat in de **clusterSettings** kenmerk.  
6. Klik op de groene **plaatsen** knop die aan de bovenkant van het rechter deelvenster doorvoeren van de wijziging aan de App Service-omgeving heeft gevonden.

Maar u de wijziging dient, duurt het ongeveer 30 minuten vermenigvuldigd met het nummer van de front-ends in de App Service-omgeving om de wijziging van kracht te laten worden.
Bijvoorbeeld, als een App Service-omgeving vier front-ends heeft, duurt het ongeveer twee uur voor de configuratie-update te voltooien. Tijdens het wijzigen van de configuratie uitgerold, geen andere vergroten/verkleinen operations of configuratie wijzigen-bewerkingen kunnen worden uitgevoerd in App Service-omgeving.

## <a name="disable-tls-10"></a>TLS 1.0 uitschakelen
Een terugkerende vraag van klanten, vooral klanten die zijn betreft de PCI-naleving audits, is het TLS 1.0 expliciet uitschakelen voor hun apps.

TLS 1.0 kan worden uitgeschakeld via de volgende **clusterSettings** post:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Volgorde van wijziging TLS-coderingssuites
Een andere vraag van klanten is als ze de lijst met door de server onderhandeld coderingen wijzigen en dit kan worden gerealiseerd kunnen door het wijzigen van de **clusterSettings** zoals hieronder wordt weergegeven. De lijst met coderingssuites die beschikbaar kan worden opgehaald uit [deze MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Als onjuiste waarden worden ingesteld voor de coderingssuite die SChannel kan niet begrijpt, kan alle TLS-communicatie met de server mogelijk niet meer. In dat geval moet u verwijderen de *FrontEndSSLCipherSuiteOrder* vermelding uit **clusterSettings** en het verzenden van de bijgewerkte Resource Manager-sjabloon als u wilt terugkeren naar de standaard coderingssuite Instellingen.  Wees voorzichtig met deze functionaliteit gebruik.
> 
> 

## <a name="get-started"></a>Aan de slag
De Azure Quickstart-Resource Manager sjabloonsite bevat een sjabloon met de basisdefinitie voor [maken van een App-serviceomgeving](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
