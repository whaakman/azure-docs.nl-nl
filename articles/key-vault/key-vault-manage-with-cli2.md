---
title: Beheren van Azure Sleutelkluis met CLI | Microsoft Docs
description: Gebruik deze handleiding om algemene taken in de Sleutelkluis automatiseren met behulp van de CLI 2.0
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: barclayn
ms.openlocfilehash: eaeb50ca8a83fcfee6689acf549f20ba5d44c51d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Beheren met CLI 2.0 Sleutelkluis

In dit artikel wordt beschreven hoe u aan de slag met Azure Sleutelkluis met behulp van de Azure CLI 2.0. U kunt informatie bekijken over:
- Het maken van een geharde container (een kluis) in Azure
- Het opslaan en beheren van de cryptografische sleutels en geheimen in Azure. 
- Het proces van het gebruik van Azure-opdrachtregelinterface voor Cross-Platform een kluis met een sleutel of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt maken. 
- Hoe een toepassing vervolgens kunt gebruiken die sleutel of het wachtwoord.

Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

**Geschatte duur:** 20 minuten

> [!NOTE]
> Deze zelfstudie bevat geen instructies voor het schrijven van de Azure-toepassing of een van de stappen bevat, die wordt beschreven hoe een toepassing gebruik van een sleutel of geheim in de sleutelkluis toestemming geeft.
>

Zie voor een overzicht van Azure Sleutelkluis [wat is Azure Sleutelkluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een abonnement op Microsoft Azure Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial).
* Opdrachtregelinterface versie 2.0 of hoger. Installeer de nieuwste versie en verbinden om uw Azure-abonnement, Zie [installeren en configureren van de Azure platformoverschrijdende opdrachtregelinterface 2.0](/cli/azure/install-azure-cli).
* Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of het wachtwoord dat u in deze zelfstudie maakt. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie het bijbehorende Leesmij-bestand voor instructies.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Help-informatie met Azure platformoverschrijdende opdrachtregelinterface
Deze zelfstudie wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal-opdrachtprompt bent)

De--help- of -h parameter kan worden gebruikt om help voor specifieke opdrachten weer te geven. U kunt ook de azure-help [opdracht] [opties] indeling kan ook worden gebruikt om de dezelfde informatie te retourneren. Bijvoorbeeld de volgende opdrachten uit alle dezelfde informatie geretourneerd:

```azurecli-interactive
az account set --help
az account set -h
```

Raadpleeg bij twijfel over de parameters die door een opdracht die nodig zijn om te helpen met behulp van--help, -h of az help [opdracht].

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Azure CLI installeren](/cli/azure/install-azure-cli)
* [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen
Meld u aan met een organisatie-account met de volgende opdracht:

```azurecli-interactive
az login -u username@domain.com -p password
```

of als u wilt zich aanmelden door interactief typen

```azurecli-interactive
az login
```

Als u meerdere abonnementen hebt en u een specifiek abonnement voor Azure Sleutelkluis wilt gebruiken, typt u het volgende om de abonnementen voor uw account weer te geven:

```azurecli-interactive
az account list
```

Typ vervolgens het volgende om het abonnement op te geven dat u wilt gebruiken:

```azurecli-interactive
az account set --subscription <subscription name or ID>
```

Zie voor meer informatie over het configureren van Azure platformoverschrijdende opdrachtregelinterface [Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken
Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

```azurecli-interactive
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Voor een lijst met alle mogelijke locaties type:

```azurecli-interactive
az account list-locations
``` 

Als u meer informatie nodig hebt, typt u: 

```azurecli-interactive
az account list-locations -h
```

## <a name="register-the-key-vault-resource-provider"></a>De registerbronprovider is Sleutelkluis
Wanneer u probeert te maken van een nieuwe sleutelkluis mogelijk ziet u de fout 'het abonnement is niet geregistreerd voor gebruik van de naamruimte 'Microsoft.KeyVault' '. Zorg dat de Sleutelkluis-resourceprovider is geregistreerd in uw abonnement als dit bericht wordt weergegeven:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
```

>[!NOTE]
Dit hoeft slechts één keer per abonnement worden uitgevoerd.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Gebruik de `az keyvault create` opdracht om een sleutelkluis te maken. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een sleutelkluis en de geografische locatie.

Bijvoorbeeld:

- Als u de kluisnaam **ContosoKeyVault**
- De naam van de resourcegroep van **ContosoResourceGroup**
- De locatie van **Oost-Azië**

typt u:

```azurecli-interactive
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **naam**: In het voorbeeld is dit ContosoKeyVault. U gebruikt deze naam voor andere Sleutelkluis-opdrachten.
* **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Tot dusver is er nog niemand anders gemachtigd.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een sleutel of geheim toevoegen aan de sleutelkluis

Als u wilt dat Azure Sleutelkluis een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `az key create` opdracht in en typt u het volgende:
```azurecli-interactive
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Echter, hebt u een bestaande sleutel in een .pem-bestand opgeslagen als een lokaal bestand in een bestand met de naam softkey.pem die u wilt uploaden naar Azure Sleutelkluis, typt u het volgende als u wilt importeren van de sleutel van de. PEM-bestand dat de sleutel met software in de Sleutelkluis-service beveiligt:

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```

U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure Sleutelkluis, met behulp van de URI verwijzen. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** altijd de huidige versie en gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.

Als u wilt een geheim toevoegen aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd voor Azure Sleutelkluis, typt u het volgende:

```azurecli-interactive
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Als u nu naar het wachtwoord wilt verwijzen dat u hebt toegevoegd aan Azure Sleutelkluis, kunt u de URI van het wachtwoord gebruiken. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om altijd de huidige versie op te halen en gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om deze specifieke versie op te halen.

We bekijken de sleutel of geheim dat u zojuist hebt gemaakt:

* Als u de sleutel wilt weergeven, typt: 

```azurecli-interactive
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Als u het geheim wilt weergeven, typt u: 

```azurecli-interactive
az keyvault secret list --vault-name 'ContosoKeyVault'
```

## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory
Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. Het is niet specifiek voor Azure Sleutelkluis maar opgenomen, voor awareness.

> [!IMPORTANT]
> Uw account, de kluis en de toepassing die u in deze stap registreert moet allemaal in dezelfde Azure-directory bevinden om de zelfstudie te kunnen voltooien.
>
>

Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren. Hiervoor moet de eigenaar van de toepassing deze eerst registreren bij Azure Active Directory. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

- Een **toepassings-id** 
- Een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). 

De toepassing moet beide waarden aan Azure Active Directory verstrekken om een token te verkrijgen. Hoe de toepassing is geconfigureerd om dit te doen, is afhankelijk van de toepassing. In de [Key Vault-voorbeeldtoepassing](https://www.microsoft.com/download/details.aspx?id=45343) moet de eigenaar van de toepassing deze waarden instellen in het bestand app.config.

Voor gedetailleerde stappen voor het registreren van een toepassing met Azure Active Directory rekening met het artikel [toepassingen integreren met Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) of [portal gebruik maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md) registreren van de toepassing in Azure Active Directory:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik aan de linkerkant op **App-registraties**. Als App-registraties niet wordt weergegeven, klikt u op **Meer services** om na te gaan of het daar wordt weergegeven.  
[!NOTE]
U moet de directory selecteren die ook het Azure-abonnement bevat waarmee u de sleutelkluis hebt gemaakt. 
3. Klik op **Nieuwe toepassing registreren**.
4. Geef op de blade **Maken** een naam voor uw toepassing op, selecteer **WEBTOEPASSING EN/OF WEB-API** (de standaardinstelling) en geef de **AANMELDINGS-URL** voor uw webtoepassing op. Als u deze informatie op dit moment niet hebt, kunt u deze voor deze stap verzinnen. (U kunt bijvoorbeeld http://test1.contoso.com opgeven.) Het maakt niet uit of deze sites bestaan. 

    ![Nieuwe toepassing registreren](./media/key-vault-manage-with-cli2/new-application-registration.png)
    >[!WARNING]
    Zorg ervoor dat u **WEBTOEPASSING EN/OF WEB-API** kiest. Als u dat niet doet, wordt de optie **Sleutels** niet weergegeven onder instellingen.

5. Klik op de knop **Maken**.
6. Wanneer de app-registratie is voltooid, kunt u de lijst met geregistreerde apps zien. Zoek naar de app die u zojuist hebt geregistreerd en klik erop.
7. Klik op de blade **Geregistreerde app** en kopieer de **toepassings-id**.
8. Klik op **Alle instellingen**.
9. Klik op de blade **Instellingen** op **Sleutels**.
9. Typ een beschrijving in het vak **Sleutelbeschrijving** en selecteer een tijdsduur. Klik vervolgens op **OPSLAAN**. De pagina wordt vernieuwd en bevat nu een sleutelwaarde. 
10. U gaat de **toepassings-id**- en **sleutel**gegevens in de volgende stap gebruiken om machtigingen voor uw kluis in te stellen.


## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing toestemming geven om de sleutel of het geheim te gebruiken
Voor het autoriseren van de toepassing toegang krijgen tot de sleutel of geheim in de kluis, gebruikt u de `az keyvault set-policy` opdracht.

Bijvoorbeeld, als de kluisnaam van uw ContosoKeyVault is en de toepassing die u wilt machtigen client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, voert u het volgende:

```azurecli-interactive
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Als u dezelfde toepassing wilt autoriseren voor het lezen van geheimen in uw kluis, voert u de volgende opdracht uit:

```azurecli-interactive
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u een Hardware Security Module (HSM) wilt gebruiken
Voor de zekerheid kunt u sleutels in HSM's (Hardware Security Module) importeren of genereren die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

U moet een kluis-abonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben voor het maken van deze met HSM beveiligde sleutels.

Wanneer u de keyvault maakt, voegt u de parameter 'sku':

```azurecli-interactive
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een HSM beveiligde sleutel:

```azurecli-interactive
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel van een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Zie voor meer instructies over hoe dit BYOK-pakket te genereren gedetailleerde, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
Als u de sleutelkluis en de sleutel of geheim niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de `az keyvault delete` opdracht:

```azurecli-interactive
az keyvault delete --name 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```azurecli-interactive
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere opdrachten Azure platformoverschrijdende opdrachtregelinterface
Andere opdrachten die u wellicht nuttig voor het beheer van Azure Sleutelkluis.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli-interactive
az keyvault key list --vault-name 'ContosoKeyVault'
```

Deze opdracht wordt een volledige lijst met eigenschappen voor de opgegeven sleutel weergegeven:

```azurecli-interactive
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel:

```azurecli-interactive
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

```azurecli-interactive
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Hier volgt een voorbeeld van hoe u een specifiek geheim verwijdert:

```azurecli-interactive
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een volledig overzicht van Azure CLI voor sleutelkluis-opdrachten, [Sleutelkluis CLI verwijzing](/cli/azure/keyvault).

- Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.

- Zie voor informatie over Azure Sleutelkluis en HSM's, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).
