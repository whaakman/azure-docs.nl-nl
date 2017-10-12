---
title: Gebruikersaccount voor Azure AD maken | Microsoft Docs
description: "In dit artikel wordt beschreven hoe u gebruikersaccountreferenties van Azure AD voor runbooks in Azure Automation maakt om te verifiëren bij Azure en het klassieke Azure."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
keywords: Azure Active Directory-gebruiker, Azure Service Management, Azure AD-gebruikersaccount
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 8f24e6e57c2eec5950c8c12d9f4383ce11cf5c11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Runbooks met klassieke Azure-implementatie en Resource Manager verifiëren
In dit artikel worden de stappen beschreven die u moet uitvoeren om een Azure AD-gebruikersaccount te configureren voor Azure Automation-runbooks die worden uitgevoerd met resources van een klassiek Azure-implementatiemodel of Azure Resource Manager.  Hoewel dit een ondersteunde verificatie-identiteit blijft voor op Azure Resource Manager gebaseerde runbooks, is het gebruik van een Uitvoeren als-account voor Azure de aanbevolen methode.       

## <a name="create-a-new-azure-active-directory-user"></a>Een nieuwe Azure Active Directory-gebruiker maken
1. Meld u aan bij de klassieke Azure-portal als servicebeheerder voor het Azure-abonnement dat u wilt beheren.
2. Selecteer **Active Directory** en selecteer vervolgens de naam van de organisatiedirectory.
3. Selecteer het tabblad **Gebruikers** en selecteer vervolgens in het opdrachtgebied **Gebruiker toevoegen**.
4. Selecteer op de pagina **Vertel ons meer over deze gebruiker** onder **Type gebruiker** de optie **Nieuwe gebruiker in uw organisatie**.
5. Voer een gebruikersnaam in.  
6. Selecteer de mapnaam die is gekoppeld aan uw Azure-abonnement op de Active Directory-pagina.
7. Geef op de pagina **Gebruikersprofiel** een voor- en achternaam op, een gebruiksvriendelijke naam en gebruiker in de lijst **Rollen**.  Schakel **Multi-Factor Authentication** niet in.
8. Noteer de volledige naam van de gebruiker en het tijdelijke wachtwoord.
9. Selecteer **Instellingen > Beheerders > Toevoegen**.
10. Typ de volledige gebruikersnaam van de gebruiker die u hebt gemaakt.
11. Selecteer het abonnement dat de gebruiker moet gaan beheren.
12. Meld u af bij Azure en meld u opnieuw aan met het account dat u zojuist hebt gemaakt. U wordt gevraagd om het wachtwoord van de gebruiker te wijzigen.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Een Automation-account maken in de klassieke Azure-portal
In deze sectie voert u de volgende stappen uit om een nieuw Azure Automation-account te maken in Azure Portal, dat wordt gebruikt met uw resources waarmee runbooks worden beheerd in een klassieke Azure-implementatie.  

> [!NOTE]
> Automation-accounts die met de klassieke Azure-portal zijn gemaakt, kunnen worden beheerd met zowel de klassieke Azure-portal als Azure Portal en een van de sets met cmdlets. Zodra het account is gemaakt, maakt het niet meer uit hoe u resources binnen het account maakt en beheert. Als u van plan bent om de klassieke Azure-portal te blijven gebruiken, moet u deze gebruiken in plaats van Azure Portal voor het maken van Automation-accounts.
> 
> 

1. Meld u aan bij de klassieke Azure-portal als servicebeheerder voor het Azure-abonnement dat u wilt beheren.
2. Selecteer **Automation**.
3. Selecteer op de pagina **Automation** de optie **Een Automation-account maken**.
4. Typ in het vak **Een Automation-account maken** een naam voor uw nieuwe Automation-account en selecteer een **Regio** in de vervolgkeuzelijst.  
5. Klik op **OK** om uw instellingen te accepteren en maak het account.
6. Nadat dit is gemaakt, wordt dit weergegeven op de pagina **Automation**.
7. Klik op het account: hiermee gaat u naar de pagina Dashboard.  
8. Selecteer op de pagina Dashboard van Automation de optie **Assets**.
9. Selecteer op de pagina **Assets** de optie **Instellingen toevoegen**, onder aan de pagina.
10. Selecteer op de pagina **Instellingen toevoegen** de optie **Referentie toevoegen**.
11. Selecteer op de pagina **Referentie definiëren** de optie **Windows PowerShell-referentie** in de vervolgkeuzelijst **Referentietype** en geef een naam op voor de referentie.
12. Typ op de volgende pagina **Referentie definiëren** de gebruikersnaam van het eerder gemaakte AD-gebruikersaccount in het veld **Gebruikersnaam** en het wachtwoord in het veld **Wachtwoord** en **Wachtwoord bevestigen**. Klik op **OK** om uw wijzigingen op te slaan.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Een Automation-account maken in Azure Portal
In deze sectie voert u de volgende stappen uit om een nieuw Azure Automation-account te maken in Azure Portal, dat wordt gebruikt met uw runbooks waarmee resources worden beheerd in een Azure Resource Manager-modus.  

1. Meld u aan bij Azure Portal als servicebeheerder voor het Azure-abonnement dat u wilt beheren.
2. Selecteer **Automation-accounts**.
3. Klik op de blade Automation-accounts op **Toevoegen**.<br><br>![Automation-account toevoegen](media/automation-create-aduser-account/add-automation-acct-properties.png)
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

