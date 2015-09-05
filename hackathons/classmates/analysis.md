# Analysis

{% import './data.html' as data %}

After completing the warmup exercises, your task is to do four more slightly
more challenges analyses.

## How many students like sushi as their favorite food?

{% lodash %}
var total_count = 0
for (i = 0; i < _.size(data.comments); i++) {
    var text = data.comments[i].body
    var search_string=  _.last(text.split('Food: '))
    var found_count = 0

    function splitString(stringToSplit, separator) {
        var arrayOfStrings = stringToSplit.split(separator)
        var j
        var count = 0

        for (j = 0; j < arrayOfStrings.length; j++) {
            if (arrayOfStrings[j] == "Sushi") { count++ }
        }
        return count
    }
    found_count = splitString(search_string, ' ')
    total_count += found_count
}
return total_count
{% endlodash %}

The answer is {{result}}.


## Who are the students liking Python the most?

{% lodash %}
var answer = ""
var ndx = 0
for (i = 0; i < _.size(data.comments); i++) {
    var text = data.comments[i].body
    var found_name

    function splitString(stringToSplit, separator) {
        var arrayOfStrings = stringToSplit.split(separator)
        var name = arrayOfStrings[0]
        var language = arrayOfStrings[2]
        var strip = _.trimLeft(_.last(language.split(':')))

        var array = strip.split(' ')
        var language = _.trimRight(_.first(array), ',')

        if (language == "Python") {
            return (_.last(name.split('Name:')))
        }
        else {
            return 'false'
        }
    }

    found_name = splitString(text, '\r\n')

    if (found_name != 'false') {
	answer += (found_name + ',')
    }
}
return answer
{% endlodash %}

Their names are {{result}}.

## Are there more Javascript lovers or Java lovers?

{% lodash %}
var java_count = 0
var javascript_count = 0

for (i = 0; i < _.size(data.comments); i++) {
    var text = data.comments[i].body
    var found_language

    function splitString(stringToSplit, separator) {
        var arrayOfStrings = stringToSplit.split(separator)
        var language = arrayOfStrings[2]

        found_language = _.last(language.split(':'))
        if (found_language == " JAVA" || found_language == ' Java') {
            java_count++
        }
        else if (found_language == " Javascript" || found_language == 'javascript') {
            javascript_count++
        }
        else {
	    return 'false'
        }
    }
    found_language = splitString(text, '\r\n')
}

if (java_count > javascript_count) {answer = " there are more Java Lovers"}
else if (javascript_count > java_count) {answer = " there are more Javascript Lover"}
else {answer = " there are the same number of Java Lovers and Javascript Lovers..."}
return answer
{% endlodash %}

The answer is {{result}}.

##  Who like the same food as `kjblakemore`?

{% lodash %}
var food_string = ""
var search_name = "kjblakemore"
// var search_name = "willzfarmer"
var answer = ""

for (var n = 0; n < _.size(data.comments); n++) {
    var text = data.comments[n].body
    var login = data.comments[n].user.login
    var food =  _.last(text.split('Food:'))

    function splitString(stringToSplit, separator) {
        var arrayOfStrings = stringToSplit.split(separator)
        var j
        var count = 0

        for (j = 0; j < arrayOfStrings.length; j++) {
            if (arrayOfStrings[j] == food) { count++ }
        }
        return count
    }

    if (login == search_name) {
        food_string = food

        for (i = 0; i < _.size(data.comments); i++) {
            var new_text = data.comments[i].body
            var login = data.comments[i].user.login
            if (login != search_name) {
                var search_string =  _.last(new_text.split('Food:'))
                if (splitString(search_string, " ") != 0) {
                    answer += login + ','
                }
            }
        }
    }
}
if (answer == "") {answer = "none"}
return answer
{% endlodash %}

(1) Their names are {{result}}.
