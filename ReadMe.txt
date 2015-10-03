http://youtu.be/uAQEn6puhFc

#This sofware is provide to control a Hubo-Ach Robot 
#the robot DRC-Hubo raise the left leg and balance on one foot and go up and down on right leg.
#then go on the left foot, left its right leg and hands into a position almost parallel to ground, 
#and and go up and down on right leg. then lower it self and balance on both legs again
#created 10 01 2015
#by Mahmoud Al Dabbas
 
 
import hubo_ach as ha
import ach
import sys
import time
import math
from time import sleep
from ctypes import *
from time import gmtime, strftime
PI = 3.141529
# Open Hubo-Ach feed-forward and feed-back (reference and state) channels
s = ach.Channel(ha.HUBO_CHAN_STATE_NAME)
r = ach.Channel(ha.HUBO_CHAN_REF_NAME)
s.flush()
r.flush()
# feed-forward will now be refered to as "state"
state = ha.HUBO_STATE()
# feed-back will now be refered to as "ref"
ref = ha.HUBO_REF()
# Get the current feed-forward (state) 
[statuss, framesizes] = s.get(state, wait=False, last=False)
print 'Starting the movement....'
print 'Tilting to the right....'
b=0.16 #define the angle of body tiltig to shift the center of mass.
a=0.01*b #some step
c = 0
while a<=b:
    ref.ref[ha.LEB] = 0
    ref.ref[ha.LSR] = 1.5*a #left arm swing
    ref.ref[ha.RSR] = -1.5*a #right arm swing
    ref.ref[ha.LAR] = -a
    ref.ref[ha.RAR] = -a
    ref.ref[ha.LHR] = a
    ref.ref[ha.RHR] = 1*a
    ref.ref[ha.RHP] = 0
    ref.ref[ha.LHP] = 0
    ref.ref[ha.RKN] = 0
    ref.ref[ha.LKN] = 0
    ref.ref[ha.LAP] = -0
    ref.ref[ha.RAP] = -0
    ref.ref[ha.RSP] = 0 
    ref.ref[ha.LSP] = 0
    time.sleep(0.2)
    a=a+0.02*b
    r.put(ref)

time.sleep(2)
print 'Waiting 2 seconds'
b=PI/3
a=0.01*b
c = 0
print 'Now lefting the left leg'
while a<=b: # for movemnet smoothing.
    ref.ref[ha.LHP] = -1*a 
    ref.ref[ha.RSR] = -.5
    ref.ref[ha.LAR] = 0
    ref.ref[ha.LHR] = .2
    ref.ref[ha.LKN] = 2*a 
    ref.ref[ha.LAP] = -1*a 
    x=time.time()
    r.put(ref)
    print "%.20f" % time.time(),'   Now lefting the left leg'
    c=c+1
    a=a+0.01*b
    time.sleep(.02)
print 'Waiting 2 seconds'
time.sleep(2)
print 'Going up and down for two times'
for x in range (1,3):
    b=PI/5
    a=0.01*b
    c = 0
    while a<=b: #going down
        ref.ref[ha.RHP] = -1.1*a 
        ref.ref[ha.RKN] = 2*a 
        ref.ref[ha.RAP] = -1*a 
        x=time.time()
        r.put(ref)
        print "%.20f" % time.time(), c,'   Going down'
        c=c+1
        a=a+0.01*b
        time.sleep(.05)
    time.sleep(0.2)
    while a>=0: #going up
        ref.ref[ha.RSP] = 0
        ref.ref[ha.LSP] = 0 
        ref.ref[ha.RHP] = -1.1*a 
        ref.ref[ha.RKN] = 2*a 
        ref.ref[ha.RAP] = -1*a 
        x=time.time()
        r.put(ref)
        print "%.20f" % time.time(), c,'    Going up'
        c=c-1
        a=a-0.01*b
        time.sleep(.05)
    time.sleep(0.2)

a=0.0
c = 0
step = 0.01
LAR = ref.ref[ha.LAR]
RAR = ref.ref[ha.RAR]
LHR = ref.ref[ha.LHR]
RHR = ref.ref[ha.RHR]
RAR = ref.ref[ha.RAR]
RHP = ref.ref[ha.RHP]
LHP = ref.ref[ha.LHP]
RKN = ref.ref[ha.RKN]
LKN = ref.ref[ha.LKN]
LAP = ref.ref[ha.LAP]
RAP = ref.ref[ha.RAP]
RSP = ref.ref[ha.RSP]
LSP = ref.ref[ha.LSP]

print 'Now relaxing the Robot'

ref.ref[ha.LAR] = 0
ref.ref[ha.RAR] = 0
ref.ref[ha.LHR] = 0.25
ref.ref[ha.RHR] = -0.25
ref.ref[ha.LAR] = 0
ref.ref[ha.RAR] = 0
ref.ref[ha.RHP] = -0.05
ref.ref[ha.LHP] = -0.05
ref.ref[ha.RKN] = 0
ref.ref[ha.LKN] = 0
ref.ref[ha.LAP] = 0 
ref.ref[ha.RAP] = 0 
ref.ref[ha.RSP] = 0.05 
ref.ref[ha.LSP] = 0.05
r.put(ref)

print 'Waiting  15 seconds'
time.sleep(15)
ref.ref[ha.LHR] = 0
ref.ref[ha.RHR] = -0
r.put(ref)
time.sleep(2)

print 'going to the left leg...................'
b=0.165 #define the angle of body tiltig to shift the center of mass.
a=0.01*b #some step
c = 0
print 'Tilting to the left....'
while a<=b:
    ref.ref[ha.LEB] = 0
    ref.ref[ha.LSR] = 1.5*a #left arm swing
    ref.ref[ha.RSR] = -1.5*a #right arm swing
    ref.ref[ha.LAR] = a
    ref.ref[ha.RAR] = a
    ref.ref[ha.LHR] = -a
    ref.ref[ha.RHR] = -a
    ref.ref[ha.RHP] = 0
    ref.ref[ha.LHP] = 0
    ref.ref[ha.RKN] = 0
    ref.ref[ha.LKN] = 0
    ref.ref[ha.LAP] = 0
    ref.ref[ha.RAP] = 0
    ref.ref[ha.RSP] = 0 
    ref.ref[ha.LSP] = 0
    time.sleep(0.2)
    a=a+0.02*b
    r.put(ref)
print 'Waiting 2 seconds'
time.sleep(2)
b=PI/3
a=0.01*b
c = 0
print 'Now lefting the right leg'

while a<=b: # for movemnet smoothing.
    ref.ref[ha.RHP] = -1*a 
    ref.ref[ha.LSR] = .5
    ref.ref[ha.RAR] = 0
    ref.ref[ha.RHR] = -.2
    ref.ref[ha.RKN] = 2*a 
    ref.ref[ha.RAP] = -1*a 
    x=time.time()
    r.put(ref)
    print "%.20f" % time.time(), c, '    Now lefting the right leg'
    c=c+1
    a=a+0.01*b
    time.sleep(.02)
print 'Waiting 2 seconds'
time.sleep(2)



b=PI/3
a=0.01*b
c = 0
RHP =     ref.ref[ha.RHP] 
RKN =     ref.ref[ha.RKN] 
RAP =     ref.ref[ha.RAP] 
LKN =    0.2*ref.ref[ha.LKN] 
step = 0.01
print 'Tilting right leg and two arms to Parallel-to-Ground-Position'
while a<=b: # for movemnet smoothing.
    ref.ref[ha.LHP] = -1*a 
    ref.ref[ha.RSP] = 0.05*a 
    ref.ref[ha.LHY] = -0.05*a 
    ref.ref[ha.LSP] = 0.05*a 
    ref.ref[ha.LKN] = .1*a
    ref.ref[ha.RHP] = ref.ref[ha.RHP]-step*RHP
    ref.ref[ha.RKN] = ref.ref[ha.RKN]-step*RKN
    ref.ref[ha.RAP] = ref.ref[ha.RAP]-step*RAP 
    x=time.time()
    r.put(ref)
    print "%.20f" % time.time(), c , '    Tilting right leg and two arms to Parallel-to-Ground-Position'
    c=c+1
    a=a+step*b
    time.sleep(.09)#this time gets divided by the step, so totla time is 9 seconds
print 'Waiting 2 seconds'
time.sleep(2)

print 'Going up and down for two times'
for x in range (1,3):
    b=PI/5
    a=0.01*b
    c = 0
    while a<=b:
        ref.ref[ha.LHP] = -0.01*b +ref.ref[ha.LHP]
        ref.ref[ha.LKN] = 2.1*a 
        ref.ref[ha.LAP] = -1*a 
        x=time.time()
        r.put(ref)
        print "%.20f" % time.time(),c , 'Going down'
        c=c+1
        a=a+0.01*b
        time.sleep(.05)
    time.sleep(0.2)
    while a>=0:
        ref.ref[ha.LSP] = 0
        ref.ref[ha.RSP] = 0 
        ref.ref[ha.LHP] = .01*b +ref.ref[ha.LHP]
        ref.ref[ha.LKN] = 2.1*a  
        ref.ref[ha.LAP] = -1*a 
        x=time.time()
        r.put(ref)
        print "%.20f" % time.time(),c , 'Going up'
        c=c-1
        a=a-0.01*b
        time.sleep(.05)
    time.sleep(0.2)





a=0.0
c = 0
step = 0.01
LAR = ref.ref[ha.LAR]
RAR = ref.ref[ha.RAR]
LHR = ref.ref[ha.LHR]
RHR = ref.ref[ha.RHR]
RAR = ref.ref[ha.RAR]
RHP = ref.ref[ha.RHP]
LHP = ref.ref[ha.LHP]
RKN = ref.ref[ha.RKN]
LKN = ref.ref[ha.LKN]
LAP = ref.ref[ha.LAP]
RAP = ref.ref[ha.RAP]
RSP = ref.ref[ha.RSP]
LSP = ref.ref[ha.LSP]

print 'Now relaxing the Robot'

while a<=1: # for movemnet smoothing.

	ref.ref[ha.LAR] = ref.ref[ha.LAR]-.01*LAR
	ref.ref[ha.RAR] = ref.ref[ha.RAR]-.01*RAR
	ref.ref[ha.LHR] = ref.ref[ha.LHR]-.01*LHR
	ref.ref[ha.RHR] = ref.ref[ha.RHR]-.01*RHR
	ref.ref[ha.RHP] = -0.08*a + RHP-a*RHP 
	ref.ref[ha.LHP] = -0.08*a + LHP-a*LHP
	ref.ref[ha.LKN] = ref.ref[ha.LKN]-.01*LKN
	ref.ref[ha.LAP] = ref.ref[ha.LAP]-.01*LAP 
	ref.ref[ha.RAP] = -0.3*a + RAP-a*RAP
	ref.ref[ha.RSP] = 0.05*a + RSP-a*RSP 
	ref.ref[ha.LSP] = 0.05*a + LSP-a*LSP 
	r.put(ref)
	print "%.20f" % time.time(),c, '    Now relaxing the Robot'
	a=a+step
	time.sleep(.1)
time.sleep(.3)
ref.ref[ha.RAP] = 0
r.put(ref)


# Close the connection to the channels
r.close()
s.close()
