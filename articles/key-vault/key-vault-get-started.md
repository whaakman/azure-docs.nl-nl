---
title: Aan de slag met Azure Key Vault | Microsoft Docs
description: Gebruik deze handleiding om aan de slag te gaan met Azure Sleutelkluis en een geharde container in Azure te maken om cryptografiesleutels en geheimen op te slaan in Azure.
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/20/2017
ms.author: barclayn
ms.openlocfilehash: 1b70802945b710059e93b54607996ccf74510d1f
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="get-started-with-azure-key-vault"></a>Aan de slag met Azure Sleutelkluis
Dit artikel helpt u aan de slag te gaan met Azure Key Vault met behulp van PowerShell en leidt u stapsgewijs door de volgende activiteiten:
- Een beveiligde container (een kluis) maken in Azure.
- KeyVault gebruiken om cryptografische sleutels en geheimen in Azure op te slaan en te beheren.
- Een toepassing gebruik laten maken van die sleutel of wachtwoord.

Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

> [!NOTE]
> Dit artikel bevat geen instructies voor het schrijven van een Azure-toepassing. U kunt voor deze stappen de [Azure Key Vault-voorbeeldtoepassing](https://www.microsoft.com/download/details.aspx?id=45343) gebruiken.

Zie [deze equivalente zelfstudie](key-vault-manage-with-cli2.md) voor instructies om een platformonafhankelijke opdrachtregelinterface te maken.

## <a name="requirements"></a>Vereisten
Controleer voordat u verdergaat met het artikel of u over het volgende beschikt:

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Azure PowerShell**, **minimaal versie 1.1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren en te koppelen aan uw Azure-abonnement. Als u Azure PowerShell al hebt geïnstalleerd, maar niet weet welke versie u hebt, typt u `(Get-Module azure -ListAvailable).Version` in de Azure PowerShell-console. Ook wanneer u een van de versies van Azure PowerShell versie 0.9.1 tot en met 0.9.8 hebt geïnstalleerd, kunt u deze zelfstudie gebruiken en zij er slechts enkele kleine wijzigingen van toepassing. U moet bijvoorbeeld de opdracht `Switch-AzureMode AzureResourceManager` gebruiken en bepaalde Azure Key Vault-opdrachten zijn gewijzigd. Zie [Azure Key Vault-cmdlets](/powershell/module/azurerm.keyvault/#key_vault) voor een overzicht van de Key Vault-cmdlets voor versie 0.9.1 tot en met versie 0.9.8.
- **Een toepassing die kan worden geconfigureerd voor het gebruik van Key Vault**. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie het bijbehorende **Leesmij**-bestand voor instructies.

>[!NOTE]
In dit artikel wordt ervan uitgegaan dat u de basisbeginselen kent van PowerShell en Azure. Zie [Aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx) voor meer informatie over PowerShell.

Voor gedetailleerde hulp bij een van de cmdlets in deze zelfstudie kunt u de cmdlet **Get-Help** gebruiken.

```powershell-interactive
Get-Help <cmdlet-name> -Detailed
```
    
Als u bijvoorbeeld ondersteuning voor de cmdlet **Login-AzureRmAccount** wilt aanvragen, typt u:

```PowerShell
Get-Help Login-AzureRmAccount -Detailed
```

U kunt ook de volgende artikelen lezen om vertrouwd te raken met het Azure Resource Manager-implementatiemodel in Azure PowerShell:

* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell gebruiken met Resource Manager](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Verbinding maken met uw abonnementen
Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```PowerShell
Login-AzureRmAccount
```

>[!NOTE]
 Als u een specifiek exemplaar van Azure gebruikt, moet u de parameter -Environment gebruiken. Bijvoorbeeld: 
 ```powershell
 Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen op die zijn gekoppeld aan dit account en gebruikt standaard het eerste abonnement.

Als u meerdere abonnementen hebt en u een specifiek abonnement voor Azure Sleutelkluis wilt gebruiken, typt u het volgende om de abonnementen voor uw account weer te geven:

```powershell
Get-AzureRmSubscription
```

Typ vervolgens het volgende om het abonnement op te geven dat u wilt gebruiken:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het configureren van Azure PowerShell.

## <a id="resource"></a>Een nieuwe resourcegroep maken
Als u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. In deze zelfstudie maken we de resourcegroep **ContosoResourceGroup**:

```powershell
New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East US'
```

## <a id="vault"></a>Een sleutelkluis maken
Gebruik de cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) om een sleutelkluis te maken. Deze cmdlet heeft drie verplichte parameters: een **resourcegroepnaam**, een **sleutelkluisnaam** en de **geografische locatie**.

Als u bijvoorbeeld het volgende gebruikt:
- Kluisnaam **ContosoKeyVault**
- Naam van resourcegroep **ContosoResourceGroup**
- De locatie **VS - oost**

typt u:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```
![Uitvoer nadat de opdracht voor het maken van de sleutelkluis is voltooid](./media/key-vault-get-started/output-after-creating-keyvault.png)

De uitvoer van deze cmdlet toont u de eigenschappen van de sleutelkluis die u hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **Vault Name**: in het voorbeeld is dit **ContosoKeyVault**. U gebruikt deze naam voor andere Sleutelkluis-cmdlets.
* **Vault URI**: in het voorbeeld is dit https://contosokeyvault.vault.azure.net/. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Tot dusver is er nog niemand anders gemachtigd.

> [!NOTE]
> Als u een nieuwe sleutelkluis maakt, wordt mogelijk het foutbericht **Het abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.KeyVault'** weergegeven. Als dit bericht wordt weergegeven, moet u `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` uitvoeren. Nadat de registratie is voltooid, kunt u de opdracht New-AzureRmKeyVault opnieuw uitvoeren. Zie [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider) voor meer informatie.
>
>

## <a id="add"></a>Een sleutel of geheim toevoegen aan de sleutelkluis
Mogelijk hebt u meerdere manieren nodig om te kunnen communiceren met Key Vault en sleutels of geheimen.

### <a name="azure-key-vault-generates-a-software-protected-key"></a>In Azure Key Vault wordt een softwarematig beveiligde sleutel gegenereerd.

Als u wilt dat Azure Key Vault een softwarematig beveiligde sleutel voor u maakt, gebruikt u de cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) en typt u:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'
```
Als u de URI voor dit sleuteltype wilt weergeven:
```powershell
$key.id
```

U kunt de URI van de sleutel gebruiken om te verwijzen naar een sleutel die u hebt gemaakt of geüpload naar Azure Key Vault. Als u de huidige versie wilt ophalen, gebruikt u **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** als u deze specifieke versie wilt ophalen.  

### <a name="importing-an-existing-pfx-file-into-azure-key-vault"></a>Een bestaand PFX-bestand in Azure Key Vault importeren

De stappen zijn anders als er sprake is van bestaande sleutels die zijn opgeslagen in een PFX-bestand dat u wilt uploaden naar Azure Key Vault. Bijvoorbeeld:
- Als u een bestaande softwarematig beveiligde sleutel in een PFX-bestand hebt
- Het PFX-bestand heeft de naam softkey.pfx 
- Het bestand is opgeslagen op de C-schijf

Kunt u het volgende typen:

```powershell
$securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force  // This stores the password 123 in the variable $securepfxpwd
```

Typ vervolgens het volgende om de sleutel te importeren uit het PFX-bestand, dat de sleutel beveiligt met software in de Sleutelkluis-service:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoImportedPFX' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd
```

Als u de URI voor deze sleutel wilt weergeven, typt u het volgende:

```powershell
$Key.id
```
Als u de sleutel wilt weergeven, typt: 

```powershell
Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'
```
Als u wilt dat de eigenschappen van het PFX-bestand op de portal worden weergegeven, ziet dat er ongeveer uit als in de onderstaande afbeelding.

![Hoe een certificaat er in de portal uitziet](./media/key-vault-get-started/imported-pfx.png)
### <a name="to-add-a-secret-to-azure-key-vault"></a>Een geheim toevoegen aan Azure Key Vault

Als u een geheim aan de kluis wilt toevoegen, namelijk het wachtwoord met de naam SQLPassword en de waarde van Pa$$w0rd voor Azure Key Vault, converteert u de waarde Pa$$w0rd eerst naar een beveiligde tekenreeks door het volgende te typen:

```powershell    
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Typ vervolgens:

```powershell
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```


Als u nu naar het wachtwoord wilt verwijzen dat u hebt toegevoegd aan Azure Sleutelkluis, kunt u de URI van het wachtwoord gebruiken. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om altijd de huidige versie op te halen en gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om deze specifieke versie op te halen.

Als u de URI voor dit geheim wilt weergeven, typt u het volgende:

```powershell
$secret.Id
```
Als u uw geheim wilt bekijken, typt u: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`. U kunt het geheim ook in de portal weergeven.

![geheim](./media/key-vault-get-started/secret-value.png)

Als u de waarde in het geheim als tekst zonder opmaak wilt weergeven:
```powershell
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```
U sleutelkluis en sleutel of geheim kunnen nu door toepassingen worden gebruikt. U moet toepassingen autoriseren om ze te gebruiken.  

## <a id="register"></a>Een toepassing registreren met Azure Active Directory
Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. De stap is niet specifiek voor Azure Key Vault. Voor gedetailleerde stappen voor het registreren van een toepassing bij Azure Active Directory wordt u aanbevolen het artikel [Integrating applications with Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) (Toepassingen integreren met Azure Active Directory) of [Use portal to create an Azure Active Directory application and service principal that can access resources](../azure-resource-manager/resource-group-create-service-principal-portal.md) (De portal gebruiken om een Azure Active Directory-toepassing en service-principal te maken die toegang hebben tot resources) te lezen.

> [!IMPORTANT]
> Uw account, de kluis en de toepassing die u in deze stap registreert moet allemaal in dezelfde Azure-directory bevinden om de zelfstudie te kunnen voltooien.


Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren. Hiervoor moet de eigenaar van de toepassing deze eerst registreren bij Azure Active Directory. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

- Een **toepassings-id** 
- Een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). 

De toepassing moet beide waarden aan Azure Active Directory verstrekken om een token te verkrijgen. Hoe de toepassing is geconfigureerd om dit te doen, is afhankelijk van de toepassing. In de [Key Vault-voorbeeldtoepassing](https://www.microsoft.com/download/details.aspx?id=45343) moet de eigenaar van de toepassing deze waarden instellen in het bestand app.config.


De toepassing registreren in Azure Active Directory:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **App-registraties**. Als App-registraties niet wordt weergegeven, klikt u op **Meer services** om na te gaan of het daar wordt weergegeven.  
>[!NOTE]
U moet de directory selecteren die ook het Azure-abonnement bevat waarmee u de sleutelkluis hebt gemaakt. 
3. Klik op **Nieuwe toepassing registreren**.
4. Geef op de blade **Maken** een naam voor uw toepassing op, selecteer **WEBTOEPASSING EN/OF WEB-API** (de standaardinstelling) en geef de **AANMELDINGS-URL** voor uw webtoepassing op. Als u deze informatie op dit moment niet hebt, kunt u deze voor deze stap verzinnen. (U kunt bijvoorbeeld http://test1.contoso.com opgeven.) Het maakt niet uit of deze sites bestaan. 

    ![Nieuwe toepassing registreren](./media/key-vault-get-started/new-application-registration.png)
    >[!WARNING]
    Zorg ervoor dat u **WEBTOEPASSING EN/OF WEB-API** kiest. Als u dat niet doet, wordt de optie **Sleutels** niet weergegeven onder instellingen.

5. Klik op de knop **Maken**.
6. Wanneer de app-registratie is voltooid, kunt u de lijst met geregistreerde apps zien. Zoek naar de app die u zojuist hebt geregistreerd en klik erop.
7. Klik op de blade **Geregistreerde app** en kopieer de **toepassings-id**.
8. Klik op **Alle instellingen**.
9. Klik op de blade **Instellingen** op **Sleutels**.
9. Typ een beschrijving in het vak **Sleutelbeschrijving** en selecteer een tijdsduur. Klik vervolgens op **OPSLAAN**. De pagina wordt vernieuwd en bevat nu een sleutelwaarde. 
10. U gaat de **toepassings-id**- en **sleutel**gegevens in de volgende stap gebruiken om machtigingen voor uw kluis in te stellen.

## <a id="authorize"></a>De toepassing toestemming geven om de sleutel of het geheim te gebruiken
Als u de toepassing toegang wilt verlenen toegang tot de sleutel of het geheim in de kluis, gebruikt u de cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).

Als bijvoorbeeld uw kluisnaam **ContosoKeyVault** is en de toepassing die u wilt hosten de client-id 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft, kunt u het volgende uitvoeren om de toepassing toestemming te geven om ondertekenings- en versleutelingsbewerking met de sleutels in uw kluis uit te voeren:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Als u dezelfde toepassing wilt autoriseren voor het lezen van geheimen in uw kluis, voert u de volgende opdracht uit:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get
```

## <a id="HSM"></a>Met een Hardware Security Module (HSM) werken
Voor de zekerheid kunt u sleutels in HSM's (Hardware Security Module) importeren of genereren die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete) te gaan.

Om deze met HSM beveiligde sleutels te maken, moet u de [Azure Sleutelkluis Premium-servicelaag gebruiken voor ondersteuning voor met HSM beveiligde sleutels](https://azure.microsoft.com/pricing/free-trial/). Deze functionaliteit is niet beschikbaar voor Azure China.

Wanneer u de sleutelkluis maakt, voegt u de parameter **-SKU** toe:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US' -SKU 'Premium'
```


U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze sleutelkluis. Als u een sleutel wilt maken die is beveiligd met HSM, stelt u de parameter **-Destination** in op HSM:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'
```

U kunt de volgende opdracht gebruiken om een sleutel uit een PFX-bestand op uw computer te importeren. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'
```

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. In dit scenario kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Key Vault-service, zonder dat de sleutel de HSM-grens verlaat:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'
```

Zie [Met HSM beveiligde sleutels voor Azure Key Vault genereren en overdragen](key-vault-hsm-protected-keys.md) voor gedetailleerde instructies om dit BYOK-pakket te genereren.

## <a id="delete"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
Als u de sleutelkluis en de sleutel of het geheim in de kluis niet meer nodig hebt, kunt u de sleutelkluis verwijderen met de cmdlet [verwijderen AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault):

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'
```

## <a id="other"></a>Overige Azure PowerShell-cmdlets
Overige opdrachten die handig kunnen zijn voor het beheren van Azure Sleutelkluis:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel.
- `$Keys[0]`: met deze opdracht geeft u een volledige lijst met eigenschappen voor de opgegeven sleutel weer.
- `Get-AzureKeyVaultSecret`: met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: voorbeeld van hoe u een specifieke sleutel verwijdert.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: voorbeeld van hoe u een specifiek geheim verwijdert.

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is Azure Sleutelkluis?](key-vault-whatis.md) voor algemene informatie over Azure Sleutelkluis.
- Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) om te zien hoe uw sleutelkluis wordt gebruikt.
- Zie [Azure Key Vault in een webtoepassing gebruiken](key-vault-use-from-web-application.md) voor een vervolgzelfstudie over het gebruik van Azure Key Vault in een webtoepassing.
- Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.
- Zie [Cmdlets voor Azure Sleutelkluis](/powershell/module/azurerm.keyvault/#key_vault) voor een lijst met de nieuwste Azure PowerShell 1.0-cmdlets voor Azure Key Vault.