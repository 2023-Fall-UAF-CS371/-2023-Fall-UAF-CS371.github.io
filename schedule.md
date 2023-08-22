---
layout: default
---

{% assign allpagetotal = 0 %}
{% assign allminutestotal = 0 %}
{% assign zero_based_week = 0 %}

{% capture start_of_semester %}
{{ site.data.dates.start | date: "%s" }}
{% endcapture %}

<!-- {% increment current_week %} -->
<!-- {% assign seconds_in_day = 86400 %} -->



<h3 style="text-align: center">Schedule</h3>

<table class="table table-striped"> 
  <tbody>
  
    <!-- Table header -->
    <tr>
      <th style="text-align: center">Unit</th>
      <th style="text-align: center">Week</th>
      <th style="text-align: center">Date</th>
      <th style="text-align: center">Topic</th>
      <th style="text-align: center">Content</th>
      <th style="text-align: left">Workload</th>
    </tr>
    
    <!-- Iterate through each reading in readings.tsv -->
    
    {% for entry in site.data.readings %}
    
        <!-- When a new day of the week is encountered... -->
        {% if entry.day %}
                            
            <!-- Reset all page count statistics -->
            {% assign pagetotal = 0 %}
            {% assign optionalpagetotal = 0 %}
            {% assign minutestotal = 0 %}
            {% assign optionalminutestotal = 0 %}
            
            <!-- Calculate day of week as a zero-based integer, where Monday is zero -->
            {% if entry.day == "M" %}
                {% assign day_of_week = 0 %}
            {% elsif entry.day == "T" %}            
                {% assign day_of_week = 1 %}
            {% elsif entry.day == "W" %}            
                {% assign day_of_week = 2 %}
            {% elsif entry.day == "R" %}            
                {% assign day_of_week = 3 %}
            {% elsif entry.day == "F" %}            
                {% assign day_of_week = 4 %}
            {% endif %}                                                                                                                        
	        <!-- Declare a new table row for this day of the week -->    
	        <tr>
	        
	            <!-- Declare new table data entry for the current unit -->
	            <td style="text-align: center">{% if entry.unit %}Unit {% increment current_unit %}{% endif %}</td>
	            
	            <!-- Declare new table data entry for the current week -->
	            <td style="text-align: center">{% if entry.week %}Week {% increment current_week %}{% endif %}</td>
	        
	            <!-- Declare new table data entry for the current calendar date -->
	            <td style="text-align: center">{{ current_week | minus: 2 | times: 7 | plus: day_of_week |  times: 24 | times: 60 | times: 60 | plus: start_of_semester | date: "%A<br/>%F" }}</td>
	            
	            <!-- Declare new table data entry for the current topic name -->
	            <td style="text-align: center">{% if entry.topic %}{{ entry.topic }}{% endif %}<br/>{% if entry.slides %}<a href="{{ entry.slides }}">(slides)</a>{% endif %}</td>
	        
	            <!-- Declare new table data entry for the readings for the current day -->
	            <td>
	                <ul>	        
        {% endif %}
        
        {% if entry.title %}
                       <li>
                       {% if entry.url %}
                           <a href="{{ entry.url }}">{{ entry.title }}</a>
                       {% else %}
                           {{ entry.title }} 
                       {% endif %} 
                       {% if entry.start and entry.end %}
                           ({% if entry.unit contains "page" %}p.&nbsp;{% endif %}{{ entry.start }}&nbsp;-&nbsp;{{ entry.end }})
                       {% elsif entry.start %}
                           ({% if entry.unit contains "page" %}p.&nbsp;{% endif %}{{ entry.start }})
                       {% elsif entry.length and entry.unit %}
                           ({{ reading.length.value }} {{ reading.length.unit }})
                       {% endif %}
                       </li>
        {% endif %}
        
        
        {% if entry.unit contains "page" %}
                {% capture pagetotal %}{{ pagetotal | plus: entry.length }}{% endcapture %}
                {% capture allpagetotal %}{{ allpagetotal | plus: entry.value }}{% endcapture %}
        {% elsif entry.unit contains "minute" %}
                {% capture minutestotal %}{{ minutestotal | plus: entry.length }}{% endcapture %}
                {% capture allminutestotal %}{{ allminutestotal | plus: entry.length }}{% endcapture %}              
        {% endif %}
        
        
            <!-- Close the current table row for this day of the week -->    
            {% if entry.title or entry.day %}
            {% else %}
                  </ul>
                </td>
                <td>
			      {% if pagetotal != 0 %}
			          <p>📖 {{ pagetotal }} pages</p>
			      {% endif %}
			      {% if minutestotal != 0 %}
			          <p>📺 {{ minutestotal }} minutes</p>
			      {% endif %}
            </td>        
           </tr>
           {% endif %}
    {% endfor %}
    
  </tbody>
</table>

<p>This course entails approximately {{ allpagetotal }} total pages of required readings and {{ allminutestotal | divided_by: 60 }} total hours of videos for the semester.</p>
