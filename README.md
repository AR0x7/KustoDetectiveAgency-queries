- **Case 1**
```
Shelves
| mv-expand rf_id = rf_ids to typeof(string)
| lookup Books on rf_id
| project shelf, author, book_title, total_weight, weight_gram
| summarize SumOfColumn = sum(weight_gram) by shelf, total_weight
| extend  Difference = total_weight - SumOfColumn
| sort by Difference
```
