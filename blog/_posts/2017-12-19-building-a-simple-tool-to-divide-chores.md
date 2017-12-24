---
layout: post
type: code
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

