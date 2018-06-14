---
title: HPC Pack cluster met Azure Active Directory | Microsoft Docs
description: Meer informatie over het integreren van een Microsoft HPC Pack 2016-cluster in Azure met Azure Active Directory
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/16/2017
ms.author: danlep
ms.openlocfilehash: bb0e878c4e987d111a535603cede25c639087ca7
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
ms.locfileid: "25452569"
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
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account u toegang tot meer dan één Azure AD-tenant hebt, klikt u op uw account in de rechterbovenhoek. Stel uw portal-sessie op de gewenste tenant. U moet gemachtigd zijn voor toegang tot bronnen in de directory. 
3. Klik op **Azure Active Directory** Klik in het navigatiedeelvenster links Services op **gebruikers en groepen**, en controleer of er gebruikersaccounts al gemaakt of geconfigureerd.
4. In **Azure Active Directory**, klikt u op **App registraties** > **registratie van de nieuwe toepassing**. Voer de volgende informatie in:
    * **Naam** -HPCPackClusterServer
    * **Toepassingstype** : Selecteer **Web-app / API**
    * **Eenmalige aanmelding URL**-de basis-URL voor de steekproef die standaard is`https://hpcserver`
    * Klik op **Create**.
5. Nadat de app wordt toegevoegd, selecteert u deze in de **App registraties** lijst. Klik vervolgens op **instellingen** > **eigenschappen**. Voer de volgende informatie in:
    * Selecteer **Ja** voor **Multi-verpachte**.
    * Wijziging **App ID URI** naar `https://<Directory_name>/<application_name>`. Vervang `<Directory_name`> met de volledige naam van uw Azure AD-tenant, bijvoorbeeld `hpclocal.onmicrosoft.com`, en vervang `<application_name>` met de naam die u eerder hebt gekozen.
6. Klik op **Opslaan**. Wanneer opslaan is voltooid, wordt op de pagina, klikt u op **Manifest**. Het manifest bewerken door het zoeken naar de `appRoles` instellen en klik vervolgens op toevoegen van de volgende toepassingsrol **opslaan**:

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
7. In **Azure Active Directory**, klikt u op **bedrijfstoepassingen** > **alle toepassingen**. Selecteer **HPCPackClusterServer** uit de lijst.
8. Klik op **eigenschappen**, en wijzig **Gebruikerstoewijzing vereist** naar **Ja**. Klik op **Opslaan**.
9. Klik op **gebruikers en groepen** > **gebruiker toevoegen**. Selecteer een gebruiker en selecteer een rol en klik vervolgens op **toewijzen**. Een van de beschikbare rollen (HpcUsers of HpcAdminMirror) toewijzen aan de gebruiker. Herhaal deze stap met extra gebruikers in de map. Zie voor achtergrondinformatie over cluster gebruikers, [Cluster gebruikers beheren](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).


## <a name="step-2-register-the-hpc-cluster-client-with-your-azure-ad-tenant"></a>Stap 2: De client HPC-cluster registreren bij uw Azure AD-tenant

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als uw account u toegang tot meer dan één Azure AD-tenant hebt, klikt u op uw account in de rechterbovenhoek. Stel uw portal-sessie op de gewenste tenant. U moet gemachtigd zijn voor toegang tot bronnen in de directory. 
3. In **Azure Active Directory**, klikt u op **App registraties** > **registratie van de nieuwe toepassing**. Voer de volgende informatie in:

    * **Naam** -HPCPackClusterClient    
    * **Toepassingstype** : Selecteer **systeemeigen**
    * **Omleidings-URI** - `http://hpcclient`
    * Klik op **Maken**.

4. Nadat de app wordt toegevoegd, selecteert u deze in de **App registraties** lijst. Kopieer de **toepassings-ID** waarde en op te slaan. U nodig dit later bij het configureren van uw toepassing.

5. Klik op **instellingen** > **vereist machtigingen** > **toevoegen** > **selecteert u een API**. Zoek en selecteer de HpcPackClusterServer-toepassing (gemaakt in stap 1).

6. In de **toegang inschakelen** pagina **toegang HpcClusterServer**. Klik vervolgens op **Gereed**.


## <a name="step-3-configure-the-hpc-cluster"></a>Stap 3: De HPC-cluster configureren

1. Verbinding maken met het hoofdknooppunt van HPC Pack 2016 in Azure.

2. Start HPC PowerShell.

3. Voer de volgende opdracht uit:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcPackClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    waar

    * `AADTenant`Hiermee geeft u de naam van de Azure AD-tenant, zoals`hpclocal.onmicrosoft.com`
    * `AADClientAppId`Hiermee geeft u de toepassings-ID voor de app gemaakt in stap 2.

4. Voer een van de volgende, afhankelijk van de configuratie van het hoofdknooppunt:

    * In een enkel hoofdknooppunt HPC Pack cluster, de HpcScheduler-service opnieuw te starten.

    * Voer de volgende PowerShell-opdrachten op het hoofdknooppunt de HpcSchedulerStateful-service te starten in een HPC Pack-cluster met meerdere hoofdknooppunten:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName "fabric:/HpcApplication/SchedulerStatefulService"

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

HPC Pack 2016 biedt de volgende HPC PowerShell-cmdlets voor het beheren van de lokale cache van de token. Deze cmdlets zijn handig voor het verzenden van taken niet-interactief. Zie het volgende voorbeeld:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-the-credentials-for-submitting-jobs-using-the-azure-ad-account"></a>Stel de referenties voor het indienen van taken met behulp van de Azure AD-account 

Soms wilt u de taak onder de gebruiker HPC-cluster (voor een domein HPC-cluster, voert u als een domeingebruiker; voor een niet-domein HPC-cluster moet uitvoeren als een lokale gebruiker op het hoofdknooppunt) uitvoeren.

1. Gebruik de volgende opdrachten de referenties in te stellen:

    ```powershell
    $localUser = "<username>"

    $localUserPassword="<password>"

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

