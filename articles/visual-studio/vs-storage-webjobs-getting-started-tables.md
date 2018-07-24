---
title: Aan de slag met Azure storage en Visual Studio verbonden services (webtaak projecten)
description: Aan de slag met Azure Table storage in een Azure WebJobs-project in Visual Studio nadat u verbinding met een opslagaccount met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: dd1d6ca6d43a7c1054b822336d23b4b6be392788
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212973"
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Aan de slag met Azure Storage (Azure-webtaak projecten)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
In dit artikel vindt u C#-codevoorbeelden die laten zien hoe de Azure WebJobs SDK-versie 1.x met de Azure table storage-service. De code-voorbeelden gebruiken de [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) versie 1.x.

De Azure Table storage-service kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen uit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.  Zie [aan de slag met Azure Table storage met .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) voor meer informatie.

Enkele van de codefragmenten ziet de **tabel** kenmerk dat wordt gebruikt in de functies die worden aangeroepen handmatig, dat wil zeggen, niet met behulp van een van de kenmerken van de trigger.

## <a name="how-to-add-entities-to-a-table"></a>Entiteiten toevoegen aan een tabel
U kunt entiteiten toevoegen aan een tabel met de **tabel** kenmerk met een **ICollector<T>**  of **IAsyncCollector<T>**  parameter waar **T** Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. De constructor kenmerk heeft een queryreeks-parameter met de naam van de tabel.

Het volgende voorbeeld voegt **persoon** entiteiten aan een tabel met de naam *inkomend*.

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() {
                        PartitionKey = "Test",
                        RowKey = i.ToString(),
                        Name = "Name" }
                    );
            }
        }

Doorgaans het type u gebruikt met **ICollector** is afgeleid van **TableEntity** of implementeert **ITableEntity**, maar dit hoeft te. Een van de volgende **persoon** klassen werken met de code die wordt weergegeven in de voorgaande **inkomend** methode.

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Als u werken rechtstreeks met de Azure storage-API wilt, kunt u toevoegen een **CloudStorageAccount** parameter aan de methodehandtekening.

## <a name="real-time-monitoring"></a>Realtime-controle
Omdat ingress-functies worden vaak grote hoeveelheden gegevens verwerken, biedt het WebJobs-SDK-dashboard realtime controlegegevens. De **Aanroeplogbestand** sectie geeft aan of de functie nog steeds wordt uitgevoerd.

![Inkomend verkeer functie uitgevoerd](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

De **Aanroepdetails** pagina rapporten van de functie wordt uitgevoerd (aantal entiteiten die zijn geschreven) terwijl deze wordt uitgevoerd en hebt u de mogelijkheid om af te breken deze.

![Inkomend verkeer functie uitgevoerd](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

Wanneer de functie is voltooid, de **Aanroepdetails** pagina rapporteert het aantal rijen dat is geschreven.

![Inkomend verkeer functie voltooid](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a>Het lezen van meerdere entiteiten uit een tabel
Als u wilt lezen van een tabel, gebruikt u de **tabel** kenmerk met een **IQueryable<T>**  parameter waar typen **T** is afgeleid van **TableEntity**of implementeert **ITableEntity**.

Het volgende codevoorbeeld leest en logboeken van alle rijen uit de **inkomend** tabel:

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}",
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a name="how-to-read-a-single-entity-from-a-table"></a>Het lezen van één entiteit uit een tabel
Er is een **tabel** kenmerkconstructie met twee extra parameters die u de partitiesleutel en rijsleutel opgeven kunt als u wilt koppelen aan een entiteit één tabel.

Het volgende codevoorbeeld wordt een rij in een tabel voor een **persoon** entiteit op basis van de partitie en recordsleutelwaarden sleutelwaarden die zijn ontvangen in een wachtrijbericht:  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


De **persoon** klasse in dit voorbeeld heeft geen voor het implementeren van **ITableEntity**.

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a>Het gebruik van de Storage .NET API rechtstreeks aan het werken met een tabel
U kunt ook de **tabel** kenmerk met een **CloudTable** -object voor meer flexibiliteit bij het werken met een tabel.

De volgende code voorbeeld gebruikt een **CloudTable** object toe te voegen aan één entiteit de *inkomend* tabel.

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

Voor meer informatie over het gebruik van de **CloudTable** object, Zie [aan de slag met Azure Table storage met .NET](../storage/storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-the-queues-how-to-article"></a>Verwante onderwerpen wordt gedekt door het artikel met instructies wachtrijen
Zie voor informatie over het afhandelen van de tabel verwerking geactiveerd door een wachtrijbericht of voor WebJobs-SDK's die niet specifiek zijn voor de verwerking van de tabel [aan de slag met Azure Queue storage en Visual Studio verbonden services (webtaak projecten) ](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel is codevoorbeelden die laten hoe u algemene scenario's zien voor het werken met Azure-tabellen worden opgegeven. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen voor Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

