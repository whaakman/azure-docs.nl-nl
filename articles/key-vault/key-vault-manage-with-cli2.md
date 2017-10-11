---
title: Beheren van Azure Sleutelkluis met CLI | Microsoft Docs
description: Gebruik deze handleiding om algemene taken in de Sleutelkluis automatiseren met behulp van de CLI 2.0
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 5da9f5eceda71ac85259193e0f183c72813e1679
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Beheren met CLI 2.0 Sleutelkluis
Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

## <a name="introduction"></a>Inleiding
Gebruik deze handleiding om aan de slag te gaan met Azure Sleutelkluis en een geharde container (een kluis) in Azure te maken om cryptografiesleutels en geheimen op te slaan in Azure. Dit helpt u bij het proces van het gebruik van Azure-opdrachtregelinterface voor Cross-Platform een kluis met een sleutel of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt maken. Deze vervolgens ziet u hoe een toepassing kunt vervolgens sleutel of het wachtwoord.

**Geschatte duur:** 20 minuten

> [!NOTE]
> Deze zelfstudie bevat geen instructies voor het schrijven van de Azure-toepassing of een van de stappen bevat, die wordt beschreven hoe een toepassing gebruik van een sleutel of geheim in de sleutelkluis toestemming geeft.
>
> Deze zelfstudie maakt gebruik van de nieuwste Azure CLI 2.0.
>
>

Zie [Wat is Azure Sleutelkluis?](key-vault-whatis.md) voor algemene informatie over Azure Sleutelkluis.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een abonnement op Microsoft Azure Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial).
* Opdrachtregelinterface versie 2.0 of hoger. Installeer de nieuwste versie en verbinden om uw Azure-abonnement, Zie [installeren en configureren van de Azure platformoverschrijdende opdrachtregelinterface 2.0](/cli/azure/install-azure-cli).
* Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of het wachtwoord dat u in deze zelfstudie maakt. Er is een voorbeeldtoepassing beschikbaar in het [Microsoft Downloadcentrum](http://www.microsoft.com/download/details.aspx?id=45343). Zie het bijbehorende Leesmij-bestand voor instructies.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Help-informatie met Azure platformoverschrijdende opdrachtregelinterface
Deze zelfstudie wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal-opdrachtprompt bent)

De--help- of -h parameter kan worden gebruikt om help voor specifieke opdrachten weer te geven. U kunt ook de azure-help [opdracht] [opties] indeling kan ook worden gebruikt om de dezelfde informatie te retourneren. Bijvoorbeeld de volgende opdrachten uit alle dezelfde informatie geretourneerd:

```
az account set --help
az account set -h
```

Raadpleeg bij twijfel over de parameters die door een opdracht die nodig zijn om te helpen met behulp van--help, -h of az help [opdracht].

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Azure CLI installeren](/cli/azure/install-azure-cli)
* [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen
Meld u aan met een organisatie-account met de volgende opdracht:

```
az login -u username@domain.com -p password
```

of als u wilt zich aanmelden door interactief typen

```
az login
```

Als u meerdere abonnementen hebt en u een specifiek abonnement voor Azure Sleutelkluis wilt gebruiken, typt u het volgende om de abonnementen voor uw account weer te geven:

```
az account list
```

Typ vervolgens het volgende om het abonnement op te geven dat u wilt gebruiken:

```
az account set --subscription <subscription name or ID>
```

Zie voor meer informatie over het configureren van Azure platformoverschrijdende opdrachtregelinterface [Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken
Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Gebruik de opdracht voor locatie, `az account list-locations` om te bepalen hoe een alternatieve locatie op de in dit voorbeeld opgeven. Als u meer informatie nodig hebt, typt: `az account list-locations -h`.

## <a name="register-the-key-vault-resource-provider"></a>De registerbronprovider is Sleutelkluis
Zorg ervoor dat Sleutelkluis-resourceprovider is geregistreerd in uw abonnement:

```
az provider register -n Microsoft.KeyVault
```

Dit hoeft slechts één keer per abonnement worden uitgevoerd.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken
Gebruik de `az keyvault create` opdracht om een sleutelkluis te maken. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een sleutelkluis en de geografische locatie.

Bijvoorbeeld, als u de kluisnaam ContosoKeyVault gebruikt, de naam van de resourcegroep van ContosoResourceGroup en de locatie van Oost-Azië, typt u:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **naam**: In het voorbeeld is dit ContosoKeyVault. U gebruikt deze naam voor andere Sleutelkluis-opdrachten.
* **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Tot dusver is er nog niemand anders gemachtigd.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een sleutel of geheim toevoegen aan de sleutelkluis
Als u wilt dat Azure Sleutelkluis een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `az key create` opdracht in en typt u het volgende:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Echter, hebt u een bestaande sleutel in een .pem-bestand opgeslagen als een lokaal bestand in een bestand met de naam softkey.pem die u wilt uploaden naar Azure Sleutelkluis, typt u het volgende als u wilt importeren van de sleutel van de. PEM-bestand dat de sleutel met software in de Sleutelkluis-service beveiligt:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure Sleutelkluis, met behulp van de URI verwijzen. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** altijd de huidige versie en gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.

Als u wilt een geheim toevoegen aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd voor Azure Sleutelkluis, typt u het volgende:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Als u nu naar het wachtwoord wilt verwijzen dat u hebt toegevoegd aan Azure Sleutelkluis, kunt u de URI van het wachtwoord gebruiken. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om altijd de huidige versie op te halen en gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om deze specifieke versie op te halen.

We bekijken de sleutel of geheim dat u zojuist hebt gemaakt:

* Als u de sleutel wilt weergeven, typt u: `az keyvault key list --vault-name 'ContosoKeyVault'`
* Als u het geheim wilt weergeven, typt u: `az keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing registreren met Azure Active Directory
Deze stap wordt doorgaans uitgevoerd door een ontwikkelaar op een afzonderlijke computer. Het is niet specifiek voor Azure Sleutelkluis maar opgenomen, voor de volledigheid.

> [!IMPORTANT]
> Uw account, de kluis en de toepassing die u in deze stap registreert moet allemaal in dezelfde Azure-directory bevinden om de zelfstudie te kunnen voltooien.
>
>

Toepassingen die gebruikmaken van een sleutelkluis moeten een token van Azure Active Directory gebruiken om zich te verifiëren. Hiervoor moet de eigenaar van de toepassing deze eerst registreren bij Azure Active Directory. Aan het eind van het registratieproces ontvangt de eigenaar de volgende waarden:

* Een **toepassings-ID** (ook wel een client-id genoemd) en een **verificatiesleutel** (ook wel het gedeelde geheim genoemd). De toepassing moet van deze waarden aanbieden aan Azure Active Directory een token ophalen. Hoe de toepassing is geconfigureerd om dit te doen, is afhankelijk van de toepassing. In de voorbeeldtoepassing voor Sleutelkluis configureert de eigenaar van de toepassing deze waarden in het bestand app.config.

De toepassing registreren in Azure Active Directory:

1. Meld u aan bij Azure Portal.
2. Klik aan de linkerkant op **Azure Active Directory**, en selecteer vervolgens de directory waarin u uw toepassing wilt registreren. <br> <br> 

> [!Note] 
> U moet de directory waarin het Azure-abonnement waarmee u de sleutelkluis hebt gemaakt. Als u niet weet welke directory dit is, klikt u op **Instellingen**, identificeert u het abonnement waarmee u de sleutelkluis hebt gemaakt en noteert u de directorynaam die in de laatste kolom wordt weergegeven.

3. Klik op **TOEPASSINGEN**. Als er zijn geen apps aan uw directory zijn toegevoegd, deze pagina alleen wordt weergegeven de **een App toevoegen** koppeling. Klik op de koppeling of u kunt ook klikken op de **toevoegen** op de opdrachtbalk.
4. In de wizard **TOEPASSING TOEVOEGEN** klikt u op de pagina **What do you want to do?** (Wat wilt u doen?) op **Add an application my organization is developing** (Een toepassing toevoegen die door mijn organisatie wordt ontwikkeld).
5. Op de **Vertel ons over uw toepassing** pagina, Geef een naam voor uw toepassing en selecteer **WEBTOEPASSING en/of WEB-API** (de standaardinstelling). Klik op het volgende pictogram.
6. Geef op de pagina **App-eigenschappen** de **AANMELDINGS-URL** en **APP ID URI** (URI VAN DE APP-ID) voor uw webtoepassing op. Als uw toepassing deze waarden niet heeft, kunt u voor deze stap zelf iets verzinnen (u kunt voor beide vakken bijvoorbeeld http://test1.contoso.com opgeven). Het maakt niet uit of deze sites bestaan. Belangrijk is dat voor elke toepassing in uw directory een andere URI van de app-id wordt gebruikt. De directory gebruikt deze tekenreeks om uw app te identificeren.
7. Klik op het pictogram voltooid Sla uw wijzigingen in de wizard.
8. Klik op de pagina Quick Start op **configureren**.
9. Schuif naar de sectie **sleutels**, selecteer de duur en klik vervolgens op **OPSLAAN**. De pagina wordt vernieuwd en bevat nu een sleutelwaarde. U moet nu uw toepassing configureren met deze sleutelwaarde en de waarde **CLIENT-ID**. (Instructies voor deze configuratie zijn toepassingsspecifiek.)
10. Kopieer de client-id-waarde op deze pagina. Deze waarde gebruikt in de volgende stap om machtigingen voor uw kluis in te stellen.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing toestemming geven om de sleutel of het geheim te gebruiken
Voor het autoriseren van de toepassing toegang krijgen tot de sleutel of geheim in de kluis, gebruikt u de `az keyvault set-policy` opdracht.

Bijvoorbeeld, als de kluisnaam van uw ContosoKeyVault is en de toepassing die u wilt machtigen client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, voert u het volgende:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Als u dezelfde toepassing wilt autoriseren voor het lezen van geheimen in uw kluis, voert u de volgende opdracht uit:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u een Hardware Security Module (HSM) wilt gebruiken
Voor de zekerheid kunt u sleutels in HSM's (Hardware Security Module) importeren of genereren die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

U moet een kluis-abonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben voor het maken van deze met HSM beveiligde sleutels.

Wanneer u de keyvault maakt, voegt u de parameter 'sku':

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een HSM beveiligde sleutel:

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel van een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Zie voor meer instructies over hoe dit BYOK-pakket te genereren gedetailleerde, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
Als u de sleutelkluis en de sleutel of geheim niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de `az keyvault delete` opdracht:

```
az keyvault delete --name 'ContosoKeyVault'
```

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere opdrachten Azure platformoverschrijdende opdrachtregelinterface
Andere opdrachten die u wellicht nuttig voor het beheer van Azure Sleutelkluis.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

AZ keyvault sleutellijst--naam sleutelkluis 'ContosoKeyVault'

Deze opdracht wordt een volledige lijst met eigenschappen voor de opgegeven sleutel weergegeven:

AZ keyvault sleutel weergeven--kluisnaam ContosoKeyVault--de naam 'ContosoFirstKey'

Met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel:

AZ keyvault geheime lijst--naam sleutelkluis 'ContosoKeyVault'

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

AZ keyvault sleutel verwijderen--kluisnaam ContosoKeyVault--de naam 'ContosoFirstKey'

Hier volgt een voorbeeld van hoe u een specifiek geheim verwijdert:

AZ keyvault geheim verwijderen--kluisnaam ContosoKeyVault--de naam 'SQLPassword'


## <a name="next-steps"></a>Volgende stappen
Zie voor een volledig overzicht van Azure CLI voor sleutelkluis-opdrachten, [Sleutelkluis CLI-verwijzing](/cli/azure/keyvault)

Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.
