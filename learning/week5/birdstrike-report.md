{% data src="birdstrike.json" %}
{% enddata %}

# Report

As a team, answer a subset of the questions submitted during the hackathon.
But instead of using Tableau, you will need to write Javascript/Lodash code
to derive your answers. Similar to before, each team member is responsible for
one question. But everyone should work together to come up with a good solution.
Your answer should consist of Lodash code and a brief writeup.
Utilize `_.map`, `_.filter`, `_.group` ...etc. Do not se any for loop.

This time, the data is not already prepared for you in a nice JSON format. You
will need to do it on your own, replacing the placeholder `birdstrike.json` with
real data.

# (Question 1) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


# (Question 2) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


# Which airports have the most expensive average accident? by (Name)

{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['Airport: Name'];
});
var i = 0;
var costs = _.mapValues(groups, function(d){
	var avg = _.reduce(d, function(total,e){
		// some of the costs are in quotes and have commas...
		var cost = e['Cost: Total $'];
		if (_.isString(cost)){
			// remove commas and convert to int
			var temp1 = cost.replace(/,/g,'');
			cost = _.parseInt(temp1);			
		}
		return total+cost;
	},0)/_.size(d);
	return avg;
});	
var sorted = _.sortBy(_.pairs(costs), function(d) {
    return d[1];
});
var desc = _(sorted).reverse().value()
return _.slice(desc,0,10);
{% endlodash %}

<table>
{% for key, value in result %}
	<tr>
		<td>{{key}}</td>
		<td>{{value}}</td>
	</tr>
{% endfor %}
</table>

# Which plane strikes the most birds? by (Name)

{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['Aircraft: Make/Model'];
});

var counts = _.mapValues(groups, function(d){
    return d.length;
});

var sorted = _.sortBy(_.pairs(counts), function(d) {
    return d[1];
});

var desc = _(sorted).reverse().value()

return _.slice(desc, 0,10);
{% endlodash %}
The table lists the top 10 aircraft makes that had the most incidents. 
<table><tr><td>Aircraft Make/Model</td>
	<td># Incidents</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>	

# (Question 5) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}