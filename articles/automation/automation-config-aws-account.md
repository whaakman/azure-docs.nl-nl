---
title: Verificatie met Amazon Web Services configureren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een AWS-referentie voor runbooks in Azure Automation maakt en valideert bij het beheren van AWS-resources.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: AWS-verificatie, AWS configureren
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 68805a6b28fc9454262cb0503daa23af93c76a7e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks met Amazon Web Services verifiëren
Het automatiseren van algemene taken met resources in Amazon Web Services (AWS) kan worden bewerkstelligd met Automation-runbooks in Azure.  U kunt vele taken in AWS automatiseren met Automation-runbooks, net zoals u dit kunt met resources in Azure.  U hebt slechts twee dingen nodig:

* Een AWS-abonnement en een set referenties.  Het gaat dan met name om uw AWS-toegangssleutel en de geheime sleutel.  Raadpleeg het artikel [AWS-referenties gebruiken](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) voor meer informatie.
* Een Azure-abonnement en een Automation-account.  Raadpleeg [Verificatieplanning](automation-offering-get-started.md#authentication-planning) voor meer informatie over het instellen van een Azure Automation-account.  

Als u wilt verifiëren met AWS, moet u een set AWS-referenties opgeven om uw runbooks te verifiëren die vanuit Azure Automation worden uitgevoerd. Als u al een Automation-account hebt gemaakt en u wilt dit account gebruiken om te verifiëren met AWS, kunt u de stappen in de volgende sectie uitvoeren.  Als u een account voor runbooks wilt toewijzen dat is gericht op AWS-resources, moet u eerst een nieuw [Automation-account](automation-offering-get-started.md) maken (sla de optie voor het maken van een service-principal over). Volg daarna de onderstaande stappen.

## <a name="configure-automation-account"></a>Automation-account configureren
Voor communicatie tussen Azure Automation en AWS moet u eerst uw AWS-referenties ophalen en deze opslaan als assets in Azure Automation.  Voer de volgende stappen uit die in het AWS-document [Toegangssleutels voor uw AWS-account beheren](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) zijn gedocumenteerd om een toegangssleutel te maken en kopieer de **toegangssleutel-id** en **geheime toegangssleutel** (download desgewenst uw sleutelbestand om het ergens veilig op te slaan).

Nadat u uw AWS-beveiligingssleutels hebt gemaakt en gekopieerd, moet u een referentieasset met een Azure Automation-account maken om deze sleutels veilig op te slaan en ernaar te verwijzen met uw runbooks.  Volg de stappen in de sectie **Een nieuwe referentie maken** in het artikel [Referentieassets in Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) en voer de volgende gegevens in:

1. Voer in het vak **Naam** **AWScred** in of een geschikte waarde die voldoet aan uw naamgevingsstandaarden.  
2. Typ uw **toegangs-id** in het vak **Gebruikersnaam** en uw **geheime toegangssleutel** in de vakken **Wachtwoord** en **Wachtwoord bevestigen**.   

## <a name="next-steps"></a>Volgende stappen
* Lees het oplossingsartikel [Implementatie van een virtuele machine in Amazon Web Services automatiseren](automation-scenario-aws-deployment.md) voor informatie over het maken van runbooks om taken in AWS te automatiseren.

