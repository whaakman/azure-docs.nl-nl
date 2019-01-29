---
title: Bewerkingen uitvoeren op Azure Queue storage met PowerShell | Microsoft Docs
description: Het uitvoeren van bewerkingen op Azure Queue storage met PowerShell
services: storage
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/14/2017
ms.author: rogarana
ms.component: queues
ms.openlocfilehash: b46aea2a50b12f82858b8f465f8dfbfff8aa0bbe
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165010"
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Azure Queue storage bewerkingen uitvoeren met Azure PowerShell

Azure Queue storage is een service voor het opslaan van grote aantallen berichten die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. Zie voor gedetailleerde informatie [Inleiding tot Azure Queues](storage-queues-introduction.md). In dit artikel bevat informatie over algemene wachtrij-opslagbewerkingen. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een wachtrij maken
> * Ophalen van een wachtrij
> * Een bericht toevoegen
> * Een bericht lezen
> * Een bericht verwijderen 
> * Een wachtrij verwijderen

Deze procedures voor vereist de Azure PowerShell-module Az 0,7 of hoger. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps).

Er zijn geen PowerShell-cmdlets voor de gegevenslaag voor wachtrijen. Een bericht om uit te voeren gegevens vlak bewerkingen, zoals een bericht toevoegen lezen en verwijderen van een bericht, u moet de storage-clientbibliotheek voor .NET gebruiken zoals deze wordt weergegeven in PowerShell. Maakt u een berichtobject en klikt u vervolgens kunt u opdrachten, zoals AddMessage bewerkingen van dat bericht uit te voeren. Dit artikel ziet u hoe u dat doet.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzAccount` en volg de instructies op het scherm.

```powershell
Connect-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Lijst met locaties ophalen

Als u niet weet welke locatie u kunt gebruiken, kunt u een lijst met de beschikbare locaties weergeven. Selecteer de gewenste locatie in de lijst. In deze oefening gebruikt **eastus**. Dit in de variabele Store **locatie** voor toekomstig gebruik.

```powershell
Get-AzLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) opdracht. 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Naam van de resourcegroep Store in een variabele voor toekomstig gebruik. In dit voorbeeld wordt een resourcegroep met de naam *howtoqueuesrg* wordt gemaakt in de *eastus* regio.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Een opslagaccount maken

Een standaardopslagaccount voor algemeen gebruik te maken met het gebruik van lokaal redundante opslag (LRS) [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Krijg de opslagaccountcontext op waarin het opslagaccount dat moet worden gebruikt. Als u werkt met een opslagaccount, verwijst u naar de context in plaats van herhaaldelijk de referenties op te geven.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Een wachtrij maken

Het volgende voorbeeld maakt eerst een verbinding met Azure Storage met behulp van de context van het storage, waaronder de naam van het opslagaccount en de toegangssleutel. Vervolgens wordt de [New-AzStorageQueue](/powershell/module/az.storage/New-AzStorageQueue) cmdlet voor het maken van een wachtrij met de naam 'queuename'.

```powershell
$queueName = "howtoqueue"
$queue = New-AzStorageQueue –Name $queueName -Context $ctx
```

Zie voor informatie over naamgevingsregels voor Azure Queue-Service, [naamgeving van wachtrijen en metagegevens](https://msdn.microsoft.com/library/azure/dd179349.aspx).

## <a name="retrieve-a-queue"></a>Ophalen van een wachtrij

U kunt query's uitvoeren en ophalen van een bepaalde wachtrij of een lijst met alle wachtrijen in een Storage-account. De volgende voorbeelden laten zien hoe u om op te halen van alle wachtrijen in de storage-account en een bepaalde wachtrij; beide opdrachten gebruiken de [Get-AzStorageQueue](/powershell/module/az.storage/Get-AzStorageQueue) cmdlet.

```powershell
# Retrieve a specific queue
$queue = Get-AzStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzStorageQueue -Context $ctx | select Name
```

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Bewerkingen die invloed hebben op de werkelijke berichten in de wachtrij gebruiken de .NET-opslagclientbibliotheek zoals weergegeven in PowerShell. Een bericht toevoegen aan een wachtrij, maak een nieuw exemplaar van het berichtobject [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](https://msdn.microsoft.com/library/azure/jj732474.aspx) klasse. Daarna roept u de methode [AddMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) aan. Een CloudQueueMessage kan worden gemaakt op basis van een tekenreeks (in UTF-8-indeling) of een bytematrix.

Het volgende voorbeeld ziet u hoe u een bericht toevoegen aan de wachtrij.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 1"
# Add a new message to the queue
$queue.CloudQueue.AddMessageAsync($QueueMessage)

# Add two more messages to the queue 
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 2"
$queue.CloudQueue.AddMessageAsync($QueueMessage)
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 3"
$queue.CloudQueue.AddMessageAsync($QueueMessage)
```

Als u de [Azure Storage Explorer](http://storageexplorer.com), u kunt verbinding maken met uw Azure-account en de wachtrijen weergeven in de storage-account en Zoom in op een wachtrij om de berichten in de wachtrij weer te geven. 

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Lees een bericht uit de wachtrij verwijderen

Berichten worden gelezen in volgorde van de beste probeer first in first out. Dit kan niet worden gegarandeerd. Wanneer u het bericht uit de wachtrij leest, wordt het onzichtbaar voor alle andere processen kijken naar de wachtrij. Dit zorgt ervoor dat als uw code niet kan verwerken van het bericht vanwege fout bij de hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht en probeer het opnieuw.  

Dit **de time-out voor onzichtbaarheid** wordt gedefinieerd hoe lang het bericht onzichtbaar blijft voordat deze beschikbaar zijn voor de verwerking van het opnieuw. De standaardwaarde is 30 seconden. 

Uw code leest een bericht uit de wachtrij in twee stappen. Wanneer u aanroepen de [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) methode, krijgt u het volgende bericht in de wachtrij. Een bericht dat wordt geretourneerd door **GetMessage**, wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Als u klaar bent met het bericht verwijderen uit de wachtrij, wilt u aanroepen de [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) methode. 

In het volgende voorbeeld wordt u via de drie Wachtrijberichten lezen en vervolgens wacht 10 seconden (de time-out voor onzichtbaarheid). En u opnieuw de drie berichten de berichten worden verwijderd lezen na het lezen van deze door aan te roepen **DeleteMessage**. Als u probeert te lezen van de wachtrij de berichten zijn verwijderd, wordt u $queueMessage als NULL geretourneerd.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result

# After 10 seconds, these messages reappear on the queue. 
# Read them again, but delete each one after reading it.
# Delete the message.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
```

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Als u wilt verwijderen van een wachtrij en alle berichten die erin zijn opgenomen, roept u de cmdlet Remove-AzStorageQueue. Het volgende voorbeeld ziet hoe u de specifieke wachtrij die wordt gebruikt in deze oefening met de cmdlet Remove-AzStorageQueue verwijderen.

```powershell
# Delete the queue 
Remove-AzStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt verwijderen van alle activa die u in deze oefening hebt gemaakt, moet u de resourcegroep verwijderd. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijderd het de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, hebt u geleerd over eenvoudige wachtrij-opslagbeheer met PowerShell, onder andere:

> [!div class="checklist"]
> * Een wachtrij maken
> * Ophalen van een wachtrij
> * Een bericht toevoegen
> * Lees het volgende bericht
> * Een bericht verwijderen 
> * Een wachtrij verwijderen

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell Storage-cmdlets

* [PowerShell Storage-cmdlets](/powershell/module/az.storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer

* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
