# Delete line containing 'string'

find/ replce 

sensor|db|ui.* 
<mark class="hltr-orange">Alt+Enter</mark> select line containing those contents
<mark class="hltr-orange">`Command` + `Shift` + `K`</mark> to delete line on Mac

# Regex search and Regex replace

## regex search 
```yml
r100_  : r100.
r999_  : r999.

search : (r)([/d]+)_
replace: $1$2. 
```
