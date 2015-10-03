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

GPA vs class type

{% solution %}
 // first I need to remove all entries that don't have an AVG_GRD (I have seen a couple!)
var graded = _.filter(data,function(d){
    return d.hasOwnProperty("AVG_GRD");
})
var groups = _.groupBy(graded, 'Activity_Type');
var gpa = _.map(groups, function(d,key){
    var avg = _.reduce(d, function(total,e){
        return total+e.AVG_GRD;
    },0)/_.size(d); 
    //var avg = 3;
    var obj = {}
    obj.type = key;
    obj.avg = avg;
    return obj;
})
console.log(gpa)


function computeX(d, i) {
    return 0
}

function computeHeight(d, i) {
    return 20
}

function computeWidth(d, i,data) {
    var width = 400; // svg window width
    var max = _.max(data,function(d){
            return d.avg;
        });
    var scale = width / max.avg;
    return d.avg * scale;
    return 100
}

function computeY(d, i) {
    return 20 * i
}

function computeColor(d, i) {
    return 'red'
}
function computeLabel(d){
    return d.type
}

function computeValLabel(d) {
    return _.round(d.avg,2)
}
function computeValLabelLoc(d,i,data) {
    var width = computeWidth(d,i,data);
    return width + 2;
}
var viz = _.map(gpa, function(d, i, data){

            return {
                x: computeX(d, i),
                y: computeY(d, i),
                height: computeHeight(d, i),
                width: computeWidth(d,i,data),
                color: computeColor(d, i),
                label: computeLabel(d),
                valLabel : computeValLabel(d),
                valLabelLoc : computeValLabelLoc(d,i,data)
            }
         })


var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')
{% template %}
<g transform="translate(0 ${d.y})">
    <rect x="${d.x}"
         width="${d.width}"
         height="${d.height}"
         style="fill:${d.color};
                stroke-width:3;
                stroke:rgb(0,0,0)" />
    <text transform="translate(2 15)">${d.label}</text>
    <text transform="translate(${d.valLabelLoc} 15)">${d.valLabel}</text>
</g>
{% endviz %}

{% viz %}
{% title %}

GPA vs College

{% solution %}
 // first I need to remove all entries that don't have an AVG_GRD (I have seen a couple!)
var graded = _.filter(data,function(d){
    return d.hasOwnProperty("AVG_GRD");
})
var groups = _.groupBy(graded, 'CrsPBAColl');
var gpa = _.map(groups, function(d,key){
    var avg = _.reduce(d, function(total,e){
        return total+e.AVG_GRD;
    },0)/_.size(d); 
    //var avg = 3;
    var obj = {}
    obj.type = key;
    obj.avg = avg;
    return obj;
})
console.log(gpa)


function computeX(d, i) {
    return 0
}

function computeHeight(d, i) {
    return 20
}

function computeWidth(d, i,data) {
    var width = 400; // svg window width
    var max = _.max(data,function(d){
            return d.avg;
        });
    var scale = width / max.avg;
    return d.avg * scale;
    return 100
}

function computeY(d, i) {
    return 20 * i
}

function computeColor(d, i) {
    return 'red'
}
function computeLabel(d){
    return d.type
}

function computeValLabel(d) {
    return _.round(d.avg,2)
}
function computeValLabelLoc(d,i,data) {
    var width = computeWidth(d,i,data);
    return width + 2;
}
var viz = _.map(gpa, function(d, i, data){

            return {
                x: computeX(d, i),
                y: computeY(d, i),
                height: computeHeight(d, i),
                width: computeWidth(d,i,data),
                color: computeColor(d, i),
                label: computeLabel(d),
                valLabel : computeValLabel(d),
                valLabelLoc : computeValLabelLoc(d,i,data)
            }
         })


var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')
{% template %}
<g transform="translate(0 ${d.y})">
    <rect x="${d.x}"
         width="${d.width}"
         height="${d.height}"
         style="fill:${d.color};
                stroke-width:3;
                stroke:rgb(0,0,0)" />
    <text transform="translate(2 15)">${d.label}</text>
    <text transform="translate(${d.valLabelLoc} 15)">${d.valLabel}</text>
</g>
{% endviz %}


{% viz %}

{% title %}

Course instructor CSCI

{% solution %}
 
var csci = _.filter(data, function(d){
    return (d.CrsPBADept == 'CSCI') && 
        d.hasOwnProperty("AvgCourse") && 
        d.hasOwnProperty("AvgInstructor")
});
console.log(_.size(csci));
var stuff = _.map(csci, function(d){
    var obj = {}
    obj.AvgCourse = d.AvgCourse;
    obj.AvgInstructor = d.AvgInstructor;
    obj.Workload = d.Workload.Hrs_Wk;
    return obj;
})
console.log(stuff[0])


function computeX(d, i) {
    return (6 - d.AvgCourse)*60;
}

   

function computeY(d, i) {
    return d.AvgInstructor*50;
}

function computeColor(d, i) {
    if (d.Workload == "0-3")
        return 'yellow';
    if (d.Workload == "4-6")
        return 'orange';
    if (d.Workload == "7-9")
        return 'green'
    if (d.Workload == "10-12")
        return 'blue'   
    if (d.Workload == "13-15")
        return 'red'
    return 'black'  //16+
}
/*function computeLabel(d){
    return d.type
}

function computeValLabel(d) {
    return _.round(d.avg,2)
}
function computeValLabelLoc(d,i,data) {
    var width = computeWidth(d,i,data);
    return width + 2;
} */
var viz = _.map(stuff, function(d, i, data){

            return {
                x: computeX(d, i),
                y: computeY(d, i),
                color: computeColor(d, i),
               // label: computeLabel(d),
               // valLabel : computeValLabel(d),
               // valLabelLoc : computeValLabelLoc(d,i,data)
            }
         })


var result = _.map(viz, function(d){
         // invoke the compiled template function on each viz data
         return template({d: d})
     })
return result.join('\n')


{% template %}
<circle cx="${d.x}"
     cy="${d.y}"
     r="4"
      stroke-width="1" 
      fill="${d.color}" />
{% endviz %}