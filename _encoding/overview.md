---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: TechArticle
title: overview
order: 0
date:   2018-02-26 17:42:09 +0100
---
Any information system will involve the transfer of data from one point to another. This could happen transparent to the user using previously established methods, or when you are designing a system you might need to create and define these methods yourself. 

To ensure the data is transfered efficiently and to have a certain degree of certainty the data arrives intact, we employ various encoding methods in different parts of the data transfer pipeline.

In this overview we'll quickly go over this transfer pipeline. In later chapters we will go through some terms and definitions to better understand and quantify these coding steps and then delve into the specifics of how to applying each of them.

{% include articleimage.html image=site.data.knowledge_base.encoding.images.information-system %}
Above is a diagram displaying the steps any data transfer goes through. 

#### source
The source is the system that transmits a message over a channel to a receiver. A source could be a custom integrated circuit, a temperature sensor, an image editing program, a text editor, a web server, etc. 

Anything that has some form of information it needs to share with something else, will need to have an efficient and predefined method of [encoding](https://en.wikipedia.org/wiki/Coding_theory) the information so it can be transfered. 

This information isn't sent in its raw form over a wire, it needs to be processed before it cant be sent. 

#### source-coding
In the source coding step, we take the source data and try to represent the information as efficiently as possible. We are compressing the data down to its smallest possible representation without losing its meaning.

For instance, if the information you're sending is whether a light is on or off, you wouldn't want to send the message "light is on" or "light is off", you could represent the same amount of information with a simple 1 or 0.  
On the other hand if we want to send actual text, we could still compress that information further by taking advantage of which letters are more frequent than others.

In short, what kind of encoding you apply totally depends on your source data, and there are various ways to compress data and to describe how efficient your compression is which we'll get into in a following section.

#### channel-coding
Now that we have a nice small message to transmit, we're still not done. A transmission channel is not perfect, and there is no guarantee our message will arrive in the same way as we sent it. To ensure (or be reasonably sure) our data arrives as intended, we add redundant data to our previously compressed message. 

In our previous light example, we might use a dedicated wire for that signal and not add any redundant data at all. Simply a voltage on the wire if its on and no voltage if its off could be sufficient.  
On the other hand, if we were transmitting that same information over a different channel (for instance a serial connection) we might add a bit of information to verify our message is intact.

How much data we add depends on the channel we're transmitting over, taking into account how reliable the channel is and how important it is our information arrives correctly. We might only want to be able to detect if our data arrived intact or we might want to be able to reconstruct corrupted data.

#### channel
The transmission channel could take many forms. A conductive wire, an optical cable or a wireless transmission. Depending on the transmission channel we might be able to represent our information with more than just ones and zeros, it could have tree states or 8 or 64 or any number. We are only limited by physics on what we can transmit, in which form and at what speed.

Besides how our data can be represented by our transmission channel (and the hardware to physically generate those signals) we have to take the reliability into account. For instance a [Digital TV signal](https://en.wikipedia.org/wiki/Digital_Video_Broadcasting) is using essentially the same information whether its transmitted over a cable, an simple antenna or if the data is coming from a satellite. But the reliability of those 3 channels are obviously very different which is why there is much more redundant data added in the channel encoding step for a satellite than there is for cable TV. 

By taking all this into account when designing a system we can ensure reliable information transfer even on [really poor transmission channels](http://www.bbc.com/news/technology-42338067).

#### receiving
After a message is transmitted, we have to decode it again to be able to use the data. We will take the same steps as before in the reverse order. 

First applying channel-decoding. Here we use the redundant data to verify the information we received is correct and if not, we either attempt to correct it ourselves (if there is enough redundant data to do so) or we might request that the source retransmits the data.

After that we can use source-decoding to transform the information back into something useful for the receiver.

#### summary
A source wants to transmit a message. It uses source encoding to convert the information into a usable and compressed format, then uses channel encoding to add redundant data. That data finally gets transmitted.

Optionally it will also encrypt the information between the source-coding and channel-coding step.

More details on each of these steps can be found in the next chapters, starting with a short introduction discussing entropy.

{%- assign name = page.collection -%}
{%- assign collection = site[name] | sort: 'order' -%}
{% for document in collection -%}
{%- unless page.url == document.url -%}
  {%- if document.order %}
  *   [{{ document.title }}]({{ document.url | relative_url }})
  {%- endif -%}
{%- endunless -%}
{% endfor %}
