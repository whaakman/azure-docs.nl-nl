---
title: Uitvoeren van taken onder gebruikersaccounts in Azure Batch | Microsoft Docs
description: Gebruikersaccounts voor het uitvoeren van taken in Azure Batch configureren
services: batch
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.openlocfilehash: d408c0565c0ed81fc97cc2b3976a4fc233e31302
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Taken onder gebruikersaccounts in Batch uitvoeren

Een taak in Azure Batch wordt altijd uitgevoerd onder een gebruikersaccount. Taken worden standaard uitgevoerd onder standaardgebruikersaccounts zonder beheerdersrechten. Deze standaardinstellingen voor het account van gebruiker zijn meestal voldoende. Voor bepaalde scenario's is het echter handig om te kunnen configureren van het gebruikersaccount waaronder u wilt dat een taak wordt uitgevoerd. Dit artikel wordt beschreven welke typen gebruikersaccounts en hoe u ze kunt configureren voor uw scenario.

## <a name="types-of-user-accounts"></a>Typen gebruikersaccounts

Azure Batch biedt twee soorten gebruikersaccounts voor het uitvoeren van taken:

- **Automatische gebruikersaccounts.** Automatische-gebruikersaccounts zijn ingebouwde gebruikersaccounts die automatisch worden gemaakt door de Batch-service. Taken worden standaard uitgevoerd onder een auto-gebruikersaccount. U kunt de specificatie automatisch gebruiker voor een taak om aan te geven onder welke automatisch gebruikersaccount een taak moet worden uitgevoerd. De specificatie auto-gebruiker kunt u opgeven van het niveau van de uitbreiding van bevoegdheden en het bereik van de automatische-gebruikersaccount op dat de taak wordt uitgevoerd. 

- **Een benoemde gebruikersaccount.** U kunt een of meer benoemde gebruikersaccounts voor een pool opgeven wanneer u de groep maakt. Elk gebruikersaccount wordt gemaakt op elk knooppunt van de groep. Naast de accountnaam Geef wachtwoord voor het gebruikersaccount, uitbreiding van bevoegdheden niveau en, voor Linux-toepassingen, de persoonlijke SSH-sleutel. Wanneer u een taak toevoegt, kunt u de benoemde gebruikersaccount waaronder de taak die moet worden uitgevoerd.

> [!IMPORTANT] 
> Versie van de Batch-service 2017-01-01.4.0 introduceert een belangrijke wijziging die is vereist dat u uw code om aan te roepen die versie bijwerken. Als u de code migreren van een oudere versie van Batch, houd er rekening mee dat de **runElevated** eigenschap wordt niet meer ondersteund in de REST-API of Batch-clientbibliotheken. Gebruik het nieuwe **userIdentity** eigenschap van een taak om uitbreiding van bevoegdheden op te geven. Zie de sectie [werk uw code naar de meest recente Batch-clientbibliotheek](#update-your-code-to-the-latest-batch-client-library) voor snelle richtlijnen voor het bijwerken van uw Batch-code als u een van de clientbibliotheken.
>
>

> [!NOTE] 
> De gebruikersaccounts die in dit artikel wordt besproken ondersteunen geen Remote Desktop Protocol (RDP) of Secure Shell (SSH), uit veiligheidsoverwegingen. 
>
> Zie voor verbinding met een knooppunt met de configuratie van de Linux-virtuele machine via SSH, [extern bureaublad gebruiken voor een Linux-VM in Azure](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). Zie voor verbinding met knooppunten waarop Windows wordt uitgevoerd via RDP, [verbinding maken met een Windows Server VM](../virtual-machines/windows/connect-logon.md).<br /><br />
> Zie voor verbinding met een knooppunt met de configuratie van de cloud-service via RDP, [extern bureaublad inschakelen voor een rol in Azure Cloud Services](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-to-files-and-directories"></a>Gebruikersaccount toegang tot bestanden en mappen

Zowel een auto-gebruikersaccount en een benoemde gebruikersaccount hebben lezen/schrijven toegang tot de werkmap, gedeelde map en meerdere exemplaren taken directory van de taak. Beide typen accounts beschikken over leestoegang tot de opstart- en voorbereiding-mappen.

Als een taak wordt uitgevoerd onder hetzelfde account dat is gebruikt voor het uitvoeren van een taak gestart, heeft de taak lees-/ schrijftoegang tot de map van de taak is gestart. Op dezelfde manier als een taak wordt uitgevoerd onder hetzelfde account dat is gebruikt voor het uitvoeren van een jobvoorbereidingstaak, de taak lezen-schrijven toegang heeft tot de taakmap taak voorbereiding. Als een taak wordt uitgevoerd onder een ander account dan de begintaak of jobvoorbereidingstaak, heeft de taak alleen leestoegang tot de betreffende map.

Zie voor meer informatie over de toegang tot bestanden en mappen uit een taak [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Toegang met verhoogde bevoegdheid voor taken 

Het gebruikersaccount uitbreiding van bevoegdheden niveau geeft aan of een taak wordt uitgevoerd met verhoogde toegang. Zowel een auto-gebruikersaccount en een benoemde gebruikersaccount kunnen uitvoeren met verhoogde toegang. De twee opties voor uitbreiding van bevoegdheden niveau zijn:

- **NonAdmin:** de taak wordt uitgevoerd als standaardgebruiker zonder verhoogde toegang. Het standaardniveau voor uitbreiding van bevoegdheden voor een Batch-gebruikersaccount is altijd **NonAdmin**.
- **Admin:** de taak wordt uitgevoerd als een gebruiker met toegang met verhoogde bevoegdheid en werkt met volledige Administrator-machtigingen. 

## <a name="auto-user-accounts"></a>Automatische gebruikersaccounts

Standaard uitvoeren taken in Batch onder een auto-gebruikersaccount als standaardgebruiker zonder verhoogde toegang tot en met bereik van de taak. Wanneer de gebruiker automatisch-specificatie voor taak scope is geconfigureerd, maakt de Batch-service een automatische-account voor alleen die taak.

Het alternatief voor het bereik van de taak is bereik van de groep. Wanneer de gebruiker automatisch-specificatie voor een taak voor het bereik van de groep van toepassingen is geconfigureerd, wordt de taak wordt uitgevoerd onder een auto-gebruikersaccount op dat beschikbaar is voor elke taak in de groep. Zie het gedeelte voor meer informatie over toepassingen bereik [een taak uitvoeren als de automatische-gebruiker met een bereik van de groep](#run-a-task-as-the-autouser-with-pool-scope).   

Het standaardbereik verschilt op Windows- en Linux-knooppunten:

- Op Windows-knooppunten taken standaard uitgevoerd onder het bereik van de taak.
- Linux-knooppunten kan altijd worden uitgevoerd onder het bereik van de groep van toepassingen.

Er zijn vier mogelijke configuraties voor de gebruiker automatisch-specificatie die met een gebruikersaccount van de unieke automatisch overeenkomen:

- Niet-beheerder toegang met taak-bereik (standaard automatisch gebruiker specificatie)
- Beheerderstoegang (verhoogd) met een bereik van de taak
- Niet-beheerder toegang met een bereik van de groep van toepassingen
- Beheerderstoegang met een bereik van de groep van toepassingen

> [!IMPORTANT] 
> Taken die worden uitgevoerd onder het bereik van de taak bent niet gemachtigd feitelijke aan andere taken op een knooppunt. Echter, een kwaadwillende gebruiker met toegang tot het account kan deze beperking omzeilen door het indienen van een taak die wordt uitgevoerd met administrator-bevoegdheden en heeft toegang tot andere mappen taak. Een kwaadwillende gebruiker kan verbinding maken met een knooppunt ook RDP of SSH kunt gebruiken. Het is belangrijk om te beveiligen van toegang tot de sleutels van uw Batch-account om te voorkomen dat dergelijke scenario. Als u vermoedt dat uw account is geknoeid, zorg er dan voor dat uw sleutels genereren.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Een taak uitvoeren als een auto-gebruiker met uitgebreide toegang

Als u een taak uitvoeren met verhoogde toegang nodig hebt, kunt u de gebruiker automatisch-specificatie voor de administrator-bevoegdheden configureren. Een begintaak moet mogelijk de toegang met verhoogde bevoegdheid om software te installeren op het knooppunt.

> [!NOTE] 
> In het algemeen is het beste verhoogde toegang alleen indien nodig gebruiken. Aanbevolen de minimale bevoegdheden die nodig zijn voor het bereiken van het gewenste resultaat verlenen. Bijvoorbeeld, als een begintaak software voor de huidige gebruiker, in plaats van voor alle gebruikers installeert wellicht kunt u voorkomen met verhoogde bevoegdheden geen toegang verlenen tot taken. U kunt de specificatie automatisch gebruiker voor de scope en niet-beheerders toegang groep voor alle taken die moeten worden uitgevoerd onder hetzelfde account, met inbegrip van de begintaak configureren. 
>
>

De volgende codefragmenten laten zien hoe de specificatie van de gebruiker automatisch configureren. De voorbeelden ingesteld de uitbreiding van bevoegdheden op `Admin` en de scope `Task`. Bereik van de taak is de standaardinstelling, maar is hier opgenomen om het voorbeeld te houden.

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

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Een taak uitvoeren als een auto-gebruiker met een bereik van de groep van toepassingen

Wanneer een knooppunt is ingericht, twee hele groep automatisch-gebruikersaccounts zijn gemaakt op elk knooppunt in de groep, één met verhoogde toegang en één zonder verhoogde toegang. Het bereik van de automatische-gebruiker aan groep-bereik voor een bepaalde taak instellen, voert de taak onder een van deze twee hele pool automatisch-gebruikersaccounts. 

Wanneer u toepassingen bereik voor de automatische-gebruiker, alle taken die worden uitgevoerd met beheerdersbevoegdheden uitgevoerd onder dezelfde groep wide auto-gebruikersaccount opgeven. Taken die worden uitgevoerd zonder beheerdersrechten uitgevoerd op deze manier ook onder één groep wide auto-gebruikersaccount. 

> [!NOTE] 
> De twee toepassingen wide auto-gebruikersaccounts zijn afzonderlijke accounts. Taken die worden uitgevoerd onder de hele groep Administrator-account kunnen geen gegevens delen met taken die worden uitgevoerd onder de standaard-account en vice versa. 
>
>

Het voordeel in wordt uitgevoerd onder dezelfde auto-gebruikersaccount is dat taken kunnen geen gegevens delen met andere taken op hetzelfde knooppunt uitvoert.

Delen van geheimen tussen taken is een scenario waarin actieve taken onder een van de twee toepassingen wide auto-gebruikersaccounts handig is. Stel bijvoorbeeld dat een begintaak moet voor het inrichten van een geheim naar het knooppunt dat andere taken kunnen gebruiken. U kunt Windows Data Protection API (DPAPI), maar het administrator-bevoegdheden vereist. In plaats daarvan kunt u het geheim op gebruikersniveau beveiligen. Taken die worden uitgevoerd onder dezelfde gebruikersaccount hebben toegang tot het geheim zonder verhoogde toegang.

Een ander scenario waar u mogelijk wilt uitvoeren van taken onder een gebruikersaccount op de automatische met bereik van de groep van toepassingen is een bestandsshare Message Passing Interface (MPI). Een MPI-bestandsshare is handig wanneer de knooppunten in de MPI-taak moeten werken op dezelfde bestandsgegevens. Het hoofdknooppunt maakt een bestandsshare die de onderliggende knooppunten toegang hebben tot als ze worden uitgevoerd onder dezelfde auto-gebruikersaccount. 

Het volgende codefragment stelt de automatische-gebruiker scope aan groep-bereik voor een taak in Batch .NET. Het niveau van de uitbreiding van bevoegdheden wordt weggelaten, zodat de taak wordt uitgevoerd onder de hele groep automatisch-standaardgebruikersaccount.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Benoemde gebruikersaccounts

Wanneer u een groep maakt, kunt u met de naam gebruikersaccounts definiëren. Een benoemde gebruikersaccount heeft een naam en het wachtwoord dat u opgeeft. U kunt het niveau van de uitbreiding van bevoegdheden voor een benoemde gebruikersaccount opgeven. U kunt ook een persoonlijke SSH-sleutel voor Linux-knooppunten opgeven.

Een gebruikersaccount met de naam bestaat op alle knooppunten in de groep en is beschikbaar voor alle taken die worden uitgevoerd op die knooppunten. U kunt een willekeurig aantal benoemde gebruikers voor een groep definiëren. Wanneer u een taak of taak verzameling toevoegt, kunt u opgeven dat de taak wordt uitgevoerd onder een van de benoemde gebruikersaccounts die zijn gedefinieerd in de groep.

Een gebruikersaccount met de naam is handig als u wilt alle taken uitvoeren in een taak onder hetzelfde gebruikersaccount, maar ze van taken die in andere taken worden uitgevoerd op hetzelfde moment te isoleren. U kunt bijvoorbeeld een benoemde gebruiker voor elke taak maken en uitvoeren van taken onder een gebruikersaccount met de naam van elke taak. Vervolgens kunt elke taak een geheim delen met een eigen taken, maar niet met taken die in andere taken worden uitgevoerd.

U kunt ook een gebruikersaccount met de naam voor het uitvoeren van een taak die op externe bronnen zoals bestandsshares worden machtigingen ingesteld. Met een benoemde gebruikersaccount bepalen van de gebruikers-id en de identiteit van die gebruiker machtigingen instellen kunt gebruiken.  

Benoemde gebruikersaccounts inschakelen wachtwoordloze SSH tussen Linux-knooppunten. U kunt een benoemde gebruikersaccount gebruiken met Linux-knooppunten die moeten worden uitgevoerd van taken met meerdere instanties. Elk knooppunt in de pool kan taken onder een gebruikersaccount dat is gedefinieerd voor de hele groep uitvoeren. Zie voor meer informatie over taken met meerdere instanties [gebruik van meerdere\-taken MPI-toepassingen uit te voeren van exemplaar van](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Benoemde gebruikersaccounts maken

Voor benoemde gebruikersaccounts in Batch maakt, moet u een verzameling van gebruikersaccounts toevoegen aan de groep. De volgende codefragmenten laten zien hoe gebruikersaccounts te maken met de naam in .NET, Java en Python. Deze codefragmenten laten zien hoe admin en niet-beheerders accounts op een groep met de naam maken. De voorbeelden van toepassingen met behulp van de configuratie van de cloud-service maken, maar u dezelfde manier gebruiken bij het maken van een Windows- of Linux-toepassingen met behulp van de configuratie van de virtuele machine.

#### <a name="batch-net-example-windows"></a>Batch .NET-voorbeeld (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Een taak onder een gebruikersaccount met de naam uitvoeren met verhoogde toegang

Als een taak wordt uitgevoerd als een gebruiker met verhoogde bevoegdheid, instellen van de taak **UserIdentity** eigenschap aan een benoemde gebruikersaccount dat is gemaakt met de **ElevationLevel** eigenschap ingesteld op `Admin`.

Dit codefragment geeft aan dat de taak wordt uitgevoerd onder een gebruikersaccount met de naam. Dit account benoemde gebruiker is gedefinieerd in de groep wanneer de groep is gemaakt. In dit geval is de benoemde gebruiker-account gemaakt met de beheerdersmachtigingen:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a>Werk uw code naar de meest recente Batch-clientbibliotheek

Versie van de Batch-service 2017-01-01.4.0 introduceert een belangrijke wijziging, vervangt de **runElevated** eigenschap beschikbaar in eerdere versies met de **userIdentity** eigenschap. De volgende tabellen bevatten een eenvoudige toewijzing die u gebruiken kunt om bij te werken van uw code uit eerdere versies van de clientbibliotheken.

### <a name="batch-net"></a>Batch .NET

| Als uw code gebruikt...                  | Bijwerken naar...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated`niet opgegeven | Er is geen update vereist                                                                                               |

### <a name="batch-java"></a>Batch Java

| Als uw code gebruikt...                      | Bijwerken naar...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated`niet opgegeven | Er is geen update vereist                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Als uw code gebruikt...                      | Bijwerken naar...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, waarbij <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | `user_identity=user`, waarbij <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| `run_elevated`niet opgegeven | Er is geen update vereist                                                                                                                                  |


## <a name="next-steps"></a>Volgende stappen

### <a name="batch-forum"></a>Batch-Forum

De [Azure Batch-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) is een goede plaats om te bespreken Batch en vragen over de service op MSDN. Kop op via voor nuttige vastgemaakt berichten, en stel uw vragen wanneer deze zich voordoen tijdens het bouwen van uw Batch-oplossingen.