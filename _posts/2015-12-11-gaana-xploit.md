---
layout:     post
title:      "'xploit'ing gaana.com for free song downloads"
date:       2015-12-11 22:00:00
categories: hack
thumbnail:  cogs
---

_this is an (other) old blog post from my previous blog and was working at the
time of posting it. i'm pretty sure they've fixed it now._

I know that the title sounds both politically and morally wrong, but this's for bringing to light the *ease* with which songs can be downloaded from the internet (especially with on-demand streaming services, who make the job easier (for those who know where to look!)).

Through this post, I'd quickly go through what I did to get me MP3's from [gaana.com](http://gaana.com/) *for free*, simply because they allow me to do so! Not officialy of-course, but let's see what I need to officially 'download' songs from there.

* You need to buy gaana+ subscription to download
* songs download is only for  android and  iphone users

No; I don't need a gaana+ subscription. I can't afford it (well, not yet). And, no. Just because you have a login-id, password, doesn't mean that you can download songs. But then, you allow me to stream songs for free; so, technically, I should be able to allowed to "save them" for "future usage". 

The last time I used their Android app, I did try *Downloads*. It used to work. Mind the "used to". Even so, the Downloads were 'encrypted' (sort of), and saved under /data/data/com.gaana/ (you get the drift). Now, even the mobile app asks for a gaana+ subscription (for which again, I *politely* say, NO!)

the 'benefits' of gaana+ are given as:

* High Defination music
* Ad FREE experience
* Unlimited HD Downloads

Going through it, point-by-point, first off; the Song quality. There are three 'options' that users have: Low, Medium and High. Medium is 64 kbps, while High is 128 kbps (ain't bothered to check for the Low, but surely less than 64 kbps for sure). So, considering that 128 kbps is pretty 'okay' for a 'normal' person (me: I like my audio lossless, or at 320 kbps, not that I can make out much of a difference though), being able to download music off a on-demand streaming service such as gaana.com will be a 'boon' for people.

Ad FREE experience? [AdBlock](https://getadblock.com/) FTW! Period.

Unlimited HD Downloads. This is one hitch. Unlimited basically means infinite, which is not defined. And I don't think there's that much HD content to download in this universe.
.
.
.
Jokes apart, I think what they're trying to mean is that you can download as many songs as you want (as if you are letting me allow download any now!), considering the fact that they will infact pay back every artiste whose works I download.

>Now, I don't want all of that! I just want to listen to music, online! And if I do like it, I'll consider buying it, to support the artist (if I can afford it; and if it is available).

That's **not** the normal user for you. If you had the option of getting it for free, you'd do so, won't you?

The first time I visited Gaana was when a friend suggested  it to me, saying that they had almost every Hindi song. Me, though not being a fan of either Hindi, or songs for the matter, still went and checked out the site for two reasons:

* it seemed overrated. every song? (in fact they do).
* it was overly advertised (maybe that's the reason they're trying to get everyone to pay)

So, the website was easy to use, not a very 'cluttered' interface and all. Soon enough, I was looking at all the network requests, and the first thing I caught was the mp3 file. The intention was to check the quality of the file. It was 64 kbps. I even remember posting about it on [Twitter](https://twitter.com/the_wisenerd/status/576283551911387136), only for Gaana to respond 'quickly', and me quickly discovering the three different 'qualities' available for streaming (the 'high' being 128 kbps, as I noted [here](https://twitter.com/the_wisenerd/status/576290410139512832)). One thing that intrigued me, was a gs2.php POST; right above the mp3 file source. It seemed that it was what returned the mp3 file to be played.

It should be looked at how it was done. There was a variable ``` STREAMIMG_BASE_URL ``` (https://gnr-w1.gaana.com/) defined in the main page itself. The gs2.php file was requested as such:

```
https://gnr-w1.gaana.com/gs2.php?request_type=web&track_id=<trackid>&quality=<low|medium|high>
```

The only 'unknown' here was track_id; which didn't take me much time to figure out; I could get it using ``` gaanaMaster.getCurrentInfo() ``` (again, ``` gaanaMaster ```? Oh, it was in one of the JS files called by the page.

So, that settled it. All I had to do was get the trackID from gaanaMaster and put that in the gs2.php parameters, and voila! I had the direct link to the MP3 file that was being played.

I thought it was pure 'luck', so I decided to play around with the track_id, and found out that it worked for every ID that I put in (except a few).

Well, it worked (technically), but it returned a '.mp4' file or something, which I had no access to (literally), and then found that the file was being downloaded as 'fragments'. Also, the source of the file fragments were from akamaihd.net (yes, the CDN used by FB and Twitter). Digging a bit deeper, I found that for all the songs that had 'songs' being 'downloaded' as MP4s, there was a new ``` AkamaiAdvancedStreamingPlugin.swf ``` which pretty much made sense, since the content was being served from akamaihd.net unlike the other MP3s which were being served from ``` streams.gaana.com ```. Today, when I was discussing with my friends, about writing this post, one of them suggested that some of the content might be on akamaihd, for reducing server load, since they have to serve a lot of people. That suddenly made sense, since most of the songs which were being served from akamaihd.net were considerably 'popular'. 

Skipping the part where I wrote a Google Chrome extension which uses this information to automatically open the mp3 in a new tab upon clicking the "Download" button, I'll skip to the part where the 'breakthrough' came.

There was a ``` case 'download': ``` in one of the JS files being loaded. After unminifying, I found that there was another gs2.php (or the same gs2.php), but calling it with a different set of parameters caused it to return yet, the source of the mp3 file to stream.

It was basically one line.

```
var url = BASE_URL + "streamprovider/gs2.php?action=freedownload&track_id=" + row.id
```

BASE\_URL was ``` http://gaana.com/ ``` and row.id  (track_id) could've easily been gotten from gaanaMaster.

To my surprise, this too worked. And, this worked almost everywhere (the previous gnr-w1/..gs2.php was 'erratic').

And since it was in the source, saying (screamin!) ``` 'download' ```, I noticed it, and quicly updated my little chrome extension to read a couple of variables and got me the mp3 file. 

So, basically, you can use the following URL to obtain 'free' songs from Gaana.

```
var url = http://gaana.com/streamprovider/gs2.php?action=freedownload&track_id=<id>&quality=<low|medium|high>
```

It is that simple to get songs from gaana.com.

One question. Why?

What can be done? Not much. Whatever's to be done, is to be done by the guys at Gaana. I can't think something better, but they could follow something similar to akamaihd.net, where you have a ```AkamaiAdvancedStreamingPlugin``` and data is downloaded in fragments (encrypted), and can be accessed only through the SWF.

That seems to be the most *secure* way of doing stuff (one, because that's one level of security, where I can't get in; and another, for you can't keep such a *feature* right in the source code where the normal user *might* look) for now.

Having said so, here's some *showcasing* of the *vulnerability*. 

* Download [this](https://github.com/thewisenerd/GaanaDL/archive/master.zip), my chrome extension (source [here](https://github.com/thewisenerd/GaanaDL)) and extract the folder `GaanaDL-master` to someplace safe. 
* Now, go to ```chrome://extensions ``` and click on 'load unpacked extension' and load the folder `GaanaDL-master`.

Then, browse over to Gaana.com, and play any song. Click the 'Download' button. You'll get an alert with the song name (to copy and save file as, and then, the mp3 will be opened in a new tab, where you can `ctrl + s` save the mp3, with the file name that you got from the alert.

The purpose of this post wasn't specifically aimed at showcasing this *so called* vulnerability, but the fact that there isn't much security in most of the on-demand streaming services available. And that perhaps is one of the most common reasons for piracy.

Hoping for this to be fixed soon by Gaana.com, and hope there's better 'security' in the future, so that there won't be any crazy guys like me writing Chrome Extensions to get stuff for free from you. 

That's all for tonight,

Peace out,
thewisenerd.