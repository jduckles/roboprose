# Report for {{ client_name }}

We're happy to provide this report on all the amazing ways that {{ client_name }}
will be able to improve their business once they've implemented our recommendations. 

We feel that the benefits to {{ client_name }} will be apparent once the work has been done.


# Terms 

During our initial consultation {{ company_name }} constultants worked with the team 
of {{ client_name }} to develop a plan for the future of our work together. 

We suggest that work begin on {{ begin_date }} and complete on {{ end_date }}. 

Terms of this agreement will be:

{% if currency == "usd" %} * All payments to be made in US Dollars {% endif %}
{% if currency == "nzd" %} * All payments to be made in New Zealand Dollars {% endif %}
{% if term %} * All payments must be made in {{ term }}-days {% endif %}

# The Plan


{% if productA %}
## Product A

ProductA will be developed in collaboration with {{ client_name }} in order to better understand how 
{{ company_name }} can improve its business. Work for Product A will begin on {{ start_date }}

{% endif %}

{% if productB %}
## Product B 

ProductB will draw from the outputs of ProductA in order to develop a set of key actions and activities which 
ProductB can improve {{ cleint_name }}'s bottom line. We'll work with the staff to make magical things happen 
that will make you all heaps of money. This work will begin at the completion of the work for ProductA and be 
completed prior to {{ end_date }}. Based on an initial estimate.

{% endif %}

{% if impact %}

We expect to have an impact of growing your new business by {{ impact_percent }}%.

{% endif %} 

