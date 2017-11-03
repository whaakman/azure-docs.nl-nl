---
title: Aan de slag met Azure storage en Visual Studio verbonden services (webtaak projecten)
description: Hoe u aan de slag met Azure Table storage in een Azure WebJobs-project in Visual Studio nadat u verbinding met een opslagaccount met Visual Studio hebt verbonden services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4e0c77e08bff971277a09d6066f259db84617616
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Aan de slag met Azure Storage (Azure webtaak projecten)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht
Dit artikel vindt u C#-codevoorbeelden die laten zien hoe de Azure WebJobs SDK-versie te gebruiken 1.x met de Azure table storage-service. De code voorbeelden gebruiken de [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) versie 1.x.

De service Azure Table storage kunt u voor het opslaan van grote hoeveelheden gestructureerde gegevens. De service is een NoSQL-gegevensarchief die geverifieerde aanroepen vanuit binnen en buiten de Azure-cloud accepteert. Azure-tabellen zijn ideaal voor het opslaan van gestructureerde, niet-relationele gegevens.  Zie [aan de slag met Azure Table storage met .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) voor meer informatie.

Sommige van de code codefragmenten weergeven de **tabel** kenmerk wordt gebruikt in de functies die worden genoemd handmatig, dat wil zeggen, niet met behulp van een van de trigger-kenmerken.

## <a name="how-to-add-entities-to-a-table"></a>Entiteiten toevoegen aan een tabel
U kunt entiteiten toevoegen aan een tabel met de **tabel** kenmerk met een **ICollector<T>**  of **IAsyncCollector<T>**  parameter waar **T** Hiermee geeft u het schema van de entiteiten die u wilt toevoegen. De kenmerkconstructor heeft een tekenreeksparameter gebruikt met de naam van de tabel.

Het volgende codevoorbeeld wordt toegevoegd **persoon** entiteiten aan een tabel met de naam *inkomend*.

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

Meestal het type u gebruiken met **ICollector** is afgeleid van **TableEntity** of implementeert **ITableEntity**, maar heeft geen aan. Een van de volgende **persoon** werk klassen met de code die wordt weergegeven in de voorgaande **inkomend** methode.

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

Als u samenwerken met de Azure storage-API wilt, kunt u toevoegen een **CloudStorageAccount** -parameter voor de methodehandtekening.

## <a name="real-time-monitoring"></a>Realtime-controle
Omdat ingress-functies te vaak grote hoeveelheden gegevens verwerken, kunt het dashboard WebJobs SDK u realtime controlegegevens. De **aanroep logboek** gedeelte wordt uitgelegd of de functie nog steeds wordt uitgevoerd.

![Inkomend functie uitgevoerd](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

De **aanroep Details** pagina rapporten van de functie voortgang (aantal entiteiten dat is geschreven) wanneer deze actief is en hebt u de mogelijkheid om af te breken deze.

![Inkomend functie uitgevoerd](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

Wanneer de functie is voltooid, de **aanroep Details** pagina rapporteert het aantal rijen dat is geschreven.

![Inkomend functie is voltooid](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a>Het lezen van meerdere entiteiten uit een tabel
Als u wilt lezen van een tabel, gebruikt u de **tabel** kenmerk met een **IQueryable<T>**  parameter waarin typen **T** is afgeleid van **TableEntity**of implementeert **ITableEntity**.

Het volgende codevoorbeeld leest en registreert alle rijen uit de **inkomend** tabel:

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
Er is een **tabel** kenmerkconstructor met twee aanvullende parameters u de partitiesleutel en de rijsleutel opgeven kunt wanneer u wilt binden aan een met één Tabelentiteit.

Het volgende codevoorbeeld leest de rij in een tabel voor een **persoon** entiteit op basis van partitie sleutel- en sleutelwaarden in een wachtrijbericht ontvangen:  

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

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a>Het gebruik van de API van de .NET-opslag rechtstreeks aan het werken met een tabel
U kunt ook de **tabel** kenmerk met een **CloudTable** -object voor meer flexibiliteit bij het werken met een tabel.

De volgende code voorbeeld wordt een **CloudTable** object toevoegen aan één entiteit de *inkomend* tabel.

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

## <a name="related-topics-covered-by-the-queues-how-to-article"></a>Verwante onderwerpen gedekt door het wachtrijen how-to artikel
Zie voor meer informatie over het afhandelen van de tabel verwerking geactiveerd door een wachtrijbericht of voor WebJobs SDK scenario's die niet specifiek zijn voor de verwerking van de tabel [aan de slag met Azure Queue storage en Visual Studio verbonden services (webtaak projecten) ](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel is opgegeven codevoorbeelden die laten hoe u algemene scenario's zien voor het werken met Azure-tabellen te verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

