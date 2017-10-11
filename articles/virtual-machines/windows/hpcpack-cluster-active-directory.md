---
title: HPC Pack cluster met Azure Active Directory | Microsoft Docs
description: Meer informatie over het integreren van een HPC Pack 2016-cluster in Azure met Azure Active Directory
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: c5a06a9c810349b1bcce01c7f73563941a5af0ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Een HPC Pack cluster in Azure met Azure Active Directory beheren
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) ondersteunt de integratie met [Azure Active Directory](../../active-directory/index.md) (Azure AD) voor beheerders die een HPC Pack cluster in Azure implementeren.



Volg de stappen in dit artikel voor de volgende taken op hoog niveau: 
* Uw cluster HPC Pack handmatig te integreren met uw Azure AD-tenant
* Beheren en plannen van taken in uw cluster HPC Pack in Azure 

Een clusteroplossing HPC Pack integreren met Azure AD volgt standaard stappen voor het integreren van andere toepassingen en services. In dit artikel wordt ervan uitgegaan dat u bekend bent met basisgebruiker management in Azure AD. Zie voor meer informatie en achtergrond, de [Azure Active Directory-documentatie](../../active-directory/index.md) en de volgende sectie.

## <a name="benefits-of-integration"></a>Voordelen van de integratie


Azure Active Directory (Azure AD) is een multitenant cloud-gebaseerde directory en identity management-service dat eenmalige aanmelding (SSO) toegang tot cloudoplossingen biedt.

Integratie van een cluster HPC Pack met Azure AD kunt u de volgende drie doelen te bereiken:

* Verwijder de traditionele Active Directory-domeincontroller uit het cluster HPC Pack. Zo kunt u de kosten van het onderhoud van het cluster als dit niet nodig zijn voor uw bedrijf en versnellen het implementatieproces is te verlagen.
* Gebruikmaken van de volgende voordelen die worden uitgevoerd door Azure AD:
    *   Eenmalige aanmelding 
    *   Met behulp van een lokale AD-identiteit voor het cluster HPC Pack in Azure 

    ![Azure Active Directory-omgeving](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Vereisten
* **HPC Pack 2016 cluster is geïmplementeerd in virtuele machines in Azure** - voor de stappen, Zie [een HPC Pack 2016-cluster in Azure implementeren](hpcpack-2016-cluster.md). U moet de DNS-naam van het hoofdknooppunt en de referenties van de Clusterbeheerder van een om de stappen in dit artikel te voltooien.

  > [!NOTE]
  > Azure Active Directory-integratie wordt niet ondersteund in versies van HPC Pack vóór HPC Pack 2016.



* **Clientcomputer** -moet u een clientcomputer Windows of Windows Server voor het uitvoeren van HPC Pack client-hulpprogramma's. Als u alleen de HPC Pack web-portal of REST-API gebruiken om taken te verzenden wilt, kunt u elke clientcomputer van uw keuze.

* **Hulpprogramma's voor HPC Pack client** -de hulpprogramma's voor HPC Pack client installeren op de clientcomputer, met behulp van de beschikbare vrije installatiepakket van het Microsoft Download Center.


## <a name="step-1-register-the-hpc-cluster-server-with-your-azure-ad-tenant"></a>Stap 1: De HPC-cluster-server geregistreerd met uw Azure AD-tenant
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Klik op **Active Directory** in het menu links en klik vervolgens op de gewenste map in uw abonnement. U moet gemachtigd zijn voor toegang tot bronnen in de directory.
3. Klik op **gebruikers**, en controleer of er gebruikersaccounts al gemaakt of geconfigureerd.
4. Klik op **toepassingen** > **toevoegen**, en klik vervolgens op **mijn organisatie ontwikkelt toepassing toevoegen**. Voer de volgende gegevens in de wizard:
    * **Naam** -HPCPackClusterServer
    * **Type** : Selecteer **Web-toepassing en/of Web-API**
    * **Eenmalige aanmelding URL**-de basis-URL voor de steekproef die standaard is`https://hpcserver`
    * **App ID URI** - `https://<Directory_name>/<application_name>`. Vervang `<Directory_name`> met de volledige naam van uw Azure AD-tenant, bijvoorbeeld `hpclocal.onmicrosoft.com`, en vervang `<application_name>` met de naam die u eerder hebt gekozen.

5. Nadat de app wordt toegevoegd, klikt u op **configureren**. Configureer de volgende eigenschappen:
    * Selecteer **Ja** voor **toepassing multitenant is**
    * Selecteer **Ja** voor **Gebruikerstoewijzing vereist voor toegang tot app**.

6. Klik op **Opslaan**. Wanneer opslaan is voltooid, klikt u op **beheren Manifest**. Deze actie downloadt van uw toepassing JavaScript object notation (JSON) manifestbestand. Het gedownloade manifest bewerken door het zoeken naar de `appRoles` instellen en de volgende toepassingsrol toevoegen:
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. Sla het bestand op. Klik in de portal **beheren Manifest** > **uploaden Manifest**. Vervolgens kunt u het bewerkte manifest uploaden.
8. Klik op **gebruikers**, selecteert u een gebruiker en klik vervolgens op **toewijzen**. Een van de beschikbare rollen (HpcUsers of HpcAdminMirror) toewijzen aan de gebruiker. Herhaal deze stap met extra gebruikers in de map. Zie voor achtergrondinformatie over cluster gebruikers, [Cluster gebruikers beheren](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).

   > [!NOTE] 
   > Voor het beheren van gebruikers, wordt u aangeraden de Azure Active Directory-preview-blade in de [Azure-portal](https://portal.azure.com).
   >


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a>Stap 2: De client HPC-cluster registreren bij uw Azure AD-tenant

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Klik op **Active Directory** in het menu links en klik vervolgens op de gewenste map in uw abonnement. U moet gemachtigd zijn voor toegang tot bronnen in de directory.
3. Klik op **toepassingen** > **toevoegen**, en klik vervolgens op **mijn organisatie ontwikkelt toepassing toevoegen**. Voer de volgende gegevens in de wizard:

    * **Naam** -HPCPackClusterClient
    * **Type** : Selecteer **Native Client-toepassing**
    * **Omleidings-URI** - `http://hpcclient`

4. Nadat de app wordt toegevoegd, klikt u op **configureren**. Kopieer de **Client-ID** waarde en op te slaan. U nodig dit later bij het configureren van uw toepassing.

5. In **machtigingen voor andere toepassingen**, klikt u op **toepassing toevoegen**. Zoeken en toevoegen aan de HpcPackClusterServer-toepassing (gemaakt in stap 1).

6. In de **gedelegeerde machtigingen** vervolgkeuzelijst **toegang HpcClusterServer**. Klik vervolgens op **Opslaan**.


## <a name="step-3-configure-the-hpc-cluster"></a>Stap 3: De HPC-cluster configureren

1. Verbinding maken met het hoofdknooppunt van HPC Pack 2016 in Azure.

2. Start HPC PowerShell.

3. Voer de volgende opdracht uit:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    waar

    * `AADTenant`Hiermee geeft u de naam van de Azure AD-tenant, zoals`hpclocal.onmicrosoft.com`
    * `AADClientAppId`Hiermee geeft u de client-ID voor de app gemaakt in stap 2.

4. Start de service HpcSchedulerStateful.

    In een cluster met meerdere hoofdknooppunten, kunt u de volgende PowerShell-opdrachten uitvoeren op het hoofdknooppunt overschakelen van de primaire replica voor de service HpcSchedulerStateful:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-the-client"></a>Stap 4: Beheren en verzenden van taken van de client

Voor het installeren van de hulpprogramma's voor HPC Pack client op uw computer, de HPC Pack 2016 setup-bestanden (volledige installatie) te downloaden vanaf het Microsoft Download Center. Wanneer u de installatie begint, selecteer de installatieoptie voor de **HPC Pack client utilities**.

Als u met het voorbereiden van de clientcomputer, installeert u het certificaat wordt gebruikt tijdens [HPC cluster setup](hpcpack-2016-cluster.md) op de clientcomputer. Windows certificate management standaardprocedures gebruiken voor het installeren van het openbare certificaat naar de **certificaten-huidige gebruiker** > **Trusted Root Certification Authorities** opslaan. 

U kunt nu de HPC Pack opdrachten uitvoeren of HPC Pack Job manager GUI gebruiken voor het verzenden en cluster taken beheren met behulp van de Azure AD-account. Zie voor de opties voor het indienen van taak [taken op een HPC Pack indienen HPC-cluster in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> Wanneer u een verbinding met het cluster HPC Pack in Azure voor het eerst uitvoert, verschijnt een pop-upvensters. Voer uw Azure AD-referenties aan te melden. Het token wordt vervolgens in de cache opgeslagen. Hoger verbindingen met het cluster in Azure gebruiken de token in cache tenzij gewijzigde verificatie of de cache is uitgeschakeld.
>
  
Bijvoorbeeld, na het voltooien van de vorige stappen u kunt een query voor de taken van een on-premises client als volgt:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Handig cmdlets taak te verzenden met Azure AD-integratie 

### <a name="manage-the-local-token-cache"></a>De lokale tokencache beheren

HPC Pack 2016 biedt twee nieuwe HPC PowerShell-cmdlets voor het beheren van de lokale cache van de token. Deze cmdlets zijn handig voor het verzenden van taken niet-interactief. Zie het volgende voorbeeld:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a>Stel de referenties voor het indienen van taken met behulp van de Azure AD-account 

Soms wilt u de taak onder de gebruiker HPC-cluster (voor een domein HPC-cluster, voert u als een domeingebruiker; voor een niet-domein HPC-cluster moet uitvoeren als een lokale gebruiker op het hoofdknooppunt) uitvoeren.

1. Gebruik de volgende opdrachten de referenties in te stellen:

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Vervolgens dient u de taak als volgt. De taak wordt uitgevoerd onder $localUser op de rekenknooppunten.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Als `–Credential` niet wordt opgegeven met `Submit-HpcJob`, de functie of de taak wordt uitgevoerd onder een lokale toegewezen gebruiker als de Azure AD-account. (De HPC-cluster maakt een lokale gebruiker met dezelfde naam als de Azure AD-account voor de taak).
    
3. Uitgebreide gegevens voor de Azure AD-account instellen. Dit is handig wanneer een MPI-taak wordt uitgevoerd op Linux-knooppunten met behulp van de Azure AD-account.

   * Uitgebreide gegevens voor de Azure AD-account zelf instellen

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Uitgebreide gegevens instellen en uitvoeren als gebruiker met HPC-cluster
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

