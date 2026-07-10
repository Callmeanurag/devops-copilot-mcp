# Why am I building this?

As a DevOps Engineer, I spend a significant part of my day working with tools like Azure DevOps, GitHub Actions, Kubernetes, Terraform, Docker, and GitHub. Whenever a deployment fails, the investigation usually starts the same way—opening multiple dashboards, checking workflow logs, switching to the terminal, running `kubectl` commands, reading events, and trying to connect all the pieces together.

Over time, I started wondering:

> *What if I could simply ask an AI, "Why did today's deployment fail?" and it could investigate everything for me?*

That question led me to discover the **Model Context Protocol (MCP)**.

Instead of reading documentation from start to finish, I wanted to understand MCP the way I learn best—by building something practical that solves a real problem.

So this repository is my journey of building an **AI-powered DevOps Copilot** from scratch.

The idea isn't to create another "Hello World" MCP project. The goal is to build something that feels like an internal engineering tool—an assistant that can investigate failed GitHub Actions workflows, inspect Kubernetes resources, analyze deployment logs, generate Root Cause Analysis (RCA), and automate repetitive DevOps tasks through natural language.

I'm intentionally documenting everything as I learn. You'll find not only the final implementation but also the questions I had, the mistakes I made, the architecture decisions, and how my understanding of MCP evolved over time.

Hopefully, by the end of this journey, I'll not only understand MCP but also have a production-style DevOps project that demonstrates how AI can genuinely improve engineering workflows.