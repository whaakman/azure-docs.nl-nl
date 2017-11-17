---
title: Beheren van Azure Sleutelkluis met CLI | Microsoft Docs
description: Gebruik deze handleiding om algemene taken in de Sleutelkluis automatiseren met behulp van de CLI
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: barclayn
ms.openlocfilehash: 94ea95e7f40c8d47dd18422a9c0795655dae365b
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-key-vault-using-cli"></a>Beheren van de Sleutelkluis met CLI

Met deze zelfstudie kunt u aan de slag met Azure Sleutelkluis een geharde container (een kluis) maken in Azure. Azure sleutelkluis wordt gebruikt voor het opslaan en beheren van de cryptografische sleutels en geheimen. In dit artikel begeleidt u bij het proces van het gebruik van Azure platformoverschrijdende opdrachtregelinterface voor het maken van een kluis. U wordt vervolgens communiceren met de kluis verschillende algemene bewerkingen uit te voeren. 

Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

**Geschatte duur:** 20 minuten

> [!NOTE]
> Deze zelfstudie bevat geen instructies voor het schrijven van de Azure-toepassing een van de stappen bevat. Deze voorbeeldtoepassing wordt gebruikt om weer te geven hoe een toepassing kan worden geautoriseerd gebruik van een sleutel of geheim dat is opgeslagen in de sleutelkluis.
> Dit artikel is gericht op het configureren van Azure Sleutelkluis met behulp van de platformoverschrijdende opdrachtregelinterface. Zie voor instructies voor Azure PowerShell [deze equivalente zelfstudie](key-vault-get-started.md).

Zie [Wat is Azure Sleutelkluis?](key-vault-whatis.md) voor algemene informatie over Azure Sleutelkluis.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een abonnement op Microsoft Azure Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial).
* Opdrachtregelinterface versie 0.9.1 of hoger. Installeer de nieuwste versie en verbinden om uw Azure-abonnement, Zie [installeren en configureren van de Azure-opdrachtregelinterface voor Cross-Platform](../cli-install-nodejs.md).
* Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of het wachtwoord dat u in deze zelfstudie maakt. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie het bijbehorende Leesmij-bestand voor instructies.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Help-informatie met Azure platformoverschrijdende opdrachtregelinterface

Deze zelfstudie wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal-opdrachtprompt bent)

De--help- of -h parameter kan worden gebruikt om help voor specifieke opdrachten weer te geven. U kunt ook de azure-help [opdracht] [opties] indeling kan ook worden gebruikt om de dezelfde informatie te retourneren. Bijvoorbeeld de volgende opdrachten uit alle dezelfde informatie geretourneerd:

    azure account set --help

    azure account set -h

    azure help account set

Raadpleeg bij twijfel over de parameters die door een opdracht die nodig zijn om te helpen met behulp van--help, -h of azure help [opdracht].

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager-model voor de implementatie in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Het installeren en configureren van Azure-opdrachtregelinterface voor Cross-Platform](../cli-install-nodejs.md)
* [Met behulp van Azure platformoverschrijdende opdrachtregelinterface met Azure Resource Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Aan te melden bij Azure met behulp van de volgende opdracht:

```azurecli-interactive
azure login -u username -p password
```

of als u wilt zich aanmelden door interactief typen

```azurecli-interactive
azure login
```

Als u meerdere abonnementen hebt en u een specifiek abonnement voor Azure Sleutelkluis wilt gebruiken, typt u het volgende om de abonnementen voor uw account weer te geven:

```azurecli-interactive
azure account list
```

Typ vervolgens het volgende om het abonnement op te geven dat u wilt gebruiken:

```azurecli-interactive
azure account set <subscription name>
```

Zie voor meer informatie over het configureren van Azure platformoverschrijdende opdrachtregelinterface [installeren en configureren van Azure platformoverschrijdende Command-Line Interface](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Overschakelen op het gebruik van Azure Resource Manager
Azure Resource Manager vereist dat de Sleutelkluis, dus typ het volgende overschakelen naar modus Azure Resource Manager:

```azurecli-interactive
azure config mode arm
```

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken
Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

```azurecli-interactive
azure group create 'ContosoResourceGroup' 'East Asia'
```

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Gebruik de opdracht voor locatie, `azure location list` om te bepalen hoe een alternatieve locatie op de in dit voorbeeld opgeven. Als u meer informatie nodig hebt, typt u:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>De registerbronprovider is Sleutelkluis
Zorg ervoor dat Sleutelkluis-resourceprovider is geregistreerd in uw abonnement:

```azurecli-interactive
azure provider register Microsoft.KeyVault
```

Dit hoeft slechts één keer per abonnement worden uitgevoerd.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Gebruik de `azure keyvault create` opdracht om een sleutelkluis te maken. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een sleutelkluis en de geografische locatie.

Bijvoorbeeld:
- Als u de kluisnaam **ContosoKeyVault**
- De naam van de resourcegroep van **ContosoResourceGroup**
- De locatie van **Oost-Azië** type:

```azurecli-interactive
azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **Naam**: In het voorbeeld is dit ContosoKeyVault. U gebruikt deze naam voor andere Sleutelkluis-cmdlets.
* **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Tot dusver is er nog niemand anders gemachtigd.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een sleutel of geheim toevoegen aan de sleutelkluis

Als u wilt dat Azure Sleutelkluis een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `azure key create` opdracht in en typt u het volgende:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software
````

Echter, hebt u een bestaande sleutel in een .pem-bestand opgeslagen als een lokaal bestand in een bestand met de naam softkey.pem die u wilt uploaden naar Azure Sleutelkluis, typt u het volgende als u wilt importeren van de sleutel van de. PEM-bestand dat de sleutel met software in de Sleutelkluis-service beveiligt:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software
```

U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure Sleutelkluis, met behulp van de URI verwijzen. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** altijd de huidige versie en gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.

Als u wilt een geheim toevoegen aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd voor Azure Sleutelkluis, typt u het volgende:

```azurecli-interactive
azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'
```

Als u nu naar het wachtwoord wilt verwijzen dat u hebt toegevoegd aan Azure Sleutelkluis, kunt u de URI van het wachtwoord gebruiken. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om altijd de huidige versie op te halen en gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om deze specifieke versie op te halen.

We bekijken de sleutel of geheim dat u zojuist hebt gemaakt:

* Als u de sleutel wilt weergeven, typt u: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Als u het geheim wilt weergeven, typt u: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory

Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. Het is niet specifiek voor Azure Sleutelkluis maar opgenomen, voor de volledigheid.

> [!IMPORTANT]
> Voor het voltooien van de zelfstudie, uw account, de kluis en de toepassing die u in deze stap registreren wilt moet alle zich in dezelfde Azure Active directory.

Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren. Hiervoor moet de eigenaar van de toepassing deze eerst registreren bij Azure Active Directory. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

- Een **toepassings-ID** 
- Een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). 

De toepassing moet beide waarden aan Azure Active Directory verstrekken om een token te verkrijgen. Hoe de toepassing is geconfigureerd om dit te doen, is afhankelijk van de toepassing. Voor de [voorbeeldtoepassing voor Sleutelkluis](https://www.microsoft.com/download/details.aspx?id=45343), de eigenaar van de toepassing deze waarden ingesteld in het bestand app.config.

Voor gedetailleerde stappen voor het registreren van een toepassing met Azure Active Directory rekening met het artikel [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) of [portal gebruik maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md) registreren van de toepassing in Azure Active Directory:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **App registraties**. Als er geen app registraties u klikt u op **meer services** en er vinden.  
    >[!NOTE]
    U moet de directory waarin het Azure-abonnement waarmee u de sleutelkluis hebt gemaakt. 
3. Klik op **registratie van de nieuwe toepassing**.
4. Op de **maken** blade een naam voor uw toepassing en selecteer vervolgens **WEBTOEPASSING en/of WEB-API** (de standaardinstelling) en geef de **AANMELDINGS-URL** voor uw web de toepassing. Als u deze informatie op dit moment geen hebt, kunt u deze maken voor deze stap (bijvoorbeeld, kunt u http://test1.contoso.com opgeven). Het maakt niet uit of deze sites bestaan. 

   ![Nieuwe toepassing registreren](./media/key-vault-manage-with-cli/new-application-registration.png)

5. Klik op de knop **Maken**.
6. Wanneer de app-registratie is voltooid, kunt u de lijst met geregistreerde apps zien. De app die u zojuist hebt geregistreerd en klikt u op te zoeken.
7. Klik op de **geregistreerde app** blade kopiëren de **toepassings-ID**
8. Klik op **alle instellingen**
9. Op de **instellingen** blade Klik op **sleutels**
10. Type in een beschrijving in de **sleutel beschrijving** vak en selecteert u een duur en klik vervolgens op **opslaan**. De pagina wordt vernieuwd en bevat nu een sleutelwaarde. 
11. U gebruikt de **toepassings-ID** en de **sleutel** informatie in de volgende stap machtigingen worden ingesteld voor uw kluis.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing toestemming geven om de sleutel of het geheim te gebruiken
Voor het autoriseren van de toepassing toegang krijgen tot de sleutel of geheim in de kluis gebruiken:

```azurecli-interactive
azure keyvault set-policy
```

Bijvoorbeeld, als de kluisnaam van uw ContosoKeyVault is en de toepassing die u wilt machtigen client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, voert u het volgende:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'
```

> [!NOTE]
> Als u op Windows-opdrachtprompt uitvoert, moet u enkele aanhalingstekens vervangen door dubbele aanhalingstekens en ook escape voor de interne dubbele aanhalingstekens. Bijvoorbeeld: ' [\"ontsleutelen\",\"aanmelding\"] '.
> 

Als u dezelfde toepassing wilt autoriseren voor het lezen van geheimen in uw kluis, voert u de volgende opdracht uit:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'
```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u een Hardware Security Module (HSM) wilt gebruiken
Voor de zekerheid kunt u sleutels in HSM's (Hardware Security Module) importeren of genereren die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

U moet een kluis-abonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben voor het maken van deze met HSM beveiligde sleutels.

Wanneer u de keyvault maakt, voegt u de parameter 'sku':

```azurecli-interactive
azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een HSM beveiligde sleutel:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'
```

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel van een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'
```

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'
```

Zie voor meer instructies over hoe dit BYOK-pakket te genereren gedetailleerde, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
Als u de sleutelkluis en de sleutel of geheim niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de opdracht azure keyvault verwijderen:

```azurecli-interactive
azure keyvault delete --vault-name 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```azurecli-interactive
azure group delete --name 'ContosoResourceGroup'
```


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere opdrachten Azure platformoverschrijdende opdrachtregelinterface
Andere opdrachten die u wellicht nuttig voor het beheer van Azure Sleutelkluis.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli-interactive
azure keyvault key list --vault-name 'ContosoKeyVault'
```

Deze opdracht wordt een volledige lijst met eigenschappen voor de opgegeven sleutel weergegeven:

```azurecli-interactive
azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli-interactive
azure keyvault secret list --vault-name 'ContosoKeyVault'
```

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

```azurecli-interactive
azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Hier volgt een voorbeeld van hoe u een specifiek geheim verwijdert:

```azurecli-interactive
azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'
```


## <a name="next-steps"></a>Volgende stappen
- Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.
- Zie [Wat is Azure Sleutelkluis?](key-vault-whatis.md) voor algemene informatie over Azure Sleutelkluis.
- Werken met Azure Key Vault met behulp van Powershell [Azure Key Vault aan de slag](key-vault-get-started.md).


