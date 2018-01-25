---
title: Het oplossen van veelvoorkomende problemen met Azure Automation | Microsoft Docs
description: Dit artikel bevat informatie over en oplossen van veelvoorkomende fouten van Azure Automation.
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
tags: top-support-issue
keywords: Fout bij het Automation, het oplossen van problemen probleem
ms.assetid: 5f3cfe61-70b0-4e9c-b892-d02daaeee07d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: sngun; v-reagie
ms.openlocfilehash: 95abba4fd293e2d04a58f0d07f955aca808434b7
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Het oplossen van veelvoorkomende problemen in Azure Automation 
Dit artikel bevat help het oplossen van veelvoorkomende fouten u tegenkomen in Azure Automation en mogelijke oplossingen om op te lossen ze voorgesteld.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatiefouten bij het werken met Azure Automation-runbooks
### <a name="scenario-sign-in-to-azure-account-failed"></a>Scenario: Meld u bij de Azure-Account is mislukt
**Fout:** het foutbericht "Unknown_user_type: onbekende gebruikerstype" bij het werken met de cmdlets Add-AzureAccount of Login-AzureRmAccount.

**Reden voor de volgende fout:** deze fout treedt op als de referentienaam asset niet geldig is of als de gebruikersnaam en het wachtwoord dat u gebruikt voor het instellen van het Automation-referentieasset niet geldig zijn.

**Tips voor probleemoplossing:** om te bepalen wat er mis is, voert u de volgende stappen uit:  

1. Zorg ervoor dat er geen speciale tekens, inclusief de  **@**  teken in de naam van Automation-referentie asset die u gebruikt voor het verbinding maken met Azure.  
2. Controleer of u kunt de gebruikersnaam en wachtwoord die zijn opgeslagen in de Azure Automation-referentie in uw lokale PowerShell ISE-editor. U kunt dit doen door het uitvoeren van de volgende cmdlets in de PowerShell ISE:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Als uw verificatie is mislukt lokaal, dit betekent dat u uw Azure Active Directory-referenties correct hebt ingesteld. Raadpleeg [verificatie met Azure met Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogbericht ophalen van de Azure Active Directory-account juist ingesteld.  

### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scenario: Kan het Azure-abonnement vinden
**Fout:** het foutbericht ' het abonnement met de naam ``<subscription name>`` is niet gevonden ' als u werkt met de Select-AzureSubscription of Select-AzureRmSubscription-cmdlets.

**Reden voor de volgende fout:** deze fout treedt op als de naam van het abonnement is niet geldig of als de Azure Active Directory-gebruiker die is bij het ophalen van de details van abonnement is niet geconfigureerd als een beheerder van het abonnement.

**Tips voor probleemoplossing:** om te bepalen als u naar Azure hebt geverifieerd en toegang tot het abonnement dat u probeert hebben te selecteren, voert u de volgende stappen uit:  

1. Zorg ervoor dat u uitvoert het **Add-AzureAccount** voordat u de **Select-AzureSubscription** cmdlet.  
2. Als u nog steeds deze foutmelding ziet, wijzigt u uw code door toe te voegen de **Get-AzureSubscription** cmdlet volgende de **Add-AzureAccount** cmdlet en vervolgens de code wordt uitgevoerd. Nu controleren of de uitvoer van Get-AzureSubscription de details van uw abonnement bevat.  

   * Als u niet alle details van abonnement in de uitvoer ziet, betekent dit dat het abonnement nog niet is geïnitialiseerd.  
   * Als u de details van abonnement in de uitvoer ziet, moet u ervoor zorgen dat u de juiste abonnementsnaam of ID met de **Select-AzureSubscription** cmdlet.   

### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scenario: Verificatie naar Azure is mislukt omdat de multi-factor authentication is ingeschakeld
**Fout:** het foutbericht ' Add-AzureAccount: AADSTS50079: sterke verificatie inschrijving (bewijs-up) is vereist ' verificatie bij Azure met uw Azure-gebruikersnaam en wachtwoord.

**Reden voor de volgende fout:** als u multi-factor authentication-server op uw Azure-account hebt, kunt u een Azure Active Directory-gebruiker niet gebruiken voor verificatie op Azure. In plaats daarvan moet u een certificaat of een service-principal gebruiken voor verificatie op Azure.

**Tips voor probleemoplossing:** voor het gebruik van een certificaat met de cmdlets van Azure classic deployment model, Raadpleeg [maken en het toevoegen van een certificaat voor het beheren van Azure-services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Raadpleeg voor het gebruik van een service-principal met Azure Resource Manager-cmdlets, [service principal met Azure portal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md) en [verifiëren van een service-principal met Azure Resource Manager.](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Veelvoorkomende fouten bij het werken met runbooks
### <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-it-failed-to-start-each-time"></a>Scenario: Het begin van de runbook-taak is drie keer geprobeerd, maar niet wordt gestart elke keer
**Fout:** uw runbook is mislukt met de fout "" de taak is drie keer geprobeerd maar deze is mislukt."

**Reden voor de volgende fout:** deze fout kan zijn veroorzaakt door de volgende redenen:  

1. Limiet voor geheugen. Er zijn voorgeschreven limiet op hoeveel geheugen toegewezen aan een Sandbox [Servicelimieten Automation](../azure-subscription-service-limits.md#automation-limits) zodat een taak deze mislukken kan als het gebruik van meer dan 400 MB aan geheugen. 

2. Incompatibele module. Dit kan gebeuren als de module-afhankelijkheden zijn niet juist is en als dat niet het geval is, uw runbook doorgaans retourneert een '-opdracht niet gevonden' of 'Kan niet binden voor parameter'-bericht. 

**Tips voor probleemoplossing:** een van de volgende oplossingen het probleem oplossen:

* Voorgestelde methoden voor het samenwerken om binnen de geheugenlimiet zijn Splits de werkbelasting tussen meerdere runbooks, niet zo veel mogelijk gegevens in het geheugen, om onnodige uitvoer schrijven van uw runbooks niet te verwerken of houd rekening met het aantal controlepunten u in PowerShell workflow-runbooks schrijven.  

* Bijwerken van uw Azure-modules door de stappen te volgen [het bijwerken van Azure PowerShell-modules in Azure Automation](automation-update-azure-modules.md).  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenario: Runbook is mislukt vanwege deserialiseerbaar object
**Fout:** uw runbook is mislukt met de fout ' kan niet binden parameter ``<ParameterName>``. Kan niet worden geconverteerd de ``<ParameterType>`` waarde van het type Deserialized ``<ParameterType>`` naar het type ``<ParameterType>``'.

**Reden voor de volgende fout:** als uw runbook een PowerShell Workflow is, wordt naar complexe objecten opgeslagen in een gedeserialiseerde indeling om uw runbook-status behouden als de werkstroom wordt onderbroken.

**Tips voor probleemoplossing:** een van de volgende drie oplossingen kunt dit probleem oplossen:

1. Als u complexe objecten van een cmdlet naar een andere zijn sluizen, moet u deze cmdlets verpakken in een InlineScript.
2. Geeft de naam of waarde die u nodig hebt van het complexe object in plaats van het gehele object doorgeven.
3. Een PowerShell-runbook gebruiken in plaats van een PowerShell Workflow-runbook.

### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scenario: De runbooktaak is mislukt omdat het toegewezen quotum overschreden
**Fout:** de runbooktaak is mislukt met de fout 'de quota voor de maandelijkse totale taakuitvoeringstijd is bereikt voor dit abonnement'.

**Reden voor de volgende fout:** deze fout treedt op wanneer de taak wordt uitgevoerd, de beschikbare quota 500 minuut voor uw account overschrijdt. Dit quotum geldt voor alle typen taken uitvoeren zoals het testen van een taak, start een taak vanuit de portal, een taak wordt uitgevoerd met behulp van webhooks en plannen van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Zie voor meer informatie over prijzen voor automatisering, [Automation prijzen](https://azure.microsoft.com/pricing/details/automation/).

**Tips voor probleemoplossing:** als u gebruiken van meer dan 500 minuten per maand wordt verwerkt wilt, moet u uw abonnement te wijzigen van de gratis laag aan de laag Basic. U kunt upgraden naar de basisstaffel door de volgende stappen:  

1. Meld u aan bij uw Azure-abonnement  
2. Selecteer het Automation-account dat u wilt upgraden  
3. Klik op **instellingen** > **prijzen**.
4. Klik op **inschakelen** pagina onder uw account wilt bijwerken op de **Basic** laag.

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenario: Cmdlet is niet herkend bij het uitvoeren van een runbook
**Fout:** de runbooktaak is mislukt met de fout '``<cmdlet name>``: de term ``<cmdlet name>`` niet wordt herkend als de naam van een cmdlet, functie, scriptbestand of programma. "

**Reden voor de volgende fout:** deze fout wordt veroorzaakt als de engine PowerShell de cmdlet die u in uw runbook gebruikt niet kan vinden. Dit kan zijn omdat de module met de cmdlet in het account ontbreekt, er een naamconflict met de runbooknaam van een is, of de cmdlet ook in een andere module bestaat en de naam kan niet worden omgezet in Automation.

**Tips voor probleemoplossing:** een van de volgende oplossingen het probleem oplossen:  

* Controleer of u de naam van de cmdlet juist hebt ingevoerd.  
* Controleer of de cmdlet bestaat in uw Automation-account en er zijn geen conflicten. Om te controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkingsmodus en zoek naar de cmdlet die u wilt zoeken in de bibliotheek of voer **Get-Command ``<CommandName>``** . Zodra u hebt gevalideerd die de cmdlet voor het account beschikbaar is en dat er geen naam conflicteert met andere cmdlets of runbooks, toe te voegen aan het canvas en zorg ervoor dat u van een geldige parameters ingesteld in uw runbook gebruikmaakt.  
* Als er een naamconflict van de cmdlet is beschikbaar in twee verschillende modules, kunt u deze kunt oplossen met behulp van de volledig gekwalificeerde naam van de cmdlet. U kunt bijvoorbeeld **ModuleName\CmdletName**.  
* Als u de runbook on-premises worden uitgevoerd in een hybrid worker-groep, zorg ervoor dat wordt de module/cmdlet is geïnstalleerd op de computer die als host fungeert voor de hybride worker.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scenario: Een langdurige runbook consistent is mislukt met de uitzondering: ' de taak kan niet worden voortgezet omdat deze is herhaaldelijk onbeschikbaar gemaakt vanaf het controlepunt dezelfde'.
**Reden voor de volgende fout:** dit gedrag is inherent vanwege de 'Evenredige verdeling' bewaking van processen in Azure Automation, waarmee automatisch een runbook wordt geblokkeerd als deze meer dan drie uur wordt uitgevoerd. Het foutbericht geretourneerd biedt echter geen in 'nu' opties. Een runbook kan worden onderbroken voor een aantal oorzaken hebben. Onderbreekt geval voornamelijk als gevolg van fouten. Bijvoorbeeld, een niet-onderschepte uitzondering in een runbook, een netwerkstoring of vastlopen van een op de Runbook-Worker waarop het runbook wordt uitgevoerd, alle ertoe leiden dat het runbook worden onderbroken en starten vanaf de laatste controlepunt wanneer hervat.

**Tips voor probleemoplossing:** de gedocumenteerde oplossing om te voorkomen dat dit probleem is met controlepunten in een werkstroom. Raadpleeg voor meer informatie, [Learning PowerShell-werkstromen](automation-powershell-workflow.md#checkpoints). Een gedetailleerde informatie over het 'Evenredige verdeling' en de controlepunten vindt u in dit artikel blog [met behulp van controlepunten in Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Veelvoorkomende fouten bij het importeren van modules
### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd na het importeren
**Fout:** een module niet kan worden geïmporteerd of met succes worden geïmporteerd, maar geen cmdlets worden opgehaald.

**Reden voor de volgende fout:** enkele veelvoorkomende redenen die een module kan niet met succes geïmporteerd naar Azure Automation zijn:

* De structuur komt niet overeen met de structuur die het Automation nodig heeft om te worden.
* De module is afhankelijk van een andere module die niet is geïmplementeerd voor uw Automation-account.
* De module ontbreekt de bijbehorende afhankelijkheden in de map.
* De **nieuw AzureRmAutomationModule** cmdlet voor het uploaden van de module wordt gebruikt en u hebt niet het volledige opslagpad gegeven of de module met behulp van een openbaar toegankelijke URL niet geladen.

**Tips voor probleemoplossing:** een van de volgende oplossingen het probleem oplossen:

* Zorg ervoor dat de module de volgende indeling heeft: ModuleName.Zip  **->**  ModuleName of het versienummer  **->**  (ModuleName.psm1, ModuleName.psd1)
* Open het bestand .psd1 en controleert u of de module afhankelijkheden heeft. Als dit het geval is, uploadt u deze modules naar het Automation-account.
* Zorg ervoor dat eventuele waarnaar wordt verwezen dll's aanwezig zijn in de modulemap.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Veelvoorkomende fouten bij het werken met Desired State Configuration (DSC)
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenario: Het knooppunt bevindt zich in de status mislukt met een 'Niet gevonden'-fout
**Fout:** het knooppunt heeft een rapport met **mislukt** status en met de fout ' de poging tot het ophalen van de actie van de server https://``<url>``//accounts/``<account-id>``Nodes(AgentId=``<agent-id>``) / GetDscAction is mislukt omdat een geldige configuratie ``<guid>`` kan niet worden gevonden. "

**Reden voor de volgende fout:** deze fout treedt meestal op wanneer het knooppunt is toegewezen aan een configuratienaam (bijvoorbeeld ABC) in plaats van een knooppunt configuratienaam (bijvoorbeeld ABC. WebServer).

**Tips voor probleemoplossing:**

* Zorg ervoor dat u het knooppunt met 'knooppunt configuratienaam' en niet de 'configuratienaam' toewijst.
* U kunt een knooppuntconfiguratie toewijzen aan een knooppunt met Azure portal of met een PowerShell-cmdlet.

  * Om de configuratie van een knooppunt toewijzen aan een knooppunt met Azure portal, opent u de **DSC-knooppunten** pagina, selecteer vervolgens een knooppunt en klik op **toewijzen knooppuntconfiguratie** knop.  
  * Gebruiken om de configuratie van een knooppunt aan een knooppunt met behulp van PowerShell-cmdlet toewijst, **Set AzureRmAutomationDscNode** cmdlet

### <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenario: Er is geen knooppuntconfiguraties (MOF-bestanden) zijn gemaakt tijdens het compileren van een configuratie
**Fout:** DSC-Compilatietaak wordt onderbroken met de fout: 'compilatie is voltooid, maar er is geen configuration.mofs knooppunt zijn gegenereerd'.

**Reden voor de volgende fout:** wanneer de volgende expressie de **knooppunt** sleutelwoord in de DSC-configuratie wordt geëvalueerd naar `$null`, en vervolgens geen knooppuntconfiguraties worden geproduceerd.

**Tips voor probleemoplossing:** een van de volgende oplossingen het probleem oplossen:

* Zorg ervoor dat de expressie naast de **knooppunt** -sleutelwoord in de definitie van de configuratie is niet naar $null evalueren.
* Als u ConfigurationData doorgeeft bij het compileren van de configuratie, zorgt u ervoor dat u de verwachte waarden die vereist dat de configuratie van doorgeeft [ConfigurationData](automation-dsc-compile.md#configurationdata).

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenario: Het rapport DSC-knooppunt wordt de status 'wordt uitgevoerd' vastgelopen
**Fout:** DSC Agent levert 'Er is geen exemplaar gevonden met de opgegeven eigenschapswaarden '.

**Reden voor de volgende fout:** u uw versie van WMF hebt bijgewerkt en WMI zijn beschadigd.

**Tips voor probleemoplossing:** om op te lossen de probleem Volg de instructies in de [DSC bekende problemen en beperkingen](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) artikel.

### <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a>Scenario: Een kan referenties niet gebruiken in een DSC-configuratie
**Fout:** DSC-Compilatietaak is onderbroken met de fout: "System.InvalidOperationException fout bij het verwerken van de eigenschap '-referentie van het type ``<some resource name>``: omzetten en opslaan van een versleutelde wachtwoord als leesbare tekst is alleen toegestaan als PSDscAllowPlainTextPassword is ingesteld op true '.

**Reden voor de volgende fout:** u een referentie in een configuratie hebt gebruikt maar geen juiste **ConfigurationData** instellen **PSDscAllowPlainTextPassword** op true voor de configuratie van elk knooppunt.

**Tips voor probleemoplossing:**

* Zorg ervoor dat u in de juiste doorgeven **ConfigurationData** instellen **PSDscAllowPlainTextPassword** op true voor de configuratie van elk knooppunt vermeld in de configuratie. Zie voor meer informatie [activa in Azure Automation DSC](automation-dsc-compile.md#assets).

## <a name="common-errors-when-onboarding-solutions"></a>Veelvoorkomende fouten bij het voorbereiden oplossingen

Wanneer onboarding-oplossingen die u mogelijk stuit fouten. Hier volgt een overzicht van de algemene fouten die mogelijk worden uitgevoerd op.

### <a name="computergroupqueryformaterror"></a>ComputerGroupQueryFormatError

**Reden voor de volgende fout:**

Deze foutcode betekent dat de opgeslagen zoekopdracht computer groepsquery gebruikt voor het doel van de oplossing is niet de juiste indeling. U hebt de query gewijzigd, of door het systeem is gewijzigd.

**Tips voor probleemoplossing:**

U kunt de query voor deze oplossing en de oplossing wordt de query reonboard verwijderen. De query kan worden gevonden in uw werkruimte onder **opgeslagen zoekopdrachten**. De naam van de query is **MicrosoftDefaultComputerGroup**, en de categorie van de query is de naam van de oplossing die is gekoppeld aan deze query. Als meerdere oplossingen zijn ingeschakeld, de **MicrosoftDefaultComputerGroup** ziet u meerdere keren onder **opgeslagen zoekacties**.

### <a name="policyviolation"></a>PolicyViolation

**Reden voor de volgende fout:**

Deze foutcode betekent dat de implementatie is mislukt vanwege een beleid schendt.

**Tips voor probleemoplossing:**

Controleer de meldingen in de rechterbovenhoek van de Azure portal of navigeer naar de resourcegroep waarin uw automation-account en selecteer **implementaties** onder **instellingen** om de mislukte weer te geven de implementatie. Voor meer informatie over Azure beleid bezoek: [overzicht van Azure-beleid](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json)

## <a name="next-steps"></a>Volgende stappen

Als u de voorgaande stappen hebt gevolgd en het antwoord niet kunt vinden, kunt u de aanvullende ondersteuning volgend opties bekijken:

* Help opvragen van Azure-experts. Geef uw probleem aan de [MSDN Azure of Stack Overflow-forums](https://azure.microsoft.com/support/forums/).
* Het bestand is een incident voor ondersteuning van Azure. Ga naar de [site ondersteuning van Azure](https://azure.microsoft.com/support/options/) en klik op **ondersteuning krijgen** onder **technische en ondersteuning voor facturering**.
* Post een bericht een aanvraag voor een Script op [Script Center](https://azure.microsoft.com/documentation/scripts/) als u een Azure Automation-runbookoplossing of een integratiemodule zoekt.
* Feedback geven of functieaanvragen voor Azure Automation post een bericht op [User Voice](https://feedback.azure.com/forums/34192--general-feedback).
