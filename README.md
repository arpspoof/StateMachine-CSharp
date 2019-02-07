# StateMachine-CSharp
CSharp implementation for a general purpose single-active-state state machine

## Features
- Basics: Creating state machine, add states to state machine, allow usage of Trigger to drive state transition
- Allow recursive state machines. i.e. A state machine can be embedded in a state of another state machine
- State transition event system to support actions on various time slot
- Epsilon transition supported
- Allow pre-validating and cancelling state transition
- Allow state recovery from a string descriptor 

## Usage (temp)
1. Download source code and compile as class library
2. Add reference to the compiled class library file (dll)
3. using Infra.Algorithm.StateMachine name space

## API - Build up the machine
#### State (Abstract class)
A general state in a state machine. It can be a simple state or another state machine
**events**
- **StateEnter**: Fired before entering this state
- **StateLeave**: Fired before leaving this state

*Note: In a general state transition, StateLeave will always be fired before StateEnter*
#### SimpleState : State
A basic state which cannot be sub-divided
**events**
- *StateEnter*: Inherited from State
- *StateLeave*: Inherited from State
#### StateMachine : State
A state machine is also a state
**events**
- *StateEnter*: Inherited from State
- *StateLeave*: Inherited from State

*Note: Inner state machine will start running after entering this state and will exit before leaving*
## API - Transitions
#### Trigger
Triggers are used to drive state transition. For each connecting edge between one state and another, a trigger is required to determine when to do the state transition. The state transition will only be performed when the **Source State** is active **and** the trigger is fired. The trigger will have no effect when the source state is inactive. For epsilon transitions, pass **null** to the trigger parameter whenever needed
**methods**
- **Fire()**: Activate the trigger. If there is one transition edge using this trigger and source state is active, transition will be performed.
- **StopTransition()**: Call this in event handler of event **BeforeTransition** to cancel the trigger event. This maybe helpful when pre-validation is required.

**events**
- **BeforeTransition**: Fired before the trigger is going to drive a state transition
- **AfterTransition**: Fired after the trigger drived a state transition

#### Methods of StateMachine to build up transitions
- **AddState(State)**: Add a state and return state id. The id is used to add transitions
- **AddEntryTransition(int, Trigger)**: Add an edge from the entry point to some inner state. The state machine will be initialized in entry point when it starts to run. 
- **AddExitTransition(int, Trigger)**: Add an edge from some inner state to the exit point. The state machine will end when reaching the exit point. If the state machine is also a state, it may then transit to other states after the inner state machine has ended.
- **AddTransition(int, int, Trigger)**: Connect two inner states.

*Note: Set the trigger to be null to make an epsilon (unconditional) transition. Such transition will immediately happen when source state becomes active*
## API - Run the machine
#### Methods of StateMachine to operate the machine
- **Run()**: Start the state machine so that it will reach its entry point and then entry transitions may become possible. Only call to the outmost state machine. The embedded machines will run automatically.
- **Reset()**: Stop the machine and clean up everything. Need to call **Run** again if a re-run is needed. No need to call reset before the first run.

## API - State Recovery
At any time, you can back up the state information of a state machine (including states of all inner machines). And you can also recover the state using the generated state descriptor after the state machine is reset.
#### Methods of StateMachine related to backup and recovery
- **GenerateActiveStateDescriptor()**: Returns a string to describe the current state (including all sub machines)
- **RecoverState(string)**: Recover the state from the descriptor. A **Reset** will be automatically called before executing this operation.
