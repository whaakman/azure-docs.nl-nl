---
title: Openbare IP-adressen toevoegen in Azure Stack | Microsoft Docs
description: Leer hoe u meer openbare IP-adressen toevoegen aan Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2019
ms.author: jeffgilb
ms.reviewer: scottnap
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 09805719262f0a1d30f3b38af4b5209667d25e5a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195367"
---
# <a name="add-public-ip-addresses"></a>Openbare IP-adressen toevoegen
*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*  

Leer hoe u meer openbare IP-adressen toevoegen aan Azure Stack.  In dit artikel, verwijzen we naar de externe adressen als openbare IP-adressen, maar in Azure Stack is dit bedoeld om te verwijzen naar het IP-adresblokken toe te voegen aan het externe netwerk.  Of dat externe netwerk openbare Internet routeerbaar is of is op een Intranet en maakt gebruik van privé-adresruimte maakt niet uit voor de doeleinden van dit artikel.  De stappen zijn hetzelfde. 

## <a name="add-a-public-ip-address-pool"></a>Een openbare IP-adresgroep toevoegen
U kunt openbare IP-adressen toevoegen aan uw Azure Stack-systeem op elk gewenst moment na de eerste implementatie van het Azure Stack-systeem. Bekijk hoe u [verbruik weergave openbare IP-adressen](azure-stack-viewing-public-ip-address-consumption.md) om te zien wat de huidige gebruik en openbare IP-adres beschikbaar is in uw Azure Stack.

Op hoog niveau, is het proces van het toevoegen van een nieuw blok met openbare IP-adres met Azure Stack ziet er als volgt:

 ![IP-stroom toevoegen](media/azure-stack-add-ips/flow.PNG)

## <a name="obtain-the-address-block-from-your-provider"></a>Het adresblok niet verkrijgen van uw provider
Het eerste wat dat u moet doen is om op te halen van de-Adresblok dat u wilt toevoegen aan Azure Stack.  Afhankelijk van waar u uw Adresblok van krijgen, moet u rekening houden met de doorlooptijd is en deze beheren op basis van de snelheid waarmee u openbare IP-adressen in Azure Stack verbruikt.  

> [!IMPORTANT]
> Azure Stack accepteert-Adresblok dat u opgeeft, zolang het een geldig Adresblok is en niet met een bestaande-adresbereik in Azure Stack overlapt.  Zorg ervoor dat u een geldig Adresblok dat routeerbaar is en niet-overlappende met het externe netwerk waarmee Azure Stack is verbonden.  Als u het bereik met Azure Stack toevoegt, kunt u het niet verwijderen.

## <a name="add-the-ip-address-range-to-azure-stack"></a>Het IP-adresbereik toevoegen aan Azure Stack

1. Navigeer in een internetbrowser naar uw portal-Beheerdashboard.  In dit voorbeeld gebruiken we https://adminportal.local.azurestack.external.  
2.  Meld u aan de Azure Stack-beheerportal als een cloud-operator.
3.  Op het standaarddashboard: de lijst met regio zoeken en selecteer de regio die u beheren wilt, bijvoorbeeld, lokale.
4.  De resourceproviders tegel en klik op de Netwerkresourceprovider vinden.
5.  Klik op het openbare IP-adres voor groepen van tegel-gebruik.
6.  Klik op de knop van de groep IP toevoegen.
7.  Geef een naam voor de IP-adresgroep.  De naam die u dient alleen voor kunt u de IP-adresgroep eenvoudig kunt identificeren, zodat u kunt deze aanroepen wat u maar wilt.  Het is raadzaam om te maken van de naam van de hetzelfde als het adresbereik, maar dat is niet vereist.
8.   Voer de-Adresblok dat u wilt toevoegen in CIDR-notatie.  Bijvoorbeeld: 192.168.203.0/24
9.  Wanneer u een geldige CIDR-bereik in het veld adres adresbereik (CIDR-blok) het eerste IP-adres opgeeft, worden laatste IP-adres en beschikbare IP-adressen velden automatisch ingevuld.  Ze zijn alleen-lezen en automatisch gegenereerd, zodat u kunt deze zonder te wijzigen van de waarde in het veld adresbereik niet wijzigen.
10. Bekijk de informatie op de blade en te bevestigen dat alles ziet er corrigeren, klikt u op Ok om door te de wijziging doorvoeren en het adresbereik toevoegen aan Azure Stack.


## <a name="next-steps"></a>Volgende stappen 
[Eenheid revisie-knooppunt schaalacties](azure-stack-node-actions.md) 
