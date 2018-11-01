---
title: Avere vFXT vereisten - Azure
description: Vereisten voor Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 823bf50a54ff43fa95f7136c137e3d8f3303c3e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633932"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Voorbereidingen voor het maken van de vFXT Avere

In dit artikel wordt uitgelegd dat de vereiste taken voor het maken van een Avere vFXT-cluster.

## <a name="create-a-new-subscription"></a>Maak een nieuw abonnement

Beginnen met het maken van een nieuw Azure-abonnement. Gebruik een afzonderlijk abonnement voor elk Avere vFXT project waarmee u eenvoudig alle projectresources en onkosten bijhouden, andere projecten beschermen tegen mogelijke Resourcebeperking tijdens het inrichten en opschonen te vereenvoudigen.  

Een nieuw Azure-abonnement maken in Azure portal:

* Navigeer naar de [blade abonnementen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)
* Klik op de **+ toevoegen** bovenaan op de knop
* Meld u aan als u hierom wordt gevraagd
* Selecteer een aanbieding en doorloopt u de stappen voor het maken van een nieuw abonnement

## <a name="configure-subscription-owner-permissions"></a>Eigenaarsmachtigingen voor abonnement configureren

Een gebruiker met de van eigenaarsmachtigingen voor het abonnement moet het cluster vFXT maken. Abonnement eigenaarsmachtigingen zijn nodig voor deze acties, onder andere:

* Voorwaarden voor de Avere vFXT-software accepteren
* De functie cluster knooppunt toegang maken
* Toestaan dat het clusterknooppunt van de domeincontroller voor het beheren van resourcegroepen en virtuele netwerken 
* De netwerkcontroller cluster maken en wijzigen van de clusterknooppunten toestaan 

Er zijn twee oplossingen als u niet wilt dat deze eigenaar toegang geven tot de gebruikers die de vFXT maken:

* De eigenaar van een resource-groep kan een cluster maken als deze voorwaarden wordt voldaan:

  * De eigenaar van een abonnement moet [accepteren voor Avere vFXT software](#accept-software-terms-in-advance) en [maken van de functie cluster knooppunt toegang](avere-vfxt-deploy.md#create-the-cluster-node-access-role).
  * Alle Avere vFXT resources moeten worden geïmplementeerd in de resourcegroep, met inbegrip van:
    * Clustercontroller
    * Clusterknooppunten
    * Blob Storage
    * Netwerkelementen
 
* Een gebruiker die geen eigenaar-machtigingen kunt vFXT clusters maken als een extra toegang-rol is gemaakt en aan de gebruiker vooraf toegewezen. Deze rol biedt echter aanzienlijke machtigingen aan deze gebruikers. [In dit artikel](avere-vfxt-non-owner.md) wordt uitgelegd hoe u aan niet-eigenaren om clusters te maken.

## <a name="quota-for-the-vfxt-cluster"></a>Het quotum voor het cluster vFXT

U moet voldoende quotum voor de volgende Azure-onderdelen hebben. Indien nodig, [een verhoging aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> De virtuele machines en SSD-onderdelen die hier worden vermeld, zijn voor het cluster vFXT zelf. U moet extra quotum voor de VM's en SSD die u wilt gebruiken voor uw compute-farm.  Zorg ervoor dat het quotum voor de regio waar u van plan bent om uit te voeren van de werkstroom is ingeschakeld.

|Azure-onderdeel|Quota|
|----------|-----------|
|Virtuele machines|3 of meer D16s_v3 of E32s_v3|
|Premium-SSD-opslag|200 GB ruimte op de OS plus 1 TB tot 4 TB ruimte in cache per knooppunt |
|Storage-account (optioneel) |v2|
|Opslag van back-end-gegevens (optioneel) |Een nieuwe LRS Blob-container |

## <a name="accept-software-terms-in-advance"></a>Software vooraf voorwaarden accepteren

> [!NOTE] 
> Deze stap is niet vereist als de eigenaar van een abonnement het Avere vFXT-cluster maakt.

Voordat u een cluster maken kunt, moet u de voorwaarden van de service voor de Avere vFXT-software accepteren. Als u geen eigenaar van een abonnement bent, hebt u de eigenaar van een abonnement accepteren tevoren. Deze stap moet slechts één keer per abonnement worden uitgevoerd.

De software om voorwaarden te accepteren van tevoren: 

1. Een cloudshell openen in Azure portal of door te bladeren naar <https://shell.azure.com>. Meld u aan met uw abonnements-ID.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Voert u deze opdracht voor het accepteren van de service en programmatische toegang inschakelen voor de vFXT Avere voor installatiekopieën van Azure-software: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-node:latest
   ```

## <a name="next-step-create-the-vfxt-cluster"></a>Volgende stap: maken van het cluster vFXT

Na het voltooien van deze vereisten, kunt u gaan in het cluster zelf te maken. Lezen [implementeren van het cluster vFXT](avere-vfxt-deploy.md) voor instructies.