---
title: Azure DevTest Labs Veelgestelde vragen | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Azure DevTest Labs vinden.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: v-craic
ms.openlocfilehash: 24a3220a21280684a34405ac4c3d9f9eab9e3683
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-devtest-labs-faq"></a>Veelgestelde vragen over Azure DevTest Labs
Vind antwoorden op enkele veelgestelde vragen over Azure DevTest Labs.

**Algemeen**
## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet wordt weergegeven, kunt u ons weten, dus je kunt een antwoord vinden.

* Post een vraag aan het einde van deze Veelgestelde vragen. Bezighouden met het team van Azure-Cache en andere communityleden over dit artikel.
* Als u wilt een breder publiek bereiken, kunt u een vraag posten op de [Azure DevTest Labs van MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Bezighouden met het team van Azure DevTest Labs en andere leden van de community.
* Voor functieaanvragen, het verzenden van uw aanvragen en ideeën aan [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Waarom moet ik Azure DevTest Labs gebruiken?
Azure DevTest Labs uw team tijd en geld bespaart. Ontwikkelaars kunnen hun eigen omgeving maken met behulp van verschillende andere databases. Ze kunnen ook artefacten gebruiken om snel te implementeren en configureren van toepassingen. U kunt met behulp van aangepaste installatiekopieën en formules, virtuele machines (VM's) opslaan als sjabloon en eenvoudig te reproduceren in het team. DevTest Labs biedt ook verschillende beleidsregels voor configureerbare die beheerders gebruiken kunnen om te verkleinen verspilling en beheren van een team omgevingen lab. Deze beleidsregels bevatten automatisch afsluiten, kosten drempelwaarde, maximum aantal virtuele machines per gebruiker en de maximale grootte van de virtuele machine. Zie voor een uitgebreidere uitleg van DevTest Labs de [overzicht](devtest-lab-overview.md) of de [Introductievideo](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Wat houdt 'probleemloos Self-service'?
Self-service probleemloos betekent dat ontwikkelaars en testers uw eigen omgeving maken indien nodig. Beheerders hebben de beveiliging van weten dat DevTest Labs kunt verspilling en beheer kosten kunt minimaliseren. Beheerders kunnen opgeven welke VM-grootten zijn toegestaan, wordt het maximum aantal virtuele machines, en wanneer virtuele machines worden gestart en afgesloten. DevTest Labs ook eenvoudig om te controleren van de kosten en instellen van waarschuwingen, kunt u de hoogte blijven van de hoe lab bronnen worden gebruikt.

## <a name="how-can-i-use-devtest-labs"></a>Hoe kan ik DevTest Labs gebruiken?
DevTest Labs is nuttig elk gewenst moment u dev vereisen of testomgeving, en wilt ze snel reproduceren, of ze beheren met behulp van kostenbesparende beleid.

Hier volgen enkele scenario's die gebruikmaken van onze klanten DevTest Labs voor:

* Dev beheren en omgevingen op één plek te testen. Gebruik beleid voor kosten te verlagen en maken van aangepaste installatiekopieën te delen in het team is gebaseerd.
* Een toepassing ontwikkelen met behulp van aangepaste installatiekopieën op te slaan de status van de schijf in de ontwikkeling fasen.
* Kosten ten opzichte van de voortgang bijhouden.
* Maak massaopslag testomgevingen voor kwaliteit zekerheid te testen.
* Gebruik artefacten en formules om eenvoudig te configureren en een toepassing in verschillende omgevingen reproduceren.
* Virtuele machines voor hackathons (ontwikkeling of tests samenwerkingsmogelijkheden) distribueren en vervolgens gemakkelijk inrichting ervan ongedaan maakt ze wanneer de gebeurtenis wordt beëindigd.

## <a name="how-am-i-billed-for-devtest-labs"></a>Hoe ben ik DevTest Labs gefactureerd?
DevTest Labs is een gratis service. Labs maken en configureren van beleid, sjablonen en artefacten in DevTest Labs is gratis. U betaalt alleen voor de Azure-resources in uw labs, zoals virtuele machines, opslagaccounts en virtuele netwerken gebruikt. Zie voor meer informatie over de kosten van lab resources [prijzen van Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Beveiliging**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Wat zijn de verschillende beveiligingsniveaus in DevTest Labs?
Beveiligingstoegang wordt bepaald door [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-built-in-roles.md). Voor meer informatie over de werking van toegang, is het nuttig om informatie over de verschillen tussen een machtiging, een rol en een bereik, zoals gedefinieerd door RBAC.

* **Machtiging**: een machtiging is een gedefinieerde toegang tot een specifieke actie. Een machtiging kan bijvoorbeeld leestoegang tot alle virtuele machines.
* **Rol**: een rol is een reeks machtigingen die kunnen worden gegroepeerd en toegewezen aan een gebruiker. Bijvoorbeeld, heeft een gebruiker met de rol van een abonnement eigenaar toegang tot alle bronnen binnen een abonnement.
* **Bereik**: een scope is een niveau binnen de hiërarchie van een Azure-resource. Een scope kan bijvoorbeeld een resourcegroep, een lab dat één of het hele abonnement.

Er zijn twee typen van functies die de gebruikersmachtigingen definiëren binnen het bereik van DevTest Labs:

* **Labeigenaar**: een labeigenaar heeft toegang tot alle bronnen in de testomgeving. De eigenaar van een testomgeving kunt beleid wijzigen, lezen en schrijven naar alle virtuele machines, het virtuele netwerk wijzigen, enzovoort.
* **Lab-gebruiker**: een lab-gebruiker alle lab-resources, zoals virtuele machines, beleid en virtuele netwerken kunt weergeven. Maar een lab-gebruiker kan geen beleidsregels of alle virtuele machines die zijn gemaakt door andere gebruikers te wijzigen. 

U kunt ook aangepaste rollen maken in DevTest Labs. Zie voor meer informatie over aangepaste rollen maken in DevTest Labs, [gebruikersmachtigingen toewijzen aan specifieke labbeleidsregels](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Omdat scopes hiërarchische, wanneer een gebruiker machtigingen op een bepaalde scope heeft, krijgt de gebruiker automatisch deze machtigingen voor elke scope lager niveau in het bereik. Bijvoorbeeld, als een gebruiker de rol van de eigenaar van het abonnement is toegewezen, heeft de gebruiker toegang tot alle bronnen in een abonnement. Deze bronnen omvatten alle virtuele machines, alle virtuele netwerken en alle labs. De eigenaar van een abonnement neemt automatisch de rol van de labeigenaar van het. Het tegenovergestelde is echter niet het geval. Een labeigenaar heeft toegang tot een lab een bereik lager is dan het abonnementsniveau is. Daarom zien niet de labeigenaar van een van virtuele machines, virtuele netwerken of andere bronnen die zich buiten het lab.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hoe maak ik een functie waarmee gebruikers een specifieke taak uitvoeren?
Zie voor een uitgebreide artikel over het maken van aangepaste rollen en machtigingen toewijzen aan een rol [gebruikersmachtigingen toewijzen aan specifieke labbeleidsregels](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Hier volgt een voorbeeld van een script dat wordt gemaakt van de rol *DevTest Labs geavanceerde gebruiker*, die gemachtigd is om te starten en stoppen alle virtuele machines in de testomgeving:

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
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>Wordt DevTest Labs geïntegreerd met mijn toolchain CI/CD?
Als u Visual Studio Team Services gebruikt, kunt u een [DevTest Labs taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) uw pijplijn release in DevTest Labs automatiseren. Enkele van de taken die u met deze extensie kunt doen:

* Maken en implementeren van een virtuele machine automatisch. U kunt ook de virtuele machine met de laatste build configureren met behulp van Azure bestand kopiëren of PowerShell Team Services-taken.
* Automatisch de status van een virtuele machine vastleggen na het testen om te reproduceren een bug op dezelfde virtuele machine voor verder onderzoek.
* Verwijder de virtuele machine aan het einde van de pijplijn release wanneer deze niet langer nodig is.

De volgende blogberichten bieden richtlijnen en informatie over het gebruik van de extensie Team Services:

* [DevTest Labs en de uitbreiding van de Visual Studio Team Services](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Een nieuwe virtuele machine in een bestaande DevTest Labs testomgeving van Team Services implementeren](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Met behulp van het releasebeheer Team Services voor continue implementatie in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Voor andere continue integratie (CI) / toolchains continue levering (CD), kunt u de dezelfde scenario's door bereiken implementeren [Azure Resource Manager-sjablonen](https://aka.ms/dtlquickstarttemplate) met behulp van [Azure PowerShell-cmdlets](../azure-resource-manager/resource-group-template-deploy.md) en [.NET SDK's](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). U kunt ook gebruiken [REST-API's voor DevTest Labs](http://aka.ms/dtlrestapis) integreren met uw toolchain.  


**Virtuele machines**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-blade-that-i-see-in-devtest-labs"></a>Waarom zie ik virtuele machines kan niet op de blade virtuele Machines die ik Zie in DevTest Labs?
Wanneer u een virtuele machine in DevTest Labs maken, krijgt u machtiging voor toegang tot die VM. Vindt u de virtuele machine op de blade labs zowel op de **virtuele Machines** blade. Gebruikers die zijn toegewezen aan de gebruikersrol van DevTest Labs lab ziet alle virtuele machines die zijn gemaakt in de testomgeving op het lab **alle virtuele Machines** blade. Gebruikers die beschikken over de gebruikersrol van DevTest Labs lab worden leestoegang voor VM-resources die u hebt gemaakt met andere gebruikers echter automatisch niet verleend. Deze virtuele machines worden daarom niet weergegeven op de **virtuele Machines** blade.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Wat is het verschil tussen een aangepaste installatiekopie en een formule?
Een aangepaste installatiekopie is een virtuele harde schijf (VHD). Een formule is een installatiekopie die u kunt met extra instellingen configureren en vervolgens opslaan en reproduceren. Een aangepaste installatiekopie is mogelijk beter als u snel verschillende omgevingen te maken wilt met behulp van de dezelfde installatiekopie van een eenvoudige, niet-wijzigbaar. Een formule is mogelijk beter als u wilt dat de configuratie van uw virtuele machine met de meest recente bits reproduceren als onderdeel van een virtueel netwerk of subnet of als een virtuele machine met een bepaalde grootte. Zie voor een uitgebreidere uitleg [vergelijken van aangepaste installatiekopieën en formules in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hoe maak ik meerdere virtuele machines met dezelfde sjabloon tegelijk?
U hebt twee opties voor het maken van tegelijkertijd meerdere virtuele machines met dezelfde sjabloon:
* U kunt de [Visual Studio Team Services-taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks). 
* U kunt [genereren van een Resource Manager-sjabloon](devtest-lab-add-vm.md#save-azure-resource-manager-template) tijdens het maken van een virtuele machine, en [implementeren van de Resource Manager-sjabloon uit de Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Hoe kan ik mijn bestaande Azure VM's in mijn lab DevTest Labs verplaatsen?
Uw bestaande virtuele machines naar DevTest Labs kopiëren:

1. Kopieer de VHD-bestand van uw bestaande virtuele machine met behulp van een [Windows PowerShell-script](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1).
2. [De aangepaste installatiekopie maken](devtest-lab-create-template.md) in uw testomgeving DevTest Labs.
3. Een virtuele machine in de testomgeving van uw aangepaste installatiekopie maken.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kan ik meerdere schijven koppelen aan mijn VM's?
Ja, kunt u meerdere schijven toevoegen aan uw virtuele machines.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Als ik wil een installatiekopie van het Windows-besturingssysteem voor mijn testdoeleinden gebruiken, heb ik een MSDN-abonnement aanschaffen?
Voor het gebruik van Windows clientinstallatiekopieën van het besturingssysteem (Windows 7 of hoger) voor de ontwikkeling of tests in Azure, moet u het volgende doen:

- [Een MSDN-abonnement aanschaffen](https://www.visualstudio.com/products/how-to-buy-vs).
- Als u een Enterprise Agreement hebt, maakt u een Azure-abonnement met de [Enterprise ontwikkelen en testen aanbieding](https://azure.microsoft.com/en-us/offers/ms-azr-0148p).

Zie voor meer informatie over de Azure-krediet voor elke aanbieding MSDN [maandelijkse Azure-tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hoe ik het proces van het VHD-bestanden voor het maken van aangepaste installatiekopieën uploaden automatiseren?
Als u wilt uploaden VHD-bestanden voor het maken van aangepaste installatiekopieën automatiseren, hebt u twee opties:

* Gebruik [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) kopiëren of VHD-bestanden uploaden naar het opslagaccount dat is gekoppeld aan het lab.
* Gebruik [Azure Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md). Opslagverkenner is een zelfstandige app die wordt uitgevoerd op Windows-, OS X- en Linux.   

Zoek het doelopslagaccount die is gekoppeld aan uw testomgeving:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer op het menu links **resourcegroepen**.
3. Zoek en selecteer de resourcegroep die is gekoppeld aan uw testomgeving.
4. Onder **overzicht**, selecteer een van de storage-accounts.
5. Selecteer **Blobs**.
6. Zoek naar uploads in de lijst. Als er nog geen bestaat, terug naar stap 4 en probeer een ander opslagaccount.
7. Gebruik de **URL** als het doel in uw AzCopy-opdracht.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hoe ik het proces van het verwijderen van alle VM's in mijn lab automatiseren?
U kunt virtuele machines verwijderen uit uw lab in de Azure portal. U kunt ook de virtuele machines in uw testomgeving verwijderen met behulp van een PowerShell-script. In het volgende voorbeeld onder de **waarden wijzigen** opmerkingen, wijzigt u de parameterwaarden. U kunt ophalen de `subscriptionId`, `labResourceGroup`, en `labName` waarden in het deelvenster lab in de Azure portal.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Login-AzureRmAccount

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
## <a name="what-are-artifacts"></a>Wat zijn artefacten?
Artefacten zijn aanpasbare elementen die u gebruiken kunt voor het implementeren van de meest recente bits of implementeren van de dev-hulpprogramma's voor een virtuele machine. Koppelen aan uw virtuele machine artefacten bij het maken van de virtuele machine. Nadat de virtuele machine is ingericht, wordt de artefacten implementeren en configureren van uw virtuele machine. Verschillende vooraf bestaande artefacten zijn beschikbaar in onze [openbare GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). U kunt ook [ontwerpen van uw eigen artefacten](devtest-lab-artifact-author.md).


**Configuratie van het testlab**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Hoe maak ik een lab van Resource Manager-sjabloon
Wij bieden een [GitHub-opslagplaats met Azure Resource Manager-sjablonen lab](https://aka.ms/dtlquickstarttemplate) die u kunt implementeren als-is of als u wilt maken van aangepaste sjablonen voor uw labs wijzigen. Elke sjabloon heeft een koppeling voor het implementeren van de testomgeving als-is in uw eigen Azure-abonnement. Of u kunt de sjabloon aanpassen en [implementeren met behulp van PowerShell of Azure CLI](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Waarom wordt mijn VM's in verschillende resourcegroepen met willekeurige namen gemaakt? Kan ik wijzigen of deze brongroepen wijzigen?
Resourcegroepen zijn op deze manier gemaakt zodat DevTest Labs gebruikersmachtigingen en toegang tot virtuele machines kunt beheren. Hoewel u kunt een virtuele machine naar een andere resourcegroep verplaatsen en de naam die u wilt gebruiken, raden wij aan dat u wijzigingen niet in de resourcegroepen aanbrengen. We werken op het verbeteren van deze ervaring zodat meer flexibiliteit.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hoeveel labs kan ik maken onder hetzelfde abonnement?
Er is niet een bepaalde limiet van het aantal labs dat per abonnement kan worden gemaakt. De hoeveelheid resources die per abonnement worden gebruikt, is echter beperkt. U kunt meer informatie over de [limieten en quota's voor het Azure-abonnementen](../azure-subscription-service-limits.md) en [hoe deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Hoeveel virtuele machines kan ik per lab maken?
Er is geen specifieke limiet voor het aantal virtuele machines die per lab kunnen worden gemaakt. De resources (VM-kernen, openbare IP-adressen, enzovoort) die worden gebruikt, zijn echter beperkt per abonnement. U kunt meer informatie over de [limieten en quota's voor het Azure-abonnementen](../azure-subscription-service-limits.md) en [hoe deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hoe kan ik een directe koppeling met mijn lab delen

1. In de Azure portal, gaat u naar het lab.
2. De lab-URL in uw browser kopiëren en vervolgens te delen met uw lab-gebruikers.

> [!NOTE]
> Als een lab-gebruiker is een externe gebruiker die heeft een [Microsoft-account](#what-is-a-microsoft-account), maar die is geen lid van de Active Directory-exemplaar van uw organisatie, de gebruiker ziet mogelijk een foutbericht wanneer ze proberen te krijgen tot de gedeelde koppeling. Als een externe gebruiker een foutbericht ziet, vraagt u de gebruiker eerst hun naam te selecteren in de rechterbovenhoek van de Azure portal. Klik in de **Directory** gedeelte van het menu, de gebruiker kunt selecteren de map waar het lab bestaat.
>
>

## <a name="what-is-a-microsoft-account"></a>Wat is een Microsoft-account?
Een Microsoft-account is een account dat u gebruikt voor bijna alles wat die u met Microsoft-apparaten en services doet. Het is een e-mailadres en wachtwoord dat u aan te melden bij Skype, Outlook.com, OneDrive, Windows phone of Xbox Live. Een enkel account betekent dat uw bestanden, foto's, contactpersonen en instellingen u op elk apparaat volgen kunnen.

> [!NOTE]
> Een Microsoft-account gebruikt om te worden aangeroepen een *Windows Live ID*.
>
>


**Problemen oplossen**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Mijn artefact is mislukt tijdens het maken van VM. Hoe kan ik oplossen?
Zie voor informatie over het ophalen van Logboeken voor de mislukte artefacten, [het artefact mislukte diagnoses in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Waarom wordt niet mijn bestaande virtuele netwerk juist opslaan?
Een kans is dat de naam van uw virtuele netwerk punten bevat. Als dit het geval is, proberen te verwijderen van de punten of afbreekstreepjes vervangen. Probeer vervolgens opnieuw op te slaan van het virtuele netwerk.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Waarom krijg ik een 'Bovenliggende resource is niet gevonden'-Fout bij het inrichten van een virtuele machine vanuit PowerShell?
Wanneer een bron een bovenliggend item naar een andere bron is, wordt de bovenliggende resource moet bestaan voordat u de onderliggende resource maken. Als de bovenliggende resource niet bestaat, ziet u een **ParentResourceNotFound** bericht. Als u een afhankelijkheid op de bovenliggende resource niet opgeeft, kan de onderliggende resource worden geïmplementeerd voordat u het bovenliggende item.

Virtuele machines zijn onderliggende resources onder een lab in een resourcegroep. Wanneer u een Resource Manager-sjablonen gebruikt voor het implementeren van virtuele machines met behulp van PowerShell, moet de naam van de resourcegroep opgegeven in het PowerShell-script de naam van de resource van het testlab. Zie voor meer informatie [veelvoorkomende fouten van de Azure-implementatie oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Waar vind ik meer foutinformatie als de implementatie van een virtuele machine mislukt?
VM-implementatiefouten worden vastgelegd in activiteitenlogboeken. U vindt lab VM activiteitenlogboeken onder **controlelogboeken** of **diagnostische gegevens van virtuele machine** in het menu van de resource op de testomgeving VM blade (de blade wordt weergegeven nadat u de virtuele machine van de **mijn virtuele machines** lijst).

Soms is foutmelding de implementatie voordat VM-implementatie wordt gestart. Een voorbeeld is wanneer de limiet voor het abonnement voor een bron die is gemaakt met de virtuele machine wordt overschreden. In dit geval de details van fout vastgelegd in het lab-niveau activiteitenlogboeken. Activiteitenlogboeken bevinden zich op de onderkant van de **configuratie en het beleid** instellingen. Zie voor meer informatie over het gebruik van de activiteit geregistreerd in Azure, [activiteitenlogboeken wilt controleren acties op resources bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
