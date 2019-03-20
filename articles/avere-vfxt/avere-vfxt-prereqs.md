---
title: Avere vFXT vereisten - Azure
description: Vereisten voor Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 5642f3acd108d0d3f504fc132522936d1b5ab870
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082582"
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

Een gebruiker met de van eigenaarsmachtigingen voor het abonnement moet het cluster vFXT maken. Abonnement eigenaarsmachtigingen zijn nodig voor deze acties, onder andere:

* Voorwaarden voor de Avere vFXT-software accepteren
* De functie cluster knooppunt toegang maken 

Er zijn twee oplossingen als u niet wilt dat deze eigenaar toegang geven tot de gebruikers die de vFXT maken:

* De eigenaar van een resource-groep kan een cluster maken als deze voorwaarden wordt voldaan:

  * De eigenaar van een abonnement moet [accepteren voor Avere vFXT software](#accept-software-terms) en [maken van de functie cluster knooppunt toegang](#create-the-cluster-node-access-role). 
  * Alle Avere vFXT resources moeten worden geïmplementeerd in de resourcegroep, met inbegrip van:
    * Clustercontroller
    * Clusterknooppunten
    * Blob Storage
    * Netwerkelementen
 
* Een gebruiker die geen eigenaar-machtigingen kunt vFXT clusters maken met behulp van op rollen gebaseerd toegangsbeheer (RBAC vooraf) machtigingen toewijzen aan de gebruiker. Deze methode biedt aanzienlijke machtigingen aan deze gebruikers. [In dit artikel](avere-vfxt-non-owner.md) wordt uitgelegd hoe u om een toegangsrol voor het autoriseren van niet-eigenaren van het maken van clusters te maken.

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

## <a name="create-access-roles"></a>Toegang tot rollen maken 

[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/index.yml) (RBAC) biedt de vFXT cluster netwerkcontroller knooppunten en clusterknooppunten autorisatie noodzakelijke taken uit te voeren.

* De netwerkcontroller cluster heeft toestemming nodig voor maken en wijzigen van virtuele machines om te kunnen maken van het cluster. 

* Afzonderlijke vFXT knooppunten nodig hebt voor handelingen zoals lezen van de Azure-resource-eigenschappen, opslag te beheren en de andere knooppunten netwerkinterface-instellingen beheren als onderdeel van de bewerking van het normale cluster.

Voordat u uw Avere vFXT cluster maken kunt, moet u een aangepaste rol voor gebruik met de clusterknooppunten opgeven. 

U kunt de standaardrol van de sjabloon te accepteren voor de netwerkcontroller cluster. Standaard geeft het cluster netwerkcontroller resource group-eigenaarsbevoegdheden. Als u liever een aangepaste rol voor de controller maken, Zie [aangepaste toegang controllerrol](avere-vfxt-controller-role.md).

> [!NOTE] 
> Alleen de eigenaar van een abonnement of een gebruiker met de rol eigenaar of beheerder van gebruikerstoegang, kunt rollen maken. De rollen kunnen vooraf worden gemaakt.  

### <a name="create-the-cluster-node-access-role"></a>De functie cluster knooppunt toegang maken

<!-- caution - this header is linked to in the template so don't change it unless you can change that -->

Voordat u de vFXT Avere voor Azure-cluster kunt maken, moet u de rol van de cluster-knooppunt maken.

> [!TIP] 
> Interne gebruikers van Microsoft moeten de bestaande rol met de naam 'Cluster-Runtime Avere Operator' in plaats van er wordt geprobeerd gebruiken om er een maken. 

1. Kopieer dit bestand. Uw abonnements-ID in de regel AssignableScopes toevoegen.

   (De huidige versie van dit bestand is opgeslagen in de opslagplaats github.com/Azure/Avere als [AvereOperator.txt](https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereOperator.txt).)  

   ```json
   {
      "AssignableScopes": [
          "/subscriptions/PUT_YOUR_SUBSCRIPTION_ID_HERE"
      ],
      "Name": "Avere Operator",
      "IsCustom": "true",
      "Description": "Used by the Avere vFXT cluster to manage the cluster",
      "NotActions": [],
      "Actions": [
          "Microsoft.Compute/virtualMachines/read",
          "Microsoft.Network/networkInterfaces/read",
          "Microsoft.Network/networkInterfaces/write",
          "Microsoft.Network/virtualNetworks/subnets/read",
          "Microsoft.Network/virtualNetworks/subnets/join/action",
          "Microsoft.Network/networkSecurityGroups/join/action",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "DataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ]
   }
   ```

1. Sla het bestand op als ``avere-operator.json`` of een vergelijkbare gemakkelijk te onthouden naam. 


1. Open een Azure Cloud shell en meld u aan met uw abonnements-ID (beschreven [eerder in dit document](#accept-software-terms)). Gebruik deze opdracht om de rol te maken:

   ```bash
   az role definition create --role-definition /avere-operator.json
   ```

Naam van de rol wordt gebruikt bij het maken van het cluster. In dit voorbeeld wordt de naam is ``avere-operator``.

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