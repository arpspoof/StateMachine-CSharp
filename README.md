# StateMachine-CSharp
CSharp implementation for a general purpose single-active-state state machine

## Features
- Basics: Creating state machine, add states to state machine, allow usage of Trigger to drive state transition
- Allow recursive state machines. i.e. A state machine can be embedded in a state of another state machine
- State transition event system to support actions on various time slot
- Allow pre-validating and cancelling state transition
- Allow state recovery from a string descriptor 

## API - Build up the machine
#### State (Abstract class)
**A general state in a state machine. It can be a simple state or another state machine**
**events**
- **StateEnter**: Fired before entering this state
- **StateLeave**: Fired before leaving this state
*Note: In a general state transition, StateLeave will always be fired before StateEnter*
#### SimpleState
**A basic state which cannot be sub-divided**
**events**
- *StateEnter*: Inherited from State
- *StateLeave*: Inherited from State
#### StateMachine
**A state machine is also a state**
**events**
- *StateEnter*: Inherited from State
- *StateLeave*: Inherited from State
*Note: Inner state machine will start running after entering this state and will exit before leaving*
## API - Add Transitions
