---
layout: schedule
permalink: /lectures/
title: Schedule
description: Autumn 2026
---

<tr class="info"><td colspan="4">首次上课：2026年9月20日（周日），补10月6日课程；第二次：9月22日（周二）；第三次：9月24日（周四）。根据<a href="https://jwc.seu.edu.cn/xl/main.psp" target="_blank">学校校历</a>，10月1日至7日国庆放假，11月5日校运会停课。以下授课进度暂定，具体调整以课程通知为准；作业安排见<a href="{{ "/homework/" | relative_url }}">homework</a>页面。</td></tr>

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}

{% for item in site.data.lectures %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = lecture.date | date: "%s" | divided_by: 86400 %}
{% if lecture.date == "TBD" %}
    {% assign event_type = "upcoming" %}
{% elsif today_date > lecture_date %}
    {% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
    {% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}

<tr class="{{ event_type }}">
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
