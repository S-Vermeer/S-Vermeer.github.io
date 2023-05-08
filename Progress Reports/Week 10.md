# Progress Report - Week 10

### General info
**Week**: 10  
**Phase**: 3 - Component Development  
**Contacted**: Simona  
**Document(s) handed in**: {XXX}  

##### Biggest accomplishment
Setting up the docker images so the images are linked to the repositories I use.

##### Biggest setback
I spent a lot of time on CORS, and though I documented it and learned from it, it still is quite a lot of time spent trying to figure it out.

### Daily progress
![Test Image](basic-weekly-template.png)

##### Monday
On Monday I tried to get requests to the API working by trying to execute python in javascript and though I got it to work to run python in javascript, the errors from the requests module still existed so this was still not an option. Though it should allow GET requests, CORS is still not allowing me to request to the API. So I tried to see whether I could figure out how to do it from the original javascript approach. So far without success. I also started on documenting my experimenting and research on CORS.

##### Tuesday
On tuesday I continued experimenting with CORS and after documenting some more and finding out that it could only be fixed by indirectly requesting the API through our own API, a proxy or such or by asking the creator to alter the API, I decided to create my own API. It was already quite clear at the start that to connect the AI model and the front-end I was going to need to make an API, but I hadn't started yet. Seeing as in my research on CORS I saw flash coming by a lot with ways to resolve CORS errors, and I looked into it and it looked simple enough, I decided to try and see whether I could get a simple API set up using Flask.

I spent most of the afternoon working on getting the API working, and when it seemed to work, switching to docker so I would be able to host it on my own domain seeing as the ngrok had a 'failsafe' making it hard to request from there. It needed approval to continue, which didn't work for us so docker it is. At the end of the day it was able to run on docker with localhost (python http.server), and worked on Chrome. Firefox is still giving a CORS error, however that will probably be resolved by transferring to the same domain name.

##### Wednesday
On Wednesday I finished creating a public repository on docker with our two images, with the goal that it can be connected to a domain and will work on both firefox and chrome. I also worked on updating the Software Architecture document as I now had better info on how my system worked and could actually visualize it.

##### Thursday
On Thursday I reran a few graphs so they could be observed better, worked on updating the research document to have the AR section be supported by POC's and worked on C1 and C2 contexts. C3 and C4 are not currently needed, as the system is not developed far enough.

##### Friday
I researched how much fine dust weighed, I didn't document much yet as it was mostly reading but I went through several research papers trying to find an answer. 
