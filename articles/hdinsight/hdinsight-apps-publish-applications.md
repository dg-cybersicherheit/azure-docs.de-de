---
title: "Veröffentlichen von HDInsight-Anwendungen – Azure| Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zum Erstellen und Veröffentlichen von HDInsight-Anwendungen."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 6aa66cac35bc317fc87003e6c3d824544c53de88
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Veröffentlichen von HDInsight-Anwendungen im Azure Marketplace
Eine HDInsight-Anwendung kann von Benutzern in einem Linux-basierten HDInsight-Cluster installiert werden. Diese Anwendungen können von Microsoft oder von unabhängigen Softwareanbietern (Independent Software Vendors, ISVs) bezogen oder aber selbst entwickelt werden. In diesem Artikel erfahren Sie, wie Sie eine HDInsight-Anwendung auf Azure Marketplace veröffentlichen.  Allgemeine Informationen zum Veröffentlichen im Azure Marketplace finden Sie unter [Veröffentlichen eines Angebots im Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight-Anwendungen verwenden das Modell *Bring Your Own License (BYOL)*. Bei diesem Modell muss der Anbieter die Anwendung für die Endbenutzer lizenzieren. Den Endbenutzern werden von Azure lediglich die erstellten Ressourcen (wie etwa der HDInsight-Cluster und dessen virtuelle Computer/Knoten) in Rechnung gestellt. Die Abrechnung für die eigentliche Anwendung wird derzeit nicht über Azure abgewickelt.

Weitere Artikel zu HDInsight-Anwendungen:

* [Installieren von HDInsight-Anwendungen](hdinsight-apps-install-applications.md): Hier erfahren Sie, wie Sie eine HDInsight-Anwendung in Ihren Clustern installieren.
* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Enthält Informationen zum Installieren und Testen benutzerdefinierter HDInsight-Anwendungen.

## <a name="prerequisites"></a>Voraussetzungen
Für die Übermittlung einer benutzerdefinierten Anwendung an den Marketplace benötigen Sie eine zuvor erstellte und getestete benutzerdefinierte Anwendung. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Enthält Informationen zum Installieren und Testen benutzerdefinierter HDInsight-Anwendungen.

Darüber hinaus benötigen Sie ein registriertes Entwicklerkonto. Informationen hierzu finden Sie unter [Veröffentlichen eines Angebots im Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) sowie unter [Erstellen eines Microsoft-Entwicklerkontos](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definieren der Anwendung
Die Veröffentlichung von Anwendungen im Azure Marketplace umfasst zwei Schritte.  Im ersten Schritt geben Sie durch das Definieren einer Datei vom Typ **createUiDef.json** an, mit welchen Clustern Ihre Anwendung kompatibel ist. Im zweiten Schritt wird dann die Vorlage über das Azure-Portal veröffentlicht. Im Anschluss finden Sie ein Beispiel für eine Datei vom Typ „createUiDef.json“:

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Feld | Beschreibung | Mögliche Werte |
| --- | --- | --- |
| types |Die Clustertypen, mit denen die Anwendung kompatibel ist. |Hadoop, HBase, Storm, Spark (oder eine beliebige Kombination dieser Werte) |
| tiers |Die Clustertarife, mit denen die Anwendung kompatibel ist. |Standard, Premium (oder beides) |
| versions |Die HDInsight-Clustertypen, mit denen die Anwendung kompatibel ist. |3.4 |

## <a name="application-install-script"></a>Skript für die Anwendungsinstallation
Bei jeder Installation einer (vorhandenen oder neuen) Anwendung in einem Cluster wird ein Edgeknoten erstellt, auf dem das Skript für die Anwendungsinstallation ausgeführt wird.
  > [!IMPORTANT]
  > Der Name des Skripts für die Anwendungsinstallation muss für einen bestimmten Cluster mit dem unten angegebenen Format eindeutig sein.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Beachten Sie, dass der Skriptname drei Teile umfasst:
  > 
  > 1. Ein Skriptnamenpräfix, das entweder den Anwendungsnamen oder einen für die Anwendung relevanten Namen enthalten sollte
  > 2. „-“ für eine bessere Lesbarkeit
  > 3. Eine eindeutige Zeichenfolgenfunktion mit dem Anwendungsnamen als Parameter
  > 
  > Das oben genannte Beispiel lautet schlussendlich in der Liste persistenter Skriptaktionen wie folgt: hue-install-v0-4wkahss55hlas. Eine Beispiel-JSON-Nutzlast finden Sie unter [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
Das Installationsskript muss die folgenden Merkmale aufweisen:
1. Stellen Sie sicher, dass das Skript idempotent ist. Mehrere Aufrufe des Skripts müssen zum gleichen Ergebnis führen.
2. Das Skript muss ordnungsgemäß mit Versionen versehen werden. Wählen Sie einen anderen Speicherort für das Skript, wenn Sie ein Upgrade vornehmen oder Änderungen testen, damit Kunden, die gerade versuchen die Anwendung zu installieren, nicht beeinträchtigt werden. 
3. Versehen Sie die Skripts an jeder Stelle mit einer adäquaten Protokollierung. Die Skriptprotokolle sind in der Regel die einzige Möglichkeit, Probleme bei der Installation von Anwendungen zu beheben.
4. Stellen Sie sicher, dass für Aufrufe externer Dienste oder Ressourcen eine angemessene Anzahl von Wiederholungen vorgesehen ist, damit die Installation nicht von vorübergehenden Netzwerkproblemen betroffen ist.
5. Wenn Ihr Skript Dienste auf den Knoten startet, stellen Sie sicher, dass die Dienste überwacht werden und so konfiguriert sind, dass Sie bei Neustarts von Knoten automatisch gestartet werden.

## <a name="package-application"></a>Packen der Anwendung
Erstellen Sie eine ZIP-Datei mit allen Dateien, die für die Installation der HDInsight-Anwendung erforderlich sind. Die ZIP-Datei wird im Schritt [Veröffentlichen der Anwendung](#publish-application) benötigt.

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Ein Beispiel finden Sie unter [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md).
* Alle erforderlichen Skripts.

> [!NOTE]
> Die Anwendungsdateien (einschließlich Webanwendungsdateien, sofern vorhanden) können sich auf einem beliebigen, öffentlich zugänglichen Endpunkt befinden.
> 

## <a name="publish-application"></a>Veröffentlichen der Anwendung
Gehen Sie zum Veröffentlichen einer HDInsight-Anwendung wie folgt vor:

1. Melden Sie sich beim [Azure-Veröffentlichungsportal](https://publish.windowsazure.com/)an.
2. Klicken Sie links auf **Lösungsvorlagen**, um eine neue Lösungsvorlage zu erstellen.
3. Geben Sie einen Titel ein, und klicken Sie dann auf **Neue Lösungsvorlage erstellen**.
4. Klicken Sie auf **Create Dev Center account and join the Azure program** (Dev Center-Konto erstellen und am Azure-Programm teilnehmen), um Ihr Unternehmen zu registrieren, sofern Sie diesen Schritt noch nicht ausgeführt haben.  Informationen hierzu finden Sie unter [Erstellen eines Microsoft-Entwicklerkontos](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Klicken Sie auf **Define some Topologies to get Started**(Definieren einiger Topologien für den Einstieg). Eine Lösungsvorlage ist allen zugehörigen Topologien übergeordnet. Sie können in einem Angebot/einer Lösungsvorlage mehrere Topologien definieren. Wenn ein Angebot in die Stagingumgebung überführt wird, werden alle Topologien einbezogen. 
6. Geben Sie einen Topologienamen ein, und klicken Sie dann auf das Pluszeichen (+).
7. Geben Sie eine neue Version ein, und klicken Sie dann auf das Pluszeichen (+).
8. Laden Sie die ZIP-Datei hoch, die Sie im Schritt [Packen der Anwendung](#package-application)vorbereitet haben.  
9. Klicken Sie auf **Zertifizierung anfordern**. Das Microsoft-Zertifizierungsteam prüft die Dateien und zertifiziert die Topologie.

## <a name="next-steps"></a>Nächste Schritte
* [Installieren von HDInsight-Anwendungen](hdinsight-apps-install-applications.md): Hier erfahren Sie, wie Sie eine HDInsight-Anwendung in Ihren Clustern installieren.
* [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
* [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe der Skriptaktion zusätzliche Anwendungen installieren.
* [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight mit Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Hier erfahren Sie, wie Sie ARM-Vorlagen für die Erstellung von HDInsight-Clustern aufrufen.
* [Verwenden leerer Edgeknoten in HDInsight](hdinsight-apps-use-edge-node.md): Erfahren Sie, wie Sie einen leeren Edgeknoten zum Zugreifen auf HDInsight-Cluster, Testen von HDInsight-Anwendungen und Hosten von HDInsight-Anwendungen verwenden.


