Hello {{ first_name }} {{ last_name }},

 We see that you have the following invoices past due:
 {% for invoice in invoices %}
 * #{{ invoice.invoice_num }} - ${{ invoice.invoice_amount }}
 {% endfor %}
