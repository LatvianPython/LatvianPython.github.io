---
layout: post
quote: Python wrapper for Oracle Forms API. 🐍
syntax_highlighting: true
---

Implemented based on a [White Paper](https://www.oracle.com/technetwork/developer-tools/forms/documentation/274326-134740.pdf){:rel="noreferrer"}
💖Oracle

Available on [Python Package Index](https://pypi.org/project/pyoracle-forms/)

Implementation at [GitHub](https://github.com/LatvianPython/pyoracle_forms)


<br>
Example script
{% highlight python %}
from pyoracle_forms import Module, initialize_context
initialize_context(version="12c", encoding="utf-8")

with Module.load("./your_form.fmb") as module:
    for data_block in module.data_blocks:
        for item in data_block.items:
            item.font_name = "Comic Sans MS"

    module.save()
{% endhighlight %}

The implementation uses the C header files that come with the Oracle Forms Builder to add properties to
Python classes that represent each of the objects supported by the Oracle Forms API.


<br>
The C headers contain convenience macros for every possible property that can be changed for forms objects.

<br>
Example of convenience macros for LOVs
{% highlight C %}
#define d2flovg_font_nam(ctx,obj,val) d2flovgt_GetTextProp(ctx,obj,D2FP_FONT_NAM,val)
#define d2flovg_font_siz(ctx,obj,val) d2flovgn_GetNumProp(ctx,obj,D2FP_FONT_SIZ,val)
#define d2flovg_font_spcing(ctx,obj,val) d2flovgn_GetNumProp(ctx,obj,D2FP_FONT_SPCING,val)
{% endhighlight %}

The macros are parsed and turned into something that can be used later to create properties for Python classes

{% highlight JSON %}
  {
    "short_name": "font_spcing",
    "data_type": "num",
    "macro_name": "D2FP_FONT_SPCING",
    "property_number": 86
  }
{% endhighlight %}

A limitation with dynamically created classes is that there is no auto-complete available when you develop scripts
using the library. Currently, only one forms object is set up for static properties, the main Forms Module. It
should be possible to create static definitions for all the forms objects, from Alerts to Windows. Having everything
static will either require a lot of typing, or the creation of a script to generate Python classes, and I 
would favour the latter approach.

<br>
Statically created class
{% highlight python %}
@forms_object
class Module(BaseObject):
    object_type = FormsObjects.module

    name = Text("NAME")
    comments = Text("COMMENT")
    console_window = Text("CONSOLE_WIN")
    cursor_mode = Number("CRSR_MODE")
    direction = Number("LANG_DIR")
    ...

    alerts: Subobjects[Alert] = Subobjects("ALERT")
    attached_libraries: Subobjects[AttachedLibrary] = Subobjects("ATT_LIB")
    data_blocks: Subobjects[DataBlock] = Subobjects("BLOCK")
    ...
{% endhighlight %}

Not everything is supported by the library, there is not a simple way to add new objects to forms, nor is 
everything supported that can be done via Oracle Forms Builder. Though just having the option to 
programmatically access and change anything from the property palette should be enough for anyone,
unless you want to build forms from scratch with Python code. 


<br>
You could:

* gather fun statistics on what tables are used in data blocks, and what columns are used
* automatically redefine field type definitions based on changes in the database tables
* add triggers or perform mass changes to existing trigger code
* run code formatters and/or linters on forms PL/SQL code
* find duplicate procedures used in multiple forms that could be extracted into libraries
* keep formatting of fields consistent (though you would likely just want to use subclassing)
* change font of all fields to Comic Sans 🤡
* *whatever you can imagine*


<br>
I have not used this library with any production project, perhaps one day. 
Even so, the library should work fine as at the time of writing this the library has 100% code coverage
so there must be 0 bugs.
Jokes aside, I see no reason why it should fail at the basic operations. I just need a compelling reason to use 
the library.
