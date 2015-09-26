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

# Which airlines have the worst luck with birdstrikes in terms of damage caused?
This question was asked by (calebhsu).

{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['Aircraft: Airline/Operator'];
});
var i = 0;
var costs = _.mapValues(groups, function(d){
	var total = _.reduce(d, function(total,e){
		// some of the costs are in quotes and have commas...
		var cost = e['Cost: Total $'];
		if (_.isString(cost)){
			// remove commas
			var temp1 = cost.replace(/,/g,'');
			var temp2 = _.parseInt(temp1);
			cost = temp2;
		}
		return total+cost;
	},0);
	return total;
    
});

var sorted = _.sortBy(_.pairs(costs), function(d) {
    return d[1];
});
var desc = _(sorted).reverse().value()
return _.slice(desc,0,10);
{% endlodash %}

Below are the top 10 Airlines with most total costs.
<table><tr><td>Airline</td>
	<td>Total Cost</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

# What is the most common flight phase where a birdstrike occurred?
This question was asked by (KevinKGifford).
{% lodash %}
var groups = _.groupBy(data, function(n) {
    return n['When: Phase of flight'];
});

var counts = _.mapValues(groups, function(d){
    return d.length;
});
var sorted = _.sortBy(_.pairs(counts), function(d) {
    return d[1];
});

var desc = _(sorted).reverse().value()
return desc;
{% endlodash %}
<table><tr><td>Flight Phase</td>
	<td># Incidents</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


# What airports have the most expensive average accident?
This question was asked by (satchelspencer ).

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

Below are the top 10 Airports with largest average costs.
<table><tr><td>Airline</td>
	<td>Total Cost</td></tr>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

# Which plane strikes the most birds? 
This question was asked by (twagar95).
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



# what state had the highest number of bird strikes?
This question was asked by (drewdinger).
{% lodash %}
var groups = _.groupBy(data, 'Origin State')
var stateCount = _.mapValues(groups, function(d){
    return d.length
})

var newStateCount = _.map(stateCount, function(val,key){
	return {"state": key, "count":val}
})

_.remove(newStateCount, function(n){
 	return n.state == 'N/A'	
})

var sorted =  _.sortBy(newStateCount, "count").reverse();
return _.slice(sorted,0,10);
{% endlodash %}

The table lists the top 10 States that had the most incidents. 
<table><tr><td>State</td>
	<td># Incidents</td></tr>
{% for item in result %}
    <tr>
        <td>{{item.state}}</td>
        <td>{{item.count}}</td>
    </tr>
{% endfor %}
</table>