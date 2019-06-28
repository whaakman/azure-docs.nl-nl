---
title: Back-end-opslag toevoegen aan het Microsoft Azure FXT Edge Filer-cluster
description: Back-end-opslag en de clientgerichte pseudonamespace configureren voor Azure FXT Edge Filer
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 0a16e654ff92c450438ac91c590b42d22201d015
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450455"
---
# <a name="tutorial-add-back-end-storage-and-configure-the-virtual-namespace"></a>Zelfstudie: Back-end-opslag toevoegen en configureren van de virtuele-naamruimte 

In deze zelfstudie wordt uitgelegd hoe u back-edge-opslag voor uw cache toevoegen en over het instellen van het virtuele bestandssysteem clientgerichte. 

Het cluster maakt verbinding met back-end opslagsystemen voor toegang tot de gegevensaanvraag voor clients en voor het opslaan van wijzigingen meer permanent dan in de cache. 

De naamruimte is de clientgerichte pseudo-bestandssysteem waarmee u kunt het wisselen van back-end opslag zonder veranderende client-side-werkstromen. 

In deze zelfstudie leert u: 

> [!div class="checklist"]
> * Back-end-opslag toevoegen aan het cluster Azure FXT Edge Filer 
> * Over het definiëren van het pad clientgerichte voor opslag

## <a name="about-back-end-storage"></a>Informatie over back-end-opslag

De Azure FXT Edge Filer-cluster maakt gebruik van een *filer core* definitie van een back-end-opslagsysteem koppelen aan het cluster FXT.

Azure FXT Edge Filer is compatibel met enkele populaire NAS hardware-systemen en Lege containers van Azure Blob of andere opslag in de cloud kunt gebruiken. 

Cloud storage-containers moet leeg zijn als het toegevoegd, zodat het besturingssysteem FXT kunt alle van de gegevens op het opslagvolume cloud volledig beheren. U kunt uw bestaande gegevens verplaatsen naar de cloudcontainer na het toevoegen van de container met het cluster als een filer core.

Gebruik het Configuratiescherm een filer core toevoegen aan uw systeem.

> [!NOTE]
> 
> Als u wilt het gebruik van Amazon AWS en Google Cloud storage, moet u een FlashCloud<sup>TM</sup> functie licentie. Neem contact op met uw Microsoft-vertegenwoordiger voor een licentiesleutel en volg de instructies in de verouderde configuratiehandleiding voor [toevoegen of verwijderen van licenties van de functie](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/install_licenses.html#install-licenses).
> 
> Ondersteuning voor Azure Blob-opslag is opgenomen in de Azure FXT Edge Filer-softwarelicentie. 

Lees deze gedeeltes van de Cluster-configuratie-handleiding voor meer gedetailleerde informatie over het toevoegen van core filter:

* Lees voor meer informatie over kiezen en het toevoegen van een filer core voorbereiden, [werken met Core filter](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/core_filer_overview.html#core-filer-overview).
* Lees deze artikelen voor gedetailleerde vereisten en stapsgewijze instructies:

  * [Een nieuwe NAS Core Filer toevoegen](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html#create-core-filer-nas)
  * [Een nieuwe Cloud Core Filer toevoegen](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#create-core-filer-cloud)

Nadat u een filer core hebt toegevoegd, kunt u de instellingen op de pagina met Details van Core Filer instellingen bijwerken.

## <a name="add-a-core-filer"></a>Toevoegen van een filer core

Een filer core definiëren door te klikken op de **maken** knop op de **Core Filer** > **Core-filter beheren** instellingenpagina.

![Op de knop maken klikt boven de lijst met core filter op de pagina Core-filter beheren](media/fxt-cluster-config/create-core-filer-button.png)

De **nieuwe Core Filer toevoegen** wizard leidt u door het proces voor het maken van een filer core die is gekoppeld aan uw back endopslag. Handleiding voor het Cluster is een stapsgewijze beschrijving van het proces, dit is een ander voor NFS/NAS-opslag en voor opslag in de cloud (koppelingen zijn hierboven). 

Subtaken zijn onder andere:

* Geef het type core filer (NAS of cloud)

  ![Eerste pagina van de hardware NAS nieuwe core filer wizard. De optie voor 'cloud core filer' is uitgeschakeld en wordt een foutbericht weergegeven over de licentie ontbreekt.](media/fxt-cluster-config/new-nas-1.png)

* Naam van de core-filer instellen. Kies een naam waarmee de clusterbeheerders begrijpen welke opslagsysteem vertegenwoordigt.

* Voor het filter van NAS-core, geeft u de volledig gekwalificeerde domeinnaam (FQDN) of IP-adres. FQDN-naam wordt aanbevolen voor alle core-filter en voor SMB-toegang is vereist.

* Selecteer een beleid voor cache - de tweede pagina van de wizard worden de beschikbare cache-beleidsregels voor de nieuwe core filer. Lees voor meer informatie, de [sectie beleid de handleiding voor het Cluster in de cache](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_manage_cache_policies.html). 

  ![Tweede pagina van een hardware NAS core filer wizard nieuwe; de Cache-beleid in het menu is geopend, met dat verschillende opties en beleidsopties drie geldige cache (bypass, caching en caching van lezen/schrijven lezen) uitgeschakeld.](media/fxt-cluster-config/new-nas-choose-cache-policy.png)

* Voor opslag in de cloud, moet u de cloud service- en toegangsbeheer referenties, onder andere parameters opgeven. Lees voor meer informatie, [Cloud-service en het protocol](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html#cloud-service-and-protocol) in de handleiding voor het Cluster.

  ![Belangrijkste filer informatie in de wizard Nieuwe Core Filer in de cloud](media/fxt-cluster-config/new-core-filer-cloud3.png) <!-- xxx get an Azure version of this screenshot xxx -->
  
  Als u cloud-referenties voor dit cluster al hebt toegevoegd, worden ze weergegeven in de lijst. Bijwerken en toevoegen van de referenties in de **Cluster** > **Cloudreferenties** instellingenpagina. 

Na het invullen van alle vereiste instellingen in de wizard, klikt u op de **Filer toevoegen** knop om in te dienen de wijziging.

Na enkele ogenblikken wordt het opslagsysteem dat wordt weergegeven op de **Dashboard** filter lijst core en is toegankelijk via core filer pagina's met instellingen.

![Core filer "belegd-NAS' op de instellingenpagina van Core-filter beheren met de detailweergave filer uitgevouwen](media/fxt-cluster-config/core-filer-in-manage-page.png)

De core filer in deze schermafbeelding is een vserver ontbreekt. U moet de filer core koppelen aan een vserver en maken van een verbinding zodat clients toegang de opslag tot hebben. Deze stappen worden hieronder beschreven in [configureren van de naamruimte](#configure-the-namespace).

## <a name="configure-the-namespace"></a>De naamruimte configureren

Het Azure FXT Edge Filer-cluster maakt een virtueel bestandssysteem met de naam de *cluster naamruimte* die vereenvoudigt de clienttoegang tot gegevens die zijn opgeslagen op verschillende back-endsystemen. Omdat clients bestanden met behulp van een virtueel pad vragen, kunnen u opslagsystemen toegevoegd of vervangen zonder te hoeven wijzigen van de client-werkstroom. 

De clusternaamruimte kunt u cloud- en NAS-opslagsystemen in een vergelijkbaar bestandsstructuur aanwezig. 

Van het cluster vservers onderhouden van de naamruimte en de inhoud aan clients. Er zijn twee stappen voor het maken van de clusternaamruimte: 

1. Een vserver maken 
1. Instellen van koppelingen tussen de back-end opslagsystemen en de paden clientgerichte bestandssysteem 

### <a name="create-a-vserver"></a>Een vserver maken

VServers zijn virtuele bestandsservers die bepalen hoe gegevens stromen tussen de client en van het cluster core filter:

* VServers clientgerichte IP-adressen
* VServers maken van de naamruimte en koppelingen waarmee de structuur van de virtuele map clientgerichte wordt toegewezen aan de uitvoer op back-endopslag definiëren
* VServers afdwingen bestand toegangsbeheer, met inbegrip van core filer export beleidsregels en gebruikerssystemen verificatie
* VServers bieden SMB-infrastructuur

Voordat u begint met het configureren van een cluster vserver, lees de documentatie van de gekoppelde en raadpleegt u uw Microsoft-vertegenwoordiger voor help understanding naamruimte en vservers. Als u gebruikmaakt van VLAN's, [maken ze](fxt-configure-network.md#adjust-network-settings) voordat u de vserver maakt. 

Deze secties van de handleiding voor het Cluster kunt u raken met de FXT vserver en globale naamruimte functies:

* [Het maken van en werken met VServers](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#creating-and-working-with-vservers)
* [Met behulp van een globale Namespace](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gns_overview.html)
* [Het maken van een VServer](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver)

U moet ten minste één vserver voor uw cluster. 

Voor het maken van een nieuwe vserver, moet u de volgende informatie:

* De naam in te stellen voor de vserver

* Het bereik van IP-adressen clientgerichte die wordt afgehandeld tijdens de vserver

  Wanneer u de vserver maakt, moet u een bereik van aaneengesloten IP-adressen opgeven. U kunt meer adressen later toevoegen met behulp van de **gerichte clientnetwerk** instellingenpagina.

* Als uw netwerk VLAN's, welke VLAN moet worden gebruikt voor deze vserver

Gebruik de **VServer** > **VServers beheren** instellingenpagina te maken van een nieuwe vserver. Lees voor meer informatie [het maken van een VServer](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_vserver_manage.html#creating-a-vserver) in de handleiding voor het Cluster. 

![pop-upvenster voor het maken van een nieuwe vserver](media/fxt-cluster-config/new-vserver.png)

### <a name="create-a-junction"></a>Een verbinding maken

Een *koppelingspunten* een opslagpad back-end wordt toegewezen aan de client-zichtbare naamruimte.

U kunt dit systeem gebruiken voor het vereenvoudigen van het pad dat wordt gebruikt in koppelpunten van de client en capaciteit naadloos schalen omdat één virtueel pad aankan meerdere core filter-opslag.

![Pagina van de wizard Nieuwe verbinding toevoegen met instellingen ingevuld](media/fxt-cluster-config/add-junction-full.png)

Raadpleeg [ **VServer** > **Namespace** ](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html) in het Cluster configuratiehandleiding voor meer informatie over het maken van een naamruimte-verbinding.

![De VServer > pagina Namespace-instellingen van de details voor een verbinding](media/fxt-cluster-config/namespace-populated.png)

## <a name="configure-export-rules"></a>Exporteren-regels configureren

Nadat u een vserver zowel een filer core hebt, moet u de Exportregels aanpassen en beleidsregels die bepalen hoe clients toegang krijgen tot bestanden op de uitvoer van de filer core exporteren.

Gebruik eerst de **VServer** > **regels exporteren** pagina toe te voegen nieuwe regels om te wijzigen van het standaardbeleid of om uw eigen aangepaste export-beleid te maken.

Gebruik vervolgens de **VServer** > **beleid exporteren** pagina aan het aangepaste beleid toepassen op uw core-filer uitvoer wanneer via die vserver geopend.

Lees het artikel Cluster configuratiehandleiding [toegang beheren tot Core Filer uitvoer](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/export_rules_overview.html) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

Na het toevoegen van opslag en configureren van de naamruimte clientgerichte, voert u de eerste configuratie van uw cluster: 

> [!div class="nextstepaction"]
> [De netwerkinstellingen van het cluster configureren](fxt-configure-network.md)
