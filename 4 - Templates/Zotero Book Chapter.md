---
"Authors:": "{{authors}}"
Year: '{{ date | format ("YYYY") }}'
Chapter: "{{shortTitle}}"
---


# {{title}}
{{pdfZoteroLink}}

## Notes


{%-
    set zoteroColors = {
        "#2ea8e5": "blue",
        "#5fb236": "green",
        "#a28ae5": "purple",
        "#ff6666": "red",
        "#ffd400": "yellow",
        "#f19837": "orange",
        "#aaaaaa": "grey",
        "#e56eee": "magenta"
    }
-%}

{%-
   set colorHeading = {
		"yellow": "Yellow: Chapter Objective(s)/Questions:",
		"blue": "Blue: Premises, Facts, Supporting points ",
		"green": "Green: Key Terms & Definitions",
		"purple": "Purple: Quotes, Examples, Important Studies",
		"red": "Red: Important People/Dates/Statistics:",
		"orange": "Orange: Important Theories/Concepts/Interesting Ideas:",
		"grey": "Grey: Limitations/Cons:",
		"magenta": "Magenta: Summaries & Conclusions"
   }
-%}

{%- macro calloutHeader(type) -%}
    {%- switch type -%}
        {%- case "highlight" -%}
        Highlight
        {%- case "image" -%}
        Image
        {%- default -%}
        Note
    {%- endswitch -%}
{%- endmacro %}

{%- set newAnnot = [] -%}
{%- set newAnnotations = [] -%}
{%- set annotations = annotations | filterby("date", "dateafter", lastImportDate) %}

{%- for annot in annotations -%}
    {%- if annot.color in zoteroColors -%}
        {%- set customColor = zoteroColors[annot.color] -%}
    {%- elif annot.colorCategory|lower in colorHeading -%}
    	{%- set customColor = annot.colorCategory|lower -%}
    {%- else -%}
	    {%- set customColor = "other" -%}
    {%- endif -%}
    {%- set newAnnotations = (newAnnotations.push({"annotation": annot, "customColor": customColor}), newAnnotations) -%}
{%- endfor -%}

{%- for color, heading in colorHeading -%}
{%- for entry in newAnnotations | filterby ("customColor", "startswith", color) -%}
{%- set annot = entry.annotation -%}

{%- if entry and loop.first %}
# {{colorHeading[color]}}
{%- endif %}

> [!quote]+ {{calloutHeader(annot.type)}} ([Page {{annot.page}}]({{annot.desktopURI}}))

{%- if annot.annotatedText %}
> {{annot.annotatedText}} {% if annot.hashTags %}{{annot.hashTags}}{% endif -%}
{%- endif %}

{%- if annot.imageRelativePath %}
> ![[{{annot.imageRelativePath}}]]
{%- endif %}

{%- if annot.ocrText %}
> {{annot.ocrText}}
{%- endif %}

{%- if annot.comment %}
> - **{{annot.comment}}**
{%- endif -%}

{%- endfor -%}
{%- endfor -%}





## Tags
1. 