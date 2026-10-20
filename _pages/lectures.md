---
layout: schedule
permalink: /lectures/
title: Schedule
description: Autumn 2026
---


{% assign current_module = 0 %}
{% assign skip_classes = 0 %}

{% for item in site.data.lectures %}
{% if item.date %}
{% assign lecture = item %}
{% assign lecture_iso_date = lecture.date | split: ', ' | last | replace: '/', '-' %}

<tr class="upcoming" data-lecture-date="{{ lecture_iso_date | escape }}">
    <th scope="row">{{ lecture.date }}</th>
    {% if lecture.title contains '期末考试' %}
    {% assign skip_classes = skip_classes | plus: 1 %}
    <td colspan="4" align="center">{{ lecture.title }}</td>
    {% else %}
    <td>
        Lecture #{{ forloop.index | minus: current_module | minus: skip_classes }}
        {% if lecture.lecturer %}({{ lecture.lecturer }}){% endif %}:
        <br />
        {{ lecture.title }}
        <br />
        [
            {% if lecture.slides %}
            {% for slide in lecture.slides %}
                <a href="{{ slide }}" target="_blank">slides #{{forloop.index}}</a>
            {% endfor %}

            {% else %}
              slides
            {% endif %}

        ]
    </td>
    <td>
        {% if lecture.readings %}
        <ul>
        {% for reading in lecture.readings %}
            <li>{{ reading }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    <td>
        <p>{{ lecture.logistics }}</p>
    </td>
    {% endif %}
</tr>
{% else %}
{% assign current_module = current_module | plus: 1 %}
{% assign module = item %}
#<tr class="info">
#    <td colspan="5" align="center"><strong>{{ module.title }}</strong></td>
#</tr>
{% endif %}
{% endfor %}
