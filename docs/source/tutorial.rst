Tutorial
========

Use the mixin
--------------------

Let's create our first Django pony form. Just mix in the *PonyFormMixin*.

**forms.py:**

::

    from django import forms
    from django_pony_forms.pony_forms import PonyFormMixin

    class ExampleForm(PonyFormMixin, forms.Form):
        name = forms.CharField()

Let's render the form in a template:

**index.html**

::

    <form method="post">
        {% csrf_token %}
        {{ form }}
        <input type="submit" value="Save" />
    </form>

The html for the ``{{ form }}`` looks like this:

::

    <div class="form-row required" id="row-name">
        <label for="id_name">Name</label>
        <input id="id_name" type="text" name="name" />
    </div>

Use the Bootstrap mixin
-------------------------

You can also mix in *BootstrapFormMixin* to produce html for Twitter Bootstrap.

::

    from django import forms
    from django_pony_forms.pony_forms import BootstrapFormMixin

    class ExampleForm(BootstrapFormMixin, forms.Form):
        name = forms.CharField()

The produced html looks like this:

::

    <div class="control-group">
        <div class="control-label">
            <label for="id_name">Name</label>
        </div>
        <div class="controls">
            <input id="id_name" type="text" name="name" />
        </div>
    </div>

Write a form template
---------------------
You can define the following form templates:

* Form template; variable *form_template*
* Row template; variable *row_template*
* Errorlist template; variabe *errorlist_template*


Let's change the errorlist template:

::

    class ExampleForm(PonyFormMixin, forms.Form):
        errorlist_template = 'my_errorlist.html'

        name = forms.CharField()

**my_errorlist.html:**

::

    {% for error in errors %}
        <span class="help-inline">{{ error }}</span>
    {% endfor %}

Use pony forms in a view template
---------------------------------

You can also ignore the form template and define the form in your view template.

This example uses the **rows** property of the pony form.

::

    class ExampleForm(PonyFormMixin, forms.Form):
        name = forms.CharField()
        start_date = forms.DateField()

::
    
    <form method="post">
        {% csrf_token %}
        {{ form.top_errors }}
        {{ form.hidden_fields }}

        {{ form.rows.name }}
        {{ form.rows.start_date }}
        <input type="submit" value="Save" />
    </form>

Use fieldsets
-------------

This example defines the fieldsets *first* and *second*:

::

    class ExampleForm(PonyFormMixin, forms.Form):
        fieldset_definitions = dict(
            first='field1', 'field3'],
            second=['field2', 'field4']
        )

        field1 = forms.CharField()
        field2 = forms.CharField()
        field3 = forms.CharField()
        field4 = forms.CharField()

Let's use the fieldsets in the view template:

::

    <form method="post">
        {% csrf_token %}
        {{ form.top_errors }}
        {{ form.hidden_fields }}

        <div class="first">
            {{ form.fieldsets.first }}
        </div>

        <div class="second">
            {{ form.fieldsets.second }}
        </div>

        <input type="submit" value="Save" />
    </form>