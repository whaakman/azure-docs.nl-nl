---
title: Problemen oplossen met Azure Automation-Runbooks
description: Meer informatie over het oplossen van problemen met Azure Automation-runbooks
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: b96d723f6c7ca423343c0586f59770abb55ada9f
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929346"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Fouten met runbooks oplossen

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatiefouten bij het werken met Azure Automation-runbooks

### <a name="sign-in-failed"></a>Scenario: Meld u aan bij Azure-Account is mislukt

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met de `Add-AzureAccount` of `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op als de naam van de referentie-asset niet geldig is of als de gebruikersnaam en het wachtwoord die u hebt gebruikt voor het instellen van het Automation-referentieasset niet geldig zijn.

#### <a name="resolution"></a>Oplossing

Om te bepalen wat er mis is, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat u geen speciale tekens heeft, met inbegrip van de **@** teken in de naam van Automation-referentie asset die u gebruikt voor het verbinding maken met Azure.  
2. Controleer dat u de gebruikersnaam en het wachtwoord die zijn opgeslagen in de Azure Automation-referentie in uw lokale PowerShell ISE-editor kunt gebruiken. U kunt dit doen door het uitvoeren van de volgende cmdlets in PowerShell ISE:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Als de verificatie lokaal mislukt, betekent dit dat u uw Azure Active Directory-referenties juist nog niet hebt ingesteld. Raadpleeg [zich verifiëren bij Azure met behulp van Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogbericht om op te halen van de Azure Active Directory-account juist ingesteld.  

### <a name="unable-to-find-subscription"></a>Scenario: Kan het Azure-abonnement vinden

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met de `Select-AzureSubscription` of `Select-AzureRmSubscription` cmdlets.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Fout

Deze fout treedt op als naam van het abonnement niet geldig is of als de Azure Active Directory-gebruiker die is bij het ophalen van details van het abonnement is niet geconfigureerd als een beheerder van het abonnement.

#### <a name="resolution"></a>Oplossing

Om te bepalen als u hebt geverifieerd naar Azure en hebben toegang tot het abonnement dat u wilt selecteren, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat u uitvoert het **Add-AzureAccount** voordat u de **Select-AzureSubscription** cmdlet.  
2. Als u nog steeds deze foutmelding ziet, wijzigt u de code door toe te voegen de **Get-AzureSubscription** cmdlet hieronder de **Add-AzureAccount** cmdlet en vervolgens de code wordt uitgevoerd. Nu kunt u controleren of de uitvoer van Get-Azure-abonnement de details van uw abonnement bevat.  

   * Als u niet alle abonnementsgegevens in de uitvoer ziet, betekent dit dat het abonnement nog niet is geïnitialiseerd.  
   * Als u de details van het abonnement in de uitvoer ziet, controleert u of dat u van het juiste abonnementsnaam of ID met gebruikmaakt de **Select-AzureSubscription** cmdlet.

### <a name="auth-failed-mfa"></a>Scenario: De verificatie bij Azure is mislukt omdat de multi-factor authentication is ingeschakeld

#### <a name="issue"></a>Probleem

U ontvangt het de volgende fout wanneer verificatie bij Azure met uw Azure gebruikersnaam en wachtwoord:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Oorzaak

Als u MFA op uw Azure-account hebt, kunt u een Azure Active Directory-gebruiker niet gebruiken voor verificatie op Azure. In plaats daarvan moet u een certificaat of een service-principal gebruiken om te worden geverifieerd bij Azure.

#### <a name="resolution"></a>Oplossing

Raadpleeg voor het gebruik van een certificaat met de klassieke Azure-implementatie-cmdlets voor model, [maken en toevoegen van een certificaat voor het beheren van Azure-services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Voor het gebruik van een service-principal met Azure Resource Manager-cmdlets, raadpleegt u [service-principal met behulp van Azure portal maken](../../azure-resource-manager/resource-group-create-service-principal-portal.md) en [verifiëren van een service-principal met Azure Resource Manager.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Veelvoorkomende fouten bij het werken met runbooks

### <a name="job-attempted-3-times"></a>Scenario: Het begin van de runbook-taak drie keer is uitgevoerd, maar deze kan niet worden gestart elke keer

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. Limiet voor geheugen. Er zijn voorgeschreven limiet op hoeveel geheugen toegewezen aan een Sandbox [Automation Servicelimieten](../../azure-subscription-service-limits.md#automation-limits) , zodat een taak uitvoeren kan als er meer dan 400 MB aan geheugen.

2. Module die incompatibel is. Dit kan gebeuren als de module-afhankelijkheden niet juist zijn en als dat niet het geval is, uw runbook doorgaans retourneert 'Opdracht niet gevonden' of 'Kan niet binden parameter'-bericht.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Voorgestelde methoden voor het werk binnen de geheugenlimiet zijn de werkbelasting tussen meerdere runbooks splitsen, niet zo veel gegevens in het geheugen, om onnodige uitvoer schrijven in uw runbooks niet te verwerken of houd rekening met het aantal controlepunten u in uw PowerShell-werkstroom schrijven runbooks.  

* Uw Azure-modules bijwerken met de volgende stappen [het bijwerken van Azure PowerShell-modules in Azure Automation](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Scenario: Runbook is mislukt vanwege gedeserialiseerde object

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Oorzaak

Als uw runbook een PowerShell-werkstroom is, slaat deze complexe objecten in een gedeserialiseerde indeling om de status van uw runbook behouden als de werkstroom wordt onderbroken.

#### <a name="resolution"></a>Oplossing

Een van de volgende drie oplossingen kunt dit probleem oplossen:

1. Als u complexe objecten van een cmdlet naar een andere zijn sluizen, moet u deze cmdlets verpakken in een InlineScript.
2. De naam of de waarde die u nodig hebt van het complexe object in plaats van het doorgeven van het gehele object doorgegeven.
3. Een PowerShell-runbook gebruiken in plaats van een PowerShell Workflow-runbook.

### <a name="quota-exceeded"></a>Scenario: De Runbook-taak is mislukt omdat het toegewezen quotum overschreden

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de fout:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer de taakuitvoering is groter dan het gratis quotum van 500 minuten voor uw account. Dit quotum is van toepassing op alle soorten taken worden uitgevoerd, zoals een taak testen, starten van een taak vanuit de portal, een taak wordt uitgevoerd met behulp van webhooks en plannen van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Zie voor meer informatie over de prijzen voor automatisering, [Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Oplossing

Als u gebruiken van meer dan 500 minuten per maand wordt verwerkt wilt, moet u uw abonnement wijzigen van de gratis laag naar de Basic-laag. U kunt upgraden naar de Basic-laag door te nemen van de volgende stappen uit:  

1. Meld u aan bij uw Azure-abonnement  
2. Selecteer het Automation-account dat u wilt upgraden  
3. Klik op **instellingen** > **prijzen**.
4. Klik op **inschakelen** op de pagina van beneden naar uw account bijwerken naar de **Basic** laag.

### <a name="cmdlet-not-recognized"></a>Scenario: Cmdlet niet wordt herkend als een runbook uitvoeren

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de fout:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt wanneer de PowerShell-engine de cmdlet die u in uw runbook gebruikt niet kan vinden. Dit kan zijn omdat de module met de cmdlet in het account ontbreekt, er een naamconflict geeft met de naam van een runbook wordt of de cmdlet ook in een andere module bestaat en de naam kan niet worden omgezet in Automation.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:  

* Controleer of u de cmdletnaam van de juist hebt ingevoerd.  
* Zorg ervoor dat de cmdlet bestaat in uw Automation-account en er zijn geen conflicten. Om te controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkingsmodus en zoeken naar de cmdlet die u wilt zoeken in de bibliotheek of uitvoeren `Get-Command <CommandName>`. Nadat u hebt gevalideerd die de cmdlet voor het account beschikbaar is, en dat er geen naam conflicteert met andere cmdlets of runbooks, toe te voegen aan het canvas en zorg ervoor dat u van een geldige parameter ingesteld in uw runbook gebruikmaakt.  
* Als er een naamconflict en de cmdlet beschikbaar in twee verschillende modules is, kunt u deze kunt oplossen met behulp van de volledig gekwalificeerde naam van de cmdlet. Bijvoorbeeld, kunt u **ModuleName\CmdletName**.  
* Als u de runbook on-premises worden uitgevoerd in een hybrid worker-groep en zorg ervoor dat is de module/cmdlet op de computer die als host fungeert voor de hybrid worker geïnstalleerd.

### <a name="evicted-from-checkpoint"></a>Scenario: Een langlopende runbook consistent is mislukt met de uitzondering: "de taak kan niet worden voortgezet te voeren, omdat het herhaaldelijk is verwijderd uit het hetzelfde controlepunt"

#### <a name="issue"></a>Probleem

Dit gedrag is standaard vanwege de bewaking van 'Geoorloofd delen' van de processen in Azure Automation, waarmee automatisch een runbook wordt geblokkeerd als deze meer dan drie uur wordt uitgevoerd. Het foutbericht geretourneerd biedt echter geen in 'wat nu' opties.

#### <a name="cause"></a>Oorzaak

Een runbook kan worden onderbroken voor een aantal oorzaken hebben. Wordt onderbroken gebeurt meestal vanwege fouten. Bijvoorbeeld, een niet-onderschepte uitzondering in een runbook, een netwerkstoring of een crash van de Runbook-Worker waarop het runbook wordt uitgevoerd, alle ertoe leiden dat het runbook wordt onderbroken en starten vanaf de laatste controlepunt wanneer hervat.

#### <a name="resolution"></a>Oplossing

De gedocumenteerde oplossing om te voorkomen dat dit probleem is met controlepunten in een werkstroom. Raadpleeg voor meer informatie, [Learning PowerShell-werkstromen](../automation-powershell-workflow.md#checkpoints). Een meer gedetailleerde informatie over het 'Geoorloofd delen' en de controlepunten vindt u in dit blogartikel [met behulp van controlepunten in Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

### <a name="long-running-runbook"></a>Scenario: Een langlopende runbook kan niet worden voltooid

#### <a name="issue"></a>Probleem

Dit gedrag is inherent aan Azure-sandboxes geladen vanwege de bewaking van 'Geoorloofd delen' van de processen in Azure Automation, waarmee automatisch een runbook wordt geblokkeerd als deze meer dan drie uur wordt uitgevoerd.

#### <a name="cause"></a>Oorzaak

Het runbook werd uitgevoerd dan de limiet 3 uur is toegestaan door evenredige deel in een Azure-Sandbox

#### <a name="resolution"></a>Oplossing

De aanbevolen oplossing is om uit te voeren van het runbook op een [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Hybrid Workers worden niet beperkt door de [evenredige deel](../automation-runbook-execution.md#fair-share) 3 uur runbook beperken die Azure-sandboxes geladen zijn.

## <a name="common-errors-when-importing-modules"></a>Veelvoorkomende fouten bij het importeren van modules

### <a name="module-fails-to-import"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd na het importeren

#### <a name="issue"></a>Probleem

Een module te importeren is mislukt of is geïmporteerd, maar geen cmdlets worden geëxtraheerd.

#### <a name="cause"></a>Oorzaak

Er zijn enkele veelvoorkomende redenen waarom dat een module niet met succes in Azure Automation importeren kan:

* De structuur komt niet overeen met de structuur die het Automation nodig heeft om te worden.
* De module is afhankelijk van een andere module die niet is geïmplementeerd voor uw Automation-account.
* De module ontbreekt de bijbehorende afhankelijkheden in de map.
* De `New-AzureRmAutomationModule` cmdlet voor het uploaden van de module wordt gebruikt en u hebt niet de volledige opslagpad gegeven of de module met behulp van een openbaar toegankelijke URL niet geladen.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Zorg ervoor dat de module de volgende indeling heeft: ModuleName.Zip **->** ModuleName of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)
* Open het .psd1-bestand en zien of de module afhankelijkheden heeft. Als dit het geval is, moet u deze modules uploaden naar het Automation-account.
* Zorg ervoor dat eventuele waarnaar wordt verwezen, dll's zijn aanwezig in de modulemap.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet wordt gezien of kan niet aan het oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.