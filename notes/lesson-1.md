# Lesson 1

Arvo - Urbit OS
: event handler and dispatcher

Agent
: programm in userspace
: a core
: state machine

vase
: pair of type and value
: `[type value]`
: produced by `!>` zapgar


cage
: a pair of mark and vase
: `[mark [type value]]`

mark
: a set of transformation rules
: can be a validator

path
: a list of @ta (knots)
: identifier for resource locations

wire
: alias of path
: unique identifier for a request

sign and sign-arvo 
: incoming events (in response to a standing request for data).

card
: message

term
: alias of @tas

`!>` zapgar
: build a vase

`[~ ..on-init]`
: the agent core itself
: copy of the current core
: . first dort is subject
: . second dot is "resolve against" (like :)


## Example

### %alfa

```hoon
|_  =bowl:gall 
++  on-init   [~ ..on-init]  :: first-time startup (set-up state)
++  on-save   !>(~)  :: envases, produces vase of current state
++  on-load   |=(vase [~ ..on-init])  :: unwraps old vase, makes state changes
++  on-poke   |=(cage !!)  :: single-time interaction
++  on-watch  |=(path !!)  :: standing subscription
++  on-leave  |=(path [~ ..on-init])  :: quite subscription
++  on-peek   |=(path ~)   :: single-time data request
++  on-agent  |=([wire sign:agent:gall] !!)  :: receive data from another agent
++  on-arvo   |=([wire sign-arvo] !!)   :: reveive data from Arvo OS
++  on-fail   |=([term tang] [~ ..on-init])  :: handle crash
--
```


### %bravo

```dojo
|commit %bravo
|install our %bravo
|rein %base [& %bravo]
:bravo +dbug
:bravo &noun ~
```

```hoon
/+  default-agent, dbug
|%
+$  versioned-state
  $%  state-0
  ==
+$  state-0
  $:  [%0 values=(list @)]
  ==
+$  card  card:agent:gall
--
%-  agent:dbug
=|  state-0
=*  state  -
^-  agent:gall
|_  =bowl:gall
+*  this .
    default  ~(. (default-agent this %|) bowl)
++  on-init
  ^-  (quip card _this)
  ~&  >  '%bravo initialized successfully'
  =.  state  [%0 *(list @)]  [~ this]
++  on-save   on-save:default
++  on-load   on-load:default
++  on-poke   on-poke:default
++  on-arvo   on-arvo:default
++  on-watch  on-watch:default
++  on-leave  on-leave:default
++  on-peek   on-peek:default
++  on-agent  on-agent:default
++  on-fail   on-fail:default
--
```
