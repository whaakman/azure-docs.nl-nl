---
title: Een zelfstandig Azure Automation-account maken
description: In dit artikel leidt u door de stappen van het maken, testen en met behulp van een voorbeeld van de beveiliging principal verificatie in Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 021ab6cd29634c2e2f17234e188edce2a14449cf
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42057199"
---
# <a name="create-a-standalone-azure-automation-account"></a>Een zelfstandig Azure Automation-account maken

In dit artikel wordt beschreven hoe u een Azure Automation-account maken in Azure portal. U kunt de portal Automation-account gebruiken om te evalueren en meer informatie over Automation zonder aanvullende beheeroplossingen of integratie met Azure Log Analytics. U kunt deze beheeroplossingen toevoegen of integreren in Log Analytics voor geavanceerde controle van runbooktaken op elk gewenst moment in de toekomst.

Met een Automation-account, kunt u runbooks verifiëren met het beheren van resources in Azure Resource Manager of het klassieke implementatiemodel.

Wanneer u een Automation-account in Azure portal maakt, worden deze accounts worden automatisch gemaakt:

* **Uitvoeren als-account**. Dit account heeft de volgende taken:
  * Hiermee maakt u een service-principal in Azure Active Directory (Azure AD).
  * Hiermee maakt u een certificaat.
  * Toegewezen Contributor Role-Based Access Control (RBAC), die Azure Resource Manager-resources beheert met behulp van runbooks.
* **Klassieke uitvoeren als-account**. Dit account wordt een beheercertificaat geüpload. Het certificaat beheert klassieke resources met behulp van runbooks.

Met deze accounts voor u gemaakt, kunt u snel starten het bouwen en implementeren van runbooks ter ondersteuning van uw automatiseringsbehoeften.

## <a name="permissions-required-to-create-an-automation-account"></a>Vereiste machtigingen voor het maken van een Automation-account

Maken of bijwerken van een Automation-account en de taken die worden beschreven in dit artikel, moet u de volgende rechten en machtigingen hebben:

* Voor het maken van een Automation-account, moet uw Azure AD-gebruikersaccount worden toegevoegd aan een rol die machtigingen heeft die equivalent zijn aan de rol eigenaar voor **Microsoft. Automation** resources. Zie voor meer informatie, [toegangsbeheer op basis van rollen in Azure Automation](automation-role-based-access-control.md).
* In de Azure-portal onder **Azure Active Directory** > **beheren** > **App-registraties**als **App-registraties**  is ingesteld op **Ja**, kunnen gebruikers niet-beheerders in uw Azure AD-tenant [Active Directory-toepassingen registreren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Als **App-registraties** is ingesteld op **Nee**, moet de gebruiker die deze actie uitvoert een globale beheerder zijn in Azure AD.

Als u niet lid zijn van Active Directory-exemplaar van het abonnement voordat u wordt toegevoegd aan de rol van globale beheerder/CO-beheerder van het abonnement, wordt u als Gast toegevoegd aan Active Directory. In dit scenario ziet u dit bericht op de **Automation-Account toevoegen** pagina: "U bent niet gemachtigd om te maken."

Als een gebruiker is toegevoegd aan de rol globale beheerder/CO-beheerder eerst, kunt u deze verwijderen uit Active Directory-exemplaar van het abonnement, en ze vervolgens toegewezen aan de volledige gebruikersrol in Active Directory.

Om te controleren of gebruikersrollen:

1. In de Azure-portal, gaat u naar de **Azure Active Directory** deelvenster.
1. Selecteer **Gebruikers en groepen**.
1. Selecteer **alle gebruikers**.
1. Nadat u een specifieke gebruiker selecteert, selecteert u **profiel**. De waarde van de **gebruikerstype** kenmerk onder het gebruikersprofiel mag geen **Gast**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Een nieuw automatiseringsaccount maken in Azure portal

Voor het maken van een Azure Automation-account in Azure portal, moet u de volgende stappen uitvoeren:

1. Meld u aan bij de Azure portal met een account dat lid is van de rol Abonnementsbeheerders en CO-beheerder van het abonnement.
1. Selecteer **+ een Resource maken**.
1. Zoeken naar **Automation**. Selecteer in de lijst met zoekresultaten **Automation**.

   ![Zoek en selecteer Automation en besturing in de Azure Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Selecteer op het volgende scherm **maken**.
  ![Automation-account toevoegen](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

  > [!NOTE]
  > Als u het volgende bericht in de **Automation-Account toevoegen** deelvenster uw account is geen lid van de rol Abonnementsbeheerders en CO-beheerder van het abonnement.
  >
  > ![Waarschuwing voor Automation-account toevoegen](media/automation-create-standalone-account/create-account-without-perms.png)
  >
1. In de **Automation-Account toevoegen** deelvenster in de **naam** vak, voer een naam in voor uw nieuwe Automation-account. Deze naam kan niet worden gewijzigd nadat deze is geselecteerd.
1. Als u meer dan één abonnement hebt, in de **abonnement** , geeft u het abonnement dat u wilt gebruiken voor het nieuwe account.
1. Voor **resourcegroep**, invoeren of een nieuwe of bestaande resourcegroep selecteren.
1. Voor **locatie**, selecteert u een Azure-datacenter-locatie.
1. Voor de **Azure uitvoeren als-account maken** optie, zorg ervoor dat **Ja** is geselecteerd en selecteer vervolgens **maken**.

  > [!NOTE]
  > Als u ervoor kiezen geen uitvoeren als-account maken door te selecteren **Nee** voor **Azure uitvoeren als-account maken**, een bericht wordt weergegeven in de **Automation-Account toevoegen** deelvenster. Hoewel het account is gemaakt in Azure portal, kan het account een overeenkomstige verificatie-id beschikt niet over in uw model klassieke implementatie abonnement of in de adreslijstservice van het Azure Resource Manager-abonnement. Het Automation-account geen daarom toegang tot resources in uw abonnement. Hierdoor kunnen alle runbooks die verwijzen naar dit account geen taken verifiëren en uitvoeren met resources in die implementatiemodellen.
  >
  > ![Waarschuwing voor Automation-account toevoegen](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
  >
  > Wanneer de service-principal niet is gemaakt, wordt de rol Inzender niet toegewezen.
  >

1. Selecteer voor het volgen van de voortgang van het maken van een Automation-account, in het menu **meldingen**.

### <a name="resources-included"></a>Beschikbare resources

Wanneer het Automation-account is gemaakt, worden er automatisch verschillende resources voor u gemaakt. Na het maken, deze runbooks kunnen veilig worden verwijderd als u niet wilt houden. Het Run As-Accounts, kan worden gebruikt om uw account in een runbook te verifiëren, en moet worden gelaten, tenzij u een andere groep maken of ze niet vereist. In de volgende tabel vindt u een overzicht van de bronnen voor het Uitvoeren als-account.

| Resource | Beschrijving |
| --- | --- |
| AzureAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook dat laat hoe u verifieert zien met behulp van de uitvoeren als-account. Het runbook ophaalt alle Resource Managers-resources. |
| AzureAutomationTutorialScript Runbook |Een voorbeeld van de PowerShell-runbook dat laat hoe u verifieert zien met behulp van de uitvoeren als-account. Het runbook ophaalt alle Resource Managers-resources. |
| AzureAutomationTutorialPython2-runbook |Een voorbeeld van Python-runbook dat laat hoe u verifieert zien met behulp van de uitvoeren als-account. Het runbook een lijst met alle resourcegroepen die aanwezig zijn in het abonnement. |
| AzureRunAsCertificate |Een certificaatasset dat automatisch wordt gemaakt wanneer het Automation-account wordt gemaakt, of met behulp van een PowerShell-script voor een bestaand account. Het certificaat wordt geverifieerd met Azure, zodat u Azure Resource Manager-resources vanuit runbooks beheren kunt. Dit certificaat is één jaar geldig. |
| AzureRunAsConnection |Een verbindingsasset dat automatisch wordt gemaakt wanneer het Automation-account wordt gemaakt, of met behulp van een PowerShell-script voor een bestaand account. |

In de volgende tabel vindt u een overzicht van de bronnen voor het klassieke Uitvoeren als-account.

| Resource | Beschrijving |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Een voorbeeld van een grafisch runbook. Het runbook wordt alle klassieke virtuele machines in een abonnement ophaalt met behulp van het klassieke uitvoeren als-Account (certificaat). Vervolgens geeft de namen van de virtuele machine en de status. |
| AzureClassicAutomationTutorial Script Runbook |Een voorbeeld van de PowerShell-runbook. Het runbook wordt alle klassieke virtuele machines in een abonnement ophaalt met behulp van het klassieke uitvoeren als-Account (certificaat). Vervolgens geeft de namen van de virtuele machine en de status. |
| AzureClassicRunAsCertificate |Een certificaatasset dat automatisch wordt gemaakt. Het certificaat wordt geverifieerd met Azure, zodat u klassieke Azure-resources vanuit runbooks beheren kunt. Dit certificaat is één jaar geldig. |
| AzureClassicRunAsConnection |Een verbindingsasset dat automatisch wordt gemaakt. De asset wordt geverifieerd met Azure, zodat u klassieke Azure-resources vanuit runbooks beheren kunt. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over grafisch ontwerpen, [grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Mijn eerste Python2-runbook](automation-first-runbook-textual-python2.md) om aan de slag te gaan met Python2-runbooks.
