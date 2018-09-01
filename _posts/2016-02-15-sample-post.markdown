---
layout: post
title: First publication
date: 2016-02-15 15:32:24.000000000 +09:00
---

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

感谢研一的时候做湿实验的经历。在当时杨贞标老师组轮转，接受了关跃峰老师的直接指导，以及张莹师姐带着做实验的经历。这段时间认识了王水老师，贾蓓师姐，葛光军师兄，他们无论从实际实验操作技能，还是从举手投足表现出的科研素养，人文关怀，都深深地影响了自己。还有杨组的很多人，不能一一致谢，内心非常感激。

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight python %}

import numpy as np
import matplotlib.pyplot as plt
x = np.linspace(-3, 3, 100)
y = 9 - x**2 
plt.xlim(-3,3)
plt.xticks([-3, -2, -1, 0, 1, 2, 3])
plt.ylim(0.0, 9.0)
plt.plot(x,y)

ax = plt.gca() #获取当前坐标轴
ax.set_aspect('equal') #设置横轴比相等
ax.grid(True) #显示网格
plt.show()

{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
