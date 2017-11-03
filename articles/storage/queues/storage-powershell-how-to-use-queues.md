---
title: Bewerkingen uitvoeren op Azure Queue storage met PowerShell | Microsoft Docs
description: Het uitvoeren van bewerkingen op Azure Queue storage met PowerShell
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 36eb6db83bb902b35efb8c9666ab06e0c618d602
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Azure Queue storage bewerkingen uitvoeren met Azure PowerShell

Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen. Zie voor gedetailleerde informatie [Inleiding tot Azure wachtrijen](storage-queues-introduction.md). Dit artikel bevat informatie over algemene wachtrij-opslagbewerkingen. Procedures voor:

> [!div class="checklist"]
> * Een wachtrij maken
> * Een wachtrij ophalen
> * Een bericht toevoegen
> * Een bericht gelezen
> * Een bericht verwijderen 
> * Een wachtrij verwijderen

Deze procedures voor vereist de Azure PowerShell-moduleversie 3,6 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Er zijn geen PowerShell-cmdlets voor het vlak van gegevens voor wachtrijen. Een bericht om uit te voeren van gegevens toevoegen vlak bewerkingen, zoals een bericht lezen en verwijderen van een bericht, met het gebruik van de storage-clientbibliotheek voor .NET, zoals deze wordt weergegeven in PowerShell. U maakt een message-object en klikt u vervolgens kunt u opdrachten zoals AddMessage bewerkingen uitvoeren op het bericht. Dit artikel laat zien hoe u dat doet.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Lijst met locaties ophalen

Als u niet welke locatie die u wilt gebruiken weet, kunt u de beschikbare locaties weergeven. Nadat de lijst wordt weergegeven, vinden die u wilt gebruiken. In deze oefening gebruikt **eastus**. Slaat u dit in de variabele **locatie** voor toekomstig gebruik.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Naam van de resourcegroep opgeslagen in een variabele voor toekomstig gebruik. In dit voorbeeld wordt een resourcegroep met de naam *howtoqueuesrg* wordt gemaakt in de *eastus* regio.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Storage-account maken

Maken van een standaard algemeen opslagaccount met lokaal redundante opslag (LRS) met behulp van [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Haal de context van het opslagaccount waarin de storage-account moet worden gebruikt. Wanneer optreedt op een storage-account, maar u verwijzen naar de context in plaats van herhaaldelijk geven de referenties.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Een wachtrij maken

Het volgende voorbeeld maakt eerst een verbinding met Azure Storage met context van het opslagaccount, waaronder de opslagaccountnaam en de toegangssleutel. Vervolgens roept [nieuw AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet voor het maken van een wachtrij met de naam 'wachtrijnaam'.

```powershell
$queueName = "howtoqueue"
$queue = New-AzureStorageQueue –Name $queueName -Context $ctx
```

Zie voor informatie over naamgevingsregels voor Azure Queue-Service, [naamgeving van wachtrijen en metagegevens](http://msdn.microsoft.com/library/azure/dd179349.aspx).

## <a name="retrieve-a-queue"></a>Een wachtrij ophalen

U kunt een query en ophalen van een specifieke wachtrij of een lijst met alle wachtrijen in een opslagaccount. De volgende voorbeelden laten zien hoe u voor het ophalen van alle wachtrijen in het opslagaccount en een specifieke wachtrij; beide opdrachten gebruiken de [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.

```powershell
# Retrieve a specific queue
$queue = Get-AzureStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzureStorageQueue -Context $ctx | select Name
```

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Bewerkingen die invloed van de werkelijke berichten in de wachtrij gebruiken de storage-clientbibliotheek voor .NET, zoals weergegeven in PowerShell. Als u wilt een bericht toevoegen aan een wachtrij, maak een nieuw exemplaar van het berichtobject [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klasse. Daarna roept u de methode [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) aan. Een CloudQueueMessage kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix.

Het volgende voorbeeld laat zien hoe u een bericht toevoegt aan de wachtrij.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 1"
# Add a new message to the queue
$queue.CloudQueue.AddMessage($QueueMessage)

# Add two more messages to the queue 
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 2"
$queue.CloudQueue.AddMessage($QueueMessage)
$queueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage `
  -ArgumentList "This is message 3"
$queue.CloudQueue.AddMessage($QueueMessage)
```

Als u de [Azure Opslagverkenner](http://storageexplorer.com), u kunt verbinding maken met uw Azure-account en de wachtrijen weergeven in het opslagaccount en inzoomen op een wachtrij de berichten in de wachtrij weergeven. 

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Lees een bericht uit de wachtrij verwijderen

Berichten worden gelezen in volgorde van de beste probeer first-in-first-out. Dit kan niet worden gegarandeerd. Wanneer u het bericht uit de wachtrij gelezen, wordt het onzichtbaar voor alle andere processen kijken naar de wachtrij. Dit zorgt ervoor dat als de code niet kan verwerken van het bericht vanwege problemen met hardware of software, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw.  

Dit **time-out voor onzichtbaarheid** definieert hoe lang het bericht onzichtbaar blijft voordat deze beschikbaar zijn voor de verwerking opnieuw. De standaardwaarde is 30 seconden. 

Uw code wordt een bericht gelezen uit de wachtrij in twee stappen. Als u aanroept de [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) methode, krijgt u het volgende bericht in de wachtrij. Een bericht dat wordt geretourneerd door **GetMessage**, wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Voor het voltooien van het bericht uit de wachtrij te verwijderen, belt u het [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) methode. 

In het volgende voorbeeld wordt u via de drie Wachtrijberichten lezen en vervolgens wacht tien seconden (time-out voor onzichtbaarheid). Vervolgens u opnieuw de drie berichten verwijderen van de berichten lezen na het lezen van deze door aan te roepen **DeleteMessage**. Als u probeert te lezen van de wachtrij de berichten zijn verwijderd, wordt $queueMessage geretourneerd als NULL.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queueMessage 

# After 10 seconds, these messages reappear on the queue. 
# Read them again, but delete each one after reading it.
# Delete the message.
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
$ queueMessage = $queue.CloudQueue.GetMessage($invisibleTimeout)
$ queue.CloudQueue.DeleteMessage($queueMessage)
```

## <a name="delete-a-queue"></a>Een wachtrij verwijderen
Roep de cmdlet Remove-AzureStorageQueue voor het verwijderen van een wachtrij en alle berichten hierin. Het volgende voorbeeld laat zien hoe de specifieke wachtrij gebruikt in deze oefening met de cmdlet Remove-AzureStorageQueue verwijderen.

```powershell
# Delete the queue 
Remove-AzureStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Resources opschonen

Om alle te verwijderen van de activa die u in deze oefening hebt gemaakt, verwijder de resourcegroep. Hiermee verwijdert u ook alle resources binnen de groep. In dit geval verwijdert deze de storage-account gemaakt en de resourcegroep zelf.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel how-to, hebt u geleerd over basic wachtrij opslagbeheer met PowerShell, inclusief hoe:

> [!div class="checklist"]
> * Een wachtrij maken
> * Een wachtrij ophalen
> * Een bericht toevoegen
> * Het volgende bericht gelezen
> * Een bericht verwijderen 
> * Een wachtrij verwijderen

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Opslag voor Microsoft Azure PowerShell-cmdlets
* [PowerShell-cmdlets voor opslag](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.