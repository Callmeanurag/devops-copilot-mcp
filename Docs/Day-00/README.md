# Day 00 – Project Kickoff

**Date:** 11 July 2026

**Time Spent:** 45 Minutes

**Difficulty:** ⭐☆☆☆☆

**Status:** ✅ Completed

---

# 🎯 Objective

Before writing a single line of code, I wanted to answer a few important questions:

- What exactly am I building?
- Why am I building it?
- How will I approach learning MCP?
- What should the final product look like?

The goal of Day 00 is not to learn every MCP concept. Instead, it's about creating a clear roadmap so that every future milestone has a purpose.


---

# 🚀 Why This Approach?

When I first came across the **Model Context Protocol (MCP)**, my instinct was to open the official documentation and start reading.
It didn't take long to realize that this wasn't going to work for me. I've always learned best by building real projects. Reading concepts without applying them rarely sticks with me. Instead of creating another "Hello World" MCP example, I wanted to build something that solves a real problem I face as a DevOps Engineer.
So I decided to build an **AI-powered DevOps Copilot**.

The idea is simple:

Rather than trying to learn everything about MCP first, I'll build the project one milestone at a time. Every milestone will introduce one new MCP concept while solving one real DevOps problem.

By the end of this journey, I don't just want to know what MCP is, but I want to be confident enough to build production-style AI tools using it.

---

# 📖 What is MCP? (Current Understanding)

My current understanding is that the **Model Context Protocol (MCP)** provides a standardized way for AI applications to communicate with external tools and services.
It is an open-source, universal standard designed to connect AI models with external tools, data sources, and systems. It is created and Open-sourced by **Anthropic**.

In simple analogy, we can say MCP acts as a **USB-C port for AI**. Just as USB-C created a single standard to connect any hardware device, the Model Context Protocol (MCP) creates a single standard for AI models to connect with any software tool or data source.

Instead of directly accessing Kubernetes, Azure DevOps, or Terraform, the AI requests an MCP Server to execute specific tools and then uses the returned information to generate a response.

I expect this understanding to evolve as I progress through the project.

---

# 🎯 Final Vision

The goal of this project is to build a production-style **DevOps Copilot** capable of:

- Investigating failed GitHub Actions workflows
- Reading Kubernetes cluster information
- Analyzing deployment logs
- Restarting deployments
- Executing operational DevOps tasks
- Generating AI-powered Root Cause Analysis (RCA)
- Automatically creating GitHub Issues for failed deployments
- Interacting with GitHub, Kubernetes, Terraform, and other DevOps tools through MCP

---

# 📚 Topics Covered Today

- Project Kickoff
- Why this learning approach?
- What is MCP? (High-Level)
- Final Product Vision
- Learning Strategy
- Project Roadmap

> **No coding today.**

---

# ❓ What I Don't Know Yet

- What exactly is an MCP Client?
- How does Claude discover available tools?
- Why do we need an MCP Server?
- How are tools registered?
- How is MCP different from calling a REST API directly?
- Can a single MCP Server expose multiple tools?

---

# 🛣️ Learning Strategy

This repository is **not** based on tutorials.
Instead, I'll learn MCP the same way I usually learn new technologies—by solving real problems.

Each milestone will introduce:

- One new MCP concept
- One practical DevOps capability
- One improvement to the DevOps Copilot

Rather than rushing to build the final product, I'll gradually evolve it from a simple MCP project into a production-style engineering assistant.

---

# 🗺️ Project Roadmap

| Milestone | Goal | Status |
|-----------|------|--------|
| 0 | Project Kickoff | ✅ |
| 1 | Understanding the Request Lifecycle | ⏳ |
| 2 | Host, Client, Server & Tool | ⏳ |
| 3 | Build My First MCP Server | ⏳ |
| 4 | Build My First MCP Tool | ⏳ |
| 5 | Connect Claude Desktop | ⏳ |
| 6 | GitHub Integration | ⏳ |
| 7 | GitHub Actions Integration | ⏳ |
| 8 | Kubernetes Integration | ⏳ |
| 9 | Terraform Integration | ⏳ |
| 10 | AI-Powered Incident Investigation | ⏳ |
| 11 | Production-Ready DevOps Copilot | ⏳ |

---

# 📌 Questions to Explore

- How does a request travel through the MCP ecosystem?
- Why can't AI directly execute DevOps commands?
- What responsibilities belong to the Host?
- What does the MCP Client actually do?
- How does an MCP Server expose tools?
- How does the AI decide which tool to invoke?

---

# ✅ Progress

- [x] Project idea finalized
- [x] Repository created
- [x] Folder structure created
- [x] Final vision defined
- [x] Learning strategy documented
- [ ] Understand the Request Lifecycle
- [ ] Understand Host
- [ ] Understand MCP Client
- [ ] Understand MCP Server
- [ ] Build the first MCP Server

---

# 💭 Reflection

Today, I intentionally avoided writing any code.

Instead, I focused on understanding the problem I want to solve and defining a clear roadmap for the project.

One important realization is that learning **MCP itself is not the final goal**. The real goal is to build a useful DevOps Copilot that can interact with real DevOps tools, while MCP serves as the standard communication layer between AI and those tools.

This project will serve as my engineering diary, documenting not only successful implementations but also my learning process, questions, mistakes, and improvements.

Tomorrow, I will begin by understanding the complete request lifecycle:

```