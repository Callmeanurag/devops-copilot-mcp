# Day 01 – Understanding the MCP Request Lifecycle

**📅 Date:** 12 July 2026

**⏱ Time Spent:** XX Minutes

**⭐ Difficulty:** ⭐⭐☆☆☆

**📌 Status:** ✅ Completed

---

# 🎯 Objective

Yesterday, I finished Day 00 feeling pretty confident.

I had learned the basic architecture of MCP and knew there were components like the Host, MCP Client, MCP Server, and Tools.
But then I asked myself something embarrassingly simple.

> **"If I type a prompt in Claude and press Enter... what actually happens next?"**

Surprisingly, I didn't know the answer. I knew the names of the components, but I couldn't explain how they worked together.
So instead of writing code today, I decided to understand the journey of a request from start to finish.

---

# 🔄 Looking Back at Day 00

Yesterday was all about the bigger picture.
I wasn't trying to build anything.
I just wanted to answer three questions.

- What is MCP?
- Why does it exist?
- What am I actually trying to build?

By the end of the day, I had a rough picture in my head.

```
User
↓
Claude
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

The diagram looked simple. Understanding how everything worked together was a different story.
That's what today was about.

---

# 🤔 One Question Changed Everything

I kept thinking about one example throughout today's learning.

Imagine I ask Claude:

> **"Why did today's deployment fail?"**

Claude has never seen my GitHub repository. It has never connected to my Kubernetes cluster.
It definitely doesn't know what happened in today's deployment.
So...

**How does it answer that question?**

That was sometging I want to figure out today.

---

# 🔄 Following the Request

Instead of trying to memorize definitions, I followed the request itself.

*(Please checkout the Lifecycle Diagram.)*

Once I started looking at the architecture from the request's point of view, everything slowly started making more sense.

---

## 👤 It Starts With Me

Everything begins with a question and That's obvious.
But something I never appreciated before is, how little context the user actually provides.

->I don't mention APIs.

->I don't mention GitHub.

->I don't mention Kubernetes.

->I simply ask a question and expect an answer.

The complexity starts only after I press Enter.

---

## 💬 Claude Gets the Question

My first stop is Claude Desktop. Until today, I never really thought about it.
I open Claude every day.
I ask questions.
I get answers.
That's it.

But today I realized Claude has to make a decision before answering, right?

Can I answer this from what I already know?

Or do I need fresh information?

That was my first "aha!" moment.

---

## 🚫 Claude Doesn't Guess

This part actually surprised me.
When I ask,

> "What is Kubernetes?"

Claude already knows the answer.
But when I ask,

> "Why did today's deployment fail?"

Today's deployment isn't part of Claude's training data, there's no way it can know that. That information exists somewhere else.

Maybe in GitHub.

Maybe in Kubernetes.

Maybe in deployment logs.

Instead of guessing, Claude looks for a way to retrieve that information.
That makes much more sense than pretending AI magically knows everything.

---

## 🔌 MCP Enters the Picture

This is where MCP finally started making sense to me.
Before today, I thought MCP was just another API layer. Now I see it differently.
Claude needs information. MCP provides a standard way to reach the tools that can fetch that information.
I still don't know exactly how the Client and Server communicate internally.

And that's completely fine.

Today's goal wasn't to understand the implementation.
It was simply to understand why MCP exists in the first place.

---

## 🛠️ The Tool Does the Real Work

Another misconception I had was about the Tool.

Initially, I thought Claude itself would somehow directly communicate with GitHub and Kubernetes.
That's not how it works.

Instead, Claude asks a Tool to perform a specific task.
That task could be something like:

- Check a GitHub Actions workflow
- Read Kubernetes Pods
- Fetch deployment logs
- Read Terraform outputs

The Tool knows how to talk to those systems.
Claude doesn't need to.

That separation suddenly made the architecture feel much cleaner.

---

## 📦 Raw Data Comes Back

One thing I completely misunderstood before today was what GitHub or Kubernetes actually return.

I imagined they would send back something like:

> "The deployment failed because Docker couldn't pull the image."

But that's not what happens.

They return data.

Sometimes JSON.

Sometimes logs.

Sometimes API responses.

The explanation isn't coming from GitHub.
It's coming from Claude after reading that data.

The Tool retrieves that information and sends it back through MCP.

---

## 🤖 Finally, Claude Responds

This is where AI adds its real value. Claude doesn't generate deployment information.

It receives the information.

Then it reads it.

Understands it.

Finds the important details.

And explains everything in a way that's easy for humans to understand.

That's why I don't see raw logs on my screen. I see an explanation.
And that's exactly what I wanted in the first place.

But now I know there's a lot happening behind the scenes before it reaches me.

---

# 💡 What Clicked Today

If I had to pick one thing that finally clicked today, it would be this:

> **MCP doesn't give AI more intelligence. It gives AI access to information it didn't have before.**

The intelligence was already there. The missing piece was access to real-world data.

That single sentence changed how I look at the whole architecture.

---

# 🔄 Let's See the Deeper Picture

When I type "Why did today's deployment fail?" and hit Enter, a multi step sequence start running. The AI doesn't know my infrastructure, it has to orchestrate a data gathering process through a Request Lifecycle.

->**Step1: The Initial Evaluation**

The moment we press Enter, our prompt goes to the Host application's language Model. The LLM parses our request: "Why did today's deployment fail?"

It then immediately realizes, "I don't have this data in my static training weights." 
Because it is connected to our local MCP Client, it looks at its available "inventory" of tools that were negotiated during the initial handshake. It spots a server with tools like git_get_latest_commits() and fetch_production_logs().

->**Step2: The Tool Call (JSON-RPC)**

The LLM decides it needs to see the logs to answer our question. It tells the Host application to execute a tool.The MCP Client formats this intent into a standard JSON-RPC 2.0 message.It sends a message over the active transport, something like this (like local stdio or a remote network connection):

{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "fetch_production_logs",
    "arguments": { "timeframe": "today", "status": "error" }
  },
  "id": 1
}

->**Step 3: Server Processing (Your Python Code Fires)**

Our background MCP Server is listening. It receives that raw JSON payload.
If we used a framework like FastMCP, it intercepts the incoming parameters and runs them through a validation check (like Pydantic) to make sure the data matches the expected type strings.

Our native Python function executes: it reaches out to our infrastructure API (like AWS, GitHub Actions, or Azure), downloads today's failing log stack traces, and captures the raw crash dump.

->**Step 4: Structuring the Evidence**

Our server wraps the raw test or crash errors back into a unified MCP response object:

{
  "jsonrpc": "2.0",
  "result": {
    "content": [
      {
        "type": "text",
        "text": "CRITICAL: Database connection timeout in build container at 14:22 UTC."
      }
    ]
  },
  "id": 1
}

This payload travels back through the "universal cable" to the Host client.

->**Step 5: The Final Synthesis (The Second LLM Pass)**

The MCP Client catches this result and passes it directly into the LLM's active context window.
The LLM reads the log text: "Database connection timeout in build container."

It combines this fresh, real-time evidence with its deep core reasoning capabilities.

It stream-generates a human-friendly answer onto your screen: "**Today's deployment failed at 14:22 UTC because the build container couldn't reach your database. It looks like a network timeout—you should check your security group settings.**"

This also proves that,  MCP didn't replace our cloud provider's API. It simply served as the standardized translator that allowed the LLM to say, "Give me the logs," and allowed your Python script to reply, "Here they are," without either side needing to know the other's internal codebase.

---

# 🧠 My Mental Model

Right now, this is how I explain MCP to myself.

> AI is great at understanding and explaining information.

> External systems are great at storing real-time information.

> MCP is the bridge that connects those two worlds.

It's probably not the complete picture.

But for Day 01, it's a mental model that finally makes sense to me.

---

# 📚 What I Learned Today

Today's learning wasn't about code.

It was about understanding the flow.

Here's what I'm taking away:

- Every interaction starts with a user's question.
- Claude first decides whether it already knows the answer.
- If it doesn't, it looks for external information.
- MCP helps Claude communicate with the right tools.
- Tools interact with external systems.
- External systems return raw data.
- Claude turns that raw data into something humans can understand.

---

# 🤔 Questions I'm Carrying Forward

Even though today's session answered a lot of questions, it also created a few new ones.

- What exactly is the MCP Client?
- Why do we need both a Client and a Server?
- How does Claude know which Tool to call?
- Can one MCP Server expose multiple Tools?
- Who starts the MCP Server?

I'm deliberately not answering these today.

That's exactly what Day 02 is for.

---

# 💭 Reflection

I'll admit something. A part of me wanted to start coding today.

It almost felt strange to spend another day without writing a single line of code.
But I don't regret it.

Yesterday I could draw the architecture. Today I can explain the journey.

That's real progress.

I've also realized that this project isn't just about learning MCP.
It's about changing the way I learn new technologies.

Instead of rushing to code, I'm trying to build a solid mental model first.

Hopefully, that'll save me from blindly copying examples later.

---

# ⏭️ What's Next?

Now that I understand how a request travels through the MCP ecosystem, I want to zoom in on each component individually.

Tomorrow I'll stop treating the Host, Client, Server, and Tool as simple boxes in a diagram and finally understand what responsibility each one actually has.

Only after that will I start building my first MCP application.