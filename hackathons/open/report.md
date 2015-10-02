# Report

Use only Javascript and SVG to produce a data analysis / visualization report.

# Authors

This report is prepared by
* [Kari Santos](https://github.com/karisantos)
* [Zachary Lamb](https://github.com/ZachLamb)
* [Denis Kazakov](https://github.com/94kazakov)

<a name="top"/>
<div id="autonav"></div>

{% data src="../fcq/fcq.clean.json" %}
{% enddata %}

{% viz %}

{% title %}

GPA vs. Lecture Type?

{% solution %}
var graded =_.filter(data,function(d){
	return d.hasOwnProperty('AVG_GRD')
})
var groups = _.groupBy(graded, function(d){
    return d['Activity_Type']
})
var avg_gpa = _.mapValues(groups, function(d){
	var avg = _.reduce(d, function(total,a){
		var gpa = a['AVG_GRD']
		return total+gpa
		},0)/_.size(d).prec(4)
		return avg
})
console.log(avg_gpa)

// TODO: add real code to convert groups (which is an object) into an array like below
// This array should have a lot more elements.
var counts = _.pairs(avg_gpa)

console.log(counts)

// TODO: modify the code below to produce a nice vertical bar charts

function computeX(d, i) {
    return 0
}

function computeHeight(d, i) {
    return 20
}

function computeWidth(d, i) {
    return 20 * i + 100
}

function computeY(d, i) {
    return 20 * i
}

function computeColor(d, i) {
    return 'red'
}

var viz = _.map(counts, function(d, i){
            return {
                x: computeX(d, i),
                y: computeY(d, i),
                height: computeHeight(d, i),
                width: computeWidth(d, i),
                color: computeColor(d, i)
            }
         })
console.log(viz)

var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')

{% template %}

<rect x="0"
      y="${d.y}"
      height="20"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />

{% endviz %}

# (Question 2)
What are the 3 hardest courses in each college?


Use the warmup exercise as the template to produce an answer here.

# (Question 3)

Use the warmup exercise as the template to produce an answer here. Remove this
question if you work as a unit of two.
