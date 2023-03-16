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
    <tr>
    <th style="text-align: center">Week</th>
      <th style="text-align: center">Date</th>
      <th style="text-align: center">Topic</th>
    <th style="text-align: center">Content</th>
      <th style="text-align: left">Workload</th>
    </tr>
    {% for lecture in site.data.schedule %}
          {% assign pagetotal = 0 %}
        {% assign optionalpagetotal = 0 %}
        {% assign minutestotal = 0 %}
        {% assign optionalminutestotal = 0 %}
    <tr>
      <td style="text-align: center">{% if lecture.start_new_week %}Week {% increment current_week %}{% endif %}</td>
      <td style="text-align: center">{{ current_week | minus: 2 | times: 7 | plus: lecture.day_of_week |  times: 24 | times: 60 | times: 60 | plus: start_of_semester | date: "%A<br/>%F" }}</td>
      <td style="text-align: center">{% if lecture.topic %}{{ lecture.topic }}{% endif %}<br/>{% if lecture.slides %}<a href="{{ lecture.slides }}">(slides)</a>{% endif %}</td>
      <td>
        {% if lecture.reading %}
          <ul>
          {% for reading in lecture.reading %}
            <li>
            {% if reading.grad_level %}&#x2605;
            {% elsif reading.optional %}&#x24d8;
            {% else %}{% endif %}
            {% if reading.url %}
            <a href="{{ reading.url }}">{{ reading.title }}</a>
            {% else %}
            {{ reading.title }} 
            {% endif %}
            {% if reading.pages %}
            (p.&nbsp;{{ reading.pages }})
            {% elsif reading.times %}
            ({{ reading.times }})
            {% elsif reading.length and reading.length.unit and reading.length.value %}
            ({{ reading.length.value }} {{ reading.length.unit }})
            {% endif %}
            </li>
            {% if reading.length and reading.length.unit and reading.length.value %}
                {% if reading.length.unit == "pages" %}
                    {% if reading.optional %}
                        {% capture optionalpagetotal %}{{ optionalpagetotal | plus: reading.length.value }}{% endcapture %}
                    {% else %}
                        {% capture pagetotal %}{{ pagetotal | plus: reading.length.value }}{% endcapture %}
                        {% capture allpagetotal %}{{ allpagetotal | plus: reading.length.value }}{% endcapture %}
                    {% endif %}
                {% elsif reading.length.unit == "minutes" %}
                    {% if reading.optional %}
                        {% capture optionalminutestotal %}{{ optionalminutestotal | plus: reading.length.value }}{% endcapture %}
                    {% else %}
                        {% capture minutestotal %}{{ minutestotal | plus: reading.length.value }}{% endcapture %}
                        {% capture allminutestotal %}{{ allminutestotal | plus: reading.length.value }}{% endcapture %}
                    {% endif %}                
                {% endif %}
            {% endif %}
          {% endfor %}
          </ul>
        {% endif %}
      </td>
      <td>
			  {% if pagetotal != 0 %}
			      <p>📖 {{ pagetotal }} pages</p>
			  {% endif %}
			  {% if minutestotal != 0 %}
			      <p>📺 {{ minutestotal }} minutes</p>
			  {% endif %}
			  {% if optionalpagetotal != 0 %}
			      <p>ℹ️ 📖 {{ optionalpagetotal }} pages</p>
			  {% endif %}
			  {% if optionalminutestotal != 0 %}
			      <p>ℹ️ 📺 {{ optionalminutestotal }} minutes</p>
			  {% endif %}
      </td>
    </tr>
    {% endfor %}

  </tbody>
</table>

<p>This course entails approximately {{ allpagetotal }} total pages of required readings and {{ allminutestotal | divided_by: 60 }} total hours of videos for the semester.</p>
