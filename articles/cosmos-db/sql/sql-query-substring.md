---
title: SUBSTRING in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion SUBSTRING in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c4df749de8c80b4b81410693c848b8a3c669f77d
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350388"
---
# <a name="substring-azure-cosmos-db"></a>SUBSTRING (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Gibt einen Teil eines Zeichenfolgenausdrucks zurück. Das angegebene Zeichen ist der Nullpunkt, von dem ab die Teilzeichenfolge in angegebener Länge bzw. bis zum Ende der Zeichenfolge zurückgegeben wird.  
  
## <a name="syntax"></a>Syntax
  
```sql
SUBSTRING(<str_expr>, <num_expr1>, <num_expr2>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.
  
*num_expr1*  
   Ist ein numerischer Ausdruck zur Angabe des Startzeichens. Der Wert 0 entspricht dem ersten Zeichen von *str_expr*.
  
*num_expr2*  
   Ist ein numerischer Ausdruck, der die maximale Anzahl von Zeichen angibt, die von *str_expr* zurückgegeben werden sollen. 0 oder ein geringerer Wert ergibt eine leere Zeichenfolge.

## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt die Teilzeichenfolge „abc“ zurück, beginnend bei 1 und für eine Länge von 1 Zeichen.  
  
```sql
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Hier ist das Resultset.  
  
```json
[{"substring": "b"}]  
```

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](../index-policy.md#includeexclude-strategy), wenn die Startposition `0` ist.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
