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
    
    {% for entry in site.data.readings %}
    
        {% if entry.day %}
                            
            {% assign pagetotal = 0 %}
            {% assign optionalpagetotal = 0 %}
            {% assign minutestotal = 0 %}
            {% assign optionalminutestotal = 0 %}
            
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
	        <tr>
	        
	            <td style="text-align: center">{% if entry.unit %}Unit {% increment current_unit %}{% endif %}</td>
	            
	            <td style="text-align: center">{% if entry.week %}Week {% increment current_week %}{% endif %}</td>
	        
	            <td style="text-align: center">{{ current_week | minus: 2 | times: 7 | plus: day_of_week |  times: 24 | times: 60 | times: 60 | plus: start_of_semester | date: "%A<br/>%F" }}</td>
	            
	            <td style="text-align: center">{% if entry.topic %}{{ entry.topic }}{% endif %}<br/>{% if entry.slides %}<a href="{{ entry.slides }}">(slides)</a>{% endif %}</td>
	        
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
                           ({% if entry.units contains "page" %}p.&nbsp;{% endif %}{{ entry.start }}&nbsp;-&nbsp;{{ entry.end }})
                       {% elsif entry.start %}
                           ({% if entry.units contains "page" %}p.&nbsp;{% endif %}{{ entry.start }})
                       {% elsif entry.length and entry.units %}
                           ({{ reading.length.value }} {{ reading.length.unit }})
                       {% endif %}
                       </li>
        {% endif %}
        
        
        {% if entry.units contains "page" %}
                {% capture pagetotal %}{{ pagetotal | plus: entry.length }}{% endcapture %}
                {% capture allpagetotal %}{{ allpagetotal | plus: entry.length }}{% endcapture %}
        {% elsif entry.units contains "minute" %}
                {% capture minutestotal %}{{ minutestotal | plus: entry.length }}{% endcapture %}
                {% capture allminutestotal %}{{ allminutestotal | plus: entry.length }}{% endcapture %}              
        {% endif %}
        
        
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
