---
title: Verificatie met Amazon webservices configureren
description: In dit artikel wordt beschreven hoe u een AWS-referentie voor runbooks in Azure Automation maakt en valideert bij het beheren van AWS-resources.
keywords: AWS-verificatie, AWS configureren
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: f8ac816e03df9bb41207a0463d3fa8aa58754943
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks met Amazon Web Services verifiëren

Het automatiseren van algemene taken met resources in Amazon Web Services (AWS) kan worden bewerkstelligd met Automation-runbooks in Azure. U kunt vele taken in AWS automatiseren met Automation-runbooks, net zoals u dit kunt met resources in Azure. U hebt slechts twee dingen nodig:

* Een AWS-abonnement en een set referenties. Het gaat dan met name om uw AWS-toegangssleutel en de geheime sleutel. Raadpleeg het artikel [AWS-referenties gebruiken](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) voor meer informatie.
* Een Azure-abonnement en een Automation-account.

Als u wilt verifiëren met AWS, moet u een set AWS-referenties opgeven om uw runbooks te verifiëren die vanuit Azure Automation worden uitgevoerd. Als u al een Automation-account gemaakt hebt en u wilt gebruiken om te verifiëren met AWS, kunt u de stappen in de volgende sectie: als u reserveren van een account voor runbooks die gericht is op AWS-resources wilt, moet eerst maakt u een nieuw [ Automation-account](automation-offering-get-started.md) (Sla de optie voor het maken van een service-principal) en gebruik de volgende stappen:

## <a name="configure-automation-account"></a>Automation-account configureren

Voor communicatie tussen Azure Automation en AWS moet u eerst uw AWS-referenties ophalen en deze opslaan als assets in Azure Automation. Voer de volgende stappen uit die in het AWS-document [Toegangssleutels voor uw AWS-account beheren](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) zijn gedocumenteerd om een toegangssleutel te maken en kopieer de **toegangssleutel-id** en **geheime toegangssleutel** (download desgewenst uw sleutelbestand om het ergens veilig op te slaan).

Nadat u uw AWS-beveiligingssleutels hebt gemaakt en gekopieerd, moet u een referentieasset met een Azure Automation-account maken om deze sleutels veilig op te slaan en ernaar te verwijzen met uw runbooks. Volg de stappen in de sectie: **een nieuwe referentie maken** in de [Referentieassets in Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) artikel en voer de volgende informatie:

1. Voer in het vak **Naam** **AWScred** in of een geschikte waarde die voldoet aan uw naamgevingsstandaarden.
2. Typ uw **toegangs-id** in het vak **Gebruikersnaam** en uw **geheime toegangssleutel** in de vakken **Wachtwoord** en **Wachtwoord bevestigen**.

## <a name="next-steps"></a>Volgende stappen

* Lees het oplossingsartikel [Implementatie van een virtuele machine in Amazon Web Services automatiseren](automation-scenario-aws-deployment.md) voor informatie over het maken van runbooks om taken in AWS te automatiseren.