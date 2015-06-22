
# 把大template拆解成小template可以减少整体刷新

Whenever a piece of data within a collection is chnged, the templates where that data is displayed is immediately refreshed. As such, if we add, edit, or remove data from the “Todos” collection, the entire “todosList” template will be refreshed – including all of the data that hasn’t changed.

This needless refreshing of data is ultimately a performance leak, although it’s easy enough to fix, so there’s no reason to ignore.

What we want to do is to split our templates down into a number of smaller templates, so when a piece of data is changed, it’s only the smallest chunks that have to refreshed.
