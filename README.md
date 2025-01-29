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
<img src="https://snipboard.io/1TpHid.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/g1snE7.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So let's go ahead and search up "Windows security", and I'll go ahead and select the "Virus and threat protection". Click on "Manage settings", and disable "Real-time protection".
<br />
<br />
<img src="https://snipboard.io/dzch21.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yHijUL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now if I take a look at the downloads it says lazagne was blocked as unsafe by Microsoft Defender. Whenever you see this, what we can do is click on the three dots and click on "Keep". It will say, "This app is unsafe". Now click "Keep anyways".
<br />
<br />
<img src="https://snipboard.io/B3YQgd.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6jnxCV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now what we can do is take a look at the downloads and let's open up a Powershell. So I'll hold shift and right-click and click on "Open Powershell window here". Just to make sure it works, I'll just type in "Lazagne", and it works awesome.
<br />
<br />
<img src="https://snipboard.io/12eXBw.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/P4D7hS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Because I ran this, LimaCharlie should also pick up the process for Lazagne. So let's head over to LimaCharlie and take a look. So let's head over to "Sensors List" and click on our SOAR sensor.
<br />
<br />
<img src="https://snipboard.io/tiwDky.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  From here scroll down, we want to select "Timeline". Now we executed this not even a minute ago so it should be somewhere here, or what we can do is type in "Lazagne". Taking a look at the earliest event, we do see it at '20:52' generating an event of "NEW_DOCUMENT".
<br />
<br />
<img src="https://snipboard.io/Ndc47Z.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/MGh19a.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  We do see the path as "C:\Users\Administrator\Downloads". We have a hash and the process ID as well, but what we're interested in the most is the new process event. So click on that, and on the right, we get all of this juicy information. It's all pretty good information to have to start generating our detection rule.
<br />
<br />
<img src="https://snipboard.io/60nDyY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/pQ3wYG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/HCjhSn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So how do we do this? Well first and foremost, let's go ahead and open up a new tab for LimaCharlie and I'll head over to our organization and from here we want to select "Automation" and "DNR rules". On the top right we do have a new rule that we can use. So I'll click on "New Rule", and if this is your first time building a rule this might look pretty intimidating, but don't worry I'll show you a hack that you can use going forward.
<br />
<br />
<img src="https://snipboard.io/GsruTK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/r13gCA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/baT9sM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/cy6ZHB.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now we don't need to build a rule from scratch. I mean there's a lot of different formats and all that stuff so what we can do instead is, go back and let's just search up a rule that is similar to ours. We know that Lazagne is a credential access tool, so I'll type in "Credential" and hey look at that, we have some the credential stuff. Let's take a look at all the available ones here.
<br />
<br />
<img src="https://snipboard.io/21kMI3.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  What we're most interested in is the process creation. Now you might say, "How do you know that?" Well if we go back over to the events, take a look at the event type. It is set to "NEW_PROCESS", so these are all process creations.
<br />
<br />
<img src="https://snipboard.io/1jViCp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/i8ygTm.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So if I head back over to the DNR rules, we do see a Windows process creation. So let's go ahead and click on that, and at the bottom we do see view the content of this rule in the GitHub repository. So I'll click on that. 
<br />
<br />
<img src="https://snipboard.io/T4pqgc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/FNQoRx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  There you go, here's the rule right here. So what we can do is go ahead and click on "Raw" and copy this. Head back into our rule and click on the pencil icon. Now we can go ahead and paste in what we just copied. Now if you take a look at the top, we do see a detect and we do see a respond. So this essentially signifies the two blocks here.
<br />
<br />
<img src="https://snipboard.io/cexIWJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/5UR7vX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/8yfIEB.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now we can go ahead and paste in what we just copied. Now if you take a look at the top, we do see a detect and we do see a respond. So this essentially signifies the two blocks here.
<br />
<br />
<img src="https://snipboard.io/jJHk0V.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/oqLZep.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Starting from respond. I'll just select all of this and cut it, and then I'll paste it inside the respond block. Now we don't actually need the respond here, so I'll just go ahead and remove it.
<br />
<br />
<img src="https://snipboard.io/x2Rosq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/OXWUlI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nJghmG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Remember that response is just telling us that, "Hey this is the response portion". Similar to detect, we don't need it so we'll remove it and expand this. There you go.
<br />
<br />
<img src="https://snipboard.io/UGmVe9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/FG3KRn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/dMyrbA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Starting from the beginning we do see events with "NEW_PROCESS" and "EXISTING_PROCESS". So what does that mean? Well, let's head back over to our Lazagne events. What we want to do is select "Event Collection", and from here its just simply grabbing these event types. So if I search up "process", we see "EXISTING_PROCESS" and if we scroll down we also see "NEW_PROCESS" as well.
<br />
<br />
<img src="https://snipboard.io/elKPky.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/s8edA5.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/WzOmon.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So this section is essentially saying this detection will only trigger if the event is under the new process or existing process event type. Now we get our first operator of "and". So what does that mean? Well, it means the event type must be either new process or existing process "and". The "and" is specified by this operator.
<br />
<br />
<img src="https://snipboard.io/I1qOEc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EcipnO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/PDyz3m.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  To get a list of operators, they actually provide some right here. So "Op Reference", there's "and/or", "is", "exists", "contains", "starts with", "ends with", "it greater than", and just many others. So I'll go back over to expand, and again the operator being used here is "and". Currently, this is what it's doing in plain english.
<br />
<br />
<img src="https://snipboard.io/SLEdnT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wHtUEQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6IMTVA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So let's remove this and then let's continue on. "rules" is going to be the criteria, so meaning you must follow these rules. The operator is Windows, case sensitive is false, and "ends with". So it's using two different operators, "is" and "ends with". Where the "path" is "event/FILE_PATH", and you might be like, "Where did they get that?"
<br />
<br />
<img src="https://snipboard.io/ETwD2v.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Well, heading back over to our sensor. Let's go back to our timeline and search for our Lazagne process. So we're clicking on the "NEW_PROCESS" because we're interested in the new or existing process. So I'll click on that and we do see a "FILE_PATH" here. Going back over to the "Detection and Response Rule", we see the "event/FILE_PATH". 
<br />
<br />
<img src="https://snipboard.io/gHp5sU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/IGunxE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/YUDJOw.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/C8ePuI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So the "event" has a nested field of "FILE_PATH", so that is why we have "event/FILE_PATH". For example, let's just say I am interested in the hash. Then what will I do? Well, I'll type in "event/HASH", just like this, but I'll keep it back as "FILE_PATH".
<br />
<br />
<img src="https://snipboard.io/j7btDu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Qq8s5A.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/C8XEcJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/AfvDOp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Where the "value" ends in "\DeviceCredentialDeployment.exe". Let's bring this back as plain English here, so what the heck does this mean? Well, essentially it is the event type and must be either "NEW_PROCESS" or "EXISTING_PROCESS", and must be a Windows machine. We can ignore case sensitivity; the file path must end with "DeviceCredentialDeployment.exe".
<br />
<br />
<img src="https://snipboard.io/b2iLXJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/knYdz0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now if we wanted to detect our process of Lazagne, we can simply replace "DeviceCredentialDeployment" with Lazagne, and in theory, that should technically work. So let's go ahead and do that. I'll remove "DeviceCredentialDeployment" and I'll say "Lazagne".
<br />
<br />
<img src="https://snipboard.io/khCQAZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/hIlsu4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now if we take a look at our events, we can see that the "FILE_PATH" ends with "LaZagne" here, but notice how the "z" is capitalized. That is why we have the case sensitive as "false". So regardless, even if our value has a lowercase "z" or a capital Z, we should still be okay.
<br />
<br />
<img src="https://snipboard.io/h5oLRS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ipjoBG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So I'll keep this as that, but let's go ahead and add some more options because if you recall, let me head back over to my server. Take a look at the flags that are available for LaZagne. We have "all", "browsers", "chat", "databases", "games", "git", "mails", "maven", and a lot of others. So let's just type, "all". Look at that, we got some passwords here.
<br />
<br />
<img src="https://snipboard.io/Pnlwkz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/rVYtcp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ghK9c6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now what we can do is head back over to the LimaCharlie. I'll save this for now. Let's refresh this and we'll wait a bit until the process is generated. So after a couple of seconds, we do get our new process that occurred. I'll click on this one here.
<br />
<br />
<img src="https://snipboard.io/1KvB2G.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/U15aXz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/UJoq4Q.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So taking a look at our file path, we do see "LaZagna.exe" at the end. Take a look at the command line, we do see an "all" this time. If we look at the previous process for the command line, we just see it as "LaZagna.exe". We don't see any command line arguments associated with it.
<br />
<br />
<img src="https://snipboard.io/nlpctT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NgVp3b.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/T02cbx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now let's add on our detection rule to include this command line argument. The new rule that I want to create is that the event type must be either "NEW_PROCESS" or "EXISTING_PROCESS", and it must be Windows. The "file_path" should end with "LaZagne.exe", or the command line should end with "all", "lasagna", or "hash == LaZagne's hash.
<br />
<br />
<img src="https://snipboard.io/GrtIAn.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/1Yifgy.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now I do foresee some false positives for the command line "that ends with all". For example, we have "ipconfig /all", which will trigger that. However, for this demo let's just keep it as it is. So how do we build this out? Currently, we have this section good to go because we have the "NEW_PROCESS", the "EXISTING_PROCESS", and it is Windows.
<br />
<br />
<img src="https://snipboard.io/fpqkBc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/lHENQ1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So what do we do? How do we create the other ones? Well, we must include an operation of "or" and then I'll press tab on my keyboard. So our new rules is going to be living under this particular "or".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  So these rules will be "case sensitive: false" because I don't want any of that. So I'll type "op: ends with" under the case sensitive. Then, I'll type "path: event/FILE_PATH" under the operator. Second to last, I'll type "value: lazagne.exe" under the path. Awesome, so we satisfied these criteria.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  Now we have to take a look at the command line, but before I do that, let's go back and align our operator with our case sensitive. Perfect.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  The way you can tell, it's kind of hard to see, but I'll just try and draw it out here. There's a line right there. So this line will tell you what your fields are under, if that makes sense. Adding to that, I want all of my stuff to be under the "case sensitive".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />  
 For the second one, I'm going to type "- case sensitive: false", and just like before I'll type "op: ends with" under the "case sensitive. Finally, I'll type "path: event/COMAND_LINE" under the operator.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
 Why is it the command line? Well, let's go back over to our "Processes". We'll see the "COMMAND_LINE" being used as a field and we're interested in particularly this command line argument of "all". So that's why we see an ends with "COMMAND_LINE" and the value is going to be "all". 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
 So that means that this one is now good. Let me start erasing some of these so it's a lot easier to see. There you go that looks pretty good.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
 What's next? "COMMAND_LINE" contains LaZagne, okay let's do that. So I'll type "case sensitive: false", then press tab on my keyboard. Afterwards, I'll type "op: contains" under the "case sensitive". Then, I'll type "path: event/COMMAND_LINE" under the operator. Finally, I'll type "value: \LaZagne.exe" under the "path".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  The last we're going to do is our hash, so let's do that now. We don't need this anymore, which is the previous rule that we modified, so I'll just remove that. 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  I'll just keep on building on this one here. So I'll type in "case sensitive: false" and I'll type in "op: is". The reason why it says "is", is because I want this to be equal to.
  
  Then, I'll type in "path: event/HASH" and "value: " with the hash right underneath it. So essentially that is now our rule. The event type must be either "NEW_PROCESS" or "EXISTING_PROCESS" and must be windows. 
  
  If we take a look here, the "file_path" must end with "lazagne.exe". Then, the "command_line" must end with all. So if we take a look at our second one, we have the operator as "ends with".
  
Where the command line is set to "all". Then, we have "command_line" contains "lazagne.exe". Looking at the third one, we see our case sensitive as "false". Operator is "contains". The path is "COMMAND_LINE" and the value is "lasagne".

Finally, we have "hash == lasagna hash". So we take a look at the case sensitive, that is "false". The operator is now "is" because we want this to be equal to. The path is "event/HASH" and the value is the hash of the lazagne file. So it has four criteria to meet within this one particular "or" operator.

Again, there's a big line right here that signifies that this particular "or", is using the rules to meet its criteria. So I'll go ahead and remove everything else at the bottom. Now let's go ahead and click on "Restore". Finally, we can move on to the "Respond" block. 

So the "action" is currently set to "report". If you want a list of actions, you can look at their documentation. Essentially, what "report" does is if a certain event meets this criterion for this detection, it will then generate a detection under the following "Detection" tab, and that is what we want.

So we want this as a "report". For the metadata, I'm going to have it as "author" and let's just say "mydfir". For "Description", I'll say "Detects LaZagne (SOAR-EDR Tool)". For "falsepositives", I'll say "To the Moon". For "level", "medium" is fine.

For "references", I don't have a reference so I'll remove that. For "tags", I'll say "attack.credential_access". I'm going to remove "t128". For the "name", I'll say "MyDFIR - HackTool - LaZagne (SOAR-EDR). I think this looks pretty good.

So let's go ahead and save the rule. I'm going to call this as "MyDFIR-Lazagne-SOAR-EDR". I'm going to save this out and our new rule is created. So what I love about LimaCharlie is that it can test your rule. How do you test it?

Well, click on "Target Event" to copy the entire event. Then, we need to paste it in to test it. From here, scroll down and click on "Test Event" . Look at that, four operations were evaluated with the following results as true. All green, looking good.

Just to quickly pause and recap what I just did, I didn't build the rule from scratch. Initially, I used an existing rule in GitHub and I just built on that. If you're starting out, or in fact, even if you're advanced, you should take a look at an existing Rule and then modify that to fit your use case.

In my opinion, it's a lot easier to follow and alter an existing rule then start from scratch because by following an existing rule, you can follow its format and take a look at what field names that they're using.

So again highly recommend you take a look at the GitHub, look at existing rules, copy it, and then alter it to fit your use case. Now, all we really need to do is start generating this. So I'm going to actually go back to the homepage here by clicking on the home icon.

Go into the organization and let's select "Detections". So we have a lot of detections here and I'll delete them all.  Now that we have a clean slate, I'll head back over to my server and let's clear out the screen. Let's go ahead and run ".\LaZagne.exe all".

In theory, our detection should catch this. So if we go over to our LimaCharlie, theres no detections yet. After refreshing the detections, we do see a couple detections here. At the very bottom, we have the "MyDFIR - HackTool - Lazagne", so that is pretty awesome.

I'll go ahead and click on that and here's our event. We have our "COMMAND_LINE", "LasZgne", "all", we have the file path, we got the hash, we have our host name, the external IP address, and we also have our "link".

So what I'm doing here is taking a look at the available fields within this detection, so then I can use it in the Playbook or story when we build it in time. Before I forget, I'll copy this rule and you can find it in the description down below. That way if you have some problems with your formatting, you can go ahead and just copy mine.

I hope that everything went smoothly for you. Creating a detection rule in LimaCharlie can be a little bit confusing, especially if you have never created a rule, but just like anything else, the more times you do it, the better you'll become at it. LimaCharlie has amazing documentation that you can reference anytime you get stuck. In part four, we'll begin to set out Slack and Tines for automation. That is it for this part and I hope to see you in part four. Remember to stay curious and do things differently.
