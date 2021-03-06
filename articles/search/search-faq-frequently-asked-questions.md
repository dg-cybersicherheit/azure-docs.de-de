---
title: "Häufig gestellte Fragen (FAQ) zu Azure Search | Microsoft-Dokumentation"
description: "Enthält Antworten auf häufige Fragen zu Microsoft Azure Search."
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: 02d5fac8cf9067ec544668f306fe49b805b3d164
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---

# <a name="azure-search---frequently-asked-questions-faq"></a>Azure Search – Häufig gestellte Fragen (FAQ)
 
 Hier finden Sie Antworten auf häufig gestellte Fragen zu Konzepten, Code und Szenarien von Azure Search.

## <a name="platform"></a>Plattform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Wie unterscheidet sich Azure Search von der Volltextsuche in meinem DBMS?

Azure Search unterstützt mehrere Datenquellen, die [linguistische Analyse für viele Sprachen](https://docs.microsoft.com/rest/api/searchservice/language-support), die [benutzerdefinierte Analyse für interessante und ungewöhnliche Dateneingaben](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), Suchrangsteuerungen mithilfe von [Bewertungsprofilen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) und Benutzeroberflächenfunktionen wie automatische Vervollständigungen, Treffermarkierung und Facettennavigation. Darüber hinaus sind weitere Features vorhanden, z.B. Synonyme und umfassende Abfragesyntax, aber dies sind normalerweise keine außergewöhnlichen Features.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Was ist der Unterschied zwischen Azure Search und Elasticsearch?

Beim Vergleichen von Suchtechnologie fragen Kunden häufig danach, worin sich Azure Search genau von Elasticsearch unterscheidet. Kunden, die Azure Search anstelle von Elasticsearch für ihre Suchanwendungsprojekte nutzen, entscheiden sich meistens für diese Vorgehensweise, weil wir eine wichtige Aufgabe vereinfacht haben oder weil sie die integrierte Integration in andere Microsoft-Technologie benötigen:

+ Azure Search ist ein vollständig verwalteter Clouddienst mit Vereinbarungen zum Servicelevel (SLAs) von 99,9%, wenn die Bereitstellung mit ausreichender Redundanz erfolgt (zwei Replikate für Lesezugriff, drei Replikate für Lese-/Schreibzugriff).
+ Die [NLPs (Natural Language Processors](https://docs.microsoft.com/rest/api/searchservice/language-support) von Microsoft ermöglichen eine professionelle linguistische Analyse.  
+ [Azure Search Indexer](search-indexer-overview.md) können für die anfängliche und inkrementelle Indizierung für viele verschiedene Azure-Datenquellen einen Crawlvorgang durchführen.
+ Wenn Sie eine schnelle Reaktion auf Schwankungen des Abfrage- oder Indizierungsvolumens benötigen, können Sie [Schieberegler-Steuerelemente](search-manage.md#scale-up-or-down) im Azure-Portal verwenden oder ein [PowerShell-Skript](search-manage-powershell.md) ausführen und die Shardverwaltung direkt umgehen.  
+ [Features für die Bewertung und Optimierung](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) ermöglichen eine Beeinflussung der Suchrangbewertungen über die Funktionen des Suchmoduls hinaus. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Kann ich den Azure Search-Dienst anhalten und so die Abrechnung von Gebühren stoppen?

Sie können den Dienst nicht anhalten. Compute- und Speicherressourcen werden beim Erstellen des Diensts für die exklusive Nutzung durch Sie zugeordnet. Es ist nicht möglich, diese Ressourcen je nach Bedarf freizugeben und dann erneut zu nutzen. 

## <a name="indexing-operations"></a>Indizierungsvorgänge

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Können Indizes oder Indexmomentaufnahmen gesichert und wiederhergestellt (oder heruntergeladen und verschoben) werden?

Sie können zwar jederzeit [eine Indexdefinition abrufen](https://docs.microsoft.com/rest/api/searchservice/get-index), aber es ist kein Feature für die Indexextraktion, Momentaufnahmen oder Sicherung/Wiederherstellung vorhanden, mit dem ein in der Cloud ausgeführter *aufgefüllter* Index auf ein lokales System heruntergeladen oder in einen anderen Azure Search-Dienst verschoben werden kann. 

Indizes werden mit von Ihnen geschriebenem Code erstellt und aufgefüllt und nur in Azure Search in der Cloud ausgeführt. Normalerweise bearbeiten Kunden, die einen Index in einen anderen Dienst verschieben möchten, ihren Code so, dass ein neuer Endpunkt verwendet wird, und führen die Indizierung dann erneut aus. Wenn Sie sich eine Funktion zum Erstellen einer Momentaufnahme oder zum Sichern eines Index wünschen, können Sie bei [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index) Ihre Stimme dafür abgeben.

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Kann ich die Indizierung von SQL-Datenbankreplikaten aus durchführen (gilt für [Azure SQL-Datenbank-Indexer](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))?

 Es gelten keine Einschränkungen für die Verwendung von primären oder sekundären Replikaten als Datenquelle, wenn Sie einen Index völlig neu erstellen. Wenn Sie einen Index mit inkrementellen Updates (basierend auf geänderten Datensätzen) aktualisieren, wird aber das primäre Replikat benötigt. Diese Anforderung stammt aus SQL-Datenbank und garantiert die ausschließliche Änderungsnachverfolgung für primäre Replikate. Wenn Sie versuchen, sekundäre Replikate für eine Workload zur Indexaktualisierung zu verwenden, ist nicht garantiert, dass Sie alle Daten erhalten.

## <a name="search-operations"></a>Suchvorgänge

### <a name="can-i-search-across-multiple-indexes"></a>Kann ich übergreifend mehrere Indizes durchsuchen?

Nein. Dieser Vorgang wird nicht unterstützt. Die Suche ist immer auf einen einzelnen Index beschränkt.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Kann ich den Zugriff auf den Suchkorpus nach Benutzeridentität beschränken?

Azure Search verfügt nicht über ein rollenbasiertes Sicherheitsmodell für den Datenzugriff pro Benutzer. Die Authentifizierung wird entweder mit vollständigen Rechten oder schreibgeschützt auf Dienstebene durchgeführt. Einige Kunden haben Lösungen basierend auf dem [`$filter`-Suchparameter](https://docs.microsoft.com/rest/api/searchservice/search-documents) implementiert, aber dies ist, wenn überhaupt, nur als Problemumgehung anzusehen. Wenn Sie dieses Feature benötigen, können Sie auf [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security) Ihre Stimme dafür abgeben.

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Warum ergeben sich keine Übereinstimmungen für Begriffe, von denen ich weiß, dass sie gültig sind?

Häufig ist Benutzern in diesem Zusammenhang nicht bekannt, dass jeder Abfragetyp unterschiedliche Suchverhalten und Grade der linguistischen Analyse unterstützt. Die Volltextsuche, bei der es sich um eine gängige Workload handelt, enthält eine Sprachanalysephase, in der Ausdrücke in ihre Grundformen unterteilt werden. Dieser Aspekt der Analyse von Abfragen bewirkt, dass sich mehr Möglichkeiten für potenzielle Übereinstimmungen ergeben, da der in Token unterteilte Ausdruck mit einer größeren Zahl von Varianten übereinstimmt.

Wenn Sie [andere Abfragetypen](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) aufrufen (u.a. Platzhaltersuche, Fuzzysuche, NEAR-Suche, Vorschläge), ist keine linguistische Analyse vorhanden. Ausdrücke werden der Abfragestruktur unverändert hinzugefügt. 

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Warum ist der Suchrang für jeden Treffer eine Konstante bzw. immer der Wert 1,0?

Standardmäßig werden Suchergebnisse nach den [statistischen Eigenschaften übereinstimmender Ausdrücke](search-lucene-query-architecture.md#stage-4-scoring) bewertet und im Resultset von hoch bis niedrig sortiert. Einige Abfragetypen (Platzhalter, Präfix, regulärer Ausdruck) tragen aber immer eine konstante Bewertungspunktzahl zur Gesamtbewertung des Dokuments bei. Dieses Verhalten ist beabsichtigt. Azure Search wendet eine konstante Bewertungspunktzahl an, damit per Abfrageerweiterung ermittelte Übereinstimmungen in die Ergebnisse eingebunden werden können, ohne dass die Rangfolge beeinträchtigt wird. 

Beispiel: Angenommen, die Eingabe von „tour*“ für eine Platzhaltersuche in englischem Text führt zu den Übereinstimmungen „tours“, „tourettes“ und „tourmaline“. Aufgrund der Art dieser Ergebnisse lässt sich nicht sicher ableiten, welche Begriffe „wertvoller“ als andere sind. Daher werden Begriffshäufigkeiten beim Bewerten von Ergebnissen in Abfragen vom Typ Platzhalter, Präfix und regulärer Ausdruck ignoriert. Suchergebnisse erhalten basierend auf einer Teileingabe eine konstante Bewertung, um eine Bevorzugung von potenziell unerwarteten Übereinstimmungen zu vermeiden.

## <a name="design-patterns"></a>Entwurfsmuster

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Was ist der beste Ansatz zum Implementieren der lokalisierten Suche?

Die meisten Kunden wählen keine Sammlung, sondern dedizierte Felder, wenn in demselben Index unterschiedliche Gebietsschemas (Sprachen) unterstützt werden sollen. Gebietsschemaspezifische Felder ermöglichen die Zuweisung eines passenden Analysemoduls. Beispiel: Der Microsoft French Analyzer wird einem Feld zugewiesen, das französische Zeichenfolgen enthält. Hierdurch wird auch die Filterung vereinfacht. Wenn Sie wissen, dass eine Abfrage auf einer französischsprachigen Seite (fr-fr) initiiert wird, können Sie die Suchergebnisse auf dieses Feld beschränken. Oder erstellen Sie ein [Bewertungsprofil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), um dem Feld eine höhere relative Gewichtung zu verleihen.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine Frage zu einem fehlenden Feature bzw. einer fehlenden Funktion? Fordern Sie das Feature auf der [User Voice-Website](https://feedback.azure.com/forums/263029-azure-search) an.

## <a name="see-also"></a>Weitere Informationen

 [Stack Overflow: Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Funktionsweise der Volltextsuche in Azure Search](search-lucene-query-architecture.md)  
 [Was ist Azure Search?](search-what-is-azure-search.md)

 
