---
title: Veelgestelde vragen Azure DevTest Labs | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 08b27acc2d3114486b86aa0c311b9396c0bed48f
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297312"
---
# <a name="azure-devtest-labs-faq"></a>Veelgestelde vragen over Azure DevTest Labs
Vind antwoorden op enkele veelgestelde vragen over Azure DevTest Labs.

**Algemeen**
## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet wordt weergegeven, laat u ons weten, zodat we u een antwoord vinden kunnen helpen.

* Een vraag stellen aan het einde van deze Veelgestelde vragen. Neem contact op met de Azure Cache-team en andere communityleden over dit artikel.
* Als u wilt een breder publiek bereiken, kunt u een vraag plaatst op de [Azure DevTest Labs MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Neem contact op met de Azure DevTest Labs-team en andere leden van de community.
* Voor functieaanvragen, het verzenden van aanvragen en ideeën te [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Waarom zou ik Azure DevTest Labs gebruiken?
Azure DevTest Labs kunt uw team tijd en geld besparen. Ontwikkelaars kunnen hun eigen omgevingen maken met behulp van meerdere verschillende databases. Ook kunnen ze artefacten gebruiken om snel te implementeren en configureren van toepassingen. U kunt met behulp van aangepaste installatiekopieën en formules, virtuele machines (VM's) opslaan als sjabloon en ze eenvoudig met het team te reproduceren. DevTest Labs biedt ook diverse configureerbare beleidsregels die beheerders gebruiken kunnen voor verminderen verspilling en beheren van een team omgevingen lab. Deze beleidsregels bevatten automatisch afsluiten, drempel voor kosten, maximum aantal virtuele machines per gebruiker en de maximale grootte van de virtuele machine. Zie voor een uitgebreidere uitleg van DevTest Labs, de [overzicht](devtest-lab-overview.md) of de [inleidende video](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Wat betekent 'zorgeloze selfservice'?
Zorgeloze selfservice betekent dat ontwikkelaars en testers hun eigen omgevingen maken indien nodig. Beheerders hebben de beveiliging van de wetenschap dat DevTest Labs kunt verspilling en beheer kosten kunt minimaliseren. Beheerders kunnen opgeven welke VM-grootten zijn toegestaan, het maximum aantal virtuele machines, en wanneer virtuele machines worden gestart en afsluiten. DevTest Labs kunt u gemakkelijk om te controleren van de kosten en instellen van waarschuwingen, kunt u blijven op de hoogte van het labresources die worden gebruikt.

## <a name="how-can-i-use-devtest-labs"></a>Hoe kan ik DevTest Labs gebruiken?
DevTest Labs is handig telkens wanneer u dev vereisen of testomgevingen, en wilt ze snel te reproduceren, of ze beheren door middel van geld te besparen.

Hier volgen enkele scenario's die gebruikmaken van onze klanten DevTest Labs voor:

* Beheren van de ontwikkel- en testomgevingen op één plek. Gebruik beleid voor kosten te verlagen en maken van aangepaste installatiekopieën voor het delen van builds met het team.
* Een toepassing ontwikkelen met behulp van aangepaste installatiekopieën op te slaan de status van de schijf tijdens de fasen van de ontwikkeling.
* Kosten ten opzichte van de voortgang bijhouden.
* Maak massaal en testomgevingen voor het testen van kwaliteit assurance.
* Gebruik artefacten en formules om eenvoudig te reproduceren van een toepassing in verschillende omgevingen.
* Virtuele machines voor hackathons (samenwerkingsverband ontwikkelen of testen werk) distribueren en eenvoudig inrichting wanneer de gebeurtenis is beëindigd.

## <a name="how-am-i-billed-for-devtest-labs"></a>Hoe Word ik gefactureerd voor DevTest Labs?
DevTest Labs is een gratis service. Labs maken en configureren van beleid, sjablonen en artefacten in DevTest Labs is gratis. U betaalt alleen voor de Azure-resources die in uw labs, zoals virtuele machines, opslagaccounts en virtuele netwerken worden gebruikt. Zie voor meer informatie over de kosten van de labresources [prijzen voor Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Beveiliging**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Wat zijn de verschillende beveiligingsniveaus in DevTest Labs?
Toegang wordt bepaald door [Role-Based Access Control (RBAC)](../role-based-access-control/built-in-roles.md). Meer informatie over de werking van toegang, kunt zo u informatie over de verschillen tussen een machtiging, een rol en een bereik, zoals gedefinieerd door RBAC.

* **Machtiging**: een machtiging is een gedefinieerde toegang tot een specifieke actie. Een machtiging kan bijvoorbeeld leestoegang tot alle virtuele machines.
* **Rol**: een rol is een reeks machtigingen die kunnen worden gegroepeerd en toegewezen aan een gebruiker. Bijvoorbeeld, heeft een gebruiker met een abonnement de eigenaarsrol toegang tot alle resources binnen een abonnement.
* **Bereik**: een scope is een niveau binnen de hiërarchie van een Azure-resource. Een bereik kan bijvoorbeeld een resourcegroep, een enkel lab of het hele abonnement.

Binnen het bereik van DevTest Labs zijn er twee soorten rollen die de gebruikersmachtigingen definiëren:

* **Lab-eigenaar**: lab-eigenaar heeft toegang tot alle resources in het lab. Een lab-eigenaar kan beleid wijzigen, lezen en schrijven naar virtuele machines, wijzigen van het virtuele netwerk, enzovoort.
* **Lab-gebruiker**: een lab-gebruiker kan zien alle labresources, zoals virtuele machines, beleidsregels en virtuele netwerken. Maar de gebruiker van een lab-beleid of virtuele machines die zijn gemaakt door andere gebruikers niet wijzigen. 

U kunt ook aangepaste rollen maken in DevTest Labs. Zie voor meer informatie over het maken van aangepaste rollen in DevTest Labs, [gebruikersmachtigingen verlenen voor specifieke lab beleid](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Omdat bereiken hiërarchische, zijn wanneer een gebruiker machtigingen met een bepaald bereik heeft, wordt deze machtigingen aan elk bereik op lagere niveaus in het bereik automatisch verleend door de gebruiker. Bijvoorbeeld, als een gebruiker de rol van de eigenaar van het abonnement is toegewezen, heeft de gebruiker toegang tot alle resources in een abonnement. Deze bronnen omvatten alle virtuele machines, alle virtuele netwerken en alle labs. De eigenaar van een abonnement neemt automatisch de rol van de eigenaar van het lab. Het tegenovergestelde is echter niet het geval. Een lab-eigenaar heeft toegang tot een lab een lagere omvang dan het abonnementsniveau is. Een lab-eigenaar zien niet daarom virtuele machines, virtuele netwerken of andere bronnen die zich buiten het lab.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hoe maak ik een functie waarmee gebruikers een specifieke taak uit te voeren?
Zie voor een uitgebreide artikel over het maken van aangepaste rollen en machtigingen toewijzen aan een rol, [gebruikersmachtigingen verlenen voor specifieke lab beleid](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Hier volgt een voorbeeld van een script dat wordt gemaakt van de rol *DevTest Labs-geavanceerde gebruiker*, die is gemachtigd om te starten en stoppen van alle virtuele machines in de testomgeving:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**CI/CD-integratie en automatisering**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>DevTest Labs integreren in mijn CI/CD-toolchain?
Als u van Azure DevOps gebruikmaakt, kunt u een [DevTest Labs-taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) voor het automatiseren van uw release-pijplijn in DevTest Labs. Enkele van de taken die u met deze extensie doen kunt zijn onder andere:

* Maak en implementeer een virtuele machine automatisch. U kunt ook de virtuele machine met de meest recente build configureren met behulp van Azure bestand kopiëren of PowerShell Azure DevOps-Services-taken.
* De status van een virtuele machine automatisch vastleggen na het testen om te reproduceren een bug in dezelfde virtuele machine voor meer informatie.
* De virtuele machine aan het einde van de release-pijplijn verwijderen wanneer deze niet meer nodig is.

De volgende blogberichten bieden richtlijnen en informatie over het gebruik van de Services van Azure DevOps-extensie:

* [DevTest Labs en de Azure DevOps-extensie](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Een nieuwe virtuele machine in een bestaande DevTest Labs-testomgeving van Azure DevOps-Services implementeren](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Met behulp van release management voor Azure DevOps-Services voor continue implementatie in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Voor andere continue integratie (CI) / toolchains continue levering (CD), kunt u dezelfde scenario's door bereiken implementeren [Azure Resource Manager-sjablonen](https://aka.ms/dtlquickstarttemplate) met behulp van [Azure PowerShell-cmdlets](../azure-resource-manager/resource-group-template-deploy.md) en [.NET SDK's](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). U kunt ook gebruiken [REST API's voor DevTest Labs](http://aka.ms/dtlrestapis) om te integreren met uw hulpprogrammaketen.  


**Virtuele machines**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-blade-that-i-see-in-devtest-labs"></a>Waarom zie ik virtuele machines niet op de blade virtuele Machines die ik in DevTest Labs te zien?
Wanneer u een virtuele machine in DevTest Labs maken, krijgt u machtiging voor toegang tot deze VM. Vindt u de virtuele machine zowel op de blade labs en klik op de **virtuele Machines** blade. Gebruikers die zijn toegewezen aan de gebruikersrol van DevTest Labs lab ziet alle virtuele machines die zijn gemaakt in de testomgeving op van het lab **alle virtuele Machines** blade. Gebruikers met de gebruikersrol van de DevTest Labs-lab worden leestoegang tot de VM-resources die andere gebruikers hebben gemaakt, automatisch niet verleend. Deze virtuele machines worden daarom niet weergegeven op de **virtuele Machines** blade.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Wat is het verschil tussen een aangepaste installatiekopie en een formule?
Een aangepaste installatiekopie is een virtuele harde schijf (VHD). Een formule is een installatiekopie die u kunt configureren met extra instellingen en klik vervolgens opslaan en reproduceren. Een aangepaste installatiekopie is mogelijk beter als u snel verschillende omgevingen maken wilt met behulp van dezelfde basis-, onveranderbare installatiekopie. Een formule is mogelijk beter als u wilt de configuratie van uw virtuele machine met de meest recente materialen reproduceren als onderdeel van een virtueel netwerk of subnet, of als een virtuele machine met een bepaalde grootte. Zie voor een uitgebreidere uitleg [vergelijking van aangepaste installatiekopieën en formules in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hoe maak ik meerdere virtuele machines met dezelfde sjabloon in één keer?
U hebt twee opties voor het maken van meerdere virtuele machines tegelijkertijd uit dezelfde sjabloon:
* U kunt de [Azure DevOps-taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks). 
* U kunt [genereren van een Resource Manager-sjabloon](devtest-lab-add-vm.md#save-azure-resource-manager-template) tijdens het maken van een virtuele machine, en [implementeren van de Resource Manager-sjabloon vanuit Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Hoe verplaats ik mijn bestaande Azure-VM's in mijn lab DevTest Labs?
Uw bestaande virtuele machines naar DevTest Labs kopiëren:

1. Kopieer de VHD-bestand van uw bestaande virtuele machine met behulp van een [Windows PowerShell-script](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1).
2. [De aangepaste installatiekopie maken](devtest-lab-create-template.md) binnen uw lab in DevTest Labs.
3. Een virtuele machine in een testomgeving door uw aangepaste installatiekopie maken.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kan ik meerdere schijven koppelen aan mijn virtuele machines?
Ja, kunt u meerdere schijven koppelen aan uw virtuele machines.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Als ik wil een installatiekopie van het Windows-besturingssysteem voor mijn testdoeleinden gebruiken, heb ik een MSDN-abonnement te kopen?
Voor het gebruik van Windows clientinstallatiekopieën van het besturingssysteem (Windows 7 of hoger) voor de ontwikkeling en testen in Azure, moet u het volgende doen:

- [MSDN-abonnement aanschaffen](https://www.visualstudio.com/products/how-to-buy-vs).
- Als u een Enterprise Agreement hebt, maakt u een Azure-abonnement met de [Enterprise Dev/Test-aanbieding](https://azure.microsoft.com/offers/ms-azr-0148p).

Zie voor meer informatie over de Azure-tegoed voor elke aanbieding MSDN [maandelijkse Azure-tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hoe Automatiseer ik het proces van het VHD-bestanden voor het maken van aangepaste installatiekopieën uploaden?
Als u wilt uploaden VHD-bestanden voor het maken van aangepaste installatiekopieën automatiseren, hebt u twee opties:

* Gebruik [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) te kopiëren of VHD-bestanden uploaden naar het opslagaccount dat is gekoppeld aan het lab.
* Gebruik [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md). Storage Explorer is een zelfstandige app die wordt uitgevoerd op Windows, OS X en Linux.   

Zoek de doel-opslagaccount dat is gekoppeld aan uw testomgeving:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer in het menu links **resourcegroepen**.
3. Zoek en selecteer de resourcegroep die is gekoppeld aan uw testomgeving.
4. Onder **overzicht**, selecteer een van de storage-accounts.
5. Selecteer **Blobs**.
6. Uiterlijk voor het uploaden van bestanden in de lijst. Als u nog geen bestaat, gaat u terug naar stap 4 en probeer een ander opslagaccount.
7. Gebruik de **URL** als het doel in de AzCopy-opdracht.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hoe Automatiseer ik het proces van het verwijderen van alle VM's in mijn lab?
U kunt virtuele machines verwijderen uit uw lab in Azure portal. U kunt ook alle virtuele machines in uw testomgeving verwijderen met behulp van een PowerShell-script. In het volgende voorbeeld, onder de **waarden wijzigen** opmerking, wijzigt u de parameterwaarden. U vindt de `subscriptionId`, `labResourceGroup`, en `labName` waarden in het deelvenster lab in Azure portal.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Connect-AzureRmAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefacten**
## <a name="what-are-artifacts"></a>Wat zijn de artefacten?
Artefacten zijn aanpasbare elementen die u gebruiken kunt om te implementeren van uw laatste gegevens of onze ontwikkelprogramma's implementeren op een virtuele machine. Artefacten toevoegen aan uw virtuele machine bij het maken van de virtuele machine. Nadat de virtuele machine is ingericht, wordt de artefacten implementeren en configureren van uw virtuele machine. Diverse bestaande artefacten zijn beschikbaar in onze [openbare GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). U kunt ook [ontwerpen van uw eigen artefacten](devtest-lab-artifact-author.md).


**Configuratie van het testlab**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Hoe kan ik een lab maken van een Resource Manager-sjabloon?
We bieden een [GitHub-opslagplaats van Azure Resource Manager-sjablonen voor lab](https://aka.ms/dtlquickstarttemplate) die u kunt implementeren als- of aanpassen voor het maken van aangepaste sjablonen voor uw labs. Elke sjabloon is een koppeling naar het lab als implementeren-is in uw eigen Azure-abonnement. Of u kunt de sjabloon aanpassen en [implementeren met behulp van PowerShell of Azure CLI](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Waarom wordt mijn virtuele machines in verschillende resourcegroepen bevinden, met willekeurige namen gemaakt? Kan ik wijzigen of aanpassen van deze resourcegroepen?
Resourcegroepen zijn op deze manier gemaakt zodat DevTest Labs gebruikersmachtigingen en toegang tot virtuele machines kunt beheren. Hoewel u kunt een virtuele machine naar een andere resourcegroep verplaatsen en de naam die u wilt gebruiken, raden wij aan dat geen u wijzigingen in de resourcegroepen aanbrengt. Er wordt gewerkt aan het verbeteren van deze ervaring om toe te staan van meer flexibiliteit.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hoeveel labs kan ik onder hetzelfde abonnement maken?
Er is niet een bepaalde limiet voor het aantal labs dat per abonnement kan worden gemaakt. De hoeveelheid resources die worden gebruikt per abonnement is echter beperkt. U kunt meer informatie over de [limieten en quota's voor Azure-abonnementen](../azure-subscription-service-limits.md) en [deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Het aantal VM's kan ik maken per lab?
Er is geen specifieke limiet voor het aantal virtuele machines dat kan worden gemaakt per lab. De resources (VM-kernen, openbare IP-adressen, enzovoort) die worden gebruikt, zijn echter beperkt per abonnement. U kunt meer informatie over de [limieten en quota's voor Azure-abonnementen](../azure-subscription-service-limits.md) en [deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hoe kan ik een directe koppeling naar Mijn lab delen

1. In de Azure-portal, gaat u naar het lab.
2. Kopieer de URL lab in uw browser en deze vervolgens delen met uw labgebruikers.

> [!NOTE]
> Als een lab-gebruiker is een externe gebruiker die heeft een [Microsoft-account](#what-is-a-microsoft-account), maar die niet lid zijn van Active Directory-exemplaar van uw organisatie is, de gebruiker ziet een foutbericht wanneer ze proberen toegang tot de gedeelde koppeling. Als een externe gebruiker een foutbericht weergegeven ziet, vraagt u de gebruiker selecteert u eerst de naam in de rechterbovenhoek van de Azure-portal. Klik in de **Directory** gedeelte van het menu, de gebruiker kunt selecteren de map waar het lab bestaat.
>
>

## <a name="what-is-a-microsoft-account"></a>Wat is een Microsoft-account?
Een Microsoft-account is een account dat u gebruikt voor bijna alles wat die u met Microsoft-apparaten en services doet. Het is een e-mailadres en wachtwoord op waarmee u zich aanmelden bij Skype, Outlook.com, OneDrive, Windows phone en Xbox Live. Één account betekent dat uw bestanden, foto's, contactpersonen en instellingen u op elk apparaat volgen kunnen.

> [!NOTE]
> Een Microsoft-account gebruikt om te worden aangeroepen een *Windows Live ID*.
>
>


**Problemen oplossen**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Mijn gegevensartefact is mislukt tijdens het maken van virtuele machine. Hoe kan ik deze oplossen?
Zie voor meer informatie over het verkrijgen van Logboeken voor de mislukte artefact, [hoe u met artefacten vaststellen in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Waarom is niet mijn bestaande virtuele netwerk opslaan correct?
Een mogelijkheid is dat de naam van uw virtuele netwerk punten bevat. Als dit het geval is, probeert de perioden verwijderen of deze overal vervangen door afbreekstreepjes bevatten. Probeer vervolgens opnieuw op te slaan van het virtuele netwerk.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Waarom krijg ik een foutbericht "Bovenliggende resource is niet gevonden" wanneer ik een virtuele machine vanuit PowerShell inrichten?
Wanneer een resource een bovenliggend item naar een andere resource is, moet de bovenliggende resource aanwezig zijn voordat u de onderliggende resource maakt. Als de bovenliggende resource niet bestaat, ziet u een **ParentResourceNotFound** bericht. Als u een afhankelijkheid op de bovenliggende resource niet opgeeft, kan de onderliggende bron worden geïmplementeerd voordat het bovenliggende item.

Virtuele machines zijn onderliggende resources onder een lab in een resourcegroep. Wanneer u virtuele machines implementeren met behulp van PowerShell met Resource Manager-sjablonen, moet de naam van de resourcegroep opgegeven in het PowerShell-script de naam van de resourcegroep van het testlab. Zie voor meer informatie, [algemene Azure-implementatie oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Waar vind ik meer foutinformatie uitval van een VM-implementatie?
Fouten bij de implementatie van de virtuele machine worden vastgelegd in de activiteitenlogboeken. U vindt lab VM activiteitenlogboeken onder **auditlogboeken** of **diagnostische gegevens van virtuele machine** in het resourcemenu op de VM-blade van de testomgeving (de blade wordt weergegeven nadat u de virtuele machine van de **mijn virtuele machines** lijst).

Soms treedt de implementatiefout op voordat de VM-implementatie begint. Een voorbeeld daarvan is wanneer de abonnementslimiet voor een resource die is gemaakt met de virtuele machine wordt overschreden. In dit geval worden de foutgegevens vastgelegd in de activiteitenlogboeken lab-niveau. Activiteitenlogboeken zijn in de rechterbenedenhoek van de **configuratie en het beleid** instellingen. Zie voor meer informatie over het gebruik van activiteit-in Azure logboeken [activiteitenlogboeken om te controleren van acties op resources bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
