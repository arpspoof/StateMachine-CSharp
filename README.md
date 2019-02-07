# StateMachine-CSharp
CSharp implementation for a general purpose single-active-state state machine

## Features
- Basics: creating state machine, add states to state machine, allow usage of Trigger to drive state transition
- Allow recursive state machines. i.e. A state machine can be embedded in a state of another state machine
- State transition event system to support actions on various time slot
- Allow pre-validating and cancelling state transition
- Allow state recovery from a string descriptor 
