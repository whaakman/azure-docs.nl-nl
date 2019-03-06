---
title: Avere vFXT implementeren voor Azure
description: Stappen voor het implementeren van het cluster Avere vFXT in Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 7dbfc39075bb42b1ec13823849eb769e117ddd4a
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57409683"
---
# <a name="deploy-the-vfxt-cluster"></a>Het vFXT-cluster implementeren

Deze procedure begeleidt u stapsgewijs door de implementatiewizard beschikbaar vanuit de Azure Marketplace. De wizard wordt automatisch het cluster geïmplementeerd met behulp van een Azure Resource Manager-sjabloon. Nadat u de parameters in het formulier invoeren en klikt u op **maken**, Azure is automatisch voltooid voor deze stappen:

* Hiermee maakt u de clustercontroller, dit is een eenvoudige virtuele machine die de software die nodig zijn om te implementeren en beheren van het cluster bevat.
* Stelt u de resourcegroep en virtuele netwerkinfrastructuur, waaronder het maken van nieuwe elementen.
* Knooppunt VM's van het cluster maakt en configureert u deze als het cluster Avere.
* Als dat wordt gevraagd, maakt u een nieuwe Azure Blob-container en configureert deze als een cluster core filer.

Nadat u de instructies in dit document te volgen, hebt u een virtueel netwerk, een subnet, een domeincontroller en een cluster vFXT zoals wordt weergegeven in het volgende diagram. In dit diagram ziet u de optionele Azure Blob core filer, met inbegrip van een nieuwe Blob storage-container (in een nieuw opslagaccount, niet weergegeven) en een service-eindpunt voor Microsoft storage binnen het subnet. 

![diagram waarin drie concentrische rechthoeken met Avere onderdelen van het cluster. De buitenste rechthoek heet 'Resource group' en bevat een hexagoon met het label 'Blob-opslag (optioneel)'. De volgende rechthoek in het label ' virtueel netwerk: 10.0.0.0/16' en bevat geen unieke onderdelen. De binnenste rechthoek heet 'Subnet:10.0.0.0/24' en een virtuele machine met het label 'clustercontroller', een stapel drie VM's met het label vFXT knooppunten (vFXT cluster) en een hexagoon met het label 'Service-eindpunt' bevat. Er is een pijl met verbinding van het service-eindpunt (dat zich binnen het subnet) en de blob-opslag (dit is buiten het subnet en een vnet, in de resourcegroep). De pijl wordt doorgegeven via het subnet en de grenzen van het virtuele netwerk.](media/avere-vfxt-deployment.png)  

Zorg ervoor dat u deze vereisten hebt opgelost voordat u met behulp van de sjabloon maken:  

1. [Nieuw abonnement](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Eigenaarsmachtigingen voor abonnement](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Het quotum voor het cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Aangepaste rollen](avere-vfxt-prereqs.md#create-access-roles) -moet u een rol voor het beheer van toegang op basis van rollen om toe te wijzen aan de clusterknooppunten. U hebt de mogelijkheid ook een aangepaste toegang-rol voor de netwerkcontroller cluster maken, maar de meeste gebruikers de rol eigenaar standaard, waardoor de bevoegdheden van de netwerkcontroller overeenkomt op een resourcegroepeigenaar zal duren. Lezen [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md#owner) voor meer informatie.
1. [Storage service-eindpunt (indien nodig)](avere-vfxt-prereqs.md#create-a-storage-service-endpoint-in-your-virtual-network-if-needed) - vereist voor implementeert met behulp van een bestaand virtueel netwerk en het maken van blob-opslag

Lees voor meer informatie over de stappen voor de implementatie van cluster en het plannen van [van plan bent uw Avere vFXT systeem](avere-vfxt-deploy-plan.md) en [implementatieoverzicht](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>De vFXT Avere voor Azure maken

Toegang tot de sjabloon maken in Azure portal door te zoeken naar Avere en selecteren ' Avere vFXT ARM implementatie ". 

![Stukjes browservenster met de Azure-portal met brood 'Nieuw > Marketplace > Alles'. In de alles pagina, het zoekveld heeft de term 'avere' en het tweede resultaat ' Avere vFXT ARM implementatie ' wordt beschreven in red om deze te markeren.](media/avere-vfxt-template-choose.png)

Na het lezen van gegevens op de pagina Avere vFXT ARM-implementatie, klikt u op **maken** om te beginnen. 

![Azure marketplace met de eerste pagina van de implementatie van sjabloon weergeven](media/avere-vfxt-deploy-first.png)

De sjabloon is onderverdeeld in vier stappen - pagina's van twee verzamelen van informatie, plus de validatie en de bevestiging stappen. 

* Pagina is gericht op de instellingen voor het cluster netwerkcontroller-VM. 
* Pagina 2 verzamelt parameters voor het maken van het cluster en de bijbehorende resources, zoals subnetten en opslag. 
* Pagina drie ziet u de instellingen en valideert de configuratie. 
* Pagina vier wordt uitgelegd software voorwaarden en bepalingen, en kunt u het proces voor het maken van cluster starten. 

## <a name="page-one-parameters---cluster-controller-information"></a>Pagina één parameters - controller clustergegevens

De eerste pagina van de sjabloon voor de implementatie verzamelt informatie over de clustercontroller. 

![Eerste pagina van de sjabloon voor de implementatie](media/avere-vfxt-deploy-1.png)

Vul in de volgende informatie:

* **De naam van de cluster-netwerkcontroller** -de naam voor het cluster netwerkcontroller-VM instellen.

* **Gebruikersnaam van de controller** -vult u de gebruikersnaam van de hoofdmap voor de virtuele machine van de clustercontroller. 

* **Verificatietype** -Kies wachtwoord of SSH openbare sleutelverificatie om verbinding te maken met de domeincontroller. De methode SSH openbare sleutel wordt aanbevolen. Lees [maken en gebruiken van SSH-sleutels](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) als u hulp nodig hebt.

* **Wachtwoord** of **openbare SSH-sleutel** -afhankelijk van het verificatietype dat u hebt geselecteerd, moet u een openbare RSA-sleutel of een wachtwoord in de volgende velden opgeven. Deze referentie gebruikt met de eerder opgegeven gebruikersnaam.

* **Avere cluster rol-ID maken** -dit veld gebruiken om op te geven van de functie voor het beheer van toegang voor de clustercontroller. De standaardwaarde is de ingebouwde rol [eigenaar](../role-based-access-control/built-in-roles.md#owner). Eigenaarsbevoegdheden voor de clustercontroller zijn beperkt tot de resourcegroep van het cluster. 

  U moet de unieke id die overeenkomt met de rol. Voor de standaardwaarde (eigenaar) is de GUID 8e3af657-a8ff-443c-a75c-2fe8c4bcb635. Als u de GUID voor een aangepaste rol zoekt, gebruikt u deze opdracht: 

  ```azurecli
  az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
  ```

* **Abonnement** -Selecteer het abonnement voor de vFXT Avere. 

* **Resourcegroep** : Selecteer een bestaande lege resourcegroep voor de Avere vFXT cluster, of klik op 'Nieuw' en voer de naam van een nieuwe resourcegroep. 

* **Locatie** -Selecteer de Azure-locatie voor uw cluster en de resources.

Klik op **OK** wanneer u klaar bent. 

> [!NOTE]
> Als u wilt dat de cluster-domeincontroller naar een openbare IP-adres hebben, maakt u een nieuw virtueel netwerk voor het cluster in plaats van een bestaand virtueel netwerk selecteren. Deze instelling is op de pagina 2.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Pagina twee parameters - vFXT clustergegevens

De tweede pagina van de sjabloon voor de implementatie kunt u de clustergrootte, knooppunttype, cachegrootte en parameters van de opslag, naast andere instellingen instellen. 

![Tweede pagina van de sjabloon voor de implementatie](media/avere-vfxt-deploy-2.png)

* **Aantal clusterknooppunten Avere vFXT** -kiest u het aantal knooppunten te gebruiken in het cluster. De minimumwaarde is drie knooppunten en de maximumwaarde is 12. 

* **Wachtwoord voor het beheer van cluster** -maken van het wachtwoord voor Clusterbeheer. Dit wachtwoord wordt gebruikt met de gebruikersnaam ```admin``` zich aanmeldt bij het cluster van het Configuratiescherm om te controleren van het cluster en om instellingen te configureren.

* **De clusterfunctie operations Avere** -Geef de naam van de access control-rol voor de clusterknooppunten. Dit is een aangepaste rol die is gemaakt als een vereiste stap. 

  Het voorbeeld dat wordt beschreven in [maken van de functie cluster knooppunt toegang](avere-vfxt-prereqs.md#create-the-cluster-node-access-role) slaat het bestand als ```avere-operator.json``` en de bijbehorende naam is ```avere-operator```.

* **De naam van de cluster vFXT Avere** -Geef het cluster een unieke naam. 

* **Grootte** -in deze sectie ziet u het VM-type dat wordt gebruikt voor de clusterknooppunten. Hoewel er slechts één aanbevolen optie, de **grootte wijzigen** koppeling opent u een tabel met informatie over dit exemplaartype en een koppeling naar een prijscalculator.  

* **Cachegrootte per knooppunt** -de cluster-cache is verdeeld over de clusterknooppunten, dus de totale cachegrootte op uw Avere vFXT cluster zullen de cachegrootte per knooppunt vermenigvuldigd met het aantal knooppunten. 

  De aanbevolen configuratie is het gebruik van 4 TB per knooppunt voor Standard_E32s_v3 knooppunten.

* **Virtueel netwerk** - definiëren een nieuw vnet voor het cluster bevatten, of Selecteer een bestaand vnet die voldoet aan de vereisten die worden beschreven [van plan bent uw Avere vFXT systeem](avere-vfxt-deploy-plan.md#resource-group-and-network-infrastructure). 

  > [!NOTE]
  > Als u een nieuw vnet maakt, wordt de clustercontroller een openbaar IP-adres hebben zodat u toegang hebben tot de nieuwe particulier netwerk. Als u een bestaand vnet kiest, wordt de cluster-netwerkcontroller geconfigureerd zonder een openbaar IP-adres. 
  > 
  > Een openbaar zichtbaar IP-adres op de clustercontroller eenvoudiger toegang tot het cluster vFXT biedt, maar maakt u een kleine beveiligingsrisico. 
  >  * Een openbaar IP-adres op de clustercontroller kunt u gebruiken als een host jump verbinding maken met de Avere vFXT-cluster op basis van buiten het privé-subnet.
  >  * Als u een openbaar IP-adres op de controller niet ingesteld, moet u een andere jump host, een VPN-verbinding of ExpressRoute gebruiken voor toegang tot het cluster. Bijvoorbeeld, maken de controller binnen een virtueel netwerk waarvoor al een VPN-verbinding geconfigureerd.
  >  * Als u een domeincontroller met een openbaar IP-adres maakt, moet u de virtuele machine van de controller beveiligen met een netwerkbeveiligingsgroep. Standaard de vFXT Avere voor Azure-implementatie wordt gemaakt van een netwerkbeveiligingsgroep en binnenkomende toegang beperkt tot alleen poort 22 voor domeincontrollers met openbare IP-adressen. Kunt u het systeem verder beschermen met vergrendelen omlaag toegang tot het bereik van IP-bronadressen - dat wil zeggen, alleen verbindingen toestaan vanaf computers die u wilt gebruiken voor toegang tot het cluster.

  De sjabloon implementeren configureert ook de nieuwe vnet met een storage-service-eindpunt voor Azure Blob storage en netwerktoegangsbeheer vergrendeld voor alleen IP-adressen uit het subnet van het cluster. 

* **Subnet** : Kies een subnet van uw bestaande virtuele netwerk of een nieuwe maken. 

* **Maken en gebruiken van blob-opslag** -Kies **waar** naar een nieuwe Azure Blob-container maken en configureren als back-end-opslag voor het nieuwe Avere vFXT cluster. Deze optie maakt ook een nieuw opslagaccount in dezelfde resourcegroep bevinden als het cluster en een Microsoft storage service-eindpunt binnen het subnet van het cluster. 
  
  Als u een bestaand virtueel netwerk opgeeft, moet een storage-service-eindpunt hebben voordat u het cluster maakt. (Voor meer informatie lezen [van plan bent uw Avere vFXT systeem](avere-vfxt-deploy-plan.md).)

  Stel dit veld in **false** als u niet wilt dat een nieuwe container maken. In dit geval moet u koppelt en opslag configureren nadat het cluster is gemaakt. Lezen [opslag configureren](avere-vfxt-add-storage.md) voor instructies. 

* **(Nieuw) Storage-account** : als het maken van een nieuwe Azure Blob-container, voer een naam voor het nieuwe opslagaccount. 

## <a name="validation-and-purchase"></a>Validatie en -aanschaf

Pagina drie bevat een overzicht van de configuratie en valideert de parameters. Nadat de validatie is geslaagd, klikt u op de **OK** om door te gaan. 

![Derde pagina van de sjabloon voor de implementatie - validatie](media/avere-vfxt-deploy-3.png)

Op de vierde pagina, klikt u op de **maken** knop aan de voorwaarden accepteren en de vFXT Avere voor Azure-cluster maken. 

![Vierde pagina van de sjabloon voor de implementatie - voorwaarden en bepalingen, knop maken](media/avere-vfxt-deploy-4.png)

Implementatie van het cluster duurt 15-20 minuten.

## <a name="gather-template-output"></a>Verzamel de uitvoer van de sjabloon

Wanneer de Avere vFXT sjabloon klaar is met het cluster te maken, voert deze enkele belangrijke informatie over het nieuwe cluster. 

> [!TIP]
> Zorg ervoor dat u het beheer van IP-adres van de uitvoer van de sjabloon te kopiëren. U moet dit adres om het cluster te beheren.

Als u wilt deze informatie vinden, volgt u deze procedure:

1. Ga naar de resourcegroep voor uw cluster-netwerkcontroller.

1. Aan de linkerkant, klikt u op **implementaties**, en vervolgens **avere.vfxt-microsoft-sjabloon**.

   ![Portal-pagina van de resourcegroep met implementaties die zijn geselecteerd op de linker- en microsoft-avere.vfxt-sjablonen worden weergegeven in een tabel onder de naam van de implementatie](media/avere-vfxt-outputs-deployments.png)

1. Aan de linkerkant, klikt u op **uitvoer**. Kopieer de waarden in elk van de velden. 

   ![voert de pagina tonen SSHSTRING, RESOURCE_GROUP, locatie, NETWORK_RESOURCE_GROUP, netwerk, SUBNET, SUBNET_ID, VSERVER_IPs en MGMT_IP waarden in de velden aan de rechterkant van de labels](media/avere-vfxt-outputs-values.png)

## <a name="next-step"></a>Volgende stap

Nu dat het cluster actief is en u weet dat het beheer van IP-adres, kunt u [verbinding maken met het cluster-configuratieprogramma](avere-vfxt-cluster-gui.md) als ondersteuning wilt inschakelen, opslag toevoegen als nodig hebt en andere clusterinstellingen voor aanpassen.
