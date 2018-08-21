---
title: Software-updates van Microsoft in Azure Stack-validatie als een service valideren | Microsoft Docs
description: Informatie over het valideren van software-updates van Microsoft met validatie uit als een service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a9e1bb2f134e0e51803266f5e7f94c05ab6966f2
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235405"
---
# <a name="validate-software-updates-from-microsoft"></a>Valideren van software-updates van Microsoft

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft brengt regelmatig updates voor de Azure Stack-software. Deze updates worden aan Azure Stack-partners CO engineering voorafgaand aan openbaar beschikbaar wordt gesteld zodat ze kunnen de updates op basis van hun oplossingen te valideren en feedback naar Microsoft verstrekt.

## <a name="test-an-existing-solution"></a>Een bestaande oplossing testen

1. Aanmelden bij de [validatie portal](https://azurestackvalidation.com).

2. Selecteer een bestaande oplossing waar de bijgewerkte van Microsoft is geïmplementeerd en selecteer **Start** op de **validatie van het pakket** tegel.

    ![Validatie van het pakket](media/image3.png)

3. Voer de naam van de validatie.

4. Voer de URL naar de OEM-pakket dat is geïnstalleerd op de oplossing tijdens de implementatie. Gebruik de URL voor het pakket die zijn opgeslagen op de Azure blob-service. Zie voor meer informatie, [maken van een Azure storage-blob voor het opslaan van logboeken](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

5. Selecteer **uploaden** om toe te voegen uw configuratiebestand voor implementatie. Raadpleeg de [valideren van een nieuwe Azure Stack-oplossing](azure-stack-vaas-validate-solution-new.md) voor informatie over het uploaden van uw configuratiebestand voor implementatie.

6. Het configuratiebestand voor implementatie moet worden aangepast met het parameterbestand voor de juiste omgeving, Zie [omgeving parameters](azure-stack-vaas-parameters.md#environment-parameters) voor meer informatie.

    > [!Note]   
    > Het configuratiebestand voor implementatie kan verder worden aangepast door algemene testparameters toe te voegen. Zie voor meer informatie, [werkstroom algemene parameters voor Azure Stack-validatie als een service](azure-stack-vaas-parameters.md)

7. De gebruikersnaam en het wachtwoord voor de tenant-gebruiker, servicebeheerder en cloud-beheerder moeten handmatig worden ingevoerd.

8. Geef de URL naar de Azure Storage-blob voor het opslaan van de diagnostische logboeken. Zie voor meer informatie, [maken van een Azure storage-blob voor het opslaan van logboeken](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    > [!Note]  
    > Beschrijvende labels kunnen worden ingevoerd als u de naam van de werkstroom.

10. Selecteer **indienen** om op te slaan van de werkstroom.

De werkstroom van de oplossing wordt gedurende ongeveer 24 uur uitgevoerd. Een koppeling naar of instructies over het plannen van de tests toevoegen. Schakel in het hulpprogramma.

Meer informatie over het bewaken van de voortgang van een validatie uitgevoerd vinden, Zie [bewaken van een test ](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over [Azure Stack-validatie als een service](https://docs.microsoft.com/azure/azure-stack/partner).
