---
title: HPC Pack-cluster voor Excel- en SOA | Microsoft Docs
description: Aan de slag die grootschalige Excel- en SOA-workloads worden uitgevoerd op een HPC Pack-cluster in Azure
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
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971856"
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Aan de slag met Excel- en SOA-workloads op een HPC Pack-cluster in Azure
In dit artikel wordt beschreven hoe u een Microsoft HPC Pack 2012 R2-cluster op Azure virtual machines implementeren met behulp van een Azure-quickstart-sjabloon of optioneel een Azure PowerShell-script voor implementatie. Het cluster maakt gebruik van Azure Marketplace VM-installatiekopieën die zijn ontworpen voor het uitvoeren van Microsoft Excel of een service oriented architecture (SOA)-workloads met HPC Pack. U kunt het cluster gebruiken voor het uitvoeren van Excel HPC en SOA-services vanuit een on-premises clientcomputer. De Excel-HPC-services bevatten Excel-werkmap offloading en de gebruiker gedefinieerde functies van Excel of de UDF's.

> [!IMPORTANT] 
> In dit artikel is gebaseerd op functies, sjablonen en scripts voor HPC Pack 2012 R2. In dit scenario wordt momenteel niet ondersteund in HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Op een hoog niveau toont het volgende diagram de HPC Pack-cluster dat u maakt.

![HPC-cluster met knooppunten met Excel-workloads][scenario]

## <a name="prerequisites"></a>Vereisten
* **Clientcomputer** -moet u een Windows-clientcomputer voorbeeld Excel- en SOA-taken indienen bij het cluster. U moet ook een Windows-computer om uit te voeren van het script voor implementatie van Azure PowerShell-cluster (als u ervoor deze implementatiemethode kiest).
* **Azure-abonnement** -als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.
* **Quotum voor kerngeheugens** -moet u mogelijk vergroot het quotum voor kernen, met name als u meerdere clusterknooppunten met multicore VM-grootten implementeert. Als u een Azure-quickstart-sjabloon gebruikt, wordt het quotum voor kerngeheugens in Resource Manager is per Azure-regio. In dat geval moet u mogelijk vergroot het quotum voor in een bepaalde regio. Zie [Azure-abonnementslimieten, quotums en beperkingen](../../azure-subscription-service-limits.md). Om een quotum te verhogen [opent u een ondersteuningsaanvraag online klant](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratis.
* **Microsoft Office-licentie** : als u bij het implementeren van compute-knooppunten met behulp van een Marketplace HPC Pack 2012 R2 VM-installatiekopie met Microsoft Excel, een 30-daagse evaluatieversie van Microsoft Excel Professional Plus 2013 is geïnstalleerd. Nadat de evaluatieperiode moet u een geldige Microsoft Office-licentie voor het activeren van Excel om door te gaan naar het uitvoeren van workloads opgeven. Zie [Excel-activering](#excel-activation) verderop in dit artikel. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Step 1. Een HPC Pack-cluster in Azure instellen
Laten we zien twee opties voor het instellen van het cluster met HPC Pack 2012 R2: eerste, met behulp van een Azure-quickstart-sjabloon en de Azure-portal; en seconde, met behulp van een Azure PowerShell-script voor implementatie.

### <a name="option-1-use-a-quickstart-template"></a>Optie 1. Een quickstart-sjabloon gebruiken
Een Azure-quickstart-sjabloon gebruiken om snel te implementeren een HPC Pack-cluster in Azure portal. Als u de sjabloon in de portal opent, krijgt u een eenvoudige gebruikersinterface waarin u de instellingen voor uw cluster invoeren. Hier volgen de stappen. 

> [!TIP]
> Als u wilt, gebruikt u een [Azure Marketplace-sjabloon](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) waarmee een vergelijkbare cluster specifiek voor werkbelastingen van Excel worden gemaakt. De stappen verschillen enigszins van de volgende opties.
> 
> 

1. Ga naar de [sjabloonpagina HPC-Cluster maken op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Als u wilt, kunt u informatie over de sjabloon en de broncode controleren.
2. Klik op **implementeren in Azure** om te beginnen een implementatie met de sjabloon in Azure portal.
   
   ![Sjabloon implementeren in Azure][github]
3. Volg deze stappen voor het invoeren van de parameters voor de sjabloon HPC-cluster in de portal.
   
   a. Op de **Parameters** pagina, invoeren of wijzigen van waarden voor de sjabloonparameters. (Klik op het pictogram naast elke instelling voor help-informatie.) Voorbeeldwaarden worden in het volgende scherm weergegeven. In dit voorbeeld wordt een cluster met de naam *hpc01* in de *hpc.local* domein die bestaan uit een hoofdknooppunt en 2-rekenknooppunten. De compute-knooppunten zijn gemaakt op basis van een HPC Pack VM-installatiekopie met Microsoft Excel.
   
   ![Parameters invoeren][parameters-new-portal]
   
   > [!NOTE]
   > Het hoofdknooppunt van virtuele machine automatisch op basis van gemaakt wordt de [nieuwste Marketplace-installatiekopie](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) van HPC Pack 2012 R2 op Windows Server 2012 R2. Op dit moment is de installatiekopie gebaseerd op HPC Pack 2012 R2 Update 3.
   > 
   > COMPUTE-knooppunt VM's zijn gemaakt op basis van de meest recente installatiekopie van de geselecteerde compute-knooppunt-familie. Selecteer de **ComputeNodeWithExcel** optie voor de meest recente HPC Pack compute-knooppunt-installatiekopie met een evaluatieversie van Microsoft Excel Professional Plus 2013. Voor het implementeren van een cluster voor sessies van algemene SOA- of Excel UDF-offloading, kiest u de **ComputeNode** optie (zonder Excel is geïnstalleerd).
   > 
   > 
   
   b. Kies het abonnement.
   
   c. Maak een resourcegroep voor het cluster, zoals *hpc01RG*.
   
   d. Kies een locatie voor de resourcegroep, zoals VS-midden.
   
   e. Op de **juridische voorwaarden** pagina, lees de voorwaarden. Als u akkoord gaat, klikt u op **aankoop**. Vervolgens, wanneer u klaar bent met instellen van de waarden voor de sjabloon, klikt u op **maken**.
4. Wanneer de implementatie is voltooid (meestal duurt het ongeveer 30 minuten), het certificaatbestand van het cluster exporteren van het hoofdknooppunt van het cluster. In een latere stap importeert u dit openbare certificaat op de clientcomputer voor de server-side '-verificatie voor beveiligde HTTP-binding.
   
   a. In de Azure-portal, gaat u naar het dashboard, selecteert u het hoofdknooppunt en klikt u op **Connect** aan de bovenkant van de pagina verbinding maken met behulp van extern bureaublad.
   
    <!-- ![Connect to the head node][connect] -->
   
   b. Gebruik standaard procedures in Certificate Manager het hoofdknooppunt-certificaat (te vinden onder Cert: \LocalMachine\My) zonder de persoonlijke sleutel te exporteren. In dit voorbeeld exporteren *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Het certificaat exporteren][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Optie 2. Gebruik het implementatiescript HPC Pack IaaS
Het implementatiescript HPC Pack IaaS biedt een andere veelzijdige manier om een HPC Pack-cluster te implementeren. Er wordt een cluster gemaakt in het klassieke implementatiemodel, dat de sjabloon maakt gebruik van het implementatiemodel Azure Resource Manager. Het script is ook compatibel met een abonnement in de globale Azure of Azure China-service.

**Aanvullende vereisten**

* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** : downloaden en uitpakken van de meest recente versie van het script uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controleer de versie van het script door uit te voeren `New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.5.0 of hoger van het script.

**Het configuratiebestand maken**

 Het HPC Pack IaaS-implementatiescript wordt een XML-configuratiebestand gebruikt als invoer die worden beschreven van de infrastructuur van de HPC-cluster. Vervang de waarden voor uw omgeving in het volgende voorbeeld-configuratiebestand voor het implementeren van een cluster dat bestaat uit een hoofdknooppunt en 18 rekenknooppunten gemaakt op basis van de installatiekopie van rekenknooppunt met Microsoft Excel. Zie voor meer informatie over het configuratiebestand, het bestand Manual.rtf in de scriptmap en [een HPC-cluster maken met het implementatiescript HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

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

* De **VMName** van het hoofdknooppunt **moet** gelijk zijn aan de **ServiceName**, of de SOA-taken niet kunnen worden uitgevoerd.
* Zorg ervoor dat u opgeeft **EnableWebPortal** zodat het certificaat van het hoofdknooppunt is gegenereerd en geëxporteerd.
* Het bestand bevat een PowerShell-script na configuratie PostConfig.ps1 die wordt uitgevoerd op het hoofdknooppunt. Het volgende voorbeeldscript configureert u de Azure storage-verbindingsreeks, verwijdert u de rol van de compute-knooppunt van het hoofdknooppunt en zorgt voor alle knooppunten online wanneer ze worden geïmplementeerd. 

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
2. Wijzig de map naar de scriptmap (E:\IaaSClusterScript in dit voorbeeld).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. Voer de volgende opdracht voor het implementeren van de HPC Pack-cluster. In dit voorbeeld wordt ervan uitgegaan dat het configuratiebestand in E:\HPCDemoConfig.xml bevinden zich.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

Het implementatiescript HPC Pack wordt uitgevoerd gedurende een bepaalde periode. Wat het script is is om te exporteren en het clustercertificaat downloaden en opslaan in de map documenten van de huidige gebruiker op de clientcomputer. Het script genereert een bericht dat lijkt op het volgende. In een volgende stap maakt importeren u het certificaat in het juiste certificaatarchief.    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Stap 2. Offload-Excel-werkmappen en UDF's uitvoeren vanuit een on-premises client
### <a name="excel-activation"></a>Excel-activering
Wanneer u de ComputeNodeWithExcel VM-installatiekopie voor werkbelastingen voor productie, moet u een geldige licentiesleutel voor Microsoft Office Excel activeren op de rekenknooppunten opgeven. Anders de evaluatieversie van Excel verloopt na 30 dagen en uitvoeren van Excel-werkmappen mislukken met de COMException (0x800AC472). 

U kunt Excel rearm 30 dagen van evaluatietijd: Meld u aan bij het hoofdknooppunt en clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` op alle Excel rekenknooppunten via HPC Cluster Manager. U kunt maximaal twee keer rearm. Hierna moet u een geldig Office-licentiesleutel opgeven.

Office Professional Plus 2013 is geïnstalleerd op de VM-installatiekopie is een volume-editie met een algemene productcode voor volumelicentie (GVLK). U kunt deze wel activeren via Key Management Service (KMS) / op Active Directory gebaseerde activering (AD BA) of Multiple Activation Key (MAK). 

    * Een bestaande KMS-server gebruiken voor het gebruik van KMS/AD-BA, of een nieuw abonnement met behulp van Microsoft Office 2013 Volume License Pack instellen. (Als u wilt instellen van de server op het hoofdknooppunt.) Vervolgens activeert u de KMS-hostsleutel via het Internet of telefonisch. Vervolgens clusrun `ospp.vbs` voor de KMS-server en de poort en het activeren van Office op alle de Excel-rekenknooppunten. 

    * Met MAK, eerste clusrun `ospp.vbs` voor invoer van de sleutel en vervolgens te activeren alle de Excel-rekenknooppunten via het Internet of telefonisch. 

> [!NOTE]
> Retail-productcodes voor Office Professional Plus 2013 kunnen niet worden gebruikt met deze VM-installatiekopie. Als u geldige sleutels en installatiemedia voor Office of Excel-versie dan deze versie van de volume Office Professional Plus 2013 hebt, kunt u ze in plaats daarvan gebruiken. Eerst verwijderen van dit volume-editie en installeer de versie die u hebt. De opnieuw geïnstalleerde Excel compute-knooppunt kan worden vastgelegd als een aangepaste VM-installatiekopie moet worden gebruikt in een implementatie op grote schaal.
> 
> 

### <a name="offload-excel-workbooks"></a>Excel-werkmappen-offload
Volg deze stappen voor het offloaden van een Excel-werkmap, zodat deze wordt uitgevoerd op het HPC Pack-cluster in Azure. Om dit te doen, moet u Excel 2010 of 2013 is al geïnstalleerd op de clientcomputer hebben.

1. Gebruik een van de opties in stap 1 voor het implementeren van een HPC Pack-cluster met de Excel-knooppunt afbeelding berekenen. Verkrijgen van de cluster-certificaatbestand (.cer) en de cluster-gebruikersnaam en het wachtwoord.
2. Importeer het clustercertificaat onder Cert: \CurrentUser\Root op de clientcomputer.
3. Zorg ervoor dat Excel is geïnstalleerd. Maak een Excel.exe.config-bestand met de volgende inhoud in dezelfde map als Excel.exe op de clientcomputer. Deze stap zorgt ervoor dat de HPC Pack 2012 R2 Excel COM-invoegtoepassing wordt geladen is.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. De client ingesteld voor het verzenden van taken naar de HPC Pack-cluster. Een optie is voor het downloaden van de volledige [HPC Pack 2012 R2 Update 3 installatie](http://www.microsoft.com/download/details.aspx?id=49922) en de HPC Pack-client installeren. U kunt ook downloaden en installeren de [client hulpprogramma's van HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923) en de juiste Visual C++ 2010 redistributable voor uw computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).
5. In dit voorbeeld gebruiken we een voorbeeld-Excel-werkmap met de naam ConvertiblePricing_Complete.xlsb. U kunt dit downloaden [hier](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Kopieer de Excel-werkmap naar een werkmap zoals D:\Excel\Run.
7. Open de Excel-werkmap. Op de **ontwikkelen** lint, klikt u op **COM-invoegtoepassingen** en bevestigt u dat de HPC Pack Excel COM-invoegtoepassing is geladen.
   
   ![Excel-invoegtoepassing voor HPC Pack][addin]
8. De macro VBA HPCControlMacros in Excel bewerken door te wijzigen van de regels voor opmerkingen, zoals wordt weergegeven in het volgende script. Vervangen door de juiste waarden voor uw omgeving.
   
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
9. Kopieer de Excel-werkmap naar een Uploadmap, zoals D:\Excel\Upload. Deze map is opgegeven in de constante HPC_DependsFiles in de VBA-macro.
10. De werkmap op het cluster in Azure worden uitgevoerd, klikt u op de **Cluster** knop op het werkblad.

### <a name="run-excel-udfs"></a>Excel UDF's worden uitgevoerd
Als u wilt uitvoeren van Excel UDF's, volgt u de voorgaande stappen 1-3 voor het instellen van de clientcomputer. Voor Excel UDF's moet u niet de Excel-toepassing geïnstalleerd op rekenknooppunten. Dus als het maken van uw cluster rekenknooppunten, u kunt ervoor kiezen een normale compute-knooppunt afbeelding in plaats van de installatiekopie van rekenknooppunt met Excel.

> [!NOTE]
> Er is een 34 tekens in de Excel 2010 en 2013 cluster connector-dialoogvenster. U kunt dit dialoogvenster gebruiken om op te geven van het cluster dat wordt uitgevoerd de UDF's. Als de naam van het volledige cluster is langer (bijvoorbeeld hpcexcelhn01.southeastasia.cloudapp.azure.com), past niet in het dialoogvenster. De tijdelijke oplossing is om in te stellen van een variabele machine hele zoals *CCP_IAASHN* met de waarde van de lange clusternaam. Voer vervolgens *CCP_IAASHN %* in het dialoogvenster als de naam van het cluster-hoofdknooppunt. 
> 
> 

Nadat het cluster is geïmplementeerd, doorgaan met de volgende stappen voor het uitvoeren van een voorbeeld van een ingebouwde Excel UDF. Zie voor aangepaste Excel UDF's, deze [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) de XLL's bouwen en implementeren op de IaaS-cluster.

1. Open een nieuwe Excel-werkmap. Op de **ontwikkelen** lint, klikt u op **Add-Ins**. Klik vervolgens in het dialoogvenster op **Bladeren**, navigeer naar de map %CCP_HOME%Bin\XLL32 en selecteert u het voorbeeld ClusterUDF32.xll. Als de ClusterUDF32 niet op de clientcomputer bestaat, kopieert u deze in de map %CCP_HOME%Bin\XLL32 op het hoofdknooppunt.
   
   ![Selecteer de UDF][udf]
2. Klik op **bestand** > **opties** > **geavanceerde**. Onder **formules**, Controleer **toestaan van de gebruiker gedefinieerde XLL functies om uit te voeren van een rekencluster**. Klik vervolgens op **opties** en voer de naam van het volledige cluster in **hoofdknooppunt clusternaam**. (Als zijn aangegeven eerder dit invoervak is beperkt tot 34 tekens, zodat een lange clusternaam mogelijk niet voldoende. U kunt hier een variabele voor alle computers gebruiken voor een lange clusternaam.)
   
   ![De UDF configureren][options]
3. Als u wilt de UDF-berekening uitvoeren op het cluster, klikt u op de cel met de waarde =XllGetComputerNameC() en druk op Enter. De functie haalt gewoon de naam van het rekenknooppunt waarop de UDF wordt uitgevoerd. Voor de eerste keer uitvoert, wordt een dialoogvenster referenties gevraagd om de gebruikersnaam en wachtwoord verbinding maken met het IaaS-cluster.
   
   ![-UDF uitvoeren][run]
   
   Druk op Alt-Shift-Ctrl + F9 de berekening wilt uitvoeren op alle cellen wanneer er veel cellen dat moet worden berekend.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Stap 3. Een SOA-workloads uitvoeren vanaf een on-premises client
Voor het uitvoeren algemene SOA-toepassingen op het cluster met HPC Pack IaaS, eerst gebruiken een van de methoden in stap 1 om het cluster te implementeren. Geef een algemene compute-knooppunt afbeelding in dit geval omdat u geen Excel nodig op de rekenknooppunten. Volg deze stappen.

1. Bij het ophalen van het clustercertificaat, importeert u het op de clientcomputer onder Cert: \CurrentUser\Root.
2. Installeer de [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) en [client hulpprogramma's van HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923). Deze hulpprogramma's kunnen u ontwikkelen en uitvoeren van de SOA-clienttoepassingen.
3. Download de HelloWorldR2 [voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633). Open de HelloWorldR2.sln in Visual Studio 2010- of 2012. (In dit voorbeeld is momenteel niet compatibel met nieuwere versies van Visual Studio.)
4. Maak eerst het EchoService-project. Vervolgens implementeert u de service met het IaaS-cluster op dezelfde manier als die u op een on-premises cluster implementeren. Zie de Readme.doc in HelloWordR2 voor gedetailleerde stappen. Wijzig en bouw de HellWorldR2 en andere projecten zoals beschreven in de volgende sectie voor het genereren van de SOA-clienttoepassingen die worden uitgevoerd op een Azure IaaS-cluster.

### <a name="use-http-binding-with-azure-storage-queue"></a>Http-binding gebruiken met Azure storage-wachtrij
Voor het gebruik van Http-binding met een Azure storage-wachtrij, moet u enkele wijzigingen aanbrengen aan de voorbeeldcode.

* Werk de clusternaam.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* (Optioneel), gebruikt u de standaard TransportScheme in SessionStartInfo of deze expliciet zijn ingesteld op Http.

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
  
    Of stel de basicHttpBinding expliciet het gebruik.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* (Optioneel) de UseAzureQueue-vlag ingesteld op true in SessionStartInfo. Als niet is ingesteld, deze wordt ingesteld op ' True ' wanneer ze de naam van het cluster heeft Azure domeinachtervoegsels en de TransportScheme is Http.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Gebruik Http-binding zonder Azure storage-wachtrij
Voor het gebruik van Http-binding zonder een Azure storage-wachtrij, moet u expliciet de UseAzureQueue-vlag ingesteld op false in de SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Gebruik NetTcp-binding
De configuratie is vergelijkbaar met het verbinding maken met een on-premises cluster voor het gebruik van NetTcp-binding. U moet een aantal eindpunten op het hoofdknooppunt VM openen. Als u het HPC Pack IaaS-implementatiescript gebruikt om het cluster te maken, bijvoorbeeld de eindpunten in Azure portal als volgt instellen.

1. De virtuele machine stoppen.
2. Toevoegen van de TCP-poorten 9090, 9087, 9091, 9094 voor de sessie Broker, werknemer- en gegevensservices, respectievelijk Broker
   
    ![Eindpunten configureren][endpoint-new-portal]
3. Start de virtuele machine.

De SOA-clienttoepassing vereist geen wijzigingen, behalve de naam van de kop voor de volledige naam voor IaaS-cluster te wijzigen.

## <a name="next-steps"></a>Volgende stappen
* Zie [deze resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voor meer informatie over het uitvoeren van Excel-werkbelastingen met HPC Pack.
* Zie [SOA-Services beheren in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) voor meer informatie over het implementeren en beheren van services met HPC Pack SOA.

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
