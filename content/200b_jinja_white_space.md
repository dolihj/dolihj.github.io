```j2
   `{%- if ... %}` strips before
   `{%- if ... -%}` strips before and after
   `{%+ if ... %}` preserves before
   `{%+ if ... -%}` preserves before and strips after
   remember that `{% endif %}` is treated separately

```
[Jinja2 Whitespace Control ](https://tedboy.github.io/jinja2/templ6.html)

{%- : lstrip_block 
{%+ : disable lstrip_block  

-%} : temp_block 
+%} : temp_block  

Just Print 
{% raw %} {% endraw %}