ps aux | grep visudo 
sudo lsof -p PID - List Open Files

aux: 
- a (all): Shows processes for all users, not just your currently logged-in user.
- u (user-oriented): Displays user-friendly details, adding columns like USER, %CPU, %MEM, and START time, rather than just raw technical numbers.
- x (no control tty): Includes processes that don't have an attached terminal (such as background system daemons, services started by the system kernel, or GUI apps running in the background).
 
R (Running / Runnable): The process is actively running on the CPU or waiting in the run queue to be scheduled.

S (Interruptible Sleep): The process is paused, waiting for an event, input, or resource to complete (this is the most common state for background apps and daemons).

D (Uninterruptible Sleep): The process is waiting for a hardware event (usually disk I/O or network storage) and cannot be interrupted or killed until it finishes.

T (Stopped): The process has been paused, either by a job control signal (like pressing Ctrl + Z in the terminal) or because a debugger is actively tracing it.

Z (Zombie / Defunct): The process has finished executing and died, but its parent process hasn't read its exit status yet. It takes up no resources other than a slot in the process table.

X (Dead): The process is completely dead and about to be removed (you will rarely or never see this in practice).

Common Modifiers (Extra flags attached to the state):
<: High priority (given more CPU time, "not nice").

N: Low priority (given less CPU time, "nice").

s: Session leader (the process controls a terminal session).

l: Multi-threaded (has multiple threads running).

+: Runs in the foreground process group of its terminal.

kill -l -- List all the process signals
ping localhost  >> /tmp/ping.log & 
fg 
jobs 
ps
ps -o ppid,pid,cmd
   PPID     PID CMD
   5794    5795 -bash
   5795    5956 ping localhost
   5795    5999 ps -o ppid,pid,cmd

top -> -o -> COMMAND=ping
k -> process PID -> SIG
SHIFT + T
SHIFT + P - CPU
SHIFT + M
ps -exo pid,ppid,command,%cpu,%mem | grep ping
-e - All processes (-A)
pstree -pn
fuser /tmp/ping.log