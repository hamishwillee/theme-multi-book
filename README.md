# theme-dronecode

This is the proposed dronecode theme (under construction). It modifies the sidebar to add links to each of the main dronecode documentation books and displays the current book's TOC under the appropriate heading. If you click a link you get taken to the new book. If all our projects use this same theme then users will have the impression of a single library, even though the resources are spread across a number of URLs.

<img src="https://github.com/hamishwillee/theme-dronecode/blob/image_branch/dronecode_gitbook_sidebar.jpg" width="300px" />

> The image above shows the current theme. The Minimal Test Website book is only present during testing and will be removed in the final version.

> **Note** 
>  * If the theme is added to a library that does not match the supported set (as shown) then it's toc will be displayed *above* the list of libraries.
>  * The theme is written for the current version of gitbook 3.2.2 and will probably not work on gitbook 4+ (which has been re-written).
>  * Currently only English library links are supported. The theme can easily be translated (information below)

## Install the theme

To install the theme, add the following text to the target book's `book.json`
```json
 "plugins": [
        ...,
        "theme-dronecode@git+https://github.com/hamishwillee/theme-dronecode.git"
    ],
 ```
The theme uses a variable called `book_id` to identify the current book and display the TOC under the appropriate heading. The supported ids/books are: `'docs.px4','dev.px4','docs.qgc','dev.qgc','dev.mavlink','minimal_test_gitbook'`.

Add the section below with the appropriate book id for the library.
 ```json
{
    "variables": {
        "book_id": "docs.px4"
    },
 }
 ```

## Updating the theme

### Adding a new language

The theme uses native gitbook i18n support (translation strings are stored a folder named `_i18n` as a JSON file named with the language code (e.g. `/_i18n/zh.json`). If no translation is provided for a particular language the English strings will be used.

> **Note** If you create a translation file for a language you MUST also add values for the library links!

To create a translation:

1. Copy the desired translation file from the default theme [_i18n](https://github.com/GitbookIO/theme-default/tree/master/_i18n) folder
to [this theme's _i18n folder](https://github.com/hamishwillee/theme-dronecode/tree/master/_i18n).
1. Open this theme's [en.json](https://github.com/hamishwillee/theme-dronecode/blob/master/_i18n/en.json) file and copy all the theme-specific strings (e.g. "DEVGUIDE_LINK_PX4", "DEVGUIDE_LINK_MAVLINK, etc) at the end into the new translation file
1. Translate the strings. IFF the target library has a translation for the current language also change the URL to point to the translated library URL for that language. 
1. Commit the translation!

> **Note** The native i18n system uses a translation file in the current theme by default and the default theme's translation file if none is defined. If you don't provide a new translation file for a language English translation values will be used. If you do provide a translation then it must include ALL strings.

## Adding a new book to the sidebar

Books are added to the sidebar in [summary.html](https://github.com/hamishwillee/theme-dronecode/blob/master/_layouts/website/summary.html).

1. Add a new book_id in the array below (this same book_id must be defined in the associated book's variables).
   ```
   <!-- Display toc at top if it is not a supported library --> 
   {% if book.book_id not in ['docs.px4','dev.px4','docs.qgc','dev.qgc','dev.mavlink','minimal_test_gitbook'] %} {{ toc() }} {% endif %}
   ```
   This ensures that the TOC will not be displayed up the top of the theme.
1. In the block `external_libraries` (`{% block external_libraries %}  `) add a new list item following the same pattern as the others. This will display the library, and then the TOC IFF this is the library for the current book. E.g. 
   ```
   {% block external_libraries %}  
   <li class="external_library" style="font-weight:bold;">{{ "USERGUIDE_LINK_PX4"|t }} </li>
       {% if book.book_id  == 'docs.px4' %} {{ toc() }} {% endif %}
   ```

