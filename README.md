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
- **Case 2**
```
let poppy_votes = (Votes
| where vote == "Poppy"
| extend timestamp_new = format_datetime(Timestamp,'yy-MM-dd [HH:mm]')
| distinct vote, via_ip, timestamp_new);
let corrected_votes = (Votes
| where vote != "Poppy"
| union poppy_votes);
corrected_votes
| summarize Count=count() by vote
| as hint.materialized=true T
| extend Total = toscalar(T | summarize sum(Count))
| project vote, Percentage = round(Count*100.0 / Total, 1), Count
| order by Count
```
