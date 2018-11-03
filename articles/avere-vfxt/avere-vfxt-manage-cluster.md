---
title: Het Avere vFXT cluster - Azure beheren
description: Avere cluster beheren - toevoegen of verwijderen van knooppunten, starten, stoppen of Vernietig het cluster vFXT
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 9eba9eef315f1ec6eb25ea21c1d4747ab002d078
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958913"
---
# <a name="manage-the-avere-vfxt-cluster"></a>Het Avere vFXT-cluster beheren

Nadat het cluster is gemaakt, moet u mogelijk clusterknooppunten toevoegen of stoppen of opnieuw opstarten van het cluster. En wanneer uw project is voltooid. u moet weten stoppen en het cluster permanent verwijderen. 

Afhankelijk van de beheertaak cluster moet u mogelijk het Avere van het Configuratiescherm, het script voor het maken van vfxt.py cluster of de Azure-portal gebruiken om dit te doen. 

Deze tabel geeft een overzicht van deze hulpprogramma's voor elke taak kunnen worden gebruikt. 

| Bewerking | Avere Configuratiescherm | vfxt.PY  | Azure Portal |
| --- | --- | --- | --- |
| Clusterknooppunten toevoegen | nee | ja | nee |
| Clusterknooppunten verwijderen | ja | nee | nee |
| Stoppen van een clusterknooppunt | Ja (ook services opnieuw starten of opnieuw opstarten) | nee | een VM-knooppunt van de portal ommige wordt geïnterpreteerd als een knooppuntfout |
| Start een gestopte knooppunt | nee | nee | ja |
| Een knooppunt één cluster vernietigen | nee | nee | ja |
| Opnieuw opstarten van het cluster |  |  |  |
| Afgesloten of het cluster veilig stoppen | ja | ja | nee |
| Vernietig het cluster  | nee | ja | Ja, maar de integriteit van gegevens kan niet worden gegarandeerd |

Gedetailleerde instructies voor elke tool zijn hieronder vermeld.

## <a name="about-stopped-instances-in-azure"></a>Over gestopt-exemplaren in Azure

Wanneer u afgesloten of een Azure-VM te stoppen, stopt compute-kosten, maar u nog steeds moet betalen voor de opslag. Als u een knooppunt vFXT of het cluster volledige vFXT afsluit en u niet van plan bent deze opnieuw te starten, moet u de Azure-portal te verwijderen van de bijbehorende virtuele machines. 

In de Azure-portal een *gestopt* knooppunt (dit kan opnieuw worden gestart) wordt de status **gestopt** in de Azure-portal; een *verwijderd* knooppunt wordt de status **gestopt (toewijzing opgeheven)**  en deze niet meer leidt tot kosten voor berekenen of opslaan.

Voordat de virtuele machine te verwijderen, ervoor zorgen dat alle gewijzigde gegevens is geschreven in de cache naar het back-end-opslag met behulp van de opties voor Avere het Configuratiescherm of vfxt.py te stoppen of sluit het cluster.

## <a name="manage-the-cluster-with-avere-control-panel"></a>Het cluster met Avere Configuratiescherm beheren 

Het Configuratiescherm Avere kan worden gebruikt voor deze taken: 

* Stoppen of opnieuw opstarten van afzonderlijke knooppunten
* Een knooppunt uit het cluster verwijderen
* Stoppen of opnieuw opstarten van het hele cluster

Configuratiescherm Avere bepaalt de volgorde van de integriteit van gegevens, zodat deze probeert te maken de gewijzigde gegevens schrijven naar de back endopslag voordat een mogelijk destructieve bewerking. Dit maakt het een veiliger optie dan de Avere-portal. 

Avere Configuratiescherm openen via een webbrowser. Volg de instructies in [toegang tot het cluster vFXT](avere-vfxt-cluster-gui.md) als u hulp nodig hebt.

### <a name="manage-nodes-with-avere-control-panel"></a>Beheer knooppunten met Avere-Configuratiescherm

De **FXT knooppunten** instellingenpagina bevat besturingselementen voor het beheren van afzonderlijke knooppunten.

Als u wilt afsluiten, opnieuw opstarten of een knooppunt verwijderen, het knooppunt vinden in de lijst op de **FXT knooppunten** pagina en klik op de knop in de **acties** kolom.

> [!NOTE] 
> IP-adressen kunnen verplaatsen tussen clusterknooppunten wanneer het aantal actieve knooppunten wordt gewijzigd.

Lezen [Cluster > knooppunten FXT](<http://library.averesystems.com/ops_guide/4_7/gui_fxt_nodes.html#gui-fxt-nodes>) in de Avere cluster instellingen guide voor meer informatie.

### <a name="stop-or-reboot-the-cluster-with-avere-control-panel"></a>Stoppen of opnieuw opstarten van het cluster met Avere-Configuratiescherm

De **systeemonderhoud** instellingenpagina bevat opdrachten voor het clusterservices opnieuw te starten, opnieuw opstarten van het cluster of het cluster veilig ommige. Lezen [beheer > systeemonderhoud](<http://library.averesystems.com/ops_guide/4_7/gui_system_maintenance.html#gui-system-maintenance>) (in de Avere cluster instellingen guide) voor meer informatie.

Wanneer een cluster wordt afgesloten, het boeken van statusberichten naar de **Dashboard** tabblad in eerste instantie. Na enkele ogenblikken wordt reageert de sessie Avere Configuratiescherm, wat betekent dat het cluster is afgesloten.

## <a name="manage-the-cluster-with-vfxtpy"></a>Het cluster met vfxt.py beheren 

Het script vfxt.py kan worden gebruikt voor deze beheertaken cluster:

* Nieuwe knooppunten toevoegen aan een cluster
* Stoppen en starten van een cluster  
* Een cluster vernietigen

Zoals Avere het Configuratiescherm probeer vfxt.py bewerkingen om te controleren of de gewijzigde gegevens permanent worden opgeslagen op opslag voor back-end voordat u afsluit of vernietigen van het cluster of het knooppunt. Dit maakt het een veiliger optie dan de Avere-portal.

vfxt.PY is vooraf geïnstalleerd in het cluster netwerkcontroller-VM. <!-- (If you want to install it on another system, refer to http://github.com/Azure/AvereSDK) xxx change when this repo goes  public -->

Een volledige vfxt.py (Engelstalig) is beschikbaar op GitHub: [Clusterbeheer met vfxt.py in de Cloud](https://github.com/azure/averesdk/blob/master/docs/README.md)

### <a name="add-cluster-nodes-with-vfxtpy"></a>Clusterknooppunten met vfxt.py toevoegen

Een voorbeeldscript van de opdracht voor het toevoegen van knooppunten van het cluster is opgenomen op de clustercontroller. Zoek ``./add-nodes`` op de domeincontroller en open het in een editor naar deze aanpassen met de clustergegevens van uw. 

Het cluster moet worden uitgevoerd voor het gebruik van deze opdracht. 

De volgende waarden opgeven: 

* Resourcegroepnaam voor het cluster, en ook voor netwerk- en opslagresources als ze zijn niet gelijk zijn aan het cluster
* Locatie van cluster
* Clusternetwerk en subnet 
* Toegang tot de rol van knooppunt 
* Cluster-IP-adresbeheer en beheerderswachtwoord 
* Aantal knooppunten om toe te voegen (1, 2 of 3)
* Waarden uit de knooppunt exemplaar type en de cache-grootte 

Als u het model niet gebruikt, moet u een opdracht als de volgende, met inbegrip van alle van de hierboven beschreven gegevens opgeven. 

```bash
   vfxt.py --cloud-type azure --from-environment \
   --resource-group GROUP_NAME \ 
   [--network-resource-group GROUP_NAME --storage-resource-group GROUP_NAME]  \
   --location LOCATION --azure-network NETWORK_NAME --azure-subnet SUBNET_NAME \
   --add-nodes --nodes NODE_COUNT \
   --management-address CLUSTER_IP --admin-password ADMIN_PASSWORD \
   --instance-type TYPE --node-cache-size SIZE \
   --azure-role ROLE_NAME \
   --log ~/vfxt.log
```

Lees voor meer informatie, [knooppunten toevoegen aan een cluster](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#add-nodes-to-a-cluster) in de vfxt.py (Engelstalig).

### <a name="stop-a-cluster-with-vfxtpy"></a>Een cluster met vfxt.py stoppen

```bash
vfxt.py --cloud-type azure --from-environment --stop --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET
```

### <a name="start-a-stopped-cluster-with-vfxtpy"></a>Een gestopte cluster met vfxt.py starten

```bash
vfxt.py --cloud-type azure --from-environment --start --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --instances INSTANCE1_ID INSTANCE2_ID INSTANCE3_ID ...
```    

Omdat het cluster is gestopt, moet u de exemplaar-id's om op te geven van de clusterknooppunten doorgeven. Lezen [op te geven welke cluster wijzigen](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#specifying-which-cluster-to-modify) in de vfxt.py (Engelstalig) voor meer informatie.

### <a name="destroy-a-cluster-with-vfxtpy"></a>Vernietig een cluster met vfxt.py

```bash
vfxt.py --cloud-type azure --from-environment --destroy --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --management-address ADMIN_IP
```

De optie ``--quick-destroy`` kan worden gebruikt als u niet wilt schrijven gewijzigde gegevens uit de cache van het cluster.

Lees de [vfxt.py (Engelstalig)](<https://github.com/Azure/AvereSDK/blob/master/docs/README.md>) voor meer informatie.  

## <a name="manage-cluster-vms-from-the-azure-portal"></a>Cluster-VM's beheren vanuit Azure portal 

De Azure-portal kan worden gebruikt om het cluster virtuele machines afzonderlijk vernietigen, maar de integriteit van gegevens kan niet worden gegarandeerd als het cluster geen duidelijke eerst wordt afgesloten. 

De Azure-portal kan worden gebruikt voor deze beheertaken cluster: 

* Start een gestopte vFXT-knooppunt
* Stoppen van een afzonderlijke vFXT-knooppunt (in het cluster wordt dit weergegeven als een knooppuntfout.)
* Vernietig een cluster vFXT *als* hoeft u niet om ervoor te zorgen dat de gewijzigde gegevens in de cache van het cluster is geschreven naar de filer core
* VFXT knooppunten en andere resources van het cluster permanent verwijderd nadat ze zijn afgesloten veilig

### <a name="restart-vfxt-instances-from-the-azure-portal"></a>VFXT exemplaren vanuit Azure portal opnieuw starten

Als u een gestopte knooppunt opnieuw opstarten wilt, moet u de Azure-portal. Selecteer **virtuele machines** in het menu links en klik vervolgens op de naam van de virtuele machine in de lijst om de overzichtspagina te openen.

Klik op de **Start** knop aan de bovenkant van de overzichtspagina om opnieuw te activeren van de virtuele machine.

![Azure portal scherm met de optie voor een gestopte virtuele machine starten](media/avere-vfxt-start-stopped-incurring-annot.png)

### <a name="delete-cluster-nodes"></a>Clusterknooppunten verwijderen

Als u wilt verwijderen van een knooppunt uit het cluster vFXT, maar blijven de rest van het cluster, moet u eerst [het knooppunt verwijderen uit het cluster](#manage-nodes-with-avere-control-panel) met het Avere van het Configuratiescherm.

> [!CAUTION]
> Als u een knooppunt verwijdert zonder deze eerst te verwijderen uit het cluster vFXT, is het mogelijk dat gegevens verloren gaan.

Een of meer exemplaren die worden gebruikt als vFXT knooppunt permanent vernietigen, gebruikt de Azure portal.
Selecteer **virtuele machines** in het menu links en klik vervolgens op de naam van de virtuele machine in de lijst om de overzichtspagina te openen.

Klik op de **verwijderen** knop aan de bovenkant van de overzichtspagina van de virtuele machine permanent vernietigen.

U kunt deze methode gebruiken om clusterknooppunten permanent te verwijderen nadat ze zijn afgesloten veilig. 

### <a name="destroy-the-cluster-from-the-azure-portal"></a>Vernietig het cluster vanuit Azure portal

> [!NOTE] 
> Als u eventuele resterende clientwijzigingen in de cache worden geschreven naar de back-end-opslag wilt, gebruikt u de vfxt.py `--destroy` optie of gebruik Avere in het Configuratiescherm op het cluster correct afgesloten voordat u verwijdert de exemplaren van de knooppunten in de Azure-portal.

U kunt Knooppuntexemplaren permanent vernietigen door ze te verwijderen in Azure portal. U kunt ze één voor één zoals is beschreven hierboven, of u kunt verwijderen de **virtuele Machines** pagina alle van de cluster-VM's zoeken, selecteert u deze met de selectievakjes in en klikt u op de **verwijderen** knop om ze te verwijderen in alle een actie.

![Lijst met virtuele machines in de portal, gefilterd op de term 'cluster', waarvan er drie van de vier gecontroleerd en is gemarkeerd](media/avere-vfxt-multi-vm-delete.png)

### <a name="delete-additional-cluster-resources-from-the-azure-portal"></a>Aanvullende clusterbronnen verwijderen uit de Azure portal

Als u aanvullende bronnen voor het cluster vFXT hebt gemaakt, is het raadzaam om ze als onderdeel van het cluster af kwijtraken te verwijderen. U moet niet-elementen die bevatten gegevens die u nodig hebt, of de items die worden gedeeld met andere projecten vernietigen.

Naast het verwijderen van de clusterknooppunten, houd rekening met het verwijderen van deze onderdelen: 

* Het cluster netwerkcontroller-VM
* Gegevensschijven die zijn gekoppeld aan de clusterknooppunten
* Netwerkinterfaces en openbare IP-adressen die zijn gekoppeld aan de clusteronderdelen
* Virtuele netwerken
* Storage-accounts (**alleen** als ze geen belangrijke gegevens bevatten)
* Beschikbaarheidsset 

![Azure-portal 'alle resources' lijst met resources die zijn gemaakt voor een testcluster](media/avere-vfxt-all-resources-list.png)

### <a name="delete-a-clusters-resource-group-from-the-azure-portal"></a>Resourcegroep van een cluster verwijderen uit de Azure portal

Als u een resourcegroep specifiek voor het cluster hebt gemaakt, kunt u alle gerelateerde resources voor het cluster vernietigen door het vernietigen van de resourcegroep. 

> [!Caution] 
> De resourcegroep alleen verwijderen als u er zeker van zijn dat er niets van waarde bevindt zich in de groep. Bijvoorbeeld, moet u ervoor dat u hebt alle benodigde gegevens verplaatst van een storage-containers binnen de resourcegroep.  

Als u wilt een resourcegroep verwijderen, klikt u op **resourcegroepen** in het menu links van de portal en filter de lijst met resourcegroepen te vinden die u hebt gemaakt voor het cluster vFXT. Selecteer de resourcegroep en klik op de drie punten aan de rechterkant van het paneel. Kies **Resourcegroep verwijderen**. De portal wordt u gevraagd om te bevestigen van de verwijdering niet ongedaan worden gemaakt is.  

![Resourcegroep met de actie "Resourcegroep verwijderen"](media/avere-vfxt-delete-resource-group.png)
