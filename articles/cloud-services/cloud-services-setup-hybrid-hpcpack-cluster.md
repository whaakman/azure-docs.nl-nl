---
title: Instellen van een hybride HPC Pack-cluster in Azure | Microsoft Docs
description: Informatie over het gebruik van Microsoft HPC Pack en Azure voor het instellen van een kleine, hybride van de krachtige high performance computing (HPC)-cluster
services: cloud-services
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: hpc-pack
ms.assetid: ''
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 55dfd7e5ea93ae941d73612cc70ed82d48db725a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236735"
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Een hybride HPC-cluster (High Performance Computing) instellen met Microsoft HPC Pack en Azure Compute-knooppunten op aanvraag
Gebruik Microsoft HPC Pack 2012 R2 en Azure voor het instellen van een klein, hybride high performance computing-(HPC)-cluster. Het cluster wordt weergegeven in dit artikel bestaat uit een on-premises HPC Pack-hoofdknooppunt en sommige compute-knooppunten die u implementeert op aanvraag in een Azure-cloudservice. U kunt vervolgens rekentaken uitvoeren op de hybride cluster.

![Hybride HPC-cluster][Overview] 

In deze zelfstudie leert u een benadering, ook wel genoemd cluster-burst naar de cloud-schaalbare, on-demand Azure-resources gebruiken rekenintensieve toepassingen uit te voeren.

In deze zelfstudie wordt ervan uitgegaan dat geen ervaring met HPC Pack of rekenclusters. Dit is uitsluitend bedoeld om bij het implementeren van een hybride rekencluster snel voor demonstratiedoeleinden te gebruiken. Zie voor overwegingen en stappen voor het implementeren van een hybride HPC Pack-cluster op grotere schaal in een productieomgeving, of met HPC Pack 2016, de [gedetailleerde richtlijnen](https://go.microsoft.com/fwlink/p/?LinkID=200493). Voor andere scenario's met HPC Pack, met inbegrip van geautomatiseerde Clusterimplementatie in virtuele machines van Azure, Zie [opties voor HPC-cluster met Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement** -als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten.
* **Een on-premises-computer met Windows Server 2012 R2 of Windows Server 2012** -deze computer gebruiken als het hoofdknooppunt van het HPC-cluster. Als u Windows Server worden niet al wordt uitgevoerd, kunt u downloadt en installeert een [evaluatieversie](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * De computer moet worden toegevoegd aan een Active Directory-domein. Voor test-doeleinden, kunt u de computer hoofdknooppunt configureren als een domeincontroller. Toevoegen van de serverfunctie Active Directory Domain Services en het niveau van de computer van het hoofdknooppunt als een domeincontroller, Zie de documentatie voor Windows Server.
  * Ter ondersteuning van HPC Pack, het besturingssysteem moet worden geïnstalleerd op een van de volgende talen: Engels, Japans of Chinees (Vereenvoudigd).
  * Controleer of belangrijke en essentiële updates zijn geïnstalleerd.
* **HPC Pack 2012 R2** - [downloaden](https://go.microsoft.com/fwlink/p/?linkid=328024) de installatie voor de meest recente versie gratis verpakt en kopieer de bestanden naar de computer van het hoofdknooppunt. Kies installatiebestanden in dezelfde taal als de installatie van Windows Server.

    >[!NOTE]
    > Als u gebruiken van HPC Pack 2016 in plaats van HPC Pack 2012 R2 wilt, is aanvullende configuratie nodig. Zie de [gedetailleerde richtlijnen](https://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Domeinaccount** -dit account moet worden geconfigureerd met lokale beheerdersmachtigingen op het hoofdknooppunt van HPC Pack te installeren.
* **TCP-verbindingen op poort 443** van het hoofdknooppunt naar Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>HPC Pack installeren op het hoofdknooppunt
U eerst installeren Microsoft HPC Pack op uw on-premises computer met Windows Server. Deze computer wordt het hoofdknooppunt van het cluster.

1. Meld u met behulp van een domeinaccount met lokale beheerdersmachtigingen op met het hoofdknooppunt.

2. Start de installatiewizard van HPC Pack door het uitvoeren van Setup.exe uit de installatiebestanden van HPC Pack.

3. Op de **HPC Pack 2012 R2 installeren** scherm, klikt u op **nieuwe installatie of nieuwe functies toe te voegen aan een bestaande installatie**.

    ![HPC Pack 2012 Setup][install_hpc1]

4. Op de **Microsoft Software gebruikersovereenkomst pagina**, klikt u op **volgende**.

5. Op de **Type installatie selecteren** pagina, klikt u op **een nieuw HPC-cluster maken met het maken van een hoofdknooppunt**, en klik vervolgens op **volgende**.

6. De wizard wordt uitgevoerd verschillende voorafgaand aan installatie testen. Klik op **volgende** op de **installatieregels** pagina als alle tests zijn geslaagd. Anders, bekijk de informatie en breng eventueel benodigde wijzigingen in uw omgeving. Voer de tests opnieuw uit of indien nodig de installatiewizard opnieuw starten.
7. Op de **HPC DB configuratie** pagina, controleert u of **hoofdknooppunt** is ingeschakeld voor alle HPC-databases, en klik vervolgens op **volgende**. 

    ![DB-configuratie][install_hpc4]

8. Accepteer de standaardselecties op de overige pagina's van de wizard. Op de **vereiste onderdelen installeren** pagina, klikt u op **installeren**.
   
    ![Installeren][install_hpc6]

9. Nadat de installatie is voltooid, schakel het selectievakje **Start HPC Cluster Manager** en klik vervolgens op **voltooien**. (U start HPC Cluster Manager in een latere stap.)
   
    ![Voltooien][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Voorbereiden van het Azure-abonnement
Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com) met uw Azure-abonnement. Na het voltooien van deze stappen kunt u Azure-knooppunten van het hoofdknooppunt van het on-premises implementeren. 
  
  > [!NOTE]
  > Maak ook een notitie van uw Azure-abonnement-ID, die u later nodig. De ID vinden in **abonnementen** in de portal.
  > 

### <a name="upload-the-default-management-certificate"></a>Het standaard management-certificaat uploaden
HPC Pack installeert een zelfondertekend certificaat op het hoofdknooppunt, de standaard Microsoft HPC Azure Management certificaat genaamd, die u kunt als een Azure-beheercertificaat uploaden. Dit certificaat is opgegeven voor testen en proof of concept-implementaties voor het beveiligen van de verbinding tussen het hoofdknooppunt en Azure.

1. Vanaf de computer van het hoofdknooppunt, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).

2. Klik op **abonnementen** > *your_subscription_name*.

3. Klik op **beheercertificaten** > **uploaden**.

4. Blader op het hoofdknooppunt van het bestand C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Klik vervolgens op **uploaden**.

   
De **standaard HPC Azure Management** certificaat wordt weergegeven in de lijst met beheercertificaten.

### <a name="create-an-azure-cloud-service"></a>Een Azure-cloudservice maken
> [!NOTE]
> Maak de cloudservice en het opslagaccount (in een latere stap) in dezelfde geografische regio voor de beste prestaties.
> 
> 

1. Klik in de portal op **Cloudservices (klassiek)** > **+ toevoegen**.

2.  Typ een DNS-naam voor de service, kiest u een resourcegroep en een locatie en klik vervolgens op **maken**.


### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken
1. Klik in de portal op **opslagaccounts (klassiek)** > **+ toevoegen**.

2. Typ een naam voor het account en selecteer de **klassieke** implementatiemodel.

3. Kies een resourcegroep en een locatie en laat de overige instellingen op de standaardwaarden. Klik vervolgens op **Maken**.

## <a name="configure-the-head-node"></a>Het hoofdknooppunt configureren
Vereist voor het gebruik van HPC Cluster Manager voor het implementeren van Azure-knooppunten en voor het verzenden van taken, voert u eerst bepaalde stappen voor het cluster configureren.

1. Start op het hoofdknooppunt HPC Cluster Manager. Als de **hoofdknooppunt Selecteer** in het dialoogvenster wordt weergegeven, klikt u op **lokale Computer**. De **implementatie takenlijst** wordt weergegeven.

2. Onder **vereist implementatietaken**, klikt u op **configureren van uw netwerk**.
   
    ![Netwerk configureren][config_hpc2]

3. Selecteer in de Wizard netwerkconfiguratie, **alle knooppunten alleen op een bedrijfsnetwerk** (topologie 5). Deze configuratie van het netwerk is de eenvoudigste voor demonstratiedoeleinden te gebruiken.
   
    ![Topologie 5][config_hpc3]
   
4. Klik op **volgende** om standaardwaarden op de overige pagina's van de wizard te accepteren. Klik vervolgens op de **revisie** tabblad **configureren** om de netwerkconfiguratie te voltooien.

5. In de **implementatie takenlijst**, klikt u op **referenties van de installatie**.

6. In de **Installatiereferenties** dialoogvenster typt u de referenties van het domeinaccount dat u gebruikt voor het installeren van HPC Pack. Klik vervolgens op **OK**. 
   
    ![Van Installatiereferenties][config_hpc6]
   
7. In de **implementatie takenlijst**, klikt u op **configureren de naamgeving van nieuwe knooppunten**.

8. In de **knooppunt opgeven Naming reeks** dialoogvenster vak, accepteer de standaardwaarde naamgeving van reeksen en klikt u op **OK**. Hoewel de Azure knooppunten die u in deze zelfstudie toevoegen automatisch heten deze stap hebt voltooid.
   
    ![Naamgeving van knooppunt][config_hpc8]
   
9. In de **implementatie takenlijst**, klikt u op **maken van een knooppuntsjabloon**. Later in de zelfstudie, kunt u de knooppuntsjabloon gebruiken Azure knooppunten toevoegen aan het cluster.

10. In de Wizard knooppunt sjabloon maken het volgende doen:
    
    a. Op de **knooppunttype sjabloon kiezen** pagina, klikt u op **Windows Azure-knooppuntsjabloon**, en klik vervolgens op **volgende**.
    
    ![Knooppuntsjabloon][config_hpc10]
    
    b. Klik op **volgende** naar accepteer de standaardnaam van de sjabloon.
    
    c. Op de **abonnementsgegevens bieden** pagina, Voer uw Azure-abonnement-ID (beschikbaar in de gegevens van uw Azure-account). Klik op **beheercertificaat**, zoeken naar **standaard Microsoft HPC Azure Management.** Klik op **Volgende**.
    
    ![Knooppuntsjabloon][config_hpc12]
    
    d. Op de **bieden informatie over Service** pagina, selecteert u de cloudservice en het opslagaccount dat u in de vorige stap hebt gemaakt. Klik op **Volgende**.
    
    ![Knooppuntsjabloon][config_hpc13]
    
    e. Klik op **volgende** om standaardwaarden op de overige pagina's van de wizard te accepteren. Klik vervolgens op de **revisie** tabblad **maken** om de knooppuntsjabloon te maken.
    
    > [!NOTE]
    > De Azure-knooppunt-sjabloon bevat instellingen voor u (ingericht) starten en stoppen van de knooppunten handmatig met behulp van HPC Cluster Manager standaard. U kunt eventueel een schema om te starten en stoppen van de Azure-knooppunten automatisch configureren.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Azure-knooppunten aan het cluster toevoegen
Gebruik nu de knooppuntsjabloon Azure knooppunten toevoegen aan het cluster. De knooppunten toevoegen aan het cluster hun configuratiegegevens worden opgeslagen zodat u beginnen met kunt (ingericht) ze op elk gewenst moment in de cloudservice. Uw abonnement alleen wordt in rekening gebracht voor Azure-knooppunten wanneer de exemplaren worden uitgevoerd in de cloudservice.

Volg deze stappen voor het toevoegen van twee kleine knooppunten.

1. In HPC Cluster Manager, klikt u op **knooppunt Management** (met de naam **resourcebeheer** in de huidige versies van HPC Pack) > **knooppunt toevoegen**.
   
    ![Knooppunt toevoegen][add_node1]

2. In de Wizard knooppunt toevoegen op de **implementatiemethode selecteren** pagina, klikt u op **toevoegen Windows Azure-knooppunten**, en klik vervolgens op **volgende**.
   
    ![Azure-knooppunt toevoegen][add_node1_1]

3. Op de **nieuwe knooppunten opgeven** pagina, selecteert u de Azure node-sjabloon die u eerder hebt gemaakt (met de naam standaard **AzureNode standaardsjabloon**). Geef vervolgens **2** knooppunten van de grootte van **kleine**, en klik vervolgens op **volgende**.
   
    ![Knooppunten opgeven][add_node2]
   
4. Op de **de wizard knooppunt toevoegen** pagina, klikt u op **voltooien**.
    
     Twee Azure-knooppunten, met de naam **AzureCN-0001** en **AzureCN 0002**, worden nu weergegeven in HPC Cluster Manager. Beide zijn de **niet geïmplementeerd** staat.
   
    ![Extra knooppunten][add_node3]

## <a name="start-the-azure-nodes"></a>Start de Azure-knooppunten
Als u wilt het gebruik van de clusterresources in Azure, gebruikt u HPC Cluster Manager om te starten (ingericht) de Azure-knooppunten en ze online brengt.

1. In HPC Cluster Manager, klikt u op **knooppunt Management** (met de naam **resourcebeheer** in de huidige versies van HPC Pack), en selecteer het Azure-knooppunten.

2. Klik op **Start**, en klik vervolgens op **OK**.
   
   ![Start de knooppunten][add_node4]
   
    De knooppunten worden overgezet naar de **Provisioning** staat. Het logboek van de inrichting voor het volgen van de voortgang van de inrichting weergeven.
   
    ![Inrichten knooppunten][add_node6]

3. Na een paar minuten, de Azure-knooppunten inrichting voltooien en zijn de **Offline** staat. In deze status, de rolinstanties worden uitgevoerd, maar cluster taken kunnen nog niet accepteren.

4. Als u wilt bevestigen dat de rolinstanties worden uitgevoerd, klikt u in Azure portal, klikt u op **Cloudservices (klassiek)** > *your_cloud_service_name*.
   
   U ziet twee **HpcWorkerRole** exemplaren (knooppunten) die worden uitgevoerd in de service. HPC Pack ook automatisch wordt geïmplementeerd twee **HpcProxy** exemplaren (grootte normaal) voor het afhandelen van communicatie tussen het hoofdknooppunt en Azure.

   ![Exemplaren die worden uitgevoerd][view_instances1]

5. Om de Azure-knooppunten online cluster taken uit te voeren, selecteert u de knooppunten, met de rechtermuisknop op en klik vervolgens op **Online brengen**.
   
    ![Offline knooppunten][add_node7]
   
    HPC Cluster Manager geeft aan dat de knooppunten de **Online** staat.

## <a name="run-a-command-across-the-cluster"></a>Een opdracht in het cluster uitvoeren
Om te controleren of de installatie, gebruikt u de HPC Pack **clusrun** opdracht uit te voeren van een opdracht of een toepassing op een of meer clusterknooppunten. Als een eenvoudig voorbeeld gebruik **clusrun** om op te halen van de IP-adresconfiguratie van de Azure-knooppunten.

1. Open een opdrachtprompt als beheerder op het hoofdknooppunt.

2. Typ de volgende opdracht:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Als u wordt gevraagd, voert u het beheerderswachtwoord van het cluster. Hier ziet u uitvoer die vergelijkbaar is met de volgende opdracht.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Een testtaak uitvoeren
Een testtaak die wordt uitgevoerd op de hybride cluster nu verzenden. In dit voorbeeld is een eenvoudige parametrische taak (een soort intrinsiek parallelle berekeningen). In dit voorbeeld wordt uitgevoerd subtaken die een geheel getal op zichzelf met behulp van toevoegen de **stellen /a** opdracht. Alle knooppunten in het cluster bijdragen aan het voltooien van de subtaken voor gehele getallen tussen 1 en 100.

1. Klik in HPC Cluster Manager op **Taakbeheer** > **nieuwe parametrische Zwaaihoek taak**.
   
    ![Nieuwe taak][test_job1]

2. In de **nieuwe parametrische Zwaaihoek taak** in het dialoogvenster **vanaf de opdrachtregel**, type `set /a *+*` (overschrijven de standaard-opdrachtregel die wordt weergegeven). Laat de standaardwaarden voor de overige instellingen ongewijzigd en klik vervolgens op **indienen** om de taak te verzenden.
   
    ![Parametrische opschoning][param_sweep1]

3. Wanneer de taak is voltooid, dubbelklikt u op de **Mijn taak Zwaaihoek** taak.

4. Klik op **taken**, en klik vervolgens op een subtaak om de berekende uitvoer van deze subtaak weer te geven.
   
    ![Taakresultaten][view_job361]

5. Als u wilt zien welk knooppunt de berekening uitgevoerd voor deze subtaak, klikt u op **toegewezen knooppunten**. (U mogelijk de naam van een ander knooppunt weergeven voor uw cluster.)
   
    ![Taakresultaten][view_job362]

## <a name="stop-the-azure-nodes"></a>Stoppen van de Azure-knooppunten
Nadat u het cluster uitproberen, stopt u de Azure-knooppunten om onnodige kosten op uw account te voorkomen. Deze stap de cloudservice stopt en verwijdert u de instanties van Azure.

1. In HPC Cluster Manager in **knooppunt Management** (met de naam **resourcebeheer** in eerdere versies van HPC Pack), selecteert u beide Azure knooppunten. Klik vervolgens op **stoppen**.
   
    ![Knooppunten stoppen][stop_node1]

2. In de **stoppen Windows Azure-knooppunten** in het dialoogvenster, klikt u op **stoppen**. 
   
3. De knooppunten worden overgezet naar de **stoppen** staat. Na een paar minuten HPC Cluster Manager laat zien dat de knooppunten zijn **niet geïmplementeerd**.
   
    ![Niet-geïmplementeerde knooppunten][stop_node4]

4. Om te bevestigen dat de rolinstanties niet langer zijn uitgevoerd in Azure, in de Azure-portal, klik op **Cloudservices (klassiek)** > *your_cloud_service_name*. Er zijn geen exemplaren zijn geïmplementeerd in de productieomgeving. 
   
    Dit is de zelfstudie voltooid.

## <a name="next-steps"></a>Volgende stappen
* Verken de documentatie voor [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Als u een hybride implementatie van HPC Pack-cluster op grotere schaal instelt, Zie [Burst naar Werkrolinstanties van Azure met Microsoft HPC Pack](https://go.microsoft.com/fwlink/p/?LinkID=200493).
* Voor andere manieren om te maken van een HPC Pack-cluster in Azure, met inbegrip van met behulp van Azure Resource Manager-sjablonen, Zie [opties voor HPC-cluster met Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
