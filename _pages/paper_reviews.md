---
title: Paper Reviews
permalink: /paper_reviews/
classes: wide
---

Below are a list of reviews of papers I've read in Deep Learning and Reinforcement
Learning (so far). They include a brief summary, what I found interesting/new,
questions I had when reading them, and answers for a few of them. Bear in mind,
these notes make more sense after reading the paper, they are not intended as
tutorials.

## Deep Reinforcement Learning

{% for post in site.paper_reviews %}
    {% if post.path contains 'deep_rl' %}
        {%include archive-single.html %}
    {% endif %}
{% endfor %}

## Computer Vision

{% for post in site.paper_reviews %}
    {% if post.path contains 'computer_vision' %}
        {%include archive-single.html %}
    {% endif %}
{% endfor %}

## Natural Language Processing

{% for post in site.paper_reviews %}
    {% if post.path contains 'nlp' %}
        {%include archive-single.html %}
    {% endif %}
{% endfor %}


