# tmux_session_manager
simple bash script to spawn pre-configured tmux sessions and easily swap between them. The tmux session is wrapped in an ssh-agent if a valid ssh keypath is passed to main

The code here expects that the calling code has a `session_name` variable and a `create_session` method defined. A simple example:

```
#!/bin/bash

source "$(dirname "$0")/lib/tmux_session_setup"

session_name="work"
work_directory="~/development/rickety_stool_store"

create_session() {
  tmux new-session -d -s "$session_name"
  tmux rename-window -t "$session_name 'vim'
  tmux send-keys -t "$session_name" cd ${work_directory}/" C-m
  tmux send-keys -t "$session_name" 'vim' C-m
  tmux new-window -t "$session_name" 
  tmux send-keys -t "$session_name" "cd ${work_directory}/projects" C-m
  tmux rename-window -t "$session_name" 'todos'
  tmux send-keys -t "$session_name" "cat todos.txt"
  tmux select-window -t vim
}

main "path/to/ssh/key"
```
