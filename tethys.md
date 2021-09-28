## Links
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/app_class.html">App Class</a>
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/app_settings.html">App Setting</a>
- <a href="http://docs.tethysplatform.org/en/stable/supplementary/key_concepts.html">MVC Concepts</a>
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/templating.html">App Templating</a>
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/gizmos.html">Gizmo</a>
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/workspaces.html">Workspace API</a>
- <a href="http://docs.tethysplatform.org/en/stable/tethys_sdk/tethys_services/persistent_store.html">Persistent Storage API</a>

## Django Templates
---
### Templating
- **Variables**: Variables are denoted by double curly brace syntax like this: `{{ variable }}`. Template variables are replaced by the value of the variable. Dot notation can be used to access attributes of an object, keys of dictionaries, and items in lists or tuples: `{{ my_object.attribute }}` , `{{ my_dict.key }}`, and `{{ my_list.3 }}`.
- **Filters**: Variables can be modified by filters which look like this: `{{ variable|filter:argument }}`. Filters modify the value of the variable output such as formatting dates, formatting numbers, changing the letter case, or concatenating multiple variables.
- **Tags**: Tags use curly-brace-percent-sign syntax like this: `{% tag %}`. Tags perform many different functions including creating text, controlling flow, or loading external information to be used in the app. Some commonly used tags include `for`, `if`, `block`, and `extends`.
- **Blocks**: The block tags in the Tethys templates are used to override the content in the different areas of the app base template. For example, any HTML written inside the `app_content` block will render in the app content area of the app.

## Patterns
---
### Creating New Page
1. Modify the model as necessary to support the data for the new page
2. Create a new HTML template
3. Create a new controller function
4. Add a new UrlMap in `app.py`
### Form Validation Patterns
1. Define a "value" variable for each input in the form and assign it the initial value for the input
2. Define an "error" variable for each input to handle error messages and initially set them to the empty string
3. Check to see if the form is submitted and if the form has been submitted:
    - Extract the value of each input from the GET or POST parameters and overwrite the appropriate value variable from step 1
    - Validate the value of each input, assigning an error message (if any) to the appropriate error variable from step 2 for each input with errors.
    - If there are no errors, save or process the data, and then redirect to a different page
    - If there are errors continue on and re-render the form with error messages
4. Define all gizmos and variables used to populate the form:
    - Pass the value variable created in step 1 to the initial argument of the corresponding gizmo
    - Pass the error variable created in step 2 to the error argument of the corresponding gizmo
5. Render the page, passing all gizmos to the template through the context
### SQL Alchemy
Each class in an SQLAlchemy data model defines a table in the database. The model you defined above consists of a single table called "dams", as denoted by the `__tablename__` property of the `Dam` class. The `Dam` class inherits from a `Base` class that we created in the previous lines from the `declarative_base` function. This inheritance notifies SQLAlchemy that the `Dam` class is part of the data model.

The class defines seven other properties that are instances of SQLAlchemy `Column` class: id, latitude, longitude, name, owner, river, date_built. These properties define the columns of the "dams" table. The column type and options are defined by the arguments passed to the Column class. For example, the latitude column is of type `Float` while the id column is of type `Integer`. The `id` column is flagged as the primary key for the table. IDs will be generated for each object when they are committed.

This class is not only used to define the tables for your persistent store, it is also used to create new entries and query the database.
