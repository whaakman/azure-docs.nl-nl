---
title: Het certificeren van uw VM-installatiekopie voor de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u kunt testen en het verzenden van een VM-installatiekopie voor certificering voor Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/26/2018
ms.author: pbutlerm
ms.openlocfilehash: 69ebe7fde454d2f0e98371406de56f48c9ea97ae
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639688"
---
# <a name="certify-your-vm-image"></a>Uw VM-installatiekopie certificeren

Nadat u maken en implementeren van uw virtuele machine (VM), moet u testen en verzenden van de VM-installatiekopie voor certificering voor Azure Marketplace. Dit artikel wordt uitgelegd waar u de *Test Certificeringshulpprogramma voor Azure Certified*, hoe u dit hulpprogramma gebruiken om te certificeren van uw VM-installatiekopie en over het uploaden van de verificatieresultaten naar de Azure-container waarin uw VHD's zich bevinden. 


## <a name="download-and-run-the-certification-test-tool"></a>Downloaden en uitvoeren van het certificeringshulpprogramma van test

Het Certificeringshulpprogramma van Test voor Azure Certified op een lokale Windows-machine wordt uitgevoerd, maar test een-op basis van Azure Windows of Linux-VM.  Er wordt gecontroleerd of uw gebruiker VM-installatiekopie compatibel met Microsoft Azure is, of aan de richtlijnen en vereisten rondom het voorbereiden van uw VHD is voldaan. De uitvoer van het hulpprogramma is een compatibiliteitsrapport, u naar uploaden zult de [Cloud Partner-Portal](https://cloudpartner.azure.com) om aan te vragen van VM-certificering.

1. Download en installeer de meest recente [Test Certificeringshulpprogramma voor Azure Certified](https://www.microsoft.com/download/details.aspx?id=44299). 
2. Open het certificeringshulpprogramma en klik vervolgens op **nieuwe Test Start**.
3. Uit de **testen informatie** scherm, voert u een **naam Test** voor de test.
4. Selecteer de **Platform** voor uw virtuele machine, hetzij `Windows Server` of `Linux`. Uw keuze van platform is van invloed op de overige opties.
5. Als uw VM maakt gebruik van deze databaseservice, selecteert u de **testen voor Azure SQL Database** selectievakje.

   ![Certificaat test hulpprogramma eerste pagina](./media/publishvm_025.png)


## <a name="connect-the-certification-tool-to-a-vm-image"></a>Het certificeringshulpprogramma verbinden met een VM-installatiekopie

  Het hulpprogramma maakt verbinding met virtuele machines op basis van Windows met [PowerShell](https://docs.microsoft.com/powershell/) en maakt verbinding met virtuele Linux-machines via [SSH.Net](https://www.ssh.com/ssh/protocol/).

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Het certificeringshulpprogramma verbinden met een Linux VM-installatiekopie

1. Selecteer de **SSH-verificatie** modus: `Password Authentication` of `key File Authentication`.
2. Als u gebruikmaakt van verificatie op basis van wachtwoorden, voer waarden in voor de **VM DNS-naam**, **gebruikersnaam**, en **wachtwoord**.  U kunt eventueel de standaardinstelling wijzigen **SSH-poort** getal.

     ![Wachtwoordverificatie van Linux VM-installatiekopie](./media/publishvm_026.png)

3. Als u verificatie met een sleutel op basis van bestanden, voer waarden in voor de **VM DNS-naam**, **gebruikersnaam**, en **privésleutel** locatie.  Desgewenst kunt u opgeven een **wachtwoordzin** of wijzigen van de standaard **SSH-poort** getal.

     ![Verificatie van Linux VM-installatiekopie](./media/publishvm_027.png)

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Het certificeringshulpprogramma verbinden met een VM op basis van een Windows-installatiekopie**
1. Voer de volledig gekwalificeerde **VM DNS-naam** (bijvoorbeeld `MyVMName.Cloudapp.net`).
2. Voer waarden in voor de **gebruikersnaam** en **wachtwoord**.

   ![Wachtwoordverificatie van Windows-VM-installatiekopie](./media/publishvm_028.png)


## <a name="run-a-certification-test"></a>Een CA-test uitvoeren

Nadat u de parameterwaarden hebt opgegeven voor uw VM-installatiekopie in het certificeringshulpprogramma, selecteert u **testverbinding** om te controleren of een geldige verbinding met uw virtuele machine. Nadat een verbinding is gecontroleerd, selecteer **volgende** om de test te starten.  Wanneer de test voltooid is, wordt een tabel weergegeven met de testresultaten (geslaagd/mislukt/waarschuwing).  Het volgende voorbeeld ziet de resultaten van de Linux-VM te testen. 

![De resultaten van certificering voor Linux VM-installatiekopie](./media/publishvm_029.png)

Als een van de test mislukt, wordt uw installatiekopie is *niet* gecertificeerd. In dit geval Raadpleeg de vereisten en foutberichten van de opgegeven wijzigingen aanbrengen en test opnieuw uitvoeren. 

Na de geautomatiseerde test, moet u bieden aanvullende informatie over uw VM-installatiekopie op de **vragenlijst** scherm.  Het bevat twee tabbladen die moeten worden voltooid.  De **algemene beoordeling** tabblad bevat **True/False** vragen, terwijl de **Kernel aanpassing** bevat meerdere selectie en vrije vragen.  Voltooien van de vragen op beide tabbladen en selecteer vervolgens **volgende**.

![Certificering hulpprogramma vragenlijst](./media/publishvm_030.png)

Het laatste scherm kunt u aanvullende informatie, zoals een SSH-toegang tot gegevens van een Linux VM-installatiekopie en een uitleg voor elke mislukte beoordelingen als u op zoek zijn uitzonderingen. 

Klik ten slotte **rapport genereren** te downloaden van de resultaten van de logboekbestanden voor de uitgevoerde testcases verder aan de hand van uw antwoorden op de vragenlijst. De resultaten in dezelfde container als de virtuele harde schijven kunt opslaan.

![Testresultaten voor certificering opslaan](./media/publishvm_031.png)


## <a name="next-steps"></a>Volgende stappen

Vervolgens moet u [een uniform resource-id's (URI) genereren voor elke VHD](./cpp-get-sas-uri.md) dat u verzendt naar de marketplace. 
