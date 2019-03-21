---
title: Azure Databricks implementeren in uw virtuele netwerk (Preview)
description: Dit artikel wordt beschreven hoe u Azure Databricks implementeert in uw virtuele netwerk, ook wel bekend als VNet-injectie.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: c29d2e1df0979481c0c8a1e1f2cd4d22b013212a
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227628"
---
# <a name="deploy-azure-databricks-in-your-virtual-network-preview"></a>Azure Databricks implementeren in uw virtuele netwerk (Preview)

De standaardimplementatie van Azure Databricks is een volledig beheerde service in Azure: alle gegevens vlak-resources, met inbegrip van een virtueel netwerk (VNet), worden geïmplementeerd in een vergrendelde resourcegroep. Als u de aanpassing van het netwerk vereist, maar kunt u Azure Databricks resources implementeren in uw eigen virtuele netwerk (ook wel VNet injectie), waarop u kunt:

* Azure Databricks verbinden met andere Azure-services (zoals Azure Storage) op een veiliger manier met behulp van service-eindpunten.
* Verbinding maken met on-premises gegevens bronnen voor gebruik met Azure Databricks, profiteren van de gebruiker gedefinieerde routes.
* Azure Databricks verbinden met een virtueel netwerkapparaat op al het uitgaande verkeer inspecteren en actie ondernemen volgens toestaan en weigeren van regels.
* Azure Databricks voor het gebruik van aangepaste DNS configureren.
* (NSG) regels voor netwerkbeveiligingsgroepen om op te geven van uitgaand verkeer beperkingen configureren.
* Azure Databricks-clusters in uw bestaande virtuele netwerk implementeren.

Azure Databricks-resources implementeren op uw eigen virtuele netwerk ook kunt u profiteren van flexibele CIDR-bereiken (overal tussen /16- / 24 voor het virtuele netwerk en tussen /18- / 26 voor de subnetten).

  > [!NOTE]
  > U kunt het virtuele netwerk voor een bestaande werkruimte niet vervangen. Als de huidige werkruimte kan niet geschikt voor het vereiste aantal actieve clusterknooppunten, moet u een andere werkruimte maken in een grotere virtuele netwerk. Ga als volgt [deze gedetailleerde migratiestappen](howto-regional-disaster-recovery.md#detailed-migration-steps) resources (laptops, configuraties van clusters, taken) van de oude kopiëren naar nieuwe werkruimte.

## <a name="virtual-network-requirements"></a>Vereisten voor virtueel netwerk

U kunt de interface Azure Databricks-werkruimte-implementatie in Azure portal gebruiken automatisch een bestaand virtueel netwerk configureren met de vereiste subnetten, netwerkbeveiligingsgroep en instellingen voor opname in de whitelist of kunt u Azure Resource Manager sjablonen voor uw virtuele netwerk configureren en implementeren van uw werkruimte.

Het virtuele netwerk dat u uw Azure Databricks-werkruimte te implementeren, moet voldoen aan de volgende vereisten:

### <a name="location"></a>Locatie

Het virtuele netwerk moet zich bevinden op dezelfde locatie bevinden als de Azure Databricks-werkruimte.

### <a name="subnets"></a>Subnetten

Het virtuele netwerk, twee subnetten die zijn toegewezen aan Azure Databricks moet bevatten:

   1. Een privé-subnet met een geconfigureerde netwerkbeveiligingsgroep waarmee de cluster-interne communicatie

   2. Een openbaar subnet met een geconfigureerde netwerkbeveiligingsgroep waarmee communicatie met de controlelaag van Azure Databricks.

### <a name="address-space"></a>Adresruimte

Een CIDR-blok tussen /16-/ 24 is voor het virtuele netwerk en een CIDR-blok tussen /18-/26 voor de persoonlijke en openbare subnetten.

### <a name="whitelisting"></a>Goedkeuring

Alle binnenkomende en uitgaande verkeer tussen de subnets en de Azure Databricks-controlelaag moet in de whitelist opgenomen.

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie wordt beschreven hoe u een Azure Databricks-werkruimte maken in Azure portal en deze implementeren in uw eigen bestaande virtuele netwerk. Azure Databricks werkt bij het virtuele netwerk met twee nieuwe subnetten en netwerkbeveiligingsgroepen met behulp van de CIDR-bereiken die u hebt opgegeven, accounttoewijzing inkomende en uitgaande subnetverkeer, en implementeert u de werkruimte op het virtueel netwerk is bijgewerkt.

## <a name="prerequisites"></a>Vereisten

Een virtueel netwerk waarop u de Azure Databricks-werkruimte wilt implementeren, moet u hebben. U kunt een bestaand virtueel netwerk gebruiken of een nieuwe maken, maar het virtuele netwerk moet zich in dezelfde regio als de Azure Databricks-werkruimte die u van plan bent om te maken. Een CIDR-bereik tussen /16 /24 is vereist voor het virtuele netwerk.

  > [!Warning]
  > Een werkruimte met een kleinere virtuele netwerk – dat wil zeggen, een lagere CIDR-bereik: kan worden uitgevoerd buiten het IP-adressen (netwerk ruimte) sneller dan een werkruimte met een grotere virtueel netwerk. Bijvoorbeeld, een werkruimte met een/24 virtueel netwerk en /26 subnetten hebben een maximum van 64 knooppunten tegelijk, actief, terwijl een werkruimte met een /20 virtueel netwerk en /22 subnetten kunnen werken met een maximum van 1024-knooppunten.

  De subnetten van het wordt automatisch gemaakt wanneer u uw werkruimte configureert, en u de mogelijkheid hebt te geven van de CIDR-bereik voor de subnetten tijdens de configuratie.

## <a name="configure-the-virtual-network"></a>Het virtuele netwerk configureren

1. Selecteer in de Azure portal, **+ een resource maken > Analytics > Azure Databricks** opent het dialoogvenster van de service Azure Databricks.

2. Volg de configuratiestappen die worden beschreven in stap 2: Een Azure Databricks-werkruimte maken in de introductiehandleiding en selecteert u de implementatie van Azure Databricks-werkruimte in de optie voor uw Virtueelnetwerk.

   ![Azure Databricks-service maken](./media/vnet-injection/create-databricks-service.png)

3. Selecteer het virtuele netwerk dat u wilt gebruiken.

   ![Opties voor virtueel netwerk](./media/vnet-injection/select-vnet.png)

4. Geef de CIDR-bereiken in een blok tussen /18-/26 voor twee subnetten gemaakt, speciaal voor Azure Databricks:

   * Een openbaar subnet wordt gemaakt met een gekoppelde netwerkbeveiligingsgroep waarmee communicatie met de controlelaag van Azure Databricks.
   * Een privé-subnet wordt gemaakt met een gekoppelde netwerkbeveiligingsgroep waarmee de cluster-interne communicatie.

5. Klik op **maken** naar de Azure Databricks-werkruimte in het virtuele netwerk implementeert.

## <a name="advanced-resource-manager-configurations"></a>Configuraties voor geavanceerde resource manager

Als u meer controle over de configuratie van het virtuele netwerk wilt – bijvoorbeeld, u wilt gebruiken van bestaande subnetten, bestaande netwerkbeveiligingsgroepen te gebruiken of maken van uw eigen beveiligingsregels – kunt u de volgende Azure Resource Manager-sjablonen in plaats van de Portal virtueel netwerk configureren en werkruimte-implementatie.

### <a name="all-in-one"></a>Alles in één

Gebruik voor het maken van een virtueel netwerk, netwerkbeveiligingsgroepen en Azure Databricks-werkruimte in een de [All-in-one-sjabloon voor het VNet geïnjecteerd Databricks-werkruimten](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection/).

Wanneer u deze sjabloon gebruikt, hoeft u niet alle handmatige zwarte lijst plaatsen van subnetverkeer doen.

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Voor het maken van netwerk-beveiligingsgroepen met de vereiste regels voor een bestaand virtueel netwerk gebruikt de [netwerk groep sjabloon voor Databricks VNet injectie](https://azure.microsoft.com/resources/templates/101-databricks-nsg-for-vnet-injection).

Wanneer u deze sjabloon gebruikt, hoeft u niet alle handmatige zwarte lijst plaatsen van subnetverkeer doen.

### <a name="virtual-network"></a>Virtueel netwerk

Gebruik voor het maken van een virtueel netwerk met de juiste openbare en particuliere subnetten de [sjabloon voor virtueel netwerk voor Databricks VNet injectie](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection).

Als u deze sjabloon gebruiken zonder de sjabloon netwerk groepen ook gebruik te maken, moet u handmatig in de whitelist toevoegen aan de netwerk-beveiligingsgroepen die u met het virtuele netwerk gebruikt.

### <a name="azure-databricks-workspace"></a>Azure Databricks-werkruimte

Voor het implementeren van een Azure Databricks-werkruimte aan een bestaand virtueel netwerk met openbare en particuliere subnetten en correct geconfigureerde netwerkbeveiligingsgroepen al ingesteld, gebruikt u de [werkruimtesjabloon voor Databricks VNet injectie](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection).

Als u deze sjabloon gebruiken zonder de sjabloon netwerk groepen ook gebruik te maken, moet u handmatig in de whitelist toevoegen aan de netwerk-beveiligingsgroepen die u met het virtuele netwerk gebruikt.

## <a name="whitelisting-subnet-traffic"></a>Opname in de Whitelist subnetverkeer

Als u geen gebruik maakt de [Azure-portal](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-portal) of [Azure Resource Manager-sjablonen](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-advanced) voor het maken van uw netwerk-beveiligingsgroepen, moet u handmatig het verkeer naar de volgende lijst met toegestane adressen in uw subnetten.

|Richting|Protocol|Bron|Bronpoort|Doel|Doelpoort|
|---------|--------|------|-----------|-----------|----------------|
|Inkomend|\*|VirtualNetwork|\*|\*|\*|
|Inkomend|\*|Besturingselement vlak NAT IP|\*|\*|22|
|Inkomend|\*|Besturingselement vlak NAT IP|\*|\*|5557|
|Uitgaand|\*|\*|\*|IP-Web-App|\*|
|Uitgaand|\*|\*|\*|SQL (service tag)|\*|
|Uitgaand|\*|\*|\*|Opslag (servicetag)|\*|
|Uitgaand|\*|\*|\*|VirtualNetwork|\*|

Lijst met toegestane adressen subnetverkeer dat gebruikmaakt van de volgende IP-adressen. Voor SQL (metastore) en Storage (opslag van artefacten en logboekbestanden), moet u de Sql- en opslagcapaciteit [servicetags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

|Azure Databricks-regio|Service|Openbare IP|
|-----------------------|-------|---------|
|US - oost|Besturingselement vlak NAT </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|US - oost 2|Besturingselement vlak NAT </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|US - noord-centraal|Besturingselement vlak NAT </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|US - centraal|Besturingselement vlak NAT </br></br>Webapp|23.101.152.95/32 </br></br>40.70.58.221/32|
|US - zuid-centraal|Besturingselement vlak NAT </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|US - west|Besturingselement vlak NAT </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|US - west 2|Besturingselement vlak NAT </br></br>Webapp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Canada - midden|Besturingselement vlak NAT </br></br>Webapp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Canada - oost|Besturingselement vlak NAT </br></br>Webapp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Verenigd Koninkrijk West|Besturingselement vlak NAT </br></br>Webapp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Verenigd Koninkrijk Zuid|Besturingselement vlak NAT </br></br>Webapp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Europa -west|Besturingselement vlak NAT </br></br>Webapp|23.100.0.135/32 </br></br>52.232.19.246/32|
|Europa - noord|Besturingselement vlak NAT </br></br>Webapp|23.100.0.135/32 </br></br>52.232.19.246/32|
|India - centraal|Besturingselement vlak NAT </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|India - zuid|Besturingselement vlak NAT </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|India - west|Besturingselement vlak NAT </br></br>Webapp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Azië - zuidoost|Besturingselement vlak NAT </br></br>Webapp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Azië - oost|Besturingselement vlak NAT </br></br>Webapp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Australië - oost|Besturingselement vlak NAT </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Australië - zuidoost|Besturingselement vlak NAT </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Australië - centraal|Besturingselement vlak NAT </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Australië - centraal 2|Besturingselement vlak NAT </br></br>Webapp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Japan - oost|Besturingselement vlak NAT </br></br>Webapp|13.78.19.235/32 </br></br>52.246.160.72/32|
|Japan - west|Besturingselement vlak NAT </br></br>Webapp|13.78.19.235/32 </br></br>52.246.160.72/32|

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="workspace-launch-errors"></a>Werkruimte starten fouten

Starten van een werkruimte in een aangepaste virtueel netwerk mislukt op de Azure Databricks aanmeldingsscherm met de volgende fout: **"We hebben aangetroffen bij het maken van uw werkruimte. Zorg ervoor dat de configuratie van de aangepaste juist is en probeer het opnieuw."**

Deze fout wordt veroorzaakt door een netwerkconfiguratie voldoet niet aan vereisten. Bevestig dat u de instructies in dit onderwerp hebt gevolgd bij het maken van de werkruimte.

### <a name="cluster-creation-errors"></a>Fouten bij het cluster maken

**De exemplaren is niet bereikbaar: Resources zijn niet bereikbaar is via SSH.**

Mogelijke oorzaak: verkeer vanuit de besturingselement vlak voor de werknemers wordt geblokkeerd. Oplossen door ervoor te zorgen dat aan de regels voor binnenkomende beveiliging voldoen. Als u naar een bestaand virtueel netwerk verbonden met uw on-premises netwerk implementeert, controleert u uw instellingen met behulp van de gegevens in uw Azure Databricks-werkruimte met uw On-Premises netwerk verbindt.

**Fout bij het onverwachte starten: Er is een onverwachte fout opgetreden tijdens het instellen van het cluster. Probeer het opnieuw en neem contact op met Azure Databricks als het probleem zich blijft voordoen. Interne fout: Een time-out opgetreden bij het plaatsen van knooppunt.**

Mogelijke oorzaak: verkeer van werknemers tot Azure Storage-eindpunten wordt geblokkeerd. Oplossen door ervoor te zorgen dat uitgaande beveiligingsregels voldoen aan vereisten. Als u aangepaste DNS-servers gebruikt, moet u ook de status van de DNS-servers in uw virtuele netwerk controleren.

**Cloud-Provider starten is mislukt: Er is een cloud-provider-fout opgetreden tijdens het instellen van het cluster. Zie de handleiding Azure Databricks voor meer informatie. Azure-foutcode: AuthorizationFailed/InvalidResourceReference.**

Mogelijke oorzaak: het virtuele netwerk of subnetten bestaan niet meer. Zorg ervoor dat het virtuele netwerk en subnetten bestaan.

**Cluster beëindigd. Reden: Fout bij het opstarten van de Spark: Spark kan niet starten in de tijd. Dit probleem kan worden veroorzaakt door een niet-functionerende Hive-metastore, ongeldig Spark-configuraties of functioneert niet goed init-scripts. Raadpleeg de logboeken van de Spark-stuurprogramma om op te lossen dit probleem en neem contact op met Databricks als het probleem zich blijft voordoen. Interne fout: Spark kan niet starten: Stuurprogramma is niet gestart in de tijd.**

Mogelijke oorzaak: Container kan niet communiceren met de hosting-exemplaar of DBFS storage-account. Oplossen door een aangepaste route naar de subnetten voor het opslagaccount DBFS toe te voegen met de volgende hop Internet wordt.

### <a name="notebook-command-errors"></a>Laptop opdracht fouten

**Opdracht is vastgelopen**

Mogelijke oorzaak: communicatie met de worker-naar-worker is geblokkeerd. Oplossen door ervoor te zorgen dat de beveiligingsregels voor binnenkomend verkeer te voldoen aan de vereisten.

**Laptop-werkstroom mislukt met de uitzondering: com.databricks.WorkflowException: org.apache.http.conn.ConnectTimeoutException**

Mogelijke oorzaak: het verkeer van werknemers naar Azure Databricks-Web-App is geblokkeerd. Oplossen door ervoor te zorgen dat de uitgaande beveiligingsregels voldoen aan vereisten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevens uitpakken, transformeren en laden met Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
