---
title: Een hybride HPC Pack cluster in Azure instellen | Microsoft Docs
description: Informatie over het gebruik van Microsoft HPC Pack en Azure voor het instellen van een klein, hybride high performance computing-cluster (HPC)
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: ad5c13723eef352148a40e3e7f4f2ff616867296
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Een hybride high performance computing-cluster (HPC) met Microsoft HPC Pack en Azure rekenknooppunten op aanvraag instellen
Gebruik Microsoft HPC Pack 2012 R2 en Azure voor het instellen van een klein, hybride high performance computing-cluster (HPC). Het cluster weergegeven in dit artikel bestaat uit een lokale HPC Pack-hoofdknooppunt en sommige compute knooppunten die u implementeert op aanvraag in een Azure-cloudservice. Vervolgens kunt u op het cluster hybrid compute-taken uitvoeren.

![Hybride HPC-cluster][Overview] 

Deze zelfstudie ziet u een benadering wel cluster 'burst naar de cloud' schaalbare, bellen op Azure-resources gebruiken rekenintensieve toepassingen uit te voeren.

Deze zelfstudie wordt ervan uitgegaan dat geen ervaring met rekenclusters of HPC Pack. Het is bedoeld alleen bij het implementeren van een hybride rekencluster snel voor demonstratiedoeleinden. Zie voor overwegingen en stappen voor het implementeren van een hybride HPC Pack cluster op groter schaal in een productieomgeving of HPC Pack 2016 gebruiken de [gedetailleerde richtlijnen](http://go.microsoft.com/fwlink/p/?LinkID=200493). Voor andere scenario's met HPC Pack, met inbegrip van geautomatiseerde implementatie van het cluster in Azure virtuele machines, Zie [opties voor HPC-cluster met Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Vereisten
* **Azure-abonnement** -als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten.
* **Een on-premises-computer met Windows Server 2012 R2 of Windows Server 2012** -deze computer gebruiken als het hoofdknooppunt van het HPC-cluster. Als u al Windows Server worden niet uitgevoerd, kunt u downloaden en installeren van een [evaluatieversie](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * De computer moet worden toegevoegd aan Active Directory-domein. Voor testdoeleinden kunt u de computer hoofdknooppunt configureren als een domeincontroller. De Active Directory Domain Services-serverfunctie toevoegen en het niveau van het hoofdknooppunt computer als een domeincontroller, Zie de documentatie voor Windows Server.
  * Ter ondersteuning van HPC Pack, het besturingssysteem moet worden geïnstalleerd in een van de volgende talen: Engels, Japans of Chinees (Vereenvoudigd).
  * Controleer of belangrijke en kritieke updates zijn geïnstalleerd.
* **HPC Pack 2012 R2** - [downloaden](http://go.microsoft.com/fwlink/p/?linkid=328024) de installatie van het pakket voor de nieuwste versie kosteloos en kopieer de bestanden naar de computer van het hoofdknooppunt. Kies installatiebestanden in dezelfde taal als de installatie van Windows Server.

    >[!NOTE]
    > Als u wilt HPC Pack 2016 gebruiken in plaats van HPC Pack 2012 R2 is aanvullende configuratie nodig. Zie de [gedetailleerde richtlijnen](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Domeinaccount** -dit account moet worden geconfigureerd met lokale Administrator-machtigingen op het hoofdknooppunt HPC Pack te installeren.
* **TCP-verbinding op poort 443** vanaf het hoofdknooppunt van naar Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>HPC Pack installeren op het hoofdknooppunt
U eerst installeren Microsoft HPC Pack op uw lokale computer met Windows Server. Deze computer wordt het hoofdknooppunt van het cluster.

1. Meld u als een domeinaccount met lokale beheerdersmachtigingen heeft op met het hoofdknooppunt.

2. Start de installatiewizard van HPC Pack door het uitvoeren van Setup.exe uit de installatiebestanden HPC Pack.

3. Op de **HPC Pack 2012 R2 installeren** scherm, klikt u op **nieuwe installatie of het toevoegen van nieuwe functies aan een bestaande installatie**.

    ![HPC Pack 2012 Setup][install_hpc1]

4. Op de **Microsoft Software gebruikersovereenkomst pagina**, klikt u op **volgende**.

5. Op de **Type installatie selecteren** pagina, klikt u op **een nieuw HPC-cluster maken door het maken van een hoofdknooppunt**, en klik vervolgens op **volgende**.

6. De wizard wordt uitgevoerd voor verschillende voorafgaand aan installatie tests. Klik op **volgende** op de **installatieregels** pagina als u alle tests zijn geslaagd. Anders Bekijk de informatie en breng eventueel benodigde wijzigingen in uw omgeving. Voer de tests opnieuw, of indien nodig start u de installatiewizard opnieuw.
7. Op de **HPC DB configuratie** pagina, controleert u of **hoofdknooppunt** is ingeschakeld voor alle HPC-databases en klik vervolgens op **volgende**. 

    ![DB-configuratie][install_hpc4]

8. Accepteer de standaardselecties op de overige pagina's van de wizard. Op de **vereiste onderdelen installeren** pagina, klikt u op **installeren**.
   
    ![Installeren][install_hpc6]

9. Nadat de installatie is voltooid, schakel het selectievakje **Start HPC Cluster Manager** en klik vervolgens op **voltooien**. (U start HPC Cluster Manager in een latere stap.)
   
    ![Voltooien][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Voorbereiden van het Azure-abonnement
Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com) met uw Azure-abonnement. Na het voltooien van deze stappen kunt u Azure knooppunten van het hoofdknooppunt van het lokale implementeren. 
  
  > [!NOTE]
  > Maak ook een notitie van uw Azure-abonnement-ID, die u later nodig. De ID vinden in **abonnementen** in de portal.
  > 

### <a name="upload-the-default-management-certificate"></a>Het standaard management-certificaat uploaden
HPC Pack installeert een zelfondertekend certificaat op het hoofdknooppunt, de standaard van Microsoft HPC Azure Management certificaat genaamd, die u als een Azure-beheercertificaat uploaden kunt. Dit certificaat wordt aangeboden voor testen en bewijs van concept implementaties voor het beveiligen van de verbinding tussen het hoofdknooppunt en Azure.

1. Vanaf het hoofdknooppunt computer, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com).

2. Klik op **abonnementen** > *your_subscription_name*.

3. Klik op **beheercertificaten** > **uploaden**.

4. Blader op het hoofdknooppunt van het bestand C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Klik vervolgens op **uploaden**.

   
De **standaard HPC Azure Management** certificaat wordt weergegeven in de lijst met beheercertificaten.

### <a name="create-an-azure-cloud-service"></a>Een Azure-cloudservice maken
> [!NOTE]
> Maak de cloudservice en het opslagaccount (in een later stadium) in dezelfde geografische regio voor de beste prestaties.
> 
> 

1. Klik in de portal op **Cloudservices (klassiek)** > **+ toevoegen**.

2.  Typ een DNS-naam voor de service, kiest u een resourcegroep en een locatie en klik vervolgens op **maken**.


### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken
1. Klik in de portal op **opslagaccounts (klassiek)** > **+ toevoegen**.

2. Typ een naam voor het account en selecteer de **klassieke** implementatiemodel.

3. Kies een resourcegroep en een locatie en andere instellingen laten op standaardwaarden. Klik vervolgens op **Maken**.

## <a name="configure-the-head-node"></a>Het hoofdknooppunt configureren
Als u HPC Cluster Manager Azure knooppunten implementeren en om taken te verzenden, moet u eerst een aantal configuratiestappen vereist cluster uitvoeren.

1. Start op het hoofdknooppunt HPC Cluster Manager. Als de **hoofdknooppunt selecteren** dialoogvenster wordt weergegeven, klikt u op **lokale Computer**. De **implementatie takenlijst** wordt weergegeven.

2. Onder **implementatietaken vereist**, klikt u op **configureren van uw netwerk**.
   
    ![Netwerk configureren][config_hpc2]

3. Selecteer in de Wizard netwerkconfiguratie, **alle knooppunten alleen op een bedrijfsnetwerk** (topologie 5). Deze netwerkconfiguratie is de eenvoudigste voor demonstratiedoeleinden.
   
    ![Topologie 5][config_hpc3]
   
4. Klik op **volgende** standaardwaarden op de overige pagina's van de wizard accepteren. Klik op de **revisie** tabblad **configureren** om de netwerkconfiguratie te voltooien.

5. In de **implementatie takenlijst**, klikt u op **referenties van de installatie**.

6. In de **Installatiereferenties** dialoogvenster typt u de referenties van het domeinaccount dat u gebruikt voor het installeren van HPC Pack. Klik vervolgens op **OK**. 
   
    ![Van Installatiereferenties][config_hpc6]
   
7. In de **implementatie takenlijst**, klikt u op **configureren de naamgeving van nieuwe knooppunten**.

8. In de **Geef knooppunt Naming reeks** dialoogvenster vak, accepteer de standaardinstelling naamgeving van reeksen en klik op **OK**. Deze stap uitvoert, zelfs als de Azure knooppunten die u in deze zelfstudie toevoegen automatisch een naam zijn.
   
    ![Naamgeving van knooppunt][config_hpc8]
   
9. In de **implementatie takenlijst**, klikt u op **maken van een knooppuntsjabloon**. Verderop in de zelfstudie kunt u de knooppuntsjabloon Azure knooppunten toevoegen aan het cluster.

10. Ga als volgt te werk in de Wizard knooppunt sjabloon maken:
    
    a. Op de **knooppunttype sjabloon kiezen** pagina, klikt u op **Windows Azure-knooppuntsjabloon**, en klik vervolgens op **volgende**.
    
    ![Knooppuntsjabloon][config_hpc10]
    
    b. Klik op **volgende** naar accepteer de standaardnaam van de sjabloon.
    
    c. Op de **abonnementsgegevens bieden** pagina, Voer uw Azure-abonnement-ID (beschikbaar in uw Azure-account-informatie). Klik op **beheercertificaat**, zoeken naar **Microsoft HPC Azure Management Default.** Klik op **Volgende**.
    
    ![Knooppuntsjabloon][config_hpc12]
    
    d. Op de **servicegegevens bieden** pagina, selecteert u de cloudservice en het opslagaccount dat u in de vorige stap hebt gemaakt. Klik op **Volgende**.
    
    ![Knooppuntsjabloon][config_hpc13]
    
    e. Klik op **volgende** standaardwaarden op de overige pagina's van de wizard accepteren. Klik op de **revisie** tabblad **maken** de knooppuntsjabloon wilt maken.
    
    > [!NOTE]
    > Standaard bevat de knooppuntsjabloon Azure instellingen voor u (ingericht) starten en stoppen van de knooppunten handmatig met behulp van HPC Cluster Manager. U kunt desgewenst een schema te starten en stoppen van de Azure knooppunten automatisch configureren.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Azure knooppunten toevoegen aan het cluster
Nu de knooppuntsjabloon gebruiken Azure knooppunten toevoegen aan het cluster. De knooppunten toevoegen aan het cluster hun configuratiegegevens worden opgeslagen zodat u starten kunt (ingericht) ze op elk gewenst moment in de cloudservice. Uw abonnement alleen opgehaald in rekening gebracht voor Azure knooppunten nadat de exemplaren worden uitgevoerd in de cloudservice.

Volg deze stappen als u twee kleine knooppunten toevoegen.

1. In HPC Cluster Manager, klikt u op **knooppunt Management** (aangeroepen **bronbeheer** in huidige versies van HPC Pack) > **knooppunt toevoegen**.
   
    ![Knooppunt toevoegen][add_node1]

2. In de Wizard knooppunt toevoegen op de **implementatiemethode selecteren** pagina, klikt u op **toevoegen Windows Azure knooppunten**, en klik vervolgens op **volgende**.
   
    ![Azure knooppunt toevoegen][add_node1_1]

3. Op de **nieuwe knooppunten opgeven** pagina, selecteer de knooppuntsjabloon die u eerder hebt gemaakt voor het Azure-(standaard genoemd **AzureNode standaardsjabloon**). Geef vervolgens **2** knooppunten van de grootte van **kleine**, en klik vervolgens op **volgende**.
   
    ![Knooppunten opgeven][add_node2]
   
4. Op de **de wizard knooppunt toevoegen** pagina, klikt u op **voltooien**.
    
     Twee Azure knooppunten, met de naam **AzureCN 0001** en **AzureCN 0002**, worden nu weergegeven in HPC Cluster Manager. Beide zijn de **niet geïmplementeerd** status.
   
    ![Toegevoegde knooppunten][add_node3]

## <a name="start-the-azure-nodes"></a>Starten van de Azure knooppunten
Als u de clusterbronnen in Azure gebruiken wilt, gebruikt u HPC Cluster Manager om te starten (ingericht) de Azure-knooppunten en ze online brengt.

1. In HPC Cluster Manager, klikt u op **knooppunt Management** (aangeroepen **bronbeheer** in huidige versies van HPC Pack), en selecteer de Azure knooppunten.

2. Klik op **Start**, en klik vervolgens op **OK**.
   
   ![Beginknooppunten][add_node4]
   
    De knooppunten worden overgezet naar de **inrichten** status. Bekijk het logboek inrichting om de voortgang van de inrichting te volgen.
   
    ![Inrichten knooppunten][add_node6]

3. Na een paar minuten Azure knooppunten inrichting en zich in de **Offline** status. In deze toestand is, is de rolexemplaren worden uitgevoerd, maar taken cluster nog niet accepteren.

4. Klik om te bevestigen dat de rolexemplaren worden uitgevoerd, klikt u in de Azure portal **Cloudservices (klassiek)** > *your_cloud_service_name*.
   
   U ziet twee **HpcWorkerRole** exemplaren (knooppunten) uitgevoerd in de service. HPC Pack implementeert ook automatisch twee **HpcProxy** exemplaren (grootte normaal) voor het afhandelen van communicatie tussen het hoofdknooppunt en Azure.

   ![Exemplaren die worden uitgevoerd][view_instances1]

5. Voor het maken van de Azure knooppunten online cluster taken uitvoeren, selecteert u de knooppunten, klik met de rechtermuisknop en klik vervolgens op **Online brengen**.
   
    ![Offline knooppunten][add_node7]
   
    HPC Cluster Manager geeft aan dat de knooppunten de **Online** status.

## <a name="run-a-command-across-the-cluster"></a>Een opdracht in het cluster wordt uitgevoerd
Gebruiken om te controleren van de installatie, het HPC Pack **clusrun** opdracht voor het uitvoeren van een opdracht of een toepassing op een of meer clusterknooppunten. Als een eenvoudig voorbeeld gebruiken **clusrun** ophalen van de IP-adresconfiguratie van de Azure knooppunten.

1. Open een opdrachtprompt als beheerder op het hoofdknooppunt.

2. Typ de volgende opdracht:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Als u wordt gevraagd, voert u het wachtwoord van de cluster. U ziet de uitvoer van de opdracht ziet er als volgt.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Voer een test
Nu verzenden van een testtaak die wordt uitgevoerd op de hybride-cluster. In dit voorbeeld is een eenvoudige parametrische taak (een soort intrinsiek parallelle berekeningen). In dit voorbeeld wordt uitgevoerd subtaken die een geheel getal aan zichzelf met behulp van toevoegen de **/a ingesteld** opdracht. Alle knooppunten in het cluster bijdragen aan de subtaken voor gehele getallen van 1 tot 100 is voltooid.

1. Klik in HPC Cluster Manager op **Jobbeheer** > **nieuwe parametrische Sweep taak**.
   
    ![Nieuwe taak][test_job1]

2. In de **nieuwe parametrische Sweep taak** het dialoogvenster **opdrachtregel**, type `set /a *+*` (overschrijven de standaard-opdrachtregel die wordt weergegeven). Laat de standaardwaarden voor de overige instellingen en klik vervolgens op **indienen** voor het verzenden van de taak.
   
    ![Parametrische opschoning][param_sweep1]

3. Wanneer de taak is voltooid, dubbelklikt u op de **mijn Sweep taak** taak.

4. Klik op **taken weergeven**, en klik vervolgens op een subtaak om de berekende uitvoer van deze subtaak weer te geven.
   
    ![Resultaten van de taak][view_job361]

5. Als u wilt zien welk knooppunt de berekening uitgevoerd voor deze subtaak, klikt u op **toegewezen knooppunten**. (U mogelijk de naam van een ander knooppunt weergegeven voor uw cluster.)
   
    ![Resultaten van de taak][view_job362]

## <a name="stop-the-azure-nodes"></a>De Azure knooppunten stoppen
Nadat u het cluster uitproberen, stopt u de Azure knooppunten om te voorkomen dat onnodige kosten voor je account. Deze stap de cloudservice stopt en verwijdert u de Azure-rolexemplaren.

1. In HPC Cluster Manager in **knooppunt Management** (aangeroepen **bronbeheer** in eerdere versies van HPC Pack), selecteert u beide Azure knooppunten. Klik vervolgens op **stoppen**.
   
    ![Knooppunten stoppen][stop_node1]

2. In de **stoppen Windows Azure knooppunten** in het dialoogvenster, klikt u op **stoppen**. 
   
3. De knooppunten worden overgezet naar de **stoppen** status. Na een paar minuten HPC Cluster Manager geeft aan dat de knooppunten **niet geïmplementeerd**.
   
    ![Niet-geïmplementeerde knooppunten][stop_node4]

4. Om te bevestigen dat de rolinstanties niet langer zijn uitgevoerd in Azure, in de Azure-portal klikt u op **Cloudservices (klassiek)** > *your_cloud_service_name*. Er zijn geen exemplaren worden geïmplementeerd in de productieomgeving. 
   
    Deze zelfstudie is voltooid.

## <a name="next-steps"></a>Volgende stappen
* Bekijk de documentatie voor [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Als u een hybride implementatie van het cluster op schaal groter HPC Pack instelt, Zie [Burst naar Azure Worker-Rolexemplaren met Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Voor andere manieren om te maken van een HPC Pack-cluster in Azure, met inbegrip van Azure Resource Manager-sjablonen, Zie [opties voor HPC-cluster met Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


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
