# Modules Overview

## Getting Started

Inside of modules you will see the new folder added: 'IMPACT'. Inside of the IMPACT folder you will see the new module added: 'HarvestTeardownData'. There is also a navigation class which will be covered later.

![](<.gitbook/assets/image (19).png>)

Inside of the 'HarvestTeardownData' folder...

![](<.gitbook/assets/image (1).png>)

## Columns

The Columns class houses the names and attributes for each column within the table/module.

![](<.gitbook/assets/image (28).png>)

## Dialog

The Dialog class houses the dialog or modal commands. (more info in detailed Dialog section). This class is in TypeScript which is a superset of JavaScript

![](<.gitbook/assets/image (15).png>)

This page will help with understanding what the Dialog class is used for

{% embed url="https://volkanceylan.gitbooks.io/serenity-guide/content/serene_tour/edit_dialogs.html" %}

## EndPoint

This page will help with understanding what the EndPoint class is used for

{% embed url="https://volkanceylan.gitbooks.io/serenity-guide/content/services/service_endpoints.html" %}

## Form

The form class houses all the inputs that will be displayed in the Dialog

![](<.gitbook/assets/image (13).png>)

## Grid

The Grid class houses different details and components which is render as visuals on the web page. Here you can add special conditions to the Grid and page layout like buttons, inline images, etc.This class is in TypeScript which is a superset of JavaScript.

![](<.gitbook/assets/image (26).png>)

## Index

The Index page is the html page where the grid is rendered. Here you can add Javascript functions and other elements the the html page.

![](<.gitbook/assets/image (7).png>)

## Page

The Page class is the controller class. Consider it being the Parent class. It is where the page requests are sent for a page. It houses the route for each page with its parent folder.

![](<.gitbook/assets/image (25).png>)

## Repository

The Repository class is an extension of Endpoint.

## Row

The Row class houses all of the column data. It is the list of data from the table or view. Each column must match the tables column data type and name exactly.&#x20;

## Navigation

By default when a new identifier or parent folder is created, a Navigation class is also created. This navigation class houses the icons, links and attributes for the modules within the parent module.&#x20;

![](<.gitbook/assets/image (21).png>)

