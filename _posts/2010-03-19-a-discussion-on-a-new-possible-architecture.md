---
excerpt: "An email converstion ( 22/4/09 - 25/4/09 ):\r\n\r\nTo: Samuel Thibault\r\nIs
  kernel an absolute requirement when we are talking about Hurd? I find that Mach
  (or any kernel)is doing scheduling, IPC and low level IO. Scheduling, pre-emption
  can itself be a process which is never pre-empted. This would NOT be posible with
  the current harware architecture of course. So is the case with IPC and low level
  IO.\r\n\r"
categories: [page, paper]
layout: page
title: A Discussion on a new possible architecture
created: 1269028542
---
An email converstion ( 22/4/09 - 25/4/09 ):

To: Samuel Thibault
Is kernel an absolute requirement when we are talking about Hurd? I find that Mach (or any kernel)is doing scheduling, IPC and low level IO. Scheduling, pre-emption can itself be a process which is never pre-empted. This would NOT be posible with the current harware architecture of course. So is the case with IPC and low level IO.

This is being said keeping in mind a new architecture in which Hurd or similar OS will benifit the most. The idea is simple - let there be some processors  that are wired up in a way that some of them can talk to each other. They have a common communication protocol which also includes things like port rights as in case of translators. Other than the common communication protocol they have processes (or part of it) running in it. One more level of memory hierarchy shall be added.

I am thinking of something like this. It must be possible in years to come when they are planning to bring in more cores. Can you comment?

--
Mahesh M
-------------------------------------------------------------


From: Samuel Thibault <samuel.thibault@ens-lyon.org>

> I am thinking of something like this. It must be possible in years to come when
> they are planning to bring in more cores. Can you comment?

You are forgetting something: with nowadays' cores you do not tell a
core to switch task.  You send it a inter-processor interrupt to trigger
the kernel there, which will achieve the switch.  If you consider the
Cell processor, however, the main core (the PPU) can make cores (SPUs)
switch to something else.  That however is quite costly and people don't
do that, they embedded a little runtime layer in the SPUs to handle
jobs, thus some sort of kernel.

The thing is that a scheduler _has_ to be able preempt tasks. Doing it
remotely is costly, that's why it's done locally through interrupts.
With the current x86 task mechanism (which is not used by usual OSes),
the timer interrupt could make the processor automatically switch to the
scheduler task (one per core), the RPC trap could make the processor
automatically switch to the RPC task, and then the memory management
(MM) task can be implemented on top of that.

The issue with nowadays' x86 is that all of them need to live in ring0:
the scheduler to be able to switch to any other task, the RPC task to
read/write memory between tasks, the memory management (MM) task to
achieve memory operations.  And when you live in ring0 you can do
anything including trashing other ring0 tasks (particularly the RPC
task).  That's why it's usually just assembled into a kernel.  With some
piece of hardware that would allow a separation of these capabilities,
it would be possible, yes.  I fear such hardware won't ever exist.

Samuel

---------------------------------------------------

To: Samuel Thibault <samuel.thibault@ens-lyon.org>
hi,

Actully your comments gave me a more clear picture of what I was thinking :-). What I wanted, perhaps!!


I was trying to think of a new architecture. So I would consider these as the reasons why I am thinking to create a new one! But the main thing, as in case of cell processors, is that they are not meant to switch the processes in a core remotely. They are not efficient in that. But they can be made. This will bring in the idea of taking inter-processor communication to be made cheaper than the existing method(using the kernel to handle things). Its high time that the sticking to the traditional things be stopped(I think thats the reason why some of the limitaions still exist; backward compatibility!!). The runtime layer that you talked about can be implemented in hardware. If we have enough cores (say 500 or so) we can easily give each core a process. And that would lead the interprocess communication to be same as inter-processor communication, which will be implemented in hardware. These interprocessor communications will have the properties similar to IPC used in Hurd using Mach (Port permissions etc... ) and they are very efficient in doing that. In some of the latest cell processors, the banndwidth is now far advanced. I think they are more worried about the efficiency of each core rather than the cell as a whole. The specialised use of these processors in vectors graphics itself is a reason for that. That would imply that they are just meant to help the main core, provide parallelism when ever possible and thats it. They are not and cant be used to the fullest due to the architecture limitations. What they would say is that the processor servers the very purpose of what it was created for, aid in vector graphics, and it does that very well. So its a success. But what I would say is that they can be used much more.


Now in my architecture scheduler will have to detect a process being created and ask another process (resource allocator) to allocate the new process a core for its execution. Again, its not regarding the current architecture.



>    The issue with nowadays' x86 is that all of them need to live in ring0:
>    the scheduler to be able to switch to any other task, the RPC task to
>    read/write memory between tasks, the memory management (MM) task to
>    achieve memory operations.  And when you live in ring0 you can do
>    anything including trashing other ring0 tasks (particularly the RPC
>    task).  That's why it's usually just assembled into a kernel.  With some
>    piece of hardware that would allow a separation of these capabilities,
>    it would be possible, yes.  I fear such hardware won't ever exist.


This, i think, will be solved in the architecture I suggested. To explain how, I will have to write something much more elaborate. For which I need more feed back and  a bit more of knowledge in the hardware part, which I'm trying to gain.

--------------------------------------------


From:Samuel Thibault <samuel.thibault@ens-lyon.org>
> (I think thats the reason why some of the limitaions still exist;
> backward compatibility!!)

Sure it is.  Unfortunately there's a big mass of x86 software that
people want to be able to run fast.  Parallelism will already be a big
barrier for them, completely changing the hardware will be even harder
to get mainline.  You can sure dream but that's unfortunately not to
realize.
