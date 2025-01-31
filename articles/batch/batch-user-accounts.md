---
title: Uitvoeren van taken onder gebruikersaccounts - Azure Batch | Microsoft Docs
description: Gebruikersaccounts voor het uitvoeren van taken in Azure Batch configureren
services: batch
author: laurenhughes
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: bd5c16d755ef9b71f36b3d499838b12e6099ba6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65595378"
---
> [!NOTE] 
> De gebruikersaccounts die in dit artikel worden besproken verschilt van de gebruikers zijn accounts die worden gebruikt voor Remote Desktop Protocol (RDP) of Secure Shell (SSH), uit veiligheidsoverwegingen. 
>
> Zie voor verbinding met een knooppunt met de configuratie van de Linux-virtuele machine via SSH, [extern bureaublad gebruiken met een Linux-VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). Zie voor verbinding met knooppunten waarop Windows wordt uitgevoerd via RDP, [verbinding maken met een Windows Server VM](../virtual-machines/windows/connect-logon.md).<br /><br />
> Zie voor verbinding met een knooppunt met de cloudserviceconfiguratie via RDP, [extern bureaublad inschakelen voor een rol in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>


# <a name="run-tasks-under-user-accounts-in-batch"></a>Uitvoeren van taken onder gebruikersaccounts in Batch

Een taak in Azure Batch is altijd wordt uitgevoerd onder een gebruikersaccount. Standaard, taken worden uitgevoerd onder standaardgebruikersaccounts, zonder beheerdersmachtigingen. Deze standaardinstellingen voor het account van gebruiker zijn meestal voldoende. Voor bepaalde scenario's, maar is dit handig voor het configureren van het gebruikersaccount waarmee u wilt dat een taak uit te voeren. In dit artikel wordt beschreven welke typen gebruikersaccounts en hoe u ze kunt configureren voor uw scenario.

## <a name="types-of-user-accounts"></a>Typen gebruikersaccounts

Azure Batch biedt twee soorten gebruikersaccounts voor het uitvoeren van taken:

- **Automatisch-gebruikersaccounts.** Automatisch-gebruikersaccounts zijn ingebouwde gebruikersaccounts die automatisch zijn gemaakt door de Batch-service. Standaard, taken worden uitgevoerd onder een auto-gebruikersaccount. U kunt de specificatie auto-gebruiker voor een taak om aan te geven welke auto-gebruikersaccount dat een taak moet worden uitgevoerd. De specificatie auto-gebruiker kunt u opgeven de uitbreiding van bevoegdheden niveau en het bereik van de auto-gebruikersaccount op dat de taak wordt uitgevoerd. 

- **Een benoemde gebruiker-account.** U kunt een of meer benoemde gebruikersaccounts voor een pool opgeven wanneer u de pool maakt. Elk gebruikersaccount dat is gemaakt op elk knooppunt van de groep. Naast de accountnaam geeft u wachtwoord voor het gebruikersaccount, kan leiden tot misbruik niveau, en, voor Linux-pools, de persoonlijke SSH-sleutel. Wanneer u een taak toevoegt, kunt u het account van de benoemde gebruiker waarvan die taak moet worden uitgevoerd.

> [!IMPORTANT] 
> De Batch-service-versie 2017-01-01.4.0 introduceert belangrijke wijziging waarvoor uw code om aan te roepen die versie bij te werken. Als u de code migreren van een oudere versie van Batch, houd er rekening mee dat de **runElevated** eigenschap wordt niet meer ondersteund in de REST-API of Batch-clientbibliotheken. Gebruik de nieuwe **userIdentity** eigenschap van een taak om op te geven tot misbruik van bevoegdheden. Zie de sectie [uw code bijwerken naar de meest recente versie van Batch-clientbibliotheek](#update-your-code-to-the-latest-batch-client-library) voor snelle richtlijnen voor het bijwerken van uw Batch-code als u een van de clientbibliotheken.
>
>

## <a name="user-account-access-to-files-and-directories"></a>Toegang tot het gebruikersaccount op bestanden en mappen

Zowel een auto-gebruikersaccount en een gebruikersaccount met de naam hebben voor lezen/schrijven toegang tot de werkmap, gedeelde map en meerdere exemplaren taken directory van de taak. Beide typen accounts hebben leestoegang tot de opstart- en taak voorbereiding-mappen.

Als een taak wordt uitgevoerd onder hetzelfde account dat is gebruikt voor het uitvoeren van een starttaak, heeft de taak lees-/ schrijftoegang tot de map van de taak start. Op dezelfde manier als een taak wordt uitgevoerd onder hetzelfde account dat is gebruikt voor het uitvoeren van een jobvoorbereidingstaak, heeft de taak lees-/ schrijftoegang tot de taakmap voorbereiding-taak. Als een taak wordt uitgevoerd onder een ander account dan het begintaak of de jobvoorbereidingstaak, heeft de taak alleen leestoegang tot de betreffende map.

Zie voor meer informatie over de toegang tot bestanden en mappen van een taak [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Toegang met verhoogde bevoegdheid voor taken 

Het gebruikersaccount dat kan leiden tot misbruik niveau geeft aan of een taak wordt uitgevoerd met uitgebreide toegang. Zowel een auto-gebruikersaccount en een gebruikersaccount met de naam kunnen uitvoeren met toegang met verhoogde bevoegdheid. De twee opties voor het niveau van bevoegdheden zijn:

- **NonAdmin:** De taak wordt uitgevoerd als standaardgebruiker zonder toegang met verhoogde bevoegdheid. Het standaardniveau van de uitbreiding van bevoegdheden voor een Batch-gebruikersaccount is altijd **NonAdmin**.
- **Beheerder:** De taak wordt uitgevoerd als een gebruiker met toegang met verhoogde bevoegdheid en werkt met volledige beheerdersrechten. 

## <a name="auto-user-accounts"></a>Automatisch-gebruikersaccounts

Standaard uitvoeren taken in Batch in een auto-gebruikersaccount als standaardgebruiker zonder toegang met verhoogde bevoegdheid en met een bereik van de taak. Wanneer de gebruiker automatisch-specificatie is geconfigureerd voor het bereik van de taak, maakt de Batch-service een auto-gebruikersaccount voor die taak alleen.

Het alternatief voor het bereik van de taak is een bereik van de groep van toepassingen. Wanneer de specificatie auto-gebruiker voor een taak is geconfigureerd voor een bereik van de groep, wordt de taak wordt uitgevoerd onder een auto-gebruikersaccount op die beschikbaar is voor elke taak in de groep. Zie de sectie een taak uitvoeren als de auto-gebruiker met een bereik van de groep van toepassingen voor meer informatie over het bereik van de groep van toepassingen.   

Het bereik van standaard verschilt op Windows en Linux-knooppunten:

- Op Windows-knooppunten, taken standaard uitgevoerd onder bereik van de taak.
- Linux-knooppunten kan altijd worden uitgevoerd onder het bereik van de groep van toepassingen.

Er zijn vier mogelijke configuraties voor de specificatie van auto-gebruiker, die elk komt met een unieke auto-gebruikersaccount overeen:

- Niet-beheerder toegang met taak-bereik (de standaard automatisch-gebruiker-specificatie)
- Beheerderstoegang (verhoogd) met een bereik van de taak
- Niet-beheerder toegang met een bereik van de groep van toepassingen
- Beheerderstoegang met een bereik van de groep van toepassingen

> [!IMPORTANT] 
> Taken die worden uitgevoerd onder bereik van de taak hebt feitelijke toegang tot andere taken op een knooppunt niet. Echter, een kwaadwillende gebruiker met toegang tot het account kan deze beperking omzeilen door het indienen van een taak die wordt uitgevoerd met administrator-bevoegdheden en toegang heeft tot de mappen van andere taak. Een kwaadwillende gebruiker kan ook gebruiken via RDP of SSH verbinding maken met een knooppunt. Het is belangrijk om te beveiligen van toegang tot de sleutels van uw Batch-account om te voorkomen dat een dergelijk scenario. Als u vermoedt dat uw account is aangetast dat, zorg er dan voor dat uw sleutels opnieuw genereren.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Een taak uitvoert als een auto-gebruiker met toegang met verhoogde bevoegdheid

Wanneer u een taak uitvoert met uitgebreide toegang nodig hebt, kunt u de auto-gebruiker-specificatie voor de administrator-bevoegdheden configureren. Een begintaak moet mogelijk de toegang met verhoogde bevoegdheid om software te installeren op het knooppunt.

> [!NOTE] 
> In het algemeen is het raadzaam om te gebruiken toegang met verhoogde bevoegdheid alleen indien nodig. Aanbevolen procedures raden aan de minimale bevoegdheden die nodig zijn voor het bereiken van het gewenste resultaat te verlenen. Bijvoorbeeld, als een begintaak software voor de huidige gebruiker, in plaats van voor alle gebruikers installeert, u mogelijk om te voorkomen dat het verlenen van toegang met verhoogde bevoegdheid naar taken. U kunt de specificatie auto-gebruiker voor de pool bereik en niet-beheerders toegang voor alle taken die moeten worden uitgevoerd onder de dezelfde account, met inbegrip van de begintaak configureren. 
>
>

De volgende codefragmenten laten zien hoe het configureren van de gebruiker automatisch-specificatie. In de voorbeelden ingesteld voor de uitbreiding op `Admin` en de scope `Task`. Bereik van de taak is de standaardinstelling, maar is hier opgenomen om het voorbeeld te houden.

#### <a name="batch-net"></a>Batch .NET

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Batch Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Batch Python

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Een taak uitvoert als een auto-gebruiker met een bereik van de groep van toepassingen

Wanneer een knooppunt is ingericht, twee gehele pool automatisch-gebruikersaccounts zijn gemaakt op elk knooppunt in de groep, één met toegang met verhoogde bevoegdheid en regel zonder toegang met verhoogde bevoegdheid. Bereik van een auto-van de gebruiker aan het bereik van de groep van toepassingen voor een bepaalde taak instellen: de taak onder een van deze twee accounts voor de hele pool automatisch-gebruiker wordt uitgevoerd. 

Wanneer u toepassingen bereik voor de automatische-gebruiker, alle taken die worden uitgevoerd met beheerderstoegang uitgevoerd onder de dezelfde groep hele auto-gebruikersaccount opgeeft. Op deze manier taken die worden uitgevoerd zonder beheerdersmachtigingen wordt ook uitgevoerd onder een account met één groep hele automatisch-gebruiker. 

> [!NOTE] 
> De twee toepassingen hele auto-gebruikersaccounts zijn afzonderlijke accounts. Taken die worden uitgevoerd onder de hele groep Administrator-account kunnen geen gegevens delen met taken die worden uitgevoerd onder de standard-account, en vice versa. 
>
>

Het voordeel aan die wordt uitgevoerd onder de dezelfde auto-gebruikersaccount is dat de taken kunnen gegevens delen met andere taken worden uitgevoerd op hetzelfde knooppunt zijn.

Geheimen tussen taken delen is een scenario waarin actieve taken volgens een van de twee toepassingen hele auto-gebruikersaccounts handig is. Stel bijvoorbeeld dat een begintaak moet voor het inrichten van een geheim op het knooppunt dat andere taken kunnen gebruiken. U kunt de Windows Data Protection API (DPAPI), maar er zijn beheerdersbevoegdheden vereist. U kunt de geheime sleutel die op het gebruikersniveau van de in plaats daarvan kunt beveiligen. Taken die worden uitgevoerd onder de dezelfde gebruikersaccount hebben toegang tot het geheim zonder toegang met verhoogde bevoegdheid.

Een ander scenario waar u mogelijk wilt uitvoeren van taken in een auto-gebruikersaccount met bereik van de pool is een bestandsshare Message Passing Interface (MPI). Een MPI-bestandsshare is nuttig wanneer de knooppunten in de MPI-taak moeten werken op de dezelfde gegevens uit een bestand. Het hoofdknooppunt maakt u een bestandsshare die de onderliggende knooppunten toegang hebben tot als ze worden uitgevoerd onder de dezelfde auto-gebruikersaccount. 

Het volgende codefragment stelt de auto-gebruiker scope aan het bereik van de groep van toepassingen voor een taak in Batch .NET. Het niveau van de uitbreiding van bevoegdheden wordt weggelaten, zodat de taak wordt uitgevoerd onder de hele pool automatisch-standaardgebruikersaccount.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Benoemde gebruiker-accounts

Wanneer u een pool maakt, kunt u accounts van de benoemde gebruiker definiëren. Een benoemde gebruiker-account heeft een naam en wachtwoord die u opgeeft. U kunt het niveau van de uitbreiding van bevoegdheden voor een gebruikersaccount met de naam opgeven. U kunt ook een persoonlijke SSH-sleutel opgeven voor Linux-knooppunten.

Een gebruikersaccount met de naam bestaat op alle knooppunten in de groep en is beschikbaar voor alle taken die worden uitgevoerd op deze knooppunten. U kunt een willekeurig aantal benoemde gebruikers voor een groep definiëren. Wanneer u een taak of een verzameling van de taak toevoegt, kunt u opgeven dat de taak wordt uitgevoerd onder een van de benoemde gebruiker-accounts dat is gedefinieerd in de pool.

Een gebruikersaccount met de naam is handig als u wilt alle taken uitvoeren in een taak in het hetzelfde gebruikersaccount, maar ze van taken die in andere taken worden uitgevoerd op hetzelfde moment isoleren. U kunt bijvoorbeeld een benoemde gebruiker voor elke taak maken en uitvoeren van de taken van elke taak onder dat account benoemde gebruiker. Elke taak kan een geheim vervolgens delen met een eigen taken, maar niet met taken die in andere taken worden uitgevoerd.

U kunt ook een benoemde gebruiker-account gebruiken om uit te voeren van een taak die stelt de machtigingen van externe bronnen, zoals bestandsshares. Met een gebruikersaccount met de naam bepalen van de gebruikers-id en de identiteit van die gebruiker kunt gebruiken om machtigingen te stellen.  

Benoemde gebruikersaccounts inschakelen SSH zonder wachtwoord tussen de Linux-knooppunten. U kunt een benoemde gebruiker-account gebruiken met Linux-knooppunten die u wilt uitvoeren van taken met meerdere instanties. Elk knooppunt in de pool kan taken onder een gebruikersaccount dat is gedefinieerd in de hele groep uitvoeren. Zie voor meer informatie over taken met meerdere instanties, [gebruik van meerdere\-taken voor het uitvoeren van MPI-toepassingen van exemplaar van](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Benoemde gebruiker-accounts maken

Benoemde gebruiker om accounts te maken in Batch, moet u een verzameling van gebruikersaccounts toevoegen aan de groep. De volgende codefragmenten laten zien hoe u met de naam om gebruikersaccounts te maken in .NET, Java en Python. Deze codefragmenten laten zien hoe admin en niet-beheerders met de naam van accounts in een groep maken. De voorbeelden pools met behulp van de configuratie van de cloud service maken, maar u dezelfde benadering gebruiken bij het maken van een Windows- of Linux-toepassingen met behulp van de configuratie van de virtuele machine.

#### <a name="batch-net-example-windows"></a>Batch .NET-voorbeeld (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Batch .NET-voorbeeld (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Batch Java-voorbeeld

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Batch Python-voorbeeld

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Een taak in een benoemde gebruikersaccount met toegang met verhoogde bevoegdheid uitvoeren

Als u wilt een taak uitvoert als een gebruiker met verhoogde bevoegdheid, instellen van de taak **UserIdentity** eigenschap aan een benoemde gebruikersaccount dat is gemaakt met de **ElevationLevel** eigenschap ingesteld op `Admin`.

Dit codefragment geeft aan dat de taak wordt uitgevoerd onder een gebruikersaccount met de naam. Dit account de benoemde gebruiker is gedefinieerd in de groep wanneer de pool is gemaakt. In dit geval is de benoemde gebruiker-account gemaakt met de beheerdersmachtigingen:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a>Uw code bijwerken naar de meest recente versie van Batch-clientbibliotheek

De Batch-service-versie 2017-01-01.4.0 introduceert belangrijke wijziging, vervangen de **runElevated** eigenschap beschikbaar in eerdere versies met de **userIdentity** eigenschap. De volgende tabellen bevatten een eenvoudige toewijzing die u gebruiken kunt om bij te werken van uw code uit eerdere versies van de clientbibliotheken.

### <a name="batch-net"></a>Batch .NET

| Als uw code wordt gebruikgemaakt van...                  | Het bijwerken...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated` Niet opgegeven | Er is geen update vereist                                                                                               |

### <a name="batch-java"></a>Batch Java

| Als uw code wordt gebruikgemaakt van...                      | Het bijwerken...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated` Niet opgegeven | Er is geen update vereist                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Als uw code wordt gebruikgemaakt van...                      | Het bijwerken...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, waarbij <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin))`                |
| `run_elevated=False`                      | `user_identity=user`, waarbij <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin))`             |
| `run_elevated` Niet opgegeven | Er is geen update vereist                                                                                                                                  |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een gedetailleerd overzicht van Batch, [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md).
