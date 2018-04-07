---
title: HPC Pack cluster voor Excel- en SOA | Microsoft Docs
description: Aan de slag grootschalige workloads voor Excel- en SOA-uitgevoerd op een HPC Pack-cluster in Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: aaf26e04fdb38fd76f4ab8211f9fdda8ebafd668
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Excel- en SOA-belastingen uitgevoerd op een HPC Pack-cluster in Azure aan de slag
In dit artikel laat zien hoe een Microsoft HPC Pack 2012 R2-cluster op Azure virtuele machines implementeren met behulp van een Azure quickstart-sjabloon of desgewenst een Azure PowerShell-script voor implementatie. Virtuele machine in Azure Marketplace-installatiekopieën die zijn ontworpen voor Microsoft Excel of een service oriented architecture (SOA) werkbelastingen HPC Pack maakt gebruik van het cluster. Het cluster kunt u vanuit een on-premises clientcomputer Excel HPC en SOA-services uitvoeren. De Excel-HPC-services bevatten Excel-werkmap offloading en de gebruiker gedefinieerde functies van Excel of de UDF's.

> [!IMPORTANT] 
> In dit artikel is gebaseerd op de functies, sjablonen en scripts voor HPC Pack 2012 R2. Dit scenario is momenteel niet ondersteund in HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Op een hoog niveau toont het volgende diagram het cluster HPC Pack dat u maakt.

![HPC-cluster met knooppunten waarop Excel werkbelastingen worden uitgevoerd][scenario]

## <a name="prerequisites"></a>Vereisten
* **Clientcomputer** -moet u een Windows-clientcomputer om voorbeeld-Excel- en SOA-taken voor het cluster te verzenden. U moet ook een Windows-computer voor het uitvoeren van het implementatiescript van Azure PowerShell-cluster (als u deze implementatiemethode kiest).
* **Azure-abonnement** -als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.
* **Quotum voor kernen** -moet u mogelijk verhogen van het quotum van kernen, vooral als u verschillende clusterknooppunten met multicore VM-grootten implementeert. Als u een Azure quickstart-sjabloon gebruikt, is het quotum voor kernen in Resource Manager per Azure-regio. In dat geval moet u mogelijk Verhoog het quotum in een specifieke regio. Zie [Azure-abonnement limieten, quota's en beperkingen](../../azure-subscription-service-limits.md). Een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zonder kosten.
* **Microsoft Office-licentie** - als het implementeren van compute knooppunten met de installatiekopie van een virtuele machine Marketplace HPC Pack 2012 R2 met Microsoft Excel, een 30-daagse evaluatieversie van Microsoft Excel Professional Plus 2013 is geïnstalleerd. Nadat de evaluatieperiode moet u een geldige licentie voor Microsoft Office Excel om door te gaan om uit te voeren werkbelastingen activeren opgeven. Zie [Excel-activering](#excel-activation) verderop in dit artikel. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Step 1. Een HPC Pack cluster in Azure instellen
Laten we zien twee opties voor het instellen van het cluster HPC Pack 2012 R2: eerste, met behulp van een Azure quickstart-sjabloon en de Azure portal; en de tweede pagina, met behulp van een Azure PowerShell-script voor implementatie.

### <a name="option-1-use-a-quickstart-template"></a>Optie 1. Een Quick Start-sjabloon gebruiken
Gebruik een Azure quickstart-sjabloon voor het snel implementeren van een cluster HPC Pack in de Azure portal. Als u de sjabloon in de portal opent, krijgt u een eenvoudige gebruikersinterface waarin u de instellingen voor uw cluster invoeren. Hier volgen de stappen. 

> [!TIP]
> Als u wilt, gebruikt u een [Azure Marketplace sjabloon](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) die wordt een soortgelijke cluster specifiek voor werkbelastingen van Excel. De stappen anders van de volgende enigszins.
> 
> 

1. Ga naar de [sjabloonpagina HPC-Cluster maken op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Als u wilt, kunt u informatie over de sjabloon en de broncode controleren.
2. Klik op **implementeren in Azure** starten van een implementatie met de sjabloon in de Azure-portal.
   
   ![Sjabloon implementeren in Azure][github]
3. Volg deze stappen voor het invoeren van de parameters voor de sjabloon HPC-cluster in de portal.
   
   a. Op de **Parameters** pagina Typ of wijzig de waarden voor de sjabloonparameters. (Klik op het pictogram naast elke instelling voor help-informatie). Voorbeeldwaarden weergegeven in het volgende scherm. In dit voorbeeld wordt een cluster met de naam *hpc01* in de *hpc.local* domein die bestaan uit een hoofdknooppunt en 2 rekenknooppunten. De rekenknooppunten worden van de installatiekopie van een HPC Pack VM met Microsoft Excel gemaakt.
   
   ![Parameters invoeren][parameters-new-portal]
   
   > [!NOTE]
   > Het hoofdknooppunt VM automatisch wordt gemaakt van de [nieuwste Marketplace-installatiekopie](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van HPC Pack 2012 R2 op Windows Server 2012 R2. Momenteel worden de installatiekopie is gebaseerd op HPC Pack 2012 R2 Update 3.
   > 
   > COMPUTE knooppunt virtuele machines worden gemaakt van de meest recente installatiekopie van de geselecteerde compute knooppunt familie. Selecteer de **ComputeNodeWithExcel** optie voor het meest recente HPC Pack compute-installatiekopie van het knooppunt met een evaluatieversie van Microsoft Excel Professional Plus 2013. Voor het implementeren van een cluster voor algemene SOA-sessies of Excel UDF-offloading, kies de **ComputeNode** optie (zonder Excel geïnstalleerd).
   > 
   > 
   
   b. Kies het abonnement.
   
   c. Maken van een resourcegroep voor het cluster, zoals *hpc01RG*.
   
   d. Kies een locatie voor de resourcegroep, zoals VS-midden.
   
   e. Op de **juridische voorwaarden** controleert u de voorwaarden. Als u akkoord gaat, klikt u op **aankoop**. Klik, wanneer u klaar bent met de waarden voor de sjabloon in te stellen, klikt u op **maken**.
4. Wanneer de implementatie is voltooid (het duurt meestal ongeveer 30 minuten), het cluster-certificaatbestand van het hoofdknooppunt van het cluster exporteren. In een latere stap, moet u deze openbaar certificaat op de clientcomputer voor de verificatie-serverzijde voor beveiligde HTTP-binding importeren.
   
   a. In de Azure portal, gaat u naar het dashboard, selecteert u het hoofdknooppunt en op **Connect** boven aan de pagina verbinding maken via Extern bureaublad.
   
    <!-- ![Connect to the head node][connect] -->
   
   b. Gebruik standaard procedures in Certificaatbeheer het hoofdknooppunt-certificaat (te vinden onder Cert: \LocalMachine\My) zonder de persoonlijke sleutel te exporteren. In dit voorbeeld exporteren *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Het certificaat exporteren][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Optie 2. Gebruik het implementatiescript HPC Pack IaaS
Het implementatiescript HPC Pack IaaS biedt een andere veelzijdige manier voor het implementeren van een cluster HPC Pack. Wordt gemaakt van een cluster in het klassieke implementatiemodel, terwijl de sjabloon worden gebruikt voor het Azure Resource Manager-implementatiemodel. Het script is ook compatibel met een abonnement in de globale Azure of Azure China-service.

**Aanvullende vereisten**

* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** - downloaden en uitpakken van de meest recente versie van het script van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controleer de versie van het script dat door het uitvoeren van `New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.5.0 of hoger van het script.

**Het configuratiebestand maken**

 Het implementatiescript HPC Pack IaaS maakt gebruik van een XML-configuratiebestand als invoer die worden beschreven van de infrastructuur van de HPC-cluster. Vervang de waarden voor uw omgeving in het volgende voorbeeld-configuratiebestand voor het implementeren van een cluster die bestaan uit een hoofdknooppunt en 18 rekenknooppunten gemaakt op basis van de installatiekopie van het compute-knooppunt met Microsoft Excel. Zie het bestand Manual.rtf in de scriptmap voor meer informatie over het configuratiebestand en [een HPC-cluster maken met het implementatiescript HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Opmerkingen over het configuratiebestand**

* De **VMName** van het hoofdknooppunt **moet** niet dezelfde zijn als de **ServiceName**, of de SOA-taken niet kunnen worden uitgevoerd.
* Zorg ervoor dat u opgeeft **EnableWebPortal** zodat het hoofdknooppunt certificaat wordt gegenereerd en geëxporteerd.
* Het bestand geeft een PowerShell-script na configuratie PostConfig.ps1 die wordt uitgevoerd op het hoofdknooppunt. Het volgende voorbeeldscript configureert u de verbindingsreeks voor Azure storage, de berekeningsfunctie knooppunt verwijdert uit het hoofdknooppunt en alle knooppunten online brengt wanneer ze zijn geïmplementeerd. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Het script uitvoeren**

1. Open de PowerShell-console op de clientcomputer als beheerder.
2. Wijzig de directory in de scriptmap (E:\IaaSClusterScript in dit voorbeeld).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. Voer de volgende opdracht voor het implementeren van het cluster HPC Pack. In dit voorbeeld wordt ervan uitgegaan dat het configuratiebestand in E:\HPCDemoConfig.xml bevinden zich.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

Het implementatiescript HPC Pack kan enige tijd worden uitgevoerd. Er is alleen die het script doet om te exporteren en de cluster-certificaat downloaden en opslaan in de map documenten van de huidige gebruiker op de clientcomputer. Het script genereert een bericht dat lijkt op het volgende. In de volgende stap moet u het certificaat in het juiste certificaatarchief importeren.    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Stap 2. Offload Excel-werkmappen en UDF's uitvoert vanuit een on-premises client
### <a name="excel-activation"></a>Excel-activering
Wanneer u de ComputeNodeWithExcel VM-installatiekopie voor productieworkloads, moet u een geldige licentiecode van Microsoft Office Excel activeren op de rekenknooppunten. Anders de evaluatieversie van Excel verloopt na 30 dagen en uitvoeren van Excel-werkmappen mislukt met de COMException (0x800AC472). 

U kunt Excel opnieuw 30 dagen van evaluatietijd rearm: Meld u aan bij de hoofdknooppunt en clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` op alle Excel rekenknooppunten via HPC Cluster Manager. U kunt maximaal twee keer rearm. Daarna moet u een geldige sleutel van de Office-licentie opgeven.

Office Professional Plus 2013 is geïnstalleerd op de VM-installatiekopie is een volume-editie met een Generic Volume License Key (GVLK). Kan worden geactiveerd via Key Management Service (KMS) / Active Directory gebaseerde activering (AD BA) of Multiple Activation Key (MAK). 

    * Een bestaande KMS-server gebruiken voor het gebruik van AD-KMS-BA of een nieuwe met behulp van Microsoft Office 2013 Volume License Pack instellen. (Als u wilt de server instellen op het hoofdknooppunt.) Activeer vervolgens de sleutel van de KMS-host via Internet of telefonisch. Vervolgens clusrun `ospp.vbs` stelt u de KMS-server en de poort en het activeren van Office op alle de Excel-rekenknooppunten. 

    * U met MAK, eerste clusrun `ospp.vbs` invoeren van de sleutel en alle geactiveerd het op Excel rekenknooppunten via Internet of telefonisch. 

> [!NOTE]
> Retail-productcodes voor Office Professional Plus 2013 kunnen niet worden gebruikt met deze VM-installatiekopie. Als u geldige sleutels en installatiemedia voor Office of Microsoft Excel-versies dan deze versie van de volume Office Professional Plus 2013 hebt, kunt u ze in plaats daarvan. Eerst verwijderen van dit volume-editie en installeer de versie die u hebt. De opnieuw geïnstalleerde Excel-rekenknooppunt kan worden vastgelegd als een aangepaste installatiekopie van de virtuele machine moet worden gebruikt in een implementatie op grote schaal.
> 
> 

### <a name="offload-excel-workbooks"></a>Offload Excel-werkmappen
Volg deze stappen voor een Excel-werkmap offload, zodat deze wordt uitgevoerd op het cluster HPC Pack in Azure. Om dit te doen, moet u Excel 2010 of 2013 is al geïnstalleerd op de clientcomputer hebben.

1. Gebruik een van de opties in stap 1 voor het implementeren van een cluster met HPC Pack met de Excel-compute installatiekopie van het knooppunt. Verkrijgen van de cluster-certificaatbestand (.cer) en de cluster-gebruikersnaam en het wachtwoord.
2. Importeer het certificaat van het cluster onder Cert: \CurrentUser\Root op de clientcomputer.
3. Zorg ervoor dat Excel is geïnstalleerd. Maak een Excel.exe.config-bestand met de volgende inhoud in dezelfde map als Excel.exe op de clientcomputer. Deze stap zorgt ervoor dat de HPC Pack 2012 R2 Excel COM-invoegtoepassing geladen.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. De client ingesteld voor het verzenden van taken naar het cluster HPC Pack. Een mogelijkheid is het downloaden van de volledige [HPC Pack 2012 R2 Update 3 installatie](http://www.microsoft.com/download/details.aspx?id=49922) en installeer de client HPC Pack. U kunt ook downloaden en installeren de [HPC Pack 2012 R2 Update 3 client-hulpprogramma's](https://www.microsoft.com/download/details.aspx?id=49923) en de juiste Visual C++ 2010 redistributable voor uw computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).
5. In dit voorbeeld gebruiken we een voorbeeld-Excel-werkmap met de naam ConvertiblePricing_Complete.xlsb. U kunt dit downloaden [hier](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Kopieer de Excel-werkmap naar een werkmap zoals D:\Excel\Run.
7. Open de Excel-werkmap. Op de **ontwikkelen** lint, klikt u op **COM-invoegtoepassingen** en Bevestig dat de HPC Pack Excel COM-invoegtoepassing is geladen.
   
   ![Excel-invoegtoepassing voor HPC Pack][addin]
8. Bewerk de VBA-macro HPCControlMacros in Excel door het wijzigen van de opmerkingen regels, zoals wordt weergegeven in het volgende script. Vervang de juiste waarden voor uw omgeving.
   
   ![Excel-macro voor HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Kopieer de Excel-werkmap naar een Uploadmap zoals D:\Excel\Upload. Deze map is opgegeven in de constante HPC_DependsFiles in de VBA-macro.
10. De werkmap op het cluster in Azure worden uitgevoerd, klikt u op de **Cluster** knop in het werkblad.

### <a name="run-excel-udfs"></a>Excel UDF's uitvoeren
Voor het uitvoeren van Excel UDF's, de voorgaande stappen 1-3 voor het instellen van de clientcomputer. Voor Excel UDF's hoeft u niet de Excel-toepassing geïnstalleerd op rekenknooppunten. Dus bij het maken van uw cluster rekenknooppunten, kunt u een normale compute-installatiekopie van een knooppunt in plaats van de installatiekopie van het knooppunt compute met Excel.

> [!NOTE]
> Er is een tekenlimiet 34 in de Excel 2010 en 2013 cluster connector dialoogvenster. Dit dialoogvenster kunt u het cluster waarop de UDF's opgeven. Als de naam van het volledige cluster is langer (bijvoorbeeld hpcexcelhn01.southeastasia.cloudapp.azure.com), past niet in het dialoogvenster. De tijdelijke oplossing is het instellen van een variabele van alle computers, zoals *CCP_IAASHN* met de waarde van de naam van het cluster lang. Voer de *% CCP_IAASHN %* in het dialoogvenster als de naam van het cluster hoofdknooppunt. 
> 
> 

Nadat het cluster is geïmplementeerd, doorgaan met de volgende stappen uit te voeren van een voorbeeld van een ingebouwde Excel UDF. Zie voor aangepaste Excel UDF's, deze [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) het bouwen van de XLL's en deze implementeren op het IaaS-cluster.

1. Open een nieuwe Excel-werkmap. Op de **ontwikkelen** lint, klikt u op **-invoegtoepassingen**. Klik vervolgens in het dialoogvenster op **Bladeren**, navigeer naar de map %CCP_HOME%Bin\XLL32 en selecteert u het voorbeeld ClusterUDF32.xll. Als de ClusterUDF32 niet op de clientcomputer bestaat, Kopieer de verbindingsreeks in de map %CCP_HOME%Bin\XLL32 op het hoofdknooppunt.
   
   ![Selecteer de UDF][udf]
2. Klik op **bestand** > **opties** > **geavanceerde**. Onder **formules**, Controleer **toestaan XLL gebruiker gedefinieerde functies worden uitgevoerd van een rekencluster**. Klik vervolgens op **opties** en voer de naam van het volledige cluster in **hoofdknooppunt clusternaam**. (Als zijn aangegeven eerder deze invoervak is beperkt tot 34 tekens, zodat een lange clusternaam niet past. U kunt hier een variabele voor alle computers voor een lange clusternaam.)
   
   ![De UDF configureren][options]
3. Als u wilt de UDF-berekening uitvoeren op het cluster, klik op de cel met de waarde =XllGetComputerNameC() en druk op Enter. De functie haalt gewoon de naam van het rekenknooppunt waarop de UDF wordt uitgevoerd. Voor de eerste keer uitvoert, wordt een dialoogvenster referenties gevraagd om de gebruikersnaam en wachtwoord verbinding maken met het IaaS-cluster.
   
   ![UDF uitvoeren][run]
   
   Druk op Alt-Shift-Ctrl + F9 de berekening uitvoeren op alle cellen wanneer er veel cellen berekenen.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Stap 3. Een SOA-werkbelasting uitvoeren vanuit een on-premises client
Algemene SOA-toepassingen op het cluster HPC Pack IaaS uitgevoerd, eerst gebruik een van de methoden in stap 1 voor het implementeren van het cluster. Geef een algemeen compute knooppunt installatiekopie in dit geval omdat u geen Excel nodig op de rekenknooppunten. Volg deze stappen.

1. Bij het ophalen van het certificaat van het cluster, moet u het importeren op de clientcomputer onder Cert: \CurrentUser\Root.
2. Installeer de [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) en [HPC Pack 2012 R2 Update 3 client-hulpprogramma's](https://www.microsoft.com/download/details.aspx?id=49923). Deze hulpprogramma's kunnen u ontwikkelen en uitvoeren van de SOA-clienttoepassingen.
3. Download de HelloWorldR2 [voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633). Open de HelloWorldR2.sln in Visual Studio 2010 of 2012. (Dit voorbeeld is niet compatibel met latere versies van Visual Studio).
4. Bouw het project EchoService eerst. Vervolgens implementeert u de service aan het cluster IaaS op dezelfde manier als die u op een on-premises-cluster implementeren. Zie de Readme.doc in HelloWordR2 voor gedetailleerde stappen. Wijzig en bouwen van de HellWorldR2 en andere projecten zoals beschreven in de volgende sectie voor het genereren van de SOA-clienttoepassingen die worden uitgevoerd op een Azure IaaS-cluster.

### <a name="use-http-binding-with-azure-storage-queue"></a>Http-binding gebruiken met Azure storage-wachtrij
Voor het gebruik van Http-binding met een Azure storage-wachtrij, kunt u enkele wijzigingen aanbrengen door de voorbeeldcode.

* Werk de naam van het cluster.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* Optioneel, gebruikt u de standaard TransportScheme in SessionStartInfo of expliciet instellen naar Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Standaard-binding voor de BrokerClient gebruiken.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Of u expliciet de basicHttpBinding ingesteld.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* U kunt desgewenst de UseAzureQueue-vlag ingesteld op true in SessionStartInfo. Als niet is ingesteld, dit wordt ingesteld op standaard, wanneer de clusternaam Azure domeinachtervoegsels en de TransportScheme Http is ingesteld op true.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Gebruik Http-binding zonder Azure storage-wachtrij
Voor het gebruik van Http-binding zonder een Azure storage-wachtrij, moet u expliciet de UseAzureQueue-vlag ingesteld op false in de SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Gebruik NetTcp-binding
De configuratie is voor het gebruik van NetTcp-binding, net als bij het verbinding maken met een on-premises-cluster. U moet enkele eindpunten op het hoofdknooppunt VM openen. Als u het HPC Pack IaaS-implementatiescript gebruikt om de cluster te maken, bijvoorbeeld de eindpunten in de Azure portal als volgt instellen.

1. Stop de virtuele machine.
2. Toevoegen van de TCP-poorten 9090, 9087, 9091, 9094 voor de sessie Broker, respectievelijk worker en Data services Broker
   
    ![Eindpunten configureren][endpoint-new-portal]
3. Start de virtuele machine.

De SOA-clienttoepassing vereist geen wijzigingen behalve het wijzigen van de hoofd-naam in de volledige naam van IaaS-cluster.

## <a name="next-steps"></a>Volgende stappen
* Zie [deze resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voor meer informatie over het uitvoeren van Excel-werkbelastingen met HPC Pack.
* Zie [SOA-Services beheren in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) voor meer informatie over het implementeren en beheren van SOA-services met HPC Pack.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
