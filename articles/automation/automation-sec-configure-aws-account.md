---
title: Verificatie met Amazon Web Services configureren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een AWS-referentie voor runbooks in Azure Automation maakt en valideert bij het beheren van AWS-resources.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: AWS-verificatie, AWS configureren
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 00b217a4cddac0a893564db27ffb4f460973c246
ms.openlocfilehash: eee65672b3a9615afe2850b0cbc6daa275fc04ec


---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks met Amazon Web Services verifiëren
Het automatiseren van algemene taken met resources in Amazon Web Services (AWS) kan worden bewerkstelligd met Automation-runbooks in Azure.  U kunt vele taken in AWS automatiseren met Automation-runbooks, net zoals u dit kunt met resources in Azure.  U hebt slechts twee dingen nodig:

* Een AWS-abonnement en een set referenties.  Het gaat dan met name om uw AWS-toegangssleutel en de geheime sleutel.  Lees voor meer informatie het artikel [AWS-referenties gebruiken](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Een Azure-abonnement en een Automation-account.  Raadpleeg het artikel [Uitvoeren als-account voor Azure configureren](automation-sec-configure-azure-runas-account.md) voor meer informatie over het instellen van een Azure Automation-account.  

Als u wilt verifiëren met AWS, moet u een set AWS-referenties opgeven om uw runbooks te verifiëren die vanuit Azure Automation worden uitgevoerd. Als u al een Automation-account hebt gemaakt en u wilt dit account gebruiken om te verifiëren met AWS, kunt u de stappen in de volgende sectie uitvoeren.  Als u een toegewezen account voor runbooks wilt dat is gericht op AWS-resources, moet u eerst een nieuw [Uitvoeren als-account voor Automation](automation-sec-configure-azure-runas-account.md) maken (sla de optie voor het maken van een service-principal over). Volg daarna de onderstaande stappen.

## <a name="configure-automation-account"></a>Automation-account configureren
Voor communicatie tussen Azure Automation en AWS moet u eerst uw AWS-referenties ophalen en deze opslaan als assets in Azure Automation.  Voer de volgende stappen uit die in het AWS-document [Toegangssleutels voor uw AWS-account beheren](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) zijn gedocumenteerd om een toegangssleutel te maken en kopieer de **toegangssleutel-id** en **geheime toegangssleutel** (download desgewenst uw sleutelbestand om het ergens veilig op te slaan).

Nadat u uw AWS-beveiligingssleutels hebt gemaakt en gekopieerd, moet u een referentieasset met een Azure Automation-account maken om deze sleutels veilig op te slaan en ernaar te verwijzen met uw runbooks.  Volg de stappen in de sectie **Een nieuwe referentieasset maken** in het artikel [Referentieassets in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) en voer de volgende gegevens in:

1. Voer in het vak **Naam** **AWScred** in of een geschikte waarde die voldoet aan uw naamgevingsstandaarden.  
2. Typ in het vak **Gebruikersnaam** uw **toegangs-id** en uw **geheime toegangssleutel** in het vak **Wachtwoord** en **Wachtwoord bevestigen**.   

## <a name="next-steps"></a>Volgende stappen
* Lees het oplossingsartikel [Implementatie van een virtuele machine in Amazon Web Services automatiseren](automation-scenario-aws-deployment.md) voor informatie over het maken van runbooks om taken in AWS te automatiseren.



<!--HONumber=Nov16_HO2-->


