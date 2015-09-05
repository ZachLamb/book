{% import '../../hackathons/classmates/data.html' as data %}

# Report

There are {{ data.comments.length }} students who gave a self-introduction. As a
class, we brainstormed and came up with a long list of further questions we can
ask based on this data. Our team chose to tackle on the following:

# Names of everyone's favorite languages?

{% lodash %}
var language_string = ""
for (i = 0; i < _.size(data.comments); i++) {
    var text = data.comments[i].body
    var languages

    function splitString(stringToSplit, separator) {
        var arrayOfStrings = stringToSplit.split(separator)
        var language_line = arrayOfStrings[2]
        var line = language_line.split(':')
        var favorite_language = line[1]
        var splitline = favorite_language.split(" ")

        if (favorite_language != "") {
            return (splitline[1])
        }
        else {
            return 'false'
        }
    }

    languages = splitString(text, '\r\n')
    if (languages == "JAVA") {languages = "Java"}
    language_string += (_.capitalize(languages) + ',')
}

var array = language_string.split(',')
var saved_array = language_string.split(',')
var new_array = array
var key = array[0].split()
var found_keys = key + ','

while (new_array.length > 1) {
    new_array = _.difference(array, key)
    array = new_array
    key = new_array[0].split()
    found_keys += (key + ',')
}

var sort_keys = found_keys.split(",")
var answer = ""

for (var i = 0; i < sort_keys.length; i++) {
    if (sort_keys[i] != "") {
    var count = 0
        for (var j = 0; j < saved_array.length; j++) {
            if(saved_array[j] == sort_keys[i]) { count++ }
        }
        answer += (sort_keys[i] + ':' + count + ' ')
    }
}

return answer
{% endlodash %}

The most loved languages are {{result}}
# How many students commented on the Introduction before the class started?

{% lodash %}
count = 0
for (i = 0; i < _.size(data.comments); i++) {
    var class_start = "2015-08-24T22:00:00"
    var create_string = _.trimRight(data.comments[i].created_at, 'Z')
    var update_string = _.trimRight(data.comments[i].updated_at, 'Z')
    var CreateTime = new Date(create_string)
    var ClassTime = new Date(class_start)

    if( ( CreateTime.getTime() - ClassTime.getTime() ) > 0 ) { }
    else { count++ }
}
return count 
{% endlodash %}
{{result}} students commented on the Intro before class started.

# How many students updated their comments?

{% lodash %}
count = 0
for (i = 0; i < _.size(data.comments); i++) {
    var create_string = _.trimRight(data.comments[i].created_at, 'Z')
    var update_string = _.trimRight(data.comments[i].updated_at, 'Z')

    var CreateTime = new Date(create_string)
    var UpdateTime = new Date(update_string)

    if( ( UpdateTime.getTime() - CreateTime.getTime() ) > 0 ) { count++ }
    else { }
}
return count 
{% endlodash %}

{{result}} students have updated their comments.

# How many are computer science majors?

{% lodash %}
var text = _.pluck(data.comments, 'body') 
var bodies = _.filter(text, function(str) { return str.indexOf('Computer Science') > 0 }); 
return ((bodies.length /26.00) * 100.00).toPrecision(2);
{% endlodash %}

{{result}}% are Computer Science majors.

We found that a majority of students loved Python. More than half the class commented before the start of class. Only a small percentage updated their comments ,and that a majority of the class are CS majors (Surprise!).
