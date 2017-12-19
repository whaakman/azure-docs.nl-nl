---
title: Service Fabric-cluster maken in de Azure portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een beveiligde Service Fabric-cluster in Azure met behulp van de Azure portal en Azure Key Vault instelt.
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: be880efdcf1276252c76f27c2f2fd99edd606caa
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Maken van een Service Fabric-cluster in Azure met Azure portal
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Dit is een stapsgewijze handleiding die u bij de stappen helpt voor het instellen van een beveiligde Service Fabric-cluster in Azure met behulp van de Azure-portal. Deze handleiding doorloopt u de volgende stappen uit:

* Sleutelkluis instellen voor het beheren van sleutels voor de clusterbeveiliging.
* Een beveiligde cluster in Azure maken via de Azure portal.
* Beheerders met behulp van certificaten verifiëren.

> [!NOTE]
> Voor meer geavanceerde beveiligingsopties, zoals gebruikersverificatie met Azure Active Directory en het instellen van certificaten voor Toepassingsbeveiliging, [maken van het cluster met Azure Resource Manager][create-cluster-arm].
> 
> 

Een beveiligde cluster is een cluster dat voorkomt onbevoegde toegang tot beheertaken uit te voeren, waaronder implementeren, bijwerken en verwijderen van toepassingen, services en de gegevens die ze bevatten. Een niet-beveiligde cluster is een cluster iedereen kan verbinding maken met op elk gewenst moment en beheerbewerkingen uitvoeren. Hoewel het is mogelijk om een niet-beveiligde cluster te maken, is het **ten zeerste aangeraden om een beveiligde cluster te maken**. Een niet-beveiligde cluster **later niet kan worden beveiligd** -een nieuw cluster moet worden gemaakt.

De concepten zijn hetzelfde voor het maken van beveiligde clusters, ongeacht of de clusters Linux-clusters of Windows-clusters. Zie voor meer informatie en helper scripts voor het maken van beveiligde Linux-clusters, [maken van beveiligde clusters](service-fabric-cluster-creation-via-arm.md). De parameters die zijn verkregen door het Help-script dat kunnen worden invoer rechtstreeks in de portal, zoals beschreven in de sectie [een cluster maken in de Azure portal](#create-cluster-portal).

## <a name="configure-key-vault"></a>Key Vault configureren 
### <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Maakt gebruik van deze handleiding [Azure PowerShell][azure-powershell]. Bij het starten van een nieuwe PowerShell-sessie aanmelden bij uw Azure-account en selecteer uw abonnement voordat u Azure opdrachten uitvoert.

Aanmelden bij uw azure-account:

```powershell
Login-AzureRmAccount
```

Selecteer uw abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="set-up-key-vault"></a>Key Vault instellen
Dit deel van de handleiding helpt u bij het maken van een Sleutelkluis voor een Service Fabric-cluster in Azure en voor Service Fabric-toepassingen. Zie voor een volledige handleiding op Sleutelkluis de [Sleutelkluis instructiehandleiding][key-vault-get-started].

Service Fabric gebruikt x.509-certificaten voor het beveiligen van een cluster. Azure Sleutelkluis wordt gebruikt voor het beheren van certificaten voor Service Fabric-clusters in Azure. Wanneer een cluster is geïmplementeerd in Azure, wordt de Azure-resourceprovider verantwoordelijk voor het maken van Service Fabric-clusters certificaten ophaalt uit Sleutelkluis en installeert ze op het cluster virtuele machines.

Het volgende diagram illustreert de relatie tussen de Sleutelkluis, een Service Fabric-cluster en de Azure-resourceprovider die gebruikmaakt van certificaten die zijn opgeslagen in de Sleutelkluis bij het maken van een cluster:

![Installatie van het certificaat][cluster-security-cert-installation]

#### <a name="create-a-resource-group"></a>Een resourcegroep maken
De eerste stap is het maken van een nieuwe resourcegroep specifiek voor Sleutelkluis. Sleutelkluis in een eigen resourcegroep stellen wordt aanbevolen, zodat u kunt resourcegroepen berekenings- en - zoals de resourcegroep die uw Service Fabric-cluster heeft - zonder verlies van uw sleutels en geheimen verwijderen. De resourcegroep met uw Sleutelkluis moet in dezelfde regio bevinden als het cluster dat wordt gebruikt.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

#### <a name="create-key-vault"></a>Key Vault maken
Een Sleutelkluis maken in de nieuwe resourcegroep. De Sleutelkluis **moet zijn ingeschakeld voor de implementatie van** om toe te staan van de Service Fabric-resourceprovider certificaten van het verkrijgen en installeren op clusterknooppunten:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

Als u een bestaande Sleutelkluis hebt, kunt u dit inschakelen voor implementatie met een van de volgende manieren:

##### <a name="azure-powershell"></a>Azure PowerShell

```powershell
PS C:\Users\vturecek> Set-AzureRmKeyVaultAccessPolicy -VaultName 'myvault' -EnabledForDeployment
```

##### <a name="azure-cli"></a>Azure CLI:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


### <a name="add-certificates-to-key-vault"></a>Voeg certificaten aan Sleutelkluis
Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie voor meer informatie over hoe de certificaten worden gebruikt in Service Fabric, [scenario's voor beveiliging van Service Fabric-cluster][service-fabric-cluster-security].

#### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Dit certificaat is vereist voor het beveiligen van een cluster en onbevoegde toegang tot deze voorkomen. Biedt clusterbeveiliging in een aantal manieren:

* **Cluster-verificatie:** knooppunt naar communicatie voor cluster federation verifieert. Alleen knooppunten die u hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* **Server-verificatie:** verifieert de eindpunten van het cluster-beheer met een management-client, zodat de management-client, weet deze communiceert met het echte cluster. Dit certificaat biedt ook SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* De onderwerpnaam van het certificaat moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de Service Fabric-cluster. Dit is vereist voor SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de `.cloudapp.azure.com` domein. Verkrijgen van een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam voor uw cluster gebruikt.

#### <a name="client-authentication-certificates"></a>Certificaten voor clientverificatie
Aanvullende clientcertificaten verifiëren van beheerders voor beheertaken cluster. Service Fabric heeft twee toegangsniveaus: **admin** en **alleen-lezengebruiker**. Ten minste moet één certificaat voor beheerderstoegang worden gebruikt. Voor extra beveiliging op gebruikersniveau toegang, moet u een afzonderlijk certificaat opgegeven. Zie voor meer informatie over toegang tot rollen [toegangsbeheer op basis van rollen voor Service Fabric-clients][service-fabric-cluster-security-roles].

U hoeft niet voor het uploaden van certificaten voor clientverificatie voor Sleutelkluis voor gebruik met Service Fabric. Deze certificaten hoeft alleen te worden opgegeven voor gebruikers die gemachtigd zijn voor Clusterbeheer. 

> [!NOTE]
> Azure Active Directory is de aanbevolen manier om clients voor clusterbeheerbewerkingen te verifiëren. Voor het gebruik van Azure Active Directory, moet u [maken van een cluster met Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om beveiligingsredenen toepassing. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van configuratiewaarden die van toepassing
* Versleuteling van gegevens over knooppunten tijdens de replicatie 

Toepassingscertificaten kunnen niet worden geconfigureerd wanneer u een cluster via de Azure portal. Voor het configureren van toepassingscertificaten tijdens de installatie cluster, moet u [maken van een cluster met Azure Resource Manager][create-cluster-arm]. U kunt ook toepassingscertificaten toevoegen aan het cluster nadat deze is gemaakt.

#### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certificaten voor Azure-resource provider opmaak
Bestanden met persoonlijke sleutel (.pfx) kunnen worden toegevoegd en gebruikt rechtstreeks via de Sleutelkluis. De Azure-resourceprovider vereist echter sleutels worden opgeslagen in een speciale JSON-indeling met het .pfx als een Base64-gecodeerde tekenreeks en het wachtwoord van de persoonlijke sleutel. Om te voldoen deze vereisten, sleutels moeten worden geplaatst in een JSON-tekenreeks en vervolgens opgeslagen als *geheimen* in de Sleutelkluis.

Om dit proces gemakkelijker te maken, een PowerShell-module is [beschikbaar op GitHub][service-fabric-rp-helpers]. Volg deze stappen voor het gebruik van de module:

1. De volledige inhoud van de opslagplaats downloaden naar een lokale map. 
2. Importeer de module in uw PowerShell-venster:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

De `Invoke-AddCertToKeyVault` opdracht in deze PowerShell-module automatisch de persoonlijke sleutel van een certificaat in een JSON-tekenreeks indelingen en geüpload naar de Sleutelkluis. Gebruik dit voor het certificaat van het cluster en eventuele extra toepassingscertificaten toevoegen aan de Sleutelkluis. Herhaal deze stap voor elke extra certificaten die u wilt installeren in uw cluster.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Dit zijn de Sleutelkluis-vereisten voor het configureren van een Service Fabric-cluster Resource Manager-sjabloon die certificaten voor verificatie, eindpuntbeveiliging management en verificatie en eventuele aanvullende Toepassingsbeveiliging installeert functies die gebruikmaken van X.509-certificaten. Nu hebt u nu de volgende instellingen in Azure:

* Sleutelkluis-resourcegroep
  * Key Vault
    * Certificaat voor serververificatie cluster

< /a "maken-cluster-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Cluster maken in de Azure portal
### <a name="search-for-the-service-fabric-cluster-resource"></a>Zoeken naar de Service Fabric-cluster-bron
![zoeken naar Service Fabric-cluster sjabloon op de Azure-portal.][SearchforServiceFabricClusterTemplate]

1. Meld u aan bij [Azure Portal][azure-portal].
2. Klik op **nieuw** toevoegen van een nieuwe resource-sjabloon. Zoeken naar de sjabloon Service Fabric-Cluster in de **Marketplace** onder **Alles**.
3. Selecteer **Service Fabric-Cluster** uit de lijst.
4. Navigeer naar de **Service Fabric-Cluster** blade, klikt u op **maken**,
5. De **maken Service Fabric-cluster** blade bevat de volgende vier stappen.

#### <a name="1-basics"></a>1. Basisbeginselen
![Schermopname van het maken van een nieuwe resourcegroep.][CreateRG]

In de blade grondbeginselen moet u de algemene informatie opgeven voor uw cluster.

1. Voer de naam van het cluster.
2. Voer een **gebruikersnaam** en **wachtwoord** voor extern bureaublad voor de virtuele machines.
3. Zorg ervoor dat u selecteert de **abonnement** dat het cluster worden geïmplementeerd op, met name als u meerdere abonnementen hebt.
4. Maak een **nieuwe resourcegroep**. Het is raadzaam zodat deze dezelfde naam als het cluster, omdat het helpt bij het vinden ze later, vooral wanneer u probeert te wijzigingen aanbrengen in uw implementatie of verwijder uw cluster.
   
   > [!NOTE]
   > Hoewel u een bestaande resourcegroep gebruiken kunt, is het raadzaam om een nieuwe resourcegroep te maken. Hierdoor kunt gemakkelijk clusters die u niet hoeft verwijderen.
   > 
   > 
5. Selecteer de **regio** in waarop u wilt maken van het cluster. U moet dezelfde regio als uw Sleutelkluis is in.

#### <a name="2-cluster-configuration"></a>2. Clusterconfiguratie
![Een knooppunttype maken][CreateNodeType]

Configureer de clusterknooppunten. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Het cluster kan meer dan één knooppunttype hebben moet, maar het primaire knooppunttype (de eerste optie die u in de portal definieert) ten minste vijf virtuele machines, omdat dit het knooppunttype waar de Service Fabric-systeemservices worden geplaatst. Configureer geen **plaatsingseigenschappen** omdat een standaardeigenschap van de plaatsing van 'NodeTypeName' wordt automatisch toegevoegd.

> [!NOTE]
> Een veelvoorkomend scenario voor typen met meerdere knooppunten is een toepassing met een front-end-service en een back-endservice. U wilt de front-end-service plaatsen op kleinere virtuele machines (VM-grootten zoals D2) met poorten openen met het Internet, maar u wilt de back-endservice plaatsen op grotere virtuele machines (met VM-grootten zoals D4, D6 en D15) zonder internetverbinding poorten geopend.
> 
> 

1. Kies een naam voor uw knooppunttype (1-12 tekens die alleen letters en cijfers bevatten).
2. De minimale **grootte** van virtuele machines voor het primaire knooppunt type wordt aangedreven door de **duurzaamheid** laag die u voor het cluster kiest. De standaardwaarde voor de laag duurzaamheid is Brons. Zie voor meer informatie over duurzaamheid [het kiezen van de Service Fabric-cluster betrouwbaarheid en duurzaamheid][service-fabric-cluster-capacity].
3. Selecteer het VM-grootte en de prijscategorie. D-reeks VMs SSD-stations hebben en worden sterk aanbevolen voor stateful toepassingen. Geen gebruik van een VM SKU gedeeltelijke kernen heeft of kleiner zijn dan 7 GB aan beschikbare schijfruimte capaciteit. 
4. De minimale **getal** van virtuele machines voor het primaire knooppunt type wordt aangedreven door de **betrouwbaarheid** laag die u kiest. De standaardwaarde voor de betrouwbaarheidslaag is Zilver. Zie voor meer informatie over de betrouwbaarheid, [het kiezen van de Service Fabric-cluster betrouwbaarheid en duurzaamheid][service-fabric-cluster-capacity].
5. Het aantal virtuele machines voor het knooppunttype kiezen. U kunt omhoog of omlaag het aantal virtuele machines in een knooppunttype later op schalen, maar op het primaire knooppunttype minimaal wordt aangedreven door het niveau van betrouwbaarheid die u hebt gekozen. Andere knooppunttypen kunnen een minimum van 1 VM hebben.
6. Aangepaste eindpunten configureren. Dit veld kunt u een door komma's gescheiden lijst met poorten die u wilt weergeven via de Azure Load Balancer met het openbare Internet voor uw toepassingen invoeren. Bijvoorbeeld als u een webtoepassing met uw cluster implementeert wilt, voert u '80' hier voor verkeer op poort 80 in uw cluster. Zie voor meer informatie over eindpunten [communiceren met toepassingen][service-fabric-connect-and-communicate-with-services]
7. Configureren van cluster **diagnostics**. Diagnostische gegevens zijn standaard ingeschakeld op het cluster om te helpen bij het oplossen van problemen. Als u wilt uitschakelen, wijziging van diagnostische gegevens van de **Status** in-of uitschakelen op **uit**. Het uitschakelen van diagnostische gegevens is **niet** aanbevolen.
8. Selecteer de Fabric upgrade modus die u instellen van uw cluster wilt op. Selecteer **automatische**, als u wilt dat het systeem automatisch kunnen worden opgepikt de meest recente versie en probeer het bijwerken van uw cluster naar het. De modus instellen op **handmatige**, als u wilt kiezen van een ondersteunde versie.

> [!NOTE]
> Wij ondersteunen alleen clusters die met een ondersteunde versie van service Fabric. U selecteert de **handmatige** modus u onderneemt op de verantwoordelijkheid voor het upgraden van uw cluster naar een ondersteunde versie. Voor meer informatie over de Fabric-modus Zie upgrade de [service fabric-cluster upgrade-document.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Beveiliging
![Schermopname van beveiligingsconfiguraties op Azure-portal.][SecurityConfigs]

De laatste stap is het certificaat informatie als u wilt beveiligen van het cluster met behulp van de Sleutelkluis en een certificaat informatie eerder hebt gemaakt.

* Vul de velden van het primaire certificaat voor de uitvoer die is verkregen van het uploaden van de **cluster certificaat** Sleutelkluis via de `Invoke-AddCertToKeyVault` PowerShell-opdracht.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Controleer de **geavanceerde instellingen configureren** vak in te voeren voor clientcertificaten voor **Beheerclient** en **alleen-lezen client**. Voer de vingerafdruk van het clientcertificaat admin en de vingerafdruk van het certificaat alleen-lezengebruiker in, indien van toepassing in deze velden. Wanneer beheerders probeert verbinding maken met het cluster, krijgen ze toegang alleen als ze beschikken over een certificaat met een vingerafdruk die overeenkomt met de vingerafdruk waarden die u hier opgeeft.  

#### <a name="4-summary"></a>4. Samenvatting

Als u wilt maken van het cluster hebt voltooid, klikt u op **samenvatting** zien van de configuraties die u hebt opgegeven of download de Azure Resource Manager-sjabloon die die gebruikt voor het implementeren van uw cluster. Nadat u hebt opgegeven dat de instellingen verplicht de **OK** knop wordt groen en u kunt het proces voor het cluster maken starten door erop te klikken.

U kunt de voortgang van het maken in de meldingen bekijken. (Klik op het belpictogram bij de statusbalk rechtsboven in uw scherm.) Als u hebt geklikt **vastmaken aan Startboard** tijdens het maken van het cluster, ziet u **Service Fabric-Cluster implementeren** vastgemaakt aan de **Start** mededelingenbord.

### <a name="view-your-cluster-status"></a>De clusterstatus van uw bekijken
![Schermopname van het clusterdetails in het dashboard.][ClusterDashboard]

Als uw cluster is gemaakt, kunt u uw cluster in de portal controleren:

1. Ga naar **Bladeren** en klik op **Service Fabric-Clusters**.
2. Zoek uw cluster en klik erop.
3. U kunt de gegevens van uw cluster nu bekijken op het dashboard, waaronder het openbare eindpunt van het cluster en een koppeling naar Service Fabric Explorer.

De **knooppunt Monitor** gedeelte van het cluster dashboard blade geeft het aantal virtuele machines die in orde is en niet in orde zijn. U vindt meer informatie over de status van het cluster op [Service Fabric health model inleiding][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric-clusters vereisen een bepaald aantal knooppunten voor altijd voor beschikbaarheid onderhouden en het behouden van status - aangeduid als 'onderhouden quorum'. Daarom het is doorgaans niet veilig afsluiten alle machines in het cluster tenzij u eerst hebt uitgevoerd een [volledige back-up van de staat][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele-Machineschaalset-exemplaar of een clusterknooppunt
Elk van de NodeTypes geeft u in uw cluster resultaten in een virtuele-Machineschaalset ophalen van de installatie. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Volgende stappen
U hebt op dit moment een beveiligde cluster dat gebruik van certificaten voor beheer-verificatie. Vervolgens [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en meer informatie over hoe [toepassing geheimen beheren](service-fabric-application-secret-management.md).  Ook meer informatie over [Service Fabric-ondersteuningsopties](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
