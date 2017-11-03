
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Als u wilt opslaan kosten, kunt u onderbreken en hervatten van compute bronnen op de aanvraag. Bijvoorbeeld als u niet de database's nachts en in het weekend gebruikt, kunt u tijdens de tijdstippen onderbreken en hervatten gedurende de dag. U geen afgeschreven voor dwu's terwijl de database is onderbroken.

Wanneer u een database onderbreken:

* Reken-en geheugenbronnen worden geretourneerd aan de groep met beschikbare resources in het datacenter
* DWU-kosten zijn voor de duur van de pauze nul.
* Opslag van gegevens wordt niet beïnvloed en uw gegevens intact blijven. 
* SQL Data Warehouse annuleert alle bewerkingen voor actieve of in de wachtrij.

