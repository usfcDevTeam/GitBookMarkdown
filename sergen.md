# Sergen

## Launch Serenity CodeGenerator

&#x20;From Visual Studio open 'Package Manager Console'

* Path: (View -> Other Windows -> Package Manager Console)

![Visual Studio](<.gitbook/assets/image (14).png>)



* Type: sergen

![Package Manager Console](<.gitbook/assets/image (22).png>)

## Sergen

![](<.gitbook/assets/image (11).png>)

### Choose connection string

![](<.gitbook/assets/image (2).png>)

### Select table and Change module name

Select the table or view you would like code generated for.

![](<.gitbook/assets/image (8).png>)

Change the Identifier (optional).&#x20;

![](<.gitbook/assets/image (18).png>)

{% hint style="warning" %}
Identifier has to be Pascal Case (WordsWithCapitalFirstLettersConcatenatedTogether)
{% endhint %}

### Types of files generated

![](<.gitbook/assets/image (4).png>)

* Row - Creates Row class that has get/set properties for each column in the table.
* Service - Creates the SQL query (Select, Update, Insert, Delete, etc) for the table selected
* UI - Creates the html page and Grid Typescript class.
* Custom - \*coming soon\*

### Generate files

Click 'Generate Code for Checked Entities'

![](<.gitbook/assets/image (16).png>)

After generating the necessary files a Sergen prompt

![](<.gitbook/assets/image (9).png>)

And a Visual Studio prompt: Click 'Reload Solution'

![](<.gitbook/assets/image (10).png>)

