<h1>SOAR EDR Project | Intro</h1>

<p align="center">
<img src="https://snipboard.io/6Nkpjh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part three of five for the series on the SOAR EDR project. If you haven't seen the previous parts where we go over how to build out a workflow for this project and set up LimaCharlie on our machines, I would highly recommend you go and read that first. 
<br />
<br />
<img src="https://snipboard.io/tvJ8Xf.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Today our objective is to generate telemetry related do Lazagne, which is the password recovery tool that we'll be using. Then, create a detection and response rule in LimaCharlie to detect this activity. So then we can send it over to Tines for automation. Let's get started.
<br />
<br />
<img src="https://snipboard.io/PjGQSe.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To begin head over to Lazagne's GitHub. I'll leave all of the links down below for you. What we want to do is click on the releases and we'll click on Lazagne.exe, and now that I realize, we need to disable our Windows security.
<br />
<br />
<img src="https://snipboard.io/YetNvg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So let's go ahead and search up "Windows security", and I'll go ahead and select the "Virus and threat protection". Click on "Manage settings", disable "Real-time protection".
  
  Now if I take a look at the downloads it says lazagne was blocked as unsafe by Microsoft Defender. Whenever you see this, what we can do is click on the three dots and click on "Keep". It will say, "This app is unsafe". Now click "Keep anyways".
  
  Now what we can do is take a look at the downloads and let's open up a Powershell. So I'll hold shift and right-click and click on "Open Powershell window here". Just to make sure it works, I'll just type in "Lazagne", and it works awesome.
  
  Because I ran this, LimaCharlie should also pick up the process for Lazagne. So let's head over to LimaCharlie and take a look. So let's head over to "Sensors List" and click on our SOAR sensor.
  
  From here scroll down, we want to select "Timeline". Now we executed this not even a minute ago so it should be somewhere here, or what we can do is type in "Lazagne". Taking a look at the earliest event, we do see it at '20:52' generating an event of "NEW_DOCUMENT".
  
  We do see the path as "C:\Users\Administrator\Downloads". We have a hash and the process ID as well, but what we're interested in the most is the new process event. So click on that, and on the right, we get all of this juicy information. It's all pretty good information to have to start generating our detection rule.
  
  So how do we do this? Well first and foremost, let's go ahead and open up a new tab for LimaCharlie and I'll head over to our organization and from here we want to select "Automation" and "DNR rules". On the top right we do have a new rule that we can use. So I'll click on "New Rule", and if this is your first time building a rule this might look pretty intimidating, but don't worry I'll show you a hack that you can use going forward.
  
  Now we don't need to build a rule from scratch. I mean there's a lot of different formats and all that stuff so what we can do instead is, go back and let's just search up a rule that is similar to ours. We know that Lazagne is a credential access tool, so I'll type in "Credential" and hey look at that, we have some the credential stuff. Let's take a look at all the available ones here.
  
  What we're most interested in is the process creation. Now you might say, "How do you know that?" Well if we go back over to the events, take a look at the event type. It is set to "NEW_PROCESS", so these are all process creations.
  
  So if I head back over to the DNR rules, we do see a Windows process creation. So let's go ahead and click on that, and at the bottom we do see view the content of this rule in the GitHub repository. So I'll click on that. 
  
  There you go, here's the rule right here. So what we can do is go ahead and click on "Raw" and copy this. Head back into our rule and click on the pencil icon. Now we can go ahead and paste in what we just copied. Now if you take a look at the top, we do see a detect and we do see a respond. So this essentially signifies the two blocks here.
  
  Starting from respond. I'll just select all of this and cut it, and then I'll paste it inside the respond block. Now we don't actually need the respond here, so I'll just go ahead and remove it. Remember that respond is just telling us that, "Hey this is the response portion". Similar to detect, we don't need it so we'll remove it and expand this. There you go.
  
  Starting from the beginning we do see events with "NEW_PROCESS" and "EXISTING_PROCESS". So what does that mean? Well, let's head back over to our Lazagne events. What we want to do is select "Event Collection", and from here its just simply grabbing these event types. So if I search up "process", we see "EXISTING_PROCESS" and if we scroll down we also see "NEW_PROCESS" as well.
  
  So this section is essentially saying this detection will only trigger if the event is under the new process or existing process event type. Now we get our first operator of "and". So what does that mean? Well, it means the event type must be either new process or existing process "and". The "and" is specified by this operator.
  
  To get a list of operators, they actually provide some right here. So "Op Reference", there's "and/or", "is", "exists", "contains", "starts with", "ends with", "it greater than", and just many others. So I'll go back over to expand, and again the operator being used here is "and". Currently, this is what it's doing in plain english.
  
  So let's remove this and then let's continue on. "rules" is going to be the criteria, so meaning you must follow these rules. The operator is Windows, case sensitive is false, and "ends with". So it's using two different operators, "is" and "ends with". Where the "path" is "event/FILE_PATH", and you might be like, "Where did they get that?"
  
  Well, heading back over to our sensor. Let's go back to our timeline and search for our Lazagne process. So we're clicking on the "NEW_PROCESS" because we're interested in the new or existing process. So I'll click on that and we do see a "FILE_PATH" here. Going back over to the "Detection and Response Rule", we see the "event/FILE_PATH". 
  
  So the "event" has a nested field of "FILE_PATH", so that is why we have "event/FILE_PATH". For example, let's just say I am interested in the hash. Then what will I do? Well, I'll type in "event/HASH", just like this, but I'll keep it back as "FILE_PAH".
  
  Where the "value" ends in "\DeviceCredentialDeployment.exe". Let's bring this back as plain English here, so what the heck does this mean? Well, essentially it is the event type and must be either "NEW_PROCESS" or "EXISTING_PROCESS", and must be a Windows machine. We can ignore case sensitivity; the file path must end with "DeviceCredentialDeployment.exe".
  
  Now if we wanted to detect our process of Lazagne, we can simply replace "DeviceCredentialDeployment" with Lazagne, and in theory, that should technically work. So let's go ahead and do that. I'll remove "DeviceCredentialDeployment" and I'll say "Lazagne".
  
  Now if we take a look at our events, we can see that the "FILE_PATH" ends with "LaZagne" here, but notice how the "z" is capitalized. That is why we have the case sensitive as "false". So regardless, even if our value has a lowercase "z" or a capital Z, we should still be okay.
  
  So I'll keep this as that, but let's go ahead and add some more options because if you recall, let me head back over to my server. Take a look at the flags that are available for LaZagne. We have "all", "browsers", "chat", "databases", "games", "git", "mails", "maven", and a lot of others. So let's just type, "all".
  
  Look at that, we got some passwords here. Now what we can do is head back over to the LimaCharlie. I'll save this for now. Let's refresh this and we'll wait a bit until the process is generated. So after a couple of seconds, we do get our new process that occurred. I'll click on this one here. I'll just zoom out just a bit since it's a lot easier to see it like this.
  
  
  So taking a look at our file path, we do see "LaZagna.exe" at the end. Take a look at the command line, we do see an "all" this time. If we look at the previous process for the command line, we just see it as "LaZagna.exe". We don't see any command line arguments associated with it.
  
  Now let's add on our detection rule to include this command line argument. The new rule that I want to create is that the event type must be either "NEW_PROCESS" or "EXISTING_PROCESS", and it must be Windows. The "file_path" should end with "LaZagne.exe", or the command line should end with "all", "lasagna", or "hash == LaZagne's hash.
  
  Now I do foresee some false positives for the command line that ends with "all". For example, we have "ipconfig /all", that's going to trigger that. However, for this demo let's just keep it as it is. So how do we build this out? Currently, we have this section good to go because we have the "NEW_PROCESS", the "EXISTING_PROCESS", and it is Windows.
  
  So what do we do? How do we create the other ones? Well, we must include an operation of "or" and then I'll press tab on my keyboard. So our new rules is going to be living under this particular "or".
  
  
  So these rules will be "case sensitive: false" because I don't want any of that. So I'll type "op: ends with" under the case sensitive. Then, I'll type "path: event/FILE_PATH" under the operator. Second to last, I'll type "value: lazagne.exe" under the path. Awesome, so we satisfied these criteria.
  
  
  Now we have to take a look at the command line, but before I do that, let's go back and align our operator with our case sensitive. Perfect.
  
  The way you can tell, it's kind of hard to see, but I'll just try and draw it out here. There's a line right there. So this line will tell you what your fields are under, if that makes sense. Adding to that, I want all of my stuff to be under the "case sensitive".
  
  
 For the second one, I'm going to type "- case sensitive: false", and just like before I'll type "op: ends with" under the "case sensitive. Finally, I'll type "path: event/COMAND_LINE" under the operator.
 
 Why is it the command line? Well, let's go back over to our "Processes". We'll see the "COMMAND_LINE" being used as a field and we're interested in particularly this command line argument of "all". So that's why we see an ends with "COMMAND_LINE" and the value is going to be "all". 
 
 
 So that means that this one is now good. Let me start erasing some of these so it's a lot easier to see. There you go that looks pretty good.
 
 
 What's next? "COMMAND_LINE" contains LaZagne, okay let's do that. So I'll type "case sensitive: false", then press tab on my keyboard. Afterwards, I'll type "op: contains" under the "case sensitive". Then, I'll type "path: event/COMMAND_LINE" under the operator. Finally, I'll type "value: \LaZagne.exe" under the "path".
 
  The last we're going to do is our hash, so let's do that now. We don't need this anymore, which is the previous rule that we modified, so I'll just remove that. 
  
  I'll just keep on building on this one here. So I'll type in "case sensitive: false" and I'll type in "op: is". The reason why it says "is", is because I want this to be equal to.
  
  Then, I'll type in "path: event/HASH" and "value: " with the hash right underneath it. So essentially that is now our rule. The event type must be either "NEW_PROCESS" or "EXISTING_PROCESS" and must be windows. 
  
  If we take a look here, the "file_path" must end with Lazagne. So we have the operator as ends with lasagna okay and then the command line must end with all so if we take a look at our second one we have the operator as ends with where the command line is set to all so that is good and then we have command line contains lasagna looking at the third one which is this we see our case sensitive as false operator is contains the path is command line and the value is lasagna finally we have our hash equals the lasagna hash so we take a look at the case sensitive that is false the operator is now is because we want this to be equal to and the path is event slash and the value is the hash of the lasagna file now again it is very difficult to see but there's a line right here there's a line right here line here and a line here so it has four criterias to meet within this one particular or operator and again there's a big line right here that signifies that this particular or this one right here is using the rules to meet its criteria so I'll go ahead and remove everything else at the bottom and let's go ahead and click on restore now we can move on to the respond block I wonder if I can expand the respond no I cannot that's okay so the action is currently set to report if you want a list of actions you can look at their documentation but essentially what report does is that if a certain event meets this criteria for this detection it will then generate a detection under the following detection Tab and that is what we want so we want this as a report the metadata I'm going to have it as author and let's just say my dfir the
description detects lasagna and then just bracket sord DED sword no sword dedr tool false positives uh let's just say to the Moon level mediums fine reference I don't have a reference so I'll remove that the tags it is going to be credential access I'm just going to remove the technique ID here and the name I'm going to say my dfir Das Haack tool Das lasagna and then bracket s- EDR beautiful I think this looks pretty good so let's go ahead and save the rule I'm going to call this as my defer Das lasagna Das s- EDR there you go I'm going to save this out and our new rule is created so what I really love about lima charlie here is that it has the capability to actually test your rule and how do you test it well click on target event and we just need to paste in the event to test it so let's go back over to our event here and I'll go ahead and just copy the entire event and let's paste it in now from here if I scroll down we can click on test event and nice look at that four operations were evaluated with the following results true true true true all green looking good just to quickly pause and recap what the heck I just did I didn't build the rule from scratch I mean I kind of did later on but what I did in the beginning was using existing rule found here in the GitHub and essentially I just built on that if you're starting out or in fact even if you're Advanced you should take a look at an existing Rule and then modify that to fit your use case in my opinion it's a lot easier to follow and alter an existing rule then start from scratch because by following an existing rule you can follow its format and take a look at what field names that they're using so again highly recommend you take a look at the GitHub look at existing Str rules copy it and then alter it to fit your use case and now all we really need to do is start generating this so I'm going to actually go back to the homepage here by clicking on the LC icon go into the organization and let's select detections so we have a lot of detections here and I am going to delete them all this cannot be undone yes that is okay now we have a clean slate I'm going to head back over to my server and let's clear out the screen here let's go ahead and run lasagna space all in theory our detection should catch this so lasagna runs over to our lima charlie there's no detections yet we'll just refresh it and still no detections but while we wait for the rule to be generated I released the my dfir sock analyst course which contains over 30 plus Hands-On labs and five exclusive sock projects if you or someone you know is interested in particularly the security operations domain this course is designed to help students learn how to ask better questions and investigate Network endpoint mare identity and Cloud related Telemetry once you purchase it it is yours for life and updates are free I'll put this in the description if you're interested and after refreshing the detections we do see a couple detections here at the very bottom we have the my df- hack tool Das lasagna that is awesome I'll go ahead and click on that and here's our event we have our command line lasagna all we have the file path we got the hash we have our host name the external IP address o and we also have our link so what I'm doing here is taking a look at the available Fields within this detection so then I can use it in the Playbook or story when we build it in time and before I forget I'll copy this Rule and you can find it in the description down below that way if you have some problems with your formatting you can go ahead and just copy mine I hope that everything went smoothly for you creating a detection rule in Lima Charlie can be a little bit confusing especially if you have never created a rule before but just like anything else the more reps you do the better you'll become at it and lima charlie has amazing documentation that you can reference anytime you get stuck as a reminder links to my S course and the rule file will be in the description Down Below in part four we'll begin to set out slack and times for automation that is it for the video and I hope to see you in part four remember to stay curious and do things differently
