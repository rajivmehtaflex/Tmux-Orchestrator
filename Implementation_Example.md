You are absolutely correct! The `README.md` and `CLAUDE.md` files provide specific commands and workflows for monitoring. My apologies for not highlighting them more directly.

Here is a revised, sequential guide that uses the **exact commands and methods described in the repository documentation** to track the progress of your autonomous agents.

---

### **Sequential Guide to Tracking Progress (Based on Repository Docs)**

You can execute these commands in a separate terminal window or in a new `tmux` window (`Ctrl-b c`) to avoid disturbing the agents.

**Step 1: View the High-Level Status (Orchestrator's Check-in)**
The orchestrator is designed to perform its own automated checks. The `schedule_with_note.sh` script triggers a command that gives a detailed status report. You will see the output of this command periodically in the main orchestrator window.

*   **What the Agent Runs:** `python3 claude_control.py status detailed`
*   **How You Use This:** You don't run this command yourself. Instead, navigate to the main `orchestrator` window (`tmux switch-client -t orchestrator`) and watch for the "Orchestrator Check-in Report" to appear automatically at the scheduled interval (e.g., every 15 minutes). This report provides a summary of each team's progress.

**Step 2: Manually Capture Real-Time Agent Activity**
The `CLAUDE.md` file specifies using `tmux capture-pane` to see what an agent is doing or has recently done without interrupting it.

*   **Command to Check an Agent's Window:**
    ```bash
    # Replace <session>:<window> with your target, e.g., backend-team:1 for the developer
    tmux capture-pane -t <session>:<window> -p | tail -30
    ```
*   **Practical Example:** To see the last 30 lines of what the **Frontend Developer** is doing:
    ```bash
    tmux capture-pane -t frontend-team:1 -p | tail -30
    ```

**Step 3: Check for Server Errors**
The documentation also highlights checking the server windows for errors.

*   **Command to Check a Server Window for Errors:**
    ```bash
    # Example for the Backend Server window
    tmux capture-pane -t backend-team:2 -p | grep -i error
    ```
*   **How You Use This:** If this command returns any lines, it means the server has logged an error, which you can then investigate or feed to the Project Manager agent.

**Step 4: Review Concrete Progress via Git Commits**
As per the mandatory "Git Discipline" rules in `CLAUDE.md`, agents commit their work every 30 minutes. This is the most reliable way to track completed work.

*   **Command to View Commit History:**
    1.  Navigate to the project directory:
        ```bash
        # Example for the frontend project
        cd ~/my-todo-app/frontend
        ```
    2.  Run the git log command:
        ```bash
        git log --oneline -10
        ```
*   **How You Use This:** This shows you the last 10 commits, giving you a clear, time-stamped history of what features or fixes have been successfully implemented and saved.

**Step 5: Actively Request a Status Update**
The `README.md` emphasizes using the `send-claude-message.sh` script to communicate directly with agents. This is how you "intervene when needed."

*   **Command to Request a Status Update:**
    1.  Navigate to the orchestrator directory:
        ```bash
        cd /path/to/tmux-orchestrator
        ```
    2.  Use the script to send a message to a Project Manager:
        ```bash
        ./send-claude-message.sh frontend-team:0 "STATUS UPDATE: Please provide: 1) Completed tasks, 2) Current work, 3) Any blockers"
        ```
*   **How You Use This:** After sending this command, switch to the `frontend-team:0` window to see the PM's response. This is the most direct way to get a summary without having to read through all the code and logs yourself.