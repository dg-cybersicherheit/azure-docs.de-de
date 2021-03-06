---
title: "Einführung in die Table-API von Azure Cosmos DB | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Cosmos DB verwenden können, um riesige Mengen von Schlüssel-Wert-Daten mit geringer Latenzzeit mithilfe der gängigen OSS MongoDB-APIs zu speichern und abzufragen."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/09/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: ef57753aeeace0086c815d83600f92422996032a
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Einführung in die Table-API von Azure Cosmos DB

[Azure Cosmos DB](introduction.md) ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen für unternehmenskritische Anwendungen. Azure Cosmos DB bietet [sofort einsetzbare globale Verteilung](distribute-data-globally.md), [flexible Skalierung von Durchsatz und Speicher](partition-data.md) weltweit, Latenzzeiten im einstelligen Millisekundenbereich beim 99. Perzentil, [fünf wohldefinierte Konsistenzebenen](consistency-levels.md) sowie garantierte hohe Verfügbarkeit, gestützt durch [branchenführende Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/) (SLAs, Service Level Agreements). Azure Cosmos DB [indiziert automatisch Daten](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), sodass Sie sich nicht mit der Schema- und Indexverwaltung befassen müssen. Es unterstützt mehrere Datenmodelle – Dokumente, Schlüssel-Werte, Diagramme und spaltenorientierte Datenmodelle. 

![Azure Table Storage-API und Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB stellt die Tabellen-API (Vorschauversion) für Anwendungen bereit, die einen Schlüsselwertspeicher mit flexiblem Schema, vorhersagbarer Leistung, globaler Verteilung und hohem Durchsatz benötigen. Die Tabellen-API bietet die gleiche Funktionalität wie Azure Table Storage, nutzt jedoch die Vorteile des Azure Cosmos DB-Moduls. 

Sie können Azure Table Storage weiterhin für Tabellen mit hohen Speicheranforderungen und niedrigeren Durchsatzanforderungen verwenden. In einem zukünftigen Update von Azure Cosmos DB wird die Unterstützung speicheroptimierter Tabellen eingeführt, und für vorhandene und neue Azure Table Storage-Konten wird ein Upgrade auf Azure Cosmos DB durchgeführt.

## <a name="premium-and-standard-table-apis"></a>Premium- und Standard-Table-APIs
Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur „Premium Table“-Vorschau von Azure Cosmos DB folgende Vorteile:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Vorschau) |
| --- | --- | --- |
| Latenz | Schnell, aber keine Obergrenzen für die Latenzzeit | Latenzzeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, gesichert durch <10 ms Latenz bei Lese- und <15 ms Latenz bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit |
| Durchsatz | Hochgradig skalierbar, aber kein dediziertes Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](request-units.md), der durch SLAs gesichert wird. Konten haben keine Obergrenze für den Durchsatz und unterstützen >10 Millionen Vorgänge/s pro Tabelle |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für hohe Verfügbarkeit. Es kann kein Failover initiiert werden | [Sofort einsetzbare globale Verteilung](distribute-data-globally.md) aus einer bis in über 30 Regionen, Unterstützung von [automatischen und manuellen Failovern](regional-failover.md) jederzeit weltweit |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. Das Datenbankmodul von Azure Cosmos DB kann Aggregate, Geoabfragen und Sortieren unterstützen |
| Konsistenz | Stark innerhalb der primären Region, eventuell in der sekundären Region | [Fünf klar definierte Konsistenzebenen](consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen |
| Preise | Speicheroptimiert  | Durchsatzoptimiert |
| SLAs | Verfügbarkeit von 99,9 % | Verfügbarkeit von 99,99 % in einer einzelnen Region und die Möglichkeit, weitere Regionen für höhere Verfügbarkeit hinzuzufügen. [Branchenführende, umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) zur allgemeinen Verfügbarkeit |

## <a name="how-to-get-started"></a>Erste Schritte

Erstellen Sie im [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto, und beginnen Sie mit [Quickstart for Table API using .NET (Schnellstart für Table-API mit .NET)](create-table-dotnet.md). 

## <a name="next-steps"></a>Nächste Schritte

Hier einige Hinweise, um Ihnen den Einstieg zu erleichtern:
* Machen Sie die ersten Schritte mit [Azure Cosmos DB: Build a .NET application using the Table API (Azure Cosmos DB: Erstellen einer .NET-Anwendung mit der Table-API)](create-table-dotnet.md) mithilfe des vorhandenen .NET Table SDK.
* Erfahren Sie mehr über die [globale Verteilung mit Azure Cosmos DB](distribute-data-globally.md).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).

