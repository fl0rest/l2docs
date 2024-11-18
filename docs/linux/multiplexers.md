### What are multiplexers?

Multiplexers allow you to open several terminal sessions as one single terminal, think splitting your terminal into multiple parts to do different tasks.

#### Why use them?

We usually use multiplexers for one of two reasons:

1. Do multiple tasks at once
2. Keep sessions alive because of long running tasks

#### How they work

Most multiplexers run a server in the background allowing you to establish connections to it via sockets usually found in `/tmp`

#### Most used multiplexers

Most people choose between GNU Screen and tmux for their terminal multiplexers depending on their needs.

A quick comparison:

    - GNU Screen
        - Older, monolithic traditional approach
        - Single process handling all session management
        - Less feature rich but more stable
    - TMUX
        - Newer more configurable environment
        - Separate server and client processes
        - Robust window and session management


Recommendation:

Use GNU Screen for maximum compatibility, if you don't care how it looks just that it works.

Use Tmux for better and newer features and customization.
