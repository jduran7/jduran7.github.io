---
layout: post
type: code
title: Building a chore schedule generator (Javascript)
---

Let's say you have 6 people working at a basic science lab.

{% highlight javascript %}
var people = [
  "Ben",
  "Chris",
  "Marco",
  "Fadi",
  "Lauren",
  "Lexi"
];
{% endhighlight %}

And you have 6 different tasks that need to be done every 2 weeks...

{% highlight javascript %}
var tasks = [
"GC Agar / GC liquid media prep",
"Pipette tips and general refills",
"Trash/recycling",
"Dish washing",
"General cleaning/lab order",
"Update -to order- board (ALL)",
];
{% endhighlight %}

How do you divide the work so that each person takes care of a different task each week, and that the workload is evenly distributed among lab members?

The answer lies in a simple arithmetic operator: **"modulo"**.

If you're not familiar with the concept, **modulo** is the operation that we use to find the *remainder* after dividing number A by number B.

Here are some examples:

> 3 % 2 = **1**

> 5 % 3 = **2**

> 1 % 3 = **1** (remember that you first have to turn that 1 into a 10 since 1 is less than 3)

OK, but how is modulo useful here?

Modulo can help us by generating indexes (1...n), no matter the dimensions of our desired table.

Let's explore that through a more visual example. In this 4x3 table, we have 12 cells that need to be assigned an index. The indexes will later be replaced with the names of the people participating in these activities.

>We will be counting from index 0 instead of 1, since this is the way javascript works with indexes. Every number will be divided by 3 since that is the number of tasks in this example.

| Task 1 | Task 2 | Task 3 |
|:--------|:-------:|--------:|
| 0%3   | 1%3   | 2%3  |
|----
| 3%3   | 4%3   | 5%3   |
|----
| 6%3   | 7%3   | 8%3   |
|----
| 9%3   | 10%3   | 11%3   |
{: rules="groups"}  

<br> 
Which translates to the following once we perform the operations:

| Task 1 | Task 2 | Task 3 |
|:--------|:-------:|--------:|
| 0   | 1   | 2  |
|----
| 0   | 1   | 2   |
|----
| 0   | 1   | 2   |
|----
| 0   | 1   | 2   |
{: rules="groups"}

<br>
But now we have a problem, work is distributed evenly in the sense that everybody will be performing the same amount of tasks in the defined timeframe, but notice how each person has to perform the very same task each week! No fun!

The solution for this is to implement a "sliding window" system through which indexes are offset on every row, to prevent the repetition of tasks. The way I did it was to start at the first cell of each row using the number of that row as the "base index", and then adding one to that initial index as I filled the rest of the cells in that row:

{% highlight javascript %}
for(i=0;i<weeks/interval;i++){
    for(j=0;j<tasks.length;j++){
      sequence.push(people[(i+j)%people.length]);
    }
    sortedSequence.push(sequence);
    sequence = [];
}
{% endhighlight %}

The labels (dates) for each of the rows were generated using [moment.js](https://momentjs.com/), a popular JavaScript library for handling dates and times.

The following function was used to get the corresponding Monday and Sunday that mark the start and end of the week(s), depending on a specified interval (tasks need to be performed every 1, 2... n weeks):

{% highlight javascript %}
function getWeek(x,interval) {
	var firstDay = x.weekday(1).format('MMMM DD');
	var lastDay = x.clone().add(6*interval+(interval-1),"day");
	if(lastDay.format('MMMM')!= x.format('MMMM')){
		lastDay = lastDay.format('MMMM DD');
	}
	else{
		lastDay = lastDay.format('DD');
	}
	return firstDay + " - " + lastDay;
}
{% endhighlight %}

Since I didn't want to end up with the same table every time we run the script, I implemented a randomizing algorithm (Fisher-Yates):

{% highlight javascript%}
function shuffle(array) {
  var copy = [], n = array.length, i;

  // While there remain elements to shuffle…
  while (n) {

    // Pick a remaining element…
    i = Math.floor(Math.random() * array.length);

    // If not already shuffled, move it to the new array.
    if (i in array) {
      copy.push(array[i]);
      delete array[i];
      n--;
    }
  }

  return copy;
}
{% endhighlight %}

Then, the dates, names and labels for the table are put together. 

{% highlight javascript%}
//Returns the contents of the table
function assignChores(people, tasks, weeks, interval){
  people = shuffle(people);
  var myList = generateList(weeks, interval);
  var sequence = [];
  var sortedSequence = [];
  var finalSchedule = {};
  
  for(i=0;i<weeks/interval;i++){
    for(j=0;j<tasks.length;j++){
      sequence.push(people[(i+j)%people.length]);
    }
    sortedSequence.push(sequence);
    sequence = [];
  }

  for(i=0;i<sortedSequence.length;i++){
  	finalSchedule[myList[i]] = sortedSequence[i];
  }
  
  return finalSchedule;
}
{% endhighlight %}

And the HTML table is written. At this point, you can specify the length in weeks of your table, and the intervals (how often do tasks need to be performed). Those two values are passed as the third and fourth arguments to assignChores(). In this case, we are generating a table for 10 weeks and an interval of 2 weeks:

{% highlight javascript %}
//writing the table
var source = assignChores(people, tasks, 52, 2);
var table = '';
var rows = Object.keys(source);
var cols = tasks.length;

table += '<thead><tr><td></td>' 
for(i=0;i<tasks.length;i++){
  table += '<td><b>' + tasks[i] + '</b></td>';
}
table+= '</tr></thead>'

for(r=0;r<rows.length;r++){
  table += '<tr><td>' + rows[r] + '</td>';
  for(c=0;c<cols;c++){
    table += '<td><div id="names">' + source[rows[r]][c] + '</div></td>'
  }
  table += '</tr>';
}
document.write(' <div id="main"><table class="striped centered">' + table + '</table></div>');
{% endhighlight %}

I added some extra jQuery functionality to let people mark if they have already completed a task, which is represented by a green checkmark that pops up right next to their name when their name is clicked.

{% highlight javascript %}
//Places a green checkmark right next to a name when it is clicked
$(function(){
  $('#main').css('cursor','default');
  $("#main #names").click( function() {
    $(this).toggleClass("greenLetters"); 
  if ($(this).hasClass("greenLetters")) {
    $(this).append("<div class=\"checkmark\">&#10004</div>");
        } else {
            $(this).find(".checkmark").remove();
        }
    });
});
{% endhighlight %}

And finally, here's what it looks like with some CSS styling ([materialize](http://materializecss.com/)):

<video  style="display:block; width:100%; height:auto;" autoplay controls loop="loop">
       <source src="{{ site.baseurl }}/assets/choreschedule.webm"  type="video/webm"  />
</video>





