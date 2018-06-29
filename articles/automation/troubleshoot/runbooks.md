---
title: Fouten met Azure Automation-Runbooks oplossen
description: Meer informatie over het oplossen van problemen met Azure Automation-runbooks
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: bb340b8439927f191bc4a22f385d85d4e21b1cdb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063866"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Fouten met runbooks oplossen

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatiefouten bij het werken met Azure Automation-runbooks

### <a name="sign-in-failed"></a>Scenario: Meld u bij de Azure-Account is mislukt

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout is opgetreden bij het werken met de `Add-AzureAccount` of `Connect-AzureRmAccount` cmdlets.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op als de referentienaam asset niet geldig is of als de gebruikersnaam en het wachtwoord dat u gebruikt voor het instellen van het Automation-referentieasset niet geldig zijn.

#### <a name="resolution"></a>Oplossing

Om te bepalen wat er mis is, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat er geen speciale tekens, inclusief de **@** teken in de naam van Automation-referentie asset die u gebruikt voor het verbinding maken met Azure.  
2. Controleer of u kunt de gebruikersnaam en wachtwoord die zijn opgeslagen in de Azure Automation-referentie in uw lokale PowerShell ISE-editor. U kunt dit doen door het uitvoeren van de volgende cmdlets in de PowerShell ISE:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Als uw verificatie is mislukt lokaal, dit betekent dat u uw Azure Active Directory-referenties correct hebt ingesteld. Raadpleeg [verificatie met Azure met Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogbericht ophalen van de Azure Active Directory-account juist ingesteld.  

### <a name="unable-to-find-subscription"></a>Scenario: Kan het Azure-abonnement vinden

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout is opgetreden bij het werken met de `Select-AzureSubscription` of `Select-AzureRmSubscription` cmdlets.:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Fout

Deze fout treedt op als de naam van het abonnement is niet geldig of als de Azure Active Directory-gebruiker die is bij het ophalen van de details van abonnement is niet geconfigureerd als een beheerder van het abonnement.

#### <a name="resolution"></a>Oplossing

Om te bepalen als u naar Azure hebt geverifieerd en toegang tot het abonnement dat u probeert hebben te selecteren, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat u uitvoert het **Add-AzureAccount** voordat u de **Select-AzureSubscription** cmdlet.  
2. Als u nog steeds deze foutmelding ziet, wijzigt u uw code door toe te voegen de **Get-AzureSubscription** cmdlet volgende de **Add-AzureAccount** cmdlet en vervolgens de code wordt uitgevoerd. Nu controleren of de uitvoer van Get-AzureSubscription de details van uw abonnement bevat.  

   * Als u niet alle details van abonnement in de uitvoer ziet, betekent dit dat het abonnement nog niet is geïnitialiseerd.  
   * Als u de details van abonnement in de uitvoer ziet, moet u ervoor zorgen dat u de juiste abonnementsnaam of ID met de **Select-AzureSubscription** cmdlet.

### <a name="auth-failed-mfa"></a>Scenario: Verificatie naar Azure is mislukt omdat de multi-factor authentication is ingeschakeld

#### <a name="issue"></a>Probleem

U ontvangt het de volgende fout wanneer verificatie met Azure met uw Azure-gebruikersnaam en wachtwoord:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Oorzaak

Als u multi-factor authentication-server op uw Azure-account hebt, kunt u een Azure Active Directory-gebruiker niet gebruiken voor verificatie op Azure. In plaats daarvan moet u een certificaat of een service-principal gebruiken voor verificatie op Azure.

#### <a name="resolution"></a>Oplossing

Als u wilt gebruiken een certificaat met de cmdlets van Azure classic deployment model, Raadpleeg [maken en het toevoegen van een certificaat voor het beheren van Azure-services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Raadpleeg voor het gebruik van een service-principal met Azure Resource Manager-cmdlets, [service principal met Azure portal maken](../../azure-resource-manager/resource-group-create-service-principal-portal.md) en [verifiëren van een service-principal met Azure Resource Manager.](../../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Veelvoorkomende fouten bij het werken met runbooks

### <a name="job-attempted-3-times"></a>Scenario: Het begin van de runbook-taak is drie keer geprobeerd, maar niet wordt gestart elke keer

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Oorzaak

Deze fout kan zijn veroorzaakt door de volgende redenen:

1. Limiet voor geheugen. Er zijn voorgeschreven limiet op hoeveel geheugen toegewezen aan een Sandbox [Servicelimieten Automation](../../azure-subscription-service-limits.md#automation-limits) zodat een taak deze mislukken kan als het gebruik van meer dan 400 MB aan geheugen.

2. Incompatibele module. Dit kan gebeuren als de module-afhankelijkheden zijn niet juist is en als dat niet het geval is, uw runbook doorgaans retourneert een '-opdracht niet gevonden' of 'Kan niet binden voor parameter'-bericht.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen oplossen het probleem:

* Voorgestelde methoden voor het samenwerken om binnen de geheugenlimiet zijn Splits de werkbelasting tussen meerdere runbooks, niet zo veel mogelijk gegevens in het geheugen, om onnodige uitvoer schrijven van uw runbooks niet te verwerken of overweeg hoeveel controlepunten die u in uw PowerShell-werkstroom schrijven runbooks.  

* Bijwerken van uw Azure-modules door de stappen te volgen [het bijwerken van Azure PowerShell-modules in Azure Automation](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Scenario: Runbook is mislukt vanwege deserialiseerbaar object

#### <a name="issue"></a>Probleem

Uw runbook is mislukt met de fout:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Oorzaak

Als uw runbook een PowerShell-werkstroom is, worden opgeslagen naar complexe objecten in een gedeserialiseerde indeling om uw runbook-status behouden als de werkstroom wordt onderbroken.

#### <a name="resolution"></a>Oplossing

Een van de volgende drie oplossingen kunt dit probleem oplossen:

1. Als u complexe objecten van een cmdlet naar een andere zijn sluizen, moet u deze cmdlets verpakken in een InlineScript.
2. Geeft de naam of waarde die u nodig hebt van het complexe object in plaats van het gehele object doorgeven.
3. Een PowerShell-runbook gebruiken in plaats van een PowerShell Workflow-runbook.

### <a name="quota-exceeded"></a>Scenario: De runbooktaak is mislukt omdat het toegewezen quotum overschreden

#### <a name="issue"></a>Probleem

De runbooktaak is mislukt met de fout:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer de taak wordt uitgevoerd, de beschikbare quota 500 minuut voor uw account overschrijdt. Dit quotum geldt voor alle typen taken uitvoeren zoals het testen van een taak, start een taak vanuit de portal, een taak wordt uitgevoerd met behulp van webhooks en plannen van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Zie voor meer informatie over prijzen voor automatisering, [Automation prijzen](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Oplossing

Als u gebruiken van meer dan 500 minuten per maand wordt verwerkt wilt, moet u uw abonnement te wijzigen van de gratis laag aan de laag Basic. U kunt upgraden naar de basisstaffel door de volgende stappen:  

1. Meld u aan bij uw Azure-abonnement  
2. Selecteer het Automation-account dat u wilt upgraden  
3. Klik op **instellingen** > **prijzen**.
4. Klik op **inschakelen** pagina onder uw account wilt bijwerken op de **Basic** laag.

### <a name="cmdlet-not-recognized"></a>Scenario: Cmdlet is niet herkend bij het uitvoeren van een runbook

#### <a name="issue"></a>Probleem

De runbooktaak is mislukt met de fout:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt als de engine PowerShell de cmdlet die u in uw runbook gebruikt niet kan vinden. Dit kan zijn omdat de module met de cmdlet in het account ontbreekt, er een naamconflict met de runbooknaam van een is, of de cmdlet ook in een andere module bestaat en de naam kan niet worden omgezet in Automation.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen oplossen het probleem:  

* Controleer of u de naam van de cmdlet juist hebt ingevoerd.  
* Controleer of de cmdlet bestaat in uw Automation-account en er zijn geen conflicten. Om te controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkingsmodus en zoek naar de cmdlet die u wilt zoeken in de bibliotheek of voer `Get-Command <CommandName>`. Zodra u hebt gevalideerd die de cmdlet voor het account beschikbaar is en dat er geen naam conflicteert met andere cmdlets of runbooks, toe te voegen aan het canvas en zorg ervoor dat u van een geldige parameters ingesteld in uw runbook gebruikmaakt.  
* Als er een naamconflict van de cmdlet is beschikbaar in twee verschillende modules, kunt u deze kunt oplossen met behulp van de volledig gekwalificeerde naam van de cmdlet. U kunt bijvoorbeeld **ModuleName\CmdletName**.  
* Als u de runbook on-premises worden uitgevoerd in een hybrid worker-groep, zorg ervoor dat wordt de module/cmdlet is geïnstalleerd op de computer die als host fungeert voor de hybride worker.

### <a name="evicted-from-checkpoint"></a>Scenario: Een langdurige runbook consistent is mislukt met de uitzondering: "de taak kan niet worden voortgezet omdat deze is herhaaldelijk onbeschikbaar gemaakt vanaf het controlepunt dezelfde"

#### <a name="issue"></a>Probleem

Dit gedrag is inherent vanwege de 'Evenredige verdeling' bewaking van processen in Azure Automation, waarmee automatisch een runbook wordt geblokkeerd als deze meer dan drie uur wordt uitgevoerd. Het foutbericht geretourneerd biedt echter geen in 'nu' opties.

#### <a name="cause"></a>Oorzaak

Een runbook kan worden onderbroken voor een aantal oorzaken hebben. Onderbreekt geval voornamelijk als gevolg van fouten. Bijvoorbeeld, een niet-onderschepte uitzondering in een runbook, een netwerkstoring of vastlopen van een op de Runbook-Worker waarop het runbook wordt uitgevoerd, alle ertoe leiden dat het runbook worden onderbroken en starten vanaf de laatste controlepunt wanneer hervat.

#### <a name="resolution"></a>Oplossing

De gedocumenteerde oplossing om te voorkomen dat dit probleem is met controlepunten in een werkstroom. Raadpleeg voor meer informatie, [Learning PowerShell-werkstromen](../automation-powershell-workflow.md#checkpoints). Een gedetailleerde informatie over het 'Evenredige verdeling' en de controlepunten vindt u in dit artikel blog [met behulp van controlepunten in Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Veelvoorkomende fouten bij het importeren van modules

### <a name="module-fails-to-import"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd na het importeren

#### <a name="issue"></a>Probleem

Een module niet kan worden geïmporteerd of met succes worden geïmporteerd, maar geen cmdlets worden opgehaald.

#### <a name="cause"></a>Oorzaak

Er zijn enkele veelvoorkomende redenen die een module kan niet met succes geïmporteerd naar Azure Automation:

* De structuur komt niet overeen met de structuur die het Automation nodig heeft om te worden.
* De module is afhankelijk van een andere module die niet is geïmplementeerd voor uw Automation-account.
* De module ontbreekt de bijbehorende afhankelijkheden in de map.
* De `New-AzureRmAutomationModule` cmdlet voor het uploaden van de module wordt gebruikt en u hebt niet het volledige opslagpad gegeven of de module met behulp van een openbaar toegankelijke URL niet geladen.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen oplossen het probleem:

* Zorg ervoor dat de module de volgende indeling heeft: ModuleName.Zip **->** ModuleName of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)
* Open het bestand .psd1 en controleert u of de module afhankelijkheden heeft. Als dit het geval is, uploadt u deze modules naar het Automation-account.
* Zorg ervoor dat eventuele waarnaar wordt verwezen dll's aanwezig zijn in de modulemap.

## <a name="next-steps"></a>Volgende stappen

Als u hebt uw probleem niet wordt weergegeven of niet uw probleem kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een incident voor ondersteuning van Azure kunt opslaan. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.