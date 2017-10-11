---
title: Meld u aan bij Azure met CLI | Microsoft Docs
description: Verbinding maken met uw Azure-abonnement met de Azure-opdrachtregelinterface (Azure CLI) voor Mac, Linux en Windows
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="log-in-to-azure-from-the-azure-cli"></a>Meld u aan bij Azure met Azure CLI
De Azure CLI is een set van open-source, platformoverschrijdende opdrachten voor het werken met Azure-resources. Dit artikel worden de verschillende manieren waarop u de referenties van uw Azure-account voor de Azure CLI verbinding met uw Azure-abonnement op te geven:

* Voer de `azure login` CLI-opdracht om te verifiëren via Azure Active Directory. Deze methode biedt u toegang tot de CLI-opdrachten in beide [opdracht modi](#cli-command-modes). Wanneer u de opdracht zonder extra opties uitvoert `azure login` vraagt u om door te gaan interactief aanmelden via een webportal. Voor extra `azure login` opdracht Opties, raadpleegt u de scenario's in dit artikel of type `azure login --help`.
* Als u alleen wilt gebruiken van Azure Service Management modus CLI-opdrachten (niet aanbevolen voor de meeste nieuwe implementaties), kunt u downloaden en installeren van een bestand met instellingen publiceren op uw computer.

Als u nog niet de CLI hebt geïnstalleerd, raadpleegt u [Azure CLI installeren](cli-install-nodejs.md). Als u geen Azure-abonnement hebt, kunt u binnen een paar minuten een [gratis account](http://azure.microsoft.com/free/) maken.

Zie voor achtergrondinformatie over andere account-id's en Azure-abonnementen [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Scenario 1: azure-aanmelding met interactieve aanmelding
Met bepaalde accounts, moet u om uit te voeren met de CLI `azure login` en ga vervolgens door de aanmelding met een webbrowser via een webportal, een proces genaamd *interactieve aanmelding*. Een veelvoorkomende reden is wanneer u een account voor werk of school hebt (ook wel een *organisatieaccount*) die is ingesteld om meervoudige verificatie te vereisen. Ook interactieve aanmelding met je Microsoft-account gebruiken als u wilt gebruiken van opdrachten in de modus Resource Manager.

Interactieve aanmelding is eenvoudig: type `azure login` --zonder opties--zoals weergegeven in het volgende voorbeeld:

```
azure login
```                                                                                             

De uitvoer ziet er ongeveer als volgt:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
Kopieer de code die u in de opdrachtuitvoer aangeboden en open een browser naar http://aka.ms/devicelogin of andere pagina als u opgeeft. (U kunt een browser op dezelfde computer of op een andere computer of apparaat kunt openen.) Voer de code en wordt u gevraagd de gebruikersnaam en wachtwoord invoeren voor de identiteit die u wilt gebruiken. Wanneer dit proces is voltooid, geeft de opdrachtshell van de aanmelding is voltooid. Dit kan als volgt uitzien:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Met interactieve aanmelding, worden verificatie en autorisatie uitgevoerd met Azure Active Directory. Als u de identiteit van een Microsoft-account gebruikt, heeft uw Azure Active Directory-standaarddomein toegang tot het aanmeldingsproces. (Als u zich aangemeld voor een gratis Azure-account, Azure Active Directory automatisch gemaakt een standaarddomein voor uw account.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Scenario 2: azure-aanmelding met een gebruikersnaam en wachtwoord
Gebruik de `azure login` opdracht met de gebruikersnaam (`-u`) parameter om te verifiëren wanneer u wilt gebruiken een werk- of schoolaccount die multifactor-verificatie niet vereist. U wordt gevraagd op de opdrachtregel voor het wachtwoord (of u kunt desgewenst het wachtwoord doorgeven als een extra parameter van de `azure login` opdracht). Het volgende voorbeeld wordt de gebruikersnaam van een organisatieaccount doorgegeven:

    azure login -u myUserName@contoso.onmicrosoft.com

U wordt gevraagd uw wachtwoord in te voeren:

    info:    Executing command login
    Password: *********

De aanmelding wordt voltooid.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Als dit de eerste keer aangemeld met deze referenties, wordt u gevraagd om te controleren die u wilt geen verificatietoken in de cache. Deze vraag vindt ook plaats als u eerder hebt gebruikt de `azure logout` opdracht (Zie verderop in het artikel). Uitvoeren als u wilt deze prompt voor automatiseringsscenario's omzeilen, `azure login` met de `-q` parameter.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Scenario 3: azure-aanmelding met een service-principal
Als u een service-principal voor een Active Directory-toepassing maken en de service-principal machtigingen op uw abonnement heeft, kunt u de `azure login` opdracht voor het verifiëren van de service-principal. Afhankelijk van uw scenario kunt u de referenties van de service-principal opgeven als expliciete parameters van de `azure login` opdracht. De volgende opdracht geeft bijvoorbeeld de service principal name en de Active Directory-tenant-ID:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

U wordt gevraagd het wachtwoord op te geven. U kunt ook de referenties van de via een script of toepassing code voor CLI of een certificaat voor verificatie van de service-principal niet-interactief voor automatiseringsscenario's gebruiken. Zie voor meer informatie en voorbeelden [verifiëren van een service-principal met Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Scenario 4: Gebruik een bestand met publicatie-instellingen
Als u alleen hoeft de Azure Service Management modus CLI-opdrachten (bijvoorbeeld voor het implementeren van virtuele Azure-machines in het klassieke implementatiemodel) gebruiken, kunt u verbinding maken met behulp van een bestand met publicatie-instellingen. Deze methode wordt een certificaat geïnstalleerd op uw lokale computer waarmee u beheertaken voor uitvoeren als het abonnement en het certificaat geldig zijn.

* **Voor het downloaden van het bestand publiceren instellingen** voor uw account, zorg ervoor dat de CLI in Service Management-modus door te typen `azure config mode asm`. Voer de volgende opdracht:

        azure account download

Hiermee opent u de standaardbrowser en vraagt u zich aanmeldt bij de [klassieke Azure-portal](https://manage.windowsazure.com). Nadat u zich aanmeldt, een `.publishsettings` bestand downloads. Noteer waar dit bestand wordt opgeslagen.

> [!NOTE]
> Als uw account gekoppeld aan meerdere Azure Active Directory-tenants is, kunt u worden gevraagd om te selecteren welke Active Directory die u wilt een publish settings-bestand voor downloaden.
>
>

Wanneer met behulp van de downloadpagina of via de klassieke Azure portal, wordt de geselecteerde Active Directory de standaardwaarde aan die door de klassieke portal en download de pagina. Zodra een standaard actief is, ziet u de tekst '**Klik hier om terug te keren naar de selectiepagina**' aan de bovenkant van de downloadpagina. Gebruik de onderstaande koppeling om terug te keren naar de selectiepagina.

* **Het bestand publiceren-instellingen te importeren**, voer de volgende opdracht:

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> Na het importeren van de publicatie-instellingen, verwijdert u de `.publishsettings` bestand. Het is niet langer vereist voor de Azure CLI en vormt een beveiligingsrisico, omdat omdat deze kan worden gebruikt voor toegang tot uw abonnement.
>
>

## <a name="cli-command-modes"></a>CLI-opdrachtmodi
De Azure CLI biedt twee opdrachtmodi voor het werken met Azure-resources met andere opdrachtsets:

* **Resource Manager-modus** : voor het werken met Azure-resources in het Resource Manager-implementatiemodel. Uitvoeren als u wilt instellen in deze modus, `azure config mode arm`.
* **Service Management-modus** : voor het werken met Azure-resources in het klassieke implementatiemodel. Uitvoeren als u wilt instellen in deze modus, `azure config mode asm`.

Toen geïnstalleerd, is de huidige release van de CLI in de modus Resource Manager.

> [!NOTE]
> De modus Resource Manager en Service Management-modus elkaar wederzijds uit. Dat wil zeggen, kunnen niet resources die zijn gemaakt in de modus voor één worden beheerd vanaf de andere modus.
>
>

## <a name="multiple-subscriptions"></a>Meerdere abonnementen
Als u meerdere Azure-abonnementen hebt, verleent verbinding maken met Azure toegang tot alle abonnementen die zijn gekoppeld aan uw referenties. Een abonnement is geselecteerd als de standaard en gebruikt door de Azure CLI bij het uitvoeren van bewerkingen. U kunt de abonnementen kunt weergeven, met inbegrip van de huidige standaardabonnement, met behulp van de `azure account list` opdracht. Met deze opdracht retourneert informatie ziet er als volgt:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

In de bovenstaande lijst de **huidige** kolom wordt aangegeven dat het huidige standaardabonnement als Azure-sub-1. U kunt het standaardabonnement wijzigen met de `azure account set` opdracht en geeft u het abonnement dat u wilt gebruiken als de standaardwaarde. Bijvoorbeeld:

    azure account set Azure-sub-2

Hiermee wijzigt u het standaardabonnement op Azure-sub-2.

> [!NOTE]
> Het wijzigen van het standaardabonnement wordt direct van kracht en is een algemene wijziging; nieuwe Azure CLI-opdrachten gebruiken de nieuwe standaardabonnement of u ze vanaf de opdrachtregel hetzelfde exemplaar of een ander exemplaar uitvoeren.
>
>

Als u wilt een niet-standaard-abonnement met de Azure CLI gebruiken, maar niet wilt dat de huidige standaardwaarde te wijzigen, kunt u de `--subscription` optie voor de opdracht en geef de naam van het abonnement dat u wilt gebruiken voor de bewerking.

Wanneer u met uw Azure-abonnement verbonden bent, kunt u beginnen met de Azure CLI-opdrachten om te werken met Azure-resources.

## <a name="storage-of-cli-settings"></a>Opslag van CLI-instellingen
Of u zich aan met de `azure login` opdracht of het importeren van publicatie-instellingen, uw CLI-profiel en de logboeken worden opgeslagen in een `.azure` directory zich in uw `user` directory. Uw `user` map wordt beveiligd door het besturingssysteem. We raden u echter aan dat u extra stappen ondernemen voor het versleutelen van uw `user` directory. U kunt dit op de volgende manieren doen:

* Van Windows, de mapeigenschappen van de wijzigen of gebruik van BitLocker.
* Schakel op de Mac, FileVault voor de map.
* In Ubuntu gebruikt u de functie om de basismap te versleutelen. Andere Linux-distributies bieden vergelijkbare functies.

## <a name="logging-out"></a>Afmelden
Meld u af met de volgende opdracht:

    azure logout -u <username>

Als de abonnementen die zijn gekoppeld aan het account alleen met Active Directory, verwijderingen afmelden met informatie over het abonnement uit het lokale profiel worden geverifieerd. Echter, als een bestand met publicatie-instellingen ook voor de abonnementen is geïmporteerd, afmelden alleen verwijderingen Active Directory gegevens uit het lokale profiel gerelateerde.

## <a name="next-steps"></a>Volgende stappen
* Zie voor het gebruik van Azure CLI-opdrachten [Azure CLI-opdrachten in de modus Resource Manager](virtual-machines/azure-cli-arm-commands.md) en [Azure CLI-opdrachten in de modus van de Service Management](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* Als u meer informatie over de Azure CLI, broncode downloaden, problemen melden, of bijdragen aan het project, gaat u naar de [GitHub-opslagplaats voor de Azure CLI](https://github.com/azure/azure-xplat-cli).
* Als u met de Azure CLI of Azure problemen, gaat u naar de [Azure-Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
