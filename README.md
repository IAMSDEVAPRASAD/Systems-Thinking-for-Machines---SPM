# Systems-Thinking-for-Machines---SPM

Hi guys, this blog is a learning journal of a 23 yr old 2 years experienced Industrial Automation guy. The topics we are going to explore are things I learned as a part of my 2 yr career journey. 

 The things we focus are abstract concepts necessary for a good system design and debugging. Btw we are not gonna run around lot of terminologies only just things to get things done. When you walk out, you will be a good thinker. Lets begin...

1. Understanding req - what to do ? why to do? What to optimize ? What to skip ?
2. Observe -> Think -> Act
3. How to think in systems

## 1. Understanding req - What to do ? Why to do ? What to optimize ? What to skip ?

What is a problem? Anything that causes discomfort is a problem. It may be your car not starting, to your manager calling you for a Sunday work . It can be anything.

The first and foremost thing in solving a problem is **Defining the problem** it can also be said as Understanding the problem or Understanding the requirement. Which means trying to understand the problem ie the thing needs to be done.

Let's begin with a problem. Program a machine to pick & place a part, Simple huh? No I think so, so what's next?

Any problem as a whole looks too big and too complex, so when you try to understand it try breaking it down **“Divide and conquer”**. Yeah, begin with breaking the problem to small elements.

Before you break down the problem you must **have some basic knowledge of it** .

So, what is a machine? For an Automation Engineer, A machine is something which takes multiple parts into it and **assemble them** to a desired assembly or **test them** wether it pass some under certain conditions.

How does it work? The machine will have hardware (mechanical or electro mechanical) which will be connected to a slave controller of some kind which takes command from a master controller , based on the command from master the slaves controls the hardware. The controllers and hardware will have some wires or cables among them to communicate. 

Now to our pick & place machine, **What hardware do our machine have?** Our machine has a robot with a gripper to pick and place, a conveyer to move parts in and out, a stopper cylinder to make sure part is in a fixed place in a conveyor and a camera to check the part-these are probably the things we will control. 

**How these guys are communicating?** Consider a master controller (PLC or PC) needs to control it the master controller will be connected to slave devices using cables for communication (ethernet cable, RS232,....) these cables work on certain protocols (ethercat, Profinet, ...). The way master and slave are connected is called **Network topology**. The slave devices are further connected to hardware ie field devices. Common field devices are Input module, Output module, motor drive, camera and robot.

<img width="966" height="592" alt="Connection_Diagram" src="https://github.com/user-attachments/assets/e6405549-e108-49a1-ad8d-a949da0449ab" />

The field devices are controlled in a specific way such that a part comes in gets assembled and goes out. The master controller has some **control logic called as Auto sequence** which controls the field devices step by step. What if command fails or doesn't execute? its an alarm!!! **logic to handle alarm** should be there in controller. I don't want the user to wait during autosequence to check wether all devices are communicating and behaving in expected way , so here comes the **Manual mode** to check all slave devices response to a command. What if some one hits the robot in manual mode somewhere, I don't want people calling me midnight for support, so here comes **Interlocks**. Interlocks are used to prevent unsafe movements in a machine. Machine should be safe for both Man & Itself. For man safety we will have seperate hardware level **Safety devices**, for quick response. Btw who know when will our program hangs.

Yeah good, now we have basics-basics to Divide and conquer. 

Now back to the problem,Program a machine to pick & place a part. Our program should have logic to communicate with field devices, logic to send commands in a order (Auto sequence), away for user to check individual operations are working (Manual screen), Alarm logic (Manual mode and Auto mode) and Interlock logic (Manual mode and Auto mode). Those are our current requirements to build our software, we will update requirements as we move forward because **we don't know every thing at start.**

I hope all are now aware **What to do ? Why to do ?** . Now going to What to optimize ? What to skip ?

Imagine machine running and the user wants to increase speed, if those are inside Auto sequence there is a good chance you can be called upon all holidays to office, even so you come you will take plenty of time to change inside code, this is **What you need to optimise**, so move them out give a way for a technical team to change them when you are not there. Here comes **Recipie screen**, this is where we can change them. Recipie screen allows the user to change parameters like speed, acceleration, deacceleration, delay time, sensor debounce time, timeout duration, part pick point and so on.... the end goal is no magic numbers inside code. Btw recipie is not only for speed change we can have multiple recipies for different types of product to be assembled .To make sure only technical team can acess it bring **Role-Based Access Control** . Not every thing needs to be in screen some specific device parameters like minimum & maximum velocity, slave device communication pars,... can be kept in some files call them as **startup files** which need not to be shown in UI until customer asks, this is the part **What to skip ?**. 

Now our requirement has been updated with **Recipie screen, Role-Based Access Control and startup files**.

Figuring out What to do ? Why to do? What to optimize ? What to skip ? saves our time during system design and developing control logic. We can stop this topic here . We can discuss other topics next.


## 2. Observe -> Think -> Act 

To be frank, most of the people I observe act on a command or go with the group, they don't think. So what? I don't want you to do the same thing. **There maybe situations where acting quickly will be rewarded rather than thinking and acting** , but I hope it wont work in Engineering. Who knows you may figure out things with **bruteforce** , but i dont want spending too muchtime in office, so i dont prefer it unless needed.

So do i want you to think and act? No. One of the mistakes I did is thinking & acting. There may be 1000s of parameters which may lead to a result, but there is no necessity to analyse all 1000 of them. You will end in constant analysis if you think without context. 

An example : 

2 engineers a are working in our imaginary machine. The tray will be lifted up by a servo motor, once it reaches a certain point it will be stopped based on a sensor feedback. Once the tray stops at a point the robot picks it up. One engineer teaches robot (x,y,z) points to pick the part from a tray it went for probably a hour. Then all begin testing it in Auto sequence, the robot didn't pick. Tried multiple times still didn't. 

Then what, the guy who taught robot points begin to say no repeatability in servo motor, delayed sensor feedback, stop command going slowly to drive from control logic, ... . Other guy trying to figure out. Then one more guy said that robot points are taught with tray in 180 degrees. Then we figured out why it happened, its due to improper setting of a **poka yoke** or fool proof sensor. Btw poka yoke setups prevent these stuff.

The guy who taught robot points just thinks, discussing of all technical possibilities without observing. That's why observation is necessary. 

So what? Observe first understand the context, **not everything needs to be solved, not everything needs to be understood, there will be trade offs, sweetspots, sometimes you don't need absolute truth** - you need good enough truth to solve the problem. Figure it out & Act.

Let's move on to last part.
