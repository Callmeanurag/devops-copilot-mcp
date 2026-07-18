# Day 02 – Understanding the MCP Components

**📅 Date:** 18 July 2026

**⏱ Time Spent:** 60 Minutes

**⭐ Difficulty:** ⭐⭐☆☆☆

**📌 Status:** ✅ Completed

---

# 🎯 Objective

After understanding the MCP Request Lifecycle in Day 01, I realized something.

I knew **where** the request was going, but I still didn't know **who was responsible for what**. Terms like **Host**, **MCP Client**, **MCP Server**, and **Tool** sounded familiar, but if someone asked me to explain the responsibility of each one, I probably couldn't do it confidently. So instead of writing code today, I wanted to understand the role of every component in the MCP architecture.

---

# 🔄 Looking Back at Day 01

On Day 01, I followed the journey of a request.

```
User
    ↓
Host
    ↓
MCP Client
    ↓
MCP Server
    ↓
Tool
    ↓
GitHub / Kubernetes / Terraform
    ↓
Response
```

That answered one question:

> **What happens after I press Enter?**

Today, I want to answer another one.

> **Who is responsible for what?**

---

# 🤔 The Question That Started Today

While looking at the architecture, one question kept bothering me.

> **Why do we need so many components?**

Why can't Claude talk directly to GitHub?

Why do we need both an MCP Client and an MCP Server?

Why isn't the Tool just another API?

The more I looked at the diagram, the more I realized every box exists for a reason.

Today's goal is to understand those reasons.

---

# 👤 Host

## What is it?

A Host is the main AI aaplication, such as a Chat Assistant or an IDE, that a user uses to interact with. It acts as the orchestrator, receiving natural language prompts and coordinating one or more MCP Clients to fetch tools and context from the External Sources. The host is the entry point that receives a request from the user and ensures it flows through the system.

As per the official documentation, MCP Host is the AI application that coordinates and manages one or multiple MCP clients.

## Why do we need it?

As we know that in MCP, the Host is the main AI application where we interact with. We need Host because it serves as the **Brain** and **Control Center**. It hosts the Large Language Model (LLM) and manages the entire workflow of getting external data.

the Host is required to:

1. **Manage the AI Environment:** It houses the LLM and provides the interface where you type your prompts and receive answers.
2. **Spawn MCP Clients:** It creates the individual, internal MCP clients that actually connect to and talk with various MCP servers (like GitHub, Google Drive, or local files).
3. **Coordinate Multiple Tools:** If your prompt requires querying a database and searching a codebase, the Host aggregates all these requests and merges the tool responses so the LLM can   synthesize a single, accurate answer.
4. **Enforce Security:** It controls permissions and user consent, ensuring external tools don't access files or execute commands without your knowledge.

## Responsibilities

- Receives the user's prompt
- Displays the AI response
- Starts and manages the MCP Client
- Provides the user interface

## What it does NOT do

- Doesn't execute Tools
- Doesn't fetch GitHub logs
- Doesn't talk directly to Kubernetes

## Examples

- Claude Desktop
- Cursor
- VS Code (with MCP support)

---

# 🔌 MCP Client

## What is it?

Client is the program-side component within an AI application or agent (like Claude Desktop or an IDE extension) that securely requests data and actions from an MCP Server to ground LLMs.

As per the official documentation, MCP Client is A component that maintains a connection to an MCP server and obtains context from an MCP server for the MCP host to use.

In the MCP architecture, the client acts as the consumer:

1. **Initiates Requests:** It sends standardized, JSON-RPC 2.0 requests to servers to invoke tools, search databases, or read files.
2. **Handles Permissions:** It ensures privacy by managing user consent and validating responses before feeding information into the LLM.
3. **Supports Sampling:** It allows servers to request that the client (which has access to the AI model) perform AI-dependent tasks on the server's behalf.

Usually, an AI "host" application creates one dedicated client for each MCP server it connects to

## Why do we need it?

An MCP Client is needed because LLMs cannot talk directly to external data sources or tools without a structured, secure mediator. Without an MCP Client, we have to write custom, hard-coded API integrations for every new tool or database an AI needs to use.
1. Unified IntegrationOne Protocol: Connects an AI application to any MCP-compliant tool or dataset using a single, unified communication standard.Plug-and-Play: Eliminates the need to rewrite custom API wrapper code every time you add a new data source to your AI.
2. Context GroundingDynamic Access: Fetches real-time, external data (like local files, live database tables, or current web pages) that the LLM was not trained on.Reduced Hallucinations: Supplies the LLM with exact, relevant text snippets right before it generates a response, ensuring factual accuracy.
3. Security and ControlGatekeeper Role: Acts as a secure barrier between the raw external server and the LLM, preventing untrusted code execution.User Consent: Pauses actions to prompt you for explicit approval before allowing a server to delete a file or modify a database.
4. Direct AI SamplingReverse Communication: Allows a simple server to ask the client's LLM to handle complex tasks (like summarizing text or refactoring code) during execution.

## Responsibilities

- Discovers available MCP Servers
- Knows which Tools are available
- Sends requests
- Receives responses
- Passes responses back to the Host

## What it does NOT do

- Doesn't execute Tools
- Doesn't call GitHub directly
- Doesn't generate AI responses

---

# 🖥️ MCP Server

## What is it?

An MCP Server (Model Context Protocol Server) is a lightweight adapter that securely connects AI models to external tools, databases, and data sources. It acts like a universal "USB for AI," it eliminates the need to write custom integration code for every single system.

Instead of letting an AI application directly access sensitive internal systems, the AI talks to an MCP server. The server handles translation, authentication, and security. It typically exposes three core capabilities to the AI:

1. **Tools:** Specific functions the AI can execute (e.g., querying a database, creating a GitHub pull request, or fetching real-time stock data).
2. **Resources:** Data the AI can read (e.g., file contents, local logs, or company wikis).
3. **Prompts:** Pre-defined interaction templates to guide AI responses.

MCP servers can run locally on your machine or remotely over HTTP. 

As per the official documentation, MCP servers are programs that expose specific capabilities to AI applications through standardized protocol interfaces.
Common examples include file system servers for document access, database servers for data queries, GitHub servers for code management, Slack servers for team communication, and calendar servers for scheduling.

## Why do we need it?

We need an MCP Server because AI models are isolated by default. They cannot natively interact with your local files, company databases, or external APIs without custom engineering. Before MCP, connecting an AI model to a tool required writing unique, complex integration code for every single combination of AI app and data source. If you switched from one AI client to another, you had to rewrite everything.MCP solves this problem by acting as a universal translator, offering several key benefits:

1. Write Once, Use Anywhere: Developers build one MCP server for a tool (like PostgreSQL or Jira), and any MCP-compliant AI application can instantly use it.
2. Security & Control: The server acts as a firewall, meaning you decide exactly what data the AI can see and what actions it can take.
3. Local Privacy: It allows AI applications to securely query data stored locally on your machine without uploading everything to the cloud.
4. Standardized Format: It replaces messy, custom API wrappers with a clean protocol, making AI development faster, cheaper, and less prone to errors.

## Responsibilities

- Exposes one or more Tools
- Receives requests from the MCP Client
- Executes the requested Tool
- Returns structured results

## What it does NOT do

- Doesn't decide which Tool should be called
- Doesn't communicate with the user
- Doesn't generate AI responses

---

# 🛠️ Tool

## What is it?

Tool is an executable action that an AI model can perform on an external system. While Resources only allow the AI to read data, Tools allow the AI to do things, interact with apps, and change states in the real world. Every tool is defined by the us and exposed through the MCP server. It consists of three main elements:

1. **Name:** A unique identifier (e.g., calculate_salaries or create_github_issue).
2. **Description:** A text explanation of what the tool does, which the AI reads to decide when to use it.
3. **Schema:** A strict definition of the input parameters the tool expects (e.g., text, numbers, or dates).

As per the official documentation, Tools are executable functions that AI applications can invoke to perform actions (e.g., file operations, API calls, database queries).

## How it Works (The Workflow)

1. **The Request:** You ask the AI to "Fix bug #12 and close the ticket".
2. **The Decision:** The AI looks at the available tools, sees a close_jira_ticket tool, and decides to use it.
3. **The Execution:** The AI generates the required parameters (e.g., ticket_id: 12) and asks the MCP server to run it.
4. **The Result:** The MCP server executes the code, closes the ticket via API, and returns a success message to the AI.

## Why do we need it?

Tools are needed because AI models cannot actually interact with the physical or digital world on their own. By default, an AI model can only predict the next word in a sentence; it has no hands, no internet access, and no execution power.

Tools change this by turning the AI from a passive "thinker" into an active "doer." We need them for several critical reasons:
1. **Taking Action:** Without tools, an AI can only tell you how to do something (e.g., "Here is the code to create a GitHub issue"). With tools, the AI can actually create the issue for you. 
2. **Dynamic Problem Solving:** The AI model itself decides when and how to use a tool based on your prompt. You do not need to hardcode the logic; the AI orchestrates the workflow dynamically.
3. **Real-Time Data Writing:** While resources let AI read data, tools let AI write, update, or delete data across your databases, cloud services, and local files.
4. **Safety Boundaries:** Tools act as a secure gateway. Instead of giving an AI unrestricted access to your entire system, you only give it access to specific, pre-defined functions (tools) that you control.

## Responsibilities

- Performs one specific task
- Talks to external systems
- Returns structured information

Examples

- Read GitHub Actions logs
- Restart Kubernetes Pod
- Fetch Terraform Outputs
- Read Deployment Status

## What it does NOT do

- Doesn't talk directly to the user
- Doesn't decide when it should run
- Doesn't explain results in natural language

---

# 🤝 How Everything Works Together

**(Check the attached image.)**

---

# 📊 Responsibilities Summary

| Component  | Responsibility |
|------------|----------------|
| User       | Asks a question |
| Host       | Receives prompts and displays responses |
| MCP Client | Finds the right Tool and forwards requests |
| MCP Server | Owns and executes Tools |
| Tool       | Performs a specific task |
| External Systems | Return real-time information |

---

# 🍽️ Real-Life Analogy

The easiest way for me to understand MCP was to compare it with a restaurant.

Imagine you walk into a restaurant.

You don't enter the kitchen.

You don't tell the chef how to cook.

You simply tell the waiter what you want.

Here's how that maps to MCP.

| Restaurant | MCP |
|------------|-----|
| Customer   | User |
| Waiter     | Host |
| Restaurant Order System | MCP Client |
| Kitchen    | MCP Server |
| Chef       | Tool |
| Ingredients| GitHub / Kubernetes / Terraform |

Everyone has a specific responsibility.

The waiter doesn't cook.

The chef doesn't take orders directly from customers.

The kitchen doesn't decide what the customer wants.

That separation of responsibilities is exactly what makes MCP clean and scalable.

---

# 🚫 Common Misconceptions

Some things I misunderstood before today.

- I thought Claude directly talked to GitHub.
- I thought the MCP Client executed Tools.
- I thought the MCP Server generated AI responses.
- I thought a Tool was just another API.

After today's learning, I know those assumptions weren't correct.

---

# ⚠️ Limitations

While learning today, I also realized what each component is **not** responsible for.

- The Host is not responsible for executing Tools.
- The MCP Client is not responsible for business logic.
- The MCP Server doesn't decide which Tool should run.
- The Tool doesn't explain the results.
- External systems don't generate AI responses.

Keeping these responsibilities separate is what makes the architecture flexible.

---

# 📝 My Understanding So Far

If someone asked me today,

> **"Can you explain MCP in simple words?"**

This is how I would answer.

--> The Host is the AI application I interact with.

--> The MCP Client acts like the communication layer. It knows which MCP Servers are available and forwards requests to the appropriate one.

--> The MCP Server owns the available Tools and executes them whenever a request arrives.

--> A Tool performs one specific task, like reading GitHub logs or checking the status of a Kubernetes Pod.

Finally, external systems provide the real-time data, and Claude turns that raw information into a response that humans can easily understand.

This understanding will probably evolve as I continue building this project, but for now, this mental model makes the most sense to me.

---

# 📚 Questions for Day 03

There are still a few things I haven't explored.

- What is FastMCP?
- What is the MCP SDK?
- How are Tools registered?
- How does Claude discover available Tools?
- How does an MCP Server keep running?

I'll leave those questions for tomorrow.

---

# 💭 Reflection

I thought today would be easier than Day 01.

It wasn't.

Following the request was one thing.

Understanding why every component exists was much harder.

But I'm glad I spent time on it.

I feel much more confident now because I no longer see the architecture as a collection of random boxes.

Each component has one responsibility, and together they make the entire MCP ecosystem work.

I still haven't written any code, but that's okay.

Building something is much easier once you understand why it was designed that way.

---

# ⏭️ What's Next?

Now that I understand the responsibility of each component, it's finally time to build something.

In **Day 03**, I'll create my first MCP Server, expose my first Tool, and connect it with Claude Desktop.

This is where the theory finally starts turning into code.