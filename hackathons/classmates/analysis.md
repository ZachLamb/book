# Analysis

{% import './data.html' as data %}

After completing the warmup exercises, your task is to do four more slightly
more challenges analyses.

## How many students like sushi as their favorite food?

{% lodash %}

 var bodies = _.pluck(data.comments,'body')
 console.log(bodies)
 var x = _.map(bodies,functional( item)){return _.last(item.split('Food:'))}
 console.log(x)
{% endlodash %}

The answer is {{result}}.

## Who are the students liking Python the most?

{% lodash %}
return "[answer]"
{% endlodash %}

Their names are {{result}}.

## Are there more Javascript lovers or Java lovers?

{% lodash %}
return "[answer]"
{% endlodash %}

The answer is {{result}}.

## Who like the same food as `kjblakemore`?

{% lodash %}
return "[answer]"
{% endlodash %}

Their names are {{result}}.
