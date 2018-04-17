---
title: Een zelfstandig Azure Automation-account maken
description: Dit artikel begeleidt u bij de stappen voor het maken, testen en het gebruik van een voorbeeld van de beveiligingsverificatie in Azure Automation.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 619b0a12122fcd6b51a4e34a3021bc99047f9251
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-standalone-azure-automation-account"></a>Een zelfstandig Azure Automation-account maken

In dit artikel laat zien hoe een Azure Automation-account maken in de Azure-portal. U kunt het portal Automation-account gebruiken om te evalueren en meer informatie over automatisering zonder aanvullende oplossingen of -integratie met Azure-logboekanalyse. U kunt deze oplossingen toevoegen of integreren met Log Analytics voor geavanceerde controle van runbooktaken op elk gewenst moment in de toekomst.

Met een Automation-account, kunt u runbooks verifiëren met het beheren van resources in Azure Resource Manager of het klassieke implementatiemodel.

Wanneer u een Automation-account in de Azure portal maakt, worden deze accounts worden automatisch gemaakt:

* **Run As-account**. Dit account heeft de volgende taken:
  * Een service-principal gemaakt in Azure Active Directory (Azure AD).
  * Een certificaat gemaakt.
  * Wijst de Contributor Role-Based toegangsbeheer (RBAC), die Azure Resource Manager-resources met behulp van runbooks beheert.
* **Klassieke Run As-account**. Dit account uploadt een beheercertificaat. Het certificaat beheert klassieke resources met behulp van runbooks.

Met deze accounts voor u gemaakt, kunt u snel starten maken en implementeren van runbooks ter ondersteuning van uw automatiseringsbehoeften.

## <a name="permissions-required-to-create-an-automation-account"></a>Vereiste machtigingen voor het maken van een Automation-account

Maken of bijwerken van een Automation-account en voer de taken die worden beschreven in dit artikel, moet u de volgende rechten en machtigingen hebben:

* Voor het maken van een Automation-account, moet uw Azure AD-gebruikersaccount worden toegevoegd aan een rol met de machtigingen die equivalent zijn aan de rol van eigenaar voor **Microsoft. Automation** resources. Zie voor meer informatie [toegangsbeheer op basis van rollen in Azure Automation](automation-role-based-access-control.md).
* In de Azure-portal onder **Azure Active Directory** > **beheren** > **App registraties**als **App registraties**  is ingesteld op **Ja**, kunnen gebruikers niet-beheerders in uw Azure AD-tenant [Active Directory-toepassingen registreren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Als **App registraties** is ingesteld op **Nee**, moet de gebruiker die deze actie uitvoert een globale beheerder zijn in Azure AD.

Als u niet lid zijn van de Active Directory-exemplaar van het abonnement voordat u aan het abonnement globale beheerder/CO-rol worden toegevoegd, zijn u als Gast toegevoegd aan Active Directory. In dit scenario wordt dit bericht wordt weergegeven op de **Automation-Account toevoegen** pagina: "U bent niet gemachtigd om te maken."

Als een gebruiker is toegevoegd aan de rol globale beheerder/CO-eerst, kunt u uit de Active Directory-exemplaar van het abonnement verwijderen en vervolgens opnieuw toe te voegen aan de volledige gebruikersrol in Active Directory.

Gebruikersrollen controleren:

1. In de Azure portal, gaat u naar de **Azure Active Directory** deelvenster.
1. Selecteer **gebruikers en groepen**.
1. Selecteer **alle gebruikers**.
1. Nadat u een specifieke gebruiker selecteert, selecteert u **profiel**. De waarde van de **gebruikerstype** kenmerk onder profiel van de gebruiker mag geen **Gast**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Maak een nieuw automatiseringsaccount in de Azure portal

Voor het maken van een Azure Automation-account in de Azure portal, moet u de volgende stappen uitvoeren:

1. Aanmelden bij de Azure-portal met een account dat lid is van de rol van de beheerders abonnement en een CO-beheerder van het abonnement.
1. Selecteer **+ maken van een Resource**.
1. Zoeken naar **Automation**. Selecteer in de lijst met zoekresultaten **Automation**.

   ![Zoek en selecteer Automation & besturingselement in Azure Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Selecteer op het volgende scherm **maken**.
  ![Automation-account toevoegen](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

  > [!NOTE]
  > Als u ziet het volgende bericht in de **Automation-Account toevoegen** deelvenster uw account is niet lid zijn van de rol van de beheerders abonnement en een CO-beheerder van het abonnement.
  >
  > ![Waarschuwing voor Automation-account toevoegen](media/automation-create-standalone-account/create-account-without-perms.png)
  >
1. In de **Automation-Account toevoegen** deelvenster, in de **naam** Voer een naam voor uw nieuwe Automation-account.
1. Als u meer dan één abonnement hebt, in de **abonnement** geeft u het abonnement dat u wilt gebruiken voor het nieuwe account.
1. Voor **resourcegroep**Typ of Selecteer een nieuwe of bestaande resourcegroep.
1. Voor **locatie**, selecteer de locatie van een Azure-datacenter.
1. Voor de **Azure uitvoeren als-account maken** optie, zorg ervoor dat **Ja** is geselecteerd en selecteer vervolgens **maken**.

  > [!NOTE]
  > Als u niet uitvoeren als-account maken door te selecteren **Nee** voor **Azure uitvoeren als-account maken**, een bericht weergegeven in de **Automation-Account toevoegen** deelvenster. Hoewel het account is gemaakt in de Azure portal, hebben het account geen overeenkomstige verificatie-id in uw model klassieke implementatie abonnement of in de directoryservice van de Azure Resource Manager-abonnement. Het Automation-account geen daarom toegang tot bronnen in uw abonnement. Dit voorkomt dat alle runbooks die verwijzen naar dit account kunnen taken verifiëren en uitvoeren op basis van bronnen in de implementatiemodellen.
  >
  > ![Waarschuwing voor Automation-account toevoegen](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
  >
  > Wanneer de service-principal niet gemaakt is, is niet de rol Inzender toegewezen.
  >

1. Selecteer om de voortgang van het maken van de Automation-account in het menu te volgen **meldingen**.

### <a name="resources-included"></a>Beschikbare resources

Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt. Na het maken van deze runbooks kan deze veilig worden verwijderd als u niet wenst te houden. De Run As-Accounts, kunnen worden gebruikt om uw account in een runbook te verifiëren en te laten, tenzij u een nieuw wachtwoord instellen of ze niet nodig. In de volgende tabel vindt u een overzicht van de bronnen voor het Uitvoeren als-account.

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook |Een grafische voorbeeldrunbook dat laat hoe u verifieert zien met behulp van het Run As-account. Het runbook haalt alle Resource Manager-resources. |
| AzureAutomationTutorialScript Runbook |Een voorbeeldrunbook PowerShell die u laat hoe u verifieert zien met behulp van het Run As-account. Het runbook haalt alle Resource Manager-resources. |
| AzureAutomationTutorialPython2-runbook |Een voorbeeldrunbook Python die u laat hoe u verifieert zien met behulp van het Run As-account. Het runbook bevat alle resourcegroepen die aanwezig zijn in het abonnement. |
| AzureRunAsCertificate |Een certificaatasset dat automatisch wordt gemaakt wanneer het Automation-account wordt gemaakt, of met behulp van een PowerShell-script voor een bestaande account. Het certificaat wordt geverifieerd met Azure zodat u Azure Resource Manager-resources van runbooks beheren kunt. Dit certificaat is één jaar geldig. |
| AzureRunAsConnection |Een verbindingsasset dat automatisch wordt gemaakt wanneer het Automation-account wordt gemaakt, of met behulp van een PowerShell-script voor een bestaande account. |

In de volgende tabel vindt u een overzicht van de bronnen voor het klassieke Uitvoeren als-account.

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Een voorbeeld grafisch runbook. Het runbook haalt alle klassieke VM's in een abonnement met behulp van de klassieke Run As-Account (certificaat). Vervolgens wordt weergegeven de VM-namen en de status. |
| AzureClassicAutomationTutorial Script Runbook |Een voorbeeldrunbook PowerShell. Het runbook haalt alle klassieke VM's in een abonnement met behulp van de klassieke Run As-Account (certificaat). Vervolgens wordt weergegeven de VM-namen en de status. |
| AzureClassicRunAsCertificate |Een certificaatasset dat automatisch wordt gemaakt. Het certificaat wordt geverifieerd met Azure zodat u de klassieke Azure-resources van runbooks beheren kunt. Dit certificaat is één jaar geldig. |
| AzureClassicRunAsConnection |Een verbindingsasset dat automatisch wordt gemaakt. De asset verifieert met Azure zodat u de klassieke Azure-resources van runbooks beheren kunt. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over grafisch ontwerpen, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Mijn eerste Python2-runbook](automation-first-runbook-textual-python2.md) om aan de slag te gaan met Python2-runbooks.
