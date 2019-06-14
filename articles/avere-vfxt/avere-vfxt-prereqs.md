---
title: Avere vFXT vereisten - Azure
description: Vereisten voor Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 352833b12c00abbefcf7016d27dfb580ee25e450
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409235"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Voorbereiden op het maken van de Avere vFXT

In dit artikel wordt uitgelegd dat de vereiste taken voor het maken van een Avere vFXT-cluster.

## <a name="create-a-new-subscription"></a>Maak een nieuw abonnement

Beginnen met het maken van een nieuw Azure-abonnement. Gebruik een afzonderlijk abonnement voor elk Avere vFXT project waarmee u eenvoudig alle projectresources en onkosten bijhouden, andere projecten beschermen tegen mogelijke Resourcebeperking tijdens het inrichten en opschonen te vereenvoudigen.  

Een nieuw Azure-abonnement maken in Azure portal:

* Navigeer naar de [blade abonnementen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)
* Klik op de **+ toevoegen** bovenaan op de knop
* Meld u aan als u hierom wordt gevraagd
* Selecteer een aanbieding en doorloopt u de stappen voor het maken van een nieuw abonnement

## <a name="configure-subscription-owner-permissions"></a>Eigenaarsmachtigingen voor abonnement configureren

Een gebruiker met de van eigenaarsmachtigingen voor het abonnement moet het cluster vFXT maken. Abonnement eigenaarsmachtigingen zijn nodig om te accepteren van de servicevoorwaarden voor de software en andere acties worden uitgevoerd. 

Er zijn enkele scenario's voor tijdelijke oplossing waarmee een niet-eigenaren te maken van een vFTX Avere voor Azure-cluster. Deze scenario's hebben betrekking op het beperken van resources en aanvullende rollen toewijzen aan de maker. In beide gevallen moet de eigenaar van een abonnement ook [accepteren voor Avere vFXT software](#accept-software-terms) tevoren. 

| Scenario | Beperkingen | Toegang tot rollen die zijn vereist om de Avere vFXT-cluster te maken | 
|----------|--------|-------|
| De beheerder van de resource-groep | Het virtuele netwerk, het cluster netwerkcontroller en de clusterknooppunten moeten worden gemaakt binnen de resourcegroep | [Beheerder van gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator) en [Inzender](../role-based-access-control/built-in-roles.md#contributor) rollen, zowel binnen het bereik van de doelresourcegroep | 
| Externe vnet | De clustercontroller en de clusterknooppunten zijn gemaakt binnen de resourcegroep, maar een bestaand virtueel netwerk in een andere resourcegroep wordt gebruikt | (1) [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator) en [Inzender](../role-based-access-control/built-in-roles.md#contributor) rollen binnen het bereik van de resourcegroep vFXT; en (2) [Inzender voor virtuele machines](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [gebruikerstoegang Beheerder](../role-based-access-control/built-in-roles.md#user-access-administrator), en [Avere Inzender](../role-based-access-control/built-in-roles.md#avere-contributor) rollen binnen het bereik van de VNET-resourcegroep. |
 
Een alternatief is de rol van een aangepaste op rollen gebaseerde toegang-beheer (RBAC) vooraf maken en toewijzen van bevoegdheden voor de gebruiker, zoals uitgelegd in [in dit artikel](avere-vfxt-non-owner.md). Deze methode biedt aanzienlijke machtigingen aan deze gebruikers. 

## <a name="quota-for-the-vfxt-cluster"></a>Het quotum voor het cluster vFXT

U moet voldoende quotum voor de volgende Azure-onderdelen hebben. Indien nodig, [een verhoging aanvragen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> De virtuele machines en SSD-onderdelen die hier worden vermeld, zijn voor het cluster vFXT zelf. U moet extra quotum voor de VM's en SSD die u wilt gebruiken voor uw compute-farm.  Zorg ervoor dat het quotum voor de regio waar u van plan bent om uit te voeren van de werkstroom is ingeschakeld.

|Azure-onderdeel|Quota|
|----------|-----------|
|Virtuele machines|3 of meer E32s_v3|
|Premium SSD-opslag|200 GB ruimte in het besturingssysteem plus 1 tot 4 TB ruimte in de cache per knooppunt |
|Opslagaccount (optioneel) |v2|
|Back-endopslag van gegevens (optioneel) |Een nieuwe LRS Blob-container |

## <a name="accept-software-terms"></a>Software-voorwaarden accepteren

> [!NOTE] 
> Deze stap is niet vereist als de eigenaar van een abonnement het Avere vFXT-cluster maakt.

Tijdens het maken, moet u de voorwaarden van de service voor de Avere vFXT-software accepteren. Als u geen eigenaar van een abonnement bent, hebt u de eigenaar van een abonnement accepteren tevoren. Deze stap moet slechts één keer per abonnement worden uitgevoerd.

De software om voorwaarden te accepteren van tevoren: 

1. Een cloudshell openen in Azure portal of door te bladeren naar <https://shell.azure.com>. Meld u aan met uw abonnements-ID.

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Voert u deze opdracht voor het accepteren van de service en programmatische toegang inschakelen voor de vFXT Avere voor de installatiekopie van het Azure-software: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>Maken van een service-eindpunt voor opslag in uw virtuele netwerk (indien nodig)

Een [service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) houdt Azure Blob-verkeer lokale in plaats van deze routering buiten het virtuele netwerk. Het wordt aanbevolen voor alle vFXT Avere voor Azure-cluster die gebruikmaakt van Azure Blob voor back-end-gegevensopslag. 

Als u een bestaand vnet opgeeft en het maken van een nieuwe Azure Blob-container voor uw back-end-opslag als onderdeel van het maken van een cluster, moet u een service-eindpunt in het vnet voor Microsoft-opslag hebben. Dit eindpunt moet bestaan voordat u het cluster maakt, het maken, anders mislukt. 

Een storage-service-eindpunt wordt aanbevolen voor alle vFXT Avere voor Azure-cluster die gebruikmaakt van Azure Blob-opslag, zelfs als u de opslag later toevoegen. 

> [!TIP] 
> * Deze stap overslaan als u een nieuw virtueel netwerk als onderdeel van het maken van clusters maakt. 
> * Deze stap is optioneel als u geen Blob-opslag tijdens het maken van clusters maakt. In dat geval kunt u het service-eindpunt later maken als u besluit te gebruiken van Azure Blob.

Het storage-service-eindpunt van de Azure-portal maken. 

1. Klik in de portal op **virtuele netwerken** aan de linkerkant.
1. Selecteer het vnet voor uw cluster. 
1. Klik op **Service-eindpunten** aan de linkerkant.
1. Klik op **toevoegen** aan de bovenkant.
1. De service als ``Microsoft.Storage`` en kies subnet van het cluster.
1. Klik onderaan op **toevoegen**.

   ![Schermafbeelding van Azure portal met aantekeningen voor de stappen voor het maken van het service-eindpunt](media/avere-vfxt-service-endpoint.png)


## <a name="next-step-create-the-vfxt-cluster"></a>Volgende stap: Maken van het cluster vFXT

Na het voltooien van deze vereisten, kunt u gaan in het cluster zelf te maken. Lezen [implementeren van het cluster vFXT](avere-vfxt-deploy.md) voor instructies.