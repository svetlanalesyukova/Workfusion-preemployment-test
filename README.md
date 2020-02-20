# Workfusion-preemployment-test
Hi! Here is a document that you need to peer review. Please convert this
document to [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) using the editor of your choice (Atom, Sublime
Text, etc.). Then, submit your Markdown file to github.com. To do this, you
may need to create an account and a repository. Note that the repository
needs to be public so we can review your work.

If you feel that some edits are a matter of style, follow the [Google
Developer Style Guide](https://developers.google.com/style/), or just leave your considerations as a comment.
Thank you, and good luck!

—————

**Bot Tasks** contain work to be done by WorkFusion’s Bot. These Bots
perform the steps in your Business Process which are more suited for
automation than human labor. Bot Tasks can exist only as a Business
Processes step.
For example: address parsing, determining the width and height of an
image, interacting with your API or repository, or filtering content that do
not meet certain criteria.

**Bot Configs** are written using Web-Harvest library and are executed for
each Record separately. For writing Web-Harvest XML confi gs you can
use pre-defi ned Web-Harvest XML elements
described here or WorkFusion plugins.

**Bot Config Bundle** is a bundled java library containing a bot confi g and
a java code reference that can be used in this bot config.

# How to Create, Test, and Run

1. Create and test a Web-Harvest config using the WorkFusion
    Studio (Eclipse-based IDE).

2. Create a Bot Use Case in WorkFusion. Choose the Bot Use Case
    type (ETL is the best practice, because it better fits for reusing and
    adds flexibility)

3. Create a BP and include a Bot Step based on this Use Case.
    Alternatively, you can use WorkFusion Repository and import a Bot
    Config Bundle to WorkFusion.

4. Test the Bot Config in BP using a small input batch.

5. Make changes, if needed: column names, use proxy, datastore
    connection, output column names.

6. Update the created Bot Use Case and use it in your production BP.

Alternatively you can create a Bot Task in BP (Design Process tab) from
scratch (Blank Use Case) and paste your code, but this approach leads to
multiple issues.

# Execution Details

XML configuration is executing for each submission. If your input CSV file
contains 40 Records, the WebHarvest XML configuration will be executed
40 times, once for each Record.

If some error occurs, the Bot Config will be run 5 times.

# Results

The results of a Bot Task are defined in the <export> plugin settings.

All the columns created by the Bot Task can be used in further BP steps
(both Manual and Bot).


# Hello World Example

The following example:

1. Gets HTTP response from URLs listed in the **url_to_check** column of
    the input data file;
2. Records the HTTP response into the **http_response** variable;
3. Exports all original columns and appends a new **http** column
    containing the **http_response** variable value.

**Bot Config Example**
```html
<?xml version="1.0" encoding="UTF-8"?>
<config>
    <!--defining variable-->
    <var-def name="http_response">
        <!--passing the appropriate value from url_to_check column in input
data file as a parameter for http plugin-->
        <http url="${url_to_check}"></http>
    </var-def>

    <!--exporting all original input columns-->
    <export include-original-data="true">
        <!--adding a new column with the http plugin result to the export
file-->
        <single-column name="http" value="${http_response}"/>
    </export>

</config>
```
