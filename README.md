jquery.form.js
===========

jQuery plugin for generation and processing Twitter Bootstrap v3.3.5 framework forms

Usage
-----

#### jQuery#form(schema, data, callback)

* schema — object with structure of the form fields
* data — object with initialization data of the form fields
* callback — function to call upon successful validation

Before using, you must assign templates functions (`$.fn.form.templates = [function, ...]`)

For example, you can use underscore.js library and templates.html from this repository

#### Usage example

```javascript
require(["jquery.form", "underscore"], function($) {

    "use strict";

    $.get("templates.html", function(response) {

        $.fn.form.templates = $(response).filter("[data-forms-template]").get()
        .reduce(function(mem, elem) {

            mem[$(elem).data("forms-template")] = _.template($(elem).html());
            mem[$(elem).data("forms-template")].wrapped = $(elem).data("wrapped") !== false;

            return mem;

        }, {});

        $("<form>").form({
            first_name: {
                title: "First name",
                required: true,
                type: "text"
            },
            last_name: {
                title: "Last name",
                type: "text"
            },
            gender: {
                title: "Gender",
                type: "select",
                values: [{
                    id: 1,
                    title: "man"
                }, {
                    id: 2,
                    title: "woman"
                }],
                required: true,
                show_if: "data.last_name === 'Taranov'"
            },
            submit: {
                title: "Next &raquo;",
                type: "submit"
            }
        }, {
            first_name: "Oleg"
        }, function(data) {

            $("<p>").text(JSON.stringify(data)).insertAfter(this);

        }).appendTo($("<div class='container'>").appendTo($("body")));
    });
});
```
#### Webpack
For compiling using Webpack, you can do this:
```javascript
import $ from 'jquery';
import jForm from 'jquery.form';

$.fn.form = jForm;
$.fn.tooltip = function() {};
```

In this case, for some unknown for me reason, $.fn.tooltip is not mixed from Twitter Bootstrap. So, you can write your own or find another solution to correct mixing $.fn.tooltip from Bootstrap.
