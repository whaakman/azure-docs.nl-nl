---
title: Avere vFXT implementeren voor Azure
description: Stappen voor het implementeren van het cluster Avere vFXT in Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 359ada08f1d9df6b60fc27ca385f6003af498e17
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958598"
---
# <a name="deploy-the-vfxt-cluster"></a>Het vFXT-cluster implementeren

Voor het maken van een cluster vFXT, is de eenvoudigste manier met een netwerkcontroller cluster, wordt een virtuele machine waarvoor de vereiste scripts, sjablonen en software-infrastructuur voor het maken en beheren van het cluster vFXT is.

U een nieuwe vFXT-cluster implementeert, bevat deze stappen:

1. [Maken van de clustercontroller](#create-the-cluster-controller-vm).
1. Als u met behulp van Azure Blob-opslag, [maken van een opslageindpunt](#create-a-storage-endpoint-if-using-azure-blob) in uw virtuele netwerk.
1. [Verbinding maken met de netwerkcontroller cluster](#access-the-controller). De rest van deze stappen moeten worden uitgevoerd van het cluster netwerkcontroller-VM. 
1. [Maken van de toegangsrol](#create-the-cluster-node-access-role) voor de clusterknooppunten. Een prototype wordt geboden.
1. [Aanpassen van een script voor het maken van het cluster](#edit-the-deployment-script) voor het type vFXT cluster dat u wilt maken.
1. [Het script voor het cluster maken uitvoeren](#run-the-script).

Lees voor meer informatie over de stappen voor de implementatie van cluster en het plannen van [van plan bent uw Avere vFXT systeem](avere-vfxt-deploy-plan.md) en [implementatieoverzicht](avere-vfxt-deploy-overview.md). 

Nadat u de instructies in dit document te volgen, hebt u een virtueel netwerk, een subnet, een domeincontroller en een cluster vFXT zoals wordt weergegeven in het volgende diagram:

![diagram van vnet met optionele blob-opslag en een subnet met drie virtuele machines met het label vFXT knooppunten/vFXT cluster en één virtuele machine met het label clustercontroller gegroepeerd](media/avere-vfxt-deployment.png)

Zorg ervoor dat u deze vereisten hebt opgelost voordat u begint:  

1. [Nieuw abonnement](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Eigenaarsmachtigingen voor abonnement](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Het quotum voor het cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)

Desgewenst kunt u de rol van de cluster-knooppunt [voordat](avere-vfxt-pre-role.md) het maken van de clustercontroller, maar het is eenvoudiger te dit later doen.

## <a name="create-the-cluster-controller-vm"></a>Het cluster netwerkcontroller-VM maken

De eerste stap is het maken van de virtuele machine die wordt gemaakt en de knooppunten van het vFXT configureren. 

De clustercontroller is een Linux-VM met de Avere vFXT clustersoftware maken en de scripts die vooraf is geïnstalleerd. Het hoeft niet flink wat verwerkingstaken stroom of de opslag ruimte, zodat u goedkope opties kan kiezen. Deze virtuele machine af en toe gedurende de levensduur van het cluster vFXT gebruikt.

Er zijn twee methoden voor het maken van het cluster netwerkcontroller-VM. Een [Azure Resource Manager-sjabloon](#create-controller---arm-template) wordt geleverd [hieronder](#create-controller---arm-template) voor het vereenvoudigen van het proces, maar u kunnen ook maken de controller van de [Azure Marketplace-installatiekopie](#create-controller---azure-marketplace-image). 

U kunt een nieuwe resourcegroep als onderdeel van het maken van de controller maken.

> [!TIP]
>
> Bepalen of niet gebruik van een openbaar IP-adres op de clustercontroller. Een openbaar IP-adres biedt eenvoudiger toegang tot het cluster vFXT, maar maakt u een kleine beveiligingsrisico.
>
>  * Een openbaar IP-adres op de clustercontroller kunt u gebruiken als een host jump verbinding maken met de Avere vFXT-cluster op basis van buiten het privé-subnet.
>  * Als u een openbaar IP-adres op de controller niet ingesteld, moet u een andere jump host, een VPN-verbinding of ExpressRoute gebruiken voor toegang tot het cluster. Bijvoorbeeld, maken de controller binnen een virtueel netwerk met een VPN-verbinding geconfigureerd.
>  * Als u een domeincontroller met een openbaar IP-adres maakt, moet u de virtuele machine van de controller beveiligen met een netwerkbeveiligingsgroep. Instellen dat toegang alleen via poort 22 voor toegang tot internet.

### <a name="create-controller---resource-manager-template"></a>Controller - Resource Manager-sjabloon maken

Klik op de knop 'Implementeren naar Azure' hieronder om het cluster netwerkcontroller knooppunt vanuit de portal. Implementeren van deze sjabloon maakt u de virtuele machine die u maakt en beheert de Avere vFXT-cluster.

[![knop controller maken](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Geef de volgende informatie.

In de **BASIC** sectie:  

* **Abonnement** voor het cluster
* **Resourcegroep** voor het cluster 
* **Locatie** 

In de **instellingen** sectie:

* Al dan niet een nieuw virtueel netwerk maken

  * Als u een nieuw vnet maakt, wordt de cluster-netwerkcontroller toegewezen openbaar IP-adres zodat u kunt deze benaderen. Een netwerkbeveiligingsgroep wordt ook gemaakt voor dit vnet die binnenkomend verkeer tot alleen poort 22 beperkt.
  * Als u gebruiken van ExpressRoute of een VPN wilt-verbinding maken met de clustercontroller, deze waarde instelt op `false` en geeft u een bestaand vnet in de resterende velden. De clustercontroller gebruikt dat vnet voor netwerkcommunicatie. 

* Virtueel netwerk-resourcegroep, naam en de subnetnaam van de - Typ de namen van bestaande resources (als u een bestaand vnet) of typ de nieuwe namen als het maken van een nieuw vnet
* **Naam van de domeincontroller** -een naam voor de netwerkcontroller-VM instellen
* De standaardwaarde is de gebruikersnaam van beheerder controller- `azureuser`
* SSH-sleutel - plakken de openbare sleutel koppelen aan de gebruikersnaam van de beheerder. Lezen [maken en gebruiken van SSH-sleutels](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) als u hulp nodig hebt.

Onder **voorwaarden en bepalingen**: 

* Lees de voorwaarden van de service en klik op het selectievakje in om deze te accepteren. 

  > [!NOTE] 
  > Als u geen eigenaar van een abonnement bent, hebt u een eigenaar van de voorwaarden accepteren voor u door de vereiste stappen in [accepteren software vooraf](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 

Klik op **aankoop** wanneer u klaar bent. Na vijf of zes minuten is het knooppunt van de domeincontroller actief en werkend.

U moet naar de pagina van de uitvoer voor het verzamelen van informatie die nodig is voor het cluster. Lezen [invoer nodig is voor cluster maken](#inputs-needed-for-cluster-creation) voor meer informatie.

### <a name="create-controller---azure-marketplace-image"></a>Controller - Azure Marketplace-installatiekopie maken

De sjabloon domeincontroller vinden door te zoeken naar de Azure Marketplace voor de naam van de ``Avere``. Selecteer de **Avere vFXT voor Azure-Controller** sjabloon. 

Als u hebt nog niet gedaan, accepteer de voorwaarden en programmatische toegang inschakelen voor de Marketplace-installatiekopie door te klikken op de 'wilt implementeren via een programma?" koppeling onder de **maken** knop.

> [!NOTE] 
> Tijdens de eerste week van de algemene beschikbaarheid (en met 31 oktober - 7 November 2018), moet u de opdrachtregeloptie te gebruiken om te accepteren voor twee software-installatiekopieën in plaats van deze procedure te volgen. Volg de instructies in [accepteren software vooraf](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 

![Schermafbeelding van een koppeling naar programmatische toegang, die lager dan de knop maken is](media/avere-vfxt-deploy-programmatically.png)

Klik op de **inschakelen** knop en de instelling opslaan.

![Schermopname die laat zien muisklik om in te schakelen toegang op programmeerniveau](media/avere-vfxt-enable-program.png)

Ga terug naar de startpagina van de **Avere vFXT voor Azure-Controller** sjabloon en klikt u op **maken**. 

Vul in het eerste deelvenster of bevestig deze eenvoudige opties:

* **Abonnement**
* **Resourcegroep** (Voer een nieuwe naam als u wilt een nieuwe groep maken.)
* **Naam van virtuele machine** -naam van de domeincontroller
* **Regio**
* **Opties voor beschikbaarheid** -redundantie is niet vereist
* **Afbeelding** -installatiekopie van het Avere vFXT netwerkcontroller knooppunt
* **Grootte** : laat de standaardwaarde of kies een ander goedkope type
* **Administrator-account** -instellen hoe u zich aanmeldt bij de clustercontroller: 
  * Selecteer de gebruikersnaam en wachtwoord of openbare SSH-sleutel (aanbevolen).
  
    > [!TIP] 
    > Een SSH-sleutel is veiliger. Lezen [maken en gebruiken van SSH-sleutels](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) als u hulp nodig hebt. 
  * Geef de gebruikersnaam 
  * Plak de SSH-sleutel, of geef en Bevestig het wachtwoord
* **Regels voor binnenkomende poort** - als via een openbaar IP-adres, open poort 22 (SSH)

Klik op **volgende** schijfopties instellen:

* **OS-schijf type** -de standaardwaarde van de harde schijf is voldoende
* **Niet-beheerde schijven gebruiken** - niet nodig
* **Gegevensschijven** -niet gebruiken

Klik op **volgende** voor netwerkopties:

* **Virtueel netwerk** : Selecteer het vnet voor de controller, of voer een naam in om een nieuwe vnet te maken. Als u niet gebruiken van een openbaar IP-adres op de domeincontroller wilt, kunt u zoeken binnen een vnet met ExpressRoute of een andere toegangsmethode al ingesteld.
* **Subnet** -Selecteer een subnet binnen het vnet (optioneel). Als u een nieuw vnet maakt, kunt u een nieuw subnet op hetzelfde moment.
* **Openbaar IP-adres** -als u een openbaar IP-adres gebruiken wilt, kunt u deze hier. 
* **Netwerkbeveiligingsgroep** -laat de standaardinstelling (**Basic**) 
* **Openbare binnenkomende poorten** : als u dit besturingselement zodat toegang vanaf een SSH-verkeer via een openbaar IP-adres gebruiken. 
* **Versnelde netwerken** is niet beschikbaar voor deze virtuele machine.

Klik op **volgende** management opties instellen:

* **Diagnostische gegevens over opstarten** -wijzigen **uitschakelen**
* **Besturingssysteem Gast diagnostics** -laat uitgeschakeld
* **Opslagaccount voor diagnostische gegevens** : (optioneel) Selecteer of geef een nieuw account voor het opslaan van diagnostische gegevens.
* **Beheerde service-identiteit** -deze optie om te wijzigen **op**, waarmee een Azure AD-service-principal voor de netwerkcontroller cluster wordt gemaakt.
* **Automatisch afsluiten** -laat uitgeschakeld 

Op dit moment kunt u **revisie + maken** als u niet wilt dat exemplaar tags gebruiken. Klik anders op **volgende** twee keer om over te slaan de **Gast config** pagina en om de tags-pagina te openen. Wanneer er wordt voltooid, klikt u op **revisie + maken**. 

Nadat u uw selecties worden gevalideerd, klikt u op de **maken** knop.  

Het maken van duurt vijf of zes minuten.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Maak een opslageindpunt (als u Azure Blob)

Als u van Azure Blob-opslag voor uw back-end-gegevensopslag gebruikmaakt, moet u een service-eindpunt voor opslag in uw virtuele netwerk maken. Dit [service-eindpunt](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) houdt Azure Blob-verkeer lokale in plaats van deze routering via het internet.

1. Klik in de portal op **virtuele netwerken** aan de linkerkant.
1. Selecteer het vnet voor de controller. 
1. Klik op **Service-eindpunten** aan de linkerkant.
1. Klik op **toevoegen** aan de bovenkant.
1. De service als ``Microsoft.Storage`` en van de controller subnet kiezen.
1. Klik onderaan op **toevoegen**.

  ![Schermafbeelding van Azure portal met aantekeningen voor de stappen voor het maken van het service-eindpunt](media/avere-vfxt-service-endpoint.png)

## <a name="gather-needed-inputs"></a>Vereiste invoer verzamelen

U moet de volgende informatie om het cluster te maken. 

Als u het knooppunt van de domeincontroller met behulp van de Resource Manager-sjabloon hebt gemaakt, kunt u [ophalen van de gegevens uit de uitvoer van de sjabloon](#finding-template-output). 

Verbinding maken met de controller nodig: 

* Controller-gebruikersnaam en SSH-sleutel of het wachtwoord
* IP-adres van controller of een andere methode verbinding maken met de VM-controller

Nodig voor het maken van clusters: 

* Resourcegroepnaam
* Azure-locatie 
* Naam van virtueel netwerk
* Subnetnaam
* Knooppunt-clusterrolnaam op
* Naam van het opslagaccount als het maken van een Blob-container

Ook vindt u ontbrekende informatie door te bladeren naar de pagina controller VM-gegevens. Bijvoorbeeld, klikt u op **alle resources** en zoek de naam van de domeincontroller, en klik vervolgens op de naam van de domeincontroller om de details te bekijken.

### <a name="finding-template-output"></a>Uitvoer van de sjabloon zoeken

Als u wilt deze informatie van de Resource Manager-uitvoer van de sjabloon vinden, als volgt:

1. Ga naar de resourcegroep voor uw cluster-netwerkcontroller.

1. Aan de linkerkant, klikt u op **implementaties**, en vervolgens **Microsoft.Template**.

   ![Resource group-portalpagina met implementaties die zijn geselecteerd op de linker- en Microsoft.Template weergeven in een tabel onder de naam van de implementatie](media/avere-vfxt-deployment-template.png)

1. Aan de linkerkant, klikt u op **uitvoer**. Kopieer de waarden in elk van de velden. 

   ![pagina met SSHSTRING, RESOURCE_GROUP, locatie, netwerk, SUBNET en wordt weergegeven in de velden aan de rechterkant van de labels SUBNET_ID uitvoer](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>Toegang tot de controller

U doet de rest van de implementatiestappen, moet u verbinding maken met de clustercontroller.

1. De methode voor het verbinding maken met uw clustercontroller is afhankelijk van uw installatie.

   * Als de controller een openbaar IP-adres, SSH aan van de controller-IP-adres als de gebruikersnaam van de beheerder heeft instellen (bijvoorbeeld ``ssh azureuser@40.117.136.91``).
   * Als de domeincontroller heeft geen een openbaar IP-adres, gebruikt u een [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) of een VPN-verbinding met uw vnet.

1. Na het aanmelden bij de domeincontroller, verifiëren door te voeren `az login`. De verificatiecode die is opgegeven in de shell kopiëren en vervolgens een webbrowser gebruiken om te laden [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin) en verifiëren met het Microsoft-systeem. Ga terug naar de shell om bevestiging.

   ![Vanaf de opdrachtregel-uitvoer van de 'AZ login'-opdracht voor het weergeven van de browser-code voor koppeling en verificatie](media/avere-vfxt-azlogin.png)

1. Uw abonnement toevoegen door te voeren van deze opdracht met behulp van uw abonnements-ID:  ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>De functie cluster knooppunt toegang maken

> [!NOTE] 
> Als u geen eigenaar van een abonnement bent en de rol niet al is gemaakt, hebt u de eigenaar van een abonnement als volgt te werk- of gebruik de procedure in [Avere vFXT cluster runtime toegangsrol zonder een domeincontroller maken](avere-vfxt-pre-role.md).

[Op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) biedt de clusterknooppunten vFXT autorisatie noodzakelijke taken uit te voeren.  

Als onderdeel van normale vFXT bewerking van het cluster, individuele vFXT knooppunten nodig hebt voor handelingen zoals lezen van de Azure-resource-eigenschappen, opslag te beheren en de andere knooppunten netwerkinterface-instellingen beheren. 

1. Open op de controller de ``/avere-cluster.json`` bestand in een teksteditor.

   ![console met een opdracht lijst en klikt u vervolgens een "vi /avere-cluster.json"](media/avere-vfxt-open-role.png)

1. Bewerk het bestand voor het opnemen van uw abonnements-ID en de regel boven het verwijderen. Sla het bestand op als ``avere-cluster.json``.

   ![Console teksteditor met de abonnements-ID en de 'verwijdert u deze regel' geselecteerd voor verwijdering](media/avere-vfxt-edit-role.png)

1. Gebruik deze opdracht om de rol te maken:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

U geeft de naam van de rol aan het script voor het cluster maken in de volgende stap. 

## <a name="create-nodes-and-configure-the-cluster"></a>Knooppunten maken en configureren van het cluster

Als u wilt de Avere vFXT-cluster maakt, een van de voorbeeldscripts opgenomen op de controller bewerken en voer dit er. Voorbeelden van scripts bevinden zich in de hoofdmap (`/`) op de clustercontroller.

* Als u maken van een Blob-container wilt wilt gebruiken als de Avere-vFXT back-end-opslagsysteem, gebruikt u de ``create-cloudbacked-cluster`` script.

* Als u opslag later toevoegt, gebruikt u de ``create-minimal-cluster`` script.

> [!TIP]
> Prototype scripts voor het toevoegen van knooppunten en vernietigen van het cluster vFXT ook zijn opgenomen in de `/` map van het cluster netwerkcontroller-VM.

### <a name="edit-the-deployment-script"></a>Bewerk het script voor implementatie

Open het voorbeeldscript in een teksteditor. Het is raadzaam om op te slaan van het aangepaste script met een andere naam om te voorkomen dat de oorspronkelijke voorbeeld overschrijven.

Geef waarden voor deze scriptvariabelen.

* Resourcegroepnaam

  * Als u netwerk- of -onderdelen die zich in verschillende resourcegroepen bevinden, verwijder de opmerkingen in de variabelen en ook deze namen opgeven. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Locatienaam
* Naam van virtueel netwerk
* Subnetnaam
* Azure AD-runtime rolnaam - als u het voorbeeld in gevolgd [maken van de functie cluster knooppunt toegang](#create-the-cluster-node-access-role), gebruikt u ``avere-cluster``. 
* Naam van opslagaccount (als het maken van een nieuwe blobcontainer)
* De naam van de cluster - u geen twee clusters met vFXT met dezelfde naam in dezelfde resourcegroep bevinden. 
* Beheerderswachtwoord - kiest u een beveiligd-wachtwoordverificatie voor bewaking en beheer van het cluster. Dit wachtwoord is toegewezen aan de gebruiker ``admin``. 
* Knooppunt exemplaartype - Zie [vFXT knooppuntgrootten](avere-vfxt-deploy-plan.md#vfxt-node-sizes) voor meer informatie
* Cachegrootte van knooppunt - Zie [vFXT knooppuntgrootten](avere-vfxt-deploy-plan.md#vfxt-node-sizes) voor meer informatie

Sla het bestand en afsluiten.

### <a name="run-the-script"></a>Het script uitvoeren
Voer het script door de bestandsnaam die u hebt gemaakt. (Voorbeeld: `./create-cloudbacked-cluster-west1`)  

Houd rekening met het uitvoeren van deze opdracht in een [terminal multiplexer](http://linuxcommand.org/lc3_adv_termmux.php) zoals `screen` of `tmux` in het geval de verbinding wordt verbroken.  
De uitvoer wordt ook vastgelegd in `~/vfxt.log`.

Wanneer het script is voltooid, kopieert u het beheer van IP-adres, die nodig voor het Clusterbeheer is.

![Vanaf de opdrachtregel-uitvoer van het script voor het weergeven van het beheer van IP-adres aan het einde](media/avere-vfxt-mgmt-ip.png)

### <a name="next-step"></a>Volgende stap

Nu dat het cluster actief is en u weet dat het beheer van IP-adres, kunt u [verbinding maken met het cluster-configuratieprogramma](avere-vfxt-cluster-gui.md) ondersteuning inschakelen en het toevoegen van opslag, indien nodig.
