# Generating Roboprose with Jinja2

The [Jinja2 template engine](http://jinja.pocoo.org/docs/2.10/templates/) is a piece of Python
code that can be used to develop automatically generated prose which fills in various variables
based on a dataset provided. This can of course be integrated into web pages or driven by databases.
However, for simple applications, it can be useful to think of this ability to generate roboprose
as similar to the mail merge functionality in office document tools. These are the tools that generate
form letters or print address labels, or envelopes, they take a set of data and make multiple documents
from an iteration across those data.

We can also use this kind of workflow and the flexibility of the Jinja template engine allowing templates
to contain logic. This is an advancement from the way office tools will merge keys into a given location.

Example of a simple template (using Office/Word style syntax `<< >>`:
```
Hello <<first_name>> <<last_name>>,

We see that your account is overdue since <<overdue_date>> by <<amount_due>>

Sincerely,
Accounting
```
This would be able to merge a data file of the form:
|first_name | last_name | overdue_date | amount_due |
|----|-------|----------|--------------|-----------|
|Adam | Smith | 2019-01-01 | $10 |
|Bob | Armstrong | 2018-02-03| $20|

To create two documents prepared to send to each person.

Jinja allows us to have more complicated templates with internal structure:

examples/embedded_list.md
```
Hello {{ first_name }} {{ last_name }},

 We see that you have the following invoices past due:
 {% for invoice in invoices %}
 * #{{ invoice.invoice_num }} - ${{ invoice.invoice_amount }}
 {% endfor %}
```

In this template we have a new value invoices, which can contain multiple invoices. Rather than a csv file format, we can use
yaml and be able to store the data like:

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
