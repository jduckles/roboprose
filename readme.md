# What is Roboprose

Roboprose is prose that you setup so that your computer (the robot) is able to write on your behalf. The below workflow
will enable you to generate automatically filled contracts, agreements, invoices, letters, reports and more. 
The idea here is to use templating tools to streamline business and data science workflows enabling you to create data driven 
documents. In the below examples I use the command line tool `jinja2` from the [`jinja2-cli`](https://pypi.org/project/jinja2-cli/) python package. There are tools like `knitr` and RMarkdown which do this as well, this is a lightweight, small-tools approach that can be integrated with many other stystmes and tools. At the end I show an example of using pandoc to create styled Word documents or HTML web pages. 

# Installing Jinja2-cli

The Jinja2-cli is a python package that will give us a command-line tool `jinja2` which will let us fill templates from the shell.

```
pip3 install jinja2-cli 
```

# Generating Roboprose with Jinja2

The [Jinja2 template engine](http://jinja.pocoo.org/docs/2.10/templates/) is a piece of Python
code that can be used to create templates and fill them with data (called the context in Jinja2). 

A simple Jinja template might look like:

```
Hello {{ first_name }}, how are you today?
```

When provided a python dictionary of values (the **context**) we can fill the **template**. So in this 
simple case our context could be the python dictionary:

```
{ "first_name": "Albert"}
```

Using Jinja2 in Python we can **fill** a **template** with a **context**. We're going to use the `jinja2-cli` which will 
enable us to use, rather than using Python dictionaries, fill templates with yaml, json, ini files, or URL 
query strings. In the below examples we'll use yaml because of its simple and easy to read syntax. `jinja2-cli` will also enable us to specify key-valu pairs with the `-D` option so we can pass values in without creating a file. 


For simple applications, it can be useful to think of this ability to generate roboprose
as similar to the mail merge functionality in office document tools. These are the tools that generate
form letters or print address labels, or envelopes, they take a set of data and make multiple documents
for say each roww in a spreadsheet or csv table. An example of an email workflow using Jinja2 at [jduckles/emailutil](https://github.com/jduckles/emailutil)

Example of a simple template (using Office/Word style syntax `<< >>`:
```
Hello {{ first_name }} {{ last_name }},

We see that your account is overdue since {{ overdue_date }}  by  {{ amount_due }}.

Sincerely,
Accounting
```

If we looped over the rows in the following table:

| first_name | last_name | overdue_date | amount_due |
| -- | ----- | -------- | ------------ | 
| Adam | Smith | 2019-01-01 | $10 |
| Bob | Armstrong | 2018-02-03| $20 |

We could create two output documents prepared to send to each person.

This is no better than a simple mail merge, except the UNIX small-tools approach means we can take the output Markdown and feed it into an email, a document, a database anywhere. 

Jinja2 will also allow us to have more complicated templates with internal structure, a simple example 
shows how we can loop over a list of invoices and print a bullet item for each of them:

examples/embedded_list.md
```
Hello {{ first_name }} {{ last_name }},

 We see that you have the following invoices past due:
 {% for invoice in invoices %}
 * #{{ invoice.invoice_num }} - ${{ invoice.invoice_amount }}
 {% endfor %}
```

In this template we have a new value `invoices`, which can contain multiple entries, when we loop over each entry we can call 
its values by their nested key value for example `invoice.invoice_num`. 

Rather than a csv file format, we can use yaml and be able to store the data like:

`examples/embedded_list.yml`
```
first_name: Adam
last_name: Smith
invoices:
    - invoice_num: 1024
      invoice_amount: $10
    - invoice_num: 2034
      invoice_amount: $400
```

Putting it together:

```
jinja2 examples/embedded_list.md examples/embedded_list.yml
```

## Using if statements to include/exclude sections of prose

`examples/logic.md`
```
{% if producta %}Product A will help your company to be more effective and make more money {% endif %}
{% if productb %}Product B will help you to grow your company faster with less challenges{% endif %}
{% if disclaimer %}We disclaim all liability {% endif %}

```
Then we can turn on and off clauses for each contract:

`examples/logic.yml`
```
producta: true
productb: false
disclaimer: true
```

Putting them together:
```
$ jinja2 examples/logic.md examples/logic.yml
* Product A will help your company to be more effective and make more money

* We disclaim all liability
```

## Pandoc

Pandoc is a piece of software which can convert between various text document formats. It can take markdown data and style
it to be a nice well formatted word document. It can take a word document and convert it back to markdown. We'll use pandoc
here with a custom template to demonstrate how we can use it to create a nice letter or contract document from a plain text
filled template.

```
jinja2 examples/proposal.md examples/proposal1.yml > proposal1.md
jinja2 examples/proposal.md examples/proposal2.yml > proposal2.md

pandoc -f markdown proposal1.md -o proposal1.docx
pandoc -f markdown proposal2.md -o proposal2.docx
```

Proposal 1:

![](https://jduckles-dropshare.s3-us-west-2.amazonaws.com/Screen-Shot-2019-05-23-16-46-17.86.png)

Proposal 2:

![](https://jduckles-dropshare.s3-us-west-2.amazonaws.com/Screen-Shot-2019-05-23-16-47-13.46.png)


## Application areas

* Contracts and business agreements
* Consulting reports
* Invoices
* Customized and data-drive reports
* More!
