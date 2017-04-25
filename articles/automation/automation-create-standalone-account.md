---
title: Zelfstandig Azure Automation-account maken | Microsoft Docs
description: Zelfstudie die u helpt bij het maken en testen en bij het voorbeeldgebruik van de verificatie van beveiligingsprincipals in Azure Automation.
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 97b863748dd726e19217e360645b8e6189c010b3
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-standalone-azure-automation-account"></a>Een zelfstandig Azure Automation-account maken
In dit onderwerp wordt beschreven hoe u een Automation-account kunt maken vanuit Azure Portal, als u Azure Automation wilt evalueren en leren gebruiken, zonder hierbij gebruik te maken van de aanvullende beheeroplossingen of integratie met OMS Log Analytics om geavanceerde bewaking van runbooktaken te bieden.  U kunt deze beheeroplossingen op enig moment in de toekomst toevoegen aan of integreren in Log Analytics.  Met een Automation-account kunt u runbooks verifiëren waarmee resources worden beheerd in Azure Resource Manager of een klassieke Azure-implementatie.

Wanneer u in Azure Portal een Automation-account maakt, wordt automatisch het volgende gemaakt:

* Uitvoeren als-account; hiermee wordt een nieuwe service-principal in Azure Active Directory gemaakt, een certificaat, en wordt het RBAC (op rollen gebaseerd toegangsbeheer) toegewezen, dat wordt gebruikt om Resource Manager-resources te beheren met runbooks.   
* Klassiek uitvoeren als-account; door een beheercertificaat te uploaden, dat wordt gebruikt om klassieke resources te beheren met runbooks.  

Dit maakt het proces voor u eenvoudiger zodat u sneller runbooks kunt maken en implementeren, ter ondersteuning van uw automatiseringsbehoeften.  

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Nieuw Automation-account maken vanuit Azure Portal
In deze sectie voert u de volgende stappen uit om vanuit Azure Portal een nieuw Azure Automation-account te maken.    

>[!NOTE]
>Voor het maken van een Automation-account moet u lid zijn van de rol Servicebeheerders of medebeheerder zijn van het abonnement dat toegang verleent tot het abonnement. U moet ook als gebruiker worden toegevoegd aan het standaard Active Directory-exemplaar van dat abonnement. Aan het account hoeft geen bevoorrechte rol te worden toegewezen.
>
>Als u geen lid bent van het Active Directory-exemplaar van het abonnement voordat u wordt toegevoegd aan de rol van medebeheerder van het abonnement, wordt u als gast toegevoegd aan Active Directory. In dat geval wordt de waarschuwing 'U hebt geen machtigingen voor het maken...' weergegeven op de blade **Automation-account toevoegen**.
>
>Gebruikers die zijn toegevoegd aan de rol Medebeheerder, kunnen worden verwijderd uit het Active Directory-exemplaar van het abonnement en opnieuw worden toegevoegd, zodat ze een volledige gebruiker worden in Active Directory. U kunt deze situatie controleren in het deelvenster **Azure Active Directory** in Azure Portal door **Gebruikers en groepen** te selecteren. Selecteer vervolgens **Alle gebruikers**, daarna de specifieke gebruiker en tot slot **Profiel**. De waarde van het kenmerk **Gebruikerstype** onder het gebruikersprofiel mag niet gelijk zijn aan **Gast**.

1. Meld u aan bij Azure Portal met een account dat lid is van de rol Abonnementsbeheerders en dat medebeheerder is van het abonnement.
2. Klik op **Nieuw**.<br><br> ![De optie Nieuw selecteren in Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Zoek naar **Automation** en klik in de zoekresultaten op **Automation en beheer***.<br><br> ![Zoek naar en selecteer Automation in Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. Klik op de blade Automation-accounts op **Toevoegen**.<br><br>![Automation-account toevoegen](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
   
   > [!NOTE]
   > Als u op de blade **Automation-account toevoegen** de volgende waarschuwing ziet, is uw account geen lid van de rol Abonnementsbeheerders en geen medebeheerder van het abonnement.<br><br>![Waarschuwing bij Automation-account toevoegen](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. Typ op de blade **Automation-account toevoegen** in het vak **Naam** een naam voor uw nieuwe Automation-account.
5. Als u meer dan één abonnement hebt, geeft u er een op voor het nieuwe account, evenals een nieuwe of bestaande **resourcegroep** en de **locatie** van een Azure-datacenter.
6. Controleer of de waarde **Ja** is geselecteerd voor de optie **Een Uitvoeren als-account voor Azure maken** en klik op de knop **Maken**.  
   
   > [!NOTE]
   > Als u de optie **Nee** selecteert omdat u geen Uitvoeren als-account wilt maken, wordt een waarschuwing weergegeven op de blade **Automation-account toevoegen**.  Hoewel het account wordt gemaakt in Azure Portal, heeft het geen overeenkomstige verificatie-id in de adreslijstservice van het klassieke of Resource Manager-abonnement en daardoor ook geen toegang tot resources in uw abonnement.  Hierdoor kunnen alle runbooks die naar dit account verwijzen, geen taken verifiëren en uitvoeren met resources in die implementatiemodellen.
   > 
   > ![Waarschuwing bij Automation-account toevoegen](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Wanneer de service-principal niet is gemaakt, wordt de rol Inzender niet toegewezen.
   > 

7. Terwijl in Azure het Automation-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

### <a name="resources-included"></a>Beschikbare resources
Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt.  In de volgende tabel vindt u een overzicht van de bronnen voor het Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureAutomationTutorialScript Runbook |Een voorbeeld van een PowerShell-runbook dat laat zien hoe u verifieert met behulp van het Uitvoeren als-account en dat alle Resource Managers-resources ophaalt. |
| AzureRunAsCertificate |Certificaatasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt.  Hiermee kunt u verifiëren met Azure zodat u Azure Resource Manager-resources kunt beheren vanuit runbooks.  Dit certificaat is één jaar geldig. |
| AzureRunAsConnection |Verbindingsasset dat automatisch tijdens het maken van het Automation-account of met onderstaand PowerShell-script voor een bestaande account wordt gemaakt. |

In de volgende tabel vindt u een overzicht van de bronnen voor het klassieke Uitvoeren als-account.<br>

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
| AzureClassicAutomationTutorial Script Runbook |Een voorbeeld van een PowerShell-runbook dat alle klassieke virtuele machines in een abonnement ophaalt met het klassieke Uitvoeren als-account (certificaat) en vervolgens de VM-naam en -status weergeeft. |
| AzureClassicRunAsCertificate |Certificaatasset dat automatisch wordt gemaakt en wordt gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks.  Dit certificaat is één jaar geldig. |
| AzureClassicRunAsConnection |Verbindingsasset dat automatisch wordt gemaakt en wordt gebruikt voor verificatie met Azure, zodat u klassieke Azure-resources kunt beheren vanuit runbooks. |


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie Mijn eerste PowerShell Workflow-runbook (automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks.
