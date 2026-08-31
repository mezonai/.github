We built Mezon — a native, high-performance Discord alternative with a C backend using io_uring. Here’s why Electron-based chat apps need to die. I’m going to be blunt: Discord is a great product wrapped in a fundamentally broken runtime. Let me explain what we’ve been building and why it matters.

The Electron problem Discord ships Chromium + Node.js just to render a chat window. You’re running a full browser to send messages. On a 16 GB machine, Discord routinely consumes 500 MB–1 GB of RAM at idle. Electron abstracts the OS beautifully — and performs terribly because of it. React Native on mobile isn’t much better: the JS bridge between your code and native UI is a constant bottleneck. Mezon replaces all of this.
Download the Medium app

What “native” actually means in Mezon Mezon’s clients are built against platform-native UI frameworks — no Electron, no JS bridge, no React Native. On mobile you get actual native views. On desktop, native windowing. On the web, a lean compiled client. The result: idle RAM is a fraction of Discord’s, scrolling is silky, and startup is near-instant. This isn’t a marginal improvement — it’s a different category of software.

The server: C + io_uring The Mezon server is written in C and built around Linux’s io_uring interface. io_uring lets the kernel batch async I/O operations with near-zero syscall overhead — the same technology powering the fastest databases and web servers today. We handle far more concurrent connections per core than Node.js/Go/JVM-based servers like Discord uses. Lower latency, lower infrastructure cost, higher ceiling. We’re not just “another Discord clone.” The architecture is built for people tired of paying a RAM tax just to chat. Happy to go deep on the io_uring event loop, native UI layer decisions, or protocol design — ask below.

TL;DR: Discord = Electron (a browser). Mezon = native UI everywhere + C + io_uring server. Less memory, faster performance, higher throughput. Chat apps should be fast.
