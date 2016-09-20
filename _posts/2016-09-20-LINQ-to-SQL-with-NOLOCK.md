---
layout: post
title: LINQ to SQL with NOLOCK
author: ozkar garcia
tags: [LINQ]
category: LINQ
---

If your project uses LINQ to SQL for your database access, you may want to use NOLOCK to set the isolation level to ReadUncommitted on your Select statements. The problem is that LINQ does not understand query hints. The way this works with LINQ is by running your Select statements under the context of a transaction scope, as follows:

```
string type = string.Empty;

using (DataContext ctx = new DataContext(ConnectionString))
{
  using (var tx = new System.Transactions.TransactionScope(TransactionScopeOption.Required,
         new TransactionOptions() { IsolationLevel = IsolationLevel.ReadUncommitted }))
  {
     var row = (from record in ctx.Log
                where record.Id == 99999
                select record).FirstOrDefault();
     type = row.Type;
  }
}
```
The code here creates a transaction scope and sets the isolation level to ReadUncommitted (NOLOCK). The Select statement runs under the scope of the transaction, and it returns the value from a query. The using clause ensures that both the transactions as well as the data context are disposed once the code goes out of scope.

There is also the preferable option to use Stored Procedures to eliminate the need to use transaction scope. In the Stored Procedure, you have the flexibility to use query hints.

I hope this helps.