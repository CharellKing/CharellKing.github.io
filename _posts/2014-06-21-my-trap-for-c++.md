---
title: My Trap For C++
date: 2014-06-21 11:56 +0800
layout: post
permalink: /blog/2014/06/21 my-trap-for-c++.html
comments: true
categories:
  - C++
tags:
  - C++

---

Discovery my fauls in my habit and overcome them.

## Initialize variable

In 2012, at about 1:00am, I still worked in company, suddenly other staff discovevied crashes in server. I checked it, crashes occured when container's iterator isn't initialized; because of mass copying-code, the bugs appears in many places. I rapidly completed all the variable initializaiton.

From then on, when a variable is declared, I must initialize it.

Gernerally, we initialize different types as below:

**general type**

<pre><code>
int a = 0;
double a = 0.0;
char a = '\0';
</code></pre>

**general type's array**

<pre><code>
int a[10] = {0};
double f[10] = {0.0};
</code></pre>

**pointer**

<pre><code>
int* p_a = NULL;
std::vector<int>* p_a = NULL;
</code></pre>

**class**

<pre><code>
class Object {
 public:
  Object():a(0), b(0.0), c('\0') {
  }
 private:
  int a;
  double b;
  char c;
};
</code></pre>

Certainly you can initialize your variable with other value instead of default value above when your variable is not a undefilized variable.

## Remove when make iteration to container

When making iteration to container, you are removing your container's elements.

<pre><code>
for (iter = c.begin(); c.end() != iter; ++iter) {
  if (xxx) {
    erase(iter);
  }
}
</code></pre>

Obviously, you can find out the bug as soon as possible. When a function with removing the container's element is called in the "for" body, it isn't obvious for you.

So when crashes appear during the iteration to container, you should check the "for" body carefully.

I list the method of doing iteration when remove elements in the containers as below:

**vector, list**
<pre><code>
std::vector<int>::iterator iter = v.begin();
while (v.end() != iter) {
  if (xxx) {
    iter = v.erase(iter);
  } else {
    ++iter;
  }
}
</code></pre>

**map, set, multimap, multiset**
<pre><code>
std::map<int>::iterator iter = rbtree.begin();
while (rbtree.end() != iter) {
  if (xxx) {
    v.erase(iter++);
  } else {
    ++iter;
  }
}
</code></pre>
To know all kind of the container's "erase" function, you can refer the website:

[cplusplus](http://www.cplusplus.com)

### Forget increment or decrement when use "while"

In the recent past, I use mass "while" without increment or decrement, make mass infinite loop. At that time, I lose the
faith to myself. I replace "while" with "for" in my code as possible.

<pre><code>
for(std::hash_map<int, std::vector<int> >::iterator iter = m.begin(); m.end() != iter; ++iter) {
//TODO
}
</code></pre>

Obviousely "for" style reminds me to add increment or decrement. But "for" style's condition is too long, you can break
the line and "while" style is cleaner. Now I can overcome the problem, I firstly implement a "while" frame with
increment or decrement, then add my code in the "TODO" position.

<pre><code>
std::hash_map<int, std::vector<int> >::iterator iter = m.begin();
while (m.end() != iter) {
  //TODO
  ++iter;
}
</code></pre>

Tips: When add "continue" in the "TODO" position, after execute the "contine" command, "while" don't make indecrement or decrement, but "for" make it.
