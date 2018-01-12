---
title: Gebruikersaccount voor Azure AD maken | Microsoft Docs
description: "In dit artikel wordt beschreven hoe een gebruikersaccountreferenties Azure AD voor runbooks maken in Azure Automation om te verifiëren in Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: Azure Active Directory-gebruiker, Azure Service Management, Azure AD-gebruikersaccount
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Runbooks met klassieke Azure-implementatie en Resource Manager verifiëren
In dit artikel worden de stappen beschreven die u moet uitvoeren om een Azure AD-gebruikersaccount te configureren voor Azure Automation-runbooks die worden uitgevoerd met resources van een klassiek Azure-implementatiemodel of Azure Resource Manager.  Terwijl de identiteit van een ondersteunde verificatie voor uw Azure Resource Manager gebaseerde runbooks, blijft dit is de aanbevolen methode om een Azure uitvoeren als-account te gebruiken.       

## <a name="create-a-new-azure-active-directory-user"></a>Een nieuwe Azure Active Directory-gebruiker maken
1. Meld u aan bij Azure Portal als servicebeheerder voor het Azure-abonnement dat u wilt beheren.
2. Selecteer **Azure Active Directory** > **gebruikers en groepen** > **alle gebruikers** > **nieuwe gebruiker**.
3. Geef details op voor de gebruiker, zoals **naam** en **gebruikersnaam**.  
4. Noteer de volledige naam van de gebruiker en het tijdelijke wachtwoord.
5. Selecteer **functie Directory**.
6. Rol Global of beperkt beheerder toe te wijzen.
7. Meld u af bij Azure en meld u opnieuw aan met het account dat u zojuist hebt gemaakt. U wordt gevraagd om het wachtwoord van de gebruiker te wijzigen.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Een Automation-account maken in Azure Portal
In deze sectie voert u de volgende stappen uit om een nieuw Azure Automation-account te maken in Azure Portal, dat wordt gebruikt met uw runbooks waarmee resources worden beheerd in een Azure Resource Manager-modus.  

1. Meld u aan bij Azure Portal als servicebeheerder voor het Azure-abonnement dat u wilt beheren.
2. Selecteer **Automation-accounts**.
3. Selecteer **Toevoegen**.<br><br>![Automation-account toevoegen](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. Typ op de blade **Automation-account toevoegen** in het vak **Naam** een naam voor uw nieuwe Automation-account.
5. Als u meer dan één abonnement hebt, geeft u het abonnement voor het nieuwe account op, evenals een nieuwe of bestaande **resourcegroep** en de **locatie** van een Azure-datacenter.
6. Selecteer de waarde **Ja** voor de optie **Een Uitvoeren als-account voor Azure maken** en klik op de knop **Maken**.  
   
    > [!NOTE]
    > Als u de optie **Nee** selecteert omdat u geen Uitvoeren als-account wilt maken, wordt een waarschuwing weergegeven op de blade **Automation-account toevoegen**.  Hoewel het account wordt gemaakt en wordt toegewezen aan de rol **Inzender** in het abonnement, heeft het account geen overeenkomstige verificatie-id in de adreslijstservice van uw abonnementen en daardoor ook geen toegang tot resources in uw abonnement.  Hierdoor kunnen alle runbooks die naar dit account verwijzen, geen taken verifiëren en uitvoeren in Azure Resource Manager-resources.
    > 
    >

    <br>![Waarschuwing bij Automation-account toevoegen](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Terwijl in Azure het Automation-account wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

Als het maken van de referentie is voltooid, moet u vervolgens een referentieasset maken om het Automation-account te koppelen aan het eerder gemaakte AD-gebruikersaccount.  Let op: We hebben alleen het Automation-account gemaakt en dit is niet aan een verificatie-identiteit gekoppeld.  Volg de stappen die worden beschreven in het artikel [Referentieassets in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) en voer de waarde in voor **gebruikersnaam** in de indeling **domein\gebruiker**.

## <a name="use-the-credential-in-a-runbook"></a>De referentie in een runbook gebruiken
U kunt de referentie in een runbook ophalen met de activiteit [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) en deze vervolgens gebruiken met [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) om verbinding te maken met uw Azure-abonnement. Als de referentie een beheerder van meerdere Azure-abonnementen is, dan moet u ook [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) gebruiken om het juiste abonnement op te geven. Dit wordt getoond in de onderstaande voorbeeld-Windows PowerShell die doorgaans wordt weergegeven boven aan de meeste Azure Automation-runbooks.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

U moet deze regels na [controlepunten](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) in uw runbook herhalen. Als het runbook wordt onderbroken en vervolgens voor een andere werkrol wordt hervat, moet de verificatie opnieuw worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
* Lees over de verschillende runbooktypen en stappen voor het maken van uw eigen runbooks in het volgende artikel [Azure Automation-runbooktypen](automation-runbook-types.md)

