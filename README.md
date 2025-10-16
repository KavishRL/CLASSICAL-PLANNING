# ExpNo:10 Implementation of Classical Planning Algorithm
# Algorithm or Steps Involved:
<ol>
  <li>Define the initial state</li>
  <li>Define the goal state</li>
  <li>Define the actions</li>
  <li>Find a <b>plan</b> to reach the goal state</li>
  <li>Print the plan</li>
</ol>

# Example - 1
```
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B']
```
# Example - 2
```
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B', 'move_B_to_C']
```

# Please Prepare Solution or Definition For the method find_plan(initial_state, goal_state, actions)
```
from collections import deque

def find_plan(initial_state, goal_state, actions):
    # Convert dicts to frozensets for hashing
    def state_to_tuple(state):
        return frozenset(state.items())

    # Check if goal is satisfied
    def is_goal(state):
        return all(state.get(k) == v for k, v in goal_state.items())

    # Apply action to get new state
    def apply_action(state, action):
        new_state = state.copy()
        new_state.update(action['effect'])
        return new_state

    # Check if action can be applied
    def is_applicable(state, action):
        return all(state.get(k) == v for k, v in action['precondition'].items())

    visited = set()
    queue = deque([(initial_state, [])])

    while queue:
        state, plan = queue.popleft()
        state_key = state_to_tuple(state)

        if state_key in visited:
            continue
        visited.add(state_key)

        if is_goal(state):
            return plan

        for name, action in actions.items():
            if is_applicable(state, action):
                new_state = apply_action(state, action)
                queue.append((new_state, plan + [name]))

    return None  # No plan found
```
#RESULT OBTAINED
