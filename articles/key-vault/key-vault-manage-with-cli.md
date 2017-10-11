---
title: Beheren van Azure Sleutelkluis met CLI | Microsoft Docs
description: Gebruik deze handleiding om algemene taken in de Sleutelkluis automatiseren met behulp van de CLI
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: c2565a742ce4f6ab5f7639a54c4a475f00cbc260
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli"></a>Beheren van de Sleutelkluis met CLI

Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

## <a name="introduction"></a>Inleiding

Gebruik deze handleiding om aan de slag te gaan met Azure Sleutelkluis en een geharde container (een kluis) in Azure te maken om cryptografiesleutels en geheimen op te slaan in Azure. Dit helpt u bij het proces van het gebruik van Azure-opdrachtregelinterface voor Cross-Platform een kluis met een sleutel of het wachtwoord dat u vervolgens met een Azure-toepassing gebruiken kunt maken. Deze vervolgens ziet u hoe een toepassing kunt vervolgens sleutel of het wachtwoord.

**Geschatte duur:** 20 minuten

> [!NOTE]
> Deze zelfstudie bevat geen instructies voor het schrijven van de Azure-toepassing of een van de stappen bevat, die wordt beschreven hoe een toepassing gebruik van een sleutel of geheim in de sleutelkluis toestemming geeft.
> 
> Het is momenteel niet mogelijk om Azure Sleutelkluis in de Azure-portal te configureren. Gebruik in plaats daarvan deze instructies platformoverschrijdende opdrachtregelinterface. Zie voor instructies voor Azure PowerShell [deze equivalente zelfstudie](key-vault-get-started.md).
> 
> 

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

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager in Azure platformoverschrijdende opdrachtregelinterface lezen:

* [Het installeren en configureren van Azure-opdrachtregelinterface voor Cross-Platform](../cli-install-nodejs.md)
* [Met behulp van Azure platformoverschrijdende opdrachtregelinterface met Azure Resource Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Meld u aan met een organisatie-account met de volgende opdracht:

    azure login -u username -p password

of als u wilt zich aanmelden door interactief typen

    azure login

> [!NOTE]
> De methode aanmelding werkt alleen met organisatie-account. Een organisatie-account is een gebruiker die wordt beheerd door uw organisatie en gedefinieerd in Azure Active Directory-tenant van uw organisatie.
> 
> 

Als u nog geen een organisatie-account en een Microsoft-account gebruikt zich aanmelden bij uw Azure-abonnement, kunt u gemakkelijk maken met de volgende stappen uit.

1. Meld u aan bij de aanmelding bij de [Azure Management Portal](https://manage.windowsazure.com/), en klik op Active Directory.
2. Als er geen map bestaat, selecteer uw directory maken en voer de vereiste gegevens.
3. Selecteer uw directory en een nieuwe gebruiker toevoegen. Deze nieuwe gebruiker is een organisatieaccount. Tijdens het maken van de gebruiker die u hebt opgegeven met zowel een e-mailadres voor de gebruiker en een tijdelijk wachtwoord. Deze gegevens niet opslaan omdat het wordt gebruikt in een andere stap.
4. Vanuit de portal, selecteert u instellingen en selecteer vervolgens de beheerders. Selecteer de optie toevoegen en de nieuwe gebruiker toevoegen als medebeheerder. Hierdoor kan de organisatie-account voor het beheren van uw Azure-abonnement.
5. Ten slotte, meld u af bij de Azure-portal en vervolgens weer aanmelden met de nieuwe organisatie-account. Als dit de eerste keer aangemeld met dit account, wordt u gevraagd het wachtwoord te wijzigen.

Zie voor meer informatie over het gebruik van een organisatie-account met Microsoft Azure [aanmelden voor Microsoft Azure als organisatie](../active-directory/sign-up-organization.md).

Als u meerdere abonnementen hebt en u een specifiek abonnement voor Azure Sleutelkluis wilt gebruiken, typt u het volgende om de abonnementen voor uw account weer te geven:

    azure account list

Typ vervolgens het volgende om het abonnement op te geven dat u wilt gebruiken:

    azure account set <subscription name>

Zie voor meer informatie over het configureren van Azure platformoverschrijdende opdrachtregelinterface [installeren en configureren van Azure platformoverschrijdende Command-Line Interface](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Overschakelen op het gebruik van Azure Resource Manager
Azure Resource Manager vereist dat de Sleutelkluis, dus typ het volgende overschakelen naar modus Azure Resource Manager:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken
Wanneer u Azure Resource Manager gebruikt, worden alle gerelateerde resources gemaakt binnen een resourcegroep. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

    azure group create 'ContosoResourceGroup' 'East Asia'

De eerste parameter is de naam van resourcegroep en de tweede parameter is de locatie. Gebruik de opdracht voor locatie, `azure location list` om te bepalen hoe een alternatieve locatie op de in dit voorbeeld opgeven. Als u meer informatie nodig hebt, typt u:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>De registerbronprovider is Sleutelkluis
Zorg ervoor dat Sleutelkluis-resourceprovider is geregistreerd in uw abonnement:

`azure provider register Microsoft.KeyVault`

Dit hoeft slechts één keer per abonnement worden uitgevoerd.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Gebruik de `azure keyvault create` opdracht om een sleutelkluis te maken. Dit script heeft drie verplichte parameters: de naam van een resource-groep, de naam van een sleutelkluis en de geografische locatie.

Bijvoorbeeld, als u de kluisnaam ContosoKeyVault gebruikt, de naam van de resourcegroep van ContosoResourceGroup en de locatie van Oost-Azië, typt u:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u zojuist hebt gemaakt. De twee belangrijkste eigenschappen zijn:

* **Naam**: In het voorbeeld is dit ContosoKeyVault. U gebruikt deze naam voor andere Sleutelkluis-cmdlets.
* **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die via de REST API gebruikmaken van uw kluis, moeten deze URI gebruiken.

Uw Azure-account is nu gemachtigd om alle bewerkingen op deze sleutelkluis uit te voeren. Tot dusver is er nog niemand anders gemachtigd.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een sleutel of geheim toevoegen aan de sleutelkluis

Als u wilt dat Azure Sleutelkluis een softwarematig beveiligde sleutel voor u te maken, gebruikt u de `azure key create` opdracht in en typt u het volgende:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Echter, hebt u een bestaande sleutel in een .pem-bestand opgeslagen als een lokaal bestand in een bestand met de naam softkey.pem die u wilt uploaden naar Azure Sleutelkluis, typt u het volgende als u wilt importeren van de sleutel van de. PEM-bestand dat de sleutel met software in de Sleutelkluis-service beveiligt:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

U kunt nu naar de sleutel die u hebt gemaakt of geüpload naar Azure Sleutelkluis, met behulp van de URI verwijzen. Gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** altijd de huidige versie en gebruik **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** voor deze specifieke versie.

Als u wilt een geheim toevoegen aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd voor Azure Sleutelkluis, typt u het volgende:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Als u nu naar het wachtwoord wilt verwijzen dat u hebt toegevoegd aan Azure Sleutelkluis, kunt u de URI van het wachtwoord gebruiken. Gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om altijd de huidige versie op te halen en gebruik **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om deze specifieke versie op te halen.

We bekijken de sleutel of geheim dat u zojuist hebt gemaakt:

* Als u de sleutel wilt weergeven, typt u: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Als u het geheim wilt weergeven, typt u: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

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
2. Klik aan de linkerkant op **Active Directory** en selecteer vervolgens de directory waarin u de toepassing wilt registreren. <br> <br> 

>[!NOTE] 
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
Voor het autoriseren van de toepassing toegang krijgen tot de sleutel of geheim in de kluis, gebruikt u de `azure keyvault set-policy` opdracht.

Bijvoorbeeld, als de kluisnaam van uw ContosoKeyVault is en de toepassing die u wilt machtigen client-ID van 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u machtigen van de toepassing wilt te ontsleutelen en meld u aan met de sleutels in uw kluis, voert u het volgende:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> Als u op Windows-opdrachtprompt uitvoert, moet u enkele aanhalingstekens vervangen door dubbele aanhalingstekens en ook escape voor de interne dubbele aanhalingstekens. Bijvoorbeeld: ' [\"ontsleutelen\",\"aanmelding\"] '.
> 
> 

Als u dezelfde toepassing wilt autoriseren voor het lezen van geheimen in uw kluis, voert u de volgende opdracht uit:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u een Hardware Security Module (HSM) wilt gebruiken
Voor de zekerheid kunt u sleutels in HSM's (Hardware Security Module) importeren of genereren die de HSM-grens nooit verlaten. De HSM's zijn FIPS 140-2 Level 2-gevalideerde modules. Als deze vereiste niet van toepassing is op u, kunt u deze sectie overslaan om naar [De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen](#delete-the-key-vault-and-associated-keys-and-secrets) te gaan.

U moet een kluis-abonnement dat ondersteuning biedt voor met HSM beveiligde sleutels hebben voor het maken van deze met HSM beveiligde sleutels.

Wanneer u de keyvault maakt, voegt u de parameter 'sku':

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

U kunt softwarematige beveiligde sleutels (zoals hiervoor) en met HSM beveiligde sleutels toevoegen aan deze kluis. Stel de doel-parameter op 'HSM' voor het maken van een HSM beveiligde sleutel:

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

U kunt de volgende opdracht gebruiken voor het importeren van een sleutel van een .pem-bestand op uw computer. Met deze opdracht wordt de sleutel geïmporteerd in HSM's in de Sleutelkluis-service:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Met de volgende opdracht wordt een BYOK-pakket (Bring Your Own Key) geïmporteerd. Hiermee kunt u de sleutel in uw lokale HSM genereren en overdragen naar HSM's in de Sleutelkluis-service, zonder dat de sleutel de HSM-grens verlaat:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Zie voor meer instructies over hoe dit BYOK-pakket te genereren gedetailleerde, [HSM-Protected sleutels gebruiken met Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>De sleutelkluis en de bijbehorende sleutels en geheimen verwijderen
Als u de sleutelkluis en de sleutel of geheim niet meer nodig hebt, kunt u de sleutelkluis verwijderen met behulp van de opdracht azure keyvault verwijderen:

    azure keyvault delete --vault-name 'ContosoKeyVault'

U kunt ook een volledige Azure-resourcegroep verwijderen. Deze bevat de sleutelkluis en alle andere resources die u hebt opgenomen in de groep:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere opdrachten Azure platformoverschrijdende opdrachtregelinterface
Andere opdrachten die u wellicht nuttig voor het beheer van Azure Sleutelkluis.

Met deze opdracht worden alle sleutels en geselecteerde eigenschappen weergegeven in een tabel:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Deze opdracht wordt een volledige lijst met eigenschappen voor de opgegeven sleutel weergegeven:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Met deze opdracht worden alle geheime namen en geselecteerde eigenschappen weergegeven in een tabel:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Hier volgt een voorbeeld van hoe u een specifieke sleutel verwijdert:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Hier volgt een voorbeeld van hoe u een specifiek geheim verwijdert:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Volgende stappen
Zie de [Ontwikkelaarshandleiding voor Azure Key Vault](key-vault-developers-guide.md) voor het programmeren van verwijzingen.

