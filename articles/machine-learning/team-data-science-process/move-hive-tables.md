---
title: Erstellen von Hive-Tabellen und Laden von Daten aus Azure Blob Storage| Microsoft-Dokumentation
description: Erstellen von Hive-Tabellen und Laden von Daten aus Blobs in Hive-Tabellen
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 593df249429bf1dcc5a59312830ed78f7cf642e8
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Erstellen von Hive-Tabellen und Laden von Daten aus Azure Blob Storage
In diesem Thema werden allgemeine Hive-Abfragen beschrieben, mit denen Hive-Tabellen erstellt und Daten aus Azure Blob Storage geladen werden. Es werden auch einige Hinweise zur Partitionierung der Hive-Tabellen und zur Verwendung des ORC-Formats (Optimized Row Columnar) zur Verbesserung der Abfrageleistung bereitgestellt.

Dieses **Menü** bietet Links zu Themen, in denen beschrieben wird, wie Daten in Zielumgebungen erfasst werden, in denen die Daten während des Team Data Science-Prozesses (TDSP) gespeichert und verarbeitet werden.

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie Folgendes abgeschlossen haben:

* Sie haben ein Azure-Speicherkonto erstellt. Anweisungen, die Sie ggf. benötigen, finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/common/storage-create-storage-account.md).
* Sie haben einen angepassten Hadoop-Cluster mit dem HDInsight-Dienst bereitgestellt.  Anweisungen dazu finden Sie unter [Anpassen von Azure HDInsight Hadoop-Clustern für die erweiterte Analyse](customize-hadoop-cluster.md).
* Sie haben den Remotezugriff auf den Cluster aktiviert, sich angemeldet und die Hadoop-Befehlszeilenkonsole geöffnet. Anweisungen finden Sie unter [Zugreifen auf den Hauptknoten von Hadoop-Clustern](customize-hadoop-cluster.md).

## <a name="upload-data-to-azure-blob-storage"></a>Hochladen von Daten in Azure Blob Storage
Wenn Sie einen virtuellen Azure-Computer mithilfe der Anweisungen in [Einrichten eines virtuellen Azure-Computers für die erweiterte Analyse](../data-science-virtual-machine/setup-virtual-machine.md) erstellt haben, wurde diese Skriptdatei bereits in das Verzeichnis *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* auf dem virtuellen Computer heruntergeladen. Sie müssen Ihr eigenes Datenschema implementieren und die Konfiguration von Azure Blob Storage in den entsprechenden Feldern dieser Abfragen vornehmen. Danach sollten diese Hive-Abfragen für die Übermittlung bereit sein.

Es wird davon ausgegangen, dass die Daten für die Hive-Tabellen in einem **unkomprimierten** Tabellenformat vorliegen und dass die Daten in den Standardcontainer (oder einen zusätzlichen Container) des Speicherkontos hochgeladen wurden, das vom Hadoop-Cluster verwendet wird.

Wenn Sie mit den **NYC Taxi Trip-Daten**üben möchten, gehen Sie so vor:

* **Laden** Sie die 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) -Dateien (12 Fahrtendateien und 12 Fahrpreisdateien) herunter.
* **Entpacken** Sie alle Dateien als CSV-Dateien
* **Laden** Sie diese Dateien in den Standardcontainer (oder einen anderen geeigneten Container) im Azure-Speicherkonto, das im Thema [Anpassen von Azure HDInsight Hadoop-Clustern für erweiterte Analyseprozesse und -technologien](customize-hadoop-cluster.md) erstellt wurde. Den Prozess zum Hochladen der CSV-Dateien in den Standardcontainer für das Speicherkonto finden Sie auf dieser [Seite](hive-walkthrough.md#upload).

## <a name="submit"></a>Übermitteln von Hive-Abfragen
Hive-Abfragen können folgendermaßen übermittelt werden:

1. [Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile im Hauptknoten des Hadoop-Clusters](#headnode)
2. [Übermitteln von Hive-Abfragen mit dem Hive-Editor](#hive-editor)
3. [Übermitteln von Hive-Abfragen mit Azure PowerShell-Befehlen](#ps)

Hive-Abfragen sind ähnlich wie SQL-Abfragen. Als mit SQL vertrauter Benutzer finden Sie möglicherweise die Informationen im [Cheat Sheet „Hive for SQL Users“](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) (Spickzettel zu Hive für SQL-Benutzer) nützlich.

Beim Übermitteln von Hive-Abfragen können Sie auch das Ziel der Ausgabe der Hive-Abfragen steuern. Diese kann auf den Bildschirm, in eine lokale Datei auf dem Hauptknoten oder in ein Azure-Blob erfolgen.

### <a name="headnode"></a> 1. Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile im Hauptknoten des Hadoop-Clusters
Wenn die Hive-Abfrage komplex ist, führt die direkte Übermittlung über den Hauptknoten des Hadoop-Clusters i. d. R. schneller zu Ergebnissen als das Senden im Hive-Editor oder über Azure PowerShell-Skripts.

Melden Sie sich am Hauptknoten des Hadoop-Clusters an, öffnen Sie die Hadoop-Befehlszeile auf dem Desktop des Hauptknotens und geben Sie den Befehl `cd %hive_home%\bin` ein.

Sie haben drei Möglichkeiten zum Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile:

* direkt
* mithilfe von HQL-Dateien
* über die Hive-Befehlskonsole

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Übermitteln der Hive-Abfragen direkt an der Hadoop-Befehlszeile
Sie können einen Befehl wie `hive -e "<your hive query>;` ausführen, um einfache Hive-Abfragen direkt an der Hadoop-Befehlszeile zu übermitteln. Hier ist ein Beispiel. Das rote Kästchen enthält den Befehl, der die Hive-Abfrage übermittelt, und das grüne Kästchen umschließt die Ausgabe der Hive-Abfrage.

![Arbeitsbereich erstellen](./media/move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Übermitteln der Hive-Abfragen in HQL-Dateien
Wenn die Hive-Abfrage sehr komplex ist und aus mehreren Zeilen besteht, ist das direkte Bearbeiten der Abfragen an der Befehlszeile oder an der Hive-Konsole nicht sehr praktikabel. Alternativ können die Hive-Abfragen mit einem Text-Editor auf dem Hauptknoten des Hadoop-Clusters in einer HQL-Datei in einem lokalen Verzeichnis auf dem Hauptknoten gespeichert werden. Anschließend kann die Hive-Abfrage in der HQL-Datei mithilfe des `-f` -Arguments wie folgt übermittelt werden:

    hive -f "<path to the .hql file>"

![Arbeitsbereich erstellen](./media/move-hive-tables/run-hive-queries-3.png)

**Unterdrücken der Fortschrittsanzeige bei Hive-Abfragen**

Standardmäßig wird bei Hive-Abfragen, die über die Hadoop-Befehlszeile übermittelt werden, der Fortschritt des Map/Reduce-Auftrags auf dem Bildschirm angezeigt. Um die Fortschrittsanzeige des Map/Reduce-Auftrags zu unterdrücken, können Sie das Argument `-S` ("S" als Großbuchstabe) wie folgt an der Befehlszeile verwenden:

    hive -S -f "<path to the .hql file>"
zu erstellen und zu verwalten.    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Übermitteln von Hive-Abfragen an der Hive-Befehlskonsole
Sie können auch zunächst die Hive-Befehlskonsole starten, indem Sie den Befehl `hive` an der Hadoop-Befehlszeile eingeben und dann die Hive-Abfragen über die Hive-Befehlskonsole übermitteln. Beispiel: In diesem Beispiel kennzeichnen die beiden roten Kästchen die Befehle, die zum Starten der Hive-Befehlskonsole verwendet werden, und die Hive-Abfrage, die über die Hive-Befehlskonsole übermittelt wird. Das grüne Kästchen markiert die Ausgabe der Hive-Abfrage.

![Arbeitsbereich erstellen](./media/move-hive-tables/run-hive-queries-2.png)

In den vorherigen Beispielen wurden die Ergebnisse der Hive-Abfrage direkt auf den Bildschirm ausgeben. Sie können die Ausgabe aber auch in eine lokale Datei auf dem Hauptknoten oder in ein Azure-Blob schreiben. Sie können dann andere Tools verwenden, um die Ausgabe der Hive-Abfragen weiter zu analysieren.

**Ausgeben der Hive-Abfrageergebnisse in eine lokale Datei**
Für die Ausgabe der Hive-Abfrageergebnisse in ein lokales Verzeichnis auf dem Hauptknoten müssen Sie die Hive-Abfrage an der Hadoop-Befehlszeile wie folgt übermitteln:

    hive -e "<hive query>" > <local path in the head node>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage in die Datei `hivequeryoutput.txt` im Verzeichnis `C:\apps\temp` geschrieben.

![Arbeitsbereich erstellen](./media/move-hive-tables/output-hive-results-1.png)

**Ausgeben der Hive-Abfrageergebnisse in ein Azure-Blob**

Sie können die Hive-Abfrageergebnisse auch in ein Azure-Blob im Standardcontainer des Hadoop-Clusters ausgeben. Die Hive-Abfrage hierfür sieht folgendermaßen aus:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage in das Blob-Verzeichnis `queryoutputdir` innerhalb des Standardcontainer des Hadoop-Cluster geschrieben. Hier müssen Sie nur den Namen des Verzeichnisses ohne den Namen des Blobs angeben. Es wird ein Fehler ausgelöst, wenn Sie sowohl den Verzeichnis- als auch den Blobnamen angeben, wie z. B. `wasb:///queryoutputdir/queryoutput.txt`.

![Arbeitsbereich erstellen](./media/move-hive-tables/output-hive-results-2.png)

Wenn Sie den Standardcontainer des Hadoop-Clusters mit Tools wie Azure Storage-Explorer öffnen, wird die Ausgabe der Hive-Abfrage wie in der folgenden Abbildung angezeigt. Sie können Filter (markiert durch den roten Kasten) anwenden, um nur das Blob mit den angegebenen Buchstaben im Namen abzurufen.

![Arbeitsbereich erstellen](./media/move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. Übermitteln von Hive-Abfragen mit dem Hive-Editor
Sie können auch die Abfragekonsole (Hive-Editor) verwenden, indem Sie in einem Webbrowser eine URL im Format *https://<Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* eingeben. Sie müssen angemeldet sein, um diese Konsole anzeigen zu können, weshalb hier Ihre Hadoop-Clusteranmeldeinformationen erforderlich sind.

### <a name="ps"></a> 3. Übermitteln von Hive-Abfragen mit Azure PowerShell-Befehlen
Sie können zum Übermitteln von Hive-Abfragen auch PowerShell verwenden. Eine Anleitung hierzu finden Sie unter [Übermitteln von Hive-Aufträgen mit PowerShell](../../hdinsight/hadoop/apache-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Erstellen von Hive-Datenbanken und -Tabellen
Die Hive-Abfragen wurden im [GitHub-Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) freigegeben und können von dort heruntergeladen werden.

Mit dieser Hive-Abfrage erstellen Sie eine Hive-Tabelle.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Im Folgenden werden die Felder beschrieben, mit denen Sie die Implementierung und andere Konfigurationen vornehmen:

* **<database name>**: Der Name der zu erstellenden Datenbank. Wenn Sie die Standarddatenbank verwenden möchten, kann die Abfrage *create database...* ausgelassen werden.
* **<table name>**: Der Name der Tabelle, die in der angegebenen Datenbank erstellt werden soll. Wenn Sie die Standarddatenbank verwenden möchten, kann auf die Tabelle direkt mit *<table name>* ohne <database name> verwiesen werden.
* **<field separator>**: Das Trennzeichen für die Felder in der Datendatei, die in die Hive-Tabelle hochgeladen werden soll.
* **<line separator>**: Das Trennzeichen für die Zeilen in der Datendatei.
* **<storage location>**: Der Azure-Speicherort zum Speichern der Daten der Hive-Tabellen. Wenn Sie *LOCATION <storage location>* nicht angeben, werden die Datenbank und die Tabellen standardmäßig im Verzeichnis *hive/warehouse/* im Standardcontainer des Hive-Clusters gespeichert. Wenn Sie den Speicherort angeben möchten, muss sich dieser im Standardcontainer für die Datenbank und die Tabellen befinden. Auf diesen Speicherort muss als relativer Speicherort zum Standardcontainer des Clusters im Format *'wasb:///<Verzeichnis 1>/'* oder *'wasb:///<Verzeichnis 1>/<Verzeichnis 2>/'* usw. verwiesen werden. Nachdem die Abfrage ausgeführt wurde, werden die relativen Verzeichnisse innerhalb des Standardcontainers erstellt.
* **TBLPROPERTIES("skip.header.line.count"="1")**: Wenn die Datendatei einen Header enthält, müssen Sie diese Eigenschaft **am Ende** der *create table*-Abfrage einfügen. Andernfalls wird der Header als ein Datensatz in die Tabelle geladen. Wenn die Datendatei keinen Header enthält, kann dieser Konfigurationsschritt in der Abfrage ausgelassen werden.

## <a name="load-data"></a>Laden von Daten in Hive-Tabellen
Mit dieser Hive-Abfrage laden Sie Daten eine Hive-Tabelle.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

* **<path to blob data>**: Wenn sich die in die Hive-Tabelle hochzuladende Blobdatei im Standardcontainer des HDInsight Hadoop-Clusters befindet, muss der *<path to blob data>* das Format *'wasb:///<directory in this container>/<blob file name>'* aufweisen. Die Blobdatei kann sich auch in einem zusätzlichen Container des HDInsight Hadoop-Clusters befinden. In diesem Fall muss *<path to blob data>* das Format *'wasb://<container name><storage account name>.blob.core.windows.net/<blob file name>'* aufweisen.

  > [!NOTE]
  > Die Blobdaten, die in die Hive-Tabelle hochgeladen werden sollen, müssen sich im Standard- oder einem zusätzlichen Container des Speicherkontos für den Hadoop-Cluster befinden. Andernfalls misslingt die Abfrage *LOAD DATA* , weil sie keinen Zugriff auf die Daten hat.
  >
  >

## <a name="partition-orc"></a>Weiterführende Themen: Partitionieren von Tabellen und Speichern von Hive-Daten im ORC-Format
Wenn die Daten sehr umfangreich sind, ist ein Partitionieren der Tabelle für Abfragen, die nur wenige Partitionen der Tabelle durchsuchen müssen, sehr hilfreich. Es ist z. B. sinnvoll, die Protokolldaten einer Website nach dem Datum zu partitionieren.

Neben der Partitionierung der Hive-Tabellen ist es auch vorteilhaft, die Hive-Daten im ORC-Format (Optimized Row Columnar) zu speichern. Weitere Informationen zur ORC-Formatierung finden Sie unter <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Das Verwenden von ORC-Dateien verbessert die Leistung, wenn Hive Daten liest, schreibt und verarbeitet</a>.

### <a name="partitioned-table"></a>Partitionierte Tabelle
Mit dieser Hive-Abfrage erstellen Sie eine Hive-Tabelle und laden Daten in diese Tabelle.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Beim Abfragen partitionierter Tabellen wird empfohlen, die Partitionierungsbedingung am **Anfang** der `where`-Klausel einzufügen, um die Effizienz der Suche zu verbessern.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Speichern von Hive-Daten im ORC-Format
Sie können nicht direkt Daten im ORC-Speicherformat aus dem Blob in Hive-Tabellen laden. Mit den folgenden Schritten können Sie Daten aus Azure-Blobs, die im ORC-Format gespeichert sind, in Hive-Tabellen laden.

Erstellen Sie die externe Tabelle **STORED AS TEXTFILE** , und laden Sie Daten aus dem Blobspeicher in die Tabelle.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Erstellen Sie eine interne Tabelle mit demselben Schema und demselben Feldtrennzeichen wie die in Schritt 1 erstellte externe Tabelle, und speichern Sie die Hive-Daten im ORC-Format.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Wählen Sie Daten aus der in Schritt 1 erstellten externen Tabelle aus, und fügen Sie sie in die ORC-Tabelle ein:

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Wenn die TEXTFILE-Tabelle *<database name>.<external textfile table name>* Partitionen aufweist, wird mit dem Befehl `SELECT * FROM <database name>.<external textfile table name>` in Schritt 3 die Partitionsvariable als Feld im zurückgegebenen Dataset ausgewählt. Das Einfügen in die *<database name>.<ORC table name>* schlägt fehl, weil *<database name>.<ORC table name>* die Partitionsvariable nicht als Feld im Tabellenschema enthält. In diesem Fall müssen Sie die in *<database name>.<ORC table name>* einzufügenden Felder explizit wie folgt auswählen:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Sie können den *<external textfile table name>* ohne Bedenken weglassen, wenn Sie die folgende Abfrage verwenden, nachdem alle Daten in *<database name>.<ORC table name>* eingefügt wurden:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Sie besitzen nun eine einsatzbereite Tabelle mit Daten im ORC-Format.  
